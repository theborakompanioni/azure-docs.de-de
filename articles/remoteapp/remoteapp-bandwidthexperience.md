---
title: "Azure RemoteApp – wie wirken die Netzwerkbandbreite und die Qualität der Benutzerfreundlichkeit zusammen? | Microsoft Docs"
description: "Erfahren Sie, wie die Netzwerkbandbreite in Azure RemoteApp die Qualität der Benutzerfreundlichkeit beeinflussen kann."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 74ebc1fb-5187-4056-b08c-0e03b5ecaca6
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 1a1f772c12b2eb4470bd239f0325cc4ae18b9680
ms.lasthandoff: 03/31/2017


---
# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp – wie wirken die Netzwerkbandbreite und die Qualität der Benutzerfreundlichkeit zusammen?
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Wenn Sie die [gesamte Netzwerkbandbreite](remoteapp-bandwidth.md) betrachten, die für Azure RemoteApp erforderlich ist, bedenken Sie die folgenden Faktoren – sie sind alle Teil eines dynamischen Systems, das die gesamte Benutzerfreundlichkeit beeinflusst. 

* **Verfügbare Netzwerkbandbreite und die aktuellen Netzwerkbedingungen**: Eine Reihe von Parametern (Verlust, Latenzzeit und Jitter) im gleichen Netzwerk zu einem bestimmten Zeitpunkt kann das Streaming der Anwendung beeinträchtigen, d.h. die Benutzerfreundlichkeit insgesamt reduzieren. Die im Netzwerk verfügbare Bandbreite ist von Überlastung, zufälligem Verlust und Latenzzeit abhängig, da alle diese Parameter den Überlastungskontrollmechanismus beeinflussen, was sich wiederum auf die Übertragungsrate zur Vermeidung von Kollisionen auswirkt.  Beispielsweise sorgt ein verlustreiches Netzwerk oder ein Netzwerk mit hoher Latenz sogar dann für eine schlechte Benutzerfreundlichkeit, wenn seine Bandbreite 1.000 MB beträgt. Verlust und Latenz variieren je nach Anzahl der Benutzer im gleichen Netzwerk und deren Aktivitäten (z. B. Videos ansehen, große Dateien herunter- oder hochladen, drucken).
* **Verwendungsszenario**: Die Benutzerfreundlichkeit hängt davon ab, was die Benutzer individuell und als Gruppe im gleichen Netzwerk ausführen. Zum Lesen einer Folie muss z. B. nur ein einzelner Frame aktualisiert werden. Wenn der Benutzer den Inhalt eines Textdokuments überfliegt und scrollt, muss eine höhere Anzahl von Frames pro Sekunde aktualisiert werden. In diesem Szenario beansprucht die bidirektionale Kommunikation mit dem Server schließlich mehr Netzwerkbandbreite. Außerdem sollten Sie ein extremes Beispiel berücksichtigen: Mehrere Benutzer betrachten HD-Videos (z. B. in 4K-Auflösung), halten HD-Telefonkonferenzen ab, spielen 3D-Videospiele oder arbeiten mit CAD-Systemen. Alle diese zusammen können auch eine sehr hohe Netzwerkbandbreite praktisch unbrauchbar machen.
* **Bildschirmauflösung und die Anzahl der Bildschirme**: Um größere Bildschirme vollständig zu aktualisieren, ist mehr Netzwerkbandbreite erforderlich als bei kleineren Bildschirmen. Die zu Grunde liegende Technologie leistet hervorragende Arbeit dabei, nur die Bereiche der Bildschirme zu verschlüsseln und zu übertragen, die aktualisiert wurden, doch von Zeit zu Zeit muss der gesamte Bildschirm aktualisiert werden. Wenn der Benutzer über einen Bildschirm mit höherer Auflösung (z. B. 4 K) verfügt, erfordert die Aktualisierung mehr Netzwerkbandbreite als ein Bildschirm mit niedrigerer Auflösung (z. B. 1024x768px). Die gleiche Logik gilt, wenn Sie mehr als einen Bildschirm für die Umleitung verwenden. Die Bandbreite muss mit der Anzahl der Bildschirme zunehmen.
* **Zwischenablage und Geräteumleitung** : Dies ist kein so offensichtliches Problem, aber wenn ein Benutzer eine große Datenmenge in der Zwischenablage speichert, dauert es in vielen Fällen eine gewisse Zeit, diese Informationen aus dem Remotedesktop-Client an den Server zu übertragen. Während die Benutzerfreundlichkeit beim Herunterstreamen gut ist, kann sie beim Hochstreamen des Zwischenablageninhalts beeinträchtigt werden. Das Gleiche gilt auch für die Geräteumleitung – wenn ein Scanner oder eine Webcam eine große Datenmenge erzeugt, die an den Server hochgestreamt werden muss, oder ein Drucker ein großes Dokument empfangen muss, oder lokaler Speicher für eine in der Cloud ausgeführte App verfügbar sein muss, damit sie eine große Datei kopieren kann, könnten Benutzer feststellen, dass Frames ausgelassen werden oder das Video vorübergehend „eingefroren“ wird, da die für die Geräteumleitung erforderlichen Daten die Anforderungen an die Netzwerkbandbreite heraufsetzen. 

Betrachten Sie alle diese Faktoren beim Auswerten des Netzwerkbandbreiten-Bedarfs unbedingt als zusammenwirkendes System.

Kehren Sie nun zum [Hauptartikel über die Netzwerkbandbreite zurück](remoteapp-bandwidth.md), oder fahren Sie mit dem [Testen Ihrer Netzwerkbandbreite](remoteapp-bandwidthtests.md) fort.

## <a name="learn-more"></a>Weitere Informationen
* [Schätzen der Netzwerkbandbreiten-Nutzung von Azure RemoteApp](remoteapp-bandwidth.md)
* [Azure RemoteApp – Testen Ihrer Netzwerkbandbreiten-Nutzung mit einigen häufigen Szenarios](remoteapp-bandwidthtests.md)
* [Azure RemoteApp-Netzwerkbandbreite – allgemeine Richtlinien (wenn Sie keine eigenen erstellen können)](remoteapp-bandwidthguidelines.md)


