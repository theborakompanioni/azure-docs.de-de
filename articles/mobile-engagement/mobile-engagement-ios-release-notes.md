---
title: "Versionshinweise zum iOS-SDK für Azure Mobile Engagement | Microsoft Docs"
description: "Neueste Updates und Verfahren für das iOS-SDK für Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a43ff0f6-90d5-4b3c-8d7a-a1db21bc776b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-ios
ms.devlang: objective-c
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 9bdaa57f9902373ccf796ff109332b64c66bf9e7
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="azure-mobile-engagement-ios-sdk-release-notes"></a>Versionshinweise zum iOS-SDK für Azure Mobile Engagement

## <a name="410-07172017"></a>4.1.0 (07/17/2017)
* Behobene Badges werden im Hintergrund gelöscht.
* Warnungen in XCode 9 über APIs, die nicht in der Hauptwarteschlange aufgerufen werden, wurden behoben.
* Speicherverlust in Reach-Umfragen wurde behoben.
* Unterstützung für iOS 6.X eingestellt. Ab dieser Version muss das Bereitstellungsziel Ihrer Anwendung mindestens über iOS 7 verfügen.

## <a name="401-12132016"></a>4.0.1 (13.12.2016)
* Verbesserte Protokollübermittlung im Hintergrund.

## <a name="400-09122016"></a>4.0.0 (12.09.2016)
* Feste Benachrichtigung auf iOS 10-Geräten nicht umgesetzt.
* XCode 7 als veraltet markiert.

## <a name="324-06302016"></a>3.2.4 (30.06.2016)
* Probleme mit der Aggregation zwischen technischen Protokollen und anderen Protokollen wurden behoben.

## <a name="323-06072016"></a>3.2.3 (07.06.2016)
* Der Fehler, dass kein Übermittlungsfeedback gemeldet wird, wenn die App sich im Hintergrund befindet, wurde behoben.
* Das Senden technischer Protokolle wurde optimiert.

## <a name="322-04072016"></a>3.2.2 (07.04.2016)
* Das Problem beim Abbruch der HTTP-Anforderung, das manchmal zum Absturz führte, wurde behoben.

## <a name="321-12112015"></a>3.2.1 (11.12.2015)
* Es wurde eine Verzögerung behoben, wenn eine neue Instanz der App durch eine Benachrichtigung mit Deep-Links ausgelöst wird.

## <a name="320-10082015"></a>3.2.0 (08.10.2015)
* Bitcode im SDK aktiviert, um die Verwendung mit **Xcode 7**zu ermöglichen.
* Fehler im Zusammenhang mit In-App-Benachrichtigungen behoben.
* Die Zuverlässigkeit von In-App-Benachrichtigungen bei niedriger Akkukapazität und in ähnlichen Szenarien verbessert.
* Zusätzliche Konsolenprotokolle entfernt, die von Drittanbieterbibliothek generiert werden.

## <a name="310-08262015"></a>3.1.0 (26.08.2015)
* Behebung von iOS 9-Kompatibilitätsproblemen mit einer Drittanbieterbibliothek. Beim Senden von Umfrageergebnissen, Anwendungsinformationen oder extrahierten Daten wurden Abstürze verursacht.

## <a name="300-06192015"></a>3.0.0 (19.06.2015)
* Mobile Engagement verwendet stille Pushbenachrichtigungen.
* Unterstützung für iOS 4.X eingestellt. Ab dieser Version muss das Bereitstellungsziel Ihrer Anwendung mindestens über iOS 6 verfügen.

## <a name="220-05212015"></a>2.2.0 (21.05.2015)
* Die Mobile Engagement-Geräte-ID für Geräte mit einer früheren Version als iOS 6 basiert jetzt auf einer während der Installation generierten GUID.

## <a name="210-04242015"></a>2.1.0 (24.04.2015)
* Swift-Kompatibilität hinzugefügt.
* Wenn Sie auf eine Benachrichtigung klicken, wird die Aktions-URL nun direkt nach dem Öffnen der Anwendung ausgeführt.
* Fehlende Headerdatei im SDK-Paket hinzugefügt.
* Behebung eine Fehlers, wenn der Mobile Engagement-Absturzmelder deaktiviert wurde.

## <a name="200-02172015"></a>2.0.0 (17.02.2015)
* Erste Version von Azure Mobile Engagement
* appId/sdkKey-Konfiguration wird durch die Konfiguration einer Verbindungszeichenfolge ersetzt.
* API entfernt, um beliebige XMPP-Nachrichten über beliebige XMPP-Entitäten zu senden und zu empfangen.
* API entfernt, um Nachrichten zwischen Geräten zu senden und zu empfangen.
* Verbesserungen der Sicherheit
* SmartAd-Verfolgung entfernt.

