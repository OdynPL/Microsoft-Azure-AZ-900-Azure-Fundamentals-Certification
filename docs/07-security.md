<a id="sec-07-security"></a>
## 7. Security (Bezpieczeństwo)

[ Powrót do spisu treści](../README.md)


### Defense-in-depth (model warstwowy)

<img src="../assets/defenseindepth.svg" alt="Defense in Depth - model warstwowy bezpieczenstwa">

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

<img src="../assets/keyvault.svg" alt="Azure Key Vault - przechowywanie sekretow i kluczy">

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
---

[Poprzednia strona](./06-identity-access.md) | [Spis treści](../README.md) | [Następna strona](./08-governance-compliance.md)
