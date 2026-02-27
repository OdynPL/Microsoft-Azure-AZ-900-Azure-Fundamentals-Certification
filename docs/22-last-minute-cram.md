<a id="sec-22-last-minute-cram"></a>
## 22. Last-minute cram (pułapki + porównania)

[ Powrót do spisu treści](../README.md)


### 40 najczęstszych pułapek egzaminacyjnych AZ‑900

1. Subskrypcja nie kosztuje sama w sobie — płacisz za zasoby.
2. Resource Group to kontener logiczny, nie granica sieci.
3. RBAC dziedziczy się tylko w dół hierarchii.
4. `Owner` może zarządzać dostępem, `Contributor` nie.
5. Azure Policy `Deny` blokuje wdrożenie.
6. `Audit` w Policy nie blokuje wdrożenia.
7. Lock `ReadOnly` blokuje zmiany i usunięcie.
8. Lock `Delete` blokuje tylko usuwanie.
9. Region to nie to samo co Availability Zone.
10. Availability Set to nie Availability Zone.
11. HA zwykle dotyczy 1 regionu, DR zwykle 2+ regionów.
12. RTO = czas odtworzenia, RPO = dopuszczalna utrata danych.
13. Egress jest płatny, ingress zazwyczaj bezpłatny.
14. ExpressRoute jest prywatny, ale nie szyfruje danych domyślnie.
15. VPN Site‑to‑Site łączy sieci, Point‑to‑Site pojedynczego użytkownika.
16. NSG to ACL L3/L4, nie pełny firewall aplikacyjny.
17. Azure Firewall to stateful firewall, NSG nie.
18. DDoS Standard chroni L3/L4, nie L7.
19. WAF chroni warstwę aplikacyjną HTTP(S), np. OWASP Top 10.
20. Private Endpoint daje prywatny IP, Service Endpoint nie.
21. Front Door działa globalnie na edge (L7).
22. Traffic Manager działa na DNS, nie proxy’uje ruchu HTTP.
23. Load Balancer działa na L4 (TCP/UDP).
24. Application Gateway działa na L7 i może mieć WAF.
25. Azure Functions = event-driven serverless; płatność za wykonania (Consumption).
26. Durable Functions służą do długich workflow ze stanem.
27. ACI to szybkie uruchomienie kontenera bez orkiestracji.
28. ACA jest wyżej poziomem niż ACI (mikroserwisy, scale-to-zero).
29. AKS daje pełną orkiestrację Kubernetes.
30. PaaS zmniejsza odpowiedzialność za OS i patchowanie.
31. W IaaS odpowiadasz m.in. za OS i hardening VM.
32. Entra ID to IAM, nie klasyczny AD Domain Services.
33. Conditional Access ocenia kontekst (user/device/location/risk).
34. PIM daje JIT/JEA dla ról uprzywilejowanych.
35. Managed Identity eliminuje potrzebę trzymania sekretów w kodzie.
36. Blob tiers: Hot/Cool/Cold/Archive różnią się kosztem zapisu i odczytu.
37. LRS/ZRS/GRS/RA‑GRS to poziomy redundancji storage.
38. Cosmos DB to globalny NoSQL z multi‑region i niskimi opóźnieniami.
39. Preview nie oznacza pełnej gotowości produkcyjnej i pełnego SLA.
40. GA oznacza stabilność produkcyjną i standardowe warunki wsparcia/SLA.

---

### Szybkie porównania usług (ściąga)

#### Compute

| Usługa | Kiedy wybierać | Kluczowa cecha |
|---|---|---|
| Virtual Machines | pełna kontrola nad OS | IaaS, największa elastyczność |
| App Service | web/API bez administrowania serwerami | PaaS dla aplikacji HTTP |
| Azure Functions | eventy, krótkie zadania | serverless, trigger + binding |
| Azure Container Apps | mikroserwisy/kontenery bez K8s | autoscaling + scale-to-zero |
| AKS | złożone kontenery na dużą skalę | pełny Kubernetes |

#### Integracja i zdarzenia

| Usługa | Wzorzec | Najlepsze zastosowanie |
|---|---|---|
| Service Bus | kolejki/topics (enterprise messaging) | niezawodna integracja systemów |
| Event Grid | routing eventów (push) | zdarzenia reaktywne w Azure |
| Event Hub | streaming telemetry | duży wolumen zdarzeń/IoT/logi |
| Storage Queue | prosta kolejka | lekkie scenariusze async |

#### Sieć i ruch web

| Usługa | Warstwa | Zastosowanie |
|---|---|---|
| Load Balancer | L4 | ruch TCP/UDP dla VM/VMSS |
| Application Gateway | L7 | routing HTTP(S) + opcjonalny WAF |
| Front Door | L7 Edge | globalne aplikacje web, accel + WAF |
| Traffic Manager | DNS | globalny routing/failover DNS |

#### Tożsamość i bezpieczeństwo

| Mechanizm | Co robi | Typowe użycie |
|---|---|---|
| RBAC | autoryzacja do zasobów | kto co może zrobić |
| Conditional Access | polityki dostępu zależne od kontekstu | wymuszenie MFA/compliance |
| PIM | czasowe role uprzywilejowane | JIT dla adminów |
| Managed Identity | tożsamość dla aplikacji bez sekretów | dostęp do KV/Storage/SQL |

#### Storage

| Typ | Charakter danych | Przykład |
|---|---|---|
| Blob | obiektowe, niestrukturalne | backupy, logi, media |
| Files | SMB/NFS share | udziały plików |
| Queue | wiadomości | komunikacja async |
| Table | NoSQL key-value | metadane, telemetria |
| Managed Disks | dyski VM | OS/Data disk dla IaaS |

---

### Jak używać tej sekcji 24h przed egzaminem

- Przerób 40 pułapek 2 razy (rano i wieczorem).
- Jeśli mylisz 2 usługi, wróć do odpowiedniej tabeli porównawczej.
- Skup się na różnicach: zakres odpowiedzialności, warstwa sieci, model płatności, poziom zarządzania.

### Exam-day strategy (pod pytania podchwytliwe)

- Najpierw szukaj słów‑kluczy w pytaniu: **global/regional**, **L3/L4/L7**, **IaaS/PaaS/SaaS**, **HA/DR**, **RBAC/Policy**.
- Eliminuj odpowiedzi absolutne typu „zawsze”, „nigdy”, jeśli scenariusz dopuszcza wyjątki.
- Jeśli pytanie dotyczy „kto odpowiada”, użyj modelu **Shared Responsibility**.
- Jeśli pytanie dotyczy „najmniej operacji”, wybieraj bardziej zarządzane usługi (PaaS/serverless).
- Zostaw trudne pytania na koniec i wróć po pierwszym przejściu.

---
---

[Poprzednia strona](./21-deployment.md) | [Spis treści](../README.md) | [Następna strona](./23-azure-key-vault.md)
