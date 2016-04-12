<properties
   pageTitle="Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell | Microsoft Azure"
   description="Diese Seite enthält eine Übersicht über die wichtigen Informationen zur Überbrückung für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/01/2016"
   ms.author="ganesr"/>

# Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell

Dieser Artikel enthält eine Übersicht über das Verschieben einer ExpressRoute-Verbindung vom klassischen zum Resource Manager-Bereitstellungsmodell.

[AZURE.INCLUDE [vpn-gateway-sm-rm](../../includes/vpn-gateway-sm-rm-include.md)]

Eine einzelne ExpressRoute-Verbindung kann zum Herstellen einer Verbindung mit virtuellen Netzwerken (VNets) verwendet werden, die mit dem klassischen Bereitstellungsmodell oder dem Resource Manager-Bereitstellungsmodell bereitgestellt wurden. Eine ExpressRoute-Verbindung kann unabhängig von ihrer Erstellung jetzt Verknüpfungen zu virtuellen Netzwerken beider Bereitstellungsmodelle herstellen.

![](./media/expressroute-move/expressroute-move-1.png)

## Im klassischen Bereitstellungsmodell erstellte ExpressRoute-Verbindungen

ExpressRoute-Verbindungen, die im klassischen Bereitstellungsmodell erstellt wurden, müssen zuerst auf das Resource Manager-Bereitstellungsmodell umgestellt werden, um die Konnektivität für das klassische und das Resource Manager-Bereitstellungsmodell zu ermöglichen. Es kommt nicht zu einem Verbindungsverlust oder einer Störung, wenn eine Verbindung verschoben wird. Alle Verknüpfungen zwischen Verbindung und VNet im klassischen Bereitstellungsmodell (innerhalb eines Abonnements und abonnementübergreifend) werden beibehalten. Nachdem die Verschiebung erfolgreich abgeschlossen wurde, ist das Aussehen und Verhalten der ExpressRoute-Verbindung mit dem Aussehen und Verhalten einer ExpressRoute-Verbindung identisch, die unter dem Resource Manager-Bereitstellungsmodell erstellt wurde. Sie können jetzt Verbindungen mit virtuellen Netzwerken im Resource Manager-Bereitstellungsmodell erstellen.

Nachdem eine ExpressRoute-Verbindung auf das Resource Manager-Bereitstellungsmodell umgestellt wurde, können Sie den Lebenszyklus der ExpressRoute-Verbindung nur noch mit dem Resource Manager-Bereitstellungsmodell verwalten. Dies bedeutet, dass das Hinzufügen, Aktualisieren und Löschen von Peerings, das Aktualisieren von Verbindungseigenschaften wie Bandbreite, SKU und Abrechnungstyp und das Löschen von Verbindungen nur unter dem Resource Manager-Bereitstellungsmodell durchgeführt werden kann. Der Abschnitt unten enthält weitere Informationen zur Verwaltung des Zugriffs auf beide Bereitstellungsmodelle für Verbindungen, die unter dem Resource Manager-Bereitstellungsmodell erstellt wurden.

Für die Verschiebung ist es nicht erforderlich, dass Sie Ihren Konnektivitätsanbieter einbeziehen.

## Unter dem Resource Manager-Bereitstellungsmodell erstellte ExpressRoute-Verbindungen

Für ExpressRoute-Verbindungen, die unter dem Resource Manager-Bereitstellungsmodell erstellt wurden, können Sie es ermöglichen, dass sie über beide Bereitstellungsmodelle zugänglich sind. Der Zugang über beide Bereitstellungsmodelle kann für alle ExpressRoute-Verbindungen Ihres Abonnements ermöglicht werden.

- Für ExpressRoute-Verbindungen, die mit dem Resource Manager-Bereitstellungsmodell erstellt wurden, besteht standardmäßig kein Zugang zum klassischen Bereitstellungsmodell.
- ExpressRoute-Verbindungen, die vom klassischen Bereitstellungsmodell auf das Resource Manager-Bereitstellungsmodell umgestellt wurden, sind standardmäßig über beide Bereitstellungsmodelle zugänglich.
- Für eine ExpressRoute-Verbindung besteht immer Zugang zum Resource Manager-Bereitstellungsmodell, und zwar unabhängig davon, ob sie unter dem Resource Manager-Bereitstellungsmodell oder dem klassischen Bereitstellungsmodell erstellt wurde. Dies bedeutet, dass Sie Verbindungen mit virtuellen Netzwerken herstellen können, die unter dem Resource Manager-Bereitstellungsmodell erstellt wurden, indem Sie die Anleitung unter [Verknüpfen von virtuellen Netzwerken](expressroute-howto-linkvnet-arm.md) befolgen. 
- Der Zugang zum klassischen Bereitstellungsmodell wird in der ExpressRoute-Verbindung durch den Parameter „allowClassicOperations“ gesteuert. 

>[AZURE.IMPORTANT] Es gelten alle Kontingente, die auf der Seite [Diensteinschränkungen](../azure-subscription-service-limits.md) dokumentiert sind. Beispiel: Eine Standardverbindung kann maximal zehn VNet-Verknüpfungen bzw. Verbindungen für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell zusammen aufweisen.


### Steuern des Zugangs zum klassischen Bereitstellungsmodell

Sie können für eine einzelne ExpressRoute-Verbindung die Verknüpfung mit VNets unter beiden Bereitstellungsmodellen ermöglichen, indem Sie den Parameter „allowClassicOperations“ der ExpressRoute-Verbindung festlegen.

Wenn Sie „allowClassicOperations“ auf TRUE festlegen, können Sie Verknüpfungen mit VNets aus beiden Bereitstellungsmodellen mit der ExpressRoute-Verbindung einrichten. Sie können eine Verknüpfung mit VNets unter dem klassischen Bereitstellungsmodell einrichten, indem Sie die Anleitung unter [Verknüpfen virtueller Netzwerke im klassischen Bereitstellungsmodell](expressroute-howto-linkvnet-classic.md) verwenden. Sie können eine Verknüpfung mit VNets unter dem Resource Manager-Bereitstellungsmodell einrichten, indem Sie die Anleitung unter [Verknüpfen virtueller Netzwerke im Resource Manager-Bereitstellungsmodell](expressroute-howto-linkvnet-arm.md) verwenden.

Wenn Sie „allowClassicOperations“ auf FALSE festlegen, wird der Zugang zur Verbindung über das klassische Bereitstellungsmodell blockiert. Alle VNet-Verknüpfungen im klassischen Bereitstellungsmodell werden aber beibehalten. In diesem Fall ist die ExpressRoute-Verbindung unter dem klassischen Bereitstellungsmodell nicht sichtbar.

### Unterstützte Vorgänge im klassischen Bereitstellungsmodell

Die folgenden klassischen Vorgänge werden für eine ExpressRoute-Verbindung unterstützt, wenn „allowClassicOperations“ auf TRUE festgelegt ist.

 - Abrufen von Informationen zur ExpressRoute-Verbindung
 - Erstellen/Aktualisieren/Abrufen/Löschen von VNet-Verbindungen mit klassischen VNets
 - Erstellen/Aktualisieren/Abrufen/Löschen von VNet-Verknüpfungsautorisierungen für abonnementübergreifende Konnektivität

Sie können die folgenden klassischen Vorgänge nicht durchführen, wenn „allowClassicOperations“ auf TRUE festgelegt ist.

 - Erstellen/Aktualisieren/Abrufen/Löschen von BGP-Peerings für private Azure-, öffentliche Azure- und Microsoft-Peerings
 - Löschen einer ExpressRoute-Verbindung

## Kommunikation von klassisch zu Resource Manager

Die ExpressRoute-Verbindung fungiert wie eine Brücke zwischen dem klassischen und dem Resource Manager-Bereitstellungsmodell. Der Datenverkehr zwischen virtuellen Computern in VNets unter dem klassischen Bereitstellungsmodell und virtuellen Computern in VNets unter dem Resource Manager-Bereitstellungsmodell fließt über die ExpressRoute, wenn beide VNets mit derselben ExpressRoute-Verbindung verknüpft sind. Der aggregierte Durchsatz ist basierend auf der Durchsatzkapazität des Gateways des virtuellen Netzwerks beschränkt. In diesen Fällen gelangt der Datenverkehr nicht in das Netzwerk des Verbindungsanbieters oder in Ihr Netzwerk. Der Fluss des Datenverkehrs zwischen den VNets ist vollständig auf das Microsoft-Netzwerk beschränkt.

## Zugriff auf öffentliche Azure- und Microsoft-Peeringressourcen

Sie können weiterhin ohne Störungen auf Ressourcen zugreifen, die normalerweise per öffentlichem Azure-Peering und Microsoft-Peering verfügbar sind.

## Unterstützte Funktionen

In diesem Abschnitt wird beschrieben, was bei dieser Funktion unterstützt wird.

 - Eine einzelne ExpressRoute-Verbindung kann verwendet werden, um auf VNets zuzugreifen, die mit dem klassischen Bereitstellungsmodell oder dem Resource Manager-Bereitstellungsmodell bereitgestellt wurden.
 - Sie können eine ExpressRoute-Verbindung aus dem klassischen Bereitstellungsmodell in das Resource Manager-Bereitstellungsmodell verschieben. Nach dem Verschieben weist die ExpressRoute-Verbindung das gleiche Aussehen und Verhalten wie alle anderen ExpressRoute-Verbindungen auf, die unter dem Resource Manager-Bereitstellungsmodell erstellt wurden.
 - Nur die ExpressRoute-Verbindung kann verschoben werden. Verbindungsverknüpfungen, VNets und VPN-Gateways werden bei diesem Vorgang nicht verschoben.
 - Nachdem eine ExpressRoute-Verbindung auf das Resource Manager-Bereitstellungsmodell umgestellt wurde, können Sie den Lebenszyklus der ExpressRoute-Verbindung nur noch mit dem Resource Manager-Bereitstellungsmodell verwalten. Dies bedeutet, dass das Hinzufügen, Aktualisieren und Löschen von Peerings, das Aktualisieren von Verbindungseigenschaften wie Bandbreite, SKU und Abrechnungstyp und das Löschen von Verbindungen nur unter dem Resource Manager-Bereitstellungsmodell durchgeführt werden kann.
 - Die ExpressRoute-Verbindung fungiert wie eine Brücke zwischen dem klassischen und dem Resource Manager-Bereitstellungsmodell. Der Datenverkehr zwischen virtuellen Computern in VNets unter dem klassischen Bereitstellungsmodell und virtuellen Computern in VNets unter dem Resource Manager-Bereitstellungsmodell fließt über die ExpressRoute, wenn beide VNets mit derselben ExpressRoute-Verbindung verknüpft sind. 
 - Abonnementübergreifende Konnektivität im klassischen und Resource Manager-Bereitstellungsmodell.

## Nicht unterstützte Funktionen

In diesem Abschnitt wird beschrieben, was bei dieser Funktion nicht unterstützt wird.

 - Verschieben von Verbindungsverknüpfungen, Gateways und virtuellen Netzwerken vom klassischen zum Resource Manager-Bereitstellungsmodell.
 - Verwalten des Lebenszyklus der ExpressRoute-Verbindung über das klassische Bereitstellungsmodell.
 - RBAC-Unterstützung für das klassische Bereitstellungsmodell. Es ist nicht möglich, RBAC-Steuervorgänge für die Verbindung über das klassische Bereitstellungsmodell durchzuführen. Alle Administratoren und Co-Admins des Abonnements können die Verknüpfung von VNets mit der Verbindung aktivieren und deaktivieren.

## Konfiguration

Befolgen Sie die Anleitung unter [Verschieben einer ExpressRoute-Verbindung vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-howto-move-arm.md).

## Nächste Schritte

- Workflowinformationen finden Sie unter [Bereitstellungsworkflows für ExpressRoute-Verbindungen und Verbindungszustände](expressroute-workflows.md).
- Konfigurieren Sie Ihre ExpressRoute-Verbindung.

	- [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-arm.md)
	- [Konfigurieren des Routings](expressroute-howto-routing-arm.md)
	- [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)

<!---HONumber=AcomDC_0406_2016-->