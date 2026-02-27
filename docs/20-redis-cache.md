<a id="sec-20-redis-cache"></a>
## 20. Azure Cache for Redis

[ Powrót do spisu treści](../README.md)


**Azure Cache for Redis** to w pełni zarządzana usługa cache w pamięci, oparta na popularnym silniku Redis. Umożliwia drastyczne przyspieszenie aplikacji poprzez przechowywanie często używanych danych w szybkiej pamięci zamiast odpytywania bazy danych.

<img src="../assets/redis_cache_overview.svg" alt="Azure Cache for Redis - jak dziala cache" width="100%">

### 20.1 Po co uzywac cache?

| Problem | Rozwiazanie z Redis |
|---------|--------------------|
| Wolne odpowiedzi z bazy danych | Cache przechowuje dane w pamieci (1-5ms vs 100-500ms) |
| Wysokie obciazenie bazy | Redis przejmuje 80-90% odczytow |
| Sesje uzytkownikow rozrzucone | Centralne przechowywanie sesji |
| Kosztowne obliczenia | Cache wynikow, nie powtarzaj obliczen |
| Limity połączeń do DB | Mniej zapytań = mniej połączeń |

### 20.2 Typowe przypadki uzycia

**1. Cache danych (Data Caching)**
- Wyniki zapytan SQL/NoSQL
- Odpowiedzi z zewnetrznych API
- Dane konfiguracyjne

**2. Session Store**
- Sesje uzytkownikow dla load-balanced aplikacji
- Koszyki zakupowe
- Tokeny uwierzytelniania

**3. Message Broker**
- Pub/Sub dla real-time powiadomien
- Kolejki zadan (job queues)
- Event streaming

**4. Leaderboards / Counting**
- Rankingi w grach (sorted sets)
- Liczniki odwiedzin, lajkow
- Rate limiting

### 20.3 Warstwy cenowe (Tiers)

<img src="../assets/redis_tiers.svg" alt="Azure Cache for Redis - warstwy cenowe" width="100%">

| Tier | SLA | Replikacja | Persistence | Clustering | Kiedy uzywac |
|------|-----|------------|-------------|------------|-------------|
| **Basic** | Brak | Brak | Nie | Nie | Dev/Test |
| **Standard** | 99.9% | Primary + Replica | Nie | Nie | Wiekszosc produkcji |
| **Premium** | 99.9% | P + R + Geo | RDB/AOF | Do 10 shardow | Duze obciazenia |
| **Enterprise** | 99.999% | Active-Active | Pełna | Tak | Mission-critical |

### 20.4 Kluczowe funkcje

**Persistence (tylko Premium+):**
- **RDB** - snapshoty w regularnych odstepach
- **AOF** - zapisuje kazda operacje zapisu (wiecej danych, mniej utraty)

**Clustering (tylko Premium+):**
- Rozdziela dane na wiele shardow
- Do 1.2 TB danych (Premium) / 14 TB (Enterprise)
- Automatyczne resharding

**Geo-replication:**
- Premium: Active-Passive (DR)
- Enterprise: Active-Active (multi-region write)

**VNet Integration (Premium+):**
- Redis w prywatnej sieci
- Brak publicznego IP
- Private Endpoints

### 20.5 Wzorce cache - szczegoly

**Cache-Aside (Lazy Loading):**

<img src="../assets/cache_aside_pattern.svg" alt="Cache-Aside Pattern" width="100%">

Najpopularniejszy wzorzec. Aplikacja jest odpowiedzialna za logike cache:
1. App sprawdza cache
2. **Cache HIT** → zwroc dane natychmiast (1-5ms)
3. **Cache MISS** → czytaj z DB → zapisz do cache → zwroc

| Zalety | Wady |
|--------|------|
| Tylko potrzebne dane w cache | Pierwsze zapytanie zawsze wolne (cold start) |
| Odporne na awarie cache | Dane moga byc nieaktualne (stale data) |
| Proste w implementacji | Wymaga logiki w aplikacji |

---

**Write-Through (zapis synchroniczny):**

<img src="../assets/write_through_pattern.svg" alt="Write-Through Pattern" width="100%">

Cache dziala jako posrednik przy zapisie:
1. App zapisuje do cache
2. Cache **synchronicznie** zapisuje do DB (App czeka!)
3. Potwierdzenie dla App

| Zalety | Wady |
|--------|------|
| Dane zawsze aktualne w cache i DB | Wyzsza latencja zapisu |
| Brak niespojnosci | Kazdy zapis to 2 operacje |
| Proste odczyty | Wymaga wsparcia cache |

---

**Write-Behind / Write-Back (zapis asynchroniczny):**

<img src="../assets/write_behind_pattern.svg" alt="Write-Behind Pattern" width="100%">

Najszybszy zapis - cache potwierdza natychmiast:
1. App zapisuje do cache
2. Cache **natychmiast** potwierdza (App nie czeka!)
3. Cache **asynchronicznie** zapisuje do DB (pozniej, w tle)

| Zalety | Wady |
|--------|------|
| Najszybszy zapis | Ryzyko utraty danych przy awarii |
| Niskie latencje | Dane moga byc niezsynchronizowane |
| Batching zapisow do DB | Zlozona implementacja |

---

### 20.6 Tworzenie przez CLI

```bash
# Utworzenie Resource Group
az group create --name myRG --location westeurope

# Utworzenie Redis Cache (Standard C1)
az redis create --name myrediscache --resource-group myRG \
    --location westeurope --sku Standard --vm-size C1

# Pobranie connection string
az redis list-keys --name myrediscache --resource-group myRG

# Wynik:
# primaryKey: "abc123..."
# secondaryKey: "xyz789..."
```

**Connection string format:**
```
myrediscache.redis.cache.windows.net:6380,password=<primaryKey>,ssl=True,abortConnect=False
```

### 20.7 Przykład użycia w kodzie (.NET)

```csharp
using Microsoft.Extensions.Configuration;
using StackExchange.Redis;
using System.Text.Json;

public class RedisCacheService
{
    private readonly IDatabase _db;
    
    public RedisCacheService(IConfiguration configuration)
    {
        // Connection string z appsettings.json (Secret Manager / Key Vault w produkcji)
        var connectionString = configuration.GetConnectionString("Redis")
            ?? throw new InvalidOperationException("Brak Redis ConnectionString");
        
        var redis = ConnectionMultiplexer.Connect(connectionString);
        _db = redis.GetDatabase();
    }
    
    public async Task SetAsync<T>(string key, T value, TimeSpan? expiry = null)
    {
        var json = JsonSerializer.Serialize(value);
        await _db.StringSetAsync(key, json, expiry ?? TimeSpan.FromMinutes(30));
    }
    
    public async Task<T?> GetAsync<T>(string key)
    {
        var cached = await _db.StringGetAsync(key);
        return cached.HasValue 
            ? JsonSerializer.Deserialize<T>(cached!) 
            : default;
    }
    
    public async Task<T> GetOrSetAsync<T>(string key, Func<Task<T>> factory, TimeSpan? expiry = null)
    {
        var cached = await GetAsync<T>(key);
        if (cached is not null) return cached;
        
        var value = await factory();
        await SetAsync(key, value, expiry);
        return value;
    }
}

// Użycie z DI
services.AddSingleton<RedisCacheService>();

// W kontrolerze
var user = await _cache.GetOrSetAsync($"user:{id}", 
    () => _userRepository.GetByIdAsync(id),
    TimeSpan.FromMinutes(15));
```

**appsettings.json:**
```json
{
  "ConnectionStrings": {
    "Redis": "myrediscache.redis.cache.windows.net:6380,password=ACCESS_KEY,ssl=True,abortConnect=False"
  }
}
```

**Best Practice:** W produkcji użyj Azure Key Vault lub Managed Identity (Premium tier)!

### 20.8 Best Practices

**Klucze:**
- Uzywaj prefixow: `user:123`, `product:456`, `session:abc`
- Unikaj bardzo długich kluczy (max 1KB, optymalnie < 100 bajtów)

**TTL (Time To Live):**
- Zawsze ustawiaj TTL dla danych cache
- Typowe wartosci: 5-60 minut dla danych, 30 min - 24h dla sesji

**Eviction Policy:**
- `volatile-lru` - usun najstarsze klucze z TTL (domyslne)
- `allkeys-lru` - usuń najstarsze ze wszystkich kluczy
- `noeviction` - blad gdy brak pamieci

**Monitoring:**
- Azure Monitor dla metryk (hits, misses, memory)
- Cache Hit Ratio powinna byc > 90%
- Alerts na wysokie uzycie pamieci

---
---

[Poprzednia strona](./19-azure-cli.md) | [Spis treści](../README.md) | [Następna strona](./21-deployment.md)
