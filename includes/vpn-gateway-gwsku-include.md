Beim Erstellen eines Gateways des virtuellen Netzwerks müssen Sie die gewünschte Gateway-SKU angeben. Wählen Sie die SKUs aus, die Ihre Anforderungen im Bezug auf Workloadtypen, Durchsätze, Funktionen und SLAs erfüllen.

[!INCLUDE [classic SKU](./vpn-gateway-classic-sku-support-include.md)]

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

###  <a name="workloads"></a>Produktion *vs.* Dev-Test-Workloads

Aufgrund von Unterschieden in SLAs und Funktionen werden die folgenden SKUs für die Produktion *bzw.* Dev-Test empfohlen:

| **Workload**                       | **SKUs**               |
| ---                                | ---                    |
| **Produktion, kritische Workloads** | VpnGw1, VpnGw2, VpnGw3 |
| **Dev-Test oder Proof of Concept**   | Basic                  |
|                                    |                        |

Wenn Sie die alten SKUs verwenden, sind die SKU-Empfehlungen für die Produktion die Standard- und HighPerformance-SKUs. Informationen zu den alten SKUs finden Sie im Artikel zu [Gateway-SKUs (alt)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md).

###  <a name="feature"></a>Gateway-SKU-Funktionsgruppen

Die neuen Gateway-SKUs optimieren die auf den Gateways bereitgestellten Funktionen:

| **SKU**| **Funktionen**|
| ---    | ---         |
|Basic   | Routenbasiert: 10 Tunnel mit P2S<br>Richtlinienbasiert (IKEv1): 1 Tunnel; kein P2S|
| VpnGw1, VpnGw2, VpnGw3 |Routenbasiertes VPN mit bis zu 30 Tunneln (*) <br>P2S, BGP, Aktiv/Aktiv, benutzerdefinierte IPsec-/IKE-Richtlinie, Koexistenz von ExpressRoute/VPN |
|        |             |

(*) Sie können „PolicyBasedTrafficSelectors“ konfigurieren, um eine Verbindung zwischen einem routenbasierten VPN-Gateway (VpnGw1, VpnGw2, VpnGw3) und mehreren lokalen richtlinienbasierten Firewallgeräten herzustellen. Ausführliche Informationen finden Sie unter [Connect VPN gateways to multiple on-premises policy-based VPN devices using PowerShell](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md) (Herstellen einer Verbindung zwischen VPN-Gateways und mehreren lokalen richtlinienbasierten VPN-Geräten mit PowerShell).

###  <a name="resize"></a>Ändern der Größe von Gateway-SKUs

1. Sie können die SKU-Größe zwischen VpnGw1, VpnGw2 und VpnGw3 ändern.
2. Bei Verwendung der alten Gateway-SKUs kann die Größe weiterhin zwischen Basic-, Standard- und HighPerformance-SKUs geändert werden.
2. Sie können die Größe **nicht** von Basic/Standard/HighPerformance-SKUs in die neuen VpnGw1/VpnGw2/VpnGw3-SKUs ändern. Sie müssen stattdessen zu den neuen SKUs [migrieren](#migrate).

###  <a name="migrate"></a>Migrieren von alten SKUs zu den neuen SKUs

[!INCLUDE [Migrate SKU](./vpn-gateway-migrate-legacy-sku-include.md)]
