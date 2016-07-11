<properties
	pageTitle="Azure AD v2.0: Android-App | Microsoft Azure"
	description="Es wird beschrieben, wie Sie eine Android-App erstellen, bei der sowohl Benutzer mit persönlichen Microsoft-Konten als auch mit Geschäfts- oder Schulkonten angemeldet werden, und wie die Graph-API mit Drittanbieterbibliotheken aufgerufen wird."
	services="active-directory"
	documentationCenter=""
	authors="brandwe"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/31/2016"
	ms.author="brandwe"/>

#  Hinzufügen der Anmeldung zu einer Android-App mit dem v2.0-Endpunkt unter Verwendung einer Drittanbieterbibliothek mit Graph-API

Die Microsoft Identity-Plattform nutzt offene Standards, z.B. OAuth2 und OpenID Connect. Dadurch können Entwickler eine beliebige Bibliothek verwenden, um sie in unsere Dienste zu integrieren. Um Entwickler bei der Nutzung unserer Plattform mit anderen Bibliotheken zu unterstützen, haben wir einige exemplarische Vorgehensweisen wie diese erstellt. Darin wird veranschaulicht, wie Sie Drittanbieterbibliotheken konfigurieren, um eine Verbindung mit der Microsoft Identity-Plattform herzustellen. Die meisten Bibliotheken, die die [RFC6749 OAuth2-Spezifikation](https://tools.ietf.org/html/rfc6749) implementieren, können eine Verbindung mit der Microsoft Identity-Plattform herstellen.

Diese Anwendung ermöglicht Benutzern das Anmelden bei ihrer Organisation und das Suchen nach der eigenen Person in der Organisation mit der Graph-API.

Wenn OAuth2 oder OpenID Connect neu für Sie ist, ergibt diese Beispielkonfiguration für Sie vielleicht noch nicht viel Sinn. Wir empfehlen Ihnen, sich eine kurze [Übersicht über das Protokoll](active-directory-v2-protocols-oauth-code.md) anzusehen.


> [AZURE.NOTE]
	Für einige Funktionen unserer Plattform, die über einen Ausdruck in diesen Standards verfügen, z.B. Bedingter Zugriff und Intune-Richtlinienverwaltung, müssen Sie unsere Open Source-Microsoft Azure-Identitätsbibliotheken verwenden.


> [AZURE.NOTE]
	Nicht alle Szenarien und Funktionen von Azure Active Directory werden vom v2.0-Endpunkt unterstützt. Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.


## Herunterladen
Der Code für dieses Tutorial wird [auf GitHub](git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git) verwaltet. Um folgen zu können, können Sie [das App-Gerüst als ZIP-Datei herunterladen](git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git/archive/skeleton.zip) oder das Gerüst klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Oder beginnen Sie sofort nach dem Durchführen des Downloads:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## Registrieren einer App
Erstellen Sie eine neue App unter [apps.dev.microsoft.com](https://apps.dev.microsoft.com), oder führen Sie die folgenden [ausführlichen Schritte](active-directory-v2-app-registration.md) aus. Stellen Sie sicher, dass Sie:

- die Ihrer App zugewiesene **Anwendungs-ID** kopieren. Sie benötigen Sie in Kürze.
- Fügen Sie die **Mobile**-Plattform Ihrer App hinzu.
- Kopieren Sie den **Umleitungs-URI** aus dem Portal. Sie müssen den Standardwert `https://login.microsoftonline.com/common/oauth2/nativeclient` verwenden.


## Herunterladen der Drittanbieterbibliothek „nxoauth2“ und Starten eines Arbeitsbereichs

Für diese exemplarische Vorgehensweise verwenden wir OIDCAndroidLib von GitHub. Hierbei handelt es sich um eine OAuth2-Bibliothek, die auf dem OpenID Connect-Code von Google basiert. Sie implementiert das native Anwendungsprofil und unterstützt den Endbenutzer-Autorisierungsendpunkt. Dies sind alle Voraussetzungen, die für die Integration in die Microsoft Identity-Plattform erfüllt sein müssen.

*  Klonen

Beginnen Sie, indem Sie das OIDCAndroidLib-Repository auf Ihrem Computer klonen.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

### Einrichten der Android Studio-Umgebung

*  Erstellen des Projekts Erstellen Sie ein neues AndroidStudio-Projekt, und führen Sie die Schritte des Standard-Assistenten aus.

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

*  Einrichten der Projektmodule Der einfachste Weg zum Festlegen der Module besteht meist darin, das geklonte Repository an den Projektspeicherort zu verschieben. Sie können als ersten Schritt auch das Projekt erstellen und das Klonen dann direkt am Projektspeicherort durchführen.

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

Öffnen Sie als Nächstes im Kontextmenü die Projektmoduleinstellungen, oder verwenden Sie die Tastenkombination `Ctrl + Alt + Maj + S`.

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

Entfernen Sie das App-Standardmodul, da wir nur die Projektcontainereinstellungen benötigen.

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

Als Nächstes müssen wir die Module aus dem geklonten Repository in das aktuelle Projekt klonen.

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

> Wiederholen Sie diese Schritte für das `oidlib-sample`-Modul.

Überprüfen Sie die oidclib-Abhängigkeiten im `oidlib-sample`-Modul.

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

Klicken Sie auf „OK“, und warten Sie auf die Gradle-Synchronisierung.

„settings.gradle“ sollte wie folgt aussehen:

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

*  Erstellen Sie die Beispiel-App, um sicherzustellen, dass das Beispiel richtig ausgeführt wird.

Die Verwendung mit Azure Active Directory ist noch nicht möglich. Zuerst müssen wir einige Endpunkte konfigurieren. So wird sichergestellt, dass es nicht zu Android Studio-Problemen kommt, bevor wir mit der Anpassung der Beispiel-App beginnen.

Erstellen Sie das `oidlib-sample`, und führen Sie es als Ziel in Android Studio aus.

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

*  Bereinigen

Sie können das Verzeichnis `app ` löschen, das beim Entfernen des Moduls aus dem Projekt übrig geblieben ist. Aus Sicherheitsgründen wird es von Android Studio nicht gelöscht.

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

Außerdem können Sie die Ausführungskonfiguration entfernen, die beim Entfernen des Moduls aus dem Projekt ebenfalls übrig geblieben ist, indem Sie das Menü „Konfigurationen bearbeiten“ öffnen.

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG) ![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## Konfigurieren der Endpunkte des Beispiels

Nachdem `oidlib-sample` nun erfolgreich ausgeführt wird, bearbeiten wir einige Endpunkte, um die Verwendung mit Azure Active Directory zu ermöglichen.

* Konfigurieren des Clients

Öffnen Sie die Datei `oidc_clientconf.xml`, und nehmen Sie die folgenden Änderungen vor:

1. Da wir nur OAuth2-Flüsse zum Abrufen eines Tokens und Aufrufen der Graph-API verwenden, legen wir für den Client die ausschließliche Nutzung von OAuth2 fest. Die Verwendung von OIDC wird in einem späteren Beispiel beschrieben.

```xml
    <bool name="oidc_oauth2only">true</bool>
```

2. Konfigurieren Sie die Client-ID, die Sie über das Registrierungsportal erhalten haben.

```xml
    <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
    <string name="oidc_clientSecret"></string>
```

3. Konfigurieren Sie den Umleitungs-URI, den Sie über das Registrierungsportal erhalten haben.

```xml
    <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
```

4. Konfigurieren Sie die Bereiche, die Sie benötigen, um auf die Graph-API zuzugreifen.

```xml
    <string-array name="oidc_scopes">
        <item>openid</item>
        <item>User.ReadBasic.All</item>
        <item>offline_access</item>
    </string-array>
```

Hier legen wir `oidc_scopes` fest. Die Bereiche, die wir für diese Anwendung anfordern, lauten `User.ReadBasic.All`. Dies ermöglicht es uns, das grundlegende Profil aller Benutzer in unserem Verzeichnis zu lesen. Weitere Informationen zu allen verfügbaren Bereichen für Microsoft Graph finden Sie [hier](https://graph.microsoft.io/docs/authorization/permission_scopes).

Wenn Sie nach einer Beschreibung von `openid` oder `offline_access` als Bereiche in OpenID Connect suchen, hilft Ihnen die [Übersicht über das Protokoll](active-directory-v2-protocols-oauth-code.md) weiter.

* Konfigurieren der Clientendpunkte

Öffnen Sie die Datei `oidc_endpoints.xml`, und nehmen Sie die folgenden Änderungen vor:

```xml
<!-- Stores OpenID Connect provider endpoints. -->
<resources>
    <string name="op_authorizationEnpoint">https://login.microsoftonline.com/common/oauth2/v2.0/authorize</string>
    <string name="op_tokenEndpoint">https://login.microsoftonline.com/common/oauth2/v2.0/token</string>
    <string name="op_userInfoEndpoint">https://www.example.com/oauth2/userinfo</string>
    <string name="op_revocationEndpoint">https://www.example.com/oauth2/revoketoken</string>
</resources>
```

Diese Endpunkte sollten sich niemals ändern, wenn Sie OAuth2 als Protokoll verwenden.

> [AZURE.NOTE] 
Die Endpunkte für `userInfoEndpoint` und `revocationEndpoint` werden von Azure Active Directory derzeit nicht unterstützt. Wir behalten also die Standardwerte von „example.com“ bei und verfügen somit über eine Erinnerung, dass dies im Beispiel nicht vorhanden ist.


## Konfigurieren eines Graph-API-Aufrufs

Öffnen Sie die Datei `HomeActivity.java`, und nehmen Sie die folgenden Änderungen vor:

```Java
   //TODO: set your protected resource url
    private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
```

Hier führen wir einen einfachen Graph-API-Aufruf durch, mit dem die Informationen zurückgegeben werden.

## Fertig!

Dies sind alle erforderlichen Änderungen. Führen Sie `oidlib-sample` für die Anwendung aus, und klicken Sie auf „Anmelden“.

Klicken Sie nach der erfolgreichen Authentifizierung auf die Schaltfläche „Request Protected Resource“ (Geschützte Ressource anfordern), um den Aufruf der Graph-API zu testen.

## Abrufen von Sicherheitsupdates für unser Produkt

Wir empfehlen Ihnen, den Erhalt von Benachrichtigungen zu Sicherheitsvorfällen einzurichten. Rufen Sie dazu [diese Seite](https://technet.microsoft.com/security/dd252948) auf, und abonnieren Sie die Sicherheitsempfehlungen.

<!---HONumber=AcomDC_0629_2016-->