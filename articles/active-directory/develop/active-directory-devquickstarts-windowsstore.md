---
title: Erste Schritte in Azure AD Windows Store | Microsoft-Dokumentation
description: "Erstellen Sie Windows Store-Apps, die für die Anmeldung in Azure AD integriert sind und über OAuth durch Azure AD geschützte APIs aufrufen."
services: active-directory
documentationcenter: windows
author: jmprieur
manager: mbaldwin
editor: 
ms.assetid: 3b96a6d1-270b-4ac1-b9b5-58070c896a68
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 09/16/2016
ms.author: jmprieur
ms.custom: aaddev
ms.translationtype: HT
ms.sourcegitcommit: 9afd12380926d4e16b7384ff07d229735ca94aaa
ms.openlocfilehash: 6b5189dc06d7f8b0ed4426944948b904feba847e
ms.contentlocale: de-de
ms.lasthandoff: 07/15/2017

---
# <a name="integrate-azure-ad-with-windows-store-apps"></a>Integrieren von Azure AD in Windows Store-Apps
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

> [!NOTE]
> Projekte mit Windows Store 8.1 und älteren Versionen werden in Visual Studio 2017 nicht unterstützt.  Weitere Informationen finden Sie unter [Visual Studio 2017 – Zielplattformen und Kompatibilität](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

Bei der Entwicklung von Apps für den Windows Store können Sie Ihre Benutzer mit Azure Active Directory (Azure AD) einfach und problemlos über deren Active Directory-Konten authentifizieren. Durch die Integration in Azure AD kann Ihre Anwendung auf sichere Weise alle Web-APIs nutzen, die durch Azure AD geschützt sind, z.B. die Office 365-APIs oder die Azure-API.

Für Desktop-Apps aus dem Windows Store, die auf geschützte Ressourcen zugreifen müssen, bietet Azure AD die Active Directory Authentication Library (ADAL). Die einzige Aufgabe der ADAL besteht darin, der App das Abrufen von Zugriffstoken zu erleichtern. Um zu zeigen, wie einfach das geht, wird in diesem Artikel erläutert, wie die Windows Store-App „Directory Searcher“ mit folgenden Funktionen erstellt wird:

* Abrufen der Zugriffstoken zum Aufrufen der Azure AD Graph-API mit dem [OAuth 2.0-Authentifizierungsprotokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)
* Durchsuchen eines Verzeichnisses nach Benutzern mit einem bestimmten Benutzerprinzipalnamen (User Principal Name, UPN)
* Abmelden von Benutzern

## <a name="before-you-get-started"></a>Bevor Sie beginnen
* Laden Sie das [Gerüstprojekt](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip) oder [vollständige Beispiel](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) herunter. Bei beiden Downloads handelt es sich um eine Visual Studio 2015-Projektmappe.
* Außerdem benötigen Sie einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und bei dem Sie die App registrieren. Wenn Sie über noch keinen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](active-directory-howto-tenant.md).

Wenn Sie bereit sind, führen Sie die Verfahren in den folgenden drei Abschnitten aus.

## <a name="step-1-register-the-directorysearcher-app"></a>Schritt 1: Registrieren der App DirectorySearcher
Damit Ihre Anwendung Token abrufen kann, müssen Sie sie zunächst bei Ihrem Azure AD-Mandanten registrieren und ihr die Berechtigung für den Zugriff auf die Azure AD Graph-API erteilen. Das geht so:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie in der oberen Leiste auf Ihr Konto. Wählen Sie dann in der Liste **Verzeichnis** den Active Directory-Mandanten aus, bei dem Sie die App registrieren möchten.
3. Klicken Sie im linken Bereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen** aus.
5. Folgen Sie den Aufforderungen zum Erstellen einer **nativen Clientanwendung**.
  * **Name** enthält eine Beschreibung der App für Benutzer.
  * **Umleitungs-URI** ist eine Kombination aus einem Schema und einer Zeichenfolge, die Azure AD für die Rückgabe der Tokenantworten verwendet. Geben Sie fürs Erste einen Platzhalterwert ein (z.B. **http://DirectorySearcher**). Sie ersetzen diesen Wert später.
6. Nach Abschluss der Registrierung weist Azure AD der App eine eindeutige Anwendungs-ID zu. Kopieren Sie den Wert auf der Registerkarte **Anwendung**, da Sie ihn später benötigen.
7. Wählen Sie auf der Seite **Einstellungen** zunächst **Erforderliche Berechtigungen** und dann **Hinzufügen** aus.
8. Für die App **Azure Active Directory** wählen Sie **Microsoft Graph** als API aus.
9. Fügen Sie unter **Delegierte Berechtigungen** die Berechtigung **Als der angemeldete Benutzer auf das Verzeichnis zugreifen** hinzu. Mit dieser Berechtigung kann die App die Graph-API auf Benutzer abfragen.

## <a name="step-2-install-and-configure-adal"></a>Schritt 2: Installieren und Konfigurieren von ADAL
Nachdem Sie nun eine App in Azure AD erstellt haben, können Sie die ADAL installieren und Ihren identitätsbezogenen Code schreiben. Damit die ADAL mit Azure AD kommunizieren kann, müssen Sie ihr einige Informationen zur App-Registrierung bereitstellen.

1. Fügen Sie die ADAL mithilfe der Paket-Manager-Konsole dem Projekt „DirectorySearcher“ hinzu.

    ```
    PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

2. Öffnen Sie im Projekt „DirectorySearcher“ die Datei „MainPage.xaml.cs“.
3. Ersetzen Sie die Werte im Abschnitt **Config Values** durch die Werte, die Sie im Azure-Portal eingegeben haben. Sobald Ihr Code die ADAL verwendet, verweist er auf diese Werte.
  * *tenant* ist die Domäne Ihres Azure AD-Mandanten, z.B. „contoso.onmicrosoft.com“.
  * *clientId* ist die Client-ID der App, die Sie aus dem Portal kopiert haben.
4. Sie müssen nun den Rückruf-URI für Ihre Windows Store-App ermitteln. Fügen Sie in dieser Zeile in der Methode `MainPage` einen Haltepunkt ein:
    ```
    redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
    ```
5. Erstellen Sie die Projektmappe, und stellen Sie sicher, dass alle Paketverweise wiederhergestellt werden. Falls Pakete fehlen, öffnen Sie den NuGet-Paket-Manager, und stellen Sie die Pakete wieder her.
6. Führen Sie die App aus, und kopieren Sie bei Erreichen des Haltepunkts den Wert von `redirectUri`. Der Wert sollte etwa wie folgt aussehen:

    ```
    ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
    ```

7. Fügen Sie zurück im Azure-Portal auf der Registerkarte **Einstellungen** der App einen **RedirectUri** mit dem vorherigen Wert hinzu.  

## <a name="step-3-use-adal-to-get-tokens-from-azure-ad"></a>Schritt 3: Verwenden der ADAL zum Abrufen von Token aus Azure AD
Das Grundprinzip der ADAL ist wie folgt: Wann immer die App ein Zugriffstoken benötigt, ruft sie `authContext.AcquireToken(…)` auf, und die ADAL erledigt alles Weitere.  

1. Initialisieren Sie den `AuthenticationContext` der App, bei dem es sich um die primäre ADAL-Klasse handelt. Diese Aktion übergibt an die ADAL die zur Kommunikation mit Azure AD notwendigen Koordinaten und weist sie an, wie Token zwischengespeichert werden sollen.

    ```C#
    public MainPage()
    {
        ...

        authContext = new AuthenticationContext(authority);
    }
    ```

2. Suchen Sie die `Search(...)`-Methode, die aufgerufen wird, wenn Benutzer auf der Benutzeroberfläche der App auf die Schaltfläche **Suchen** klicken. Diese Methode übergibt an die Azure AD Graph-API eine GET-Anforderung zum Abfragen von Benutzern, deren UPN mit dem angegebenen Suchbegriff beginnt. Für die Abfrage der Graph-API müssen Sie dem **Authorization**-Header der Anforderung ein Zugriffstoken hinzufügen. An dieser Stelle tritt die ADAL auf den Plan.

    ```C#
    private async void Search(object sender, RoutedEventArgs e)
    {
        ...
        AuthenticationResult result = null;
        try
        {
            result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
        }
        catch (AdalException ex)
        {
            if (ex.ErrorCode != "authentication_canceled")
            {
                ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
            }
            return;
        }
        ...
    }
    ```
    Wenn die App über einen `AcquireTokenAsync(...)`-Aufruf ein Token anfordert, versucht die ADAL, ein Token zurückzugeben, ohne den Benutzer nach seinen Anmeldeinformationen zu fragen. Wenn die ADAL feststellt, dass sich der Benutzer zum Abrufen eines Tokens anmelden muss, zeigt sie einen Anmeldedialog an, erfasst die Anmeldeinformationen des Benutzers und gibt nach erfolgreicher Authentifizierung ein Token zurück. Falls die ADAL aus welchem Grund auch immer kein Token zurückgeben kann, ist der Status von *AuthenticationResult* „Fehler“.
3. An dieser Stelle wird das zuvor abgerufene Zugriffstoken benötigt. Hängen Sie in der `Search(...)`-Methode außerdem das Token an die Graph-API-GET-Anforderung im **Authorization**-Header an:

    ```C#
    // Add the access token to the Authorization header of the call to the Graph API, and call the Graph API.
    httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

    ```
4. Mit dem Objekt `AuthenticationResult` können Sie in der App auch Informationen zum Benutzer wie die Benutzer-ID anzeigen:

    ```C#
    // Update the page UI to represent the signed-in user
    ActiveUser.Text = result.UserInfo.DisplayableId;
    ```
5. Sie können mit der ADAL den Benutzer auch wieder von der Anwendung abmelden. Wenn der Benutzer auf die Schaltfläche **Abmelden** klickt, stellen Sie sicher, dass beim nächsten Aufruf von `AcquireTokenAsync(...)` ein Anmeldebildschirm angezeigt wird. Mithilfe der ADAL ist diese Aktion so einfach wie das Leeren des Tokencaches:

    ```C#
    private void SignOut()
    {
        // Clear session state from the token cache.
        authContext.TokenCache.Clear();

        ...
    }
    ```

## <a name="whats-next"></a>Nächste Schritte
Sie haben nun eine funktionierende Windows Store-App, die Benutzer authentifizieren, Web-APIs über OAuth 2.0 sicher aufrufen und grundlegende Benutzerinformationen abfragen kann.

Sofern nicht bereits geschehen, ist es nun an der Zeit, Ihren Mandanten mit Benutzern zu füllen.
1. Führen Sie danach Ihre App „DirectorySearcher“ aus, und melden Sie sich als einer der Benutzer an.
2. Suchen Sie anhand des UPN nach anderen Benutzern.
3. Schließen Sie die Anwendung, und führen Sie sie erneut aus. Wie Sie sehen, bleibt die Benutzersitzung erhalten.
4. Melden Sie sich ab (klicken Sie mit der rechten Maustaste, um die untere Leiste anzuzeigen) und unter einem anderen Benutzernamen wieder an.

Die ADAL erleichtert Ihnen die Integration all dieser allgemeinen Identitätsfunktionen in die App. Sie übernimmt die unangenehmen Verwaltungsarbeiten für Sie, wie z.B. die Cacheverwaltung, Unterstützung des OAuth-Protokolls, Anzeige einer Anmeldeschnittstelle für den Benutzer und Aktualisierung abgelaufener Token. Sie müssen lediglich einen einzigen API-Aufruf kennen: `authContext.AcquireToken*(…)`.

Zur Referenz können Sie das [vollständige Beispiel](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip) (ohne Ihre Konfigurationswerte) herunterladen.

Sie können sich nun weiteren Identitätsszenarien zuwenden. Sehen Sie sich z.B. das [Schützen einer .NET Web-API mit Azure AD](active-directory-devquickstarts-webapi-dotnet.md) an.

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

