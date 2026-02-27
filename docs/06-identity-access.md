<a id="sec-06-identity-access"></a>
## 6. Identity & Access (Microsoft Entra)

[ Powrót do spisu treści](../README.md)


**Microsoft Entra ID**  
Centralny system tożsamości w Azure i Microsoft 365. Odpowiada za uwierzytelnianie użytkowników, aplikacji i urządzeń. 

<img src="../assets/msentraid.svg" alt="Microsoft Entra ID - centralny system tozsamosci">
 
Kluczowe funkcje:
- **SSO (Single Sign-On)** – jedno logowanie do wielu aplikacji (SaaS, on-prem, Azure).  
- **MFA (Multi-Factor Authentication)** – dodatkowe potwierdzenie tożsamości (aplikacja, SMS, klucz FIDO2).  
- **External Identities** – bezpieczny dostęp dla partnerów/klientów bez tworzenia kont wewnętrznych.  
- **Passwordless** – logowanie bez hasła (Windows Hello, FIDO2, Phone Sign-in).  
- **Directory roles** – role administracyjne dla zarządzania tożsamością.

**Microsoft Entra Domain Services (Entra DS)**
- zarządzane usługi domenowe kompatybilne z AD (LDAP, Kerberos, NTLM),
- przydatne dla starszych aplikacji wymagających klasycznych mechanizmów domenowych,
- działa jako usługa zarządzana — bez stawiania własnych kontrolerów domeny.

Egzaminowo: **Entra ID ≠ Entra Domain Services** (inne cele i funkcje).

**Jak Entra ID uwierzytelnia użytkownika**?

<img src="../assets/userauthorization.svg" alt="Autoryzacja uzytkownika w Entra ID">

Proces logowania przebiega w kilku krokach:
1. **Identyfikacja** – użytkownik podaje login (UPN), Entra ID ustala tenant i polityki.  
2. **Uwierzytelnienie** – hasło, passwordless (FIDO2/Hello), certyfikat lub SSO z urządzenia.  
3. **Kontrole dostępu** (Conditional Access) – Entra ID ocenia ryzyko logowania, lokalizację, stan urządzenia, zgodność z politykami oraz wymaga MFA jeśli to konieczne.  
4. **Wydanie tokenów** – po pozytywnym potwierdzeniu wydawane są ID Token, Access Token i Refresh Token (OAuth2/OIDC), które aplikacje wykorzystują do autoryzacji.

**Access Token** i **ID Token** zwykle wygasają po około **1 godzinie**, a **Refresh Token** działa dłużej (typowo do **90 dni nieaktywności**) — dokładny czas zależy od polityk bezpieczeństwa i konfiguracji tenantu.

**Jak uwierzytelniane są aplikacje?**

<img src="../assets/appauthorization.svg" alt="Autoryzacja aplikacji w Entra ID">

Aplikacje korzystają z własnej tożsamości aplikacyjnej:
- sekret klienta,  
- certyfikat,  
- lub **Managed Identity** (najbardziej bezpieczny model, bez sekretów).  

**Managed Identity** to mechanizm nadawania aplikacjom tożsamości zarządzanej w pełni przez Azure — bez sekretów, bez haseł, bez certyfikatów.  

<img src="../assets/managedidentity.svg" alt="Managed Identity - tozsamosc zarzadzana dla aplikacji">

**Jak działa Managed Identity**:
- aplikacja nie przechowuje żadnych sekretów ani kluczy  
- uwierzytelnia się poprzez lokalny endpoint MSI dostępny tylko wewnątrz usługi  
- Azure potwierdza, że aplikacja ma tożsamość MI oraz sprawdza jej role RBAC  
- Entra ID wystawia **Access Token** dla konkretnej usługi  
- aplikacja dodaje token: `Authorization: Bearer <token>` i łączy się z zasobem

**Rodzaje Managed Identity**:
- **System-assigned** – tożsamość jest związana z jednym zasobem; tworzy sie sama i znika wraz z zasobem.  
- **User-assigned** – osobny zasób tożsamości, który można przypisac do wielu aplikacji; bardziej elastyczne.

Aplikacja uwierzytelnia sie w Entra ID, a nastepnie otrzymuje **Access Token** do zasobów takich jak Storage, Key Vault, SQL, Event Hub itd.

**Jak uwierzytelniane są urządzenia?**

<img src="../assets/deviceauthorization.svg" alt="Autoryzacja urzadzen w Entra ID">

Urządzenia mogą być:
- Azure AD Registered (BYOD),  
- Azure AD Joined (urządzenia firmowe),  
- Hybrid Joined (AD on‑prem + Entra ID).  

Entra ID sprawdza:
- czy urządzenie jest zapisane w katalogu,  
- czy jest zgodne (Intune compliance),  
- czy spełnia polityki bezpieczeństwa organizacji.  

Dzięki temu możliwe jest SSO, wymuszanie MFA, lub blokada dostępu dla niezaufanych urządzeń.

---

**RBAC (Role-Based Access Control)**  
Mechanizm autoryzacji w Azure oparty na rolach przypisywanych do określonych zakresów zasobów (scope).  
Umożliwia precyzyjne kontrolowanie, kto co może zrobić w danym zasobie — zgodnie z zasadą least privilege.

<img src="../assets/rbac.svg" alt="RBAC - Role-Based Access Control">

Najważniejsze elementy:

- **Role wbudowane**  
  Podstawowe role obejmują:  
  - **Owner** – pełna administracja zasobami + zarządzanie dostępem  
  - **Contributor** – pełna administracja zasobami, ale bez nadawania uprawnień  
  - **Reader** – tylko odczyt  

  Dodatkowo istnieją setki **ról specjalistycznych**, np. Storage Blob Reader, Key Vault Secrets Officer, Virtual Machine Contributor.

- **Custom roles**  
  Gdy role wbudowane są zbyt szerokie, można tworzyć role **niestandardowe** oparte na pojedynczych akcjach (actions / notActions), 
  np. tylko „odczyt sekretów Key Vault” lub „restart VM bez możliwości kasowania”.

- **Scope (zakres) i dziedziczenie**  
  Scope definiuje, do czego rola ma zastosowanie:  
  - **Subscription** → dziedziczy na wszystkie Resource Groups i zasoby  
  - **Resource Group** → dziedziczy na zasoby w tej grupie  
  - **Resource** → dotyczy tylko jednego zasobu  
  Uprawnienia przypisane wyżej **zawsze dziedziczą w dół**, dlatego zaleca się przypisywanie ról na możliwie najniższym poziomie.

- **Model decyzyjny**  
  Jeżeli użytkownik ma wiele ról w danym scope, sumują się one (model additive).  
  Blokady resource lock **nie są** elementem RBAC — działają oddzielnie.

- **Zastosowania mechanizmu RBAC**  
  RBAC jest kluczowe dla bezpieczeństwa:  
  - ogranicza nadmiarowe uprawnienia  
  - zapewnia podział obowiązków (SoD)  
  - precyzyjnie definiuje, kto może zarządzać Storage, VM, SQL, App Service, Key Vault itd.  
  - integruje się z **Managed Identity**, dzięki czemu aplikacje również korzystają z RBAC zamiast sekretów

---

**Conditional Access**
Mechanizm kontroli dostępu, który ocenia kontekst logowania i decyduje, czy użytkownik powinien uzyskać dostęp, zostać zablokowany, czy wykonać dodatkową akcję (np. MFA).  

Jest to kluczowy element modelu **Zero Trust — zasada: „never trust, always verify”**.

<img src="../assets/conditionalaccess.svg" alt="Conditional Access - kontrola dostepu warunkowego">

**Kluczowe atrybuty oceniane podczas logowania:**
- **Ryzyko logowania / ryzyko użytkownika** – wykrywanie anomalii, podejrzanych lokalizacji, nietypowych zachowań.  
- **Lokalizacja** – kraje, zakresy IP, sieci zaufane, VPN, corporate network.  
- **Stan urządzenia** – zgodność z Intune (compliant / non-compliant), typ OS, poziom zabezpieczeń.  
- **Typ aplikacji i klienta** – przeglądarka, aplikacja mobilna, desktopowa, klient legacy, API.  
- **Typ zasobu** – niektóre aplikacje lub API mogą wymagać mocniejszych zasad.  

**Dzialania (akcje) podejmowane przez zasady:**
- **Wymuszenie MFA** – dodatkowy czynnik potwierdzenia tożsamości.  
- **Blokada dostępu** – gdy ryzyko jest zbyt duże lub warunki nie są spełnione.  
- **Wymuszenie zgodnego urzadzenia** – tylko urządzenia spełniające wymagania Intune.  
- **Wymuszenie sesji bezhaslowej** – logowanie passwordless.  
- **Sesje z ograniczeniami** – np. zakaz pobierania plikow (MCAS/Defender for Cloud Apps).  

---

**PIM (Privileged Identity Management)**  
Mechanizm nadawania uprawnien administracyjnych na zasadzie JIT (Just‑In‑Time).

Najwazniejsze funkcje:
- Tymczasowa aktywacja roli tylko na okres wykonywanych zadan.  
- Wymuszenie uzasadnienia, zgody (approval) lub MFA przed nadaniem roli.  
- Audyt aktywacji, notyfikacje i alerty.  
- Minimalizacja ryzyka naduzyc przez stale podniesione uprawnienia.

---

**Managed Identities**  
Tożsamość systemowa dla aplikacji działających w Azure — bez kluczy, bez haseł, bez sekretów.  

Zastosowania:
- Dostęp aplikacji do Azure Storage, Key Vault, SQL, Event Hub itp.  
- Obsługa wbudowana w usługi Azure (VM, App Service, Functions, Logic Apps).  
- Kluczowe korzysci:
  - automatyczne zarządzanie tożsamością przez Azure  
  - brak potrzeby przechowywania i rotowania sekretow  
  - zgodnosc z Zero Trust  

Rodzaje:
- **System-assigned** – tożsamość przypisana do jednego zasobu  
- **User-assigned** – tożsamość tworzona jako oddzielny zasób, współdzielona między aplikacjami

---
---

[Poprzednia strona](./05-storage.md) | [Spis treści](../README.md) | [Następna strona](./07-security.md)
