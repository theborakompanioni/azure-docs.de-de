Beim Erstellen eines Gateways des virtuellen Netzwerks müssen Sie die gewünschte Gateway-SKU angeben. Wenn Sie eine höhere Gateway-SKU wählen, werden dem Gateway mehr CPUs und eine höhere Bandbreite zugewiesen. Infolgedessen unterstützt das Gateway einen höheren Netzwerkdurchsatz im virtuellen Netzwerk.

Für VPN-Gateways können die folgenden SKUs verwendet werden:

* Basic
* Standard
* HighPerformance

VPN Gateway verwendet nicht die UltraPerformance-Gateway-SKU. Informationen zur UltraPerformance-SKU finden Sie in der Dokumentation zu [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Berücksichtigen Sie bei der Auswahl einer SKU Folgendes:

* Wenn Sie einen richtlinienbasierten VPN-Typ verwenden möchten, müssen Sie die Basic-SKU verwenden. Richtlinienbasierte VPNs (früher als statisches Routing bezeichnet) werden in anderen SKUs nicht unterstützt.
* BGP wird in der Basic-SKU nicht unterstützt.
* Die gemeinsame Verwendung von VPN Gateway und ExpressRoute wird in der Basic-SKU nicht unterstützt.
* Aktiv-Aktiv-S2S-VPN-Gatewayverbindungen können nur in der HighPerformance-SKU konfiguriert werden.



<!--HONumber=Nov16_HO2-->


