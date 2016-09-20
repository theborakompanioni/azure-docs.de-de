<properties
	pageTitle="Grenzwerte für den Azure Search-Dienst | Microsoft Azure"
	description="Grenzwerte für den Dienst, die bei der Kapazitätsplanung verwendet werden, sowie Höchstwerte für Anforderungen und Antworten für Azure Search."
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
	ms.date="08/03/2016"
	ms.author="heidist"/>

# Grenzwerte für den Azure Search-Dienst

Die Höchstwerte für Speicher, Workloads und Mengen von Indizes, Dokumenten und anderen Objekten hängen davon ab, ob Sie Azure Search zum **Free**-, **Basic**- oder **Standard**-Tarif hinzufügen.

- **Free** ist ein gemeinsamer mehrinstanzfähiger Dienst, der Teil Ihres Azure-Abonnements ist. Es handelt sich um eine Option für vorhandene Abonnenten ohne zusätzliche Kosten, die es Ihnen ermöglicht, den Dienst zu testen, bevor Sie sich für spezifische Ressourcen anmelden.
- **Basic** bietet spezifische Computerressourcen für Produktionsworkloads mit geringerem Umfang.
- **Standard** wird auf dedizierten Computern ausgeführt. Sie bieten höhere Speicher- und Verarbeitungskapazität auf jeder Stufe, einschließlich der Mindestkonfiguration. Standard ist in vier Ebenen verfügbar: S1, S2, S3 und S3 High Density (S3 HD). S3 und S3 HD befinden sich derzeit in der Vorschau und werden in der Vorschauphase zu einem um 50% reduzierten Preis angeboten.

Alle Tarife können [im Portal bereitgestellt werden](search-create-service-portal.md). Ein Dienst wird anfangs einer Partition und einem Replikat zugeordnet, aber Sie können die Zuteilung der Ressource ändern, sobald der Dienst erstellt ist. Weitere Informationen siehe [Kapazitätsplanung in Azure Search](search-capacity-planning.md).

## Nach Grenzwerten für Abonnements

[AZURE.INCLUDE [azure-search-limits-per-subscription](../../includes/azure-search-limits-per-subscription.md)]

## Nach Dienstbeschränkungen ##

[AZURE.INCLUDE [azure-search-limits-per-service](../../includes/azure-search-limits-per-service.md)]

## Nach Indexlimits ##

Es gibt eine 1:1-Entsprechung zwischen Grenzwerten für Indizes und Grenzwerten für Indexer. Einen Grenzwert von 200 Indizes pro S2-Dienst vorausgesetzt, beträgt die maximale Zahl von Indexern und Indexerdatenquellen für den gleichen Dienst auch 200.

Ressource|Kostenlos|Basic |S1|S2|S3 (Vorschau)|S3 HD (Vorschau) 
---|---|---|---|---- |---|----
Index: maximale Anzahl von Feldern pro Index|1000|100 <sup>1</sup>|1000|1000|1000|1000 
Index: maximale Anzahl von Bewertungsprofilen pro Index|16|16|16|16|16|16 
Index: maximale Anzahl von Funktionen pro Profil|8|8|8|8|8|8 
Indexer: maximale Indizierungslast pro Aufruf|10\.000 Dokumente|Nur durch maximale Dokumentanzahl beschränkt|Nur durch maximale Dokumentanzahl beschränkt|Nur durch maximale Dokumentanzahl beschränkt|Nur durch maximale Dokumentanzahl beschränkt|– <sup>2</sup> 
Indexer: maximale Ausführungszeit|3 Minuten|24 Stunden|24 Stunden|24 Stunden|24 Stunden|– <sup>2</sup> 
Blobindexer: maximale Blobgröße, MB|16|16|128|256|256|– <sup>2</sup> 
Blobindexer: maximale Anzahl der Zeichen des aus einem Blob extrahierten Inhalts|32\.000|64\.000|4 Millionen|4 Millionen|4 Millionen|– <sup>2</sup> 

<sup>1</sup> Der Basic-Tarif ist die einzige SKU mit einem unteren Grenzwert von 100 Feldern pro Index.

<sup>2</sup> S3 HD unterstützt derzeit weder Indexer noch Indexerdatenquellen. Bitte wenden Sie sich an den Azure-Support, wenn Sie diese Funktion dringend benötigen.

## Dokumentgrößenlimits ##

Ressource|Kostenlos|Basic |S1|S2|S3 (Vorschau)|S3 HD (Vorschau) 
---|---|---|---|---- |---|----
Individuelle Dokumentgröße pro Index-API|<16 MB|<16 MB|<16 MB |<16 MB|<16 MB|<16 MB

Bezieht sich auf die maximale Dokumentgröße beim Aufrufen einer Index-API. Die Dokumentgröße ist tatsächlich eine Begrenzung der Größe des Anforderungstexts der Index-API. Da Sie einen Batch von mehreren Dokumenten auf einmal an die Index-API übergeben können, hängt die tatsächliche maximale Größe davon ab, wie viele Dokumente im Batch vorhanden sind. Für einen Batch mit einem einzelnen Dokument beträgt die maximale Dokumentgröße 16 MB von JSON.

Um die Dokumentgröße niedrig zu halten, achten Sie darauf, nicht abfragbare Daten von der Anforderung auszuschließen. Bilder und andere binäre Daten können nicht direkt abgefragt werden und sollten nicht im Index gespeichert werden. Um nicht abfragbare Daten in Suchergebnisse zu integrieren, definieren Sie ein nicht durchsuchbares Feld, in dem ein URL-Verweis auf die Ressource gespeichert wird.

## Workloadgrenzen (Abfragen pro Sekunde) ##

Ressource|Kostenlos|Basic|S1|S2|S3 (Vorschau)|S3 HD (Vorschau)
---|---|---|---|----|---|----
QPS|N/V|~3 pro Replikat|~15 pro Replikat|~60 pro Replikat|>60 pro Replikat|>60 pro Replikat

Abfragen pro Sekunde (Queries Per Second, QPS) ist ein ungefährer Wert, der auf Heuristik basiert, mit simulierten und tatsächlichen Kundenworkloads zum Ableiten der geschätzten Werte. Der genaue QPS-Durchsatz variiert abhängig von Ihren Daten und der Art der Abfrage.

Auch wenn oben grobe Schätzungen angegeben werden, ist die tatsächliche Rate schwierig zu bestimmen. Dies gilt vor allem für den gemeinsamen Dienst im Free-Tarif, bei dem der Durchsatz auf der verfügbaren Bandbreite und der Konkurrenz um Systemressourcen basiert. Im Free-Tarif werden die Compute- und Speicherressourcen von mehreren Abonnenten gemeinsam verwendet, sodass der QPS-Wert für Ihre Lösung stets in Abhängigkeit davon variiert, wie viele andere Workloads gleichzeitig ausgeführt werden.

Auf der Standard-Stufe können Sie den QPS-Wert genauer schätzen, da Sie die Kontrolle über mehr Parameter haben. Im Abschnitt mit bewährten Methoden unter [Verwalten Ihres Suchdiensts in Microsoft Azure](search-manage.md) finden Sie Anleitungen zur Berechnung des QPS-Werts für Ihre Workloads.

## API-Anforderungsgrenzwerte

- Maximal 16 MB pro Anforderung <sup>1</sup>
- Maximale URL-Länge von 8 KB
- Maximal 1000 Dokumente pro Batch von Hochlade-, Zusammenführungs- oder Löschvorgängen für Indizes
- Maximal 32 Felder in $orderby-Klausel
- Maximale Suchbegriffgröße ist 32.766 Bytes (32 KB minus 2 Bytes) von UTF-8-codiertem Text

<sup>1</sup> In Azure Search darf der Inhalt einer Anforderung nicht größer als 16 MB sein. Dies beschränkt möglicherweise den Inhalt einzelner Felder oder Sammlungen, für die ansonsten keine theoretischen Beschränkungen gelten. (Weitere Informationen zur Feldzusammensetzung und den Beschränkungen finden Sie unter [Supported data types](https://msdn.microsoft.com/library/azure/dn798938.aspx) [Unterstützte Datentypen]).

## API-Antwortengrenzwerte

- Maximale Rückgabe von 1000 Dokumenten pro Seite mit Suchergebnissen
- Maximale Rückgabe von 100 Vorschlägen pro Anforderung der Vorschlags-API

## API-Schlüsselgrenzwerte

API-Schlüssel werden für die Dienstauthentifizierung verwendet. Es gibt zwei Arten. Administratorschlüssel werden im Anforderungsheader angegeben und gewähren vollständigen Lese-und Schreibzugriff für den Dienst. Abfrageschlüssel sind schreibgeschützt, die in der URL angegeben und normalerweise an Clientanwendungen verteilt werden.

- Maximal 2 Administratorschlüssel pro Dienst
- Maximal 50 Abfrageschlüssel pro Dienst

<!---HONumber=AcomDC_0907_2016-->