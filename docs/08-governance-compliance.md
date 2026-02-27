<a id="sec-08-governance-compliance"></a>
## 8. Governance & Compliance

[ Powrót do spisu treści](../README.md)


Governance w Azure to **zestaw zasad i narzędzi**, które pomagają utrzymać porządek, bezpieczeństwo i kontrolę nad zasobami w chmurze.

### Azure Policy

<img src="../assets/azurepolicy.svg" alt="Azure Policy - wymuszanie zgodnosci konfiguracji">

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

<img src="../assets/resourceLocks.svg" alt="Resource Locks - blokady zasobow">

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

<img src="../assets/tags.svg" alt="Tags - metadane zasobow Azure">

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

<img src="../assets/blueprints.svg" alt="Azure Blueprints - powtarzalne srodowiska">

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

<img src="../assets/azurearc.svg" alt="Azure Arc - zarzadzanie zasobami poza Azure">

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

<img src="../assets/sharedresponsibility.svg" alt="Shared Responsibility Model - podzial odpowiedzialnosci">

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

<img src="../assets/servicetrust.svg" alt="Service Trust Portal - raporty zgodnosci">

- **Microsoft Service Trust Portal** – raporty zgodności (ISO, SOC, PCI), audyty, certyfikaty.
- **Data Residency (rezydencja danych)** – dane pozostają w wybranej geografii (np. EU).
- **Azure Government / Azure China** – suwerenne regiony dla wymagań regulacyjnych.

---

### Well‑Architected Framework (WAF – 5 filarów)

<img src="../assets/wellarchitected.svg" alt="Well-Architected Framework - 5 filarow">

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

<img src="../assets/supportplans.svg" alt="Support Plans - plany wsparcia technicznego">

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
---

[Poprzednia strona](./07-security.md) | [Spis treści](../README.md) | [Następna strona](./09-monitoring-logging.md)
