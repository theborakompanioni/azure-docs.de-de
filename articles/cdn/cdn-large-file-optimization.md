---
title: "Optimieren großer Dateidownloads mit Azure CDN"
description: "Fundierte Einblicke in die Optimierung großer Dateidownloads"
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
ms.openlocfilehash: 27e202b05f86eeee7071f3fae145caeba3d66827
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017

---
# <a name="large-file-download-optimization-via-azure-cdn"></a>Optimieren großer Dateidownloads mit Azure CDN

Die Dateigrößen von Inhalten, die über das Internet übermittelt werden, sind aufgrund immer umfangreicherer Funktionalität, verbesserter Grafik und umfassenderer Medieninhalte stetig gewachsen. Dies wird durch viele Faktoren beeinflusst, einschließlich Breitbandversorgung, größerer und kostengünstiger Speichergeräte, Verbreitung von High-Definition-Videos, mit dem Internet verbundener Geräte (IoT) usw.  Die Bereitstellung eines schnelleren und effizienteren Übermittlungsverfahrens für solche großen Dateien ist unerlässlich, um eine unterbrechungsfreie und angenehme Kundenerfahrung sicherzustellen. 

Die Übermittlung großer Dateien stellt mehrere Herausforderungen dar. Zum einen kann die durchschnittliche Zeit zum Herunterladen einer großen Datei von Bedeutung sein, da viele Anwendungen nicht alle Daten sequenziell herunterladen. In einigen Fällen laden Anwendungen den letzten Teil der Datei vor dem ersten herunter. Wenn nur ein kleiner Teil einer Datei angefordert wird oder ein Benutzer einen Download unterbricht, kann es daher passieren, dass der Download zu einem Fehler führt oder verzögert wird, bis die gesamte Datei vom CDN von der Quelle abgerufen wurde. 

Zum anderen beobachten viele Benutzer durch die zunehmende Verbreitung großer Dateien im Internet häufig Wartezeiten bei der Anzeige aufgrund des Durchsatzes oder der Geschwindigkeit zwischen Benutzer und Datei. Darüber hinaus haben auch Netzwerküberlastung und Kapazitätsprobleme Auswirkungen auf den Durchsatz. Diese Probleme, gekoppelt mit den größeren Abständen zwischen Server und Endbenutzer, stellen zusätzliche Auslöser für Paketverluste dar, durch die die Qualität weiter reduziert wird. Der Qualitätsverlust aufgrund von begrenztem Durchsatz und erhöhten Paketverlusten kann selbst zu einer erheblich höheren Wartezeit bis zum Abschluss eines Dateidownloads führen. 

Und schließlich werden viele große Dateien nicht vollständig übermittelt. Benutzer können einen Download bei der Hälfte abbrechen oder sich nur die ersten Minuten eines langen MP4-Videos ansehen. Aus diesem Grund ist es für viele Unternehmen, die Software und Medien übermitteln, hilfreich, nur einen Teil einer Datei, die vom Benutzer angefordert wird, zuzustellen. Auf diese Weise werden nur die angeforderten Teile effizient an die am weitesten entfernten Bereiche des Internets übermittelt, womit der an der Quelle ausgehende Datenverkehr und damit auch die Speicher- und E/A-Auslastung auf dem Ursprungsserver reduziert werden. 

Azure CDN von Akamai bietet jetzt ein Feature zur effizienten Übermittlung von großen Dateien an Endbenutzer weltweit – und das bei skalierbaren und verkürzten Wartezeiten und einer reduzierten Last auf dem Ursprungsserver. Dieses Feature steht über das Feature „Optimiert für“ auf Azure CDN-Endpunkten zur Verfügung, die unter einem Azure CDN-Profil mit dem Tarif „Standard Akamai“ erstellt wurden.

## <a name="configuring-cdn-endpoint-to-optimize-delivery-of-large-files"></a>Konfigurieren von CDN-Endpunkten zur Optimierung der Übermittlung großer Dateien

Sie können Ihren CDN-Endpunkt über das Azure-Portal für eine optimierte Übermittlung großer Dateien konfigurieren, indem Sie einfach während der Erstellung des Endpunkts die Option „Download großer Dateien“ unter der Eigenschaftenauswahl „Optimiert für“ auswählen. Sie können dafür auch unsere REST-APIs oder eines der Client-SDKs verwenden. Die folgenden Screenshots veranschaulichen die Vorgehensweise über das Azure-Portal.

![Neuer CDN-Endpunkt](./media/cdn-large-file-optimization/01_Adding.png)  
 
*Abbildung 1: Hinzufügen eines neuen CDN-Endpunkts aus dem CDN-Profil*
 
![LFO ausgewählt](./media/cdn-large-file-optimization/02_Creating.png)

*Abbildung 2: Erstellen eines CDN-Endpunkts mit Optimierung für den Download großer Datei*

Nachdem der CDN-Endpunkt erstellt wurde, werden die Optimierungen für große Dateien auf alle Dateien angewendet, die bestimmte Kriterien erfüllen. Im folgenden Abschnitt wird dies im Detail beschrieben.

## <a name="optimizing-for-delivery-of-large-files-with-azure-cdn-from-akamai"></a>Optimieren für die Übermittlung großer Dateien mit Azure CDN von Akamai

Für Azure CDN von Akamai können Sie das Feature zur Optimierung für große Dateien verwenden, um Netzwerkoptimierungen und -konfigurationen zu aktivieren, die das Übermitteln großer Dateien schneller und flexibler machen. Bei der allgemeinen Webbereitstellung mit Akamai können nur Dateien unter 1,8 GB zwischengespeichert werden. Ein Tunneling (nicht Caching) ist für Dateien bis zu 150 GB möglich. Mit der Optimierung für große Dateien können auch Dateien mit bis zu 150 GB zwischengespeichert werden.

Die Optimierung für große Dateien ist besonders effektiv, wenn bestimmte Bedingungen im Hinblick auf den Betrieb des Ursprungsservers, die angeforderten Dateitypen und die Größe der angeforderten Dateien erfüllt sind. Bevor wir im Einzelnen ins Detail gehen, ist eine allgemeine Übersicht über die Funktionsweise der Optimierung notwendig. 

### <a name="object-chunking"></a>Objektblockerstellung 

Azure CDN von Akamai verwendet eine Technik namens Objektblockerstellung, bei der das CDN kleinere Teile der Datei vom Ursprung abruft, wenn eine große Datei angefordert wird. Wenn der CDN-Edge-/POP-Server vom Endbenutzer eine Anforderung für eine vollständige Datei oder einen Bytebereich daraus empfängt, prüft er zunächst, ob der Dateityp der Liste der Dateitypen angehört, die für diese Optimierung unterstützt werden, und ob die Datei den Größenanforderungen entspricht. Wenn die Datei größer als 10 MB ist, beginnt der CDN-Edgeserver mit dem Anfordern der Datei vom Ursprungsserver in Blöcken von 2 MB. Wenn der CDN-Edgeserver einen Block empfängt, wird dieser zwischengespeichert und sofort für den Endbenutzer bereitgestellt, während das CDN den nächsten Block parallel abruft. Dieses Vorababrufen stellt sicher, dass der Inhalt früher verfügbar ist, da der Server dem Endbenutzer so immer einen Block voraus ist und damit die Wartezeit reduzieren kann. Dieser Prozess wird fortgesetzt, bis die gesamte Datei heruntergeladen wurde (wenn der Endbenutzer die gesamte Datei angefordert hat), bis alle angeforderten Bytebereiche verfügbar sind (wenn der Endbenutzer Bytebereiche angefordert hat) oder bis der Client die Verbindung beendet. 

Details zur Anforderung von Bytebereichen finden Sie in der [RFC 7233](https://tools.ietf.org/html/rfc7233).

Das CDN speichert alle Blöcke so zwischen, wie sie empfangen werden. Daher muss nicht die gesamte Datei im CDN-Cache zwischengespeichert werden. Nachfolgende Anforderungen für die Datei oder für Bytebereiche werden aus dem CDN-Cache übermittelt. Falls nicht alle Blöcke auf dem CDN zwischengespeichert wurden, erfolgt erneut ein Vorababruf einzelner Blöcke. Wie Sie sehen, hängt diese Optimierung davon ab, dass der Ursprungsserver Anforderungen für Bytebereiche unterstützt. _Wenn der Ursprungsserver keine Anforderungen für Bytebereiche unterstützt, ist diese Optimierung nicht sehr effektiv._ 

### <a name="caching"></a>Caching
Bei großen Dateien werden andere Standardablaufzeiten für die Zwischenspeicherung verwendet als für die allgemeine Übermittlung. Die Unterscheidung erfolgt zwischen der positiven und negativen Zwischenspeicherung basierend auf HTTP-Antwortcodes. Wenn der Ursprungsserver eine Ablaufzeit per Cache-Control- oder Expires-Header in der Antwort angibt, berücksichtigt das CDN diesen Wert immer. Macht der Ursprungsserver keine Angabe und entspricht die Datei nach Dateityp und -größe den Bedingungen der Liste großer Dateien für diesen Optimierungstyp, dann verwendet das CDN die Standardwerte für die Optimierung großer Dateien. Andernfalls verwendet das CDN die Standardeinstellungen für die allgemeine Webbereitstellung.

 
|    | Allgemeine Webübermittlung | Optimierung großer Dateien 
--- | --- | --- 
Caching – positiv <br> HTTP 200, 203, 300, <br> 301, 302 und 410 | 7 Tage |1 Tag  
Caching – negativ <br> HTTP 204, 305, 404 <br> und 405 | (Keine) | 1 Sekunde 

### <a name="dealing-with-origin-failure"></a>Umgang mit Fehlern im Original

Bei der Optimierung für große Dateien wird das Lesezeitlimit an der Quelle von 2 Sekunden bei allgemeinen Webbereitstellung auf 2 Minuten erhöht, damit größere Dateien nicht zu vorzeitigen Verbindungstimeouts führen.

Wie bei allgemeinen Webbereitstellungen wird bei einem Verbindungstimeout eine bestimmte Anzahl an Wiederholungen versucht, bis schließlich ein 504-Gateway-Timeoutfehler an den Client gesendet wird. 

### <a name="conditions-for-large-file-optimization"></a>Bedingungen für die Optimierung großer Dateien

Die folgende Tabelle enthält die Kriterien für die Optimierung großer Dateien:

Bedingung | Werte 
--- | --- 
Unterstützte Dateitypen | 3G2, 3GP, ASF, AVI, BZ2, DMG, EXE, F4V, FLV, <br> GZ, HDP, ISO, JXR, M4V, MKV, MOV, MP4, <br> MPEG, MPG, MTS, PKG, QT, RM, SWF, TAR, <br> TGZ, WDP, WEBM, WEBP, WMA, WMV, ZIP  
Minimale Dateigröße | 10 MB 
Maximale Dateigröße | 150 GB 
Merkmale des Ursprungsservers | Muss Bytebereichsanforderungen unterstützen 

## <a name="optimizing-for-delivery-of-large-files-with-azure-cdn-from-verizon"></a>Optimieren für die Übermittlung großer Dateien mit Azure CDN von Verizon

Azure CDN von Verizon kann große Dateien ohne eine Obergrenze bei der Dateigröße übermitteln und verfügt über verschiedene Features, die eine schnellere Aktivierung der Übermittlung großer Dateien möglich machen.

### <a name="complete-cache-fill"></a>Complete Cache Fill

Azure CDN von Verizon weist über ein Standardfeature zur vollständigen Cacheauffüllung auf, bei dem das CDN per Pull eine Datei in den Cache abruft, wenn die ursprüngliche Anforderung abgebrochen wird oder verloren geht. 

Dieses Feature eignet sich am besten für große Medienobjekte, die von den Benutzern in der Regel nicht vollständig heruntergeladen werden (z.B. Videos mit progressivem Download). Dieses Feature ist daher bei Azure CDN von Verizon standardmäßig aktiviert. Standardmäßig wird der Edgeserver gezwungen, einen Hintergrundabruf des Medienobjekts vom Ursprungsserver zu initiieren. Anschließend befindet sich das Asset im lokalen Cache des Edgeservers. Sobald sich das vollständige Medienobjekt im Cache befindet, kann der Edgeserver Bytebereichsanforderungen an das CDN für das zwischengespeicherte Objekt beantworten.

Das Standardverhalten zum vollständigen Auffüllen des Caches kann über das Regelmodul im Tarif Verizon Premium deaktiviert werden.

### <a name="peer-cache-fill-hotfiling"></a>Peercache-Auffüllung bei häufig verwendeten Dateien

Dies ist ein Standardfeature von Azure CDN von Verizon, bei dem ein komplexer proprietärer Algorithmus basierend auf Metriken wie Bandbreite und aggregierten Anforderungen zusätzliche Edgeserver für das Caching nutzen kann, um Clientanforderungen für große und beliebte Objekte zu beantworten. Dies verhindert Situationen, in denen eine große Anzahl von zusätzlichen Anforderungen an den Ursprungsserver eines Kunden gesendet werden würden. 

### <a name="conditions-for-large-file-optimization"></a>Bedingungen für die Optimierung großer Dateien

Die Optimierungsfeatures für Verizon sind standardmäßig aktiviert, und es gelten keine Beschränkungen für die maximale Dateigröße. 

## <a name="additional-considerations"></a>Weitere Überlegungen

Es gibt einige zusätzliche Aspekte, die bei der Verwendung dieses Optimierungstyps berücksichtigt werden müssen.
 
### <a name="azure-cdn-from-akamai"></a>Azure CDN von Akamai

- Die Aufteilung in Blöcke führt zu zusätzlichen Anforderungen an den Ursprungsserver, dafür ist aber die gesamte Datenmenge, die vom Ursprungsserver übermittelt werden muss, wesentlich kleiner, da eine Aufteilung in Blöcke zu besseren Cachingergebnissen im CDN führt.
- Ein weiterer Vorteil ist die geringere Arbeitsspeicher- und E/A-Auslastung am Ursprungsserver, da kleinere Teile der Datei übermittelt werden. 
- Für die Blöcke, die im CDN zwischengespeichert werden, sind keine zusätzlichen Anforderungen an den Ursprungsserver erforderlich, bis der Inhalt im Cache abläuft oder aus anderen Gründen aus dem Cache entfernt wird. 
- Der Benutzer kann Bereichsanforderungen an das CDN übermitteln, die wie bei normalen Dateien behandelt werden. Die Optimierung wird nur angewendet, wenn es sich um einen gültigen Dateityp handelt und der Bytebereich zwischen 10 MB und 150 GB liegt. Wenn die durchschnittlich angeforderte Dateigröße kleiner als 10 MB ist, empfiehlt es sich, stattdessen die allgemeine Webbereitstellung zu verwenden.

### <a name="azure-cdn-from-verizon"></a>Azure CDN von Verizon

Die Optimierung für allgemeine Webbereitstellungen kann auch für große Dateien verwendet werden.

