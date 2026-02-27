<a id="sec-30-data-factory"></a>
## 30. Azure Data Factory

[ Powrót do spisu treści](../README.md)


Azure Data Factory (ADF) to serverless usługa **ETL/ELT** do tworzenia pipeline'ów danych - automatycznych przepływów, które pobierają dane ze źródeł, przekształcają je i ładują do systemów docelowych.

### Po co Data Factory?

Masz dane w różnych miejscach: SQL Server, Salesforce, Blob Storage, Oracle. Chcesz je **połączyć i załadować** do hurtowni danych (Azure Synapse). Data Factory robi to automatycznie!

### Jak to działa?

<img src="../assets/adf_overview.svg" alt="Azure Data Factory - ETL/ELT Pipeline">

1. **EXTRACT** - Połącz się ze źródłami (SQL, API, pliki)
2. **TRANSFORM** - Oczyść, połącz, zagreguj dane
3. **LOAD** - Zapisz do systemu docelowego

---

### Główne komponenty

<img src="../assets/adf_components.svg" alt="Azure Data Factory - komponenty">

| Komponent | Opis |
|-----------|------|
| **Pipeline** | Kontener na Activities (kroki) |
| **Activity** | Pojedyncza operacja: Copy, Data Flow, Lookup |
| **Dataset** | Wskaźnik na dane: tabela SQL, plik CSV |
| **Linked Service** | Połączenie do źródła danych |
| **Trigger** | Kiedy uruchomić: schedule, event |
| **Integration Runtime** | Silnik wykonawczy: Azure lub Self-hosted |

---

### Typy Activities

| Typ | Opis |
|-----|------|
| **Data Movement** | Copy Activity |
| **Data Transformation** | Data Flow, Databricks |
| **Control Flow** | If, ForEach, Until |
| **External** | Azure Function, Stored Procedure |

---

### Integration Runtime

| Typ IR | Użycie |
|--------|--------|
| **Azure IR** | Cloud-to-cloud (domyślny) |
| **Self-hosted IR** | Cloud-to-on-premises |
| **Azure-SSIS IR** | Migracja pakietów SSIS |

**Self-hosted IR** - gdy dane są on-premises lub firewall blokuje dostęp.

---

### Przyklady uzycia

**1. Codzienna synchronizacja sprzedazy:**
```
Trigger: Codziennie o 2:00
Pipeline:
  1. Copy z SQL Server (on-prem) do Blob Storage
  2. Data Flow - agregacja po regionach
  3. Copy do Azure Synapse
  4. Send email z raportem
```

**2. Event-driven processing:**
```
Trigger: Nowy plik w Blob Storage
Pipeline:
  1. Lookup - sprawdz format pliku
  2. If JSON: Parsuj i laduj do Cosmos DB
  3. If CSV: Przeksztalc i laduj do SQL Database
  4. Archive - przenies plik do archiwum
```

---

### Tworzenie Data Factory - CLI

```bash
# Utworz Data Factory
az datafactory create \
  --resource-group myRG \
  --factory-name myDataFactory \
  --location westeurope

# Utworz Linked Service (polaczenie do Blob Storage)
az datafactory linked-service create \
  --resource-group myRG \
  --factory-name myDataFactory \
  --linked-service-name AzureBlobStorage \
  --properties '{"type":"AzureBlobStorage","typeProperties":{"connectionString":"DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=xxx"}}'

# Uruchom pipeline
az datafactory pipeline create-run \
  --resource-group myRG \
  --factory-name myDataFactory \
  --pipeline-name MyPipeline
```

---

### Mapping Data Flow vs Copy Activity

| Cecha | Copy Activity | Mapping Data Flow |
|-------|---------------|-------------------|
| **Cel** | Proste kopiowanie | Zlozone transformacje |
| **Transformacje** | Brak (1:1 copy) | Join, Aggregate, Pivot, Filter |
| **Wydajnosc** | Bardzo szybkie | Wolniejsze (Spark pod spodem) |
| **Koszt** | Nizszy | Wyzszy |
| **Kiedy?** | Migracja danych | ETL z logika biznesowa |

---

### Cennik

| Składnik | Cena |
|----------|------|
| **Orkiestracja** | ~$1/1000 uruchomień |
| **Data Movement** | ~$0.25/DIU-godz |
| **Data Flow** | ~$0.27/vCore-godz |
| **Self-hosted IR** | Bezpłatne |

---

### Best Practices

- Parametryzuj pipeline'y (bez hardcode)
- Hasła w Key Vault, nie w ADF
- Monitoruj z Azure Monitor
- Git integration dla wersjonowania

---

### FAQ - Egzamin

| Pytanie | Odpowiedź |
|---------|----------|
| Co to ADF? | Serverless ETL/ELT w chmurze |
| Co to Pipeline? | Kontener na Activities |
| Co to Linked Service? | Połączenie do źródła danych |
| Co to Integration Runtime? | Silnik wykonawczy (Azure/Self-hosted) |
| Kiedy Self-hosted IR? | Dane on-premises lub w prywatnej sieci |
| ADF vs Synapse Pipelines? | Synapse = ADF + Analytics w jednym |
| Ile connectorow ma ADF? | 90+ wbudowanych |
| Czy ADF jest serverless? | TAK - pay-per-use |

> **Egzamin:** Azure Data Factory to serverless ETL/ELT service do integracji danych. Pipeline = kontener na Activities. Activity = operacja (Copy, Data Flow). Linked Service = polaczenie do zrodla. Integration Runtime = silnik wykonawczy (Azure dla cloud, Self-hosted dla on-premises). 90+ wbudowanych connectorow. Data Flow uzywa Apache Spark do transformacji.

---
---

[Poprzednia strona](./29-load-balancing.md) | [Spis treści](../README.md) | [Następna strona](./31-azure-storage.md)
