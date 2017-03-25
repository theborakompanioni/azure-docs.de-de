---
title: "Azure CLI-Skript – Überwachen und Skalieren einer einzelnen SQL-Datenbank | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit der Azure CLI"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: sample
ms.devlang: CLI
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 03/14/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 4cfc4ab09f7adead289cb949373730bffcfa13ec
ms.lasthandoff: 03/10/2017

---

# <a name="monitor-and-scale-a-single-sql-database-using-the-azure-cli"></a>Überwachen und Skalieren einer einzelnen SQL­-Datenbank mit der Azure CLI

Dieses CLI-Beispielskript skaliert eine einzelne Azure SQL-Datenbank nach Abfrage der Größeninformationen für die Datenbank auf eine andere Leistungsstufe. 

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde. 

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts unter Windows finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).


## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Überwachen und Skalieren einer einzelnen SQL­Datenbank")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#create) | Erstellt einen logischen Server, der eine Datenbank hostet. |
| [az sql db show-usage](https://docs.microsoft.com/cli/azure/sql/db#show-usage) | Zeigt die Größennutzungsinformationen für eine Datenbank an. |
| [az sql db update](https://docs.microsoft.com/cli/azure/sql/db#update) | Aktualisiert Datenbankeigenschaften (z.B. die Dienstebene oder Leistungsstufe) oder verschiebt eine Datenbank in Pools für elastische Datenbanken, daraus hinaus oder zwischen ihnen. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |
|||

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../sql-database-cli-samples.md).

