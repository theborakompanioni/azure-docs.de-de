---
title: "Erste Schritte mit Azure Notification Hubs für Apps für die universelle Windows-Plattform | Microsoft-Dokumentation"
description: "In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs Pushbenachrichtigungen an eine App für die universelle Windows-Plattform senden."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: erikre
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: e18a810bcdbd97c79418f53c647df8723ecb6076
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="get-started-with-notification-hubs-for-universal-windows-platform-apps"></a>Erste Schritte mit Notification Hubs für Apps für die universelle Windows-Plattform

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Übersicht
Dieser Artikel zeigt, wie Sie mit Azure Notification Hubs Pushbenachrichtigungen an eine App für die universelle Windows-Plattform senden.

In diesem Artikel erstellen Sie eine leere Windows Store-App, die Pushbenachrichtigungen unter Verwendung des Windows-Pushbenachrichtigungsdiensts (WNS) empfängt. Am Ende dieses Artikels können Sie über Ihren Notification Hub Pushbenachrichtigungen an alle Geräte senden, die Ihre App ausführen.

## <a name="before-you-begin"></a>Voraussetzungen
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Den vollständigen Code für dieses Tutorial finden Sie auf [GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) oder höher
* [Installierte Entwicklungstools für UWP-Apps](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Ein aktives Azure-Konto   
    Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
* Ein aktives Windows Store-Konto

Dieses Tutorial muss für alle anderen Notification Hubs-Tutorials für UWP-Apps abgeschlossen werden.

## <a name="register-your-app-for-the-windows-store"></a>Registrieren der App für den Windows Store
Ordnen Sie Ihre App dem Windows Store zu, um Pushbenachrichtigungen an UWP-Apps senden zu können. Konfigurieren Sie anschließend Ihren Notification Hub für die Integration in WNS.

1. Falls Sie Ihre App noch nicht registriert haben, navigieren Sie zum [Windows Dev Center](https://dev.windows.com/overview), melden Sie sich dort mit Ihrem Microsoft-Konto an, und wählen Sie dann **Neue App erstellen** aus.

2. Geben Sie einen Namen für Ihre App ein, und wählen Sie anschließend **App-Namen reservieren** aus. Dadurch wird eine neue Windows Store-Registrierung für Ihre App erstellt.

3. Erstellen Sie in Visual Studio ein neues Visual C# Store-App-Projekt mit der UWP-Vorlage **Leere App**, und wählen Sie anschließend **OK** aus.

4. Übernehmen Sie die Standardwerte für die Zielversion und die Mindestversion der Plattform.

5. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Windows Store-App-Projekt, und wählen Sie **Store** und anschließend **App mit Store verknüpfen** aus.  
    Daraufhin wird der Assistent **App mit Windows Store verknüpfen** angezeigt.

6. Melden Sie sich im Assistenten mit Ihrem Microsoft-Konto an.

7. Wählen Sie die App aus, die Sie in Schritt 2 registriert haben. Wählen Sie dann **Weiter** und anschließend **Zuordnen** aus. Dadurch werden die erforderlichen Windows Store-Registrierungsinformationen zum Anwendungsmanifest hinzugefügt.

8. Wählen Sie auf der Seite [Windows Dev Center](http://dev.windows.com/overview) für Ihre neue App **Dienste** > **Pushbenachrichtigungen** > **WNS/MPNS** aus.

9. Wählen Sie **Neue Benachrichtigung** aus.

10. Wählen Sie die Vorlage **Blank (Toast)** (Leer (Popup)) und anschließend **OK** aus.

11. Geben Sie unter **Name** einen Namen und unter **Kontext** eine visuelle Kontextnachricht für die Benachrichtigung ein. Wählen Sie anschließend **Als Entwurf speichern** aus.

12. Navigieren Sie zum [App-Registrierungsportal](http://apps.dev.microsoft.com), und melden Sie sich an.

13. Wählen Sie Ihren Anwendungsnamen aus. Notieren Sie sich in den Plattformeinstellungen für **Windows Store** das Kennwort unter **Anwendungsgeheimnis** sowie die **Paketsicherheits-ID (SID)**.

    >[!WARNING]
    >Der geheime Schlüssel der Anwendung und die Paket-SID sind wichtige Sicherheitsanmeldeinformationen. Geben Sie diese Werte nicht weiter, und verteilen Sie sie nicht mit Ihrer Anwendung.

## <a name="configure-your-notification-hub"></a>Konfigurieren Ihres Notification Hub
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="5">
<li><p>Wählen Sie <b>Notification Services</b> > <b>Windows (WNS)</b> aus, und geben Sie dann das geheime Anwendungskennwort in das Feld <b>Sicherheitsschlüssel</b> ein. Geben Sie im Feld <b>Paket-SID</b> den Wert ein, den Sie im vorherigen Abschnitt in WNS ermittelt haben, und wählen Sie anschließend <b>Speichern</b> aus.</p>
</li>
</ol>

![Die Felder für Paket-SID und Sicherheitsschlüssel](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Ihr Notification Hub ist jetzt für die Zusammenarbeit mit WNS konfiguriert. Sie verfügen über die Verbindungszeichenfolgen, die Sie zum Registrieren Ihrer App und zum Senden von Benachrichtigungen benötigen.

## <a name="connect-your-app-to-the-notification-hub"></a>Verbinden Ihrer App mit dem Notification Hub
1. Klicken Sie in Visual Studio mit der rechten Maustaste auf die Projektmappe, und wählen Sie **NuGet-Pakete verwalten** aus.  
    Das Fenster **NuGet-Pakete verwalten** wird geöffnet.

2. Geben Sie in das Suchfeld die Zeichenfolge **WindowsAzure.Messaging.Managed** ein, wählen Sie **Installieren** aus, und akzeptieren Sie die Nutzungsbedingungen.
   
    ![Das Fenster „NuGet-Pakete verwalten“][20]
   
    Dadurch wird mithilfe des [WindowsAzure.Messaging.Managed-NuGet-Pakets](http://nuget.org/packages/WindowsAzure.Messaging) die Azure Messaging-Bibliothek für Windows heruntergeladen, installiert und ein Verweis auf die Bibliothek hinzugefügt.

3. Öffnen Sie die Projektdatei „App.xaml.cs“, und fügen Sie die folgenden `using`-Anweisungen hinzu: 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;

4. Fügen Sie außerdem in der Datei „App.xaml.cs“ der Klasse **App** die folgende **InitNotificationsAsync**-Methodendefinition hinzu:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("<your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    Dieser Code ruft den Kanal-URI für die App von WNS ab und registriert dann diesen Kanal-URI bei Ihrem Benachrichtigungshub.
   
    >[!NOTE]
    >* Ersetzen Sie den Platzhalter **hub name** durch den Namen des Notification Hubs aus dem Azure-Portal. 
    >* Ersetzen Sie außerdem den Verbindungszeichenfolgen-Platzhalter durch die Verbindungszeichenfolge **DefaultListenSharedAccessSignature**, die Sie in einem der vorherigen Abschnitte über die Seite **Zugriffsrichtlinien** Ihres Notification Hubs ermittelt haben.
   > 
   > 
5. Fügen Sie in der Datei „App.xaml.cs“ am Anfang des **OnLaunched**-Ereignishandlers den folgenden Aufruf zu der neuen **InitNotificationsAsync**-Methode hinzu:
   
        InitNotificationsAsync();
   
    Dadurch wird gewährleistet, dass der Kanal-URI bei jedem Start der Anwendung beim Notification Hub registriert wird.

6. Drücken Sie**** F5, um die App auszuführen. Ein Dialogfeld mit dem Registrierungsschlüssel wird angezeigt.

Ihre App kann jetzt Popupbenachrichtigungen empfangen.

## <a name="send-notifications"></a>Senden von Benachrichtigungen
Der Benachrichtigungsempfang in der App kann schnell durch Senden von Benachrichtigungen im [Azure-Portal](https://portal.azure.com/) getestet werden. Verwenden Sie die Schaltfläche **Testsendevorgang** im Notification Hub, wie in der folgenden Abbildung zu sehen:

![Der Bereich „Testsendevorgang“](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Pushbenachrichtigungen werden normalerweise in einem Back-End-Dienst wie Mobile Services oder ASP.NET unter Verwendung einer kompatiblen Bibliothek gesendet. Falls für Ihr Back-End keine Bibliothek verfügbar ist, können Sie Benachrichtigungsnachrichten auch direkt über die REST-API senden. 

In diesem Tutorial erfahren Sie, wie Sie Ihre Client-App ganz einfach testen, indem Sie Benachrichtigungen, die das .NET SDK für Notification Hubs verwenden, in einer Konsolenanwendung anstatt in einem Back-End-Dienst senden. Als nächster Schritt zum Senden von Benachrichtigungen über ein ASP.NET-Back-End empfiehlt sich das Tutorial [Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer]. Benachrichtigungen können jedoch auf folgende Arten gesendet werden:

* **REST-Schnittstelle:** Mithilfe der [REST-Schnittstelle](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx) können Sie Benachrichtigungen auf allen Back-End-Plattformen unterstützen.

* **Microsoft Azure Notification Hubs .NET SDK:** Führen Sie im NuGet-Paket-Manager für Visual Studio [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) aus.

* **Node.js:** Informationen zur Verwendung von Notification Hubs mit Node.js finden Sie [hier](notification-hubs-nodejs-push-notification-tutorial.md).
* **Azure Mobile Apps:** Ein Beispiel für das Senden von Benachrichtigungen über eine in Notification Hubs integrierte mobile Azure-App finden Sie unter [Hinzufügen von Pushbenachrichtigungen für Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).

* **Java oder PHP:** Beispiele für das Senden von Benachrichtigungen mithilfe der REST-APIs finden Sie unter den folgenden Links:
    * [Java](notification-hubs-java-push-notification-tutorial.md)
    * [PHP](notification-hubs-php-push-notification-tutorial.md)

## <a name="optional-send-notifications-from-a-console-app"></a>(Optional) Senden von Benachrichtigungen aus einer Konsolenanwendung
Gehen Sie wie folgt vor, um Benachrichtigungen unter Verwendung einer .NET-Konsolenanwendung zu senden: 

1. Klicken Sie mit der rechten Maustaste auf die Projektmappe, und wählen Sie **Hinzufügen** > **Neues Projekt** aus. Wählen Sie unter **Visual C#** die Optionen **Windows** > **Konsolenanwendung** und anschließend **OK** aus.
   
    Der Projektmappe wird eine neue Visual C#-Konsolenanwendung hinzugefügt. Sie können das Projekt auch in einer separaten Projektmappe hinzufügen.

2. Wählen Sie in Visual Studio die Optionen **Extras** > **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.
   
    Die Paket-Manager-Konsole wird in Visual Studio geöffnet.

3. Legen Sie im Fenster der Paket-Manager-Konsole als **Standardprojekt** Ihr neues Konsolenanwendungsprojekt fest, und führen Sie dann im Konsolenfenster den folgenden Befehl aus:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Dadurch wird mithilfe des [Microsoft.Azure.NotificationHubs-NuGet-Pakets](http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) ein Verweis auf das Azure Notification Hubs-SDK hinzugefügt.
   
    ![Der Name des Standardprojekts](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)

4. Öffnen Sie die Datei „Program.cs“, und fügen Sie die folgende `using`-Anweisung hinzu:
   
        using Microsoft.Azure.NotificationHubs;

5. Fügen Sie in der Klasse **Program** folgende Methode hinzu:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
    >[!NOTE]
    >* Ersetzen Sie den Platzhalter **hub name** durch den Namen des Notification Hubs aus dem Azure-Portal. 
    >* Ersetzen Sie den Verbindungszeichenfolgen-Platzhalter durch die Verbindungszeichenfolge **DefaultFullSharedAccessSignature**, die Sie im Abschnitt „Konfigurieren Ihres Notification Hub“ über die Seite **Zugriffsrichtlinien** Ihres Notification Hubs ermittelt haben.
    >* Verwenden Sie die Verbindungszeichenfolge mit Vollzugriff (*Full*), nicht die Verbindungszeichenfolge mit Lauschzugriff (*Listen*). Die Verbindungszeichenfolge mit Abhörzugriff verfügt nicht über die Berechtigungen zum Senden von Benachrichtigungen.
   > 
   > 
6. Fügen Sie der Methode **Main** die folgenden Zeilen hinzu:
   
         SendNotificationAsync();
         Console.ReadLine();

7. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Konsolenanwendungsprojekt, und wählen Sie **Als Startprojekt festlegen** aus, um es als Startprojekt festzulegen. Drücken Sie dann**** F5, um die Anwendung auszuführen.
   
    Sie erhalten eine Popupbenachrichtigung auf allen registrierten Geräten. Durch Auswählen des Popupbanners wird die App geladen.

Sie finden alle unterstützten Nutzlasten auf MSDN in den Themen [Toast-Katalog], [Kachelkatalog] und [Übersicht über Signale].

## <a name="next-steps"></a>Nächste Schritte
In diesem einfachen Beispiel haben Sie über das Portal oder über eine Konsolen-App Übertragungsbenachrichtigungen an alle Ihre Windows-Geräte gesendet. Als Nächstes empfehlen wir das Tutorial [Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer]. Darin erfahren Sie, wie Sie Benachrichtigungen über ein ASP.NET-Back-End mithilfe von Tags an bestimmte Benutzer senden.

Wenn Sie Ihre Benutzer nach Interessengruppen unterteilen möchten, lesen Sie [Verwenden von Benachrichtigungshubs zum Senden von neuesten Nachrichten]. 

Weitere allgemeine Informationen zu Notification Hubs finden Sie im [Notification Hubs-Leitfaden](notification-hubs-push-notification-overview.md).

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Verwenden von Notification Hubs für Pushbenachrichtigungen an Benutzer]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Verwenden von Benachrichtigungshubs zum Senden von neuesten Nachrichten]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[Toast-Katalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[Kachelkatalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[Übersicht über Signale]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
 

