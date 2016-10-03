<properties
	pageTitle="Azure Active Directory: v2.0-Android-App | Microsoft Azure"
	description="Es wird beschrieben, wie Sie eine Android-App erstellen, bei der sich Benutzer sowohl mit ihrem persönlichen Microsoft-Konto als auch mit ihrem Geschäfts-, Schul- oder Unikonto anmelden können, und die die Graph-API mithilfe von Drittanbieterbibliotheken aufruft."
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
	ms.date="09/16/2016"
	ms.author="brandwe"/>

#  Hinzufügen der Anmeldung bei einer Android-App mit dem v2.0-Endpunkt unter Verwendung einer Drittanbieterbibliothek mit Graph-API

Die Microsoft Identity-Plattform nutzt offene Standards, z.B. OAuth2 und OpenID Connect. Entwickler können jede gewünschte Bibliothek verwenden, um unsere Dienste zu integrieren. Um Entwickler bei der Nutzung unserer Plattform mit anderen Bibliotheken zu unterstützen, haben wir einige exemplarische Vorgehensweisen wie diese erstellt. Darin wird veranschaulicht, wie Sie Drittanbieterbibliotheken konfigurieren, um eine Verbindung mit der Microsoft-Identitätsplattform herzustellen. Die meisten Bibliotheken, die die [RFC6749 OAuth2-Spezifikation](https://tools.ietf.org/html/rfc6749) implementieren, können eine Verbindung mit der Microsoft-Identitätsplattform herstellen.

Mit der in dieser exemplarischen Vorgehensweise erstellten Anwendung können Benutzer sich bei ihrer Organisation anmelden und sich dann mithilfe der Graph-API in ihrer Organisation selbst suchen.

Wenn OAuth2 oder OpenID Connect neu für Sie sind, ergibt diese Beispielkonfiguration für Sie vielleicht noch keinen Sinn. Es empfiehlt sich, den Artikel [v2.0-Protokolle – OAuth 2.0-Autorisierungscodefluss](active-directory-v2-protocols-oauth-code.md) zu lesen, um Hintergrundinformationen zu erhalten.

> [AZURE.NOTE] Für einige Funktionen unserer Plattform, die über einen Ausdruck in den Standards OAuth2 oder OpenID Connect verfügen, z.B. für den bedingten Zugriff und die Intune-Richtlinienverwaltung, müssen Sie unsere Microsoft Azure-Open Source-Identitätsbibliotheken verwenden.

Der v2.0-Endpunkt unterstützt nicht alle Szenarien und Funktionen von Azure Active Directory.

> [AZURE.NOTE] Lesen Sie die Informationen zu den [Einschränkungen des v2.0-Endpunkts](active-directory-v2-limitations.md), um herauszufinden, ob Sie den v2.0-Endpunkt verwenden sollten.


## Code von GitHub herunterladen
Der Code für dieses Tutorial wird [auf GitHub](git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git) verwaltet. Um folgen zu können, können Sie [das App-Gerüst als ZIP-Datei herunterladen](git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git/archive/skeleton.zip) oder das Gerüst klonen:

```
git clone --branch skeleton git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

Sie können auch einfach das Beispiel herunterladen und direkt anfangen:

```
git@github.com:Azure-Samples/active-directory-android-native-oidcandroidlib-v2.git
```

## Registrieren einer App
Erstellen Sie im [Anwendungsregistrierungsportal](https://apps.dev.microsoft.com) eine neue App, oder führen Sie die detaillierten Schritte unter [Registrieren einer App mit dem v2.0-Endpunkt](active-directory-v2-app-registration.md) aus. Stellen Sie sicher, dass Sie:

- Kopieren Sie die Ihrer App zugewiesene **Anwendungs-ID**. Sie benötigen sie in Kürze.
- Fügen Sie die **Mobile**-Plattform Ihrer App hinzu.

> Hinweis: Im Anwendungsregistrierungsportal wird ein Wert für den **Umleitungs-URI** bereitgestellt. In diesem Beispiel müssen Sie jedoch den Standardwert `https://login.microsoftonline.com/common/oauth2/nativeclient` verwenden.


## Herunterladen der Drittanbieterbibliothek „NXOAuth2“ und Erstellen eines Arbeitsbereichs

Für diese exemplarische Vorgehensweise verwenden Sie OIDCAndroidLib von GitHub. Hierbei handelt es sich um eine OAuth2-Bibliothek, die auf dem OpenID Connect-Code von Google basiert. Sie implementiert das native Anwendungsprofil und unterstützt den Autorisierungsendpunkt des Benutzers. Dies sind alle Voraussetzungen, die für die Integration in die Microsoft-Identitätsplattform erfüllt sein müssen.

Klonen Sie das OIDCAndroidLib-Repository auf Ihren Computer.

```
git@github.com:kalemontes/OIDCAndroidLib.git
```

![androidStudio](media/active-directory-android-native-oidcandroidlib-v2/emotes-url.png)

## Einrichten der Android Studio-Umgebung

1. Erstellen Sie ein neues Android Studio-Projekt, und akzeptieren Sie die Standardeinstellungen im Assistenten.

	![Neues Projekt im Android Studio erstellen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample1.PNG)

	![Android-Zielgeräte](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample2.PNG)

	![Eine Aktivität für ein mobiles Gerät hinzufügen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample3.PNG)

2. Um Ihre Projektmodule einzurichten, verschieben Sie das geklonte Repository in den Speicherort des Projekts. Sie können auch das Projekt erstellen und es dann direkt in den Projektspeicherort klonen.

	![Projektmodule](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4_1.PNG)

3. Öffnen Sie die Projektmoduleinstellungen über das Kontextmenü oder mithilfe der Tastenkombination STRG+ALT+UMSCHALT+S.

	![Projektmoduleinstellungen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample4.PNG)

4. Entfernen Sie das App-Standardmodul, da Sie nur die Projektcontainereinstellungen benötigen.

	![Das App-Standardmodul](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample5.PNG)

5. Importieren Sie die Module aus dem geklonten Repository in das aktuelle Projekt.

	![Gradle-Projekt importieren](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample6.PNG) ![Neue Modulseite erstellen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample7.PNG)

6. Wiederholen Sie diese Schritte für das `oidlib-sample`-Modul.

7. Überprüfen Sie die oidclib-Abhängigkeiten im `oidlib-sample`-Modul.

	![oidclib-Abhängigkeiten im oidlib-sample-Modul](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8.PNG)

8. Klicken Sie auf **OK**, und warten Sie auf die Gradle-Synchronisierung.

	„settings.gradle“ sollte wie folgt aussehen:

	![Screenshot von „settings.gradle“](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample8_1.PNG)

9. Erstellen Sie die Beispiel-App, um sicherzustellen, dass das Beispiel richtig ausgeführt wird.

	Die Verwendung mit Azure Active Directory ist noch nicht möglich. Zuerst müssen wir einige Endpunkte konfigurieren. So wird sichergestellt, dass es nicht zu Android Studio-Problemen kommt, bevor wir mit der Anpassung der Beispiel-App beginnen.

10. Erstellen Sie das `oidlib-sample`, und führen Sie es als Ziel in Android Studio aus.

	![Fortschritt der oidlib-sample-Erstellung](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample9.png)

11. Löschen Sie das Verzeichnis `app `, das beim Entfernen des Moduls aus dem Projekt übrig geblieben ist. Aus Sicherheitsgründen wird es von Android Studio nicht gelöscht.

	![Dateistruktur mit App-Verzeichnis](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample12.PNG)

12. Öffnen Sie das Menü **Edit Configurations**, um die Ausführungskonfiguration zu entfernen, die beim Entfernen des Moduls aus dem Projekt ebenfalls übrig geblieben ist.

	![Menü zum Bearbeiten von Konfigurationen](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample10.PNG) ![Ausführungskonfiguration der App](media/active-directory-android-native-oidcandroidlib-v2/SetUpSample11.PNG)

## Konfigurieren der Endpunkte des Beispiels

Nachdem `oidlib-sample` nun erfolgreich ausgeführt wird, bearbeiten Sie einige Endpunkte, um die Verwendung mit Azure Active Directory zu ermöglichen.

### Konfigurieren des Clients durch Bearbeiten der Datei „oidc\_clientconf.xml“

1. Da Sie nur OAuth2-Flüsse zum Abrufen eines Tokens und Aufrufen der Graph-API verwenden, legen Sie für den Client die ausschließliche Nutzung von OAuth2 fest. OIDC wird in einem späteren Beispiel beschrieben.

	```xml
	    <bool name="oidc_oauth2only">true</bool>
	```

2. Konfigurieren Sie die Client-ID, die Sie über das Registrierungsportal erhalten haben.

	```xml
	    <string name="oidc_clientId">86172f9d-a1ae-4348-aafa-7b3e5d1b36f5</string>
	    <string name="oidc_clientSecret"></string>
	```

3. Ändern Sie Ihren Umleitungs-URI in den unten stehenden URI.

	```xml
	    <string name="oidc_redirectUrl">https://login.microsoftonline.com/common/oauth2/nativeclient</string>
	```

4. Konfigurieren Sie die Bereiche, die Sie benötigen, um auf die Graph-API zuzugreifen.

	```xml
	    <string-array name="oidc_scopes">
	        <item>openid</item>
	        <item>https://graph.microsoft.com/User.Read</item>
	        <item>offline_access</item>
	    </string-array>
	```

Der `User.Read`-Wert in `oidc_scopes` ermöglicht Ihnen das Lesen des Basisprofils des angemeldeten Benutzers. Weitere Informationen zu allen verfügbaren Bereichen finden Sie unter [Microsoft Graph-Berechtigungsbereiche](https://graph.microsoft.io/docs/authorization/permission_scopes).

Erläuterungen zu `openid` oder `offline_access` als Bereiche in OpenID Connect finden Sie unter [v2.0-Protokolle – OAuth 2.0-Autorisierungscodefluss](active-directory-v2-protocols-oauth-code.md).

### Konfigurieren der Clientendpunkte durch Bearbeiten der Datei „oidc\_endpoints.xml“

- Öffnen Sie die Datei `oidc_endpoints.xml`, und nehmen Sie die folgenden Änderungen vor:

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
Die Endpunkte für `userInfoEndpoint` und `revocationEndpoint` werden in Azure Active Directory zurzeit nicht unterstützt. Wenn Sie diese mit dem Standardwert „example.com“ beibehalten, werden Sie daran erinnert, dass sie im Beispiel nicht verfügbar sind.


## Konfigurieren eines Graph-API-Aufrufs

- Öffnen Sie die Datei `HomeActivity.java`, und nehmen Sie die folgenden Änderungen vor:

	```Java
	   //TODO: set your protected resource url
	    private static final String protectedResUrl = "https://graph.microsoft.com/v1.0/me/";
	```

Hier gibt ein einfacher Graph-API-Aufruf die gewünschten Informationen zurück.

Mehr müssen Sie nicht ändern. Führen Sie die Anwendung `oidlib-sample` aus, und klicken Sie auf **Anmelden**.

Klicken Sie nach erfolgreicher Authentifizierung auf die Schaltfläche **Request Protected Resource**, um den Aufruf der Graph-API zu testen.

## Abrufen von Sicherheitsupdates für unser Produkt

Wir empfehlen Ihnen, den Erhalt von Benachrichtigungen zu Sicherheitsvorfällen einzurichten. Rufen Sie dazu das [Security TechCenter](https://technet.microsoft.com/security/dd252948) auf, und abonnieren Sie die Sicherheitsempfehlungen.

<!---HONumber=AcomDC_0921_2016-->