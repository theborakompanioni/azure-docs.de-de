
---
title: Übersicht über Internetlastenausgleich | Microsoft Docs
description: Übersicht über den Lastenausgleich mit Internetzugriff und die zugehörigen Features. Wie funktioniert ein Lastenausgleich für Azure mithilfe von virtuellen Computern und Clouddiensten.
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: tysonn

ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2016
ms.author: sewhee

---
# Lastenausgleich für Internetzugriff (Übersicht)
Das Azure-Lastenausgleichsmodul führt die Zuordnung zwischen öffentlicher IP-Adresse und Portnummer des eingehenden Datenverkehrs zu den privaten IP-Adressen und Portnummern der virtuellen Computer durch und ordnet umgekehrt den Antwortdatenverkehr aus dem virtuellen Computer zu. Mithilfe von Lastenausgleichsregeln können Sie bestimmte Arten von Datenverkehr auf mehrere virtuelle Computer oder Dienste verteilen. Sie können zum Beispiel die Netzwerklast von Webanfragen auf mehrere Webserver oder Webrollen verteilen.

> [!NOTE]
> Azure Load Balancer ermöglicht Netzwerkdatenverkehr mit Hashverteilung zwischen mehreren virtuellen Computerinstanzen unter Verwendung der Standardeinstellungen (weitere Informationen zur Hashverteilung finden Sie unter [Load Balancer-Features](load-balancer-overview.md#load-balancer-features)). Wenn Sie Sitzungsaffinität benötigen, lesen Sie die Informationen unter [Lastenausgleichsmodul-Verteilungsmodus](load-balancer-distribution-mode.md).
> 
> 

Für einen Clouddienst, der Instanzen von Web- oder Workerrollen enthält, können Sie einen öffentlichen Endpunkt in der Dienstdefinition (CSDEF-Datei) definieren.

Die Datei "servicedefinition.csdef" enthält die Endpunktkonfiguration. Wenn Sie mehrere Rolleninstanzen für eine Web- oder Workerrollenbereitstellung haben, wird der Lastenausgleich für diese eingerichtet. Die Art und Weise, Instanzen zu Ihrer Cloudbereitstellung hinzuzufügen, ändert die Instanzenanzahl in der Dienstkonfigurationsdatei (CSFG-Datei).

Die folgende Abbildung zeigt einen Endpunkt für verschlüsselten Datenverkehr mit Lastenausgleich, der von drei virtuellen Computern für den öffentlichen und privaten TCP-Port 443 genutzt wird. Diese drei virtuellen Computer bilden einen Satz mit Lastenausgleich.

![Beispiel für öffentlichen Lastenausgleich](./media/load-balancer-internet-overview/IC727496.png))

Wenn Internetclients Webseitenanfragen über den TCP-Port 443 an die öffentliche IP-Adresse des Clouddiensts senden, verteilt Azure Load Balancer diese Anfragen auf die drei virtuellen Computer im Satz mit Lastenausgleich. Weitere Informationen zu Lastenausgleichsalgorithmen finden Sie auf der [Übersichtsseite zum Lastenausgleich](load-balancer-overview.md#load-balancer-features).

## Nächste Schritte
Nachdem Sie sich nun über den Lastenausgleich für Internetzugriff informiert haben, lesen Sie die Informationen zum [internen Lastenausgleich](load-balancer-internal-overview.md), um herauszufinden, welcher Lastenausgleich sich für Ihre Cloudbereitstellung besser eignet.

Sie können auch einen [Lastenausgleich für Internetzugriff erstellen](load-balancer-get-started-internet-arm-ps.md) und die Art des [Verteilungsmodus](load-balancer-distribution-mode.md) des Lastenausgleichs für ein bestimmtes Datenverkehrsverhalten im Netzwerk konfigurieren.

Wenn es für Ihre Anwendung erforderlich ist, dass Verbindungen mit Servern hinter einem Lastenausgleich aufrechterhalten werden, informieren Sie sich über [TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md). Hier erfahren Sie mehr über das Verhalten von Leerlaufverbindungen bei der Verwendung des Azure-Lastenausgleichs.

<!---HONumber=AcomDC_0831_2016-->