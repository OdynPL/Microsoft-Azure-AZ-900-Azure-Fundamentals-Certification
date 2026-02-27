<a id="sec-12-extended-services"></a>
## 12. Extended Azure Services (Usługi rozszerzone)

[ Powrót do spisu treści](../README.md)


### Integracja i zdarzenia

- **Azure Service Bus**  
  Broker komunikatów klasy enterprise — **niezawodne kolejki** (1:1) oraz publish/subscribe (1→n), z obsługą transakcji, DLQ i protokołu AMQP. Idealny do integracji mikroserwisów oraz scenariuszy wymagających trwałego kolejkowania.

  <img src="../assets/servicebus.svg" alt="Azure Service Bus - enterprise broker komunikatow">

- **Azure Event Hub**  
  Platforma do zbierania i przetwarzania **strumieni danych** na ogromną skalę (telemetria, logi, IoT), nawet miliony zdarzeń na sekundę. Świetnie współpracuje z Databricks, Spark, Stream Analytics i może działać jako Kafka‑as‑a‑Service.

  <img src="../assets/eventhub.svg" alt="Azure Event Hub - przetwarzanie strumieni danych">

- **Azure Event Grid**  
  Lekki **router zdarzeń** typu push, idealny do budowania architektur event‑driven o niskich opóźnieniach. Umożliwia reagowanie na zdarzenia z usług Azure (np. BlobCreated → Function) i integrację systemów poprzez eventy.

  <img src="../assets/eventgrids.svg" alt="Azure Event Grid - router zdarzen event-driven">

---

### API i edge

<img src="../assets/apiedge.svg" alt="API Management i Edge Services">

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

<img src="../assets/aiservices.svg" alt="Azure AI Services - Cognitive, OpenAI, ML">

**Cognitive Services / Azure OpenAI / Azure ML**

- **Cognitive Services**  
  Gotowe modele AI bez trenowania: OCR, analiza obrazów, rozpoznawanie mowy, tłumaczenia, sentiment, wykrywanie anomalii.

- **Azure OpenAI**  
  Modele generatywne (GPT, embeddings): generowanie tekstu, podsumowania, chatboty, wyszukiwanie semantyczne, automatyzacja procesów.

- **Azure Machine Learning (Azure ML)**  
  Platforma MLOps: trening modeli, eksperymenty, rejestr modeli, pipeline’y, wdrożenia do endpointów, monitorowanie jakości i driftu.

---

<img src="../assets/dataplatform.svg" alt="Azure Data Platform - Data Factory, Synapse, Databricks">

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

<img src="../assets/iotschema.svg" alt="Azure IoT - Hub, Central, Digital Twins, Sphere">

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
---

[Poprzednia strona](./11-iac-automation.md) | [Spis treści](../README.md) | [Następna strona](./13-pytania-az900.md)
