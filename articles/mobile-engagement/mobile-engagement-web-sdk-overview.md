<properties
	pageTitle="Übersicht über das Azure Mobile Engagement Web SDK | Microsoft Azure"
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
	ms.date="06/07/2016"
	ms.author="piyushjo" />


# Web SDK für Azure Mobile Engagement

Beginnen Sie hier, um alle Details zur Integration von Azure Mobile Engagement in eine Web-App zu erhalten. Wenn Sie es zunächst nur probieren möchten, stellen Sie sicher, dass Sie das [15-Minuten-Lernprogramm](mobile-engagement-web-app-get-started.md) durchführen

## Integrationsverfahren
1. Beginnen Sie hier: [Integration von Mobile Engagement in Ihre Web-App](mobile-engagement-web-integrate-engagement.md)

2. Tag-Plan-Implementierung: [Verwenden der erweiterten Mobile Engagement-Tagging-API in Ihrer Web-App](mobile-engagement-web-use-engagement-api.md)

## Versionshinweise

### 2\.0.1 (10.06.2016)

-   SDK für IE8 und IE9 deaktiviert.
-   Erkennung des Webbrowsers Opera korrigiert.

Alle Versionen finden Sie unter [Vollständige Versionshinweise](mobile-engagement-web-release-notes.md).

## Upgrade-Verfahren

### Von 1.2.1 zu 2.0.0

Im Folgenden wird beschrieben, wie Sie die Migration einer SDK-Integration vom Capptain-Dienst, der von Capptain-SAS angeboten wird, in eine App von Azure Mobile Engagement durchführen. Wenn eine Migration von einer früheren Version erfolgt, sehen Sie auf der Capptain-Website nach, wie eine Migration auf Version 1.2.1 durchgeführt wird. Führen Sie anschließend das folgende Verfahren aus.

Diese Version des Engagement Web SDK unterstützt nicht Samsung-tv, OperaTV, WebOS und das „Reach“-Feature.

>[AZURE.IMPORTANT] Capptain und Mobile Engagement sind nicht dieselben Dienste, und die unten beschriebene Vorgehensweise hebt nur hervor, wie die Migration der Clientapp durchzuführen ist. Bei der Migration des SDK in die App werden Ihre Daten NICHT von den Capptain-Servern zu den Mobile Engagement-Servern migriert.

#### JavaScript-Dateien

Ersetzen Sie die Datei `capptain-sdk.js` durch die Datei `azure-engagement.js`, und aktualisieren Sie Ihre Skriptimporte entsprechend.

#### Entfernen von Capptain Reach

Diese Version des Engagement Web SDK unterstützt nicht das „Reach“-Feature. Wenn Sie Capptain Reach in Ihrer Anwendung integriert haben, muss dieses Feature entfernt werden.

Entfernen Sie den Reach-Css-Import von Ihrer Seite, und löschen Sie die zugehörige CSS-Datei (standardmäßig „Capptain-reach.css“).

Löschen Sie die Reach-Ressourcen: Das Symbol „Schließen“ (standardmäßig „Capptain-close.png“) und das Markensymbol (standardmäßig „capptain-notification-icon“).

Entfernen Sie die Reach-Benutzeroberfläche für In-App-Benachrichtigungen. Das Standardlayout sieht wie folgt aus:

	<!-- capptain notification -->
	<div id="capptain_notification_area" class="capptain_category_default">
	  <div class="icon">
	    <img src="capptain-notification-icon.png" alt="icon" />
	  </div>
	  <div class="content">
	    <div class="title" id="capptain_notification_title"></div>
	    <div class="message" id="capptain_notification_message"></div>
	  </div>
	  <div id="capptain_notification_image"></div>
	  <div>
	    <button id="capptain_notification_close">Close</button>
	  </div>
	</div>

Entfernen Sie die Reach-Benutzeroberfläche für Text-\\Webankündigungen und -umfragen. Das Standardlayout sieht wie folgt aus:

	<div id="capptain_overlay" class="capptain_category_default">
	  <button id="capptain_overlay_close">x</button>
	  <div id="capptain_overlay_title"></div>
	  <div id="capptain_overlay_body"></div>
	  <div id="capptain_overlay_poll"></div>
	  <div id="capptain_overlay_buttons">
	    <button id="capptain_overlay_exit"></button>
	    <button id="capptain_overlay_action"></button>
	  </div>
	</div>

Entfernen Sie das `reach`-Objekt aus Ihrer Konfiguration, sofern vorhanden. Es sieht wie folgt aus:

	window.capptain = {
	  [...]
	  reach: {
	    [...]
	  }
	}

Entfernen Sie andere Reach-Anpassungen wie z.B. Kategorien.

#### Entfernen veralteter APIs

Einige der APIs aus Capptain sind in der Engagement-Version des SDK veraltet.

Entfernen Sie alle Aufrufe der folgenden APIs: `agent.connect`, `agent.disconnect`, `agent.pause`, `agent.sendMessageToDevice`.

Entfernen Sie die folgenden Rückrufe, sofern vorhanden, aus Ihrer Capptain-Konfiguration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived`, `onPushMessageReceived`.

#### Konfiguration

Engagement verwendet jetzt eine Verbindungszeichenfolge zum Konfigurieren der SDK-Bezeichner wie dem Anwendungsbezeichner.

Ersetzen Sie die Anwendungs-ID durch Ihre Verbindungszeichenfolge. Beachten Sie auch, dass das globale Objekt für die SDK-Konfiguration sich von `capptain` in `azureEngagement` ändert.

Vor der Migration:

	window.capptain = {
	  appId: ...,
	  [...]
	};

Nach der Migration:

	window.azureEngagement = {
	  connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
	  [...]
	};

Die Verbindungszeichenfolge für Ihre Anwendung wird im Azure-Portal angezeigt.

#### JavaScript-APIs

Das globale JavaScript-Objekt `window.capptain` wurde in `window.azureEngagement` umbenannt. Sie können aber den `window.engagement`-Alias für API-Aufrufe verwenden (Sie können den Alias nicht zum Definieren der SDK-Konfiguration verwenden).

Beispiel: `capptain.deviceId` wird `engagement.deviceId`, `capptain.agent.startActivity` wird `engagement.agent.startActivity` usw.

Wenn Sie bereits eine ältere Version unseres SDKs in die Anwendung integriert haben, finden Sie Informationen dazu in [Upgrade-Verfahren](mobile-engagement-web-upgrade-procedure.md).

<!---HONumber=AcomDC_0615_2016-->