---
title: "Verwenden von Azure Notification Hubs zum Übermitteln von aktuellen Nachrichten (Universelle Windows-Plattform)"
description: Verwenden Sie Azure Notification Hubs mit Tags in der Registrierung, um aktuelle Nachrichten an eine UWP-App zu senden.
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: d510e7e665adec9607aeee80802c466b363d5d5b
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="use-notification-hubs-to-send-breaking-news"></a>Verwenden von Notification Hubs zum Übermitteln von aktuellen Nachrichten
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Übersicht
In diesem Thema wird gezeigt, wie Sie mit Azure Notification Hubs Benachrichtigungen zu aktuellen Nachrichten an eine Windows Store- oder Windows Phone 8.1-App (kein Silverlight) senden können. Wenn Ihr Ziel Windows Phone 8.1 Silverlight ist, lesen Sie den Abschnitt zur [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md)-Version. 

Nachdem Sie diesen Prozess abgeschlossen haben, können Sie die Registrierung für Kategorien aktueller Nachrichten, an denen Sie interessiert sind, durchführen. Sie erhalten dann nur für diese Kategorien Pushbenachrichtigungen. Dieses Szenario kann für viele Anwendungen verwendet werden (z.B. RSS-Reader oder Apps für Musikliebhaber), bei denen Benachrichtigungen an Benutzergruppen gesendet werden müssen, die zuvor Interesse daran bekundet haben. 

Übertragungsszenarien können Sie durch das Einfügen von einem oder mehreren *Tags* ermöglichen, wenn Sie eine Registrierung im Notification Hub erstellen. Wenn Benachrichtigungen zu einem Tag gesendet werden, erhalten alle Geräte, die für das Tag registriert wurden, diese Benachrichtigung. Da es sich bei Tags um einfache Zeichenfolgen handelt, müssen diese nicht im Voraus eingerichtet werden. Weitere Informationen zu Tags finden Sie unter [Notification Hubs – Weiterleitung und Tagausdrücke](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows Store- und Windows Phone-Projekte der Version 8.1 und älter werden in Visual Studio 2017 nicht unterstützt. Weitere Informationen finden Sie unter [Visual Studio 2017 – Zielplattformen und Kompatibilität](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs). 

## <a name="prerequisites"></a>Voraussetzungen
Dieses Thema basiert auf der App, die Sie in [Erste Schritte mit Notification Hubs][get-started] erstellt haben. Bevor Sie mit diesem Tutorial beginnen, müssen Sie zunächst [Erste Schritte mit Notification Hubs][get-started] abschließen.

## <a name="add-category-selection-to-the-app"></a>Hinzufügen der Kategorieauswahl zur App
Der erste Schritt besteht darin, der vorhandenen Hauptseite Benutzeroberflächenelemente hinzuzufügen, die dem Benutzer die Auswahl der Kategorien für die Registrierung ermöglichen. Die ausgewählten Kategorien werden auf dem Gerät gespeichert. Wenn die App gestartet wird, wird eine Geräteregistrierung in Ihrem Notification Hub mit den ausgewählten Kategorien als Tags erstellt.

1. Öffnen Sie die Projektdatei „MainPage.xaml“, und kopieren Sie folgenden Code in das **Grid**-Element:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>

2. Klicken Sie mit der rechten Maustaste auf das Projekt **Shared**, und fügen Sie eine neue Klasse mit dem Namen **Notifications** und dann der Klassendefinition den Modifizierer **public** hinzu. Fügen Sie anschließend der neuen Codedatei die folgenden **using**-Anweisungen hinzu:
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;

3. Kopieren Sie den folgenden Code in die neue **Notifications**-Klasse:
   
        private NotificationHub hub;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }
   
        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }
   
    Diese Klasse verwendet den lokalen Speicher, um Nachrichtenkategorien zu speichern, die das Gerät empfangen soll. Anstelle der *RegisterNativeAsync*-Methode rufen Sie *RegisterTemplateAsync* auf, um die Registrierung für die Kategorien mithilfe einer Vorlagenregistrierung durchzuführen. 
   
    Da Sie möglicherweise mehrere Vorlagen (z.B. eine Popupbenachrichtigung und eine für Kacheln) registrieren möchten, geben Sie auch einen Vorlagennamen an (z.B. „simpleWNSTemplateExample“). Benennen Sie die Vorlagen so, dass Sie sie später aktualisieren oder löschen können.
   
    >[!NOTE]
    >Wenn ein Gerät mehrere Vorlagen mit demselben Tag registriert, werden bei einer eingehenden Nachricht für das entsprechende Tag mehrere Benachrichtigungen an das Gerät verschickt (eine pro Vorlage). Dies ist hilfreich, um dieselbe logische Nachricht in mehreren visuellen Darstellungen anzuzeigen (z.B. als Signal und als Popupbenachrichtigung in einer Windows Store-Anwendung).
   
    Weitere Informationen finden Sie unter [Vorlagen](notification-hubs-templates-cross-platform-push-messages.md).

4. Fügen Sie in der Datei "App.xaml.cs" folgende Eigenschaft zur Klasse **App** hinzu:
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    Diese Eigenschaft wird verwendet, um eine **Notifications**-Instanz zu erstellen und darauf zuzugreifen.
   
    Ersetzen Sie im Code die Platzhalter `<hub name>` und `<connection string with listen access>` durch den Namen Ihres Notification Hubs und die Verbindungszeichenfolge für *DefaultListenSharedAccessSignature*, die Sie zuvor abgerufen haben.
   
   > [!NOTE]
   > Da Anmeldenamen, die mit einer Client-App verteilt werden, normalerweise nicht sehr sicher sind, sollten Sie nur den Schlüssel für den *Abhörzugriff* mit Ihrer Client-App verteilen. Der Abhörzugriff ermöglicht der App, sich für Benachrichtigungen zu registrieren, aber es können keine vorhandenen Registrierungen geändert und keine Benachrichtigungen versendet werden. Der Vollzugriffsschlüssel wird in einem gesicherten Back-End-Dienst für das Versenden von Benachrichtigungen und das Ändern vorhandener Registrierungen verwendet.
   > 
   > 
5. Fügen Sie in der Projektdatei „MainPage.xaml.cs“ die folgende Zeile hinzu:
   
        using Windows.UI.Popups;

6. Fügen Sie in der Projektdatei "MainPage.xaml.cs" die folgende Methode hinzu:
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
   
    Diese Methode erstellt eine Liste von Kategorien und verwendet die **Notifications**-Klasse zum Speichern der Liste im lokalen Speicher. Außerdem registriert sie die entsprechenden Tags in Ihrem Notifications Hub. Wenn die Kategorien geändert werden, wird die Registrierung mit den neuen Kategorien neu erstellt.

Ihre App kann jetzt einen Satz von Kategorien im lokalen Speicher auf dem Gerät speichern. Die App registriert sich beim Notification Hub, wenn Benutzer die Kategorieauswahl ändern.

## <a name="register-for-notifications"></a>Registrieren für Benachrichtigungen
In diesem Abschnitt führen Sie die Registrierung beim Notification Hub während des Startvorgangs durch, wobei die im lokalen Speicher gespeicherten Kategorien verwendet werden.

> [!NOTE]
> Da sich der durch den Windows Notification Service (WNS) zugeteilte Kanal-URI jederzeit ändern kann, sollten Sie sich regelmäßig für Benachrichtigungen registrieren, um Benachrichtigungsfehler zu vermeiden. Dieses Beispiel registriert sich jedes Mal für Benachrichtigungen, wenn die App gestartet wird. Für häufig ausgeführte Apps (öfters als einmal täglich) können Sie die Registrierung wahrscheinlich überspringen, wenn weniger als ein Tag seit der letzten Registrierung vergangen ist, um Bandbreite einzusparen.
> 
> 

1. Öffnen Sie die Datei „App.xaml.cs“, und aktualisieren Sie die `notifications`InitNotificationsAsync**-Methode, sodass die** -Klasse für Abonnements anhand von Kategorien verwendet wird.
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    Dadurch wird sichergestellt, dass beim Start der App die Kategorien aus dem lokalen Speicher abgerufen werden und eine Registrierung für diese Kategorien angefordert wird. Die **InitNotificationsAsync**-Methode wurde im Rahmen des Tutorials [Erste Schritte mit Notification Hubs][get-started] erstellt.

2. Fügen Sie in der Projektdatei "MainPage.xaml.cs" der *OnNavigatedTo* -Methode den folgenden Code hinzu:
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }
   
    Durch diesen Code wird die Hauptseite basierend auf dem Status der zuvor gespeicherten Kategorien aktualisiert.

Die App ist nun vollständig. Sie kann einen Satz von Kategorien im lokalen Speicher des Geräts speichern, der für die Registrierung beim Notification Hub verwendet wird, wenn Benutzer die Kategorieauswahl ändern. Im nächsten Abschnitt definieren Sie ein Back-End, das Kategoriebenachrichtigungen an diese App senden kann.

## <a name="send-tagged-notifications"></a>Senden von Benachrichtigungen mit Tags
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a>Ausführen der App und Erzeugen von Benachrichtigungen
1. Drücken Sie in Visual Studio die Taste **F5**, um die App zu starten.  
    Die Benutzeroberfläche der App bietet verschiedene Umschaltmöglichkeiten, mit denen Sie die Kategorien auswählen können, die Sie abonnieren möchten. 
   
    ![App für aktuelle Nachrichten][1]

2. Aktivieren Sie eine oder mehrere Kategorien, und klicken Sie dann auf **Subscribe**.
   
    Die App konvertiert die ausgewählten Kategorien in Tags, und fordert eine neue Geräteregistrierung für die ausgewählten Tags vom Notification Hub an. Die registrierten Kategorien werden zurückgegeben und in einem Dialogfeld angezeigt.
   
    ![Umschalter für Kategorien und Schaltfläche „Subscribe“ (Abonnieren)][19]

3. Verwenden Sie eine dieser Möglichkeiten, um eine neue Benachrichtigung vom Back-End zu versenden:

   * **Konsolen-App:** Starten Sie die Konsolenanwendung.
   * **Java/PHP:** Führen Sie Ihre App/Ihr Skript aus.
     
     Die Benachrichtigungen für die ausgewählten Kategorien werden als Popupbenachrichtigungen angezeigt.
     
     ![Popupbenachrichtigungen][14]

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie aktuelle Nachrichten nach Kategorie übermittelt werden. Sie können nun eines der folgenden Tutorials durchführen, die andere komplexe Notification Hub-Szenarien veranschaulichen:

* [Verwenden von Notification Hubs zum Übermitteln von lokalisierten aktuellen Nachrichten:] In diesem Tutorial erfahren Sie, wie Sie die App für aktuelle Nachrichten erweitern, um das Senden von lokalisierten Benachrichtigungen zu ermöglichen.

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification
[Verwenden von Notification Hubs zum Übermitteln von lokalisierten aktuellen Nachrichten:]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591

