---
title: "Azure AD v2 – Windows-Desktop: Erste Schritte – Verwendung | Microsoft-Dokumentation"
description: "Informationen, wie Windows Desktop .NET (XAML)-Anwendungen eine API aufrufen können, die Zugriffstoken vom Azure Active Directory v2-Endpunkt anfordert"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 4c0e6cd7ec4a91040af588a406fbad8b8c1607e9
ms.contentlocale: de-de


---

## <a name="use-the-microsoft-authentication-library-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Verwenden der Microsoft Authentication Library (MSAL) zum Abrufen eines Tokens für die Microsoft Graph-API

Dieser Abschnitt zeigt, wie Sie die MSAL verwenden, um ein Token für die Microsoft Graph-API abzurufen.

1.    Fügen Sie in `MainWindow.xaml.cs` den Verweis auf die MSAL-Bibliothek zur Klasse hinzu:

```csharp
using Microsoft.Identity.Client;
```
<!-- Workaround for Docs conversion bug -->
<ol start="2">
<li>
Ersetzen Sie den Code der <code>MainWindow</code>-Klasse durch:
</li>
</ol>

```csharp
public partial class MainWindow : Window
{
    //Set the API Endpoint to Graph 'me' endpoint
    string _graphAPIEndpoint = "https://graph.microsoft.com/v1.0/me";

    //Set the scope for API call to user.read
    string[] _scopes = new string[] { "user.read" };


    public MainWindow()
    {
        InitializeComponent();
    }

    /// <summary>
    /// Call AcquireTokenAsync - to acquire a token requiring user to sign-in
    /// </summary>
    private async void CallGraphButton_Click(object sender, RoutedEventArgs e)
    {
        AuthenticationResult authResult = null;

        try
        {
            authResult = await App.PublicClientApp.AcquireTokenSilentAsync(_scopes, App.PublicClientApp.Users.FirstOrDefault());
        }
        catch (MsalUiRequiredException ex)
        {
            // A MsalUiRequiredException happened on AcquireTokenSilentAsync. This indicates you need to call AcquireTokenAsync to acquire a token
            System.Diagnostics.Debug.WriteLine($"MsalUiRequiredException: {ex.Message}");

            try
            {
                authResult = await App.PublicClientApp.AcquireTokenAsync(_scopes);
            }
            catch (MsalException msalex)
            {
                ResultText.Text = $"Error Acquiring Token:{System.Environment.NewLine}{msalex}";
            }
        }
        catch (Exception ex)
        {
            ResultText.Text = $"Error Acquiring Token Silently:{System.Environment.NewLine}{ex}";
            return;
        }

        if (authResult != null)
        {
            ResultText.Text = await GetHttpContentWithToken(_graphAPIEndpoint, authResult.AccessToken);
            DisplayBasicTokenInfo(authResult);
            this.SignOutButton.Visibility = Visibility.Visible;
        }
    }
}
```

<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen
#### <a name="getting-a-user-token-interactive"></a>Interaktives Abrufen eines Benutzertokens
Das Aufrufen der `AcquireTokenAsync`-Methode führt zum Öffnen eines Fensters, in dem der Benutzer aufgefordert wird, sich anzumelden. Anwendungen erfordern in der Regel, dass sich ein Benutzer interaktiv anmeldet, wenn er erstmals auf eine geschützte Ressource zugreifen muss oder ein automatischer Vorgang zum Beziehen eines Tokens misslingt (wenn z.B. das Kennwort des Benutzers abgelaufen ist).

#### <a name="getting-a-user-token-silently"></a>Automatisches Abrufen eines Benutzertokens
`AcquireTokenSilentAsync` verarbeitet das Beziehen und Verlängern von Token ohne Eingreifen des Benutzers. Nachdem `AcquireTokenAsync` zum ersten Mal ausgeführt wurde, ist `AcquireTokenSilentAsync` die übliche Methode zum Abrufen von Token, die für den Zugriff auf geschützte Ressourcen bei nachfolgenden Aufrufen verwendet werden, da Aufrufe zum Anfordern oder Verlängern von Token automatisch erfolgen.
Irgendwann schlägt `AcquireTokenSilentAsync` fehl, z.B. wenn sich der Benutzer abgemeldet oder sein Kennwort auf einem anderen Gerät geändert hat. Wenn die MSAL feststellt, dass das Problem durch Anfordern einer interaktiven Aktion gelöst werden kann, löst sie `MsalUiRequiredException` aus. Ihre Anwendung kann diese Ausnahme auf zwei Arten handhaben:

1.    Sofortiges Aufrufen von `AcquireTokenAsync`, was dazu führt, dass der Benutzer zur Anmeldung aufgefordert wird. Dieses Muster wird in der Regel in Onlineanwendungen verwendet, in denen kein Offline-Inhalt in der Anwendung für den Benutzer verfügbar ist. Das von diesem geführten Setup generierte Beispiel folgt diesem Muster. Sie können es in Aktion sehen, wenn Sie das Beispiel erstmals ausführen. Da kein Benutzer die Anwendung bisher verwendet hat, enthält `PublicClientApp.Users.FirstOrDefault()` einen NULL-Wert, und die Ausnahme `MsalUiRequiredException` wird ausgelöst. Der Code im Beispiel behandelt dann die Ausnahme durch Aufrufen von `AcquireTokenAsync`, was dazu führt, dass der Benutzer zur Anmeldung aufgefordert wird.

2.    Anwendungen können dem Benutzer auch visuell zu verstehen geben, dass eine interaktive Anmeldung erforderlich ist, damit der Benutzer den richtigen Zeitpunkt zum Anmelden wählen oder die Anwendung `AcquireTokenSilentAsync` zu einem späteren Zeitpunkt wiederholen kann. Dies wird normalerweise verwendet, wenn der Benutzer andere Funktionen der Anwendung nutzen kann, ohne unterbrochen zu werden, z.B. wenn Offline-Inhalt in der Anwendung verfügbar ist. In diesem Fall kann der Benutzer entscheiden, wann er sich für den Zugriff auf die geschützte Ressource anmelden oder die veralteten Informationen aktualisieren möchte. Ihre Anwendung kann sich auch für einen Wiederholungsversuch von `AcquireTokenSilentAsync` entscheiden, wenn das Netzwerk wiederhergestellt wird, nachdem es vorübergehend nicht verfügbar war.
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-using-the-token-you-just-obtained"></a>Aufrufen der Microsoft Graph-API mit dem zuvor bezogenen Token

1. Fügen Sie die nachstehende neue Methode Ihrer Datei `MainWindow.xaml.cs` hinzu. Die Methode dient zum Richten einer `GET`-Anforderung an die Graph-API mithilfe eines „Authorize“-Headers:

```csharp
/// <summary>
/// Perform an HTTP GET request to a URL using an HTTP Authorization header
/// </summary>
/// <param name="url">The URL</param>
/// <param name="token">The token</param>
/// <returns>String containing the results of the GET operation</returns>
public async Task<string> GetHttpContentWithToken(string url, string token)
{
    var httpClient = new System.Net.Http.HttpClient();
    System.Net.Http.HttpResponseMessage response;
    try
    {
        var request = new System.Net.Http.HttpRequestMessage(System.Net.Http.HttpMethod.Get, url);
        //Add the token in Authorization header
        request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", token);
        response = await httpClient.SendAsync(request);
        var content = await response.Content.ReadAsStringAsync();
        return content;
    }
    catch (Exception ex)
    {
        return ex.ToString();
    }
}
```
<!--start-collapse-->
### <a name="more-information-on-making-a-rest-call-against-a-protected-api"></a>Weitere Informationen zum Richten eines REST-Aufrufs an eine geschützte API

In dieser Beispielanwendung dient die `GetHttpContentWithToken`-Methode zum Richten einer HTTP `GET`-Anforderung an eine geschützte Ressource, die ein Token erfordert, und anschließenden Zurückgeben des Inhalts an den Aufrufer. Diese Methode fügt das abgerufene Token in den *HTTP-Autorisierungsheader* ein. In diesem Beispiel ist die Ressource der Endpunkt *me* der Microsoft Graph-API, der die Profilinformationen des Benutzers anzeigt.
<!--end-collapse-->

## <a name="add-a-method-to-sign-out-the-user"></a>Hinzufügen einer Methode zum Abmelden des Benutzers

1. Fügen Sie die folgende Methode Ihrer Datei `MainWindow.xaml.cs` zum Abmelden des Benutzers hinzu:

```csharp
/// <summary>
/// Sign out the current user
/// </summary>
private void SignOutButton_Click(object sender, RoutedEventArgs e)
{
    if (App.PublicClientApp.Users.Any())
    {
        try
        {
            App.PublicClientApp.Remove(App.PublicClientApp.Users.FirstOrDefault());
            this.ResultText.Text = "User has signed-out";
            this.CallGraphButton.Visibility = Visibility.Visible;
            this.SignOutButton.Visibility = Visibility.Collapsed;
        }
        catch (MsalException ex)
        {
            ResultText.Text = $"Error signing-out user: {ex.Message}";
        }
    }
}
```
<!--start-collapse-->
### <a name="more-info-on-sign-out"></a>Weitere Informationen zum Abmelden

`SignOutButton_Click` entfernt den Benutzer aus dem MSAL-Benutzercache. Dadurch wird die MSAL effektiv angewiesen, den aktuellen Benutzer zu vergessen, sodass eine künftige Anforderung zum Abrufen eines Tokens nur erfolgreich ist, wenn diese interaktiv gestaltet wird.
Obwohl die Anwendung in diesem Beispiel einen einzelnen Benutzer unterstützt, werden von der MSAL Szenarien unterstützt, in denen mehrere Konten gleichzeitig angemeldet werden können. Ein Beispiel ist eine E-Mail-Anwendung, in der ein Benutzer mehrere Konten hat.
<!--end-collapse-->

## <a name="display-basic-token-information"></a>Anzeigen grundlegender Informationen zum Token

1. Fügen Sie die folgende Methode Ihrer Datei `MainWindow.xaml.cs` hinzu, um grundlegende Informationen zum Token anzuzeigen:

```csharp
/// <summary>
/// Display basic information contained in the token
/// </summary>
private void DisplayBasicTokenInfo(AuthenticationResult authResult)
{
    TokenInfoText.Text = "";
    if (authResult != null)
    {
        TokenInfoText.Text += $"Name: {authResult.User.Name}" + Environment.NewLine;
        TokenInfoText.Text += $"Username: {authResult.User.DisplayableId}" + Environment.NewLine;
        TokenInfoText.Text += $"Token Expires: {authResult.ExpiresOn.ToLocalTime()}" + Environment.NewLine;
        TokenInfoText.Text += $"Access Token: {authResult.AccessToken}" + Environment.NewLine;
    }
}
```
<!--start-collapse-->
### <a name="more-information"></a>Weitere Informationen

Über *OpenID Connect* abgerufene Token enthalten auch eine kleine Teilmenge von Informationen zum Benutzer. `DisplayBasicTokenInfo` zeigt grundlegende Informationen im Token: beispielsweise den Anzeigenamen und die ID des Benutzers sowie das Ablaufdatum des Tokens und die Zeichenfolge, die das Zugriffstoken selbst darstellt. Diese Informationen werden Ihnen angezeigt. Sie können mehrmals auf die Schaltfläche *Microsoft Graph-API* klicken, um zu erkennen, dass dasselbe Token für nachfolgende Anforderungen wiederverwendet wurde. Sie können auch feststellen, dass das Ablaufdatum verlängert wurde, wenn die MSAL entscheidet, dass es Zeit ist, das Token zu verlängern.
<!--end-collapse-->


