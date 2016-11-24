
## <a name="faq---hosting-your-arpa-zone-in-azure-dns"></a>Häufig gestellte Fragen – Hosten der ARPA-Zone in Azure DNS

### <a name="can-i-host-arpa-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a>Kann ich ARPA-Zonen für meine ISP-zugewiesenen IP-Blöcke in Azure DNS hosten?

Ja. Das Hosten der ARPA-Zonen für eigene IP-Adressbereiche in Azure DNS wird vollständig unterstützt.

[Erstellen Sie einfach die Zone in Azure DNS](../articles/dns/dns-getstarted-create-dnszone.md), und wenden Sie sich dann an Ihren Internetdienstanbieter, um [die Zone zu delegieren](../articles/dns/dns-domain-delegation.md).  Sie können dann die PTR-Einträge für jedes Reverse-Lookup auf dieselbe Weise wie andere Eintragstypen verwalten.

Sie können auch [mit der Azure-Befehlszeilenschnittstelle eine vorhandene Reverse-Lookup-Zone importieren](../articles/dns/dns-import-export.md).

### <a name="how-much-does-hosting-my-arpa-zone-cost"></a>Wie viel kostet das Hosten meiner ARPA-Zone?

Das Hosten der ARPA-Zone für Ihren dem ISP zugewiesenen IP-Block in Azure DNS wird mit den [Standardsätzen für Azure DNS](https://azure.microsoft.com/pricing/details/dns/) berechnet.

### <a name="can-i-host-arpa-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a>Kann ich in Azure DNS ARPA-Zonen für IPv4- und IPv6-Adressen hosten?

Ja.


<!--HONumber=Nov16_HO3-->


