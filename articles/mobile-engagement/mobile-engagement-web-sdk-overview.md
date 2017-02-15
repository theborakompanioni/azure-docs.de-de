---
title: "Übersicht über das Azure Mobile Engagement Web SDK | Microsoft Docs"
description: "Die neuesten Updates und Verfahren für das Web SDK für Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 5bbc2fda-0f3f-43d0-a73d-0f2c0f8dc25b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 10/18/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 770a83131a3e661771db50b22ce7de25b2d541cf


---
# <a name="azure-mobile-engagement-web-sdk"></a>Azure Mobile Engagement Web SDK
Beginnen Sie hier, um alle Details zur Integration von Azure Mobile Engagement in eine Web-App zu erhalten. Sehen Sie sich unser [15-minütiges Tutorial](mobile-engagement-web-app-get-started.md)an, wenn Sie sich erst informieren möchten, bevor Sie mit einer eigenen Web-App beginnen.

## <a name="integration-procedures"></a>Integrationsverfahren
1. Erfahren Sie, wie Sie [Mobile Engagement in Ihre Web-App integrieren](mobile-engagement-web-integrate-engagement.md).
2. Lesen Sie die Informationen zur Tag-Plan-Implementierung unter [Verwenden der Mobile Engagement-API in einer Web-App](mobile-engagement-web-use-engagement-api.md).

## <a name="release-notes"></a>Anmerkungen zu dieser Version
### <a name="202-10182016"></a>2.0.2 (10/18/2016)
* Absturz beim privaten Surfen (Safari) behoben.
* Absturz von Browsern mit deaktivierten Cookies behoben.

Alle Versionen finden Sie unter [Vollständige Anmerkungen zu dieser Version](mobile-engagement-web-release-notes.md).

## <a name="upgrade-procedures"></a>Upgrade-Verfahren
### <a name="upgrade-from-121-to-200"></a>Upgrade von 1.2.1 auf 2.0.0
In den folgenden Abschnitten wird beschrieben, wie Sie die Migration des Mobile Engagement Web SDK vom Capptain-Dienst, die von Capptain SAS angeboten wird, in eine Azure Mobile Engagement-App durchführen. Wenn eine Migration von einer älteren Version als 1.2.1 erfolgt, sehen Sie auf der Capptain-Website nach, wie eine Migration zu Version 1.2.1 durchgeführt wird. Führen Sie anschließend die folgenden Schritte aus.

Diese Version des Mobile Engagement Web SDK unterstützt nicht Samsung Smart TV, Opera TV, webOS und das „Reach“-Feature.

> [!IMPORTANT]
> Capptain und Azure Mobile Engagement sind nicht dieselben Dienste, und die folgenden Vorgehensweisen heben nur hervor, wie die Client-App migriert wird. Bei der Migration des Mobile Engagement Web SDK in die App werden Ihre Daten nicht von einem Capptain-Server auf einen Mobile Engagement-Server migriert.
> 
> 

#### <a name="javascript-files"></a>JavaScript-Dateien
Ersetzen Sie die Datei „capptain-sdk.js“ durch die Datei „Azure-engagement.js“, und aktualisieren Sie Ihre Skriptimporte entsprechend.

#### <a name="remove-capptain-reach"></a>Entfernen von Capptain Reach
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

Entfernen Sie das `reach` -Objekt aus Ihrer Konfiguration, sofern vorhanden. Es sieht folgendermaßen aus:

    window.capptain = {
      [...]
      reach: {
        [...]
      }
    }

Entfernen Sie andere Reach-Anpassungen, z.B. Kategorien.

#### <a name="remove-deprecated-apis"></a>Entfernen veralteter APIs
Einige der APIs aus Capptain sind im Mobile Engagement Web SDK veraltet.

Entfernen Sie alle Aufrufe der folgenden APIs: `agent.connect`, `agent.disconnect`, `agent.pause` und `agent.sendMessageToDevice`.

Entfernen Sie alle folgenden Rückrufe aus Ihrer Capptain-Konfiguration: `onConnected`, `onDisconnected`, `onDeviceMessageReceived` und `onPushMessageReceived`.

#### <a name="configuration"></a>Konfiguration
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

#### <a name="javascript-apis"></a>JavaScript-APIs
Das globale JavaScript-Objekt `window.capptain` wurde in `window.azureEngagement` umbenannt. Sie können aber den `window.engagement`-Alias für API-Aufrufe verwenden. Sie können diesen Alias nicht zum Definieren der SDK-Konfiguration verwenden.

Beispiel: `capptain.deviceId` wird `engagement.deviceId`, `capptain.agent.startActivity` wird `engagement.agent.startActivity` usw.

Wenn Sie bereits eine frühere Version des Azure Mobile Engagement Web SDK in Ihre Anwendung integriert haben, lesen Sie die Informationen zu [Upgradeverfahren](mobile-engagement-web-upgrade-procedure.md).




<!--HONumber=Nov16_HO3-->


