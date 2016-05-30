<properties
	pageTitle="Erweiterte Konfiguration für das Android-SDK für Azure Mobile Engagement"
	description="Beschreibt die erweiterten Konfigurationsoptionen, einschließlich des Android-Manifests mit dem Android-SDK für Azure Mobile Engagement."
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="Java"
	ms.topic="article"
	ms.date="05/12/2016"
	ms.author="piyushjo;ricksal" />

# Erweiterte Konfiguration für das Android-SDK für Azure Mobile Engagement

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-logging.md)

Hier wird erläutert, wie verschiedene Konfigurationsoptionen für Android-Apps in Azure Mobile Engagement konfiguriert werden.

## Voraussetzungen

[AZURE.INCLUDE [Voraussetzungen](../../includes/mobile-engagement-android-prereqs.md)]

## Erforderliche Berechtigungen
Für etliche Optionen sind bestimmte Berechtigungen erforderlich, die hier zu Referenzzwecken alle aufgeführt sind und bei der jeweiligen Funktion genannt werden. Fügen Sie diese Berechtigungen unmittelbar vor oder nach dem `<application>`-Tag in der Datei „AndroidManifest.xml“ des Projekts hinzu.

Der Berechtigungscode muss ähnlich dem folgenden Code aussehen, in dem Sie die entsprechende Berechtigung aus der nachstehenden Tabelle einfügen.

	<uses-permission android:name="android.permission.[specific permission]"/>


| Berechtigung | Verwendung |
| ---------- | --------- |
| INTERNET | Erforderlich. Für grundlegende Berichterstellung |
| ACCESS\_NETWORK\_STATE | Erforderlich. Für grundlegende Berichterstellung |
| RECEIVE\_BOOT\_COMPLETED | Erforderlich. Zum Anzeigen des Benachrichtigungs-Centers nach dem Geräteneustart |
| WAKE\_LOCK | Dringend empfohlen. Ermöglicht die Datenerfassung im WLAN oder bei ausgeschaltetem Bildschirm. |
| VIBRATE | Optional. Aktiviert Vibration beim Eingang von Nachrichten. |
| DOWNLOAD\_WITHOUT\_NOTIFICATION | Optional. Aktiviert Android-Benachrichtigungen mit großen Bildern. |
| WRITE\_EXTERNAL\_STORAGE | Optional. Aktiviert Android-Benachrichtigungen mit großen Bildern. |
| ACCESS\_COARSE\_LOCATION | Optional. Aktiviert die Echtzeit-Berichterstellung für Speicherorte. |
| ACCESS\_FINE\_LOCATION | Optional. Aktiviert die GPS-basierte Berichterstellung für Speicherorte. |

Ab Android M werden [bestimmte Berechtigungen zur Laufzeit verwaltet](mobile-engagement-android-location-reporting.md#Android-M-Permissions).

Wenn Sie bereits ``ACCESS_FINE_LOCATION`` verwenden, müssen Sie nicht auch noch ``ACCESS_COARSE_LOCATION`` verwenden.

## Konfigurationsoptionen für das Android-Manifest

### Absturzbericht

Fügen Sie folgenden Code (zwischen den Tags `<application>` und `</application>`) hinzu, um Absturzberichte zu deaktivieren:

	<meta-data android:name="engagement:reportCrash" android:value="false"/>

### Burstschwellenwert

Standardmäßig meldet der Engagement-Dienst Protokolle in Echtzeit. Wenn Ihre Anwendung Protokolle sehr häufig meldet, ist es besser, die Protokolle zu puffern und sie in regelmäßigen Abständen alle auf einmal zu melden (dies wird als „Burstmodus“ bezeichnet). Fügen Sie dazu folgenden Code zwischen dem `<application>`- und dem `</application>`-Tag hinzu:

	<meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

Der Burst-Modus verlängert leicht die Akkulaufzeit, wirkt sich jedoch auf den Engagement-Monitor aus: Die Dauer von allen Sitzungen und Aufträgen wird auf den Burst-Schwellenwert gerundet (folglich sind eventuell Sitzungen und Aufträge, die kürzer als der Burst-Schwellenwert sind, möglicherweise nicht sichtbar). Es wird empfohlen, einen Burst-Schwellenwert von höchstens 30000 (30 s) zu verwenden.

### Sitzungstimeout

Eine Sitzung wird standardmäßig 10 Sekunden nach dem Ende ihrer letzten Aktivität beendet (was in der Regel durch Drücken der Start- oder Zurück-Taste, durch den Wechsel des Mobiltelefons in den Leerlauf oder durch den Wechsel zu einer anderen Anwendung ausgelöst wird). Dadurch wird eine Sitzungsteilung vermieden, wenn der Benutzer die Sitzung beendet und sehr schnell zur Anwendung zurückkehrt (dies kann bei einer Bildübernahme, beim Prüfen einer Benachrichtigung usw. auftreten). Sie können diesen Parameter ändern. Fügen Sie dazu Folgendes (zwischen den Tags `<application>` und `</application>`) hinzu:

	<meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## Deaktivieren der Protokollberichterstellung

### Verwenden eines Methodenaufrufs

Sie können Folgendes aufrufen, wenn von Engagement keine Protokolle mehr gesendet werden sollen:

	EngagementAgent.getInstance(context).setEnabled(false);

Dieser Aufruf ist persistent: er verwendet eine freigegebene Einstellungsdatei.

Wenn Engagement beim Aufrufen dieser Funktion aktiv ist, kann es bis zu einer Minute dauern, bis der Dienst beendet wird. Beim nächsten Start der Anwendung wird der Dienst jedoch überhaupt nicht gestartet.

Sie können die Protokollberichterstellung erneut aktivieren, indem Sie dieselbe Funktion mit `true` aufrufen.

### Integration in eigenem `PreferenceActivity`

Anstatt diese Funktion aufzurufen, können Sie diese Einstellung auch direkt im vorhandenen `PreferenceActivity` integrieren.

Sie können Engagement für die Verwendung Ihrer Einstellungsdatei (mit gewünschtem Modus) in der Datei `AndroidManifest.xml` mithilfe von `application meta-data` konfigurieren:

-   Der `engagement:agent:settings:name`-Schlüssel wird zum Definieren des Namens der freigegebenen Einstellungsdatei verwendet.
-   Der `engagement:agent:settings:mode`-Schlüssel wird zum Definieren des Modus der freigegebenen Einstellungsdatei verwendet. Sie sollten denselben Modus wie im `PreferenceActivity` verwenden. Der Modus muss als Zahl übergeben werden. Wenn Sie eine Kombination von konstanten Flags im Code verwenden, überprüfen Sie den Gesamtwert.

Engagement verwendet immer den booleschen `engagement:key`-Schlüssel innerhalb der Einstellungsdatei zum Verwalten dieser Einstellung.

Das folgende Beispiel von `AndroidManifest.xml` veranschaulicht die Standardwerte:

	<application>
	    [...]
	    <meta-data
	      android:name="engagement:agent:settings:name"
	      android:value="engagement.agent" />
	    <meta-data
	      android:name="engagement:agent:settings:mode"
	      android:value="0" />

Dann können Sie `CheckBoxPreference` wie folgt im Einstellungslayout hinzufügen:

	<CheckBoxPreference
	  android:key="engagement:enabled"
	  android:defaultValue="true"
	  android:title="Use Engagement"
	  android:summaryOn="Engagement is enabled."
	  android:summaryOff="Engagement is disabled." />

<!---HONumber=AcomDC_0518_2016-->