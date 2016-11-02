Beim Erstellen eines Gateways des virtuellen Netzwerks müssen Sie die gewünschte Gateway-SKU angeben. Wenn Sie eine höhere Gateway-SKU wählen, werden dem Gateway mehr CPUs und eine höhere Bandbreite zugewiesen. Infolgedessen unterstützt das Gateway einen höheren Netzwerkdurchsatz im virtuellen Netzwerk.

Für VPN-Gateways können die folgenden SKUs verwendet werden:

- Basic
- Standard
- HighPerformance

Berücksichtigen Sie bei der Auswahl einer SKU die folgenden Einschränkungen:

- Wenn Sie einen richtlinienbasierten VPN-Typ verwenden möchten, müssen Sie die Basic-Gateway-SKU verwenden. Richtlinienbasierte VPNs (früher als statisches Routing bezeichnet) werden in anderen SKUs nicht unterstützt.
- BGP wird in der Basic-SKU nicht unterstützt.
- Die gemeinsame Verwendung von VPN Gateway und ExpressRoute wird in der Basic-SKU nicht unterstützt.


<!--HONumber=Oct16_HO2-->


