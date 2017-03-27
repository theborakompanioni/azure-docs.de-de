---
title: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung einer Web-App mit DocumentDB | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung einer Web-App mit DocumentDB"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 02/21/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 80b6d446fd9a0ac6434c8415b97ff0f07eb0da12
ms.lasthandoff: 03/11/2017

---

# <a name="connect-a-web-app-to-a-documentdb"></a>Herstellen einer Verbindung einer Web-App mit einer DocumentDB-Instanz

In diesem Szenario erfahren Sie, wie Sie eine Azure DocumentDB-Instanz und eine Azure-Web-App erstellen. Anschließend verknüpfen Sie die DocumentDB-Instanz mithilfe von App-Einstellungen mit der Web-App.

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure DocumentDB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App, die DocumentDB-Instanz und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Erstellt eine Azure-Web-App im App Service-Plan. |
| [az documentdb create](https://docs.microsoft.com/en-us/cli/azure/documentdb#create) | Erstellt eine DocumentDB-Instanz. Hier werden die Daten gespeichert. |
| [az documentdb list-keys](https://docs.microsoft.com/en-us/cli/azure/documentdb#list-keys) | Listet die Zugriffsschlüssel für das angegebene Azure DocumentDB-Datenbankkonto auf. |
| [az appservice web config appsetings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) | Erstellt oder aktualisiert eine App-Einstellung für eine Azure-Web-App. App-Einstellungen werden als Umgebungsvariablen für Ihre App verfügbar gemacht. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).
