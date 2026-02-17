
# Microsoft Azure – AZ‑900 Fundamentals (Kompendium w 1 pliku)

> Kompletny, praktyczny README do szybkiej nauki i utrwalenia wiedzy pod egzamin **AZ‑900 Microsoft Azure Fundamentals**. Zawiera: podstawy chmury, architekturę Azure, kluczowe usługi (Compute/Networking/Storage), tożsamość i bezpieczeństwo (Microsoft Entra), governance i koszty, monitoring, IaC & automatyzację, rozszerzone usługi oraz **podchwytliwe pytania egzaminacyjne**.

---

## Spis treści
- [1. Cloud Concepts (Podstawy chmury)](#1-cloud-concepts-podstawy-chmury)
- [2. Azure Architecture (Architektura i hierarchia)](#2-azure-architecture-architektura-i-hierarchia)
- [3. Compute Services (Usługi obliczeniowe)](#3-compute-services-usługi-obliczeniowe)
- [4. Networking (Sieci i łączność)](#4-networking-sieci-i-łączność)
- [5. Storage (Przechowywanie danych)](#5-storage-przechowywanie-danych)
- [6. Identity & Access (Microsoft Entra)](#6-identity--access-microsoft-entra)
- [7. Security (Bezpieczeństwo)](#7-security-bezpieczeństwo)
- [8. Governance & Compliance](#8-governance--compliance)
- [9. Monitoring & Logging](#9-monitoring--logging)
- [10. Costs & Billing (Koszty i rozliczenia)](#10-costs--billing-koszty-i-rozliczenia)
- [11. IaC & Automation (Infrastruktura jako kod)](#11-iac--automation-infrastruktura-jako-kod)
- [12. Extended Azure Services (Usługi rozszerzone)](#12-extended-azure-services-usługi-rozszerzone)
- [13. Podchwytliwe pytania AZ‑900](#13-podchwytliwe-pytania-az900)
- [14. Glosariusz skrócony](#14-glosariusz-skrócony)
- [Załącznik A – Diagramy ASCII](#załącznik-a--diagramy-ascii)

---

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

#### **Throttling / Rate Limiting**
Kontrola obciążenia aplikacji i API w czasie autoskalowania — ochrona przed lawinowym ruchem.

---

## Odporność, Wysoka Dostępność i Disaster Recovery

### **High Availability (HA) – Wysoka dostępność**
Projektowanie infrastruktury w taki sposób, aby usługa pozostawała dostępna mimo awarii pojedynczych elementów.

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

## 2. Azure Architecture (Architektura i hierarchia)
**Hierarchia i scope:**
```
Tenant → Management Group → Subscription → Resource Group → Resource
```
- **Tenant (Microsoft Entra ID)** – tożsamość organizacji
- **Management Group** – grupowanie subskrypcji, polityki na poziomie całej organizacji
- **Subscription** – rozliczenia, limity, granica uprawnień
- **Resource Group (RG)** – logiczny kontener zasobów
- **Resource** – pojedyncza usługa (VM, VNet, Storage, App Service itd.)

**Globalna infrastruktura:**
- **Region** – zestaw współpołączonych centrów danych
- **Availability Zone (AZ)** – fizycznie separowane DC w obrębie regionu (osobne zasilanie/chłodzenie/sieć)
- **Geography** – granica rezydencji danych (np. EU/US)
- **Region Pair** – sparowany region w tej samej geografii (aktualizacje, DR)

**ARM – warstwa zarządzania:**
- Spójne API (Portal/CLI/PowerShell/REST)
- Deklaratywne wdrożenia: **ARM Templates** (JSON) / **Bicep**
- Organizacja: **tags**, **locks** (Delete/ReadOnly)

---

## 3. Compute Services (Usługi obliczeniowe)
- **Virtual Machines** – rozmiary serii (B, Dv5/Ev5, pamięciochłonne/obliczeniowe/GPU), managed disks, VM extensions
- **Availability Sets** – Fault Domains/Update Domains (redukcja ryzyka awarii/maintenance)
- **VM Scale Sets (VMSS)** – autoskalowanie wielu VM, rolling upgrades
- **App Service** – Web/API/Mobile Apps, deployment slots
- **Azure Functions** – consumption/premium, trigery i bindingi
- **Azure Container Instances (ACI)** – szybkie kontenery bez orkiestracji
- **AKS** – zarządzany Kubernetes (integracja ACR, autoscaling)
- **Azure Bastion** – bezpieczny RDP/SSH w przeglądarce bez publicznego IP na VM

---

## 4. Networking (Sieci i łączność)
- **VNet / Subnets / UDR** – prywatne adresacje, trasowanie
- **VNet Peering** – łączenie sieci przy niskim opóźnieniu
- **VPN Gateway** – IPsec site‑to‑site/point‑to‑site
- **ExpressRoute** – prywatne łącze do Azure
- **Private Endpoint / Service Endpoint** – prywatny dostęp do usług PaaS

**Równoważenie i edge:**
- **Azure Load Balancer (L4)** – SNAT, inbound/outbound
- **Application Gateway (L7 + WAF)** – HTTP(S), URL‑based routing, Web Application Firewall
- **Traffic Manager (DNS LB)** – geo/priority/weighted
- **Front Door** – globalny entry point (acceleracja + WAF + routing)

---

## 5. Storage (Przechowywanie danych)
**Typy:**
- **Blob** (Hot/Cool/Archive)
- **File** (SMB, Azure Files, File Sync)
- **Queue** (kolejki wiadomości)
- **Table** (NoSQL key‑value)
- **Managed Disks** (OS/Data/Temp, Premium/Standard/Ultra)

**Redundancja:**
- **LRS** (kopie w jednym DC)
- **ZRS** (kopie w wielu AZ w regionie)
- **GRS / RA‑GRS** (replikacja cross‑region)

**Narzędzia i migracje:**
- **Storage Explorer**, **AzCopy**
- **Azure Migrate**, **Data Box**

---

## 6. Identity & Access (Microsoft Entra)
- **Microsoft Entra ID** – tożsamość, SSO, MFA, External Identities, Passwordless
- **RBAC** – Owner/Contributor/Reader + role niestandardowe; dziedziczenie w dół scope
- **Conditional Access** – zasady warunkowe (ryzyko, lokalizacja, urządzenie)
- **PIM** – Just‑In‑Time dla ról uprzywilejowanych
- **Managed Identities** – tożsamość dla aplikacji bez sekretów

---

## 7. Security (Bezpieczeństwo)
- **Microsoft Defender for Cloud** – Secure Score, rekomendacje, ochrona obciążeń (VM/DB/Storage)
- **Key Vault** – Secrets/Keys/Certificates (+ HSM)
- **DDoS Protection** – Basic/Standard
- **WAF** – na Application Gateway/Front Door

---

## 8. Governance & Compliance
- **Azure Policy** – Definition (pojedyncza reguła) i Initiative (zestaw); **Effects**: Deny/Audit/Append/Modify; przypisanie na **scope** (MG/Sub/RG/Resource)
- **Resource Locks** – Delete/ReadOnly (ochrona przed przypadkowym usunięciem/modyfikacją)
- **Tags** – metadane dla organizacji i rozliczeń
- **Azure Blueprints** – pakiet governance (Policy + RBAC + ARM/Bicep + struktura RG)
- **Azure Arc** – governance i zarządzanie zasobami on‑prem/multi‑cloud

---

## 9. Monitoring & Logging
- **Azure Monitor** – **Metrics** (czasowe), **Logs** (KQL, Log Analytics), **Alerts** (metryki/zapytania)
- **Service Health** – incydenty i maintenance w regionach/usługach
- **Activity Log** – kto/co/kiedy (operacje ARM)
- **Resource Logs** – szczegółowe logi działania (wymagają **Diagnostic Settings**)
- **Workbooks** – dashboardy + wizualizacje + KQL

---

## 10. Costs & Billing (Koszty i rozliczenia)
**Za co płacisz:** compute (czas działania), storage (GB/mies.), **egress** sieci, licencje

**Optymalizacja:**
- **Reservations** (1/3 lata)
- **Spot VMs** (tańsze, preemptible)
- **Azure Hybrid Benefit** (Windows/SQL)

**Narzędzia:**
- **Pricing Calculator**, **TCO Calculator**, **Cost Management** (budżety/alerty/analizy)

---

## 11. IaC & Automation (Infrastruktura jako kod)
- **ARM Templates (JSON)** / **Bicep** – deklaratywne wdrożenia, idempotencja, GitOps
- **Terraform** – multi‑cloud IaC (provider AzureRM)
- **DSC** – wymuszanie stanu konfiguracji
- **Automation** – runbooki, Update Management, Change Tracking

---

## 12. Extended Azure Services (Usługi rozszerzone)
**Integracja i zdarzenia:**
- **Service Bus** (kolejki/topics)
- **Event Hub** (telemetria/big‑data ingress)
- **Event Grid** (router zdarzeń)

**API i edge:**
- **API Management** (APIM)
- **Front Door / CDN / Traffic Manager**

**AI i Data:**
- **Cognitive Services / Azure OpenAI / Azure ML**
- **Data Factory / Synapse Pipelines / Databricks**

**IoT:**
- **IoT Hub / IoT Central / Digital Twins / Sphere**

**Inne przydatne:**
- **Azure Bastion**, **Azure Load Testing**, **Azure Cache for Redis**, **App Configuration / Feature Flags**, **AKS Fleet**, **Container Registry (ACR)**, **Azure Lighthouse**, **Azure Virtual Desktop**

---

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

## 14. Glosariusz skrócony
- **Resource** – pojedynczy zasób w Azure (VM/VNet/Storage/App itd.)
- **Resource Group** – kontener zasobów
- **Subscription** – granica rozliczeń, limitów, uprawnień
- **Management Group** – grupowanie subskrypcji
- **Region / AZ / Geography / Region Pair** – elementy globalnej infrastruktury
- **ARM / Bicep** – deklaratywne wdrożenia
- **RG Tags / Locks** – metadane i blokady (Delete/ReadOnly)

---

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
