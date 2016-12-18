---
title: "Windows Universal Apps SDK – Inhalt"
description: "Informieren Sie sich über den Inhalt des Windows Universal Apps SDK für Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: dwrede
editor: 
ms.assetid: 8fa1b701-1c2b-4aec-940c-06c974ef5405
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ea47cba55c9689e23eb3592ae2559649d3377055


---
# <a name="windows-universal-apps-sdk-content"></a>Windows Universal Apps SDK – Inhalt
In diesem Dokument wird aufgelistet und beschrieben, was durch das SDK in der Anwendung bereitgestellt wird.

## <a name="the-resources-folder"></a>Der Ordner `/Resources`
Dieser Ordner enthält alle von Mobile Engagement benötigten Ressourcen. Sie können diese auch an Ihre App anpassen.

* `EngagementConfiguration.xml` : Die Konfigurationsdatei von Mobile Engagement, hier können Sie Mobile Engagement-Einstellungen (Mobile Engagement-Verbindungszeichenfolge, Absturzbericht...) anpassen.

### <a name="html-folder"></a>/html-Ordner
* `EngagementNotification.html` : Der `Notification`-Webansichts-HTML-Entwurf für In-App-Banner.
* `EngagementAnnouncement.html` : Der `Announcement`-Webansichts-HTML-Entwurf für In-App-Interstitialansichten.

### <a name="images-folder"></a>/images-Ordner
* `EngagementIconNotification.png` : Das Markensymbol, das auf der linken Seite einer Benachrichtigung angezeigt wird. Ersetzen Sie dieses Symbol durch Ihr eigenes Markensymbol.
* `EngagementIconOk.png` : Das `Ok`-Symbol der Reach-Inhaltsseiten für die Aktions- oder Validierungsschaltfläche.
* `EngagementIconNOK.png` : Das `NOK`-Symbol, das verwendet wird, wenn die Validierungsschaltfläche der Reach-Inhaltsseiten deaktiviert ist.
* `EngagementIconClose.png` : Das `Close`-Symbol der Reach-Benachrichtigungen und -Inhalte für die Schaltfläche zum Verwerfen.

### <a name="overlay-folder"></a>/overlay-Ordner
* `EngagementPageOverlay.cs` : Die Overlayseite, die für das Hinzufügen der Engagement-Reach-In-App-Benutzeroberfläche zu ihrem untergeordneten Element verantwortlich ist.




<!--HONumber=Nov16_HO3-->


