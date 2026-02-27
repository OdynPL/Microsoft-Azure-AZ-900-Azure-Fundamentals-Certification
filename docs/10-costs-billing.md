<a id="sec-10-costs-billing"></a>
## 10. Costs & Billing (Koszty i rozliczenia)

[ Powrót do spisu treści](../README.md)


<img src="../assets/costsbilling.svg" alt="Azure Costs and Billing - koszty i rozliczenia">

**Za co płacisz w Azure:**
- **Compute (czas działania)**  
  Opłaty za czas pracy zasobów obliczeniowych (VM, App Service, AKS node pools, Functions).  
  Rozliczanie zwykle „pay‑as‑you‑go” — za sekundę lub minutę działania.

- **Storage (GB / miesiąc)**  
  Koszt zależny od ilości przechowywanych danych oraz liczby operacji.  
  Uwzględnia typ przestrzeni dyskowej (Hot / Cool / Archive), replikację (LRS/ZRS/GRS) oraz transakcje.

- **Egress sieci (wyjście z Azure)**  
  Dane *opuszczające* Azure — np. wysyłane do Internetu, do lokalnego datacenter lub między regionami.  
  To jedno z najczęstszych „ukrytych” źródeł kosztów, bo transfer wychodzący jest **zawsze płatny**.

- **Ingress sieci (wejście do Azure)**  
  Dane *wchodzące* do Azure — np. upload plików, dane z on‑prem, ruch przychodzący z Internetu.  
  W modelu transferu sieciowego Azure **ingress jest zazwyczaj bezpłatny**.  
  W praktyce: za pobieranie z Azure płacisz, za wysyłanie do Azure — nie.

- **Licencje**  
  Płatne modele licencji dla Windows Server, SQL Server, RedHat, SUSE, a także usług takich jak Defender.  
  Rozliczane per rdzeń, instancję lub użytkownika.

---

### **Optymalizacja kosztów:**

- **Reservations (1 lub 3 lata)**  
  Rezerwacje na VM, SQL, Cosmos DB i inne usługi.  
  Oszczędność 40–70% — idealne dla stabilnych workloadów produkcyjnych.

- **Spot VMs**  
  Tanie VM działające, gdy dostępna jest wolna moc obliczeniowa.  
  Mogą zostać przerwane — dobre do batch jobs, CI/CD, symulacji, renderingu.

- **Azure Hybrid Benefit (AHB)**  
  Możliwość wykorzystania posiadanych licencji Windows/SQL z Software Assurance.  
  Obniża koszt VM i SQL PaaS nawet o 30–50%.

---

### **Narzędzia do kosztów i analizy:**

- **Pricing Calculator**  
  Kalkulator kosztów — pozwala wyliczyć przewidywane koszty architektury przed wdrożeniem.

- **TCO (Total Cost of Ownership) Calculator**  
  Porównanie kosztów on‑premise vs Azure (sprzęt, energia, zarządzanie, amortyzacja).  
  Pomocny przy planowaniu migracji i budowaniu business case.

- **Cost Management**  
  Wbudowane narzędzie do analizy i kontroli wydatków:
  - budżety i alerty,
  - analiza kosztów po tagach, subskrypcjach i zasobach,
  - prognozy wydatków,
  - rekomendacje optymalizacyjne (np. niewykorzystywane IP/VM/dyski).

---
---

[Poprzednia strona](./09-monitoring-logging.md) | [Spis treści](../README.md) | [Następna strona](./11-iac-automation.md)
