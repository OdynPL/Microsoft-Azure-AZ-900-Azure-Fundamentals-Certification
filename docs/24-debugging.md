<a id="sec-24-debugging"></a>
## 24. Debugowanie aplikacji Azure

[ Powrót do spisu treści](../README.md)


<img src="../assets/debug_monitoring_flow.svg" alt="Azure Debugging - przeplyw monitoringu">

### Przegląd narzędzi do debugowania

Azure oferuje bogaty zestaw narzędzi do monitorowania, diagnostyki i debugowania aplikacji działających w chmurze.

---

### Application Insights

Główne narzędzie do monitorowania wydajności i diagnostyki aplikacji (APM - Application Performance Management).

**Co zbiera:**
- **Requests** - przychodzące żądania HTTP, czas odpowiedzi, status code
- **Dependencies** - wywołania do SQL, HTTP, Azure services
- **Exceptions** - nieobsłużone wyjątki z pełnym stack trace
- **Traces** - logi aplikacji (ILogger, Console.WriteLine)
- **Performance counters** - CPU, pamięć, GC
- **Custom events/metrics** - własne zdarzenia biznesowe

**Kluczowe funkcje:**

| Funkcja | Opis |
|---------|------|
| **Live Metrics** | Real-time monitoring (1s refresh) |
| **Application Map** | Wizualizacja zależności i flow |
| **Transaction Search** | Szukanie konkretnych requestów |
| **Failures** | Analiza błędów i wyjątków |
| **Performance** | Analiza czasów odpowiedzi |
| **Availability** | Testy ping z różnych lokalizacji |
| **Smart Detection** | Automatyczne wykrywanie anomalii |

**Konfiguracja w App Service:**

```bash
# Włączenie Application Insights dla App Service
az webapp config appsettings set --name myApp \
    --resource-group myRG \
    --settings APPLICATIONINSIGHTS_CONNECTION_STRING="InstrumentationKey=xxx;IngestionEndpoint=https://..."

# Lub przez extension
az monitor app-insights component create --app myAppInsights \
    --resource-group myRG --location westeurope

az webapp config appsettings set --name myApp \
    --resource-group myRG \
    --settings APPINSIGHTS_INSTRUMENTATIONKEY=$(az monitor app-insights component show \
        --app myAppInsights --resource-group myRG --query instrumentationKey -o tsv)
```

**Integracja w kodzie (.NET):**

```csharp
// Program.cs (.NET 6+)
builder.Services.AddApplicationInsightsTelemetry();

// Logowanie custom events
public class OrderController : ControllerBase
{
    private readonly TelemetryClient _telemetry;
    
    public OrderController(TelemetryClient telemetry)
    {
        _telemetry = telemetry;
    }
    
    [HttpPost]
    public IActionResult CreateOrder(Order order)
    {
        // Custom event
        _telemetry.TrackEvent("OrderCreated", new Dictionary<string, string>
        {
            { "OrderId", order.Id.ToString() },
            { "CustomerId", order.CustomerId }
        });
        
        // Custom metric
        _telemetry.TrackMetric("OrderValue", order.TotalAmount);
        
        return Ok();
    }
}
```

---

### Log Analytics i KQL

Centralne repozytorium logów z możliwością zaawansowanych zapytań.

**Przykłady zapytań KQL:**

```kusto
// Wszystkie wyjatki z ostatniej godziny
exceptions
| where timestamp > ago(1h)
| project timestamp, problemId, outerMessage, innermostMessage
| order by timestamp desc

// Top 10 najwolniejszych requestow
requests
| where timestamp > ago(24h)
| where success == true
| summarize avg(duration), count() by name
| top 10 by avg_duration desc

// Bledy HTTP 5xx
requests
| where timestamp > ago(1h)
| where resultCode startswith "5"
| summarize count() by bin(timestamp, 5m), resultCode
| render timechart

// Dependency failures (SQL, HTTP)
dependencies
| where timestamp > ago(1h)
| where success == false
| summarize count() by type, target, resultCode
| order by count_ desc

// Korelacja request -> dependencies -> exceptions
requests
| where timestamp > ago(1h)
| where success == false
| join kind=leftouter (dependencies | where success == false) on operation_Id
| join kind=leftouter (exceptions) on operation_Id
| project timestamp, request_name=name, dep_type=type, exception=outerMessage
```

---

### Kudu / SCM Console

Zaawansowana konsola diagnostyczna dla App Service.

**Dostęp:** `https://yourapp.scm.azurewebsites.net`

**Funkcje:**

| Funkcja | Opis |
|---------|------|
| **Debug Console** | CMD/PowerShell w przegladarce |
| **Process Explorer** | Lista procesow, handles, threads |
| **Log Stream** | Podglad logow w czasie rzeczywistym |
| **File Manager** | Przegladanie plikow aplikacji |
| **Deployment Logs** | Historia deploymentow |
| **Environment** | Zmienne srodowiskowe, wersje runtime |
| **Site Extensions** | Instalacja rozszerzen (np. profiler) |

**Przydatne ścieżki w Kudu:**

```
/api/dump                    - Memory dump
/api/processes               - Lista procesów
/api/processes/0/threads     - Threads głównego procesu
/api/vfs/                    - Virtual File System (browse files)
/api/logstream               - Log streaming endpoint
/DebugConsole                - Interaktywna konsola
```

---

### Remote Debugging

<img src="../assets/debug_tools_interactive.svg" alt="Remote Debugging - interaktywne debugowanie">

Debugowanie aplikacji bezpośrednio z Visual Studio.

**Włączenie Remote Debugging:**

```bash
# Włącz remote debugging dla App Service
az webapp config set --name myApp \
    --resource-group myRG \
    --remote-debugging-enabled true

# Sprawdź status
az webapp config show --name myApp \
    --resource-group myRG \
    --query remoteDebuggingEnabled
```

**W Visual Studio:**
1. View -> Cloud Explorer
2. Znajdź App Service
3. Right-click -> Attach Debugger
4. Ustaw breakpoint w kodzie
5. Wywołaj request do aplikacji

> **Uwaga:** Remote debugging spowalnia aplikacje! Używaj tylko w Dev/Test, NIE na produkcji.

---

### Snapshot Debugger

<img src="../assets/debug_tools_diagnostic.svg" alt="Snapshot Debugger - diagnostyka produkcyjna">

Przechwytuje stan aplikacji w momencie wyjątku - bez zatrzymywania produkcji.

**Jak działa:**
1. Aplikacja rzuca wyjątek
2. Snapshot Debugger przechwytuje:
   - Call stack
   - Wartości zmiennych lokalnych
   - Stan obiektów
3. Snapshot dostępny w Application Insights -> Failures

**Włączenie:**

```csharp
// NuGet: Microsoft.ApplicationInsights.SnapshotCollector

// Program.cs
builder.Services.AddApplicationInsightsTelemetry();
builder.Services.AddSnapshotCollector();
```

**appsettings.json:**
```json
{
  "ApplicationInsights": {
    "ConnectionString": "InstrumentationKey=xxx;..."
  },
  "SnapshotCollector": {
    "IsEnabled": true,
    "ThresholdForSnapshotting": 1,
    "MaximumSnapshotsRequired": 3
  }
}
```

---

### App Service Diagnostics

Wbudowany system diagnostyki "Diagnose and solve problems".

**Kategorie:**

| Kategoria | Co analizuje |
|-----------|-------------|
| **Availability** | Downtime, restarts, health checks |
| **Performance** | High CPU, memory, slow responses |
| **Configuration** | App settings, connection strings |
| **SSL/Certificates** | Certificate issues |
| **Deployment** | Deployment failures |

**Auto-Heal Rules:**

Automatyczne akcje naprawcze:

```bash
# Restart przy wysokim CPU
az webapp config set --name myApp \
    --resource-group myRG \
    --auto-heal-enabled true

# Konfiguracja reguł (w Portal lub ARM template)
# Triggers: slowRequests, memoryLimit, requestCount, statusCodes
# Actions: recycle, logEvent, customAction
```

---

### Log Streaming

Podgląd logów w czasie rzeczywistym.

**Azure CLI:**

```bash
# Stream logów App Service
az webapp log tail --name myApp --resource-group myRG

# Stream logów Functions
az functionapp log stream --name myFunc --resource-group myRG

# Włącz logowanie do filesystem
az webapp log config --name myApp \
    --resource-group myRG \
    --application-logging filesystem \
    --level verbose
```

**W Portal:** App Service -> Monitoring -> Log stream

---

### Health Checks

Monitorowanie zdrowia aplikacji przez Azure.

```bash
# Konfiguracja health check endpoint
az webapp config set --name myApp \
    --resource-group myRG \
    --generic-configurations '{"healthCheckPath": "/health"}'
```

**Endpoint w aplikacji (.NET):**

```csharp
// Program.cs
builder.Services.AddHealthChecks()
    .AddSqlServer(connectionString)
    .AddRedis(redisConnection)
    .AddUrlGroup(new Uri("https://api.external.com"), "external-api");

app.MapHealthChecks("/health");
```

---

### Alerts i powiadomienia

<img src="../assets/debug_alerts.svg" alt="Azure Alerts - alerty i powiadomienia">

```bash
# Alert na wysokie CPU
az monitor metrics alert create --name HighCPUAlert \
    --resource-group myRG \
    --scopes /subscriptions/.../resourceGroups/myRG/providers/Microsoft.Web/sites/myApp \
    --condition "avg CpuPercentage > 80" \
    --window-size 5m \
    --evaluation-frequency 1m \
    --action /subscriptions/.../resourceGroups/myRG/providers/microsoft.insights/actionGroups/myActionGroup

# Alert na bledy HTTP 5xx (KQL)
az monitor scheduled-query create --name Http5xxAlert \
    --resource-group myRG \
    --scopes /subscriptions/.../resourceGroups/myRG/providers/microsoft.insights/components/myAppInsights \
    --condition "count > 10" \
    --condition-query "requests | where resultCode startswith '5'" \
    --window-size 5m
```

---

### Porównanie narzędzi

| Narzędzie | Use case | Wpływ na perf | Produkcja? |
|-----------|----------|---------------|------------|
| **Application Insights** | APM, metryki, logi | Minimalny | TAK |
| **Log Analytics** | Zaawansowane zapytania | Brak | TAK |
| **Remote Debugging** | Interaktywne debugowanie | WYSOKI | NIE |
| **Snapshot Debugger** | Debug exceptions | Minimalny | TAK |
| **Kudu Console** | Diagnostyka, pliki | Brak | Ostrożnie |
| **Log Streaming** | Real-time logi | Niski | TAK |
| **App Diagnostics** | Troubleshooting | Brak | TAK |

---

### Best Practices

| Praktyka | Opis |
|----------|------|
| **Włącz App Insights** | Dla każdej aplikacji produkcyjnej |
| **Structured logging** | Używaj ILogger z parametrami |
| **Correlation IDs** | Przekazuj przez cały pipeline |
| **Health checks** | Endpoint /health dla LB i Azure |
| **Alerts** | Na kluczowe metryki i błędy |
| **Retention policy** | Dostosuj do wymagań compliance |
| **Sampling** | Dla wysokiego ruchu (redukuje koszty) |

> **Egzamin:** Application Insights to główne narzędzie APM w Azure. Log Analytics umożliwia zapytania KQL. Remote debugging NIE powinno być używane na produkcji.

---
---

[Poprzednia strona](./23-azure-key-vault.md) | [Spis treści](../README.md) | [Następna strona](./25-service-bus.md)
