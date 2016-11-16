---
title: Optimieren von ExpressRoute-Routing | Microsoft Docs
description: "Diese Seite enthält Informationen zur Optimierung des Routings, wenn ein Kunde über mehr als eine ExpressRoute-Verbindung verfügt, mit der die Verbindung zwischen Microsoft und dem Unternehmensnetzwerk des Kunden hergestellt werden kann."
documentationcenter: na
services: expressroute
author: charwen
manager: carmonm
editor: 
ms.assetid: fca53249-d9c3-4cff-8916-f8749386a4dd
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: charwen
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 26f0992e734f0aae96ac6e8b7040d661d5fb063c


---
# <a name="optimize-expressroute-routing"></a>Optimieren von ExpressRoute-Routing
Wenn Sie mehrere ExpressRoute-Verbindungen nutzen, verfügen Sie über mehr als einen Weg zur Herstellung einer Verbindung mit Microsoft. Dies kann ein suboptimales Routing zur Folge haben. Es kann also sein, dass Ihr Datenverkehr für den Weg zu Microsoft und von Microsoft in Ihr Netzwerk mehr Zeit benötigt. Je länger der Netzwerkpfad, desto höher die Latenz. Die Latenz wirkt sich direkt auf die Anwendungsleistung und die Benutzerfreundlichkeit aus. In diesem Artikel wird dieses Problem veranschaulicht, und es wird beschrieben, wie Sie das Routing mit den standardmäßigen Routingtechnologien optimieren.

## <a name="suboptimal-routing-case-1"></a>Suboptimales Routing – 1. Fall
Wir sehen uns nun ein Beispiel zum Routingproblem an. Angenommen, Sie verfügen über zwei Niederlassungen in den USA: eine in Los Angeles und eine in New York. Die Niederlassungen sind über ein Wide Area Network (WAN) verbunden, wobei es sich entweder um Ihr eigenes Backbonenetzwerk oder die IP-VPN-Verbindung Ihres Service Providers handeln kann. Sie verfügen über zwei ExpressRoute-Verbindungen – eine in der Region „USA, Westen“ und eine in der Region „USA, Osten“ –, die ebenfalls per WAN verbunden sind. Es sind also zwei Wege zum Herstellen der Verbindung mit dem Microsoft-Netzwerk vorhanden. Nehmen Sie weiter an, dass Sie über jeweils eine Azure-Bereitstellung (z.B. Azure App Service) in „USA, Westen“ und „USA, Osten“ verfügen. Sie möchten die Benutzer in Los Angeles mit Azure in der Region „USA, Westen“ und die Benutzer in New York mit Azure in der Region „USA, Osten“ verbinden, da durch Ihren Dienstadministrator vorgesehen ist, dass Benutzer in beiden Niederlassungen auf die Azure-Dienste in ihrer Nähe zugreifen. So soll eine optimale Leistung erzielt werden. Leider funktioniert dieser Plan nur für die Benutzer an der Ostküste gut, aber nicht für die Benutzer an der Westküste. Die Ursache des Problems ist Folgendes: Für jede ExpressRoute-Verbindung kündigen wir für Sie sowohl das Präfix in der Azure-Region „USA, Osten“ (23.100.0.0/16) als auch das Präfix in der Azure-Region „USA, Westen“ (13.100.0.0/16) an. Wenn Sie nicht wissen, welches Präfix für welche Region gilt, können Sie die Präfixe auch nicht unterschiedlich behandeln. Für Ihr WAN kann der Eindruck entstehen, dass beide Präfixe näher an „USA, Osten“ als an „USA, Westen“ liegen, sodass die Benutzer beider Niederlassungen an die ExpressRoute-Verbindung in der Region „USA, Osten“ geleitet werden. Das Ergebnis ist, dass es in der Niederlassung in Los Angeles viele unzufriedene Benutzer gibt.

![](./media/expressroute-optimize-routing/expressroute-case1-problem.png)

### <a name="solution-use-bgp-communities"></a>Lösung: Verwenden von BGP-Communitys
Zum Optimieren des Routings für die Benutzer beider Niederlassungen müssen Sie wissen, welches Präfix für die Azure-Region „USA, Westen“ und welches Präfix für die Region „USA, Osten“ gilt. Wir codieren diese Informationen mithilfe von [BGP-Communitywerten](expressroute-routing.md). Wir haben jeder Azure-Region einen eindeutigen BGP-Communitywert zugewiesen, z.B. „12076:51004“ für „USA, Osten“ und „12076:51006“ für „USA, Westen“. Da Sie jetzt wissen, welches Präfix für welche Azure-Region gilt, können Sie konfigurieren, welche ExpressRoute-Verbindung vorgezogen werden sollte. Da wir BGP zum Austauschen von Routinginformationen verwenden, können Sie die lokale Einstellung von BGP nutzen, um das Routing zu beeinflussen. In unserem Beispiel können Sie 13.100.0.0/16 in „USA, Westen“ einen höheren Wert für die lokale Einstellung als für „USA, Osten“ zuweisen (bzw. einen höheren Wert für die lokale Einstellung für 23.100.0.0/16 in „USA, Osten“ als in „USA, Westen“). Mit dieser Konfiguration wird Folgendes sichergestellt: Wenn beide Wege zu Microsoft verfügbar sind, verwenden die Benutzer in Los Angeles die ExpressRoute-Verbindung in „USA, Westen“ zum Herstellen der Verbindung mit Azure „USA, Westen“, während die Benutzer in New York die ExpressRoute-Verbindung in „USA, Osten“ zum Herstellen der Verbindung mit Azure „USA, Osten“ verwenden. Das Routing ist somit auf beiden Seiten optimiert. 

![](./media/expressroute-optimize-routing/expressroute-case1-solution.png)

## <a name="suboptimal-routing-case-2"></a>Suboptimales Routing – 2. Fall
Dies ist ein weiteres Beispiel, bei dem Verbindungen von Microsoft einen längeren Weg zum Erreichen Ihres Netzwerks zurücklegen. In diesem Fall verwenden Sie lokale Exchange-Server und Exchange Online in einer [Hybridumgebung](https://technet.microsoft.com/library/jj200581%28v=exchg.150%29.aspx). Ihre Niederlassungen sind mit einem WAN verbunden. Sie kündigen die Präfixe Ihrer lokalen Server in beiden Niederlassungen gegenüber Microsoft über die beiden ExpressRoute-Verbindungen an. Exchange Online initiiert Verbindungen mit den lokalen Servern in Fällen wie der Postfachmigration. Unglücklicherweise wird die Verbindung mit der Niederlassung in Los Angeles an die ExpressRoute-Verbindung in „USA, Osten“ geleitet, bevor der Web über den ganzen Kontinent zurück an die Westküste zurückgelegt wird. Die Ursache des Problems ist ähnlich wie im ersten Fall. Ohne entsprechenden Hinweis kann das Microsoft-Netzwerk nicht ermitteln, welches Kundenpräfix näher an „USA, Osten“ und welches näher an „USA, Westen“ liegt. So wird der falsche Weg zu Ihrer Niederlassung in Los Angeles gewählt.

![](./media/expressroute-optimize-routing/expressroute-case2-problem.png)

### <a name="solution-use-as-path-prepending"></a>Lösung: Voranstellen von AS PATH
Es gibt zwei Lösungen des Problems. Die erste Lösung besteht darin, dass Sie einfach das lokale Präfix 177.2.0.0/31 für die Niederlassung in Los Angeles unter der ExpressRoute-Verbindung in „USA, Westen“ und das lokale Präfix 177.2.0.2/31 für die Niederlassung in New York unter der ExpressRoute-Verbindung in „USA, Osten“ ankündigen. Das Ergebnis ist, dass für Microsoft nur noch ein Weg zum Herstellen der Verbindung mit Ihren Niederlassungen vorhanden ist. Die Mehrdeutigkeit wurde beseitigt, und das Routing wurde optimiert. Bei diesem Aufbau müssen Sie sich Gedanken über Ihre Failoverstrategie machen. Falls der Weg zu Microsoft per ExpressRoute unterbrochen wird, müssen Sie sicherstellen, dass Exchange Online weiterhin eine Verbindung mit Ihren lokalen Servern herstellen kann. 

Die zweite Lösung besteht darin, weiterhin beide Präfixe unter beiden ExpressRoute-Verbindungen anzukündigen und darauf hinzuweisen, welches Präfix in der Nähe welcher Niederlassung liegt. Da die BGP-Voranstellung von AS Path unterstützt wird, können Sie AS Path für Ihr Präfix konfigurieren, um das Routing zu beeinflussen. In diesem Beispiel können Sie AS PATH für 172.2.0.0/31 in „USA, Osten“ verlängern, damit die ExpressRoute-Verbindung in „USA, Westen“ für Datenverkehr vorgezogen wird, der für dieses Präfix bestimmt ist (da von unserem Netzwerk angenommen wird, dass der Weg zu diesem Präfix im Westen kürzer ist). Auf ähnliche Weise können Sie AS PATH für 172.2.0.2/31 in „USA, Westen“ verlängern, damit von uns die ExpressRoute-Verbindung in „USA, Osten“ vorgezogen wird. Das Routing ist dann für beide Büros optimiert. Wenn bei diesem Aufbau eine ExpressRoute-Verbindung unterbrochen wird, kann Exchange Online Sie trotzdem noch über eine andere ExpressRoute-Verbindung und Ihr WAN erreichen. 

> [!IMPORTANT]
> Wir entfernen private AS-Nummern in AS PATH für die unter Microsoft-Peering empfangenen Präfixe. Sie müssen in AS PATH öffentliche AS-Nummern anfügen, um das Routing für Microsoft-Peering zu beeinflussen.
> 
> 

![](./media/expressroute-optimize-routing/expressroute-case2-solution.png)

> [!IMPORTANT]
> Die hier aufgeführten Beispiele gelten zwar für Microsoft-Peering und öffentliches Peering, für privates Peering werden jedoch die gleichen Funktionen unterstützt. Das Voranstellen von AS PATH funktioniert darüber hinaus in einer einzelnen ExpressRoute-Verbindung, sodass die Auswahl der primären und sekundären Pfade beeinflusst werden kann.
> 
> 




<!--HONumber=Nov16_HO2-->


