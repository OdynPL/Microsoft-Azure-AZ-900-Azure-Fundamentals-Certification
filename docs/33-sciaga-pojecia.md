# **AZ-900 – Ściąga pojęć do druku (wersja rozszerzona)**

[⬅ Powrót do spisu treści](../README.md)

## **1. Podstawy chmury i korzyści**
**Cloud Computing** – Dostarczanie zasobów IT przez internet (serwery, bazy, aplikacje).
**Public Cloud** – Usługi dostępne dla każdego (np. Azure, AWS, GCP).
**Private Cloud** – Chmura tylko dla jednej organizacji.
**Hybrid Cloud** – Połączenie chmury publicznej i prywatnej.
**Elasticity** – Automatyczne dopasowanie zasobów do potrzeb.
**Scalability** – Możliwość zwiększania/zmniejszania zasobów.
    - **Vertical scaling** – zwiększanie mocy jednej maszyny (np. więcej CPU/RAM).
    - **Horizontal scaling** – dodawanie kolejnych maszyn/instancji (np. więcej serwerów pracujących równolegle).
**Agility** – Szybkie wdrażanie i zmiany.
**High Availability (HA)** – Minimalizacja przestojów, ciągłość działania.
**Disaster Recovery (DR)** – Odtwarzanie po awarii, kopie zapasowe.
**Fault Tolerance** – Odporność na awarie.
**Geo-distribution** – Rozmieszczenie usług w wielu regionach.
**CapEx/OpEx** – Wydatki inwestycyjne vs operacyjne.
**SLA (Service Level Agreement)** – Gwarantowany poziom dostępności usługi (np. 99,9%, 99,99%). Określa ile maksymalnie może trwać niedostępność usługi w miesiącu. Przekroczenie SLA = możliwość ubiegania się o kredyt/zwrot.
**RTO/RPO** – Recovery Time/Point Objective: maksymalny czas odtworzenia usługi/akceptowalna utrata danych po awarii.
**Soft Delete** – Ochrona przed przypadkowym usunięciem danych.
**Immutable Storage** – Niezmienialne kopie, np. backupy nie do usunięcia.
**Szyfrowanie at-rest/in-transit** – Dane domyślnie szyfrowane w Azure.
**Multi-Region Deployment** – Wdrażanie usług w wielu regionach dla wysokiej dostępności.
**Predictable Performance/Costs** – Przewidywalność wydajności i kosztów (np. rezerwacje, autoskalowanie).
**Economies of Scale** – Niższe koszty dzięki dużej skali działania chmury.

## **2. Modele usług chmurowych**
**IaaS** – Wynajmujesz infrastrukturę (VM, sieci, storage). Pełna kontrola.
**PaaS** – Platforma do uruchamiania aplikacji (App Service, SQL DB). Bez zarządzania OS.
**SaaS** – Gotowe aplikacje (Office 365, Gmail).
**Serverless** – Kod bez zarządzania serwerami (Azure Functions).
**FaaS (Function as a Service)** – Kod uruchamiany na żądanie, np. Azure Functions.

## **3. Modele odpowiedzialności**
**Shared Responsibility** – Microsoft: infrastruktura, Ty: dane, dostęp.

## **4. Hierarchia i organizacja zasobów**
**Management Group > Subscription > Resource Group > Resource** – Struktura administracyjna.
**RBAC** – Zarządzanie dostępem do zasobów (role, zakres).
**Tagi** – Etykiety do organizacji i raportowania kosztów (np. Environment, Owner, Project). Ułatwiają filtrowanie, raportowanie, automatyzację i rozliczanie kosztów. Tagi nie dziedziczą się automatycznie na zasoby podrzędne.
**Resource Locks** – Blokady przed usunięciem/zmianą zasobów.
    - **CanNotDelete (Delete Lock):** nie można usunąć zasobu, ale można go modyfikować.
    - **ReadOnly Lock:** nie można usuwać ani modyfikować zasobu, tylko odczyt.
**Subscription** – Jednostka rozliczeniowa, granica kosztów i uprawnień.
**Management Group** – Grupowanie subskrypcji dla centralnego zarządzania politykami.
**Azure Lighthouse** – Zarządzanie wieloma środowiskami klientów (MSP).
**Azure Arc** – Zarządzanie zasobami spoza Azure (multi-cloud, on-premises).

## **5. Kluczowe pojęcia Azure**
**Region** – Lokalizacja centrum danych.
**Availability Zone (AZ)** – Niezależne strefy w regionie, zwiększają odporność.
**Region Pair** – Dwa powiązane regiony do DR.
**Resource Group** – Logicza grupa zasobów.
**ARM/Bicep** – Automatyzacja wdrożeń (Infrastructure as Code).
**Azure Free Account** – Darmowe środki na start ($200, 12 miesięcy).
**Azure Active Directory Domain Services (AAD DS)** – Zarządzane usługi domenowe w chmurze.
**Resource ID** – Unikalny identyfikator zasobu w Azure.
**Service Principal** – Tożsamość aplikacji do automatyzacji i integracji.

## **6. Compute (obliczenia)**
**Virtual Machine (VM)** – Maszyna wirtualna, pełna kontrola.
**Scale Set** – Automatyczne skalowanie wielu VM.
**App Service** – Hostowanie aplikacji web/API bez zarządzania serwerem.
**AKS** – Orkiestracja kontenerów (Kubernetes).
**Container Instances (ACI)** – Szybkie uruchamianie pojedynczych kontenerów.
**Azure Functions** – Serverless, kod uruchamiany na żądanie.
**Service Fabric** – Orkiestracja mikroserwisów.
**Dedicated Host** – Fizyczny serwer dedykowany dla jednej organizacji.
**VMSS (Virtual Machine Scale Set)** – Automatyczne skalowanie VM.
**Burstable VM** – VM z elastycznym zużyciem CPU.

## **7. Storage (magazynowanie)**
**Blob Storage** – Przechowywanie plików/obiektów.
**File Storage** – Udziały plików SMB/NFS.
**Queue/Table Storage** – Kolejki i tabele NoSQL.
**Disk Storage** – Dyski dla VM (Standard HDD/SSD, Premium SSD, Ultra SSD).
**Data Lake Storage** – Analiza dużych zbiorów danych.
**Premium Storage** – Dyski SSD dla wysokiej wydajności.
**Azure File Sync** – Synchronizacja plików między on-premises a Azure Files.
**Data Box Gateway** – Wirtualne urządzenie do transferu danych do chmury.
**Redundancy (nadmiarowość):**
    - **LRS** – 3 kopie w jednym regionie.
    - **ZRS** – Kopie w różnych strefach w regionie.
    - **GRS** – Kopie w innym regionie.
    - **RA-GRS** – GRS + odczyt w regionie zapasowym.
    - **GZRS/RA-GZRS** – ZRS + geo-replikacja.
**Blob Tiers:**
    - **Hot** – Częsty dostęp, wyższy koszt.
    - **Cool** – Rzadki dostęp, niższy koszt.
    - **Archive** – Archiwizacja, najniższy koszt, długi czas przywracania.
**Geo-replikacja** – Automatyczne kopiowanie danych do innego regionu.
**Blob Storage vs File Storage:**
    - **Blob:** obiekty, backupy, archiwa, REST API.
    - **File:** udziały plików, SMB/NFS, współdzielenie między VM.

## **8. Networking (sieci)**
**VNet** – Prywatna sieć w Azure.
**Subnet** – Podział VNet.
**NSG** – Reguły ruchu sieciowego.
**Azure Firewall** – Zaawansowana ochrona, stateful.
**DDoS Protection** – Ochrona przed atakami.
**ExpressRoute** – Dedykowane łącze do Azure.
**VPN Gateway** – Szyfrowane połączenie do Azure.
**Private Link/Endpoint** – Prywatny dostęp do usług.
**Bastion** – Bezpieczny dostęp do VM przez przeglądarkę.
**Load Balancer** – Rozkładanie ruchu (L4).
**Application Gateway** – Rozkładanie ruchu (L7), WAF.
**Front Door** – Globalny rozkład ruchu, CDN, WAF.
**Traffic Manager** – Rozkład ruchu na poziomie DNS.
**Azure DNS** – Zarządzanie rekordami DNS.
**Content Delivery Network (CDN)** – Szybkie dostarczanie treści.
**Peering** – Łączenie VNetów w tym samym lub różnych regionach.
**Application Security Group (ASG)** – Grupowanie VM dla reguł NSG.
**Public IP vs Private IP** – Publiczny adres dostępny z internetu, prywatny tylko w sieci Azure.

## **9. Identity & Security (tożsamość i bezpieczeństwo)**
**Entra ID (Azure AD)** – Zarządzanie tożsamością.
**MFA** – Uwierzytelnianie wieloskładnikowe.
**Conditional Access** – Dostęp zależny od warunków.
**PIM** – Zarządzanie uprawnieniami uprzywilejowanymi.
**Managed Identity** – Tożsamość dla aplikacji bez haseł.
**Key Vault** – Bezpieczne przechowywanie sekretów.
**Zero Trust** – Zasada „nikomu nie ufaj”.
**Defender for Cloud (dawniej Security Center)** – Centrum zarządzania bezpieczeństwem.
**Azure Information Protection** – Klasyfikacja i ochrona dokumentów.
**Sentinel** – SIEM/SOAR, analiza zagrożeń, automatyzacja reakcji na incydenty.
**Conditional Access Policy** – Reguły dostępu zależne od kontekstu użytkownika.
**Privileged Access Workstation (PAW)** – Bezpieczne stanowisko do administrowania.
**Just-in-Time (JIT) Access** – Tymczasowy dostęp do VM.
**Managed HSM** – Zarządzany sprzętowy moduł bezpieczeństwa.

## **10. Bazy danych**
**Azure SQL Database** – Relacyjna baza danych (OLTP).
**Azure Database for MySQL/PostgreSQL** – Zarządzane bazy open source.
**Azure Synapse Analytics** – Analityka, hurtownie danych (OLAP).
**Cosmos DB** – NoSQL, globalna replikacja, niskie opóźnienia.
**Azure Cache for Redis** – Baza in-memory, cache.
**Przykład:**
    - **OLTP:** Azure SQL Database, MySQL, PostgreSQL
    - **OLAP:** Synapse Analytics
    - **NoSQL:** Cosmos DB
    - **In-memory:** Redis
**Azure Database Migration Service** – Migracja baz danych do Azure.
**Geo-Replication** – Replikacja bazy do innego regionu.

## **11. Koszty i narzędzia**
**Pay-as-you-go** – Płacisz za zużycie.
**Reserved Instances** – Rezerwacja na dłużej, taniej.
**Spot VM** – Tanie, przerywane VM.
**Pricing Calculator** – Szacowanie kosztów.
**TCO Calculator** – Porównanie kosztów chmura vs. on-premises.
**Budżety i alerty** – Kontrola wydatków.
**Cost Management** – Analiza i optymalizacja kosztów.
**Marketplace** – Gotowe rozwiązania partnerów.
**Azure Reservations** – Rezerwacja zasobów na 1/3 lata dla niższych kosztów.
**Azure Hybrid Benefit** – Oszczędność kosztów licencji Windows Server/SQL.
**Cost Analysis** – Analiza wydatków w Azure Cost Management.

## **12. Governance & Compliance**
**Azure Policy** – Wymuszanie zasad.
**Initiative** – Grupa polityk.
**Blueprints** – Szablony wdrożeń zgodnych z politykami firmy.
**Przykład Azure Policy:** wymuszanie tagów, regionów, typów maszyn.
**Resource Locks** – Blokady przed usunięciem.
**Compliance** – Zgodność (ISO, GDPR, NIST, PCI-DSS, HIPAA).
**Cloud Adoption Framework** – Dobre praktyki wdrożenia chmury.
**Purview** – Zarządzanie danymi i zgodnością.
**Management Locks** – Blokady na poziomie subskrypcji, grupy lub zasobu.
**Policy Assignment** – Przypisywanie polityk na różnych poziomach hierarchii.
**Compliance Manager** – Narzędzie do zarządzania zgodnością i audytami.

## **13. Narzędzia zarządzania, migracji i automatyzacji**
**Portal** – Interfejs webowy.
**PowerShell/CLI** – Automatyzacja i zarządzanie.
**Cloud Shell** – Terminal w przeglądarce.
**ARM Templates/Bicep** – Infrastructure as Code.
**DevTest Labs** – Szybkie środowiska testowe.
**Azure Monitor** – Monitorowanie zasobów.
**Log Analytics** – Analiza logów.
**Service Health** – Powiadomienia o awariach.
**Advisor** – Rekomendacje optymalizacyjne.
**Azure Migrate** – Narzędzia do oceny, planowania i migracji serwerów, baz danych, aplikacji do Azure.
**Data Box** – Fizyczne urządzenia do migracji dużych ilości danych do Azure (np. Data Box Disk, Data Box Heavy). Zamawiasz urządzenie, kopiujesz dane, odsyłasz do Microsoft.
**Azure DevOps** – CI/CD, repozytoria kodu, tablice zadań, testy.
**Application Insights** – Monitorowanie aplikacji (część Azure Monitor).
**Azure Automation** – Automatyzacja zadań administracyjnych (runbooki).
**Update Management** – Zarządzanie aktualizacjami VM.
**Change Tracking** – Monitorowanie zmian konfiguracji.
**Blueprint Assignment** – Przypisywanie szablonów wdrożeniowych.

## **14. Usługi rozszerzone**
**Service Bus** – Kolejki, pub/sub.
**Event Hub** – Streaming danych.
**Event Grid** – Routing zdarzeń.
**API Management** – Zarządzanie API.
**Logic Apps** – Automatyzacja procesów.
**Data Factory** – Integracja i przetwarzanie danych.
**Cognitive Services** – AI, rozpoznawanie obrazów, tekstu.
**IoT Hub** – Obsługa urządzeń IoT.
**Azure Virtual Desktop** – Pulpity w chmurze.
**Data Box** – Transfer dużych danych (również offline).
**Logic Apps vs Functions:**
    - **Logic Apps:** automatyzacja procesów biznesowych, integracje bez kodu.
    - **Functions:** event-driven, własny kod, automatyczne skalowanie.
**Service Bus vs Event Hub vs Event Grid:**
    - **Service Bus:** kolejki, pub/sub, integracja systemów.
    - **Event Hub:** streaming telemetry/big data.
    - **Event Grid:** routing zdarzeń między usługami.
**Azure API Apps** – Hostowanie i zarządzanie API.
**Azure Data Lake Analytics** – Analiza dużych zbiorów danych.
**Azure Purview** – Katalogowanie i klasyfikacja danych.
**Azure Sentinel** – Integracja z innymi narzędziami SIEM/SOAR.
**Azure VMware Solution** – Uruchamianie środowisk VMware w Azure.

## **16. Praktyczne wskazówki i porównania**
**Zwracaj uwagę na różnice między usługami** (np. **NSG vs Firewall**, **CDN vs Front Door**).
**W pytaniach egzaminacyjnych często liczy się szczegół** (np. czy usługa jest **PaaS** czy **IaaS**, czy ruch jest szyfrowany, czy usługa ma **SLA 99,9%** czy **99,99%**).
**Praktyczne narzędzia:** **Azure Advisor** (rekomendacje), **Service Health** (awarie), **Cost Management** (koszty), **Azure Migrate/Data Box** (migracje), **Sentinel** (SIEM/SOAR), **Purview** (governance danych).
**App Service vs VM:** **App Service** = **PaaS**, szybkie wdrożenia, automatyczne skalowanie; **VM** = **IaaS**, pełna kontrola, własny OS.
**Blob Storage vs File Storage:** **Blob** = obiekty, **REST API**; **File** = udziały plików, **SMB/NFS**.
**NSG vs Firewall:** **NSG** = reguły na subnet/VM, **Firewall** = centralna ochrona, logi.
**ExpressRoute vs VPN:** **ExpressRoute** = dedykowane, **VPN** = szyfrowane przez internet.
**CDN vs Front Door vs Traffic Manager:** **CDN** = cache, **Front Door** = globalny routing, **Traffic Manager** = DNS.
**Service Bus vs Event Hub vs Event Grid:** **Service Bus** = kolejki, **Event Hub** = streaming, **Event Grid** = routing zdarzeń.
**Logic Apps vs Functions:** **Logic Apps** = automatyzacja bez kodu, **Functions** = własny kod, event-driven.
**Azure Policy vs Blueprint:** **Policy** = pojedyncze zasady, **Blueprint** = zestaw polityk, ról i szablonów.
**Azure Monitor vs Application Insights:** **Monitor** = cała platforma monitoringu, **Insights** = szczegółowe monitorowanie aplikacji.
**Azure Advisor vs Cost Management:** **Advisor** = rekomendacje, **Cost Management** = analiza i optymalizacja kosztów.

---

## **15. Pułapki egzaminacyjne (trick questions)**

---
### **Dodatkowe typowe pułapki i pytania egzaminacyjne:**
**Support Plans:** Każdy plan (oprócz Free) pozwala otwierać zgłoszenia serwisowe, ale tylko wyższe (np. **Professional Direct**) umożliwiają architekturę review przez Microsoft.
**Redundancja Storage:** Jeśli wymagany jest odczyt z regionu zapasowego, wybierz **RA-GRS/RA-GZRS**, nie samo **GRS/GZRS**.
**Modele usług:** **VM = IaaS**, **App Service = PaaS**, **Office 365 = SaaS**. **VM** nie jest **PaaS/SaaS**.
**Web App Plan:** Wymagania: własna domena, **SSL**, dedykowane instancje, min. plan **Basic/Standard** (**Free/Shared** nie obsługują tych funkcji).
**Podział administracyjny:** Oddziały firmy = osobne **Subscriptions** lub **Resource Groups**, NIE osobne **Azure AD Tenants** (to osobne katalogi, nie podział administracyjny).
**Expenditure Model:** **Pay-as-you-go** = model operacyjny (**OpEx**), nie **CapEx**.
**Geo-replikacja:** Dane muszą być przechowywane na wielu węzłach i w różnych lokalizacjach = **GRS/RA-GRS** lub **GZRS/RA-GZRS**.
**SLA:** **Preview** (wersja testowa) nie ma gwarantowanego **SLA**.
**Load Balancing:** **Azure Load Balancer = L4**, **Application Gateway/Front Door = L7**.
**Azure Policy:** Wymusza zgodność, ale nie blokuje fizycznie wdrożenia (może tylko oznaczyć **non-compliant**).
**Azure Marketplace:** Służy do wdrażania gotowych rozwiązań partnerów, nie do zarządzania kosztami.
**Azure Hybrid Benefit:** Pozwala użyć własnych licencji **Windows Server/SQL** w Azure, obniżając koszty.
**Azure Reservations:** Rezerwacja **VM** na 1/3 lata = niższy koszt, ale mniejsza elastyczność.
**Azure Advisor:** Daje rekomendacje, ale nie wymusza zmian.
**Azure Migrate:** Służy do oceny i migracji serwerów, baz, aplikacji do Azure.
**Azure DevTest Labs:** Szybkie środowiska testowe, nie produkcyjne.
**Azure Arc:** Zarządzanie zasobami spoza Azure (**on-premises**, **multi-cloud**).
**Azure Lighthouse:** Zarządzanie wieloma środowiskami klientów przez partnerów/**MSP**.
**Azure Bastion:** Bezpieczny dostęp do **VM** przez przeglądarkę, bez publicznego IP na VM.
**ExpressRoute:** Łącze dedykowane, nie szyfruje ruchu domyślnie (**VPN** szyfruje).
**Azure Free Account:** $200 na 30 dni + wybrane usługi na 12 miesięcy za darmo.
**VPN** szyfruje, **ExpressRoute** nie domyślnie.
**Azure Firewall** to **stateful firewall**, **NSG** nie.
**DDoS Standard** chroni **L3/L4**, nie **L7**.
**WAF** chroni **HTTP(S)**, np. **OWASP Top 10**.
**Private Endpoint** daje prywatny IP, **Service Endpoint** nie.
**Front Door** działa globalnie na edge (**L7**), **Traffic Manager** to **DNS**.
**Load Balancer** działa na **L4**, **Application Gateway** na **L7** (z **WAF**).
**Azure Functions** = event-driven serverless, płatność za wykonania (**Consumption**).
**Durable Functions** = workflow ze stanem.
**ACI** = szybkie uruchomienie kontenera bez orkiestracji, **ACA** = mikroserwisy, scale-to-zero.
**AKS** = pełna orkiestracja **Kubernetes**.
**PaaS** zmniejsza odpowiedzialność za **OS** i patchowanie, **IaaS** = samodzielny **OS**.
**Entra ID** to **IAM**, nie klasyczny **AD Domain Services**.
**Conditional Access** ocenia kontekst (**user/device/location/risk**).
**PIM** daje **JIT/JEA** dla ról uprzywilejowanych.
**Managed Identity** eliminuje potrzebę trzymania sekretów w kodzie.
**Blob tiers:** **Hot/Cool/Archive** różnią się kosztem zapisu i odczytu.
**LRS/ZRS/GRS/RA-GRS** to poziomy redundancji storage.
**Cosmos DB** = globalny **NoSQL** z **multi-region** i niskimi opóźnieniami.
**Preview ≠ GA** (brak pełnego **SLA**).

[⬅ Powrót do spisu treści](../README.md)






