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
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 5fce9171dc249445d78cee248fb2a7ec62a3dabb
ms.lasthandoff: 03/11/2017

---

# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a>Erstellen einer Web-App mit Continuous Deployment über Visual Studio Team Services

Dieses Beispielskript führt Folgendes über Azure CLI 2.0 aus: 

* Erstellt eine Web-App im Azure App Service der Azure-Region „Europa, Westen“. 
* Stellen Sie Ihren Web-App-Code aus Visual Studio Team Services bereit.
* Zeigt die bereitgestellte Azure-Web-App im Browser an.

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie `az login` aus, um sich bei Azure anzumelden.
* Fügen Sie Ihren Web-App-Code in ein Visual Studio Team Services-Repository ein.
* Für ein Visual Studio Team Services-Repository in Ihrem Besitz können Sie [ein Zugriffstoken erstellen](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate).

## <a name="create-app-sample"></a>Erstellen eines App-Beispiels

[!code-azurecli[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Erstellen einer Web-App mit Continuous Deployment über Visual Studio Team Services")]

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
