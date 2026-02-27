
# Microsoft Azure – AZ‑900 Fundamentals (Kompendium w 1 pliku)

> Kompletny, praktyczny README do szybkiej nauki i utrwalenia wiedzy pod egzamin **AZ‑900 Microsoft Azure Fundamentals**. Zawiera: podstawy chmury, architekturę Azure, kluczowe usługi (Compute/Networking/Storage), tożsamość i bezpieczeństwo (Microsoft Entra), governance i koszty, monitoring, IaC & automatyzację, rozszerzone usługi oraz **podchwytliwe pytania egzaminacyjne**.

---

## Spis treści
- [1. Cloud Concepts (Podstawy chmury)](#sec-01-cloud-concepts)
- [2. Azure Architecture (Architektura i hierarchia)](#sec-02-azure-architecture)
- [3. Compute Services (Usługi obliczeniowe)](#sec-03-compute-services)
- [4. Networking (Sieci i łączność)](#sec-04-networking)
- [5. Storage (Przechowywanie danych)](#sec-05-storage)
- [6. Identity & Access (Microsoft Entra)](#sec-06-identity-access)
- [7. Security (Bezpieczeństwo)](#sec-07-security)
- [8. Governance & Compliance](#sec-08-governance-compliance)
- [9. Monitoring & Logging](#sec-09-monitoring-logging)
- [10. Costs & Billing (Koszty i rozliczenia)](#sec-10-costs-billing)
- [11. IaC & Automation (Infrastruktura jako kod)](#sec-11-iac-automation)
- [12. Extended Azure Services (Usługi rozszerzone)](#sec-12-extended-services)
- [13. Podchwytliwe pytania AZ‑900](#sec-13-pytania-az900)
- [14. Glosariusz skrócony](#sec-14-glosariusz)
- [15. Azure Free Account - Free Tier](#sec-15-free-account)
- [16. SLA - Service Level Agreement](#sec-16-sla)
- [17. Bazy danych (Databases)](#sec-17-databases)
- [18. Subskrypcje (Subscription Models)](#sec-18-subscription-models)
- [19. Azure CLI](#sec-19-azure-cli)
- [20. Azure Cache for Redis](#sec-20-redis-cache)
- [21. Wdrażanie aplikacji na Azure (Deployment)](#sec-21-deployment)
- [22. Last-minute cram (pułapki + porównania)](#sec-22-last-minute-cram)
- [Załącznik A – Diagramy ASCII](#sec-appendix-ascii)

---

<a id="sec-01-cloud-concepts"></a>
## 1. Cloud Concepts (Podstawy chmury)

## Modele wdrożenia

### Public Cloud
Usługi uruchamiane w infrastrukturze dostawcy (np. Microsoft Azure). Dostępność i skalowanie zapewnia provider, a Ty płacisz za zużycie. Szybki time‑to‑market, szeroka oferta usług, brak konieczności utrzymywania hardware’u.
<br>

<img src="assets/public_cloud.svg">

**Kiedy używać:** projekty o zmiennym obciążeniu, potrzeba globalnego zasięgu, krótkie cykle wdrożeniowe.  
**Ryzyka/uwagi:** lock‑in, zgodność/regulacje, kontrola nad siecią niższa niż on‑prem.

---

### Private Cloud
Dedykowana chmura dla jednej organizacji (własne DC / hosting dedykowany / Azure Stack HCI). Najwyższa kontrola i możliwość dopasowania do rygorów zgodności, kosztem zwinności i CAPEX/OPEX.
<br>

<img src="assets/private_cloud.svg">

**Kiedy używać:** silne wymagania compliance, izolacja, specyficzne potrzeby bezpieczeństwa/latencji.  
**Ryzyka/uwagi:** większe koszty utrzymania, mniejsza elastyczność skalowania.

---

### Hybrid Cloud
Połączenie środowisk on‑prem/private z public cloud. Umożliwia przenoszenie obciążeń, burst do chmury i stopniową migrację, z zachowaniem kontroli nad danymi krytycznymi.
<br>

<img src="assets/hybrid_cloud.svg">

**Kiedy używać:** modernizacja etapowa, wymogi rezydencji danych, integracje z istniejącymi systemami.  
**Ryzyka/uwagi:** złożoność sieci/identyfikacji/monitoringu, potrzeba spójnego governance.

---

### Community Cloud
Chmura współdzielona przez grupę organizacji o wspólnych wymaganiach — np. sektor publiczny, medyczny, edukacyjny lub finansowy. Zapewnia wspólne standardy bezpieczeństwa, zgodność regulacyjną i kontrolę nad środowiskiem.
<br>

<img src="assets/community_cloud.svg">

**Kiedy używać:** organizacje o wspólnych regulacjach (RODO, HIPAA, FINREP), współdzielone koszty, potrzeba jednolitych polityk bezpieczeństwa.  
**Ryzyka/uwagi:** uzgodnienie governance między uczestnikami, współdzielona odpowiedzialność, potencjalnie mniejsza elastyczność niż public cloud.

---

### Multi‑Cloud
Wykorzystanie wielu dostawców chmury (np. Azure, AWS, GCP) jednocześnie. Pozwala redukować vendor lock‑in, wybierać najlepsze usługi z różnych platform oraz zwiększyć odporność na awarie jednego providera.
<br>

<img src="assets/multi_cloud.svg">

**Kiedy używać:** minimalizacja zależności od jednego dostawcy, strategia „best‑of‑breed”, wymagania biznesowe lub prawne dotyczące dywersyfikacji.  
**Ryzyka/uwagi:** wyższa złożoność operacyjna, trudniejsze bezpieczeństwo i monitoring, konieczność ujednolicenia narzędzi i procesów (IaC/CI/CD).

---

### Distributed Cloud
Model, w którym usługi chmurowe dostawcy (np. Azure, AWS, GCP) są fizycznie uruchamiane bliżej użytkownika — w lokalnych centrach danych, edge‑location, on‑prem lub w regionach partnerskich. Pozwala zachować jedno zarządzanie chmurą, ale wykonywać obliczenia tam, gdzie są potrzebne.
<br>

<img src="assets/distributed_cloud.svg">

**Kiedy używać:** niska latencja, wymagania rezydencji danych, obciążenia przemysłowe/IoT, obliczenia blisko fabryki lub oddziału.  
**Ryzyka/uwagi:** większa złożoność wdrożeń, koordynacja między lokalnymi zasobami a centralną chmurą, zależność od infrastruktury partnerów.

---

### Edge Cloud
Przetwarzanie danych bezpośrednio na „krawędzi” sieci — blisko urządzeń IoT, fabryk, sklepów, samochodów autonomicznych czy systemów przemysłowych. Minimalizuje opóźnienia i redukuje transfer danych do regionów chmurowych.
<br>

<img src="assets/edge_cloud.svg">

**Kiedy używać:** ultra‑niska latencja, IoT, urządzenia mobilne, autonomiczne systemy, lokalne decyzje w czasie rzeczywistym.  
**Ryzyka/uwagi:** konieczność zarządzania wieloma lokalizacjami, ograniczone zasoby sprzętowe, bardziej złożone bezpieczeństwo.

---

## Modele usług chmurowych (Cloud Service Models)

### IaaS (Infrastructure as a Service)
Dostawca udostępnia zasoby infrastruktury: maszyny wirtualne, sieci, load balancery, firewalle, dyski.  
Użytkownik zarządza systemem operacyjnym, aplikacjami i konfiguracją.

<img src="assets/iaas.svg">

**Przykłady:** Azure VM, VNet, Load Balancer, Storage.  
**Kiedy używać:** migracje lift‑and‑shift, systemy wymagające pełnej kontroli nad OS.  
**Ryzyka/uwagi:** większe koszty utrzymania i administracji.

---

### PaaS (Platform as a Service)
Środowisko uruchomieniowe dla aplikacji bez konieczności zarządzania OS, patchami i infrastrukturą.  
Odpowiadasz jedynie za kod i dane.

<img src="assets/paas.svg">

**Przykłady:** Azure App Service, Azure SQL, Functions (Premium).  
**Kiedy używać:** API, mikroserwisy, aplikacje web.  
**Plusy:** automatyczne skalowanie, wysoka dostępność, szybkie wdrażanie.

---

### SaaS (Software as a Service)
W pełni gotowe aplikacje dostarczane jako usługa — bez instalacji i utrzymania.

<img src="assets/saas.svg">

**Przykłady:** Microsoft 365, Power BI, Salesforce.  
**Kiedy używać:** poczta, dokumenty, CRM, HR, analiza danych.  
**Plusy:** minimalny narzut operacyjny.

---

### Serverless / FaaS (Function as a Service)
Kod uruchamiany na żądanie, bez serwerów i bez opłat stałych — płacisz tylko za wykonanie.

<img src="assets/faas.svg">

**Przykłady:** Azure Functions (Consumption), AWS Lambda.  
**Kiedy używać:** event‑driven, automatyzacje, integracje, IoT.  
**Plusy:** pełna autoskalowalność.

---

### NaaS (Network as a Service)
Sieć jako usługa — routing, VPN, SD‑WAN, firewalle, połączenia między chmurami, edge networking.

<img src="assets/naas.svg">

**Przykłady:** Azure Virtual WAN, AWS Cloud WAN.  
**Kiedy używać:** globalna sieć firmy, integracje multi‑cloud, oddziały.  
**Ryzyka/uwagi:** zależność od usług sieciowych dostawcy.

---

### CaaS (Container as a Service)
Zarządzane środowisko kontenerowe — bez utrzymywania VM czy orkiestracji.

<img src="assets/caas.svg">

**Przykłady:** Azure Container Instances, AWS Fargate, Cloud Run.  
**Kiedy używać:** mikroserwisy, API, krótkie zadania batch.  
**Plusy:** zero administrowania infrastrukturą.

---

### DaaS (Desktop as a Service)
Zdalne, hostowane w chmurze środowiska pulpitu użytkownika.

<img src="assets/daas.svg">

**Przykłady:** Azure Virtual Desktop, Windows 365.  
**Kiedy używać:** praca zdalna, call‑center, access z dowolnego urządzenia.  
**Ryzyka/uwagi:** zależność od jakości łącza.

---

### iPaaS (Integration Platform as a Service)
Platformy integracyjne do łączenia systemów, API, ETL i workflow.

<img src="assets/ipaas.svg">

**Przykłady:** Azure Logic Apps, MuleSoft, Boomi.  
**Kiedy używać:** integracje ERP/CRM, automatyzacja procesów.  
**Plusy:** szybkie łączenie systemów bez pisania backendu.

---

### MBaaS / BaaS (Mobile/Backend as a Service)
Backend dostarczany jako usługa — auth, bazy, storage, push, API.

<img src="assets/mbaas.svg">

**Przykłady:** Firebase, AWS Amplify.  
**Kiedy używać:** aplikacje mobilne, prototypy, szybki development.

---

### BPaaS (Business Process as a Service)
Gotowe procesy biznesowe jako usługa, np. HR, płace, księgowość, CRM.

<img src="assets/bpaas.svg">

**Przykłady:** Workday, Dynamics 365, Salesforce.  
**Kiedy używać:** outsourcing procesów biznesowych.  
**Plusy:** standaryzacja i skalowalność.

---

## Korzyści chmury (Cloud Benefits)

Egzamin AZ-900 wymaga znajomości **4 kluczowych obszarów korzyści** wynikających z korzystania z chmury:

<img src="assets/cloudbenefits.svg">

### 1. Wysoka dostępność i skalowalność (High Availability & Scalability)

**Wysoka dostępność (HA):**
- Usługi chmurowe działają w wielu strefach i regionach, minimalizując ryzyko przestojów.
- SLA gwarantujące dostępność od 99.9% do 99.999%.
- Automatyczny failover i load balancing.

**Skalowalność:**
- **Vertical Scaling** — zwiększanie mocy pojedynczego zasobu (CPU, RAM).
- **Horizontal Scaling** — dodawanie kolejnych instancji.
- **Elastyczność** — skalowanie w górę i w dół w zależności od obciążenia.

### 2. Niezawodność i przewidywalność (Reliability & Predictability)

**Niezawodność (Reliability):**
- Decentralizowana architektura — awaria jednego komponentu nie wpływa na cały system.
- Region pairs i geo-redundancy zapewniają disaster recovery.
- Self-healing — automatyczne zastępowanie uszkodzonych instancji.

**Przewidywalność (Predictability):**
- **Przewidywalność wydajności** — autoskalowanie i load balancing gwarantują stabilną wydajność.
- **Przewidywalność kosztów** — Cost Management, budgety, alerty i Pricing Calculator pozwalają planować wydatki.
- Azure Well-Architected Framework dostarcza sprawdzone wzorce.

### 3. Bezpieczeństwo i governance (Security & Governance)

**Bezpieczeństwo:**
- Wbudowana ochrona DDoS, firewalle, szyfrowanie danych w spoczynku i w tranzycie.
- Microsoft Defender for Cloud — ciągła analiza i rekomendacje bezpieczeństwa.
- Zero Trust, MFA, Conditional Access.

**Governance:**
- Azure Policy wymusza standardy organizacyjne.
- RBAC ogranicza dostęp zgodnie z zasadą least privilege.
- Resource Locks chronią przed przypadkowym usunięciem.
- Management Groups umożliwiają centralne zarządzanie wieloma subskrypcjami.

### 4. Zarządzalność (Manageability)

**Management OF the cloud** — jak chmura zarządza zasobami:
- Autoskalowanie zasobów na podstawie obciążenia.
- Wdrażanie z szablonów (ARM Templates, Bicep).
- Automatyczne monitorowanie i alerty.
- Self-healing i auto-recovery.

**Management IN the cloud** — jak zarządzasz chmurą:
- **Azure Portal** — graficzny interfejs webowy.
- **Azure CLI / PowerShell** — automatyzacja i skrypty.
- **Cloud Shell** — terminal w przeglądarce.
- **APIs / SDKs** — programowy dostęp.
- **Infrastructure as Code** — ARM Templates, Bicep, Terraform.

---

## CAPEX vs OPEX i model konsumpcyjny

- **CAPEX** – wydatki inwestycyjne (zakup serwerów, sprzętu, licencji z góry).
- **OPEX** – wydatki operacyjne (płatność cykliczna za faktyczne zużycie usług).
- **Azure działa głównie w modelu OPEX / pay‑as‑you‑go** – płacisz za wykorzystane zasoby, a nie za utrzymywanie własnego hardware.

**Na egzamin AZ‑900:** cloud zwykle redukuje CAPEX, zwiększa elastyczność kosztów i skraca czas wdrożeń.

---

## Modele cenowe Azure (Cloud Pricing Models)

<img src="assets/pricingmodels.svg">

Azure oferuje trzy główne modele cenowe dla zasobów compute:

| Model | Oszczędność | Zobowiązanie | Kiedy używać |
|-------|-------------|--------------|--------------|
| **Pay-As-You-Go** | 0% (cena bazowa) | Brak | Dev/test, zmienne obciążenie |
| **Reserved Instances** | do 72% | 1-3 lata | Produkcja, stałe obciążenie 24/7 |
| **Spot Instances** | do 90% | Brak, ale może być przerwane | Batch, ML training, tolerancja przerwań |

**Dodatkowe sposoby na oszczędności:**
- **Azure Hybrid Benefit** — wykorzystanie istniejących licencji Windows/SQL Server (do 40% oszczędności).
- **Dev/Test Pricing** — tańsze ceny dla środowisk developerskich (wymaga subskrypcji VS).
- **Azure Savings Plans** — zobowiązanie do określonej kwoty/h przez 1-3 lata (elastyczniej niż RI).

---

## Skalowanie i elastyczność

### **Vertical Scaling (Scale Up / Scale Down)**
Zwiększanie lub zmniejszanie *mocy pojedynczej instancji* zasobu — np. większy rozmiar VM (więcej CPU/RAM/dysk), mocniejsza baza danych, większy plan App Service.

<img src="assets/vertical_scaling.svg">

**Charakterystyka:**
- Skalowanie „w górę” = mocniejsza maszyna.
- Skalowanie „w dół” = tańsza/słabsza maszyna.
- Zwykle **wymaga restartu** (przestój zależny od usługi).
- Ograniczenia sprzętowe — istnieje „sufit” skali.

**Kiedy używać:** bazy danych, monolity, workloady zależne od pojedynczego węzła.

---

### **Horizontal Scaling (Scale Out / Scale In)**
Dodawanie lub usuwanie *instancji zasobu*. Zamiast jednej mocnej maszyny — wiele mniejszych pracujących równolegle.

<img src="assets/horizontal_scaling.svg">

**Charakterystyka:**
- Scale out = dodawanie instancji.
- Scale in = usuwanie instancji.
- **Brak przestojów** — nowa instancja dołącza automatycznie.
- Wymaga stateless lub odpowiedniego przechowywania sesji.

**Przykłady:**  
- VMSS (Virtual Machine Scale Sets)  
- App Service z wieloma instancjami  
- Kubernetes / AKS (HPA, VPA, node autoscaling)

**Kiedy używać:** API, web, mikroserwisy, obciążenia rozproszone.

---

### **Autoskalowanie (Automatic Scaling / Auto‑Scale)**
Automatyczne zwiększanie/zmniejszanie liczby instancji lub mocy zasobu na podstawie *metryk, progów i reguł*.

<img src="assets/autoscaling.svg">

**Rodzaje autoskalowania:**
- **Metric-based** – CPU, RAM, latency, queue depth, request count.
- **Schedule-based** – konkretne godziny/dni (np. nocne / weekendy).
- **Event-based** – zdarzenia aplikacyjne (np. komunikat w kolejce).
- **Predictive autoscaling** – AI/ML przewiduje obciążenie (np. Azure Functions Premium, App Service Premium).

**Co potrafi autoscaler:**
- Dynamicznie dołącza/usuwa instancje VM / kontenerów / workerów.
- Reaguje na przeciążenia i koszt.
- Zapewnia odporność — jeśli instancja padnie, zostaje automatycznie odtworzona.

**Kiedy używać:** zmienne obciążenia, obciążenia sezonowe, systemy o ruchu skokowym (e‑commerce, IoT, integracje).

---

### **Additional Concepts (warto dodać w chmurze)**

#### **Bursting**
Tymczasowe podniesienie mocy zasobu ponad nominalną (np. Azure Premium Disks, niektóre VM).

#### **Load Balancing**
Load balancer rozprowadza ruch między instancjami, umożliwiając skuteczne scale-out.

#### **Stateless Architecture**
Wymagana do efektywnego autoskalowania — stan przechowywany poza instancjami (cache/DB/redis).

- Ważna różnica:
  - **Stateless** to serwer, który nie pamięta niczego między żądaniami.
  - **Stateful** to serwer, który musi pamiętać stan, więc kolejne żądania muszą wracać do niego.

#### **Throttling / Rate Limiting**
Kontrola obciążenia aplikacji i API w czasie autoskalowania — ochrona przed lawinowym ruchem.

---

## Odporność, Wysoka Dostępność i Disaster Recovery

### **High Availability (HA) – Wysoka dostępność**
Projektowanie infrastruktury w taki sposób, aby usługa pozostawała dostępna mimo awarii pojedynczych elementów.

<img src="assets/high_availability.svg">

**Kluczowe mechanizmy:**
- **Redundancja zasobów** – co najmniej 2 instancje (VM, kontenery, App Service) pracujące równolegle.
- **Availability Zones (AZ)** – fizycznie odseparowane strefy w regionie, każda z własnym zasilaniem, chłodzeniem i siecią.
- **Load Balancing** – równoważenie ruchu między instancjami (L4/L7).
- **Health probes** – automatyczne wykrywanie niedziałających instancji i wyłączanie ich z rotacji.
- **Autoskalowanie** – dodawanie instancji, aby uniknąć przeciążeń.

**Cel:** minimalizacja przestojów (downtime) w obrębie *jednego regionu*.

---

### **Fault Tolerance – Odporność na awarie**
System działa nawet wtedy, gdy część komponentów ulegnie całkowitej awarii — bez utraty usługi i bez przerwy.

<img src="assets/resilience.svg">

**Cechy architektury odpornej na awarie:**
- **Brak pojedynczego punktu awarii (SPOF)** — każdy element ma co najmniej jeden duplikat.
- **Active/Active** – wszystkie instancje stale działają i są gotowe przejąć ruch.
- **Self‑healing** – automatyczne zastępowanie uszkodzonych zasobów (VMSS, Kubernetes).
- **Dane replikowane synchronicznie** – brak utraty danych przy padzie jednego węzła (np. ZRS dla storage).

**Cel:** ciągłość działania przy awariach sprzętu, instancji, stref AZ lub fragmentów infrastruktury.

---

### **Disaster Recovery (DR) – Odzyskiwanie po awarii**
Zabezpieczenie na wypadek katastrofalnej awarii całego regionu lub utraty danych.  
Skupia się na **przywróceniu pracy systemu w innym regionie**.

<img src="assets/disaster_recovery.svg">

**Podstawowe pojęcia:**
- **RTO (Recovery Time Objective)** – jak szybko system musi zostać przywrócony (czas).
- **RPO (Recovery Point Objective)** – ile danych można stracić (czas od ostatniej replikacji).

**Techniki w Azure:**
- **Region Pairs** – każdy region ma sparowany region w tej samej geografii (szybsze przywracanie, chroniona aktualizacja).
- **Asynchroniczna replikacja danych** – Storage GRS/RA‑GRS, SQL Geo-Replication, Cosmos DB multi-region.
- **Azure Site Recovery (ASR)** – automatyczna replikacja VM, odtwarzanie aplikacji, orchestracja failover.
- **Backupy i snapshoty** – dane krytyczne przechowywane niezależnie od głównej infrastruktury.

**Scenariusze DR:**
- **Cold Standby** – zasoby odtwarzane dopiero po awarii (najtańsze).
- **Warm Standby** – ograniczona liczba maszyn działa w regionie zapasowym.
- **Hot Standby / Active-Passive** – pełna gotowość, ale ruch idzie tylko do jednego regionu.
- **Active-Active Multi‑Region** – oba regiony obsługują ruch jednocześnie.

**Cel:** utrzymanie pracy biznesu nawet w przypadku utraty całego regionu.

---

### **Podsumowanie różnic**

| Mechanizm            | Ochrona przed awarią | Poziom geograficzny | Przestój | Koszt |
|----------------------|----------------------|----------------------|----------|-------|
| **High Availability** | awaria instancji, AZ | 1 region             | bardzo niski | średni |
| **Fault Tolerance**  | pełna odporność       | 1 region (Active/Active) | praktycznie brak | wysoki |
| **Disaster Recovery** | awaria regionu        | 2+ regiony           | zależny od RTO | zróżnicowany |

---

### **Dobre praktyki odporności systemów**
- Projektuj **stateless** — stan w DB, cache, storage.
- Używaj **multi-zone** dla HA i **multi-region** dla DR.
- Testuj **regularnie failover** i aktualizuj runbooki DR.
- Wymuszaj **replikację danych** zgodnie z RPO.
- Zapewnij **monitoring + alerty** (Azure Monitor, KQL).

---

<a id="sec-02-azure-architecture"></a>
## 2. Azure Architecture (Architektura i hierarchia)
**Hierarchia i scope:**

**Hierarchia i scope — logika porządkowania zasobów w Azure**

Poniższy diagram przedstawia pełną hierarchię zarządzania w Azure — od najwyższego poziomu tożsamości (Tenant), przez struktury organizacyjne (Management Groups), aż po subskrypcje, grupy zasobów i pojedyncze zasoby.

<img src="assets/hierarchy_scope.svg">

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

<img src="assets/azure_hierarchy.svg">

- Jeden **Resource** może należeć tylko do jednej **Resource Group**.

## Globalna infrastruktura Azure

<img src="assets/azure_global_architecture.svg">

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

<img src="assets/arm_layer.svg">

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

<a id="sec-03-compute-services"></a>
## 3. Compute Services (Usługi obliczeniowe)

- **Virtual Machines (VM)**  

    Elastyczne maszyny wirtualne w wielu seriach (B, Dv5/Ev5, pamięciochłonne, obliczeniowe, GPU).  
    Obsługują **managed disks**, **VM Extensions** i dowolne systemy operacyjne.

    <img src="assets/vm.svg">

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

    <img src="assets/avsets.svg">

    **Fault Domains** czyli niezależne fizyczne strefy (różne racki, zasilanie, sieć).  

    **Update Domains** czyli logiczne grupy aktualizacji, restartowane osobno.

    - **VM Scale Sets (VMSS)**  

    **VM Scale Sets** to usługa umożliwiająca automatyczne uruchamianie, skalowanie i zarządzanie wieloma identycznymi maszynami wirtualnymi (VM). Wszystkie instancje oparte są na jednym *modelu* konfiguracji, co zapewnia spójność środowiska przy dużej skali.

    <img src="assets/vmss.svg">

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

    <img src="assets/appservice.svg">

    Umożliwia łatwe wdrażanie aplikacji, obsługuje **deployment slots**, integruje się z CI/CD  
    (GitHub Actions, Azure DevOps) i zapewnia automatyczne skalowanie instancji w zależności od obciążenia.

- **Azure Functions**

    **Serverless compute** uruchamiany „na żądanie” — kod wykonuje się tylko wtedy, gdy pojawi się zdarzenie, a Ty płacisz wyłącznie za czas wykonania.
   
    Funkcje działają w planach **Consumption** (auto‑skalowanie, płatność za wykonania) oraz **Premium** (stałe instancje, VNET, dłuższe timeouty), obsługując dziesiątki triggerów i bindingów.

    <img src="assets/azurefunctions.svg">

    Azure Functions = event‑driven + serverless + automatyczne skalowanie + bogaty ekosystem triggerów i bindingów.

    ### Plany hostingowe (Hosting Plans)

    <img src="assets/functions_hosting_plans.svg">

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

    <img src="assets/functions_triggers_bindings.svg">

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
        
        <img src="assets/azurefunctions_standard.svg">
        
        - Brak stanu (Stateless)
        - Krótki stan działania
        - Brak koordynacji, ręczna koordynacja
        - Brak wbudowanego retry
        - Brak natywnego wsparcia dla workflow
    - **Durable** functions
        
        <img src="assets/azurefunctions_durable.svg">
        
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

    **JavaScript/Node.js (HTTP Trigger):**
    ```javascript
    module.exports = async function (context, req) {
        const name = req.query.name || req.body?.name || 'World';
        
        context.res = {
            status: 200,
            body: `Hello, ${name}!`
        };
    };
    ```

    **Python (HTTP Trigger):**
    ```python
    import azure.functions as func

    def main(req: func.HttpRequest) -> func.HttpResponse:
        name = req.params.get('name', 'World')
        return func.HttpResponse(f"Hello, {name}!")
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

    <img src="assets/aci.svg">

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

    <img src="assets/azurekubernetesservice.svg">

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

    <img src="assets/azurebastion.svg">

    Działa przez przeglądarkę (HTML5), minimalizuje powierzchnię ataku, usuwa konieczność otwierania portów 3389/22. 

    Zapewnia dostęp w pełni przez sieć prywatną (VNet).

---

<a id="sec-04-networking"></a>
## 4. Networking (Sieci i łączność)

---

### **VNet / Subnets / UDR**

Podstawowa sieć prywatna w Azure – kontrola adresacji, segmentacji oraz tras statycznych (UDR) do kierowania ruchu.

- **VNet (Virtual Network)**  
  Logiczna sieć prywatna w Azure, działająca podobnie do klasycznej sieci LAN.  
  Umożliwia pełną kontrolę nad adresacją IP, komunikacją między zasobami, integracją z siecią on‑prem oraz izolacją środowisk.

- **Subnets**  
  Podział VNet na mniejsze logiczne segmenty w celu separacji usług, zwiększenia bezpieczeństwa i kontroli dostępu.  
  Pozwalają przypisywać różne NSG, UDR i zasady per segment, np. subnet „App”, „DB”, „Gateway”.

- **UDR (User Defined Routes)**  
  Niestandardowe trasy wymuszające kierowanie ruchu przez określone urządzenia, np. firewall NVA.

<img src="assets/azurevnets.svg">

Najczęstsze zastosowania:
- kierowanie ruchu przez firewall (NVA / Azure Firewall)  
- wymuszenie ruchu między subnetami  
- tunelowanie ruchu do on‑prem / VPN / ExpressRoute  
- blokowanie lub przekierowanie wybranych sieci

### **Azure DNS**

Usługa hostowania stref DNS w Azure:
- public DNS zones dla rekordów dostępnych z Internetu,
- private DNS zones dla rozwiązywania nazw wewnątrz VNet,
- integracja z Private Endpoint (rekordy `privatelink`).

Egzaminowo: **Azure DNS odpowiada za rozwiązywanie nazw**, a nie za filtrowanie ruchu czy balansowanie L7.

---

### **NSG (Network Security Groups)**

Podstawowy firewall warstwy L3/L4 dla subnetów i NIC VM.

Reguły filtrują:
- źródło / cel (IP, zakres, tag),
- porty,
- protokół.

> NSG działa jak ACL – pozwala / blokuje ruch, ale go **nie inspektuje**.

---

### **ASG (Application Security Groups)**

Logiczne grupowanie VM według funkcji (App, API, DB).  
NSG może wskazywać ASG zamiast adresów IP — ułatwia to zarządzanie dynamicznymi środowiskami.

---

### **Azure Firewall**

<img src="assets/azure_firewall.svg">

Azure Firewall to w pełni zarządzany, stanowy firewall warstwy L3–L7, wdrażany jako usługa PaaS w dedykowanym subnecie (`AzureFirewallSubnet`).

**Kluczowe funkcje:**
| Funkcja | Opis |
|---------|------|
| **FQDN Filtering** | Filtrowanie po nazwach domenowych (np. `*.microsoft.com`) |
| **URL Filtering** | Filtrowanie po pełnych URL (Premium) |
| **Web Categories** | Blokowanie kategorii stron (gambling, adult, etc.) |
| **Threat Intelligence** | Automatyczne blokowanie znanych malicious IPs |
| **TLS Inspection** | Deszyfrowanie i inspekcja HTTPS (Premium) |
| **SNAT/DNAT** | Source/Destination NAT dla ruchu sieciowego |
| **IDPS** | Intrusion Detection & Prevention (Premium) |

#### Warstwy Azure Firewall

| Cecha | Standard | Premium |
|-------|----------|---------|
| L3-L4 filtering | Tak | Tak |
| FQDN filtering | Tak | Tak |
| Threat Intelligence | Alert/Deny | Alert/Deny |
| TLS Inspection | Nie | Tak |
| IDPS | Nie | Tak |
| URL Filtering | Nie | Tak |
| Web Categories | Podstawowe | Rozszerzone |
| Cena | ~$1.25/godz | ~$1.75/godz |

#### Kolekcje reguł (Rule Collections)

Reguły są przetwarzane w kolejności priorytetu (niższy numer = wyższy priorytet):

**1. NAT Rules (DNAT)** - priorytet 100-300
```
Przekierowanie ruchu przychodzącego z Internetu do wewnętrznych zasobów.

Przykład:
  Source: * (Internet)
  Destination: Firewall Public IP:443
  Translated: 10.0.1.5:443 (internal web server)
```

**2. Network Rules (L3/L4)** - priorytet 200-65000
```
Filtrowanie po IP, porcie i protokole.

Przykład:
  Source: 10.0.0.0/16 (VNet)
  Destination: 8.8.8.8
  Port: 53
  Protocol: UDP
  Action: Allow
```

**3. Application Rules (L7)** - priorytet 200-65000
```
Filtrowanie HTTP/HTTPS po FQDN lub URL.

Przykład:
  Source: 10.0.0.0/16
  Target FQDNs: *.windowsupdate.com, *.microsoft.com
  Protocol: Https:443
  Action: Allow
```

#### Konfiguracja przez Azure CLI

```bash
# Utworzenie Resource Group i VNet
az group create --name myFWRG --location westeurope
az network vnet create --name myVNet --resource-group myFWRG \
    --address-prefix 10.0.0.0/16

# Utworzenie AzureFirewallSubnet (wymagana nazwa!)
az network vnet subnet create --name AzureFirewallSubnet \
    --resource-group myFWRG --vnet-name myVNet \
    --address-prefix 10.0.1.0/26

# Utworzenie Public IP dla Firewall
az network public-ip create --name myFW-PIP --resource-group myFWRG \
    --sku Standard --allocation-method Static

# Utworzenie Azure Firewall
az network firewall create --name myFirewall \
    --resource-group myFWRG --location westeurope

# Konfiguracja IP
az network firewall ip-config create --firewall-name myFirewall \
    --name myFWConfig --public-ip-address myFW-PIP \
    --resource-group myFWRG --vnet-name myVNet

# Dodanie Network Rule Collection
az network firewall network-rule create \
    --firewall-name myFirewall --resource-group myFWRG \
    --collection-name "AllowDNS" --priority 200 \
    --action Allow --name "DNS" \
    --source-addresses "10.0.0.0/16" \
    --destination-addresses "8.8.8.8" "8.8.4.4" \
    --destination-ports 53 --protocols UDP

# Dodanie Application Rule Collection
az network firewall application-rule create \
    --firewall-name myFirewall --resource-group myFWRG \
    --collection-name "AllowWeb" --priority 300 \
    --action Allow --name "MicrosoftUpdates" \
    --source-addresses "10.0.0.0/16" \
    --target-fqdns "*.windowsupdate.com" "*.microsoft.com" \
    --protocols Https=443
```

#### Firewall Policy (zalecane)

Firewall Policy to centralny obiekt zarządzania regułami, który można przypisać do wielu firewalli:

```bash
# Utworzenie Firewall Policy
az network firewall policy create --name myPolicy \
    --resource-group myFWRG --location westeurope

# Utworzenie Rule Collection Group
az network firewall policy rule-collection-group create \
    --name DefaultRuleCollectionGroup \
    --policy-name myPolicy --resource-group myFWRG \
    --priority 100

# Przypisanie Policy do Firewall
az network firewall update --name myFirewall \
    --resource-group myFWRG \
    --firewall-policy myPolicy
```

#### Hub-Spoke Architecture

<img src="assets/hub_spoke_architecture.svg">

**Hub-Spoke** to najpopularniejsza architektura sieciowa w Azure dla środowisk enterprise. Opiera się na centralnym VNecie (Hub), który łączy się z wieloma VNetami aplikacyjnymi (Spokes) za pomocą VNet Peering.

**Koncepcja:**
- **Hub VNet** = centralny węzeł z usługami współdzielonymi
- **Spoke VNets** = izolowane VNety dla różnych aplikacji/środowisk
- **VNet Peering** = połączenie Hub ↔ Spoke (bez tranzytywności!)

**Co umieszczamy w Hub:**
| Usługa | Rola |
|--------|------|
| **Azure Firewall** | Centralny firewall dla całego ruchu |
| **VPN Gateway** | Połączenie z on-premises |
| **ExpressRoute Gateway** | Dedykowane połączenie z on-prem |
| **Azure Bastion** | Bezpieczny dostęp RDP/SSH |
| **DNS Server** | Centralne rozwiązywanie nazw |
| **NVA** | Network Virtual Appliances (3rd party) |

**Co umieszczamy w Spokes:**
- Aplikacje produkcyjne (Spoke: Production)
- Środowiska deweloperskie (Spoke: Development)
- Bazy danych (Spoke: Databases)
- Zarządzanie i monitoring (Spoke: Management)

**Dlaczego Hub-Spoke?**

| Zaleta | Opis |
|--------|------|
| **Izolacja** | Każdy Spoke jest oddzielony od innych |
| **Centralne bezpieczeństwo** | Jeden Firewall kontroluje cały ruch |
| **Koszt** | Współdzielone usługi (VPN GW, Firewall) zamiast duplikacji |
| **Skalowalność** | Łatwe dodawanie nowych Spokes |
| **Compliance** | Różne regulacje dla różnych Spokes |

**Ważne: VNet Peering NIE jest tranzytywny!**

```
Spoke1 ←→ Hub ←→ Spoke2

Spoke1 NIE może rozmawiać z Spoke2 bezpośrednio przez peering!
Ruch musi przejść przez Hub (np. przez Azure Firewall lub NVA).
```

**Rozwiązania dla komunikacji Spoke-to-Spoke:**
1. **Azure Firewall/NVA** - ruch routowany przez Hub (UDR)
2. **Virtual WAN** - natywna tranzytywność
3. **Bezpośredni peering** - Spoke ↔ Spoke (dodatkowy koszt, brak kontroli)

**Przykładowa konfiguracja sieci:**
```
Hub VNet:        10.0.0.0/16
  ├── AzureFirewallSubnet:  10.0.1.0/26
  ├── GatewaySubnet:        10.0.2.0/27
  └── AzureBastionSubnet:   10.0.3.0/26

Spoke1 (Prod):   10.1.0.0/16
Spoke2 (Dev):    10.2.0.0/16
Spoke3 (DB):     10.3.0.0/16
On-premises:     192.168.0.0/16
```

**Konfiguracja UDR dla Spoke-to-Spoke przez Firewall:**

```bash
# Route Table dla każdego Spoke
az network route-table create --name Spoke1-RT --resource-group myRG

# Route do innych Spokes przez Firewall
az network route-table route create --name ToSpoke2 \
    --route-table-name Spoke1-RT --resource-group myRG \
    --address-prefix 10.2.0.0/16 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 10.0.1.4  # Firewall private IP

# Route do on-premises przez Firewall
az network route-table route create --name ToOnPrem \
    --route-table-name Spoke1-RT --resource-group myRG \
    --address-prefix 192.168.0.0/16 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 10.0.1.4

# Default route (Internet) przez Firewall
az network route-table route create --name ToInternet \
    --route-table-name Spoke1-RT --resource-group myRG \
    --address-prefix 0.0.0.0/0 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 10.0.1.4
```

**Porównanie: Hub-Spoke vs Virtual WAN**

| Cecha | Hub-Spoke (manual) | Azure Virtual WAN |
|-------|-------------------|-------------------|
| Tranzytywność | UDR + Firewall/NVA | Natywna |
| Zarządzanie | Ręczne | Automatyczne |
| Skalowalność | Do ~100 Spokes | Do 1000+ |
| Koszt | Niższy (DIY) | Wyższy (managed) |
| Złożoność | Średnia | Niska |
| Multi-region | Wymaga konfiguracji | Wbudowane |

> **Egzamin:** Hub-Spoke to wzorzec architektoniczny, nie usługa Azure. Virtual WAN to managed service, który implementuje Hub-Spoke automatycznie.

#### Porównanie z NSG

| Cecha | NSG | Azure Firewall |
|-------|-----|----------------|
| Warstwa | L3/L4 | L3-L7 |
| Stateful | Tak | Tak |
| FQDN filtering | Nie | Tak |
| URL filtering | Nie | Tak (Premium) |
| Threat Intelligence | Nie | Tak |
| TLS Inspection | Nie | Tak (Premium) |
| Centralne zarządzanie | Nie | Tak (Policy) |
| Logging | Flow Logs | Azure Monitor |
| Koszt | Darmowe | ~$1.25-1.75/godz |
| Kiedy używać | Per-subnet ACL | Centralny firewall |

> **Egzamin:** NSG = prosty ACL na poziomie subnet/NIC, Azure Firewall = enterprise-grade firewall z inspekcją ruchu.

---

### **VNet Peering**

Bezpośrednie, szybkie połączenie dwóch VNetów **bez tuneli, NAT i VPN**.

<img src="assets/vnetpeering.svg">

Cechy:
- komunikacja IP‑to‑IP po prywatnym backbone Azure,  
- bardzo niskie opóźnienia,  
- brak NAT,  
- globalny peering między regionami.

---

### **VPN Gateway**

Brama VPN umożliwiająca szyfrowane połączenia IPsec.

<img src="assets/vpngateway.svg">

#### **Site‑to‑Site (S2S)**
- Używasz S2S, gdy chcesz połączyć całą sieć on‑prem z Azure.
- tunel router ↔ Azure (cała sieć ↔ VNet),  
- wymaga publicznego IP po stronie on‑prem.

#### **Point‑to‑Site (P2S)**  
- VPN dla pojedynczego użytkownika (OpenVPN / IKEv2 / Azure VPN Client),  
- nie wymaga infrastruktury po stronie użytkownika.

<img src="assets/vpns2sp2p.svg">

---

### **ExpressRoute**

Prywatne łącze WAN do Azure, całkowicie poza publicznym Internetem.

<img src="assets/expressroute.svg">

Cechy:
- 50 Mbps – 100 Gbps,  
- stabilne parametry i SLA operatorskie,  
- private peering (VNet), Microsoft peering (wybrane usługi publiczne Microsoft),  
- idealny do krytycznych aplikacji i masowych migracji.
- połączenie jest prywatne, ale nie szyfrowane.

Rodzaje:
- **Standard** – połączenia do regionu w tym samym obszarze geograficznym.
- **Premium** – dostęp do wszystkich regionów + większe limity VNetów/tras.

---

### **Public Endpoint / Private Endpoint / Service Endpoint**

<img src="assets/privateendpoints.svg">

#### **Public Endpoint**
- publiczny adres IP usługi PaaS,
- dostęp przez Internet (można ograniczyć firewallami / Access Restrictions),
- brak izolacji sieciowej — ruch zawsze trafia na publiczny endpoint.

#### **Private Endpoint**
- prywatny IP w Twoim VNet (z subnetu),
- dostęp przez Azure Private Link (prywatny backbone),
- wymaga prywatnych stref DNS (privatelink.azure.com),
- najwyższy poziom izolacji (pełne odcięcie Internetu).

#### **Service Endpoint**
- nie tworzy prywatnego IP,
- ruch do PaaS przechodzi po prywatnym backbone Azure,
- nadal używa publicznego endpointu usługi,
- mniej izolacji niż Private Endpoint.

Pułapki:
- Public Endpoint + złe reguły = przypadkowe wystawienie danych do Internetu
- Private Endpoint może „schować” usługę przed światem (konieczny poprawny DNS)
- Service Endpoint nie działa cross‑tenant
- Private Endpoint blokuje publiczny dostęp przy ustawieniu Deny public network access
- Service Endpoint nie korzysta z NAT — usługa widzi regionalny adres źródłowy Azure

---

### **Public IP (Basic vs Standard)**

- **Basic**
  - brak stref AZ,  
  - mniej bezpieczne,  
  - brak SLA.

- **Standard**
  - strefowy (zone‑redundant),  
  - domyślnie „secure by default”,  
  - płatny za sam przydział.

---

### **NAT Gateway**

Zalecane rozwiązanie dla wychodzącego ruchu Internetowego (egress).

Cechy:
- jedno źródło ruchu (stały publiczny IP/prefix),  
- wysoka skalowalność (miliony SNAT),  
- lepsze niż SNAT na Load Balancer.

---

### **DDoS Protection**

- **Basic** – zawsze włączone, automatyczne.  
- **Standard** – ochrona warstw **L3/L4**: wolumetria, SYN flood, UDP flood.

> DDoS Standard nie chroni L7 — od tego jest **WAF**.

---

### **WAF (Web Application Firewall)**

Firewall L7 (HTTP/S), ochrona webowych aplikacji przed:
- OWASP Top 10,  
- botami,  
- wstrzykiwaniem,  
- anomaliami.

Dostępny jako:
- WAF on Application Gateway (regionalny),  
- WAF on Azure Front Door (globalny edge).

---

### **Load Balancing i Edge Networking**

#### **Azure Load Balancer (L4)**  

<img src="assets/azureloadbalancer.svg">

Warstwa 4 – TCP/UDP. Szybki, prosty, idealny dla VM/VMSS.

#### **Application Gateway (L7 + WAF)**  

<img src="assets/applicationgateway.svg">

Warstwa 7 – routing HTTP/S, WAF, SSL termination, cookie affinity.

#### **Traffic Manager (DNS LB)**  

<img src="assets/trafficmanager.svg">

Stosowany na poziomie DNS – georouting, failover, weighted.

#### **Azure Front Door (L7 Edge)**   

<img src="assets/frontdoor.svg">

Globalny CDN + smart routing L7 + WAF na edge.

---

### **Porównanie usług LB (skrót)**

| Usługa | Warstwa | Zastosowania | Zakres |
|--------|---------|--------------|--------|
| Load Balancer | L4 | VM/VMSS, TCP/UDP | Regional |
| Application Gateway | L7 | Web apps + WAF | Regional |
| Traffic Manager | DNS | Global routing DNS | Global |
| Front Door | L7 Edge | Global web apps, WAF, caching | Global |

---

### **Network Watcher**

Narzędzia do diagnostyki:
- IP Flow Verify,  
- NSG Flow Logs,  
- Connection Troubleshoot,  
- Packet Capture.

Umożliwia analizę ruchu i debugowanie problemów sieciowych.

---

<a id="sec-05-storage"></a>
## 5. Storage (Przechowywanie danych)

**Azure Storage** to zestaw usług do przechowywania plików, obiektów, danych NoSQL oraz dysków dla maszyn wirtualnych. 

Poniżej znajdziesz jasne i zrozumiałe wyjaśnienie, czym są poszczególne typy storage, po co się ich używa oraz jak działa redundancja danych.

<img src="assets/storagetypes.svg">

---

## **Typy danych i ich zastosowania**

**Blob Storage** (obiekty / pliki)
Najbardziej uniwersalne miejsce na duże, niestrukturalne dane.

- **Hot** najczęściej używane dane, najwyższy koszt przechowywania, najniższy koszt dostępu.
- **Cool** dane rzadziej używane (dni–miesiące).
- **Cold** dane używane bardzo rzadko (miesiące–lata), tańsze od Cool, droższe od Archive pod względem odczytu.
- **Archive** dane długoterminowe, najniższy koszt przechowywania, ale bardzo drogi i wolny odczyt (kilka godzin).

Zastosowania: kopie zapasowe, logi, obrazy, filmy, dane ML, statyczne pliki

---

**Azure Files** (udzialy sieciowe)
Chmurowy odpowiednik klasycznego Windows File Server.

- SMB/NFS udziały sieciowe  
- Azure File Sync do synchronizacji z lokalnym serwerem plików  
- Zastosowania: udziały użytkowników, zasoby współdzielone, migracje serwerów plików

**Access Tiers** dla Azure Files
Azure Files obsługuje warstwy wydajności i warstwy dostępu, podobnie jak Blob Storage — ale inaczej działające.

Warstwy wydajności (Performance Tiers):
- **Standard** (HDD/SSD) – najtańszy, idealny dla klasycznych udziałów SMB/NFS
- **Premium** (SSD) – wysoka przepustowość i niski latency (np. FSLogix, intensywne I/O)

---

**Queue Storage** (kolejki wiadomości)
Prosta, skalowalna kolejka komunikatów dla architektur event‑driven.

- komunikacja asynchroniczna między usługami  
- idealna dla mikroserwisów, workerów, batch processing

---

**Table Storage** (NoSQL key‑value)
Tania, szybka i schematless baza NoSQL do ogromnych ilości danych.

- dane w formie PartitionKey + RowKey  
- idealne dla logów, metadanych, telemetrii, konfiguracji  
- alternatywa: Cosmos DB Table API

---

**Managed Disks** (dyski dla VM)
Dyski zarządzane przez Azure, wykorzystywane przez maszyny wirtualne.

- Premium SSD — produkcja, wysokie IOPS  
- Standard SSD — solidny kompromis cena/wydajność  
- Standard HDD — archiwizacja, testy  
- Ultra SSD — bardzo wysokie IOPS i throughput  
- OS Disk / Data Disk / Temp Disk (lokalny, nietrwały)

---

## **Redundancja danych (odporność na awarie)**

<img src="assets/storageredundancy.svg">

**LRS — Locally Redundant Storage**
- 3 kopie danych w jednym datacenter  
- najniższy koszt  
- odporność tylko w obrębie pojedynczego DC

**ZRS — Zone Redundant Storage**
- replikacja między 3 strefami Availability Zone  
- ochrona przed awarią całej strefy  
- stabilny wybór dla środowisk produkcyjnych

**GRS — Geo Redundant Storage**
- LRS + kopia do regionu pary  
- 6 kopii danych łącznie  
- ochrona przed awarią całego regionu

**RA‑GRS — Read‑Access GRS**
- jak GRS, ale region zapasowy jest możliwy do odczytu  
- idealne dla globalnych aplikacji, które potrzebują dostępu read-only w czasie awarii

### Storage account options (skrót)

- **Performance tiers**: `Standard` (HDD/SSD) oraz `Premium` (SSD).
- **Redundancy options**: LRS, ZRS, GRS, RA‑GRS (zależnie od typu konta i regionu).
- **Access model**: public endpoint, private endpoint, firewall/VNet rules.
- **Security options**: szyfrowanie at-rest (Microsoft-managed keys lub customer-managed keys), soft delete, immutability (dla wybranych scenariuszy).

Na AZ‑900 kluczowe jest dobranie konta pod: koszt, wydajność, wymagania sieciowe i odporność.

### Backup vs Replication (częsta pułapka)

- **Replication** (LRS/ZRS/GRS/RA‑GRS) zwiększa dostępność i trwałość danych, ale **nie zastępuje backupu**.
- **Backup** służy do odtworzenia danych po błędach logicznych (np. przypadkowe usunięcie, ransomware, nadpisanie).
- Egzaminowo: replikacja chroni głównie przed awarią infrastruktury, backup chroni przed utratą danych z powodów operacyjnych i bezpieczeństwa.

### Dostęp do Storage: Entra RBAC vs SAS vs Access Keys

| Metoda | Kiedy używać | Ryzyko / uwaga |
|---|---|---|
| Entra ID + RBAC | domyślnie dla użytkowników i aplikacji | najlepszy model least privilege |
| SAS Token | delegowany, czasowy dostęp do konkretnego obiektu/zakresu | kontroluj TTL i uprawnienia (Read/Write/List) |
| Access Keys | tylko gdy nie da się użyć RBAC/SAS | szerokie uprawnienia; wymaga rotacji kluczy |

Rekomendacja praktyczna: **najpierw RBAC**, potem **SAS** dla delegacji, a **Access Keys** traktuj jako opcję awaryjną/legacy.

---

## **Narzędzia i migracje**

<img src="assets/storagetools.svg">

**Storage Explorer**
Narzędzie GUI do zarządzania Blob/Files/Queue/Table.

- przeglądanie i edycja danych  
- zarządzanie dostępem (SAS)  
- wygodne operacje na dużych strukturach plików  

**AzCopy**
Najszybsze narzędzie do przesyłania danych do/z Azure Storage.

- praca w CLI  
- wysoka wydajność  
- idealne do migracji dużych wolumenów

<img src="assets/azuremigrate.svg">

**Azure Migrate**
Aplikacje, VM‑ki i dane mogą być analizowane i migrowane do Azure.

- automatyczne rekomendacje  
- integracja z Azure Storage (np. blob staging)


<img src="assets/databox.svg">

**Data Box**  
Fizyczne urzadzenia Azure do przenoszenia bardzo duzych ilosci danych **offline**, bez wykorzystania Internetu.  
Stosowane wtedy, gdy lacze sieciowe jest zbyt wolne, niestabilne lub kosztowne.

- **Data Box Disk** – do ok. 40 TB; zestaw szyfrowanych dyskow SSD USB.  
  Szybkie wdrozenie, lekkie migracje, wiele lokalizacji.

- **Data Box** – do ok. 100 TB; pelne urzadzenie NAS w formie walizki.  
  Wysoka przepustowosc lokalna, SMB/NFS/REST, duze migracje danych.

- **Data Box Heavy** – do ok. 1 PB; przemyslowe urzadzenie na kolach.  
  Bardzo duze migracje (archiwa, data lakes, media), transfery wielogigabitowe.
 
---

<a id="sec-06-identity-access"></a>
## 6. Identity & Access (Microsoft Entra)

**Microsoft Entra ID**  
Centralny system tozsamosci w Azure i Microsoft 365. Odpowiada za uwierzytelnianie uzytkownikow, aplikacji i urzadzen. 

<img src="assets/msentraid.svg">
 
Kluczowe funkcje:
- **SSO (Single Sign-On)** – jedno logowanie do wielu aplikacji (SaaS, on-prem, Azure).  
- **MFA (Multi-Factor Authentication)** – dodatkowe potwierdzenie tozsamosci (aplikacja, SMS, klucz FIDO2).  
- **External Identities** – bezpieczny dostep dla partnerow/klientow bez tworzenia kont wewnetrznych.  
- **Passwordless** – logowanie bez hasla (Windows Hello, FIDO2, Phone Sign-in).  
- **Directory roles** – role administracyjne dla zarzadzania tozsamoscia.

**Microsoft Entra Domain Services (Entra DS)**
- zarządzane usługi domenowe kompatybilne z AD (LDAP, Kerberos, NTLM),
- przydatne dla starszych aplikacji wymagających klasycznych mechanizmów domenowych,
- działa jako usługa zarządzana — bez stawiania własnych kontrolerów domeny.

Egzaminowo: **Entra ID ≠ Entra Domain Services** (inne cele i funkcje).

**Jak Entra ID uwierzytelnia uzytkownika**?

<img src="assets/userauthorization.svg">

Proces logowania przebiega w kilku krokach:
1. **Identyfikacja** – uzytkownik podaje login (UPN), Entra ID ustala tenant i polityki.  
2. **Uwierzytelnienie** – haslo, passwordless (FIDO2/Hello), certyfikat lub SSO z urzadzenia.  
3. **Kontrole dostepu** (Conditional Access) – Entra ID ocenia ryzyko logowania, lokalizacje, stan urzadzenia, zgodnosc z politykami oraz wymaga MFA jesli to konieczne.  
4. **Wydanie tokenow** – po pozytywnym potwierdzeniu wydawane sa ID Token, Access Token i Refresh Token (OAuth2/OIDC), ktore aplikacje wykorzystuja do autoryzacji.

**Access Token** i **ID Token** zwykle wygasają po około **1 godzinie**, a **Refresh Token** działa dłużej (typowo do **90 dni nieaktywności**) — dokładny czas zależy od polityk bezpieczeństwa i konfiguracji tenantu.

**Jak uwierzytelniane sa aplikacje?**

<img src="assets/appauthorization.svg">

Aplikacje korzystaja z wlasnej tozsamosci aplikacyjnej:
- sekret klienta,  
- certyfikat,  
- lub **Managed Identity** (najbardziej bezpieczny model, bez sekretow).  

**Managed Identity** to mechanizm nadawania aplikacjom tozsamosci zarzadzanej w pelni przez Azure — bez sekretow, bez hasel, bez certyfikatow.  

<img src="assets/managedidentity.svg">

**Jak dziala Managed Identity**:
- aplikacja nie przechowuje żadnych sekretów ani kluczy  
- uwierzytelnia sie poprzez lokalny endpoint MSI dostepny tylko wewnatrz uslugi  
- Azure potwierdza, ze aplikacja ma tozsamosc MI oraz sprawdza jej role RBAC  
- Entra ID wystawia **Access Token** dla konkretnej uslugi  
- aplikacja dodaje token: `Authorization: Bearer <token>` i laczy sie z zasobem

**Rodzaje Managed Identity**:
- **System-assigned** – tożsamość jest związana z jednym zasobem; tworzy sie sama i znika wraz z zasobem.  
- **User-assigned** – osobny zasób tożsamości, który można przypisac do wielu aplikacji; bardziej elastyczne.

Aplikacja uwierzytelnia sie w Entra ID, a nastepnie otrzymuje **Access Token** do zasobów takich jak Storage, Key Vault, SQL, Event Hub itd.

**Jak uwierzytelniane sa urzadzenia?**

<img src="assets/deviceauthorization.svg">

Urzadzenia moga byc:
- Azure AD Registered (BYOD),  
- Azure AD Joined (urzadzenia firmowe),  
- Hybrid Joined (AD on‑prem + Entra ID).  

Entra ID sprawdza:
- czy urzadzenie jest zapisane w katalogu,  
- czy jest zgodne (Intune compliance),  
- czy spelnia polityki bezpieczenstwa organizacji.  

Dzieki temu mozliwe jest SSO, wymuszanie MFA, lub blokada dostepu dla niezaufanych urzadzen.

---

**RBAC (Role-Based Access Control)**  
Mechanizm autoryzacji w Azure oparty na rolach przypisywanych do okreslonych zakresow zasobow (scope).  
Umozliwia precyzyjne kontrolowanie, kto co moze zrobic w danym zasobie — zgodnie z zasada least privilege.

<img src="assets/rbac.svg">

Najwazniejsze elementy:

- **Role wbudowane**  
  Podstawowe role obejmuja:  
  - **Owner** – pelna administracja zasobami + zarzadzanie dostepem  
  - **Contributor** – pelna administracja zasobami, ale bez nadawania uprawnien  
  - **Reader** – tylko odczyt  

  Dodatkowo istnieja setki **ról specjalistycznych**, np. Storage Blob Reader, Key Vault Secrets Officer, Virtual Machine Contributor.

- **Custom roles**  
  Gdy role wbudowane sa zbyt szerokie, mozna tworzyc role **niestandardowe** oparte na pojedynczych akcjach (actions / notActions), 
  np. tylko „odczyt sekretow Key Vault” lub „restart VM bez mozliwosci kasowania”.

- **Scope (zakres) i dziedziczenie**  
  Scope definiuje, do czego rola ma zastosowanie:  
  - **Subscription** → dziedziczy na wszystkie Resource Groups i zasoby  
  - **Resource Group** → dziedziczy na zasoby w tej grupie  
  - **Resource** → dotyczy tylko jednego zasobu  
  Uprawnienia przypisane wyzej **zawsze dziedzicza w dol**, dlatego zaleca sie przypisywanie ról na mozliwie najnizszym poziomie.

- **Model decyzyjny**  
  Jeżeli uzytkownik ma wiele ról w danym scope, sumuja sie one (model additive).  
  Blokady resource lock **nie sa** elementem RBAC — dzialaja oddzielnie.

- **Zastosowania mechanizmu RBAC**  
  RBAC jest kluczowe dla bezpieczenstwa:  
  - ogranicza nadmiarowe uprawnienia  
  - zapewnia podzial obowiazkow (SoD)  
  - precyzyjnie definiuje, kto moze zarzadzac Storage, VM, SQL, App Service, Key Vault itd.  
  - integruje sie z **Managed Identity**, dzięki czemu aplikacje rowniez korzystaja z RBAC zamiast sekretow

---

**Conditional Access**
Mechanizm kontroli dostepu, ktory ocenia kontekst logowania i decyduje, czy uzytkownik powinien uzyskac dostep, zostac zablokowany, czy wykonac dodatkowa akcje (np. MFA).  

Jest to kluczowy element modelu **Zero Trust — zasada: „never trust, always verify”**.

<img src="assets/conditionalaccess.svg">

**Kluczowe atrybuty oceniane podczas logowania:**
- **Ryzyko logowania / ryzyko uzytkownika** – wykrywanie anomalii, podejrzanych lokalizacji, nietypowych zachowan.  
- **Lokalizacja** – kraje, zakresy IP, sieci zaufane, VPN, corporate network.  
- **Stan urzadzenia** – zgodnosc z Intune (compliant / non-compliant), typ OS, poziom zabezpieczen.  
- **Typ aplikacji i klienta** – przegladarka, aplikacja mobilna, desktopowa, klient legacy, API.  
- **Typ zasobu** – niektore aplikacje lub API moga wymagac mocniejszych zasad.  

**Dzialania (akcje) podejmowane przez zasady:**
- **Wymuszenie MFA** – dodatkowy czynnik potwierdzenia tozsamosci.  
- **Blokada dostepu** – gdy ryzyko jest zbyt duze lub warunki nie sa spelnione.  
- **Wymuszenie zgodnego urzadzenia** – tylko urządzenia spełniające wymagania Intune.  
- **Wymuszenie sesji bezhaslowej** – logowanie passwordless.  
- **Sesje z ograniczeniami** – np. zakaz pobierania plikow (MCAS/Defender for Cloud Apps).  

---

**PIM (Privileged Identity Management)**  
Mechanizm nadawania uprawnien administracyjnych na zasadzie JIT (Just‑In‑Time).

Najwazniejsze funkcje:
- Tymczasowa aktywacja roli tylko na okres wykonywanych zadan.  
- Wymuszenie uzasadnienia, zgody (approval) lub MFA przed nadaniem roli.  
- Audyt aktywacji, notyfikacje i alerty.  
- Minimalizacja ryzyka naduzyc przez stale podniesione uprawnienia.

---

**Managed Identities**  
Tozsamosc systemowa dla aplikacji dzialajacych w Azure — bez kluczy, bez hasel, bez sekretow.  

Zastosowania:
- Dostep aplikacji do Azure Storage, Key Vault, SQL, Event Hub itp.  
- Obsluga wbudowana w uslugi Azure (VM, App Service, Functions, Logic Apps).  
- Kluczowe korzysci:
  - automatyczne zarzadzanie tozsamoscia przez Azure  
  - brak potrzeby przechowywania i rotowania sekretow  
  - zgodnosc z Zero Trust  

Rodzaje:
- **System-assigned** – tozsamosc przypisana do jednego zasobu  
- **User-assigned** – tozsamosc tworzona jako oddzielny zasob, wspoldzielona miedzy aplikacjami

---

<a id="sec-07-security"></a>
## 7. Security (Bezpieczeństwo)

### Defense-in-depth (model warstwowy)

<img src="assets/defenseindepth.svg">

Bezpieczeństwo realizowane przez wiele warstw ochrony:
- **Physical** (DC),
- **Identity & Access** (MFA, CA, RBAC, PIM),
- **Perimeter/Network** (DDoS, Firewall, NSG, WAF),
- **Compute** (patching, hardening, EDR),
- **Application** (secure coding, API protection),
- **Data** (szyfrowanie, klasyfikacja, backup, immutability).

Cel: przełamanie jednej warstwy nie powinno oznaczać kompromitacji całego środowiska.

- **Microsoft Defender for Cloud**  
  Kompleksowa platforma zabezpieczeń chmurowych zapewniająca:
  - **Secure Score** – miernik poziomu bezpieczeństwa środowiska z rekomendacjami działań naprawczych.
  - **Rekomendacje bezpieczeństwa** – zalecenia dot. konfiguracji, podatności i zgodności ze standardami (CIS, NIST, ISO).
  - **Ochrona obciążeń (Cloud Workload Protection):**
    - **VM** – wykrywanie zagrożeń, analiza zachowań, agentless scanning.
    - **Bazy danych** – ochrona SQL/PostgreSQL/MySQL, wykrywanie anomalii.
    - **Storage** – skanowanie pod kątem malware, identyfikacja niepoprawnych konfiguracji.
  - **Defender for Containers** – analiza obrazów, runtime protection, integracja z AKS.
  - **Defender for APIs** – wykrywanie ryzyk i nadużyć API.

<img src="assets/keyvault.svg">

- **Key Vault**  
  Bezpieczne przechowywanie i zarządzanie:
  - **Secrets** – hasła, tokeny, connection stringi.
  - **Keys** – klucze kryptograficzne do szyfrowania i podpisywania.
  - **Certificates** – pełny cykl życia certyfikatów, automatyczne odnowienia.
  - **HSM (Managed HSM)** – sprzętowe moduły kryptograficzne FIPS 140-2 Level 3.

- **DDoS Protection**  
  Ochrona przed atakami DDoS:
  - **Basic** – automatyczna ochrona dla wszystkich usług publicznych Azure.
  - **Standard** – zaawansowana ochrona (L3/L4), adaptacyjne profile ruchu, automatyczne łagodzenie, alerty, raporty, **DDoS Rapid Response (DRR)** oraz SLA finansowe.

- **WAF (Web Application Firewall)**  
  Ochrona aplikacji webowych przed atakami (OWASP Top 10):
  - **Application Gateway WAF** – filtrowanie L7, Managed/Custom Rules, bot protection, TLS termination.
  - **Azure Front Door WAF** – globalna ochrona na edge, rate limiting, szybkie reagowanie.
  - Wspólne polityki, integracja z Log Analytics i Sentinel.

---

<a id="sec-08-governance-compliance"></a>
## 8. Governance & Compliance

Governance w Azure to **zestaw zasad i narzędzi**, które pomagają utrzymać porządek, bezpieczeństwo i kontrolę nad zasobami w chmurze.

### Azure Policy

<img src="assets/azurepolicy.svg">

Mechanizm wymuszający zgodność konfiguracji zasobów z wymaganiami organizacji.

- **Policy Definition** – pojedyncza reguła (np. „wymagaj tagu Environment”, „blokuj publiczne IP”).
- **Policy Initiative** – zestaw wielu reguł grupowanych w standardy (np. CIS, NIST, ISO, Zero Trust).

#### Effects (działania polityk)
- **Deny** – blokuje wdrożenie **(Owner również nie może ominąć Deny)**.
- **Audit** – oznacza zasób jako niezgodny, ale nie blokuje wdrożenia.
- **Append** – dodaje brakujące właściwości (np. tagi).
- **Modify** – automatycznie poprawia konfigurację podczas wdrażania.
- **DeployIfNotExists** – tworzy brakujące zasoby towarzyszące (np. diag settings).

#### Exemptions (wyjątki)
Tymczasowe wyłączenie polityki dla wybranego scope — bez jej usuwania.  
Stosowane w okresach migracji, wyjątkach biznesowych lub scenariuszach legacy.

#### Remediation Tasks
Automatyczne zadania naprawcze działające na istniejących zasobach:
- poprawiają konfigurację (Modify),
- doinstalowują wymagane elementy (DeployIfNotExists).

#### Scope i dziedziczenie
Management Group → Subscription → Resource Group → Resource  
(polityki dziedziczą się **wyłącznie w dół**)

---

### Resource Locks

<img src="assets/resourceLocks.svg">

Zabezpieczają zasoby przed przypadkowym usunięciem lub edycją.

- **Delete** – blokada usunięcia zasobu.
- **ReadOnly** – blokada usunięcia i modyfikacji (tylko odczyt).

Stosowane szczególnie dla:
- Key Vault,  
- Storage (backupy),  
- VNet / Firewall,  
- Log Analytics Workspace.

---

### Tags

<img src="assets/tags.svg">

To specjalne **metadane** przypisywane do zasobów w celu:
- mają klucz i wartość
- **maksymalnie 50 tagów** na resource
- organizacji i klasyfikacji (Environment, Owner, Project),
- chargeback/showback (koszty per projekt/dział),
- automatyzacji (np. auto‑shutdown),
- wymuszania governance (Azure Policy może wymagać tagów).

Ważne!
- Management Groups **nie mogą mieć** tagów.

---

### Microsoft Purview

**Microsoft Purview** wspiera governance danych i compliance:
- katalogowanie i odkrywanie źródeł danych,
- klasyfikację danych (wrażliwe/PII),
- data lineage (śledzenie przepływu danych),
- polityki i kontrolę dostępu do danych.

Na AZ‑900: Purview to narzędzie do **zarządzania ładem danych**, nie zamiennik dla Azure Policy.

---

### Azure Blueprints

Blueprint pozwala w jednym, powtarzalnym procesie stworzyć całe środowisko Azure zgodne ze standardami organizacji — od zasad i uprawnień po strukturę zasobów.

<img src="assets/blueprints.svg">

Pakiet governance łączący:
- Azure Policy,  
- RBAC,  
- ARM/Bicep templates,  
- strukturę Resource Groups,

w jedno spójne, powtarzalne środowisko (np. Landing Zone).

Idealne dla enterprise i audytowalnych wdrożeń.

Uwaga!
- Obecnie Blueprints są przestarzałe, a zalecanym podejściem jest **Azure Landing Zones** + Bicep/ARM + Policy + GitOps.

**Azure Landing Zones (ALZ)** to kompletna, gotowa architektura i zestaw najlepszych praktyk, które pozwalają szybko i poprawnie zbudować bezpieczne, skalowalne środowisko chmurowe zgodne z CAF.

---

### Azure Arc

**Azure Arc** to usługa, która pozwala zarządzać serwerami, Kubernetesem i usługami działającymi poza Azure tak, jakby były natywnymi zasobami Azure.

<img src="assets/azurearc.svg">

Rozszerza Azure na środowiska:
- on‑premise,  
- multi‑cloud,  
- edge.

Umożliwia:
- nakładanie Azure Policy poza Azure,  
- zarządzanie serwerami, Kubernetes, SQL spoza Azure,  
- zbieranie logów/telemetrii z różnych środowisk,  
- utrzymanie spójnego governance.

---

### Shared Responsibility Model

<img src="assets/sharedresponsibility.svg">

Podział odpowiedzialności między Azure a klientem.

#### Azure odpowiada za:
- fizyczną infrastrukturę (DC, sieć, hypervisor),
- platformę PaaS oraz jej aktualizacje,
- dostępność usług i SLA,
- bezpieczeństwo warstw poniżej OS (w modelu PaaS).

#### Klient odpowiada za:
- tożsamość i dostęp (MFA, CA, RBAC, PIM),
- dane (szyfrowanie, backup, klasyfikacja),
- konfiguracje usług (NSG, firewall, KV access),
- bezpieczeństwo aplikacji,
- rotację kluczy, SAS, tokenów,
- aktualizacje OS w VM (IaaS).

---

### Service Trust, Compliance i Rezydencja Danych

**Service Trust Portal** służy do szybkiego sprawdzania, jak Microsoft spełnia wymagania bezpieczeństwa i zgodności, co ułatwia audyty i ocenę ryzyka.

<img src="assets/servicetrust.svg">

- **Microsoft Service Trust Portal** – raporty zgodności (ISO, SOC, PCI), audyty, certyfikaty.
- **Data Residency (rezydencja danych)** – dane pozostają w wybranej geografii (np. EU).
- **Azure Government / Azure China** – suwerenne regiony dla wymagań regulacyjnych.

---

### Well‑Architected Framework (WAF – 5 filarów)

<img src="assets/wellarchitected.svg">

1. **Cost Optimization** – eliminacja marnotrawstwa, right‑size, automatyzacja kosztów.  
2. **Operational Excellence** – automatyzacja, CI/CD, versioning, monitoring.  
3. **Performance Efficiency** – wybór właściwych usług, autoscaling, cache.  
4. **Reliability** – HA, DR, self‑healing, multi‑zone/multi‑region.  
5. **Security** – Zero Trust, least privilege, szyfrowanie, segmentacja.

---

### Cloud Adoption Framework & Landing Zone

**CAF** obejmuje:
- strategię i uzasadnienie biznesowe,  
- plan migracji,  
- przygotowanie organizacji,  
- governance (Policy, RBAC, MG),  
- operacje (monitoring, DevOps).

**Landing Zone** = gotowy wzorzec środowiska produkcyjnego:
- hierarchia Management Groups,
- polityki bezpieczeństwa,
- sieć hub/spoke,
- RBAC baseline,
- logowanie i monitoring,
- zgodność z CAF i WAF.

---

### Support Plans (wsparcie techniczne)

<img src="assets/supportplans.svg">

Na AZ‑900 warto znać różnicę między planami wsparcia:

- **Basic** – podstawowe wsparcie dla billing/subscription i dokumentacja; brak wsparcia technicznego 1:1 dla problemów produkcyjnych.
- **Developer** – najniższy płatny plan techniczny, dobry do środowisk dev/test.
- **Standard** – szybsze czasy reakcji i wsparcie dla workloadów produkcyjnych.
- **Professional Direct** – najwyższy poziom komercyjnego wsparcia Azure (advisory + szybsza obsługa krytycznych zgłoszeń).

W dużych organizacjach spotkasz też model **Unified Support** (enterprise), ale na poziomie AZ‑900 zwykle wystarczy zrozumienie powyższych poziomów.

---

### Service Lifecycle (Preview vs GA)

- **Public Preview** – funkcja testowa, zwykle bez pełnego SLA i z możliwymi zmianami API/feature.
- **General Availability (GA)** – usługa produkcyjna, stabilna i objęta standardowymi zasadami SLA.
- **Retirement / Deprecation** – usługa lub funkcja wycofywana; Microsoft publikuje harmonogram migracji.

Egzaminowo: **preview ≠ produkcja krytyczna**; do workloadów produkcyjnych preferuj funkcje **GA**.

---

<a id="sec-09-monitoring-logging"></a>
## 9. Monitoring & Logging

<img src="assets/azuremonitor.svg">

- **Azure Monitor**
  Centralna platforma monitoringu w Azure, która zbiera, przechowuje, analizuje i wizualizuje dane operacyjne z zasobów.
  
  - **Metrics (metryki czasowe)**
    Dane numeryczne zbierane w krótkich odstępach czasu (np. CPU %, pamięć, IOPS, opóźnienia).
    - przeznaczone do szybkiej analizy trendów,
    - idealne do alertów w czasie zbliżonym do rzeczywistego,
    - przechowywane w wysokiej rozdzielczości (1-minutowe interwały).

  - **Logs (KQL / Log Analytics)**
    Dane o wysokiej szczegółowości zapisywane w Log Analytics Workspace.
    - logi strukturalne i niestandardowe,
    - analiza poprzez **Kusto Query Language (KQL)**,
    - możliwość łączenia danych z wielu usług,
    - fundament dla Security, Monitoring, Governance.

  - **Alerts**
    Mechanizm powiadamiania o problemach, oparty na:
    - metrykach (np. CPU > 80% przez 5 min),
    - zapytaniach KQL (np. błędy 500 w ostatnich 10 minutach),
    - logach aktywności, logach zasobów,
    - integracji z Action Groups (e-mail, Teams, webhook, ITSM).

- **Azure Advisor**

**Azure Advisor** to wbudowany doradca dla platformy Azure, analizujący środowisko pod kątem najlepszych praktyk Microsoft i generujący konkretne rekomendacje dotyczące optymalizacji kosztów, wydajności, bezpieczeństwa, niezawodności i operacyjności.

<img src="assets/azureadvisor.svg">

### Kategorie rekomendacji
- **Cost** – redukcja kosztów (right-size, wyłączanie nieużywanych zasobów, Reserved Instances, Azure Hybrid Benefit).  
- **Security** – rekomendacje z Defender for Cloud zwiększające poziom zabezpieczeń.  
- **Reliability** – wskazówki dotyczące wysokiej dostępności, redundancji, odporności i usunięcia SPOF.  
- **Operational Excellence** – automatyzacja, kopie zapasowe, poprawa procesów operacyjnych.  
- **Performance** – optymalizacja wydajności (autoscale, właściwe SKU, zwiększanie przepustowości).

### Przykładowe rekomendacje
- Zmniejszenie przewymiarowanych VM lub wyłączanie ich poza godzinami pracy.  
- Dodanie redundancji (np. wdrożenia w wielu strefach AZ).  
- Migracja storage do wydajniejszych SKU (np. Premium SSD).  
- Dodanie tagów wymaganych przez governance.  

Azure Advisor jest narzędziem **proaktywnym**, często wskazywanym na egzaminie AZ‑900 jako źródło rekomendacji dotyczących kosztów, bezpieczeństwa, wydajności oraz utrzymania środowiska.

- **Service Health**
  Informacje o stanie platformy Azure dotyczące:
  - trwających incydentów,
  - planowanych prac konserwacyjnych,
  - degradacji usług w wybranych regionach.
  Można konfigurować alerty na awarie wpływające na konkretne zasoby.

- **Resource Health**
  Stan konkretnego zasobu (np. pojedynczej VM, SQL DB, App Service):
  - czy problem wynika z platformy Azure,
  - czy z konfiguracji po stronie klienta,
  - historia zmian stanu i rekomendacje działań.

**Service Health vs Resource Health (skrót egzaminowy):**

| Narzędzie | Zakres | Typowe pytanie egzaminacyjne |
|---|---|---|
| Service Health | usługi/regiony/subskrypcja | „Czy w regionie jest incydent Azure?” |
| Resource Health | pojedynczy zasób | „Dlaczego ta konkretna VM/app nie działa?” |

- **Activity Log**
  Log operacyjny na poziomie subskrypcji, zawierający:
  - **kto** wykonał operację,
  - **co** zmieniono,
  - **kiedy** i **z jakiego źródła**,
  - dotyczy głównie operacji ARM (tworzenie/usuwanie/aktualizacje zasobów).
  Jest podstawą audytów i analizy zmian.

- **Resource Logs**
  Szczegółowe logi działania konkretnych usług (np. Storage Access Logs, Key Vault Audit Logs).
  - wymagają włączenia **Diagnostic Settings**,
  - można je wysłać do:
    - Log Analytics Workspace,
    - Event Hub,
    - Storage Account,
  - pozwalają analizować zachowanie aplikacji, ruch, błędy, opóźnienia, dostęp.

- **Workbooks**
  Zaawansowane interaktywne dashboardy:
  - łączą wizualizacje, tekst, parametry, zapytania KQL,
  - nadają się do tworzenia raportów operacyjnych, security, SRE/DevOps,
  - w pełni konfigurowalne (heatmapy, wykresy, tabele, integracja z Azure Monitor).

---

<a id="sec-10-costs-billing"></a>
## 10. Costs & Billing (Koszty i rozliczenia)

<img src="assets/costsbilling.svg">

**Za co płacisz w Azure:**
- **Compute (czas działania)**  
  Opłaty za czas pracy zasobów obliczeniowych (VM, App Service, AKS node pools, Functions).  
  Rozliczanie zwykle „pay‑as‑you‑go” — za sekundę lub minutę działania.

- **Storage (GB / miesiąc)**  
  Koszt zależny od ilości przechowywanych danych oraz liczby operacji.  
  Uwzględnia typ przestrzeni dyskowej (Hot / Cool / Archive), replikację (LRS/ZRS/GRS) oraz transakcje.

- **Egress sieci (wyjście z Azure)**  
  Dane *opuszczające* Azure — np. wysyłane do Internetu, do lokalnego datacenter lub między regionami.  
  To jedno z najczęstszych „ukrytych” źródeł kosztów, bo transfer wychodzący jest **zawsze płatny**.

- **Ingress sieci (wejście do Azure)**  
  Dane *wchodzące* do Azure — np. upload plików, dane z on‑prem, ruch przychodzący z Internetu.  
  W modelu transferu sieciowego Azure **ingress jest zazwyczaj bezpłatny**.  
  W praktyce: za pobieranie z Azure płacisz, za wysyłanie do Azure — nie.

- **Licencje**  
  Płatne modele licencji dla Windows Server, SQL Server, RedHat, SUSE, a także usług takich jak Defender.  
  Rozliczane per rdzeń, instancję lub użytkownika.

---

### **Optymalizacja kosztów:**

- **Reservations (1 lub 3 lata)**  
  Rezerwacje na VM, SQL, Cosmos DB i inne usługi.  
  Oszczędność 40–70% — idealne dla stabilnych workloadów produkcyjnych.

- **Spot VMs**  
  Tanie VM działające, gdy dostępna jest wolna moc obliczeniowa.  
  Mogą zostać przerwane — dobre do batch jobs, CI/CD, symulacji, renderingu.

- **Azure Hybrid Benefit (AHB)**  
  Możliwość wykorzystania posiadanych licencji Windows/SQL z Software Assurance.  
  Obniża koszt VM i SQL PaaS nawet o 30–50%.

---

### **Narzędzia do kosztów i analizy:**

- **Pricing Calculator**  
  Kalkulator kosztów — pozwala wyliczyć przewidywane koszty architektury przed wdrożeniem.

- **TCO (Total Cost of Ownership) Calculator**  
  Porównanie kosztów on‑premise vs Azure (sprzęt, energia, zarządzanie, amortyzacja).  
  Pomocny przy planowaniu migracji i budowaniu business case.

- **Cost Management**  
  Wbudowane narzędzie do analizy i kontroli wydatków:
  - budżety i alerty,
  - analiza kosztów po tagach, subskrypcjach i zasobach,
  - prognozy wydatków,
  - rekomendacje optymalizacyjne (np. niewykorzystywane IP/VM/dyski).

---

<a id="sec-11-iac-automation"></a>
## 11. IaC & Automation (Infrastruktura jako kod)

**IaC (Infrastructure as Code)** to podejście, w którym całą infrastrukturę — serwery, sieci, bazy, konfiguracje — definiuje się i zarządza nią za pomocą plików kodu, zamiast ręcznych kliknięć w portal, co zapewnia automatyzację, powtarzalność i pełną kontrolę wersji.

<img src="assets/iac.svg">

- **ARM Templates (JSON) / Bicep**
  Deklaratywne podejście do definiowania infrastruktury:
  - **Deklaratywność** – opisujesz *jaki* ma być stan zasobów, a Azure zajmuje się wdrożeniem.
  - **Idempotencja** – wielokrotne uruchamianie tych samych szablonów daje ten sam efekt; brak ryzyka duplikacji.
  - **GitOps** – idealne do łączenia z CI/CD; kod infrastruktury w repozytorium pozwala na kontrolę wersji, PR review i automatyczne wdrażanie.
  - **ARM (JSON)** – niskopoziomowe, bardziej szczegółowe, natywne dla Azure.
  - **Bicep** – nowszy, prostszy i czytelniejszy język, który kompiluje się do ARM; wspiera moduły, lepszą walidację i łatwiejsze utrzymanie.

  Uwaga!
  - **ARM templates** definiują co wdrożyć — konkretne zasoby i ich konfigurację.
  - **Blueprints** definiują całe środowisko — mogą zawierać ARM templates plus polityki, RBAC i strukturę zasobów, czyli pełny pakiet governance.

- **Terraform**
  Popularne i uniwersalne narzędzie IaC działające w modelu multi‑cloud:
  - **Provider AzureRM** – umożliwia pełne zarządzanie zasobami Azure.
  - **Stan (state)** – Terraform śledzi zmiany infrastruktury i porównuje stan bieżący z planowanym.
  - **Plan → Apply** – weryfikacja zmian przed wdrożeniem (co zostanie utworzone, zmodyfikowane lub usunięte).
  - **Multi‑cloud** – jedna składnia dla Azure, AWS, GCP, VMware, K8s.
  - Świetny wybór dla organizacji działających w wielu środowiskach lub chcących odseparować IaC od konkretnego dostawcy.

- **DSC (Desired State Configuration)**
  Mechanizm konfiguracji hostów Windows (i częściowo Linux):
  - Definiujesz docelowy stan systemu (np. zainstalowane role, pliki, serwisy).
  - DSC zapewnia **wymuszanie i utrzymanie** zadeklarowanego stanu.
  - Używany głównie do konfiguracji serwerów, VM, środowisk hybrydowych.
  - Integruje się z Azure Automation State Configuration.

- **Automation**
  Zestaw narzędzi do automatyzacji operacji i zarządzania:
  - **Runbooki** – skrypty PowerShell i Python wykonywane w chmurze; idealne do automatyzacji operacji, cleanupów, rotacji kluczy.
  - **Update Management** – automatyczne instalowanie poprawek dla VM (Windows i Linux) bez potrzeby własnej infrastruktury patchowania.
  - **Change Tracking & Inventory** – monitorowanie zmian plików, rejestru, usług, pakietów oraz zbieranie szczegółowej inwentaryzacji serwerów.
  - Świetne do operacji typu: harmonogramy, cykliczne zadania, utrzymanie zgodności środowisk.

---

<a id="sec-12-extended-services"></a>
## 12. Extended Azure Services (Usługi rozszerzone)

### Integracja i zdarzenia

- **Azure Service Bus**  
  Broker komunikatów klasy enterprise — **niezawodne kolejki** (1:1) oraz publish/subscribe (1→n), z obsługą transakcji, DLQ i protokołu AMQP. Idealny do integracji mikroserwisów oraz scenariuszy wymagających trwałego kolejkowania.

  <img src="assets/servicebus.svg">

- **Azure Event Hub**  
  Platforma do zbierania i przetwarzania **strumieni danych** na ogromną skalę (telemetria, logi, IoT), nawet miliony zdarzeń na sekundę. Świetnie współpracuje z Databricks, Spark, Stream Analytics i może działać jako Kafka‑as‑a‑Service.

  <img src="assets/eventhub.svg">

- **Azure Event Grid**  
  Lekki **router zdarzeń** typu push, idealny do budowania architektur event‑driven o niskich opóźnieniach. Umożliwia reagowanie na zdarzenia z usług Azure (np. BlobCreated → Function) i integrację systemów poprzez eventy.

  <img src="assets/eventgrids.svg">

---

### API i edge

<img src="assets/apiedge.svg">

**API Management (APIM)**
APIM to centralny *API Gateway*, który pozwala w jednym miejscu wystawiać, zabezpieczać i zarządzać wszystkimi API w organizacji.

Zapewnia m.in.:
- **transformacje request/response** (np. JSON ↔ XML, dodawanie nagłówków),
- **bezpieczeństwo i autoryzację** (OAuth2, OpenID Connect, JWT, certyfikaty),
- **rate‑limiting i throttling** – ochrona backendów przed przeciążeniem,
- **polityki (Policies)** działające bez zmian w kodzie aplikacji,
- **Developer Portal** – dokumentacja, testowanie API i zarządzanie kluczami,
- **wersjonowanie i lifecycle management** API.

Idealny jako warstwa pośrednia między aplikacjami klienckimi a backendami.

---

#### **Front Door / CDN / Traffic Manager**

**Azure Front Door**
Globalny gateway warstwy L7 działający na edge Microsoftu.

Oferuje:
- inteligentny routing HTTP/HTTPS (latency‑based, path‑based, failover),
- **WAF** na edge,
- **globalne load balancing**,
- **caching** i akcelerację aplikacji,
- **TLS termination** blisko użytkownika.

Świetny dla aplikacji działających globalnie, wymagających wysokiej wydajności i ochrony.

**Azure CDN**
Content Delivery Network przeznaczony do:

- szybkiego dostarczania statycznych treści (CSS, JS, obrazy, wideo),
- redukcji obciążenia backendu poprzez cache na edge,
- poprawy czasu ładowania aplikacji.

Działa świetnie samodzielnie lub jako uzupełnienie Front Door.

**Azure Traffic Manager**
Globalny load balancer oparty o **DNS** (nie przetwarza sam ruchu HTTP).  

Obsługuje:
- routing geograficzny (np. kierowanie użytkowników do najbliższego regionu),
- **failover** między regionami,
- **weighted routing** (kontrolowany podział ruchu),
- scenariusze blue/green i A/B.

Używany, gdy potrzebne jest sterowanie ruchem na poziomie DNS i niezależnie od protokołu.

---

### Azure Marketplace

**Azure Marketplace** to katalog gotowych rozwiązań (obrazy VM, aplikacje, appliance’y, SaaS), które można szybko wdrożyć w swojej subskrypcji.

Kiedy używać:
- gdy potrzebujesz gotowego rozwiązania produkcyjnego bez budowania od zera,
- gdy chcesz przyspieszyć PoC lub migrację,
- gdy potrzebujesz komercyjnego wsparcia dostawcy rozwiązania.

Na egzaminie: Marketplace to przede wszystkim **szybkie wdrażanie gotowych ofert partnerów** + rozliczenie przez Azure.

---

### AI i Data

<img src="assets/aiservices.svg">

**Cognitive Services / Azure OpenAI / Azure ML**

- **Cognitive Services**  
  Gotowe modele AI bez trenowania: OCR, analiza obrazów, rozpoznawanie mowy, tłumaczenia, sentiment, wykrywanie anomalii.

- **Azure OpenAI**  
  Modele generatywne (GPT, embeddings): generowanie tekstu, podsumowania, chatboty, wyszukiwanie semantyczne, automatyzacja procesów.

- **Azure Machine Learning (Azure ML)**  
  Platforma MLOps: trening modeli, eksperymenty, rejestr modeli, pipeline’y, wdrożenia do endpointów, monitorowanie jakości i driftu.

---

<img src="assets/dataplatform.svg">

**Data Factory / Synapse Pipelines / Databricks**

- **Azure Data Factory (ADF)**  
  Narzędzie do budowania i uruchamiania przepływów ETL/ELT między różnymi źródłami danych.

- **Synapse Pipelines**  
  Pipeline’y działające w ekosystemie Synapse, idealne do orkiestracji danych w Synapse i Data Lake.

- **Databricks**  
  Platforma oparta na Spark do skalowalnego przetwarzania danych, analityki i uczenia maszynowego.

---

### IoT (Internet of Things)

**IoT (Internet of Things)** to koncepcja, w której fizyczne urządzenia łączą się z internetem, wymieniają dane i pozwalają zdalnie monitorować oraz sterować realnym światem w sposób automatyczny i inteligentny.

<img src="assets/iotschema.svg">

**IoT Hub**
Zarządzany hub komunikacyjny IoT obsługujący MQTT/AMQP/HTTPS, z dwukierunkową komunikacją między urządzeniami a chmurą.  
Zapewnia telemetrię, komendy, konfigurację oraz **Device Twin** do przechowywania stanu i metadanych urządzeń.

**IoT Central**
Platforma IoT typu SaaS, która eliminuje potrzebę tworzenia backendu.  
Oferuje gotowe dashboardy, analitykę, alerty, zarządzanie urządzeniami i szybkie wdrożenia IoT bez programowania infrastruktury.

**Azure Digital Twins**
Silnik do modelowania cyfrowych reprezentacji obiektów i ich relacji (digital twins).  
Umożliwia odwzorowanie budynków, fabryk, urządzeń i procesów, analizę stanów, symulacje i integrację z telemetrią IoT Hub.

**Azure Sphere**
Kompletny i bezpieczny ekosystem IoT: specjalne MCU, zabezpieczony Linux OS oraz usługa bezpieczeństwa zapewniająca aktualizacje, certyfikację i ochronę urządzeń.  
Idealny dla urządzeń wymagających najwyższego poziomu bezpieczeństwa.

---

### Inne przydatne

**Azure Bastion**
Bezpieczny dostęp RDP/SSH do VM bez publicznych IP – działa przez portal i eliminuje potrzebę otwierania portów.

**Azure Load Testing**
Testy obciążeniowe w oparciu o JMeter, analiza wydajności i integracja z CI/CD bez utrzymywania własnej infrastruktury.

**Azure Cache for Redis**
Szybki in‑memory distributed cache do sesji, kolejek, pub/sub, ograniczania ruchu na bazach i niskich opóźnień.

**App Configuration / Feature Flags**
Centralna konfiguracja i kontrolowane wdrażanie funkcji (feature flags), z wersjonowaniem i pełną integracją z aplikacjami.

**AKS Fleet**
Zarządzanie wieloma klastrami AKS: governance, rollouty, multi‑cluster services, idealne dla środowisk enterprise.

**Container Registry (ACR)**
Prywatny rejestr OCI/Docker z buildami (ACR Tasks), skanowaniem podatności, replikacją geo i integracją z AKS.

**Azure Lighthouse**
Delegowane zarządzanie subskrypcjami i tenantami – doskonałe dla MSP, DevOps i scenariuszy multi‑tenant.

**Azure Virtual Desktop**
Wirtualne desktopy i aplikacje Windows w chmurze: multisession, FSLogix, autoscaling.

---

### Dodatkowe przydatne usługi

**Azure Private Link**
Prywatny dostęp do usług Azure (SQL, Storage, Key Vault) w ramach sieci bez ruchu publicznego.

**Managed HSM**
Sprzętowe moduły bezpieczeństwa (HSM) do generowania i przechowywania kluczy zgodnie z FIPS 140‑2.

**Azure Monitor / Application Insights**
Monitoring metryk, logów, zależności i wydajności aplikacji – podstawa obserwowalności.

**Logic Apps Standard**
Serverless workflowy i automatyzacje z setkami konektorów (SAP, SQL, Salesforce, EDI, B2B).

---

<a id="sec-13-pytania-az900"></a>
## 13. Podchwytliwe pytania AZ‑900
| Pytanie | Odpowiedź |
|---|---|
| Czy subskrypcja ma „swój” koszt? | **Nie.** Płacisz za zasoby; subskrypcja to granica rozliczeń i uprawnień. |
| Czy Region = Availability Zone? | **Nie.** Region to zestaw DC; AZ to separowane DC w regionie. |
| Czy Resource Group musi być w tym samym regionie co zasoby? | **Nie.** Lokalizacja RG to metadane – zasoby mogą być w innych regionach (z wyjątkami usług). |
| Czy RBAC działa „w górę” hierarchii? | **Nie.** Uprawnienia dziedziczą **w dół** (MG → Sub → RG → Resource). |
| Czy Bastion wymaga publicznego IP VM? | **Nie.** Działa przez portal bez publicznego IP. |
| Czy Azure Policy zawsze blokuje wdrożenie? | **Nie.** Zależy od **Effect** (np. Audit nie blokuje). |
| Czy Availability Set = Availability Zone? | **Nie.** Set to logiczne FD/UD; AZ to fizyczna separacja DC. |
| Czy outbound (egress) ruch sieci jest darmowy? | **Nie.** Egress jest rozliczany. |

---

<a id="sec-14-glosariusz"></a>
## 14. Glosariusz skrócony

| Pojęcie | Definicja |
|--------|-----------|
| Tenant (Entra ID) | Instancja tożsamości dla organizacji. |
| Directory | Katalog użytkowników, grup i aplikacji. |
| Subscription | Granica rozliczeń, limitów i uprawnień. |
| Management Group | Hierarchiczne grupowanie subskrypcji. |
| Resource Group | Kontener logiczny dla zasobów. |
| Resource | Pojedynczy zasób Azure (VM, VNet, Storage itd.). |
| Tag | Metadana opisująca zasób. |
| Resource Lock | Blokada Delete/ReadOnly chroniąca zasoby. |
| Scope | Poziom przypisywania RBAC/polityk (MG→Sub→RG→Resource). |
| Region | Fizyczna lokalizacja centrum danych. |
| Availability Zone | Strefa niezależna w regionie. |
| Zone Redundant Services | Usługi działające w wielu AZ. |
| Geography | Zestaw regionów w jednej jurysdykcji. |
| Region Pair | Sparowane regiony DR. |
| Edge Location | Punkt POP dla CDN/Front Door. |
| SLA | Gwarancja dostępności. |
| HA (High Availability) | Wysoka dostępność rozwiązania. |
| DR (Disaster Recovery) | Odtwarzanie po awarii. |
| Entra ID (Azure AD) | Usługa tożsamości i uwierzytelniania. |
| User | Użytkownik w katalogu. |
| Group | Grupa uprawnień. |
| Role / RBAC | Role definiujące dostęp do zasobów. |
| Service Principal | Tożsamość aplikacji. |
| Managed Identity | Systemowa tożsamość bez kluczy. |
| Conditional Access | Dostęp zależny od warunków (lokacja, urządzenie). |
| PIM | Zarządzanie rolami uprzywilejowanymi. |
| App Registration | Rejestracja aplikacji w Entra ID. |
| VNet | Prywatna sieć w Azure. |
| Subnet | Segmentacja sieci. |
| NSG | Reguły sieciowe L3–L4. |
| ASG | Grupy aplikacyjne VM. |
| UDR (Route Table) | Niestandardowy routing. |
| VNet Peering | Połączenie prywatne między VNetami. |
| Private Endpoint | Prywatny endpoint do usług PaaS. |
| Private Link | Prywatny dostęp do usług Azure. |
| Azure Firewall | Firewall warstw L3–L7. |
| WAF | Firewall aplikacyjny L7. |
| Load Balancer | Load balancing L4. |
| Application Gateway | Load balancing L7 + WAF. |
| Front Door | Globalny routing HTTP(S). |
| Traffic Manager | DNS load balancing. |
| VPN Gateway | Tunel IPsec Azure ↔ on‑prem. |
| ExpressRoute | Prywatna łączność MPLS z Azure. |
| Bastion | Bezpieczny RDP/SSH bez publicznych IP. |
| VM (Virtual Machine) | Maszyna wirtualna. |
| VMSS | Skalowanie grup VM. |
| Availability Set | Separacja VM w domenach awarii. |
| App Service | Hosting aplikacji Web/API. |
| Function App | Serverless oparty o trigerry. |
| Container Apps | Zarządzane kontenery serverless. |
| AKS | Zarządzany Kubernetes. |
| ACR | Prywatny rejestr obrazów OCI. |
| Logic Apps | Workflow integracyjny. |
| Batch | Przetwarzanie wsadowe/HPC. |
| AVD | Wirtualne desktopy Windows. |
| Dedicated Host | Fizyczny host tylko dla jednego klienta. |
| Storage Account | Konto usług magazynowych. |
| Blob Storage | Obiektowy magazyn danych. |
| Azure Files | Udostępnione SMB/NFS. |
| Queue Storage | Proste kolejki komunikatów. |
| Table Storage | NoSQL key‑value. |
| Disk Storage | Dyski VM. |
| Azure SQL Database | Zarządzana baza SQL. |
| SQL Managed Instance | Prawie pełna instancja SQL w PaaS. |
| Cosmos DB | Globalna baza NoSQL z niskimi opóźnieniami. |
| PostgreSQL Flexible Server | Zarządzany PostgreSQL. |
| MySQL Flexible Server | Zarządzany MySQL. |
| MariaDB | PaaS MariaDB. |
| Synapse Analytics | Lakehouse + SQL + Spark. |
| Databricks | Spark + Delta Lake + MLflow. |
| Redis Cache | In-memory distributed cache. |
| Service Bus | Messaging enterprise (kolejki/topics). |
| Event Hub | Streaming telemetrii i big‑data. |
| Event Grid | Routing zdarzeń serverless. |
| Storage Queue | Kolejki w Storage. |
| API Management | API Gateway. |
| App Configuration | Centralna konfiguracja. |
| Feature Flags | Przełączniki funkcji w aplikacji. |
| Azure Monitor | Centralny monitoring zasobów. |
| Metrics | Metryki czasowe. |
| Logs | Logi w KQL. |
| Activity Log | Operacje zarządcze. |
| Diagnostic Settings | Kierowanie logów/metryk. |
| Application Insights | Telemetria i APM. |
| Alerts | Alerty metryczne/logowe. |
| Workbooks | Dashboardy i wizualizacje. |
| Change Tracking | Zmiany konfiguracji VM. |
| Update Management | Aktualizacje systemów. |
| Azure Policy | Wymuszanie konfiguracji zasobów. |
| Policy Definition | Pojedyncza reguła. |
| Initiative | Zestaw reguł. |
| Blueprints | Szablony governance. |
| Defender for Cloud | Bezpieczeństwo i Secure Score. |
| Key Vault | Sekrety, klucze i certyfikaty. |
| Managed HSM | Sprzętowy moduł kryptograficzny. |
| DDoS Protection | Ochrona przed atakami wolumetrycznymi. |
| IAM | Identity & Access Management. |
| Zero Trust | Model bezpieczeństwa „never trust, always verify”. |
| Azure DevOps | Repos, Boards, Pipelines, Artifacts. |
| GitHub Actions | CI/CD jako usługa. |
| Pipelines | Procesy budowania i wdrażania. |
| Artifacts | Rejestr paczek. |
| ARM Templates | Deklaratywne szablony JSON. |
| Bicep | DSL IaC dla ARM. |
| Terraform | Multi-cloud IaC. |
| GitOps | Model declarative + repository-driven. |
| Runbook | Automatyczne skrypty. |
| DSC | Wymuszanie stanu konfiguracji. |
| Azure Machine Learning | MLOps i trening modeli. |
| Azure OpenAI | Modele generatywne GPT. |
| Cognitive Services | Gotowe modele AI (Vision/Speech/Language). |
| Data Factory | ETL/ELT i integracja danych. |
| Synapse Pipelines | Orkiestracja danych w Synapse. |
| Databricks | Spark + ML + Lakehouse. |
| Power BI Embedded | Wizualizacja danych w aplikacjach. |
| IoT Hub | Dwukierunkowa komunikacja IoT. |
| Device Twin | Stan i właściwości urządzeń. |
| IoT Edge | Moduły kontenerowe na urządzeniach. |
| IoT Central | Platforma IoT SaaS. |
| Device Provisioning Service | Automatyczna rejestracja urządzeń IoT. |
| Azure Digital Twins | Modele i relacje obiektów fizycznych. |
| Azure Sphere | Bezpieczny MCU i OS IoT. |
| Cost Management | Analiza kosztów i budżety. |
| Reservations | Rezerwacje na 1/3 lata. |
| Azure Hybrid Benefit | Wykorzystanie własnych licencji. |
| Spot VM | Tanie, przerywalne VM. |
| Egress | Ruch wychodzący z chmury (płatny). |
| Ingress | Ruch przychodzący (bezpłatny). |
| TCO Calculator | Porównanie kosztów on‑prem vs Azure. |
| Soft Delete | Odzyskiwanie skasowanych zasobów. |
| Immutable Storage | Nie­zmienialne dane (compliance). |
| Hot/Cool/Archive | Warstwy przechowywania danych. |
| Autoscale | Automatyczne skalowanie zasobów. |
| Replica | Kopia danych. |
| Shard | Partycja danych. |
| SAS Token | Delegowany dostęp do Storage. |
| CORS | Polityki cross-origin. |
| CDN | Sieć dostarczania treści. |

---

<a id="sec-15-free-account"></a>
## 15. Azure Free Account (Free Tier)

- **US$200 kredytu** na pierwszy miesiąc (trial).
- Po wyczerpaniu – świadomie przejdź na PAYG lub wyłącz zasoby.
- Wybrane usługi mają **12‑miesięczny Free Tier** (limity darmowe).

---

<a id="sec-16-sla"></a>
## 16. SLA (Service Level Agreement)

<img src="assets/sla.svg">

- Każda usługa ma własne SLA (np. 99,9 / 99,95 / 99,99%).
- Złożone SLA ≈ iloczyn SLA składników (przy niezależności awarii).
- Przykład: 99,95% × 99,9% ≈ 99,85%.
- Rozmieszczenie w wielu **AZ (Availability Zones)** zwykle podnosi SLA (np. VM).

---

<a id="sec-17-databases"></a>
## 17. Bazy danych (Databases)

<img src="assets/databases.svg">

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

<img src="assets/cosmosdb_architecture.svg">

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

<img src="assets/cosmosdb_apis.svg">

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

<img src="assets/cosmosdb_comparison.svg">

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

### **Azure Database for PostgreSQL / MySQL (Flexible Server)**
Zarządzane instancje popularnych baz open‑source.

- automatyczne backupy, maintenance, skalowanie
- tryby: Single Server (legacy) i Flexible Server (zalecany)
- idealne dla aplikacji korzystających z open‑source DB

---

<a id="sec-18-subscription-models"></a>
## 18. Subskrypcje (Subscription Models)

<img src="assets/subscriptions.svg">

**Azure Subscription Models**
Azure oferuje różne modele subskrypcji w zależności od tego, jak organizacja chce kupować i rozliczać usługi chmurowe.

**Pay-As-You-Go (PAYG)**
- Płacisz tylko za zużycie, bez umów i zobowiązań — idealne na start i małe projekty.
- PAYG nie daje żadnych stałych zniżek, ale można obniżyć koszty poprzez Reservations lub Azure Hybrid Benefit.

**Enterprise Agreement (EA)**
- Długoterminowa umowa dla dużych firm, dająca zniżki i centralne zarządzanie kosztami.
- EA daje największe zniżki, bo są oparte na wolumenie i deklarowanym wydatku organizacji.

**CSP (Cloud Solution Provider)**
- Kupujesz Azure przez partnera, który zapewnia rozliczenia i dodatkowe wsparcie.
- W CSP zniżki zależą od partnera, a nie od Microsoft — partner może dać rabat lub własne ceny.

---

<a id="sec-19-azure-cli"></a>
## 19. Azure CLI

Azure CLI to **narzędzie wiersza poleceń**, które pozwala zarządzać usługami Azure z terminala — szybciej i wygodniej niż przez portal.

Jest cross‑platform (Windows, Linux, macOS), działa też w Azure Cloud Shell.

Pozwala wykonywać polecenia administracyjne oraz automatyzować zadania w skryptach.

### Inne narzędzia zarządzania Azure

- **Azure Portal** – GUI do administracji i szybkiej konfiguracji zasobów.
- **Azure PowerShell** – automatyzacja i administracja skryptami PowerShell.
- **Azure Cloud Shell** – gotowe środowisko CLI/PowerShell w przeglądarce, bez instalacji lokalnej.
- **Azure Mobile App** – podgląd alertów i podstawowe operacje administracyjne z telefonu.

### **Oficjalne API** Microsoft tutaj:
- https://learn.microsoft.com/en-us/cli/azure/reference-index?view=azure-cli-latest

### Microsoft opisuje schemat jako:
- az <grupa> <podgrupa> <komenda> --parametr wartość

### LOGIN
- az login
- az account set --subscription "<SUB>"

### RESOURCE GROUP
- az group create --name MyRG --location westeurope
- az group delete --name MyRG --yes --no-wait

### VM
- az vm create --resource-group MyRG --name MyVM --image UbuntuLTS
- az vm start --resource-group MyRG --name MyVM
- az vm stop --resource-group MyRG --name MyVM
- az vm delete --resource-group MyRG --name MyVM --yes

### STORAGE
- az storage account create --name mystorage --resource-group MyRG --sku Standard_LRS

### NETWORK
- az network vnet create --resource-group MyRG --name MyVNet
- az network public-ip create --resource-group MyRG --name MyIP

### WEB APP
- az appservice plan create --name MyPlan --resource-group MyRG --sku B1
- az webapp create --resource-group MyRG --plan MyPlan --name mywebapp123

### AKS
- az aks create --resource-group MyRG --name MyAKS --node-count 3
- az aks get-credentials --resource-group MyRG --name MyAKS

### RBAC
- az role assignment create --assignee <USER> --role Reader --scope /subscriptions/<SUB>

---

<a id="sec-20-redis-cache"></a>
## 20. Azure Cache for Redis

Azure Cache for Redis to w pelni zarzadzana usluga cache w pamieci, oparta na popularnym silniku Redis. Umozliwia drastyczne przyspieszenie aplikacji poprzez przechowywanie czesto uzywanych danych w szybkiej pamieci zamiast odpytywania bazy danych.

<img src="assets/redis_cache_overview.svg" alt="Azure Cache for Redis - jak dziala cache" width="100%">

### 20.1 Po co uzywac cache?

| Problem | Rozwiazanie z Redis |
|---------|--------------------|
| Wolne odpowiedzi z bazy danych | Cache przechowuje dane w pamieci (1-5ms vs 100-500ms) |
| Wysokie obciazenie bazy | Redis przejmuje 80-90% odczytow |
| Sesje uzytkownikow rozrzucone | Centralne przechowywanie sesji |
| Kosztowne obliczenia | Cache wynikow, nie powtarzaj obliczen |
| Limity polaczen do DB | Mniej zapytan = mniej polaczen |

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

<img src="assets/redis_tiers.svg" alt="Azure Cache for Redis - warstwy cenowe" width="100%">

| Tier | SLA | Replikacja | Persistence | Clustering | Kiedy uzywac |
|------|-----|------------|-------------|------------|-------------|
| **Basic** | Brak | Brak | Nie | Nie | Dev/Test |
| **Standard** | 99.9% | Primary + Replica | Nie | Nie | Wiekszosc produkcji |
| **Premium** | 99.9% | P + R + Geo | RDB/AOF | Do 10 shardow | Duze obciazenia |
| **Enterprise** | 99.999% | Active-Active | Pelna | Tak | Mission-critical |

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

<img src="assets/cache_aside_pattern.svg" alt="Cache-Aside Pattern" width="100%">

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

<img src="assets/write_through_pattern.svg" alt="Write-Through Pattern" width="100%">

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

<img src="assets/write_behind_pattern.svg" alt="Write-Behind Pattern" width="100%">

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

### 20.7 Przyklad uzycia w kodzie (.NET)

```csharp
// NuGet: StackExchange.Redis
var redis = ConnectionMultiplexer.Connect("myrediscache.redis.cache.windows.net:6380,password=xxx,ssl=True");
var db = redis.GetDatabase();

// Zapis
db.StringSet("user:123", JsonSerializer.Serialize(user), TimeSpan.FromMinutes(30));

// Odczyt
var cached = db.StringGet("user:123");
if (cached.HasValue)
{
    return JsonSerializer.Deserialize<User>(cached);
}
```

### 20.8 Best Practices

**Klucze:**
- Uzywaj prefixow: `user:123`, `product:456`, `session:abc`
- Unikaj bardzo dlugich kluczy (max 1KB, optymalnie < 100 bajtow)

**TTL (Time To Live):**
- Zawsze ustawiaj TTL dla danych cache
- Typowe wartosci: 5-60 minut dla danych, 30 min - 24h dla sesji

**Eviction Policy:**
- `volatile-lru` - usun najstarsze klucze z TTL (domyslne)
- `allkeys-lru` - usun najstarsze ze wszystkich kluczy
- `noeviction` - blad gdy brak pamieci

**Monitoring:**
- Azure Monitor dla metryk (hits, misses, memory)
- Cache Hit Ratio powinna byc > 90%
- Alerts na wysokie uzycie pamieci

---

<a id="sec-21-deployment"></a>
## 21. Wdrażanie aplikacji na Azure (Deployment)

Wdrażanie (deployment) to proces przeniesienia aplikacji ze środowiska deweloperskiego do produkcji. Azure oferuje wiele metod wdrażania, od prostych (przez portal) po zaawansowane (CI/CD pipelines).

<img src="assets/deployment_methods.svg" alt="Metody wdrażania na Azure" width="100%">

### 20.1 Metody wdrażania - przegląd

| Metoda | Zalety | Wady | Kiedy używać |
|--------|--------|------|--------------|
| **Azure Portal** | Wizualny, łatwy start | Nieskalowalny, brak automatyzacji | Nauka, testy, jednokrotne wdrożenia |
| **Azure CLI / PowerShell** | Skryptowalny, powtarzalny | Wymaga znajomości składni | Automatyzacja prostych zadań |
| **ARM Templates / Bicep (IaC)** | Pełna powtarzalność, wersjonowanie | Krzywa uczenia się | Infrastruktura produkcyjna |
| **CI/CD Pipelines** | Pełna automatyzacja, testy | Złożona konfiguracja | Profesjonalne projekty |

### 20.2 Wdrażanie przez Azure Portal

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

### 20.3 Wdrażanie przez Azure CLI

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

### 20.4 Infrastructure as Code (IaC)

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

### 20.5 CI/CD Pipelines

<img src="assets/cicd_pipeline.svg" alt="CI/CD Pipeline" width="100%">

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

### 20.6 Deployment Slots (Blue-Green Deployment)

<img src="assets/deployment_slots.svg" alt="Deployment Slots" width="100%">

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

### 20.7 Wdrażanie kontenerów

<img src="assets/container_deployment.svg" alt="Wdrażanie kontenerów" width="100%">

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

### 20.8 Porównanie usług dla kontenerów

| Usługa | Typ | Orkiestracja | Kiedy używać |
|--------|-----|--------------|--------------|
| **App Service** | PaaS | Brak (1 kontener) | Proste web apps |
| **ACI** | CaaS | Brak | Batch jobs, szybkie testy |
| **AKS** | Kubernetes | Pełna | Mikroserwisy, duże systemy |
| **Container Apps** | Serverless | Dapr, KEDA | Scale-to-zero, eventy |

### 20.9 Dobre praktyki wdrażania

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

<a id="sec-22-last-minute-cram"></a>
## 22. Last-minute cram (pułapki + porównania)

### 40 najczęstszych pułapek egzaminacyjnych AZ‑900

1. Subskrypcja nie kosztuje sama w sobie — płacisz za zasoby.
2. Resource Group to kontener logiczny, nie granica sieci.
3. RBAC dziedziczy się tylko w dół hierarchii.
4. `Owner` może zarządzać dostępem, `Contributor` nie.
5. Azure Policy `Deny` blokuje wdrożenie.
6. `Audit` w Policy nie blokuje wdrożenia.
7. Lock `ReadOnly` blokuje zmiany i usunięcie.
8. Lock `Delete` blokuje tylko usuwanie.
9. Region to nie to samo co Availability Zone.
10. Availability Set to nie Availability Zone.
11. HA zwykle dotyczy 1 regionu, DR zwykle 2+ regionów.
12. RTO = czas odtworzenia, RPO = dopuszczalna utrata danych.
13. Egress jest płatny, ingress zazwyczaj bezpłatny.
14. ExpressRoute jest prywatny, ale nie szyfruje danych domyślnie.
15. VPN Site‑to‑Site łączy sieci, Point‑to‑Site pojedynczego użytkownika.
16. NSG to ACL L3/L4, nie pełny firewall aplikacyjny.
17. Azure Firewall to stateful firewall, NSG nie.
18. DDoS Standard chroni L3/L4, nie L7.
19. WAF chroni warstwę aplikacyjną HTTP(S), np. OWASP Top 10.
20. Private Endpoint daje prywatny IP, Service Endpoint nie.
21. Front Door działa globalnie na edge (L7).
22. Traffic Manager działa na DNS, nie proxy’uje ruchu HTTP.
23. Load Balancer działa na L4 (TCP/UDP).
24. Application Gateway działa na L7 i może mieć WAF.
25. Azure Functions = event-driven serverless; płatność za wykonania (Consumption).
26. Durable Functions służą do długich workflow ze stanem.
27. ACI to szybkie uruchomienie kontenera bez orkiestracji.
28. ACA jest wyżej poziomem niż ACI (mikroserwisy, scale-to-zero).
29. AKS daje pełną orkiestrację Kubernetes.
30. PaaS zmniejsza odpowiedzialność za OS i patchowanie.
31. W IaaS odpowiadasz m.in. za OS i hardening VM.
32. Entra ID to IAM, nie klasyczny AD Domain Services.
33. Conditional Access ocenia kontekst (user/device/location/risk).
34. PIM daje JIT/JEA dla ról uprzywilejowanych.
35. Managed Identity eliminuje potrzebę trzymania sekretów w kodzie.
36. Blob tiers: Hot/Cool/Cold/Archive różnią się kosztem zapisu i odczytu.
37. LRS/ZRS/GRS/RA‑GRS to poziomy redundancji storage.
38. Cosmos DB to globalny NoSQL z multi‑region i niskimi opóźnieniami.
39. Preview nie oznacza pełnej gotowości produkcyjnej i pełnego SLA.
40. GA oznacza stabilność produkcyjną i standardowe warunki wsparcia/SLA.

---

### Szybkie porównania usług (ściąga)

#### Compute

| Usługa | Kiedy wybierać | Kluczowa cecha |
|---|---|---|
| Virtual Machines | pełna kontrola nad OS | IaaS, największa elastyczność |
| App Service | web/API bez administrowania serwerami | PaaS dla aplikacji HTTP |
| Azure Functions | eventy, krótkie zadania | serverless, trigger + binding |
| Azure Container Apps | mikroserwisy/kontenery bez K8s | autoscaling + scale-to-zero |
| AKS | złożone kontenery na dużą skalę | pełny Kubernetes |

#### Integracja i zdarzenia

| Usługa | Wzorzec | Najlepsze zastosowanie |
|---|---|---|
| Service Bus | kolejki/topics (enterprise messaging) | niezawodna integracja systemów |
| Event Grid | routing eventów (push) | zdarzenia reaktywne w Azure |
| Event Hub | streaming telemetry | duży wolumen zdarzeń/IoT/logi |
| Storage Queue | prosta kolejka | lekkie scenariusze async |

#### Sieć i ruch web

| Usługa | Warstwa | Zastosowanie |
|---|---|---|
| Load Balancer | L4 | ruch TCP/UDP dla VM/VMSS |
| Application Gateway | L7 | routing HTTP(S) + opcjonalny WAF |
| Front Door | L7 Edge | globalne aplikacje web, accel + WAF |
| Traffic Manager | DNS | globalny routing/failover DNS |

#### Tożsamość i bezpieczeństwo

| Mechanizm | Co robi | Typowe użycie |
|---|---|---|
| RBAC | autoryzacja do zasobów | kto co może zrobić |
| Conditional Access | polityki dostępu zależne od kontekstu | wymuszenie MFA/compliance |
| PIM | czasowe role uprzywilejowane | JIT dla adminów |
| Managed Identity | tożsamość dla aplikacji bez sekretów | dostęp do KV/Storage/SQL |

#### Storage

| Typ | Charakter danych | Przykład |
|---|---|---|
| Blob | obiektowe, niestrukturalne | backupy, logi, media |
| Files | SMB/NFS share | udziały plików |
| Queue | wiadomości | komunikacja async |
| Table | NoSQL key-value | metadane, telemetria |
| Managed Disks | dyski VM | OS/Data disk dla IaaS |

---

### Jak używać tej sekcji 24h przed egzaminem

- Przerób 40 pułapek 2 razy (rano i wieczorem).
- Jeśli mylisz 2 usługi, wróć do odpowiedniej tabeli porównawczej.
- Skup się na różnicach: zakres odpowiedzialności, warstwa sieci, model płatności, poziom zarządzania.

### Exam-day strategy (pod pytania podchwytliwe)

- Najpierw szukaj słów‑kluczy w pytaniu: **global/regional**, **L3/L4/L7**, **IaaS/PaaS/SaaS**, **HA/DR**, **RBAC/Policy**.
- Eliminuj odpowiedzi absolutne typu „zawsze”, „nigdy”, jeśli scenariusz dopuszcza wyjątki.
- Jeśli pytanie dotyczy „kto odpowiada”, użyj modelu **Shared Responsibility**.
- Jeśli pytanie dotyczy „najmniej operacji”, wybieraj bardziej zarządzane usługi (PaaS/serverless).
- Zostaw trudne pytania na koniec i wróć po pierwszym przejściu.

<a id="sec-appendix-ascii"></a>
## Załącznik A – Diagramy ASCII
**Hierarchia i scope:**
```
Tenant
 └─ Management Groups
     └─ Subscription(s)
         └─ Resource Group(s)
             └─ Resource(s)
```

**Region i strefy AZ:**
```
Region (low-latency network)
 ├─ AZ1 (independent power/cooling/network)
 ├─ AZ2 (independent power/cooling/network)
 └─ AZ3 (independent power/cooling/network)
```

---
