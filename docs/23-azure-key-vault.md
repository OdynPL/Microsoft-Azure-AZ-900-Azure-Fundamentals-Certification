<a id="sec-23-azure-key-vault"></a>
## 23. Azure Key Vault

[ Powrót do spisu treści](../README.md)


<img src="../assets/keyvault_structure.svg" alt="Azure Key Vault - struktura i typy obiektow">

### Czym jest Azure Key Vault?

**Azure Key Vault** to w pełni zarządzana usługa do bezpiecznego przechowywania i zarządzania wrażliwymi danymi:
- **Secrets** – hasła, connection stringi, API keys, tokeny
- **Keys** – klucze kryptograficzne do szyfrowania i podpisywania
- **Certificates** – certyfikaty TLS/SSL z automatycznym odnowieniem

**Główne zalety:**
- Centralizacja sekretów w jednym miejscu
- Brak sekretów hardkodowanych w kodzie/configach
- Pełny audit trail (kto, kiedy, co)
- Integracja z Managed Identity (zero credentials w aplikacji)
- Szyfrowanie w spoczynku i w tranzycie
- Zgodność z regulacjami (FIPS, SOC, ISO, PCI)

---

### Typy obiektów w Key Vault

| Typ | Opis | Przykłady użycia |
|-----|------|------------------|
| **Secrets** | Dowolny ciąg znaków do 25KB | Connection stringi, API keys, hasła |
| **Keys** | Klucze kryptograficzne (RSA, EC) | Szyfrowanie danych, podpisywanie JWT |
| **Certificates** | Certyfikaty X.509 + klucz prywatny | TLS dla App Gateway, HTTPS binding |

**Keys - typy i algorytmy:**

| Typ klucza | Rozmiary | Użycie |
|------------|----------|--------|
| **RSA** | 2048, 3072, 4096 bit | Szyfrowanie, podpisywanie |
| **EC (Elliptic Curve)** | P-256, P-384, P-521 | Szybsze podpisy, mniejsze klucze |
| **oct (Symmetric)** | 128, 192, 256 bit | AES encryption (tylko HSM) |

**Operacje kryptograficzne:**
```
Encrypt / Decrypt    - szyfrowanie danych kluczem
Sign / Verify        - podpis cyfrowy i weryfikacja
Wrap / Unwrap        - owijanie kluczy (key encryption keys)
```

---

### Kontrola dostępu

<img src="../assets/keyvault_access_flow.svg" alt="Key Vault Access Flow - przeplyw dostepu">

Key Vault obsługuje dwa modele dostępu:

**1. RBAC (Azure Role-Based Access Control) - ZALECANE**

| Rola | Uprawnienia |
|------|-------------|
| **Key Vault Administrator** | Pełne zarządzanie vault + zawartością |
| **Key Vault Secrets Officer** | Zarządzanie sekretami (CRUD) |
| **Key Vault Secrets User** | Tylko odczyt sekretów (Get, List) |
| **Key Vault Crypto Officer** | Zarządzanie kluczami |
| **Key Vault Crypto User** | Operacje kryptograficzne (Sign, Encrypt) |
| **Key Vault Certificates Officer** | Zarządzanie certyfikatami |

**Przykład przypisania roli:**
```bash
# Nadanie roli "Key Vault Secrets User" dla Managed Identity
az role assignment create \
    --role "Key Vault Secrets User" \
    --assignee <managed-identity-principal-id> \
    --scope /subscriptions/<sub>/resourceGroups/<rg>/providers/Microsoft.KeyVault/vaults/<vault-name>
```

**2. Access Policy (Legacy)**

Starszy model – przypisujesz uprawnienia per principal:
```bash
az keyvault set-policy --name myKeyVault \
    --object-id <app-object-id> \
    --secret-permissions get list
```

> **Uwaga:** Microsoft zaleca RBAC dla nowych wdrożeń. Access Policy nie integruje się z Conditional Access ani PIM.

---

### Tworzenie Key Vault przez CLI

```bash
# Utworzenie Key Vault (Standard tier)
az keyvault create --name myKeyVault \
    --resource-group myRG \
    --location westeurope \
    --sku standard \
    --enable-rbac-authorization true \
    --enable-soft-delete true \
    --soft-delete-retention-days 90 \
    --enable-purge-protection true

# Premium tier (HSM-backed keys)
az keyvault create --name myHSMVault \
    --resource-group myRG \
    --location westeurope \
    --sku premium \
    --enable-rbac-authorization true
```

---

### Zarządzanie sekretami

```bash
# Dodanie sekretu
az keyvault secret set --vault-name myKeyVault \
    --name "DatabaseConnectionString" \
    --value "Server=tcp:myserver.database.windows.net;..."

# Pobranie sekretu
az keyvault secret show --vault-name myKeyVault \
    --name "DatabaseConnectionString" \
    --query "value" -o tsv

# Lista sekretów
az keyvault secret list --vault-name myKeyVault -o table

# Usunięcie sekretu (soft delete)
az keyvault secret delete --vault-name myKeyVault \
    --name "DatabaseConnectionString"

# Odzyskanie usuniętego sekretu
az keyvault secret recover --vault-name myKeyVault \
    --name "DatabaseConnectionString"

# Trwałe usunięcie (purge) - wymaga uprawnień
az keyvault secret purge --vault-name myKeyVault \
    --name "DatabaseConnectionString"
```

---

### Wersjonowanie sekretów

Key Vault automatycznie wersjonuje każdy obiekt:

```
https://mykeyvault.vault.azure.net/secrets/DatabaseConnectionString
  └── /versions/abc123def456...  (wersja 1)
  └── /versions/xyz789ghi012...  (wersja 2 - aktualna)
```

**Pobieranie konkretnej wersji:**
```bash
az keyvault secret show --vault-name myKeyVault \
    --name "DatabaseConnectionString" \
    --version "abc123def456"
```

**Bez podania wersji** → zawsze zwraca **najnowszą aktywną** wersję.

---

### Integracja z aplikacjami

<img src="../assets/keyvault_integration.svg" alt="Key Vault Integration - integracja z aplikacjami">

**1. App Service / Azure Functions (Key Vault Reference)**

W Application Settings zamiast wartości używasz referencji:
```
@Microsoft.KeyVault(SecretUri=https://mykeyvault.vault.azure.net/secrets/MySecret/)
```

Lub z konkretną wersją:
```
@Microsoft.KeyVault(SecretUri=https://mykeyvault.vault.azure.net/secrets/MySecret/abc123)
```

**Wymagania:**
- App Service musi mieć System-assigned Managed Identity
- Managed Identity musi mieć rolę "Key Vault Secrets User"

**2. Kod aplikacji (.NET)**

```csharp
// Używając Azure.Identity i Azure.Security.KeyVault.Secrets
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

var client = new SecretClient(
    new Uri("https://mykeyvault.vault.azure.net/"),
    new DefaultAzureCredential() // używa Managed Identity
);

KeyVaultSecret secret = await client.GetSecretAsync("DatabaseConnectionString");
string connectionString = secret.Value;
```

---

### Zarzadzanie certyfikatami

```bash
# Import istniejącego certyfikatu (PFX)
az keyvault certificate import --vault-name myKeyVault \
    --name "MyCert" \
    --file ./certificate.pfx \
    --password "pfx-password"

# Żądanie certyfikatu od CA (DigiCert, GlobalSign)
az keyvault certificate create --vault-name myKeyVault \
    --name "MyCert" \
    --policy @policy.json

# Self-signed certificate
az keyvault certificate create --vault-name myKeyVault \
    --name "SelfSignedCert" \
    --policy "$(az keyvault certificate get-default-policy)"

# Pobranie certyfikatu (public part)
az keyvault certificate show --vault-name myKeyVault \
    --name "MyCert"
```

**Auto-renewal:**
Key Vault może automatycznie odnawiać certyfikaty przed wygaśnięciem:
- Zintegrowane CA (DigiCert, GlobalSign) – pełna automatyzacja
- Self-signed – automatyczne regenerowanie
- Manual CA – wysyła powiadomienia

---

### Customer-Managed Keys (CMK)

Key Vault przechowuje klucze szyfrujące dane w innych usługach:

| Usługa | Jak używa Key Vault |
|--------|---------------------|
| **Azure Storage** | CMK do szyfrowania blob/file/queue |
| **Azure SQL** | TDE (Transparent Data Encryption) |
| **Azure Disk Encryption** | Szyfrowanie dysków VM |
| **Cosmos DB** | CMK dla danych at-rest |
| **Azure Backup** | Szyfrowanie backup vaults |

**Konfiguracja CMK dla Storage:**
```bash
# 1. Utworzenie klucza w Key Vault
az keyvault key create --vault-name myKeyVault \
    --name "storage-encryption-key" \
    --kty RSA --size 2048

# 2. Nadanie uprawnień Storage Account
az role assignment create \
    --role "Key Vault Crypto Service Encryption User" \
    --assignee <storage-account-principal-id> \
    --scope /subscriptions/.../vaults/myKeyVault

# 3. Konfiguracja Storage do używania CMK
az storage account update --name mystorageaccount \
    --resource-group myRG \
    --encryption-key-source Microsoft.Keyvault \
    --encryption-key-vault https://mykeyvault.vault.azure.net \
    --encryption-key-name storage-encryption-key
```

---

### Bezpieczeństwo Key Vault

<img src="../assets/keyvault_security_tiers.svg" alt="Key Vault Security - warstwy bezpieczenstwa">

**Soft Delete (domyślnie włączone):**
- Usunięty vault/secret/key trafia do "soft deleted" state
- Można odzyskać przez 7-90 dni (default 90)
- Chroni przed przypadkowym usunięciem

**Purge Protection:**
- Blokuje trwałe usunięcie (purge) w okresie retencji
- **Wymagane** gdy używasz CMK dla innych usług
- Po włączeniu **nie można wyłączyć**

**Network Security:**

```bash
# Firewall - tylko wybrane IP
az keyvault update --name myKeyVault \
    --default-action Deny \
    --bypass AzureServices

az keyvault network-rule add --name myKeyVault \
    --ip-address 203.0.113.5

# Private Endpoint (zalecane dla produkcji)
az network private-endpoint create \
    --name myKVPrivateEndpoint \
    --resource-group myRG \
    --vnet-name myVNet \
    --subnet PrivateEndpointSubnet \
    --private-connection-resource-id <keyvault-resource-id> \
    --group-id vault \
    --connection-name myKVConnection
```

---

### Diagnostyka i monitoring

```bash
# Włączenie logów diagnostycznych
az monitor diagnostic-settings create \
    --name "KeyVaultLogs" \
    --resource <keyvault-resource-id> \
    --workspace <log-analytics-workspace-id> \
    --logs '[{"category":"AuditEvent","enabled":true}]'
```

**Kluczowe metryki:**
- **ServiceApiHit** – liczba żądań API
- **ServiceApiLatency** – czas odpowiedzi
- **ServiceApiResult** – sukces vs błędy

**Audit events logują:**
- Kto uzyskał dostęp (principal ID)
- Do jakiego obiektu (secret/key/cert)
- Jaka operacja (Get, Set, Delete)
- Czas i wynik operacji

---

### Porównanie warstw cenowych

| Aspekt | Standard | Premium |
|--------|----------|---------|
| **Secrets** | ✅ | ✅ |
| **Keys (software)** | ✅ | ✅ |
| **Keys (HSM)** | ❌ | ✅ |
| **Certificates** | ✅ | ✅ |
| **FIPS 140-2** | Level 1 | Level 2 |
| **Cena (10K ops)** | ~$0.03 | ~$0.03 + $1/klucz HSM |
| **Use case** | Większość scenariuszy | Banking, compliance |

---

### Managed HSM (osobna usługa)

Dla najwyższych wymagań compliance:
- **FIPS 140-2 Level 3** (vs Level 2 w Premium)
- **Single-tenant** dedicated HSM
- Pełna kontrola nad domeną bezpieczeństwa
- ~$3/godz (~$2200/miesiąc)

---

### Best Practices

| Praktyka | Opis |
|----------|------|
| **Używaj RBAC** | Zamiast Access Policy |
| **Managed Identity** | Zero credentials w kodzie |
| **Soft Delete + Purge Protection** | Zawsze włączone |
| **Private Endpoint** | Dla produkcji |
| **Osobny vault per środowisko** | Dev/Test/Prod izolowane |
| **Rotacja sekretów** | Automatyczna lub regularna |
| **Monitoruj dostęp** | Log Analytics + alerty |
| **Taguj zasoby** | Environment, Owner, Project |

---

### Podchwytliwe pytania egzaminacyjne

| Pytanie | Odpowiedź |
|---------|-----------|
| Gdzie przechowywać connection string do SQL? | **Key Vault** (nie App Settings!) |
| Co chroni przed przypadkowym usunięciem? | **Soft Delete** |
| Co wymagane dla CMK? | **Purge Protection** |
| Jaki tier dla HSM-backed keys? | **Premium** |
| Jak App Service czyta z Key Vault bez credentials? | **Managed Identity** |
| RBAC vs Access Policy? | **RBAC** jest zalecane |
| Czy Key Vault szyfruje dane? | Nie – przechowuje klucze, które szyfrują dane |

> **Egzamin:** Key Vault to centralne miejsce na sekrety, klucze i certyfikaty. Używaj Managed Identity dla dostępu aplikacji bez credentials w kodzie.

---
---

[Poprzednia strona](./22-last-minute-cram.md) | [Spis treści](../README.md) | [Następna strona](./24-debugging.md)
