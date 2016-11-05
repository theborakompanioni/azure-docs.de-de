---
title: Erste Schritte der Authentifizierung für Mobile Apps in der Xamarin.Forms-App | Microsoft Docs
description: Erfahren Sie, wie Sie Mobile Apps zum Authentifizieren von Benutzern Ihrer Xamarin.Forms-App über eine Vielzahl von Identitätsanbietern nutzen können, darunter AAD, Google, Facebook, Twitter und Microsoft.
services: app-service\mobile
documentationcenter: xamarin
author: wesmc7777
manager: dwrede
editor: ''

ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 06/16/2016
ms.author: wesmc

---
# Hinzufügen der Authentifizierung zu Ihrer Xamarin.Forms-App
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## Übersicht
In diesem Thema wird die Authentifizierung von Benutzern einer mobilen App Service-App über Ihre Clientanwendung veranschaulicht. In diesem Tutorial fügen Sie dem Xamarin.Forms-Schnellstartprojekt durch Verwenden eines von App Service unterstützten Identitätsanbieters eine Authentifizierungsfunktion hinzu. Nach der erfolgreichen Authentifizierung und Autorisierung durch Ihre mobile App wird die Benutzer-ID angezeigt, und Sie erhalten Zugriff auf beschränkte Tabellendaten.

## Voraussetzungen
Wir empfehlen Ihnen, zuerst das Tutorial [Erstellen einer Xamarin.Forms-App](app-service-mobile-xamarin-forms-get-started.md) durchzuarbeiten, um für dieses Tutorial das bestmögliche Ergebnis zu erzielen. Nach Abschluss dieses Tutorials verfügen Sie über ein Xamarin.Forms-Projekt, bei dem es sich um eine TodoList-App für mehrere Plattformen handelt.

Wenn Sie das heruntergeladene Schnellstart-Serverprojekt nicht verwenden, müssen Sie Ihrem Projekt das Authentifizierungs-Erweiterungspaket hinzufügen. Weitere Informationen zu Servererweiterungspaketen finden Sie unter [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) (in englischer Sprache).

## Registrieren Ihrer App für die Authentifizierung und Konfigurieren von App Services
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## Einschränken von Berechtigungen für authentifizierte Benutzer
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## Hinzufügen der Authentifizierung zur portablen Klassenbibliothek
Mobile Apps verwendet die [LoginAsync]-Erweiterungsmethode auf dem [MobileServiceClient], um für einen Benutzer die Anmeldung mit der App Service-Authentifizierung durchzuführen. In diesem Beispiel wird ein vom Server verwalteter Authentifizierungsfluss verwendet, bei dem in der App die Anmeldeoberfläche des Anbieters angezeigt wird. Weitere Informationen finden Sie unter [Vom Server verwaltete Authentifizierung](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow). Sie können stattdessen auch die [Vom Client verwaltete Authentifizierung](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow) verwenden, um in Ihrer Produktions-App eine bessere Benutzeroberfläche bereitzustellen.

Für die Authentifizierung mit einem Xamarin.Forms-Projekt definieren Sie in der portablen Klassenbibliothek für die App eine **IAuthenticate**-Schnittstelle. Außerdem aktualisieren Sie die in der portablen Klassenbibliothek definierte Benutzeroberfläche, um die Schaltfläche **Anmelden** hinzuzufügen. Auf diese Schaltfläche klicken Benutzer, um die Authentifizierung zu starten. Nach der erfolgreichen Anmeldung werden die Daten aus dem mobilen App-Back-End geladen.

Sie müssen die **IAuthenticate**-Schnittstelle für jede Plattform implementieren, die von Ihrer App unterstützt wird.

1. Öffnen Sie in Visual Studio oder Xamarin Studio die Datei „App.cs“ im Projekt mit **Portable** im Name, also das Projekt „Portable Klassenbibliothek“. Fügen Sie anschließend die folgende `using`-Anweisung hinzu:
   
        using System.Threading.Tasks;
2. Fügen Sie in „App.cs“ die folgende `IAuthenticate`-Schnittstellendefinition unmittelbar vor der `App`-Klassendefinition hinzu.
   
        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. Fügen Sie der **App**-Klasse die folgenden statischen Member hinzu, um die Schnittstelle mit einer plattformspezifischen Implementierung zu initialisieren.
   
        public static IAuthenticate Authenticator { get; private set; }
   
        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. Öffnen Sie die Datei „TodoList.xaml“ des Projekts „Portable Klassenbibliothek“, und fügen Sie das folgende **Button**-Element im *buttonsPanel*-Layoutelement nach der vorhandenen Schaltfläche hinzu:
   
          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>
   
    Diese Schaltfläche löst die vom Server verwaltete Authentifizierung mit Ihrem mobilen App-Back-End aus.
5. Öffnen Sie die Datei „TodoList.xaml.cs“ im Projekt „Portable Klassenbibliothek“, und fügen Sie der `TodoList`-Klasse dann das folgende Feld hinzu:
   
        // Track whether the user has authenticated. 
        bool authenticated = false;
6. Ersetzen Sie die vorhandene **OnAppearing**-Methode durch den folgenden Code:
   
        protected override async void OnAppearing()
        {
            base.OnAppearing();
   
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);
   
                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }
   
    So wird sichergestellt, dass die Daten für den Dienst erst aktualisiert werden, nachdem der Benutzer authentifiziert wurde.
7. Fügen Sie der **TodoList**-Klasse den folgenden Handler für das **Clicked**-Ereignis hinzu:
   
        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();
   
            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. Speichern Sie die Änderungen, und erstellen Sie das Projekt „Portable Klassenbibliothek“ erneut, um sicherzustellen, dass es fehlerfrei ist.

## Hinzufügen der Authentifizierung zur Android-App
In diesem Abschnitt wird das Implementieren der **IAuthenticate**-Schnittstelle im Android-App-Projekt gezeigt. Überspringen Sie diesen Abschnitt, wenn Sie Android-Geräte nicht unterstützen.

1. Klicken Sie in Visual Studio oder Xamarin Studio mit der rechten Maustaste auf das Projekt **droid**, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Drücken Sie F5, um das Projekt im Debugger zu starten. Stellen Sie anschließend sicher, dass ein Ausnahmefehler mit dem Statuscode 401 (Nicht autorisiert) ausgelöst wird, nachdem die App gestartet wurde. Der Grund für den Fehler ist, dass der Zugriff nur auf autorisierte Benutzer beschränkt ist.
3. Öffnen Sie im Android-Projekt die Datei „MainActivity.cs“, und fügen Sie die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Aktualisieren Sie die **MainActivity**-Klasse wie folgt, um die **IAuthenticate**-Schnittstelle zu implementieren:
   
        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. Aktualisieren Sie die **MainActivity**-Klasse, indem Sie wie folgt ein **MobileServiceUser**-Feld und eine **Authenticate**-Methode hinzufügen, die für die **IAuthenticate**-Schnittstelle erforderlich ist:
   
        // Define a authenticated user.
        private MobileServiceUser user;
   
        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }
   
            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();
   
            return success;
        }

    Falls Sie einen anderen Identitätsanbieter als Facebook verwenden, müssen Sie für [MobileServiceAuthenticationProvider] einen anderen Wert auswählen.

1. Fügen Sie der **OnCreate**-Methode der **MainActivity**-Klasse vor dem Aufruf von `LoadApplication()` den folgenden Code hinzu:
   
        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);
   
    So wird sichergestellt, dass der Authentifikator vor dem Laden der App initialisiert wird.
2. Erstellen Sie die App erneut, und führen Sie sie aus. Melden Sie sich dann über den ausgewählten Authentifizierungsanbieter an, und stellen Sie sicher, dass Sie als authentifizierter Benutzer auf Daten zugreifen können.

## Hinzufügen der Authentifizierung zur iOS-App
In diesem Abschnitt wird das Implementieren der **IAuthenticate**-Schnittstelle im iOS-App-Projekt gezeigt. Überspringen Sie diesen Abschnitt, wenn Sie iOS-Geräte nicht unterstützen.

1. Klicken Sie in Visual Studio oder Xamarin Studio mit der rechten Maustaste auf das **iOS**-Projekt, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Drücken Sie F5, um das Projekt im Debugger zu starten. Stellen Sie anschließend sicher, dass ein Ausnahmefehler mit dem Statuscode 401 (Nicht autorisiert) ausgelöst wird, nachdem die App gestartet wurde. Der Grund für den Fehler ist, dass der Zugriff nur auf autorisierte Benutzer beschränkt ist.
3. Öffnen Sie die Datei „AppDelegate.cs“ im iOS-Projekt, und fügen Sie die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Aktualisieren Sie die **AppDelegate**-Klasse wie folgt, um die **IAuthenticate**-Schnittstelle zu implementieren:
   
        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. Aktualisieren Sie die **AppDelegate**-Klasse, indem Sie wie folgt ein **MobileServiceUser**-Feld und eine **Authenticate**-Methode hinzufügen, die für die **IAuthenticate**-Schnittstelle erforderlich ist:
   
        // Define a authenticated user.
        private MobileServiceUser user;
   
        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }
   
            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         
   
            return success;
        }
   
    Falls Sie einen anderen Identitätsanbieter als Facebook verwenden, müssen Sie für [MobileServiceAuthenticationProvider] einen anderen Wert auswählen.
6. Fügen Sie der **FinishedLaunching**-Methode vor dem Aufruf von `LoadApplication()` die folgende Codezeile hinzu:
   
        App.Init(this);
   
    So wird sichergestellt, dass der Authentifikator vor dem Laden der App initialisiert wird.
7. Erstellen Sie die App erneut, und führen Sie sie aus. Melden Sie sich dann über den ausgewählten Authentifizierungsanbieter an, und stellen Sie sicher, dass Sie als authentifizierter Benutzer auf Daten zugreifen können.

## Hinzufügen von Authentifizierung zu Windows-App-Projekten
Dieser Abschnitt veranschaulicht das Implementieren der **IAuthenticate**-Schnittstelle in Windows 8.1- und Windows Phone 8.1-App-Projekten. Die gleichen Schritte gelten auch für UWP-Projekte (Universelle Windows-Plattform). Überspringen Sie diesen Abschnitt, wenn Sie Windows-Geräte nicht unterstützen.

1. Klicken Sie in Visual Studio mit der rechten Maustaste entweder auf das Projekt **WinApp** oder **WinPhone81**, und klicken Sie dann auf **Als Startprojekt festlegen**.
2. Drücken Sie F5, um das Projekt im Debugger zu starten. Stellen Sie anschließend sicher, dass ein Ausnahmefehler mit dem Statuscode 401 (Nicht autorisiert) ausgelöst wird, nachdem die App gestartet wurde. Der Grund für den Fehler ist, dass der Zugriff nur auf autorisierte Benutzer beschränkt ist.
3. Öffnen Sie die Datei „MainPage.xaml.cs“ für das Windows-App-Projekt, und fügen Sie die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;
   
    Ersetzen Sie `<your_Portable_Class_Library_namespace>` durch den Namespace für Ihre portable Klassenbibliothek.
4. Aktualisieren Sie die **MainPage**-Klasse wie folgt, um die **IAuthenticate**-Schnittstelle zu implementieren:
   
        public sealed partial class MainPage : IAuthenticate
5. Aktualisieren Sie die **MainPage**-Klasse, indem Sie wie folgt ein **MobileServiceUser**-Feld und eine **Authenticate**-Methode hinzufügen, die für die **IAuthenticate**-Schnittstelle erforderlich ist:
   
        // Define a authenticated user.
        private MobileServiceUser user;
   
        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;
   
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
   
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }
   
            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();
   
            return success;
        }

    Falls Sie einen anderen Identitätsanbieter als Facebook verwenden, müssen Sie für [MobileServiceAuthenticationProvider] einen anderen Wert auswählen.

1. Fügen Sie im Konstruktor für die **MainPage**-Klasse vor dem Aufruf von `LoadApplication()` die folgende Codezeile hinzu:
   
        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
   
    Ersetzen Sie `<your_Portable_Class_Library_namespace>` durch den Namespace für Ihre portable Klassenbibliothek. Wenn dies das WinApp-Projekt ist, können Sie mit Schritt 8 fortfahren. Der nächste Schritt gilt nur für das WinPhone81-Projekt, für das Sie den Anmelderückruf durchführen müssen.
2. (Optional) Öffnen Sie im App-Projekt **WinPhone81** die Datei „App.xaml.cs“, und fügen Sie die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;
   
    Ersetzen Sie `<your_Portable_Class_Library_namespace>` durch den Namespace für Ihre portable Klassenbibliothek.
3. Fügen Sie der **App**-Klasse die folgende **OnActivated**-Methodenüberschreibung hinzu:
   
       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);
   
           // We just need to handle activation that occurs after web authentication. 
           if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
           {
               // Get the client and call the LoginComplete method to complete authentication.
               var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
               client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
           }
       }
   
   Wenn die Methodenüberschreibung bereits vorhanden ist, fügen Sie einfach den bedingten Code aus dem obigen Codeausschnitt hinzu.
4. Erstellen Sie die App erneut, und führen Sie sie aus. Melden Sie sich dann über den ausgewählten Authentifizierungsanbieter an, und stellen Sie sicher, dass Sie als authentifizierter Benutzer auf Daten zugreifen können.

## Nächste Schritte
Nachdem Sie dieses einfache Tutorial zur Authentifizierung abgeschlossen haben, können Sie mit den folgenden Tutorials fortfahren:

* [Hinzufügen von Pushbenachrichtigungen zur App](app-service-mobile-xamarin-forms-get-started-push.md) Hier erfahren Sie, wie Sie Ihrer App die Unterstützung von Pushbenachrichtigungen hinzufügen und Ihr Mobile App-Back-End für die Verwendung von Azure Notification Hubs zum Senden von Pushbenachrichtigungen konfigurieren.
* [Aktivieren der Offlinesynchronisierung für Ihre App](app-service-mobile-xamarin-forms-get-started-offline-data.md) Erfahren Sie, wie Sie mithilfe eines Mobile App-Back-Ends Ihrer App Offlineunterstützung hinzufügen. Die Offlinesynchronisierung ermöglicht Endbenutzern die Interaktion mit einer mobilen App (Anzeigen, Hinzufügen und Ändern von Daten), auch wenn keine Netzwerkverbindung vorhanden ist.

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

<!---HONumber=AcomDC_0629_2016-->