---
title: Windows Universal Apps SDK-Versionshinweise
description: Azure Mobile Engagement – Windows Universal Apps SDK-Versionshinweise
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: dwrede
editor: ''

ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo

---
# Windows Universal Apps SDK-Versionshinweise
## 3\.4.0 (19.04.2016)
* Reach-Overlayverbesserungen.
* „TestLogLevel“-API zum Aktivieren/Deaktivieren/Filtern von Konsolenprotokollen, die vom SDK ausgegeben wurden, wurde hinzugefügt.
* Behebung des Problems mit Inaktivitätsbenachrichtigungen, die sich darauf bezogen, dass die erste Aktivität beim App-Start nicht angezeigt wurde.

## 3\.3.1 (18.02.2016)
* Behebung von Konflikten zwischen dem Inhalt der HTML-Webankündigung und dem SDK der HTML-Seite.
* Verbesserungen der Stabilität.

## 3\.3.0 (22.01.2016)
* Behebung von Absturzformatierung auf UWP-Apps, die im Releasemodus ausgeführt werden.
* Behebung von 1px-Rand auf Benachrichtigungen für Universal 8.1-Apps.
* ms-appx- und ms-appdata-Schemas in Aktions-URLs verfügbar.
* Verbesserungen der Stabilität.

## 3\.2.0 (20.11.2015)
* Unterstützung für universelle Windows 10-Plattform-Anwendungen hinzugefügt.
* Freigabefunktion für Pushkanal hinzugefügt, um Kanalkonflikte zu beheben (jetzt kompatibel mit Azure Notification Hubs).
* Absturz beim Anfordern der Geräte-ID direkt nach der Initialisierung behoben.
* Verbesserungen bei Konsolenprotokollen.
* Absturz beim Analysieren einiger nicht behandelter Ausnahmen behoben.

## 3\.1.0 (21.05.2015)
* Die Mobile Engagement-Geräte-ID basiert jetzt auf einer während der Installation generierten GUID.

## 3\.0.1 (29.04.2015)
* Es wurde ein Fehler behoben, der sich auf die SDK-Initialisierung in einigen Apps für Windows Phone WinRT auswirkt.

## 3\.0.0 (03.04.2015)
* Einführung des Mobile Engagement SDK für universelle Apps (Windows und Windows Phone WinRT).
* Standardbenachrichtigungssymbol aktualisiert.
* Beim Klicken auf eine Benachrichtigung wird Feedback zur Systembenachrichtigungsaktion gesendet.
* Systembenachrichtigung korrigiert, die manchmal in einer App wiederholt wird, nachdem darauf geklickt wurde.

## 2\.0.0 (17.02.2015)
* Erste Version von Azure Mobile Engagement
* appId/sdkKey-Konfiguration wird durch die Konfiguration einer Verbindungszeichenfolge ersetzt.
* Verbesserungen der Sicherheit

<!---HONumber=AcomDC_0824_2016-->