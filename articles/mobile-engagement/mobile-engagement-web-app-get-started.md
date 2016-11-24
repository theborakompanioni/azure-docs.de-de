---
title: "Erste Schritte mit Azure Mobile Engagement für Web-Apps | Microsoft Docs"
description: "Erfahren Sie mehr über die Verwendung von Azure Mobile Engagement mit Analysen und Pushbenachrichtigungen für Web-Apps."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04afe53a-4caf-4c80-bd75-20cc630cd75c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: hero-article
ms.date: 06/01/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 555342e88c912a3f43c578a40dc34933996ade4c
ms.openlocfilehash: ef75623a0b8c2a4d38f527a26cbc2cf070765302


---
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Erste Schritte mit Azure Mobile Engagement für Web-Apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Sie Azure Mobile Engagement verwenden, um die Web-App-Nutzung zu verstehen.

Für dieses Lernprogramm ist Folgendes erforderlich:

* Visual Studio 2015 oder ein anderer Editor Ihrer Wahl
* [Web-SDK](http://aka.ms/P7b453) 

Dieses Web-SDK befindet sich in der Vorschauphase und unterstützt derzeit nur Analytics und noch kein Senden von Browser- oder In-App-Pushbenachrichtigungen. 

> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a>Einrichten von Mobile Engagement für Ihre Web-App
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="a-idconnecting-appaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
In diesem Lernprogramm wird eine „einfache Integration“ dargestellt. Dabei handelt es sich um den minimalen erforderlichen Satz zur Sammlung von Daten.

Wir erstellen eine einfache Web-App mit Visual Studio, um die Integration zu veranschaulichen. Sie können die Schritte aber auch mit einer beliebigen Webanwendung ausführen, die außerhalb von Visual Studio erstellt wurde. 

### <a name="create-a-new-web-app"></a>Erstellen einer neuen Web-App
In den folgenden Schritten wird die Verwendung von Visual Studio 2015 angenommen, obwohl die Schritte in früheren Versionen von Visual Studio ähnlich sind. 

1. Starten Sie Visual Studio, und wählen Sie auf dem **Startbildschirm** die Option **Neues Projekt**.
2. Wählen Sie im Popupmenü die Option **Web** -> **ASP.NET-Webanwendung**. Geben Sie **Name**, **Speicherort** und **Lösungsname** der App ein, und klicken Sie dann auf **OK**.
3. Wählen Sie im Popupmenü die Option **Vorlage auswählen** und unter **ASP.Net 4.5-Vorlagen** die Option **Leer**, und klicken Sie auf **OK**. 

Sie haben nun ein neues leeres Web-App-Projekt erstellt, in die das Azure Mobile Engagement-Web-SDK integriert wird.

### <a name="connect-your-app-to-mobile-engagement-backend"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End.
1. Erstellen Sie in Ihrer Lösung einen neuen Ordner mit dem Namen **javascript**, und fügen Sie dem Ordner die Web-SDK-JS-Datei **azure-engagement.js** hinzu. 
2. Fügen Sie im Ordner „javascript“ eine neue Datei mit dem Namen **main.js** und dem folgenden Code hinzu. Denken Sie daran, die Verbindungszeichenfolge zu aktualisieren. Dieses `azureEngagement` -Objekt wird zum Zugreifen auf die Web-SDK-Methoden verwendet. 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio mit JS-Dateien][1]

## <a name="enable-real-time-monitoring"></a>Aktivieren der Überwachung in Echtzeit
Um mit dem Senden von Daten zu beginnen und sicherzustellen, dass die Benutzer aktiv sind, müssen Sie mindestens eine Aktivität an das Mobile Engagement-Back-End senden. Im Web-App-Kontext ist eine Aktivität eine Webseite. 

1. Erstellen Sie in Ihrer Projektmappe eine neue Seite mit dem Namen **home.html** , und legen Sie sie als Startseite für die Web-App fest. 
2. Binden Sie die beiden JavaScript-Elemente ein, die wir weiter oben auf dieser Seite hinzugefügt haben, indem Sie im body-Tag Folgendes hinzufügen. 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. Aktualisieren des body-Tags zum Aufrufen der `startActivity` -Methode von EngagementAgent
   
        <body onload="engagement.agent.startActivity('Home')">
4. Die Datei **home.html** sieht wie folgt aus:
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a>Verbinden der App mit Überwachung in Echtzeit
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a>Erweitern von Analysen
Hier sind alle Methoden aufgeführt, die unter dem Web-SDK derzeit verfügbar sind und die Sie für Analysen verwenden können:

1. Aktivitäten/Webseiten:
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. Ereignisse
   
        engagement.agent.sendEvent(name, extras);
3. Fehler
   
        engagement.agent.sendError(name, extras);
4. Aufträge
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png




<!--HONumber=Nov16_HO2-->


