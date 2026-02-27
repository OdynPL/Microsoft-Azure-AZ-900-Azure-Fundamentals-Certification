<a id="sec-25-service-bus"></a>
## 25. Azure Service Bus

[ Powrót do spisu treści](../README.md)


<img src="../assets/servicebus_queue_vs_topic.svg" alt="Service Bus - Queue vs Topic porownanie">

### Czym jest Azure Service Bus?

Azure Service Bus to w pełni zarządzana usługa **enterprise message broker** z kolejkami (queues) i tematami pub/sub (topics). Służy do **rozłączania aplikacji** i umożliwia niezawodną, asynchroniczną komunikację między komponentami.

**Kluczowe cechy:**
- Message queuing (FIFO)
- Publish/Subscribe (Topics + Subscriptions)
- At-least-once / At-most-once delivery
- Dead-letter queue (DLQ)
- Sessions (ordered processing)
- Scheduled messages
- Message deferral
- Duplicate detection
- Auto-forwarding

---

### Queue vs Topic - kiedy użyć?

| Scenariusz | Użyj | Dlaczego |
|------------|------|----------|
| 1 sender -> 1 receiver | **Queue** | Point-to-point |
| Load balancing między workerami | **Queue** | Competing consumers |
| 1 sender -> N receivers | **Topic** | Broadcast |
| Różne subskrypcje z filtrami | **Topic** | Message filtering |
| Event-driven architecture | **Topic** | Pub/Sub pattern |
| Background job processing | **Queue** | Work queue |

---

### Tiers - porównanie

<img src="../assets/servicebus_tiers.svg" alt="Service Bus Tiers - Basic, Standard, Premium">

| Cecha | Basic | Standard | Premium |
|-------|-------|----------|---------|
| **Queues** | Tak | Tak | Tak |
| **Topics** | NIE | Tak | Tak |
| **Max message size** | 256 KB | 256 KB | 100 MB |
| **Sessions** | NIE | Tak | Tak |
| **Transactions** | NIE | Tak | Tak |
| **Duplicate detection** | NIE | Tak | Tak |
| **Geo-DR** | NIE | NIE | Tak |
| **VNet** | NIE | NIE | Tak |
| **BYOK encryption** | NIE | NIE | Tak |
| **Dedicated resources** | NIE | NIE | Tak |
| **Use case** | Dev/Test | Production | Enterprise |

> **Egzamin:** Basic tier NIE obsługuje Topics! Standard wystarcza dla większości scenariuszy produkcyjnych.

---

### Tworzenie Service Bus - Azure CLI

```bash
# Utwórz namespace (kontener dla queues/topics)
az servicebus namespace create --name myServiceBusNS \
    --resource-group myRG \
    --location westeurope \
    --sku Standard

# Utwórz queue
az servicebus queue create --name myQueue \
    --namespace-name myServiceBusNS \
    --resource-group myRG \
    --max-size 1024 \
    --default-message-time-to-live P14D \
    --lock-duration PT1M \
    --dead-lettering-on-message-expiration true

# Utwórz topic
az servicebus topic create --name myTopic \
    --namespace-name myServiceBusNS \
    --resource-group myRG \
    --max-size 1024

# Utwórz subscription dla topic
az servicebus topic subscription create --name mySubscription \
    --topic-name myTopic \
    --namespace-name myServiceBusNS \
    --resource-group myRG \
    --max-delivery-count 10

# Pobierz connection string
az servicebus namespace authorization-rule keys list \
    --name RootManageSharedAccessKey \
    --namespace-name myServiceBusNS \
    --resource-group myRG \
    --query primaryConnectionString -o tsv
```

---

### C# - Service Bus z DefaultAzureCredential

```csharp
using Azure.Identity;
using Azure.Messaging.ServiceBus;
using Microsoft.Extensions.Configuration;

// === KONFIGURACJA (DI) ===
public static class ServiceBusExtensions
{
    public static IServiceCollection AddServiceBus(this IServiceCollection services, IConfiguration config)
    {
        // Użyj Managed Identity / Azure CLI / VS credentials
        var fullyQualifiedNamespace = config["ServiceBus:Namespace"]!;
        
        services.AddSingleton(_ => new ServiceBusClient(
            fullyQualifiedNamespace, 
            new DefaultAzureCredential()));
        
        return services;
    }
}

// === SENDER (Wysyłanie wiadomości) ===
public class OrderSender
{
    private readonly ServiceBusSender _sender;
    
    public OrderSender(ServiceBusClient client, IConfiguration config)
    {
        _sender = client.CreateSender(config["ServiceBus:QueueName"]!);
    }
    
    public async Task SendOrderAsync(Order order)
    {
        var message = new ServiceBusMessage(BinaryData.FromObjectAsJson(order))
        {
            MessageId = order.Id,
            ContentType = "application/json",
            Subject = "NewOrder"
        };
        message.ApplicationProperties["Priority"] = order.IsPriority ? "High" : "Normal";
        
        await _sender.SendMessageAsync(message);
    }
    
    public async Task SendBatchAsync(IEnumerable<Order> orders)
    {
        using var batch = await _sender.CreateMessageBatchAsync();
        foreach (var order in orders)
        {
            if (!batch.TryAddMessage(new ServiceBusMessage(BinaryData.FromObjectAsJson(order))))
            {
                await _sender.SendMessagesAsync(batch);
                batch.Dispose();
                // Utwórz nowy batch...
            }
        }
        if (batch.Count > 0) await _sender.SendMessagesAsync(batch);
    }
}

// === PROCESSOR (BackgroundService) ===
public class OrderProcessor : BackgroundService
{
    private readonly ServiceBusProcessor _processor;
    private readonly ILogger<OrderProcessor> _logger;
    
    public OrderProcessor(ServiceBusClient client, IConfiguration config, ILogger<OrderProcessor> logger)
    {
        _logger = logger;
        _processor = client.CreateProcessor(config["ServiceBus:QueueName"]!, new ServiceBusProcessorOptions
        {
            AutoCompleteMessages = false,
            MaxConcurrentCalls = 4,
            PrefetchCount = 10
        });
        
        _processor.ProcessMessageAsync += ProcessAsync;
        _processor.ProcessErrorAsync += ErrorAsync;
    }
    
    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        await _processor.StartProcessingAsync(ct);
        await Task.Delay(Timeout.Infinite, ct);
    }
    
    private async Task ProcessAsync(ProcessMessageEventArgs args)
    {
        try
        {
            var order = args.Message.Body.ToObjectFromJson<Order>();
            _logger.LogInformation("Processing order {OrderId}", order.Id);
            
            // Przetwarzanie...
            
            await args.CompleteMessageAsync(args.Message);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Error processing message");
            await args.AbandonMessageAsync(args.Message);
        }
    }
    
    private Task ErrorAsync(ProcessErrorEventArgs args)
    {
        _logger.LogError(args.Exception, "Service Bus error");
        return Task.CompletedTask;
    }
}
```

**appsettings.json:**
```json
{
  "ServiceBus": {
    "Namespace": "myservicebusns.servicebus.windows.net",
    "QueueName": "orders"
  }
}
```

**Wymagane RBAC role:**
- `Azure Service Bus Data Sender` - wysyłanie
- `Azure Service Bus Data Receiver` - odbieranie
        
---

### Subscription Filters

<img src="../assets/servicebus_filters.svg" alt="Service Bus Filters - SQL, Correlation, True/False">

Filtrowanie wiadomości na poziomie subscription:

| Typ filtra | Opis | Przykład |
|------------|------|----------|
| **SQL Filter** | SQL-like WHERE | `OrderType = 'Premium' AND Amount > 100` |
| **Correlation Filter** | Property matching | Match on MessageId, Subject, To |
| **True Filter** | Wszystkie wiadomości | Default (brak filtra) |
| **False Filter** | Żadne wiadomości | Blokuje subscription |

```csharp
// SQL Filter
new SqlRuleFilter("Region = 'Europe' AND Priority > 5")

// Correlation Filter (wydajniejszy)
new CorrelationRuleFilter
{
    Subject = "NewOrder",
    ApplicationProperties = { { "OrderType", "Premium" } }
}
```

---

### Dead-Letter Queue (DLQ)

<img src="../assets/servicebus_dlq.svg" alt="Service Bus Dead-Letter Queue">

Wiadomości trafiają do DLQ gdy:
- Przekroczona liczba prób dostarczenia (MaxDeliveryCount)
- Wiadomość wygasła (TTL)
- Ręcznie przeniesiona przez aplikację
- Filter evaluation exception

```csharp
// Odbieranie z DLQ
var dlqProcessor = client.CreateProcessor(
    queueName, 
    new ServiceBusProcessorOptions { SubQueue = SubQueue.DeadLetter }
);

dlqProcessor.ProcessMessageAsync += async args =>
{
    // Sprawdź powód DLQ
    var reason = args.Message.DeadLetterReason;
    var description = args.Message.DeadLetterErrorDescription;
    
    Console.WriteLine($"DLQ Message: {args.Message.Body}");
    Console.WriteLine($"Reason: {reason}, Description: {description}");
    
    // Napraw i ponownie wyślij lub zloguj
    await args.CompleteMessageAsync(args.Message);
};
```

---

### Sessions (Ordered Processing)

<img src="../assets/servicebus_sessions.svg" alt="Service Bus Sessions - ordered processing">

Sessions gwarantują FIFO processing dla wiadomości z tym samym SessionId:

```csharp
// Wysyłanie z SessionId
var message = new ServiceBusMessage("Order item 1")
{
    SessionId = "Order-12345"  // Wszystkie items tego zamówienia
};
await sender.SendMessageAsync(message);

// Odbieranie session messages
var sessionProcessor = client.CreateSessionProcessor(queueName, 
    new ServiceBusSessionProcessorOptions
    {
        MaxConcurrentSessions = 5,
        SessionIdleTimeout = TimeSpan.FromMinutes(1)
    });

sessionProcessor.ProcessMessageAsync += async args =>
{
    // args.SessionId - ID sesji
    // args.GetSessionStateAsync() - stan sesji
    Console.WriteLine($"Session: {args.SessionId}, Message: {args.Message.Body}");
    await args.CompleteMessageAsync(args.Message);
};
```

---

### Message TTL (Time-To-Live)

<img src="../assets/servicebus_ttl.svg" alt="Service Bus TTL - Time-To-Live">

TTL określa jak długo wiadomość może pozostać w kolejce zanim wygaśnie.

**Default values:**

| Poziom | Default TTL | Max TTL |
|--------|-------------|---------|
| **Queue/Topic** | 14 dni | TimeSpan.MaxValue (unlimited) |
| **Message** | Dziedziczy z Queue | TimeSpan.MaxValue |
| **Subscription** | Dziedziczy z Topic | TimeSpan.MaxValue |

**Co się dzieje po wygaśnięciu TTL:**
- Jeżeli `DeadLetteringOnMessageExpiration = true` -> wiadomość trafia do DLQ
- Jeżeli `DeadLetteringOnMessageExpiration = false` -> wiadomość jest usuwana

**Ustawienie TTL na Queue (CLI):**

```bash
# Utwórz queue z TTL 7 dni
az servicebus queue create --name myQueue \
    --namespace-name myServiceBusNS \
    --resource-group myRG \
    --default-message-time-to-live P7D \
    --dead-lettering-on-message-expiration true

# Zmień TTL na istniejącej queue (30 dni)
az servicebus queue update --name myQueue \
    --namespace-name myServiceBusNS \
    --resource-group myRG \
    --default-message-time-to-live P30D
```

**Format TTL (ISO 8601 Duration):**
- `P14D` = 14 dni
- `P7D` = 7 dni
- `PT1H` = 1 godzina
- `PT30M` = 30 minut
- `P1DT12H` = 1 dzień i 12 godzin

**Ustawienie TTL na pojedynczej wiadomości (C#):**

```csharp
// TTL na poziomie wiadomości (nadpisuje default z queue)
var message = new ServiceBusMessage("Important data")
{
    TimeToLive = TimeSpan.FromHours(1)  // Wygaśnie za 1 godzinę
};
await sender.SendMessageAsync(message);

// Wiadomość bez TTL (użyje default z queue)
var message2 = new ServiceBusMessage("Normal data");
await sender.SendMessageAsync(message2);
```

**Sprawdzenie kiedy wiadomość wygaśnie:**

```csharp
processor.ProcessMessageAsync += async args =>
{
    // ExpiresAt = EnqueuedTime + TTL
    var expiresAt = args.Message.ExpiresAt;
    var enqueuedTime = args.Message.EnqueuedTime;
    var ttl = args.Message.TimeToLive;
    
    Console.WriteLine($"Enqueued: {enqueuedTime}");
    Console.WriteLine($"TTL: {ttl}");
    Console.WriteLine($"Expires: {expiresAt}");
    
    await args.CompleteMessageAsync(args.Message);
};
```

> **Tip:** Ustawiaj rozsądne TTL! Zbyt długi = zalegające wiadomości i koszty. Zbyt krótki = utrata danych.

---

### Scheduled Messages

<img src="../assets/servicebus_scheduled.svg" alt="Service Bus Scheduled Messages">

```csharp
// Zaplanuj wiadomość na później
var scheduledMessage = new ServiceBusMessage("Scheduled reminder");
long sequenceNumber = await sender.ScheduleMessageAsync(
    scheduledMessage, 
    DateTimeOffset.UtcNow.AddHours(1)  // Za godzinę
);

// Anuluj zaplanowaną wiadomość
await sender.CancelScheduledMessageAsync(sequenceNumber);

// Lub ustaw ScheduledEnqueueTime
var message = new ServiceBusMessage("Later...")
{
    ScheduledEnqueueTime = DateTimeOffset.UtcNow.AddMinutes(30)
};
await sender.SendMessageAsync(message);
```

---

### Managed Identity (bez connection string)

```csharp
using Azure.Identity;
using Azure.Messaging.ServiceBus;

string fullyQualifiedNamespace = "myServiceBusNS.servicebus.windows.net";
string queueName = "myQueue";

// Managed Identity (App Service, Functions, AKS)
await using var client = new ServiceBusClient(
    fullyQualifiedNamespace, 
    new DefaultAzureCredential()
);

await using var sender = client.CreateSender(queueName);
await sender.SendMessageAsync(new ServiceBusMessage("Secure message!"));
```

**Wymagane RBAC role:**
- `Azure Service Bus Data Sender` - wysyłanie
- `Azure Service Bus Data Receiver` - odbieranie
- `Azure Service Bus Data Owner` - pełny dostęp

---

### Service Bus vs Storage Queue vs Event Grid vs Event Hubs

| Cecha | Service Bus | Storage Queue | Event Grid | Event Hubs |
|-------|-------------|---------------|------------|------------|
| **Typ** | Message broker | Simple queue | Event routing | Event streaming |
| **Pattern** | Queue + Pub/Sub | Queue only | Pub/Sub | Stream |
| **Max size** | 256KB/100MB | 64 KB | 1 MB | 1 MB |
| **Ordering** | FIFO (Sessions) | NIE | NIE | Partition |
| **DLQ** | Tak | NIE | Tak | NIE |
| **Throughput** | High | Medium | Very high | Very high |
| **Use case** | Enterprise | Simple tasks | Events | Telemetry |
| **Cena** | Higher | Low | Pay per event | Medium |

> **Egzamin:** Service Bus to enterprise message broker. Storage Queue to prosta, tania kolejka. Event Grid to event routing (reactive). Event Hubs to streaming (big data).

---

### Best Practices

| Praktyka | Opis |
|----------|------|
| **Reuse clients** | ServiceBusClient i Sender/Processor są thread-safe |
| **Use batching** | SendMessagesAsync(batch) dla wielu wiadomości |
| **Set appropriate TTL** | Unikaj zalegających wiadomości |
| **Monitor DLQ** | Regularnie sprawdzaj dead-letter queue |
| **Use Managed Identity** | Unikaj connection strings w kodzie |
| **Idempotent handlers** | At-least-once może dostarczyć duplikaty |
| **Enable duplicate detection** | Dla krytycznych wiadomości |
| **Use Premium for prod** | Jeżeli potrzebujesz VNet, Geo-DR |

---

### Przykładowy scenariusz: Order Processing

```csharp
// 1. API otrzymuje zamówienie -> publikuje do Topic
public class OrderController : ControllerBase
{
    private readonly ServiceBusSender _sender;
    
    [HttpPost]
    public async Task<IActionResult> CreateOrder(Order order)
    {
        var message = new ServiceBusMessage(BinaryData.FromObjectAsJson(order))
        {
            MessageId = order.Id.ToString(),  // Dla duplicate detection
            Subject = "NewOrder",
            ApplicationProperties = 
            {
                { "OrderType", order.IsPremium ? "Premium" : "Standard" },
                { "Region", order.Region }
            }
        };
        
        await _sender.SendMessageAsync(message);
        return Accepted();
    }
}

// 2. Subscription "InventoryUpdate" (filter: wszystkie zamówienia)
// 3. Subscription "PremiumNotify" (filter: OrderType = 'Premium')
// 4. Subscription "EuropeAnalytics" (filter: Region = 'Europe')
```

---

### FAQ - Egzamin

| Pytanie | Odpowiedź |
|---------|----------|
| Czym różni się Queue od Topic? | Queue = point-to-point, Topic = pub/sub |
| Czy Basic tier ma Topics? | NIE, tylko Standard/Premium |
| Co to jest DLQ? | Dead-letter queue dla nieprzetworzonych wiadomości |
| Jak zagwarantować FIFO? | Użyj Sessions (SessionId) |
| Max message size w Standard? | 256 KB |
| Jak filtrować w subscriptions? | SQL Filter lub Correlation Filter |
| Service Bus vs Event Grid? | Service Bus = messaging, Event Grid = event routing |

> **Egzamin:** Service Bus to enterprise message broker obsługujący queues i topics (pub/sub). Basic tier NIE obsługuje topics. Sessions zapewniają FIFO. DLQ przechowuje nieprzetworzone wiadomości.

---
---

[Poprzednia strona](./24-debugging.md) | [Spis treści](../README.md) | [Następna strona](./26-event-hub.md)
