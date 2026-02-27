<a id="sec-21-deployment"></a>
## 21. Wdrażanie aplikacji na Azure (Deployment)

[ Powrót do spisu treści](../README.md)


Wdrażanie (deployment) to proces przeniesienia aplikacji ze środowiska deweloperskiego do produkcji. Azure oferuje wiele metod wdrażania, od prostych (przez portal) po zaawansowane (CI/CD pipelines).

<img src="../assets/deployment_methods.svg" alt="Metody wdrażania na Azure" width="100%">

### 21.1 Metody wdrażania - przegląd

| Metoda | Zalety | Wady | Kiedy używać |
|--------|--------|------|--------------|
| **Azure Portal** | Wizualny, łatwy start | Nieskalowalny, brak automatyzacji | Nauka, testy, jednokrotne wdrożenia |
| **Azure CLI / PowerShell** | Skryptowalny, powtarzalny | Wymaga znajomości składni | Automatyzacja prostych zadań |
| **ARM Templates / Bicep (IaC)** | Pełna powtarzalność, wersjonowanie | Krzywa uczenia się | Infrastruktura produkcyjna |
| **CI/CD Pipelines** | Pełna automatyzacja, testy | Złożona konfiguracja | Profesjonalne projekty |

### 21.2 Wdrażanie przez Azure Portal

Najprostsza metoda, idealna do nauki:

1. **Azure Portal → Create a resource**
2. Wybierz typ zasobu (np. Web App, VM)
3. Wypełnij formularz (nazwa, region, pricing tier)
4. **Review + Create → Create**

**Przykład wdrożenia Web App:**
- Resource Group → Wybierz lub utwórz
- Name → unikalna nazwa (myapp.azurewebsites.net)
- Runtime stack → np. .NET 8, Node 20, Python 3.12
- Region → np. West Europe
- Pricing plan → Free F1 (do testów) lub B1/S1 (produkcja)

### 21.3 Wdrażanie przez Azure CLI

Skryptowalny sposób wdrażania, idealny do automatyzacji:

```bash
# Logowanie do Azure
az login

# Utworzenie Resource Group
az group create --name myResourceGroup --location westeurope

# Utworzenie App Service Plan
az appservice plan create --name myAppServicePlan \
    --resource-group myResourceGroup \
    --sku B1 --is-linux

# Utworzenie Web App
az webapp create --name myUniqueAppName \
    --resource-group myResourceGroup \
    --plan myAppServicePlan \
    --runtime "DOTNET|8.0"

# Wdrożenie kodu z ZIP
az webapp deploy --resource-group myResourceGroup \
    --name myUniqueAppName \
    --src-path ./publish.zip --type zip
```

### 21.4 Infrastructure as Code (IaC)

**ARM Templates** - JSON opisujący infrastrukturę:
```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "resources": [{
    "type": "Microsoft.Web/sites",
    "apiVersion": "2022-09-01",
    "name": "[parameters('appName')]",
    "location": "[resourceGroup().location]"
  }]
}
```

**Bicep** - uproszczona składnia (kompiluje się do ARM):
```bicep
resource webApp 'Microsoft.Web/sites@2022-09-01' = {
  name: appName
  location: resourceGroup().location
  properties: {
    serverFarmId: appServicePlan.id
  }
}
```

| Cecha | ARM Templates | Bicep |
|-------|---------------|-------|
| Składnia | JSON (verbose) | DSL (zwięzły) |
| Krzywa uczenia | Stroma | Łagodniejsza |
| IntelliSense | Ograniczony | Pełny (VS Code) |
| Modularność | Linked templates | Natywne moduły |

### 21.5 CI/CD Pipelines

<img src="../assets/cicd_pipeline.svg" alt="CI/CD Pipeline" width="100%">

**CI (Continuous Integration):**
- Automatyczne budowanie po każdym pushu
- Uruchamianie testów jednostkowych
- Analiza jakości kodu
- Tworzenie artefaktów (build output)

**CD (Continuous Deployment/Delivery):**
- Automatyczne wdrażanie na środowiska (dev → staging → prod)
- Testy integracyjne i E2E
- Approval gates przed produkcją
- Rollback w razie problemów

**Azure DevOps Pipeline (YAML):**
```yaml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

stages:
- stage: Build
  jobs:
  - job: BuildJob
    steps:
    - task: DotNetCoreCLI@2
      inputs:
        command: 'publish'
        publishWebProjects: true
        
- stage: Deploy
  jobs:
  - deployment: DeployWeb
    environment: 'production'
    strategy:
      runOnce:
        deploy:
          steps:
          - task: AzureWebApp@1
            inputs:
              appName: 'myapp'
```

**GitHub Actions:**
```yaml
name: Deploy to Azure
on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    
    - name: Build
      run: dotnet publish -c Release
      
    - name: Deploy to Azure
      uses: azure/webapps-deploy@v2
      with:
        app-name: 'myapp'
        publish-profile: ${{ secrets.AZURE_PUBLISH_PROFILE }}
```

### 21.6 Deployment Slots (Blue-Green Deployment)

<img src="../assets/deployment_slots.svg" alt="Deployment Slots" width="100%">

**Deployment Slots** to funkcja App Service pozwalająca na tzw. **blue-green deployment** - wdrażanie bez przestojów.

**Jak to działa:**
1. **Production slot** - aktualnie działająca wersja (np. v1.0)
2. **Staging slot** - nowa wersja do wdrożenia (np. v2.0)
3. Po testach wykonujesz **Swap** - zamiana slotów
4. Użytkownicy natychmiast widzą nową wersję
5. Jeśli problem → Swap z powrotem (instant rollback)

**Zalety:**
- **Zero-downtime deployments** - brak przerwy w działaniu
- **Warm-up** - staging slot jest już rozgrzany
- **Instant rollback** - jeden klik przywraca poprzednią wersję
- **Testowanie na produkcji** - staging może używać produkcyjnej konfiguracji

```bash
# Utworzenie staging slot
az webapp deployment slot create --name myapp \
    --resource-group myRG --slot staging

# Wdrożenie na staging
az webapp deploy --name myapp --slot staging \
    --src-path ./app.zip --type zip

# Swap staging <-> production
az webapp deployment slot swap --name myapp \
    --resource-group myRG --slot staging --target-slot production
```

### 21.7 Wdrażanie kontenerów

<img src="../assets/container_deployment.svg" alt="Wdrażanie kontenerów" width="100%">

**Przepływ wdrożenia kontenera:**
1. **Build image** - `docker build -t myapp .`
2. **Push do ACR** - `docker push myacr.azurecr.io/myapp:v1`
3. **Deploy** - uruchom na wybranej usłudze

**Azure Container Registry (ACR):**
- Prywatny rejestr obrazów Docker
- Integracja z AKS, App Service, ACI
- Geo-replication dla HA
- Vulnerability scanning

```bash
# Utworzenie ACR
az acr create --name myacr --resource-group myRG --sku Basic

# Build i push w jednym kroku (bez lokalnego Dockera)
az acr build --registry myacr --image myapp:v1 .

# Uruchomienie na ACI
az container create --resource-group myRG \
    --name mycontainer \
    --image myacr.azurecr.io/myapp:v1 \
    --cpu 1 --memory 1 \
    --registry-login-server myacr.azurecr.io \
    --registry-username myacr \
    --registry-password <password>
```

### 21.8 Porównanie usług dla kontenerów

| Usługa | Typ | Orkiestracja | Kiedy używać |
|--------|-----|--------------|--------------|
| **App Service** | PaaS | Brak (1 kontener) | Proste web apps |
| **ACI** | CaaS | Brak | Batch jobs, szybkie testy |
| **AKS** | Kubernetes | Pełna | Mikroserwisy, duże systemy |
| **Container Apps** | Serverless | Dapr, KEDA | Scale-to-zero, eventy |

### 21.9 Dobre praktyki wdrażania

**Środowiska (Environments):**
- **Development** - dla programistów, częste wdrożenia
- **Staging** - kopia produkcji, testy UAT
- **Production** - dla użytkowników końcowych

**Strategie wdrażania:**
| Strategia | Opis | Ryzyko |
|-----------|------|--------|
| **Rolling** | Stopniowa wymiana instancji | Niskie |
| **Blue-Green** | Swap slotów/środowisk | Bardzo niskie |
| **Canary** | 5-10% ruchu na nową wersję | Bardzo niskie |
| **Big Bang** | Wszystko naraz | Wysokie |

**Checklist przed produkcją:**
- [ ] Wszystkie testy przeszły
- [ ] Code review zatwierdzony
- [ ] Backup bazy danych wykonany
- [ ] Monitoring skonfigurowany
- [ ] Rollback plan przygotowany
- [ ] Approval gate zatwierdzony

---
---

[Poprzednia strona](./20-redis-cache.md) | [Spis treści](../README.md) | [Następna strona](./22-last-minute-cram.md)
