---
title: Erste Schritte mit Azure Mobile Engagement für Xamarin.Android
description: Erfahren Sie mehr über die Verwendung von Azure Mobile Engagement mit Analysefunktionen und Pushbenachrichtigungen für Xamarin.Android-Apps.
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: ''

ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/16/2016
ms.author: piyushjo

---
# <a name="get-started-with-azure-mobile-engagement-for-xamarin.android-apps"></a>Erste Schritte mit Azure Mobile Engagement für Xamarin.Android-Apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema erfahren Sie, wie Sie mithilfe von Azure Mobile Engagement die Nutzung Ihrer App verstehen können und wie Sie Pushbenachrichtigungen an segmentierte Benutzer einer Xamarin.Android-Anwendung senden.
In diesem Tutorial wird ein einfaches Übertragungsszenario mit Mobile Engagement dargestellt. Darin erstellen Sie eine leere Xamarin.Android-App, die einfache Daten erfasst und Pushbenachrichtigungen über Google Cloud Messaging (GCM) empfängt.

Für dieses Lernprogramm ist Folgendes erforderlich:

* [Xamarin Studio](http://xamarin.com/studio). Sie können auch Visual Studio mit Xamarin verwenden, in den Anweisungen in diesem Tutorial wird jedoch Xamarin Studio eingesetzt. Installationsanweisungen finden Sie unter [Setup und Installation für Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> Sie benötigen ein aktives Azure-Konto, um dieses Lernprogramm abzuschließen. Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).
> 
> 

## <a name="<a-id="setup-azme"></a>setup-mobile-engagement-for-your-android-app"></a><a id="setup-azme"></a>Einrichten von Mobile Engagement für Ihre App
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="<a-id="connecting-app"></a>connect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
In diesem Lernprogramm wird eine "einfache Integration" dargestellt. Dabei handelt es sich um den minimalen erforderlichen Satz zur Sammlung von Daten und zum Senden einer Pushbenachrichtigung. 

Wir erstellen eine einfache App mit Xamarin Studio, um die Integration zu veranschaulichen.

### <a name="create-a-new-xamarin.android-project"></a>Erstellen eines neuen Xamarin.Android-Projekts
1. Starten Sie **Xamarin Studio**, und klicken Sie dann auf **Datei** -> **Neu** -> **Projektmappe**. 
   
    ![][1]
2. Wählen Sie **Android-App**, und stellen Sie sicher, dass als Sprache **C#** ausgewählt ist. Klicken Sie auf **Weiter**.
   
    ![][2]
3. Geben Sie **App-Name** und **Organisations-ID** ein. Vergewissern Sie sich, dass **Google Play Services** aktiviert ist, und klicken Sie dann auf **Weiter**. 
   
    ![][3]
4. Aktualisieren Sie ggf. **Projektname**, **Projektmappenname** und **Speicherort**, und klicken Sie auf **Erstellen**.
   
    ![][4]

Xamarin Studio erstellt die App, in die wir Mobile Engagement integrieren. 

### <a name="connect-your-app-to-mobile-engagement-backend"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End.
1. Klicken Sie im Projektmappenfenster mit der rechten Maustaste auf den Ordner **Pakete**, und wählen Sie **Pakete hinzufügen...**.
   
    ![][5]
2. Suchen Sie nach dem **Microsoft Azure Mobile Engagement Xamarin SDK** , und fügen Sie es zur Projektmappe hinzu.  
   
    ![][6]
3. Öffnen Sie **MainActivity.cs** , und fügen Sie die folgenden using-Anweisungen hinzu:
   
        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;
4. Fügen Sie in der `OnCreate` -Methode Folgendes hinzu, um die Verbindung mit dem Mobile Engagement-Back-End zu initialisieren. Fügen Sie auch **ConnectionString**hinzu. 
   
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

### <a name="add-permissions-and-a-service-declaration"></a>Hinzufügen von Berechtigungen und einer Dienstdeklaration
1. Öffnen Sie im Eigenschaftenordner die Datei **Manifest.xml** . Wählen Sie die Registerkarte für die Quelle, um die XML-Quelle direkt zu aktualisieren.
2. Fügen Sie diese Berechtigungen in der Datei „Manifest.xml“ (im Ordner **Eigenschaften`<application>`) des Projekts unmittelbar vor oder nach dem **-Tag hinzu:
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
3. Fügen Sie folgendes zwischen den Tags `<application>` und `</application>` hinzu, um den Agentdienst zu deklarieren:
   
        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>
4. Ersetzen Sie in dem Code, den Sie gerade eingefügt haben, `"<Your application name>"` in der Beschriftung (in "label"). Dies wird im Menü **Einstellungen** angezeigt. Hier können Benutzer die auf dem Gerät ausgeführten Dienste sehen. Sie können z. B. das Wort "Dienst" in der Beschriftung hinzufügen.

### <a name="send-a-screen-to-mobile-engagement"></a>Senden eines Bildschirms an Mobile Engagement
Um mit dem Senden von Daten zu beginnen und sicherzustellen, dass die Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm an das Mobile Engagement-Back-End schicken. Stellen Sie dazu sicher, dass `MainActivity` von `EngagementActivity` anstelle von `Activity` erbt.

    public class MainActivity : EngagementActivity

Alternativ dazu gilt Folgendes: Wenn das Erben von `EngagementActivity` nicht möglich ist, müssen Sie in `OnResume` bzw. `OnPause` die `.StartActivity`-Methode bzw. die `.EndActivity`-Methode hinzufügen.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }

            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

## <a name="<a-id="monitor"></a>connect-app-with-real-time-monitoring"></a><a id="monitor"></a>Verbinden der App mit Überwachung in Echtzeit
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="<a-id="integrate-push"></a>enable-push-notifications-and-in-app-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und In-App-Messaging
Mit Mobile Engagement können Sie mit Ihren Benutzern interagieren und diese mit Push-Benachrichtigungen und In-App-Nachrichten im Rahmen von Kampagnen erreichen. Dieses Modul nennt sich REACH im Mobile Engagement-Portal.
In den folgenden Abschnitten richten Sie Ihre App für den Empfang ein.

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png



<!--HONumber=Oct16_HO2-->


