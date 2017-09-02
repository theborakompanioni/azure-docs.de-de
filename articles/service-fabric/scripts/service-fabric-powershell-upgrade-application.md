---
title: "Azure PowerShell-Skriptbeispiel – Durchführen eines Upgrades für eine Service Fabric-Anwendung | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Durchführen eines Upgrades für eine Service Fabric-Anwendung."
services: service-fabric
documentationcenter: 
author: rwike77
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 08/23/2017
ms.author: ryanwi
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 43c1c0d38d12a36c39a3962a3399d9c05937e8b1
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---

# <a name="upgrade-a-service-fabric-application"></a>Durchführen eines Upgrades für eine Service Fabric-Anwendung

Mit diesem Beispielskript wird ein Upgrade für eine ausgeführte Service Fabric-Anwendungsinstanz auf Version 1.3.0 durchgeführt. Das Skript kopiert das neue Anwendungspaket in den Clusterimagespeicher, registriert den Anwendungstyp, startet ein überwachtes Upgrade und prüft kontinuierlich den Upgradestatus, bis das Upgrade abgeschlossen ist oder ein Rollback ausgeführt wird. Passen Sie die Parameter nach Bedarf an. 

Wenn Sie das Service Fabric-PowerShell-Modul benötigen, installieren Sie es zusammen mit dem [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Durchführen eines Upgrades für eine Anwendung")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Ruft alle Anwendungen im Service Fabric-Cluster oder in einer bestimmten Anwendung ab.  |
| [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | Ruft den Status eines Service Fabric-Anwendungsupgrades ab. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Ruft die für den Service Fabric-Cluster registrierten Service Fabric-Anwendungstypen ab. |
| [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Hebt die Registrierung eines Service Fabric-Anwendungstyps auf.  |
| [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopiert ein Service Fabric-Anwendungspaket in den Imagespeicher.  |
| [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Registriert einen Service Fabric-Anwendungstyp. |
| [Start-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | Führt ein Upgrade für eine Service Fabric-Anwendung auf die angegebene Anwendungstypversion durch. |


## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Fabric-PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/service-fabric/?view=azureservicefabricps).

Zusätzliche PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).

