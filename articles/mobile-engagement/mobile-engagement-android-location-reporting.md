---
title: Speicherort-Berichterstellung für das Android-SDK für Azure Mobile Engagement
description: Hier wird beschrieben, wie Sie die Speicherort-Berichterstellung für das Android-SDK für Azure Mobile Engagement-konfigurieren.
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: ''

ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal

---
# Speicherort-Berichterstellung für das Android-SDK für Azure Mobile Engagement
> [!div class="op_single_selector"]
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

In diesem Thema wird die Berichterstellung für Speicherorte für Ihre Android-Anwendung erläutert.

## Voraussetzungen
[!INCLUDE [Voraussetzungen](../../includes/mobile-engagement-android-prereqs.md)]

## Speicherortberichte
Wenn Sie die Speicherorte melden möchten, müssen Sie einige Zeilen zur Konfiguration hinzufügen (zwischen den Tags `<application>` und `</application>`).

### Verzögerte Bereichsspeicherortberichte
Die verzögerte Berichterstellung für Bereichsspeicherorte ermöglicht Ihnen, das Land, die Region und den Ort zu melden, die Geräten zugeordnet sind. Diese Art der Berichterstellung für Speicherorte verwendet ausschließlich Netzwerkspeicherorte (auf Basis von Zell-ID oder WLAN). Der Gerätebereich wird höchstens einmal pro Sitzung gemeldet. Das GPS wird niemals verwendet, daher hat diese Art von Standortbericht nur geringe Auswirkungen auf den Akku.

Die gemeldeten Bereiche werden dazu verwendet, um geografische Statistiken zu Benutzern, Sitzungen, Ereignissen und Fehlern zu berechnen. Sie können auch als Kriterium für Reach-Kampagnen verwendet werden.

Verzögerte Bereichsspeicherortberichte aktivieren Sie mithilfe der zuvor in diesem Verfahren erwähnten Konfiguration:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen auch eine Speicherortberechtigung angeben. Dieser Code verwendet die ``COARSE``-Berechtigung:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Wenn Ihre App sie erfordert, können Sie stattdessen ``ACCESS_FINE_LOCATION`` verwenden.

### Echtzeit-Berichterstellung für Speicherorte
Mithilfe der Echtzeit-Berichterstellung für Speicherorte können der Längen- und Breitengrad gemeldet werden, die Geräten zugeordnet sind. Diese Art der Berichterstellung für Speicherorte verwendet standardmäßig ausschließlich Netzwerkspeicherorte (auf Basis von Zell-ID oder WLAN). Die Berichterstellung ist nur aktiv, wenn die Anwendung im Vordergrund ausgeführt wird (z.B. während einer Sitzung).

Echtzeit-Speicherorte werden *NICHT* zum Berechnen von Statistiken verwendet. Ihr einziger Zweck ist, die Verwendung des Echtzeit-Geofencing <Reach-Audience-geofencing>-Kriteriums in Reach-Kampagnen zu ermöglichen.

Um die Echtzeit-Berichterstellung für Speicherorte zu aktivieren, fügen Sie eine Codezeile hinzu, in der Sie die Engagement-Verbindungszeichenfolge in der Startprogrammaktivität festlegen. Das Ergebnis sieht wie folgt aus:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### GPS-basierte Berichterstellung
Die Echtzeit-Berichterstellung für Speicherorte verwendet standardmäßig nur netzwerkbasierte Speicherorte. Verwenden Sie das Konfigurationsobjekt, um die Verwendung von viel genaueren GPS-basierten Speicherorten zu aktivieren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

Sie müssen auch die folgende Berechtigung hinzufügen, sofern diese noch nicht vorhanden ist:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### Berichterstellung im Hintergrund
Die Echtzeit-Berichterstellung für Speicherorte ist standardmäßig nur aktiv, wenn die Anwendung im Vordergrund ausgeführt wird (z.B. während einer Sitzung). Verwenden Sie das folgende Konfigurationsobjekt, um die Berichterstellung auch im Hintergrund zu aktivieren:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> Wenn die Anwendung im Hintergrund ausgeführt wird, werden nur netzwerkbasierte Speicherorte gemeldet, auch wenn Sie das GPS aktiviert haben.
> 
> 

Wenn der Benutzer das Gerät neu startet, wird der Hintergrundbericht über Speicherorte beendet. Damit er beim Starten des Geräts automatisch neu gestartet wird, fügen Sie diesen Code hinzu.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

Sie müssen auch die folgende Berechtigung hinzufügen, sofern diese noch nicht vorhanden ist:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## Android M-Berechtigungen
Ab Android M werden bestimmte Berechtigungen zur Laufzeit verwaltet und erfordern die Genehmigung des Benutzers.

Wenn Sie auf die Android-API-Ebene 23 abzielen, werden die Laufzeitberechtigungen für neue App-Installationen standardmäßig deaktiviert. Andernfalls sind sie standardmäßig aktiviert.

Sie können diese Berechtigungen im Einstellungsmenü des Geräts aktivieren oder deaktivieren. Das Deaktivieren der Berechtigungen im Systemmenü beendet alle Hintergrundprozesse der Anwendung. Dabei handelt es sich um ein Systemverhalten, das keine Auswirkungen auf die Möglichkeit hat, Pushbenachrichtigungen im Hintergrund zu empfangen.

Für die Berichterstellung für Speicherorte in Mobile Engagement müssen folgende Berechtigungen zur Laufzeit genehmigt werden:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

Fordern Sie über ein Standarddialogfeld Berechtigungen vom Benutzer an. Wenn der Benutzer zustimmt, teilen Sie ``EngagementAgent`` mit, diese Änderung in Echtzeit zu berücksichtigen. Andernfalls wird die Änderung beim nächsten Start der Anwendung durch den Benutzer ausgeführt.

Hier ist ein Codebeispiel, dass Sie in einer Aktivität Ihrer Anwendung verwenden können, um Berechtigungen anzufordern und das Ergebnis (wenn positiv) weiterzuleiten an ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

<!---HONumber=AcomDC_0817_2016-->