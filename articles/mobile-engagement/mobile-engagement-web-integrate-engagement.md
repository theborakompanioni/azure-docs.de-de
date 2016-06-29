<properties
	pageTitle="Integration des Azure Mobile Engagement Web SDK | Microsoft Azure"
	description="Neueste Updates und Verfahren für das Web SDK für Azure Mobile Engagement"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="web"
	ms.devlang="js"
	ms.topic="article"
	ms.date="02/29/2016"
	ms.author="piyushjo" />

#Integrieren von Mobile Engagement in eine Webanwendung

> [AZURE.SELECTOR]
- [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
- [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
- [iOS](mobile-engagement-ios-integrate-engagement.md)
- [Android](mobile-engagement-android-integrate-engagement.md)

In diesem Verfahren wird die einfachste Art der Aktivierung der Analyse- und Überwachungsfunktionen von Mobile Engagement in Ihrer Webanwendung beschrieben.

Mithilfe der folgenden Schritte kann der Protokollbericht aktiviert werden, der zum Berechnen aller Statistiken zu Benutzern, Sitzungen, Aktivitäten, Abstürzen und technischen Informationen erforderlich ist. Der Bericht von Protokollen, die zur Berechnung anderer Statistiken wie Ereignisse, Fehler und Aufträge erforderlich ist, muss manuell mithilfe der Mobile Engagement-API erfolgen (siehe [Verwenden der erweiterten Mobile Engagement-API für Tags in einer Webanwendung](mobile-engagement-web-use-engagement-api.md)), da diese Statistiken anwendungsabhängig sind.

## Einführung

Laden Sie das Web SDK [hier](http://aka.ms/P7b453) herunter. Das SDK wird als einzelne JavaScript-Datei namens **azure-engagement.js** bereitgestellt, die Sie allen Seiten Ihrer Website oder Webanwendung hinzufügen müssen.

Dieses Skript **muss** **nach** einem Skript oder Codeausschnitt geladen werden, den Sie schreiben müssen, um Mobile Engagement für Ihre Anwendung zu konfigurieren.

## Browserkompatibilität

Das Mobile Engagement Web SDK verwendet eine native JSON-Codierung-/Decodierung und domänenübergreifende AJAX-Anforderungen (auf Basis der W3C CORS-Spezifikation).

* Edge 12 und höher
* IE 10 und höher
* Firefox 3.5 und höher
* Chrome 4 und höher
* Safari 6 und höher
* Opera 12 und höher

## Konfigurieren von Mobile Engagement

Schreiben Sie wie folgt ein Skript zur Erstellung eines globalen JavaScript-Objekt des Typs **AzureEngagement**.
 
Da Ihre Website ggf. mehrere Seiten enthält, wird bei diesem Beispiel vorausgesetzt, dass dieses Skript allen Seiten hinzugefügt wird. In diesem Verfahren nennen wir es `azure-engagement-conf.js`.

	window.azureEngagement = {
	  connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
	  appVersionName: '1.0.0',
	  appVersionCode: 1
	};

Die `connectionString` für die Anwendung wird im Azure-Portal angezeigt.

> [AZURE.NOTE] `appVersionName` und `appVersionCode` sind optional. Es wird empfohlen, sie so zu konfigurieren, dass für die Analyse Versionsinformationen verarbeitet werden können.

## Hinzufügen von Mobile Engagement-Skripts zu Ihren Seiten

	<head>
	  ...
	  <script type="text/javascript" src="azure-engagement-conf.js"></script>
	  <script type="text/javascript" src="azure-engagement.js"></script>
	  ...
	</head>

Oder

	<body>
	  ...
	  <script type="text/javascript" src="azure-engagement-conf.js"></script>
	  <script type="text/javascript" src="azure-engagement.js"></script>
	  ...
	</body>

## Alias

Nach dem Laden erstellt das SDK-Skript den **Engagement**-Alias für den Zugriff auf SDK-APIs (der nicht zum Definieren der SDK-Konfiguration verwendet werden kann). Dieser Alias wird in dieser Dokumentation als Verweis verwendet.

Beachten Sie, dass wenn der Standardalias im Konflikt mit einer anderen globalen Variablen auf Ihrer Seite steht, Sie ihn in der Konfiguration wie folgt neu definieren können, bevor das SDK geladen wird:

	window.azureEngagement = {
	  connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
	  appVersionName: '1.0.0',
	  appVersionCode: 1
	  alias:'anotherAlias'
	};

## Grundlegende Berichterstellung

### Sitzungsnachverfolgung

Eine Mobile Engagement-Sitzung ist in eine Abfolge von Aktivitäten aufgeteilt, die von einem Namen bestimmt werden.

Für eine klassische Website wird empfohlen, eine andere Aktivität auf jeder Seite Ihrer Website zu deklarieren. Für eine Website oder Webanwendung, die die aktuelle Seite nie ändert, möchten Sie die Aktivitäten ggf. etwas genauer verfolgen.

In beiden Fällen starten oder ändern Sie die aktuelle Benutzeraktivität durch Aufrufen der `engagement.agent.startActivity`-Funktion wie in diesem Beispiel:

	<body onload="yourOnload()">

	<!-- -->

	yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
	};

Eine offene Sitzung wird automatisch vom Engagement-Server binnen drei Minuten nach Schließen der Anwendungsseite beendet.

Alternativ können Sie eine Sitzung auch manuell beenden, indem Sie `engagement.agent.endActivity` aufrufen. Dadurch wird die aktuelle Benutzeraktivität als „Inaktiv“ festgelegt und die Sitzung 10 Sekunden später beendet, es sei denn, die Sitzung wird durch einen erneuten Aufruf von `engagement.agent.startActivity` in der Zwischenzeit fortgesetzt.
 
Diese 10-sekündige Verzögerung kann im globalen **engagement**-Objekt konfiguriert werden:

	engagement.sessionTimeout = 2000; // 2 seconds
	// or
	engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [AZURE.NOTE] `engagement.agent.endActivity` kann nicht im Rückruf von `onunload` verwendet werden, da Ajax-Aufrufe in dieser Phase nicht möglich sind.

## Erweiterte Berichterstellung

Wenn Sie optional anwendungsspezifische `events`, `errors` und `jobs` melden möchten, müssen Sie die Engagement-API über das `engagement.agent`-Objekt verwenden.

Die Engagement-API gestattet die Verwendung aller erweiterten Mobile Engagement-Funktionen und wird im Abschnitt [Verwenden der erweiterten Mobile Engagement-API für Tags in einer Webanwendung](mobile-engagement-web-use-engagement-api.md) ausführlich beschrieben.

## Anpassen der für AJAX-Aufrufe verwendeten URLs

Sie können die vom SDK verwendeten URLs anpassen. Um beispielsweise die Protokoll-URL (den SDK-Endpunkt für die Protokollierung) neu zu definieren, können Sie die Konfiguration wie folgt überschreiben:

	window.azureEngagement = {
	  ...
	  urls: {
	    ...        
	    getLoggerUrl: function() {
	    return 'someProxy/log';
	    }
	  }
	};

Wenn Ihre URL-Funktionen eine mit `/`, `//`, `http://` oder `https://` beginnende Zeichenfolge zurückgibt, wird das Standardschema nicht verwendet.

Standardmäßig wird das `https://`-Schema für diese URLs verwendet. Wenn Sie das Standardschema anpassen möchten, überschreiben Sie die Konfiguration wie folgt:

	window.azureEngagement = {
	  ...
	  urls: {
        ...	     
	    scheme: '//'
	  }
	};

<!---HONumber=AcomDC_0615_2016-->