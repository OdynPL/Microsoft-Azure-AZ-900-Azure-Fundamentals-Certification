<a id="sec-11-iac-automation"></a>
## 11. IaC & Automation (Infrastruktura jako kod)

[ Powrót do spisu treści](../README.md)


**IaC (Infrastructure as Code)** to podejście, w którym całą infrastrukturę — serwery, sieci, bazy, konfiguracje — definiuje się i zarządza nią za pomocą plików kodu, zamiast ręcznych kliknięć w portal, co zapewnia automatyzację, powtarzalność i pełną kontrolę wersji.

<img src="../assets/iac.svg" alt="Infrastructure as Code - automatyzacja infrastruktury">

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
---

[Poprzednia strona](./10-costs-billing.md) | [Spis treści](../README.md) | [Następna strona](./12-extended-services.md)
