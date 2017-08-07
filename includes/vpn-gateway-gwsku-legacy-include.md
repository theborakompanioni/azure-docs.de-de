Die alten VPN-Gateway-SKUs lauten wie folgt:

* Basic
* Standard
* HighPerformance

VPN Gateway verwendet nicht die UltraPerformance-Gateway-SKU. Informationen zur UltraPerformance-SKU finden Sie in der Dokumentation zu [ExpressRoute](../articles/expressroute/expressroute-about-virtual-network-gateways.md).

Beachten Sie bei Verwendung der alten SKUs Folgendes:

* Wenn Sie einen richtlinienbasierten VPN-Typ verwenden möchten, müssen Sie die Basic-SKU verwenden. Richtlinienbasierte VPNs (früher als statisches Routing bezeichnet) werden in anderen SKUs nicht unterstützt.
* BGP wird in der Basic-SKU nicht unterstützt.
* Die gemeinsame Verwendung von VPN Gateway und ExpressRoute wird in der Basic-SKU nicht unterstützt.
* Aktiv-Aktiv-S2S-VPN-Gatewayverbindungen können nur in der HighPerformance-SKU konfiguriert werden.