|    | **VPN Gateway-Durchsatz (1)** | **Max. IPsec-Tunnel für VPN Gateway (2)** | **ExpressRoute-Gateway-Durchsatz** | **Gemeinsame Verwendung von VPN-Gateway und ExpressRoute**|
|--- |----------------------------|-----------------------------------|-------------------------------------|-----------------------------------------|
| **Basic-SKU (3)**              |  100 MBit/s | 10                         |  500 MBit/s                           | Nein   |
| **Standard-SKU (4)**           |  100 MBit/s | 10                         | 1.000 MBit/s                           | Ja  |
| **High-Performance-SKU (4)**   | 200 MBit/s  | 30                         | 2.000 MBit/s                           | Ja  |

- (1) Der VPN-Durchsatz ist eine grobe Schätzung, die auf Messungen zwischen den VNets einer Azure-Region basiert. Es ist kein garantierter Durchsatz für standortübergreifende Verbindungen über das Internet. Hierbei wird der maximal mögliche Durchsatz gemessen.
- (2) Die Tunnelanzahl bezieht sich auf routenbasierte VPNs. Ein richtlinienbasiertes VPN kann nur einen Site-to-Site-VPN-Tunnel unterstützen.
- (3) BGP wird für die Basic-SKU nicht unterstützt.
- (4) Richtlinienbasierte VPNs werden für diese SKU nicht unterstützt. Sie werden nur für die Basic-SKU unterstützt.

<!--HONumber=Oct16_HO2-->


