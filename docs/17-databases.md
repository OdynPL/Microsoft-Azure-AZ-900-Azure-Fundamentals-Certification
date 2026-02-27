<a id="sec-17-databases"></a>
## 17. Bazy danych (Databases)

[ Powrót do spisu treści](../README.md)


<img src="../assets/databases.svg" alt="Azure Databases - SQL, Cosmos DB, PostgreSQL, MySQL">

Azure oferuje kilka modeli baz danych dostępnych jako IaaS, PaaS lub globalne, skalowalne systemy NoSQL. Poniżej najważniejsze usługi wymagane na poziomie AZ‑900.

### **Azure SQL Database (Single / Elastic Pool)**
W pełni zarządzana baza danych SQL w modelu PaaS.

- automatyczne backupy, patchowanie, aktualizacje
- wysokie SLA, autoskalowanie, brak zarządzania OS
- modele: Single Database lub Elastic Pool (wspólne zasoby dla wielu DB)
- najlepsza opcja dla nowych aplikacji cloud‑native
- brak pełnego wsparcia SQL Agent / cross‑instance features (w porównaniu z MI)

### **SQL Managed Instance (MI)**
Zarządzana instancja SQL z niemal 100% zgodnością z SQL Server on‑prem.

- wsparcie dla SQL Agent, Linked Servers, Service Broker
- PaaS z automatyzacją, ale zachowuje funkcje instancyjne
- idealny do migracji aplikacji, które wymagają pełnej zgodności z SQL Server
- sieciowo wymaga VNet (bez public endpoint domyślnie)

### **SQL Server on Virtual Machine (SQL on VM)**
Pełny SQL Server działający na maszynie wirtualnej (IaaS).

- pełna kontrola nad OS, wersją SQL, konfiguracją, agentami
- wymaga własnego patchowania, backupów, HA
- najlepsze dla scenariuszy, które wymagają pełnego dostępu do instancji lub niestandardowych rozszerzeń

#### C# - Microsoft.Data.SqlClient

```csharp
using Azure.Identity;
using Microsoft.Data.SqlClient;
using Microsoft.Extensions.Configuration;

public class ProductRepository
{
    private readonly string _connectionString;
    
    public ProductRepository(IConfiguration configuration)
    {
        // Connection string z appsettings.json lub Azure Key Vault
        _connectionString = configuration.GetConnectionString("SqlDatabase")
            ?? throw new InvalidOperationException("Brak ConnectionString");
    }
    
    public async Task<List<Product>> GetExpensiveProductsAsync(decimal minPrice)
    {
        await using var connection = new SqlConnection(_connectionString);
        await connection.OpenAsync();
        
        await using var command = new SqlCommand(
            "SELECT Id, Name, Price FROM Products WHERE Price > @price", connection);
        command.Parameters.AddWithValue("@price", minPrice);
        
        var products = new List<Product>();
        await using var reader = await command.ExecuteReaderAsync();
        while (await reader.ReadAsync())
        {
            products.Add(new Product(
                reader.GetInt32(0),
                reader.GetString(1),
                reader.GetDecimal(2)));
        }
        return products;
    }
}

public record Product(int Id, string Name, decimal Price);
```

**appsettings.json:**
```json
{
  "ConnectionStrings": {
    "SqlDatabase": "Server=myserver.database.windows.net;Database=mydb;Authentication=Active Directory Default;"
  }
}
```

**Best Practice:** Użyj `Authentication=Active Directory Default` z Managed Identity zamiast User/Password!

**NuGet:** `Microsoft.Data.SqlClient`, `Azure.Identity`

### **Cosmos DB (NoSQL, global distribution)**

#### Spis treści - Cosmos DB
- [Co to jest Cosmos DB?](#cosmosdb-intro)
- [Architektura globalna](#cosmosdb-architecture)
- [Multi-Model APIs](#cosmosdb-apis)
- [Request Units (RU/s)](#cosmosdb-ru)
- [Consistency Levels](#cosmosdb-consistency)
- [Partitioning](#cosmosdb-partitioning)
- [Porównanie z innymi bazami](#cosmosdb-comparison)
- [Kiedy wybrać Cosmos DB?](#cosmosdb-when)

---

<a id="cosmosdb-intro"></a>
#### Co to jest Cosmos DB?

<img src="../assets/cosmosdb_architecture.svg" alt="Cosmos DB Architecture - globalna architektura">

**Azure Cosmos DB** to w pełni zarządzana, globalnie rozproszona baza danych NoSQL, zaprojektowana dla aplikacji wymagających:
- **Ultra-niskich opóźnień** (single-digit milliseconds)
- **Globalnej dystrybucji** danych w wielu regionach
- **Elastycznego skalowania** bez przestojów
- **Wysokiej dostępności** (99.999% SLA)

Cosmos DB to **baza planet-scale** — idealna dla aplikacji działających globalnie, IoT, gaming, e-commerce i real-time analytics.

---

<a id="cosmosdb-architecture"></a>
#### Architektura globalna

Cosmos DB automatycznie replikuje dane do wybranych regionów Azure:

| Funkcja | Opis |
|---------|------|
| **Multi-region writes** | Zapis w dowolnym regionie, automatyczna synchronizacja |
| **Automatic failover** | Przełączenie w przypadku awarii regionu |
| **Conflict resolution** | Last-Write-Wins lub custom policies |
| **Turnkey global distribution** | Dodanie regionu jednym kliknięciem |

**Przykład:** Użytkownik w Europie łączy się z West Europe, użytkownik w Azji z Southeast Asia — obaj mają dostęp do tych samych danych z minimalnym opóźnieniem.

---

<a id="cosmosdb-apis"></a>
#### Multi-Model APIs

<img src="../assets/cosmosdb_apis.svg" alt="Cosmos DB APIs - SQL, MongoDB, Cassandra, Gremlin, Table">

Cosmos DB obsługuje **5 różnych interfejsów API** — wybierasz ten, który pasuje do Twojej aplikacji:

| API | Model danych | Kiedy używać |
|-----|--------------|--------------|
| **Core (SQL)** | Dokumenty JSON | Nowe aplikacje, najlepsza wydajność |
| **MongoDB** | Dokumenty BSON | Migracja z MongoDB, istniejący kod |
| **Cassandra** | Wide-column | Migracja z Apache Cassandra |
| **Gremlin** | Graf (węzły/krawędzie) | Social networks, recommendation engines |
| **Table** | Key-Value | Prosta struktura, migracja z Table Storage |

**Ważne:** Wybór API następuje **przy tworzeniu konta** Cosmos DB i nie można go później zmienić!

---

<a id="cosmosdb-ru"></a>
#### Request Units (RU/s) — Model rozliczeń

Cosmos DB używa **Request Units (RU)** jako abstrakcyjnej jednostki kosztu operacji:

```
1 RU = koszt odczytu 1 dokumentu 1KB przez ID
```

**Przykładowe koszty RU:**
| Operacja | Koszt RU |
|----------|----------|
| Odczyt 1KB dokumentu po ID | ~1 RU |
| Zapis 1KB dokumentu | ~5 RU |
| Query z filtrem | ~3-10+ RU |
| Query z agregacją | ~10-100+ RU |

**Tryby provisioning:**
- **Provisioned throughput** — deklarujesz RU/s z góry (tańsze przy przewidywalnym ruchu)
- **Autoscale** — automatyczne skalowanie 10-100% zadeklarowanego max
- **Serverless** — płacisz tylko za zużyte RU (dobre dla dev/test)

---

<a id="cosmosdb-consistency"></a>
#### Consistency Levels (Poziomy spójności)

Cosmos DB oferuje **5 poziomów spójności** — balans między wydajnością a gwarancjami:

| Poziom | Gwarancja | Latency | Kiedy używać |
|--------|-----------|---------|--------------|
| **Strong** | Zawsze najnowsze dane | Najwyższa | Finanse, krytyczne dane |
| **Bounded Staleness** | Max opóźnienie K operacji lub T czasu | Wysoka | Gaming leaderboards |
| **Session** | Spójność w ramach sesji | Średnia | **Domyślny, najczęściej używany** |
| **Consistent Prefix** | Kolejność operacji zachowana | Niska | Analityka, logi |
| **Eventual** | Brak gwarancji kolejności | Najniższa | Liczniki, statystyki |

**Session consistency** zapewnia, że użytkownik widzi swoje własne zmiany natychmiast, a zmiany innych użytkowników z minimalnym opóźnieniem.

---

<a id="cosmosdb-partitioning"></a>
#### Partitioning — Klucz do wydajności

Cosmos DB automatycznie **partycjonuje dane** na podstawie **Partition Key**:

```json
{
  "id": "order-123",
  "customerId": "CUST-456",  // ← Partition Key
  "items": [...],
  "total": 99.99
}
```

**Zasady wyboru Partition Key:**
| Dobre praktyki | Złe praktyki |
|----------------|---------------|
| Wysoka kardynalność (dużo unikalnych wartości) | Status: "active/inactive" (tylko 2 wartości) |
| Query najczęściej filtrują po tym kluczu | Timestamp (hot partition) |
| Równomierna dystrybucja danych | ID użytkownika VIP (80% danych w 1 partycji) |

**Limit:** Jedna partycja logiczna = max **20 GB** danych.

---

<a id="cosmosdb-comparison"></a>
#### Porównanie z innymi bazami NoSQL w Azure

<img src="../assets/cosmosdb_comparison.svg" alt="Cosmos DB - porownanie z innymi bazami NoSQL">

| Cecha | Cosmos DB | Table Storage | Redis Cache | MongoDB Atlas |
|-------|-----------|---------------|-------------|---------------|
| **Typ** | Multi-model NoSQL | Key-Value | In-memory cache | Document DB |
| **Global dist.** | Tak (native) | Nie (single region) | Premium only | Tak (self-managed) |
| **Latency SLA** | <10ms P99 | Brak | <1ms | Brak |
| **Multi-region write** | Tak | Nie | Nie | Tak |
| **SLA availability** | 99.999% | 99.9% | 99.9% | 99.95% |
| **Koszt** | Wysoki | Niski | Średni | Średni |

---

<a id="cosmosdb-when"></a>
#### Kiedy wybrać Cosmos DB?

**Wybierz Cosmos DB gdy:**
- Potrzebujesz **globalnej dystrybucji** z niskimi opóźnieniami
- Aplikacja wymaga **99.999% SLA**
- Masz **zmienny, nieprzewidywalny ruch** (autoscale)
- Potrzebujesz **wielu modeli danych** (dokumenty, grafy, key-value)
- Budujesz aplikacje **IoT, gaming, e-commerce, real-time**

**Nie wybieraj Cosmos DB gdy:**
- Potrzebujesz prostego, taniego storage → **Table Storage**
- Potrzebujesz cache w pamięci → **Redis Cache**
- Masz relacyjne dane z transakcjami ACID → **Azure SQL**
- Budżet jest bardzo ograniczony (Cosmos DB jest drogi!)

---

#### Podsumowanie Cosmos DB

| Aspekt | Wartość |
|--------|---------|
| **Typ** | Globally distributed NoSQL |
| **APIs** | Core SQL, MongoDB, Cassandra, Gremlin, Table |
| **SLA** | 99.999% (multi-region) |
| **Latency** | <10ms reads, <15ms writes (P99) |
| **Scaling** | Automatic partitioning, unlimited |
| **Pricing** | RU/s + Storage (pay-per-use) |
| **Best for** | Global apps, IoT, gaming, e-commerce |

---

#### C# - Microsoft.Azure.Cosmos

```csharp
using Azure.Identity;
using Microsoft.Azure.Cosmos;
using Microsoft.Extensions.Configuration;

public class OrderRepository
{
    private readonly Container _container;
    
    public OrderRepository(IConfiguration configuration)
    {
        // Użyj DefaultAzureCredential (Managed Identity / Azure CLI / VS)
        var endpoint = configuration["CosmosDb:Endpoint"]!;
        var client = new CosmosClient(endpoint, new DefaultAzureCredential(),
            new CosmosClientOptions { ApplicationRegion = Regions.WestEurope });
        
        _container = client.GetContainer(
            configuration["CosmosDb:Database"]!, 
            configuration["CosmosDb:Container"]!);
    }
    
    public async Task<Order> CreateAsync(Order order)
    {
        var response = await _container.CreateItemAsync(order, new PartitionKey(order.CustomerId));
        Console.WriteLine($"RU used: {response.RequestCharge}");
        return response.Resource;
    }
    
    public async Task<Order?> GetAsync(string id, string customerId)
    {
        try
        {
            var response = await _container.ReadItemAsync<Order>(id, new PartitionKey(customerId));
            return response.Resource;
        }
        catch (CosmosException ex) when (ex.StatusCode == System.Net.HttpStatusCode.NotFound)
        {
            return null;
        }
    }
    
    public async IAsyncEnumerable<Order> QueryAsync(decimal minTotal)
    {
        var query = _container.GetItemQueryIterator<Order>(
            new QueryDefinition("SELECT * FROM c WHERE c.total > @min ORDER BY c.total DESC")
                .WithParameter("@min", minTotal));
        
        while (query.HasMoreResults)
        {
            foreach (var item in await query.ReadNextAsync())
                yield return item;
        }
    }
}

public record Order(string Id, string CustomerId, decimal Total, List<string> Items);
```

**appsettings.json:**
```json
{
  "CosmosDb": {
    "Endpoint": "https://myaccount.documents.azure.com:443/",
    "Database": "OrdersDB",
    "Container": "Orders"
  }
}
```

**NuGet:** `Microsoft.Azure.Cosmos`, `Azure.Identity`

---

### **Azure Database for PostgreSQL / MySQL (Flexible Server)**
Zarządzane instancje popularnych baz open‑source.

- automatyczne backupy, maintenance, skalowanie
- tryby: Single Server (legacy) i Flexible Server (zalecany)
- idealne dla aplikacji korzystających z open‑source DB

---
---

[Poprzednia strona](./16-sla.md) | [Spis treści](../README.md) | [Następna strona](./18-subscription-models.md)
