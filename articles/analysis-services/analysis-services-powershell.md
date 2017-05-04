---
title: Verwalten von Azure Analysis Services mit PowerShell | Microsoft-Dokumentation
description: Verwaltung von Azure Analysis Services mit PowerShell
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: dbe1dba78fb2597dc595f9e6895d0a15181c1f05
ms.lasthandoff: 04/27/2017


---

# <a name="manage-azure-analysis-services-with-powershell"></a>Verwalten von Azure Analysis Services mit PowerShell

Dieser Artikel beschreibt PowerShell-Cmdlets, die zum Ausführen von Azure Analysis Services-Verwaltungsaufgaben für Server und Datenbanken verwendet werden. 

Für Serververwaltungsaufgaben wie das Erstellen oder Löschen eines Servers, das Anhalten oder Fortsetzen von Servervorgängen oder das Ändern des Servicelevels (Tarif) werden Azure Resource Manager (AzureRM)-Cmdlets verwendet. Für andere Aufgaben zum Verwalten von Datenbanken, z.B. Hinzufügen oder Entfernen von Rollenmitgliedern, Verarbeiten oder Partitionieren, werden die gleichen-Cmdlets im [SQLASCMDLETS](https://msdn.microsoft.com/library/hh758425.aspx)-Modul wie bei SQL Server Analysis Services verwendet.

## <a name="permissions"></a>Berechtigungen
Die meisten PowerShell-Aufgaben erfordern, dass Sie über Administratorberechtigungen auf dem verwalteten Analysis Services-Server verfügen. Geplante PowerShell-Aufgaben sind unbeaufsichtigte Vorgänge. Das Konto, in dem der Scheduler ausgeführt wird, muss auf dem Analysis Services-Server über Administratorrechte verfügen. 

Bei Servervorgängen, für die AzureRm-Cmdlets verwendet werden, muss Ihr Konto oder das Konto mit dem Scheduler außerdem zur Rolle „Besitzer“ für die Ressource in der [rollenbasierten Zugriffssteuerung in Azure](../active-directory/role-based-access-control-what-is.md) gehören. 

## <a name="server-operations"></a>Servervorgänge 
Azure Analysis Services-Cmdlets sind im Komponentenmodul [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) enthalten. Informationen zum Installieren von AzureRM-Cmdlet-Modulen, finden Sie im PowerShell-Katalog unter [Azure Resource Manager-Cmdlets](/powershell/azure/overview).

|Cmdlet|Beschreibung| 
|------------|-----------------| 
|[Get-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/get-azurermanalysisservicesserver)|Ruft die Details einer Serverinstanz ab.|  
|[New-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/new-azurermanalysisservicesserver)|Erstellt eine neue Serverinstanz.|
|[Remove-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/remove-azurermanalysisservicesserver)|Entfernt eine Serverinstanz.|  
|[Suspend-AzureRmAnalysisServicesServe](/powershell/module/azurerm.analysisservices/suspend-azurermanalysisservicesserver)|Hält eine Serverinstanz an.| 
|[Resume-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/resume-azurermanalysisservicesserver)|Setzt eine Serverinstanz fort.|  
|[Set-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver)|Ändert eine Serverinstanz.|   
|[Test-AzureRmAnalysisServicesServer](/powershell/module/azurerm.analysisservices/test-azurermanalysisservicesserver)|Überprüft das Vorhandensein einer Serverinstanz.| 

## <a name="database-operations"></a>Datenbankvorgänge
Für Vorgänge der Azure Analysis Services-Datenbank wird das gleiche [SQLASCMDLETS](https://msdn.microsoft.com/library/hh758425.aspx)-Modul wie bei SQL Server Analysis Services verwendet. Allerdings werden nicht alle Cmdlets für Azure Analysis Services unterstützt. 

Das SQLASCMDLETS-Modul bietet aufgabenspezifische Cmdlets für die Datenbankverwaltung sowie das allgemeine Cmdlet „Invoke-ASCmd“, das TMSL (Tabular Model Scripting Language)-Abfragen und -Skripts akzeptiert. Die folgenden Cmdlets im SQLASCMDLETS-Modul werden von Azure Analysis Services unterstützt.
  
|Cmdlet|Beschreibung|
|------------|-----------------| 
|[Add-RoleMember](https://msdn.microsoft.com/library/hh510167.aspx)|Hinzufügen eines Mitglieds zu einer Datenbankrolle.| 
|[Backup-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet)|Sichern einer Analysis Services-Datenbank.|  
|[Remove-RoleMember](https://msdn.microsoft.com/library/hh510173.aspx)|Entfernen eines Mitglieds aus einer Datenbankrolle.|   
|[Invoke-ASCmd](https://msdn.microsoft.com/library/hh479579.aspx)|Ausführen eines TMSL-Skripts.|
|[Invoke-ProcessASDatabase](https://msdn.microsoft.com/library/mt651773.aspx)|Verarbeiten einer Datenbank.|  
|[Invoke-ProcessPartition](https://msdn.microsoft.com/library/hh510164.aspx)|Verarbeiten einer Partition.| 
|[Invoke-ProcessTable](https://msdn.microsoft.com/library/mt651774.aspx)|Verarbeiten einer Tabelle.|  
|[Merge-Partition](https://msdn.microsoft.com/library/hh479576.aspx)|Zusammenführen einer Partition.|  
|[Restore-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet)|Wiederherstellen einer Analysis Services-Datenbank.| 
  

## <a name="related-information"></a>Verwandte Informationen
* [PowerShell-Skripterstellung in Analysis Services](https://msdn.microsoft.com/library/hh213141.aspx)
* [Programmierung von tabellarischen Modellen für den Kompatibilitätsgrad 1200](https://msdn.microsoft.com/library/mt712541.aspx)

