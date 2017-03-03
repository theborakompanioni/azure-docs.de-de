---
title: "Azure Mobile Engagement – Versionshinweise für das Windows Universal Apps SDK | Microsoft-Dokumentation"
description: "Azure Mobile Engagement – Windows Universal Apps SDK-Versionshinweise"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: de938ce5-93d5-4218-9e33-10eef102bd61
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 51858d6daca27a75e7f87af507600c7a193c874f
ms.openlocfilehash: dc5529a9e8f4eba867732f719ca8fff718c00d5a
ms.lasthandoff: 02/09/2017


---
# <a name="windows-universal-apps-sdk-release-notes"></a>Windows Universal Apps SDK-Versionshinweise
## <a name="341-11032016"></a>3.4.1 (11/03/2016)

* Verbesserungen der Stabilität.

## <a name="340-04192016"></a>3.4.0 (19.04.2016)
* Reach-Overlayverbesserungen.
* „TestLogLevel“-API zum Aktivieren/Deaktivieren/Filtern von Konsolenprotokollen, die vom SDK ausgegeben wurden, wurde hinzugefügt.
* Behebung des Problems mit Inaktivitätsbenachrichtigungen, die sich darauf bezogen, dass die erste Aktivität beim App-Start nicht angezeigt wurde.

## <a name="331-02182016"></a>3.3.1 (18.02.2016)
* Behebung von Konflikten zwischen dem Inhalt der HTML-Webankündigung und dem SDK der HTML-Seite.
* Verbesserungen der Stabilität.

## <a name="330-01222016"></a>3.3.0 (22.01.2016)
* Behebung von Absturzformatierung auf UWP-Apps, die im Releasemodus ausgeführt werden.
* Behebung von 1px-Rand auf Benachrichtigungen für Universal 8.1-Apps.
* ms-appx- und ms-appdata-Schemas in Aktions-URLs verfügbar.
* Verbesserungen der Stabilität.

## <a name="320-11202015"></a>3.2.0 (20.11.2015)
* Unterstützung für universelle Windows 10-Plattform-Anwendungen hinzugefügt.
* Freigabefunktion für Pushkanal hinzugefügt, um Kanalkonflikte zu beheben (jetzt kompatibel mit Azure Notification Hubs).
* Absturz beim Anfordern der Geräte-ID direkt nach der Initialisierung behoben.
* Verbesserungen bei Konsolenprotokollen.
* Absturz beim Analysieren einiger nicht behandelter Ausnahmen behoben.

## <a name="310-05212015"></a>3.1.0 (21.05.2015)
* Die Mobile Engagement-Geräte-ID basiert jetzt auf einer während der Installation generierten GUID.

## <a name="301-04292015"></a>3.0.1 (29.04.2015)
* Es wurde ein Fehler behoben, der sich auf die SDK-Initialisierung in einigen Apps für Windows Phone WinRT auswirkt.

## <a name="300-04032015"></a>3.0.0 (03.04.2015)
* Einführung des Mobile Engagement SDK für universelle Apps (Windows und Windows Phone WinRT).
* Standardbenachrichtigungssymbol aktualisiert.
* Beim Klicken auf eine Benachrichtigung wird Feedback zur Systembenachrichtigungsaktion gesendet.
* Systembenachrichtigung korrigiert, die manchmal in einer App wiederholt wird, nachdem darauf geklickt wurde.

## <a name="200-02172015"></a>2.0.0 (17.02.2015)
* Erste Version von Azure Mobile Engagement
* appId/sdkKey-Konfiguration wird durch die Konfiguration einer Verbindungszeichenfolge ersetzt.
* Verbesserungen der Sicherheit


