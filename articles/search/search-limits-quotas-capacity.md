<properties
	pageTitle="Grenzwerte für den Azure Search-Dienst | Microsoft Azure | Gehosteter Cloudsuchdienst"
	description="Grenzwerte für den Dienst, die bei der Kapazitätsplanung verwendet werden, sowie Höchstwerte für Anforderungen und Antworten für Azure Search."
	services="search"
	documentationCenter=""
	authors="HeidiSteen"
	manager="mblythe"
	editor=""
    tags="azure-portal"/>

<tags
	ms.service="search"
	ms.devlang="NA"
	ms.workload="search"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.date="05/25/2016"
	ms.author="heidist"/>

# Grenzwerte für den Azure Search-Dienst

Die Höchstwerte für Speicher, Workloads und Mengen von Indizes, Dokumenten und anderen Objekten hängen davon ab, ob Sie Azure Search zum **Free**-, **Basic**- oder **Standard**-Tarif hinzufügen.

- **Free** ist ein gemeinsamer mehrinstanzfähiger Dienst, der Teil Ihres Azure-Abonnements ist. Es handelt sich um eine Option für vorhandene Abonnenten ohne zusätzliche Kosten, die es Ihnen ermöglicht, den Dienst zu testen, bevor Sie sich für spezifische Ressourcen anmelden. 
- **Basic (Vorschau)** bietet spezifische Computerressourcen für Produktions-Workloads mit geringerem Umfang. Dieser Tarif befindet sich derzeit in der Vorschau und wird [in der Vorschauphase zu einem um 50 % reduzierten Preis](https://azure.microsoft.com/pricing/details/search/) angeboten.
- **Standard** wird auf dedizierten Computern ausgeführt. Sie bieten höhere Speicher- und Verarbeitungskapazität auf jeder Stufe, einschließlich der Mindestkonfiguration. Standard steht in zwei Stufen (S1 und S2) zur Verfügung. 

Mit Ausnahme des S2-Tarifs, der ein Supportticket erfordert, können alle Tarife [im Portal bereitgestellt werden](search-create-service-portal.md). Bitten wenden Sie sich an den Support oder azuresearch_contact@microsoft.com, um mit S2 zu beginnen.

## Tarifgrenzwerte

[AZURE.INCLUDE [azure-search-limits](../../includes/azure-search-limits-tier.md)]

## Speichergrenzwerte ##

[AZURE.INCLUDE [azure-search-limits](../../includes/azure-search-limits-storage.md)]

## Datengrenzwerte ##

Ressource|Kostenlos|Basic (Vorschau) |S1|S2
---|---|---|---|----
Index: maximale Anzahl von Feldern pro Index|1000|100 <sup>1</sup>|1000|1000
Index: maximale Anzahl von Bewertungsprofilen pro Index|16|16|16|16
Index: maximale Anzahl von Funktionen pro Profil|8|8|8|8
Dokumentgröße pro Index-API<sup>2</sup> in MB|<16|<16|<16|<16
Indexer: maximale Indizierungslast pro Aufruf|10\.000 Dokumente|Nur durch maximale Dokumentanzahl beschränkt|Nur durch maximale Dokumentanzahl beschränkt|Nur durch maximale Dokumentanzahl beschränkt
Indexer: maximale Ausführungszeit|3 Minuten|24 Stunden|24 Stunden|24 Stunden
Blobindexer: maximale Blobgröße, MB|16|16|128|256
Blobindexer: maximale Anzahl der Zeichen des aus einem Blob extrahierten Inhalts|32\.000|64\.000|4 Millionen|4 Millionen

<sup>1</sup> Der Basic-Tarif ist der einzige Tarif mit einem unteren Grenzwert von 100 Feldern pro Index.

<sup>2</sup> Maximale Dokumentgröße beim Aufrufen einer Index-API. Die Dokumentgröße ist tatsächlich eine Begrenzung der Größe des Anforderungstexts der Index-API. Da Sie einen Batch von mehreren Dokumenten auf einmal an die Index-API übergeben können, hängt die maximale Größe davon ab, wie viele Dokumente im Batch vorhanden sind. Für einen Batch mit einem einzelnen Dokument beträgt die maximale Dokumentgröße 16 MB von JSON.

Um die Dokumentgröße niedrig zu halten, achten Sie darauf, nicht abfragbare Daten von der Anforderung auszuschließen. Bilder und andere binäre Daten können nicht direkt abgefragt werden und sollten nicht im Index gespeichert werden. Um nicht abfragbare Daten in Suchergebnisse zu integrieren, definieren Sie ein nicht durchsuchbares Feld, in dem ein URL-Verweis auf die Ressource gespeichert wird.

## Workloadgrenzen (Abfragen pro Sekunde) ##

Ressource|Kostenlos|Basic (Vorschau) |S1|S2
---|---|---|---|----
QPS|–|~3 pro Replikat|~15 pro Replikat|~60 pro Replikat

Abfragen pro Sekunde (Queries Per Second, QPS) ist ein ungefährer Wert, der auf Heuristik basiert, mit simulierten und tatsächlichen Kundenworkloads zum Ableiten der geschätzten Werte. Der genaue QPS-Durchsatz variiert abhängig von Ihren Daten und der Art der Abfrage.

Auch wenn oben grobe Schätzungen angegeben werden, sind die tatsächlichen Abfragen pro Sekunde (QPS) schwierig zu bestimmen. Dies gilt vor allem für den gemeinsamen Dienst im Free-Tarif, bei dem der Durchsatz auf der verfügbaren Bandbreite und der Konkurrenz um Systemressourcen basiert. Die Compute- und Speicherressourcen zur Unterstützung des gemeinsamen Diensts werden von mehreren Abonnenten gemeinsam verwendet, sodass der QPS-Wert für Ihre Lösung stets in Abhängigkeit davon variiert, wie viele andere Workloads gleichzeitig ausgeführt werden.

Auf der Standard-Stufe können Sie den QPS-Wert genauer schätzen, da Sie die Kontrolle über mehr Parameter haben. Im Abschnitt mit bewährten Methoden unter [Verwalten der Suchlösung](search-manage.md) finden Sie Anleitungen zur Berechnung des QPS-Werts für Ihre Workloads.

## API-Anforderungsgrenzwerte

- Maximal 16 MB pro Anforderung <sup>1</sup>
- Maximale URL-Länge von 8 KB
- Maximal 1000 Dokumente pro Batch von Hochlade-, Zusammenführungs- oder Löschvorgängen für Indizes
- Maximal 32 Felder in $orderby-Klausel
- Maximale Suchbegriffgröße ist 32.766 Bytes (32 KB minus 2 Bytes) von UTF-8-codiertem Text

<sup>1</sup> In Azure Search darf der Inhalt einer Anforderung nicht größer als 16 MB sein. Dies beschränkt möglicherweise den Inhalt einzelner Felder oder Sammlungen, für die ansonsten keine theoretischen Beschränkungen gelten. (Weitere Informationen zur Feldzusammensetzung und den Beschränkungen finden Sie unter [Unterstützte Datentypen [Azure Search]](https://msdn.microsoft.com/library/azure/dn798938.aspx)).

## API-Antwortengrenzwerte

- Maximale Rückgabe von 1000 Dokumenten pro Seite mit Suchergebnissen
- Maximale Rückgabe von 100 Vorschlägen pro Anforderung der Vorschlags-API

## API-Schlüsselgrenzwerte

API-Schlüssel werden für die Dienstauthentifizierung verwendet. Es gibt zwei Arten. Administratorschlüssel werden im Anforderungsheader angegeben und gewähren vollständigen Lese-und Schreibzugriff für den Dienst. Abfrageschlüssel sind schreibgeschützt, die in der URL angegeben und normalerweise an Clientanwendungen verteilt werden.

- Maximal 2 Administratorschlüssel pro Dienst
- Maximal 50 Abfrageschlüssel pro Dienst

<!---HONumber=AcomDC_0601_2016-->