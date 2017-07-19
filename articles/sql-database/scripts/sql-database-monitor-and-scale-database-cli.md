---
title: "CLI-Beispiel: Überwachen und Skalieren einer einzelnen Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Azure CLI-Beispielskript zum Überwachen und Skalieren einer einzelnen Azure SQL-Datenbank"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 01911b85268244a8fddb32aa726f8a870abbaf77
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---

# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>Überwachen und Skalieren einer einzelnen SQL-­Datenbank mithilfe der CLI

In diesem Azure CLI-Skriptbeispiel wird eine einzelne Azure SQL-Datenbank nach Abfrage der Größeninformationen der Datenbank auf eine andere Leistungsstufe skaliert. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Überwachen und Skalieren einer einzelnen SQL­Datenbank")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli-interactive
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

