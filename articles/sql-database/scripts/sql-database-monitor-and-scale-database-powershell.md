---
title: "Azure PowerShell-Skript – Überwachen und Skalieren einer einzelnen SQL-Datenbank | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit PowerShell"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: sample
ms.devlang: PowerShell
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 03/07/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 1d815620192af9b3364f2dfda71749caf314abc4
ms.lasthandoff: 03/30/2017

---

# <a name="monitor-and-scale-a-single-sql-database-using-powershell"></a>Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit PowerShell

Dieses PowerShell-Beispielskript überwacht die Leistungsmetriken einer Datenbank, skaliert sie auf eine höhere Leistungsstufe und erstellt eine Warnungsregel auf einer der Leistungsmetriken. 

[!INCLUDE [sample-cli-install](../../../includes/sample-powershell-install-no-ssh.md)]

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.ps1 "Überwachen und Skalieren einer einzelnen SQL­Datenbank")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
 [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.5.0/new-azurermsqlserver) | Erstellt einen logischen Server, der eine Datenbank oder einen Pool für elastische Datenbanken hostet. |
| [Get-AzureRmMetric](https://docs.microsoft.com/powershell/resourcemanager/azurerm.insights/v2.5.0/get-azurermmetric) | Zeigt die Größennutzungsinformationen für die Datenbank an.|
| [Set-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.5.0/set-azurermsqldatabase) | Aktualisiert Datenbankeigenschaften oder verschiebt eine Datenbank in Pools für elastische Datenbanken, daraus hinaus oder zwischen ihnen. |
| [Add-AzureRMMetricAlertRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.insights/v2.5.0/add-azurermmetricalertrule) | Legt eine Warnungsregel zur zukünftigen automatischen Überwachung von DTUs fest. |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie in den [Azure PowerShell samples for Azure SQL Database](../sql-database-powershell-samples.md) (Azure PowerShell-Beispiele für Azure SQL-Datenbank).

