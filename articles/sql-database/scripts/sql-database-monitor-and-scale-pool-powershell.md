---
title: "Azure PowerShell-Skript – Überwachen und Skalieren eines SQL-Datenbank-Pools für elastische Datenbanken | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Überwachen und Skalieren eines SQL-Datenbank-Pools für elastische Datenbanken mit PowerShell"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: mvc
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 05/23/2017
ms.author: janeng
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: 5231f5b13c3502a5a45ceea0b329f30a2e1c311c
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017

---

# <a name="monitor-and-scale-a-sql-database-elastic-pool-using-powershell"></a>Überwachen und Skalieren eines SQL-Datenbank-Pools für elastische Datenbanken mit PowerShell

Dieses PowerShell-Beispielskript überwacht die Leistungsmetriken eines Pools für elastische Datenbanken, skaliert ihn auf eine höhere Leistungsstufe und erstellt eine Warnungsregel auf einer der Leistungsmetriken. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-pool/monitor-and-scale-pool.ps1?highlight=16-17 "Überwachen und Skalieren einer einzelnen SQL­Datenbank")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
 [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) | Erstellt einen logischen Server, der eine Datenbank oder einen Pool für elastische Datenbanken hostet. |
| [New-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/new-azurermsqlelasticpool) | Erstellt einen Pool für elastische Datenbanken auf einem logischen Server. |
| [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) | Erstellt eine Datenbank auf einem logischen Server als einzelne oder zu einem Pool gehörende Datenbank. |
| [Get-AzureRmMetric](/powershell/module/azurerm.insights/get-azurermmetric) | Zeigt die Größennutzungsinformationen für die Datenbank an.|
| [Add-AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Fügt eine metrikbasierte Warnungsregel hinzu oder aktualisiert sie. |
| [Set-AzureRmSqlElasticPool](/powershell/module/azurerm.sql/set-azurermsqlelasticpool) | Aktualisiert die Eigenschaften von Pool für elastische Datenbanken. |
| [Add-AzureRMMetricAlertRule](/powershell/module/azurerm.insights/add-azurermmetricalertrule) | Legt eine Warnungsregel zur zukünftigen automatischen Überwachung von DTUs fest. |
| [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/overview).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie in den [Azure PowerShell samples for Azure SQL Database](../sql-database-powershell-samples.md) (Azure PowerShell-Beispiele für Azure SQL-Datenbank).

