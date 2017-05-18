---
title: "Azure CLI-Skriptbeispiel – Erstellen einer Web-App mit Continuous Deployment über Visual Studio Team Services | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Web-App mit Continuous Deployment über Visual Studio Team Services"
services: app-service\web
documentationcenter: 
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 389d3bd3-cd8e-4715-a3a1-031ec061d385
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 03/20/2017
ms.author: cfowler
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: e5259bf40819aa799651e09ea5de7138e2a8ac7d
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017

---
# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a>Erstellen einer Web-App mit Continuous Deployment über Visual Studio Team Services

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und richtet dann Continuous Deployment über Visual Studio Team Services ein. Für dieses Beispiel benötigen Sie Folgendes:

* Ein Visual Studio Team Services-Repository mit Anwendungscode, für das Sie über Administratorberechtigungen verfügen.
* Ein [persönliches Zugriffstoken (PAT)](https://www.visualstudio.com/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate) für Ihr Visual Studio Team Services-Konto.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="create-app-sample"></a>Erstellen eines App-Beispiels

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Erstellen einer Web-App mit Continuous Deployment über Visual Studio Team Services")]

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

