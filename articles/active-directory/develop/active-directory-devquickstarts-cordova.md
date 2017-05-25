---
title: "Azure AD Cordova – Erste Schritte | Microsoft-Dokumentation"
description: "In diesem Thema erfahren Sie, wie Sie eine Cordova-Anwendung erstellen, die sich für die Anmeldung in Azure AD integriert und unter Verwendung von OAuth APIs aufruft, die durch Azure AD geschützt sind."
services: active-directory
documentationcenter: 
author: vibronet
manager: mbaldwin
editor: 
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 01/07/2017
ms.author: vittorib
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d7333f9752a9e03c1087a4bc588284b418d8926
ms.openlocfilehash: 4a80252f139d653ff8788b3c1a6a075448cb48e7
ms.contentlocale: de-de
ms.lasthandoff: 02/14/2017


---
# <a name="integrate-azure-ad-with-an-apache-cordova-app"></a>Integrieren von Azure AD in eine Apache Cordova-App
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Apache Cordova ermöglicht die Entwicklung von HTML5/JavaScript-Anwendungen, die auf mobilen Geräten als vollwertige native Anwendungen ausgeführt werden können. Mit Azure Active Directory (Azure AD) können Sie Ihren Cordova-Anwendungen professionelle Authentifizierungsfunktionen hinzufügen.

Ein Cordova-Plug-In umfasst native Azure AD-SDKs für iOS, Android, Windows Store und Windows Phone. Mithilfe dieses Plug-Ins können Sie Ihre Anwendung erweitern und so die Anmeldung mit den Windows Server Active Directory-Konten Ihrer Benutzer unterstützen, auf Office 365- und Azure-APIs zugreifen und sogar Aufrufe schützen, die an Ihre eigene benutzerdefinierte Web-API gerichtet sind.

In diesem Tutorial wird das Apache Cordova-Plug-In für die Active Directory-Authentifizierungsbibliothek (Active Directory Authentication Library, ADAL) verwendet, um einer einfachen App folgende Features hinzuzufügen:

* Authentifizieren eines Benutzers und Abrufen eines Tokens (mit nur einigen wenigen Codezeilen)
* Aufrufen der Graph-API unter Verwendung des Tokens, um das Verzeichnis abzufragen und die Ergebnisse anzuzeigen  
* Minimieren der Authentifizierungsaufforderungen für den Benutzer mithilfe des ADAL-Tokencaches

Für die Umsetzung dieser Verbesserungen sind folgende Schritte erforderlich:

1. Registrieren einer Anwendung in Azure AD
2. Hinzufügen von App-Code zum Anfordern von Token
3. Hinzufügen von Code, um das Token zum Abfragen der Graph-API und zum Anzeigen von Ergebnissen zu verwenden
4. Erstellen des Cordova-Bereitstellungsprojekts mit allen abzudeckenden Plattformen, Hinzufügen des Cordova ADAL-Plug-Ins und Testen der Lösung in Emulatoren

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Einen Azure AD-Mandanten, unter dem Sie über ein Konto mit Rechten für die App-Entwicklung verfügen
* Eine Entwicklungsumgebung, die für die Verwendung von Apache Cordova konfiguriert ist  

Falls Sie beides bereits eingerichtet haben, können Sie direkt mit Schritt 1 fortfahren.

Falls Sie noch nicht über einen Azure AD-Mandanten verfügen, finden Sie [hier](active-directory-howto-tenant.md) eine entsprechende Anleitung.

Falls Sie Apache Cordova noch nicht auf Ihrem Computer eingerichtet haben, installieren Sie Folgendes:

* [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Node.js](https://nodejs.org/download/)
* [Cordova-Befehlszeilenschnittstelle](https://cordova.apache.org/) (einfache Installation per NPM-Paket-Manager: `npm install -g cordova`)

Die hier aufgeführten Installationen sollten sowohl für PCs als auch für Macs geeignet sein.

Für jede Zielplattform gelten unterschiedliche Voraussetzungen:

* Erstellen und Ausführen einer App für Windows-Tablet/PC oder Windows Phone:
  * Installieren Sie [Visual Studio 2013 für Windows mit Update 2 oder höher](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express oder andere Version) oder [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).

* Erstellen und Ausführen einer App für iOS:

  * Installieren Sie Xcode 6.x oder eine höhere Version. (Kann über die [Apple Developer-Website](http://developer.apple.com/downloads) oder über den [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12) heruntergeladen werden.)
  * Installieren Sie [ios-sim](https://www.npmjs.org/package/ios-sim). Damit können Sie iOS-Apps über die Befehlszeile in iOS-Simulator starten. (Die Installation kann ganz einfach über das Terminal erfolgen: `npm install -g ios-sim`.)
* Erstellen und Ausführen einer App für Android:

  * Installieren Sie das [Java Development Kit (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) oder höher. Achten Sie darauf, dass `JAVA_HOME` (Umgebungsvariable) gemäß JDK-Installationspfad richtig festgelegt ist. (Beispiel: C:\Programme\Java\jdk1.7.0_75)
  * Installieren Sie das [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools), und fügen Sie Ihrer Umgebungsvariable `PATH` den Speicherort `<android-sdk-location>\tools` (Beispiel: C:\tools\Android\android-sdk\tools) hinzu.
  * Öffnen Sie den Android SDK Manager (beispielsweise über das Terminal: `android`), und installieren Sie Folgendes:
    * *Android 5.0.1 (API 21)* -Plattform-SDK
    * *Android SDK Build-Tools* (ab Version 19.1.0)
    * *Android Support Repository* (Extras)

  Das Android SDK stellt keine standardmäßige Emulatorinstanz bereit. Erstellen Sie eine Instanz, indem Sie `android avd` über das Terminal ausführen und dann **Erstellen** auswählen, wenn Sie die Android-App in einem Emulator ausführen möchten. Wir empfehlen, mindestens die API-Ebene 19 zu verwenden. Weitere Informationen zum Android-Emulator und zu den Optionen für die Erstellung finden Sie auf der Android-Website unter [AVD Manager](http://developer.android.com/tools/help/avd-manager.html).

## <a name="step-1-register-an-application-with-azure-ad"></a>Schritt 1: Registrieren einer Anwendung in Azure AD
Dieser Schritt ist optional. In diesem Tutorial werden vorab bereitgestellte Werte verwendet, mit denen Sie das Beispiel in Aktion sehen können, ohne selbst etwas in Ihrem eigenen Mandanten bereitstellen zu müssen. Es empfiehlt sich jedoch, diesen Schritt auszuführen und sich mit dem Prozess vertraut zu machen, da sie ihn bei der Erstellung eigener Anwendungen benötigen.

Azure AD stellt Token nur für bekannte Anwendungen aus. Bevor Sie Azure AD über Ihre App nutzen können, müssen Sie dafür einen Eintrag in Ihrem Mandanten erstellen. Gehen Sie wie folgt vor, um in Ihrem Mandanten eine neue Anwendung zu registrieren:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf der oberen Leiste auf Ihr Konto. Wählen Sie in der Verzeichnisliste** **den Azure AD-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.
3. Klicken Sie im linken Bereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen** aus.
5. Folgen Sie den Aufforderungen, und erstellen Sie eine **systemeigene Clientanwendung**. (Cordova-Apps sind zwar HTML-basiert, wir erstellen hier aber trotzdem eine systemeigene Clientanwendung. Bei Verwendung einer anderen Option als **Systemeigene Clientanwendung** funktioniert die Anwendung nicht.)
  * **Name**: Beschreibt die Anwendung für Benutzer.
  * **Umleitungs-URI**: Der URI, der zum Zurückgeben von Token an Ihre App verwendet wird. Geben Sie **http://MyDirectorySearcherApp** ein.

Nach Abschluss der Registrierung weist Azure AD Ihrer App eine eindeutige Anwendungs-ID zu. Dieser Wert wird in den nächsten Abschnitten benötigt. Er befindet sich auf der Anwendungsregisterkarte der neu erstellten App.

Um `DirSearchClient Sample` ausführen zu können, müssen Sie der neu erstellten App die Berechtigung zum Abfragen der Azure AD Graph-API gewähren:

1. Wählen Sie auf der Seite **Einstellungen** zunächst **Erforderliche Berechtigungen** und dann **Hinzufügen** aus.  
2. Wählen Sie für die Azure Active Directory-Anwendung als API die Option **Microsoft Graph** aus, und fügen Sie unter **Delegierte Berechtigungen** die Berechtigung **Als angemeldeter Benutzer auf das Verzeichnis zugreifen** hinzu.  Mit dieser Berechtigung kann die Anwendung die Graph-API nach Benutzern abfragen.

## <a name="step-2-clone-the-sample-app-repository"></a>Schritt 2: Klonen des Beispiel-App-Repositorys
Geben Sie in der Shell oder über die Befehlszeile den folgenden Befehl ein:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-the-cordova-app"></a>Schritt 3: Erstellen der Cordova-App
Cordova-Anwendungen können auf unterschiedliche Weise erstellt werden. In diesem Tutorial verwenden wir die Cordova-Befehlszeilenschnittstelle (Command-Line Interface, CLI).

1. Geben Sie in der Shell oder über die Befehlszeile den folgenden Befehl ein:

        cordova create DirSearchClient

   Dieser Befehl erstellt die Ordnerstruktur und das Gerüst für das Cordova-Projekt.

2. Wechseln Sie in den neuen Ordner „DirSearchClient“:

        cd .\DirSearchClient

3. Kopieren Sie den Inhalt des Startprojekts im Unterordner „www“ mithilfe eines Datei-Managers oder durch Ausführen des folgenden Befehls in Ihrer Shell:

  * Windows: `xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`
  * Mac: `cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`

4. Fügen Sie das Whitelist-Plug-In hinzu. Es wird zum Aufrufen der Graph-API benötigt.

        cordova plugin add cordova-plugin-whitelist

5. Fügen Sie alle Plattformen hinzu, die Sie unterstützen möchten. Führen Sie mindestens einen der folgenden Befehle aus, um ein funktionsfähiges Beispiel zu erhalten. Beachten Sie, dass Sie iOS nicht unter Windows und Windows nicht auf einem Mac emulieren können.

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. Fügen Sie Ihrem Projekt die ADAL für das Cordova-Plug-In hinzu:

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-to-authenticate-users-and-obtain-tokens-from-azure-ad"></a>Schritt 4: Hinzufügen von Code zum Authentifizieren von Benutzern und zum Abrufen von Token aus Azure AD
Die Anwendung, die Sie in diesem Tutorial entwickeln, bietet ein einfaches Feature zum Durchsuchen des Verzeichnisses. Der Benutzer kann den Alias eines beliebigen Benutzers im Verzeichnis eingeben und einige grundlegende Attribute anzeigen. Das Startprojekt enthält die Definition der grundlegenden Benutzeroberfläche der App (in „www/index.html“) und das Gerüst, das aus einfachen App-Ereigniszyklen, Benutzeroberflächenbindungen und der Anzeigelogik für Ergebnisse (in „www/js/index.js“) besteht. Nur die Logik zum Implementieren von Identitätsaufgaben muss noch hinzugefügt werden.

Als Erstes müssen Sie in Ihrem Code die Protokollwerte einführen, die Azure AD verwendet, um Ihre App und die Zielressourcen zu identifizieren. Diese Werte werden später zum Erstellen der Tokenanforderungen verwendet. Fügen Sie den folgenden Codeausschnitt am Anfang der Datei „index.js“ ein:

```javascript
var authority = "https://login.windows.net/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

Die Werte `redirectUri` und `clientId` müssen mit den Werten übereinstimmen, mit denen Ihre App in Azure AD beschrieben wird. Sie finden diese Werte im Azure-Portal auf der Registerkarte **Konfigurieren** (wie weiter oben in diesem Tutorial in Schritt 1 beschrieben).

> [!NOTE]
> Falls Sie sich dagegen entschieden haben, eine neue App in Ihrem eigenen Mandanten zu registrieren, können Sie die vorkonfigurierte Werte einfach unverändert einfügen. Daraufhin wird das Beispiel ausgeführt. Für Apps, die für die Produktion vorgesehen sind, sollten Sie allerdings immer Ihren eigenen Eintrag erstellen.

Fügen Sie als Nächstes den Code für die Tokenanforderung hinzu. Fügen Sie hierzu zwischen den Definitionen `search` und `renderData` den folgenden Codeausschnitt ein:

```javascript
    // Shows the user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize the user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers the authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Lassen Sie uns diese Funktion untersuchen, indem wir sie in ihre zwei Hauptkomponenten unterteilen.
Dieses Beispiel wurde so konzipiert, dass es für einen beliebigen Mandanten funktioniert und nicht nur mit einem bestimmten Mandanten verknüpft ist. Dabei wird der Endpunkt „/common“ verwendet. Benutzer können dadurch bei der Authentifizierung ein beliebiges Konto eingeben. Die Anforderung wird dann an den entsprechenden Mandanten weitergeleitet.

Dieser erste Teil der Methode untersucht den ADAL-Cache, um zu ermitteln, ob bereits ein Token gespeichert wurde. Falls ja, verwendet die Methode die Mandanten, aus denen das Token stammt, um ADAL neu zu initialisieren. Dies ist erforderlich, um zusätzliche Aufforderungen zu vermeiden. Bei Verwendung von „/common“ werden Benutzer nämlich immer aufgefordert, ein neues Konto einzugeben.

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Die eigentliche Tokenanforderung erfolgt im zweiten Teil der Methode. Mit der `acquireTokenSilentAsync`-Methode wird die ADAL aufgefordert, ein Token für die angegebene Ressource zurückzugeben, ohne eine Benutzeroberfläche anzuzeigen. Dies ist möglich, wenn im Cache bereits ein geeignetes Zugriffstoken gespeichert ist oder ein Aktualisierungstoken verwendet werden kann, um ein neues Zugriffstoken abzurufen, ohne eine Aufforderung anzuzeigen. Ist dieser Versuch nicht erfolgreich, wird auf `acquireTokenAsync` zurückgegriffen. In diesem Fall wird dem Benutzer eine Authentifizierungsaufforderung angezeigt.

```javascript
            // Attempt to authorize the user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers the authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Nachdem wir nun über das Token verfügen, können wir die Graph-API aufrufen und die gewünschte Suchabfrage durchführen. Fügen Sie unterhalb der Definition von `authenticate` den folgenden Codeausschnitt ein:

```javascript
// Makes an API call to receive the user list
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Über die Ausgangspunktdateien wird eine einfache Benutzeroberfläche für die Eingabe eines Benutzeralias in ein Textfeld bereitgestellt. Die Methode verwendet diesen Wert, um eine Abfrage zu erstellen, mit dem Zugriffstoken zu kombinieren, an Microsoft Graph zu senden und die Ergebnisse zu analysieren. Mit der bereits in der Ausgangspunktdatei vorhandenen `renderData`-Methode werden die Ergebnisse visualisiert.

## <a name="step-5-run-the-app"></a>Schritt 5: Ausführen der App
Ihre App ist jetzt für die Ausführung bereit. Die Bedienung ist einfach: Geben Sie nach dem Start der App den Alias des Benutzers ein, nach dem Sie suchen möchten, und klicken Sie anschließend auf die Schaltfläche. Sie werden aufgefordert, sich zu authentifizieren. Nach erfolgreicher Authentifizierung und dem erfolgreichen Abschluss der Suche werden die Attribute des gesuchten Benutzers angezeigt.

Bei den nachfolgenden Suchen wird die Suche durchgeführt, ohne dass eine Aufforderung erscheint, da das zuvor abgerufene Token bereits im Cache vorhanden ist.

Die genauen Schritte zum Ausführen der App variieren je nach Plattform.

### <a name="windows-10"></a>Windows 10
   Tablet/PC: `cordova run windows --archs=x64 -- --appx=uap`

   Mobile (erfordert ein mit dem PC verbundenes Gerät mit Windows 10 Mobile): `cordova run windows --archs=arm -- --appx=uap --phone`

   > [!NOTE]
   > Bei der ersten Ausführung werden Sie ggf. aufgefordert, sich für eine Entwicklerlizenz zu registrieren. Weitere Informationen finden Sie unter [Anfordern einer Entwicklerlizenz](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-81-tabletpc"></a>Windows 8.1-Tablet/PC
   `cordova run windows`

   > [!NOTE]
   > Bei der ersten Ausführung werden Sie ggf. aufgefordert, sich für eine Entwicklerlizenz zu registrieren. Weitere Informationen finden Sie unter [Anfordern einer Entwicklerlizenz](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-phone-81"></a>Windows Phone 8.1
   Ausführung auf einem verbundenen Gerät: `cordova run windows --device -- --phone`

   Ausführung im Standardemulator: `cordova emulate windows -- --phone`

   Verwenden Sie `cordova run windows --list -- --phone`, um alle verfügbaren Ziele anzuzeigen, und `cordova run windows --target=<target_name> -- --phone`, um die Anwendung auf einem bestimmten Gerät oder in einem bestimmten Emulator auszuführen (beispielsweise `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

### <a name="android"></a>Android
   Ausführung auf einem verbundenen Gerät: `cordova run android --device`

   Ausführung im Standardemulator: `cordova emulate android`

   Vergewissern Sie sich, dass Sie mit AVD Manager eine Emulatorinstanz erstellt haben, wie weiter oben im Abschnitt „Voraussetzungen“ beschrieben.

   Verwenden Sie `cordova run android --list`, um alle verfügbaren Ziele anzuzeigen, und `cordova run android --target=<target_name>`, um die Anwendung auf einem bestimmten Gerät oder in einem bestimmten Emulator auszuführen (beispielsweise `cordova run android --target="Nexus4_emulator"`).

### <a name="ios"></a>iOS
   Ausführung auf einem verbundenen Gerät: `cordova run ios --device`

   Ausführung im Standardemulator: `cordova emulate ios`

   > [!NOTE]
   > Vergewissern Sie sich, dass Sie das `ios-sim`-Paket für die Ausführung im Emulator installiert haben. Weitere Informationen finden Sie im Abschnitt „Voraussetzungen“.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run the application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` to see additional build and run options.

## <a name="next-steps"></a>Nächste Schritte
Das vollständige Beispiel (ohne Ihre Konfigurationswerte) steht in [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient) zur Verfügung.

Nun können Sie sich anspruchsvolleren (und interessanteren) Szenarien widmen. Empfehlung: [Schützen einer Node.js-Web-API mit Azure AD](active-directory-devquickstarts-webapi-nodejs.md)

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

