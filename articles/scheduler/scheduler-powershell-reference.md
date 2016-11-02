<properties
 pageTitle="Scheduler – PowerShell-Cmdlets-Referenz"
 description="Scheduler – PowerShell-Cmdlets-Referenz"
 services="scheduler"
 documentationCenter=".NET"
 authors="krisragh"
 manager="dwrede"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/18/2016"
 ms.author="krisragh"/>

# Scheduler – PowerShell-Cmdlets-Referenz

In der folgenden Tabelle sind die wichtigsten Cmdlets in Azure Scheduler beschrieben. Sie enthält zudem Links zu den jeweiligen Referenzseiten dieser Cmdlets.

Um Azure PowerShell zu installieren und Ihrem Azure-Abonnement zuzuordnen, lesen Sie [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Weitere Informationen zu [Azure Resource Manager-Cmdlets](https://msdn.microsoft.com/library/mt125356(v=azure.200).aspx) finden Sie unter [Verwenden von Azure PowerShell mit Azure Resource Manager](../powershell-azure-resource-manager.md).

|Cmdlet|Beschreibung des Cmdlets|
|---|---|
[Disable-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490133(v=azure.200).aspx) |Deaktiviert eine Auftragssammlung. 
[Enable-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490135(v=azure.200).aspx) |Aktiviert eine Auftragssammlung.
[Get-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490125(v=azure.200).aspx) |Ruft Scheduler-Aufträge ab.
[Get-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490132(v=azure.200).aspx) |Ruft Auftragssammlungen ab.
[Get-AzureRmSchedulerJobHistory](https://msdn.microsoft.com/library/mt490126(v=azure.200).aspx) |Ruft den Auftragsverlauf ab.
[Neue AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490136(v=azure.200).aspx) |Erstellt einen HTTP-Auftrag.
[New-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490141(v=azure.200).aspx) |Erstellt eine Auftragssammlung.
[New-AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490134(v=azure.200).aspx) |Erstellt einen Service Bus-Warteschlangenauftrag.
[New-AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490142(v=azure.200).aspx) |Erstellt einen Service Bus-Themenauftrag.
[New-AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490127(v=azure.200).aspx) |Erstellt einen Speicherwarteschlangenauftrag. 
[Remove-AzureRmSchedulerJob](https://msdn.microsoft.com/library/mt490140(v=azure.200).aspx) |Entfernt einen Scheduler-Auftrag.  
[Remove-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490131(v=azure.200).aspx) |Entfernt eine Auftragssammlung. 
[Set-AzureRmSchedulerHttpJob](https://msdn.microsoft.com/library/mt490130(v=azure.200).aspx) |Ändert einen Scheduler-HTTP-Auftrag.
[Set-AzureRmSchedulerJobCollection](https://msdn.microsoft.com/library/mt490129(v=azure.200).aspx) |Ändert eine Auftragssammlung. 
[Set-AzureRmSchedulerServiceBusQueueJob](https://msdn.microsoft.com/library/mt490143(v=azure.200).aspx) |Ändert einen Service Bus-Warteschlangenauftrag.  
[Set-AzureRmSchedulerServiceBusTopicJob](https://msdn.microsoft.com/library/mt490137(v=azure.200).aspx) |Ändert einen Service Bus-Themenauftrag. 
[Set-AzureRmSchedulerStorageQueueJob](https://msdn.microsoft.com/library/mt490128(v=azure.200).aspx) |Ändert einen Speicherwarteschlangenauftrag.   

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

## Weitere Informationen


 [Was ist Azure Scheduler?](scheduler-intro.md)

 [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)

 [Erste Schritte mit dem Scheduler im Azure-Portal](scheduler-get-started-portal.md)

 [Pläne und Abrechnung in Azure Scheduler](scheduler-plans-billing.md)

 [Azure Scheduler-REST-API – Referenz](https://msdn.microsoft.com/library/mt629143)

 [Hochverfügbarkeit und Zuverlässigkeit von Azure Scheduler](scheduler-high-availability-reliability.md)

 [Einschränkungen, Standardwerte und Fehlercodes für Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Ausgehende Authentifizierung von Azure Scheduler](scheduler-outbound-authentication.md)

<!---HONumber=AcomDC_0914_2016-->