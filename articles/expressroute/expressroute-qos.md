---
title: "QoS-Anforderungen für ExpressRoute | Microsoft Docs"
description: "Diese Seite enthält ausführliche Anforderungen für das Konfigurieren und Verwalten von QoS für ExpressRoute-Verbindungen."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: db1c1447-0283-4a09-907b-ae481adc40c7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9e74127e3140ec9973753fb5f29151c406326c36


---
# <a name="expressroute-qos-requirements"></a>QoS-Anforderungen für ExpressRoute
Skype for Business umfasst verschiedene Workloads mit jeweils unterschiedlichen QoS-Anforderungen. Wenn Sie Sprachdienste über ExpressRoute bereitstellen möchten, sollten die im Folgenden beschriebenen Anforderungen erfüllt sein.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> Die QoS-Anforderungen gelten nur für das Microsoft-Peering. Die DSCP-Werte in Ihrem Netzwerkdatenverkehr, der über öffentliches und privates Azure-Peering empfangen wird, werden auf 0 zurückgesetzt. 
> 
> 

Die folgende Tabelle enthält eine Liste mit DSCP-Markierungen, die von Skype for Business verwendet werden. Weitere Informationen finden Sie unter [Verwalten der Dienstqualität für Skype for Business](https://technet.microsoft.com/library/gg405409.aspx) .

| **Datenverkehrsklasse** | **Behandlung (DSCP-Markierung)** | **Skype for Business-Workloads** |
| --- | --- | --- |
| **Voice** |EF (46) |Skype-/Lync-Sprachanrufe |
| **Interactive** |AF41 (34) |Video |
| AF21 (18) |App-Freigabe | |
| **Standard** |AF11 (10) |Dateiübertragung |
| CS0 (0) |Alles andere | |

* Sie sollten die Workloads klassifizieren und die richtigen DSCP-Markierungen wählen. Folgen Sie [dieser Anleitung](https://technet.microsoft.com/library/gg405409.aspx) zum Festlegen von DSCP-Markierungen in Ihrem Netzwerk.
* Sie sollten mehrere QoS-Warteschlangen in Ihrem Netzwerk konfigurieren und bereitstellen. Voice muss als eigenständige Klasse definiert werden und die in RFC 3246 angegebene EF-Behandlung erhalten. 
* Sie können den gewünschten Warteschlangenmechanismus, die Richtlinie für die Auslastungserkennung und die Bandbreitenzuordnung pro Datenverkehrsklasse festlegen. Die DSCP-Markierung der Skype for Business-Workloads muss jedoch in jedem Fall erhalten bleiben. Wenn Sie andere als die oben aufgeführten DSCP-Markierungen verwenden, z. B. AF31 (26), muss dieser DSCP-Wert im Paket vor dem Senden an Microsoft auf 0 zurückgesetzt werden. Microsoft sendet nur Pakete mit dem in der obigen Tabelle aufgeführten DSCP-Wert. 

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich die Anforderungen für [Routing](expressroute-routing.md) und [NAT](expressroute-nat.md) an.
* Informationen zum Konfigurieren der ExpressRoute-Verbindung finden Sie über die folgenden Links.
  
  * [Erstellen einer ExpressRoute-Verbindung](expressroute-howto-circuit-classic.md)
  * [Konfigurieren des Routings](expressroute-howto-routing-classic.md)
  * [Verknüpfen eines VNet mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-classic.md)




<!--HONumber=Nov16_HO2-->


