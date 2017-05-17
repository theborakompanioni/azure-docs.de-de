---
title: "Azure CLI-Skript – Erstellen eines DocumentDB-API-Kontos, einer Datenbank und einer Sammlung für Azure Cosmos DB | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen eines DocumentDB-API-Kontos, einer Datenbank und einer Sammlung für Azure Cosmos DB"
services: cosmosdb
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmosdb
ms.custom: sample
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 05/10/2017
ms.author: mimig
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 87c9bfc3d23ed2c21d989e06058c017e41009453
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="azure-cosmos-db-create-an-documentdb-api-account-using-cli"></a>Azure-Cosmos-DB: Erstellen eines DocumentDB-API-Kontos mithilfe der CLI

Mit diesem CLI-Beispielskript erstellen Sie ein DocumentDB-API-Konto, eine Datenbank und eine Sammlung für Azure Cosmos DB.  

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/cosmosdb/create-cosmosdb-account-database/create-cosmosdb-account-database.sh?highlight=15-35 "Erstellen eines DocumentDB-API-Kontos, einer Datenbank und einer Sammlung für Azure Cosmos DB")]

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
