<a id="sec-19-azure-cli"></a>
## 19. Azure CLI

[ Powrót do spisu treści](../README.md)


Azure CLI to **narzędzie wiersza poleceń** do zarządzania usługami Azure. Cross-platform (Windows, Linux, macOS), działa też w Azure Cloud Shell.

**Schemat:** `az <grupa> <podgrupa> <komenda> --parametr wartość`

### Inne narzędzia zarządzania

| Narzędzie | Opis |
|-----------|------|
| **Azure Portal** | GUI w przeglądarce |
| **Azure PowerShell** | Automatyzacja skryptami PS |
| **Azure Cloud Shell** | CLI/PS w przeglądarce (bez instalacji) |
| **Azure Mobile App** | Podgląd alertów z telefonu |

---

### Podstawowe komendy

```bash
# Logowanie
az login
az account show                              # Pokaż aktywną subskrypcję
az account list --output table               # Lista subskrypcji
az account set --subscription "MySub"        # Zmień subskrypcję

# Resource Group
az group create --name MyRG --location westeurope
az group list --output table
az group delete --name MyRG --yes --no-wait

# Virtual Machine
az vm create --resource-group MyRG --name MyVM \
    --image Ubuntu2204 --size Standard_B2s \
    --admin-username azureuser --generate-ssh-keys
az vm list --output table
az vm start --resource-group MyRG --name MyVM
az vm stop --resource-group MyRG --name MyVM
az vm deallocate --resource-group MyRG --name MyVM   # Zatrzymaj i zwolnij zasoby
az vm delete --resource-group MyRG --name MyVM --yes

# Storage Account
az storage account create --name mystorageacc123 \
    --resource-group MyRG --sku Standard_LRS --kind StorageV2
az storage account list --output table
az storage container create --name mycontainer --account-name mystorageacc123

# Networking
az network vnet create --resource-group MyRG --name MyVNet \
    --address-prefix 10.0.0.0/16 --subnet-name default --subnet-prefix 10.0.0.0/24
az network nsg create --resource-group MyRG --name MyNSG
az network public-ip create --resource-group MyRG --name MyIP --sku Standard

# Web App
az appservice plan create --name MyPlan --resource-group MyRG --sku B1 --is-linux
az webapp create --resource-group MyRG --plan MyPlan --name mywebapp123 \
    --runtime "DOTNET|8.0"
az webapp list --output table
az webapp deploy --resource-group MyRG --name mywebapp123 --src-path app.zip

# AKS (Kubernetes)
az aks create --resource-group MyRG --name MyAKS --node-count 2 --generate-ssh-keys
az aks get-credentials --resource-group MyRG --name MyAKS
az aks list --output table

# RBAC
az role assignment list --assignee user@domain.com --output table
az role assignment create --assignee user@domain.com --role Reader \
    --scope /subscriptions/<SUB-ID>/resourceGroups/MyRG
az role definition list --output table

# Tagi
az resource tag --tags Environment=Dev Project=Demo \
    --ids /subscriptions/<SUB>/resourceGroups/MyRG

# Pomoc
az --help                    # Ogólna pomoc
az vm --help                 # Pomoc dla grupy vm
az vm create --help          # Pomoc dla konkretnej komendy
az find "create vm"          # Wyszukaj komendy
```

### Output formats

```bash
az vm list --output table    # Tabela (czytelna)
az vm list --output json     # JSON (do parsowania)
az vm list --output yaml     # YAML
az vm list --output tsv      # TSV (do skryptów)
az vm list --query "[].name" # JMESPath query
```

### Przydatne flagi

| Flaga | Opis |
|-------|------|
| `--output`, `-o` | Format wyjścia (table/json/yaml/tsv) |
| `--query` | JMESPath do filtrowania wyników |
| `--yes`, `-y` | Bez potwierdzenia |
| `--no-wait` | Nie czekaj na zakończenie |
| `--debug` | Szczegółowe logi |

**Dokumentacja:** [Azure CLI Reference](https://learn.microsoft.com/en-us/cli/azure/reference-index)

---
---

[Poprzednia strona](./18-subscription-models.md) | [Spis treści](../README.md) | [Następna strona](./20-redis-cache.md)
