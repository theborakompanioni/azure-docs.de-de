---
title: Erste Schritte in Azure AD Xamarin | Microsoft-Dokumentation
description: "In diesem Thema erfahren Sie, wie eine Xamarin-Anwendung erstellt wird, die sich für die Anmeldung in Azure AD integriert und über OAuth durch Azure AD geschützte APIs aufruft."
services: active-directory
documentationcenter: xamarin
author: dstrockis
manager: mbaldwin
editor: 
ms.assetid: 198cd2c3-f7c8-4ec2-b59d-dfdea9fe7d95
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 01/07/2017
ms.author: dastrock
translationtype: Human Translation
ms.sourcegitcommit: c579135f798ea0c2a5461fdd7c88244d2d6d78c6
ms.openlocfilehash: 4852939cb577d0b41a606f0d94897edfb367c4d6


---
# <a name="integrate-azure-ad-into-a-xamarin-app"></a>Integrieren von Azure AD in eine Xamarin-Anwendung
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Mithilfe von Xamarin können Sie mobile Apps in C# schreiben, die unter iOS, Android und Windows (sowohl mobile Geräte als auch PCs) ausgeführt werden können. Bei der Entwicklung einer Anwendung mit Xamarin können Sie Ihre Benutzer mit Azure AD einfach und problemlos über deren Active Directory-Konten authentifizieren. Außerdem kann Ihre Anwendung damit auf sichere Weise alle Web-APIs nutzen, die per Azure AD geschützt sind, z. B. die Office 365-APIs oder die Azure-API.

Für Xamarin-Apps, die auf geschützte Ressourcen zugreifen müssen, bietet Azure AD die Active Directory-Authentifizierungsbibliothek (ADAL). Die einzige Aufgabe von ADAL besteht darin, Ihrer Anwendung das Abrufen von Zugriffstoken zu erleichtern. Wir wollen Ihnen nun zeigen, wie einfach das geht. Dazu erstellen wir die Anwendung „DirectorySearcher“ mit folgenden Funktionen:

* Ausführung unter iOS, Android, Windows Desktop, Windows Phone und Windows Store
* Verwendung einer einzigen portablen Klassenbibliothek (PCL) zur Authentifizierung der Benutzer und zum Abrufen der Tokens für die Azure AD Graph-API
* Durchsuchen eines Verzeichnisses nach Benutzern mit einem bestimmten UPN

Zur Entwicklung der vollständigen Arbeitsanwendung müssen Sie folgende Schritte ausführen:

1. Einrichten der Xamarin-Entwicklungsumgebung
2. Registrieren Ihrer Anwendung bei Azure AD
3. Installieren und Konfigurieren von ADAL
4. Verwenden von ADAL zum Abrufen von Tokens aus Azure AD

Beginnen Sie, indem Sie [ein Projektgerüst](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip) oder [das vollständige Beispiel herunterladen](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip). Bei beidem handelt es sich um eine Visual Studio 2013-Lösung. Außerdem benötigen Sie einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und Ihre Anwendung registrieren können. Wenn Sie über noch keinen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](active-directory-howto-tenant.md).

## <a name="0-set-up-your-xamarin-development-environment"></a>0. Einrichten der Xamarin-Entwicklungsumgebung
Da in diesem Tutorial Projekte für iOS, Android und Windows erstellt werden, benötigen Sie sowohl Visual Studio als auch Xamarin. Befolgen Sie zum Erstellen der erforderlichen Umgebung alle Schritte in der Anleitung [Setup und Installation von Visual Studio und Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) auf MSDN. In dieser Anleitung finden Sie auch Materialien mit weiteren Informationen zu Xamarin, die Sie während der Installation lesen können. 

Nach Abschluss der erforderlichen Konfigurationsschritte öffnen Sie die Lösung in Visual Studio, um zu beginnen. Sie finden dort sechs Projekte: fünf plattformspezifische Projekte und die portable Klassenbibliothek `DirectorySearcher.cs`, die auf allen Plattformen verwendet werden kann.

## <a name="1-register-the-directory-searcher-application"></a>1. Registrieren der Anwendung DirectorySearcher
Damit Ihre Anwendung Tokens abrufen kann, müssen Sie sie zunächst beim Azure AD-Mandanten registrieren und ihr die Berechtigung für den Zugriff auf die Azure AD Graph-API erteilen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie oben auf der Leiste auf Ihr Konto, und wählen Sie in der Liste **Verzeichnis** den Active Directory-Mandanten aus, für den Sie Ihre Anwendungen registrieren möchten.
3. Klicken Sie im linken Navigationsbereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen**.
5. Folgen Sie den Bildschirmaufforderungen, und erstellen Sie eine neue **systemeigene Clientanwendung**.
  * Am **Namen** der Anwendung sollten die Endbenutzer die Funktion der Anwendung ablesen können.
  * Die **Umleitungs-URI** ist eine Kombination aus einem Schema und einer Zeichenfolge, die Azure AD für die Rückgabe der Tokenantworten verwendet. Geben Sie einen Wert ein, zum Beispiel `http://DirectorySearcher`.
6. Nach Abschluss der Registrierung weist AAD Ihrer App eine eindeutige Anwendungs-ID zu. Diesen Wert benötigen Sie in den nächsten Abschnitten. Daher sollten Sie ihn von der Registerkarte „Anwendung“ kopieren.
7. Wählen Sie auf der Seite **Einstellungen** zunächst **Erforderliche Berechtigungen** und dann **Hinzufügen** aus. Wählen Sie als API **Microsoft Graph** aus, und fügen Sie unter **Delegierte Berechtigungen** die Berechtigung **Verzeichnisdaten lesen** hinzu.  Mit dieser Berechtigung kann die Anwendung die Graph-API nach Benutzern abfragen.

## <a name="2-install--configure-adal"></a>2. Installieren und Konfigurieren von ADAL
Nachdem Sie nun eine Anwendung in Azure AD erstellt haben, können Sie ADAL installieren und Ihren identitätsbezogenen Code schreiben. Damit ADAL mit dem Azure AD kommunizieren kann, müssen Sie einige Informationen zur App-Registrierung bereitstellen.

* Fügen Sie zunächst über die Paket-Manager-Konsole ADAL zu jedem Projekt in der Projektmappe hinzu.

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

* Beachten Sie, dass jedem Projekt zwei Bibliotheksverweise hinzugefügt werden – der PCL-Teil von ADAL und ein plattformspezifischer Teil.
* Öffnen Sie `DirectorySearcher.cs`im Projekt DirectorySearcherLib. Ersetzen Sie die Klassenmember-Werte durch die Werte, die Sie im Azure-Portal eingegeben haben. Wenn die ADAL in Ihrem Code verwendet wird, verweist er auf diese Werte.
  
  * `tenant` ist die Domäne Ihres Azure AD-Mandanten, z. B. „contoso.onmicrosoft.com“.
  * `clientId` ist die Client-ID Ihrer Anwendung, die Sie aus dem Portal kopiert haben.
  * `returnUri` ist die Umleitungs-URI, die Sie im Portal eingegeben haben, z. B. `http://DirectorySearcher`.

## <a name="3-use-adal-to-get-tokens-from-aad"></a>3. Verwenden von ADAL zum Abrufen von Token aus AAD
*Nahezu* die gesamte Authentifizierungslogik der App befindet sich in `DirectorySearcher.SearchByAlias(...)`. In den plattformspezifischen Projekten müssen Sie der PCL von `DirectorySearcher` daher nur noch einen kontextbezogenen Parameter übergeben.

* Öffnen Sie zuerst `DirectorySearcher.cs`, und fügen Sie einen neuen Parameter zur `SearchByAlias(...)`-Methode hinzu. `IPlatformParameters` .

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

* Als Nächstes initialisieren Sie den `AuthenticationContext` – die primäre Klasse von ADAL. Dort übergeben Sie ADAL die zur Kommunikation mit Azure AD notwendigen Koordinaten. Rufen Sie danach `AcquireTokenAsync(...)` auf, das das Objekt `IPlatformParameters` akzeptiert und die Authentifizierungsroutine aufruft, die erforderlich ist, damit die Anwendung ein Token erhält.

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
* `AcquireTokenAsync(...)` versucht zunächst, ein Token für die angeforderte Ressource (in diesem Fall die Graph-API) zurückzugeben, ohne den Benutzer zur Eingabe seiner Anmeldeinformationen aufzufordern (die Authentifizierung erfolgt dann über den Cache oder durch Aktualisierung alter Tokens). Die Anmeldeseite von Azure AD wird nur angezeigt, wenn sich dies zum Abrufen des angeforderten Tokens nicht vermeiden lässt.
* Sie können das Zugriffstoken dann der Graph-API-Anforderung im Autorisierungsheader anfügen:

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

Mehr ist für die PCL von `DirectorySearcher` und den identitätsbezogenen Code Ihrer Anwendung nicht zu tun.  Sie brauchen nun in den Ansichten der einzelnen Plattformen nur noch die `SearchByAlias(...)` -Methode aufzurufen und gegebenenfalls Code für die korrekte Handhabung des UI-Lebenszyklus hinzuzufügen.

#### <a name="android"></a>Android:
* Fügen Sie in `MainActivity.cs` im unteren Klick-Handler einen `SearchByAlias(...)`-Aufruf hinzu:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
* Außerdem müssen Sie die Lebenszyklus-Methode für `OnActivityResult` überschreiben, wenn Authentifizierungsumleitungen zurück an die betreffende Methode geleitet werden sollen.  In Android stellt ADAL hierfür eine Hilfsmethode bereit:

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

#### <a name="windows-desktop"></a>Windows Desktop:
* Stellen Sie in `MainWindow.xaml.cs` einfach einen Aufruf an `SearchByAlias(...)`, der im `PlatformParameters`-Objekt des Desktops ein `WindowInteropHelper` übergibt:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="ios"></a>iOS:
* In `DirSearchClient_iOSViewController.cs` nimmt das `PlatformParameters`-Objekt von iOS lediglich einen Verweis auf den Ansichtscontroller an:

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

#### <a name="windows-universal"></a>Windows Universal:
* Öffnen Sie in Windows Universal die Datei `MainPage.xaml.cs`, und implementieren Sie die Methode `Search`, die in einem freigegebenen Projekt zur bedarfsgerechten Aktualisierung der Benutzeroberfläche eine Hilfsmethode verwendet.

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

Glückwunsch! Damit haben Sie eine funktionsfähige Xamarin-Anwendung entwickelt, die Benutzer auf fünf verschiedenen Plattformen authentifizieren und Web-APIs über OAuth 2.0 sicher aufrufen kann. Sofern nicht bereits geschehen, ist es nun an der Zeit, Ihren Mandanten mit Benutzern zu füllen. Führen Sie danach Ihre DirectorySearcher-Anwendung aus, und melden Sie sich unter einem dieser Benutzer an. Suchen Sie anhand des UPN nach anderen Benutzern.

ADAL erleichtert Ihnen die Integration allgemeiner Identitätsfunktionen in Ihre App. Es übernimmt die unangenehmen Verwaltungsarbeiten für Sie – die Cacheverwaltung, die Unterstützung des OAuth-Protokolls, die Anzeige einer Anmeldeschnittstelle für den Benutzer, die Aktualisierung abgelaufener Tokens und vieles mehr. Das Einzige, womit Sie sich noch beschäftigen müssen, ist der API-Aufruf `authContext.AcquireToken*(…)`.

Als Referenz stellen wir [hier](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip)das vollständige Beispiel (ohne Ihre Konfigurationswerte) bereit. Sie können sich nun weiteren Identitätsszenarien zuwenden. Wie wäre es zum Beispiel mit Folgendem:

[Schützen einer .NET Web-API mit Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]




<!--HONumber=Jan17_HO3-->


