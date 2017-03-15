---
title: "Vermeiden von Dienstunterbrechungen bei Azure Stream Analytics-Aufträgen | Microsoft Docs"
description: "Anleitung für Resilienz von Stream Analytics-Aufträgen bei Upgrades"
services: stream-analytics
documentationCenter: 
authors: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/27/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: a2892343432f7dced535efb3917d915736580dfb
ms.openlocfilehash: cf8eba0f68e1e803026079f02b91f1bbaec189da
ms.lasthandoff: 03/01/2017

---

# <a name="guarantee-stream-analytics-job-reliability-during-service-updates"></a>Gewährleisten der Zuverlässigkeit von Stream Analytics-Aufträgen während Dienstupdates

Ein vollständig verwalteter Dienst bietet unter anderem die Möglichkeit, dass er sich im Handumdrehen um neue Dienstfunktionen und Verbesserungen erweitern lässt. Somit kann für Stream Analytics wöchentlich (oder häufiger) eine Bereitstellung von Dienstupdates durchgeführt werden. Trotz vieler und gründlicher Tests lässt sich nicht gänzlich ausschließen, dass ein vorhandener, aktuell ausgeführter Auftrag aufgrund eines Fehlers unter Umständen unterbrochen wird. Für Kunden, die wichtige Streamingverarbeitungsaufträge ausführen, müssen derartige Risiken vermieden werden. Das Modell der **[Azure-Regionspaare](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)** stellt eine Möglichkeit dar, mit der Kunden dieses Risiko verringern können. 

## <a name="how-do-azure-paired-regions-address-this-concern"></a>Wie wirken Azure-Regionspaare diesem Risiko entgegen?

Mit Stream Analytics wird gewährleistet, dass Aufträge in Regionspaaren in separaten Batches aktualisiert werden. Dies führt zu einer ausreichenden Zeitspanne zwischen den Updates, sodass mögliche Unterbrechungsfehler ermittelt und behoben werden können.

_Mit Ausnahme der Region „Indien, Mitte“_ (deren gekoppelte Region „Indien, Süden“ für Stream Analytics nicht verfügbar ist), erfolgt die Bereitstellung eines Updates für Stream Analytics in einer Gruppe von Regionspaaren nicht gleichzeitig. Bereitstellungen in mehreren Regionen **in derselben Gruppe** können **gleichzeitig** erfolgen.

In der folgenden Tabelle sind die verschiedenen Regionspaare aufgeführt:

Regionen der Gruppe A |  | Regionen der Gruppe B
------- | ------- | -------
Japan Ost | gekoppelt mit | Japan (Westen)
Nordeuropa |  | Westeuropa
USA (Mitte) |  | USA (Ost&2;)
Asien (Osten) |  | Südostasien
USA (Mitte/Norden) |  | USA (Mitte/Süden)
Australien (Osten) |  | Australien, Südosten
USA (Ost) |  | USA (West)
Brasilien Süd |  | USA (Mitte/Süden)
China, Norden |  | China, Osten
Deutschland, Nordosten |  | Deutschland, Mitte

Für Kunden empfiehlt es sich, identische Aufträge für beide gekoppelte Regionen bereitzustellen. Neben der internen Überwachung von Stream Analytics wird Kunden zudem empfohlen, die Aufträge so zu überwachen, als handle es sich bei **beiden** um Produktionsaufträge. Wenn eine Unterbrechung als Folge des Stream Analytics-Dienstupdates ermittelt wird, werden die entsprechende Eskalation und ein Failover der Downstreamconsumer an die fehlerfreie Auftragsausgabe durchgeführt. Die Eskalation zum Support verhindert, dass die gekoppelte Region von der neuen Bereitstellung betroffen ist, und gewährleistet die Integrität der gekoppelten Aufträge.

