---
title: "Informationen zu Supportoptionen für Azure Service Fabric | Microsoft-Dokumentation"
description: "Unterstützte Azure Service Fabric-Clusterversionen und Links zu Dateisupporttickets."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/07/2016
ms.author: chackdan
translationtype: Human Translation
ms.sourcegitcommit: ca1ff55fa0ad4b7466b900c8c36386649d021909
ms.openlocfilehash: 9d71fb0e92f58771dac989a4d83a8e314c6a52b6


---
# <a name="azure-service-fabric-support-options"></a>Supportoptionen für Azure Service Fabric

Um die entsprechende Unterstützung für die Service Fabric-Cluster bereitzustellen, in denen Sie Ihre Anwendungsworkloads ausführen, haben wir verschiedene Optionen für Sie eingerichtet. Sie können je nach benötigter Supportstufe und Schweregrad des Problems die passenden Optionen auswählen. 


<a id="getlivesitesupportonazure"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-azure"></a>Melden von Problemen mit der Produktions- oder Livewebsite oder Anfordern von kostenpflichtigem Support für Azure

Um Probleme mit der Livewebsite in Ihrem in Azure bereitgestellten Service Fabric-Cluster zu melden, öffnen Sie [im Azure-Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) oder [Microsoft-Support-Portal](http://support.microsoft.com/oas/default.aspx?prid=16146) ein Ticket für Professional Support.

Weitere Informationen:
 
- [Professional Support von Microsoft für Azure](https://azure.microsoft.com/en-us/support/plans/?b=16.44)
- [Microsoft Premier Support](https://support.microsoft.com/en-us/premier)

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-or-live-site-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Melden von Problemen mit der Produktions- oder Livewebsite oder Anfordern von kostenpflichtigem Support für eigenständige Service Fabric-Cluster

Um Probleme mit der Livewebsite in Ihrem in lokalen oder anderen Clouds bereitgestellten Service Fabric-Cluster zu melden, öffnen Sie im [Microsoft-Support-Portal](http://support.microsoft.com/oas/default.aspx?prid=16146) ein Ticket für Professional Support.

Weitere Informationen:

- [Professional Support von Microsoft für lokale Bereitstellungen](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0)
- [Microsoft Premier Support](https://support.microsoft.com/en-us/premier)


<a id="getsupportonissues"></a>
## <a name="report-azure-service-fabric-issues"></a>Melden von Azure Service Fabric-Problemen 
Wir haben ein GitHub-Repository zum Melden von Service Fabric-Problemen eingerichtet.  Zudem überwachen wir aktiv die folgenden Foren.

### <a name="github-repo"></a>GitHub-Repository 
Melden Sie Azure Service Fabric-Probleme im [Git-Repository für Service Fabric-Probleme](https://github.com/Azure/service-fabric-issues). Dieses Repository dient zur Meldung und Nachverfolgung von Problemen mit Azure Service Fabric sowie zum Senden kleiner Funktionsanforderungen. **Verwenden Sie das Repository nicht zum Melden von Problemen mit der Livewebsite**.

### <a name="stackoverflow-and-msdn-forums"></a>StackOverflow und MSDN-Foren

Das [Service Fabric-Tag auf StackOverflow][stackoverflow] und das [Service Fabric-Forum auf MSDN][msdn-forum] eignen sich für Fragen zur Funktionsweise der Plattform und zur Durchführung bestimmter Aufgaben.

### <a name="azure-feedback-forum"></a>Azure-Feedbackforum

Das [Azure-Feedbackforum für Service Fabric][uservoice-forum] ist die beste Anlaufstelle, wenn Sie umfangreiche Funktionsvorschläge für das Produkt einreichen möchten, da die häufigsten Anforderungen im Rahmen unserer mittel- und langfristigen Planung geprüft werden. Wir empfehlen Ihnen, in der Community Unterstützung für Ihre Vorschläge zu gewinnen.


<a id="releasesuport"></a>
## <a name="supported-service-fabric-versions"></a>Unterstützte Service Fabric-Versionen

Achten Sie darauf, dass in Ihrem Cluster immer eine unterstützte Service Fabric-Version ausgeführt wird. Nach der Ankündigung einer neuen Service Fabric-Version beträgt die verbleibende Supportdauer der vorherigen Version noch mindestens 60 Tage. Neue Versionen werden im [Blog des Service Fabric-Teams](https://blogs.msdn.microsoft.com/azureservicefabric/) angekündigt.

In den folgenden Dokumenten finden Sie Details dazu, wie Sie sicherstellen können, dass in Ihrem Cluster immer eine unterstützte Service Fabric-Version ausgeführt wird.

- [Upgraden der Service Fabric-Version in einem Azure-Cluster](service-fabric-cluster-upgrade.md)
- [Upgraden der Service Fabric-Version in einem eigenständigen Windows Server-Cluster ](service-fabric-cluster-upgrade-windows-server.md)
 
In der folgenden Liste sind die unterstützten Service Fabric-Versionen und das Datum des Ablaufs des Supports aufgeführt.

| **Service Fabric-Laufzeitcluster** | **Ablauf des Supports** |
| --- | --- |
| Alle Clusterversionen vor 5.3.121 |20. Januar 2017 |
| 5.3.121.* |24. Februar 2017 |
| 5.3.204.* |24. Februar 2017 |
| 5.3.* |24. Februar 2017 |
| 5.4.*     |17. April 2017     |
| 5.5.*     |Aktuelle Version, daher kein Datum des Ablaufs des Supports


## <a name="next-steps"></a>Nächste Schritte

- [Upgraden der Service Fabric-Version in einem Azure-Cluster](service-fabric-cluster-upgrade.md)
- [Upgraden der Service Fabric-Version in einem eigenständigen Windows Server-Cluster ](service-fabric-cluster-upgrade-windows-server.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: http://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: http://aka.ms/servicefabricdocs
[sample-repos]: http://aka.ms/servicefabricsamples



<!--HONumber=Feb17_HO2-->


