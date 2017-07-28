---
title: "Auswählen einer SKU oder eines Tarifs für Azure Search | Microsoft Docs"
description: "Azure Search kann unter folgenden SKUs bereitgestellt werden: Free, Basic und Standard. Standard ist mit verschiedenen Ressourcenkonfigurationen und Kapazitäten verfügbar."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 8d4b7bca-02a5-43ee-b3f8-03551dfb32fd
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/24/2016
ms.author: heidist
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: f9f3a7b2369818791ffac1c8eeccef45216c2ff0
ms.contentlocale: de-de
ms.lasthandoff: 06/08/2017

---

# <a name="choose-a-sku-or-pricing-tier-for-azure-search"></a>Auswählen einer SKU oder eines Tarifs für Azure Search
In Azure Search erfolgt die [Bereitstellung eines Diensts](search-create-service-portal.md) für einen bestimmten Tarif oder eine bestimmte SKU. Folgende Optionen stehen zur Verfügung: **Free**, **Basic** und **Standard**, wobei **Standard** mit verschiedenen Konfigurationen und Kapazitäten verfügbar ist.

Dieser Artikel soll Ihnen bei der Auswahl eines Tarifs helfen. Wenn sich die Kapazität eines Tarifs als zu gering herausstellt, müssen Sie einen neuen Dienst in einem höheren Tarif bereitstellen und die Indizes anschließend neu laden. Für einen Dienst kann kein direktes SKU-Upgrade durchgeführt werden.

> [!NOTE]
> Nach dem Auswählen eines Tarifs und dem [Bereitstellen eines Suchdiensts](search-create-service-portal.md) können Sie die Anzahl der Replikate und Partitionen im Dienst erhöhen. Eine entsprechende Anleitung finden Sie unter [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Workloads in Azure Search](search-capacity-planning.md).
>
>

## <a name="how-to-approach-a-pricing-tier-decision"></a>Wählen des passenden Tarifs
Der Tarif bestimmt in Azure Search nicht die Verfügbarkeit von Features, sondern die Kapazität. Features, einschließlich der Vorschaufunktionen, stehen im Allgemeinen in jedem Tarif zur Verfügung. Als einzige Ausnahme werden Indexer in S3 HD nicht unterstützt.

> [!TIP]
> Es wird empfohlen, immer einen Dienst vom Typ **Free** bereitzustellen (jeweils einen pro Abonnement, ohne zeitliche Befristung), der jederzeit für Projekte von geringem Umfang zur Verfügung steht. Verwenden Sie den Dienst im Tarif **Free** zum Testen und Evaluieren. Erstellen Sie im Tarif **Basic** oder **Standard** einen zweiten abrechenbaren Dienst für die Produktion oder für umfangreichere Testworkloads.
>
>

Zwischen der Kapazität und den Kosten für die Dienstausführung besteht ein direkter Zusammenhang. Auf der Grundlage der Informationen in diesem Artikel können Sie entscheiden, welche SKU das passende Verhältnis bietet. Dazu benötigen Sie aber mindestens eine grobe Schätzung für Folgendes:

* Anzahl und Größe der zu erstellenden Indizes
* Anzahl und Größe der hochzuladenden Dokumente
* Ungefähre Vorstellung des Abfrageaufkommens (Abfragen pro Sekunde)

Anzahl und Größe sind wichtig, da Obergrenzen durch einen festen Grenzwert für die Anzahl von Indizes oder Dokumenten in einem Dienst oder für vom Dienst verwendete Ressourcen (Speicher oder Replikate) erreicht werden. Der tatsächliche Grenzwert für den Dienst ist dann der Wert, der jeweils zuerst aufgebraucht ist: Ressourcen oder Objekte.

Wenn Sie über diese Schätzwerte verfügen, gehen Sie der Einfachheit halber wie folgt vor:

* **Schritt 1:** Machen Sie sich anhand der SKU-Beschreibungen weiter unten mit den verfügbaren Optionen vertraut.
* **Schritt 2:** Treffen Sie anhand der Antworten auf die folgenden Fragen eine Vorentscheidung.
* **Schritt 3:** Treffen Sie Ihre endgültige Entscheidung durch Überprüfen der festen Grenzwerte für Speicher und Preise.

## <a name="sku-descriptions"></a>SKU-Beschreibungen
In der folgenden Tabelle werden die einzelnen Tarife beschrieben:

| Tarif | Primäre Szenarien |
| --- | --- |
| **Free** |Ein gemeinsam genutzter, kostenloser Dienst für Evaluierung, Untersuchung oder kleine Workloads. Da der Dienst gemeinsam mit anderen Abonnenten genutzt wird, hängen Abfragedurchsatz und Indizierung davon ab, wer den Dienst sonst noch verwendet. Kapazität: gering (50 MB oder drei Indizes mit je bis zu 10.000 Dokumenten). |
| **Basic** |Kleine Produktionsworkloads auf dedizierter Hardware. Hochverfügbar. Kapazität: bis zu drei Replikate und eine Partition (2 GB). |
| **S1** |Standard 1 unterstützt flexible Kombinationen aus Partitionen (12) und Replikaten (12) für mittlere Produktionsworkloads auf dedizierter Hardware. Sie können Kombinationen aus Partitionen und Replikaten mit Unterstützung von maximal 36 abrechenbaren Sucheinheiten zuordnen. Auf dieser Ebene beträgt die Partitionsgröße jeweils 25 GB, und der QPS-Wert liegt bei etwa 15 Abfragen pro Sekunde. |
| **S2** |Standard 2 führt größere Produktionsworkloads mit den gleichen 36 Sucheinheiten wie bei S1, aber mit größeren Partitionen und Replikaten aus. Auf dieser Ebene beträgt die Partitionsgröße jeweils 100 GB, und der QPS-Wert liegt bei ca. 60 Abfragen pro Sekunde. |
| **S3** |Standard 3 führt proportional größere Produktionsworkloads auf leistungsstärkeren Systemen in Konfigurationen mit bis zu 12 Partitionen oder 12 Replikaten unter 36 Sucheinheiten aus. Auf dieser Ebene beträgt die Partitionsgröße jeweils 200 GB, und der QPS-Wert liegt bei über 60 Abfragen pro Sekunde. |
| **S3 HD** |Standard 3 mit hoher Dichte ist für eine große Anzahl von kleineren Indizes konzipiert. Bis zu 3 Partitionen mit jeweils 200 GB sind möglich. Mehr als 60 Abfragen pro Sekunde. |

> [!NOTE]
> Maximalwerte für Replikate und Partitionen werden als Sucheinheiten (36 Einheiten pro Dienst) abgerechnet. Dadurch ergibt sich effektiv ein niedrigerer Grenzwert als der Maximalwert zunächst vermuten lässt. Wenn Sie also etwa die maximal zulässige Anzahl von 12 Replikaten verwenden möchten, können maximal drei Partitionen (12 * 3 = 36 Einheiten) vorhanden sein. Analog dazu gilt: Wenn Sie die maximale Anzahl von Partitionen verwenden möchten, muss die Anzahl der Replikate auf drei verringert werden. Eine Tabelle mit zulässigen Kombinationen finden Sie unter [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Workloads in Azure Search](search-capacity-planning.md).
>
>

## <a name="review-limits-per-tier"></a>Überprüfen von Grenzwerte pro Tarif
Das folgende Diagramm stellt einen Teil der Grenzwerte aus [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md)dar. Es enthält die Faktoren, die in der Regel für eine SKU-Entscheidung besonders relevant sind. Dieses Diagramm können Sie im Zusammenhang mit den weiter unten gestellten Fragen heranziehen.

| Ressource | Kostenlos | Basic | S1 | S2 | S3 | S3 HD |
| --- | --- | --- | --- | --- | --- | --- |
| Vereinbarung zum Servicelevel (SLA) |Nein <sup>1</sup> |Ja |Ja |Ja |Ja |Ja |
| Indexgrenzwerte |3 |5 |50 |200 |200 |1000 <sup>2</sup> |
| Dokumentgrenzwerte |10.000 insgesamt |1 Million pro Dienst |15 Mio. pro Partition |60 Mio. pro Partition |120 Mio. pro Partition |1 Mio. pro Index |
| Maximale Anzahl der Partitionen |– |1 |12 |12 |12 |3 <sup>2</sup> |
| Partitionsgröße |50 MB insgesamt |2 GB pro Dienst |25 GB pro Partition |100 GB pro Partition (bis zu maximal 1,2 TB pro Dienst) |200 GB pro Partition (bis zu maximal 2,4 TB pro Dienst) |200 GB (bis zu maximal 600 GB pro Dienst) |
| Maximale Anzahl der Replikate |– |3 |12 |12 |12 |12 |
| Abfragen pro Sekunde |– |~3 pro Replikat |~15 pro Replikat |~60 pro Replikat |>60 pro Replikat |>60 pro Replikat |

<sup>1</sup> Für Funktionen der Free- und Vorschauversion gilt keine Vereinbarung zum Servicelevel (Service Level Agreement, SLA). Für alle abrechenbaren Tarife gelten SLAs, wenn Sie genügend Redundanz für Ihren Dienst bereitstellen. Zwei oder mehr Replikate sind für die Abfrage-SLA (Lesezugriff) erforderlich. Drei oder mehr Replikate sind für die Abfrage- und Indizierungs-SLA (Lese-/Schreibzugriff) erforderlich. Die Anzahl der Partitionen wird für die SLA nicht berücksichtigt. 

<sup>2</sup> S3 und S3 HD werden durch eine Infrastruktur mit identischer hoher Verfügbarkeit unterstützt, der jeweilige maximale Grenzwert wird jedoch auf unterschiedliche Weise erreicht. S3 ist für eine kleinere Anzahl sehr umfangreicher Indizes ausgelegt. Damit ist der maximale Grenzwert ressourcengebunden (2,4 TB für jeden Dienst). S3 HD ist für eine große Zahl sehr kleiner Indizes ausgelegt. Bei 1.000 Indizes erreicht S3 HD seine Grenzwerte in Form von Indexeinschränkungen. Wenn Sie als S3 HD-Kunde mehr als 1.000 Indizes benötigen, erhalten Sie beim Microsoft-Support Informationen zur entsprechenden Vorgehensweise.

## <a name="eliminate-skus-that-dont-meet-requirements"></a>Entfernen von SKUs, die die Anforderungen nicht erfüllen
Die folgenden Fragen können dabei helfen, die passende SKU für Ihre Workload ermitteln:

1. Sind Anforderungen aufgrund einer **Vereinbarung zum Servicelevel (Service Level Agreement, SLA)** vorhanden? Sie können einen beliebigen abrechenbaren Tarif (Basic oder höher) verwenden, müssen aber Ihren Dienst für Redundanz konfigurieren. Zwei oder mehr Replikate sind für die Abfrage-SLA (Lesezugriff) erforderlich. Drei oder mehr Replikate sind für die Abfrage- und Indizierungs-SLA (Lese-/Schreibzugriff) erforderlich. Die Anzahl der Partitionen wird für die SLA nicht berücksichtigt.
2. **Wie viele Indizes** benötigen Sie? Eine der größten Variablen, die es bei der SKU-Entscheidung zu berücksichtigen gilt, ist die Anzahl von Indizes, die die jeweilige SKU unterstützt. Die Indexunterstützung ist bei den günstigeren Tarifen sehr unterschiedlich ausgeprägt. Die Anforderungen hinsichtlich der Indexanzahl kann ein Hauptentscheidungskriterium für eine bestimmte SKU sein.
3. **Wie viele Dokumente** werden in die einzelnen Indizes geladen? Anzahl und Größe der Dokumente bestimmen letztlich die Größe des Index. Wenn Sie die voraussichtliche Größe des Index abschätzen können, können Sie diesen Wert mit der Partitionsgröße pro SKU vergleichen und dabei auch die Anzahl von Partitionen einbeziehen, die zum Speichern eines Index mit entsprechender Größe erforderlich sind.
4. **Welches Abfrageaufkommen erwarten Sie**? Nach der Klärung des Speicherbedarfs können Sie sich den Abfrageworkloads zuwenden. Die SKUs S2 und S3 bieten beide einen nahezu identischen Durchsatz, Vorschau-SKUs kommen jedoch unter Umständen aufgrund von SLA-Anforderungen nicht infrage.
5. Wenn Sie den Tarif S2 oder S3 in Betracht ziehen, müssen Sie bestimmen, ob Sie [Indexer](search-indexer-overview.md) benötigen. Indexer stehen für den Tarif S3 HD noch nicht zur Verfügung. Alternativ dazu kann ein Pushmodell für Indexupdates verwendet werden, in dem Sie den Anwendungscode schreiben, um ein Dataset mithilfe von Push in einen Index zu übertragen.

Auf der Grundlage der Antworten auf die Fragen oben können die meisten Kunden eine bestimmte SKU ein- oder ausschließen. Sollten Sie sich immer noch unsicher sein, können Sie Fragen im MSDN- oder StackOverflow-Forum stellen oder sich an den Azure-Support wenden.

## <a name="decision-validation-does-the-sku-offer-sufficient-storage-and-qps"></a>Überprüfung der Entscheidung: Sind Speicherplatz und QPS der SKU ausreichend?
Sehen Sie sich im letzten Schritt noch einmal die [Preise](https://azure.microsoft.com/pricing/details/search/) und die [Abschnitte zu dienst- und indexspezifischen Grenzwerten](search-limits-quotas-capacity.md) an, um Ihre Einschätzungen anhand der Abonnement- und Dienstgrenzwerte zu überprüfen.

Sollten die Preis- oder die Speicheranforderungen nicht erfüllt werden, empfiehlt es sich unter Umständen, die Workloads beispielsweise auf mehrere kleinere Dienste aufzuteilen. Im Detail könnten Sie etwa Indizes neu gestalten, um sie zu verkleinern, oder Abfragen mithilfe von Filtern optimieren.

> [!NOTE]
> Der Speicherbedarf kann übermäßig zunehmen, wenn Dokumente überflüssige Daten enthalten. Im Idealfall enthalten Dokumente ausschließlich durchsuchbare Daten oder Metadaten. Binärdaten sind nicht durchsuchbar und sollten separat (etwa in einer Azure-Tabelle oder in einem Blobspeicher) und mit einem Feld im Index gespeichert werden, das einen URL-Verweis auf die externen Daten enthält. Die maximale Größe eines einzelnen Dokuments beträgt 16 MB (oder weniger, wenn Sie im Rahmen einer einzelnen Anforderung mehrere Dokumente gleichzeitig hochladen). Weitere Informationen finden Sie unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md) .
>
>

## <a name="next-step"></a>Nächster Schritt
Wenn Sie die optimale SKU ermittelt haben, fahren Sie mit den folgenden Schritte fort:

* [Erstellen eines Azure Search-Diensts über das Azure-Portal](search-create-service-portal.md)
* [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Arbeitslasten in Azure Search](search-capacity-planning.md)

