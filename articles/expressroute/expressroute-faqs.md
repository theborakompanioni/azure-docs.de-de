---
title: ExpressRoute – FAQ
description: Die FAQs zu ExpressRoute enthalten Informationen zu unterstützten Azure-Diensten, Kosten, Daten und Verbindungen, SLAs, Anbietern und Standorten, Bandbreite sowie zusätzliche technische Details.
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: ''

ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc

---
# <a name="expressroute-faq"></a>ExpressRoute – FAQ
## <a name="what-is-expressroute?"></a>Was ist ExpressRoute?
ExpressRoute ist ein Azure-Dienst, der es Ihnen ermöglicht, private Verbindungen zwischen Microsoft-Datencentern und der Infrastruktur bei Ihnen vor Ort oder in einer Kollokationsumgebung zu erstellen. ExpressRoute-Verbindungen erfolgen nicht über das öffentliche Internet und bieten eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei weniger Latenz als herkömmliche Verbindungen über das Internet.

### <a name="what-are-the-benefits-of-using-expressroute-and-private-network-connections?"></a>Was sind die Vorteile von ExpressRoute und privaten Netzwerkverbindungen?
ExpressRoute-Verbindungen erfolgen nicht über das öffentliche Internet und bieten eine höhere Sicherheit, größere Zuverlässigkeit und schnellere Geschwindigkeit bei konstanter Latenz als herkömmliche Verbindungen über das Internet. In einigen Fällen können durch die Verwendung von ExpressRoute-Verbindungen zum Übertragen von Daten zwischen lokalen Geräten und Azure auch drastische Kosteneinsparungen erzielt werden.

### <a name="what-microsoft-cloud-services-are-supported-over-expressroute?"></a>Welche Microsoft-Clouddienste werden von ExpressRoute unterstützt?
ExpressRoute unterstützt die meisten der derzeit verfügbaren Microsoft Azure-Dienste, z. B. Office 365.  Achten Sie in nächster Zeit auf Updates hinsichtlich der allgemeinen Verfügbarkeit.

### <a name="where-is-the-service-available?"></a>Wo ist der Dienst verfügbar?
Auf der folgenden Seite finden Sie Informationen bezüglich der Dienststandorte und Verfügbarkeit: [ExpressRoute-Partner und -Standorte](expressroute-locations.md).

### <a name="how-can-i-use-expressroute-to-connect-to-microsoft-if-i-don’t-have-partnerships-with-one-of-the-expressroute-carrier-partners?"></a>Wie kann ich ExpressRoute verwenden, um eine Verbindung mit Microsoft herzustellen, wenn ich keine Partnerschaft mit einem der ExpressRoute-Anbieterpartner habe?
Sie können einen regionalen Netzbetreiber auswählen und Ethernetverbindungen mit einem der unterstützten Exchange-Anbieterstandorte herstellen. Am Anbieterstandort können Sie dann eine Peeringverbindung mit Microsoft herstellen. Überprüfen Sie den letzten Abschnitt von [ExpressRoute-Partner und -Standorte](expressroute-locations.md) , um herauszufinden, ob Ihr Dienstanbieter an einem der Exchange-Standorte vorhanden ist. Sie können dann eine ExpressRoute-Verbindung über den Dienstanbieter anfordern, um eine Verbindung mit Azure herzustellen.

### <a name="how-much-does-expressroute-cost?"></a>Wie viel kostet ExpressRoute?
Preisinformationen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/expressroute/) .

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth,-does-the-vpn-connection-i-purchase-from-my-network-service-provider-have-to-be-the-same-speed?"></a>Wenn ich für eine ExpressRoute-Verbindung mit einer bestimmten Bandbreite bezahle, muss die VPN-Verbindung, die ich von meinem Netzwerkdienstanbieter erworben habe, die gleiche Geschwindigkeit haben?
Nein. Sie können eine VPN-Verbindung beliebiger Geschwindigkeit bei Ihrem Dienstanbieter erwerben. Die Verbindung mit Azure wird jedoch auf die von Ihnen erworbene Bandbreite der ExpressRoute-Verbindung begrenzt.

### <a name="if-i-pay-for-an-expressroute-circuit-of-a-given-bandwidth,-do-i-have-the-ability-to-burst-up-to-higher-speeds-if-required?"></a>Wenn ich für eine ExpressRoute-Verbindung mit einer bestimmten Bandbreite bezahle, habe ich die Möglichkeit, bei Bedarf höhere Geschwindigkeiten zu nutzen?
Ja. ExpressRoute-Verbindungen sind so konfiguriert, dass sie Situationen unterstützen, in denen Sie das erworbene Bandbreitenlimit um das Doppelte überschreiten können, ohne dass zusätzliche Kosten anfallen. Fragen Sie bei Ihrem Dienstanbieter an, ob er diese Funktion unterstützt.

### <a name="can-i-use-the-same-private-network-connection-with-virtual-network-and-other-azure-services-simultaneously?"></a>Kann ich die gleiche private Netzwerkverbindung gleichzeitig mit Virtual Network und anderen Azure-Diensten verwenden?
Ja. Sobald eine ExpressRoute-Verbindung eingerichtet ist, können Sie gleichzeitig auf Dienste in einem virtuellen Netzwerk und in anderen Azure-Diensten zugreifen. Über einen privaten Peeringpfad können Sie eine Verbindung mit virtuellen Netzwerken herstellen, und über den öffentlichen Peeringpfad stellen Sie die Verbindung mit anderen Diensten her.

### <a name="does-expressroute-offer-a-service-level-agreement-(sla)?"></a>Bietet ExpressRoute eine Service Level Agreement (SLA)?
Auf der Seite [Vereinbarungen zum Servicelevel (SLAs) für ExpressRoute](https://azure.microsoft.com/support/legal/sla/) finden Sie dazu weitere Informationen.

## <a name="supported-services"></a>Unterstützte Dienste
ExpressRoute unterstützt die meisten Azure-Dienste.

* Verbindungen mit virtuellen Computern und Clouddiensten, die in virtuellen Netzwerken bereitgestellt werden, werden über den privaten Peeringpfad unterstützt.
* Azure Websites werden über den öffentlichen Peeringpfad unterstützt.
* IoT Hub wird über den öffentlichen Peeringpfad unterstützt.
* Office 365 wird über den Microsoft-Peeringpfad unterstützt.
* Alle anderen Dienste sind über den öffentlichen Peeringpfad zugänglich. Es gibt folgende Ausnahmen.
  
    **Die folgenden Dienste werden nicht unterstützt:**
  
  * CDN
  * Visual Studio Team Services-Auslastungstests
  * Multi-Factor Authentication
  * Traffic Manager

## <a name="data-and-connections"></a>Daten und Verbindungen
### <a name="are-there-limits-on-the-amount-of-data-that-i-can-transfer-using-expressroute?"></a>Gibt es Limits hinsichtlich der Datenmenge, die ich mithilfe von ExpressRoute übertragen kann?
Wir haben keine Limits hinsichtlich der übertragenen Datenmenge festgelegt. In der [Preisübersicht](https://azure.microsoft.com/pricing/details/expressroute/) finden Sie Informationen zu Bandbreitentarifen.

### <a name="what-connection-speeds-are-supported-by-expressroute?"></a>Welche Verbindungsgeschwindigkeiten werden von ExpressRoute unterstützt?
Unterstützte Bandbreiten:

|50 MBit/s, 100 MBit/s, 200 MBit/s, 500 MBit/s, 1 GBit/s, 2 GBit/s, 5 GBit/s, 10 GBit/s|

### <a name="which-service-providers-are-available?"></a>Welche Dienstanbieter sind verfügbar?
Eine Liste der Dienstanbieter und Standorte finden Sie unter [ExpressRoute-Partner und -Standorte](expressroute-locations.md) .

## <a name="technical-details"></a>Technische Details
### <a name="what-are-the-technical-requirements-for-connecting-my-on-premises-location-to-azure?"></a>Was sind die technischen Voraussetzungen für die Verbindung meines lokalen Standorts mit Azure?
Die Voraussetzungen finden Sie auf der Seite [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md) .

### <a name="are-connections-to-expressroute-redundant?"></a>Sind Verbindungen mit ExpressRoute redundant?
Ja. Jede ExpressRoute-Verbindung verfügt über ein redundantes Paar von Querverbindungen mit hoher Verfügbarkeit.

### <a name="will-i-lose-connectivity-if-one-of-my-expressroute-links-fail?"></a>Geht die Konnektivität verloren, wenn einer meiner ExpressRoute-Links ausfällt?
Wenn eine der Querverbindungen ausfällt, bleibt die Konnektivität bestehen. Es steht eine redundante Verbindung zur Verfügung, um die Auslastung des Netzwerks zu unterstützen. Sie können zudem mehrere Verbindungen an einem anderen Peeringstandort erstellen, um die Ausfallsicherheit bei einem Fehler sicherzustellen.

### <a name="<a-name="onep2plink"></a>if-i'm-not-co-located-at-a-cloud-exchange-and-my-service-provider-offers-point-to-point-connection,-do-i-need-to-order-two-physical-connections-between-my-on-premises-network-and-microsoft?"></a><a name="onep2plink"></a>Wenn ich mich nicht am selben Standort wie ein Cloud Exchange befinde und mein Dienstanbieter Punkt-zu-Punkt-Verbindungen bereitstellt, muss ich zwei physische Verbindungen zwischen meinem lokalen Netzwerk und Microsoft anfordern?
Nein, Sie benötigen nur eine einzige physische Verbindung, wenn Ihr Dienstanbieter zwei virtuelle Ethernet-Verbindungen über die physische Verbindung herstellen kann. Die physische Verbindung (z. B. Glasfaser) wird auf einem Gerät der Ebene 1 (L1) beendet (siehe Abbildung unten). Die zwei virtuellen Ethernet-Verbindungen werden mit unterschiedlichen VLAN-IDs markiert, eine für die primäre Verbindung und eine für die sekundäre Verbindung. Diese VLAN-IDs befinden sich im äußeren 802.1Q-Ethernet-Header. Der innere 802.1Q-Ethernet-Header (nicht dargestellt) wird einer bestimmten [ExpressRoute-Routingdomäne](expressroute-circuit-peerings.md) zugeordnet. 

![](./media/expressroute-faqs/expressroute-p2p-ref-arch.png)

### <a name="can-i-extend-one-of-my-vlans-to-azure-using-expressroute?"></a>Kann ich eines meiner VLANs mithilfe von ExpressRoute auf Azure erweitern?
Nein. Wir unterstützen keine Schicht-2-Konnektivitätserweiterungen in Azure.

### <a name="can-i-have-more-than-one-expressroute-circuit-in-my-subscription?"></a>Kann mein Abonnement mehr als eine ExpressRoute-Verbindung umfassen?
Ja. Ihr Abonnement kann mehr als eine ExpressRoute-Verbindung enthalten. Das Limit für die Anzahl dedizierter Verbindungen ist standardmäßig auf 10 festgelegt. Wenden Sie sich an Microsoft Support, um das Limit bei Bedarf zu erhöhen.

### <a name="can-i-have-expressroute-circuits-from-different-service-providers?"></a>Kann ich ExpressRoute-Verbindungen von verschiedenen Dienstanbietern haben?
Ja. Sie können ExpressRoute-Verbindungen von verschiedenen Dienstanbietern verwenden. Jede ExpressRoute-Verbindung wird nur einem Dienstanbieter zugeordnet.

### <a name="how-do-i-connect-my-virtual-networks-to-an-expressroute-circuit"></a>Wie kann ich meine Virtual Networks mit einer ExpressRoute-Verbindung verbinden?
Die grundlegenden Schritte werden im Folgenden beschrieben.

* Sie müssen eine ExpressRoute-Verbindung herstellen, die vom Dienstanbieter aktiviert werden muss.
* Die BGP-Peerings müssen von Ihnen oder vom Anbieter konfiguriert werden.
* Sie müssen das virtuelle Netzwerk mit der ExpressRoute-Verbindung verknüpfen.

Weitere Informationen finden Sie unter [ExpressRoute-Workflows für die Verbindungsbereitstellung und Verbindungszustände](expressroute-workflows.md) .

### <a name="are-there-connectivity-boundaries-for-my-expressroute-circuit?"></a>Gibt es Konnektivitätsgrenzen für meine ExpressRoute-Verbindung?
Ja. [ExpressRoute-Partner und -Standorte](expressroute-locations.md) bietet eine Übersicht über die Konnektivitätsgrenzen für eine ExpressRoute-Verbindung. Die Konnektivität für eine ExpressRoute-Verbindung ist auf eine einzelne geopolitische Region beschränkt. Die Konnektivität kann auf verschiedene geopolitische Regionen erweitert werden, wenn Sie das ExpressRoute Premium-Feature aktivieren.

### <a name="can-i-link-to-more-than-one-virtual-network-to-an-expressroute-circuit?"></a>Kann ich mehr als ein virtuelles Netzwerk mit einer ExpressRoute-Verbindung verknüpfen?
Ja. Sie können bis zu 10 virtuelle Netzwerke mit einer ExpressRoute-Verbindung verknüpfen.

### <a name="i-have-multiple-azure-subscriptions-that-contain-virtual-networks.-can-i-connect-virtual-networks-that-are-in-separate-subscriptions-to-a-single-expressroute-circuit?"></a>Ich habe mehrere Azure-Abonnements, die Virtual Networks enthalten. Kann ich Virtual Networks, die sich in unterschiedlichen Abonnements befinden, mit einer ExpressRoute-Verbindung verbinden?
Ja. Sie können bis zu 10 andere Azure-Abonnements autorisieren, eine einzige ExpressRoute-Verbindung zu verwenden. Dieses Limit kann durch das Aktivieren des ExpressRoute Premium-Features erhöht werden.

Weitere Informationen finden Sie unter [Freigeben einer ExpressRoute-Verbindung für mehrere Abonnements](expressroute-howto-linkvnet-arm.md).

### <a name="are-virtual-networks-connected-to-the-same-circuit-isolated-from-each-other?"></a>Sind virtuelle Netzwerke, die mit der gleichen Verbindung verbunden sind, voneinander isoliert?
Nein. Alle virtuellen Netzwerke, die mit der gleichen ExpressRoute-Verbindung verknüpft sind, sind Teil der gleichen Routingdomäne und aus Routingsicht nicht voneinander isoliert. Wenn Sie eine Isolierung der Routen benötigen, müssen Sie zuerst eine separate ExpressRoute-Verbindung erstellen.

### <a name="can-i-have-one-virtual-network-connected-to-more-than-one-expressroute-circuit?"></a>Kann mehr als ein virtuelles Netzwerk mit mehr als einer ExpressRoute-Verbindung verbunden sein?
Ja. Sie können ein einzelnes virtuelles Netzwerk mit bis zu 4 ExpressRoute-Verbindungen verknüpfen. Sie müssen über vier unterschiedliche [ExpressRoute-Standorte](expressroute-locations.md)angefordert werden.

### <a name="can-i-access-the-internet-from-my-virtual-networks-connected-to-expressroute-circuits?"></a>Kann ich von meinen virtuellen Netzwerken aus, die mit ExpressRoute-Verbindungen verbunden sind, ins Internet gehen?
Ja. Wenn Sie über die BGP-Sitzung keine Standardrouten (0.0.0.0/0) oder Internet-Routepräfixe angekündigt haben, können Sie von einem virtuellen Netzwerk aus, das mit einer ExpressRoute-Verbindung verknüpft ist, eine Verbindung zum Internet herstellen.

### <a name="can-i-block-internet-connectivity-to-virtual-networks-connected-to-expressroute-circuits?"></a>Kann ich die Verbindung zum Internet für virtuelle Netzwerke blockieren, die mit ExpressRoute-Verbindungen verbunden sind?
Ja. Sie können Standardrouten (0.0.0.0/0) ankündigen, um die Internetkonnektivität von virtuellen Computern in einem virtuellen Netzwerk zu blockieren und den gesamten Datenverkehr über die ExpressRoute-Verbindung zu leiten. Beachten Sie, dass wir den Datenverkehr an Dienste, die über öffentliches Peering (z. B. Azure Storage und SQL-Datenbank) angeboten werden, wieder an Ihren Standort zurückleiten, wenn Sie keine Standardrouten ankündigen. Sie müssen Ihre Router so konfigurieren, dass der Datenverkehr über den öffentlichen Peeringpfad oder über das Internet an Azure zurückgegeben wird.

### <a name="can-virtual-networks-linked-to-the-same-expressroute-circuit-talk-to-each-other?"></a>Können virtuelle Netzwerke mit der gleichen ExpressRoute-Verbindung verknüpft sein und miteinander kommunizieren?
Ja. Virtuelle Maschinen in virtuellen Netzwerken, die mit der gleichen ExpressRoute-Verbindung verbunden sind, können miteinander kommunizieren.

### <a name="can-i-use-site-to-site-connectivity-for-virtual-networks-in-conjunction-with-expressroute?"></a>Kann ich für Virtual Networks Standort-zu-Standort-Konnektivität zusammen mit ExpressRoute verwenden?
Ja. ExpressRoute kann zusammen mit Standort-zu-Standort-VPNs verwendet werden.

### <a name="can-i-move-a-virtual-network-from-site-to-site-/-point-to-site-configuration-to-use-expressroute?"></a>Kann ich ein virtuelles Netzwerk mit einer Standort-zu-Standort-/Punkt-zu-Standort-Konfiguration so konfigurieren, dass es ExpressRoute verwendet?
Ja. Sie müssen in Ihrem virtuellen Netzwerk ein ExpressRoute-Gateway erstellen. Bei diesem Vorgang kommt es zu einer kurzen Dienstunterbrechung.

### <a name="what-do-i-need-to-connect-to-azure-storage-over-expressroute?"></a>Was brauche ich, um über ExpressRoute eine Verbindung mit Azure Storage herzustellen?
Sie müssen eine ExpressRoute-Verbindung einrichten und Routen für öffentliches Peering konfigurieren.

### <a name="are-there-limits-on-the-number-of-routes-i-can-advertise?"></a>Gibt es Limits hinsichtlich der Anzahl von Routen, die ich ankündigen kann?
Ja. Wir akzeptieren bis zu 4.000 Routenpräfixe für privates Peering und je 200 für öffentliches Peering und Microsoft-Peering. Sie können diesen Wert auf 10.000 Routen für privates Peering erhöhen, wenn Sie für ExpressRoute das Premium-Feature aktivieren.

### <a name="are-there-restrictions-on-ip-ranges-i-can-advertise-over-the-bgp-session?"></a>Gibt es Einschränkungen hinsichtlich der IP-Adressbereiche, die ich über die BGP-Sitzung ankündigen kann?
Wir akzeptieren keine privaten Präfixe (RFC1918) in der öffentlichen und Microsoft-BGP-Peeringsitzung.

### <a name="what-happens-if-i-exceed-the-bgp-limits?"></a>Was geschieht, wenn ich die BGP-Limits überschreite?
BGP-Sitzungen werden abgebrochen. Sie werden zurückgesetzt, sobald die Präfixanzahl unter das Limit fällt.

### <a name="what-is-the-expressroute-bgp-hold-time?-can-it-be-adjusted?"></a>Wie lang ist die BGP-Haltezeit für ExpressRoute? Kann sie angepasst werden?
Die Haltezeit ist 180. Die Keep-Alive-Nachrichten werden alle 60 Sekunden gesendet. Diese Einstellungen werden auf Microsoft-Seite festgelegt und können nicht geändert werden.

### <a name="after-i-advertise-the-default-route-(0.0.0.0/0)-to-my-virtual-networks,-i-can't-activate-windows-running-on-my-azure-vms.-how-to-i-fix-this?"></a>Nachdem ich die Standardroute (0.0.0.0/0) für meine Virtual Networks angekündigt habe, kann ich Windows, das auf meinen Azure-VMs ausgeführt wird, nicht mehr aktivieren. Wie kann ich dieses Problem beheben?
Die folgenden Schritte helfen Azure dabei, die Aktivierungsanforderung zu erkennen:

1. Richten Sie für die ExpressRoute-Verbindung öffentliches Peering ein.
2. Führen Sie ein DNS-Lookup durch, und suchen Sie die IP-Adresse von **kms.core.windows.net**
3. Führen Sie eines der beiden folgenden Elemente aus, damit der Schlüsselverwaltungsdienst erkennt, dass die Aktivierungsanforderung von Azure stammt und die Anforderung berücksichtigt.
   * Leiten Sie in Ihrem lokalen Netzwerk den Datenverkehr für die IP-Adresse (abgerufen in Schritt 2) über das öffentliche Peering zurück an Azure.
   * Fordern Sie Ihren Netzwerkdienstanbieter auf, den Datenverkehr über das öffentliche Peering wieder an Azure zurückzuleiten.

### <a name="can-i-change-the-bandwidth-of-an-expressroute-circuit?"></a>Kann ich die Bandbreite einer ExpressRoute-Verbindung ändern?
Ja. Die Bandbreite einer ExpressRoute-Verbindung kann erhöht werden, ohne sie beenden zu müssen. Wenden Sie sich an Ihren Konnektivitätsanbieter, um sicherzustellen, dass er die Drosselungen in seinen Netzwerken aktualisiert, um die Erhöhung der Bandbreite zu unterstützen. Sie können die Bandbreite einer ExpressRoute-Verbindung jedoch nicht reduzieren. Wenn Sie die Bandbreite verringern müssen, wird die ExpressRoute-Verbindung beendet und anschließend neu erstellt.

### <a name="how-do-i-change-the-bandwidth-of-an-expressroute-circuit?"></a>Wie kann ich die Bandbreite einer ExpressRoute-Verbindung ändern?
Sie können die Bandbreite der ExpressRoute-Verbindung mithilfe der API zum Aktualisieren der dedizierten Verbindung und des PowerShell-Cmdlets aktualisieren.

## <a name="expressroute-premium"></a>ExpressRoute Premium
### <a name="what-is-expressroute-premium?"></a>Was ist ExpressRoute Premium?
ExpressRoute Premium ist eine Sammlung der unten aufgeführten Features.

* Erhöhung des Grenzwerts für Routingtabellen von 4.000 auf 10.000 Routen für privates Peering.
* Größere Anzahl an VNets, die mit der ExpressRoute-Verbindung verbunden werden können. (Der Standardwert ist 10.) Weitere Details finden Sie in der Tabelle unten.
* Globale Konnektivität über das Microsoft-Kernnetzwerk. Jetzt können Sie ein VNet in einer geopolitischen Region mit einer ExpressRoute-Verbindung in einer anderen Region verknüpfen. **Beispiel:** Sie können ein in Westeuropa erstelltes VNet mit einer ExpressRoute-Verbindung verknüpfen, die im Silicon Valley erstellt wurde.
* Verbindung mit Office 365-Diensten und CRM Online.

### <a name="how-many-vnets-can-i-link-to-an-expressroute-circuit-if-i-enabled-expressroute-premium?"></a>Wie viele VNets kann ich mit einer ExpressRoute-Verbindung verknüpfen, wenn ich ExpressRoute Premium aktiviert habe?
Die folgenden Tabellen enthalten die ExpressRoute-Grenzwerte und die Anzahl von VNets pro ExpressRoute-Verbindung.

[!INCLUDE [expressroute-limits](../../includes/expressroute-limits.md)]

### <a name="how-do-i-enable-expressroute-premium?"></a>Wie aktiviere ich ExpressRoute Premium?
Die Features von ExpressRoute Premium können aktiviert werden, sobald Sie das Feature selbst aktiviert haben. Durch Aktualisieren des Verbindungsstatus können sie beendet werden. Sie können ExpressRoute Premium zum Zeitpunkt der Verbindungserstellung aktivieren, oder Sie können die API zum Aktualisieren der dedizierten Verbindung bzw. das PowerShell-Cmdlet aufrufen, um ExpressRoute Premium zu aktivieren.

### <a name="how-do-i-disable-expressroute-premium?"></a>Wie deaktiviere ich ExpressRoute Premium?
Sie können ExpressRoute Premium deaktivieren, indem Sie die API zum Aktualisieren der dedizierten Verbindung/das PowerShell-Cmdlet aufrufen. Stellen Sie sicher, dass Sie Ihre Konnektivitätsanforderungen skaliert haben, um die Standardlimits zu erfüllen, bevor Sie ExpressRoute Premium deaktivieren. Die Anforderung ExpressRoute Premium zu deaktivieren wird storniert, wenn Ihre Nutzung über dem Standardlimit liegt.

### <a name="can-i-pick-and-choose-the-features-i-want-from-the-premium-feature-set?"></a>Kann ich die Features der Premium-Features auswählen, die ich nutzen möchte?
Nein. Sie können keine einzelnen Features auswählen. Wenn Sie ExpressRoute Premium aktivieren, werden alle Features aktiviert.

### <a name="how-much-does-expressroute-premium-cost?"></a>Wie viel kostet ExpressRoute Premium?
Angaben zu den Preisen finden Sie in der [Preisübersicht](https://azure.microsoft.com/pricing/details/expressroute/) .

### <a name="do-i-pay-for-expressroute-premium-in-addition-to-standard-expressroute-charges?"></a>Fallen für ExpressRoute Premium neben dem standardmäßigen ExpressRoute-Tarif zusätzliche Gebühren an?
Ja. Für ExpressRoute Premium werden neben den ExpressRoute-Verbindungsgebühren und den Gebühren, die vom Konnektivitätsanbieter erhoben werden, zusätzliche Gebühren berechnet.

## <a name="expressroute-and-office-365-services-and-crm-online"></a>ExpressRoute mit Office 365-Diensten und CRM Online
[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

### <a name="how-do-i-create-an-expressroute-circuit-to-connect-to-office-365-services-and-crm-online?"></a>Wie erstelle ich eine ExpressRoute-Verbindung mit Office 365-Diensten und CRM Online?
1. Überprüfen Sie die Anforderungen auf der Seite mit den [ExpressRoute-Voraussetzungen](expressroute-prerequisites.md), um sicherzustellen, dass Sie die Anforderungen erfüllen.
2. Überprüfen Sie die Liste der Dienstanbieter und Standorte unter [ExpressRoute-Partner und -Standorte](expressroute-locations.md) , um sicherzustellen, dass Ihre Konnektivitätsanforderungen erfüllt werden.
3. Planen Sie Ihre Kapazitätsanforderungen mithilfe der Informationen unter [Netzwerkplanung und Leistungsoptimierung für Office 365](http://aka.ms/tune/).
4. Befolgen Sie die Schritte in den folgenden Workflows zum Einrichten der Verbindung: [ExpressRoute-Workflows für die Verbindungsbereitstellung und Verbindungszustände](expressroute-workflows.md).

> [!IMPORTANT]
> Stellen Sie sicher, dass das ExpressRoute Premium-Add-On aktiviert ist, wenn Sie eine Verbindung mit Office 365-Diensten und CRM Online konfigurieren.
> 
> 

### <a name="do-i-need-to-enable-azure-public-peering-to-connect-to-office-365-services-and-crm-online?"></a>Muss ich Azure Public Peering aktivieren, um eine Verbindung mit Office 365-Diensten und CRM Online herzustellen?
Nein, Sie müssen nur Microsoft-Peering aktivieren. Der Authentifizierungsdatenverkehr an Azure AD wird über Microsoft-Peering gesendet. 

### <a name="can-my-existing-expressroute-circuits-support-connectivity-to-office-365-services-and-crm-online?"></a>Unterstützen meine vorhandenen ExpressRoute-Verbindungen die Konnektivität mit Office 365-Diensten und CRM Online?
Ja. Ihre bestehende ExpressRoute-Verbindung kann so konfiguriert werden, dass sie eine Konnektivität mit Office 365-Diensten unterstützt. Stellen Sie sicher, dass Sie über ausreichend Kapazität für die Verbindung mit Office 365-Diensten verfügen, und vergewissern Sie sich, dass das Premium-Add-On aktiviert ist. [Netzwerkplanung und Leistungsoptimierung für Office 365](http://aka.ms/tune/) helfen Ihnen dabei, Ihre Konnektivitätsanforderungen zu planen. Siehe auch [Erstellen und Ändern einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md).

### <a name="what-office-365-services-can-be-accessed-over-an-expressroute-connection?"></a>Welche Office 365-Dienste sind über eine ExpressRoute-Verbindung zugänglich?
Eine aktuelle Liste der von ExpressRoute unterstützten Dienste finden Sie unter [URLs und IP-Adressbereiche von Office 365](http://aka.ms/o365endpoints) .

### <a name="how-much-does-expressroute-for-office-365-services-and-crm-online-cost?"></a>Wie viel kosten ExpressRoute für Office 365-Dienste und CRM Online?
Office 365-Dienste und CRM Online erfordern ein aktiviertes Premium-Add-On. In der [Preisübersicht](https://azure.microsoft.com/pricing/details/expressroute/) finden Sie Details zu den Kosten für ExpressRoute.

### <a name="what-regions-is-expressroute-for-office-365-supported-in?"></a>In welchen Regionen wird ExpressRoute für Office 365 unterstützt?
Weitere Informationen zur Liste der Partner und Standorte, die ExpressRoute unterstützen, finden Sie unter [ExpressRoute-Partner und -Standorte](expressroute-locations.md) .

### <a name="can-i-access-office-365-over-the-internet-even-if-expressroute-was-configured-for-my-organization?"></a>Kann ich über das Internet auf Office 365 zugreifen, selbst wenn für meine Organisation ExpressRoute konfiguriert wurde?
Ja. Office 365-Dienstendpunkte sind über das Internet erreichbar, selbst wenn ExpressRoute für Ihr Netzwerk konfiguriert wurde. Wenn Sie sich an einem Ort befinden, an dem die Verbindung mit Office 365-Diensten über ExpressRoute konfiguriert wurde, wird die Verbindung über ExpressRoute hergestellt.

### <a name="can-dynamics-ax-online-be-accessed-over-an-expressroute-connection?"></a>Kann ich über eine ExpressRoute-Verbindung auf Dynamics AX Online zugreifen?
Nein, dies wird nicht unterstützt.

<!--HONumber=Oct16_HO2-->


