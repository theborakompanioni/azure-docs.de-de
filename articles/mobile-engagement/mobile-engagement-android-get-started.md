<properties
    pageTitle="Erste Schritte mit Azure Mobile Engagement für Android-Apps"
    description="Erfahren Sie mehr über die Verwendung von Azure Mobile Engagement mit Analysefunktionen und Pushbenachrichtigungen für Android-Apps."
    services="mobile-engagement"
    documentationCenter="android"
    authors="piyushjo"
    manager="erikre"
    editor="" />

<tags
    ms.service="mobile-engagement"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="Java"
    ms.topic="hero-article"
    ms.date="08/10/2016"
    ms.author="piyushjo;ricksal" />


# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Erste Schritte mit Azure Mobile Engagement für Android-Apps

[AZURE.INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In diesem Thema erfahren Sie, wie Sie mithilfe von Azure Mobile Engagement die Nutzung Ihrer App verstehen können und wie Sie Pushbenachrichtigungen an segmentierte Benutzer einer Android-Anwendung senden.
In diesem Tutorial wird ein einfaches Übertragungsszenario mit Mobile Engagement dargestellt. Darin erstellen Sie eine leere Android-App, die einfache Daten erfasst und Pushbenachrichtigungen über Google Cloud Messaging (GCM) empfängt.

## <a name="prerequisites"></a>Voraussetzungen

Sie benötigen die [Android Developer Tools](https://developer.android.com/sdk/index.html), die die integrierte Entwicklungsumgebung Android Studio enthalten, sowie die neueste Android-Plattform.

Außerdem ist das [Mobile Engagement Android SDK](https://aka.ms/vq9mfn)erforderlich.

> [AZURE.IMPORTANT] Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Konto. Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Einrichten von Mobile Engagement für Ihre Android-App

[AZURE.INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Verbinden der App mit dem Mobile Engagement-Back-End

In diesem Lernprogramm wird eine "einfache Integration" dargestellt. Dabei handelt es sich um den minimalen erforderlichen Satz zur Sammlung von Daten und zum Senden einer Pushbenachrichtigung. Sie erstellen eine einfache App mit Android Studio, um die Integration zu veranschaulichen.

Die vollständige Dokumentation zur Integration finden Sie unter [Android SDK für Mobile Engagement](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Erstellen eines Android-Projekts

1. Starten Sie **Android Studio**, und wählen Sie im Popupfenster die Option **Start a new Android Studio project** (Neues Android Studio-Projekt starten) aus.

    ![][1]

2. Geben Sie einen App-Namen und eine Unternehmensdomäne ein. Notieren Sie sich Ihre Angaben, da Sie sie später noch benötigen. Klicken Sie auf **Next**.

    ![][2]

3. Wählen Sie den Zielformfaktor und die API-Ebene aus, und klicken Sie auf **Next**.

    >[AZURE.NOTE] Mobile Engagement erfordert mindestens „API Level 10“ (Android 2.3.3).

    ![][3]

4. Wählen Sie hier **Blank Activity** (Leere Aktivität) aus (dies ist der einzige Bildschirm für diese App), und klicken Sie auf **Next** (Weiter).

    ![][4]

5. Übernehmen Sie schließlich die Standardeinstellungen, und klicken Sie auf **Finish**.

    ![][5]

Android Studio erstellt jetzt die Demo-App, in die wir Mobile Engagement integrieren.

### <a name="include-the-sdk-library-in-your-project"></a>Schließen Sie die SDK-Bibliothek in Ihr Projekt ein

1. Laden Sie das [Mobile Engagement Android SDK](https://aka.ms/vq9mfn)herunter.
2. Extrahieren Sie die Archivdatei in einem Ordner auf Ihrem Computer.
3. Ermitteln Sie die JAR-Bibliothek für die aktuelle Version dieses SDKs, und kopieren Sie sie in die Zwischenablage.

      ![][6]

4. Navigieren Sie zum **Projektabschnitt** (1), und fügen Sie die JAR-Datei in den Ordner "libs" (2) ein.

      ![][7]

5. Synchronisieren Sie das Projekt, um die Bibliothek zu laden.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Verbinden der App mit Mobile Engagement-Back-End mithilfe der Verbindungszeichenfolge

1. Kopieren Sie die folgenden Codezeilen in die Erstellung der Aktivität (muss nur an einer Stelle der App vorgenommen werden, in der Regel in der Hauptaktivität). Öffnen Sie für diese Beispiel-App "MainActivity" im Ordner "Src" -> "main" -> "java", und fügen Sie Folgendes hinzu:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);

2. Lösen Sie die Verweise auf, indem Sie ALT+EINGABE drücken oder die folgenden import-Anweisungen hinzufügen:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;

3. Wechseln Sie auf der Seite **Verbindungsinformationen** Ihrer App zurück zum klassischen Azure-Portal, und kopieren Sie die **Verbindungszeichenfolge**.

      ![][9]

4. Fügen Sie sie in den Parameter `setConnectionString` ein, indem Sie die gesamte Zeichenfolge im folgenden Code ersetzen:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Hinzufügen von Berechtigungen und einer Dienstdeklaration

1. Fügen Sie diese Berechtigungen unmittelbar vor oder nach dem `<application>`-Tag zur Datei „Manifest.xml“ des Projekts hinzu:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>

2. Fügen Sie diesen Code zum Deklarieren des Agent-Diensts zwischen den Tags `<application>` und `</application>` ein:

        <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>"
            android:process=":Engagement"/>

3. Ersetzen Sie im eingefügten Code `"<Your application name>"` in der Beschriftung, die im Menü **Einstellungen** angezeigt wird. Hier sehen Sie die Dienste, die auf dem Gerät ausgeführt werden. Sie können z. B. das Wort "Dienst" in der Beschriftung hinzufügen.

### <a name="send-a-screen-to-mobile-engagement"></a>Senden eines Bildschirms an Mobile Engagement

Um mit dem Senden von Daten zu beginnen und sicherzustellen, dass die Benutzer aktiv sind, müssen Sie mindestens einen Bildschirm (Aktivität) an das Mobile Engagement-Back-End senden.

Wechseln Sie zu **Mainactivity.java**, und fügen Sie Folgendes hinzu, um die Basisklasse von **MainActivity** durch **EngagementActivity** zu ersetzen:

    public class MainActivity extends EngagementActivity {

> [AZURE.NOTE] Lautet die Basisklasse nicht *Aktivität*, lesen Sie die Informationen zum Erben von unterschiedlichen Klassen unter [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes) (Erweiterte Android-Berichte).


Kommentieren Sie die folgende Zeile für dieses einfache Beispielszenario aus:

    // setSupportActionBar(toolbar);

Wenn Sie das `ActionBar` -Element in Ihrer App beibehalten möchten, helfen Ihnen die Informationen unter [Berichtserstellungsoptionen mit Engagement unter Android](mobile-engagement-android-advanced-reporting.md#modifying-your-codeactivitycode-classes)weiter.

## <a name="connect-app-with-real-time-monitoring"></a>Verbinden der App mit Überwachung in Echtzeit

[AZURE.INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Aktivieren von Pushbenachrichtigungen und In-App-Messaging

Während einer Kampagne können Sie mit Mobile Engagement mit Ihren Benutzern interagieren und diese erreichen (REACH), indem Sie Pushbenachrichtigungen und In-App-Messaging verwenden. Dieses Modul nennt sich REACH im Mobile Engagement-Portal.
Im folgenden Abschnitt richten Sie Ihre App für den Empfang ein.

### <a name="copy-sdk-resources-in-your-project"></a>Kopieren von SDK-Ressourcen in Ihrem Projekt

1. Navigieren Sie wieder zum heruntergeladenen SDK-Inhalt, und kopieren Sie den Ordner **res** .

    ![][10]

2. Wechseln Sie zurück zu Android Studio, und wählen Sie das Verzeichnis **main** mit den Projektdateien aus. Fügen Sie dieses anschließend ein, um die Ressourcen dem Projekt hinzuzufügen.

    ![][11]

[AZURE.INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[AZURE.INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[AZURE.INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zum [Android-SDK](mobile-engagement-android-sdk-overview.md) , um ausführliche Informationen zur SDK-Integration zu erhalten.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png



<!--HONumber=Oct16_HO2-->


