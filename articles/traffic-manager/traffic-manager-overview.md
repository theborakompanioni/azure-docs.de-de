<properties 
   pageTitle="Was ist Traffic Manager? | Microsoft Azure"
   description="In diesem Artikel erfahren Sie, worum es sich bei Traffic Manager handelt und ob Traffic Manager die richtige Datenverkehrsrouting-Lösung für Ihre Anwendung ist."
   services="traffic-manager"
   documentationCenter=""
   authors="jtuliani"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/09/2016"
   ms.author="jtuliani" />

# Was ist Traffic Manager?

Microsoft Azure Traffic Manager ermöglicht die Verteilung von Benutzerdatenverkehr auf Ihre Dienstendpunkte in unterschiedlichen Datencentern auf der ganzen Welt.

Zu den von Traffic Manager unterstützten Endpunkte zählen virtuelle Azure-Computer, Web-Apps und Clouddienste. Darüber hinaus kann Traffic Manager auch mit externen, Azure-fremden Endpunkten verwendet werden.

Traffic Manager leitet Endbenutzeranforderungen unter Verwendung von DNS (Domain Name System) und unter Berücksichtigung der konfigurierten Datenverkehrsrouting-Methode sowie der aktuellen Endpunktintegrität an den am besten geeigneten Endpunkt weiter. Clients stellen dann eine direkte Verbindung mit dem geeigneten Dienstendpunkt her.

Traffic Manager unterstützt eine Reihe von [Datenverkehrsrouting-Methoden](traffic-manager-routing-methods.md) für unterschiedliche Anwendungsanforderungen. Darüber hinaus bietet Traffic Manager eine [Überprüfung der Endpunktintegrität sowie automatisches Failover von Endpunkten](traffic-manager-monitoring.md) und ermöglicht dadurch die Erstellung hoch verfügbarer Anwendungen mit Ausfallresilienz (auch beim Ausfall einer gesamten Azure-Region).

## Vorteile von Traffic Manager

Traffic Manager unterstützt Sie bei folgenden Aufgaben:

- **Verbessern der Verfügbarkeit wichtiger Anwendungen**: Mit Traffic Manager können Sie die Verfügbarkeit wichtiger Anwendungen gewährleisten, indem Sie Ihre Endpunkte in Azure überwachen und beim Ausfall eines Endpunkts automatisches Failover bereitstellen.
- **Verbessern der Reaktionsfähigkeit hochleistungsfähiger Anwendungen**: Azure ermöglicht die Ausführung von Clouddiensten oder Websites in Datencentern auf der ganzen Welt. Traffic Manager kann die Reaktionsfähigkeit Ihrer Anwendungen verbessern und Endbenutzer an den Endpunkt mit der niedrigsten Netzwerklatenz für den Client weiterleiten.
- **Ausführen von Upgrades oder Dienstwartungsarbeiten ohne Ausfallzeiten**: Upgrades und andere geplante Wartungsarbeiten für Ihre Anwendungen können ohne Ausfallzeiten für Endbenutzer durchgeführt werden. Hierzu wird der Datenverkehr während der Wartung von Traffic Manager an andere Endpunkte weitergeleitet.
- **Kombinieren von lokalen und cloudbasierten Anwendung**: Traffic Manager unterstützt externe, Azure-fremde Endpunkte und kann somit in Hybridszenarien mit cloudbasierten und lokalen Bereitstellungen wie „Erweiterung in die Cloud“, „Migration in die Cloud“ und „Failover in die Cloud“ verwendet werden.
- **Verteilen des Datenverkehrs für umfangreiche, komplexe Bereitstellungen**: Datenverkehrsrouting-Methoden können mit [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) zu komplexen und flexiblen Datenverkehrsrouting-Konfigurationen kombiniert werden, die auch den Anforderungen umfangreicher und komplexer Bereitstellungen gerecht werden. 

[AZURE.INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## Nächste Schritte

- Informieren Sie sich ausführlicher über die [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).

- Erfahren Sie, wie Sie mithilfe der [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md) hoch verfügbare Anwendungen entwickeln.

- Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md).

- [Erstellen Sie ein Traffic Manager-Profil](traffic-manager-manage-profiles.md).
 

<!---HONumber=AcomDC_0615_2016-->