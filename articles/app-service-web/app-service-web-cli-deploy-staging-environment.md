---
title: "Azure CLI-Skriptbeispiel – Bereitstellen einer Web-App in einer Stagingumgebung | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Bereitstellen einer Web-App in einer Stagingumgebung"
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
ms.openlocfilehash: 6e24be1f9525b976864d63dc433e853f399a545d
ms.lasthandoff: 02/27/2017

---

# <a name="deploy-a-web-app-to-a-staging-environment"></a>Bereitstellen einer Web-App in einer Stagingumgebung

Dieses Beispielskript führt Folgendes über Azure CLI 2.0 aus: 

* Erstellt eine Web-App im Azure App Service der Azure-Region „Europa, Westen“.
* Aktualisiert den App Service-Plan auf den Tarif „Standard“ (Mindeststufe für Bereitstellungsslots).
* Richtet einen Bereitstellungsslot namens „Staging“ ein.
* Stellt Ihren Web-App-Code aus GitHub im Stagingslot bereit.
* Öffnet den bereitgestellten Stagingslot zur Überprüfung im Browser.
* Überführt den Stagingslot in die Produktion.

## <a name="prerequisites"></a>Voraussetzungen

* Führen Sie `az login` aus, um sich bei Azure anzumelden.
* Fügen Sie Ihren Web-App-Code in ein GitHub-Repository ein, das Sie besitzen.

> [!NOTE]
> Wenn Sie ein öffentliches GitHub-Repository verwenden, das Sie nicht besitzen, stellt App Service Code aus diesem Repository bereit, kann aber nicht den SSH-Schlüssel und die Webhooks einrichten, der/die für Continuous Deployment erforderlich ist/sind.
>
>

## <a name="create-vm-sample"></a>Erstellen einer Beispiel-VM

[!code-azurecli[main](../../cli_scripts/app-service/deploy-deployment-slot/deploy-deployment-slot.sh "Bereitstellen einer Web-App in einer Stagingumgebung")]

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
| [az appservice plan update](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#update) | Aktualisiert einen App Service-Plan, um den zugehörigen Tarif zu skalieren. |
| [az appservice web deployment slot create](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/slot#create) | Erstellt einen Bereitstellungsslot. |
| [az appservice web source-control config](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config) | Ordnet eine Web-App einem Git- oder Mercurial-Repository zu. |
| [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) | Öffnet eine Web-App in einem Browser. |
| [az appservice web deployment slot swap](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/slot#swap) | Ändert einen angegebenen Bereitstellungsslot in eine Produktionsumgebung. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/en-us/cli/azure/overview).

Zusätzliche CLI-Skriptbeispiele für Azure App Service-Web-Apps finden Sie unter [Azure CLI-Beispiele]().

