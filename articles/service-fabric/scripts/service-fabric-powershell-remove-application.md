---
title: "Azure PowerShell-Skriptbeispiel – Entfernen einer Anwendung aus einem Cluster | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Entfernen einer Anwendung aus einem Service Fabric-Cluster."
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
ms.topic: article
ms.date: 06/20/2017
ms.author: ryanwi
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 951c9a014f587ce53450dd705fc478da625b1e6b
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Entfernen einer Anwendung aus einem Service Fabric-Cluster

Dieses Beispielskript löscht eine ausgeführte Service Fabric-Anwendungsinstanz, hebt die Registrierung für einen Anwendungstyp und eine Version aus dem Cluster auf und löscht das Anwendungspaket aus dem Clusterimagespeicher.  Durch das Löschen der Anwendungsinstanz werden auch alle ausgeführten Dienstinstanzen gelöscht, die dieser Anwendung zugeordnet sind. Passen Sie die Parameter nach Bedarf an. 

Wenn Sie das Service Fabric-PowerShell-Modul benötigen, installieren Sie es zusammen mit dem [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Entfernen einer Anwendung aus einem Cluster")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | Entfernt eine ausgeführte Service Fabric-Anwendungsinstanz aus dem Cluster  |
| [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Hebt die Registrierung eines Service Fabric-Anwendungstyps und einer Version aus dem Cluster auf |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Entfernt ein Service Fabric-Anwendungspakets aus dem Imagespeicher|

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Fabric-PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/service-fabric/?view=azureservicefabricps).

Zusätzliche PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).

