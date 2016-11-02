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
	ms.date="08/02/2016"
	ms.author="piyushjo;ricksal" />

# Erweiterte Konfiguration für das Android-SDK für Azure Mobile Engagement

> [AZURE.SELECTOR]
- [Universal Windows](mobile-engagement-windows-store-advanced-configuration.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-logging.md)

Hier wird erläutert, wie verschiedene Konfigurationsoptionen für Android-Apps in Azure Mobile Engagement konfiguriert werden.

## Voraussetzungen

[AZURE.INCLUDE [Voraussetzungen](../../includes/mobile-engagement-android-prereqs.md)]

## Erforderliche Berechtigungen
Für manche Optionen sind bestimmte Berechtigungen erforderlich, die hier zu Referenzzwecken alle aufgeführt sind und bei der jeweiligen Funktion genannt werden. Fügen Sie diese Berechtigungen unmittelbar vor oder nach dem `<application>`-Tag der Datei AndroidManifest.xml des Projekts hinzu.

Der Berechtigungscode muss ähnlich dem folgenden Code aussehen, in dem Sie die entsprechende Berechtigung aus der nachstehenden Tabelle einfügen.

	<uses-permission android:name="android.permission.[specific permission]"/>


| Berechtigung | Verwendung |
| ---------- | --------- |
| INTERNET | Erforderlich. Für grundlegende Berichterstellung |
| ACCESS\_NETWORK\_STATE | Erforderlich. Für grundlegende Berichterstellung |
| RECEIVE\_BOOT\_COMPLETED | Erforderlich. Zum Anzeigen des Benachrichtigungs-Centers nach dem Geräteneustart |
| WAKE\_LOCK | Empfohlen. Ermöglicht die Datenerfassung im WLAN oder bei ausgeschaltetem Bildschirm. |
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

Der Burstmodus verlängert leicht die Akkulaufzeit, wirkt sich jedoch auf den Engagement-Monitor aus: Die Dauer von allen Sitzungen und Aufträgen wird auf den Burstschwellenwert gerundet (folglich sind eventuell Sitzungen und Aufträge, die kürzer als der Burstschwellenwert sind, möglicherweise nicht sichtbar). Der Burstschwellenwert sollte 30.000 (30s) nicht überschreiten.

### Sitzungstimeout

 Sie können eine Aktivität beenden, indem Sie die **POS1**- oder **RÜCK**-Taste drücken, das Telefon in den Ruhezustand setzen oder zu einer anderen Anwendung wechseln. Standardmäßig wird eine Sitzung zehn Sekunden nach dem Ende ihrer letzten Aktivität beendet. Dies vermeidet eine Sitzungsteilung, wenn der Benutzer die Sitzung beendet und schnell zur Anwendung zurückkehrt, was bei einer Bildübernahme durch den Benutzer, beim Prüfen einer Benachrichtigung usw. der Fall sein kann. Sie können diesen Parameter ändern. Fügen Sie dazu folgenden Code zwischen dem `<application>`- und dem `</application>`-Tag hinzu:

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
-   Der `engagement:agent:settings:mode`-Schlüssel wird zum Definieren des Modus der freigegebenen Einstellungsdatei verwendet. Verwenden Sie den gleichen Modus wie in Ihrer `PreferenceActivity`. Der Modus muss als Zahl übergeben werden. Wenn Sie eine Kombination von konstanten Flags im Code verwenden, überprüfen Sie den Gesamtwert.

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

<!---HONumber=AcomDC_0817_2016-->