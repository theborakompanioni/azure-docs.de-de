---
title: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung zwischen einer Web-App und Cosmos DB | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung zwischen einer Web-App und Cosmos DB"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: 46cffb8949f6c8d8b51f952e52c3ddc01839389a
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---

# <a name="connect-a-web-app-to-cosmos-db"></a>Herstellen einer Verbindung zwischen einer Web-App und Cosmos DB

In diesem Szenario erfahren Sie, wie Sie ein Azure Cosmos DB-Konto und eine Azure-Web-App erstellen. Anschließend verknüpfen Sie die Cosmos DB-Instanz mithilfe von App-Einstellungen mit der Web-App.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App, die Cosmos DB-Instanz und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Erstellt eine Azure-Web-App im App Service-Plan. |
| [az cosmosdb create](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#create) | Erstellt ein Cosmos DB-Konto. Hier werden die Daten gespeichert. |
| [az cosmosdb list-keys](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#list-keys) | Listet die Zugriffsschlüssel für das angegebene Cosmos DB-Konto auf. |
| [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) | Erstellt oder aktualisiert eine App-Einstellung für eine Azure-Web-App. App-Einstellungen werden als Umgebungsvariablen für Ihre App verfügbar gemacht. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).

