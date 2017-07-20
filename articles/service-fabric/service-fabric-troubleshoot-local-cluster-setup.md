---
title: Behandeln von Problemen beim lokalen Service Fabric-Clustersetup | Microsoft Docs
description: "Dieser Artikel enthält eine Reihe von Vorschlägen für die Problembehandlung Ihres lokalen Entwicklungsclusters."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: 97f4feaa-bba0-47af-8fdd-07f811fe2202
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.translationtype: Human Translation
ms.sourcegitcommit: 74f34bdbf5707510c682814716aa0b95c19a5503
ms.openlocfilehash: aa393f884b564cee81fcf75cc2eff895efea9471
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017


---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Problembehandlung beim Einrichten des Clusters für die lokale Entwicklung
Wenn beim Interagieren mit Ihrem lokalen Azure Service Fabric Entwicklungscluster ein Problem auftritt, sind bei der Lösung unter Umständen die folgenden Vorschläge hilfreich.

## <a name="cluster-setup-failures"></a>Fehler bei der Clustereinrichtung
### <a name="cannot-clean-up-service-fabric-logs"></a>Bereinigen von Service Fabric-Protokollen nicht möglich
#### <a name="problem"></a>Problem
Während der Ausführung des DevClusterSetup-Skripts wird ein Fehler der folgenden Art angezeigt:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Lösung
Schließen Sie das aktuelle PowerShell-Fenster, und öffnen Sie ein neues PowerShell-Fenster als Administrator. Nun sollte es möglich sein, das Skript auszuführen.

## <a name="cluster-connection-failures"></a>Clusterverbindungsfehler
### <a name="service-fabric-powershell-cmdlets-are-not-recognized-in-azure-powershell"></a>Service Fabric-PowerShell-Cmdlets werden in Azure PowerShell nicht erkannt.
#### <a name="problem"></a>Problem
Wenn Sie versuchen, die Service Fabric-PowerShell-Cmdlets auszuführen, z.B. `Connect-ServiceFabricCluster` in einem Azure PowerShell-Fenster, scheitert der Vorgang mit der Fehlermeldung, dass das Cmdlet nicht erkannt wird. Der Grund dafür ist, Azure PowerShell die 32-Bit-Version von Windows PowerShell (gilt auch für 64-Bit-Betriebssystemversionen) verwendet, während die Service Fabric-Cmdlets nur in 64-Bit-Umgebungen ausgeführt werden.

#### <a name="solution"></a>Lösung
Führen Sie Service Fabric-Cmdlets immer direkt über Windows PowerShell aus.

> [!NOTE]
> Die neueste Version von Azure PowerShell erstellt keine spezielle Tastenkombination, daher sollte dieses Problem nicht mehr auftreten.
> 
> 

### <a name="type-initialization-exception"></a>Ausnahme bei der Typinitialisierung
#### <a name="problem"></a>Problem
Wenn Sie eine Verbindung mit dem Cluster in PowerShell oder im Service Fabric Explorer herstellen, wird für System.Fabric.Common.AppTrace eine TypeInitializationException angezeigt.

#### <a name="solution"></a>Lösung
Die Pfadvariable wurde während der Installation nicht ordnungsgemäß festgelegt. Melden Sie sich von Windows ab und wieder an. Dadurch wird der Pfad aktualisiert.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Fehler „Objekt ist geschlossen“ für Clusterverbindung
#### <a name="problem"></a>Problem
Für einen Aufruf von Connect-ServiceFabricCluster tritt ein Fehler der folgenden Art auf:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Lösung
Schließen Sie das aktuelle PowerShell-Fenster, und öffnen Sie ein neues PowerShell-Fenster als Administrator. Nun sollte es möglich sein, die Verbindung herzustellen.

### <a name="fabric-connection-denied-exception"></a>Ausnahme „Fabric-Verbindung verweigert“
#### <a name="problem"></a>Problem
Beim Debuggen von Visual Studio erhalten Sie einen FabricConnectionDeniedException-Fehler.

#### <a name="solution"></a>Lösung
Dieser Fehler tritt gewöhnlich beim Versuch auf, einen Diensthostprozess manuell zu starten, anstatt der Service Fabric-Laufzeit das Starten zu überlassen.

Stellen Sie sicher, dass in Ihrer Projektmappe keine Dienstprojekte als Startprojekte festgelegt sind. Nur Service Fabric-Anwendungsprojekte sollten als Startprojekte festgelegt werden.

> [!TIP]
> Wenn sich der lokale Cluster nach dem Setup ungewöhnlich verhält, können Sie ihn mit der Taskleistenanwendung „Lokaler Cluster-Manager“ zurücksetzen. Dadurch wird der vorhandene Cluster entfernt und ein neuer Cluster eingerichtet. Beachten Sie, dass alle bereitgestellten Anwendungen und zugehörigen Daten entfernt werden.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Grundlagen und Problembehandlung für Cluster mit Systemintegritätsberichten](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Visualisieren Ihres Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)


