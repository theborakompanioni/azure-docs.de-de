---
title: "Scheduler – PowerShell-Cmdlets-Referenz"
description: "Scheduler – PowerShell-Cmdlets-Referenz"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 9a26c457-d7a1-4e4a-bc79-f26592155218
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 419e489e4e99935dea49b2d3f9a405013b94048c
ms.lasthandoff: 04/27/2017


---
# <a name="scheduler-powershell-cmdlets-reference"></a>Scheduler – PowerShell-Cmdlets-Referenz
In der folgenden Tabelle sind die wichtigsten Cmdlets in Azure Scheduler beschrieben. Sie enthält zudem Links zu den jeweiligen Referenzseiten dieser Cmdlets.

Um Azure PowerShell zu installieren und Ihrem Azure-Abonnement zuzuordnen, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview). 

Weitere Informationen zu [Azure Resource Manager-Cmdlets](/powershell/azure/overview) finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).

| Cmdlet | Beschreibung des Cmdlets |
| --- | --- |
| [Disable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/disable-azurermschedulerjobcollection) |Deaktiviert eine Auftragssammlung. |
| [Enable-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/enable-azurermschedulerjobcollection) |Aktiviert eine Auftragssammlung. |
| [Get-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/get-azurermschedulerjob) |Ruft Scheduler-Aufträge ab. |
| [Get-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/get-azurermschedulerjobcollection) |Ruft Auftragssammlungen ab. |
| [Get-AzureRmSchedulerJobHistory](/powershell/module/azurerm.scheduler/get-azurermschedulerjobhistory) |Ruft den Auftragsverlauf ab. |
| [New-AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/new-azurermschedulerhttpjob) |Erstellt einen HTTP-Auftrag. |
| [New-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/new-azurermschedulerjobcollection) |Erstellt eine Auftragssammlung. |
| [New-AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebusqueuejob) |Erstellt einen Service Bus-Warteschlangenauftrag. |
| [New-AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/new-azurermschedulerservicebustopicjob) |Erstellt einen Service Bus-Themenauftrag. |
| [New-AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/new-azurermschedulerstoragequeuejob) |Erstellt einen Speicherwarteschlangenauftrag. |
| [Remove-AzureRmSchedulerJob](/powershell/module/azurerm.scheduler/remove-azurermschedulerjob) |Entfernt einen Scheduler-Auftrag. |
| [Remove-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/remove-azurermschedulerjobcollection) |Entfernt eine Auftragssammlung. |
| [Set-AzureRmSchedulerHttpJob](/powershell/module/azurerm.scheduler/set-azurermschedulerhttpjob) |Ändert einen Scheduler-HTTP-Auftrag. |
| [Set-AzureRmSchedulerJobCollection](/powershell/module/azurerm.scheduler/set-azurermschedulerjobcollection) |Ändert eine Auftragssammlung. |
| [Set-AzureRmSchedulerServiceBusQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebusqueuejob) |Ändert einen Service Bus-Warteschlangenauftrag. |
| [Set-AzureRmSchedulerServiceBusTopicJob](/powershell/module/azurerm.scheduler/set-azurermschedulerservicebustopicjob) |Ändert einen Service Bus-Themenauftrag. |
| [Set-AzureRmSchedulerStorageQueueJob](/powershell/module/azurerm.scheduler/set-azurermschedulerstoragequeuejob) |Ändert einen Speicherwarteschlangenauftrag. |

Weitere detaillierte Informationen erhalten Sie, wenn Sie die folgenden Cmdlets ausführen: 

```
Get-Help <cmdlet name> -Detailed
```
```
Get-Help <cmdlet name> -Examples
```
```
Get-Help <cmdlet name> -Full
```

## <a name="see-also"></a>Weitere Informationen
 [Was ist Azure Scheduler?](scheduler-intro.md)

 [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)

 [Erste Schritte mit dem Scheduler im Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler-REST-API – Referenz](https://msdn.microsoft.com/library/mt629143)

 [Hochverfügbarkeit und Zuverlässigkeit von Azure Scheduler](scheduler-high-availability-reliability.md)

 [Einschränkungen, Standardwerte und Fehlercodes für Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Ausgehende Authentifizierung von Azure Scheduler](scheduler-outbound-authentication.md)


