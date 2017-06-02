---
title: "Azure CLI-Skript – Erstellen eines MongoDB-API-Kontos, einer Datenbank und einer Sammlung für Azure Cosmos DB | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen eines MongoDB-API-Kontos, einer Datenbank und einer Sammlung für Azure Cosmos DB"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 416fb332681b79ac6152d4c900547d600e8f6d1b
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---

# <a name="azure-cosmos-db-create-an-mongodb-api-account-using-the-azure-cli"></a>Azure Cosmos DB: Erstellen eines MongoDB-API-Kontos über die Azure CLI

Mit diesem CLI-Beispielskript erstellen Sie ein MongoDB-API-Konto, eine Datenbank und eine Sammlung für Azure Cosmos DB. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-mongodb-account/create-cosmosdb-mongodb-account.sh?highlight=15-35 "Erstellen eines MongoDB-API-Kontos, einer Datenbank und einer Sammlung für Azure Cosmos DB")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az cosmosdb create](/cli/azure/cosmosdb#create) | Erstellt ein Konto für Azure Cosmos DB. |
| [az group delete](/cli/azure/resource#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele für Azure Cosmos DB finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](../cli-samples.md).

