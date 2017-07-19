---
title: 'CLI-Beispiel: Erstellen einer Azure SQL-Datenbank | Microsoft-Dokumentation'
description: "Azure CLI-Beispielskript zum Erstellen einer SQL-­Datenbank"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 908898ca691d2b53b9f54afa60c41e091163bd50
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---

# <a name="use-cli-to-create-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>Erstellen einer einzelnen SQL-­Datenbank und Konfigurieren einer Firewallregel mit der Azure CLI

In diesem CLI-Skriptbeispiel wird eine einzelne Azure SQL-Datenbank erstellt und eine Firewallregel auf Serverebene konfiguriert. Nach erfolgreicher Ausführung des Skripts können alle Azure-Dienste und die konfigurierte IP-Adresse auf die SQL-Datenbank zugreifen. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "Erstellen einer SQL-Datenbank")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az sql server create](/cli/azure/sql/server#create) | Erstellt einen logischen Server, der die SQL-Datenbank hostet. |
| [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create) | Erstellt eine Firewallregel, die vom eingegebenen IP-Adressbereich aus den Zugriff auf alle SQL-Datenbanken auf dem Server ermöglicht. |
| [az sql db create](/cli/azure/sql/db#create) | Erstellt die SQL-Datenbank auf dem logischen Server. |
| [az group delete](/cli/azure/resource#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../sql-database-cli-samples.md).


