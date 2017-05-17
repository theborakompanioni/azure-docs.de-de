---
title: "Azure CLI-Skript – Erstellen einer Failoverrichtlinie für hohe Verfügbarkeit | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Failoverrichtlinie für hohe Verfügbarkeit"
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
ms.openlocfilehash: 285e50ed5e0596bb18e2fb9124992af34da71f1e
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---

# <a name="create-a-failover-policy-for-high-availability-using-the-azure-cli"></a>Erstellen einer Failoverrichtlinie für hohe Verfügbarkeit mithilfe der Azure CLI

Mithilfe dieses CLI-Beispielskripts wird ein Azure Cosmos DB-Konto erstellt und anschließend für hohe Verfügbarkeit konfiguriert.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/cosmosdb/high-availability-cosmosdb-configure-failover/high-availability-cosmosdb-configure-failover.sh?highlight=23-27 "Erstellen einer Failoverrichtlinie für Azure Cosmos DB")]

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
| [az cosmosdb create](/cli/azure/sql/server#create) | Erstellt ein Konto für Azure Cosmos DB |
| [az cosmosdb update](/cli/azure/cosmosdb#update) | Aktualisiert ein Konto für Azure Cosmos DB |
| [az group delete](/cli/azure/resource#delete) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele für Azure Cosmos DB finden Sie in der [Dokumentation zur Azure Cosmos DB-CLI](../cli-samples.md).

