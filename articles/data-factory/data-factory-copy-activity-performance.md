<properties
	pageTitle="Handbuch zur Leistung und Optimierung der Kopieraktivität | Microsoft Azure"
	description="Hier erfahren Sie, welche Faktoren sich entscheidend auf die Leistung auswirken, wenn Sie Daten in Azure Data Factory mithilfe der Kopieraktivität verschieben."
	services="data-factory"
	documentationCenter=""
	authors="linda33wj"
	manager="jhubbard"
	editor="monicar"/>

<tags
	ms.service="data-factory"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/13/2016"
	ms.author="jingwang"/>


# Handbuch zur Leistung und Optimierung der Kopieraktivität
Die Azure Data Factory-Kopieraktivität bietet eine erstklassige, sichere und zuverlässige Lösung zum Laden von Daten mit hoher Leistung. Sie können Dutzende von Terabytes an Daten täglich zwischen einer großen Vielzahl von Cloud- und lokalen Datenspeichern kopieren. Blitzschnelles Datenladen ist wesentlich, um sicherzustellen, dass Sie sich auf das „Big Data“-Kernproblem konzentrieren können: Erstellen erweiterter Analyselösungen und tiefe Einblicke in all diese Daten.

Azure bietet eine Reihe von Datenspeicher- und Data Warehouse-Lösungen der Unternehmensklasse, und mit der Kopieraktivität führen Sie ein hochgradig optimiertes und benutzerfreundliches Datenladen durch, das Sie mühelos konfigurieren und einrichten können. Mit einer einzelnen Kopieraktivität können Sie Folgendes erreichen:

- Laden von Daten in **Azure SQL Data Warehouse** mit **1,2GB/s**
- Laden von Daten in **Azure-Blobspeicher** mit **1,0GB/s**
- Laden von Daten in **Azure Data Lake Store** mit **1,0GB/s**


Dieser Artikel beschreibt Folgendes:

- [Leistungsreferenznummern](#performance-reference) für unterstützte Quellen- und Senkendatenspeicher, die Ihnen bei der Projektplanung helfen;
- Features, die den Kopierdurchsatz in verschiedenen Szenarien verbessern können, einschließlich [paralleler Kopien](#parallel-copy), [Cloud-Datenverschiebungseinheiten](#cloud-data-movement-units) und [gestaffeltem Kopieren](#staged-copy);
- [Leitfaden zur Leistungsoptimierung](#performance-tuning-steps) zum Optimieren der Leistung und der Schlüsselfaktoren, die die Kopierleistung beeinflussen können.

> [AZURE.NOTE] Falls Sie mit dem grundlegenden Konzept der Kopieraktivität noch nicht vertraut sind, lesen Sie [Verschieben von Daten mit der Kopieraktivität](data-factory-data-movement-activities.md), bevor Sie sich mit diesem Artikel beschäftigen.

## Leistungsreferenz

![Leistungsmatrix](./media/data-factory-copy-activity-performance/CopyPerfRef.png)

Beachten Sie Folgendes:

- Der Durchsatz wird mithilfe der folgenden Formel berechnet: [Größe der aus der Quelle gelesenen Daten]/[Ausführungsdauer der Kopieraktivität]
- Die Leistungsreferenzwerte in der Tabelle wurden mit dem [TPC-H](http://www.tpc.org/tpch/)-Dataset in einer einzelnen Kopieraktivitätsausführung gemessen.
- Wenn Sie Daten zwischen Clouddatenspeichern kopieren, legen Sie **cloudDataMovementUnits** zum Vergleich auf „1“ und „4“ (oder „8“) fest. **parallelCopies** wird nicht angegeben. Ausführliche Informationen zu diesen Features finden Sie unter [Parallele Kopie](#parallel-copy).
- In Azure-Datenspeichern befinden sich Quelle und Senke in der gleichen Azure-Region.
- Bei einer hybriden Datenverschiebung (vom lokalen Speicherort in die Cloud oder umgekehrt) wurde eine einzelne Instanz des Gateways auf einem Computer ausgeführt, bei dem es sich nicht um den lokalen Datenspeicher handelte. Die Konfiguration finden Sie in der nächsten Tabelle. Bei Ausführung einer einzelnen Aktivität auf dem Gateway wurde vom Kopiervorgang nur einer kleiner Teil der CPU, des Arbeitsspeichers und der Netzwerkbandbreite des Testcomputers in Anspruch genommen.
	<table>
	<tr>
		<td>CPU</td>
		<td>Intel Xeon E5-2660 v2 (32 Kerne, 2,20 GHz)</td>
	</tr>
	<tr>
		<td>Arbeitsspeicher</td>
		<td>128 GB</td>
	</tr>
	<tr>
		<td>Netzwerk</td>
		<td>Internetschnittstelle: 10 Gbit/s; Intranetschnittstelle: 40 Gbit/s</td>
	</tr>
	</table>

## Parallele Kopie
Daten können **innerhalb einer Kopieraktivitätsausführung parallel** aus der Quelle gelesen oder am Ziel geschrieben werden. Dieses Feature erhöht den Durchsatz eines Kopiervorgangs und verringert den Zeitaufwand bei der Datenverschiebung.

Diese Einstellung unterscheidet sich von der **concurrency**-Eigenschaft in der Aktivitätsdefinition. Die **concurrency**-Eigenschaft bestimmt die Anzahl **gleichzeitiger Kopieraktivitätsausführungen** für die Verarbeitung von Daten aus unterschiedlichen Aktivitätsfenstern (1:00 bis 2:00 Uhr, 2:00 bis 3:00 Uhr, 3:00 bis 4:00 Uhr usw.). Dies ist beim Laden historischer Daten hilfreich. Die Funktion für parallele Kopien gilt hingegen für eine **einzelne Aktivitätsausführung**.

Ein Beispiel: Angenommen, es müssen mehrere Slices aus der Vergangenheit verarbeitet werden. Data Factory führt für jeden Slice eine Instanz der Kopieraktivität (eine Aktivitätsausführung) aus:

- Datenslice des ersten Aktivitätsfensters (1:00 bis 2:00 Uhr) => Aktivitätsausführung 1
- Datenslice des zweiten Aktivitätsfensters (2:00 bis 3:00 Uhr) => Aktivitätsausführung 2
- Datenslice des dritten Aktivitätsfensters (3:00 bis 4:00 Uhr) => Aktivitätsausführung 3

Und so weiter.

Wenn die Einstellung **concurrency** in diesem Beispiel auf „2“ festgelegt ist, können **Aktivitätsausführung 1** und **Aktivitätsausführung 2** Daten aus zwei Aktivitätsfenstern **gleichzeitig** kopieren, um die Leistung von Datenübertragungen zu verbessern. Sind der Aktivitätsausführung 1 allerdings mehrere Dateien zugeordnet, kopiert der Datenverschiebungsdienst die Dateien einzeln aus der Quelle an das Ziel.

### parallelCopies
Mithilfe der **parallelCopies**-Eigenschaft können Sie die gewünschte Parallelität für die Kopieraktivität angeben. Diese Eigenschaft können Sie sich als die maximale Anzahl von Threads in einer Kopieraktivität vorstellen, die parallel aus Quelldatenspeichern lesen oder in Senkendatenspeicher schreiben können.

Für jede Kopieraktivitätsausführung ermittelt Data Factory die Anzahl paralleler Kopien, die zum Kopieren von Daten aus dem Quelldatenspeicher sowie zum Kopieren von Daten in den Zieldatenspeicher verwendet werden. Die verwendete Standardanzahl paralleler Kopien hängt von der Art der verwendeten Quelle und Senke ab.

Quelle und Senke |	Standardanzahl der vom Dienst ermittelten parallelen Kopien
------------- | -------------------------------------------------
Kopieren von Daten zwischen dateibasierten Speichern (Blobspeicher, Data Lake Store, Amazon S3, lokales Dateisystem, lokales HDFS) | Zwischen 1 und 32. Abhängig von der Größe der Dateien und der Anzahl von Einheiten für Clouddatenverschiebungen (Data Movement Units, DMUs), die zum Kopieren von Daten zwischen zwei Clouddatenspeichern verwendet werden, oder der physischen Konfiguration des Gatewaycomputers, der für Hybridkopien (Kopieren von Daten in einen oder aus einem lokalen Datenspeicher) verwendet wird.
Kopieren von Daten aus einem **beliebigen Quelldatenspeicher in Azure Table Storage** | 4
Alle anderen Paare aus Quelle und Senke | 1

In der Regel sollten Sie mit dem Standardverhalten den besten Durchsatz erzielen. Sie können den Standardwert jedoch überschreiben und einen Wert für die **parallelCopies**-Eigenschaft angeben, um die Auslastung der Computer zu steuern, auf denen Ihre Datenspeicher gehostet werden, oder um die Kopierleistung zu optimieren. Der Wert muss zwischen 1 und 32 (jeweils einschließlich) liegen. Zur Laufzeit verwendet die Kopieraktivität maximal den von Ihnen festgelegten Wert, um eine optimale Leistung zu erzielen.

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

- Wenn Sie Daten zwischen dateibasierten Speichern kopieren, bezieht sich die Parallelität auf die Dateiebene. Innerhalb einer einzelnen Datei erfolgt keine Segmentierung. Die tatsächliche Anzahl paralleler Kopien, die der Datenverschiebungsdienst zur Laufzeit für den Kopiervorgang verwendet, entspricht maximal der Anzahl vorhandener Dateien. Bei Verwendung des Kopierverhaltens **mergeFile** steht der Kopieraktivität keine Parallelität zur Verfügung.
- Berücksichtigen Sie beim Angeben eines Werts für die **parallelCopies**-Eigenschaft die höhere Auslastung Ihrer Quell- und Senkendatenspeicher (und ggf. des Gateways, falls es sich um eine Hybridkopie handelt). Dies gilt insbesondere, wenn Sie über mehrere Aktivitäten oder gleichzeitige Ausführungen der gleichen Aktivitäten verfügen, die für den gleichen Datenspeicher ausgeführt werden. Sollten Sie eine Überlastung des Datenspeichers oder Gateways feststellen, verringern Sie den **parallelCopies**-Wert, um die Last zu reduzieren.
- Beim Kopieren von Daten aus nicht dateibasierten Speichern in dateibasierte Speicher ignoriert der Datenverschiebungsdienst die **parallelCopies**-Eigenschaft. Die Parallelität wird in diesem Fall also nicht angewendet, auch wenn sie angegeben wurde.

> [AZURE.NOTE] Das **parallelCopies**-Feature kann erst ab Version 1.11 des Datenverwaltungsgateways für Hybridkopien verwendet werden.

### Einheiten für Clouddatenverschiebungen
Eine **Einheit für Clouddatenverschiebungen** (Data Movement Unit, DMU) ist eine Messgröße für die Leistungsfähigkeit (Kombination aus zugeteilten CPU-, Speicher- und Netzwerkressourcen) einer einzelnen Einheit in Data Factory. Eine DMU kann bei Cloud-zu-Cloud-Kopien, aber nicht bei Hybridkopien verwendet werden.

Data Factory verwendet standardmäßig eine einzelne Cloud-DMU, um eine einzelne Kopieraktivitätsausführung durchzuführen. Sie können diese Standardeinstellung überschreiben, indem Sie wie im Anschluss beschrieben einen Wert für die **cloudDataMovementUnits**-Eigenschaft angeben. Informationen zum Umfang des möglichen Leistungsgewinns durch Konfigurieren weiterer Einheiten für eine bestimmte Kopierquelle und -senke finden Sie in der [Leistungsreferenz](#performance-reference).

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

Für die **cloudDataMovementUnits**-Eigenschaft **sind folgende Werte zulässig**: 1 (Standard), 2, 4 und 8. Die **tatsächliche Anzahl von Cloud-DMUs**, die der Kopiervorgang zur Laufzeit verwendet, entspricht maximal dem konfigurierten Wert. Dies ist abhängig von Ihrem Datenmuster.

> [AZURE.NOTE] Sollten Sie zur Steigerung des Durchsatzes weitere Cloud-DMUs benötigen, wenden Sie sich an den [Azure-Support](https://azure.microsoft.com/support/). Der Wert „8“ und höher kann derzeit nur verwendet werden, wenn Sie mehrere Dateien mit einer Größe von jeweils mindestens 16 MB aus Blobspeicher in Blobspeicher, Data Lake Store oder eine Azure SQL-Datenbank kopieren.

Informationen zur optimalen Verwendung dieser beiden Eigenschaften sowie zur Erhöhung des Durchsatzes beim Verschieben von Daten finden Sie in den [Beispielen für Anwendungsfälle](#case-study-use-parallel-copy). Das Standardverhalten kann verwendet werden, ohne **parallelCopies** zu konfigurieren. Wenn Sie das Feature konfigurieren und **parallelCopies** zu klein ist, werden mehrere Cloud-DMUs unter Umständen nicht optimal genutzt.

**Wichtig:** Die Berechnung der Gebühren basiert auf der Gesamtdauer des Kopiervorgangs. Wenn ein Kopierauftrag mit einer einzelnen Cloudeinheit bislang eine Stunde gedauert hat und nun mit vier Cloudeinheiten 15 Minuten dauert, fällt die Gesamtrechnung in etwa gleich hoch aus. Ein Beispiel: Angenommen, Sie verwenden vier Cloudeinheiten. Die erste Cloudeinheit benötigt zehn Minuten, die zweite ebenfalls, die dritte fünf Minuten und die vierte ebenfalls fünf Minuten – alles in einer einzelnen Kopieraktivität. Ihnen wird die gesamte Kopierzeit (Datenverschiebungszeit) in Rechnung gestellt. Das sind in diesem Fall 30 Minuten (10 + 10 + 5 + 5). Die Verwendung von **parallelCopies** hat keine Auswirkungen auf die Abrechnung.

## Gestaffeltes Kopieren
Beim Kopieren von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher können Sie ggf. Blob Storage als Stagingzwischenspeicher verwenden. Staging ist besonders in folgenden Fällen hilfreich:

1.	**Sie möchten Daten aus verschiedenen Datenspeichern über PolyBase in SQL Data Warehouse erfassen.** SQL Data Warehouse nutzt PolyBase als Mechanismus mit hohem Durchsatz, um große Datenmengen in SQL Data Warehouse zu laden. Die Quelldaten müssen sich jedoch in Blob Storage befinden und einige andere Kriterien erfüllen. Wenn Sie Daten aus einem Blob Storage-fremden Datenspeicher laden, können Sie das Kopieren von Daten über einen Blog Storage-Stagingzwischenspeicher aktivieren. In diesem Fall führt Data Factory die erforderlichen Datentransformationen durch, um die Anforderungen von PolyBase zu erfüllen. Anschließend werden die Daten mithilfe von PolyBase in SQL Data Warehouse geladen. Weitere Details finden Sie unter [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse).
2.	**Hybriddatenverschiebungen (also das Kopieren zwischen einem lokalen Datenspeicher in einen Clouddatenspeicher) können bei einer langsamen Netzwerkverbindung eine Weile dauern**. Zur Verbesserung der Leistung können Sie die Daten lokal komprimieren und dadurch die Verschiebung in den Stagingdatenspeicher in der Cloud beschleunigen. Anschließend können Sie die Daten im Stagingspeicher wieder dekomprimieren, bevor Sie sie in den Zieldatenspeicher laden.
3.	**Aufgrund der IT-Richtlinien des Unternehmens möchten Sie mit Ausnahme der Ports 80 und 443 keine weiteren Ports in der Firewall öffnen.** Ein Beispiel: Beim Kopieren von Daten aus einem lokalen Datenspeicher an eine Azure SQL-Datenbank- oder Azure SQL Data Warehouse-Senke muss die ausgehende TCP-Kommunikation über den Port 1433 sowohl für die Windows-Firewall als auch für die Unternehmensfirewall ermöglicht werden. In diesem Szenario können Sie mithilfe des Gateways zunächst Daten per HTTP oder HTTPS über den Port 443 an eine Blob Storage-Staginginstanz kopieren. Anschließend können die Daten aus dem Blob Storage-Stagingspeicher in SQL-Datenbank oder in SQL Data Warehouse geladen werden. Dadurch muss der Port 1433 nicht geöffnet werden.

### Funktionsweise des gestaffelten Kopierens
Bei aktiviertem Stagingfeature werden die Daten zunächst aus dem Quelldatenspeicher in den (von Ihnen bereitgestellten) Stagingdatenspeicher kopiert. Anschließend werden die Daten dann aus dem Stagingdatenspeicher in den Senkendatenspeicher kopiert. Dieser zweistufige Prozess wird automatisch von Data Factory verwaltet. Nach Abschluss der Datenverschiebung bereinigt Data Factory außerdem temporäre Daten im Stagingspeicher.

In einem Kopierszenario in der Cloud (wenn sich sowohl der Quell- als auch der Senkendatenspeicher in der Cloud befinden) wird das Gateway nicht verwendet. Der Data Factory-Dienst führt die Kopiervorgänge aus.

![Gestaffeltes Kopieren: Cloudszenario](media/data-factory-copy-activity-performance/staged-copy-cloud-scenario.png)

Im Hybridkopierszenario (mit einer lokalen Quelle und einer Senke in der Cloud) verschiebt das Gateway die Daten aus dem Quelldatenspeicher in einen Stagingdatenspeicher. Die Daten werden vom Data Factory-Dienst aus dem Stagingdatenspeicher in den Senkendatenspeicher verschoben. Das Kopieren von Daten aus einem Clouddatenspeicher in einen lokalen Datenspeicher mithilfe eines Stagingspeichers wird auch in umgekehrter Richtung unterstützt.

![Gestaffeltes Kopieren: Hybridszenario](media/data-factory-copy-activity-performance/staged-copy-hybrid-scenario.png)

Wenn Sie die Datenverschiebung unter Verwendung des Stagingspeichers aktivieren, können Sie angeben, ob die Daten vor dem Verschieben aus dem Quelldatenspeicher in einen Zwischen- oder Stagingdatenspeicher komprimiert und vor dem Verschieben der Daten aus einem Zwischen- oder Stagingdatenspeicher in den Senkendatenspeicher wieder dekomprimiert werden sollen.

Derzeit ist es nicht möglich, Daten unter Verwendung eines Stagingspeichers zwischen zwei lokalen Datenspeichern zu kopieren. Diese Option wird aber voraussichtlich in Kürze verfügbar.

### Konfiguration
Konfigurieren Sie für die Kopieraktivität die Einstellung **enableStaging**, um anzugeben, ob die Daten vor dem Laden in einen Zielspeicher in Blobspeicher bereitgestellt werden sollen. Wenn Sie **enableStaging** auf „TRUE“ festlegen, müssen Sie zusätzliche Eigenschaften angeben. Diese sind in der folgenden Tabelle aufgeführt. Außerdem müssen Sie für das Staging einen verknüpften Azure Storage- oder Storage-SAS-Dienst erstellen, falls noch nicht vorhanden.

Eigenschaft | Beschreibung | Standardwert | Erforderlich
--------- | ----------- | ------------ | --------
**enableStaging** | Geben Sie an, ob Sie Daten über einen Stagingzwischenspeicher kopieren möchten. | False | Nein
**linkedServiceName** | Geben Sie den Namen eines verknüpften Diensts vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) oder [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) an, um auf die Storage-Instanz zu verweisen, die als Stagingzwischenspeicher verwendet werden soll. <br/><br/> Storage kann nicht mit einer Shared Access Signature (SAS) verwendet werden, um Daten über PolyBase in SQL Data Warehouse zu laden. In allen anderen Szenarien ist dies hingegen problemlos möglich. | – | Ja, wenn **enableStaging** auf „TRUE“ festgelegt ist.
**path** | Geben Sie den gewünschten Blob Storage-Pfad für die bereitgestellten Daten an. Wenn Sie keinen Pfad angeben, erstellt der Dienst einen Container zum Speichern der temporären Daten. <br/><br/> Geben Sie nur dann einen Pfad an, wenn Sie Storage mit einer Shared Access Signature verwenden oder sich die temporären Daten an einem bestimmten Speicherort befinden müssen. | – | Nein
**enableCompression** | Gibt an, ob die Daten komprimiert werden sollen, bevor sie an das Ziel kopiert werden. Durch diese Einstellung wird die Menge der übertragenen Daten reduziert. | False | Nein

Hier sehen Sie eine Beispieldefinition der Kopieraktivität mit den Eigenschaften aus der obigen Tabelle:

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
Die Abrechnung basiert auf zwei Aspekten: Dauer des Kopiervorgangs und Art der Kopie.

- Wenn Sie einen Stagingspeicher während eines Cloudkopiervorgangs (Kopieren von Daten aus einem Clouddatenspeicher in einen anderen Clouddatenspeicher) verwenden, gilt folgende Formel: [Gesamtkopierdauer für Schritt 1 und 2] x [Einzelpreis für Cloudkopien].
- Wenn Sie Staging während eines Hybridkopiervorgangs (Kopieren von Daten aus einem lokalen Datenspeicher in einen Clouddatenspeicher) verwenden, gilt folgende Formel: [Dauer des Hybridkopiervorgangs] x [Einzelpreis für Hybridkopien] + [Dauer des Cloudkopiervorgangs] x [Einzelpreis für Cloudkopien].


## Schritte zur Optimierung der Leistung
Wir empfehlen, die folgenden Schritte auszuführen, um die Leistung des Data Factory-Diensts mit der Kopieraktivität zu verbessern:

1.	**Einrichten einer Baseline.** Testen Sie Ihre Pipeline mit der Kopieraktivität in der Entwicklungsphase mit repräsentativen Beispieldaten. Mithilfe des [Slicing-Modells](data-factory-scheduling-and-execution.md#time-series-datasets-and-data-slices) von Data Factory können Sie die verwendete Datenmenge beschränken.

	Erfassen Sie mithilfe der **App „Überwachung und Verwaltung“** die Ausführungszeit und die Leistungsmerkmale. Wählen Sie auf Ihrer Data Factory-Startseite die Option **Überwachen und verwalten** aus. Wählen Sie in der Strukturansicht das **Ausgabedataset** aus. Wählen Sie in der Liste **Activity Windows** (Aktivitätsfenster) die ausgeführte Kopieraktivität aus. Unter **Activity Windows** (Aktivitätsfenster) werden die Dauer der Kopieraktivität und die Größe der kopierten Daten aufgeführt. Der Durchsatz ist im **Activity Window Explorer** (Aktivitätsfenster-Explorer) angegeben. Unter [Überwachen und Verwalten von Azure Data Factory-Pipelines mit der neuen App „Überwachung und Verwaltung“](data-factory-monitor-manage-app.md) erfahren Sie mehr über die App.

	![Aktivitätsausführung – Details](./media/data-factory-copy-activity-performance/mmapp-activity-run-details.png)

	Später in diesem Artikel können Sie die Leistung und Konfiguration Ihres Szenarios mit der [Leistungsreferenz](#performance-reference) aus unseren Tests für die Kopieraktivität vergleichen.
2. **Diagnostizieren und Optimieren der Leistung.** Sollte die Leistung in der Praxis nicht Ihren Erwartungen entsprechen, müssen Sie Leistungsengpässe identifizieren. Anschließend können Sie die Leistung optimieren, um Engpässe zu beseitigen oder deren Auswirkungen zu minimieren. Eine vollständige Beschreibung der Leistungsdiagnose würde den Rahmen dieses Artikels sprengen, im Anschluss finden Sie jedoch einige allgemeine Aspekte:
	- Leistungsfeatures:
		- [Parallele Kopie](#parallel-copy)
		- [Einheiten für Clouddatenverschiebungen](#cloud-data-movement-units)
		- [Gestaffeltes Kopieren](#staged-copy)
	- [Quelle](#considerations-for-the-source)
	- [Senke](#considerations-for-the-sink)
	- [Serialisierung und Deserialisierung](#considerations-for-serialization-and-deserialization)
	- [Komprimierung](#considerations-for-compression)
	- [Spaltenzuordnung](#considerations-for-column-mapping)
	- [Gateway zur Datenverwaltung](#considerations-for-data-management-gateway)
	- [Weitere Überlegungen](#other-considerations)

3. **Erweitern der Konfiguration auf das gesamte Dataset.** Wenn Sie mit den Ausführungsergebnissen und mit der Leistung zufrieden sind, können Sie die Definition und den aktiven Zeitraum der Pipeline auf Ihr gesamtes Dataset erweitern.

## Hinweise zur Quelle
### Allgemein
Stellen Sie sicher, dass der zugrundeliegende Datenspeicher nicht durch andere darauf oder dafür ausgeführte Workloads überlastet wird.

Informationen zu Microsoft-Datenspeichern finden Sie in datenspeicherspezifischen [Themen zur Überwachung und Optimierung](#performance-reference). Hier können Sie sich über Leistungsmerkmale, die Minimierung von Antwortzeiten und die Maximierung des Durchsatzes informieren.

Wenn Sie Daten aus Blob Storage in SQL Data Warehouse kopieren, können Sie die Leistung ggf. mithilfe von **PolyBase** steigern. Details finden Sie unter [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse).


### Dateibasierte Datenspeicher
*(Einschließlich Blobspeicher, Data Lake Store, Amazon S3, lokale Dateisysteme und lokales HDFS)*

- **Durchschnittliche Größe und Anzahl von Dateien:** Die Kopieraktivität überträgt Daten als einzelne Dateien. Aufgrund der Bootstrap-Phase für die einzelnen Dateien ist der Gesamtdurchsatz bei gleicher zu verschiebender Datenmenge geringer, wenn die Daten nicht aus einer geringen Anzahl großer Dateien, sondern aus zahlreichen kleinen Dateien bestehen. Kombinieren Sie kleine Dateien daher möglichst zu größeren Dateien, um einen höheren Durchsatz zu erzielen.
- **Dateiformat und Komprimierung:** Weitere Methoden zur Verbesserung der Leistung finden Sie in den Abschnitten [Hinweise zur Serialisierung/Deserialisierung](#considerations-for-serialization-and-deserialization) und [Hinweise zur Komprimierung](#considerations-for-compression).
- Informationen zum Szenario mit **lokalem Dateisystem**, bei dem das **Datenverwaltungsgateway** benötigt wird, finden Sie im Abschnitt [Hinweise zum Datenverwaltungsgateway](#considerations-for-data-management-gateway).

### Relationale Datenspeicher
*(Einschließlich SQL-Datenbank, SQL Data Warehouse, Amazon Redshift, SQL Server-Datenbanken sowie Oracle-, MySQL-, DB2-, Teradata-, Sybase- und PostgreSQL-Datenbanken etc.)*

- **Datenmuster:** Ihr Tabellenschema hat Auswirkungen auf den Durchsatz beim Kopieren. Eine hohe Zeilengröße liefert zum Kopieren derselben Datenmenge eine bessere Leistung als eine niedrige. Der Grund ist, dass die Datenbank weniger Batches von Daten mit weniger Zeilen effizienter abrufen kann.
- **Abfrage oder gespeicherte Prozedur:** Optimieren Sie die Logik der Abfrage oder gespeicherten Prozedur, die Sie in der Quelle der Kopieraktivität angeben, um Daten effizienter abzurufen.
- Informationen zu **lokalen relationalen Datenbanken** wie SQL Server und Oracle, für die das **Datenverwaltungsgateway** verwendet werden muss, finden Sie im Abschnitt [Hinweise zum Gateway](#considerations-on-data-management-gateway).

## Hinweise zur Senke

### Allgemein
Stellen Sie sicher, dass der zugrundeliegende Datenspeicher nicht durch andere darauf oder dafür ausgeführte Workloads überlastet wird.

Informationen zu Microsoft-Datenspeichern finden Sie in datenspeicherspezifischen [Themen zur Überwachung und Optimierung](#performance-reference). Hier können Sie sich über Leistungsmerkmale, die Minimierung von Antwortzeiten und die Maximierung des Durchsatzes informieren.

Wenn Sie Daten aus **Blob Storage** in **SQL Data Warehouse** kopieren, können Sie die Leistung ggf. mithilfe von **PolyBase** steigern. Details finden Sie unter [Daten unter Verwendung von PolyBase in Azure SQL Data Warehouse laden](data-factory-azure-sql-data-warehouse-connector.md###use-polybase-to-load-data-into-azure-sql-data-warehouse).


### Dateibasierte Datenspeicher
*(Einschließlich Blobspeicher, Data Lake Store, Amazon S3, lokale Dateisysteme und lokales HDFS)*

- **Kopierverhalten:** Wenn Sie Daten aus einem anderen dateibasierten Datenspeicher kopieren, stehen über die **copyBehavior**-Eigenschaft drei Optionen für die Kopieraktivität zur Verfügung: Beibehalten der Hierarchie, Abflachen der Hierarchie und Zusammenführen von Dateien. Das Beibehalten oder Abflachen der Hierarchie verursacht nur einen geringen oder gar keinen Zusatzaufwand. Das Zusammenführen von Dateien ist hingegen mit einem gesteigerten Zusatzaufwand verbunden.
- **Dateiformat und Komprimierung:** Weitere Methoden zur Verbesserung der Leistung finden Sie in den Abschnitten [Hinweise zur Serialisierung/Deserialisierung](#considerations-for-serialization-and-deserialization) und [Hinweise zur Komprimierung](#considerations-for-compression).
- **Blob Storage:** Blob Storage unterstützt derzeit nur Blockblobs für die Optimierung von Datenübertragung und Durchsatz.
- Informationen zu Szenarien mit **lokalem Dateisystem**, in denen das **Datenverwaltungsgateway** verwendet werden muss, finden Sie im Abschnitt [Hinweise zum Datenverwaltungsgateway](#considerations-for-data-management-gateway).

### Relationale Datenspeicher
*(Einschließlich SQL-Datenbank, SQL Data Warehouse, SQL Server- und Oracle-Datenbanken)*

- **Kopierverhalten:** Daten werden von der Kopieraktivität abhängig von den für **sqlSink** konfigurierten Eigenschaften auf unterschiedliche Weise in die Zieldatenbank geschrieben:
	- Standardmäßig verwendet der Datenverschiebungsdienst die API für Massenkopieren, um Daten im Anfügemodus einzufügen. Damit wird die beste Leistung erzielt.
	- Wenn Sie in der Senke eine gespeicherte Prozedur konfigurieren, wendet die Datenbank die Daten nicht mittels Massenladen, sondern zeilenweise an. Dadurch verschlechtert sich die Leistung erheblich. Bei umfangreichen Datasets empfiehlt sich daher die Verwendung der **sqlWriterCleanupScript**-Eigenschaft (sofern möglich).
	- Wenn Sie die **sqlWriterCleanupScript**-Eigenschaft für jede Kopieraktivitätsausführung konfigurieren, löst der Dienst das Skript aus, und Sie fügen die Daten über die API für Massenkopieren ein. Beispiel: Um die gesamte Tabelle mit den neuesten Daten zu überschreiben, können Sie zunächst ein Skript zum Löschen aller Datensätze angeben und dann die neuen Daten durch Massenladen aus der Quelle einfügen.
- **Datenmuster und Batchgröße**:
	- Ihr Tabellenschema hat Auswirkungen auf den Durchsatz beim Kopieren. Beim Kopieren der gleichen Datenmenge erzielen Sie mit großen Zeilen eine bessere Leistung als mit kleinen Zeilen, da die Datenbank eine geringere Anzahl von Datenbatches effizienter committen kann.
	- Die Kopieraktivität fügt Daten in einer Folge von Batches ein. Die Anzahl von Zeilen in einem Batch kann dabei mithilfe der **writeBatchSize**-Eigenschaft festgelegt werden. Wenn Ihre Daten kleine Zeilen enthalten, können Sie für die **writeBatchSize**-Eigenschaft einen höheren Wert festlegen, um den Batchaufwand zu verringern und den Durchsatz zu erhöhen. Bei umfangreichen Zeilen müssen Sie vorsichtig sein, wenn Sie den Wert von **writeBatchSize** erhöhen. Ein hoher Wert kann zu einer Datenbanküberlastung und dadurch zu Kopierfehlern führen.
- Informationen zu **lokalen relationalen Datenbanken** wie SQL Server und Oracle, für die das **Datenverwaltungsgateway** verwendet werden muss, finden Sie im Abschnitt [Hinweise zum Datenverwaltungsgateway](#considerations-for-data-management-gateway).


### NoSQL-Speicher
*(Table Storage und Azure DocumentDB)*

- **Table Storage:**
	- **Partition**: Das Schreiben von Daten in überlappende Partitionen beeinträchtigt die Leistung erheblich. Sortieren Sie Ihre Daten anhand des Partitionsschlüssels, sodass sie effizient partitionsweise eingefügt werden. Sie können die Logik auch so anpassen, dass die Daten in eine einzelne Partition geschrieben werden.
- **DocumentDB:**
	- **Batchgröße:** Die **writeBatchSize**-Eigenschaft legt die Anzahl paralleler Anforderungen fest, die zum Erstellen von Dokumenten an den DocumentDB-Dienst gerichtet werden. Durch Erhöhen des Werts für **writeBatchSize** können Sie die Leistung verbessern, da mehr parallele Anforderungen an DocumentDB gesendet werden. Achten Sie jedoch darauf, dass beim Schreiben in DocumentDB keine Drosselung (Fehlermeldung: „Anforderungsrate ist groß“) auftritt. Eine Drosselung kann verschiedene Ursachen haben. Hierzu zählen etwa die Dokumentgröße, die Anzahl von Begriffen in Dokumenten und die Indizierungsrichtlinie der Zielsammlung. Verwenden Sie ggf. eine bessere Sammlung (beispielsweise S3), um einen höheren Durchsatz zu erzielen.

## Hinweise zur Serialisierung und Deserialisierung
Serialisierung und Deserialisierung können auftreten, wenn Ihr Eingabe- oder Ausgabedataset eine Datei ist. Derzeit unterstützt die Kopieraktivität Avro- und Textdatenformate wie etwa CSV und TSV.

**Kopierverhalten:**

-	Beim Kopieren von Dateien zwischen dateibasierten Datenspeichern gilt Folgendes:
	- Wenn sowohl das Eingabe- als auch das Ausgabedataset die gleichen oder keine Dateiformateinstellungen besitzen, führt der Datenverschiebungsdienst einen binären Kopiervorgang ohne Serialisierung oder Deserialisierung durch. Dadurch wird ein höherer Durchsatz erreicht als in einem Szenario mit unterschiedlichen Dateiformateinstellungen für Quelle und Senke.
	- Wenn Eingabe- und Ausgabedataset im Textformat vorliegen und nur unterschiedlich codiert sind, konvertiert der Datenverschiebungsdienst nur die Codierung. Eine im Gegensatz zum binären Kopieren aufwändigere Serialisierung und Deserialisierung findet nicht statt.
	- Wenn Eingabe- und Ausgabedataset unterschiedliche Dateiformate oder Konfigurationen (etwa Trennzeichen) besitzen, deserialisiert und transformiert der Datenverschiebungsdienst die zu streamenden Quelldaten und serialisiert sie anschließend in das von Ihnen angegebene Ausgabeformat. Dies ist im Vergleich zu anderen Szenarien mit einem wesentlich höheren Aufwand verbunden.
- Wenn Sie Dateien in einen oder aus einem nicht dateibasierten Datenspeicher kopieren (etwa aus einem dateibasierten Speicher in einen relationalen Speicher), ist der Serialisierungs- oder Deserialisierungsschritt zwingend erforderlich. Dieser Schritt bedeutet einen erheblichen Mehraufwand.

**Dateiformat:** Das von Ihnen gewählte Dateiformat beeinträchtigt unter Umständen die Leistung beim Kopieren. Avro ist beispielsweise ein kompaktes binäres Format, das nicht nur Daten, sondern auch Metadaten speichert. Es wird umfassend im Hadoop-System für Verarbeitungs- und Abfragevorgänge unterstützt. Avro verursacht jedoch einen höheren Aufwand bei der Serialisierung und Deserialisierung, was im Vergleich zum Textformat zu einem niedrigeren Durchsatz führt. Berücksichtigen Sie bei der Entscheidung für ein Dateiformat den gesamten Verarbeitungsablauf. Beginnen Sie dabei mit dem Format, in dem die Daten in Quellendatenspeichern gespeichert oder aus externen Systemen extrahiert werden, und überlegen Sie sich, welches Format am besten für die Speicherung, analytische Verarbeitung und Abfrage geeignet ist, und in welchem Format die Daten in Data Marts für die Berichterstellungs- und Visualisierungstools exportiert werden sollen. Manchmal stellt sich heraus, dass ein angesichts der Lese- und Schreibleistung eigentlich suboptimales Dateiformat bei Berücksichtigung des gesamten analytischen Prozesses trotzdem gut geeignet ist.

## Hinweise zur Komprimierung
Wenn Ihr Eingabe- oder Ausgabedataset eine Datei ist, können Sie die Kopieraktivität so konfigurieren, dass beim Schreiben von Daten in das Ziel eine Komprimierung oder Dekomprimierung ausgeführt wird. Wenn Sie sich für eine Komprimierung entscheiden, gehen Sie einen Kompromiss zwischen Eingabe/Ausgabe (E/A) und CPU ein. Das Komprimieren der Daten verursacht zusätzliche Kosten für die Computeressourcen. Im Gegenzug verringern sich aber die Kosten für Netzwerk-E/A und Speicher. Abhängig von Ihren Daten ergibt sich dadurch möglicherweise eine Verbesserung des Gesamtdurchsatzes.

**Codec:** Die Kopieraktivität unterstützt folgende Komprimierungstypen: GZIP, BZIP2 und Deflate. Alle drei Typen können von Azure HDInsight für die Verarbeitung verwendet werden. Jeder Komprimierungscodec hat seine Vorteile. BZIP2 bietet etwa den geringsten Durchsatz, im Gegenzug aber die beste Hive-Abfrageleistung, da sich dieser Typ für die Verarbeitung aufteilen lässt. GZIP ist die ausgewogenste Option und wird am häufigsten verwendet. Entscheiden Sie sich für den Codec, der am besten für Ihr End-to-End-Szenario geeignet ist.

**Ebene:** Für jeden Komprimierungscodec stehen zwei Optionen zur Verfügung: schnellste Komprimierung und optimale Komprimierung. Bei der schnellsten Komprimierung werden die Daten schnellstmöglich komprimiert, auch wenn die resultierende Datei nicht optimal komprimiert ist. Die optimale Komprimierung dauert länger, liefert aber die kleinstmögliche Datenmenge. Sie können beide Optionen testen, um die in Ihrem Fall beste Gesamtleistung zu ermitteln.

**Hinweis:** Verwenden Sie beim Kopieren umfangreicher Daten zwischen einem lokalen Datenspeicher und der Cloud ggf. einen Blobzwischenspeicher mit Komprimierung. Dies ist hilfreich, wenn die Bandbreite Ihres Unternehmensnetzwerks und Ihrer Azure-Dienste einen limitierenden Faktor darstellt und sowohl das Eingabe- als auch das Ausgabedataset in unkomprimierter Form vorliegen sollen. Um genau zu sein, können Sie eine einzelne Kopieraktivität in zwei Kopieraktivitäten aufteilen: Die erste Kopieraktivität kopiert Daten aus der Quelle in komprimierter Form in ein Zwischen- oder Stagingblob. Die zweite Kopieraktivität kopiert die komprimierten Daten aus der Stagingumgebung und dekomprimiert sie beim Schreiben in die Senke.

## Hinweise zur Spaltenzuordnung
Mit der **columnMappings**-Eigenschaft der Kopieraktivität können Eingabespalten ganz oder teilweise den Ausgabespalten zugeordnet werden. Nach dem Lesen der Daten aus der Quelle muss der Datenverschiebungsdienst eine Spaltenzuordnung für die Daten vornehmen, bevor sie in die Senke geschrieben werden. Dieser zusätzliche Verarbeitungsaufwand reduziert den Kopierdurchsatz.

Falls es sich bei Ihrem Quelldatenspeicher um einen abfragbaren Speicher (beispielsweise um einen relationalen Speicher wie SQL-Datenbank oder SQL Server) oder um einen NoSQL-Speicher (etwa um Table Storage oder DocumentDB) handelt, empfiehlt es sich unter Umständen, die Logik für Filterung und Neuanordnung von Spalten in die **query**-Eigenschaft auszulagern, anstatt die Spaltenzuordnung zu verwenden. Dadurch erfolgt die Projektion, während der Datenverschiebungsdienst Daten aus dem Quelldatenspeicher liest, was deutlich effizienter ist.

## Hinweise zum Datenverwaltungsgateway
Empfehlungen für die Gatewayeinrichtung finden Sie unter [Überlegungen zur Verwendung des Datenverwaltungsgateways](data-factory-move-data-between-onprem-and-cloud.md#Considerations-for-using-Data-Management-Gateway).

**Gatewaycomputerumgebung:** Es wird empfohlen, das Datenverwaltungsgateway auf einem dedizierten Computer zu hosten. Verwenden Sie Tools wie den Systemmonitor, um die Auslastung von CPU, Arbeitsspeicher und Bandbreite während eines Kopiervorgangs auf Ihrem Gatewaycomputer zu untersuchen. Verwenden Sie einen leistungsfähigeren Computer, falls CPU, Arbeitsspeicher oder Netzwerkbandbreite nicht mehr ausreichen.

**Gleichzeitige Ausführungen der Kopieraktivität:** In einer einzelnen Instanz des Datenverwaltungsgateways können mehrere Kopieraktivitäten gleichzeitig (also parallel) ausgeführt werden. Die maximale Anzahl gleichzeitig ausführbarer Aufträge wird auf der Grundlage der Hardwarekonfiguration des Gatewaycomputers berechnet. Weitere Kopieraufträge werden der Warteschlange hinzugefügt, bis sie vom Gateway abgerufen werden oder bis für einen anderen Auftrag ein Timeout auftritt. Zur Vermeidung von Ressourcenkonflikten auf dem Gatewaycomputer können Sie den Zeitplan Ihrer Kopieraktivität bereitstellen, um die Anzahl von Kopieraufträgen in der Warteschlange zu verringern, oder die Last auf mehrere Gatewaycomputer verteilen.


## Weitere Überlegungen
Wenn die zu kopierenden Daten sehr umfangreich sind, können Sie Ihre Geschäftslogik so anpassen, dass die Daten mithilfe des Aufteilungsmechanismus von Data Factory weiter partitioniert werden. Planen Sie anschließend eine häufigere Ausführung der Kopieraktivität, um die Datengröße für die einzelnen Kopieraktivitätsausführungen zu verringern.

Seien Sie vorsichtig bei der Anzahl von Datasets und Kopieraktivitäten, für die Data Factory gleichzeitig eine Verbindung mit dem gleichen Datenspeicher herstellen muss. Eine hohe Anzahl gleichzeitiger Kopieraufträge kann zu einer Drosselung eines Datenspeichers sowie zu einer Beeinträchtigung der Leistung, zu internen Wiederholungsversuchen bei Kopieraufträgen und in einigen Fällen zu Fehlern bei der Ausführung führen.

## Fallstudie: Kopieren aus einer lokalen SQL Server-Instanz in Blob Storage
**Szenario:** Es wird eine Pipeline erstellt, um Daten aus einer lokalen SQL Server-Instanz im CSV-Format in Blob Storage zu kopieren. Zur Beschleunigung des Kopierauftrags sollen die CSV-Dateien im BZIP2-Format komprimiert werden.

**Test und Analyse:** Der Durchsatz der Kopieraktivität beträgt weniger als 2 MB/s und liegt damit deutlich unter dem Leistungsbenchmark.

**Leistungsanalyse und -optimierung:** Zur Behebung des Leistungsproblems sehen wir uns zunächst einmal an, wie die Daten verarbeitet und verschoben werden:

1.	**Lesen der Daten:** Das Gateway stellt eine Verbindung mit SQL Server her und sendet die Abfrage. SQL Server sendet daraufhin den Datenstrom über das Intranet an das Gateway.
2.	**Serialisieren und Komprimieren der Daten:** Das Gateway serialisiert den Datenstrom im CSV-Format und komprimiert die Daten zu einem BZIP2-Datenstrom.
3.	**Schreiben der Daten:** Das Gateway lädt den BZIP2-Datenstrom über das Internet in Blob Storage hoch.

Wie Sie sehen, werden die Daten sequenziell mittels Streaming verarbeitet und verschoben: SQL Server -> LAN-> Gateway > WAN -> Blob Storage. **Die Gesamtleistung wird durch den minimalen Durchsatz der gesamten Pipeline begrenzt.**

![Datenfluss](./media/data-factory-copy-activity-performance/case-study-pic-1.png)

Der Leistungsengpass kann auf folgende Faktoren zurückzuführen sein:

-	**Quelle:** SQL Server selbst hat aufgrund einer hohen Auslastung einen geringen Durchsatz.
-	**Datenverwaltungsgateway:**
	-	**LAN:** Das Gateway ist weit vom SQL Server-Computer entfernt, und es wird eine Verbindung mit geringer Bandbreite verwendet.
	-	**Gateway:** Für folgende Vorgänge wurde das Auslastungslimit des Gateways erreicht:
		-	**Serialisierung:** Der Datenstrom lässt sich nur mit geringem Durchsatz in das CSV-Format serialisieren.
		-	**Komprimierung:** Sie haben sich für einen langsamen Komprimierungscodec entschieden – etwa für BZIP2 (2,8 MB/s mit Core i7).
	-	**WAN:** Zwischen dem Unternehmensnetzwerk und Ihren Azure-Diensten steht nur eine geringe Bandbreite zur Verfügung. (Beispiele: T1 = 1,544 KBit/s; T2 = 6,312 KBit/s)
-	**Senke:** Der Durchsatz von Blob Storage ist gering. (Dieses Szenario ist eher unwahrscheinlich, da das entsprechende SLA mindestens 60 MB/s garantiert.)

In diesem Fall verlangsamt unter Umständen die BZIP2-Datenkomprimierung die gesamte Pipeline. Dieser Engpass lässt sich möglicherweise durch einen Wechsel zum GZIP-Komprimierungscodec beheben.


## Fallstudie: Verwenden paralleler Kopien  

**Szenario I:** Kopieren von 1000 Dateien mit jeweils 1 MB aus dem lokalen Dateisystem in Blob Storage

**Analyse und Leistungsoptimierung:** Wenn Sie das Gateway beispielsweise auf einem Computer mit vier Kernen installiert haben, verwendet Data Factory standardmäßig 16 parallele Kopien, um Dateien gleichzeitig aus dem Dateisystem in Blob Storage zu verschieben. Diese parallele Ausführung hat einen hohen Durchsatz zur Folge. Die Anzahl paralleler Kopien kann auch explizit angegeben werden. Beim Kopieren zahlreicher kleiner Dateien tragen parallele Kopien aufgrund der effektiveren Ressourcennutzung zu einer deutlichen Verbesserung des Durchsatzes bei.

![Szenario 1](./media/data-factory-copy-activity-performance/scenario-1.png)

**Szenario II:** Kopieren von 20 Blobs mit jeweils 500 MB aus Blob Storage in Data Lake Store Analytics mit anschließender Leistungsoptimierung

**Analyse und Leistungsoptimierung:** In diesem Szenario kopiert Data Factory die Daten aus Blob Storage in Data Lake Store unter Verwendung einer einzelnen Kopie (**parallelCopies** = 1) und einzelner Einheiten für Clouddatenverschiebungen. Der Durchsatz entspricht in etwa den Angaben im Abschnitt [Leistungsreferenz](#performance-reference).

![Szenario 2:](./media/data-factory-copy-activity-performance/scenario-2.png)

**Szenario III:** Große Einzeldateien und hoher Gesamtumfang

**Analyse und Leistungsoptimierung:** Die Erhöhung des Werts für **parallelCopies** führt angesichts der Ressourcenbeschränkungen einer einzelnen Cloud-DMU nicht zu einer Verbesserung der Leistung. Stattdessen müssen weitere Cloud-DMUs angegeben werden, um weitere Ressourcen zum Ausführen der Datenverschiebung zu erhalten. Geben Sie keinen Wert für die **parallelCopies**-Eigenschaft an. Die Parallelität wird von Data Factory gehandhabt. Wenn Sie **cloudDataMovementUnits** im vorliegenden Fall auf „4“ festlegen, lässt sich der Durchsatz ungefähr vervierfachen.

![Szenario 3](./media/data-factory-copy-activity-performance/scenario-3.png)

## Referenz
Hier finden Sie Referenzen zur Leistungsüberwachung und -optimierung für einige der unterstützten Datenspeicher:

- Azure Storage (einschließlich Blob Storage und Table Storage): [Skalierbarkeits- und Leistungsziele für Azure Storage](../storage/storage-scalability-targets.md) und [Checkliste zu Leistung und Skalierbarkeit von Microsoft Azure Storage](../storage//storage-performance-checklist.md)
- Azure SQL-Datenbank: Sie können [die Leistung überwachen](../sql-database/sql-database-service-tiers.md#monitoring-performance) und den prozentualen Anteil der Datenbanktransaktionseinheit (Database Transaction Unit, DTU) überprüfen.
- Azure SQL Data Warehouse: Die Leistung wird in Data Warehouse-Einheiten (Data Warehouse Units, DWUs) gemessen. Weitere Informationen finden Sie unter [Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht)](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md).
- Azure DocumentDB: [Leistungsebenen in DocumentDB](../documentdb/documentdb-performance-levels.md)
- Lokale SQL Server-Instanz: [Überwachen und Optimieren der Leistung](https://msdn.microsoft.com/library/ms189081.aspx)
- Lokaler Dateiserver: [Performance Tuning for File Servers](https://msdn.microsoft.com/library/dn567661.aspx) (Leistungsoptimierung für Dateiserver)

<!---HONumber=AcomDC_0928_2016-->