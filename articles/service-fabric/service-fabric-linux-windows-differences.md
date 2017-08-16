---
title: Azure Service Fabric-Unterschiede zwischen Linux und Windows | Microsoft-Dokumentation
description: Es wird beschrieben, welche Unterschiede zwischen der Azure Service Fabric-Vorschauversion unter Linux und Azure Service Fabric unter Windows bestehen.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: e2d21b28c482427c60f708171336e6901b50e544
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a>Unterschiede zwischen Service Fabric unter Linux (Vorschau) und Windows (allgemein verfügbar)

Da es sich bei Service Fabric unter Linux um eine Vorschauversion handelt, werden einige Features zwar unter Windows unterstützt, aber noch nicht unter Linux. Beide Featuresätze werden vollständig unterstützt, wenn Service Fabric unter Linux den Status der allgemeinen Verfügbarkeit erreicht. Bei künftigen Versionen nähern sich die Features immer weiter an. Zwischen den aktuellen verfügbaren Versionen (d.h. zwischen Version 5.6 unter Windows und Version 5.5 unter Linux) bestehen die folgenden Unterschiede: 

* Reliable Collections und zustandsbehaftete Reliable Services 
* ReverseProxy 
* Eigenständiges Installationsprogramm 
* XML-Schemaüberprüfung für Manifestdateien 
* Konsolenumleitung 
* Fault Analysis Service (FAS)
* Docker Compose sowie Volume- und Protokollierungstreiber für Container 
* Ressourcenkontrolle für Container und Dienste 
* DNS-Dienst
* Unterstützung für Azure Active Directory
* CLI-Entsprechungen von bestimmten PowerShell-Befehlen 
* Für einen Linux-Cluster kann nur eine Teilmenge von PowerShell-Befehlen ausgeführt werden (im nächsten Abschnitt beschrieben).

>[!NOTE]
>In Produktionsclustern wird die Konsolenumleitung auch unter Windows nicht unterstützt.

Die Entwicklungstools unterscheiden sich bei Windows und Linux ebenfalls. VisualStudio, PowerShell, VSTS und ETW werden unter Windows und Yeoman, Eclipse, Jenkins und LTTng unter Linux verwendet.

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>PowerShell-Cmdlets, die für einen Linux-Service Fabric-Cluster nicht funktionieren

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Cmd
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Nächste Schritte
* [Vorbereiten Ihrer Entwicklungsumgebung unter Linux](service-fabric-get-started-linux.md)
* [Prepare your development environment on OSX (Vorbereiten Ihrer Entwicklungsumgebung unter OSX)](service-fabric-get-started-mac.md)
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe von Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Erstellen und Bereitstellen Ihrer ersten Service Fabric-Java-Anwendung unter Linux mithilfe des Service Fabric-Plug-Ins für Eclipse](service-fabric-get-started-eclipse.md)
* [Erstellen der ersten Java-Anwendung unter Linux](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Verwalten von Service Fabric-Anwendungen mit der Azure-CLI](service-fabric-azure-cli.md)

