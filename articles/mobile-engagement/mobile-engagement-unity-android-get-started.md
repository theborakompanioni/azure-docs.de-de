---
title: Erste Schritte mit Azure Mobile Engagement für eine Unity Android-Bereitstellung
description: Erfahren Sie mehr über die Verwendung von Azure Mobile Engagement mit Analysefunktionen und Pushbenachrichtigungen für Unity-Apps, die auf iOS-Geräten bereitgestellt werden.
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: ''

ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo

---
# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Erste Schritte mit Azure Mobile Engagement für eine Unity Android-Bereitstellung
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema erfahren Sie, wie Sie mithilfe von Azure Mobile Engagement die Nutzung Ihrer App nachvollziehen und Pushbenachrichtigungen an segmentierte Benutzer einer Unity-Anwendung senden, wenn Sie die Bereitstellung für ein Android-Gerät durchführen.
In diesem Tutorial wird das klassische Roll-A-Ball-Tutorial von Unity als Ausgangspunkt verwendet. Sie sollten die Schritte in diesem [Tutorial](mobile-engagement-unity-roll-a-ball.md) ausführen, bevor Sie mit der Mobile Engagement-Integration beginnen, die in diesem Tutorial beschrieben wird. 

Für dieses Lernprogramm ist Folgendes erforderlich:

* [Unity-Editor](http://unity3d.com/get-unity)
* [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
* Google Android SDK

> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).
> 
> 

## <a name="<a-id="setup-azme"></a>setup-mobile-engagement-for-your-android-app"></a><a id="setup-azme"></a>Einrichten von Mobile Engagement für Ihre App
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="<a-id="connecting-app"></a>connect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
### <a name="import-the-unity-package"></a>Importieren des Unity-Pakets
1. Laden Sie das [Mobile Engagement Unity-Paket](https://aka.ms/azmeunitysdk) herunter, und speichern Sie es auf Ihrem lokalen Computer. 
2. Wechseln Sie zu **Assets > Paket importieren > Benutzerdefiniertes Paket**, und wählen Sie das im vorherigen Schritt heruntergeladene Paket aus. 
   
    ![][70] 
3. Stellen Sie sicher, dass alle Dateien ausgewählt sind, und klicken Sie auf die Schaltfläche **Importieren** . 
   
    ![][71] 
4. Nach dem Importieren werden die importierten SDK-Dateien in Ihrem Projekt angezeigt.  
   
    ![][72] 

### <a name="update-the-engagementconfiguration"></a>Aktualisieren von „EngagementConfiguration“
1. Öffnen Sie die **EngagementConfiguration**-Skriptdatei im SDK-Ordner, und aktualisieren **ANDROID\_CONNECTION\_STRING** gemäß der Verbindungszeichenfolge, die Sie zuvor im Azure-Portal abgerufen haben.  
   
    ![][73]
2. Speichern Sie die Datei. 
3. Führen Sie **Datei > Engagement > Android-Manifest generieren** aus. Dies ist das Plug-In, das vom Mobile Engagement SDK hinzugefügt wird. Wenn Sie darauf klicken, werden die Projekteinstellungen automatisch aktualisiert. 
   
    ![][74]

> [!IMPORTANT]
> Führen Sie diesen Schritt bei jeder Aktualisierung der **EngagementConfiguration** -Datei aus, da Ihre Änderungen sonst nicht in der App widergespiegelt werden. 
> 
> 

### <a name="configure-the-app-for-basic-tracking"></a>Konfigurieren der App für die einfache Nachverfolgung
1. Öffnen Sie das an das Player-Objekt angefügte **PlayerController** -Skript für die Bearbeitung. 
2. Fügen Sie die folgende using-Anweisung hinzu:
   
        using Microsoft.Azure.Engagement.Unity;
3. Fügen Sie der `Start()`-Methode Folgendes hinzu:
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-the-app"></a>Bereitstellen und Ausführen der App
Stellen Sie sicher, dass das Android-SDK auf Ihrem Computer installiert ist, bevor Sie versuchen, diese Unity-App auf Ihrem Gerät bereitzustellen. 

1. Verbinden Sie ein Android-Gerät mit dem Computer. 
2. Öffnen Sie **Datei > Buildeinstellungen**. 
   
    ![][40]
3. Wählen Sie **Android**, und klicken Sie anschließend auf **Plattform wechseln**.
   
    ![][51]
   
    ![][52]
4. Klicken Sie auf **Player-Einstellungen** , und geben Sie einen gültigen Bundle Identifier an. 
   
    ![][53]
5. Klicken Sie abschließend auf **Erstellen und Ausführen**
   
    ![][54]
6. Unter Umständen werden Sie aufgefordert, den Namen eines Ordners zum Speichern des Android-Pakets anzugeben. 
7. Wenn keine Fehler auftreten, wird das Paket auf dem verbundenen Gerät bereitgestellt, und das Unity-Spiel wird auf Ihrem Smartphone angezeigt. 

## <a name="<a-id="monitor"></a>connect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden der App mit Überwachung in Echtzeit
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="<a-id="integrate-push"></a>enable-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und In-App-Messaging
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### <a name="update-the-engagementconfiguration"></a>Aktualisieren von „EngagementConfiguration“
1. Öffnen Sie die **EngagementConfiguration**-Skriptdatei im SDK-Ordner, und aktualisieren Sie **ANDROID\_GOOGLE\_NUMBER** mit der **Google-Projektnummer**, die Sie zuvor über das Google Cloud Developer-Portal abgerufen haben. Da dies ein Zeichenfolgenwert ist, muss er in doppelte Anführungszeichen gesetzt werden. 
   
    ![][75]
2. Speichern Sie die Datei. 
3. Führen Sie **Datei > Engagement > Android-Manifest generieren** aus. Dies ist das Plug-In, das vom Mobile Engagement SDK hinzugefügt wird. Wenn Sie darauf klicken, werden die Projekteinstellungen automatisch aktualisiert. 
   
    ![][74]

### <a name="configure-the-app-to-receive-notifications"></a>Konfigurieren der App für den Empfang von Benachrichtigungen
1. Öffnen Sie das an das Player-Objekt angefügte **PlayerController** -Skript für die Bearbeitung. 
2. Fügen Sie der `Start()` -Methode Folgendes hinzu:
   
        EngagementReachAgent.Initialize();
3. Nachdem die App aktualisiert wurde, können Sie sie gemäß der unten angegebenen Anleitung auf einem Gerät ausführen. 

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png



<!--HONumber=Oct16_HO2-->


