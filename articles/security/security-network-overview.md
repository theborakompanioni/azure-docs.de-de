<properties
   pageTitle="Azure Network Security im Überblick | Microsoft Azure"
   description=" Dieser Artikel erläutert Ihnen das Angebot von Microsoft Azure im Bereich Netzwerksicherheit. Wir bieten grundlegende Erklärungen zu Kernkonzepten und Anforderungen im Bereich Netzwerksicherheit und Informationen über die Vorteile von Azure in jedem dieser Bereiche. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# Die Netzwerksicherheit in Azure im Überblick

Microsoft Azure verfügt über eine robuste Netzwerkinfrastruktur zur Unterstützung der Konnektivitätsanforderungen Ihrer Anwendungen und Dienste. Netzwerkkonnektivität ist zwischen Ressourcen in Azure, zwischen lokalen und in Azure gehosteten Ressourcen und zu und aus dem Internet und Azure möglich.

Das Ziel dieses Artikel ist, Ihnen das Angebot von Microsoft Azure im Bereich Netzwerksicherheit näherzubringen. Nachstehend finden Sie grundlegende Erklärungen für einige zentrale Konzepte im Bereich Netzwerksicherheit und die entsprechenden Anforderungen. Darüber hinaus erfahren Sie, was Azure in jedem dieser Bereiche zu bieten hat. Der Text enthält zahlreiche Links zu weiteren Artikeln. So erlangen Sie ein tieferes Verständnis für die Bereiche, die Sie interessieren.

Die „Übersicht über die Netzwerksicherheit in Azure“ wird sich auf folgende Themen konzentrieren:

- Azure-Netzwerke
- Die Netzwerkzugriffssteuerung
- Ein sicherer Remotezugriff und standortübergreifende Konnektivität
- Availability
- Protokollierung
- Namensauflösung
- DMZ-Architektur
- Azure Security Center

## Azure-Netzwerke

Virtuelle Computer benötigen Netzwerkkonnektivität. Azure erfordert von einem virtuellen Computer eine Verbindung mit einem virtuellen Azure-Netzwerk, damit die Konnektivitätsanforderung unterstützt wird. Ein virtuelles Azure-Netzwerk ist ein logisches Konstrukt, das auf dem physischen Azure-Netzwerkfabric basiert. Jedes logische virtuelle Azure-Netzwerk ist von allen anderen virtuellen Azure-Netzwerken isoliert. Dadurch wird sichergestellt, dass der Netzwerkverkehr in Ihren Bereitstellungen nicht für andere Microsoft Azure-Kunden zugänglich ist.

Weitere Informationen:

- [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md)

## Die Netzwerkzugriffssteuerung
Netzwerkzugriffssteuerung bedeutet, die Konnektivität zu oder ausgehend von bestimmten Geräten oder Subnetzen innerhalb eines virtuellen Azure-Netzwerks zu begrenzen. Das Ziel der Netzwerkzugriffssteuerung ist sicherzustellen, dass Ihre virtuellen Computer und Dienste nur denjenigen Benutzern und Geräten zugänglich sind, denen Sie den Zugriff auch erlauben möchten. Zugriffssteuerungen basieren auf den Entscheidungen, Verbindungen zu und ausgehend von virtuellen Computern oder Diensten zuzulassen oder zu verweigern.

Azure unterstützt verschiedene Typen von Netzwerkzugriffssteuerungen. Diese umfassen:

- Die Steuerung der Vermittlungsschicht
- Routensteuerung und Tunnelerzwingung
- Appliances für die Sicherheit des virtuellen Netzwerks

### Die Steuerung der Vermittlungsschicht
Jede sichere Bereitstellung erfordert ein gewisses Maß an Netzwerkzugriffssteuerung. Das Ziel der Netzwerkzugriffssteuerung ist sicherzustellen, dass Ihre virtuellen Computer und die Netzwerkdienste, die auf diesen virtuellen Computern ausgeführt werden, ausschließlich mit den Netzwerkgeräten kommunizieren können, mit denen sie auch kommunizieren müssen, während alle anderen Verbindungsversuche blockiert werden.

Wenn Sie eine einfache Zugriffskontrolle auf Netzwerkebene benötigen (basierend auf IP-Adresse und TCP- oder UDP-Protokollen), können Sie Netzwerksicherheitsgruppen verwenden. Eine Netzwerksicherheitsgruppe (NSG) ist eine einfache Firewall, die zustandsbehaftete Pakete filtert und die Zugriffssteuerung auf Grundlage eines [5-Tupels](https://www.techopedia.com/definition/28190/5-tuple) ermöglicht. NSGs bieten weder eine Inspektion auf Anwendungsebene noch authentifizierte Zugriffssteuerungen.

Weitere Informationen:

- [Netzwerksicherheitsgruppen](../virtual-network/virtual-networks-nsg.md)

### Routensteuerung und Tunnelerzwingung
Die Steuerung des Routingverhaltens in Ihren virtuellen Azure-Netzwerken ist eine entscheidende Funktion in den Bereichen Netzwerksicherheit und Zugriffssteuerung. Wenn das Routing nicht ordnungsgemäß konfiguriert ist, können sich auf dem virtuellen Computer gehostete Anwendungen und Dienste mit unerwünschten Geräten verbinden. Dies können auch Geräten sein, die sich im Besitz potenzieller Angreifer befinden und von ihnen betrieben werden.

Azure-Netzwerke unterstützen die Fähigkeit, das Routingverhalten für den Netzwerkverkehr in Ihrem virtuellen Azure-Netzwerk benutzerdefiniert anzupassen. Dadurch können Sie die Standardeinträge in der Routingtabelle in Ihrem virtuellen Azure-Netzwerk ändern. Mithilfe der Steuerung des Routingverhaltens können Sie sicherstellen, dass der gesamte von einem bestimmten Gerät oder einer Gerätegruppe ausgehende Datenverkehr über einen bestimmten Punkt in Ihr virtuelles Azure-Netzwerk eintritt oder es über diesen verlässt.

Beispielsweise verfügen Sie in Ihrem virtuellen Azure-Netzwerk möglicherweise über eine Appliance für die Sicherheit des virtuellen Netzwerks. In diesem Fall möchten Sie auch gewährleisten, dass der gesamte Datenverkehr zu und ausgehend von Ihrem virtuellen Azure-Netzwerk Ihre Appliance für die Sicherheit Ihres virtuellen Netzwerks passiert. Sie erreichen dies über die Konfiguration der [benutzerdefinierten Routen](../virtual-network/virtual-networks-udr-overview.md) in Azure.

Mithilfe der [Tunnelerzwingung](https://www.petri.com/azure-forced-tunneling) können Sie sicherstellen, dass Ihre Dienste nicht über die Erlaubnis verfügen, eine Verbindung mit Geräten im Internet zu initiieren. Beachten Sie, dass sich dieser Mechanismus von der Erlaubnis eingehender Verbindungen und der Antwort auf diese unterscheidet. Front-End-Webserver müssen auf Anforderungen von Internethosts antworten. Aus dem Internet stammender, auf diesen Webservern eingehender Datenverkehr wird daher erlaubt und die Webserver dürfen antworten.

Was Sie nicht erlauben sollten, ist die Initiierung einer ausgehenden Anforderung vonseiten des Front-End-Webservers. Diese Anforderungen können ein Sicherheitsrisiko darstellen, da diese Verbindungen dazu genutzt werden können, Malware herunterzuladen. Selbst wenn diese Front-End-Server ausgehende Anfragen an das Internet initiieren sollen, möchten Sie möglicherweise die Nutzung Ihrer lokalen Webproxys erzwingen, sodass Sie die URL-Filterung und Protokollierung nutzen können.

Um dies zu verhindern, sollten Sie stattdessen die Tunnelerzwingung verwenden. Wenn Sie die Tunnelerzwingung aktivieren, werden alle Verbindungen mit dem Internet zwangsweise über Ihr lokales Gateway hergestellt. Sie können die Tunnelerzwingung konfigurieren, indem Sie benutzerdefinierte Routen nutzen.

Weitere Informationen:

- [Was sind benutzerdefinierte Routen und IP-Weiterleitung?](../virtual-network/virtual-networks-udr-overview.md)

### Appliances für die Sicherheit des virtuellen Netzwerks
Obwohl Netzwerksicherheitsgruppen, benutzerdefinierte Routen und die Tunnelerzwingung Sicherheit in der Vermittlungs- und Transportschicht des [OSI-Modells](https://en.wikipedia.org/wiki/OSI_model) bieten, kann es vorkommen, dass Sie Sicherheit auf höheren Ebenen als dem Netzwerk aktivieren möchten.

Ihre Sicherheitsanforderungen können beispielsweise Folgendes beinhalten:

- Authentifizierung und Autorisierung, bevor Zugriff auf Ihre Anwendung gestattet wird
- Angriffserkennung und die Antwort darauf
- Überprüfung der Anwendungsebene für Protokolle auf oberen Ebenen
- URL-Filterung
- Anitvirenprogramme und Antischadsoftware auf Netzwerkebene
- Antirobot-Schutz
- Anwendungszugriffssteuerung
- Zusätzlicher DDoS-Schutz (zusätzlich zu dem eigenen DDoS-Schutz des Azure-Fabric)

Sie können mit einer Azure-Partnerlösung auf diese erweiterten Netzwerksicherheitsfunktionen zugreifen. Die aktuellsten Azure-Partnerlösungen für Netzwerksicherheit finden Sie im [Azure Marketplace](https://azure.microsoft.com/marketplace/), und indem Sie nach den Stichworten „Sicherheit“ und „Netzwerksicherheit“ suchen.

## Ein sicherer Remotezugriff und standortübergreifende Konnektivität
Die Einrichtung, Konfiguration und Verwaltung Ihrer Azure-Ressourcen muss remote durchgeführt werden. Darüber hinaus möchten Sie vielleicht [Hybrid-IT](http://social.technet.microsoft.com/wiki/contents/articles/18120.hybrid-cloud-infrastructure-design-considerations.aspx)-Lösungen bereitstellen, die die Komponenten sowohl lokal als auch öffentlich in der Azure-Cloud bereitstellen. Diese Szenarien erfordern einen sicheren Remotezugriff.

Azure-Netzwerke unterstützen die folgenden Szenarien für den sicheren Remotezugriff:

- Verbinden einzelner Arbeitsstationen mit einem virtuellen Azure-Netzwerk
- Verbinden Ihres lokalen Netzwerks mit einem virtuellen Azure-Netzwerk mithilfe eines VPN
- Verbinden Ihres lokalen Netzwerks mit einem virtuellen Azure-Netzwerk mithilfe eines dedizierten WAN-Links
- Verbinden Ihrer virtuellen Azure-Netzwerke untereinander

### Verbinden einzelner Arbeitsstationen mit einem virtuellen Azure-Netzwerk
In einigen Fällen möchten Sie es möglicherweise einzelnen Entwicklern oder Betriebspersonal gestatten, virtuelle Computer und Dienste in Azure zu verwalten. Beispielsweise benötigen Sie Zugriff auf einen virtuellen Computer in einem virtuellen Azure-Netzwerk, und Ihre Sicherheitsrichtlinie lässt keinen RDP- oder SSH-Remotezugriff auf einzelne virtuelle Computer zu. In diesem Fall können Sie eine Punkt-zu-Standort-VPN-Verbindung nutzen.

Die Punkt-zu-Standort-VPN-Verbindung verwendet das [SSTP VPN](https://technet.microsoft.com/library/cc731352.aspx)-Protokoll, um Ihnen die Einrichtung einer vertraulichen und sicheren Verbindung zwischen dem Benutzer und dem virtuellen Azure-Netzwerk zu ermöglichen. Sobald die VPN-Verbindung hergestellt wurde, kann der Benutzer über die VPN-Verknüpfung mit RDP oder SSH auf jeden virtuellen Computer im virtuellen Azure-Netzwerk zugreifen (vorausgesetzt, dass der Benutzer sich authentifizieren kann und autorisiert ist).

Weitere Informationen:

- [Konfigurieren einer Punkt-zu-Standort-VPN-Verbindung mit einem virtuellen Netzwerk mithilfe von PowerShell](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)

### Verbinden Ihres lokalen Netzwerks mit einem virtuellen Azure-Netzwerk mithilfe eines VPN
Möglicherweise möchten Sie Ihr gesamtes Unternehmensnetzwerk oder Teile davon mit einem virtuellen Azure-Netzwerk verbinden. Dies ist in Hybrid-IT-Szenarien üblich, in denen Unternehmen [ihre lokalen Rechenzentren auf Azure ausweiten](https://gallery.technet.microsoft.com/Datacenter-extension-687b1d84). Oftmals hosten Unternehmen Teile eines Diensts in Azure und andere Teile lokal, z.B. wenn eine Lösung Front-End-Webserver in Azure und lokale Back-End-Datenbanken enthält. Diese Art von „standortübergreifenden“ Verbindungen erhöhen die Sicherheit der in Azure verwalteten Ressourcen und ermöglichen Szenarien wie das Erweitern des Active Directory-Domänencontrollers auf Azure.

Sie erreichen dies zum Beispiel über ein [Site-zu-Site-VPN](https://www.techopedia.com/definition/30747/site-to-site-vpn). Der Unterschied zwischen einem Standort-zu-Standort-VPN und einem Punkt-zu-Standort-VPN ist, dass ein Punkt-zu-Standort-VPN ein einzelnes Gerät mit einem virtuellen Azure-Netzwerk verbindet. Ein Standort-zu-Standort-VPN verbindet hingegen ein ganzes Netzwerk (z.B. Ihr lokales Netzwerk) mit einem virtuellen Azure-Netzwerk. Standort-zu-Standort-VPNs zu einem virtuellen Azure-Netzwerk verwenden das äußerst sichere IPsec-VPN-Protokoll im Tunnelmodus.

Weitere Informationen:

- [Erstellen eines Resource Manager-VNet mit Site-to-Site-VPN-Verbindung über das Azure-Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Planung und Entwurf für VPN Gateway](../vpn-gateway/vpn-gateway-plan-design.md)

### Verbinden Ihres lokalen Netzwerks mit einem virtuellen Azure-Netzwerk mithilfe eines dedizierten WAN-Links
Punkt-zu-Standort und Standort-zu-Standort-VPN-Verbindungen sind effektive Methoden, um die standortübergreifende Konnektivität zu aktivieren. Allerdings weisen sie, einigen Unternehmen zufolge, die folgenden Nachteile auf:

- VPN-Verbindungen verschieben Daten über das Internet – bei der Datenverschiebung über ein öffentliches Netzwerk weisen diese Verbindungen möglicherweise Sicherheitsprobleme auf. Darüber hinaus können die Zuverlässigkeit und Verfügbarkeit dieser Internetverbindungen nicht garantiert werden.
- VPN-Verbindungen mit virtuellen Azure-Netzwerken können für einige Anwendungen und Zwecke eine zu geringe Bandbreite bereitstellen, da sie ihr Maximum bei etwa 200 MB/s erreichen.

Organisationen, die die höchsten Sicherheits- und Verfügbarkeitsstufen für ihre standortübergreifenden Verbindungen benötigen, verwenden in der Regel dedizierte WAN-Links für die Verbindung zu Remotestandorten. Azure bietet Ihnen die Möglichkeit, einen dedizierten WAN-Link zu verwenden, um Ihr lokales Netzwerk mit einem virtuellen Azure-Netzwerk zu verbinden. Dies wird über Azure ExpressRoute ermöglicht.

Weitere Informationen:

- [ExpressRoute - Technische Übersicht](../expressroute/expressroute-introduction.md)

### Verbinden Ihrer virtuellen Azure-Netzwerke untereinander
Sie haben auch die Möglichkeit, viele virtuelle Azure-Netzwerke für Ihre Bereitstellungen zu verwenden. Es gibt zahlreiche Gründe, warum dies sinnvoll sein kann. Einer dieser Gründe ist möglicherweise die Vereinfachung der Verwaltung, ein anderer die Sicherheit. Unabhängig von den Beweggründen hinter der Platzierung der Ressourcen in verschiedenen virtuellen Azure-Netzwerken, möchten Sie in einigen Fällen möglicherweise, dass zwischen den Ressourcen eines jeden Netzwerks eine Verbindung hergestellt wird.

Sie erreichen dies z.B., indem die Dienste in einem virtuellen Azure-Netzwerk über ein Internet-Loopback eine Verbindung mit Diensten in einem anderen virtuellen Azure-Netzwerk herstellen. Die Verbindung würde von einem virtuellen Azure-Netzwerk ausgehen, das Internet durchlaufen und anschließend zum virtuellen Azure-Zielnetzwerk zurückkehren. Bei Verwendung dieser Option wird die Verbindung den für Internet-basierte Kommunikation typischen Sicherheitsproblemen ausgesetzt.

Ein besserer Weg wäre möglicherweise die Erstellung eines Standort-zu-Standort-VPN zwischen zwei virtuellen Azure-Netzwerken. Dieses Site-to-Site-VPN zwischen zwei virtuellen Azure-Netzwerken verwendet dasselbe [IPsec-Protokoll im Tunnelmodus](https://technet.microsoft.com/library/cc786385.aspx) wie die zuvor erwähnte standortübergreifende Site-to-Site-VPN-Verbindung.

Der Vorteil eines Standort-zu-Standort-VPN zwischen zwei virtuellen Azure-Netzwerken besteht darin, dass die VPN-Verbindung über das Azure-Netzwerkfabric erstellt wird. Hierzu wird keine Verbindung über das Internet hergestellt. Dadurch erhalten Sie im Vergleich zu Standort-zu-Standort-VPNs, die Verbindungen über das Internet herstellen, eine zusätzliche Sicherheitsebene.

Weitere Informationen:

- [Konfigurieren einer VNet-zu-VNet-Verbindung mit Azure Resource Manager und PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md)

## Availability
Verfügbarkeit ist eine der Schlüsselkomponenten eines jeden Sicherheitsprogramms. Wenn Ihre Benutzer und Systeme über das Netzwerk keinen Zugriff auf die benötigten Ressourcen haben, kann der Dienst als gefährdet betrachtet werden. Azure verfügt über Netzwerktechnologien, die die folgenden Mechanismen für hohe Verfügbarkeit unterstützen:

- HTTP-basierter Lastenausgleich
- Lastenausgleich auf Netzwerkebene
- Globaler Lastenausgleich

Der Lastenausgleich ist ein Verfahren, das entwickelt wurde, um Verbindungen zwischen mehreren Geräten gleichmäßig zu verteilen. Die Ziele des Lastenausgleichs sind:

- Verfügbarkeit erhöhen – Wenn sie die Verbindungen auf mehrere Geräte aufteilen (Lastenausgleich), sind ein oder mehrere Geräte möglicherweise nicht mehr verfügbar, und die auf den verbleibenden Onlinegeräten ausgeführten Dienste können weiterhin die Inhalte des Diensts bereitstellen.
- Leistung erhöhen – Wenn sie die Verbindungen auf mehrere Geräte aufteilen (Lastenausgleich), muss kein Gerät die Prozessorlast allein stemmen. Stattdessen werden die Verarbeitungs- und Speicheranforderungen für die Bereitstellung der Inhalte über mehrere Geräte verteilt.

### HTTP-basierter Lastenausgleich
Organisationen, die webbasierte Dienste ausführen, möchten für diese Webdiensten häufig einen HTTP-basierten Lastenausgleich verwenden, um ein angemessenes Maß an Leistung und eine hohe Verfügbarkeit sicherzustellen. Im Gegensatz zu herkömmlichen netzwerkbasierten Lastenausgleichen basieren die Entscheidungen des HTTP-basierten Lastenausgleichs auf Merkmalen des HTTP-Protokolls, nicht auf den Vermittlungs- und Transportschichtprotokollen.

Azure stellt Ihnen Azure Application Gateway bereit, um Ihnen den HTTP-basierten Lastenausgleich für Ihre webbasierten Dienste bereitzustellen. Azure Application Gateway unterstützt:

- den HTTP-basierten Lastenausgleich – Die Entscheidungen des Lastenausgleichs basierend auf den besonderen Merkmalen des HTTP-Protokolls.
- cookiebasierte Sitzungsaffinität – Diese Funktion stellt sicher, dass der Client mit dem Server hinter dem Lastenausgleich verbunden bleibt. Die gewährleistet die Stabilität der Transaktionen.
- SSL-Auslagerung – Wenn eine Clientverbindung mit dem Lastenausgleich hergestellt wird, wird die Sitzung zwischen dem Client und den Lastenausgleich mithilfe des HTTPS (SSL/)-Protokolls verschlüsselt. Sie können jedoch die Verbindung zwischen dem Lastenausgleich und dem Webserver hinter dem Lastenausgleich so einrichten, dass das HTTP-Protokoll (unverschlüsselt) verwendet wird, und dadurch die Leistung erhöhen. Dies wird als „SSL-Auslagerung“ bezeichnet, da der Webserver hinter dem Lastenausgleich die durch die Verschlüsselung hervorgerufene, zusätzliche Prozessorauslastung nicht tragen muss und daher in der Lage sein sollte, Dienstanforderungen schneller zu bedienen.
- URL-basiertes Inhaltsrouting – Diese Funktion ermöglicht es dem Lastenausgleich, ausgehend von der Ziel-URL zu entscheiden, wohin Verbindungen weitergeleitet werden sollen. Dies bietet viel mehr Flexibilität als Lösungen, die Lastenausgleichsentscheidungen auf Grundlage von IP-Adressen treffen.

Weitere Informationen:

- [Übersicht über Application Gateway](../application-gateway/application-gateway-introduction.md)

### Lastenausgleich auf Netzwerkebene
Im Gegensatz zum HTTP-basierten Lastenausgleich trifft der Lastenausgleich auf Netzwerkebene Entscheidungen auf Grundlage der IP-Adresse und Portnummern (TCP oder UDP). Mit Azure Load Balancer können Sie die Vorteile des Lastenausgleichs auf Netzwerkebene nutzen. Einige wichtige Eigenschaften von Azure Load Balancer sind:

- Lastenausgleich auf Netzwerkebene auf Grundlage von IP-Adressen und Portnummern
- Unterstützung für alle Anwendungsschichtprotokolle
- Verteilung der Last auf virtuelle Azure-Computer und Rolleninstanzen von Clouddiensten.
- Kann für Anwendungen und virtuelle Computer mit Internetzugriff (externer Lastenausgleich) und ohne Internetzugriff (interner Lastenausgleich) verwendet werden
- Endpunktüberwachung zur Feststellung, ob einer der Dienste hinter dem Lastenausgleich ist nicht mehr verfügbar ist

Weitere Informationen:

- [Internetlastenausgleich zwischen mehreren virtuellen Computern oder Diensten](../load-balancer/load-balancer-internet-overview.md)
- [Interner Lastenausgleich (Übersicht)](../load-balancer/load-balancer-internal-overview.md)

### Globaler Lastenausgleich
Einige Organisationen entscheiden sich die höchstmögliche Verfügbarkeit. Dieses Ziel ist z.B. durch das Hosten von Anwendungen in weltweit verteilten Rechenzentren erreichbar. Wird eine Anwendung in weltweit verteilten Rechenzentren gehostet, kann die Anwendung selbst dann noch ausgeführt werden, wenn eine ganze geopolitische Region nicht mehr verfügbar ist.

Zusätzlich zu den Vorteilen hinsichtlich der Verfügbarkeit, die Ihnen weltweit verteilte Rechenzentren bieten, profitiert auch die Leistung von der Verteilung. Sie können diese Leistungsvorteile mithilfe eines Mechanismus nutzen, der an den Dienst gesendete Anforderungen an das Rechenzentrum weiterleitet, das sich am nächsten bei dem Gerät befindet, das die Anforderung gesendet hat.

Mit dem globalen Lastenausgleich können Sie sich beide Vorteile zunutze machen. In Azure können Sie die Vorteile des globalen Lastenausgleichs mithilfe des Azure Traffic Manager nutzen.

Weitere Informationen:

- [Was ist Traffic Manager?](../traffic-manager/traffic-manager-overview.md)

## Protokollierung
Protokollierung auf Netzwerkebene ist eine entscheidende Funktion für jedes Netzwerksicherheitsszenario. In Azure können Sie Informationen für Netzwerksicherheitsgruppen (NSG) protokollieren, um Protokollierungsinformationen auf Netzwerkebene zu erhalten. Mit der NSG-Protokollierung erhalten Sie Informationen aus:

- Überwachungsprotokollen – Diese Protokolle werden verwendet, um alle Vorgänge anzuzeigen, die an Ihre Azure-Abonnements übermittelt werden. Diese Protokolle sind standardmäßig aktiviert und können im Azure-Portal verwendet werden.
- Ereignisprotokollen – Diese Protokolle bieten Informationen darüber, welche NSG-Regeln angewendet wurden.
- Leistungsindikatorenprotokolle – Dieses Protokolle zeigen Ihnen an, wie oft jede NSG-Regel angewendet wurde, um Datenverkehr zuzulassen oder zu verweigern.

Sie können auch das leistungsstarke Datenvisualisierungstool [Microsoft Power BI](https://powerbi.microsoft.com/what-is-power-bi/) verwenden, um diese Protokolle anzuzeigen und zu analysieren.

Weitere Informationen:

- [Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md)

## Namensauflösung
Die Namensauflösung ist eine Schlüsselfunktion aller Dienste, die in Azure gehostet werden. Eine Kompromittierung der Namensauflösungsfunktion kann für die Sicherheit bedeuten, dass ein Angreifer Anforderungen von Ihren Websites auf die Website eines Angreifers umleiten kann. Eine sichere Namensauflösung ist eine Voraussetzung für alle Ihre gehosteten Clouddienste.

Es gibt zwei Arten der Namensauflösung, die Sie berücksichtigen müssen:

- Interne Namensauflösung – Die interne Namensauflösung wird von Diensten in Ihren virtuellen Azure-Netzwerken, in lokalen Netzwerken oder in beiden verwendet. Auf Namen, die für die interne Namensauflösung verwendet werden, kann nicht über das Internet zugegriffen werden. Für die optimale Sicherheit ist es wichtig, dass Ihr internes Namensauflösungsschema nicht für externe Benutzer zugänglich ist.
- Externe Namensauflösung – Die externe Namensauflösung wird von Personen und Geräten außerhalb Ihrer lokalen und virtuellen Azure-Netzwerke verwendet. Hierbei handelt es sich um die Namen, die im Internet sichtbar sind und dazu verwendet werden, Verbindungen an Ihre cloudbasierten Dienste zu leiten.

Für die interne Namensauflösung stehen Ihnen zwei Optionen zur Verfügung:

- Ein DNS-Server in einem virtuellen Azure-Netzwerk – Beim Erstellen eines neuen virtuellen Azure-Netzwerks wird ein DNS-Server für Sie erstellt. Dieser DNS-Server kann die Namen der Computer in diesem virtuellen Azure-Netzwerk auflösen. Dieser DNS-Server kann nicht konfiguriert werden und wird vom Azure Fabric-Manager verwaltet. Dadurch ist diese Lösung für die Namensauflösung sicher.
- Bring your own DNS server (Einen eigenen DNS-Server verwenden) – Sie haben die Möglichkeit, einen DNS-Server Ihrer Wahl in Ihrem virtuellen Azure-Netzwerk bereitzustellen. Dieser DNS-Server kann ein in Active Directory integrierter DNS-Server oder eine dedizierte DNS-Serverlösung sein, die von einem Azure-Partner bereitgestellt wird und die Sie im Azure Marketplace erhalten.

Weitere Informationen:

- [Virtuelle Netzwerke im Überblick](../virtual-network/virtual-networks-overview.md)
- [Verwalten von DNS-Servern, die von einem virtuellen Netzwerk (VNet) verwendet werden](../virtual-network/virtual-networks-manage-dns-in-vnet.md)

Für die externe DNS-Auflösung stehen Ihnen zwei Optionen zur Verfügung:

- Das lokale Hosten Ihres eigenen externen DNS-Servers
- Das Hosten Ihres eigenen externen DNS-Servers bei einem Dienstanbieter

Viele große Organisationen hosten ihre eigenen DNS-Server lokal. Sie können dies, da sie über die notwendigen Fachkenntnisse im Bereich Netzwerke und die globale Präsenz verfügen.

In den meisten Fällen ist es besser, Ihre DNS-Namensauflösungsdienste bei einem Dienstanbieter zu hosten. Diese Dienstanbieter verfügen über die Fachkenntnisse im Bereich Netzwerke und eine globale Präsenz, sodass eine sehr hohen Verfügbarkeit Ihrer Namensauflösungsdienste gewährleistet werden kann. Verfügbarkeit ist entscheidend für DNS-Dienste, denn falls Ihre Namensauflösungsdienste fehlschlagen, kann keine Verbindung zu Ihren mit dem Internet verbundenen Diensten hergestellt werden.

Azure bietet Ihnen eine hoch verfügbare und leistungsstarke externe DNS-Lösung in Form von Azure DNS. Diese externe Namensauflösungslösung nutzt die weltweite Azure DNS-Infrastruktur. Sie können damit Ihre Domänen in Azure mithilfe der gleichen Anmeldeinformationen, APIs, Tools und Abrechnung wie für die anderen Azure-Dienste hosten. Als Teil von Azure bietet Azure DNS auch die strengen Sicherheitskontrollen, die in die Plattform integriert sind.

Weitere Informationen:

- [Azure DNS – Übersicht](../dns/dns-overview.md)

## DMZ-Architektur
Viele Unternehmen verwenden DMZs zum Segmentieren ihrer Netzwerke, um eine Pufferzone zwischen dem Internet und ihren Dienste zu erstellen. Der DMZ-Teil des Netzwerks wird als niedrige Sicherheitszone eingestuft. Daher werden keine wertvollen Ressourcen in diesem Netzwerksegment platziert. In der Regel finden sich dort Netzwerksicherheitsgeräte, die über eine Netzwerkschnittstelle mit dem DMZ-Segment und über eine andere Netzwerkschnittstelle verfügen. Letztere Schnittstelle ist mit einem Netzwerk mit virtuellen Computern und Diensten verbunden, die über das Internet eingehende Verbindungen akzeptieren.

Es gibt eine Reihe von Variationen des DMZ-Designs. Die Entscheidung, ob und welche Art von DMZ bereitgestellt werden soll, wenn Sie sich für die Verwendung einer DMZ entscheiden, basiert auf den Anforderungen Ihrer Netzwerksicherheit.

Weitere Informationen:

- [Microsoft Cloud Services und Netzwerksicherheit](../best-practices-network-security.md)

## Azure Security Center
Mit Azure Security Center können Sie Bedrohungen verhindern, erkennen und beheben. Darüber hinaus sorgt Security Center für eine größere Transparenz und bessere Kontrolle der Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

Mit Azure Security Center können Sie die Netzwerksicherheit wie folgt optimieren und überwachen:

- Angeben von Empfehlungen zur Netzwerksicherheit
- Überwachen des Zustands Ihrer Konfiguration der Netzwerksicherheit
- Warnen von netzwerkbasierten Bedrohungen auf Endpunkt- und Netzwerkebene

Weitere Informationen:

- [Einführung in Azure Security Center](../security-center/security-center-intro.md)

<!---HONumber=AcomDC_0810_2016-->