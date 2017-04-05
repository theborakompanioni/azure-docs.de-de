---
title: "Azure CLI-Skript – Skalieren eines Pools für elastische Datenbanken| Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Skalieren eines Pools für elastische Datenbanken"
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: sample
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 04/04/2017
ms.author: janeng
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: ec38e5cdc3a0ce9df8cbd7dd46499d52a9f04ee4
ms.lasthandoff: 03/30/2017

---

# <a name="scale-an-elastic-pool-in-azure-sql-database-using-the-azure-cli"></a>Skalieren eines Pools für elastische Datenbanken in Azure SQL-Datenbank mit der Azure CLI

Dieses CLI-Beispielskript erstellt Pools für elastische Datenbanken, verschiebt in einem Pool zusammengefasste Datenbanken und ändert Leistungsstufen. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/sql-database/scale-pool/scale-pool.sh "Verschieben der Datenbank zwischen Pools")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe und alle damit verbundenen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Dieses Skript verwendet die folgenden Befehle, um Ressourcengruppe, logischen Server, SQL-Datenbank und Firewallregeln zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#create) | Erstellt einen logischen Server, der die SQL-Datenbank hostet. |
| [az sql elastic-pools create](https://docs.microsoft.com/cli/azure/sql/elastic-pools#create) | Erstellt einen Pool für elastische Datenbanken auf dem logischen Server. |
| [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#create) | Erstellt die SQL-Datenbank auf dem logischen Server. |
| [az sql elastic-pools update](https://docs.microsoft.com/cli/azure/sql/elastic-pools#update) | Aktualisiert einen Pool für elastische Datenbanken – in diesem Beispiel wird die zugewiesene eDTU geändert. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#set) | Löscht eine Ressourcengruppe einschließlich aller geschachtelten Ressourcen. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche SQL-Datenbank-CLI-Skriptbeispiele finden Sie in der [Azure SQL-Datenbank-Dokumentation](../sql-database-cli-samples.md).

