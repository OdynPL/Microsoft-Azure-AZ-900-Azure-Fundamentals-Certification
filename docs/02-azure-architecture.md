<a id="sec-02-azure-architecture"></a>
## 2. Azure Architecture (Architektura i hierarchia)

[ Powrót do spisu treści](../README.md)

**Hierarchia i scope:**

**Hierarchia i scope — logika porządkowania zasobów w Azure**

Poniższy diagram przedstawia pełną hierarchię zarządzania w Azure — od najwyższego poziomu tożsamości (Tenant), przez struktury organizacyjne (Management Groups), aż po subskrypcje, grupy zasobów i pojedyncze zasoby.

<img src="../assets/hierarchy_scope.svg" alt="Hierarchia zasobow Azure - Tenant, Management Groups, Subscriptions">

### Hierarchia definiuje:
- **zakres (scope)**, na którym można przypisywać uprawnienia (RBAC),
- **miejsce stosowania polityk** (Azure Policy),
- **jak organizować zasoby** w skali całej organizacji,
- **gdzie przebiegają granice rozliczeń i odpowiedzialności**.

<br>

---

Ta struktura jest fundamentem pracy z Azure — wpływa na **uprawnienia, governance, koszty, compliance**, a nawet na sposób planowania architektury i CI/CD.

## Struktura Azure

- **Tenant (Microsoft Entra ID)** – główny kontener tożsamości całej organizacji. Zawiera użytkowników, grupy i zasady bezpieczeństwa.
- **Management Group** – umożliwia grupowanie wielu subskrypcji oraz nakładanie wspólnych polityk na poziomie organizacji.
- **Subscription** – określa sposób rozliczeń, limity oraz stanowi granicę uprawnień administracyjnych.
- **Resource Group (RG)** – logiczny kontener na powiązane ze sobą zasoby (np. aplikację i jej bazę danych).
- **Resource** – pojedynczy zasób lub usługa, np. VM, VNet, konto Storage, App Service itd.

<img src="../assets/azure_hierarchy.svg" alt="Hierarchia Azure - Tenant, Management Group, Subscription, Resource Group">

- Jeden **Resource** może należeć tylko do jednej **Resource Group**.

## Globalna infrastruktura Azure

<img src="../assets/azure_global_architecture.svg" alt="Globalna architektura Azure - regiony, strefy dostepnosci">

- **Geography** – obszar składający się z jednego lub kilku regionów, stanowiący granicę rezydencji i zgodności danych (np. EU, US).

- **Region** – zestaw współpołączonych centrów danych działających jako jedna lokalizacja dostarczająca usługi Azure.

- **Azure Datacenters** – fizyczne obiekty Microsoft z infrastrukturą serwerową, sieciową i zasilaniem. Są podstawą regionów oraz stref AZ, ale klienci nie zarządzają nimi bezpośrednio.

- **Regions** to miejsca na świecie, w których Azure ma swoje centra danych — dzięki temu usługi działają szybciej, bliżej użytkowników i zgodnie z lokalnymi przepisami.

  - **Public Regions**
  - Standardowe regiony Azure dostępne globalnie dla wszystkich klientów i uruchamiające pełny zestaw usług.

  - **Restricted Regions**
  - Regiony o ograniczonym dostępie, dostępne wyłącznie po akceptacji Microsoft w specjalnych scenariuszach biznesowych.

  - **Upcoming Regions**
  - Nowo ogłoszone, budowane regiony, które poszerzą globalny zasięg i spełnią lokalne wymogi rezydencji danych.

  - **Sovereign Regions** (Azure Government / China)
  - Oddzielone, suwerenne regiony przeznaczone dla rządów i krajów o szczególnych regulacjach jak USA Gov lub Chiny.

- **Availability Zone (AZ)** – fizycznie odseparowane centra danych w obrębie jednego regionu (oddzielne zasilanie, chłodzenie, sieć) zapewniające wysoki poziom odporności.

- **Region Pair** – dwa regiony w tej samej geografii sparowane ze sobą w celu zapewnienia odporności, planowania aktualizacji i disaster recovery.

## ARM (Azure Resource Manager) – warstwa zarządzania

**Azure Resource Manager (ARM)** to centralna warstwa zarządzania platformą Azure.  
Odpowiada za spójne, bezpieczne i powtarzalne zarządzanie zasobami — niezależnie od tego, z jakiego narzędzia korzystasz.

<img src="../assets/arm_layer.svg" alt="Azure Resource Manager - warstwa zarzadzania">

- **Spójne API** – ujednolicony mechanizm zarządzania zasobami Azure.

  Niezależnie od tego, czy korzystasz z Portalu, Azure CLI, PowerShell czy REST API, wszystkie operacje trafiają do tej samej warstwy zarządzającej. Dzięki temu działania są przewidywalne i działają tak samo w każdym narzędziu.

- **Deklaratywne wdrożenia** – możliwość opisywania infrastruktury w formie deklaratywnej, czyli definiujesz *co* ma powstać, a nie *jak to tworzyć*.  

  Azure realizuje to poprzez **ARM Templates (JSON)** oraz **Bicep**, umożliwiając automatyczne, powtarzalne i kontrolowane wdrożenia w modelu Infrastructure as Code.

        {
        "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",

        "parameters": {
            "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Nazwa tworzonego konta Storage."
            }
            },
            "storageSku": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
                "Standard_LRS",
                "Standard_GRS",
                "Standard_ZRS",
                "Premium_LRS"
            ],
            "metadata": {
                "description": "SKU konta Storage."
            }
            }
        },

        "resources": [
            {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2022-05-01",
            "name": "[parameters('storageAccountName')]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "[parameters('storageSku')]"
            },
            "kind": "StorageV2",
            "properties": {
                "accessTier": "Hot"
            }
            }
        ],

        "outputs": {
            "storageAccountId": {
            "type": "string",
            "value": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]"
            }
        }
        }

- **Organizacja zasobów** – możliwość nadawania zasobom struktury i zabezpieczeń.  

    Przykłady organizacji zasobów:

    - **Podziel subskrypcje** według środowisk (np. Dev / Test / Prod) lub jednostek biznesowych.
    - **Używaj Resource Groups** jako kontenerów dla jednego rozwiązania lub aplikacji — zasoby o wspólnym cyklu życia powinny być razem.
    - **Dodawaj tagi**, aby oznaczać koszty, właścicieli, środowiska i elementy automatyzacji.
    - **Dodawaj blokady (locks)**, aby chronić ważne lub krytyczne zasoby przed przypadkowym usunięciem lub zmianą.
    - **Stosuj Azure Policy**, aby wymuszać standardy organizacji, np. obowiązkowe tagi, dozwolone regiony, typy zasobów.
    - **Trzymaj się konwencji nazw**, aby zasoby były łatwe do odnalezienia, filtrowania i automatyzacji.

- **Tags** służą do dodawania metadanych (np. koszt centrum, właściciel, projekt) dla naszych zasobów,  

    Najczęstsze zastosowania:

    - **Rozliczenia i kosztorysowanie** – tagi pozwalają przypisać koszt zasobów do projektu, działu, środowiska lub zespołu. Dzięki temu raporty kosztów są czytelne i dokładne.  
    Przykłady: `CostCenter=IT01`, `Project=Ecommerce`, `Environment=Prod`

    - **Zarządzanie i porządkowanie zasobów** – ułatwiają odnajdywanie i grupowanie powiązanych elementów w dużych subskrypcjach.  
    Przykłady: `Owner=Jan.Kowalski`, `Service=BackendAPI`

    - **Automatyzacja i polityki** – tagi mogą być używane przez Azure Policy, Automation, zaplanowane skrypty i governance.  
    Przykłady: automatyczne wyłączanie maszyn z tagiem `AutoShutdown=True`

    - **Bezpieczeństwo i odpowiedzialność** – jasne wskazanie właściciela lub kontaktu sprawia, że wiadomo, kto odpowiada za utrzymanie zasobu.  
    Przykład: `OwnerEmail=devops-team@example.com`

    - **Zarządzanie cyklem życia** – tagi mogą określać datę wygaśnięcia, przeglądu lub planowanego usunięcia.  
    Przykład: `ExpiryDate=2026-06-30`

- **Locks** (`Delete` / `ReadOnly`) chronią zasoby przed przypadkowym usunięciem lub modyfikacją, co jest kluczowe w utrzymaniu porządku i bezpieczeństwa w środowisku.

    **Po co?**  
    Aby chronić zasoby przed przypadkowym usunięciem lub zmianą.

    **Rodzaje blokad:**
    - **Delete** – uniemożliwia usunięcie zasobu; konfigurację nadal można zmieniać.  
    - **ReadOnly** – blokuje usuwanie i jakiekolwiek modyfikacje; zasób działa tylko w trybie odczytu.

    **Gdzie można je stosować?**
    - na poziomie: **subscription**, **resource group**, **resource**
    - blokady dziedziczą się w dół (lock na RG obejmuje wszystkie zasoby w środku)

    **Kto może je zdjąć?**
    - tylko role uprzywilejowane (np. **Owner**)

    **Kiedy stosować?**
    - zasoby krytyczne (produkcyjne sieci, Key Vault, Storage z backupami)  
    - elementy, których nie wolno usuwać ani zmieniać

    **Dlaczego to ważne?**  
    Locki chronią środowisko przed błędami administratorów i pipeline’ów automatyzacji — zapewniają porządek i bezpieczeństwo operacyjne.

---
---

[Poprzednia strona](./01-cloud-concepts.md) | [Spis treści](../README.md) | [Następna strona](./03-compute-services.md)
