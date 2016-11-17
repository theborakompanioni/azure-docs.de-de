---
title: "Erste Schritte mit Azure Mobile Engagement für Windows Universal-Apps"
description: "Erfahren Sie, wie Sie Azure Mobile Engagement mit Analysen und Pushbenachrichtigungen für Windows Universal-Apps verwenden."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: dwrede
editor: 
ms.assetid: 48103867-7f64-4646-b019-42bd797d38e2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 59a4c270be4bb9a0d247ce81da548b58ce4baf3f


---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a>Erste Schritte mit Azure Mobile Engagement für Windows Universal-Apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema wird gezeigt, wie Sie mit Azure Mobile Engagement Ihre App-Nutzung nachvollziehen und Pushbenachrichtigungen an nach Segmenten eingeteilte Benutzer einer Windows Universal-Anwendung senden können.
In diesem Tutorial wird ein einfaches Übertragungsszenario mit Mobile Engagement dargestellt. Darin erstellen Sie eine leere Windows Universal-App, die Basisdaten zur App-Nutzung erfasst und Pushbenachrichtigungen mit dem Windows Notification Service (WNS) empfängt.

## <a name="prerequisites"></a>Voraussetzungen
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a>Einrichten von Mobile Engagement für Ihre Windows Universal-App
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="a-idconnectingappaconnect-your-app-to-the-mobile-engagement-backend"></a><a id="connecting-app"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End
In diesem Tutorial wird eine „einfache Integration“ dargestellt. Dabei handelt es sich um den minimalen erforderlichen Satz zur Sammlung von Daten und zum Senden einer Pushbenachrichtigung. Die vollständige Integrationsdokumentation finden Sie im Artikel zur [Integration des Mobile Engagement Windows Universal SDK](mobile-engagement-windows-store-sdk-overview.md).

Sie erstellen eine einfache App mit Visual Studio, um die Integration zu veranschaulichen.

### <a name="create-a-windows-universal-app-project"></a>Erstellen eines Windows Universal-App-Projekts
In den folgenden Schritten wird die Verwendung von Visual Studio 2015 angenommen, obwohl die Schritte in früheren Versionen von Visual Studio ähnlich sind.

1. Starten Sie Visual Studio, und wählen Sie auf dem **Startbildschirm** die Option **Neues Projekt**.
2. Wählen Sie im Popupfenster **Windows** -> **Universell** -> **Leere App (Universelle Windows-App)**. Geben Sie den **Namen** der App und den **Lösungsnamen** ein, und klicken Sie dann auf **OK**.
   
    ![][1]

Sie haben nun ein Projekt für eine Windows Universal-App erstellt, in die Sie als Nächstes das Azure Mobile Engagement-SDK integrieren.

### <a name="connect-your-app-to-mobile-engagement-backend"></a>Verbinden Sie Ihre App mit dem Mobile Engagement-Back-End.
1. Installieren Sie das NuGet-Paket [MicrosoftAzure.MobileEngagement] im Projekt. Wenn Sie sowohl für die Windows- als auch für die Windows Phone-Plattform entwickeln, müssen Sie dies für beide Projekte durchführen. Für Windows 8.x und Windows Phone 8.1 platziert das gleiche NuGet-Paket die richtigen plattformspezifischen Binärdateien in jedem Projekt.
2. Öffnen Sie **Package.appxmanifest** , und stellen Sie sicher, dass dort die folgende Funktion hinzugefügt ist:
   
        Internet (Client)
   
    ![][2]
3. Nun fügen Sie die zuvor für Ihre Mobile Engagement-App kopierte Verbindungszeichenfolge in die Datei `Resources\EngagementConfiguration.xml` zwischen die Tags `<connectionString>` und `</connectionString>` ein:
   
    ![][3]

    >[AZURE.TIP] Wenn für Ihre App sowohl Windows als auch Windows Phone als Plattform vorgesehen ist, sollten Sie dennoch zwei Mobile Engagement-Anwendungen erstellen – eine für jede unterstützte Plattform. Durch die Verwendung von zwei Apps wird sichergestellt, dass Sie eine korrekte Aufteilung der Zielgruppe erstellen und entsprechend gezielte Benachrichtigungen für jede Plattform senden können.

    > [AZURE.IMPORTANT] NuGet kopiert die SDK-Ressourcen in Ihrer Windows 10 UWP-Anwendung nicht automatisch. Sie müssen dies manuell anhand der angezeigten Schritte („readme.txt“) durchführen, wenn das NuGet-Paket installiert ist.  

1. In der Datei `App.xaml.cs`:
   
    a. Fügen Sie die Anweisung `using` hinzu:
   
            using Microsoft.Azure.Engagement;
   
    b. Fügen Sie eine Methode für die Engagement-Initialisierung hinzu:
   
           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);
   
             //... rest of the code
           }
   
    c. Initialisieren Sie das SDK in der **OnLaunched** -Methode:
   
            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);
   
              //... rest of the code
            }
   
    c. Fügen Sie Folgendes in der **OnActivated** -Methode ein, und fügen Sie die Methode hinzu, falls sie nicht schon vorhanden ist:
   
            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);
   
              //... rest of the code
            }

## <a name="a-idmonitoraenable-realtime-monitoring"></a><a id="monitor"></a>Aktivieren der Überwachung in Echtzeit
Um mit dem Senden von Daten zu beginnen und sicherzustellen, dass die Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm (Aktivität) an das Mobile Engagement-Back-End schicken.

1. Fügen Sie in **MainPage.Xaml.cs** die folgende `using`-Anweisung hinzu:
   
    using Microsoft.Azure.Engagement.Overlay;
2. Ändern Sie die **MainPage**-Basisklasse von **Page** in **EngagementPageOverlay**:
   
        class MainPage : EngagementPageOverlay
3. In der Datei `MainPage.xaml`:
   
    a. Fügen Sie Ihre Namespace-Deklarationen hinzu:
   
        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
   
    b. Ersetzen Sie **Page** im XML-Tagnamen durch **engagement:EngagementPageOverlay**.

> [!IMPORTANT]
> Wenn Ihre Seite die Methode `OnNavigatedTo` überschreibt, rufen Sie unbedingt `base.OnNavigatedTo(e)` auf. Andernfalls wird die Aktivität nicht gemeldet (`EngagementPage` ruft `StartActivity` in seiner `OnNavigatedTo`-Methode auf). Dies ist besonders wichtig in einem Windows Phone-Projekt, in dem die Standardvorlage eine `OnNavigatedTo` -Methode besitzt.
> 
> [!IMPORTANT]
> Verwenden Sie für **universelle Windows 10-Apps** [diese empfohlene Methode](mobile-engagement-windows-store-advanced-reporting.md#recommended-method-overload-your-codepagecode-classes) anstelle der oben genannten.
> 
> 

## <a name="a-idmonitoraconnect-app-with-realtime-monitoring"></a><a id="monitor"></a>Verbinden der App mit Überwachung in Echtzeit
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="a-idintegratepushaenable-push-notifications-and-inapp-messaging"></a><a id="integrate-push"></a>Aktivieren von Pushbenachrichtigungen und In-App-Messaging
Mit Mobile Engagement können Sie mit Ihren Benutzern interagieren und diese mit Pushbenachrichtigungen und In-App-Nachrichten im Rahmen von Kampagnen erreichen. Dieses Modul nennt sich REACH im Mobile Engagement-Portal.
In den folgenden Abschnitten richten Sie Ihre App für den Empfang ein.

### <a name="enable-your-app-to-receive-wns-push-notifications"></a>Aktivieren Sie Ihre App für den Empfang von WNS-Pushbenachrichtigungen
1. Legen Sie in der Datei `Package.appxmanifest` auf der Registerkarte **Anwendung** unter **Benachrichtigungen** die Option **Toastfähig** auf **Ja** fest.
   
    ![][5]

### <a name="initialize-the-reach-sdk"></a>Initialisieren des REACH-SDK
Rufen Sie in `App.xaml.cs` in der **InitEngagement**-Funktion direkt nach der Agent-Initialisierung **EngagementReach.Instance.Init(e);** auf:

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

Sie können jetzt ein Popup senden. Als Nächstes überprüfen wir, ob Sie diese einfache Integration richtig durchgeführt haben.

### <a name="grant-access-to-mobile-engagement-to-send-notifications"></a>Gewähren von Zugriff auf Mobile Engagement zum Senden von Benachrichtigungen
1. Öffnen Sie das [Windows Store Dev Center] in Ihrem Webbrowser, melden Sie sich an, und erstellen Sie ein Konto, falls dies erforderlich ist.
2. Klicken Sie oben rechts auf **Dashboard**, und klicken Sie im linken Menü des Bereichs auf **Neue App erstellen**.
   
    ![][9]
3. Erstellen Sie Ihre App, indem Sie ihren Namen reservieren.
   
    ![][10]
4. Sobald die App erstellt wurde, navigieren Sie im linken Menü zu **Dienste > Pushbenachrichtigungen**.
   
    ![][11]
5. Klicken Sie im Abschnitt „Pushbenachrichtigungen“ auf den Link **Live Services-Website** .
   
    ![][12]
6. Navigieren Sie zum Abschnitt „Pushanmeldeinformationen“. Stellen Sie sicher, dass der Abschnitt **App-Einstellungen** angezeigt wird, und kopieren Sie dann die **Paket-SID** und den **geheimen Clientschlüssel**.
   
    ![][13]
7. Navigieren Sie zu den **Einstellungen** Ihres Mobile Engagement-Portals, und klicken Sie auf den Abschnitt **Native Push** auf der linken Seite. Klicken Sie dann auf die Schaltfläche **Bearbeiten**, um Ihre **Paket-Sicherheits-ID (SID)** und Ihren **geheimen Schlüssel** einzugeben. Dies ist hier dargestellt:
   
    ![][6]
8. Stellen Sie abschließend sicher, dass Sie Ihre Visual Studio-App mit dieser erstellten App im App Store verknüpfen. Klicken Sie in Visual Studio auf **App mit Store verknüpfen** .
    ![][7]

## <a name="a-idsendasend-a-notification-to-your-app"></a><a id="send"></a>Versenden von Benachrichtigungen an die App
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Wenn die App ausgeführt wird, sehen Sie eine In-App-Benachrichtigung. Wenn die App geschlossen ist, sehen Sie eine Popupbenachrichtigung.
Wenn eine In-App-Benachrichtigung in der App angezeigt wird, aber keine Popupbenachrichtigung, und Sie die App in Visual Studio im Debugmodus ausführen, können Sie in der Symbolleiste **Zyklusereignisse > Anhalten** wählen. So stellen Sie sicher, dass die App angehalten wird. Wenn Sie beim Debuggen der Anwendung in Visual Studio auf die Schaltfläche „Home“ (Anfang) geklickt haben, wird diese nicht immer angehalten. Es wird zwar eine In-App-Benachrichtigung angezeigt, aber keine Popupbenachrichtigung.  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK-Dokumentation]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[Windows Store Dev Center]: https://dev.windows.com
[Windows Universal-Apps – Überlagerungsintegration]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png



<!---HONumber=Nov16_HO2-->


