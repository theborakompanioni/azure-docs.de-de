<properties 
	pageTitle="Einführung in Data Factory, einen Datenintegrationsdienst | Microsoft Azure" 
	description="Erfahren Sie, was Azure Data Factory ist: Ein cloudbasierter Datenintegrationsdienst, der das Verschieben und Transformieren von Daten orchestriert und automatisiert." 
	keywords="Datenintegration, Clouddatenintegration, Was ist Azure Data Factory?"
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
	ms.topic="get-started-article" 
	ms.date="09/08/2016" 
	ms.author="spelluru"/>

# Einführung in den Azure Data Factory-Dienst (Datenintegrationsdienst in der Cloud)

## Was ist Azure Data Factory? 
Data Factory ist ein cloudbasierter Daten-Integrationsdienst, der das Verschieben und Transformieren von Daten organisiert und automatisiert. Genau wie ein Fertigungsbetrieb, in dem Anlagen Rohmaterialien verarbeiten und in Endprodukte umwandeln, organisiert Data Factory vorhandene Dienste so, dass Rohdaten gesammelt und in nutzbare Informationen transformiert werden.

Data Factory arbeitet mit lokalen und Clouddatenquellen und SaaS, um Daten zu erfassen, vorzubereiten, umzuwandeln, zu analysieren und zu veröffentlichen. Nutzen Sie Data Factory, um Dienste zu verwalteten Datenflusspipelines zusammenzufügen und Ihre Daten mithilfe von Diensten wie [Azure HDInsight (Hadoop)](http://azure.microsoft.com/documentation/services/hdinsight/) und [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) für Ihre Big Data-Rechenanforderungen und [Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) zu transformieren, um Ihre Analyselösungen in Betrieb zu nehmen. Lassen Sie bloße tabellarische Überwachungsansichten hinter sich, und nutzen Sie die umfassenden Visualisierungen von Data Factory, um schnell die Herkunft und Abhängigkeiten zwischen Ihren Datenpipelines anzuzeigen. Überwachen Sie Ihre gesamten Datenflusspipelines über eine zentrale einheitliche Ansicht, um Probleme einfach zu erkennen und Überwachungswarnungen einzurichten.

![Diagramm: Übersicht über Data Factory (Datenintegrationsdienst)](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Abbildung 1** Sammeln Sie Daten vieler lokaler Datenquellen, führen Sie die Erfassung, Vorbereitung, Transformation und Analyse der Daten durch, und veröffentlichen Sie dann Daten, die bereit für die Nutzung sind.

Sie können Data Factory immer dann einsetzen, wenn Sie Daten verschiedener Formen und Größen sammeln, umwandeln und veröffentlichen müssen, um sich weitreichende Erkenntnisse zu verschaffen, um zwar gemäß einem zuverlässigen Zeitplan. Daten-Factory wird zur Erstellung von Datenflusspipelines für hohe Verfügbarkeit für viele Szenarien und in verschiedenen Branchen mit Analysen-Pipeline-Anforderungen verwendet. Online-Einzelhandelsunternehmen verwenden sie zum Generieren personalisierter [Produktempfehlungen](data-factory-product-reco-usecase.md) basierend auf dem Surfverhalten der Kunden. Game Studios verwenden sie, um die[Effektivität ihrer Marketing](data-factory-customer-profiling-usecase.md)-Kampagnen zu überprüfen. Erfahren Sie direkt von unseren Kunden wie und warum sie Data Factory verwenden, indem Sie die [Kundenfallstudien](data-factory-customer-case-studies.md) durchsehen.

> [AZURE.VIDEO azure-data-factory-overview]

## Wichtige Begriffe

Data Factory verfügt über einige wichtige Entitäten, die zusammenarbeiten, um Ein- und Ausgabedaten, Verarbeitungsereignisse sowie den Zeitplan und die Ressourcen zu bestimmen, die für den Ablauf des gewünschten Datenflusses erforderlich sind.

![Diagramm: Data Factory, ein Clouddaten-Integrationsdienst – Grundlegende Konzepte](./media/data-factory-introduction/data-integration-service-key-concepts.png)

**Abbildung 2.** Beziehungen zwischen Dataset, Aktivität, Pipeline und verknüpftem Dienst

### Pipelines
[Pipelines](data-factory-create-pipelines.md) sind eine logische Gruppierung von Aktivitäten. Sie dienen zum Gruppieren von Aktivitäten zu einer Einheit, die zum Ausführen einer Aufgabe verwendet wird. Eine Folge mehrerer Transformationsaktivitäten kann beispielsweise erforderlich sein, um Protokolldateidaten zu bereinigen. Diese Folge kann einen komplexen Zeitplan und Abhängigkeiten aufweisen, die orchestriert und automatisiert werden müssen. Alle Aktivitäten können in einer einzelnen Pipeline mit dem Namen „CleanLogFiles“ gruppiert werden. „CleanLogFiles“ kann dann als einzelne Einheit bereitgestellt, geplant oder gelöscht werden, anstatt alle Aktivitäten einzeln zu verwalten.

### Aktivitäten
Aktivitäten definieren die Aktionen, die Sie auf Ihre Daten anwenden. Jede Aktivität verwendet keine oder mehrere [Datasets](data-factory-create-datasets.md) als Eingaben und erzeugt ein oder mehrere Datasets als Ausgaben. Eine Aktivität ist eine Einheit für die Orchestrierung in Azure Data-Factory. Beispielsweise können Sie eine [Kopieraktivität](data-factory-data-movement-activities.md) verwenden, um das Kopieren von Daten aus einem Dataset in ein anderes zu orchestrieren. Auf ähnliche Weise können Sie eine [Hive-Aktivität](data-factory-data-transformation-activities.md) verwenden, die eine Hive-Abfrage für einen Azure-HDInsight-Cluster ausführt, um Ihre Daten zu transformieren oder zu analysieren. Azure Data Factory bietet eine Vielzahl von Aktivitäten zur Datentransformation, -analyse und -verschiebung.

### Datasets
[Datasets](data-factory-create-datasets.md) sind benannte Verweise/Zeiger auf die Daten, die Sie als Eingabe oder Ausgabe einer Aktivität verwenden möchten. Datasets bestimmen Datenstrukturen in verschiedenen Datenspeichern einschließlich Tabellen, Dateien, Ordnern und Dokumenten.

### Verknüpfter Dienst
Verknüpfte Dienste definieren die Informationen, die für Data Factory für das Verbinden mit externen Ressourcen erforderlich sind. Verknüpfte Dienste haben in Data Factory zwei Zwecke:

- Sie dienen zum Darstellen eines Datenspeichers, z.B. einer lokalen SQL Server-Instanz, Oracle-Datenbank, Dateifreigabe oder eines Azure Blob Storage-Kontos. Wie bereits erläutert, stellen Datasets Datenstrukturen innerhalb der Datenspeicher dar, die mit Data Factory über einen verknüpften Dienst verbunden sind.
- Zum Darstellen einer Computeressource, die die Ausführung einer Aktivität hosten kann. Beispielsweise wird die "HDInsightHive-Aktivität" in einem HDInsight-Hadoop-Cluster ausgeführt.

Mit diesen vier einfachen Konzepten (Datasets, Aktivitäten, Pipelines und verknüpfte Dienste) sind Sie startklar! Sie können [Ihre erste Pipeline erstellen](data-factory-build-your-first-pipeline.md) oder ein vordefiniertes Beispiel bereitstellen, indem Sie die Anweisungen in unseren [Data Factory-Beispielen](data-factory-samples.md) befolgen.

## Unterstützte Regionen
Derzeit können Data Factorys in den Regionen **USA, Westen**, **USA, Osten** und **Europa, Norden** erstellt werden. Eine Data Factory kann jedoch auf Datenspeicher und Compute Services in anderen Azure-Regionen zugreifen, um Daten zwischen Datenspeichern zu verschieben oder Daten mithilfe von Computediensten zu verarbeiten.

In Azure Data Factory selbst werden keine Daten gespeichert. Der Dienst ermöglicht das Erstellen von Datenflüssen, um die Verschiebung von Daten zwischen [unterstützten Datenspeichern](data-factory-data-movement-activities.md#supported-data-stores) und die Verarbeitung von Daten mithilfe von [Compute Services](data-factory-compute-linked-services.md) in anderen Regionen oder in einer lokalen Umgebung zu orchestrieren. Außerdem können Sie mit programmgesteuerten und UI-basierten Mechanismen [Workflows überwachen und verwalten](data-factory-monitor-manage-pipelines.md).

Obwohl Azure Data Factory nur in den Regionen **USA, Westen**, **USA, Osten** und **Europa, Norden** verfügbar ist, steht der Datenverschiebungsdienst in Data Factory [global](data-factory-data-movement-activities.md#global) in verschiedenen Regionen zur Verfügung. Falls sich ein Datenspeicher hinter einer Firewall befindet, werden die Daten stattdessen von einem in der lokalen Umgebung installierten [Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) verschoben.

Nehmen wir beispielsweise an, dass Ihre Computeumgebungen wie Azure HDInsight-Cluster und Azure Machine Learning außerhalb der Region „Europa, Westen“ ausgeführt werden. Sie können eine Azure Data Factory-Instanz in der Region „Europa, Norden“ erstellen und mit ihr Aufträge in Ihren Computeumgebungen in der Region „Europa, Westen“ planen. Es dauert ein paar Millisekunden, bis Data Factory den Auftrag in Ihrer Computeumgebung ausgelöst hat. Die Dauer für die Ausführung des Auftrags in Ihrer Computeumgebung ändert sich aber nicht.

Es ist geplant, Azure Data Factory in Zukunft in allen von Azure unterstützten Gebieten bereitzustellen.
  
## Nächste Schritte
Befolgen Sie die Schritt-für-Schritt-Anleitungen in den folgenden Tutorials, um zu erfahren, wie Sie Data Factorys mit Datenpipelines erstellen.

Lernprogramm: | Beschreibung
-------- | -----------
[Erstellen einer Datenpipeline zur Datenverarbeitung mit Hadoop-Cluster](data-factory-build-your-first-pipeline.md) | In diesem Tutorial erstellen Sie Ihre erste Azure Data Factory mit einer Datenpipeline zur **Datenverarbeitung**, indem ein Hive-Skript in einem Azure HDInsight (Hadoop)-Cluster ausgeführt wird. |
[Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | In diesem Tutorial erstellen Sie eine Data Factory mit einer Pipeline, mit der **Daten aus Blob Storage in SQL-Datenbank verschoben werden**.
[Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) | In diesem Tutorial erstellen Sie eine Data Factory mit einer Pipeline, die Daten aus einer **lokalen** SQL Server-Datenbank in ein Azure-Blob **verschiebt**. Im Rahmen der exemplarischen Vorgehensweise installieren und konfigurieren Sie das Datenverwaltungsgateway auf Ihrem Computer. 

<!---HONumber=AcomDC_0914_2016-->