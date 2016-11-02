<properties
	pageTitle="Übersicht über das Azure Mobile Engagement Web SDK | Microsoft Azure"
	description="Die neuesten Updates und Verfahren für das Web SDK für Azure Mobile Engagement"
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


# Azure Mobile Engagement Web SDK

Beginnen Sie hier, um alle Details zur Integration von Azure Mobile Engagement in eine Web-App zu erhalten. Sehen Sie sich unser [15-minütiges Tutorial](mobile-engagement-web-app-get-started.md) an, wenn Sie sich erst informieren möchten, bevor Sie mit einer eigenen Web-App beginnen.

## Integrationsverfahren
1. Erfahren Sie, wie Sie [Mobile Engagement in Ihre Web-App integrieren](mobile-engagement-web-integrate-engagement.md).

2. Lesen Sie die Informationen zur Tag-Plan-Implementierung unter [Verwenden der Mobile Engagement-API in einer Web-App](mobile-engagement-web-use-engagement-api.md).

## Versionshinweise

### 2\.0.1 (10.06.2016)

-   Das Mobile Engagement Web SDK in Internet Explorer 8 und Internet Explorer 9 wurde deaktiviert.
-   Erkennung des Webbrowsers Opera korrigiert.

Alle Versionen finden Sie unter [Vollständige Versionshinweise](mobile-engagement-web-release-notes.md).

## Upgrade-Verfahren

### Upgrade von 1.2.1 auf 2.0.0

In den folgenden Abschnitten wird beschrieben, wie Sie die Migration des Mobile Engagement Web SDK vom Capptain-Dienst, die von Capptain SAS angeboten wird, in eine Azure Mobile Engagement-App durchführen. Wenn eine Migration von einer älteren Version als 1.2.1 erfolgt, sehen Sie auf der Capptain-Website nach, wie eine Migration zu Version 1.2.1 durchgeführt wird. Führen Sie anschließend die folgenden Schritte aus.

Diese Version des Mobile Engagement Web SDK unterstützt nicht Samsung Smart TV, Opera TV, webOS und das „Reach“-Feature.

>[AZURE.IMPORTANT] Capptain und Azure Mobile Engagement sind nicht dieselben Dienste, und die folgenden Vorgehensweisen heben nur hervor, wie die Client-App migriert wird. Bei der Migration des Mobile Engagement Web SDK in die App werden Ihre Daten nicht von einem Capptain-Server auf einen Mobile Engagement-Server migriert.

#### JavaScript-Dateien

Ersetzen Sie die Datei „capptain-sdk.js“ durch die Datei „Azure-engagement.js“, und aktualisieren Sie Ihre Skriptimporte entsprechend.

#### Entfernen von Capptain Reach

Diese Version des Mobile Engagement Web SDK unterstützt nicht das „Reach“-Feature. Wenn Sie Capptain Reach in Ihre Anwendung integriert haben, müssen Sie dieses Feature entfernen.

Entfernen Sie den Reach-CSS-Import von Ihrer Seite, und löschen Sie die zugehörige CSS-Datei (standardmäßig „capptain-reach.css“).

Löschen Sie die folgenden Reach-Ressourcen: Das Symbol „Schließen“ (standardmäßig „capptain-close.png“) und das Markensymbol (standardmäßig „capptain-notification-icon“).

Entfernen Sie die Reach-Benutzeroberfläche für In-App-Benachrichtigungen. Das Standardlayout sieht folgendermaßen aus:

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

Entfernen Sie die Reach-Benutzeroberfläche für Text- und Webankündigungen und Umfragen. Das Standardlayout sieht folgendermaßen aus:

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

Entfernen Sie das `reach`-Objekt ggf. aus Ihrer Konfiguration. Es sieht folgendermaßen aus:

	window.capptain = {
	  [...]
	  reach: {
	    [...]
	  }
	}

Entfernen Sie andere Reach-Anpassungen, z.B. Kategorien.

#### Entfernen veralteter APIs

Einige der APIs aus Capptain sind im Mobile Engagement Web SDK veraltet.

Entfernen Sie alle Aufrufe der folgenden APIs: `agent.connect`, `agent.disconnect`, `agent.pause` und `agent.sendMessageToDevice`.

Entfernen Sie alle folgenden Rückrufe aus Ihrer Capptain-Konfiguration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived` und `onPushMessageReceived`.

#### Konfiguration

Mobile Engagement verwendet eine Verbindungszeichenfolge zum Konfigurieren der SDK-Bezeichner wie beispielsweise dem Anwendungsbezeichner.

Ersetzen Sie die Anwendungs-ID durch Ihre Verbindungszeichenfolge. Beachten Sie, dass sich das globale Objekt für die SDK-Konfiguration von `capptain` in `azureEngagement` ändert.

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

Das globale JavaScript-Objekt `window.capptain` wurde in `window.azureEngagement` umbenannt. Sie können aber den `window.engagement`-Alias für API-Aufrufe verwenden. Sie können diesen Alias nicht zum Definieren der SDK-Konfiguration verwenden.

Beispiel: `capptain.deviceId` wird `engagement.deviceId`, `capptain.agent.startActivity` wird `engagement.agent.startActivity` usw.

Wenn Sie bereits eine frühere Version des Azure Mobile Engagement Web SDK in Ihre Anwendung integriert haben, lesen Sie die Informationen zu [Upgradeverfahren](mobile-engagement-web-upgrade-procedure.md).

<!---HONumber=AcomDC_0713_2016-->