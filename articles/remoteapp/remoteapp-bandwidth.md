---
title: "Schätzen der Netzwerkbandbreiten-Nutzung von Azure RemoteApp | Microsoft Docs"
description: "Lernen Sie die Anforderungen an die Netzwerkbandbreite für Azure RemoteApp-Sammlungen und -Apps kennen."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 3127f4c7-f532-46c3-ba9b-649f647abec1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: e4d94d3f9736378d93e93be6645ed04ade763ca3
ms.openlocfilehash: 045410664ec70c846abbf32a36dcc8f70ac471be


---
# <a name="estimate-azure-remoteapp-network-bandwidth-usage"></a>Schätzen der Netzwerkbandbreiten-Nutzung von Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Azure RemoteApp verwendet das Remotedesktopprotokoll (Remote Desktop Protocol, RDP) zur Kommunikation zwischen Anwendungen, die in der Azure-Cloud ausgeführt werden, und Ihren Benutzern. Dieser Artikel enthält einige grundlegende Richtlinien, mit der Sie diese Netzwerknutzung einschätzen und potenziell die Nutzung der Netzwerkbandbreite durch die einzelnen Azure RemoteApp-Benutzer beurteilen können.

Die Einschätzung der Bandbreitennutzung pro Benutzer ist sehr komplex und erfordert die gleichzeitige Ausführung mehrerer Anwendungen in Multitaskingszenarien, wobei sich die Anwendungen abhängig von ihrem Netzwerkbandbreiten-Bedarf gegenseitig in der Leistung beeinträchtigen können. Auch der Typ des Remotedesktopclients (z. B. Macintosh-Client oder HTML5-Client) kann zu verschiedenen Bandbreitenergebnissen führen. Um Ihnen zu helfen, diese Komplikationen zu bewältigen, unterteilen wir die Verwendungsszenarien in mehrere allgemeine Kategorien, um reale Szenarien zu reproduzieren. (In der Praxis sind Szenarien natürlich in der Regel Mischformen der Kategorien und unterscheiden sich von Benutzer zu Benutzer.)

Bevor wir fortfahren, beachten Sie bitte, dass wir voraussetzen, dass RDP in den meisten Szenarien in Netzwerken mit einer Latenz unter 120 ms und Bandbreite über 5 MB gute bis hervorragende Ergebnisse garantiert. Dafür ausschlaggebend ist die Fähigkeit von RDP zur dynamischen Anpassung durch Nutzung der verfügbaren Netzwerkbandbreite und Berücksichtigung des geschätzten Anwendungsbandbreitenbedarfs. Dieser Artikel erweitert den Horizont über „die meisten Verwendungsszenarien“ heraus, um die Grenze zu untersuchen, bei der Szenarien außer Kontrolle geraten und sich die Benutzerfreundlichkeit verschlechtert.

Informieren Sie sich jetzt in den folgenden Artikeln über die Details, inklusive zu berücksichtigender Faktoren, grundsätzlicher Empfehlungen und dessen, was wir nicht in unsere Schätzungen einbezogen haben.

* [Wie wirken Netzwerkbandbreite und Qualität der Benutzerfreundlichkeit zusammen?](remoteapp-bandwidthexperience.md)
* [Testen Ihrer Netzwerkbandbreiten-Nutzung mit einigen gängigen Szenarien](remoteapp-bandwidthtests.md)
* [Kurzrichtlinien, falls Ihnen die Zeit oder die Möglichkeit zum Testen fehlt](remoteapp-bandwidthguidelines.md)

## <a name="what-are-we-not-including"></a>Was beziehen wir nicht ein?
Beachten Sie bei der Durchsicht der vorgeschlagenen Tests und unseren gesamten (und zugegebenermaßen allgemeinen) Empfehlungen, dass wir einige Faktoren nicht berücksichtigt haben. Dazu zählen z. B. die Komplikationen hinsichtlich der Benutzerfreundlichkeit, die der asymmetrische Charakter der Upload- bzw. Downloadbandbreite mit sich bringt. Der asymmetrische Charakter der meisten Wi-Fi-Netzwerke wirkt sich außerdem auf die Leistung und die Benutzerfreundlichkeit aus. In interaktiven Szenarien kann der Downstreamdatenverkehr eine niedrigere Priorität erhalten als der Upstreamdatenverkehr. Dies kann die Anzahl der verloren gegangenen Video- oder Audioframes erhöhen und so die Benutzerfreundlichkeit beim Streaming beeinträchtigen. Sie können mit eigenen Experimenten feststellen, was für Ihren konkreten Anwendungsfall und Ihr Netzwerk gut ist.

Wir behandeln zwar die Geräteumleitung, berücksichtigen jedoch nicht die Auswirkung des durch angeschlossene Geräte wie Speicher, Drucker, Scanner, Webcams und andere USB-Geräte verursachten Netzwerkverkehrs auf die Bandbreite. Diese Geräte verursachen in der Regel einen vorübergehenden Spitzenbedarf an Netzwerkbandbreite, der verschwindet, wenn der Vorgang abgeschlossen ist. Wenn dies jedoch häufig vorkommt, könnte sich dieser Bandbreitenbedarf bemerkbar machen.

Wir erörtern auch nicht, wie ein Benutzer andere Benutzer im gleichen Netzwerk beeinträchtigen kann. Beispielsweise kann ein Benutzer bei der Wiedergabe von 4K-Video über ein 100-MB/s-Netzwerk andere Benutzer im gleichen Netzwerk deutlich beeinträchtigen, die versuchen, das Gleiche zu tun. Leider wird es zunehmend schwieriger, die Auswirkung der gleichzeitigen Nutzung zu bestimmen, um eine allgemeine oder allumfassende Einschätzung der Gesamtleistung des Systems abzugeben. Wir können lediglich sagen, dass die zu Grunde liegende Protokolltechnologie die verfügbare Netzwerkbandbreite zwar bestmöglich nutzt, aber ihre Grenzen hat.




<!--HONumber=Dec16_HO2-->


