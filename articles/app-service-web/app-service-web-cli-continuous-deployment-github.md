---
title: "Azure CLI-Skriptbeispiel – kontinuierliche Bereitstellung einer Web-App über GitHub | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – kontinuierliche Bereitstellung einer Web-App über GitHub"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: d19b07667bc2d34f860be46c299560fe9a788d53
ms.openlocfilehash: 649521d6f3740b099ab41c8ef70f7e4430aa5de9
ms.lasthandoff: 02/27/2017

---

# <a name="continuously-deploy-web-app-from-github"></a>Kontinuierliche Bereitstellung einer Web-App über GitHub

Dieses Beispielskript führt Folgendes über Azure CLI 2.0 aus: 

* Erstellt eine Web-App im Azure App Service der Azure-Region „Europa, Westen“. 
* Stellt Ihren Web-App-Code über GitHub bereit.
* Zeigt die bereitgestellte Web-App im Browser an.

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie `az login` aus, um sich bei Azure anzumelden.
* Fügen Sie Ihren Web-App-Code in ein GitHub-Repository ein, das Sie besitzen.

> [!NOTE]
> Wenn Sie ein öffentliches GitHub-Repository verwenden, das Sie nicht besitzen, stellt App Service Code aus diesem Repository bereit, kann aber nicht den SSH-Schlüssel und die Webhooks einrichten, der/die für Continuous Deployment erforderlich ist/sind.
>
>

## <a name="create-vm-sample"></a>Erstellen einer Beispiel-VM

[!code-azurecli[main](../../cli_scripts/app-service/deploy-github/deploy-github.sh "Kontinuierliche Bereitstellung einer Web-App über GitHub")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die Web-App und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete --name $webappname
```

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/en-us/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. |
| [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#delete) | Erstellt die Web-App. |
| [az appservice web source-control config](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config) | Ordnet eine Web-App einem Git- oder Mercurial-Repository zu. |
| [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) | Öffnet eine Web-App in einem Browser. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/en-us/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele für Azure App Service-Web-Apps finden Sie unter [Azure CLI-Beispiele]().

