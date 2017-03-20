---
title: "Erstellen Ihrer ersten ASP.NET-Web-App in Azure in fünf Minuten | Microsoft-Dokumentation"
description: "Erfahren Sie, wie einfach die Ausführung von Web-Apps in App Service ist, indem Sie eine ASP.NET-Beispiel-App bereitstellen."
services: app-service\web
documentationcenter: 
author: cephalin
manager: wpickett
editor: 
ms.assetid: b1e6bd58-48d1-4007-9d6c-53fd6db061e3
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/08/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: c55f844b89729b1ef5a6316b0b2731472074219e
ms.lasthandoff: 03/15/2017


---
# <a name="create-your-first-aspnet-web-app-in-azure-in-five-minutes"></a>Erstellen Ihrer ersten ASP.NET-Web-App in Azure in fünf Minuten

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Dieser Schnellstart hilft Ihnen bei der Bereitstellung Ihrer ersten ASP.NET-Web-App in [Azure App Service](../app-service/app-service-value-prop-what-is.md) in nur wenigen Minuten.

Stellen Sie vor der Durchführung dieses Schnellstarts sicher, dass auf Ihrem Computer [die Azure CLI installiert ist](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

## <a name="create-an-aspnet-web-app-in-azure"></a>Erstellen von ASP.NET-Web-Apps in Azure
2. Melden Sie sich bei Azure an, indem Sie `az login` ausführen und den Anweisungen auf dem Bildschirm folgen.
   
    ```azurecli
    az login
    ```
   
3. Erstellen Sie eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md). Dieser fügen Sie alle Azure-Ressourcen hinzu, die Sie zusammen verwalten möchten, z.B. die Web-App und das zugehörige SQL-Datenbank-Back-End.

    ```azurecli
    az group create --location "West Europe" --name myResourceGroup
    ```

    Welche Werte Sie für `---location` verwenden können, erfahren Sie mithilfe des Azure CLI-Befehls `az appservice list-locations`.

3. Erstellen Sie einen [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) vom Typ „FREE“. 

    ```azurecli
    az appservice plan create --name my-free-appservice-plan --resource-group myResourceGroup --sku FREE
    ```

4. Erstellen Sie in `<app_name>` eine neue Web-App mit einem eindeutigen Namen.

    ```azurecli
    az appservice web create --name <app_name> --resource-group myResourceGroup --plan my-free-appservice-plan
    ```

4. Stellen Sie eine ASP.NET-Beispiel-App in GitHub bereit.

    ```azurecli
    az appservice web source-control config --name <app_name> --resource-group myResourceGroup \
    --repo-url "https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git" --branch master --manual-integration 
    ```

5. Führen Sie diesen Befehl aus, um die Liveausführung der App in Azure zu verfolgen.

    ```azurecli
    az appservice web browse --name <app_name> --resource-group myResourceGroup
    ```

Herzlichen Glückwunsch, Ihre erste ASP.NET-Web-App wird live in Azure App Service ausgeführt.

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Nächste Schritte

Erkunden Sie vorab erstellte [CLI-Skripts für Web-Apps](app-service-cli-samples.md).

