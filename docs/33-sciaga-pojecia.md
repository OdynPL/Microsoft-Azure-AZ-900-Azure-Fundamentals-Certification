

[⬅ Powrót do spisu treści](../README.md)

# **AZ-900 – Ściąga pojęć do druku (wersja rozszerzona)**


## **Szybkie porównania i skróty (do powtórki)**

---

* **On-premises network / sieć lokalna** – Tradycyjna infrastruktura IT działająca w siedzibie firmy (serwery, sieć, storage, urządzenia), zarządzana i utrzymywana samodzielnie, w przeciwieństwie do zasobów w chmurze.

* **IaaS vs PaaS vs SaaS:**
    * **IaaS:** VM, Storage, sieć – pełna kontrola, zarządzasz OS.
    * **PaaS:** App Service, SQL DB – nie zarządzasz OS, tylko aplikacją.
    * **SaaS:** Office 365 – gotowa aplikacja, tylko używasz.
* **NSG vs Firewall:**
    * **NSG:** reguły na subnet/VM, L4, prostsze.
    * **Firewall:** centralna ochrona, logi, L4/L7, zaawansowane scenariusze.
* **CDN vs Front Door vs Traffic Manager:**
    * **CDN:** cache, przyspiesza statyczne treści.
    * **Front Door:** globalny routing, WAF, L7.
    * **Traffic Manager:** DNS, routing geograficzny/failover.
* **App Service vs VM:**
    * **App Service:** PaaS, szybkie wdrożenia, automatyczne skalowanie.
    * **VM:** IaaS, pełna kontrola, własny OS.
* **Azure Policy vs RBAC:**
    * **Policy:** wymusza zasady (np. region, tagi).
    * **RBAC:** uprawnienia do zasobów.
* **Azure Backup vs Site Recovery:**
    * **Backup:** kopie zapasowe.
    * **Site Recovery:** DR, odtwarzanie po awarii.
* **Blob Storage vs File Storage:**
    * **Blob:** obiekty, backupy, REST API.
    * **File:** udziały plików, SMB/NFS.
* **ExpressRoute vs VPN:**
    * **ExpressRoute:** dedykowane łącze, nie szyfruje domyślnie.
    * **VPN:** szyfrowane połączenie przez internet.
* **Azure DevOps vs GitHub:**
    * **DevOps:** CI/CD, tablice zadań.
    * **GitHub:** repozytoria, CI/CD, społeczność.


---

## **Limity platformy Azure (wybrane przykłady)**

- **VM w subskrypcji (domyślnie):** 10–20 na region (można zwiększyć przez support request)
- **Resource Group:** max 980 zasobów
- **Storage Accounts:** 250 na subskrypcję na region
- **Public IP:** 10 na subskrypcję na region (można zwiększyć)
- **VNet:** 1000 na region
- **NSG:** 5000 na subskrypcję na region
- **Managed Disks:** 50 000 na region na subskrypcję
- **App Service Plan:** 100 na subskrypcję na region
- **Azure SQL Database:** 5000 DB na serwerze logicznym
- **Cosmos DB:** 50 kontenerów na bazę, 25 baz na konto
- **Azure Functions:** 1,5 mln wywołań/miesiąc (w planie Consumption, w ramach free tier)
- **Resource Groups na subskrypcję:** 980
- **Subskrypcje na konto:** 200
- **Tagi:** maksymalnie 50 tagów na jeden zasób lub grupę zasobów

**Uwaga:** Większość limitów można podnieść przez zgłoszenie do supportu. Aktualne limity: https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits

## **1. Podstawy chmury i korzyści**

### **Cloud benefits (Korzyści chmury)**

* **Reliability (Niezawodność)** – Usługi chmurowe są projektowane tak, aby zapewnić ciągłość działania i minimalizować ryzyko awarii dzięki nadmiarowości, automatycznym kopiom zapasowym i rozproszeniu geograficznemu.
* **Predictability (Przewidywalność)** – Chmura umożliwia przewidywalność wydajności i kosztów dzięki gwarantowanym SLA, rezerwacjom zasobów oraz narzędziom do monitorowania i prognozowania wydatków.
* **Resilience (Odporność)** – Zdolność systemu do szybkiego powrotu do działania po awarii lub zakłóceniu.
* **Agility (Zwinność)** – Możliwość szybkiego wdrażania zmian i reagowania na potrzeby biznesowe.
* **Elasticity (Elastyczność)** – Automatyczne dostosowanie zasobów do aktualnych potrzeb.
* **Scalability (Skalowalność)** – Łatwość zwiększania lub zmniejszania zasobów w zależności od obciążenia.
* **Security (Bezpieczeństwo)** – Zaawansowane mechanizmy ochrony danych, zgodność z normami i certyfikatami.
* **Cost Efficiency (Efektywność kosztowa)** – Płacisz tylko za to, czego używasz, bez inwestycji w infrastrukturę.
* **Global Reach (Globalny zasięg)** – Usługi dostępne na całym świecie, w wielu regionach.
* **On-demand self-service** – Samodzielne uruchamianie i zarządzanie zasobami bez kontaktu z dostawcą.
* **Manageability (Zarządzalność)** – Łatwość monitorowania, zarządzania i automatyzacji zasobów chmurowych dzięki narzędziom takim jak portale, API, alerty i raporty.

* **Cloud Computing** – Dostarczanie zasobów IT przez internet (serwery, bazy, aplikacje).
* **Public Cloud** – Usługi dostępne dla każdego (np. Azure, AWS, GCP).
* **Private Cloud** – Chmura tylko dla jednej organizacji.
* **Hybrid Cloud** – Połączenie chmury publicznej i prywatnej.
* **Elasticity** – Automatyczne dopasowanie zasobów do potrzeb.
* **Scalability** – Możliwość zwiększania/zmniejszania zasobów.
    * **Vertical scaling** – zwiększanie mocy jednej maszyny (np. więcej CPU/RAM).
    * **Horizontal scaling** – dodawanie kolejnych maszyn/instancji (np. więcej serwerów pracujących równolegle).
* **Agility** – Szybkie wdrażanie i zmiany.
* **High Availability (HA)** – Minimalizacja przestojów, ciągłość działania.
* **Disaster Recovery (DR)** – Odtwarzanie po awarii, kopie zapasowe.
* **Fault Tolerance** – Odporność na awarie.
* **Geo-distribution** – Rozmieszczenie usług w wielu regionach.
* **CapEx/OpEx** – Wydatki inwestycyjne vs operacyjne.
* **SLA (Service Level Agreement)** – Gwarantowany poziom dostępności usługi (np. 99,9%, 99,99%). Określa ile maksymalnie może trwać niedostępność usługi w miesiącu. Przekroczenie SLA = możliwość ubiegania się o kredyt/zwrot.
* **RTO/RPO** – Recovery Time/Point Objective: maksymalny czas odtworzenia usługi/akceptowalna utrata danych po awarii.
* **Soft Delete** – Ochrona przed przypadkowym usunięciem danych.
* **Immutable Storage** – Niezmienialne kopie, np. backupy nie do usunięcia.
* **Szyfrowanie at-rest/in-transit** – Dane domyślnie szyfrowane w Azure.
* **Multi-Region Deployment** – Wdrażanie usług w wielu regionach dla wysokiej dostępności.
* **Predictable Performance/Costs** – Przewidywalność wydajności i kosztów (np. rezerwacje, autoskalowanie).
* **Economies of Scale** – Niższe koszty dzięki dużej skali działania chmury.
* **On-demand self-service** – Możliwość samodzielnego uruchamiania i zarządzania zasobami bez kontaktu z dostawcą.
* **Rapid elasticity** – Szybkie zwiększanie lub zmniejszanie zasobów w odpowiedzi na zmieniające się potrzeby.
* **Measured Service** – Automatyczne monitorowanie i rozliczanie zużycia zasobów.
* **Resource pooling** – Wspólne zasoby dla wielu klientów (multi-tenancy).
* **Cloud Bursting** – Przenoszenie obciążenia z lokalnej infrastruktury do chmury w razie potrzeby.

## **2. Modele usług chmurowych**
* **IaaS** – Wynajmujesz infrastrukturę (VM, sieci, storage). Pełna kontrola.
* **PaaS** – Platforma do uruchamiania aplikacji (App Service, SQL DB). Bez zarządzania OS.
* **SaaS** – Gotowe aplikacje (Office 365, Gmail).
* **Serverless** – Kod bez zarządzania serwerami (Azure Functions).
* **FaaS (Function as a Service)** – Kod uruchamiany na żądanie, np. Azure Functions.

## **3. Modele odpowiedzialności**
* **Shared Responsibility** – Microsoft: infrastruktura, Ty: dane, dostęp.
* **Responsibility Matrix:**
    * **IaaS:** Microsoft odpowiada za sprzęt, sieć, Ty za OS, aplikacje, dane.
    * **PaaS:** Microsoft odpowiada za sprzęt, sieć, OS, Ty za aplikacje, dane.
    * **SaaS:** Microsoft odpowiada za wszystko poza danymi i dostępem użytkownika.

## **4. Hierarchia zasobów i organizacja**
* **Management Group > Subscription > Resource Group > Resource** – administracyjna struktura Azure.
    * **Resource** – pojedynczy zasób (np. VM, Storage, DB). Można przypisać **RBAC** bezpośrednio do zasobu.
* **RBAC** – Role i uprawnienia na różnych poziomach (dziedziczenie w dół).
* **Tagi** – Etykiety do organizacji i raportowania kosztów (max 50/tagów na zasób, klucz 128 znaków, wartość 256, case-sensitive, nie dziedziczą się).
* **Resource Locks** – Blokady przed usunięciem/zmianą:
    * **CanNotDelete:** nie można usunąć, można modyfikować.
    * **ReadOnly:** tylko odczyt.
* **Subscription** – Jednostka rozliczeniowa i granica uprawnień.
* **Management Group** – Grupowanie subskrypcji do centralnego zarządzania.
* **Azure Lighthouse** – Zarządzanie wieloma klientami (MSP).
* **Azure Arc** – Zarządzanie zasobami spoza Azure (multi-cloud, on-premises).

## **5. Kluczowe pojęcia Azure**
* **Directory** – katalog użytkowników, grup i aplikacji w Entra ID (dawniej Azure AD); techniczny kontener tożsamości, często używany zamiennie z Tenant.
* **Account** – konto użytkownika w Directory/Tenancie, może mieć wiele subskrypcji.
* **User** – pojedynczy użytkownik w Directory.
* **Group** – grupa użytkowników do zarządzania uprawnieniami.
* **Role** – zestaw uprawnień przypisanych użytkownikowi lub grupie (RBAC).
* **Tenant (dzierżawa)** – dedykowany, izolowany katalog Entra ID (dawniej Azure AD) dla jednej organizacji. W Tenantcie zarządza się użytkownikami, grupami, uprawnieniami i tożsamościami. Każda organizacja ma swój własny Tenant.
* **Tenant ID** – unikalny identyfikator (GUID) przypisany do danego Tenanta. Wykorzystywany np. przy integracjach, logowaniu aplikacji, automatyzacji.
* **Scope** – poziom przypisywania uprawnień i polityk (Management Group → Subscription → Resource Group → Resource).
* **Region** – Lokalizacja centrum danych.
* **Availability Zone (AZ)** – Niezależne strefy w regionie, zwiększają odporność.
    * Każda strefa ma własny zasilanie, chłodzenie, sieć. Umożliwia wdrożenie usług o wysokiej dostępności (np. VM w różnych AZ).
    * Przykład: VM w 2 AZ = odporność na awarię centrum danych.
    * Nie wszystkie regiony mają AZ.
* **Availability Set** – Grupa VM w jednym DC, rozkład na domeny awarii/aktualizacji. To nie to samo co AZ (fizyczne strefy). SLA 99,95% tylko przy 2+ VM w Availability Set.
* **Fault Domain** – logiczna grupa VM z tym samym zasilaniem i siecią; awaria FD nie wpływa na inne FD.
* **Region vs Recommended Region:**
    * **Region:** dowolna lokalizacja Azure (np. West Europe, North Europe).
    * **Recommended Region:** region sugerowany przez Microsoft – ma najnowsze usługi, wyższe SLA, dostępność AZ, lepsze wsparcie, niższe opóźnienia. Nie wszystkie funkcje są dostępne w każdym regionie!
    * **Alternate Region:** region uzupełniający, zapewnia DR, nie zawsze obsługuje AZ.
* **Authentication (AuthN):** weryfikacja tożsamości użytkownika (czy jesteś tym, za kogo się podajesz).
* **Authorization (AuthZ):** nadawanie uprawnień (co możesz zrobić po zalogowaniu).
* **Region Pair** – Dwa powiązane regiony do DR.
    * Aktualizacje i awarie planowane są rozdzielane między regiony w parze.
    * Dane replikowane asynchronicznie.
* **Resource Group** – Logicza grupa zasobów.
    * Można przenosić zasoby między grupami.
    * Usuwając Resource Group usuwasz wszystkie zasoby w niej.
* **GUID** – globalnie unikalny identyfikator (np. Tenant ID, Subscription ID).
* **ARM/Bicep** – Automatyzacja wdrożeń (Infrastructure as Code).
    * ARM = JSON, Bicep = prostsza składnia.
    * Pozwala na powtarzalne, automatyczne wdrożenia.
* **Azure Free Account** – Darmowe środki na start ($200, 12 miesięcy).
    * Po wykorzystaniu środków konto przechodzi na Pay-as-you-go lub jest blokowane.
* **Azure Active Directory Domain Services (AAD DS)** – Zarządzane usługi domenowe w chmurze.
    * Umożliwia korzystanie z klasycznych funkcji AD bez serwerów on-premises.
* **Resource ID** – Unikalny identyfikator zasobu w Azure.
    * Format: /subscriptions/{id}/resourceGroups/{rg}/providers/{type}/{name}
* **Service Principal** – Tożsamość aplikacji do automatyzacji i integracji.
    * Używany w automatyzacji, CI/CD, skryptach.
* **App Registration** – rejestracja aplikacji w Entra ID, umożliwia nadanie jej tożsamości i uprawnień.
* **Azure Resource Manager (ARM)** – Centralny system zarządzania zasobami Azure.
    * Pozwala na zarządzanie uprawnieniami, tagami, politykami na poziomie subskrypcji i grupy.
* **Azure Marketplace** – Sklep z gotowymi rozwiązaniami, aplikacjami i usługami.
    * Możliwość wdrożenia VM, SaaS, rozwiązań partnerów jednym kliknięciem.
* **Azure Subscription Types:**
    * **Free**, **Pay-as-you-go**, **Enterprise Agreement**, **CSP**.
    * CSP = Cloud Solution Provider (partnerzy Microsoft).

## **6. Compute (obliczenia)**
* **Virtual Machine (VM)** – Maszyna wirtualna, pełna kontrola.
* **Scale Set** – Automatyczne skalowanie wielu VM.
* **App Service** – Hostowanie aplikacji web/API bez zarządzania serwerem.
* **AKS** – Orkiestracja kontenerów (Kubernetes).
* **Container Instances (ACI)** – Szybkie uruchamianie pojedynczych kontenerów.
* **Azure Functions** – Serverless, kod uruchamiany na żądanie.
* **Service Fabric** – Orkiestracja mikroserwisów.
* **Dedicated Host** – Fizyczny serwer dedykowany dla jednej organizacji.
* **VMSS (Virtual Machine Scale Set)** – Automatyczne skalowanie VM.
* **Burstable VM** – VM z elastycznym zużyciem CPU.
* **Ephemeral Disk** – Tymczasowy dysk, tracony po restarcie VM.
* **Spot VM** – VM o niższym koszcie, może być przerwana przez Azure.
* **Custom Images** – Własne obrazy VM do szybkiego wdrożenia.
* **VM Extensions** – Automatyzacja konfiguracji VM (np. instalacja agentów).

## **7. Storage (magazynowanie)**
* **Blob Storage** – Przechowywanie plików/obiektów.
* **File Storage** – Udziały plików SMB/NFS.
* **Disk Storage** – Dyski blokowe dla VM.
* **Table Storage** – NoSQL, dane strukturalne.

**Szybka tabela porównawcza typów storage:**

| Typ           | Przeznaczenie                        | Protokół/API     |
|---------------|--------------------------------------|------------------|
| Blob          | Obiekty, backupy, archiwa            | REST API         |
| File          | Udziały plików, współdzielenie       | SMB/NFS          |
| Disk          | Dyski VM, block-level                | SCSI, SATA       |
| Table         | NoSQL, dane strukturalne             | REST API         |
* **Queue/Table Storage** – Kolejki i tabele NoSQL.
* **Disk Storage** – Dyski dla VM (Standard HDD/SSD, Premium SSD, Ultra SSD).
* **Data Lake Storage** – Analiza dużych zbiorów danych.
* **Premium Storage** – Dyski SSD dla wysokiej wydajności.
* **Azure File Sync** – Synchronizacja plików między on-premises a Azure Files.
* **Data Box Gateway** – Wirtualne urządzenie do transferu danych do chmury.
* **Redundancy (nadmiarowość):**
    * **LRS** – 3 kopie w jednym regionie (1 DC).
    * **ZRS** – 3 kopie w różnych strefach (AZ) w regionie.
    * **GRS** – 3 kopie w regionie głównym + 3 kopie w regionie zapasowym (łącznie 6).
    * **RA-GRS** – jak GRS, ale umożliwia odczyt z regionu zapasowego (6 kopii).
    * **GZRS/RA-GZRS** – 3 kopie w różnych strefach w regionie + 3 kopie w regionie zapasowym (łącznie 6).
    * **RA-GRS** – GRS + odczyt w regionie zapasowym.
    * **GZRS/RA-GZRS** – ZRS + geo-replikacja.
* **Blob Tiers:**
    * **Hot** – Częsty dostęp, wyższy koszt.
    * **Cool** – Rzadki dostęp, niższy koszt.
    * **Archive** – Archiwizacja, najniższy koszt, długi czas przywracania.
* **Geo-replikacja** – Automatyczne kopiowanie danych do innego regionu.
* **Koszty transferu danych:**
    * Transfer danych do Azure (ingress) – bezpłatny.
    * Transfer w tej samej AZ/regionie – bezpłatny.
    * Transfer między regionami (egress) – płatny.
    * Transfer z Azure do internetu (egress) – płatny.
* **Blob Storage vs File Storage:**
    * **Blob:** obiekty, backupy, archiwa, REST API.
    * **File:** udziały plików, SMB/NFS, współdzielenie między VM.

## **8. Networking (sieci)**
* **VNet** – Prywatna sieć w Azure.
* **Subnet** – Podział VNet.
* **NSG** – Reguły ruchu sieciowego.
    * Można przypisać tylko do subnetów i interfejsów VM (NIC), **nie można przypisać do całej Virtual Network!** (częsta pułapka egzaminacyjna)
* **Azure Firewall** – Zaawansowana ochrona, stateful.
* **DDoS Protection** – Ochrona przed atakami.
* **ExpressRoute** – Dedykowane łącze do Azure.
    * Oferuje wyższą przepustowość i niższe opóźnienia niż VPN.
    * Może być połączony z **MPLS** (**Multi-Protocol Label Switching** – technologia sieciowa do wydajnego, prywatnego routingu w sieciach WAN, często wykorzystywana przez firmy do łączenia oddziałów i centrów danych).
* **VPN Gateway** – Szyfrowane połączenie do Azure.
    * Obsługuje połączenia Site-to-Site, Point-to-Site, VNet-to-VNet.
* **Site-to-Site VPN** – Łączy całą sieć lokalną z siecią w Azure. Stosowane do stałego, bezpiecznego połączenia między firmą a chmurą (np. oddziały, biura).
* **Point-to-Site VPN** – Łączy pojedyncze urządzenie (np. laptop) z siecią w Azure. Stosowane do zdalnego dostępu użytkowników, np. home office, praca zdalna.
* **Public VPN** – Połączenie przez publiczny internet, mniej bezpieczne, stosowane do szybkiego, tymczasowego dostępu lub gdy nie ma dedykowanego łącza.
* **Private VPN** – Połączenie przez dedykowane, prywatne łącze (np. ExpressRoute), wyższy poziom bezpieczeństwa, stosowane do stałego, bezpiecznego połączenia między sieciami.
* **Private Link/Endpoint** – Prywatny dostęp do usług.
* **Bastion** – Bezpieczny dostęp do VM przez przeglądarkę.
    * Nie wymaga publicznego IP na VM.
* **Load Balancer** – Rozkładanie ruchu (L4).
    * Obsługuje TCP/UDP, nie analizuje ruchu aplikacji.
* **Application Gateway** – Rozkładanie ruchu (L7), WAF.
    * Obsługuje SSL offload, cookie-based affinity, WAF.
* **Front Door** – Globalny rozkład ruchu, CDN, WAF.
    * Optymalizuje dostępność i wydajność aplikacji globalnych.
* **Traffic Manager** – Rozkład ruchu na poziomie DNS.
    * Obsługuje routing geograficzny, failover, weighted.
* **Azure DNS** – Zarządzanie rekordami DNS.
    * Umożliwia hostowanie i zarządzanie rekordami DNS domen bez potrzeby własnych serwerów DNS.
    * Obsługuje typowe rekordy DNS (A, AAAA, CNAME, MX, TXT, NS, SRV).
    * Wymaga delegacji domeny do serwerów DNS Azure.
* **Content Delivery Network (CDN)** – Szybkie dostarczanie treści przez cache na całym świecie. Przydatne do: statycznych plików, streamingu wideo/audio, gier online, aktualizacji, ochrony przed DDoS. Skraca czas ładowania i odciąża serwer.
* **Peering** – Łączenie VNetów w tym samym lub różnych regionach.
    * Nie wymaga routingu przez internet.
* **Application Security Group (ASG)** – Grupowanie VM dla reguł NSG.
    * Ułatwia zarządzanie regułami dla wielu VM.
    * Dotyczy tylko VM i ich interfejsów sieciowych (nie innych zasobów).
* **Public IP vs Private IP** – Publiczny adres dostępny z internetu, prywatny tylko w sieci Azure.
* **Service Endpoint** – Umożliwia dostęp do usług Azure z prywatnej sieci bez publicznego IP.
* **Firewall Rules** – Reguły bezpieczeństwa dla baz danych i storage.
* **Route Table** – Definiuje trasowanie ruchu w sieci.
* **UDR (User Defined Route)** – Niestandardowe trasy dla ruchu sieciowego.

## **9. Identity & Security (tożsamość i bezpieczeństwo)**
* **Entra ID (Azure AD)** – Zarządzanie tożsamością.
* **MFA** – Uwierzytelnianie wieloskładnikowe.
* **Conditional Access** – Dostęp zależny od warunków.
* **PIM** – Zarządzanie uprawnieniami uprzywilejowanymi.
* **Managed Identity** – Tożsamość dla aplikacji bez haseł.
* **Key Vault** – Bezpieczne przechowywanie sekretów.
* **Zero Trust** – Zasada „nikomu nie ufaj”.
* **Defender for Cloud (dawniej Security Center)** – Centrum zarządzania bezpieczeństwem w chmurze. Monitoruje konfigurację, wykrywa zagrożenia, ocenia zgodność, daje rekomendacje i automatyzuje ochronę zasobów (VM, bazy, storage, kontenery, PaaS). Pozwala włączyć zaawansowane funkcje ochrony (Defender plans) dla różnych typów zasobów.
* **Defender for Identity** – Chmurowa usługa bezpieczeństwa do ochrony tożsamości w środowiskach hybrydowych (on-premises AD + Entra ID). Wykrywa podejrzane działania, ataki na konta, lateral movement, pass-the-hash/ticket, brute force, nieautoryzowane zmiany. Integruje się z Defender for Cloud i Sentinel. **Instaluje sensor na kontrolerze domeny i analizuje ruch oraz logi.**
* **Azure Information Protection** – Klasyfikacja i ochrona dokumentów.
* **Sentinel** – Narzędzie do monitorowania bezpieczeństwa w chmurze. Wykrywa zagrożenia, analizuje incydenty i automatyzuje reakcje, integrując różne źródła danych.
    * **SIEM** – Zbiera i analizuje logi, wykrywa zagrożenia.
    * **SOAR** – Automatyzuje reakcje na incydenty i procesy bezpieczeństwa.
* **Conditional Access Policy** – Reguły dostępu zależne od kontekstu (np. lokalizacja, urządzenie, MFA). 
    * Np. blokada spoza kraju, wymóg MFA poza biurem.
* **Privileged Access Workstation (PAW)** – Bezpieczne stanowisko do administrowania.
* **Just-in-Time (JIT) Access** – Tymczasowy dostęp do VM.
* **Managed HSM** – Zarządzany sprzętowy moduł bezpieczeństwa.

## **10. Bazy danych**
* **Azure SQL Database** – Relacyjna baza danych (OLTP).
* **Azure Database for MySQL/PostgreSQL** – Zarządzane bazy open source.
* **Azure Synapse Analytics** – Analityka, hurtownie danych (OLAP).
* **Cosmos DB** – NoSQL, globalna replikacja, niskie opóźnienia.
* **Azure Cache for Redis** – Baza in-memory, cache.
* Przykład:
    * **OLTP:** Azure SQL Database, MySQL, PostgreSQL
    * **OLAP:** Synapse Analytics
    * **NoSQL:** Cosmos DB
    * **In-memory:** Redis
* **Azure Database Migration Service** – Migracja baz danych do Azure.
* **Geo-Replication** – Replikacja bazy do innego regionu.
* **Elastic Pool** – Wspólna pula zasobów dla wielu baz SQL.
* **Hyperscale** – Skalowalna architektura dla Azure SQL.
* **Backup Retention** – Okres przechowywania kopii zapasowych.
* **Read Replica** – Odczyt z kopii bazy dla skalowania.

## **11. Koszty i narzędzia**
### **Modele rozliczania usług (billing)**
- **Nie wszystkie usługi Azure są rozliczane za sekundę!**
    - **VM (Virtual Machines):** Rozliczane za każdą rozpoczętą sekundę (per-second billing, większość VM). Naliczanie od uruchomienia do deallocation (wyłączenie VM = koniec naliczania, samo zatrzymanie nie zawsze!).
    - **Azure Functions:** Rozliczane za liczbę wywołań i czas działania (co 1 ms).
    - **Storage, bazy danych, App Service:** Zwykle rozliczane za godzinę lub za zużycie (np. GB/miesiąc).
    - **Rezerwacje, App Service Plan:** Rozliczane za godzinę lub miesiąc.
- **Wskazówka:** Sprawdzaj model billingowy każdej usługi w dokumentacji – na egzaminie mogą pojawić się pytania o dokładność rozliczania!
* **Pay-as-you-go** – Płacisz za zużycie.
* **Reserved Instances** – Rezerwacja na dłużej, taniej.
* **Spot VM** – Tanie, przerywane VM.
* **OpEx (Operating Expenditure):** model operacyjny, płacisz za bieżące zużycie (np. Pay-as-you-go).
* **CapEx (Capital Expenditure):** wydatki inwestycyjne, płatność z góry (np. rezerwacje, sprzęt on-premises).
* **Pricing Calculator** – Szacowanie kosztów.
* **TCO Calculator** – Porównanie kosztów chmura vs. on-premises.
* **Budżety i alerty** – Kontrola wydatków.
* **Cost Management** – Analiza i optymalizacja kosztów.
* **Marketplace** – Gotowe rozwiązania partnerów.
* **Azure Reservations** – Rezerwacja zasobów na 1/3 lata dla niższych kosztów.
* **Azure Hybrid Benefit** – Oszczędność kosztów licencji Windows Server/SQL.
* **Cost Analysis** – Analiza wydatków w Azure Cost Management.
* **Azure Budgets** – Ustawianie limitów wydatków i alertów.
* **Cost Alerts** – Powiadomienia o przekroczeniu budżetu.
* **Azure Price List API** – Automatyczne pobieranie cen usług.
* **Azure Advisor Recommendations** – Sugestie oszczędności i optymalizacji.
* **Azure Cost Management + Billing** – Narzędzie do monitorowania, analizowania i raportowania kosztów.

## **12. Governance & Compliance**
* **Azure Policy** – Wymuszanie zasad (np. lokalizacja, typy maszyn, wymagane tagi) na poziomie subskrypcji, grupy zasobów lub zasobu.
* **Initiative** – Grupa polityk.
* **Blueprints** – Szablony wdrożeń zgodnych z politykami firmy.
* Przykład Azure Policy: wymuszanie tagów, regionów, typów maszyn.
* **Resource Locks** – Blokady przed usunięciem.
* **Compliance** – Zgodność (ISO, GDPR, NIST, PCI-DSS, HIPAA).
* **Cloud Adoption Framework** – Dobre praktyki wdrożenia chmury.
* **Purview** – Zarządzanie danymi i zgodnością.
* **Management Locks** – Blokady na poziomie subskrypcji, grupy lub zasobu.
* **Policy Assignment** – Przypisywanie polityk na różnych poziomach hierarchii.
* **Compliance Manager** – Narzędzie do zarządzania zgodnością i audytami.
* **Azure Trust Center** – Informacje o certyfikatach, zgodności i bezpieczeństwie Azure.
* **Policy Definition** – Tworzenie własnych zasad w Azure Policy.
* **Audit Logs** – Rejestr zdarzeń i zmian w zasobach.
* **Data Residency** – Lokalizacja przechowywania danych.

## **13. Narzędzia zarządzania, migracji i automatyzacji**
* **Portal** – Interfejs webowy.
* **PowerShell/CLI** – Automatyzacja i zarządzanie.
* **Azure Portal, CLI, PowerShell** – dostępne na Windows, Linux, macOS.
* **Cloud Shell** – Terminal w przeglądarce.
* **ARM Templates/Bicep** – Infrastructure as Code.
* **IaC (Infrastructure as Code)** – zarządzanie i wdrażanie infrastruktury za pomocą kodu (np. ARM, Bicep, Terraform). Pozwala na automatyzację, powtarzalność i wersjonowanie środowisk.
* **Lift and Shift (IaaS)** – Migracja aplikacji do chmury bez zmian w architekturze, najczęściej na maszyny wirtualne (model IaaS).
* **DevTest Labs** – Szybkie środowiska testowe.
* **Azure Monitor** – Monitorowanie zasobów.
    * **Azure Monitor** – centralne narzędzie do zbierania, korelacji i analizy logów, metryk i śladów z VM, aplikacji i innych zasobów Azure.
* **Log Analytics** – Analiza logów.
* **Service Health** – Powiadomienia o awariach.
* **Advisor** – Rekomendacje optymalizacyjne.
* **Azure Migrate** – Narzędzia do oceny, planowania i migracji serwerów, baz danych, aplikacji do Azure.
* **Data Box** – Fizyczne urządzenia do migracji dużych ilości danych do Azure (np. Data Box Disk, Data Box Heavy). Zamawiasz urządzenie, kopiujesz dane, odsyłasz do Microsoft.
* **Azure DevOps** – CI/CD, repozytoria kodu, tablice zadań, testy.
* **Application Insights** – Monitorowanie aplikacji (część Azure Monitor).
* **Azure Automation** – Automatyzacja zadań administracyjnych (runbooki).
* **Update Management** – Zarządzanie aktualizacjami VM.
* **Change Tracking** – Monitorowanie zmian konfiguracji.
* **Blueprint Assignment** – Przypisywanie szablonów wdrożeniowych.
* **Azure Site Recovery** – Disaster Recovery dla VM i aplikacji.
* **Azure Backup** – Automatyczne kopie zapasowe VM, plików, baz danych.
* **Azure Resource Explorer** – Narzędzie do przeglądania zasobów i API.
* **Azure REST API** – Programistyczny dostęp do zarządzania zasobami.

## **14. Usługi rozszerzone**
* **Service Bus** – Kolejki, pub/sub.
* **Event Hub** – Streaming danych.
* **Event Grid** – Routing zdarzeń.
* **API Management** – Zarządzanie API.
* **Logic Apps** – Automatyzacja procesów.
* **Data Factory** – Integracja i przetwarzanie danych.
* **Cognitive Services** – AI, rozpoznawanie obrazów, tekstu.
* **IoT Hub** – Obsługa urządzeń IoT.
* **Azure Virtual Desktop** – Pulpity w chmurze.
* **Data Box** – Transfer dużych danych (również offline).
* Logic Apps vs Functions:
    * **Logic Apps:** automatyzacja procesów biznesowych, integracje bez kodu.
    * **Functions:** event-driven, własny kod, automatyczne skalowanie.
* Service Bus vs Event Hub vs Event Grid:
    * **Service Bus:** kolejki, pub/sub, integracja systemów.
    * **Event Hub:** streaming telemetry/big data.
    * **Event Grid:** routing zdarzeń między usługami.
* **Azure API Apps** – Hostowanie i zarządzanie API.
* **Azure Data Lake Analytics** – Analiza dużych zbiorów danych.
* **Azure Purview** – Katalogowanie i klasyfikacja danych.
* **Azure Sentinel** – Integracja z innymi narzędziami SIEM/SOAR.
* **Azure VMware Solution** – Uruchamianie środowisk VMware w Azure.

## **16. Praktyczne wskazówki i porównania**
* **Zwracaj uwagę na różnice między usługami** (np. **NSG vs Firewall**, **CDN vs Front Door**).
* **W pytaniach egzaminacyjnych często liczy się szczegół** (np. czy usługa jest **PaaS** czy **IaaS**, czy ruch jest szyfrowany, czy usługa ma **SLA 99,9%** czy **99,99%**).
* **Praktyczne narzędzia:** **Azure Advisor** (rekomendacje), **Service Health** (awarie), **Cost Management** (koszty), **Azure Migrate/Data Box** (migracje), **Sentinel** (SIEM/SOAR), **Purview** (governance danych).
* **App Service vs VM:** **App Service** = **PaaS**, szybkie wdrożenia, automatyczne skalowanie; **VM** = **IaaS**, pełna kontrola, własny OS.
* **Site-to-Site VPN:** Łączy sieć on-premises z Azure przez tunel IPsec/IKE (IKEv1/IKEv2) za pomocą VPN Gateway. Wymaga urządzenia VPN z publicznym IP po stronie lokalnej.
* **Role IAM (RBAC) – szybka tabela:**

| Rola                | Uprawnienia                                                        |
|---------------------|--------------------------------------------------------------------|
| Owner               | Pełny dostęp + zarządzanie uprawnieniami (może nadawać role)        |
| Contributor         | Pełny dostęp do zarządzania zasobami, NIE może nadawać ról          |
| Reader              | Tylko odczyt wszystkich zasobów                                    |
| VM Contributor      | Zarządzanie VM, bez dostępu do sieci/storage, nie może nadawać ról  |

* **Blob Storage vs File Storage:** **Blob** = obiekty, **REST API**; **File** = udziały plików, **SMB/NFS**.
    * **SMB (Server Message Block):** protokół udostępniania plików w sieciach Windows.
    * **NFS (Network File System):** protokół udostępniania plików w sieciach Linux/Unix.
* **NSG vs Firewall:** **NSG** = reguły na subnet/VM, **Firewall** = centralna ochrona, logi.
* **ExpressRoute vs VPN:** **ExpressRoute** = dedykowane, **VPN** = szyfrowane przez internet.
* **CDN vs Front Door vs Traffic Manager:** **CDN** = cache, **Front Door** = globalny routing, **Traffic Manager** = DNS.
* **Service Bus vs Event Hub vs Event Grid:** **Service Bus** = kolejki, **Event Hub** = streaming, **Event Grid** = routing zdarzeń.
* **Logic Apps vs Functions:** **Logic Apps** = automatyzacja bez kodu, **Functions** = własny kod, event-driven.
* **Azure Policy vs Blueprint:** **Policy** = pojedyncze zasady, **Blueprint** = zestaw polityk, ról i szablonów.
* **Azure Monitor vs Application Insights:** **Monitor** = cała platforma monitoringu, **Insights** = szczegółowe monitorowanie aplikacji.
* **Azure Advisor vs Cost Management:** **Advisor** = rekomendacje, **Cost Management** = analiza i optymalizacja kosztów.
* **Azure Backup vs Site Recovery:** Backup = kopie zapasowe, Site Recovery = DR, odtwarzanie po awarii.
* **Azure Policy vs RBAC:** Policy = zasady, RBAC = uprawnienia.
* **Azure AD vs AAD DS:** AD = klasyczna domena, AAD DS = zarządzane usługi domenowe w chmurze.
* **Azure Monitor vs Log Analytics:** Monitor = platforma, Log Analytics = analiza logów.
* **Azure DevOps vs GitHub:** DevOps = CI/CD, tablice zadań, GitHub = repozytoria, CI/CD, społeczność.
* **Tip:** Zwracaj uwagę na typy subskrypcji, poziomy SLA, regiony, typy storage, różnice w modelach usług.

---

## **15. Pułapki egzaminacyjne (trick questions)**

---
### **Dodatkowe typowe pułapki i pytania egzaminacyjne:**
* **Preview vs GA vs SLA:**
    * **Private preview:** brak formalnego wsparcia, tylko wybrani klienci, brak SLA.
    * **Public preview:** dostępne dla wszystkich, wsparcie przez Microsoft Customer Support, ale nie obowiązuje SLA.
    * **GA (General Availability):** pełne wsparcie, obowiązuje SLA.
    * **Na egzaminie:** usługi w preview (private/public) nie mają SLA, tylko GA ma gwarantowany poziom dostępności.
* **Support Plans:** Tylko plany **Basic**, **Developer**, **Standard**, **Professional Direct** pozwalają otwierać zgłoszenia serwisowe. **Free** nie umożliwia otwierania ticketów.
* **Support Plan Review:** Tylko **Professional Direct** i wyższe umożliwiają architektoniczny przegląd środowiska przez Microsoft.
* **Redundancja Storage:** Jeśli wymagany jest odczyt z regionu zapasowego, wybierz **RA-GRS** lub **RA-GZRS** (nie samo **GRS/GZRS**).
* **Modele usług:** **VM = IaaS**, **App Service = PaaS**, **Office 365 = SaaS**. **VM** nie jest **PaaS/SaaS**.
* **Web App Plan:** Własna domena, **SSL**, dedykowane instancje, min. plan **Basic/Standard** (**Free/Shared** nie obsługują tych funkcji).
* **Podział administracyjny:** Oddziały firmy = osobne **Subscriptions** lub **Resource Groups**, NIE osobne **Azure AD Tenants** (to osobne katalogi, nie podział administracyjny).
* **Expenditure Model:** **Pay-as-you-go** = model operacyjny (**OpEx**), nie **CapEx**.
* **Geo-replikacja:** Dane muszą być przechowywane na wielu węzłach i w różnych lokalizacjach = **GRS/RA-GRS** lub **GZRS/RA-GZRS**.
* **SLA:** **Preview** (wersja testowa) nie ma gwarantowanego **SLA**.
* **Load Balancing:** **Azure Load Balancer = L4**, **Application Gateway/Front Door = L7**.
* **Azure Policy:** Wymusza zgodność, ale nie blokuje fizycznie wdrożenia (może tylko oznaczyć **non-compliant**).
* **Azure Marketplace:** Służy do wdrażania gotowych rozwiązań partnerów, nie do zarządzania kosztami.
* **Azure Hybrid Benefit:** Pozwala użyć własnych licencji **Windows Server/SQL** w Azure, obniżając koszty.
* **Azure Reservations:** Rezerwacja **VM** na 1/3 lata = niższy koszt, ale mniejsza elastyczność.
* **Azure Advisor:** Daje rekomendacje, ale nie wymusza zmian.
* **Azure Migrate:** Służy do oceny i migracji serwerów, baz, aplikacji do Azure.
* **Azure DevTest Labs:** Szybkie środowiska testowe, nie produkcyjne.
* **Azure Arc:** Zarządzanie zasobami spoza Azure (**on-premises**, **multi-cloud**).
* **Azure Lighthouse:** Zarządzanie wieloma środowiskami klientów przez partnerów/**MSP**.
* **Azure Bastion:** Bezpieczny dostęp do **VM** przez przeglądarkę, bez publicznego IP na VM.
* **ExpressRoute:** Łącze dedykowane, nie szyfruje ruchu domyślnie (**VPN** szyfruje).
* **Azure Free Account:** $200 na 30 dni + wybrane usługi na 12 miesięcy za darmo.
* **VPN** szyfruje, **ExpressRoute** nie domyślnie.
* **Azure Firewall** to **stateful firewall**, **NSG** nie.
* **DDoS Standard** chroni **L3/L4**, nie **L7**.
* **WAF** (Web Application Firewall) chroni **HTTP(S)**, np. **OWASP Top 10**; działa na Application Gateway i Front Door.
* **Private Endpoint** daje prywatny IP, **Service Endpoint** nie.
* **Front Door** działa globalnie na edge (**L7**), **Traffic Manager** to **DNS**.
* **Load Balancer** działa na **L4**, **Application Gateway** na **L7** (z **WAF**).
* **Azure Functions** = event-driven serverless, płatność za wykonania (**Consumption**).
* **Durable Functions** = workflow ze stanem.
* **ACI** = szybkie uruchomienie kontenera bez orkiestracji, **ACA** = mikroserwisy, scale-to-zero.
* **AKS** = pełna orkiestracja **Kubernetes**.
* **PaaS** zmniejsza odpowiedzialność za **OS** i patchowanie, **IaaS** = samodzielny **OS**.
* **Entra ID** to **IAM**, nie klasyczny **AD Domain Services**.
* **Conditional Access** ocenia kontekst (**user/device/location/risk**).
* **PIM** (**Privileged Identity Management**) daje **JIT/JEA** dla ról uprzywilejowanych.
* **Managed Identity** eliminuje potrzebę trzymania sekretów w kodzie.
* **Blob tiers:**
    * **Hot:** Najwyższy koszt przechowywania, najniższy koszt dostępu. Brak minimalnego okresu przechowywania. Do częstego dostępu (produkcyjne dane, backupy do szybkiego odtworzenia).
    * **Cool:** Niższy koszt przechowywania, wyższy koszt dostępu niż Hot. Minimalny okres przechowywania: 30 dni. Do rzadziej używanych danych (backupy miesięczne, archiwa, dane do analizy).
    * **Archive:** Najniższy koszt przechowywania, najwyższy koszt dostępu. Minimalny okres przechowywania: 180 dni (6 miesięcy). Do długoterminowej archiwizacji (compliance, backupy wieloletnie). Czas przywracania: od kilku godzin do 24h (dane są offline, wymagają rehydratacji).
        * **Rehydratacja:** proces przywracania danych z warstwy Archive do warstwy Hot/Cool, aby można było je odczytać (dane są najpierw „odmrażane”).
    * **Podsumowanie:** Hot – dane produkcyjne, Cool – backupy/archiwa do 1 roku, Archive – archiwizacja na lata (np. 7-10 lat, zgodność z regulacjami).
* **LRS/ZRS/GRS/RA-GRS** to poziomy redundancji storage.
* **Cosmos DB** = globalny **NoSQL** z **multi-region** i niskimi opóźnieniami.
* **Preview ≠ GA** (brak pełnego **SLA**).
* **Azure AD B2C/B2B:** **B2C** = tożsamość klientów, **B2B** = partnerów.
* **Azure AD vs AAD DS:** **AD** = klasyczna domena, **AAD DS** = zarządzane usługi domenowe.
* **Azure Backup vs Site Recovery:** **Backup** = kopie zapasowe, **Site Recovery** = DR.
* **Azure Policy vs RBAC:** **Policy** = zasady, **RBAC** = uprawnienia.
* **Azure DevOps vs GitHub:** **DevOps** = CI/CD, tablice zadań, **GitHub** = repozytoria, CI/CD, społeczność.
* **Azure Monitor vs Log Analytics:** **Monitor** = platforma, **Log Analytics** = analiza logów.
* **Azure Spring Apps:** Hostowanie aplikacji Java.
* **Azure Machine Learning:** Budowa, trenowanie i wdrażanie modeli AI.
* **Azure Bot Service:** Tworzenie chatbotów.
* **Azure Media Services:** Przetwarzanie i streaming multimediów.
* **Azure Maps:** Usługi geolokalizacyjne.
* **Tip:** Zwracaj uwagę na typy subskrypcji, poziomy **SLA**, regiony, typy storage, różnice w modelach usług.








