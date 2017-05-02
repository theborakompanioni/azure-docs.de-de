### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Welche Clientbetriebssysteme kann ich bei Point-to-Site-Verbindungen verwenden?

Folgende Clientbetriebssysteme werden unterstützt:

* Windows 7 (32 Bit und 64 Bit)
* Windows Server 2008 R2 (nur 64 Bit)
* Windows 8 (32 Bit und 64 Bit)
* Windows 8.1 (32 Bit und 64 Bit)
* Windows Server 2012 (nur 64 Bit)
* Windows Server 2012 R2 (nur 64 Bit)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Kann ich für Punkt-zu-Standort-Verbindungen einen beliebigen VPN-Softwareclient mit SSTP-Unterstützung verwenden?

Nein. Nur die oben aufgeführten Windows-Betriebssystemversionen werden unterstützt.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Wie viele VPN-Clientendpunkte kann meine Punkt-zu-Standort-Konfiguration umfassen?

Wir unterstützen bis zu 128 gleichzeitige VPN-Clientverbindungen mit einem virtuellen Netzwerk.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kann ich für Punkt-zu-Standort-Verbindungen meine eigene interne PKI-Stammzertifizierungsstelle verwenden?

Ja. Bisher konnten nur selbstsignierte Stammzertifikate verwendet werden. Sie können weiterhin 20 Stammzertifikate hochladen.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Können Proxys und Firewalls mit der Punkt-zu-Standort-Funktion durchlaufen werden?

Ja. Wir verwenden das Secure Socket Tunneling-Protokoll (SSTP), um eine Tunnelverbindung durch Firewalls zu ermöglichen. Dieser Tunnel wird als HTTPS-Verbindung angezeigt.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Wird die VPN-Verbindung eines für „Punkt zu Standort“ konfigurierten Clientcomputers nach einem Neustart automatisch wiederhergestellt?

Standardmäßig wird die VPN-Verbindung des Clientcomputers nicht automatisch wiederhergestellt.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Werden automatische Verbindungswiederherstellung und DDNS bei Punkt-zu-Standort-Verbindungen auf den VPN-Clients unterstützt?

Automatische Verbindungswiederherstellung und DDNS werden in Punkt-zu-Standort-VPNs derzeit nicht unterstützt.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kann ich im gleichen virtuellen Netzwerk sowohl Standort-zu-Standort- als auch Punkt-zu-Standort-Konfigurationen verwenden?

Ja. Beide Lösungen können verwendet werden, wenn Sie über einen routenbasierten VPN-Typ für Ihr Gateway verfügen. Für das klassische Bereitstellungsmodell benötigen Sie ein dynamisches Gateway. Punkt-zu-Standort-Konfigurationen werden für VPN Gateways mit statischem Routing oder Gateways mit dem `-VpnType PolicyBased`-Cmdlet nicht unterstützt.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kann ich einen Punkt-zu-Standort-Client so konfigurieren, dass er gleichzeitig eine Verbindung mit mehreren virtuellen Netzwerken herstellt?

Ja, das ist möglich. Allerdings dürfen sich weder die IP-Präfixe noch die Punkt-zu-Standort-Adressräume der virtuellen Netzwerke überschneiden.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Wie viel Durchsatz kann ich bei einer Standort-zu-Standort- oder bei einer Punkt-zu-Standort-Verbindung erwarten?

Der genaue Durchsatz der VPN-Tunnel lässt sich nur schwer ermitteln. IPsec und SSTP sind VPN-Protokolle mit hohem Kryptografieaufwand. Außerdem wird der Durchsatz durch die Latenz und die Bandbreite zwischen Ihrem Standort und dem Internet eingeschränkt.