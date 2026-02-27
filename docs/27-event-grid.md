<a id="sec-27-event-grid"></a>
## 27. Azure Event Grid

[ Powrót do spisu treści](../README.md)


Azure Event Grid to w pełni zarządzana usługa routingu zdarzeń oparta na modelu publikuj-subskrybuj (pub/sub). Umożliwia reaktywne programowanie z sub-sekundowym opóźnieniem i płatnością per zdarzenie.

<img src="../assets/eventgrid_overview.svg" alt="Event Grid Overview" />

### Kluczowe cechy Event Grid

| Cecha | Opis |
|-------|------|
| **Model** | Reaktywny pub/sub (event-driven) |
| **Latencja** | Sub-sekundowa |
| **Przepustowość** | 10 milionów zdarzeń/sek per region |
| **Schema** | CloudEvents 1.0 / Event Grid schema |
| **Max event size** | 1 MB |
| **SLA** | 99.99% |
| **Cena** | ~$0.60 per milion zdarzeń |

### Podstawowe koncepty

**Topics** - Endpoint do wysyłania zdarzeń:
- **System Topics** - automatycznie tworzone dla usług Azure (Blob, VM, etc.)
- **Custom Topics** - tworzone przez użytkownika dla własnych aplikacji
- **Partner Topics** - zdarzenia od partnerów (Auth0, SAP, Datadog)

**Subscriptions** - łączą Topic z Handlerem:
- Definiują filtrowanie zdarzeń
- Konfigurują retry policy i dead-letter
- Wskazują destination (handler)

**Event Handlers** - przetwarzają zdarzenia:
- Azure Functions, Logic Apps, Webhooks
- Event Hubs, Service Bus, Storage Queues

### Typy Topics

<img src="../assets/eventgrid_topics.svg" alt="Event Grid Topics" />

### Tworzenie Event Grid (CLI)

```bash
# Tworzenie Custom Topic
az eventgrid topic create \
  --name myCustomTopic \
  --resource-group myRG \
  --location westeurope

# Pobieranie endpoint i klucza
endpoint=$(az eventgrid topic show --name myCustomTopic --resource-group myRG --query "endpoint" -o tsv)
key=$(az eventgrid topic key list --name myCustomTopic --resource-group myRG --query "key1" -o tsv)

# Tworzenie subskrypcji z webhook
az eventgrid event-subscription create \
  --name mySubscription \
  --source-resource-id "/subscriptions/{sub-id}/resourceGroups/myRG/providers/Microsoft.EventGrid/topics/myCustomTopic" \
  --endpoint "https://myapp.azurewebsites.net/api/events" \
  --endpoint-type webhook

# Tworzenie subskrypcji na zdarzenia Blob Storage
az eventgrid event-subscription create \
  --name blobSubscription \
  --source-resource-id "/subscriptions/{sub-id}/resourceGroups/myRG/providers/Microsoft.Storage/storageAccounts/mystorageaccount" \
  --endpoint "/subscriptions/{sub-id}/resourceGroups/myRG/providers/Microsoft.Web/sites/myFunctionApp/functions/ProcessBlob" \
  --endpoint-type azurefunction \
  --included-event-types Microsoft.Storage.BlobCreated \
  --subject-begins-with "/blobServices/default/containers/images/" \
  --subject-ends-with ".jpg"
```

### Filtrowanie zdarzeń

<img src="../assets/eventgrid_filtering.svg" alt="Event Grid Filtering" />

```bash
# Subscription z advanced filtering
az eventgrid event-subscription create \
  --name filteredSubscription \
  --source-resource-id "/subscriptions/{sub-id}/resourceGroups/myRG/providers/Microsoft.Storage/storageAccounts/mystorageaccount" \
  --endpoint "https://myfunction.azurewebsites.net/api/processLargeImages" \
  --advanced-filter data.contentLength NumberGreaterThan 1048576 \
  --advanced-filter data.contentType StringIn image/jpeg image/png
```

**Typy filtrów:**

| Typ filtra | Opis | Przykład |
|------------|------|----------|
| **Event Type** | Typ zdarzenia | `Microsoft.Storage.BlobCreated` |
| **Subject** | Ścieżka zasobu | `beginsWith: /images/`, `endsWith: .jpg` |
| **Advanced** | Filtrowanie po danych | `data.contentLength > 1000` |

**Operatory Advanced Filter:**
- `StringIn`, `StringNotIn`, `StringBeginsWith`, `StringEndsWith`, `StringContains`
- `NumberIn`, `NumberNotIn`, `NumberGreaterThan`, `NumberLessThan`
- `BoolEquals`, `IsNullOrUndefined`, `IsNotNull`

### Wysyłanie i odbieranie zdarzeń (C#)

```csharp
using Azure.Messaging.EventGrid;
using Azure.Messaging.EventGrid.SystemEvents;
using Microsoft.Azure.Functions.Worker;

// === PUBLIKOWANIE ZDARZEŃ DO CUSTOM TOPIC ===
public class EventGridPublisher
{
    private readonly EventGridPublisherClient _client;
    
    public EventGridPublisher(string topicEndpoint, string topicKey)
    {
        _client = new EventGridPublisherClient(
            new Uri(topicEndpoint),
            new Azure.AzureKeyCredential(topicKey));
    }
    
    // Wysyłanie pojedynczego zdarzenia
    public async Task PublishOrderCreatedAsync(Order order)
    {
        var eventGridEvent = new EventGridEvent(
            subject: $"orders/{order.Id}",
            eventType: "Contoso.Orders.OrderCreated",
            dataVersion: "1.0",
            data: new OrderCreatedEventData
            {
                OrderId = order.Id,
                CustomerId = order.CustomerId,
                TotalAmount = order.TotalAmount,
                Items = order.Items.Select(i => i.Name).ToList()
            });
        
        await _client.SendEventAsync(eventGridEvent);
    }
    
    // Wysyłanie batch zdarzeń (CloudEvents format)
    public async Task PublishCloudEventsAsync(IEnumerable<OrderEvent> events)
    {
        var cloudEvents = events.Select(e => new CloudEvent(
            source: "https://contoso.com/orders",
            type: e.EventType,
            jsonSerializableData: e.Data));
        
        await _client.SendEventsAsync(cloudEvents);
    }
}

// === ODBIERANIE ZDARZEŃ (Azure Function - Event Grid Trigger) ===
public class EventGridFunctions
{
    private readonly ILogger<EventGridFunctions> _logger;
    private readonly IOrderService _orderService;
    
    public EventGridFunctions(ILogger<EventGridFunctions> logger, IOrderService orderService)
    {
        _logger = logger;
        _orderService = orderService;
    }
    
    // Obsługa Custom Events
    [Function("ProcessOrderEvent")]
    public async Task ProcessOrderEvent(
        [EventGridTrigger] EventGridEvent eventGridEvent)
    {
        _logger.LogInformation($"Event Type: {eventGridEvent.EventType}");
        _logger.LogInformation($"Subject: {eventGridEvent.Subject}");
        
        switch (eventGridEvent.EventType)
        {
            case "Contoso.Orders.OrderCreated":
                var orderData = eventGridEvent.Data.ToObjectFromJson<OrderCreatedEventData>();
                await _orderService.ProcessNewOrderAsync(orderData);
                break;
                
            case "Contoso.Orders.OrderCancelled":
                var cancelData = eventGridEvent.Data.ToObjectFromJson<OrderCancelledEventData>();
                await _orderService.HandleCancellationAsync(cancelData);
                break;
        }
    }
    
    // Obsługa System Events (Blob Storage)
    [Function("ProcessBlobEvent")]
    public async Task ProcessBlobEvent(
        [EventGridTrigger] EventGridEvent eventGridEvent)
    {
        if (eventGridEvent.EventType == "Microsoft.Storage.BlobCreated")
        {
            var blobData = eventGridEvent.Data.ToObjectFromJson<StorageBlobCreatedEventData>();
            
            _logger.LogInformation($"New blob: {blobData.Url}");
            _logger.LogInformation($"Size: {blobData.ContentLength} bytes");
            _logger.LogInformation($"Content-Type: {blobData.ContentType}");
            
            // Przetwarzanie obrazu
            if (blobData.ContentType.StartsWith("image/"))
            {
                await ProcessImageAsync(blobData.Url);
            }
        }
    }
}

// Event Data classes
public record OrderCreatedEventData(
    Guid OrderId, 
    string CustomerId, 
    decimal TotalAmount, 
    List<string> Items);

public record OrderCancelledEventData(
    Guid OrderId, 
    string Reason, 
    DateTime CancelledAt);
```

### Retry Policy i Dead-Letter

<img src="../assets/eventgrid_retry.svg" alt="Event Grid Retry" />

```bash
# Konfiguracja retry i dead-letter
az eventgrid event-subscription create \
  --name mySubscription \
  --source-resource-id "/subscriptions/{sub-id}/resourceGroups/myRG/providers/Microsoft.EventGrid/topics/myTopic" \
  --endpoint "https://myapp.com/api/events" \
  --max-delivery-attempts 10 \
  --event-ttl 1440 \
  --deadletter-endpoint "/subscriptions/{sub-id}/resourceGroups/myRG/providers/Microsoft.Storage/storageAccounts/mystorage/blobServices/default/containers/deadletter"
```

**Retry Policy:**
| Parametr | Opis | Default | Zakres |
|----------|------|---------|--------|
| Max Delivery Attempts | Liczba prób dostawy | 30 | 1-30 |
| Event TTL | Czas życia zdarzenia | 1440 min (24h) | 1-1440 min |

**Kody odpowiedzi:**
- **Sukces:** 200, 201, 202, 204
- **Retry:** 408, 429, 500-599
- **No Retry (drop):** 400, 401, 403, 404, 413

### Walidacja Webhook Endpoint

Event Grid wymaga walidacji endpointu przed pierwszą dostawą zdarzeń:

```csharp
// Obsługa walidacji w Azure Function
[Function("WebhookEndpoint")]
public async Task<IActionResult> WebhookEndpoint(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")] HttpRequest req)
{
    var requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    var events = JsonSerializer.Deserialize<EventGridEvent[]>(requestBody);
    
    foreach (var eventGridEvent in events)
    {
        // Obsługa walidacji subskrypcji
        if (eventGridEvent.EventType == "Microsoft.EventGrid.SubscriptionValidationEvent")
        {
            var validationData = eventGridEvent.Data.ToObjectFromJson<SubscriptionValidationEventData>();
            
            return new OkObjectResult(new SubscriptionValidationResponse
            {
                ValidationResponse = validationData.ValidationCode
            });
        }
        
        // Przetwarzanie normalnych zdarzeń
        await ProcessEventAsync(eventGridEvent);
    }
    
    return new OkResult();
}
```

### Przykład: Reaktywna architektura

```csharp
// Scenariusz: Upload obrazu -> resize -> CDN purge -> notification
// Wszystko orkiestrowane przez Event Grid

// 1. Blob Storage emituje BlobCreated event
// 2. Azure Function przetwarza obraz
[Function("ResizeImage")]
public async Task ResizeImage([EventGridTrigger] EventGridEvent evt)
{
    var blobData = evt.Data.ToObjectFromJson<StorageBlobCreatedEventData>();
    await _imageProcessor.ResizeAsync(blobData.Url);
    
    // Publikuj własne zdarzenie o zakończeniu przetwarzania
    await _eventGridClient.SendEventAsync(new EventGridEvent(
        subject: $"images/{Path.GetFileName(blobData.Url)}",
        eventType: "Contoso.Images.Processed",
        dataVersion: "1.0",
        data: new { OriginalUrl = blobData.Url, ProcessedAt = DateTime.UtcNow }));
}

// 3. Logic App subskrybuje Contoso.Images.Processed -> purge CDN
// 4. Azure Function subskrybuje Contoso.Images.Processed -> wyślij email
[Function("NotifyImageProcessed")]
public async Task NotifyImageProcessed([EventGridTrigger] EventGridEvent evt)
{
    var data = evt.Data.ToObjectFromJson<ImageProcessedData>();
    await _emailService.SendAsync(
        to: "admin@contoso.com",
        subject: "Image processed",
        body: $"Image {data.OriginalUrl} has been processed");
}
```

### Event Grid vs Event Hub vs Service Bus

| Aspekt | Event Grid | Event Hub | Service Bus |
|--------|------------|-----------|-------------|
| **Model** | Reactive pub/sub | Streaming | Enterprise messaging |
| **Przypadek użycia** | Event routing, automation | Telemetry, logs, IoT | Transactions, orders |
| **Latencja** | Sub-sekundowa | Sub-sekundowa | Milisekundy |
| **Przepustowość** | 10M events/sec/region | Millions/sec | Thousands/sec |
| **Retencja** | 24h (retry) | 1-90 dni | Do przetworzenia |
| **Kolejność** | Nie gwarantowana | FIFO per partition | FIFO (sessions) |
| **Filtrowanie** | Event Type, Subject, Advanced | Brak (consumer side) | SQL/Correlation filters |
| **Cena** | Per event (~$0.60/M) | Per TU/PU | Per operation |
| **Główne źródła** | Azure services, custom apps | IoT, logs, clickstream | Business apps |
| **Główne handlery** | Functions, Logic Apps, webhooks | Stream Analytics, Spark | Workers, Functions |

### Kiedy użyć którego?

```
Event Grid:
✓ Reakcje na zmiany w Azure (blob created, VM started)
✓ Automatyzacja i orchestracja (serverless workflows)
✓ Wysoka liczba źródeł, niski wolumen per źródło
✓ Nie potrzebujesz retencji danych

Event Hub:
✓ Big Data streaming (miliony zdarzeń/sek)
✓ IoT telemetry, logi aplikacji, clickstream
✓ Potrzebujesz retencji (replay, Capture)
✓ Integracja z Apache Kafka

Service Bus:
✓ Enterprise messaging (zamówienia, transakcje)
✓ Gwarantowane przetworzenie (at-least-once)
✓ Potrzebujesz FIFO, sessions, transactions
✓ Pub/sub z filtrowanymi subskrypcjami
```

---

### FAQ - Egzamin

| Pytanie | Odpowiedź |
|---------|----------|
| Co to jest Event Grid? | Usługa reaktywnego routingu zdarzeń (pub/sub) |
| Ile kosztuje Event Grid? | ~$0.60 per milion zdarzeń |
| Jaka jest latencja? | Sub-sekundowa |
| Czy Event Grid gwarantuje kolejność? | NIE |
| Czym są System Topics? | Automatyczne topics dla usług Azure |
| Jakie handlery obsługuje Event Grid? | Functions, Logic Apps, webhooks, Event Hubs, Service Bus |
| Event Grid vs Service Bus? | Event Grid = event routing, Service Bus = messaging |
| Event Grid vs Event Hub? | Event Grid = reactive, Event Hub = streaming |

> **Egzamin:** Event Grid to reaktywny pub/sub do routingu zdarzeń z sub-sekundową latencją. System Topics automatycznie tworzą się dla usług Azure (Blob, VMs). NIE gwarantuje kolejności. Cena per event (~$0.60/M). Filtrowanie: Event Type, Subject, Advanced Filters.

---
---

[Poprzednia strona](./26-event-hub.md) | [Spis treści](../README.md) | [Następna strona](./28-logic-apps.md)
