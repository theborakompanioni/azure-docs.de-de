---
title: Integration des Azure Mobile Engagement Web SDK | Microsoft Docs
description: "Neueste Updates und Verfahren für das Azure Mobile Engagement Web SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: b5daa2a2-942b-489d-aa1d-568c3b25e56f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 02/29/2016
ms.author: piyushjo
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7d8eaa180e277741a583522ee62d68f5247b92bb


---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a>Integrieren von Azure Mobile Engagement in eine Webanwendung
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Die Verfahren in diesem Artikel beschreiben die einfachste Möglichkeit zum Aktivieren der Azure Mobile Engagement-Funktionen zur Analyse und Überwachung in Ihrer Webanwendung.

Führen Sie die Schritte aus, um die Protokollberichte zu aktivieren, die zum Berechnen sämtlicher Statistiken zu Benutzern, Sitzungen, Aktivitäten, Abstürzen und technischen Informationen benötigt werden. Für anwendungsabhängige Statistiken wie Ereignisse, Fehler und Aufträge müssen Sie Protokollberichte manuell aktivieren, indem Sie die Azure Mobile Engagement-API verwenden. Weitere Informationen finden Sie unter [Verwenden der erweiterten Mobile Engagement-Tagging-API in einer Webanwendung](mobile-engagement-web-use-engagement-api.md).

## <a name="introduction"></a>Einführung
[Laden Sie das Azure Mobile Engagement Web SDK herunter](http://aka.ms/P7b453).
Das Mobile Engagement Web SDK wird als einzelne JavaScript-Datei namens „azure-engagement.js“ bereitgestellt, die Sie allen Seiten Ihrer Website oder Webanwendung hinzufügen müssen.

> [!IMPORTANT]
> Bevor Sie dieses Skript ausführen, müssen Sie ein Skript oder einen Codeausschnitt ausführen, das bzw. den Sie zum Konfigurieren von Mobile Engagement für Ihre Anwendung schreiben.
> 
> 

## <a name="browser-compatibility"></a>Browserkompatibilität
Das Mobile Engagement Web SDK verwendet eine native JSON-Codierung und -Decodierung zusätzlich zu domänenübergreifende AJAX-Anforderungen (auf Basis der W3C CORS-Spezifikation). Es ist mit den folgenden Browsern kompatibel:

* Microsoft Edge 12 und höher
* Internet Explorer 10 und höher
* Firefox 3.5 und höher
* Chrome 4 und höher
* Safari 6 und höher
* Opera 12 und höher

## <a name="configure-mobile-engagement"></a>Konfigurieren von Mobile Engagement
Schreiben Sie ein Skript zur Erstellung eines globalen JavaScript-Objekts des Typs `azureEngagement`, wie im folgenden Beispiel gezeigt. Da Ihre Website ggf. mehrere Seiten hat, setzt dieses Beispiel voraus, dass dieses Skript allen Seiten hinzugefügt wird. In diesem Beispiel heißt das JavaScript-Objekt `azure-engagement-conf.js`.

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

Der `connectionString` -Wert für Ihre Anwendung wird im Azure-Portal angezeigt.

> [!NOTE]
> `appVersionName` und `appVersionCode` sind optional. Es wird jedoch empfohlen, sie zu konfigurieren, damit für die Analyse Versionsinformationen verarbeitet werden können.
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a>Hinzufügen von Mobile Engagement-Skripts zu Ihren Seiten
Fügen Sie Ihren Seiten Mobile Engagement-Skripts auf eine der folgenden Weisen hinzu:

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

Oder so:

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a>Alias
Nach dem Laden des Mobile Engagement Web SDK erstellt es den Alias **engagement** für den Zugriff auf die APIs des SDK. Sie können mit diesem Alias nicht die SDK-Konfiguration definieren. Dieser Alias wird in dieser Dokumentation als Verweis verwendet.

Beachten Sie, dass wenn der Standardalias im Konflikt mit einer anderen globalen Variablen auf Ihrer Seite steht, Sie ihn in der Konfiguration wie folgt neu definieren können, bevor das Mobile Engagement Web SDK geladen wird:

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a>Grundlegende Berichterstellung
Die grundlegende Berichterstellung in Mobile Engagement deckt Statistiken auf Sitzungsebene ab, wie z.B. Statistiken zu Benutzern, Sitzungen, Aktivitäten und Abstürzen.

### <a name="session-tracking"></a>Sitzungsnachverfolgung
Eine Mobile Engagement-Sitzung ist in eine Abfolge von Aktivitäten aufgeteilt, die von einem Namen bestimmt werden.

Für eine klassische Website wird empfohlen, eine andere Aktivität auf jeder Seite Ihrer Website zu deklarieren. Für eine Website oder Anwendung, bei der sich die aktuelle Seite nie ändert, sollten Sie die Aktivitäten in kleinerem Umfang nachverfolgen, z.B. innerhalb der Seite.

In beiden Fällen starten oder ändern Sie die aktuelle Benutzeraktivität durch Aufrufen der `engagement.agent.startActivity` -Funktion. Beispiel:

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

Eine geöffnete Sitzung wird vom Mobile Engagement-Server automatisch binnen drei Minuten nach Schließen der Anwendungsseite beendet.

Sie können eine Sitzung auch manuell durch Aufrufen von `engagement.agent.endActivity`beenden. Dadurch wird die aktuelle Benutzeraktivität auf „Idle“ festgelegt.  Die Sitzung endet 10 Sekunden später, es sei denn, sie wird durch einen neuen Aufruf von `engagement.agent.startActivity` fortgesetzt.

Sie können die Verzögerung von 10 Sekunden im globalen Engagement-Objekt wie folgt konfigurieren:

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end the session as soon as endActivity is called

> [!NOTE]
> Sie können `engagement.agent.endActivity` nicht im `onunload`-Rückruf verwenden, da derzeit keine AJAX-Aufrufe möglich sind.
> 
> 

## <a name="advanced-reporting"></a>Erweiterte Berichterstellung
Wenn Sie anwendungsspezifische Ereignisse, Fehler und Aufträge optional melden möchten, müssen Sie die Mobile Engagement-API verwenden. Auf die Mobile Engagement-API greifen Sie über das `engagement.agent` -Objekt zu.

Sie können auf alle erweiterten Funktionen in Mobile Engagement in der Mobile Engagement-API zugreifen. Weitere Informationen zur API finden Sie im Artikel [Verwenden der erweiterten Mobile Engagement-Tagging-API in einer Webanwendung](mobile-engagement-web-use-engagement-api.md).

## <a name="customize-the-urls-used-for-ajax-calls"></a>Anpassen der für AJAX-Aufrufe verwendeten URLs
Sie können URLs anpassen, die das Mobile Engagement SDK verwendet. Um beispielsweise die Protokoll-URL (den SDK-Endpunkt für die Protokollierung) neu zu definieren, können Sie die Konfiguration wie folgt überschreiben:

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

Wenn Ihre URL-Funktionen eine mit `/`, `//`, `http://` oder `https://` beginnende Zeichenfolge zurückgeben, wird das Standardschema nicht verwendet. Standardmäßig wird das `https://` -Schema für diese URLs verwendet. Wenn Sie das Standardschema anpassen möchten, überschreiben Sie die Konfiguration wie folgt:

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };



<!--HONumber=Nov16_HO3-->


