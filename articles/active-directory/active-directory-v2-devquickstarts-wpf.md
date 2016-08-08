<properties
	pageTitle="Azure AD v2.0: native .NET-App | Microsoft Azure"
	description="Vorgehensweise beim Erstellen einer systemeigenen .NET-App, bei der sich Benutzer sowohl mit ihrem persönlichen Microsoft-Konto als auch ihrem Geschäfts- oder Schulkonto anmelden können."
	services="active-directory"
	documentationCenter=""
	authors="dstrockis"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
  ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="05/31/2016"
	ms.author="dastrock"/>

# Hinzufügen der Anmeldung zu einer Windows-Desktop-App

Mit dem v2.0-Endpunkt können Sie schnell eine Authentifizierung zu Ihren Desktop-Apps hinzufügen, die sowohl persönliche Microsoft-Konten als auch Geschäfts-, Schul- oder Unikonten unterstützt. Dadurch kann Ihre App außerdem sicher mit einer Back-End-Web-API sowie mit [Microsoft Graph](https://graph.microsoft.io) und einigen [Office 365-Unified-APIs](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2) kommunizieren.

> [AZURE.NOTE]
	Nicht alle Szenarios und Funktionen von Azure Active Directory werden vom v2.0-Endpunkt unterstützt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um zu bestimmen, ob Sie den v2.0-Endpunkt verwenden sollten.

Für [systemeigene .NET- Apps, die auf einem Gerät ausgeführt werden](active-directory-v2-flows.md#mobile-and-native-apps), bietet Azure AD die Microsoft Identity Authentication Library (MSAL). Die einzige Aufgabe von MSAL besteht darin, Ihrer Anwendung das Abrufen von Token für den Aufruf von Webdiensten zu erleichtern. Um Ihnen zu zeigen, wie einfach das geht, wollen wir nun eine .NET-WPF-App mit einer Aufgabenliste entwickeln, die folgende Aktionen ausführt:

-	Anmelden von Benutzern und Abrufen von Zugriffstoken mit dem [OAuth 2.0-Authentifizierungsprotokoll](active-directory-v2-protocols.md#oauth2-authorization-code-flow).
-	Sicheres Aufrufen eines Back-End-ToDoList-Webdienstes, der ebenfalls durch OAuth 2.0 gesichert ist.
-	Abmelden von Benutzern

## Beispielcode herunterladen

Der Code für dieses Tutorial wird [auf GitHub](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet) verwaltet. Um folgen zu können, können Sie [das App-Gerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip) oder das Gerüst klonen:

```git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

Die fertige App wird außerdem am Ende dieses Lernprogramms bereitgestellt.

## Registrieren einer App
Erstellen Sie eine neue App unter [apps.dev.microsoft.com](https://apps.dev.microsoft.com), oder führen Sie die folgenden [ausführlichen Schritte](active-directory-v2-app-registration.md) aus. Stellen Sie sicher, dass Sie:

- die Ihrer App zugewiesene **Anwendungs-ID** kopieren. Sie benötigen Sie in Kürze.
- Fügen Sie die **Mobile**-Plattform Ihrer App hinzu.

## Installieren und Konfigurieren von MSAL
Nachdem Sie eine App bei Microsoft registriert haben, können Sie MSAL installieren und Code im Zusammenhang mit Identitätsfunktionen schreiben. Damit MSAL mit dem v2.0-Endpunkt kommunizieren kann, müssen Sie einige Informationen zur App-Registrierung bereitstellen.

-	Fügen Sie dazu zunächst MSAL mithilfe der Paket-Manager-Konsole zum TodoListClient-Projekt hinzu.

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-	Öffnen Sie im Projekt „TodoListClient“ `app.config`. Ersetzen Sie die Werte der Elemente in Abschnitt `<appSettings>` durch die Werte, die Sie im App-Registrierungsportal eingegeben haben. Sobald Ihr Code MSAL verwendet, verweist er auf diese Werte.
    -	`ida:ClientId` ist die **Anwendungs-ID** Ihrer App, die Sie aus dem Portal kopiert haben.

- Öffnen Sie im TodoList-Dienstprojekt `web.config` im Stammverzeichnis des Projekts.
    - Ersetzen Sie den Wert `ida:Audience` durch die gleiche **Anwendungs-ID** aus dem Portal.

## Verwenden von MSAL zum Abrufen von Token
Das Grundprinzip von MSAL ist wie folgt: Wann immer Ihre Anwendung ein Zugriffstoken benötigt, rufen Sie einfach `app.AcquireToken(...)` auf, und MSAL erledigt alles Weitere.

-	Öffnen Sie im Projekt `TodoListClient` die Datei `MainWindow.xaml.cs`, und suchen Sie die Methode `OnInitialized(...)`. Der erste Schritt ist die Initialisierung des `PublicClientApplication` Ihrer Anwendung – der primären Klasse von MSAL, die systemeigene Anwendungen darstellt. Dort übergeben Sie MSAL die zur Kommunikation mit Azure AD notwendigen Koordinaten und weisen es an, wie Token zwischengespeichert werden sollen.

```C#
protected override async void OnInitialized(EventArgs e)
{
		base.OnInitialized(e);

		app = new PublicClientApplication(new FileCache());
		AuthenticationResult result = null;
		...
}
```

- Wenn die App gestartet wird, sollte geprüft werden, ob der Benutzer bereits in der App angemeldet ist. Es soll jedoch noch keine Benutzeroberfläche für die Anmeldung erzeugt werden – der Benutzer soll zu diesem Zweck selbst auf "Anmelden" klicken. Außerdem in der `OnInitialized(...)`-Methode:

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- Wenn der Benutzer nicht angemeldet ist und auf die Schaltfläche "Anmelden" klickt, soll eine Anmeldebenutzeroberfläche aufgerufen werden, und der Benutzer soll dort seine Anmeldeinformationen eingeben. Implementieren Sie den Schaltflächenhandler für die Anmeldung:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
		// TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

- Wenn der Benutzer sich erfolgreich angemeldet hat, empfängt MSAL ein Token und legt es für Sie in einem Cache ab, sodass Sie problemlos mit dem Aufrufen der `GetTodoList()`-Methode fortfahren können. Zum Abrufen der Aufgaben eines Benutzers ist nur noch die Implementierung der `GetTodoList()`-Methode erforderlich.

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


		...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
		// If the user clicked the 'clear cache' button,
		// clear the MSAL token cache and show the user as signed out.
		// It's also necessary to clear the cookies from the browser
		// control so the next user has a chance to sign in.

		if (SignInButton.Content.ToString() == "Clear Cache")
		{
				TodoList.ItemsSource = string.Empty;
				app.UserTokenCache.Clear(app.ClientId);
				ClearCookies();
				SignInButton.Content = "Sign In";
				return;
		}

		...
```

## Ausführen

Glückwunsch! Sie verfügen nun über eine funktionierende .NET-WPF-App, mit der Sie Benutzer authentifizieren und Web-APIS mithilfe von OAuth 2.0 sicher aufrufen können. Führen Sie beide Projekte aus, und melden Sie sich entweder mit einem persönlichen Microsoft-Konto oder einem Geschäfts- oder Schulkonto an. Fügen Sie Aufgaben zur To-Do-Liste des Benutzers hinzu. Melden Sie sich ab, und anschließend wieder als anderer Benutzer an, um dessen To-Do-Liste anzuzeigen. Schließen Sie die Anwendung, und führen Sie sie erneut aus. Beobachten Sie dabei, wie die Sitzung des Benutzers intakt bleibt, weil die App Token in einer lokalen Datei zwischenspeichert.

MSAL vereinfacht das Übernehmen gemeinsamer Identitätsfeatures in die App, sowohl für persönliche als auch für Geschäftskonten. Es übernimmt die unangenehmen Verwaltungsarbeiten für Sie – die Cacheverwaltung, die Unterstützung des OAuth-Protokolls, die Anzeige einer Anmeldeschnittstelle für den Benutzer, die Aktualisierung abgelaufener Tokens und vieles mehr. Das Einzige, womit Sie sich noch beschäftigen müssen, ist der API-Aufruf `app.AcquireTokenAsync(...)`.

Als Referenz stellen wir das vollständige Beispiel (ohne Ihre Konfigurationswerte) [hier als ZIP-Datei bereit](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip). Sie können es alternativ aus GitHub klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## Nächste Schritte

Sie können nun mit den Themen für fortgeschrittenere Benutzer fortfahren. Wie wäre es zum Beispiel mit Folgendem:

- [Sichern der TodoListService-Web-API mit dem v2.0-Endpunkt >>](active-directory-v2-devquickstarts-dotnet-api.md)

Weitere Ressourcen:
- [Das v2.0-Entwicklerhandbuch >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow-Tag „msal“ >>](http://stackoverflow.com/questions/tagged/msal)

## Abrufen von Sicherheitsupdates für unsere Produkte

Wir empfehlen Ihnen, den Erhalt von Benachrichtigungen zu Sicherheitsvorfällen einzurichten. Rufen Sie dazu [diese Seite](https://technet.microsoft.com/security/dd252948) auf, und abonnieren Sie Sicherheitsempfehlungen.

<!---HONumber=AcomDC_0727_2016-->