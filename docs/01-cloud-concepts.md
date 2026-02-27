<a id="sec-01-cloud-concepts"></a>
## 1. Cloud Concepts (Podstawy chmury)

[ Powrót do spisu treści](../README.md)


## Modele wdrożenia

### Public Cloud
Usługi uruchamiane w infrastrukturze dostawcy (np. Microsoft Azure). Dostępność i skalowanie zapewnia provider, a Ty płacisz za zużycie. Szybki time‑to‑market, szeroka oferta usług, brak konieczności utrzymywania hardware’u.
<br>

<img src="../assets/public_cloud.svg" alt="Public Cloud - model chmury publicznej">

**Kiedy używać:** projekty o zmiennym obciążeniu, potrzeba globalnego zasięgu, krótkie cykle wdrożeniowe.  
**Ryzyka/uwagi:** lock‑in, zgodność/regulacje, kontrola nad siecią niższa niż on‑prem.

---

### Private Cloud
Dedykowana chmura dla jednej organizacji (własne DC / hosting dedykowany / Azure Stack HCI). Najwyższa kontrola i możliwość dopasowania do rygorów zgodności, kosztem zwinności i CAPEX/OPEX.
<br>

<img src="../assets/private_cloud.svg" alt="Private Cloud - model chmury prywatnej">

**Kiedy używać:** silne wymagania compliance, izolacja, specyficzne potrzeby bezpieczeństwa/latencji.  
**Ryzyka/uwagi:** większe koszty utrzymania, mniejsza elastyczność skalowania.

---

### Hybrid Cloud
Połączenie środowisk on‑prem/private z public cloud. Umożliwia przenoszenie obciążeń, burst do chmury i stopniową migrację, z zachowaniem kontroli nad danymi krytycznymi.
<br>

<img src="../assets/hybrid_cloud.svg" alt="Hybrid Cloud - model chmury hybrydowej">

**Kiedy używać:** modernizacja etapowa, wymogi rezydencji danych, integracje z istniejącymi systemami.  
**Ryzyka/uwagi:** złożoność sieci/identyfikacji/monitoringu, potrzeba spójnego governance.

---

### Community Cloud
Chmura współdzielona przez grupę organizacji o wspólnych wymaganiach — np. sektor publiczny, medyczny, edukacyjny lub finansowy. Zapewnia wspólne standardy bezpieczeństwa, zgodność regulacyjną i kontrolę nad środowiskiem.
<br>

<img src="../assets/community_cloud.svg" alt="Community Cloud - model chmury spolecznosciowej">

**Kiedy używać:** organizacje o wspólnych regulacjach (RODO, HIPAA, FINREP), współdzielone koszty, potrzeba jednolitych polityk bezpieczeństwa.  
**Ryzyka/uwagi:** uzgodnienie governance między uczestnikami, współdzielona odpowiedzialność, potencjalnie mniejsza elastyczność niż public cloud.

---

### Multi‑Cloud
Wykorzystanie wielu dostawców chmury (np. Azure, AWS, GCP) jednocześnie. Pozwala redukować vendor lock‑in, wybierać najlepsze usługi z różnych platform oraz zwiększyć odporność na awarie jednego providera.
<br>

<img src="../assets/multi_cloud.svg" alt="Multi-Cloud - model wielu dostawcow chmury">

**Kiedy używać:** minimalizacja zależności od jednego dostawcy, strategia „best‑of‑breed”, wymagania biznesowe lub prawne dotyczące dywersyfikacji.  
**Ryzyka/uwagi:** wyższa złożoność operacyjna, trudniejsze bezpieczeństwo i monitoring, konieczność ujednolicenia narzędzi i procesów (IaC/CI/CD).

---

### Distributed Cloud
Model, w którym usługi chmurowe dostawcy (np. Azure, AWS, GCP) są fizycznie uruchamiane bliżej użytkownika — w lokalnych centrach danych, edge‑location, on‑prem lub w regionach partnerskich. Pozwala zachować jedno zarządzanie chmurą, ale wykonywać obliczenia tam, gdzie są potrzebne.
<br>

<img src="../assets/distributed_cloud.svg" alt="Distributed Cloud - model chmury rozproszonej">

**Kiedy używać:** niska latencja, wymagania rezydencji danych, obciążenia przemysłowe/IoT, obliczenia blisko fabryki lub oddziału.  
**Ryzyka/uwagi:** większa złożoność wdrożeń, koordynacja między lokalnymi zasobami a centralną chmurą, zależność od infrastruktury partnerów.

---

### Edge Cloud
Przetwarzanie danych bezpośrednio na „krawędzi” sieci — blisko urządzeń IoT, fabryk, sklepów, samochodów autonomicznych czy systemów przemysłowych. Minimalizuje opóźnienia i redukuje transfer danych do regionów chmurowych.
<br>

<img src="../assets/edge_cloud.svg" alt="Edge Cloud - przetwarzanie na krawedzi sieci">

**Kiedy używać:** ultra‑niska latencja, IoT, urządzenia mobilne, autonomiczne systemy, lokalne decyzje w czasie rzeczywistym.  
**Ryzyka/uwagi:** konieczność zarządzania wieloma lokalizacjami, ograniczone zasoby sprzętowe, bardziej złożone bezpieczeństwo.

---

## Modele usług chmurowych (Cloud Service Models)

### IaaS (Infrastructure as a Service)
Dostawca udostępnia zasoby infrastruktury: maszyny wirtualne, sieci, load balancery, firewalle, dyski.  
Użytkownik zarządza systemem operacyjnym, aplikacjami i konfiguracją.

<img src="../assets/iaas.svg" alt="IaaS - Infrastructure as a Service">

**Przykłady:** Azure VM, VNet, Load Balancer, Storage.  
**Kiedy używać:** migracje lift‑and‑shift, systemy wymagające pełnej kontroli nad OS.  
**Ryzyka/uwagi:** większe koszty utrzymania i administracji.

---

### PaaS (Platform as a Service)
Środowisko uruchomieniowe dla aplikacji bez konieczności zarządzania OS, patchami i infrastrukturą.  
Odpowiadasz jedynie za kod i dane.

<img src="../assets/paas.svg" alt="PaaS - Platform as a Service">

**Przykłady:** Azure App Service, Azure SQL, Functions (Premium).  
**Kiedy używać:** API, mikroserwisy, aplikacje web.  
**Plusy:** automatyczne skalowanie, wysoka dostępność, szybkie wdrażanie.

---

### SaaS (Software as a Service)
W pełni gotowe aplikacje dostarczane jako usługa — bez instalacji i utrzymania.

<img src="../assets/saas.svg" alt="SaaS - Software as a Service">

**Przykłady:** Microsoft 365, Power BI, Salesforce.  
**Kiedy używać:** poczta, dokumenty, CRM, HR, analiza danych.  
**Plusy:** minimalny narzut operacyjny.

---

### Serverless / FaaS (Function as a Service)
Kod uruchamiany na żądanie, bez serwerów i bez opłat stałych — płacisz tylko za wykonanie.

<img src="../assets/faas.svg" alt="FaaS - Function as a Service / Serverless">

**Przykłady:** Azure Functions (Consumption), AWS Lambda.  
**Kiedy używać:** event‑driven, automatyzacje, integracje, IoT.  
**Plusy:** pełna autoskalowalność.

---

### NaaS (Network as a Service)
Sieć jako usługa — routing, VPN, SD‑WAN, firewalle, połączenia między chmurami, edge networking.

<img src="../assets/naas.svg" alt="NaaS - Network as a Service">

**Przykłady:** Azure Virtual WAN, AWS Cloud WAN.  
**Kiedy używać:** globalna sieć firmy, integracje multi‑cloud, oddziały.  
**Ryzyka/uwagi:** zależność od usług sieciowych dostawcy.

---

### CaaS (Container as a Service)
Zarządzane środowisko kontenerowe — bez utrzymywania VM czy orkiestracji.

<img src="../assets/caas.svg" alt="CaaS - Container as a Service">

**Przykłady:** Azure Container Instances, AWS Fargate, Cloud Run.  
**Kiedy używać:** mikroserwisy, API, krótkie zadania batch.  
**Plusy:** zero administrowania infrastrukturą.

---

### DaaS (Desktop as a Service)
Zdalne, hostowane w chmurze środowiska pulpitu użytkownika.

<img src="../assets/daas.svg" alt="DaaS - Desktop as a Service">

**Przykłady:** Azure Virtual Desktop, Windows 365.  
**Kiedy używać:** praca zdalna, call‑center, access z dowolnego urządzenia.  
**Ryzyka/uwagi:** zależność od jakości łącza.

---

### iPaaS (Integration Platform as a Service)
Platformy integracyjne do łączenia systemów, API, ETL i workflow.

<img src="../assets/ipaas.svg" alt="iPaaS - Integration Platform as a Service">

**Przykłady:** Azure Logic Apps, MuleSoft, Boomi.  
**Kiedy używać:** integracje ERP/CRM, automatyzacja procesów.  
**Plusy:** szybkie łączenie systemów bez pisania backendu.

---

### MBaaS / BaaS (Mobile/Backend as a Service)
Backend dostarczany jako usługa — auth, bazy, storage, push, API.

<img src="../assets/mbaas.svg" alt="MBaaS - Mobile Backend as a Service">

**Przykłady:** Firebase, AWS Amplify.  
**Kiedy używać:** aplikacje mobilne, prototypy, szybki development.

---

### BPaaS (Business Process as a Service)
Gotowe procesy biznesowe jako usługa, np. HR, płace, księgowość, CRM.

<img src="../assets/bpaas.svg" alt="BPaaS - Business Process as a Service">

**Przykłady:** Workday, Dynamics 365, Salesforce.  
**Kiedy używać:** outsourcing procesów biznesowych.  
**Plusy:** standaryzacja i skalowalność.

---

## Korzyści chmury (Cloud Benefits)

Egzamin AZ-900 wymaga znajomości **4 kluczowych obszarów korzyści** wynikających z korzystania z chmury:

<img src="../assets/cloudbenefits.svg" alt="Korzysci chmury - dostepnosc, skalowalnosc, niezawodnosc, przewidywalnosc">

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

<img src="../assets/pricingmodels.svg" alt="Modele cenowe Azure - Pay-As-You-Go, Reserved, Spot">

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

<img src="../assets/vertical_scaling.svg" alt="Vertical Scaling - skalowanie pionowe">

**Charakterystyka:**
- Skalowanie „w górę” = mocniejsza maszyna.
- Skalowanie „w dół” = tańsza/słabsza maszyna.
- Zwykle **wymaga restartu** (przestój zależny od usługi).
- Ograniczenia sprzętowe — istnieje „sufit” skali.

**Kiedy używać:** bazy danych, monolity, workloady zależne od pojedynczego węzła.

---

### **Horizontal Scaling (Scale Out / Scale In)**
Dodawanie lub usuwanie *instancji zasobu*. Zamiast jednej mocnej maszyny — wiele mniejszych pracujących równolegle.

<img src="../assets/horizontal_scaling.svg" alt="Horizontal Scaling - skalowanie poziome">

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

<img src="../assets/autoscaling.svg" alt="Autoskalowanie - automatyczne skalowanie zasobow">

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

<img src="../assets/high_availability.svg" alt="High Availability - wysoka dostepnosc">

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

<img src="../assets/resilience.svg" alt="Fault Tolerance - odpornosc na awarie">

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

<img src="../assets/disaster_recovery.svg" alt="Disaster Recovery - odzyskiwanie po awarii">

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

---

Poprzednia strona | [Spis treści](../README.md) | [Następna strona](./02-azure-architecture.md)
