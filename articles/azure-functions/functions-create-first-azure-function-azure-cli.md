---
title: Erstellen Ihrer ersten Funktion in Azure-CLI | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie Ihre erste Azure-Funktion für die serverlose Ausführung mit der Azure-CLI erstellen."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 08/22/2017
ms.topic: hero-article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 8bd3e4bb7423db44c48b04f25edcf1074e6ea0bd
ms.contentlocale: de-de
ms.lasthandoff: 08/23/2017

---

# <a name="create-your-first-function-using-the-azure-cli"></a>Erstellen Sie Ihre erste Funktion mit der Azure-CLI

In diesem Schnellstart-Tutorial wird beschrieben, wie Sie Azure Functions verwenden, um Ihre erste Funktion zu erstellen. Sie verwenden die Azure-CLI zum Erstellen einer Funktionen-App, d.h. die serverlose-Infrastruktur, die Ihre Funktion hostet. Der Funktionscode wird über ein GitHub-Beispielrepository bereitgestellt.    

Die folgenden Schritte können auf einem Mac oder auf einem Computer unter Windows oder Linux ausgeführt werden. 

## <a name="prerequisites"></a>Voraussetzungen 

Vor dem Ausführen dieses Beispiels benötigen Sie Folgendes:

+ Ein aktives [GitHub](https://github.com)-Konto. 
+ Ein aktives Azure-Abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie Funktionen-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` erstellt.  
Falls Sie nicht Cloud Shell verwenden, müssen Sie sich erst mithilfe von `az login` anmelden.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```


## <a name="create-an-azure-storage-account"></a>Erstellen eines Azure-Speicherkontos

Functions verwenden ein Azure Storage-Konto, um den Status und andere Informationen über Ihre Funktionen zu verwalten. Erstellen Sie ein Speicherkonto in der Ressourcengruppe, die Sie erstellt haben, indem Sie den Befehl [az storage account create](/cli/azure/storage/account#create) verwenden.

Ersetzen Sie im folgenden Befehl den Platzhalter `<storage_name>` durch Ihren eigenen eindeutigen Speicherkontonamen. Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Nach dem Erstellen des Speicherkontos zeigt die Azure-CLI ähnliche Informationen wie im folgenden Beispiel an:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktionen-App verfügen, die die Ausführung Ihrer Funktionen in Azure hostet. Die Funktionen-App bietet eine Umgebung für die serverlose Ausführung des Funktionscodes. Mit ihr können Sie Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. Erstellen Sie eine Funktionen-App mithilfe des Befehls [az functionapp create](/cli/azure/functionapp#create). 

Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch Ihren eigenen eindeutigen Namen der Funktionen-App und den Platzhalter `<storage_name>` durch den Speicherkontonamen. Da `<app_name>` als DNS-Standarddomäne für die Funktionen-App verwendet wird, muss der Name für alle Apps in Azure eindeutig sein. 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope
```
Standardmäßig wird eine Funktionen-App mit dem Hostingplan „Verbrauch“ erstellt, was bedeutet, dass Ressourcen nach Bedarf dynamisch von den Funktionen hinzugefügt werden, und Sie nur dann bezahlen, wenn Funktionen ausgeführt werden. Weitere Informationen finden Sie unter [Auswählen des richtigen Serviceplans](functions-scale.md). 

Nach Erstellung der Funktionen-App zeigt die Azure-CLI Informationen wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

Nun, da Sie eine Funktionen-App haben, können Sie den eigentlichen Funktionscode aus dem GitHub-Beispielrepository bereitstellen.

## <a name="deploy-your-function-code"></a>Bereitstellen Ihres Funktionscodes  

Es gibt mehrere Möglichkeiten, den Funktionscode in der neuen Funktionen-App zu erstellen. In diesem Thema wird eine Verbindung mit einem Beispielrepository in GitHub hergestellt. Ersetzen Sie wie zuvor im folgenden Code den Platzhalter `<app_name>` durch den Namen der Funktionen-App, die Sie erstellt haben. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --branch master \
--repo-url https://github.com/Azure-Samples/functions-quickstart \
--manual-integration 
```
Nachdem die Bereitstellungsquelle festgelegt wurde, zeigt die Azure-CLI Informationen ähnlich wie im folgenden Beispiel (zwecks besserer Lesbarkeit wurden NULL-Werte entfernt):

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "location": "West Europe",
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

## <a name="test-the-function"></a>Testen der Funktion

Verwenden Sie cURL, um die bereitgestellte Funktion auf einem Mac oder Linux-Computer oder mithilfe von Bash unter Windows zu testen. Führen Sie den folgenden cURL-Befehl aus, und ersetzen Sie dabei den Platzhalter `<app_name>` mit dem Namen der Funktionen-App. Fügen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an.

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Die Funktionsantwort wird in einem Browser angezeigt.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

Wenn cURL in der Befehlszeile nicht zur Verfügung steht, geben Sie dieselbe URL in die Adresse des Webbrowsers ein. Ersetzen Sie erneut den Platzhalter `<app_name>` durch den Namen der Funktionen-App, fügen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an, und führen Sie die Anforderung aus. 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Die Funktionsantwort wird in einem Browser angezeigt.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Andere Schnellstarts in dieser Sammlung bauen auf diesem Schnellstart auf. Wenn Sie planen, mit den nachfolgenden Schnellstarts oder Tutorials fortzufahren, sollten Sie die in diesem Schnellstart erstellten Ressourcen nicht bereinigen. Falls Sie nicht fortfahren möchten, können Sie den folgenden Befehl ausführen, um alle erstellten Ressourcen dieses Schnellstarts zu löschen:

```azurecli-interactive
az group delete --name myResourceGroup
```
Geben Sie `y` ein, wenn Sie dazu aufgefordert werden.

## <a name="next-steps"></a>Nächste Schritte

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

