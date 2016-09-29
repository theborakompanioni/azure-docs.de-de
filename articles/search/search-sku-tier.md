<properties
	pageTitle="Auswählen einer SKU oder eines Tarifs für Azure Search | Microsoft Azure"
	description="Azure Search kann unter folgenden SKUs bereitgestellt werden: Free, Basic und Standard. Standard ist mit verschiedenen Ressourcenkonfigurationen und Kapazitäten verfügbar."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="jhubbard"
	editor=""
    tags="azure-portal"/>

<tags
	ms.service="search"
	ms.devlang="NA"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.date="08/08/2016"
	ms.author="heidist"/>

# Auswählen einer SKU oder eines Tarifs für Azure Search

Im Zuge der [Dienstbereitstellung](search-create-service-portal.md) müssen Sie eine SKU oder einen Tarif angeben. Folgende Optionen stehen zur Verfügung: **Free**, **Basic** und **Standard**, wobei **Standard** mit verschiedenen Konfigurationen und Kapazitäten verfügbar ist.

Es wird empfohlen, immer einen Dienst vom Typ **Free** bereitzustellen (jeweils einen pro Abonnement, ohne zeitliche Befristung), der jederzeit für Projekte von geringem Umfang zur Verfügung steht. Verwenden Sie den Dienst im Tarif **Free** zum Testen und Evaluieren. Erstellen Sie im Tarif **Basic** oder **Standard** einen zweiten abrechenbaren Dienst für die Produktion oder für umfangreichere Testworkloads.

Die SKU bestimmt in Azure Search nicht die Verfügbarkeit von Features, sondern die Kapazität. Alle Features, einschließlich der Vorschaufeatures, stehen in jedem Tarif zur Verfügung.

## Wählen des passenden Tarifs

Zwischen der Kapazität und den Kosten für die Dienstausführung besteht ein direkter Zusammenhang. Auf der Grundlage der Informationen in diesem Artikel können Sie entscheiden, welche SKU das passende Verhältnis bietet. Dazu benötigen Sie aber mindestens eine grobe Schätzung für Folgendes:

- Anzahl und Größe der zu erstellenden Indizes
- Anzahl und Größe der hochzuladenden Dokumente
- Ungefähre Vorstellung des Abfrageaufkommens (Abfragen pro Sekunde)

Anzahl und Größe sind wichtig, da Obergrenzen durch einen festen Grenzwert für die Anzahl von Indizes oder Dokumenten in einem Dienst oder für vom Dienst verwendete Ressourcen (Speicher oder Replikate) erreicht werden. Der tatsächliche Grenzwert für den Dienst ist dann der Wert, der zuerst aufgebraucht ist: Ressourcen oder Objekte.

Wenn Sie über diese Schätzwerte verfügen, gehen Sie der Einfachheit halber wie folgt vor:

- **Schritt 1:** Machen Sie sich anhand der SKU-Beschreibungen weiter unten mit den verfügbaren Optionen vertraut.
- **Schritt 2:** Beschäftigen Sie sich mit den Fragen, um die Optionen einzugrenzen.
- **Schritt 3:** Überprüfen Sie Ihre Entscheidung anhand fester Grenzwerte für Speicher und Preise.

> [AZURE.NOTE] Wenn Sie die Kapazität unterschätzen, müssen Sie einen neuen Dienst in einem höheren Tarif bereitstellen und Ihre Indizes anschließend neu laden. Für einen Dienst kann kein direktes SKU-Upgrade durchgeführt werden.

## SKU-Beschreibungen

In der folgenden Tabelle werden die einzelnen Tarife beschrieben:

Tarif|Primäre Szenarien
----|-----------------
**Free**|Ein gemeinsam genutzter, kostenloser Dienst für Evaluierung, Untersuchung oder kleine Workloads. Da der Dienst gemeinsam mit anderen Abonnenten genutzt wird, hängen Abfragedurchsatz und Indizierung davon ab, wer den Dienst sonst noch verwendet. Kapazität: gering (50 MB oder drei Indizes mit je bis zu 10.000 Dokumenten).
**Basic**|Kleine Produktionsworkloads auf dedizierter Hardware. Hochverfügbar. Kapazität: bis zu drei Replikate und eine Partition (2 GB).
**S1**|Standard 1 unterstützt flexible Kombinationen aus Partitionen (12) und Replikaten (12) für mittlere Produktionsworkloads auf dedizierter Hardware. Sie können Kombinationen aus Partitionen und Replikaten mit Unterstützung von maximal 36 abrechenbaren Sucheinheiten zuordnen. Auf dieser Ebene beträgt die Partitionsgröße jeweils 25 GB, und der QPS-Wert liegt bei etwa 15 Abfragen pro Sekunde.
**S2**|Standard 2 führt größere Produktionsworkloads mit den gleichen 36 Sucheinheiten wie bei S1, aber mit größeren Partitionen und Replikaten aus. Auf dieser Ebene beträgt die Partitionsgröße jeweils 100 GB, und der QPS-Wert liegt bei ca. 60 Abfragen pro Sekunde.
**S3** (Vorschau)|Standard 3 führt proportional größere Produktionsworkloads auf leistungsstärkeren Systemen in Konfigurationen mit bis zu 12 Partitionen oder 12 Replikaten unter 36 Sucheinheiten aus. Auf dieser Ebene beträgt die Partitionsgröße jeweils 200 GB, und der QPS-Wert liegt bei über 60 Abfragen pro Sekunde. S3 befindet sich in der Vorschauphase und ist zu einem Einführungspreis erhältlich.
**S3 HD** (Vorschau)|Standard 3 mit hoher Dichte ist für eine große Anzahl von kleineren Indizes konzipiert. Nur eine einzelne Partition (200 GB). Mehr als 60 Abfragen pro Sekunde. S3 befindet sich in der Vorschauphase und ist zu einem Einführungspreis erhältlich.

> [AZURE.NOTE] Maximalwerte für Replikate und Partitionen werden als Sucheinheiten (36 Einheiten pro Dienst) abgerechnet. Dadurch ergibt sich effektiv ein niedrigerer Grenzwert als der Maximalwert zunächst vermuten lässt. Wenn Sie also etwa die maximal zulässige Anzahl von 12 Replikaten verwenden möchten, können maximal drei Partitionen (12 * 3 = 36 Einheiten) vorhanden sein. Analog dazu gilt: Wenn Sie die maximale Anzahl von Partitionen verwenden möchten, muss die Anzahl der Replikate auf drei verringert werden. Ein Diagramm mit zulässigen Kombinationen finden Sie unter [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Workloads in Azure Search](search-capacity-planning.md).

## Überprüfen von Grenzwerte pro Tarif

Das folgende Diagramm stellt einen Teil der Grenzwerte aus [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md) dar. Es enthält die Faktoren, die in der Regel für eine SKU-Entscheidung besonders relevant sind. Dieses Diagramm können Sie im Zusammenhang mit den weiter unten gestellten Fragen heranziehen.

Ressource|Kostenlos|Basic|S1|S2|S3 <br/>(Vorschau) |S3 HD <br/>(Vorschau) 
---|---|---|---|----|---|----
Vereinbarung zum Servicelevel (SLA)|Nein <sup>1</sup> |Ja |Ja |Ja |Nein <sup>1</sup> |Nein <sup>1</sup> 
Zulässige Indizes pro SKU|3|5|50|200|200|1000
Grenzwerte für Dokumente|10\.000 insgesamt|1 Million pro Dienst|15 Mio. pro Partition |60 Mio. pro Partition|120 Mio. pro Partition |1 Mio. pro Index
Maximale Anzahl der Partitionen|– |1 |12 |12 |12|1
Partitionsgröße|50 MB insgesamt|2 GB pro Dienst|25 GB pro Partition |100 GB pro Partition (bis zu maximal 1,2 TB pro Dienst)|200 GB pro Partition (bis zu maximal 2,4 TB pro Dienst)|200 GB (für die einzelne Partition)
Maximale Anzahl der Replikate|– |3 |12 |12 |12|12
Abfragen pro Sekunde|N/V|~3 pro Replikat|~15 pro Replikat|~60 pro Replikat|>60 pro Replikat|>60 pro Replikat

<sup>1</sup> Für Free- und Vorschau-SKUs gibt es keine SLAs. SLAs werden angewendet, sobald eine SKU allgemein verfügbar ist.


## Entfernen von SKUs, die die Anforderungen nicht erfüllen 

Die folgenden Fragen können dabei helfen, die passende SKU für Ihre Workload ermitteln:

1. Sind Anforderungen aufgrund einer **Vereinbarung zum Servicelevel (Service Level Agreement, SLA)** vorhanden? Beschränken Sie sich bei SKU-Entscheidung auf Basic und Standard (keine Vorschau).
2. **Wie viele Indizes** benötigen Sie? Eine der größten Variablen, die es bei der SKU-Entscheidung zu berücksichtigen gilt, ist die Anzahl von Indizes, die die jeweilige SKU unterstützt. Die Indexunterstützung ist bei den günstigeren Tarifen sehr unterschiedlich ausgeprägt. Die Anforderungen hinsichtlich der Indexanzahl kann ein Hauptentscheidungskriterium für eine bestimmte SKU sein.
3. **Wie viele Dokumente** werden in die einzelnen Indizes geladen? Anzahl und Größe der Dokumente bestimmen letztlich die Größe des Index. Wenn Sie die voraussichtliche Größe des Index abschätzen können, können Sie diesen Wert mit der Partitionsgröße pro SKU vergleichen und dabei auch die Anzahl von Partitionen einbeziehen, die zum Speichern eines Index mit entsprechender Größe erforderlich sind.
4. **Welches Abfrageaufkommen erwarten Sie**? Nach der Klärung des Speicherbedarfs können Sie sich den Abfrageworkloads zuwenden. Die SKUs S2 und S3 bieten beide einen nahezu identischen Durchsatz, Vorschau-SKUs kommen jedoch unter Umständen aufgrund von SLA-Anforderungen nicht infrage.

Auf der Grundlage der Antworten auf diese vier Fragen können die meisten Kunden eine bestimmte SKU ein- oder ausschließen. Sollten Sie sich immer noch unsicher sein, wenden Sie sich an den Azure-Support.

## Überprüfung der Entscheidung: Sind Speicherplatz und QPS der SKU ausreichend?

Sehen Sie sich im letzten Schritt noch einmal die [Preise](https://azure.microsoft.com/pricing/details/search/) und die [Abschnitte zu dienst- und indexspezifischen Grenzwerten](search-limits-quotas-capacity.md) an, um Ihre Prognosen anhand der Abonnement- und Dienstgrenzwerte zu überprüfen.

Sollten die Preis- oder die Speicheranforderungen nicht erfüllt werden, empfiehlt es sich unter Umständen, die Workloads beispielsweise auf mehrere kleinere Dienste aufzuteilen. Im Detail könnten Sie etwa Indizes neu gestalten, um sie zu verkleinern, oder Abfragen mithilfe von Filtern optimieren.

> [AZURE.NOTE] Der Speicherbedarf kann übermäßig zunehmen, wenn Dokumente überflüssige Daten enthalten. Im Idealfall enthalten Dokumente ausschließlich durchsuchbare Daten oder Metadaten. Binärdaten sind nicht durchsuchbar und sollten separat (etwa in einer Azure-Tabelle oder in einem Blobspeicher) und mit einem Feld im Index gespeichert werden, das einen URL-Verweis auf die externen Daten enthält. Die maximale Größe eines einzelnen Dokuments beträgt 16 MB (oder weniger, wenn Sie im Rahmen einer einzelnen Anforderung mehrere Dokumente gleichzeitig hochladen). Weitere Informationen finden Sie unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md).

## Nächster Schritt

Wenn Sie die optimale SKU ermittelt haben, fahren Sie mit den folgenden Schritte fort:

- [Erstellen eines Azure Search-Diensts über das Azure-Portal](search-create-service-portal.md)
- [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Arbeitslasten in Azure Search](search-capacity-planning.md)

<!---HONumber=AcomDC_0914_2016-->