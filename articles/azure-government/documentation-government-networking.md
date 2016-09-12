<properties
	pageTitle="Dokumentation zu Azure Government | Microsoft Azure"
	description="Dieser Artikel enthält einen Vergleich der Features sowie Anleitungen für private Verbindungen mit Azure Government."
	services="Azure-Government"
	cloud="gov" 
	documentationCenter=""
	authors="ryansoc"
	manager="zakramer"
	editor=""/>

<tags
	ms.service="multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="azure-government"
	ms.date="08/25/2016"
	ms.author="ryansoc"/>

#  Azure Government-Netzwerk

##  ExpressRoute (private Verbindung)

Es gibt zwei grundlegende Dienste, die private Netzwerkverbindungen in Azure Government bereitstellen: VPN (Site-to-Site für eine typische Organisation) und ExpressRoute.

Azure ExpressRoute wird zum Herstellen privater Verbindungen zwischen Azure Government-Datencentern und einer Infrastruktur verwendet, die sich an Ihrem Standort oder in einer Kollokationsumgebung befindet. ExpressRoute-Verbindungen werden nicht über das öffentliche Internet hergestellt. Sie bieten mehr Zuverlässigkeit, eine höhere Geschwindigkeit und weniger Latenz als herkömmliche Internetverbindungen. In bestimmten Fällen lassen sich durch die Verwendung von ExpressRoute-Verbindungen zum Übertragen von Daten zwischen lokalen Systemen und Azure erhebliche Kosteneinsparungen erzielen.

Mit ExpressRoute stellen Sie Verbindungen mit Azure an einem ExpressRoute-Standort (Exchange-Anbietereinrichtung) her oder verbinden sich direkt über Ihr vorhandenes WAN (z.B. ein MPLS-VPN), das von einem Netzwerkdienstanbieter bereitgestellt wird.

![alt text](./media/azure-government-capability-private-connectivity-options.PNG) ![alt text](./media/government-capability-expressroute.PNG)

Damit Netzwerkdienste Azure Government-Kundenanwendungen und -Lösungen unterstützen, wird dringend empfohlen, ExpressRoute (private Verbindung) für das Herstellen einer Verbindung mit Azure Government zu implementieren. Bei der Verwendung von VPN-Verbindungen sollte Folgendes berücksichtigt werden:

• Kunden müssen sich an die autorisierende Person/Agentur wenden, um zu erfragen, ob eine private Verbindung oder ein anderer sicherer Verbindungsmechanismus erforderlich ist, und weitere zu berücksichtigende Einschränkungen zu identifizieren.

• Kunden müssen entscheiden, ob das Site-to-Site-VPN über eine Zone für private Verbindungen weitergeleitet werden soll.

• Kunden müssen eine MPLS-Verbindung oder ein VPN von einem lizenzierten Dienstanbieter für private Verbindungen anfordern.

Alle Kunden, die eine Architektur mit privaten Verbindungen nutzen, müssen überprüfen, ob für die Kundenverbindung mit dem GN/I-Edgerouter-Abgrenzungspunkt (Gateway Network/Internet) für Azure Government eine entsprechende Implementierung durchgeführt und beibehalten wird. Dementsprechend muss Ihre Organisation die Netzwerkverbindung zwischen Ihrer lokalen Umgebung und dem GN/C-Edgerouter-Abgrenzungspunkt (Gateway Network/Customer) für Azure Government einrichten.

ExpressRoute ist allgemein in Azure Government verfügbar. Weitere Informationen (u.a. zu Partnern und Peeringstandorten) finden Sie in der <a href="https://azure.microsoft.com/documentation/services/expressroute/">öffentlichen Dokumentation zu ExpressRoute</a>.

Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

<!---HONumber=AcomDC_0831_2016-->