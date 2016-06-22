<properties
	pageTitle="Auswählen einer SKU oder eines Tarifs für Azure Search | Microsoft Azure"
	description="Azure Search kann unter folgenden SKUs bereitgestellt werden: Free, Basic und Standard. Standard ist mit verschiedenen Ressourcenkonfigurationen und Kapazitäten verfügbar."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="paulettm"
	editor=""
    tags="azure-portal"/>

<tags
	ms.service="search"
	ms.devlang="NA"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.date="06/05/2016"
	ms.author="heidist"/>

# Auswählen einer SKU oder eines Tarifs für Azure Search

Bevor Sie in Azure Search einen [Suchdienst erstellen](search-create-service-portal.md) können, müssen Sie wissen, unter welchem Tarif oder unter welcher SKU Sie den Dienst bereitstellen möchten. Folgende SKUs stehen zur Verfügung: Free, Basic und Standard. Standard ist mit verschiedenen Ressourcenkonfigurationen und Kapazitäten verfügbar.

Zwischen der Kapazität und den Kosten für die Dienstausführung besteht ein direkter Zusammenhang. Auf der Grundlage der Informationen in diesem Artikel können Sie entscheiden, welche SKU das passende Verhältnis bietet. Dazu benötigen Sie aber mindestens eine grobe Schätzung hinsichtlich der Anzahl und Größe der Indizes, die Sie erstellen möchten, sowie eine ungefähre Vorstellung des Abfrageaufkommens. Wenn Sie über diese Schätzwerte verfügen, gehen Sie der Einfachheit halber wie folgt vor:

- **Schritt 1** Machen Sie sich anhand der SKU-Beschreibungen weiter unten mit den verfügbaren Optionen vertraut.
- **Schritt 2** Beantworten Sie eine Reihe von Fragen, um die Auswahl einzugrenzen.
- **Schritt 3** Überprüfen Sie Ihre Entscheidung anhand fester Grenzwerte für Speicher und Preise. 

## SKU-Beschreibungen

Die folgende Tabelle enthält eine Beschreibung und einen einfachen Vergleich der einzelnen SKUs in Bezug auf Partitionen und Replikate sowie ggf. in Bezug auf Abfragen pro Sekunde (Queries Per Second, QPS). Eine Einführung in Replikate und Partitionen finden Sie unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md).

Preisstufe|Kapazität|Verwendungszweck
----|-----------|-----------
Kostenlos|50 MB oder drei Indizes und 10.000 Dokumente|Ein gemeinsam genutzter, kostenloser Dienst für Evaluierung, Untersuchung oder kleine Workloads. Da der Dienst gemeinsam mit anderen Abonnenten genutzt wird, hängen Abfragedurchsatz und Indizierung davon ab, wer den Dienst sonst noch verwendet.
Basic|Drei Replikate und eine Partition|Kleine Produktionsworkloads auf dedizierter Hardware. Hochverfügbar.
Standard 1 (S1)|Partitionen haben jeweils eine Größe von 25 GB. <br/><br/>Der QPS-Wert für Replikate beträgt ca. 15 Abfragen pro Sekunde.|Unterstützt flexible Kombinationen aus Partitionen (12) und Replikaten (12) für mittlere Produktionsworkloads auf dedizierter Hardware. Sie können Kombinationen aus Partitionen und Replikaten mit Unterstützung von maximal 36 abrechenbaren Sucheinheiten zuordnen.
Standard 2 (S2)|Partitionen haben jeweils eine Größe von 100 GB. <br/><br/>Der QPS-Wert für Replikate beträgt ungefähr 60 Abfragen pro Sekunde.|Führt größere Produktionsworkloads mit den gleichen Konfigurationen wie S1, aber mit größeren Partitionen und Replikaten aus.
Standard 3 (S3) – Vorschau|Partitionen haben jeweils eine Größe von 200 GB. <br/><br/>Der QPS-Wert für Replikate beträgt über 60 Abfragen pro Sekunde.|Führt proportional größere Produktionsworkloads auf leistungsstärkeren Systemen in Konfigurationen mit bis zu 12 Partitionen oder 12 Replikaten aus. 
Standard 3 mit hoher Dichte (S3 HD) – Vorschau|Eine einzelne Partition mit 200 GB. <br/><br/>Der QPS-Wert für Replikate beträgt über 60 Abfragen pro Sekunde.|Vorgesehen für Kunden mit einer großen Anzahl kleinerer Indizes.

## Entscheidungspfad für die Wahl einer SKUs

Das folgende Diagramm stellt einen Teil der Grenzwerte aus [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md) dar. Es enthält die wichtigsten Faktoren für eine SKU-Entscheidung (einschließlich SKU-Kapazitätsgrenzen aus der vorherigen Tabelle) sowie zusätzliche Grenzwerte in Verbindung mit Inhaltstypen (Indizes und Dokumente). Dieses Diagramm können Sie bei der Entscheidungsfindung heranziehen.

Ressource|Free|Basic|S1|S2|S3 <br/>(Vorschau) |S3 HD <br/>(Vorschau) 
---|---|---|---|----|---|----
Vereinbarung zum Servicelevel (SLA)|Nein <sup>1</sup> |Ja |Ja |Ja |Nein <sup>1</sup> |Nein <sup>1</sup> 
Maximale Anzahl der Partitionen|– |1 |12 |12 |12|1
Zulässige Indizes pro SKU|3|5|50|200|200|1000
Grenzwerte für Dokumente|10\.000 insgesamt|1 Million pro Dienst|15 Mio. pro Partition |60 Mio. pro Partition|120 Mio. pro Partition |1 Mio. pro Index
Partitionsgröße|50 MB insgesamt|2 GB pro Dienst|25 GB pro Partition |100 GB pro Partition (bis zu maximal 1,2 TB pro Dienst)|200 GB pro Partition (bis zu maximal 2,4 TB pro Dienst)|200 GB (für die einzelne Partition)
Maximale Anzahl der Replikate|N/V |3 |12 |12 |12|12
Abfragen pro Sekunde|–|~3 pro Replikat|~15 pro Replikat|~60 pro Replikat|>60 pro Replikat|>60 pro Replikat

<sup>1</sup> Free- und Vorschau-SKUs verfügen über keine SLAs. SLAs werden angewendet, sobald eine SKU allgemein verfügbar ist.

> [AZURE.NOTE] Maximalwerte für Replikate und Partitionen unterliegen einer kombinierten maximalen Abrechnungskonfiguration von 36 Einheiten. Dadurch ergibt sich effektiv ein niedrigerer Grenzwert als der Maximalwert zunächst vermuten lässt. Wenn Sie also etwa die maximal zulässige Anzahl von 12 Replikaten verwenden möchten, können maximal drei Partitionen (12 * 3 = 36 Einheiten) vorhanden sein. Analog dazu gilt: Wenn Sie die maximale Anzahl von Partitionen verwenden möchten, muss die Anzahl der Replikate auf drei verringert werden. Ein Diagramm mit zulässigen Kombinationen finden Sie unter [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Workloads in Azure Search](search-capacity-planning.md).

### Allgemeine Fragen im Zusammenhang mit der SKU-Wahl

Die folgenden Fragen können dabei helfen, die passende SKU für Ihre Workload ermitteln:

1. Benötigen Sie eine **Vereinbarung zum Servicelevel (Service Level Agreement, SLA)**? Beschränken Sie sich bei SKU-Entscheidung auf Basic und Standard (keine Vorschau).
2. **Wie viele Indizes** benötigen Sie? Eine der größten Variablen, die es bei der SKU-Entscheidung zu berücksichtigen gilt, ist die Anzahl von Indizes, die die jeweilige SKU unterstützt. Die Indexunterstützung ist bei den günstigeren Tarifen sehr unterschiedlich ausgeprägt. Die Anforderungen hinsichtlich der Indexanzahl kann ein Hauptentscheidungskriterium für eine bestimmte SKU sein.
3. **Wie viele Dokumente** werden in die einzelnen Indizes geladen? Anzahl und Größe der Dokumente bestimmen letztlich die Größe des Index. Wenn Sie die voraussichtliche Größe des Index abschätzen können, können Sie diesen Wert mit der Partitionsgröße pro SKU vergleichen und dabei auch die Anzahl von Partitionen einbeziehen, die zum Speichern eines Index mit entsprechender Größe erforderlich sind. 
4. **Welches Abfrageaufkommen erwarten Sie**? Nach der Klärung des Speicherbedarfs können Sie sich den Abfrageworkloads zuwenden. Die SKUs S2 und S3 bieten beide einen nahezu identischen Durchsatz, Vorschau-SKUs kommen jedoch unter Umständen aufgrund von SLA-Anforderungen nicht infrage. 

Auf der Grundlage der Antworten auf diese vier Fragen können die meisten Kunden eine bestimmte SKU ein- oder ausschließen. Sollten Sie sich immer noch unsicher sein, wenden Sie sich an den Azure-Support.

## Überprüfung der Entscheidung: Sind Speicherplatz und QPS der SKU ausreichend?

Sehen Sie sich im letzten Schritt nochmal die [Preise](https://azure.microsoft.com/pricing/details/search/) und die [dienst- und indexspezifischen Abschnitte der Diensteinschränkungen](search-limits-quotas-capacity.md) an, um Ihre Schätzungen anhand der Abonnement- und Diensteinschränkungen zu überprüfen.

Sollten die Preis- oder die Speicheranforderungen nicht erfüllt werden, empfiehlt es sich unter Umständen, die Workloads beispielsweise auf mehrere kleinere Dienste aufzuteilen. Im Detail könnten Sie etwa Indizes neu gestalten, um sie zu verkleinern, oder Abfragen mithilfe von Filtern optimieren.

> [AZURE.NOTE] Der Speicherbedarf kann übermäßig zunehmen, wenn Dokumente überflüssige Daten enthalten. Im Idealfall enthalten Dokumente ausschließlich durchsuchbare Daten oder Metadaten. Binärdaten, die ein Dokument unnötig vergrößern, sollten separat (etwa in einer Azure-Tabelle oder in einem Blobspeicher) und mit einem Feld im Index gespeichert werden, das einen URL-Verweis auf die externen Daten enthält. Die maximale Größe eines einzelnen Dokuments beträgt 16 MB (oder weniger, wenn Sie im Rahmen einer einzelnen Anforderung mehrere Dokumente gleichzeitig hochladen). Weitere Informationen finden Sie unter [Grenzwerte für den Azure Search-Dienst](search-limits-quotas-capacity.md).

## Nächster Schritt

Wenn Sie die optimale SKU ermittelt haben, fahren Sie mit den folgenden Schritte fort:
- [Erstellen eines Azure Search-Diensts über das Azure-Portal](search-create-service-portal.md)
- [Skalieren von Ressourcenebenen für Abfrage und Indizierung von Arbeitslasten in Azure Search](search-capacity-planning.md)

<!---HONumber=AcomDC_0608_2016-->