---
title: "Erste Schritte mit Azure Mobile Engagement für die Unity iOS-Bereitstellung"
description: "Erfahren Sie mehr über die Verwendung von Azure Mobile Engagement mit Analysefunktionen und Pushbenachrichtigungen für Unity-Apps, die auf iOS-Geräten bereitgestellt werden."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7ddfbac3-8d13-4ebe-b061-c865f357297f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c8f50404771965ec636065346ac04e059d264c3d


---
# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Erste Schritte mit Azure Mobile Engagement für die Unity iOS-Bereitstellung
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema erfahren Sie, wie Sie mithilfe von Azure Mobile Engagement die Nutzung Ihrer App nachvollziehen und bei der Bereitstellung auf einem iOS-Gerät Pushbenachrichtigungen an segmentierte Benutzer einer Unity-Anwendung senden.
In diesem Tutorial wird das klassische Roll-A-Ball-Tutorial von Unity als Ausgangspunkt verwendet. Sie sollten die Schritte in diesem [Tutorial](mobile-engagement-unity-roll-a-ball.md) ausführen, bevor Sie mit der Mobile Engagement-Integration beginnen, die in diesem Tutorial beschrieben wird. 

Für dieses Lernprogramm ist Folgendes erforderlich:

* [Unity-Editor](http://unity3d.com/get-unity)
* [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
* XCode-Editor

> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).
> 
> 

## <a name="a-idsetupazmeasetup-mobile-engagement-for-your-ios-app"></a><a id="setup-azme"></a>Einrichten von Mobile Engagement für Ihre iOS-App
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="a-idconnectingappaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
### <a name="import-the-unity-package"></a>Importieren des Unity-Pakets
1. Laden Sie das [Mobile Engagement Unity-Paket](https://aka.ms/azmeunitysdk) herunter, und speichern Sie es auf Ihrem lokalen Computer. 
2. Wechseln Sie zu **Assets > Paket importieren > Benutzerdefiniertes Paket**, und wählen Sie das im vorherigen Schritt heruntergeladene Paket aus. 
   
    ![][70] 
3. Stellen Sie sicher, dass alle Dateien ausgewählt sind, und klicken Sie auf die Schaltfläche **Importieren** . 
   
    ![][71] 
4. Nach dem Importieren werden die importierten SDK-Dateien in Ihrem Projekt angezeigt.  
   
    ![][72] 

### <a name="update-the-engagementconfiguration"></a>Aktualisieren von „EngagementConfiguration“
1. Öffnen Sie die **EngagementConfiguration**-Skriptdatei im SDK-Ordner, und aktualisieren Sie **IOS\_CONNECTION\_STRING** gemäß der Verbindungszeichenfolge, die Sie zuvor im Azure-Portal abgerufen haben.  
   
    ![][73]
2. Speichern Sie die Datei. 

### <a name="configure-the-app-for-basic-tracking"></a>Konfigurieren der App für die einfache Nachverfolgung
1. Öffnen Sie das an das Player-Objekt angefügte **PlayerController** -Skript für die Bearbeitung. 
2. Fügen Sie die folgende using-Anweisung hinzu:
   
        using Microsoft.Azure.Engagement.Unity;
3. Fügen Sie der `Start()`-Methode Folgendes hinzu:
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-the-app"></a>Bereitstellen und Ausführen der App
1. Verbinden Sie ein iOS-Gerät mit dem Computer. 
2. Öffnen Sie **Datei > Buildeinstellungen**. 
   
    ![][40]
3. Wählen Sie **iOS**, und klicken Sie anschließend auf **Plattform wechseln**.
   
    ![][41]
   
    ![][42]
4. Klicken Sie auf **Player-Einstellungen** , und geben Sie einen gültigen Bundle Identifier an. 
   
    ![][53]
5. Klicken Sie abschließend auf **Erstellen und Ausführen**
   
    ![][54]
6. Unter Umständen werden Sie aufgefordert, den Namen eines Ordners zum Speichern des iOS-Pakets anzugeben. 
   
    ![][43]
7. Wenn keine Fehler auftreten, wird das Projekt kompiliert, und Sie können es in Ihrer XCode-Anwendung öffnen. 
8. Stellen Sie sicher, dass die **Paket-ID** im Projekt korrekt angegeben wurde.  
   
    ![][75]
9. Führen Sie die App nun in XCode aus, sodass das Paket auf dem verbundenen Gerät bereitgestellt wird. Das Unity-Spiel wird auf Ihrem Smartphone angezeigt. 

## <a name="a-idmonitoraconnect-app-with-realtime-monitoring"></a><a id="monitor"></a>Verbinden der App mit Überwachung in Echtzeit
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="a-idintegratepushaenable-push-notifications-and-inapp-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und In-App-Messaging
Mit Mobile Engagement können Sie mit Ihren Benutzern und mit REACH mit Push-Benachrichtigungen und In-App-Nachrichten im Rahmen von Kampagnen interagieren. Dieses Modul nennt sich REACH im Mobile Engagement-Portal.
Sie müssen keine zusätzlichen Konfigurationsschritte in Ihrer App vornehmen, um Benachrichtigungen zu erhalten, dies ist bereits eingerichtet.

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png



<!--HONumber=Nov16_HO2-->


