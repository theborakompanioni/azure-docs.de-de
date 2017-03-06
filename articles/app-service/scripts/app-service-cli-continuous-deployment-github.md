---
title: "Azure CLI-Skriptbeispiel – Erstellen einer Web-App mit Continuous Deployment über GitHub | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer Web-App mit Continuous Deployment über GitHub"
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
ms.sourcegitcommit: 97ab792a0ad7420e97dbab378736b9815356e8a4
ms.openlocfilehash: 220c150205858dd9ae200d7636a086972aeed6e9
ms.lasthandoff: 02/27/2017

---

# <a name="create-a-web-app-with-continuous-deployment-from-github"></a>Erstellen einer Web-App mit Continuous Deployment über GitHub

Dieses Beispielskript erstellt eine Web-App in App Service mit den zugehörigen Ressourcen und richtet dann Continuous Deployment über ein GitHub-Repository ein. Informationen zur GitHub-Bereitstellung ohne Continuous Deployment finden Sie unter [Erstellen einer Web-App und Bereitstellen von Code über GitHub](app-service-cli-deploy-github.md).

Stellen Sie vor dem Ausführen dieses Skripts Folgendes sicher:

- Eine Verbindung mit Azure wurde mit dem Befehl `az login` hergestellt.
- Der Anwendungscode befindet sich in einem öffentlichen oder privaten GitHub-Repository, das Sie besitzen.
- Sie haben [ein Zugriffstoken in Ihrem GitHub-Konto erstellt](https://help.github.com/articles/creating-an-access-token-for-command-line-use/).

Dieses Beispiel wird in einer Bash-Shell ausgeführt. Optionen zum Ausführen von Azure CLI-Skripts auf einem Windows-Client finden Sie unter [Verwenden der Azure CLI unter Windows](../../virtual-machines/virtual-machines-windows-cli-options.md).

## <a name="create-app-sample"></a>Erstellen eines App-Beispiels

[!code-azurecli[main](../../../cli_scripts/app-service/deploy-github-continuous/deploy-github-continuous.sh?highlight=3-4 "Erstellen einer Web-App mit Continuous Deployment über GitHub")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die App Service-App und alle zugehörigen Ressourcen entfernt werden.

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
