---
title: Medienstreaming-Optimierung mit Azure CDN
description: "Optimieren des Streamings von Mediendateien für eine nahtlose Übermittlung"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 582eb69ccd2830643644760753cca1c96d72128d
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---
# <a name="media-streaming-optimization-via-azure-cdn"></a>Medienstreaming-Optimierung mit Azure CDN 
 
High-Definition-Videos machen einen immer größeren Teil des Internets aus. Dies führt zu verschiedenen Probleme bei der effizienten Übertragung solch großer Dateien über das Internet. Kunden erwarten eine ruckelfreie Wiedergabe bei Video on Demand- oder Livevideo-Medienobjekten bei einer Vielzahl von Netzwerken und Clients auf der ganzen Welt. Die Bereitstellung eines schnelleren und effizienteren Übermittlungsverfahrens für Dateien beim Medienstreaming ist unerlässlich, um eine unterbrechungsfreie und angenehme Kundenerfahrung sicherzustellen.  

Livestreamingmedien sind besonders schwierig – nicht nur aufgrund der Dateigrößen und der Anzahl gleichzeitiger Benutzer, sondern auch, weil Verzögerungen bei der Übermittlung zu echten Einschränkungen für die Benutzer führen. Livestreams können nicht vorzeitig zwischengespeichert werden, und große Wartezeiten sind für die Zuschauer nicht akzeptabel, sodass Videofragmente schnellstmöglich übermittelt werden sollen. 

Die Anforderungsmuster beim Streaming stellen auch einige neue Herausforderungen dar. Bei einem beliebten Livestream oder bei Erscheinen eine neuen Serie über Video on Demand (VOD) kann es sein, dass Tausende oder sogar Millionen von Zuschauern versuchen, den Stream gleichzeitig anzufordern. In diesem Fall ist eine intelligente Anforderungskonsolidierung besonders wichtig, damit die Ursprungsserver nicht überlastet werden, wenn die Medienobjekte noch nicht zwischengespeichert wurden.
 
Azure CDN von Akamai bietet jetzt ein Feature zum effizienten Streamen von Medienobjekten an Endbenutzer weltweit – und das bei skalierbaren und verkürzten Wartezeiten und einer reduzierten Last auf dem Ursprungsserver. Dieses Feature steht über das Feature „Optimiert für“ auf Azure CDN-Endpunkten zur Verfügung, die unter einem Azure CDN-Profil mit dem Tarif „Standard Akamai“ erstellt wurden. Sie sollten die Optimierung „Video on Demand-Medienstreaming“ für VOD-Medienobjekte verwenden und die Optimierung „Allgemeines Medienstreaming“, wenn Sie eine Kombination von Live- und VOD-Streaming durchführen.

Azure CDN von Verizon kann Streamingmedien direkt mit dem Optimierungstyp „Allgemeine Webbereitstellung“ übermitteln.
 
## <a name="configuring-cdn-endpoint-to-optimize-delivery-of-media-streaming-in-azure-cdn-from-akamai"></a>Konfigurieren von CDN-Endpunkten für die optimierte Übermittlung von Streamingmedien in Azure CDN von Akamai
 
Sie können Ihren CDN-Endpunkt über das Azure-Portal für eine optimierte Übermittlung großer Dateien konfigurieren, indem Sie einfach während der Erstellung des Endpunkts eine der Optionen „Allgemeines Medienstreaming“ oder „Video on Demand-Medienstreaming“ unter der Eigenschaftenauswahl „Optimiert für“ auswählen. Sie können dafür auch unsere REST-APIs oder eines der Client-SDKs verwenden. Die folgenden Screenshots veranschaulichen die Vorgehensweise über das Azure-Portal. 
  
![Hinzufügen eines neuen Endpunkts](./media/cdn-media-streaming-optimization/01_Adding.png)

*Abbildung 1: Hinzufügen eines neuen CDN-Endpunkts aus dem CDN-Profil*

![Streaming ausgewählt](./media/cdn-media-streaming-optimization/02_Creating.png) 
  
*Abbildung 2: Erstellen eines CDN-Endpunkts mit ausgewählter Option „Video on Demand-Medienstreaming“* 
 
Nachdem der CDN-Endpunkt erstellt wurde, werden die Optimierungen auf alle Dateien angewendet, die bestimmte Kriterien erfüllen. Im folgenden Abschnitt wird dies im Detail beschrieben. 
 
## <a name="media-streaming-optimizations-for-azure-cdn-from-akamai"></a>Optimierungen für das Medienstreaming für Azure CDN von Akamai
 
Die Optimierung des Medienstreamings von Akamai ist besonders effektiv bei Live- oder VOD-Streamingmedien, die einzelne Medienfragmente für die Übermittlung verwenden, und nicht ein einzelnes großes Medienobjekt, das über einen progressiven Download oder mithilfe von Bytebereichsanforderungen übertragen wird. Weitere Informationen zu dieser Form der Medienübertragung finden Sie unter [Optimierung großer Dateien](cdn-large-file-optimization.md).

Bei den Optimierungstypen für allgemeine oder VOD-Medienbereitstellungen wird ein CDN-Netzwerk mit Back-End-Optimierungen zur schnelleren Übermittelung von Medienobjekten verwendet. Dazu kommen Konfigurationen für Medienobjekte, die auf bewährten Methoden basieren.

### <a name="caching"></a>Caching

Wenn Azure CDN von Akamai erkennt, dass das gestreamte Medienobjekt ein Streamingmanifest oder -fragment ist (siehe vollständige Liste unten), verwendet das CDN andere Ablaufzeiten für das Caching als bei der allgemeinen Übermittlung. Wie immer werden Cache-Control- oder Expires-Header vom Ursprungsserver berücksichtigt. Und wenn das Objekt kein Medienobjekt ist, wird es gemäß den Ablaufzeiten für die allgemeine Webbereitstellung zwischengespeichert.

Die kurze negative Zwischenspeicherungsdauer ist hilfreich für die Entlastung des Ursprungsservers, wenn viele Benutzer ein Fragment anfordern, das noch nicht vorhanden ist, z.B. einen Livestream, dessen Pakete zum Anforderungszeitpunkt auf dem Ursprungsserver noch nicht verfügbar sind. Das längere Cachingintervall führt auch zu Entlastungen bei den Anforderungen vom Ursprungsserver, da Videoinhalte in der Regel nicht geändert werden.
 

|    | Allgemein<br> web<br>Delivery | Allgemein<br> Medien<br> streaming | VOD-<br>Medien<br> streaming  
--- | --- | --- | ---
Caching – positiv <br> HTTP 200, 203, 300, <br> 301, 302 und 410 | 7 Tage |365 Tage | 365 Tage   
Caching – negativ <br> HTTP 204, 305, 404 <br> und 405 | (Keine) | 1 Sekunde | 1 Sekunde
 
### <a name="dealing-with-origin-failure"></a>Umgang mit Fehlern im Original  

Auch für allgemeine und VOD-Medienbereitstellungen gibt es Protokolle zu Timeouts beim Ursprungsserver und Wiederholungen basierend auf bewährten Methoden für typische Anforderungsmuster. Da z.B. allgemeine Medienbereitstellungen für Live- und VOD-Medienbereitstellungen verwendet werden, wird aufgrund des zeitkritischen Livestreamings ein viel kürzeres Verbindungstimeout angewendet.

Bei einem Verbindungstimeout versucht das CDN eine bestimmte Anzahl an Wiederholungen, bis schließlich ein 504-Gateway-Timeoutfehler an den Client gesendet wird. 

Wenn eine Datei der Bedingungsliste für Dateityp und -größe entspricht, verwendet das CDN das Verhalten für das Medienstreaming – andernfalls das für allgemeine Webbereitstellungen. 
   
### <a name="conditions-for-media-streaming-optimization"></a>Bedingungen für die Medienstreaming-Optimierung 

Die folgende Tabelle enthält die Kriterien für die Optimierung 
 
Unterstützte Streamingtypen | Dateierweiterungen  
--- | ---  
Apple HLS | M3U8, M3U, M3UB, KEY, TS, AAC
Adobe HDS | F4M, F4X, DRMMETA, BOOTSTRAP, F4F,<br>Seg-Frag-URL-Struktur <br> (regulärer Ausdruck: ^(/.*)Seq(\d+)-Frag(\d+)
DASH | MPD, DASH, DIVX, ISMV, M4S, M4V, MP4, MP4V, <br> SIDX, WEBM, MP4A, M4A, ISMA
Smooth Streaming | /manifest/,/QualityLevels/Fragments/
  

 
## <a name="media-streaming-optimizations-for-azure-cdn-from-verizon"></a>Optimierungen für das Medienstreaming für Azure CDN von Verizon

Azure CDN von Verizon kann Streamingmedienobjekte direkt mit dem Optimierungstyp „Allgemeine Webbereitstellung“ übermitteln. Darüber hinaus helfen einige Features im CDN standardmäßig bei der Übermittlung von Medienobjekten.

### <a name="partial-cache-sharing"></a>Partielle Cachefreigabe

Dieses Feature erlaubt das partielle Übermitteln von zwischengespeicherten Inhalten vom CDN bei neuen Anforderungen. Dies bedeutet z.B., dass die erste Anforderung an das CDN zu einem Cachefehlversuch führt und die Anforderung an den Ursprungsserver gesendet wird. Während dieser Inhalt noch in den CDN-Cache geladen wird (und noch nicht vollständig ist), können andere Anforderungen an das CDN damit beginnen, diese Daten abzurufen. 

### <a name="cache-fill-wait-time"></a>Wartezeit für die Cacheauffüllung

In Verbindung mit der partiellen Cachefreigabe erzwingt das Feature für die Wartezeit zur Cacheauffüllung vom Edgeserver, dass dieser sämtliche nachfolgenden Anforderungen für dieselbe Ressource zurückstellt, bis HTTP-Antwortheader vom Ursprungsserver eingehen. Wenn vor Ablauf des Timers HTTP-Antwortheader vom Ursprungsserver eintreffen, werden alle zurückgestellten Anforderungen aus dem zunehmenden Cache beantwortet (während dieser gleichzeitig durch die Daten vom Ursprungsserver aufgefüllt wird). Die Wartezeit für die Cacheausfüllung ist standardmäßig auf 3.000 Millisekunden festgelegt. 


