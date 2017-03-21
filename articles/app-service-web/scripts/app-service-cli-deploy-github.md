---
title: "Azure CLI-Skriptbeispiel – Erstellen einer Web-App mit Bereitstellung über GitHub | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Web-App mit Bereitstellung über GitHub"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 0205c991-0989-4ca3-bb41-237dcc964460
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: ab4be9d9a03001789bed90e58d62fe1891608823
ms.lasthandoff: 03/11/2017

---


# <a name="create-a-web-app-with-deployment-from-github"></a>Erstellen einer Web-App mit Bereitstellung über GitHub

Dieses Beispielskript führt Folgendes über Azure CLI 2.0 aus:

* Erstellt eine Web-App im Azure App Service der Azure-Region „Europa, Westen“.
* Stellt Ihren Web-App-Code über GitHub bereit.
* Zeigt die bereitgestellte Azure-Web-App im Browser an.

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie `az login` aus, um sich bei Azure anzumelden.
* Fügen Sie Ihren Web-App-Code in ein GitHub-Repository ein.

> [!NOTE]
> Wenn Sie ein öffentliches GitHub-Repository verwenden, das Sie nicht besitzen, stellt App Service Code aus diesem Repository bereit, kann aber nicht den SSH-Schlüssel und die Webhooks einrichten, der/die für Continuous Deployment erforderlich ist/sind.
>
>

## <a name="create-app-sample"></a>Erstellen eines App-Beispiels

[!code-azurecli[main](../../../cli_scripts/app-service/deploy-github/deploy-github.sh?highlight=3 "Erstellen einer Web-App mit Bereitstellung über GitHub")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#delete) | Erstellt eine Azure-Web-App. |
| [az appservice web source-control config](https://docs.microsoft.com/cli/azure/appservice/web/source-control#config) | Ordnet eine Azure-Web-App einem Git- oder Mercurial-Repository zu. |
| [az appservice web browse](https://docs.microsoft.com/cli/azure/appservice/web#browse) | Öffnen einer Azure-Web-App in einem Browser. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).
