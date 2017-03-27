---
title: "Azure PowerShell-Skript – Kopieren einer SQL-Datenbank auf einen neuen Server | Microsoft-Dokumentation"
description: "Azure PowerShell-Skriptbeispiel – Kopieren einer SQL-Datenbank auf einen neuen Server mit PowerShell"
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
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 0f750cb3021f04ade9d639e2de14e1b119cde36e
ms.lasthandoff: 03/10/2017

---

# <a name="copy-a-sql-database-to-a-new-server-using-powershell"></a>Kopieren einer SQL-Datenbank auf einen neuen Server mit PowerShell

Dieses PowerShell-Beispielskript erstellt eine Kopie einer vorhandenen Datenbank auf einem neuen Server. 

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über das Cmdlet `Add-AzureRmAccount` eine Verbindung mit Azure hergestellt wurde.

## <a name="copy-a-database-to-a-new-server"></a>Kopieren einer Datenbank auf einen neuen Server

[!code-powershell[main](../../../powershell_scripts/sql-database/copy-database-to-new-server/copy-database-to-new-server.ps1 "Kopieren einer Datenbank auf einen neuen Server")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/new-azurermresourcegroup) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.5.0/new-azurermsqlserver) | Erstellt einen logischen Server, der eine Datenbank oder einen Pool für elastische Datenbanken hostet. |
| [New-AzureRmSqlDatabase](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.5.0/new-azurermsqldatabase) | Erstellt eine Datenbank auf einem logischen Server als einzelne oder zu einem Pool gehörende Datenbank. |
| [New-AzureRmSqlDatabaseCopy](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.5.0/new-azurermsqldatabasecopy) | Erstellt eine Kopie einer Datenbank, die die Momentaufnahme zum jetzigen Zeitpunkt verwendet. |
| [Remove-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/remove-azurermresourcegroup) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Azure PowerShell finden Sie in der [Azure PowerShell-Dokumentation](https://docs.microsoft.com/powershell/).

Zusätzliche PowerShell-Skriptbeispiele für SQL-Datenbank finden Sie in den [Azure PowerShell samples for Azure SQL Database](../sql-database-powershell-samples.md) (Azure PowerShell-Beispiele für Azure SQL-Datenbank).

