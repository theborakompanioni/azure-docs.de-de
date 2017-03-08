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
ms.sourcegitcommit: d54e58da5755009710bdcd11eaf845d6ce4f76d4
ms.openlocfilehash: 915227599645186ed11165affd378f2de282c7ea
ms.lasthandoff: 02/27/2017

---

# <a name="create-a-web-app-with-continuous-deployment-from-visual-studio-team-services"></a>Erstellen einer Web-App mit Continuous Deployment über Visual Studio Team Services

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und richtet dann Continuous Deployment über Visual Studio Team Services (VSTS) ein. 

Stellen Sie vor dem Ausführen dieses Skripts Folgendes sicher:

- Eine Verbindung mit Azure wurde mit dem Befehl `az login` hergestellt.
- Der Anwendungscode befindet sich in einem öffentlichen oder privaten Visual Studio Team Services-Repository, das Sie besitzen.
- Sie haben [ein Zugriffstoken in Visual Studio Team Services erstellt](https://www.visualstudio.com/en-us/docs/setup-admin/team-services/use-personal-access-tokens-to-authenticate).

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).

## <a name="create-app-sample"></a>Erstellen eines App-Beispiels

[!code-azurecli[main](../../../cli_scripts/app-service/deploy-vsts-continuous/deploy-vsts-continuous.sh?highlight=3-4 "Erstellen einer Web-App mit Continuous Deployment über Visual Studio Team Services")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup
```

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
