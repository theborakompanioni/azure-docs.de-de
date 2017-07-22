---
title: Erstellen Ihrer ersten Funktion im Azure-Portal | Microsoft Docs
description: "Erfahren Sie, wie Sie Ihre erste Azure-Funktion für die serverlose Ausführung mit dem Azure-Portal erstellen."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 96cf87b9-8db6-41a8-863a-abb828e3d06d
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/19/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: ed096cf202a85f44755c9eee110b1b7c5c4074ad
ms.contentlocale: de-de
ms.lasthandoff: 07/20/2017

---
# <a name="create-your-first-function-in-the-azure-portal"></a>Erstellen Ihrer ersten Funktion im Azure-Portal

Mit Azure Functions können Sie Code in einer serverlosen Umgebung ausführen, ohne vorher eine VM erstellen oder eine Webanwendung veröffentlichen zu müssen. In diesem Thema erfahren Sie, wie Sie Azure Functions verwenden können, um eine „Hello World“-Funktion im Azure-Portal zu erstellen.

![Erstellen einer Funktionen-App im Azure-Portal](./media/functions-create-first-azure-function/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="log-in-to-azure"></a>Anmelden an Azure

Melden Sie sich beim [Azure-Portal](https://portal.azure.com/)an.

## <a name="create-a-function-app"></a>Erstellen einer Funktionen-App

Sie müssen über eine Funktionen-App verfügen, die die Ausführung Ihrer Funktionen in Azure hostet. Sie können mit einer Funktionen-App Funktionen zu logischen Einheiten gruppieren. Dies erleichtert die Verwaltung, Bereitstellung und Freigabe von Ressourcen. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Die Funktionen-App wurde erfolgreich erstellt.](./media/functions-create-first-azure-function/function-app-create-success.png)

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

Erstellen Sie als Nächstes in der neuen Funktionen-App eine Funktion.

## <a name="create-function"></a>Erstellen einer Funktion mit Auslösung per HTTP

1. Erweitern Sie die neue Funktionen-App, und klicken Sie anschließend auf die Schaltfläche **+** neben **Functions**.

2.  Wählen Sie auf der Seite **Get started quickly** (Schnelleinstieg) **WebHook + API**, wählen Sie eine Sprache für die Funktion aus, und klicken Sie dann auf **Diese Funktion erstellen**. 
   
    ![Schnellstart von Functions im Azure-Portal](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

Eine Funktion wird in Ihrer gewählten Sprache anhand der Vorlage für eine Funktion mit Auslösung per HTTP erstellt. Sie können die neue Funktion ausführen, indem Sie eine HTTP-Anforderung senden.

## <a name="test-the-function"></a>Testen der Funktion

1. Klicken Sie in der neuen Funktion auf **</> Get function URL**, wählen Sie **default (Function key)** aus, und klicken Sie dann auf **Kopieren**. 

    ![Kopieren der URL der Funktion aus dem Azure-Portal](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)

2. Fügen Sie die URL der Funktion in die Adressleiste Ihres Browsers ein. Fügen Sie die Abfragezeichenfolge `&name=<yourname>` an die URL an, und betätigen Sie die Taste `Enter` auf Ihrer Tastatur, um die Anforderung auszuführen. Im Folgenden finden Sie ein Beispiel der Antwort, die im Edge-Browser von der Funktion zurückgegeben wird:

    ![Funktionsantwort im Browser.](./media/functions-create-first-azure-function/function-app-browser-testing.png)

    Die Anfrage-URL enthält einen Schlüssel, der standardmäßig über HTTP auf Ihre Funktion zugreifen muss.   

3. Wenn Ihre Funktion ausgeführt wird, werden Nachverfolgungsinformationen in die Protokolle geschrieben. Kehren Sie zu Ihrer Funktion im Portal zurück, und klicken Sie auf den Pfeil nach oben am unteren Bildschirmrand, um **Protokolle** zu erweitern. Dann wird Ihnen die Nachverfolgungsausgabe der vorherigen Ausführung angezeigt. 

   ![Viewer der Funktionsprotokolle im Azure-Portal](./media/functions-create-first-azure-function/function-view-logs.png)

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

[!INCLUDE [Clean up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Funktionen-App mit einer einfachen Funktion mit Auslösung per HTTP erstellt.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Weitere Informationen finden Sie unter [HTTP- und Webhookbindungen in Azure Functions](functions-bindings-http-webhook.md).




