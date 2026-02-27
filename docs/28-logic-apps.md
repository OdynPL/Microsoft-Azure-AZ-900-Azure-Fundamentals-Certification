<a id="sec-28-logic-apps"></a>
## 28. Azure Logic Apps

[ Powrót do spisu treści](../README.md)


Azure Logic Apps to w pełni zarządzana usługa integracyjna (iPaaS), która pozwala automatyzować przepływy pracy (workflows) i łączyć aplikacje, dane oraz usługi - bez pisania kodu lub z minimalnym kodem.

<img src="../assets/logicapps_overview.svg" alt="Azure Logic Apps - automatyzacja workflow">

### Kluczowe cechy Logic Apps

| Cecha | Opis |
|-------|------|
| **Visual Designer** | Graficzny edytor workflow w portalu Azure lub VS Code |
| **450+ Connectors** | Gotowe łączniki do Microsoft, SaaS, on-premises |
| **Serverless** | Automatyczne skalowanie (Consumption) |
| **Enterprise** | B2B, EDI, XML, AS2 (Integration Account) |
| **Event-driven** | Triggery: HTTP, Schedule, Event Grid, Service Bus |
| **Low-code/No-code** | Nie wymaga programowania |

### Podstawowe koncepty

**Workflow** - sekwencja kroków definiująca automatyzację:
- **Trigger** - zdarzenie uruchamiające workflow (1 na workflow)
- **Actions** - operacje wykonywane po triggerze (wiele)
- **Conditions** - warunki if/switch
- **Loops** - pętle For Each, Until
- **Variables** - zmienne do przechowywania danych
- **Expressions** - funkcje do transformacji danych

**Connectors** - gotowe integracje:
- **Standard** - Azure services, Office 365, SQL
- **Enterprise** - SAP, IBM MQ, EDI (wymaga Integration Account)
- **Custom** - własne API (OpenAPI/Swagger)
- **On-premises Data Gateway** - łączność z systemami lokalnymi

### Consumption vs Standard

<img src="../assets/logicapps_tiers.svg" alt="Logic Apps - Consumption vs Standard">

| Cecha | Consumption | Standard |
|-------|-------------|----------|
| **Hosting** | Multi-tenant (shared) | Single-tenant (dedicated) |
| **Skalowanie** | Serverless (auto) | App Service Plan (WS1-WS3) |
| **Rozliczenie** | Pay-per-execution | Stała cena planu |
| **VNet** | NIE (ISE legacy) | TAK (Private Endpoints) |
| **Workflows** | Tylko stateful | Stateful + Stateless |
| **Designer** | Portal Azure | Portal + VS Code |
| **Local debug** | NIE | TAK |
| **DevOps** | ARM Templates | CI/CD, Git integration |
| **Use case** | Proste integracje, PoC | Enterprise, produkcja |

### Tworzenie Logic App (CLI)

```bash
# Tworzenie Consumption Logic App
az logic workflow create \
  --name myLogicApp \
  --resource-group myRG \
  --location westeurope \
  --definition @workflow.json

# Tworzenie Standard Logic App
az logicapp create \
  --name myStandardLogicApp \
  --resource-group myRG \
  --location westeurope \
  --storage-account mystorageaccount \
  --plan myAppServicePlan

# Lista connectorów
az logic integration-account list-callback-url \
  --resource-group myRG \
  --name myIntegrationAccount
```

### Przykład: Workflow Definition (JSON)

```json
{
  "definition": {
    "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
    "contentVersion": "1.0.0.0",
    "triggers": {
      "When_a_HTTP_request_is_received": {
        "type": "Request",
        "kind": "Http",
        "inputs": {
          "schema": {
            "type": "object",
            "properties": {
              "orderId": { "type": "string" },
              "customerEmail": { "type": "string" }
            }
          }
        }
      }
    },
    "actions": {
      "Get_Order": {
        "type": "ApiConnection",
        "inputs": {
          "host": { "connection": { "name": "@parameters('$connections')['sql']['connectionId']" } },
          "method": "get",
          "path": "/v2/datasets/@{encodeURIComponent('default')}/tables/@{encodeURIComponent('Orders')}/items",
          "queries": { "$filter": "OrderId eq '@{triggerBody()?['orderId']}'" }
        },
        "runAfter": {}
      },
      "Condition": {
        "type": "If",
        "expression": {
          "and": [{ "greater": ["@body('Get_Order')?['value']?[0]?['TotalAmount']", 100] }]
        },
        "actions": {
          "Send_Premium_Email": {
            "type": "ApiConnection",
            "inputs": {
              "host": { "connection": { "name": "@parameters('$connections')['office365']['connectionId']" } },
              "method": "post",
              "path": "/v2/Mail",
              "body": {
                "To": "@{triggerBody()?['customerEmail']}",
                "Subject": "Premium Order Confirmation",
                "Body": "Thank you for your order!"
              }
            }
          }
        },
        "else": {
          "actions": {
            "Send_Standard_Email": {
              "type": "ApiConnection",
              "inputs": {
                "host": { "connection": { "name": "@parameters('$connections')['office365']['connectionId']" } },
                "method": "post",
                "path": "/v2/Mail",
                "body": {
                  "To": "@{triggerBody()?['customerEmail']}",
                  "Subject": "Order Confirmation",
                  "Body": "Your order has been received."
                }
              }
            }
          }
        },
        "runAfter": { "Get_Order": ["Succeeded"] }
      },
      "Response": {
        "type": "Response",
        "kind": "Http",
        "inputs": {
          "statusCode": 200,
          "body": { "status": "processed", "orderId": "@{triggerBody()?['orderId']}" }
        },
        "runAfter": { "Condition": ["Succeeded"] }
      }
    }
  }
}
```

### Popularne Triggery

| Trigger | Opis | Przykład użycia |
|---------|------|----------------|
| **HTTP Request** | Webhook endpoint | REST API, integrations |
| **Recurrence** | Schedule (CRON) | Scheduled jobs, reports |
| **Event Grid** | Azure events | Blob created, VM started |
| **Service Bus** | Queue/Topic message | Order processing |
| **Blob Storage** | File added/modified | Document processing |
| **Office 365** | New email, calendar | Email automation |
| **Dynamics 365** | Record created/updated | CRM workflows |
| **SharePoint** | Item created | Document approval |

### Popularne Actions

| Action | Opis |
|--------|------|
| **HTTP** | Wywołanie dowolnego REST API |
| **Parse JSON** | Parsowanie odpowiedzi JSON |
| **Compose** | Tworzenie obiektów JSON |
| **Initialize Variable** | Deklaracja zmiennej |
| **For Each** | Iteracja po tablicy |
| **Until** | Pętla while |
| **Condition** | If-then-else |
| **Switch** | Multi-branch condition |
| **Delay** | Opóźnienie wykonania |
| **Terminate** | Zakończenie workflow |

### Expressions - Przydatne funkcje

```javascript
// Dostęp do danych triggera
@triggerBody()
@triggerBody()?['propertyName']

// Dostęp do wyników akcji
@body('ActionName')
@outputs('ActionName')

// String functions
@concat('Hello ', triggerBody()?['name'])
@substring('Hello World', 0, 5)
@replace('Hello World', 'World', 'Azure')
@toLower('HELLO')
@toUpper('hello')

// Date/Time functions
@utcNow()
@addDays(utcNow(), 7)
@formatDateTime(utcNow(), 'yyyy-MM-dd')
@dayOfWeek(utcNow())

// Collection functions
@length(body('Get_Items')?['value'])
@first(body('Get_Items')?['value'])
@last(body('Get_Items')?['value'])
@contains(body('Get_Items')?['value'], 'item')

// Conditional
@if(equals(triggerBody()?['status'], 'approved'), 'Yes', 'No')
@coalesce(triggerBody()?['name'], 'Unknown')
```

### Error Handling

**Configure Run After** - kontrola wykonania akcji:
- `Succeeded` - wykonaj gdy poprzednia akcja zakończyła się sukcesem
- `Failed` - wykonaj gdy poprzednia akcja zakończyła się błędem
- `Skipped` - wykonaj gdy poprzednia akcja została pominięta
- `TimedOut` - wykonaj gdy poprzednia akcja przekroczyła timeout

**Scope** - grupowanie akcji z obsługą błędów:
```json
{
  "Scope_Try": {
    "type": "Scope",
    "actions": {
      "Risky_Action": { ... }
    }
  },
  "Scope_Catch": {
    "type": "Scope",
    "actions": {
      "Send_Error_Email": { ... }
    },
    "runAfter": { "Scope_Try": ["Failed", "TimedOut"] }
  }
}
```

**Retry Policy:**
```json
{
  "retryPolicy": {
    "type": "exponential",
    "count": 4,
    "interval": "PT10S",
    "minimumInterval": "PT5S",
    "maximumInterval": "PT1H"
  }
}
```

### Integration Account (B2B)

Dla scenariuszy enterprise B2B:

| Funkcja | Opis |
|---------|------|
| **Partners** | Definicje partnerów biznesowych |
| **Agreements** | Umowy AS2, X12, EDIFACT |
| **Schemas** | XML/JSON schemas do walidacji |
| **Maps** | XSLT transformacje |
| **Certificates** | Certyfikaty do podpisywania/szyfrowania |

```bash
# Tworzenie Integration Account
az logic integration-account create \
  --name myIntegrationAccount \
  --resource-group myRG \
  --location westeurope \
  --sku Standard
```

### Monitorowanie i diagnostyka

**Run History:**
- Portal: Logic App -> Overview -> Runs history
- Szczegóły każdego wykonania
- Input/Output każdej akcji
- Czas wykonania, status

**Diagnostic Logs:**
```bash
# Włączenie logów diagnostycznych
az monitor diagnostic-settings create \
  --name "LogicAppLogs" \
  --resource <logic-app-resource-id> \
  --workspace <log-analytics-workspace-id> \
  --logs '[{"category":"WorkflowRuntime","enabled":true}]'
```

**Metryki:**
- `RunsStarted` - liczba uruchomień
- `RunsSucceeded` - udane wykonania
- `RunsFailed` - nieudane wykonania
- `ActionLatency` - czas wykonania akcji
- `TriggerLatency` - czas odpowiedzi triggera

### Logic Apps vs Power Automate vs Azure Functions

| Aspekt | Logic Apps | Power Automate | Azure Functions |
|--------|------------|----------------|----------------|
| **Użytkownik** | Developer/IT Pro | Business User | Developer |
| **Interface** | Azure Portal / VS Code | Power Platform | Code (C#, JS, Python) |
| **Connectors** | 450+ | 900+ | Custom code |
| **Enterprise** | TAK (B2B, EDI) | Ograniczone | Custom |
| **VNet** | TAK (Standard) | NIE | TAK |
| **Koszt** | Per-execution | Per-user license | Per-execution |
| **DevOps** | ARM, CI/CD | Limited | Full CI/CD |
| **Use case** | Enterprise integration | Personal/Team automation | Custom logic |

### Przykładowe scenariusze

**1. Order Processing Pipeline:**
```
HTTP Trigger (new order)
  -> Validate JSON
  -> Insert to SQL Database
  -> If amount > $1000: Send approval email
  -> Create invoice in SAP
  -> Send confirmation to customer
  -> Post to Teams channel
```

**2. Document Approval Workflow:**
```
SharePoint Trigger (new document)
  -> Get document metadata
  -> Get manager from Azure AD
  -> Send approval email
  -> Wait for response (Approval connector)
  -> If approved: Move to "Approved" folder
  -> If rejected: Notify author
```

**3. Data Synchronization:**
```
Recurrence Trigger (every hour)
  -> Get records from Salesforce
  -> For each record:
      -> Check if exists in Dynamics 365
      -> If yes: Update
      -> If no: Create
  -> Log results to Blob Storage
```

### Best Practices

| Praktyka | Opis |
|----------|------|
| **Używaj zmiennych** | Zamiast powtarzać expressions |
| **Grupuj w Scope** | Dla czytelności i error handling |
| **Retry Policy** | Dla niestabilnych połączeń |
| **Concurrent Control** | Ogranicz równoległe wykonania |
| **Secure Inputs/Outputs** | Ukryj wrażliwe dane w logach |
| **Managed Identity** | Zamiast connection strings |
| **Standard dla produkcji** | VNet, dedicated resources |
| **Testuj lokalnie** | VS Code + Standard |

---

### FAQ - Egzamin

| Pytanie | Odpowiedź |
|---------|----------|
| Co to jest Logic Apps? | iPaaS - integracja aplikacji bez kodu |
| Ile connectorów ma Logic Apps? | 450+ |
| Consumption vs Standard? | Consumption = serverless, Standard = dedicated |
| Czy Logic Apps obsługuje VNet? | TAK, ale tylko Standard tier |
| Logic Apps vs Power Automate? | Logic Apps = IT Pro/Dev, Power Automate = Business User |
| Co to jest Integration Account? | B2B, EDI, XML schemas, maps |
| Jak debugować Logic Apps? | Run History w portalu, VS Code dla Standard |
| Czy Logic Apps jest serverless? | Consumption = TAK, Standard = NIE (App Service Plan) |

> **Egzamin:** Logic Apps to iPaaS do automatyzacji workflow z 450+ connectorami. Consumption = serverless (pay-per-execution), Standard = dedicated (VNet, VS Code debug). Integration Account wymagany dla B2B/EDI. Low-code/no-code - nie wymaga programowania.

---
---

[Poprzednia strona](./27-event-grid.md) | [Spis treści](../README.md) | [Następna strona](./29-load-balancing.md)
