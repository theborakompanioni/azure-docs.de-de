---
title: "Azure CLI-Skriptbeispiel – Bereitstellen einer Web-App über ein lokales Git-Repository | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Bereitstellen einer Web-App über ein lokales Git-Repository"
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
ms.sourcegitcommit: 343f7509e43c2305e6fa7d5e5cb9e6772e1a7e01
ms.openlocfilehash: 7c970e6d4447fed96d91143a502ec823eb60df62
ms.lasthandoff: 02/27/2017

---

# <a name="deploy-a-web-app-from-a-local-git-repository"></a>Bereitstellen einer Web-App über ein lokales Git-Repository

Dieses Beispielskript führt Folgendes über Azure CLI 2.0 aus: 

* Erstellt eine Web-App im Azure App Service der Azure-Region „Europa, Westen“.
* Stellt Web-App-Code über ein lokales Git-Repository bereit.
* Zeigt die bereitgestellte Web-App im Browser an.

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie `az login` aus, um sich bei Azure anzumelden.
* Übernehmen Sie Ihren Web-App-Code in ein lokales Git-Repository.

## <a name="create-vm-sample"></a>Erstellen einer Beispiel-VM

[!code-azurecli[main](../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh "Bereitstellen einer Web-App über ein lokales Git-Repository")]

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
| [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#delete) | Erstellt eine Web-App. |
| [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) | Erstellt eine Konfiguration der Quellcodeverwaltung für ein lokales Git-Repository. |
| [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) | Öffnet eine Web-App in einem Browser. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/en-us/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele für Azure App Service-Web-Apps finden Sie unter [Azure CLI-Beispiele]().
