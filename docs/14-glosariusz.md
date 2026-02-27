<a id="sec-14-glosariusz"></a>
## 14. Glosariusz skrócony

[ Powrót do spisu treści](../README.md)


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
---

[Poprzednia strona](./13-pytania-az900.md) | [Spis treści](../README.md) | [Następna strona](./15-free-account.md)
