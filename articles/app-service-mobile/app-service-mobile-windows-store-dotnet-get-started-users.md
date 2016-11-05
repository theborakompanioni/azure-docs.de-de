---
title: Hinzufügen von Authentifizierung zu Ihrer App für die universelle Windows-Plattform (UWP) | Microsoft Docs
description: Erfahren Sie, wie Sie Azure Mobile App Service-Apps zum Authentifizieren von Benutzern bei Ihrer App für die universelle Windows-Plattform (UWP) verwenden. Dafür können Sie eine Vielzahl von Identitätsanbietern nutzen, z.B. AAD, Google, Facebook, Twitter und Microsoft.
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: erikre
editor: ''

ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 05/14/2016
ms.author: glenga

---
# Hinzufügen der Authentifizierung zu Ihrer Windows-App
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

In diesem Thema erfahren Sie, wie Sie Ihrer mobilen App eine cloudbasierte Authentifizierung hinzufügen können. In diesem Tutorial fügen Sie dem Schnellstartprojekt für die universelle Windows-Plattform (UWP) und Mobile Apps eine Authentifizierung hinzu. Sie verwenden dazu einen Identitätsanbieter, der von Azure App Service unterstützt wird. Nach erfolgreicher Authentifizierung und Autorisierung durch das Back-End Ihrer mobilen App wird die Benutzer-ID angezeigt.

Dieses Tutorial baut auf dem Mobile App-Schnellstart auf. Sie müssen zunächst das Tutorial [Erste Schritte mit Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md) durchlaufen.

## <a name="register"></a>Registrieren Ihrer App für die Authentifizierung und Konfigurieren von App Service
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="permissions"></a>Einschränken von Berechtigungen für authentifizierte Benutzer
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nun können Sie überprüfen, ob der anonyme Zugriff auf Ihr Back-End deaktiviert wurde. Legen Sie das UWP-App-Projekt als Startprojekt fest, stellen Sie die App bereit, und führen Sie sie aus. Überprüfen Sie, ob ein nicht behandelter Ausnahmefehler mit dem Statuscode 401 (Nicht autorisiert) angezeigt wird, nachdem die App gestartet wurde. Dies liegt daran, dass die App versucht, als nicht authentifizierter Benutzer auf den Code Ihrer mobilen App zuzugreifen, die *TodoItem*-Tabelle jetzt jedoch Authentifizierung erfordert.

Als Nächstes aktualisieren Sie die App, um Benutzer zu authentifizieren, bevor diese Ressourcen von App Service anfordern.

## <a name="add-authentication"></a>Hinzufügen von Authentifizierung zur App
1. Fügen Sie in der freigegebenen UWP-App-Projektdatei „MainPage.cs“ den folgenden Codeausschnitt zur MainPage-Klasse hinzu:
   
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);
   
                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }
   
    Dieser Code authentifiziert den Benutzer mit einer Facebook-Anmeldung. Falls Sie einen anderen Identitätsanbieter als Facebook verwenden, ändern Sie den Wert für **MobileServiceAuthenticationProvider** oben entsprechend Ihrem Anbieter.
2. Löschen Sie in der vorhandenen Methodenüberschreibung **OnNavigatedTo** den Aufruf der **ButtonRefresh\_Click**-Methode (oder der **InitLocalStoreAsync**-Methode), oder kommentieren Sie diesen Aufruf aus. Dadurch wird sichergestellt, dass die Daten erst geladen werden, nachdem der Benutzer authentifiziert wurde. Als Nächstes fügen Sie der App die Schaltfläche **Sign in** hinzu, die die Authentifizierung auslöst.
3. Fügen Sie den folgenden Codeausschnitt zur MainPage-Klasse hinzu:
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. Öffnen Sie die Projektdatei „MainPage.xaml“, suchen Sie das Element, das die Schaltfläche **Save** definiert, und ersetzen Sie es durch folgenden Code:
   
        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>
5. Drücken Sie F5, um die App auszuführen, klicken Sie auf die Schaltfläche **Sign in**, und melden Sie sich mit dem von Ihnen ausgewählten Identitätsanbieter bei der App an. Wenn Sie sich erfolgreich angemeldet haben, wird die App fehlerfrei ausgeführt, und Sie können Ihr Back-End abfragen und Daten aktualisieren.

## <a name="tokens"></a>Speichern des Authentifizierungstokens auf dem Client
Im vorhergehenden Beispiel wurde eine Standardanmeldung gezeigt, die erfordert, dass der Client bei jedem Start der App sowohl den Identitätsanbieter als auch den App Service kontaktiert. Diese Methode ist nicht nur ineffizient, sie kann auch zu nutzungsbezogenen Problemen führen, wenn eine große Anzahl von Kunden die App gleichzeitig starten sollte. Ein besserer Ansatz ist es daher, das vom App Service zurückgegebene Authentifizierungstoken zwischenzuspeichern und vor einer anbieterbasierten Anmeldung zu verwenden.

> [!NOTE]
> Unabhängig davon, ob Sie clientverwaltete oder dienstverwaltete Authentifizierung verwenden, können Sie den von App Service ausgestellten Authentifizierungstoken zwischenspeichern. In diesem Lernprogramm wird die dienstverwaltete Authentifizierung verwendet.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## Nächste Schritte
Nachdem Sie dieses einfache Tutorial zur Authentifizierung abgeschlossen haben, können Sie mit den folgenden Tutorials fortfahren:

* [Hinzufügen von Pushbenachrichtigungen zur App](app-service-mobile-windows-store-dotnet-get-started-push.md) Hier erfahren Sie, wie Sie Ihrer App die Unterstützung von Pushbenachrichtigungen hinzufügen und Ihr Mobile App-Back-End für die Verwendung von Azure Notification Hubs zum Senden von Pushbenachrichtigungen konfigurieren.
* [Aktivieren der Offlinesynchronisierung für Ihre App](app-service-mobile-windows-store-dotnet-get-started-offline-data.md) Erfahren Sie, wie Sie mithilfe eines Mobile App-Back-Ends Ihrer App Offlineunterstützung hinzufügen. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (Anzeigen, Hinzufügen und Ändern von Daten), auch wenn keine Netzwerkverbindung vorhanden ist.

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

<!---HONumber=AcomDC_0629_2016-->