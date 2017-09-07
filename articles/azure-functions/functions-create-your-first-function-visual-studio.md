---
title: Erstellen Ihrer ersten Funktion in Azure mit Visual Studio | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie in Azure eine einfache per HTTP ausgelöste Funktion erstellen und veröffentlichen, indem Sie die Azure Functions-Tools für Visual Studio verwenden."
services: functions
documentationcenter: na
author: rachelappel
manager: cfowler
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, Compute, serverlose Architektur
ms.assetid: 82db1177-2295-4e39-bd42-763f6082e796
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 07/05/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 8291986b123ed98b9c2bd4ec915e1a18b6e0cbb7
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="create-your-first-function-using-visual-studio"></a>Erstellen Ihrer ersten Funktion mit Visual Studio

Mit Azure Functions können Sie Code in einer serverlosen Umgebung ausführen, ohne vorher eine VM erstellen oder eine Webanwendung veröffentlichen zu müssen.

In diesem Thema erfahren Sie, wie Sie die Visual Studio 2017-Tools für Azure Functions verwenden, um eine „hello world“-Funktion lokal zu erstellen und zu testen. Anschließend veröffentlichen Sie den Funktionscode in Azure. Diese Tools sind als Teil der Azure-Entwicklungsworkload in Visual Studio 2017 Version 15.3 oder höher verfügbar.

![Azure Functions-Code in einem Visual Studio-Projekt](./media/functions-create-your-first-function-visual-studio/functions-vstools-intro.png)

## <a name="prerequisites"></a>Voraussetzungen

Installieren Sie für dieses Tutorial Folgendes:

* [Visual Studio 2017, Version 15.3](https://www.visualstudio.com/vs/preview/), einschließlich der Workload **Azure-Entwicklung**

    ![Installation von Visual Studio 2017 mit der Workload „Azure-Entwicklung“](./media/functions-create-your-first-function-visual-studio/functions-vs-workloads.png)
    
    >[!NOTE]  
    Nach der Installation von oder dem Upgrade auf Visual Studio 2017 Version 15.3 müssen Sie auch die Visual Studio 2017-Tools für Azure Functions manuell aktualisieren. Sie können die Tools im Menü **Extras** aktualisieren. Navigieren Sie zu **Erweiterungen und Updates...** > **Updates** > **Visual Studio Marketplace** > **Azure Functions and Web Jobs Tools** > **Update**. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

## <a name="create-an-azure-functions-project-in-visual-studio"></a>Erstellen eines Azure Functions-Projekts in Visual Studio

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

Nachdem Sie das Projekt erstellt haben, können Sie Ihre erste Funktion erstellen.

## <a name="create-the-function"></a>Erstellen der Funktion

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Hinzufügen** > **Neues Element**. Wählen Sie **Azure-Funktion**, und klicken Sie auf **Hinzufügen**.

2. Wählen Sie **HttpTrigger**, geben Sie einen **Funktionsnamen** ein, wählen Sie unter **Zugriffsrechte** die Option **Anonym**, und klicken Sie auf **Erstellen**. Auf die erstellte Funktion wird von Clients aus per HTTP-Anforderung zugegriffen. 

    ![Erstellen einer neuen Azure-Funktion](./media/functions-create-your-first-function-visual-studio/functions-vstools-add-new-function-2.png)

    Ihrem Projekt wird eine Codedatei mit einer Klasse hinzugefügt, die den Funktionscode implementiert. Dieser Code basiert auf einer Vorlage, die einen Wert für den Namen empfängt und zurücksendet. Das Attribut **FunctionName** legt den Namen Ihrer Funktion fest. Das Attribut **HttpTrigger** gibt die Nachricht an, die die Funktion auslöst. 

    ![Funktionscodedatei](./media/functions-create-your-first-function-visual-studio/functions-code-page.png)

Nachdem Sie nun eine per HTTP ausgelöste Funktion erstellt haben, können Sie sie auf Ihrem lokalen Computer testen.

## <a name="test-the-function-locally"></a>Lokales Testen der Funktion

Mit Azure Functions Core-Tools können Sie ein Azure Functions-Projekt auf dem lokalen Entwicklungscomputer ausführen. Sie werden beim ersten Starten einer Funktion in Visual Studio zum Installieren dieser Tools aufgefordert.  

1. Drücken Sie F5, um Ihre Funktion zu testen. Akzeptieren Sie die entsprechende Aufforderung von Visual Studio zum Herunterladen und Installieren der Azure Functions Core (CLI)-Tools.  Sie müssen möglicherweise auch eine Firewallausnahme aktivieren, damit die Tools HTTP-Anforderungen verarbeiten können.

2. Kopieren Sie die URL Ihrer Funktion aus der Azure Functions-Laufzeitausgabe.  

    ![Lokale Azure-Laufzeit](./media/functions-create-your-first-function-visual-studio/functions-vstools-f5.png)

3. Fügen Sie die URL der HTTP-Anforderung in die Adresszeile des Browsers ein. Hängen Sie anschließend die Abfragezeichenfolge `?name=<yourname>` an diese URL an, und führen Sie die Anforderung aus. Hier ist die Antwort des Browsers auf die von der Funktion zurückgegebene lokale GET-Anforderung abgebildet: 

    ![localhost-Antwort der Funktion im Browser](./media/functions-create-your-first-function-visual-studio/functions-test-local-browser.png)

4. Klicken Sie zum Beenden des Debuggens in der Visual Studio-Symbolleiste auf die Schaltfläche **Beenden**.

Nachdem Sie sichergestellt haben, dass die Funktion auf Ihrem lokalen Computer richtig ausgeführt wird, können Sie das Projekt in Azure veröffentlichen.

## <a name="publish-the-project-to-azure"></a>Veröffentlichen des Projekts in Azure

Sie müssen in Ihrem Azure-Abonnement über eine Funktions-App verfügen, bevor Sie Ihr Projekt veröffentlichen können. Sie können eine Funktions-App direkt in Visual Studio erstellen.

[!INCLUDE [Publish the project to Azure](../../includes/functions-vstools-publish.md)]

## <a name="test-your-function-in-azure"></a>Testen der Funktion in Azure

1. Kopieren Sie die Basis-URL der Funktions-App von der Seite „Veröffentlichungsprofil“. Ersetzen Sie den Teil `localhost:port` der URL, die Sie beim lokalen Testen der Funktion verwendet haben, durch die neue Basis-URL. Stellen Sie wie zuvor sicher, dass Sie die Abfragezeichenfolge `?name=<yourname>` an diese URL anhängen und die Anforderung ausführen.

    Die URL, über die Ihre per HTTP ausgelöste Funktion aufgerufen wird, sieht wie folgt aus:

        http://<functionappname>.azurewebsites.net/api/<functionname>?name=<yourname> 

2. Fügen Sie diese neue URL für die HTTP-Anforderung in die Adresszeile des Browsers ein. Hier ist die Antwort des Browsers auf die von der Funktion zurückgegebene GET-Remoteanforderung abgebildet: 

    ![Funktionsantwort im Browser](./media/functions-create-your-first-function-visual-studio/functions-test-remote-browser.png)
 
## <a name="next-steps"></a>Nächste Schritte

Sie haben Visual Studio genutzt, um eine C#-Funktions-App mit einer einfachen Funktion zu erstellen, die per HTTP ausgelöst wird. 

+ Informationen zum Konfigurieren Ihres Projekts für die Unterstützung anderer Arten von Triggern und Bindungen finden Sie unter [Azure Functions-Tools für Visual Studio](functions-develop-vs.md) im Abschnitt [Konfigurieren des Projekts für die lokale Entwicklung](functions-develop-vs.md#configure-the-project-for-local-development).
+ Weitere Informationen zum lokalen Testen und Debuggen mit den Azure Functions Core-Tools finden Sie unter [Lokales Codieren und Testen von Azure-Funktionen](functions-run-local.md). 
+ Weitere Informationen zum Entwickeln von Funktionen wie .NET-Klassenbibliotheken finden Sie unter [Using .NET class libraries with Azure Functions](functions-dotnet-class-library.md) (Verwenden von .NET-Klassenbibliotheken mit Azure Functions). 


