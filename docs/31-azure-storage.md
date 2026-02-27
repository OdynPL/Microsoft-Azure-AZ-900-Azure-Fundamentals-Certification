<a id="sec-31-azure-storage"></a>
## 31. Azure Storage i Blob Storage

[ Powrót do spisu treści](../README.md)


Azure Storage to podstawowa usługa przechowywania danych w chmurze Azure.

### Storage Account

**Storage Account** to kontener dla usług storage z unikalną globalnie nazwą.

<img src="../assets/storage_account_overview.svg" alt="Azure Storage Account - 4 usługi">

### 4 usługi w Storage Account

| Usługa | Opis | Użycie |
|--------|------|--------|
| **Blob Storage** | Pliki binarne (REST API) | Obrazy, video, backupy |
| **File Storage** | Udziały SMB/NFS | File servery, lift-and-shift |
| **Queue Storage** | Kolejki wiadomości | Async processing |
| **Table Storage** | NoSQL (key-value) | Logi, IoT |

---

## Azure Blob Storage

Blob = **B**inary **L**arge **OB**ject - pliki, obrazy, video, backupy.

### Hierarchia

<img src="../assets/blob_hierarchy.svg" alt="Blob Storage - hierarchia">

```
Storage Account → Container → Blob
```

### 3 typy Blobów

| Typ | Max | Użycie |
|-----|-----|--------|
| **Block Blob** | 190 TB | Pliki, obrazy, video |
| **Append Blob** | 195 GB | Logi (dopisywanie) |
| **Page Blob** | 8 TB | Dyski VM (VHD) |

---

### Access Tiers

<img src="../assets/blob_access_tiers.svg" alt="Access Tiers">

| Tier | Min. czas | Użycie |
|------|-----------|--------|
| **Hot** | - | Częsty dostęp |
| **Cool** | 30 dni | Backup krótkoterminowy |
| **Cold** | 90 dni | Rzadki dostęp |
| **Archive** | 180 dni | OFFLINE, rehydratacja 1-15h |

---

### Redundancja

<img src="../assets/storage_redundancy.svg" alt="Redundancja">

| Typ | Kopie | Ochrona |
|-----|-------|----------|
| **LRS** | 3 | 1 datacenter |
| **ZRS** | 3 | 3 strefy (zone) |
| **GRS** | 6 | 2 regiony |
| **RA-GRS** | 6 | 2 regiony + read |

**Wskazówka:** Dev/Test → LRS, Produkcja → ZRS, DR → GRS

---

### Typy Storage Account

| Typ | Użycie |
|-----|--------|
| **Standard v2** | Większość zastosowań (HDD) |
| **Premium block blobs** | Low latency (SSD) |
| **Premium file shares** | Enterprise file shares |
| **Premium page blobs** | Dyski VM |

---

### Bezpieczeństwo

- **SSE** - szyfrowanie at-rest (AES-256, automatyczne)
- **HTTPS/TLS** - szyfrowanie in-transit
- **SAS** - token z ograniczonym dostępem
- **Access Keys** - pełny dostęp (2 klucze)
- **Private Endpoint** - dostęp przez VNet
- **Soft delete** - odzyskiwanie usuniętych danych

---

### Lifecycle Management

Automatyczne przenoszenie między tierami:

```json
{
  "rules": [{
    "name": "moveToArchive",
    "definition": {
      "actions": {
        "baseBlob": {
          "tierToCool": { "daysAfterModificationGreaterThan": 30 },
          "tierToArchive": { "daysAfterModificationGreaterThan": 90 }
        }
      }
    }
  }]
}
```

---

### C# - Azure.Storage.Blobs

```csharp
using Azure.Identity;
using Azure.Storage.Blobs;
using Azure.Storage.Blobs.Models;
using Microsoft.Extensions.Configuration;

public class BlobStorageService
{
    private readonly BlobContainerClient _containerClient;
    
    public BlobStorageService(IConfiguration config)
    {
        // Użyj DefaultAzureCredential (Managed Identity / Azure CLI)
        var blobUri = new Uri($"https://{config["Storage:AccountName"]}.blob.core.windows.net");
        var blobServiceClient = new BlobServiceClient(blobUri, new DefaultAzureCredential());
        _containerClient = blobServiceClient.GetBlobContainerClient(config["Storage:ContainerName"]!);
    }
    
    public async Task<Uri> UploadAsync(string blobName, Stream content, string contentType)
    {
        var blobClient = _containerClient.GetBlobClient(blobName);
        await blobClient.UploadAsync(content, new BlobHttpHeaders { ContentType = contentType });
        return blobClient.Uri;
    }
    
    public async Task<Stream> DownloadAsync(string blobName)
    {
        var blobClient = _containerClient.GetBlobClient(blobName);
        var response = await blobClient.DownloadStreamingAsync();
        return response.Value.Content;
    }
    
    public async IAsyncEnumerable<string> ListBlobsAsync(string? prefix = null)
    {
        await foreach (var blob in _containerClient.GetBlobsAsync(prefix: prefix))
            yield return blob.Name;
    }
    
    public async Task DeleteAsync(string blobName)
        => await _containerClient.GetBlobClient(blobName).DeleteIfExistsAsync();
}
```

**appsettings.json:**
```json
{
  "Storage": {
    "AccountName": "mystorageaccount",
    "ContainerName": "uploads"
  }
}
```

**NuGet:** `Azure.Storage.Blobs`, `Azure.Identity`

---

### C# - Azure.Storage.Queues

```csharp
using Azure.Identity;
using Azure.Storage.Queues;
using Azure.Storage.Queues.Models;
using System.Text.Json;

public class QueueService
{
    private readonly QueueClient _queueClient;
    
    public QueueService(IConfiguration config)
    {
        var queueUri = new Uri($"https://{config["Storage:AccountName"]}.queue.core.windows.net/{config["Storage:QueueName"]}");
        _queueClient = new QueueClient(queueUri, new DefaultAzureCredential());
    }
    
    public async Task SendAsync<T>(T message)
    {
        var json = JsonSerializer.Serialize(message);
        await _queueClient.SendMessageAsync(BinaryData.FromString(json));
    }
    
    public async Task<List<T>> ReceiveAsync<T>(int maxMessages = 5, TimeSpan? visibilityTimeout = null)
    {
        var messages = await _queueClient.ReceiveMessagesAsync(
            maxMessages, 
            visibilityTimeout ?? TimeSpan.FromMinutes(5));
        
        var results = new List<T>();
        foreach (var msg in messages.Value)
        {
            results.Add(JsonSerializer.Deserialize<T>(msg.Body.ToString())!);
            await _queueClient.DeleteMessageAsync(msg.MessageId, msg.PopReceipt);
        }
        return results;
    }
}
```

**NuGet:** `Azure.Storage.Queues`, `Azure.Identity`

---

### C# - Azure.Data.Tables (Table Storage)

```csharp
using Azure.Data.Tables;
using Azure.Identity;

public class ProductTableService
{
    private readonly TableClient _tableClient;
    
    public ProductTableService(IConfiguration config)
    {
        var tableUri = new Uri($"https://{config["Storage:AccountName"]}.table.core.windows.net");
        var serviceClient = new TableServiceClient(tableUri, new DefaultAzureCredential());
        _tableClient = serviceClient.GetTableClient("Products");
    }
    
    public async Task UpsertAsync(ProductEntity product)
        => await _tableClient.UpsertEntityAsync(product, TableUpdateMode.Replace);
    
    public async Task<ProductEntity?> GetAsync(string category, string productId)
    {
        try { return await _tableClient.GetEntityAsync<ProductEntity>(category, productId); }
        catch (Azure.RequestFailedException ex) when (ex.Status == 404) { return null; }
    }
    
    public async IAsyncEnumerable<ProductEntity> QueryAsync(string category, decimal minPrice)
    {
        var query = _tableClient.QueryAsync<ProductEntity>(
            $"PartitionKey eq '{category}' and Price gt {minPrice}");
        await foreach (var entity in query) yield return entity;
    }
}

public class ProductEntity : ITableEntity
{
    public string PartitionKey { get; set; } = "";  // Category
    public string RowKey { get; set; } = "";        // ProductId
    public string Name { get; set; } = "";
    public double Price { get; set; }
    public DateTimeOffset? Timestamp { get; set; }
    public ETag ETag { get; set; }
}
```

**NuGet:** `Azure.Data.Tables`, `Azure.Identity`

---

### Static Website

Blob Storage może hostować statyczne strony (HTML, CSS, JS).

**URL:** `https://<account>.z6.web.core.windows.net/`

---

### Blob vs Files vs Data Lake

| Cecha | Blob | Files | Data Lake Gen2 |
|-------|------|-------|----------------|
| **Protokół** | REST | SMB/NFS | REST + HNS |
| **Użycie** | Aplikacje web | File servery | Big Data |
| **ACL** | Container | Share | Plik/folder |

---

### Best Practices

- Odpowiedni tier: Hot dla aktywnych, Archive dla archiwum
- Lifecycle Management dla automatyzacji
- ZRS/GRS dla produkcji (LRS tylko dev/test)
- Soft delete dla ochrony przed usunięciem
- Private Endpoint dla wrażliwych danych

---

### FAQ - Egzamin

| Pytanie | Odpowiedź |
|---------|----------|
| Co zawiera Storage Account? | Blob, File, Queue, Table |
| Ile kopii tworzy LRS? | 3 w 1 datacenter |
| Ile kopii tworzy GRS? | 6 (2 regiony) |
| Który tier jest OFFLINE? | Archive |
| Block vs Page Blob? | Block = pliki, Page = VHD |
| Co to HNS? | Data Lake Gen2 |

---
---

[Poprzednia strona](./30-data-factory.md) | [Spis treści](../README.md) | [Następna strona](./32-app-service.md)
