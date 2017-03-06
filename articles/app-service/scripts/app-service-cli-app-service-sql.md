---
title: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung einer Web-App mit einer SQL­Datenbank | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung einer Web-App mit einer SQL­Datenbank"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 7c2efdd0-f553-4038-a77a-e953021b3f77
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 02/21/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 97ab792a0ad7420e97dbab378736b9815356e8a4
ms.openlocfilehash: 72c91ef9c59fb42e7a2a233e258622417b2f84ac
ms.lasthandoff: 02/27/2017

---

# <a name="connect-a-web-app-to-a-sql-database"></a>Herstellen einer Verbindung einer Web-App mit einer SQL­Datenbank

In diesem Szenario erfahren Sie, wie Sie eine Azure SQL-Datenbank und eine Azure-Web-App erstellen. Anschließend verknüpfen Sie die SQL-Datenbank mithilfe von App-Einstellungen mit der Web-App.

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/app-service/connect-to-sql/connect-to-sql.sh?highlight=9-10 "SQL-Datenbank")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die App Service-App und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App, eine SQL-Datenbank und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Erstellt eine Azure-Web-App im App Service-Plan. |
| [az sql server create](https://docs.microsoft.com/cli/azure/sql/server#create) | Erstellt einen SQL-Datenbankserver.  |
| [az sql db create](https://docs.microsoft.com/cli/azure/sql/db#create) | Erstellt eine neue Datenbank mit dem SQL-Datenbankserver. |
| [az appservice web config appsetings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) | Erstellt oder aktualisiert eine App-Einstellung für eine Azure-Web-App. App-Einstellungen werden als Umgebungsvariablen für Ihre App verfügbar gemacht. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).
