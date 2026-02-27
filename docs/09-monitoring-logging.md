<a id="sec-09-monitoring-logging"></a>
## 9. Monitoring & Logging

[ Powrót do spisu treści](../README.md)


<img src="../assets/azuremonitor.svg" alt="Azure Monitor - centralna platforma monitoringu">

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

<img src="../assets/azureadvisor.svg" alt="Azure Advisor - rekomendacje optymalizacji">

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
---

[Poprzednia strona](./08-governance-compliance.md) | [Spis treści](../README.md) | [Następna strona](./10-costs-billing.md)
