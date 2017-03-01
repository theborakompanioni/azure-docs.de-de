---
title: Windows Universal-Apps Reach SDK-Integration
description: Integrieren von Azure Mobile Engagement Reach in Windows Universal-Apps
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a31ca1d6-856f-4aec-898a-07969ae5f7ec
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 8fc1faac88cab0e3d3755fdf16fe3d7188169961
ms.lasthandoff: 11/17/2016


---
# <a name="windows-universal-apps-reach-sdk-integration"></a>Windows Universal-Apps Reach SDK-Integration
Bevor Sie dieser Anleitung folgen, müssen Sie das unter [Integration des Windows Universal Engagement-SDK](mobile-engagement-windows-store-integrate-engagement.md) beschriebene Integrationsverfahren befolgen.

## <a name="embed-the-engagement-reach-sdk-into-your-windows-universal-project"></a>Einbetten des Engagement Reach-SDK in das Windows Universal-Projekt
Sie müssen nichts hinzufügen. `EngagementReach` -Referenzen und -Ressourcen sind bereits in Ihrem Projekt enthalten.

> [!TIP]
> Sie können die Bilder anpassen, die sich im Ordner `Resources` Ihres Projekts befinden, insbesondere das Markensymbol (standardmäßig das Engagement-Symbol). In Universal-Apps können Sie auch den Ordner `Resources` in ihr freigegebenes Projekt bewegen, um den Inhalt zwischen den Anwendungen zu teilen. Sie müssen allerdings die Datei `Resources\EngagementConfiguration.xml` im Standardverzeichnis liegen lassen, da sie plattformabhängig ist.
> 
> 

## <a name="enable-the-windows-notification-service"></a>Aktivieren des Windows-Benachrichtigungsdienstes
### <a name="windows-8x-and-windows-phone-81-only"></a>Nur Windows 8.x und Windows Phone 8.1
Um den **Windows-Benachrichtigungsdienst** (auch als WNS, Windows Notification Service, bezeichnet) in Ihrer `Package.appxmanifest`-Datei in `Application UI` zu verwenden, klicken Sie im linken Feld auf `All Image Assets`. Rechts neben dem Feld i `Notifications`, ändern Sie `toast capable` von `(not set)` zu `(Yes)`.

### <a name="all-platforms"></a>Alle Plattformen
Sie müssen Ihre App mit Ihrem Microsoft-Konto und der Engagement-Plattform synchronisieren. Zu diesem Zweck müssen Sie ein Konto erstellen oder sich beim [Windows Dev Center](https://dev.windows.com)anmelden. Erstellen Sie anschließend eine neue Anwendung, und suchen Sie die SID sowie den geheimen Schlüssel. Gehen Sie im Engagement-Front-End zur Ihren App-Einstellungen unter `native push` und fügen Sie Ihre Anmeldedaten ein. Danach klicken Sie mit der rechten Maustaste auf das Projekt, wählen `store` und `Associate App with the Store...`. Sie müssen lediglich die von Ihnen erstellte Anwendung auswählen, bevor Sie sie synchronisieren.

## <a name="initialize-the-engagement-reach-sdk"></a>Initialisieren des Engagement-Reichweiten-SDK
Ändern Sie `App.xaml.cs`:

* Fügen Sie `EngagementReach.Instance.Init` direkt nach `EngagementAgent.Instance.Init` in Ihrer `InitEngagement`-Methode ein:
  
      private void InitEngagement(IActivatedEventArgs e)
      {
        EngagementAgent.Instance.Init(e);
        EngagementReach.Instance.Init(e);
      }
  
  Die `EngagementReach.Instance.Init` wird in einem dedizierten Thread ausgeführt. Sie müssen es nicht selbst ausführen.

> [!NOTE]
> Wenn Sie Pushbenachrichtigungen an anderer Stelle in Ihrer Anwendung verwenden, dann Sie müssen [Ihren Pushkanal freigeben](#push-channel-sharing) mit Engagement Reach.
> 
> 

## <a name="integration"></a>Integration
Engagement bietet zwei Möglichkeiten, die Reach-In-App-Banner und Interstitialansichten für Ankündigungen und Umfragen in Ihrer Anwendung hinzuzufügen: die Overlayintegration und die manuelle Integration von Webansichten. Beide Ansätze sollten nicht auf der gleichen Seite kombiniert werden.

Die Wahl zwischen den beiden Integrationen kann so zusammengefasst werden:

* Sie können die Overlayintegration auswählen, wenn Ihre Seiten bereits von Agent `EngagementPage` erben; es geht nur darum, in Ihren Seiten `EngagementPage` durch `EngagementPageOverlay` und `xmlns:engagement="using:Microsoft.Azure.Engagement"` durch `xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"` zu ersetzen.
* Sie können die manuelle Integration von Webansichten auswählen, wenn Sie die Reach-Benutzeroberfläche genau innerhalb Ihrer Seiten platzieren möchten, oder wenn Sie Ihren Seiten keine weitere Vererbungsebene hinzufügen möchten. 

### <a name="overlay-integration"></a>Überlagerungsintegration
Das Engagement-Overlay fügt dynamisch die Benutzeroberflächenelemente hinzu, die zum Anzeigen von Reach-Kampagnen auf Ihrer Seite verwendet werden. Wenn das Overlay nicht Ihrem Layout entspricht, sollten Sie stattdessen die manuelle Integration der Webansicht erwägen.

Ändern Sie in der XAML-Datei den `EngagementPage`-Verweis in `EngagementPageOverlay`.

* Fügen Sie Ihre Namespace-Deklarationen hinzu:
  
      xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"
* Ersetzen Sie `engagement:EngagementPage` durch `engagement:EngagementPageOverlay`:

**Mit EngagementPage:**

        <engagement:EngagementPage 
            xmlns:engagement="using:Microsoft.Azure.Engagement">

            <!-- Your layout -->
        </engagement:EngagementPage>

**Mit EngagementPageOverlay:**

        <engagement:EngagementPageOverlay 
            xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay">

            <!-- Your layout -->
        </engagement:EngagementPageOverlay>

Markieren Sie dann in der CS-Datei Ihre Seite in `EngagementPageOverlay` anstatt `EngagementPage`, und importieren Sie `Microsoft.Azure.Engagement.Overlay`.

            using Microsoft.Azure.Engagement.Overlay;

* Ersetzen Sie `EngagementPage` durch `EngagementPageOverlay`:

**Mit EngagementPage:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPage
              {
                [...]
              }
            }

**Mit EngagementPageOverlay:**

            using Microsoft.Azure.Engagement.Overlay;

            namespace Example
            {
              public sealed partial class ExamplePage : EngagementPageOverlay 
              {
                [...]
              }
            }


Das Engagement-Overlay fügt ein `Grid`-Element oben auf der Seite hinzu, das aus Ihrem Layout und den beiden `WebView`-Elementen besteht – einem für das Banner und dem anderen für die Interstitialansicht.

Sie können die Overlay-Elemente direkt in der `EngagementPageOverlay.cs`-Datei anpassen.

### <a name="web-views-manual-integration"></a>Manuelle Integration von Webansichten
Reach sucht auf Ihren Seiten nach den beiden `WebView` -Elementen, die für die Anzeige des Banners und der Interstitialansicht verantwortlich sind. Sie müssen nur diese beiden `WebView` -Elemente an einer beliebigen Stelle in Ihren Seiten hinzufügen – hier ist ein Beispiel:

    <Grid x:Name="engagementGrid">

      <!-- Your layout -->

      <WebView x:Name="engagement_notification_content" Visibility="Collapsed" Height="80" HorizontalAlignment="Stretch" VerticalAlignment="Top"/>
      <WebView x:Name="engagement_announcement_content" Visibility="Collapsed"  HorizontalAlignment="Stretch"  VerticalAlignment="Stretch"/>
    </Grid>


In diesem Beispiel werden die `WebView` -Elemente gestreckt, damit sie in ihren Container passen, der automatisch bei Bildschirmdrehung oder Fenstergrößenänderung ihre Größe ändert.

> [!WARNING]
> Es ist wichtig, die gleiche Benennung `engagement_notification_content` und `engagement_announcement_content` für die `WebView`-Elemente beizubehalten. Reach identifiziert sie anhand ihres Namens. 
> 
> 

## <a name="handle-datapush-optional"></a>Behandeln von Datapush (optional)
Wenn Sie Ihre Anwendung Reichweitendaten per Push empfangen soll, müssen Sie zwei Ereignisse der EngagementReach-Klasse implementieren:

Fügen Sie in „App.Xaml.cs“ im App()-Konstruktor hinzu:

            EngagementReach.Instance.DataPushStringReceived += (body) =>
            {
              Debug.WriteLine("String data push message received: " + body);
              return true;
            };

            EngagementReach.Instance.DataPushBase64Received += (decodedBody, encodedBody) =>
            {
              Debug.WriteLine("Base64 data push message received: " + encodedBody);
              // Do something useful with decodedBody like updating an image view
              return true;
            };

Sie sehen, dass der Rückruf jeder Methode einen booleschen Wert zurückgibt. Engagement sendet ein Feedback an sein Back-End nach der Verteilung der Daten per Push. Wenn der Rückruf "false" zurückgibt, wird das Feedback `exit` gesendet. Andernfalls lautet es `action`. Wird für die Ereignisse kein Rückruf festgelegt, wird das Feedback `drop` an Engagement zurückgegeben.

> [!WARNING]
> Engagement kann nicht mehrere Feedbacks für einen Daten-Push-Vorgang empfangen. Wenn Sie beabsichtigen, mehrere Handler für ein Ereignis festzulegen, beachten Sie, dass das Feedback dem letzten gesendeten Handler entspricht. In diesem Fall empfehlen wir, immer denselben Wert zurückzugeben, um verwirrendes Feedback vom Front-End zu vermeiden.
> 
> 

## <a name="customize-ui-optional"></a>Anpassen der Benutzeroberfläche (optional)
### <a name="first-step"></a>Erster Schritt
Wir ermöglichen Ihnen, die Reichweiten-Benutzeroberfläche anzupassen.

Hierzu müssen Sie eine Unterklasse der `EngagementReachHandler` -Klasse erstellen.

**Beispielcode:**

            using Microsoft.Azure.Engagement;

            namespace Example
            {
              internal class ExampleReachHandler : EngagementReachHandler
              {
               // Override EngagementReachHandler methods depending on your needs
              }
            }

Legen Sie dann den Inhalt des `EngagementReach.Instance.Handler`-Feldes mit Ihrem benutzerdefinierten Objekt in der `App.xaml.cs`-Klasse mit der `App()`-Methode fest.

**Beispielcode:**

            protected override void OnLaunched(LaunchActivatedEventArgs args)
            {
              // your app initialization 
              EngagementReach.Instance.Handler = new ExampleReachHandler();
              // Engagement Agent and Reach initialization
            }

> [!NOTE]
> Engagement verwendet standardmäßig seine eigene Implementierung von `EngagementReachHandler`.
> Sie müssen keine eigene erstellen, und wenn Sie dies tun, müssen Sie nicht jede Methode außer Kraft setzen. Das Standardverhalten ist die Auswahl des Engagement-Basisobjekts.
> 
> 

### <a name="web-view"></a>Webansicht
Standardmäßig verwendet die Reichweite die eingebetteten Ressourcen der DLL, um die Benachrichtigungen und Seiten anzuzeigen.

Um eine vollständige Anpassung zu ermöglichen, verwenden wir nur die Webansicht. Wenn Sie Layouts anpassen möchten, überschreiben Sie direkt die Ressourcendateien `EngagementAnnouncement.html` und `EngagementNotification.html`. Für Engagement muss sämtlicher Code in `<body></body>` vorliegen, um ordnungsgemäß ausgeführt zu werden. Aber Sie können einen Tag zu `engagement_webview_area`hinzufügen.

Sie können sich außerdem dafür entscheiden, Ihre eigenen Ressourcen zu verwenden.

Sie können `EngagementReachHandler`-Methoden in Ihrer Unterklasse überschreiben, damit Engagement Ihre Layouts verwendet. Achten Sie dabei jedoch auf den eingebetteten Engagement-Mechanismus:

**Beispielcode:**

            // In your subclass of EngagementReachHandler

            public override string GetAnnouncementHTML()
            {
              return base.GetAnnouncementHTML();
            }
            public override string GetAnnouncementName()
            {
              return base.GetAnnouncementName();
            }
            public override string GetNotfificationHTML()
            {
              return base.GetNotfificationHTML();
            }
            public override string GetNotfificationName()
            {
              return base.GetNotfificationName();
            }


Standardmäßig ist AnnouncementHTML `ms-appx-web:///Resources/EngagementAnnouncement.html`. Es stellt die HTML-Datei dar, die den Inhalt einer Push-Nachricht (Textankündigung, Webankündigung und Umfrageankündigung) entwirft. Der AnnouncementName lautet `engagement_announcement_content`. Es ist der Name des Webansicht-Entwurfs auf Ihrer XAML-Seite.

NotificationHTML lautet `ms-appx-web:///Resources/EngagementNotification.html`. Es stellt die HTML-Datei dar, die die Benachrichtigung einer Push-Nachricht entwirft. Der NotificationName lautet `engagement_notification_content`. Es ist der Name des Webansicht-Entwurfs auf Ihrer XAML-Seite.

### <a name="customization"></a>Anpassung
Sie können Benachrichtigungs- und Ankündigungs-Webansichten nach Belieben anpassen, wenn Sie das Engagement-Objekt beibehalten. Stellen Sie sicher, dass das Webansichtenobjekt drei Mal beschrieben wird: Das erste Mal in Ihrer XAML, das zweite Mal in Ihrer CS-Datei in der „setwebview()“-Methode und das dritte Mal in der HTML-Datei.

* Beschreiben Sie in der XAML die aktuelle Webansicht-Komponente des grafischen Layouts.
* In der CS-Datei können Sie „setwebview()“ definieren, in dem Sie die Dimension der beiden Webansichten (Benachrichtigung, Ankündigung) einstellen. Es ist sehr effektiv, wenn die Anwendung die Größe ändert.
* In der Engagement-HTML-Datei beschreiben wir den Inhalt der Webansicht, das Design und die Elementpositionen untereinander.

### <a name="launch-message"></a>Starten der Nachricht
Klickt ein Benutzer auf eine Benachrichtigung des Systems (einen Toast), startet Engagement die Anwendung, lädt den Inhalt der Push-Nachrichten und zeigt die Seite der entsprechenden Kampagne an.

Es besteht eine Verzögerung zwischen dem Start der Anwendung und der Anzeige der Seite (je nach Geschwindigkeit Ihres Netzwerks).

Um dem Benutzer zu verdeutlichen, dass etwas geladen wird, sollten Sie eine visuelle Information, wie z. B. eine Statusleiste oder eine Fortschrittsanzeige, bieten. Engagement kann dies nicht selbst verarbeiten, stellt jedoch ein paar Handler zur Verfügung.

Fügen Sie zum Implementieren des Rückrufs in „App.Xaml.cs“ in „Public App(){}“ Folgendes hinzu:

            /* The application has launched and the content is loading.
             * You should display an indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageStarted += () => { [...] };

            /* The application has finished loading the content and the page
             * is about to be displayed.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageCompleted += () => { [...] };

            /* The content has been loaded, but an error has occurred.
             * You can provide an information to the user.
             * You should hide the indicator here.
             */
            EngagementReach.Instance.RetrieveLaunchMessageFailed += () => { [...] };

Sie können den Rückruf in Ihrer „Public App(){}“-Methode der `App.xaml.cs`-Datei einstellen, vorzugsweise vor dem `EngagementReach.Instance.Init()`-Aufruf.

> [!TIP]
> Jeder Handler wird vom UI-Thread aufgerufen. Sie müssen sich keine Sorgen machen, wenn Sie eine MessageBox oder ein Benutzeroberflächenelement verwenden.
> 
> 

## <a name="a-idpush-channel-sharinga-push-channel-sharing"></a><a id="push-channel-sharing"></a> Freigeben von Pushkanälen
Wenn Sie Pushbenachrichtigungen in Ihrer Anwendung für einen anderen Zweck verwenden, müssen Sie die Funktion des Engagement-SDK zur Freigabe des Pushkanals verwenden. Dadurch werden verpasste Pushs vermieden.

* Sie können Ihre eigenen Pushkanal für die Engagement Reach-Initialisierung bereitstellen. Das SDK verwendet diesen Kanal, anstatt einen neuen anzufordern.

Aktualisieren Sie die Engagement Reach-Initialisierung mit dem Pushkanal in der `InitEngagement`-Methode aus der `App.xaml.cs`-Datei:

    /* Your own push channel logic... */
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    /*...Engagement initialization */
    EngagementAgent.Instance.Init(e);
    EngagementReach.Instance.Init(e,pushChannel);

* Wenn Sie den Pushkanal nur nach der Engagement Reach-Initialisierung nutzen möchten, können Sie einen Rückruf für Engagement Reach einrichten, um den Pushkanal zu erhalten, sobald er vom SDK erstellt wurde.

Legen Sie den Rückruf auf einen beliebigen Zeitpunkt **nach** der Reach-Initialisierung fest:

    /* Set action on the SDK push channel. */
    EngagementReach.Instance.SetActionOnPushChannel((PushNotificationChannel channel) => 
    {
      /* The forwarded channel can be null if its creation fails for any reason. */
      if (channel != null)
      {
        /* Your own push channel logic... */
      });
    }

## <a name="custom-scheme-tip"></a>Tipp zu benutzerdefinierten Schemas
Wir stellen benutzerdefinierte Schemaverwendung zur Verfügung. Sie können einen anderen URI-Typ vom Engagement-Front-End senden, der in Ihrer Engagement-Anwendung verwendet werden soll. Standardschemas wie `http, ftp, ...` werden von Windows verwaltet und ein Fenster wird angezeigt, wenn keine Standardanwendung auf dem Gerät installiert ist. Sie können auch ein benutzerdefiniertes Schema für Ihre Anwendung erstellen.

Die einfachste Möglichkeit, ein benutzerdefiniertes Schema in Ihrer Anwendung einzurichten, besteht darin, `Package.appxmanifest` im Bereich `Declarations` zu öffnen. Wählen Sie `Protocol` im Bildlauffeld „Verfügbare Deklarationen“ und fügen Sie es hinzu. Bearbeiten Sie das Feld `Name` mit dem gewünschten Namen für das neue Protokoll.

Um dieses Protokoll nun zu verwenden, bearbeiten Sie `App.xaml.cs` mit der `OnActivated`-Methode und initialisieren Sie auch hier Engagement:

            /// <summary>
            /// Enter point when app his called by another way than user click
            /// </summary>
            /// <param name="args">Activation args</param>
            protected override void OnActivated(IActivatedEventArgs args)
            {
              /* Init engagement like it was launch by a custom uri scheme */
              EngagementAgent.Instance.Init(args);
              EngagementReach.Instance.Init(args);

              //TODO design action to do when app is launch

              #region Custom scheme use
              if (args.Kind == ActivationKind.Protocol)
              {
                ProtocolActivatedEventArgs myProtocol = (ProtocolActivatedEventArgs)args;

                if (myProtocol.Uri.Scheme.Equals("protocolName"))
                {
                  string path = myProtocol.Uri.AbsolutePath;
                }
              }
              #endregion


