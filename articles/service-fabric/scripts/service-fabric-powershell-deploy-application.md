---
title: "Azure PowerShell-Skriptbeispiel – Bereitstellen einer Anwendung in einem Cluster | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Bereitstellen einer Anwendung in einem Service Fabric-Cluster."
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
ms.openlocfilehash: b9aa30e18f6997050f29792e53946d5ec747e9ff
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---

# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Bereitstellen einer Anwendung in einem Service Fabric-Cluster

Dieses Beispielskript kopiert ein Anwendungspaket in einen Clusterimagespeicher, registriert den Anwendungstyp im Cluster und erstellt eine Anwendungsinstanz aus dem Anwendungstyp.  Wenn im Anwendungsmanifest des Zielanwendungstyps Standarddienste festgelegt wurden, werden diese Dienste in diesem Schritt erstellt. Passen Sie die Parameter nach Bedarf an. 

Wenn Sie das Service Fabric-PowerShell-Modul benötigen, installieren Sie es zusammen mit dem [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Bereitstellen einer Anwendung in einem Cluster")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nachdem das Beispielskript ausgeführt wurde, kann das Skript unter [Entfernen einer Anwendung](service-fabric-powershell-remove-application.md) dazu verwendet werden, die Anwendungsinstanz zu entfernen, die Registrierung des Anwendungstyps aufzuheben und das Anwendungspaket aus dem Imagespeicher zu löschen.

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Kopiert ein Anwendungspaket in den Clusterimagespeicher  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Registriert einen Anwendungstyp und eine Version auf dem Cluster |
|[New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Erstellt eine Anwendung aus einem registrierten Anwendungstyp |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Fabric-PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/service-fabric/?view=azureservicefabricps).

Zusätzliche PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).

