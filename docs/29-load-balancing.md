<a id="sec-29-load-balancing"></a>
## 29. Load Balancing w Azure

[ Powrót do spisu treści](../README.md)


Load balancing rozdziela ruch sieciowy między wiele serwerów, zwiększając dostępność i wydajność aplikacji.

### Usługi Load Balancing w Azure

<img src="../assets/lb_azure_services.svg" alt="Azure Load Balancing Services">

| Usługa | Warstwa | Zakres | Protokół | Główne cechy |
|--------|---------|--------|----------|---------------|
| **Azure Load Balancer** | L4 | Regional | TCP/UDP | Ultra-niska latencja |
| **Application Gateway** | L7 | Regional | HTTP/S | WAF, SSL offload, URL routing |
| **Traffic Manager** | DNS | Global | Any | DNS failover, geo-routing |
| **Front Door** | L7 | Global | HTTP/S | CDN + WAF + Global LB |

---

### Algorytmy Load Balancing

#### 1. Round Robin

<img src="../assets/lb_round_robin.svg" alt="Round Robin">

Najprostszy algorytm - requesty rozdzielane równo między serwery po kolei.

- Prosta implementacja, równomierna dystrybucja
- Nie uwzględnia obciążenia serwerów
- Idealny gdy serwery mają identyczną wydajność

**Azure:** Load Balancer (domyślnie), Application Gateway

---

#### 2. Weighted Round Robin

<img src="../assets/lb_weighted_round_robin.svg" alt="Weighted Round Robin">

Round Robin z wagami - serwery z większą wagą otrzymują więcej ruchu.

- Uwzględnia różnice w wydajności serwerów
- Idealny przy heterogenicznych serwerach

**Azure:** Traffic Manager (Weighted), Front Door (Weighted)

```bash
# Traffic Manager z weighted routing
az network traffic-manager endpoint create \
  --resource-group myRG \
  --profile-name myTMProfile \
  --name endpoint-west \
  --type azureEndpoints \
  --target-resource-id /subscriptions/.../westus-app \
  --weight 3

az network traffic-manager endpoint create \
  --resource-group myRG \
  --profile-name myTMProfile \
  --name endpoint-east \
  --type azureEndpoints \
  --target-resource-id /subscriptions/.../eastus-app \
  --weight 1
```

---

#### 3. Least Connections

<img src="../assets/lb_least_connections.svg" alt="Least Connections">

Nowe requesty trafiają do serwera z najmniejszą liczbą aktywnych połączeń.

- Dynamiczne balansowanie obciążenia
- Idealny dla długich połączeń (WebSocket, keep-alive)

**Azure:** Application Gateway

---

#### 4. Source IP Hash (Session Affinity)

<img src="../assets/lb_source_ip_hash.svg" alt="Source IP Hash">

Hash z IP klienta określa docelowy serwer. Ten sam klient zawsze trafia do tego samego serwera.

- Sticky sessions bez cookie
- Problematyczne przy NAT (wielu klientów za jednym IP)

**Azure:** Load Balancer (Source IP), Application Gateway (Cookie Affinity)

```bash
# Load Balancer z session persistence
az network lb rule create \
  --resource-group myRG \
  --lb-name myLB \
  --name myRule \
  --protocol Tcp \
  --frontend-port 80 \
  --backend-port 80 \
  --frontend-ip-name myFrontEnd \
  --backend-pool-name myBackEndPool \
  --load-distribution SourceIP
```

---

#### 5. URL Path-based Routing

<img src="../assets/lb_url_path.svg" alt="URL Path-based Routing">

Routing na podstawie ścieżki URL do różnych backend pools.

- Microservices-friendly
- Różne backendy dla różnych funkcji
- Wymaga L7 load balancera

**Azure:** Application Gateway, Front Door

```bash
# Application Gateway z path-based routing
az network application-gateway url-path-map create \
  --gateway-name myAppGateway \
  --name myPathMap \
  --resource-group myRG \
  --paths /api/* \
  --address-pool apiPool \
  --http-settings apiSettings \
  --default-address-pool defaultPool \
  --default-http-settings defaultSettings

az network application-gateway url-path-map rule create \
  --gateway-name myAppGateway \
  --name imagesRule \
  --resource-group myRG \
  --path-map-name myPathMap \
  --paths /images/* \
  --address-pool imagesPool \
  --http-settings imagesSettings
```

---

#### 6. Priority-based Routing

<img src="../assets/lb_priority.svg" alt="Priority-based Routing">

Ruch kierowany do endpointu o najwyższym priorytecie. Przy awarii przechodzi do następnego.

- Active-Passive failover
- Disaster Recovery scenarios

**Azure:** Traffic Manager (Priority), Front Door (Priority)

```bash
# Traffic Manager z priority routing
az network traffic-manager profile create \
  --resource-group myRG \
  --name myTMProfile \
  --routing-method Priority \
  --unique-dns-name myapp

az network traffic-manager endpoint create \
  --resource-group myRG \
  --profile-name myTMProfile \
  --name primary-endpoint \
  --type azureEndpoints \
  --target-resource-id /subscriptions/.../primary-app \
  --priority 1

az network traffic-manager endpoint create \
  --resource-group myRG \
  --profile-name myTMProfile \
  --name secondary-endpoint \
  --type azureEndpoints \
  --target-resource-id /subscriptions/.../secondary-app \
  --priority 2
```

---

### Inne metody routingu

| Metoda | Opis | Azure Service |
|--------|------|---------------|
| **Performance** | Najniższa latencja do klienta | Traffic Manager |
| **Geographic** | Lokalizacja DNS klienta | Traffic Manager |
| **Multivalue** | Wiele zdrowych endpointów | Traffic Manager |
| **Subnet** | Podsieć klienta | Traffic Manager |

---

### Porównanie usług Azure LB

| Cecha | Load Balancer | App Gateway | Traffic Manager | Front Door |
|-------|---------------|-------------|-----------------|------------|
| **Warstwa** | L4 | L7 | DNS | L7 |
| **Zakres** | Regional | Regional | Global | Global |
| **SSL Offload** | Nie | Tak | Nie | Tak |
| **WAF** | Nie | Tak | Nie | Tak |
| **URL Routing** | Nie | Tak | Nie | Tak |
| **Session Affinity** | Source IP | Cookie | - | Tak |

---

### Scenariusze użycia

| Scenariusz | Rekomendowana usługa |
|------------|----------------------|
| Wewnętrzny ruch między VM | **Load Balancer Internal** |
| Publiczny ruch TCP/UDP non-HTTP | **Load Balancer Public** |
| Aplikacja webowa z WAF | **Application Gateway** |
| Globalna aplikacja web z CDN | **Front Door** |
| DNS failover multi-region | **Traffic Manager** |

---

### Best Practices

- Zawsze konfiguruj custom health probes
- Użyj zone-redundant LB dla HA
- Standard SKU (nie Basic) dla produkcji
- Włącz WAF dla publicznych aplikacji web
- Azure Monitor + Connection Monitor

---

### FAQ - Egzamin

| Pytanie | Odpowiedź |
|---------|----------|
| Load Balancer vs App Gateway? | LB = L4 (TCP/UDP), AppGW = L7 (HTTP) |
| Kiedy Traffic Manager? | Global DNS failover, geo-routing |
| Kiedy Front Door? | Global L7 + CDN + WAF |
| Czy LB obsługuje SSL? | NIE, to L4 - użyj App Gateway |
| Session affinity? | LB = Source IP, AppGW = Cookie |

> **Egzamin:** Load Balancer = L4 (TCP/UDP), regional. Application Gateway = L7 (HTTP/S), WAF, SSL offload. Traffic Manager = DNS global. Front Door = L7 global + CDN + WAF.

---
---

[Poprzednia strona](./28-logic-apps.md) | [Spis treści](../README.md) | [Następna strona](./30-data-factory.md)
