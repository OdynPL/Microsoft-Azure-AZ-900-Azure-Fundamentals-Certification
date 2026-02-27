<a id="sec-26-event-hub"></a>
## 26. Azure Event Hub

[ Powrót do spisu treści](../README.md)


Azure Event Hub to w pełni zarządzana usługa do strumieniowego przesyłania dużych ilości danych (Big Data streaming). Umożliwia przechwytywanie, przechowywanie i przetwarzanie milionów zdarzeń na sekundę z niskim opóźnieniem.

<img src="../assets/eventhub_overview.svg" alt="Event Hub Overview" />

### Kluczowe cechy Event Hub

| Cecha | Opis |
|-------|------|
| **Przepustowość** | Miliony zdarzeń na sekundę |
| **Protokoły** | AMQP 1.0, Kafka, HTTPS |
| **Retencja** | 1-90 dni (zależnie od tier) |
| **Partycje** | 1-2000 (zależnie od tier) |
| **Rozmiar zdarzenia** | Max 1 MB |
| **Integracja** | IoT Hub, Stream Analytics, Databricks, Spark |

### Tworzenie Event Hub (CLI)

```bash
# Tworzenie namespace
az eventhubs namespace create \
  --name myEventHubNS \
  --resource-group myRG \
  --location westeurope \
  --sku Standard

# Tworzenie Event Hub z 4 partycjami
az eventhubs eventhub create \
  --name myEventHub \
  --namespace-name myEventHubNS \
  --resource-group myRG \
  --partition-count 4 \
  --message-retention 7

# Tworzenie Consumer Group
az eventhubs eventhub consumer-group create \
  --eventhub-name myEventHub \
  --namespace-name myEventHubNS \
  --resource-group myRG \
  --name AnalyticsGroup
```

### Partycje i Consumer Groups

<img src="../assets/eventhub_partitions.svg" alt="Partycje i Consumer Groups" />

**Partycje:**
- Strumień danych jest dzielony na partycje dla skalowalności
- Partition Key określa, do której partycji trafi zdarzenie
- **FIFO gwarantowane tylko w obrębie jednej partycji**
- Liczby partycji **nie można zmienić** po utworzeniu Event Hub!

**Consumer Groups:**
- Każda grupa ma niezależny widok całego strumienia
- Umożliwia wielu konsumentom odczytywanie tych samych danych
- `$Default` tworzona automatycznie
- Max 5 połączeń na partycję w grupie (AMQP)

### Wysyłanie i odbieranie zdarzeń (C#)

```csharp
using Azure.Identity;
using Azure.Messaging.EventHubs;
using Azure.Messaging.EventHubs.Producer;
using Azure.Messaging.EventHubs.Processor;
using Azure.Storage.Blobs;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Hosting;

// === SENDER (z DefaultAzureCredential) ===
public class TelemetrySender
{
    private readonly EventHubProducerClient _producer;
    
    public TelemetrySender(IConfiguration config)
    {
        // Użyj Managed Identity zamiast connection string!
        var fullyQualifiedNamespace = config["EventHub:Namespace"]!;
        var eventHubName = config["EventHub:Name"]!;
        
        _producer = new EventHubProducerClient(
            fullyQualifiedNamespace, 
            eventHubName, 
            new DefaultAzureCredential());
    }
    
    public async Task SendBatchAsync(IEnumerable<SensorData> readings)
    {
        using var batch = await _producer.CreateBatchAsync();
        
        foreach (var reading in readings)
        {
            var eventData = new EventData(BinaryData.FromObjectAsJson(reading));
            eventData.Properties["DeviceId"] = reading.DeviceId;
            
            if (!batch.TryAdd(eventData))
            {
                await _producer.SendAsync(batch);
                batch.Dispose();
                // Utwórz nowy batch...
            }
        }
        
        if (batch.Count > 0) await _producer.SendAsync(batch);
    }
}

// === PROCESSOR (BackgroundService z DI) ===
public class TelemetryProcessor : BackgroundService
{
    private readonly EventProcessorClient _processor;
    private readonly ILogger<TelemetryProcessor> _logger;
    
    public TelemetryProcessor(IConfiguration config, ILogger<TelemetryProcessor> logger)
    {
        _logger = logger;
        
        // Storage dla checkpointów (też z Managed Identity)
        var storageUri = new Uri($"https://{config["Storage:AccountName"]}.blob.core.windows.net/{config["Storage:CheckpointContainer"]}");
        var storageClient = new BlobContainerClient(storageUri, new DefaultAzureCredential());
        
        _processor = new EventProcessorClient(
            storageClient,
            config["EventHub:ConsumerGroup"]!,
            config["EventHub:Namespace"]!,
            config["EventHub:Name"]!,
            new DefaultAzureCredential());
            
        _processor.ProcessEventAsync += ProcessAsync;
        _processor.ProcessErrorAsync += ErrorAsync;
    }
    
    protected override async Task ExecuteAsync(CancellationToken ct)
    {
        await _processor.StartProcessingAsync(ct);
        await Task.Delay(Timeout.Infinite, ct);
    }
    
    private async Task ProcessAsync(ProcessEventArgs args)
    {
        if (args.Data == null) return;
        
        var data = args.Data.EventBody.ToObjectFromJson<SensorData>();
        _logger.LogInformation("Partition {P}: Device {D}, Value: {V}", 
            args.Partition.PartitionId, data.DeviceId, data.Value);
        
        await args.UpdateCheckpointAsync();
    }
    
    private Task ErrorAsync(ProcessErrorEventArgs args)
    {
        _logger.LogError(args.Exception, "Partition {P} error", args.PartitionId);
        return Task.CompletedTask;
    }
}

public record SensorData(string DeviceId, double Value, DateTime Timestamp);
```

**appsettings.json:**
```json
{
  "EventHub": {
    "Namespace": "myeventhubns.servicebus.windows.net",
    "Name": "telemetry",
    "ConsumerGroup": "$Default"
  },
  "Storage": {
    "AccountName": "mystorageaccount",
    "CheckpointContainer": "checkpoints"
  }
}
```

**Wymagane RBAC:** `Azure Event Hubs Data Sender/Receiver`, `Storage Blob Data Contributor`

### Event Hub Capture

<img src="../assets/eventhub_capture.svg" alt="Event Hub Capture" />

Capture automatycznie archiwizuje strumień danych do Azure Storage lub ADLS Gen2:

```bash
# Włączenie Capture
az eventhubs eventhub update \
  --name myEventHub \
  --namespace-name myEventHubNS \
  --resource-group myRG \
  --enable-capture true \
  --capture-destination-name EventHubArchive.AzureBlockBlob \
  --storage-account mystorageaccount \
  --blob-container capture \
  --capture-interval 300 \
  --capture-size-limit 314572800
```

**Konfiguracja Capture:**
| Parametr | Opis | Wartości |
|----------|------|----------|
| Time Window | Interwał zapisu | 1-15 minut (default: 5) |
| Size Window | Rozmiar do zapisu | 10-500 MB (default: 300 MB) |
| Format | Format pliku | Apache Avro |
| Skip Empty | Pomijaj puste okna | true/false |

### Warstwy cenowe Event Hub

<img src="../assets/eventhub_tiers.svg" alt="Event Hub Tiers" />

| Cecha | Basic | Standard | Premium | Dedicated |
|-------|-------|----------|---------|-----------|
| **Throughput Units** | 1-20 | 1-40 (Auto-inflate) | 1-16 PU | 1-24 CU |
| **Partycje** | max 32 | max 32 | max 100 | max 2000 |
| **Retencja** | 1 dzień | 1-7 dni | do 90 dni | do 90 dni |
| **Consumer Groups** | 1 | 20 | 100 | bez limitu |
| **Capture** | NIE | TAK | TAK | TAK |
| **Kafka Protocol** | NIE | TAK | TAK | TAK |
| **VNet/Private Endpoint** | NIE | NIE | TAK | TAK |
| **Zone Redundancy** | NIE | NIE | TAK | TAK |
| **Cena (za jednostkę)** | ~$11/msc | ~$22/msc | ~$690/msc | ~$6900/msc |

### Integracja z Apache Kafka

Event Hub Standard i wyżej obsługuje protokół Apache Kafka bez zmian w kodzie aplikacji:

```csharp
// Konfiguracja Kafka dla Event Hub (używaj IConfiguration w produkcji!)
var config = new ProducerConfig
{
    BootstrapServers = configuration["EventHub:KafkaEndpoint"],  // myns.servicebus.windows.net:9093
    SecurityProtocol = SecurityProtocol.SaslSsl,
    SaslMechanism = SaslMechanism.Plain,
    SaslUsername = "$ConnectionString",
    SaslPassword = configuration.GetConnectionString("EventHub")  // Z Key Vault
};

using var producer = new ProducerBuilder<string, string>(config).Build();
await producer.ProduceAsync("myeventhub", new Message<string, string> 
{ 
    Key = "device001", 
    Value = "{\"temp\": 25.5}" 
});
```

> **Uwaga:** Kafka protocol wymaga connection string (nie obsługuje DefaultAzureCredential). Przechowuj go w Azure Key Vault!

### Event Hub vs Service Bus vs Event Grid

| Aspekt | Event Hub | Service Bus | Event Grid |
|--------|-----------|-------------|------------|
| **Scenariusz** | Big Data streaming | Enterprise messaging | Event routing |
| **Model** | Pub/Sub streaming | Queue/Topic | Pub/Sub reactive |
| **Przepustowość** | Miliony/sek | Tysiące/sek | Miliony/sek |
| **Rozmiar wiadomości** | 1 MB | 256 KB - 100 MB | 1 MB |
| **Retencja** | 1-90 dni | Do przetworzenia | 24h retry |
| **Kolejność** | FIFO per partition | FIFO (sessions) | Nie gwarantowana |
| **Protokół** | AMQP, Kafka, HTTP | AMQP, HTTP | HTTP, webhooks |
| **Typowe użycie** | IoT telemetry, logs | Orders, transactions | Blob events, alerts |

### Przykład: IoT Telemetry Pipeline

```csharp
// Kompletny pipeline: IoT -> Event Hub -> Processing -> Storage
public class IoTTelemetryPipeline
{
    private readonly EventHubProducerClient _producer;
    private readonly EventProcessorClient _processor;
    private readonly TableClient _tableClient;
    
    public async Task ProcessTelemetryAsync(ProcessEventArgs args)
    {
        var telemetry = args.Data.EventBody.ToObjectFromJson<DeviceTelemetry>();
        
        // Walidacja
        if (telemetry.Temperature > 100)
        {
            // Alert - przekroczono próg temperatury
            await SendAlertAsync(telemetry);
        }
        
        // Agregacja per minuta
        var aggregated = new TableEntity(
            partitionKey: telemetry.DeviceId,
            rowKey: telemetry.Timestamp.ToString("yyyyMMddHHmm"))
        {
            { "AvgTemp", telemetry.Temperature },
            { "MaxTemp", telemetry.Temperature },
            { "ReadingCount", 1 }
        };
        
        await _tableClient.UpsertEntityAsync(aggregated, TableUpdateMode.Merge);
        
        // Checkpoint
        await args.UpdateCheckpointAsync();
    }
}

public record DeviceTelemetry(
    string DeviceId, 
    double Temperature, 
    double Humidity, 
    DateTime Timestamp);
```

---

### FAQ - Egzamin

| Pytanie | Odpowiedź |
|---------|----------|
| Ile zdarzeń na sekundę obsługuje Event Hub? | Miliony |
| Czy Event Hub gwarantuje FIFO? | Tylko w obrębie partycji |
| Co to jest Consumer Group? | Niezależny widok strumienia dla konsumentów |
| Czy można zmienić liczbę partycji? | NIE, trzeba utworzyć nowy Event Hub |
| Co robi Capture? | Automatycznie archiwizuje do Blob/ADLS |
| Który tier obsługuje Kafka? | Standard i wyżej |
| Event Hub vs Service Bus? | Event Hub = streaming, Service Bus = messaging |

> **Egzamin:** Event Hub służy do Big Data streaming (miliony zdarzeń/sek). Partycje zapewniają skalowalność ale FIFO tylko per partition. Capture archiwizuje do storage. Kafka protocol wymaga Standard tier lub wyższego.

---
---

[Poprzednia strona](./25-service-bus.md) | [Spis treści](../README.md) | [Następna strona](./27-event-grid.md)
