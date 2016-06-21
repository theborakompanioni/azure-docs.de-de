<properties
	pageTitle="Handbuch zur Leistung und Optimierung der Kopieraktivität | Microsoft Azure"
	description="Erfahren Sie mehr zu wichtigen Faktoren, die sich auf die Leistung der Datenverschiebung in Azure Data Factory über die Kopieraktivität auswirken."
	services="data-factory"
	documentationCenter=""
	authors="spelluru"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="data-factory"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/03/2016"
	ms.author="spelluru"/>


# Handbuch zur Leistung und Optimierung der Kopieraktivität
Dieser Artikel beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken. Außerdem werden die beobachtete Leistung bei internen Tests aufgeführt und verschiedene Methoden zur Optimierung der Leistung der Kopieraktivität erläutert.

Mit der Kopieraktivität erreichen Sie einen hohen Durchsatz bei Datenverschiebungen, wie in den folgenden Beispielen dargestellt:

- Erfassen von 1 TB Daten in einem Azure-Blobspeicher aus einem lokalen Dateisystem und einem Azure-Blobspeicher in weniger als 3 Stunden (also mit 100 MBit/s)
- Erfassen von 1 TB Daten in einem Azure Data Lake-Speicher aus einem lokalen Dateisystem und einem Azure-Blobspeicher in weniger als 3 Stunden (also mit 100 MBit/s) 
- Erfassen von 1 TB Daten in Azure SQL Data Warehouse aus einem Azure-Blobspeicher in weniger als 3 Stunden (also mit 100 MBit/s) 

In den folgenden Abschnitten erfahren Sie mehr über die Leistung der Kopieraktivität und über Optimierungstipps, um sie weiter zu verbessern.

> [AZURE.NOTE] Wenn Sie mit der Kopieraktivität im Allgemeinen nicht vertraut sind, lesen Sie zuerst [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md), bevor Sie diesen Artikel lesen.

## Schritte zur Optimierung der Leistung
Nachfolgend finden Sie die typischen Schritte, die wir zur Optimierung der Leistung Ihrer Azure Data Factory-Lösung mit Kopieraktivität vorschlagen.

1.	**Einrichten einer Baseline** Testen Sie Ihre Pipeline mit der Kopieraktivität während der Entwicklungsphase mithilfe von repräsentativen Beispieldaten. Mithilfe des [Modell für das Aufteilen in Slices](data-factory-scheduling-and-execution.md#time-series-datasets-and-data-slices) von Azure Data Factory können Sie die Datenmenge beschränken, mit der Sie arbeiten.

	Erfassen Sie mit der **App „Überwachung und Verwaltung“** Merkmale der Ausführungszeit und der Leistung: Klicken Sie auf der Startseite Ihrer Data Factory auf die Kachel **Überwachung und Verwaltung**, wählen Sie das **Ausgabedataset** in der Strukturansicht aus, und wählen Sie dann die auszuführende Kopieraktivität in der Liste **Aktivitätsfenster** aus. Daraufhin sollten die Dauer der Kopieraktivität in der Liste **Aktivitätsfenster** und die Größe der Daten, die kopiert werden, sowie der Durchsatz im Fenster **Aktivitätsfenster-Explorer** auf der rechten Seite angezeigt werden. Unter [Überwachen und Verwalten von Azure Data Factory-Pipelines mit der neuen App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md) erfahren Sie mehr über die App.
	
	![Aktivitätsausführung – Details](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

	Sie können die Leistung und Konfigurationen Ihres Szenarios basierend auf internen Beobachtungen mit der [Leistungsreferenz](#performance-reference) der Kopieraktivität vergleichen.
2. **Leistungsdiagnose und Optimierung** Wenn die beobachtete Leistung unter Ihren Erwartungen liegt, müssen Sie Leistungsengpässe identifizieren und Optimierungen durchführen, um die Auswirkung von Engpässen zu beseitigen oder zu reduzieren. Eine vollständige Beschreibung der Leistungsdiagnose geht über den Rahmen dieses Artikels hinaus, nachfolgend finden Sie jedoch einige allgemeine Überlegungen.
	- [Quelle](#considerations-on-source)
	- [Senke](#considerations-on-sink)
	- [Serialisierung/Deserialisierung](#considerations-on-serializationdeserialization)
	- [Komprimierung](#considerations-on-compression)
	- [Spaltenzuordnung](#considerations-on-column-mapping)
	- [Gateway zur Datenverwaltung](#considerations-on-data-management-gateway)
	- [Weitere Überlegungen](#other-considerations)
	- [Parallele Kopie](#parallel-copy)
	- [Einheiten für Clouddatenverschiebungen](#cloud-data-movement-units)    

3. **Erweitern der Konfiguration auf Ihre gesamten Daten** Sobald Sie mit den Ergebnissen und der Leistung der Ausführung zufrieden sind, können Sie die Datasetdefinition und den aktiven Zeitraum der Pipeline erweitern, um die gesamten Daten in der Abbildung abzudecken.

## Leistungsreferenz
> [AZURE.IMPORTANT] **Haftungsausschluss:** Die folgenden Daten wurden nur für Anleitungen und allgemeine Planung veröffentlicht. Es wird vorausgesetzt, dass Bandbreite, Hardware, Konfiguration usw. zu den besten in der jeweiligen Kategorie zählen. Verwenden Sie diese Daten nur als Referenz. Der beobachtete Durchsatz bei der Datenverschiebung wird von einer Reihe von Variablen beeinflusst. In den Abschnitten weiter unten erfahren Sie, wie Sie optimieren können und eine bessere Leistung für Ihre Datenverschiebungsanforderungen erzielen. Diese Daten werden aktualisiert, wenn leistungssteigernde Verbesserungen und Funktionen hinzugefügt werden.

![Leistungsmatrix](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

Beachten Sie Folgendes:

- Der Durchsatz wird anhand der folgenden Formel berechnet: [Größe der aus der Quelle gelesenen Daten]/[Ausführungsdauer der Kopieraktivität]
- Das Dataset [TPC-H](http://www.tpc.org/tpch/) wurde genutzt, um die oben genannten Zahlen zu berechnen.
- Microsoft Azure-Datenspeichern befinden sich Quelle und Senke in derselben Azure-Region.
- **cloudDataMovementUnits** ist auf 1 festgelegt, und **parallelCopies** wurde nicht angegeben.
- Bei der Hybriddatenverschiebung (lokale Daten in die Cloud oder umgekehrt) wurde das Datenverwaltungsgateway (Einzelinstanz) mit der folgenden Konfiguration auf einem anderen Computer gehostet, als der lokale Datenspeicher. Hinweis: Da auf dem Gateway nur eine Aktivitätsausführung erfolgte, verbrauchte der Kopiervorgang nur einen kleinen Teil der CPU-/Speicherressource und Netzwerkbandbreite dieses Computers.
	<table>
	<tr>
		<td>CPU</td>
		<td>32 Prozessorkerne, 2,20&#160;GHz, Intel Xeon® E5-2660 v2</td>
	</tr>
	<tr>
		<td>Arbeitsspeicher</td>
		<td>128&#160;GB</td>
	</tr>
	<tr>
		<td>Netzwerk</td>
		<td>Internetschnittstelle: 10&#160;Gbit/s; Intranetschnittstelle: 40&#160;Gbit/s</td>
	</tr>
	</table>

## Parallele Kopie
Eine der Methoden, den Durchsatz eines Kopiervorgangs zu verbessern und die Zeit für das Verschieben der Daten zu verkürzen, ist, Daten **parallel in einer Kopieraktivitätsausführung** aus der Quelle zu lesen und/oder Daten in das Ziel zu schreiben.
 
Beachten Sie, dass sich diese Einstellung von der Eigenschaft **Parallelität** in der Aktivitätsdefinition unterscheidet. Die Eigenschaft „Parallelität“ bestimmt die Anzahl der **gleichzeitigen Kopieraktivitätsausführungen**, die zur Laufzeit zusammen ausgeführt werden, um Daten aus unterschiedlichen Aktivitätsfenstern (1:00 bis 2:00 Uhr, 2:00 bis 3:00 Uhr, 3:00 bis 4:00 Uhr usw.) zu verarbeiten. Dies ist beim Laden von Verlaufsdaten sehr nützlich. Dagegen gilt die Funktion für parallele Kopien, die hier erörtert wird, für eine **einzelne Aktivitätsausführung**.

Sehen wir uns ein **Beispielszenario** an: Im folgenden Beispiel müssen mehrere Slices aus der Vergangenheit verarbeitet werden. Der Data Factory-Dienst führt eine Instanz der Kopieraktivität (Aktivitätsausführung) für jeden Slice aus.

- Datenslice des 1. Aktivitätsfensters (1:00 bis 2:00 Uhr) == > Aktivitätsausführung 1
- Datenslice des 2. Aktivitätsfensters (2:00 bis 3:00 Uhr) == > Aktivitätsausführung 2
- Datenslice des 3. Aktivitätsfensters (3:00 bis 4:00 Uhr) == > Aktivitätsausführung 3
- und so weiter sein.

Wenn in diesem Beispiel die Einstellung **Parallelität** auf **2** festgelegt ist, können **Aktivitätsausführung 1** und **Aktivitätsausführung 2** Daten aus zwei Aktivitätsfenstern **gleichzeitig** kopieren, um die Leistung von Datenübertragungen zu verbessern. Sind jedoch mehrere Dateien mit Aktivitätsausführung 1 verknüpft, werden die Dateien nacheinander von der Quelle zum Ziel kopiert.

### parallelCopies
Sie können die **parallelCopies**-Eigenschaft verwenden, um die Parallelität anzugeben, die von der Kopieraktivität verwendet werden soll. Einfach ausgedrückt: Stellen Sie sich diese Eigenschaft als die maximale Anzahl von Threads in einer Kopieraktivität vor, die parallel aus Quelldatenspeichern lesen und/oder in Senkendatenspeicher schreiben können.

Für jede Kopieraktivitätsausführung ermittelt Azure Data Factory auf intelligente Weise die Anzahl paralleler Kopien, die zum Kopieren von Daten aus dem Quelldatenspeicher in den Zieldatenspeicher verwendet werden. Die verwendete Standardanzahl paralleler Kopien hängt von den Typen der Quelle und der Senke ab:

Quelle und Senke |	Standardanzahl der vom Dienst ermittelten parallelen Kopien
------------- | -------------------------------------------------
Kopieren von Daten zwischen **dateibasierten Speichern** (Azure-Blob, Azure Data Lake, lokales Dateisystem, lokales HDFS) | Zwischen **1 und 32**, basierend auf der **Größe der Dateien** und der **Anzahl von Einheiten für Clouddatenverschiebungen** (die Definition finden Sie im nächsten Abschnitt), die zum Kopieren von Daten zwischen zwei Clouddatenspeichern verwendet werden, oder der physischen Konfiguration des Gatewaycomputers, der für Hybridkopien (Kopieren von Daten in einen bzw. aus einem lokalen Datenspeicher) verwendet wird.
Kopieren von Daten aus einem **Quelldatenspeicher in eine Azure-Tabelle** | 4
Alle anderen Paare aus Quelle und Senke | 1

Bei der Mehrheit der Fälle sollten Sie mit dem Standardverhalten den besten Durchsatz erreichen. Sie können den Standardwert jedoch überschreiben, indem Sie einen Wert für die **parallelCopies**-Eigenschaft angeben, um die Auslastung der Computer mit Ihren Datenspeichern zu steuern oder die Kopierleistung zu optimieren. Er muss zwischen **1 und 32 (jeweils einschließlich)** liegen. Zur Laufzeit wählt die Kopieraktivität einen Wert aus, der kleiner oder gleich dem konfigurierten Wert ist, um die beste Leistung zu bieten.

	"activities":[  
	    {
	        "name": "Sample copy activity",
	        "description": "",
	        "type": "Copy",
	        "inputs": [{ "name": "InputDataset" }],
	        "outputs": [{ "name": "OutputDataset" }],
	        "typeProperties": {
	            "source": {
	                "type": "BlobSource",
	            },
	            "sink": {
	                "type": "AzureDataLakeStoreSink"
	            },
	            "parallelCopies": 8
	        }
	    }
	]

Beachten Sie Folgendes:

- Zum Kopieren von Daten zwischen dateibasierten Speichern besteht die Parallelität auf Dateiebene, es erfolgt also keine Segmentierung in einer einzelnen Datei. Die tatsächliche Anzahl von parallelen Kopien, die für den Kopiervorgang zur Laufzeit verwendet wird, liegt nicht über der Anzahl der vorhandenen Dateien. Wenn das Kopierverhalten „mergeFile“ ist, wird Parallelität nicht genutzt.
- Wenn Sie einen Wert für die parallelCopies-Eigenschaft angeben, bedenken Sie die Erhöhung der Last, die dadurch für die Quell- und Senkendatenspeicher sowie das Gateway (bei einer Hybridkopie) entsteht. Dies gilt insbesondere bei mehreren Aktivitäten oder gleichzeitigen Ausführungen der gleichen Aktivitäten für denselben Datenspeicher. Wenn Sie feststellen, dass der Datenspeicher oder das Gateway überlastet ist, verringern Sie den parallelCopies-Wert, um die Last zu reduzieren.
- Beim Kopieren von Daten aus nicht dateibasierten Speichern in dateibasierte Speicher wird die parallelCopies-Eigenschaft ignoriert, auch wenn sie angegeben wurde, und Parallelität wird nicht genutzt.

> [AZURE.NOTE] Zur Nutzung der parallelCopies-Funktion bei einer Hybridkopie müssen Sie das Datenverwaltungsgateway in der Version 1.11 oder höher verwenden.

### Einheiten für Clouddatenverschiebungen
Die **Einheit für Clouddatenverschiebungen** ist eine Messgröße, die die Leistungsfähigkeit (Kombination aus CPU-, Speicher- und Netzwerkressourcenzuweisung) einer einzelnen Einheit im Azure Data Factory-Dienst darstellt, die verwendet wird, um einen Cloud-zu-Cloud-Kopiervorgang auszuführen. Sie spielt bei Hybridkopien keine Rolle. Der Azure Data Factory-Dienst verwendet standardmäßig eine einzelne Einheit für Clouddatenverschiebungen, um eine einzelne Kopieraktivitätsausführung durchzuführen. Sie können diese Standardeinstellung überschreiben, indem Sie einen Wert für die **cloudDataMovementUnits**-Eigenschaft angeben. Zurzeit wird die cloudDataMovementUnits-Einstellung **nur** beim Kopieren von Daten **zwischen zwei Azure-Blobspeichern** oder aus einem **Azure-Blobspeicher in einen Azure Data Lake-Speicher** unterstützt, und sie wird wirksam, wenn mehrere Dateien kopiert werden müssen, die einzeln eine Größe von 16 MB und mehr aufweisen.

Wenn Sie eine Reihe von relativ großen Dateien kopieren, führt das Festlegen eines hohen Werts für die **parallelCopies**-Eigenschaft möglicherweise aufgrund von Ressourcenbeschränkungen der Einheit für Clouddatenverschiebungen nicht zu einer Verbesserung der Leistung. In solchen Fällen sollten Sie mehr Einheiten für Clouddatenverschiebungen verwenden, um große Datenmengen mit hohem Durchsatz zu kopieren. Um die Anzahl der Einheiten für Clouddatenverschiebungen anzugeben, die für die Kopieraktivität verwendet werden soll, legen Sie einen Wert für die **cloudDataMovementUnits**-Eigenschaft fest, wie unten dargestellt:

	"activities":[  
	    {
	        "name": "Sample copy activity",
	        "description": "",
	        "type": "Copy",
	        "inputs": [{ "name": "InputDataset" }],
	        "outputs": [{ "name": "OutputDataset" }],
	        "typeProperties": {
	            "source": {
	                "type": "BlobSource",
	            },
	            "sink": {
	                "type": "AzureDataLakeStoreSink"
	            },
	            "cloudDataMovementUnits": 4
	        }
	    }
	]

Die **zulässigen Werte** für die cloudDataMovementUnits-Eigenschaft sind: 1 (Standard), 2, 4 und 8. Wenn Sie weitere Einheiten für Clouddatenverschiebungen für einen höheren Durchsatz benötigen, wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/). Die **tatsächliche Anzahl von Einheiten für Clouddatenverschiebungen**, die für den Kopiervorgang zur Laufzeit verwendet wird, ist gleich oder kleiner als der konfigurierte Wert. Dies ist davon abhängig, ob die Anzahl der aus der Quelle zu kopierenden Dateien dem Kriterium für die Größe entspricht.

> [AZURE.NOTE] „parallelCopies“ muss größer oder gleich „cloudDataMovementUnits“ sein, sofern angegeben. Wenn „cloudDataMovementUnits“ größer als 1 ist, wird die parallele Datenverschiebung auf die „cloudDataMovementUnits“ für diese Kopieraktivitätsausführung verteilt, und dadurch wird der Durchsatz gesteigert.

Wenn Sie mehrere große Dateien kopieren und **cloudDataMovementUnits** dabei als 2, 4 und 8 konfiguriert ist, kann die Leistung das zwei-, vier- und siebenfache der Referenzwerte aus dem Abschnitt „Leistungsreferenz“ erreichen.

In den [Beispielen für Anwendungsfälle](#case-study---parallel-copy) finden Sie Hinweise zur besseren Nutzung der oben genannten zwei Eigenschaften zur Verbesserung des Durchsatzes bei Datenverschiebungen.
 
**Denken Sie daran**, dass Ihnen die Gebühren basierend auf der Gesamtdauer des Kopiervorgangs berechnet werden. Wenn also ein Kopierauftrag bisher mit einer Cloudeinheit eine Stunde gedauert hat und jetzt mit vier Cloudeinheiten 15 Minuten dauert, ist die Gesamtrechnung etwa gleich hoch. Ein anderes Szenario: Angenommen, Sie verwenden vier Cloudeinheiten für eine Kopieraktivität. Die erste und die zweite Einheit benötigen jeweils 10 Minuten und die dritte und vierte jeweils 5 Minuten. In diesem Fall wird Ihnen die Gesamtdauer des Kopiervorgangs (bzw. der Datenverschiebung) berechnet, also 10 + 10 + 5 + 5 = 30 Minuten. Die Verwendung von **parallelCopies** hat keine Auswirkungen auf die Abrechnung.

## Gestaffeltes Kopieren
Beim Kopieren von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher können Sie einen Azure-Blobspeicher als Stagingzwischenspeicher verwenden. Diese Stagingfunktion ist besonders in folgenden Fällen hilfreich:

1.	**Hybriddatenbewegungen (also die Bewegung aus dem lokalen Datenspeicher in einen Clouddatenspeicher oder umgekehrt) können über eine langsame Netzwerkverbindung eine Weile dauern.** Zur Verbesserung der Leistung solcher Datenbewegungen können Sie Daten lokal komprimieren, damit sie schneller über das Netzwerk an den Stagingdatenspeicher in der Cloud übertragen werden, und die Daten im Stagingspeicher dekomprimieren, bevor sie in den Zieldatenspeicher geladen werden. 
2.	**Aufgrund von IT-Richtlinien sollen in der Firewall mit Ausnahme der Ports 80 und 443 keine weiteren Ports geöffnet werden.** Ein Beispiel: Beim Kopieren von Daten aus einem lokalen Datenspeicher an eine Azure SQL-Datenbanksenke oder eine Azure SQL Data Warehouse-Senke muss ausgehende TCP-Kommunikation über den Port 1433 sowohl für die Windows-Firewall als auch für die Unternehmensfirewall ermöglicht werden. In einem solchen Szenario können Sie unter Verwendung des Datenverwaltungsgateways zunächst über HTTP(S) (also über den Port 443) Daten in einen Azure-Stagingblobspeicher kopieren und die Daten dann von dort aus in die SQL-Datenbank oder in SQL Data Warehouse laden. Der Port 1433 muss hierzu nicht aktiviert werden. 
3.	**Erfassung von Daten aus verschiedenen Datenspeichern in Azure SQL Data Warehouse über PolyBase.** Azure SQL Data Warehouse stellt PolyBase als Mechanismus für hohen Durchsatz bereit, um große Datenmengen in SQL Data Warehouse zu laden. Hierzu müssen sich die Quelldaten allerdings im Azure-Blobspeicher befinden und einige zusätzliche Kriterien erfüllen. Wenn Sie Daten aus einem Datenspeicher laden, bei dem es sich nicht um den Azure-Blobspeicher handelt, haben Sie die Möglichkeit, die Daten über einen Azure-Stagingblobspeicher zu kopieren. In diesem Fall führt Azure Data Factory die erforderlichen Transformationen für die Daten durch, um sicherzustellen, dass sie die Anforderungen von PolyBase erfüllen. Anschließend werden die Daten dann mithilfe von PolyBase in SQL Data Warehouse geladen. Ausführlichere Informationen und Beispiele finden Sie unter [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).

### Funktionsweise des gestaffelten Kopierens
Wenn Sie das Staging-Feature aktivieren, werden die Daten zunächst aus dem Quelldatenspeicher in Ihren eigenen Stagingdatenspeicher und anschließend aus dem Stagingdatenspeicher in den Senkendatenspeicher kopiert. Azure Data Factory übernimmt sowohl die Verwaltung dieses zweistufigen Prozesses als auch die Bereinigung der temporären Daten aus dem Stagingspeicher nach Abschluss der Datenbewegung.

Im **Cloudkopierszenario**, in dem sich sowohl der Quell- als auch der Senkendatenspeicher in der Cloud befindet und das Datenverwaltungsgateway nicht verwendet wird, werden die Kopiervorgänge vom **Azure Data Factory-Dienst** durchgeführt.

![Gestaffeltes Kopieren – Cloudszenario](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Im **Hybridkopierszenario** (lokale Quelle und Senke in der Cloud) wird die Bewegung der Daten aus dem Quelldatenspeicher in den Stagingdatenspeicher vom **Datenverwaltungsgateway** und die Bewegung der Daten aus dem Stagingdatenspeicher in den Senkendatenspeicher vom **Azure Data Factory-Dienst** durchgeführt.

![Gestaffeltes Kopieren – Hybridszenario](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Wenn Sie die Datenverschiebung unter Verwendung des Stagingspeichers aktivieren, können Sie angeben, ob die Daten vor dem Bewegen aus dem Quelldatenspeicher in den Zwischen-/Stagingdatenspeicher komprimiert und vor dem Bewegen der Daten aus dem Zwischen-/Stagingdatenspeicher in den Senkendatenspeicher wieder dekomprimiert werden sollen.

Das Kopieren von Daten aus einem Clouddatenspeicher in einen lokalen Datenspeicher oder zwischen zwei lokalen Datenspeichern mit Stagingspeicher wird derzeit nicht unterstützt, soll aber in Kürze verfügbar werden.

### Konfiguration
Sie können für die Kopieraktivität die Einstellung **enableStaging** konfigurieren, um anzugeben, ob die Daten in einem Azure-Blobspeicher bereitgestellt werden sollen, bevor sie in einen Zieldatenspeicher geladen werden. Wenn Sie „enableStaging“ auf „true“ festlegen, müssen Sie zusätzliche Eigenschaften angeben. Diese werden in der folgenden Tabelle aufgeführt. Außerdem müssen Sie einen mit Azure Storage oder Azure Storage SAS verknüpften Dienst als Stagingdienst erstellen, falls noch keiner vorhanden ist.

Eigenschaft | Beschreibung | Standardwert | Erforderlich
--------- | ----------- | ------------ | --------
enableStaging | Geben Sie an, ob Sie Daten über einen Stagingzwischenspeicher kopieren möchten. | False | Nein
linkedServiceName | Geben Sie den Namen eines verknüpften Diensts vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) oder [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) an, um auf die Azure Storage-Instanz zu verweisen, die als Stagingzwischenspeicher verwendet werden soll. <br/><br/> Beachten Sie, dass Azure Storage-Instanzen mit SAS (Shared Access Signature) nicht verwendet werden können, um Daten über PolyBase in Azure SQL Data Warehouse zu laden. In allen anderen Szenarien können sie problemlos verwendet werden. | – | Ja, wenn „enableStaging“ auf „true“ festgelegt ist. 
path | Geben Sie den Pfad im Azure-Blobspeicher an, der die bereitgestellten Daten enthalten soll. Wenn Sie keinen Pfad angeben, erstellt der Dienst einen Container zum Speichern der temporären Daten. <br/><br/> Der Pfad muss nur angegeben werden, wenn Sie Azure Storage mit SAS verwenden oder genau vorgegeben ist, wo sich die temporären Daten befinden müssen. | N/V | Nein
enableCompression | Geben Sie an, ob die Daten zur Entlastung des Netzwerks komprimiert werden sollen, wenn sie aus dem Quelldatenspeicher an den Senkendatenspeicher bewegt werden. | False | Nein

Hier sehen Sie eine Beispieldefinition für eine Kopieraktivität mit den oben angegebenen Eigenschaften:

	"activities":[  
	{
		"name": "Sample copy activity",
		"type": "Copy",
		"inputs": [{ "name": "OnpremisesSQLServerInput" }],
		"outputs": [{ "name": "AzureSQLDBOutput" }],
		"typeProperties": {
			"source": {
				"type": "SqlSource",
			},
			"sink": {
				"type": "SqlSink"
			},
	    	"enableStaging": true,
			"stagingSettings": {
				"linkedServiceName": "MyStagingBlob",
				"path": "stagingcontainer/path",
				"enableCompression": true
			}
		}
	}
	]

### Auswirkungen auf die Abrechnung
Die Abrechnung erfolgt auf der Grundlage der Dauer der beiden Kopierphasen sowie auf der Grundlage des jeweiligen Kopiertyps:

- Wenn Sie einen Cloudkopiervorgang (Kopieren von Daten aus einem Clouddatenspeicher in einen anderen Clouddatenspeicher – etwa von Azure Data Lake in Azure SQL Data Warehouse) mit Staging verwenden, gilt folgende Formel: [Gesamtkopierdauer für Schritt 1 und 2] x [Einzelpreis für Cloudkopien]
- Wenn Sie einen Hybridkopiervorgang (Kopieren von Daten aus einem lokalen Datenspeicher in einen Clouddatenspeicher – etwa aus einer lokalen SQL Server-Datenbank in Azure SQL Data Warehouse) mit Staging verwenden, gilt folgende Formel: [Dauer des Hybridkopiervorgangs] x [Einzelpreis für Hybridkopien] + [Dauer des Cloudkopiervorgangs] x [Einzelpreis für Cloudkopien]


## Hinweise zur Datenquelle
### Allgemein
Stellen Sie sicher, dass der zugrundeliegende Datenspeicher nicht von anderen darauf oder dafür ausgeführten Workloads überlastet ist, u. a. die Kopieraktivität.

Informationen zu Microsoft-Datenspeichern finden Sie in datenspeicherspezifischen [Themen zur Überwachung und Optimierung](#appendix-data-store-performance-tuning-reference). Diese helfen Ihnen dabei, Leistungsmerkmale, die Minimierung von Antwortzeiten und die Maximierung des Durchsatzes zu verstehen.

Wenn Sie Daten aus einem **Azure-Blobspeicher** in **Azure SQL Data Warehouse** kopieren, könnten Sie **PolyBase** aktivieren, um die Leistung zu verbessern. Details finden Sie unter [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse).


### Dateibasierte Datenspeicher
*(Einschließlich Azure-Blob, Azure Data Lake, lokales Dateisystem)*

- **Durchschnittliche Größe und Anzahl der Dateien**: Kopieraktivität überträgt Daten auf Dateibasis. Mit der gleichen zu verschiebenden Datenmenge ist der Gesamtdurchsatz langsamer, wenn die Daten aufgrund der Bootstrap-Phase für jede Datei aus einer großen Anzahl von kleinen Dateien anstelle einer kleinen Anzahl von größeren Dateien besteht. Kombinieren Sie kleine Dateien daher möglichst zu größeren Dateien, um einen höheren Durchsatz zu erzielen.
- **Dateiformat und Komprimierung**: In den Abschnitten [Hinweise zur Serialisierung/Deserialisierung](#considerations-on-serializationdeserialization) und [Hinweise zur Komprimierung](#considerations-on-compression) finden Sie weitere Methoden zur Verbesserung der Leistung.
- Außerdem finden Sie Informationen zum Szenario mit **lokalem Dateisystem**, bei dem die Verwendung des **Datenverwaltungsgateways** erforderlich ist, im Abschnitt [Hinweise zum Gateway](#considerations-on-data-management-gateway).

### Relationale Datenspeicher
*(Einschließlich Azure SQL-Datenbank, Azure SQL Data Warehouse, SQL Server-Datenbank, Oracle-Datenbank, MySQL-Datenbank, DB2-Datenbank, Teradata-Datenbank, Sybase-Datenbank, PostgreSQL-Datenbank)*

- **Datenmuster**: Tabellenschema wirkt sich auf den Kopierdurchsatz aus. Beim Kopieren derselben Datenmenge erzielen Sie mit großen Zeilen eine bessere Leistung als mit kleinen Zeilen, da die Datenbank weniger Batches von Daten mit weniger Zeilen effizienter abrufen kann.
- **Abfrage oder gespeicherte Prozedur**: Optimieren Sie die Logik der Abfrage oder gespeicherten Prozedur, die Sie in der Quelle der Kopieraktivität angeben, um die Daten effizienter abzurufen.
- Außerdem finden Sie Informationen zum Szenario mit **lokalen relationalen Datenbanken** wie SQL Server und Oracle, bei dem die Verwendung des **Datenverwaltungsgateways** erforderlich ist, im Abschnitt [Hinweise zum Gateway](#considerations-on-data-management-gateway).

## Hinweise zur Senke

### Allgemein
Stellen Sie sicher, dass der zugrundeliegende Datenspeicher nicht von anderen darauf oder dafür ausgeführten Workloads überlastet ist, u. a. die Kopieraktivität.

Informationen zu Microsoft-Datenspeichern finden Sie in datenspeicherspezifischen [Themen zur Überwachung und Optimierung](#appendix-data-store-performance-tuning-reference). Diese helfen Ihnen dabei, Leistungsmerkmale, die Minimierung von Antwortzeiten und die Maximierung des Durchsatzes zu verstehen.

Wenn Sie Daten aus einem **Azure-Blobspeicher** in **Azure SQL Data Warehouse** kopieren, könnten Sie **PolyBase** aktivieren, um die Leistung zu verbessern. Details finden Sie unter [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse).


### Dateibasierte Datenspeicher
*(Einschließlich der Azure-Blob, Azure Data Lake, lokales Dateisystem)*

- **Kopierverhalten**: Wenn Sie Daten aus einem anderen dateibasierten Datenspeicher kopieren, bietet die Kopieraktivität über die Eigenschaft „copyBehavior“ drei Verhaltenstypen: Beibehalten der Hierarchie, Abflachen der Hierarchie und Zusammenführen von Dateien. Das Beibehalten oder Abflachen der Hierarchie verursacht wenig oder keinen Leistungsaufwand, wohingegen das Zusammenführen von Dateien zusätzlichen Leistungsaufwand verursacht.
- **Dateiformat und Komprimierung**: In den Abschnitten [Hinweise zur Serialisierung/Deserialisierung](#considerations-on-serializationdeserialization) und [Hinweise zur Komprimierung](#considerations-on-compression) finden Sie weitere Methoden zur Verbesserung der Leistung.
- Für **Azure-Blob** unterstützen wir derzeit nur Blockblob für die optimierte Datenübertragung und den Durchsatz.
- Außerdem finden Sie Informationen zum Szenario mit **lokalem Dateisystem**, bei dem die Verwendung des **Datenverwaltungsgateways** erforderlich ist, im Abschnitt [Hinweise zum Gateway](#considerations-on-data-management-gateway).

### Relationale Datenspeicher
*(Einschließlich Azure SQL-Datenbank, Azure SQL Data Warehouse, SQL Server-Datenbank)*

- **Kopierverhalten**: Je nach den für „sqlSink“ konfigurierten Eigenschaften, schreibt die Kopieraktivität auf verschiedene Arten Daten in die Zieldatenbank:
	- Standardmäßig verwendet der Datenverschiebungsdienst die API für Massenkopierfunktionen, um Daten im Anfügemodus einzufügen. Damit wird die beste Leistung erzielt.
	- Wenn Sie eine gespeicherte Prozedur in der Senke konfigurieren, wendet die Datenbank die Daten zeilenweise anstatt durch Massenladen an, sodass die Leistung erheblich sinkt. Wenn die Größe der Daten umfangreich ist, sollten Sie ggf. die Verwendung der Eigenschaft „sqlWriterCleanupScript“ (siehe unten) in Betracht ziehen.
	- Wenn Sie für jede Kopieraktivitätsausführung die Eigenschaft „sqlWriterCleanupScript“ konfigurieren, löst der Dienst zuerst das Skript aus und fügt die Daten dann mithilfe der API für Massenkopierfunktionen ein. Beispiel: Zum Überschreiben der gesamten Tabelle mit den aktuellen Daten können Sie zunächst ein Skript zum Löschen aller Datensätze angeben und dann die neuen Daten durch Massenladen aus der Quelle einfügen.
- **Datenmuster und Batchgröße**:
	- Das Tabellenschema wirkt sich auf den Kopierdurchsatz aus. Beim Kopieren derselben Datenmenge erzielen Sie mit großen Zeilen eine bessere Leistung als mit kleinen Zeilen, da die Datenbank für weniger Datenbatches effizienter einen Commit ausführen kann.
	- Die Kopieraktivität fügt Daten in einer Folge von Batches ein, wobei die Anzahl der Zeilen in einem Batch mithilfe der Eigenschaft „writeBatchSize“ festgelegt werden kann. Wenn Ihre Daten Zeilen mit geringer Größe aufweisen, können Sie für die Eigenschaft „writeBatchSize“ einen höheren Wert festlegen, damit weniger Batchaufwand verursacht und der Durchsatz erhöht wird. Wenn die Zeilen Ihrer Daten sehr umfangreich sind, seien Sie beim Erhöhen des Werts von „writeBatchSize“ vorsichtig – ein großer Wert kann zu einem Kopiefehler aufgrund von Datenbanküberlastung führen.
- Außerdem finden Sie Informationen zum Szenario mit **lokalen relationalen Datenbanken** wie SQL Server und Oracle, bei dem die Verwendung des **Datenverwaltungsgateways** erforderlich ist, im Abschnitt [Hinweise zum Gateway](#considerations-on-data-management-gateway).


### NoSQL-Speicher
*(Einschließlich Azure-Tabelle, Azure DocumentDB)*

- Für **Azure-Tabelle**:
	- **Partition**: Das Schreiben von Daten in überlappende Partitionen beeinträchtigt die Leistung erheblich . Sie können Ihre Daten nach Partitionsschlüssel sortieren, damit Daten effizient in aufeinanderfolgende Partitionen eingefügt werden, oder Sie können die Logik so anpassen, dass die Daten in eine einzige Partition geschrieben werden.
- Für **Azure DocumentDB**:
	- **Batchgröße**: Die Eigenschaft „writeBatchSize“ gibt die Anzahl der parallelen Anforderungen an den DocumentDB-Dienst zum Erstellen von Dokumenten an. Sie können eine bessere Leistung erwarten, wenn Sie „writeBatchSize“ heraufsetzen, da mehr parallele Anforderungen an DocumentDB gesendet werden. Beachten Sie jedoch die Einschränkung beim Schreiben in DocumentDB (Fehlermeldung „Anforderungsrate ist groß“). Eine Einschränkung kann aufgrund einer Reihe von Faktoren erfolgen, einschließlich der Größe bzw. Anzahl der Dokumente und der Indizierungsrichtlinie der Zielsammlung. Erwägen Sie eine bessere Sammlung (z. B. S3), um einen höheren Durchsatz zu erzielen.

## Hinweise zur Serialisierung/Deserialisierung
Serialisierung und Deserialisierung können auftreten, wenn Ihr Eingabe- oder Ausgabedataset eine Datei ist. Derzeit unterstützt die Kopieraktivität Avro- und Textdatenformate (z. B. CSV und TSV).

**Kopierverhalten:**

- Beim Kopieren von Dateien zwischen dateibasierten Datenspeichern:
	- Wenn sowohl das Eingabe- als auch das Ausgabedataset dieselben oder keine Dateiformateinstellungen aufweisen, führt der Datenverschiebungsdienst eine binäre Kopie ohne Serialisierung/Deserialisierung durch. Daher erzielen Sie wahrscheinlich einen besseren Durchsatz verglichen mit dem Szenario, bei dem sich die Dateiformateinstellungen für Quelle und Senke unterscheiden.
	- Wenn sowohl das Eingabe- als auch das Ausgabedataset im Textformat vorliegen und sich nur im Codierungstyp unterscheiden, führt der Datenverschiebungsdienst nur eine Konvertierung der Codierung ohne Serialisierung/Deserialisierung durch. Dies führt im Vergleich zur binären Kopie zu einigem Leistungsaufwand.
	- Wenn das Eingabe- und Ausgabedataset unterschiedliche Dateiformate oder unterschiedliche Konfigurationen wie Trennzeichen aufweisen, deserialisiert und transformiert der Datenverschiebungsdienst die zu streamenden Quelldaten und serialisiert sie anschließend in das gewünschte Ausgabeformat. Dies führt im Vergleich zu den vorherigen Szenarien zu wesentlich höherem Leistungsaufwand.
- Beim Kopieren von Dateien in einen bzw. aus einem nicht dateibasierten Datenspeicher (z. B. aus einem dateibasierten Speicher in einen relationalen Speicher) ist eine Serialisierung oder Deserialisierung erforderlich und führt zu einem hohen Leistungsaufwand.

**Dateiformat:** Die Wahl des Dateiformats kann die Leistung beeinträchtigen. Avro ist z. B. ein kompaktes binäres Format, das Metadaten mit Daten speichert und über umfassende Unterstützung im Hadoop-System für die Verarbeitung und Abfragen verfügt. Avro ist jedoch teurer für die Serialisierung/Deserialisierung, was im Vergleich mit dem Textformat zu einem niedrigeren Durchsatz führt. Die Auswahl des mit dem Verarbeitungsablauf zu verwendenden Dateiformats sollte ganzheitlich getroffen werden. Beginnen Sie dabei mit dem Format, in dem die Daten in Quellendatenspeichern gespeichert oder aus externen Systemen extrahiert werden, dem besten Format für die Speicherung, analytische Verarbeitung und Abfragen, und in welchem Format die Daten in Data Marts für die Berichterstellungs- und Visualisierungstools exportiert werden sollen. Manchmal stellt sich heraus, dass ein für die Lese- und Schreibleistung suboptimales Dateiformat sich unter Berücksichtigung des gesamten analytischen Prozesses gut eignet.

## Hinweise zur Komprimierung
Wenn Ihr Eingabe- oder Ausgabedataset eine Datei ist, können Sie die Kopieraktivität so konfigurieren, dass beim Schreiben von Daten in das Ziel eine Komprimierung oder Dekomprimierung ausgeführt wird. Durch Aktivieren der Komprimierung, stellen Sie einen Kompromiss zwischen E/A und CPU her: Das Komprimieren der Daten kostet zusätzliche Computeressourcen, sorgt jedoch im Gegenzug für eine Reduzierung des Netzwerk-E/A und des Speichers; abhängig von Ihren Daten kann dies zu einer Verbesserung des Gesamtdurchsatzes von Kopien führen.

**Codec:** GZIP-, BZIP2- und Deflate-Komprimierung werden unterstützt. Alle drei Typen können von Azure HDInsight für die Verarbeitung verwendet werden. Jeder Komprimierungscodec ist eindeutig. BZIP2 erzielt z. B. hat den niedrigsten Durchsatz, Sie erhalten jedoch die beste Hive-Abfrageleistung, da für die Verarbeitung eine Aufteilung möglich ist; GZIP bietet die ausgewogenste Option und wird am häufigsten verwendet. Sie sollten den Codec auswählen, der für das End-to-End-Szenario am besten geeignet ist.

**Ebene:** Für jeden Komprimierungscodec Ihnen stehen zwei Optionen zur Verfügung – die schnellste Komprimierung und die optimale Komprimierung. Bei der schnellsten Komprimierung werden die Daten schnellstmöglich komprimiert, auch wenn die resultierende Datei nicht optimal komprimiert ist. Die optimale Komprimierung dauert länger, und die minimale Datenmenge wird zurückgegeben. Sie können beide Optionen testen, um die in Ihrem Fall beste Gesamtleistung zu ermitteln.

**Hinweis:** Zum Kopieren von umfangreichen Daten zwischen einem lokalen Datenspeicher und der Cloud, wobei die Bandbreite des Unternehmensnetzwerks und Azure oft den einschränkenden Faktor darstellt, und weder das Eingabe- noch das Ausgabedataset komprimiert werden soll, können Sie die Verwendung eines **Azure-Interims-BLOBS** mit Komprimierung in Betracht ziehen. Genauer gesagt, können Sie eine Kopieraktivität in zwei aufteilen: Die erste Kopieraktivität kopiert in komprimierter Form aus der Quelle in das Interims- oder Staging-BLOB, und die zweite Kopieraktivität kopiert komprimierte Daten aus der Stagingumgebung und dekomprimiert sie beim Schreiben in die Senke.

## Hinweise für die Zuordnung von Spalten
Mit der Eigenschaft „columnMappings“ in der Kopieraktivität können alle oder ein Teil der Eingabespalten den Ausgabespalten zugeordnet werden. Nach dem Lesen der Daten aus der Quelle muss der Datenverschiebungsdienst eine Spaltenzuordnung für die Daten ausführen, bevor sie in die Senke geschrieben werden. Dieser zusätzliche Verarbeitungsaufwand reduziert den Kopierdurchsatz.

Wenn Ihr Quelldatenspeicher abfragbar ist, z. B. ein relationaler Datenspeicher wie Azure SQL/SQL Server oder ein NoSQL-Datenspeicher wie Azure-Tabelle/Azure DocumentDB, können Sie in Betracht ziehen, einen Pushdownvorgang für die Logik zur Filterung/erneuten Anordnung von Spalten an die Abfrageeigenschaft durchzuführen, anstatt die Spaltenzuordnung zu verwenden. Dabei erfolgt die Projektion beim Lesen von Daten aus dem Datenquellenspeicher und ist wesentlich effizienter.

## Überlegungen zum Datenverwaltungsgateway
Empfehlungen für die Gatewayeinrichtung finden Sie unter [Überlegungen zur Verwendung des Datenverwaltungsgateways](data-factory-move-data-between-onprem-and-cloud.md#Considerations-for-using-Data-Management-Gateway).

**Gatewaycomputerumgebung:** Es wird empfohlen, einen dedizierten Computer zum Hosten des Datenverwaltungsgateways zu verwenden. Verwenden Sie Tools wie Systemmonitor, um die Auslastung von CPU, Arbeitsspeicher und Bandbreite während eines Kopiervorgangs auf Ihrem Gatewaycomputer zu überprüfen. Wechseln Sie zu einem leistungsfähigeren Computer, wenn CPU, Arbeitsspeicher oder Netzwerkbandbreite zu einem Engpass werden.

**Gleichzeitige Ausführungen der Kopieraktivität:** In einer einzelnen Instanz des Datenverwaltungsgateways können mehrere Kopieraktivitäten gleichzeitig ausgeführt werden, d. h. ein Gateway kann eine bestimmte Anzahl gleichzeitig (die Anzahl von gleichzeitigen Aufträgen wird basierend auf der Hardwarekonfiguration des Gatewaycomputers berechnet) von Kopieraufträgen ausführen. Zusätzliche Aufträge werden in der Warteschlange platziert, bis sie vom Gateway abgerufen werden oder die Zeitüberschreitung für den Auftrag erreicht ist – je nachdem, was zuerst eintritt. Zum Vermeiden von Ressourcenkonflikten auf dem Gateway können Sie den Zeitplan Ihrer Aktivitäten bereitstellen, um die Menge von gleichzeitigen Kopieraufträgen in der Warteschlange zu reduzieren, oder teilen Sie die Last auf mehrere Gateways auf.


## Weitere Überlegungen
Wenn die zu kopierenden Daten sehr groß sind, können Sie Ihre Geschäftslogik so anpassen, dass die Daten mithilfe des Aufteilungsmechanismus von Azure Data Factory weiter partitioniert werden und eine häufigere Ausführung der Kopieraktivität planen, um die Datengröße für die einzelnen Kopieraktivitäten zu reduzieren.

Seien Sie vorsichtig bei der Anzahl von Datasets und Kopieraktivitäten, die gleichzeitig für denselben Datenspeicher erfolgen. Eine große Anzahl gleichzeitiger Kopieraufträge kann einen Datenspeicher drosseln und zu einer Beeinträchtigung der Leistung, internen Wiederholungsversuchen von Kopieraufträgen und in einigen Fällen zu Fehlern bei der Ausführung führen.

## Fallstudie – Kopieren von einem lokalen SQL Server in Azure Blob
**Szenario:** Eine Pipeline wird erstellt, um Daten von einem lokalen SQL Server im CSV-Format in Azure Blob zu kopieren. Zum Beschleunigen der Kopie ist angegeben, dass die CSV-Dateien im BZIP2-Format komprimiert werden sollen.

**Test und Analyse:** Für die Kopieraktivität wird ein Durchsatz von weniger als 2 MB beobachtet, d. h. viel langsamer als die Leistungsbenchmark.

**Leistungsanalyse und -optimierung:** Zum Beheben des Leistungsproblems sehen wir uns zunächst im Detail an, wie die Daten verarbeitet und verschoben werden:

1.	**Lesen von Daten:** Gateway öffnet Verbindung mit SQL Server und sendet die Abfrage. SQL Server reagiert mit dem Senden des Datenstroms über das Intranet an das Gateway.
2.	Das Gateway **serialisiert** den Datenstrom im CSV-Format und **komprimiert** die Daten in einem BZIP2-Strom.
3.	**Schreiben von Daten:** Das Gateway lädt BZIP2-Streams über das Internet zu Azure Blob.

Wie Sie sehen, werden die Daten sequenziell durch Streaming verarbeitet und verschoben: SQL Server -> LAN-> Gateway > WAN -> Azure-Blob. **Die Gesamtleistung wird durch den minimalen Durchsatz der gesamten Pipeline abgegrenzt**.

![Datenfluss](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Bei einem oder mehreren der folgenden Faktoren kann es sich um den Leistungsengpass handeln:

1.	**Quelle:** SQL Server selbst hat aufgrund hoher Auslastung einen geringen Durchsatz.
2.	**Gateway zur Datenverwaltung**
	1.	**LAN:** Das Gateway befindet sich bei einer Verbindung mit geringer Bandbreite weit vom SQL Server entfernt.
	2.	Die **Belastung des Gatewaycomputers** hat ihre Grenzen für das Ausführen der folgenden Aufgaben erreicht:
		1.	**Serialisierung:** Der Serialisierungsdatenstrom in eine CSV-Datei weist einen langsamen Durchsatz auf.
		2.	**Komprimierung:** Ein langsamer Komprimierungscodec wurde gewählt (z. B. BZIP2 mit 2 8 MBit/s und Core i7).
	3.	**WAN:** Niedrige Bandbreite zwischen dem Unternehmensnetzwerk und Azure (z. B. T1 = 1544 Kbit/s, T2 = 6312 Kbit/s)
4.	**Senke:** Azure-Blob hat geringen Durchsatz (jedoch unwahrscheinlich, da die SLA mindestens 60 MBit/s garantiert).

In diesem Fall könnte die BZIP2-Datenkomprimierung die gesamte Pipeline verlangsamen. Ein Wechsel zum GZIP-Komprimierungscodec kann diesen Engpass beheben.


## Fallstudie: Parallele Kopien  

**Szenario I:** Kopieren von 1000 Dateien mit 1 MB aus einem lokalen Dateisystem in einen Azure-Blobspeicher

**Analyse und Leistungsoptimierung:** Wenn Sie das Datenverwaltungsgateway auf einem Computer mit vier Kernen installiert haben, verwendet Data Factory standardmäßig 16 parallele Kopien, um Dateien gleichzeitig aus dem Dateisystem in den Azure-Blob zu bewegen. Dies sollte zu einem guten Durchsatz führen. Sie können die Anzahl paralleler Kopien auch explizit angeben. Wenn Sie eine große Anzahl von kleinen Dateien kopieren, können Sie mit parallelen Kopien den Durchsatz erheblich verbessern, indem Sie die beteiligten Ressourcen effektiver nutzen.

![Szenario 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Szenario II:** Kopieren von 20 Blobs mit jeweils 500 MB aus dem Azure-Blobspeicher in den Azure Data Lake-Speicher

**Analyse und Leistungsoptimierung:** In diesem Szenario kopiert Data Factory die Daten standardmäßig mit einer einzelnen Kopie (parallelCopies = 1) aus dem Azure-Blob in Azure Data Lake und nutzt eine einzelne Einheit für Clouddatenbewegungen. Der Durchsatz entspricht ungefähr den Angaben im Abschnitt [Leistungsreferenz](#performance-reference) weiter oben.

![Szenario 2:](./media/data-factory-copy-activity-performance/scenario-2.png)

Wenn eine einzelne Datei größer als Dutzende von MB ist und der Gesamtumfang groß ist, führt das Erhöhen von „parallelCopies“ angesichts der Ressourcenbeschränkungen einer Einheit für Clouddatenverschiebungen nicht zu einer besseren Leistung. Stattdessen sollten Sie weitere Einheiten für Clouddatenverschiebungen angeben, um weitere Ressourcen zum Ausführen der Datenverschiebung zu erhalten. Geben Sie keinen Wert für die parallelCopies-Eigenschaft an, sodass Data Factory die Parallelität für Sie verarbeitet. In diesem Fall führt die Angabe von 4 für „cloudDataMovementUnits“ zu einem etwa vierfachen Durchsatz.

![Szenario 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## Referenz zum Optimieren der Datenspeicherleistung
Hier finden Sie einige Referenzen zur Leistungsüberwachung und -optimierung für einige der unterstützten Datenspeicher:

- Azure-Speicher (einschließlich Azure Blob und Azure-Tabelle): [ Skalierbarkeitsziele für Azure-Speicher](../storage/storage-scalability-targets.md) und [Checkliste zu Leistung und Skalierbarkeit für Azure-Speicher](../storage//storage-performance-checklist.md)
- Azure SQL-Datenbank: Sie können [die Leistung überwachen](../sql-database/sql-database-service-tiers.md#monitoring-performance) und den Prozentsatz der Datenbanktransaktionseinheit (Database Transaction Unit, DTU) überprüfen.
- Azure SQL Datawarehouse: Die Funktionalität wird in Data Warehouse-Einheiten (Data Warehouse Units, DWUs) gemessen. Informationen finden Sie unter [Flexible Leistung und Skalierbarkeit mit SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
- Azure DocumentDB: [Leistungsstufe in DocumentDB](../documentdb/documentdb-performance-levels.md).
- Lokale SQL Server: [Überwachen und Optimieren der Leistung](https://msdn.microsoft.com/library/ms189081.aspx)
- Lokaler Dateiserver: [Leistungsoptimierung für Dateiserver](https://msdn.microsoft.com/library/dn567661.aspx)

<!---HONumber=AcomDC_0608_2016-->