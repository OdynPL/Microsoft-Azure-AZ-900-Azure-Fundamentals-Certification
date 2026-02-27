<a id="sec-03-compute-services"></a>
## 3. Compute Services (Usługi obliczeniowe)

[ Powrót do spisu treści](../README.md)


- **Virtual Machines (VM)**  

    Elastyczne maszyny wirtualne w wielu seriach (B, Dv5/Ev5, pamięciochłonne, obliczeniowe, GPU).  
    Obsługują **managed disks**, **VM Extensions** i dowolne systemy operacyjne.

    <img src="../assets/vm.svg" alt="Azure Virtual Machines - maszyny wirtualne">

    **Managed disks** czyli niezawodne dyski zarządzane dla maszyny wirtualnej.

    **VM Extensions** czyli dodatki instalowane w VM, które automatyzują konfigurację.

    **Zasoby wymagane dla VM (egzaminowo):**
    - Compute (vCPU/RAM),
    - Storage (OS Disk + opcjonalne Data Disks),
    - Networking (VNet/Subnet/NIC/IP),
    - Security/Access (NSG, role RBAC, klucze/hasła, opcjonalnie Bastion).

- **Availability Sets**  

    Zapewniają odporność na awarie dzięki podziałowi na **Fault Domains** i **Update Domains**.  
    Chronią przed skutkami awarii sprzętu oraz planowanych aktualizacji.

    <img src="../assets/avsets.svg" alt="Availability Sets - Fault Domains i Update Domains">

    **Fault Domains** czyli niezależne fizyczne strefy (różne racki, zasilanie, sieć).  

    **Update Domains** czyli logiczne grupy aktualizacji, restartowane osobno.

    - **VM Scale Sets (VMSS)**  

    **VM Scale Sets** to usługa umożliwiająca automatyczne uruchamianie, skalowanie i zarządzanie wieloma identycznymi maszynami wirtualnymi (VM). Wszystkie instancje oparte są na jednym *modelu* konfiguracji, co zapewnia spójność środowiska przy dużej skali.

    <img src="../assets/vmss.svg" alt="VM Scale Sets - automatyczne skalowanie maszyn wirtualnych">

  - **Autoscaling** – automatyczne zwiększanie lub zmniejszanie liczby VM na podstawie metryk (CPU, RAM, Queue Length, HTTP load) lub harmonogramów.

    - **Rolling Upgrades** – bezpieczne wdrażanie aktualizacji poprzez stopniową wymianę instancji (grupami – Update Domains), bez przestojów całego systemu.

    - **Load Balancer Integration** – natywne połączenie z Azure Load Balancer lub Application Gateway, które równomiernie rozdzielają ruch na instancje.

    - **Instance Model (VM Template)** – wzorzec definiujący obraz systemu (image), rozmiar VM, dyski, konfiguracje sieciowe, rozszerzenia i bootstrap (cloud-init/UserData).

    - **High Availability** – instancje automatycznie rozkładane są między **Fault Domains / Update Domains**, zwiększając odporność na awarie.

    - **Orchestracja** – dwa tryby:  
            - *Uniform* (domyślny): wszystkie VM są identyczne i zarządzane centralnie.  
            - *Flexible*: większa elastyczność, np. do scenariuszy z różnymi typami VM lub strefami dostępności (AZ).

    VMSS pozwala budować skalowalne, wysoko dostępne farmy serwerów opartych o identyczne VM, z automatycznym balansowaniem obciążenia i kontrolowanymi aktualizacjami.

- **App Service**  

    Zarządzana platforma do uruchamiania aplikacji Web, API i Mobile — bez konieczności zarządzania infrastrukturą.  

    <img src="../assets/appservice.svg" alt="Azure App Service - zarzadzana platforma aplikacji webowych">

    Umożliwia łatwe wdrażanie aplikacji, obsługuje **deployment slots**, integruje się z CI/CD  
    (GitHub Actions, Azure DevOps) i zapewnia automatyczne skalowanie instancji w zależności od obciążenia.

- **Azure Functions**

    **Serverless compute** uruchamiany „na żądanie” — kod wykonuje się tylko wtedy, gdy pojawi się zdarzenie, a Ty płacisz wyłącznie za czas wykonania.
   
    Funkcje działają w planach **Consumption** (auto‑skalowanie, płatność za wykonania) oraz **Premium** (stałe instancje, VNET, dłuższe timeouty), obsługując dziesiątki triggerów i bindingów.

    <img src="../assets/azurefunctions.svg" alt="Azure Functions - serverless compute">

    Azure Functions = event‑driven + serverless + automatyczne skalowanie + bogaty ekosystem triggerów i bindingów.

    ### Plany hostingowe (Hosting Plans)

    <img src="../assets/functions_hosting_plans.svg" alt="Azure Functions - plany hostingowe">

    | Plan | Cold Start | Timeout | VNet | Skalowanie | Kiedy używać |
    |------|------------|---------|------|------------|--------------|
    | **Consumption** | Tak (1-3s) | 10 min | Nie | Auto (200) | Event-driven, niskie koszty |
    | **Premium (EP)** | Nie | Unlimited | Tak | Auto + pre-warmed | Produkcja, SLA, VNet |
    | **Dedicated (ASP)** | Nie | Unlimited | Tak (Standard+) | Manual | Istniejący ASP, always-on |

    **Ceny (szacunkowe):**
    - **Consumption**: 1M wykonań/mies FREE, potem ~$0.20/1M + $0.000016/GB-s
    - **Premium EP1**: ~$175/mies (1 vCPU, 3.5GB RAM)
    - **Dedicated**: zależy od App Service Plan

    ### Triggers i Bindings

    <img src="../assets/functions_triggers_bindings.svg" alt="Azure Functions - triggers i bindings">

    **Triggers** – zdarzenia uruchamiające funkcję:
    | Trigger | Opis | Przykład użycia |
    |---------|------|-----------------|
    | **HTTP** | Request REST/webhook | API endpoint, webhook receiver |
    | **Timer** | CRON schedule | Scheduled jobs, cleanup tasks |
    | **Queue Storage** | Message w kolejce | Background processing |
    | **Service Bus** | Queue/Topic message | Enterprise messaging |
    | **Blob Storage** | Nowy/zmieniony blob | Image processing, file import |
    | **Event Grid** | Azure events | React to Azure resource changes |
    | **Cosmos DB** | Change feed | Real-time data sync |

    **Input/Output Bindings** – deklaratywne powiązania z usługami Azure, bez pisania boilerplate.

    ### Kluczowe komponenty

    - **Triggers** – zdarzenia uruchamiające funkcję (np. HTTP, Timer, Queue, Service Bus, Blob, Event Grid).

    - **Input/Output Bindings** – deklaratywne powiązania z usługami Azure, bez pisania boilerplate (np. zapis do Blob Storage albo pobranie wiadomości z Queue).

    - **Function App** – kontener logiczny grupujący funkcje, ich konfigurację, connection strings i runtime.

    - **Host.json** – konfiguracja środowiskowa, retry, batch size, integracje.

    - **Application Settings** – klucze, connection stringi, konfiguracja runtime.

    - **Monitoring** – wbudowana integracja z Application Insights.

    ### Rodzaje Functions
    - **Standard** functions
        
        <img src="../assets/azurefunctions_standard.svg" alt="Standard Functions - funkcje bezstanowe">
        
        - Brak stanu (Stateless)
        - Krótki stan działania
        - Brak koordynacji, ręczna koordynacja
        - Brak wbudowanego retry
        - Brak natywnego wsparcia dla workflow
    - **Durable** functions
        
        <img src="../assets/azurefunctions_durable.svg" alt="Durable Functions - funkcje z trwalym stanem">
        
        - Stan zapisywany automatycznie
        - Mogą działać długo
      - Wbudowany orchestrator
        - Automatyczne retry
      - Wzorce workflow (chaining, fan-in/fan-out)
      - **Function Chaining** - wykonywanie funkcji jednej po drugiej w łańcuchu (wynik wykonania poprzedniej funkcji jest wejściem do następnej), np. ValidateOrder -> ChargePayment -> PrepareShipment -> SendConfirmation (orchestrator pilnuje kolejności wykonania funkcji w łańcuchu).
      - **FAN-IN** - wzorzec, który czeka aż wszystkie funkcje się zakończą i łączy ich wynik (agregacja)
      - **FAN-OUT** - to wzorzec pozwalający uruchomić wiele funkcji równolegle (np. przetwarzanie wielu plików na raz)
              
      Durable Functions mają gotowe mechanizmy do budowania złożonych, wieloetapowych, równoległych i długotrwałych workflow, bez konieczności implementowania własnej logiki kolejek, retry, czekania czy przechowywania stanu.

    ### Implementacja - przykłady kodu

    **HTTP Trigger (C#):**
    ```csharp
    [FunctionName("HelloWorld")]
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post")] HttpRequest req,
        ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");
        
        string name = req.Query["name"];
        
        return new OkObjectResult($"Hello, {name}!");
    }
    ```

    **Timer Trigger (co 5 minut):**
    ```csharp
    [FunctionName("CleanupJob")]
    public static void Run(
        [TimerTrigger("0 */5 * * * *")] TimerInfo timer,
        ILogger log)
    {
        log.LogInformation($"Cleanup executed at: {DateTime.Now}");
        // Twoja logika cleanup
    }
    ```
    
    **CRON expressions:**
    - `0 */5 * * * *` - co 5 minut
    - `0 0 * * * *` - co godzinę
    - `0 0 0 * * *` - codziennie o północy
    - `0 0 9 * * 1-5` - pon-pt o 9:00

    **Queue Trigger z Output Binding:**
    ```csharp
    [FunctionName("ProcessOrder")]
    public static void Run(
        [QueueTrigger("orders")] string orderJson,
        [Blob("processed/{rand-guid}.json")] out string outputBlob,
        [Queue("notifications")] out string notification,
        ILogger log)
    {
        var order = JsonSerializer.Deserialize<Order>(orderJson);
        
        // Zapisz do Blob Storage
        outputBlob = JsonSerializer.Serialize(order);
        
        // Wyślij notyfikację do kolejki
        notification = $"Order {order.Id} processed";
        
        log.LogInformation($"Processed order: {order.Id}");
    }
    ```

    **Cosmos DB Trigger (Change Feed):**
    ```csharp
    [FunctionName("CosmosDBTrigger")]
    public static void Run(
        [CosmosDBTrigger(
            databaseName: "mydb",
            containerName: "products",
            Connection = "CosmosDBConnection",
            LeaseContainerName = "leases")] IReadOnlyList<Product> changes,
        ILogger log)
    {
        foreach (var product in changes)
        {
            log.LogInformation($"Product changed: {product.Name}");
            // Synchronizuj z innym systemem, indeksuj w Search, etc.
        }
    }
    ```

    ### Tworzenie przez Azure CLI

    ```bash
    # Utworzenie Resource Group
    az group create --name myFuncRG --location westeurope

    # Utworzenie Storage Account (wymagane dla Functions)
    az storage account create --name myfuncstorage123 \
        --resource-group myFuncRG --sku Standard_LRS

    # Utworzenie Function App (Consumption plan)
    az functionapp create --name myfunc123 \
        --resource-group myFuncRG \
        --storage-account myfuncstorage123 \
        --consumption-plan-location westeurope \
        --runtime dotnet --runtime-version 8 \
        --functions-version 4

    # Wdrożenie kodu (ZIP deploy)
    az functionapp deployment source config-zip \
        --resource-group myFuncRG --name myfunc123 \
        --src ./publish.zip
    ```

    ### Best Practices

    | Praktyka | Opis |
    |----------|------|
    | **Krótkie wykonanie** | Funkcje powinny być szybkie (sekundy, nie minuty) |
    | **Stateless** | Nie przechowuj stanu między wywołaniami |
    | **Idempotentność** | Funkcja powinna dawać ten sam wynik przy wielokrotnym wywołaniu |
    | **Connection pooling** | Używaj static HttpClient, reuse connections |
    | **Environment variables** | Sekrety w Application Settings, nie w kodzie |
    | **Structured logging** | Używaj ILogger z kontekstem |

- **Azure Container Instances (ACI)**  

    Azure Container Instances to najszybszy sposób uruchamiania kontenerów w Azure bez potrzeby tworzenia lub zarządzania klastrem Kubernetes.

    <img src="../assets/aci.svg" alt="Azure Container Instances - szybkie uruchamianie kontenerow">

    Idealne do krótkotrwałych, jednorazowych lub prostych workloadów — batch jobs, API workers, testów, automatyzacji.

    Kluczowe cechy:
    - start kontenera w kilka sekund (bez orkiestracji)
    - płatność tylko za czas działania CPU/RAM kontenera
    - obsługa obrazów z Azure Container Registry (ACR) i Docker Hub
    - wsparcie dla sidecar containers i grup kontenerów (Container Groups)
    - integracja z VNet (opcjonalnie)
    - idealne do: jobów, event-driven tasks, ETL, automatyzacji, szybkich micro‑API

  - **Azure Container Apps (ACA)**

    Zarządzana platforma do uruchamiania kontenerów bez zarządzania klastrem Kubernetes.

    Najważniejsze cechy:
    - serverless containers (skalowanie do zera i szybkie scale-out)
    - obsługa HTTP, event-driven (KEDA), revision-based deployment
    - dobre do mikroserwisów, API i background workerów

    **Różnica egzaminacyjna (skrót):**
    - **ACI** = pojedyncze/krótkie zadania kontenerowe
    - **ACA** = aplikacje kontenerowe z autoskalowaniem i prostym modelem mikroserwisów
    - **AKS** = pełna orkiestracja Kubernetes

- **AKS (Azure Kubernetes Service)**  

    Zarządzany Kubernetes w Azure, który automatyzuje większość złożonych zadań administracyjnych — takich jak aktualizacje węzłów, skalowanie, bezpieczeństwo i integracja z ekosystemem Azure.  

    <img src="../assets/azurekubernetesservice.svg" alt="Azure Kubernetes Service - zarzadzany Kubernetes">

    Umożliwia uruchamianie kontenerów w modelu produkcyjnym bez konieczności samodzielnego zarządzania control plane.
    
    **Kluczowe elementy AKS:**
    - **Zarządzany control plane** – Kubernetes API server, scheduler, controller manager utrzymywane przez Azure.
    - **Node Pools (agent nodes)** – węzły robocze, na których działają pody (VM‑ki w tle).
    - **Autoscaling** – Cluster Autoscaler (CA) + Horizontal Pod Autoscaler (HPA).
    - **Integracja z ACR** – bezpośrednie pobieranie obrazów kontenerów z Azure Container Registry.
    - **Network model** – Kubenet lub Azure CNI (pełna integracja z VNet).
    - **Load Balancing** – automatyczne tworzenie Load Balancera dla usług typu LoadBalancer.
    - **AKS Add‑ons** – monitoring, logi, Application Gateway Ingress, Azure Policy, KEDA.
    - **Bezpieczeństwo** – MSI, Azure RBAC, polityki, OIDC, secret store CSI driver.

    **AKS** to w pełni zarządzany Kubernetes + automatyczne aktualizacje + skalowanie + integracje Azure (ACR, VNet, RBAC).

- **Azure Bastion**  

    Azure Bastion umożliwia bezpieczne połączenia RDP i SSH do maszyn wirtualnych **bez potrzeby wystawiania publicznych adresów IP**.  

    <img src="../assets/azurebastion.svg" alt="Azure Bastion - bezpieczny dostep RDP/SSH">

    Działa przez przeglądarkę (HTML5), minimalizuje powierzchnię ataku, usuwa konieczność otwierania portów 3389/22. 

    Zapewnia dostęp w pełni przez sieć prywatną (VNet).

---
---

[Poprzednia strona](./02-azure-architecture.md) | [Spis treści](../README.md) | [Następna strona](./04-networking.md)
