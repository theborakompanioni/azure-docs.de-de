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
	ms.date="09/22/2016" 
	ms.author="spelluru"/>

# Einführung in den Azure Data Factory-Dienst (Datenintegrationsdienst in der Cloud)

## Was ist Azure Data Factory? 
Data Factory ist ein cloudbasierter Datenintegrationsdienst, der das **Verschieben** und **Transformieren** von Daten organisiert und automatisiert. Mit dem Data Factory-Dienst können Sie Datenintegrationslösungen erstellen, die Daten aus verschiedenen Datenspeichern erfassen, die Daten transformieren/verarbeiten und anschließend die Ergebnisdaten in den Datenspeichern veröffentlichen.

Der Data Factory-Dienst ermöglicht die Erstellung von Datenpipelines zum Verschieben und Transformieren von Daten und die Ausführung dieser Pipelines nach einem festgelegten Zeitplan (stündlich, täglich, wöchentlich usw.). Darüber hinaus stehen umfangreiche Visualisierungen zur Verfügung, um Herkunft und Abhängigkeiten zwischen Ihren Datenpipelines anzuzeigen und alle Ihre Datenpipelines über eine zentrale, einheitliche Ansicht zu überwachen. Dadurch können Sie nicht nur mühelos Probleme lokalisieren, sondern auch Überwachungswarnungen einrichten.

![Diagramm: Übersicht über Data Factory (Datenintegrationsdienst)](./media/data-factory-introduction/what-is-azure-data-factory.png)**Abbildung 1:** Erfassen Sie Daten aus unterschiedlichen Datenquellen, bereiten Sie sie vor, transformieren und analysieren Sie sie, und veröffentlichen Sie nutzbare Daten.

## Pipelines und Aktivitäten
In einer Data Factory-Lösung erstellen Sie mindestens eine **Pipeline**. Bei einer Pipeline handelt es sich um eine logische Gruppierung von Aktivitäten. Sie dienen zum Gruppieren von Aktivitäten zu einer Einheit, die zum Ausführen einer Aufgabe verwendet wird.

**Aktivitäten** definieren die Aktionen, die Sie für Ihre Daten ausführen. Beispielsweise können Sie eine Kopieraktivität verwenden, um Daten zwischen zwei Datenspeichern zu kopieren. Analog dazu können Sie eine Hive-Aktivität verwenden, die eine Hive-Abfrage für einen Azure-HDInsight-Cluster ausführt, um Ihre Daten zu transformieren oder zu analysieren. Data Factory unterstützt zwei Arten von Aktivitäten: Datenverschiebungen und Datentransformationen.
  
## Datenverschiebungsaktivitäten 
[AZURE.INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Ausführlichere Informationen finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).

## Datentransformationsaktivitäten
[AZURE.INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Ausführlichere Informationen finden Sie im Artikel [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md).

Falls Sie Daten in einen/aus einem Datenspeicher verschieben müssen, der von der Kopieraktivität nicht unterstützt wird, oder Daten mit Ihrer eigenen Logik transformieren möchten, erstellen Sie eine **benutzerdefinierte .NET-Aktivität**. Informationen zum Erstellen und Verwenden einer benutzerdefinierten Aktivität finden Sie unter [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](data-factory-use-custom-activities.md).

## Verknüpfte Dienste
Verknüpfte Dienste definieren die Informationen, die Data Factory zum Herstellen einer Verbindung mit externen Ressourcen benötigt. (Beispiele: Azure Storage, lokale SQL Server-Instanz, Azure HDInsight) Verknüpfte Dienste haben in Data Factory zwei Zwecke:

- Sie stellen einen **Datenspeicher** dar (beispielsweise eine lokale SQL Server-Instanz, eine Oracle-Datenbank, eine Dateifreigabe oder ein Azure Blob Storage-Konto). Eine Liste mit unterstützten Datenspeichern finden Sie im Abschnitt [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).
- Sie stellen eine **Computeressource** dar, die die Ausführung einer Aktivität hosten kann. So wird beispielsweise die HDInsightHive-Aktivität in einem HDInsight-Hadoop-Cluster ausgeführt. Eine Liste mit unterstützten Computeumgebungen finden Sie unter [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md).

## Datasets 
Verknüpfte Dienste verknüpfen Datenspeicher mit einer Azure-Data Factory. Datasets stellen Datenstrukturen in den Datenspeichern dar. So stellt etwa ein verknüpfter Azure Storage-Dienst Verbindungsinformationen für Data Factory bereit, um eine Verbindung mit einem Azure Storage-Konto herstellen zu können. Ein Azure-Blob-Dataset gibt den Blobcontainer und -ordner in Azure Blob Storage an, aus dem die Pipeline die Daten lesen soll. Analog dazu stellt ein verknüpfter Azure SQL-Dienst Verbindungsinformationen für eine Azure SQL-Datenbank bereit, und ein Azure SQL-Dataset gibt die Tabelle mit den Daten an.

## Beziehung zwischen Data Factory-Entitäten
Data Factory verfügt über einige wichtige Entitäten, die zusammenarbeiten, um Ein- und Ausgabedaten, Verarbeitungsereignisse sowie den Zeitplan und die Ressourcen zu bestimmen, die für den Ablauf des gewünschten Datenflusses erforderlich sind.

![Diagramm: Data Factory, ein Clouddaten-Integrationsdienst – Grundlegende Konzepte](./media/data-factory-introduction/data-integration-service-key-concepts.png) **Abbildung 2** Beziehungen zwischen Dataset, Aktivität, Pipeline und verknüpftem Dienst

Nachdem Sie sich mit den vier einfachen Konzepten (verknüpfte Dienste, Datasets, Aktivitäten und Pipelines) vertraut gemacht haben, kann es auch schon losgehen: [Erstellen Sie Ihre erste Pipeline](data-factory-build-your-first-pipeline.md)!

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

<!---HONumber=AcomDC_0928_2016-->