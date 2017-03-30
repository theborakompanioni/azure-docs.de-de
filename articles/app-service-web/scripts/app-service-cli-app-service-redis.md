---
title: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung einer Web-App mit einem Redis-Cache | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung einer Web-App mit einem Redis-Cache"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bc8345b2-8487-40c6-a91f-77414e8688e6
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 03/20/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 043b1e90085a63c852e428ec25fa96e37eccb203
ms.lasthandoff: 03/21/2017

---

# <a name="connect-a-web-app-to-a-redis-cache"></a>Herstellen einer Verbindung einer Web-App mit einem Redis-Cache

In diesem Szenario erfahren Sie, wie Sie einen Redis-Cache und eine Azure-Web-App erstellen. Anschließend verknüpfen Sie den Redis-Cache mithilfe von App-Einstellungen mit der Web-App.

Installieren Sie bei Bedarf die Azure-Befehlszeilenschnittstelle anhand der Anleitung im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli), und führen Sie dann `az login` aus, um eine Verbindung mit Azure herzustellen.

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).

## <a name="sample-script"></a>Beispielskript

[!code-azurecli[main](../../../cli_scripts/app-service/connect-to-redis/connect-to-redis.sh "Azure Redis Cache")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App, einen Redis-Cache und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Erstellt eine Azure-Web-App im App Service-Plan. |
| [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) | Erstellt eine neue Redis-Cache-Instanz. Hier werden die Daten gespeichert. |
| [az redis list-keys](https://docs.microsoft.com/en-us/cli/azure/redis#list-keys) | Listet die Zugriffsschlüssel für Redis-Cache-Instanz auf. |
| [az appservice web config appsetings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) | Erstellt oder aktualisiert eine App-Einstellung für eine Azure-Web-App. App-Einstellungen werden als Umgebungsvariablen für Ihre App verfügbar gemacht. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).
