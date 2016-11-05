---
title: Azure Active Directory B2C | Microsoft Docs
description: Erstellen einer Windows-Desktopanwendung mit Anmeldung, Registrierung und Profilverwaltung mithilfe von Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: .net
author: dstrockis
manager: msmbaldwin
editor: ''

ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/22/2016
ms.author: dastrock

---
# Azure AD B2C: Erstellen von Windows-Desktop-Apps
Mit Azure Active Directory (Azure AD) B2C können Sie Ihren Desktop-Apps in wenigen Schritten leistungsstarke Self-Service-Features zur Identitätsverwaltung hinzufügen. In diesem Artikel erfahren Sie, wie eine .NET Windows Presentation Foundation-App (WPF) für eine Aufgabenliste erstellt wird, die Benutzerregistrierung, -anmeldung und die Verwaltung von Profilen umfasst. Die App verfügt über Unterstützung für die Registrierung und Anmeldung mit einem Benutzernamen oder einer E-Mail-Adresse. Sie bietet auch Unterstützung für die Registrierung und Anmeldung über Konten für soziale Netzwerke wie Facebook und Google.

## Erstellen eines Azure AD B2C-Verzeichnisses
Bevor Sie Azure AD B2C verwenden können, müssen Sie ein Verzeichnis oder einen Mandanten erstellen. Ein Verzeichnis ist ein Container für all Ihre Benutzer, Apps, Gruppen usw. Wenn Sie noch keines verwenden, sollten Sie [ein B2C-Verzeichnis erstellen](active-directory-b2c-get-started.md), bevor Sie die weiteren Schritte in diesem Leitfaden ausführen.

## Erstellen einer Anwendung
Als Nächstes müssen Sie in Ihrem B2C-Verzeichnis eine App erstellen. Dadurch werden Azure AD die Informationen bereitgestellt, die für die sichere Kommunikation mit Ihrer App erforderlich sind. Befolgen Sie zum Erstellen einer App [diese Anweisungen](active-directory-b2c-app-registration.md). Führen Sie folgende Schritte aus:

* Fügen Sie einen **nativen Client** in die Anwendung ein.
* Kopieren Sie den **Umleitungs-URI** `urn:ietf:wg:oauth:2.0:oob`. Dies ist die Standard-URL für dieses Codebeispiel.
* Kopieren Sie die **Anwendungs-ID**, die Ihrer App zugewiesen ist. Sie benötigen sie später.

[!INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Erstellen der Richtlinien
In Azure AD B2C wird jede Benutzeroberfläche durch eine [Richtlinie](active-directory-b2c-reference-policies.md) definiert. Dieses Codebeispiel enthält drei Benutzeroberflächen, für die Identitäten relevant sind: Registrierung, Anmeldung und Profilbearbeitung. Sie müssen für jeden Typ eine Richtlinie erstellen, wie im [Artikel mit Richtlinienreferenzen](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy) beschrieben. Beachten Sie beim Erstellen der drei Richtlinien Folgendes:

* Wählen Sie auf dem Blatt für den Identitätsanbieter **Registrierung mit Benutzer-ID** oder **E-Mail-Registrierung** aus.
* Wählen Sie den **Anzeigenamen** und andere Registrierungsattribute in der Registrierungsrichtlinie aus.
* Wählen Sie den **Anzeigenamen** und die **Objekt-ID** als Anwendungsansprüche für jede Richtlinie aus. Sie können auch andere Ansprüche auswählen.
* Notieren Sie sich die **Namen** der einzelnen Richtlinien nach ihrer Erstellung. Sie müssen das Präfix `b2c_1_` aufweisen. Sie benötigen diese Richtliniennamen später.

[!INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Nachdem Sie die drei Richtlinien erfolgreich erstellt haben, können Sie Ihre App erstellen.

## Herunterladen des Codes
Der Code für dieses Tutorial wird [auf GitHub](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet) verwaltet. Zum Erstellen des Beispiels können Sie [ein Projektgerüst als ZIP-Datei herunterladen](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip). Sie können das Gerüst auch klonen:

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

Die fertige App ist ebenfalls [als ZIP-Datei](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip) oder unter der Verzweigung `complete` im gleichen Repository verfügbar.

Nachdem Sie den Beispielcode heruntergeladen haben, öffnen Sie zum Einstieg die Visual Studio-SLN-Datei. Das `TaskClient`-Projekt ist eine WPF-Desktopanwendung, mit der der Benutzer interagiert. In diesem Tutorial wird eine Back-End-Aufgaben-Web-API aufgerufen, die in Azure gehostet und zum Speichern der Aufgabenlisten von Benutzern verwendet wird. Sie müssen die Web-API, die wir für Sie bereits ausführen, nicht selbst erstellen.

Um zu erfahren, wie eine Web-API Anforderungen mithilfe von Azure AD B2C sicher authentifiziert, können Sie sich unseren [Artikel mit den ersten Schritten für die Web-API](active-directory-b2c-devquickstarts-api-dotnet.md) ansehen.

## Ausführen von Richtlinien
Ihre App kommuniziert mit Azure AD B2C durch das Senden von Authentifizierungsnachrichten, in denen die Richtlinie, die ausgeführt werden soll, als Teil der HTTP-Anforderung angegeben wird. Für .NET-Desktopanwendungen können Sie die Vorschauversion der Microsoft Authentication Library (MSAL) zum Senden von OAuth 2.0-Authentifizierungsnachrichten, zum Ausführen von Richtlinien und zum Abrufen von Token, die Web-APIs aufrufen, verwenden.

### Installieren der MSAL
Fügen Sie die MSAL mithilfe der Paket-Manager-Konsole von Visual Studio dem `TaskClient`-Projekt hinzu.

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### Eingeben der B2C-Details
Öffnen Sie die Datei `Globals.cs`, und ersetzen Sie alle Eigenschaftswerte durch Ihre eigenen. Diese Klasse wird von `TaskClient` für Verweise auf häufig verwendete Werte verwendet.

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[!INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

### Erstellen der PublicClientApplication
Die primäre Klasse der MSAL ist `PublicClientApplication`. Mit dieser Klasse wird Ihre Anwendung im Azure AD B2C-System dargestellt. Wenn die App initialisiert wird, erstellen Sie eine Instanz von `PublicClientApplication` in `MainWindow.xaml.cs`. Diese kann im gesamten Fenster verwendet werden.

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };

    ...
```

### Initiieren eines Registrierungsflusses
Wenn sich ein Benutzer registrieren möchte, sollte ein Registrierungsfluss initiiert werden, der die Registrierungsrichtlinie verwendet, die Sie erstellt haben. Bei Verwendung der MSAL rufen Sie einfach `pca.AcquireTokenAsync(...)` auf. Die von Ihnen an `AcquireTokenAsync(...)` übergebenen Parameter legen fest, welches Token Sie erhalten, welche Richtlinie in der Authentifizierungsanforderung verwendet wird usw.

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### Initiieren eines Anmeldeflusses
Sie können einen Anmeldefluss auf die gleiche Weise wie einen Registrierungsfluss initiieren. Wenn sich ein Benutzer anmeldet, nehmen Sie den gleichen Aufruf der MSAL vor, und zwar dieses Mal mithilfe der Anmelderichtlinie:

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### Initiieren eines Profilbearbeitungsflusses
Auch in diesem Fall können Sie Ihre Profilbearbeitungsrichtlinie auf dieselbe Weise ausführen:

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

In all diesen Fällen gibt die MSAL entweder in `AuthenticationResult` ein Token zurück oder löst eine Ausnahme aus. Jedes Mal, wenn Sie ein Token von der MSAL erhalten, können Sie mit dem `AuthenticationResult.User`-Objekt die Benutzerdaten in der App (z.B. die Benutzeroberfläche) aktualisieren. ADAL speichert das Token auch für die Verwendung in anderen Teilen der Anwendung zwischen.

### Durchführen einer Prüfung auf Token beim App-Start
Sie können die MSAL auch verwenden, um den Anmeldestatus des Benutzers nachzuverfolgen. Bei dieser App soll der Benutzer auch dann angemeldet bleiben, wenn er die App schließt und dann wieder öffnet. Navigieren Sie zurück zur `OnInitialized`-Überschreibung, und verwenden Sie die `AcquireTokenSilent`-Methode von MSAL, um eine Prüfung auf zwischengespeicherte Token durchzuführen:

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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
```

## Aufrufen der Aufgaben-API
Sie haben die MSAL jetzt zum Ausführen von Richtlinien sowie zum Abrufen von Token verwendet. Wenn Sie eines dieser Token zum Aufrufen der Aufgaben-API verwenden möchten, können Sie auch die `AcquireTokenSilent`-Methode der MSAL nutzen, um eine Prüfung auf zwischengespeicherte Token durchzuführen:

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

Wenn der Aufruf von `AcquireTokenSilentAsync(...)` erfolgreich ist und ein Token im Cache gefunden wird, können Sie das Token dem `Authorization`-Header der HTTP-Anforderung hinzufügen. Die Aufgaben-Web-API nutzt diesen Header, um die Anforderung zum Lesen der Aufgabenliste eines Benutzers zu authentifizieren:

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## Abmelden des Benutzers
Abschließend können Sie mit der MSAL die Sitzung des Benutzers beenden, wenn dieser **Abmelden** auswählt. Bei Verwendung der MSAL wird dies durch das Löschen aller Token aus dem Tokencache erreicht:

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## Ausführen der Beispiel-App
Abschließend erstellen Sie das Beispiel und führen es aus. Registrieren Sie sich für die App mit einer E-Mail-Adresse oder einem Benutzernamen. Melden Sie sich ab und unter demselben Benutzer wieder an. Bearbeiten Sie das Profil dieses Benutzers. Melden Sie sich ab, und registrieren Sie sich als ein anderer Benutzer.

## Soziale Netzwerke als IDPs hinzufügen
Derzeit unterstützt die App nur die Registrierung und Anmeldung von Benutzern über **lokale Konten**. Dies sind in Ihrem B2C-Verzeichnis gespeicherte Konten, die einen Benutzernamen und ein Kennwort verwenden. Mit Azure AD B2C können Sie auch Unterstützung für andere Identitätsanbieter (IDPs) hinzufügen, ohne Ihren Code ändern zu müssen.

Um Ihrer App soziale Netzwerke als IdPs hinzuzufügen, befolgen Sie zunächst die detaillierten Anweisungen in diesen Artikeln. Sie müssen für jeden IdP, den Sie unterstützen möchten, eine Anwendung in dessen System registrieren und eine Client-ID abrufen.

* [Einrichten von Facebook als IDP](active-directory-b2c-setup-fb-app.md)
* [Einrichten von Google als IDP](active-directory-b2c-setup-goog-app.md)
* [Einrichten von Amazon als IDP](active-directory-b2c-setup-amzn-app.md)
* [Einrichten von LinkedIn als IDP](active-directory-b2c-setup-li-app.md)

Nachdem Sie Ihrem B2C-Verzeichnis die Identitätsanbieter hinzugefügt haben, müssen Sie jede der drei Richtlinien bearbeiten, um die neuen IdPs wie im [Artikel mit Richtlinienreferenzen](active-directory-b2c-reference-policies.md) beschrieben einzubeziehen. Nachdem Sie Ihre Richtlinien gespeichert haben, führen Sie die App erneut aus. Die neuen IdPs sollten als Registrierungs- und Anmeldeoptionen in allen Benutzeroberflächen für Identitäten angezeigt werden.

Sie können mit Ihren Richtlinien experimentieren und die Auswirkungen auf die Beispiel-App beobachten. Fügen Sie IdPs hinzu, oder entfernen Sie sie, bearbeiten Sie Anwendungsansprüche, oder ändern Sie Registrierungsattribute. Experimentieren Sie so lange, bis Sie die Zusammenhänge zwischen Richtlinien, Authentifizierungsanforderungen und der MSAL nachvollziehen können.

Als Referenz wird das fertige Beispiel [als ZIP-Datei bereitgestellt](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip). Sie können sie auch aus GitHub klonen:

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```

<!---HONumber=AcomDC_0727_2016-->