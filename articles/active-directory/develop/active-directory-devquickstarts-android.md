---
title: "Azure AD Android – Erste Schritte | Microsoft Docs"
description: "In diesem Thema erfahren Sie, wie eine Android-Anwendung erstellt wird, die sich für die Anmeldung in Azure AD integriert und über OAuth durch Azure AD geschützte APIs aufruft."
services: active-directory
documentationcenter: android
author: danieldobalian
manager: mbaldwin
editor: 
ms.assetid: da1ee39f-89d3-4d36-96f1-4eabbc662343
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 01/07/2017
ms.author: dadobali
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 590e2bc759567cf3d679e261592dff1e699ba295
ms.contentlocale: de-de
ms.lasthandoff: 04/27/2017


---
# <a name="integrate-azure-ad-into-an-android-app"></a>Integrieren von Azure AD in eine Android-App
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

> [!TIP]
> Testen Sie die Vorschau unseres neuen [Entwicklerportals](https://identity.microsoft.com/Docs/Android), mit dem Sie Azure AD innerhalb weniger Minuten betriebsbereit machen. Im Entwicklerportal werden Sie durch den Vorgang zum Registrieren einer App und die Integration von Azure AD in Ihren Code geleitet. Nach dem Durchführen dieser Schritte verfügen Sie über eine einfache Anwendung zur Authentifizierung von Benutzern in Ihrem Mandanten sowie über ein Back-End, das Token akzeptieren und eine Überprüfung durchführen kann.
>
>

Bei der Entwicklung einer Desktopanwendung ist es für Sie mit Azure Active Directory (Azure AD) einfach und problemlos möglich, Ihre Benutzer über deren lokale Active Directory-Konten zu authentifizieren. Außerdem kann Ihre Anwendung damit auf sichere Weise alle Web-APIs nutzen, die per Azure AD geschützt sind, z. B. die Office 365-APIs oder die Azure-API.

Für Android-Clients, die auf geschützte Ressourcen zugreifen müssen, ist unter Azure AD die Active Directory Authentication Library (ADAL) verfügbar. Die einzige Aufgabe der ADAL besteht darin, Ihrer App das Abrufen von Zugriffstoken zu erleichtern. Um Ihnen zu zeigen, wie einfach es geht, erstellen wir eine Android-Anwendung für Aufgabenlisten, mit der folgende Aktionen ausgeführt werden können:

* Abrufen von Zugriffstoken zum Aufrufen einer Aufgabenlisten-API mit dem [OAuth 2.0-Authentifizierungsprotokoll](https://msdn.microsoft.com/library/azure/dn645545.aspx)
* Abrufen der Aufgabenliste eines Benutzers
* Abmelden von Benutzern

Um zu beginnen, benötigen Sie einen Azure AD-Mandanten, in dem Sie Benutzer erstellen und eine Anwendung registrieren können. Wenn Sie über noch keinen Mandanten verfügen, [erfahren Sie hier, wie Sie einen erhalten](active-directory-howto-tenant.md).

## <a name="step-1-download-and-run-the-nodejs-rest-api-todo-sample-server"></a>Schritt 1: Herunterladen und Ausführen des „Node.js REST API TODO Sample Server“
Das Beispiel „Node.js REST API TODO“ ist speziell für die Arbeit mit dem vorhandenen Beispiel zum Erstellen einer Aufgabenlisten-REST-API für Azure AD für einen einzelnen Mandanten gedacht. Dies ist eine Voraussetzung für den Schnellstart.

Informationen zur Einrichtung finden Sie in unseren vorhandenen Beispielen unter [Microsoft Azure Active Directory-Beispiel – REST-API-Dienst für „Node.js“](active-directory-devquickstarts-webapi-nodejs.md).


## <a name="step-2-register-your-web-api-with-your-azure-ad-tenant"></a>Schritt 2: Registrieren Ihrer Web-API beim Azure AD-Mandanten
Active Directory unterstützt das Hinzufügen von zwei Arten von Anwendungen:

- Web-APIs, die Dienste für Benutzer bieten
- Anwendungen (die im Web oder auf einem Gerät ausgeführt werden), die auf Web-APIs zugreifen

In diesem Schritt registrieren Sie die Web-API, die Sie lokal zum Testen dieses Beispiels ausführen. Normalerweise handelt es sich bei dieser Web-API um einen REST-Dienst, mit dem die Funktionen bereitgestellt werden, auf die eine App zugreifen soll. Mit Azure AD können Sie einen beliebigen Endpunkt schützen.

Es wird davon ausgegangen, dass Sie die zuvor erwähnte TODO-REST-API registrieren. Dies funktioniert allerdings für alle Web-APIs, die Sie mit Azure Active Directory schützen möchten.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf der oberen Leiste auf Ihr Konto. Wählen Sie in der **Verzeichnis** den Azure AD-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.
3. Klicken Sie im linken Bereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen** aus.
5. Geben Sie einen Anzeigenamen für die Anwendung ein, z.B. **TodoListService**, wählen Sie **Webanwendung und/oder Web-API** aus, und klicken Sie auf **Weiter**.
6. Geben Sie als Anmelde-URL die Basis-URL für das Beispiel ein. Standardmäßig ist dies `https://localhost:8080`.
7. Klicken Sie auf **OK**, um die Registrierung abzuschließen.
8. Wechseln Sie jetzt im Azure-Portal zur Anwendungsseite, suchen Sie den Wert der Anwendungs-ID, und notieren Sie ihn. Sie benötigen ihn später für die Konfiguration Ihrer Anwendung.
9. Aktualisieren Sie auf der Seite **Einstellungen** -> **Eigenschaften** den App-ID-URI: Geben Sie `https://<your_tenant_name>/TodoListService` ein. Ersetzen Sie `<your_tenant_name>` durch den Namen des Azure AD-Mandanten.

## <a name="step-3-register-the-sample-android-native-client-application"></a>Schritt 3: Registrieren der systemeigenen Android-Beispiel-Clientanwendung
Sie müssen Ihre Webanwendung in diesem Beispiel registrieren. Nur so kann die Anwendung mit der eben registrierten Web-API kommunizieren. Azure AD verweigert Ihrer Anwendung sogar die Anforderung einer Anmeldung, wenn sie nicht registriert ist. Dies ist Teil der Sicherheit dieses Modells.

Es wird davon ausgegangen, dass Sie die zuvor erwähnte Beispielanwendung registrieren. Aber dieses Verfahren funktioniert für jede App, die Sie entwickeln.

> [!NOTE]
> Sie könnten sich fragen, warum Sie eine Anwendung und eine Web-API auf einem Mandanten bereitstellen. Wie Sie vielleicht schon vermuten, können Sie eine App erstellen, von der auf eine externe API zugegriffen wird, die von einem anderen Mandanten aus in Azure AD registriert wurde. Wenn Sie dies tun, werden Ihre Kunden aufgefordert, der Verwendung der API in der Anwendung zuzustimmen. Die Active Directory Authentication Library für iOS führt diese Zustimmung für Sie durch. Wenn wir uns mit den erweiterten Funktionen beschäftigen, werden Sie feststellen, dass dies ein wichtiger Teil der Aufgaben ist, die für den Zugriff auf die Suite der Microsoft-APIs von Azure und Office sowie anderer Dienstanbieter erforderlich sind. Da Sie aber nun sowohl Ihre Web-API als auch Ihre Anwendung unter dem gleichen Mandanten registriert haben, wird Ihnen keine Aufforderung zur Zustimmung angezeigt. Dies ist eher die Regel, wenn Sie eine Anwendung nur für Ihr eigenes Unternehmen entwickeln.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf der oberen Leiste auf Ihr Konto. Wählen Sie in der **Verzeichnis** den Azure AD-Mandanten aus, bei dem Sie Ihre Anwendung registrieren möchten.
3. Klicken Sie im linken Bereich auf **Weitere Dienste**, und wählen Sie **Azure Active Directory** aus.
4. Klicken Sie auf **App-Registrierungen**, und wählen Sie **Hinzufügen** aus.
5. Geben Sie einen Anzeigenamen für die Anwendung ein, z.B. **TodoListClient-Android**, wählen Sie **Systemeigene Clientanwendung** aus, und klicken Sie auf **Weiter**.
6. Geben Sie als Umleitungs-URI `http://TodoListClient` ein. Klicken Sie auf **Fertig stellen**.
7. Suchen Sie auf der Anwendungsseite den Wert der Anwendungs-ID, und notieren Sie ihn. Sie benötigen ihn später für die Konfiguration Ihrer Anwendung.
8. Wählen Sie auf der Seite **Einstellungen** zunächst **Erforderliche Berechtigungen** und dann **Hinzufügen** aus.  Suchen Sie TodoListService, wählen Sie ihn aus, und fügen Sie unter **Delegierte Berechtigungen** die Berechtigung **Auf TodoListService zugreifen** hinzu. Klicken Sie anschließend auf **Fertig**.

Für die Erstellung mit Maven können Sie „pom.xml“ auf der obersten Ebene verwenden:

1. Klonen Sie dieses Repository in einem Verzeichnis Ihrer Wahl:

  `$ git clone git@github.com:AzureADSamples/NativeClient-Android.git`  
2. Führen Sie die Schritte im Abschnitt mit den [Voraussetzungen zum Einrichten der Maven-Umgebung für Android](https://github.com/MSOpenTech/azure-activedirectory-library-for-android/wiki/Setting-up-maven-environment-for-Android) aus.
3. Richten Sie den Emulator mit SDK 19 ein.
4. Wechseln Sie zu dem Stammordner, in dem Sie das Repository geklont haben.
5. Führen Sie den folgenden Befehl aus: `mvn clean install`
6. Wechseln Sie in das Verzeichnis mit dem Schnellstartbeispiel: `cd samples\hello`
7. Führen Sie den folgenden Befehl aus: `mvn android:deploy android:run`

   Die App wird gestartet.
8. Geben Sie die Anmeldeinformationen des Testbenutzers ein, um sie auszuprobieren.

Zusätzlich zum AAR-Paket werden JAR-Pakete übermittelt.

## <a name="step-4-download-the-android-adal-and-add-it-to-your-eclipse-workspace"></a>Schritt 4: Herunterladen der Android-ADAL und Hinzufügen zum Eclipse-Arbeitsbereich
Wir haben die Verwendung für Sie vereinfacht. Sie haben mehrere Möglichkeiten zur Verwendung der ADAL in Ihrem Android-Projekt:

* Sie können den Quellcode nutzen, um diese Bibliothek in Eclipse zu importieren und mit Ihrer Anwendung zu verknüpfen.
* Bei Verwendung von Android Studio können Sie das Paketformat „AAR“ nutzen und auf die Binärdateien verweisen.

### <a name="option-1-source-zip"></a>Option 1: ZIP-Datei mit Quellcode
Klicken Sie zum Herunterladen einer Kopie des Quellcodes rechts auf der Seite auf **Download ZIP**. Sie können die ZIP-Datei auch [von GitHub herunterladen](https://github.com/AzureAD/azure-activedirectory-library-for-android/archive/v1.0.9.tar.gz).

### <a name="option-2-source-via-git"></a>Option 2: Quellcode per Git
Geben Sie Folgendes ein, um den Quellcode des SDK über Git abzurufen:

    git clone git@github.com:AzureAD/azure-activedirectory-library-for-android.git
    cd ./azure-activedirectory-library-for-android/src

### <a name="option-3-binaries-via-gradle"></a>Option 3: Binärdateien per Gradle
Sie können die Binärdateien aus dem zentralen Repository von Maven abrufen. Das AAR-Paket kann wie folgt in Ihr Projekt unter Android Studio eingefügt werden:

```gradle
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
}
```

### <a name="option-4-aar-via-maven"></a>Option 4: AAR über Maven
Wenn Sie das M2Eclipse-Plug-In verwenden, können Sie die Abhängigkeit in Ihrer Datei „pom.xml“ angeben:

```xml
<dependency>
    <groupId>com.microsoft.aad</groupId>
    <artifactId>adal</artifactId>
    <version>1.1.1</version>
    <type>aar</type>
</dependency>
```


### <a name="option-5-jar-package-inside-the-libs-folder"></a>Option 5: JAR-Paket im Ordner „libs“
Sie können die JAR-Datei aus dem Repository von Maven abrufen und im Ordner **libs** Ihres Projekts ablegen. Außerdem müssen Sie die erforderlichen Ressourcen in Ihr Projekt kopieren, da sie in den JAR-Paketen nicht enthalten sind.

## <a name="step-5-add-references-to-android-adal-to-your-project"></a>Schritt 5: Hinzufügen von Verweisen auf die ADAL für Android zu Ihrem Projekt
1. Fügen Sie einen Verweis auf Ihr Projekt hinzu, und geben Sie ihn als Android-Bibliothek an. Wenn Sie damit nicht vertraut sind, finden Sie weitere Informationen auf der [Android Studio-Website](http://developer.android.com/tools/projects/projects-eclipse.html).
2. Fügen Sie die Projektabhängigkeit für das Debuggen in Ihren Projekteinstellungen hinzu.
3. Aktualisieren Sie die Datei „AndroidManifest.xml“ Ihres Projekts, damit sie Folgendes enthält:

        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <application
            android:allowBackup="true"
            android:debuggable="true"
            android:icon="@drawable/ic_launcher"
            android:label="@string/app_name"
            android:theme="@style/AppTheme" >

            <activity
                android:name="com.microsoft.aad.adal.AuthenticationActivity"
                android:label="@string/title_login_hello_app" >
            </activity>
            ....
        <application/>

4. Erstellen Sie eine Instanz von AuthenticationContext als Hauptaktivität. Die Details dieses Aufrufs liegen außerhalb des Umfangs dieses Themas, aber das [Android-Beispiel für systemeigene Clients](https://github.com/AzureADSamples/NativeClient-Android) stellt eine gute Einstiegshilfe dar. Im folgenden Beispiel ist SharedPreferences der Standardcache, und die Autorität hat das Format `https://login.windows.net/yourtenant.onmicrosoft.com`:

    `mContext = new AuthenticationContext(MainActivity.this, authority, true); // mContext is a field in your activity`

5. Kopieren Sie diesen Codeblock, um die Beendigung der AuthenticationActivity zu behandeln, nachdem der Benutzer Anmeldeinformationen eingegeben und einen Autorisierungscode empfangen hat:

        @Override
         protected void onActivityResult(int requestCode, int resultCode, Intent data) {
             super.onActivityResult(requestCode, resultCode, data);
             if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
             }
         }

6. Zum Anfordern eines Tokens definieren Sie einen Rückruf:

        private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {

            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    textViewStatus.setText("Cancelled");
                    Log.d(TAG, "Cancelled");
                } else {
                    textViewStatus.setText("Authentication error:" + exc.getMessage());
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
            public void onSuccess(AuthenticationResult result) {
                mResult = result;

                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    textViewStatus.setText("Token is empty");
                    Log.d(TAG, "Token is empty");
                } else {
                    // request is successful
                    Log.d(TAG, "Status:" + result.getStatus() + " Expired:"
                            + result.getExpiresOn().toString());
                    textViewStatus.setText(PASSED);
                }
            }
        };

7. Abschließend fordern Sie mit diesem Rückruf ein Token an:

    `mContext.acquireToken(MainActivity.this, resource, clientId, redirect, user_loginhint, PromptBehavior.Auto, "",
                   callback);`

Im Folgenden finden Sie eine Erklärung der Parameter:

* *resource* ist erforderlich, und es handelt sich um die Ressource, auf die Sie zugreifen möchten.
* *clientid* ist erforderlich und stammt von Azure AD.
* *RedirectUri* muss für den Aufruf „acquireToken“ nicht angegeben werden. Sie können dafür Ihren Paketnamen verwenden.
* *PromptBehavior* ist eine Hilfe zum Abfragen von Anmeldeinformationen, um Cache und Cookie zu überspringen.
* *callback* wird aufgerufen, nachdem der Autorisierungscode für ein Token ausgetauscht wurde. Dazu gehört ein Objekt „AuthenticationResult“, mit dem Informationen zum Zugriffstoken, Ablaufdatum und ID-Token bereitgestellt werden.
* *acquireTokenSilent* ist optional. Sie können diesen Parameter aufrufen, um das Zwischenspeichern und Aktualisieren des Tokens durchzuführen. Er stellt auch die Synchronisierungsversion bereit. *userId* wird als Parameter akzeptiert.

        mContext.acquireTokenSilent(resource, clientid, userId, callback );

Mit dieser exemplarischen Vorgehensweise haben Sie alles, was Sie für die erfolgreiche Integration in Azure Active Directory benötigen. Weitere Beispiele hierzu finden Sie im Repository „AzureADSamples/“ bei GitHub.

## <a name="important-information"></a>Wichtige Informationen
### <a name="customization"></a>Anpassung
Ihren Anwendungsressourcen können Bibliotheksprojektressourcen überschreiben. Dies geschieht, wenn Ihre App erstellt wird. Aus diesem Grund können Sie das Layout der Authentifizierungsaktivität wie gewünscht anpassen. Stellen Sie sicher, dass die ID der Steuerelemente beibehalten wird, die von der ADAL genutzt werden (WebView).

### <a name="broker"></a>Broker
Die Brokerkomponente wird über die Unternehmensportal-App von Microsoft Intune bereitgestellt. Das Konto wird in AccountManager erstellt. Der Kontotyp lautet „com.microsoft.workaccount“. AccountManager lässt nur ein SSO-Konto zu. Für den Benutzer wird ein SSO-Cookie erstellt, nachdem die Geräteabfrage (Challenge) für eine der Apps abgeschlossen wurde.

Die ADAL verwendet das Brokerkonto, falls unter diesem Authentifikator ein Benutzerkonto erstellt wurde und Sie sich nicht für das Überspringen entschieden haben. Sie können den Brokerbenutzer wie folgt überspringen:

   `AuthenticationSettings.Instance.setSkipBroker(true);`

Sie müssen ein spezielles RedirectUri-Element für die Brokernutzung registrieren. „redirectUri“ liegt im Format `msauth://packagename/Base64UrlencodedSignature` vor. Sie können das RedirectUri-Element für Ihre App mit dem Skript „brokerRedirectPrint.ps1“ abrufen oder den API-Aufruf „mContext.getBrokerRedirectUri“ verwenden. Die Signatur bezieht sich auf Ihre Signaturzertifikate.

Das aktuelle Brokermodell ist für einen einzelnen Benutzer ausgelegt. Über „AuthenticationContext“ wird die API-Methode zum Abrufen des Brokerbenutzers bereitgestellt.

   `String brokerAccount =  mContext.getBrokerUser(); //Broker user is returned if account is valid.`

Ihr App-Manifest sollte über die folgenden Berechtigungen zum Verwenden von AccountManager-Konten verfügen. Einzelheiten finden Sie in den [AccountManager-Informationen auf der Android-Website](http://developer.android.com/reference/android/accounts/AccountManager.html).

* GET_ACCOUNTS
* USE_CREDENTIALS
* MANAGE_ACCOUNTS

### <a name="authority-url-and-ad-fs"></a>Autoritäts-URL und AD FS
Active Directory-Verbunddienste (Active Directory Federation Services, AD FS) werden nicht als STS für die Produktion erkannt. Daher müssen Sie die Instanzenermittlung deaktivieren und für den AuthenticationContext-Konstruktor „false“ übergeben.

Für die Autoritäts-URL sind die STS-Instanz und ein [Mandantenname](https://login.windows.net/yourtenant.onmicrosoft.com) erforderlich.

### <a name="querying-cache-items"></a>Abfragen von Cacheelementen
Die ADAL stellt einen Standardcache unter SharedPreferences mit einigen einfachen Funktionen für die Cacheabfrage bereit. Sie können den aktuellen Cache wie folgt aus AuthenticationContext abrufen:

    ITokenCacheStore cache = mContext.getCache();

Sie können auch eine eigene Cacheimplementierung bereitstellen, falls Sie sie anpassen möchten.

    mContext = new AuthenticationContext(MainActivity.this, authority, true, yourCache);

### <a name="prompt-behavior"></a>Verhalten der Eingabeaufforderung
Die ADAL verfügt über eine Option, mit der Sie das Verhalten der Eingabeaufforderung angeben können. PromptBehavior.Auto zeigt die Benutzeroberfläche an, wenn das Aktualisierungstoken ungültig ist und Benutzeranmeldeinformationen erforderlich sind. Mit PromptBehavior.Always wird die Cachenutzung übersprungen und immer die Benutzeroberfläche angezeigt.

### <a name="silent-token-request-from-cache-and-refresh"></a>Automatische Tokenanforderung aus dem Cache und Aktualisierung
Für eine automatische Tokenanforderung wird die Popup-Benutzeroberfläche nicht verwendet, und eine Aktivität ist nicht erforderlich. Ein Token wird aus dem Cache zurückgegeben, sofern verfügbar. Wenn das Token abgelaufen ist, versucht diese Methode, es zu aktualisieren. Falls das Aktualisierungstoken abgelaufen ist oder einen Fehler verursacht, wird „AuthenticationException“ zurückgegeben.

    Future<AuthenticationResult> result = mContext.acquireTokenSilent(resource, clientid, userId, callback );

Mit dieser Methode können Sie auch einen Synchronisierungsaufruf durchführen. Sie können „null“ auf „callback“ (Rückruf) festlegen oder acquireTokenSilentSync verwenden.

### <a name="diagnostics"></a>Diagnose
Dies sind die wichtigsten Informationsquellen zum Diagnostizieren von Problemen:

* Ausnahmen
* Protokolle
* Netzwerkablaufverfolgung

Beachten Sie, dass die Korrelations-IDs zentraler Bestandteil der Diagnose in der Bibliothek sind. Sie können für Ihre Korrelations-IDs festlegen, dass sie bei Bedarf angefordert werden können, wenn Sie eine ADAL-Anforderung mit anderen Vorgängen in Ihrem Code korrelieren möchten. Wenn Sie keine Korrelations-ID festlegen, generiert die ADAL eine nach dem Zufallsprinzip. Alle Protokollmeldungen und Netzwerkaufrufe werden dann mit der Korrelations-ID versehen. Die selbst generierte ID ändert sich mit jeder Anforderung.

#### <a name="exceptions"></a>Ausnahmen
Ausnahmen sind die erste Diagnosemaßnahme. Wir versuchen, hilfreiche Fehlermeldungen für Sie bereitzustellen. Sie können uns ein Problem melden, falls Sie eine Fehlermeldung finden, die nicht hilfreich ist. Geben Sie dabei auch die Geräteinformationen an, z.B. Modell und SDK-Nummer.

#### <a name="logs"></a>Protokolle
Sie können die Bibliothek so konfigurieren, dass Protokollmeldungen generiert werden, die Sie zum Diagnostizieren von Problemen verwenden können. Sie können die Protokollierung konfigurieren, indem Sie den folgenden Aufruf verwenden. Hiermit können Sie einen Rückruf konfigurieren, der von der ADAL zum Weitergeben der Protokollmeldungen genutzt wird, wenn diese erzeugt werden.

    Logger.getInstance().setExternalLogger(new ILogger() {
        @Override
        public void Log(String tag, String message, String additionalMessage, LogLevel level, ADALError errorCode) {
        ...
        // You can write this to log file depending on level or error code.
        writeToLogFile(getApplicationContext(), tag +":" + message + "-" + additionalMessage);
        }
    }

Meldungen können in eine benutzerdefinierte Protokolldatei geschrieben werden, wie in folgendem Code dargestellt. Leider gibt es keine standardmäßige Möglichkeit, Protokolle von einem Gerät abzurufen. Es sind einige Dienste verfügbar, die Ihnen hierbei behilflich sein können. Sie können auch eigene Wege erfinden, z. B. das Senden einer Datei an einen Server.

    private syncronized void writeToLogFile(Context ctx, String msg) {
       File directory = ctx.getDir(ctx.getPackageName(), Context.MODE_PRIVATE);
       File logFile = new File(directory, "logfile");
       FileOutputStream outputStream = new FileOutputStream(logFile, true);
       OutputStreamWriter osw = new OutputStreamWriter(outputStream);
       osw.write(msg);
       osw.flush();
       osw.close();
    }

Dies sind die Protokolliergrade:
* Error (Ausnahmen)
* Warn (Warnung)
* Info (Informationszwecke)
* Verbose (mehr Details)

Der Protokolliergrad wird wie folgt festgelegt:

    Logger.getInstance().setLogLevel(Logger.LogLevel.Verbose);

 Alle Protokollmeldungen werden zusätzlich zu allen benutzerdefinierten Protokollrückrufen an Logcat gesendet.
Sie können ein Protokoll als Datei wie unten dargestellt aus Logcat abrufen:

    adb logcat > "C:\logmsg\logfile.txt"

 Einzelheiten zu adb-Befehlen finden Sie in den [Informationen zu Logcat auf der Android-Website](https://developer.android.com/tools/debugging/debugging-log.html#startingLogcat).

#### <a name="network-traces"></a>Netzwerkablaufverfolgung
Sie können verschiedene Tools verwenden, um den per ADAL generierten HTTP-Datenverkehr zu erfassen.  Dies ist besonders hilfreich, wenn Sie mit dem OAuth-Protokoll vertraut sind oder wenn Sie für Microsoft oder andere Supportkanäle Diagnoseinformationen bereitstellen müssen.

Fiddler ist das einfachste Tool für die Nachverfolgung von HTTP. Verwenden Sie die folgenden Links, um die Einrichtung der richtigen Aufzeichnung von ADAL-Netzwerkdatenverkehr durchzuführen. Damit ein Tool für Ablaufverfolgungen wie Fiddler oder Charles nützlich ist, müssen Sie es so konfigurieren, dass es nicht verschlüsselten SSL-Datenverkehr erfasst.  

> [!NOTE]
> Ablaufverfolgungen, die mit diesem Verfahren generiert werden, können äußerst sensible Berechtigungsinformationen enthalten, z.B. Zugriffstoken, Benutzernamen und Kennwörter. Wenn Sie Produktionskonten verwenden, sollten Sie diese Ablaufverfolgungen nicht für Dritte freigeben. Wenn Sie für eine Person eine Ablaufverfolgung bereitstellen müssen, um Support zu erhalten, ist das Nachstellen des Problems mit einem temporären Konto mit Benutzernamen und Kennwörtern, für die das Freigeben kein Problem darstellt, ratsam.

* Von der Telerik-Website: [Einrichten von Fiddler für Android](http://docs.telerik.com/fiddler/configure-fiddler/tasks/ConfigureForAndroid)
* Von GitHub: [Konfigurieren von Fiddler-Regeln für ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android/wiki/How-to-listen-to-httpUrlConnection-in-Android-app-from-Fiddler)

### <a name="dialog-mode"></a>Dialogfeldmodus
Die acquireToken-Methode ohne Aktivität unterstützt eine Dialogfeldaufforderung.

### <a name="encryption"></a>Verschlüsselung
Mit ADAL werden die Token verschlüsselt und standardmäßig unter SharedPreferences gespeichert. Sehen Sie sich die StorageHelper-Klasse an, um einen Einblick in die Details zu erhalten. Für Android wurde Android Keystore für 4.3 (API 18) zum sicheren Speichern privater Schlüssel eingeführt. ADAL nutzt dieses Verfahren für API 18 und höher. Falls Sie ADAL für frühere SDK-Versionen verwenden möchten, müssen Sie unter „AuthenticationSettings.INSTANCE.setSecretKey“ einen geheimen Schlüssel angeben.

### <a name="oauth2-bearer-challenge"></a>OAuth2-Beareranforderung
Die AuthenticationParameters-Klasse enthält Funktionen zum Abrufen von „authorization_uri“ aus der OAuth2-Beareranforderung.

### <a name="session-cookies-in-webview"></a>Sitzungscookies in WebView
Android WebView löscht Sitzungscookies nach dem Schließen der App nicht. Dafür können Sie diesen Beispielcode verwenden:

    CookieSyncManager.createInstance(getApplicationContext());
    CookieManager cookieManager = CookieManager.getInstance();
    cookieManager.removeSessionCookie();
    CookieSyncManager.getInstance().sync();

Einzelheiten zu Cookies finden Sie in den [Informationen zu CookieSyncManager auf der Android-Website](http://developer.android.com/reference/android/webkit/CookieSyncManager.html).

### <a name="resource-overrides"></a>Außerkraftsetzungen von Ressourcen
Die ADAL-Bibliothek enthält die englischsprachigen Zeichenfolgen für ProgressDialog-Meldungen. Diese sollten von Ihrer Anwendung überschrieben werden, falls Sie lokalisierte Zeichenfolgen verwenden möchten.

     <string name="app_loading">Loading...</string>
     <string name="broker_processing">Broker is processing</string>
     <string name="http_auth_dialog_username">Username</string>
     <string name="http_auth_dialog_password">Password</string>
     <string name="http_auth_dialog_title">Sign In</string>
     <string name="http_auth_dialog_login">Login</string>
     <string name="http_auth_dialog_cancel">Cancel</string>

### <a name="ntlm-dialog-box"></a>NTLM-Dialogfeld
ADAL, Version 1.1.0, unterstützt ein NTLM-Dialogfeld, das über das onReceivedHttpAuthRequest-Ereignis des WebViewClient-Elements verarbeitet wird. Sie können das Layout und die Zeichenfolgen für das Dialogfeld anpassen.

### <a name="cross-app-sso"></a>App-übergreifendes SSO
Informationen zum Aktivieren von App-übergreifendem SSO unter Android mit ADAL finden Sie [hier](active-directory-sso-android.md).  

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]

