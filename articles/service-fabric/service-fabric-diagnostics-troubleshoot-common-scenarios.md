---
title: Problembehandlung mit Ereignisablaufverfolgungen | Microsoft Docs
description: "Die häufigsten Probleme beim Bereitstellen von Diensten auf Microsoft Azure Service Fabric."
services: service-fabric
documentationcenter: .net
author: mattrowmsft
manager: timlt
editor: 
ms.assetid: 5eb8ef21-da04-4ac8-8b9a-5f7ff1e0a180
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/31/2016
ms.author: mattrow
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 45cfb660db9ea1d93d136d223d65602f3a373574


---
# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Häufig auftretende Probleme und Problembehandlung beim Bereitstellen von Diensten in Azure Service Fabric
Beim Ausführen von Diensten auf dem Entwicklercomputer sind [Visual Studio-Debugtools](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)hilfreich. Für Remotecluster sind [Integritätsberichte](service-fabric-view-entities-aggregated-health.md) immer ein guter Ausgangspunkt. Am einfachsten greifen Sie auf diese Berichte über PowerShell oder [SFX](service-fabric-visualizing-your-cluster.md)zu. In diesem Artikel wird davon ausgegangen, dass Sie einen Remotecluster debuggen und über grundlegende Kenntnisse zur Verwendung der Tools verfügen.

## <a name="application-crash"></a>Anwendungsabstürze
Der Bericht „Partition is below target replica or instance count“ ist ein guter Hinweis darauf, dass der Dienst abstürzt. Um herauszufinden, an welcher Stelle Ihr Dienst abstürzt, bedarf es einiger weiterer Untersuchungen. Bei einer ordnungsgemäßen Skalierung des Diensts stellt ein Satz von gut durchdachten Ablaufverfolgungen die beste Möglichkeit dar.  Es empfiehlt sich, zu versuchen, diese Ablaufverfolgungen mit der [Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md) zu sammeln und sie mithilfe einer Lösung wie [Elasticsearch](service-fabric-diagnostic-how-to-use-elasticsearch.md) anzuzeigen und zu suchen.

![SFX-Partitionsintegrität](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

### <a name="during-service-or-actor-initialization"></a>Bei der Initialisierung des Diensts oder Akteurs
Alle Ausnahmen, die vor der Initialisierung des Diensttyps auftreten, bewirken, dass der Prozess abstürzt. Für diese Art von Abstürzen wird im Anwendungsereignisprotokoll der Fehler des Diensts angezeigt.
Dies sind die am häufigsten auftretenden Ausnahmen, bevor der Dienst initialisiert wird.

***System.IO.FileNotFoundException***

Dieser Fehler tritt oft aufgrund fehlender Assemblyabhängigkeiten auf. Überprüfen Sie die CopyLocal-Eigenschaft in Visual Studio oder im globalen Assemblycache für den Knoten.

***System.Runtime.InteropServices.COMException***
 * in System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory(IntPtr, IFabricStatefulServiceFactory)*

 Hiermit wird angezeigt, dass der Name des registrierten Diensttyps nicht mit dem Dienstmanifest übereinstimmt.

[Azure-Diagnose](service-fabric-diagnostics-how-to-setup-wad.md) kann so konfiguriert werden, dass das Ereignisprotokoll der Anwendung für alle Ihre Knoten automatisch hochgeladen wird.

### <a name="runasync-or-onactivateasync"></a>RunAsync() oder OnActivateAsync()
Wenn der Absturz während der Initialisierung oder Ausführung Ihres registrierten Diensttyps oder Akteurs auftritt, wird die Ausnahme von Azure Service Fabric abgefangen. Sie können dies über die EventSource-Anbieter anzeigen, die im Abschnitt „Nächste Schritte“ aufgeführt sind.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu vorhandenen Diagnosefunktionen von Service Fabric:

* [Diagnose für Reliable Actors](service-fabric-reliable-actors-diagnostics.md)
* [Diagnose für Reliable Services](service-fabric-reliable-services-diagnostics.md)




<!--HONumber=Nov16_HO3-->


