---
title: "Azure CLI-Skriptbeispiel – Erstellen einer ASP.NET Core-Web-App in einem Docker-Container über die Azure-Containerregistrierung | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Erstellen einer ASP.NET Core-Web-App in einem Docker-Container über die Azure-Containerregistrierung"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: 3a2d1983-ff7b-476a-ac44-49ec2aabb31a
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 02/23/2017
ms.author: cfowler
translationtype: Human Translation
ms.sourcegitcommit: 862aad510a9d6e8ae15324457d8b15378b2b2776
ms.openlocfilehash: 61d0f71d13429cffa3b7e01cf2fb18eb358a1817
ms.lasthandoff: 02/27/2017

---

# <a name="create-an-aspnet-core-web-app-in-a-docker-container-from-azure-container-registry"></a>Erstellen einer ASP.NET Core-Web-App in einem Docker-Container über die Azure-Containerregistrierung

In diesem Szenario erfahren Sie, wie Sie eine Ressourcengruppe, einen Linux-App-Serviceplan und eine Web-App erstellen und mithilfe eines Docker-Containers eine ASP.NET Core-Anwendung über die Azure-Containerregistrierung bereitstellen.

Stellen Sie vor dem Ausführen dieses Skripts sicher, dass über den Befehl `az login` eine Verbindung mit Azure hergestellt wurde.

## <a name="create-app-sample"></a>Erstellen einer Beispiel-App

[!code-azurecli[main](../../../cli_scripts/app-service/deploy-linux-acr/deploy-linux-acr.sh?highlight=6-9 "Linux-Azure-Containerregistrierung")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung 

Nach Ausführung des Skriptbeispiels können mit dem folgenden Befehl die Ressourcengruppe, die VM und alle zugehörigen Ressourcen entfernt werden.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az appservice web create](https://docs.microsoft.com/cli/azure/appservice/web#create) | Erstellt eine Azure-Web-App im App Service-Plan. |
| [az appservice web config container update](https://docs.microsoft.com/cli/azure/appservice/web/config/container#update) | Legt den Docker-Container für die Azure-Web-App fest. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).
