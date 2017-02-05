---
title: "Übersicht über Internetlastenausgleich | Microsoft Docs"
description: "Übersicht über den Lastenausgleich mit Internetzugriff und die zugehörigen Features. Wie funktioniert ein Lastenausgleich für Azure mithilfe von virtuellen Computern und Clouddiensten."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: bec4f89556a2daa41e19b0ecb2ab9bbbed849107
ms.openlocfilehash: fb3f7c08c5f3a76e4bd28f3519ab72e8b636b93c

---

# <a name="internet-facing-load-balancer-overview"></a>Lastenausgleich für Internetzugriff (Übersicht)

Das Azure-Lastenausgleichsmodul führt die Zuordnung zwischen öffentlicher IP-Adresse und Portnummer des eingehenden Datenverkehrs zu den privaten IP-Adressen und Portnummern der virtuellen Computer durch und ordnet umgekehrt den Antwortdatenverkehr aus dem virtuellen Computer zu. Mithilfe von Lastenausgleichsregeln können Sie bestimmte Arten von Datenverkehr auf mehrere virtuelle Computer oder Dienste verteilen. Sie können zum Beispiel die Netzwerklast von Webanfragen auf mehrere Webserver oder Webrollen verteilen.

Für einen Clouddienst, der Instanzen von Web- oder Workerrollen enthält, können Sie einen öffentlichen Endpunkt in der Dienstdefinition (CSDEF-Datei) definieren.

Die Datei *servicedefinition.csdef* enthält die Endpunktkonfiguration. Wenn Sie mehrere Rolleninstanzen für eine Web- oder Workerrollenbereitstellung haben, wird der Lastenausgleich für diese eingerichtet. Die Art und Weise, Instanzen zu Ihrer Cloudbereitstellung hinzuzufügen, ändert die Instanzenanzahl in der Dienstkonfigurationsdatei (CSFG-Datei).

Die folgende Abbildung zeigt einen Endpunkt für verschlüsselten Datenverkehr mit Lastenausgleich, der von drei virtuellen Computern für den öffentlichen und privaten TCP-Port 443 genutzt wird. Diese drei virtuellen Computer bilden einen Satz mit Lastenausgleich.

![Beispiel für öffentlichen Lastenausgleich](./media/load-balancer-internet-overview/IC727496.png))

Abbildung 1: Endpunkt mit Lastenausgleich für verschlüsselten Webdatenverkehr

Wenn Internetclients Webseitenanfragen über den TCP-Port 443 an die öffentliche IP-Adresse des Clouddiensts senden, verteilt Azure Load Balancer diese Anfragen auf die drei virtuellen Computer im Satz mit Lastenausgleich. Weitere Informationen zu Lastenausgleichsalgorithmen finden Sie auf der [Übersichtsseite zum Lastenausgleich](load-balancer-overview.md#load-balancer-features).

Standardmäßig verteilt Azure Load Balancer Netzwerkdatenverkehr gleichmäßig auf mehrere Instanzen virtueller Computer. Darüber hinaus können Sie Sitzungsaffinität konfigurieren. Weitere Informationen finden Sie unter [Load Balancer-Verteilungsmodus (Quell-IP-Affinität)](load-balancer-distribution-mode.md).

## <a name="next-steps"></a>Nächste Schritte

Lesen Sie die Informationen zum [internen Lastenausgleich](load-balancer-internal-overview.md) , um herauszufinden, welcher Lastenausgleich sich für Ihre Cloudbereitstellung besser eignet.

Sie können auch einen [Lastenausgleich für Internetzugriff erstellen](load-balancer-get-started-internet-arm-ps.md) und die Art des [Verteilungsmodus](load-balancer-distribution-mode.md) des Lastenausgleichs für ein bestimmtes Datenverkehrsverhalten im Netzwerk konfigurieren.

Wenn es für Ihre Anwendung erforderlich ist, dass Verbindungen mit Servern hinter einem Lastenausgleich aufrechterhalten werden, informieren Sie sich über [TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md). Hier erfahren Sie mehr über das Verhalten von Leerlaufverbindungen bei der Verwendung des Azure-Lastenausgleichs.



<!--HONumber=Nov16_HO3-->


