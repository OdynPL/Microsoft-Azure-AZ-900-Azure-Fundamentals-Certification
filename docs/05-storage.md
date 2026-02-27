<a id="sec-05-storage"></a>
## 5. Storage (Przechowywanie danych)

[ Powrót do spisu treści](../README.md)


**Azure Storage** to zestaw usług do przechowywania plików, obiektów, danych NoSQL oraz dysków dla maszyn wirtualnych. 

Poniżej znajdziesz jasne i zrozumiałe wyjaśnienie, czym są poszczególne typy storage, po co się ich używa oraz jak działa redundancja danych.

<img src="../assets/storagetypes.svg" alt="Azure Storage - typy przechowywania danych">

---

## **Typy danych i ich zastosowania**

**Blob Storage** (obiekty / pliki)
Najbardziej uniwersalne miejsce na duże, niestrukturalne dane.

- **Hot** najczęściej używane dane, najwyższy koszt przechowywania, najniższy koszt dostępu.
- **Cool** dane rzadziej używane (dni–miesiące).
- **Cold** dane używane bardzo rzadko (miesiące–lata), tańsze od Cool, droższe od Archive pod względem odczytu.
- **Archive** dane długoterminowe, najniższy koszt przechowywania, ale bardzo drogi i wolny odczyt (kilka godzin).

Zastosowania: kopie zapasowe, logi, obrazy, filmy, dane ML, statyczne pliki

---

**Azure Files** (udzialy sieciowe)
Chmurowy odpowiednik klasycznego Windows File Server.

- SMB/NFS udziały sieciowe  
- Azure File Sync do synchronizacji z lokalnym serwerem plików  
- Zastosowania: udziały użytkowników, zasoby współdzielone, migracje serwerów plików

**Access Tiers** dla Azure Files
Azure Files obsługuje warstwy wydajności i warstwy dostępu, podobnie jak Blob Storage — ale inaczej działające.

Warstwy wydajności (Performance Tiers):
- **Standard** (HDD/SSD) – najtańszy, idealny dla klasycznych udziałów SMB/NFS
- **Premium** (SSD) – wysoka przepustowość i niski latency (np. FSLogix, intensywne I/O)

---

**Queue Storage** (kolejki wiadomości)
Prosta, skalowalna kolejka komunikatów dla architektur event‑driven.

- komunikacja asynchroniczna między usługami  
- idealna dla mikroserwisów, workerów, batch processing

---

**Table Storage** (NoSQL key‑value)
Tania, szybka i schematless baza NoSQL do ogromnych ilości danych.

- dane w formie PartitionKey + RowKey  
- idealne dla logów, metadanych, telemetrii, konfiguracji  
- alternatywa: Cosmos DB Table API

---

**Managed Disks** (dyski dla VM)
Dyski zarządzane przez Azure, wykorzystywane przez maszyny wirtualne.

- Premium SSD — produkcja, wysokie IOPS  
- Standard SSD — solidny kompromis cena/wydajność  
- Standard HDD — archiwizacja, testy  
- Ultra SSD — bardzo wysokie IOPS i throughput  
- OS Disk / Data Disk / Temp Disk (lokalny, nietrwały)

---

## **Redundancja danych (odporność na awarie)**

<img src="../assets/storageredundancy.svg" alt="Storage Redundancy - LRS, ZRS, GRS, RA-GRS">

**LRS — Locally Redundant Storage**
- 3 kopie danych w jednym datacenter  
- najniższy koszt  
- odporność tylko w obrębie pojedynczego DC

**ZRS — Zone Redundant Storage**
- replikacja między 3 strefami Availability Zone  
- ochrona przed awarią całej strefy  
- stabilny wybór dla środowisk produkcyjnych

**GRS — Geo Redundant Storage**
- LRS + kopia do regionu pary  
- 6 kopii danych łącznie  
- ochrona przed awarią całego regionu

**RA‑GRS — Read‑Access GRS**
- jak GRS, ale region zapasowy jest możliwy do odczytu  
- idealne dla globalnych aplikacji, które potrzebują dostępu read-only w czasie awarii

### Storage account options (skrót)

- **Performance tiers**: `Standard` (HDD/SSD) oraz `Premium` (SSD).
- **Redundancy options**: LRS, ZRS, GRS, RA‑GRS (zależnie od typu konta i regionu).
- **Access model**: public endpoint, private endpoint, firewall/VNet rules.
- **Security options**: szyfrowanie at-rest (Microsoft-managed keys lub customer-managed keys), soft delete, immutability (dla wybranych scenariuszy).

Na AZ‑900 kluczowe jest dobranie konta pod: koszt, wydajność, wymagania sieciowe i odporność.

### Backup vs Replication (częsta pułapka)

- **Replication** (LRS/ZRS/GRS/RA‑GRS) zwiększa dostępność i trwałość danych, ale **nie zastępuje backupu**.
- **Backup** służy do odtworzenia danych po błędach logicznych (np. przypadkowe usunięcie, ransomware, nadpisanie).
- Egzaminowo: replikacja chroni głównie przed awarią infrastruktury, backup chroni przed utratą danych z powodów operacyjnych i bezpieczeństwa.

### Dostęp do Storage: Entra RBAC vs SAS vs Access Keys

| Metoda | Kiedy używać | Ryzyko / uwaga |
|---|---|---|
| Entra ID + RBAC | domyślnie dla użytkowników i aplikacji | najlepszy model least privilege |
| SAS Token | delegowany, czasowy dostęp do konkretnego obiektu/zakresu | kontroluj TTL i uprawnienia (Read/Write/List) |
| Access Keys | tylko gdy nie da się użyć RBAC/SAS | szerokie uprawnienia; wymaga rotacji kluczy |

Rekomendacja praktyczna: **najpierw RBAC**, potem **SAS** dla delegacji, a **Access Keys** traktuj jako opcję awaryjną/legacy.

---

## **Narzędzia i migracje**

<img src="../assets/storagetools.svg" alt="Storage Explorer i AzCopy - narzedzia do zarzadzania Storage">

**Storage Explorer**
Narzędzie GUI do zarządzania Blob/Files/Queue/Table.

- przeglądanie i edycja danych  
- zarządzanie dostępem (SAS)  
- wygodne operacje na dużych strukturach plików  

**AzCopy**
Najszybsze narzędzie do przesyłania danych do/z Azure Storage.

- praca w CLI  
- wysoka wydajność  
- idealne do migracji dużych wolumenów

<img src="../assets/azuremigrate.svg" alt="Azure Migrate - migracja do chmury">

**Azure Migrate**
Aplikacje, VM‑ki i dane mogą być analizowane i migrowane do Azure.

- automatyczne rekomendacje  
- integracja z Azure Storage (np. blob staging)


<img src="../assets/databox.svg" alt="Data Box - fizyczny transfer danych offline">

**Data Box**  
Fizyczne urzadzenia Azure do przenoszenia bardzo duzych ilosci danych **offline**, bez wykorzystania Internetu.  
Stosowane wtedy, gdy lacze sieciowe jest zbyt wolne, niestabilne lub kosztowne.

- **Data Box Disk** – do ok. 40 TB; zestaw szyfrowanych dyskow SSD USB.  
  Szybkie wdrozenie, lekkie migracje, wiele lokalizacji.

- **Data Box** – do ok. 100 TB; pełne urządzenie NAS w formie walizki.  
  Wysoka przepustowosc lokalna, SMB/NFS/REST, duze migracje danych.

- **Data Box Heavy** – do ok. 1 PB; przemyslowe urzadzenie na kolach.  
  Bardzo duze migracje (archiwa, data lakes, media), transfery wielogigabitowe.
 
---
---

[Poprzednia strona](./04-networking.md) | [Spis treści](../README.md) | [Następna strona](./06-identity-access.md)
