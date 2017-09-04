---
title: Verwenden von Verweisdaten und Nachschlagetabellen in Stream Analytics | Microsoft Docs
description: Verwenden von Verweisdaten in einer Stream Analytics-Abfrage
keywords: Nachschlagetabelle, Verweisdaten
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 06103be5-553a-4da1-8a8d-3be9ca2aff54
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: e167552d1dd66b9688f1e71016737cd4ee746123
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>Verwenden von Verweisdaten oder Nachschlagetabellen in einem Stream Analytics-Eingabedatenstrom
Verweisdaten (auch als Nachschlagetabellen bezeichnet) sind ein begrenzter Satz von Daten, der statisch ist oder sich nur langsam ändert und der für eine Suche oder Korrelation mit Ihrem Datenstrom verwendet wird. Für den Einsatz von Verweisdaten in Ihrem Azure Stream Analytics-Auftrag verwenden Sie in der Regel [Verweisdaten für JOIN-Vorgänge](https://msdn.microsoft.com/library/azure/dn949258.aspx) in Ihrer Abfrage. Stream Analytics verwendet Azure Blob Storage als Speicherschicht für Verweisdaten, und mit Azure Data Factory können Verweisdaten aus einer [beliebigen Anzahl von cloudbasierten und lokalen Datenspeichern](../data-factory/data-factory-data-movement-activities.md) transformiert und/oder in Azure Blob Storage kopiert werden, um sie als Verweisdaten zu verwenden. Referenzdaten werden als (in der Eingabekonfiguration definierte) Blobsequenz in aufsteigender Reihenfolge nach dem im Blobnamen angegebenen Datums- bzw. Uhrzeitwert modelliert. Hinzufügungen sind jeweils **nur** am Sequenzende möglich. Hierzu muss der verwendete Datums-/Uhrzeitwert den Wert des letzten Blobs in der Sequenz **übersteigen**.

Für Stream Analytics gilt ein **Grenzwert von 100 MB pro Blob**. Aufträge können jedoch mithilfe der **Pfadmuster**-Eigenschaft mehrere Verweisblobs verarbeiten.


## <a name="configuring-reference-data"></a>Konfigurieren von Verweisdaten
Um die Verweisdaten zu konfigurieren, müssen Sie zunächst eine Eingabe vom Typ **Verweisdaten**erstellen. Die folgende Tabelle enthält den Namen jeder Eigenschaft, die Sie beim Erstellen der Verweisdateneingabe angeben müssen, sowie die entsprechenden Beschreibungen:


<table>
<tbody>
<tr>
<td>Eigenschaftenname</td>
<td>Beschreibung</td>
</tr>
<tr>
<td>Eingabealias</td>
<td>Ein Anzeigename, der in der Auftragsabfrage verwendet wird, um auf diese Eingabe zu verweisen.</td>
</tr>
<tr>
<td>Speicherkonto</td>
<td>Der Name des Speicherkontos, in dem sich Ihre Blobs befinden. Wenn sich das Konto im gleichen Abonnement befindet wie Ihr Stream Analytics-Auftrag, können Sie es in der Dropdownliste auswählen.</td>
</tr>
<tr>
<td>Speicherkontoschlüssel</td>
<td>Der geheime Schlüssel, der dem Speicherkonto zugeordnet ist. Dies wird automatisch aufgefüllt, wenn sich das Speicherkonto im selben Abonnement befindet wie Ihr Stream Analytics-Auftrag.</td>
</tr>
<tr>
<td>Speichercontainer</td>
<td>Container stellen eine logische Gruppierung für Blobs bereit, die im Microsoft Azure-Blobdienst gespeichert sind. Wenn Sie ein Blob in den Blobdienst hochladen, müssen Sie einen Container für das Blob angeben.</td>
</tr>
<tr>
<td>Pfadmuster</td>
<td>Der Pfad, der verwendet wird, um Ihre Blobs innerhalb des angegebenen Containers zu suchen. In dem Pfad können Sie mindestens eine Instanz der folgenden beiden Variablen angeben:<BR>{date}, {time}<BR>Beispiel 1: products/{date}/{time}/product-list.csv<BR>Beispiel 2: products/{date}/product-list.csv
</tr>
<tr>
<td>Datumsformat [optional]</td>
<td>Wenn Sie innerhalb des von Ihnen angegebenen Pfadmusters „{date}“ verwendet haben, können Sie in der Dropdownliste mit den unterstützten Formaten das Datumsformat auswählen, in dem Ihre Blobs gespeichert werden sollen.<BR>Beispiele: YYYY/MM/DD, MM/DD/YYYY...</td>
</tr>
<tr>
<td>Zeitformat [optional]</td>
<td>Wenn Sie innerhalb des von Ihnen angegebenen Pfadmusters „{time}“ verwendet haben, können Sie in der Dropdownliste mit den unterstützten Formaten das Zeitformat auswählen, in dem Ihre Blobs gespeichert werden sollen.<BR>Beispiele: HH, HH/mm, HH-mm</td>
</tr>
<tr>
<td>Ereignisserialisierungsformat</td>
<td>Um sicherzustellen, dass Ihre Abfragen wie erwartet funktionieren, muss Stream Analytics das Serialisierungsformat kennen, das Sie für eingehende Datenströme verwenden. Die unterstützten Formate für Verweisdaten sind CSV und JSON.</td>
</tr>
<tr>
<td>Codieren</td>
<td>Das einzige derzeit unterstützte Codierungsformat ist UTF-8.</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Generieren von Verweisdaten nach einem Zeitplan
Wenn es sich bei Ihren Verweisdaten um ein sich langsam änderndes Dataset handelt, wird die Unterstützung für das Aktualisieren von Verweisdaten aktiviert, indem Sie in der Eingabekonfiguration ein Pfadmuster mit den Ersetzungstoken „{date}“ und „{time}“ angeben. Stream Analytics ruft die aktualisierten Definitionen von Verweisdaten auf der Grundlage dieses Pfadmusters ab. Beispiel: Das Muster `sample/{date}/{time}/products.csv` mit dem Datumsformat **YYYY-MM-DD** und dem Zeitformat **HH-mm** weist Stream Analytics an, das aktualisierte Blob `sample/2015-04-16/17-30/products.csv` am 16. April 2015 um 17:30 (UTC-Zeitzone) abzurufen.

> [!NOTE]
> Stream Analytics-Aufträge suchen derzeit nur dann nach der Blobaktualisierung, wenn die Zeit des Computers die im Blobnamen codierte Zeit erreicht. Der Auftrag sucht beispielsweise am 16. April 2015 ab 17:30 Uhr (UTC-Zeitzone) zum frühestmöglichen Zeitpunkt nach `sample/2015-04-16/17-30/products.csv`. Er sucht *nie* nach einem Blob mit einer codierten Zeit vor der letzten Erkennung.
> 
> Beispiel: Nachdem der Auftrag das Blob `sample/2015-04-16/17-30/products.csv` gefunden hat, werden alle Dateien ignoriert, deren codierte Zeit vor 17:30 Uhr am 16. April 2015 liegt. Wenn im gleichen Container also nachträglich das Blob `sample/2015-04-16/17-25/products.csv` erstellt wird, wird es vom Auftrag nicht verwendet.
> 
> Analog dazu gilt: Wenn `sample/2015-04-16/17-30/products.csv` erst am 16. April 2015 um 22:03 Uhr erstellt wird und im Container kein Blob mit einer früheren Zeit enthalten ist, verwendet der Auftrag diese Datei ab dem 16. April 2015, 22:03 Uhr. Bis dahin werden die vorherigen Verweisdaten verwendet.
> 
> Eine Ausnahme besteht darin, wenn der Auftrag Daten rückwirkend verarbeiten muss oder wenn der Auftrag zum ersten Mal gestartet wird. Zum Startzeitpunkt sucht der Auftrag nach dem aktuellsten Blob, das vor der angegebenen Startzeit des Auftrags erstellt wurde. Damit wird sichergestellt, dass beim Starten des Auftrags ein **nicht leeres** Verweisdataset zur Verfügung steht. Sollte keines gefunden werden, erscheint die folgende Diagnose: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

Mit [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) kann das Erstellen der aktualisierten Blobs orchestriert werden, die von Stream Analytics zur Aktualisierung der Verweisdatendefinitionen benötigt werden. Data Factory ist ein cloudbasierter Daten-Integrationsdienst, der das Verschieben und Transformieren von Daten organisiert und automatisiert. Data Factory unterstützt [die Verbindung zu einer großen Anzahl von cloudbasierten und lokalen Datenspeichern](../data-factory/data-factory-data-movement-activities.md) und das mühelose Verschieben von Daten in regelmäßigen von Ihnen festgelegten Abständen. Weitere Informationen sowie eine schrittweise Anleitung zum Einrichten einer Data Factory-Pipeline zum Generieren von Verweisdaten für Stream Analytics, die nach einem vordefinierten Zeitplan aktualisiert werden, finden Sie in diesem [GitHub-Beispiel](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tipps zum Aktualisieren Ihrer Verweisdaten
1. Das Überschreiben von Verweisdatenblobs hat nicht zur Folge, dass Stream Analytics das Blob erneut lädt, und in einigen Fällen kann es zu einem Auftragsfehler führen. Zum Ändern von Verweisdaten wird empfohlen, ein neues Blob mit demselben in der Auftragseingabe definierten Container und Pfadmuster hinzuzufügen und Datums-/Uhrzeitwerte zu verwenden, die den Wert des letzten Blobs in der Sequenz **übersteigen** .
2. Verweisdatenblobs werden **nicht** nach der Zeit „Zuletzt geändert“ des Blobs sortiert, sondern nur nach den Werten für Uhrzeit und Datum, die im Blobnamen mithilfe der {date}- und {time}-Ersetzungen angegeben werden.
3. In einigen wenigen Fällen muss ein Auftrag zeitlich zurückgehen. Daher dürfen Verweisdatenblobs nicht geändert oder gelöscht werden.

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun Stream Analytics kennengelernt, einen verwalteten Dienst für Stream Analytics für Daten aus dem Internet der Dinge. Weitere Informationen zu diesem Dienst finden Sie unter:

* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

