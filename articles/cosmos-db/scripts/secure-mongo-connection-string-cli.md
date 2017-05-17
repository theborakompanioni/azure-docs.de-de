---
title: "Azure CLI-Skript: Abrufen der Azure Cosmos DB-Verbindungszeichenfolge für MongoDB-Apps| Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel: Abrufen der Azure Cosmos DB-Verbindungszeichenfolge für MongoDB-Apps"
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
ms.openlocfilehash: 58e16cfc83c2057168428e8e85a6ebd143d2cd07
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="get-an-azure-cosmos-db-connection-string-for-mongodb-apps-using-the-azure-cli"></a>Abrufen einer Azure Cosmos DB-Verbindungszeichenfolge für MongoDB-Apps mithilfe der Azure CLI

Dieses Beispiel ruft eine Azure Cosmos DB-Verbindungszeichenfolge für MongoDB-Apps mithilfe der Azure CLI ab. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/cosmosdb/secure-cosmosdb-get-mongodb-connection-string/secure-cosmosdb-get-mongodb-connection-string.sh?highlight=36-39 "Abrufen der Azure Cosmos DB-Verbindungszeichenfolge für MongoDB-Apps")]

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
| [az cosmosdb update](/cli/azure/cosmosdb/name#update) | Aktualisiert ein Azure Cosmos DB-Konto. |
| [az cosmosdb list-connection-strings](/cli/azure/cosmosdb/list-connection-strings) | Ruft die Verbindungszeichenfolge für das Konto ab.|
| [az group delete](/cli/azure/resource#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele für Azure Cosmos DB finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](../cli-samples.md).

