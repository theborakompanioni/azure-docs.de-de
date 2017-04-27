---
title: Integration des Azure Mobile Engagement Android SDKs
description: "Neueste Updates und Verfahren für das Android SDK für Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 585341f9-3f39-459a-af42-864e400a0128
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 03/30/2017
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: e50c1c70a4378560f8ce3182069cba4832fa4d22
ms.lasthandoff: 03/31/2017


---
# <a name="release-notes"></a>Versionshinweise

## <a name="424-03302017"></a>4.2.4 (30.03.2017)
* Die Textfarben von In-App-Benachrichtigungen unter Android 7 wurden so korrigiert, dass sie denen in älteren Android-Versionen entsprechen.

## <a name="423-08102016"></a>4.2.3 (08/10/2016)
* Keine WLAN-Sperre mehr.
* Behebung eines Deadlocks, der auftritt, wenn „getDeviceId“ vor „init“ aufgerufen wird (Fehler seit 4.2.0).

## <a name="422-05172016"></a>4.2.2 (17.05.2016)
* Verbesserungen der Stabilität.

## <a name="421-05102016"></a>4.2.1 (10.05.2016)
* Sicherheit: Der lokale Dateizugriff für die Webansicht wurde deaktiviert.
* Sicherheit: Die Klasse `EngagementPreferenceActivity`, die die veraltete und nicht sichere Klasse `PreferenceActivity` erweitert, wurde entfernt.
* Sicherheit: Für Reach-Aktivitäten kann nun `exported="false"` verwendet werden. Dieses Flag kann auch in vorherigen SDK-Versionen verwendet werden.

## <a name="420-03112016"></a>4.2.0 (03/11/2016)
* Das SDK ist jetzt unter MIT lizenziert.
* Erlauben Sie die Angabe eines benutzerdefinierten Gerätebezeichners während der SDK-Initialisierung.

## <a name="415-02012016"></a>4.1.5 (01.02.2016)
* Verbesserungen der Stabilität.

## <a name="414-01262016"></a>4.1.4 (26.01.2016)
* Verbesserungen der Stabilität.

## <a name="413-1292015"></a>4.1.3 (9.12.2015)
* Verbesserungen der Stabilität.

## <a name="412-11252015"></a>4.1.2 (25.11.2015)
* Verbesserungen der Stabilität.

## <a name="411-11042015"></a>4.1.1 (04.11.2015)
* Verbesserungen der Stabilität.

## <a name="410-08252015"></a>4.1.0 (25.08.2015)
* Behandlung eines neuen Berechtigungsmodells für Android M.
* Konfiguration von Standortfunktionen zur Laufzeit ist nun möglich, anstatt `AndroidManifest.xml` zu verwenden.
* Korrektur eines Berechtigungsfehlers: bei Verwendung von `ACCESS_FINE_LOCATION` wird `ACCESS_COARSE_LOCATION` nicht mehr benötigt.
* Verbesserungen der Stabilität.

## <a name="400-07062015"></a>4.0.0 (06.07.2015)
* Interne Protokolländerungen zur Steigerung der Zuverlässigkeit von Analysen und Push
* Ein systemeigener Push (GCM/ADM) wird jetzt auch für In-App-Benachrichtigungen verwendet, Sie müssen daher die Anmeldeinformationen für den systemeigenen Push für jeden Push-Kampagnentyp konfigurieren.
* Korrektur für Überblick-Benachrichtigung: Es wurde nach dem Push nur 10 angezeigt.
* Beheben eines Fehlers in der Webansicht: durch Klicken auf einen Link wurde auch die Standardaktions-URL ausgeführt.
* Korrektur für seltenen Absturz im Zusammenhang mit der lokalen Speicherverwaltung.
* Korrektur der Zeichenfolgenverwaltung für die dynamische Konfiguration.
* Aktualisierung des Endbenutzer-Lizenzvertrags.

## <a name="300-02172015"></a>3.0.0 (17.02.2015)
* Erste Version von Azure Mobile Engagement
* Die „appId“-Konfiguration wird durch eine Verbindungszeichenfolgen-Konfiguration ersetzt.
* API entfernt, um beliebige XMPP-Nachrichten über beliebige XMPP-Entitäten zu senden und zu empfangen.
* API entfernt, um Nachrichten zwischen Geräten zu senden und zu empfangen.
* Verbesserungen der Sicherheit
* Google Play und die SmartAd-Nachverfolgung wurden entfernt.


