---
title: "Einführung in Data Factory, einen Datenintegrationsdienst | Microsoft Docs"
description: 'Erfahren Sie, was Azure Data Factory ist: Ein cloudbasierter Datenintegrationsdienst, der das Verschieben und Transformieren von Daten orchestriert und automatisiert.'
keywords: Datenintegration, Clouddatenintegration, Was ist Azure Data Factory?
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: cec68cb5-ca0d-473b-8ae8-35de949a009e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/25/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: 757e91c3c702a62fb143f45a3144069400c5a757
ms.openlocfilehash: e2c2611d6f1094d2c1ba2bdb2398b0b17723af8a


---
# <a name="introduction-to-azure-data-factory-service-a-data-integration-service-in-the-cloud"></a>Einführung in den Azure Data Factory-Dienst (Datenintegrationsdienst in der Cloud)
## <a name="what-is-azure-data-factory"></a>Was ist Azure Data Factory?
Data Factory ist ein cloudbasierter Daten-Integrationsdienst, der das **Verschieben** und **Transformieren** von Daten organisiert und automatisiert. Mit dem Data Factory-Dienst können Sie Datenintegrationslösungen erstellen, die Daten aus verschiedenen Datenspeichern erfassen, die Daten transformieren/verarbeiten und anschließend Ergebnisdaten in den Datenspeichern veröffentlichen.

Der Data Factory-Dienst ermöglicht die Erstellung von Datenpipelines zum Verschieben und Transformieren von Daten und die Ausführung dieser Pipelines nach einem festgelegten Zeitplan (stündlich, täglich, wöchentlich usw.). Darüber hinaus stehen umfangreiche Visualisierungen zur Verfügung, um Herkunft und Abhängigkeiten zwischen Ihren Datenpipelines anzuzeigen und die Pipelines über eine zentrale, einheitliche Ansicht zu überwachen. Dadurch können Sie nicht nur mühelos Probleme lokalisieren, sondern auch Überwachungswarnungen einrichten.

![Diagramm: Übersicht über Data Factory (Datenintegrationsdienst)](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Abbildung&1;:** Erfassen Sie Daten aus unterschiedlichen Datenquellen, bereiten Sie sie vor, transformieren und analysieren Sie sie, und veröffentlichen Sie nutzbare Daten.

## <a name="pipelines-and-activities"></a>Pipelines und Aktivitäten
In einer Data Factory-Lösung erstellen Sie mindestens eine **Pipeline**. Bei einer Pipeline handelt es sich um eine logische Gruppierung von Aktivitäten. Sie dienen zum Gruppieren von Aktivitäten zu einer Einheit, die zum Ausführen einer Aufgabe verwendet wird.

**Aktivitäten** definieren die Aktionen, die Sie für Ihre Daten ausführen. Beispielsweise können Sie eine Kopieraktivität verwenden, um Daten zwischen zwei Datenspeichern zu kopieren. Analog dazu können Sie eine Hive-Aktivität verwenden, die eine Hive-Abfrage für einen Azure-HDInsight-Cluster ausführt, um Ihre Daten zu transformieren oder zu analysieren. Data Factory unterstützt zwei Arten von Aktivitäten: Datenverschiebungen und Datentransformationen.

## <a name="data-movement-activities"></a>Datenverschiebungsaktivitäten
[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Weitere Informationen finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).

## <a name="data-transformation-activities"></a>Datentransformationsaktivitäten
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Weitere Informationen finden Sie im Artikel [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md).

Falls Sie Daten in einen/aus einem Datenspeicher verschieben müssen, der von der Kopieraktivität nicht unterstützt wird, oder Daten mit Ihrer eigenen Logik transformieren möchten, erstellen Sie eine **benutzerdefinierte .NET-Aktivität**. Informationen zum Erstellen und Verwenden einer benutzerdefinierten Aktivität finden Sie unter [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](data-factory-use-custom-activities.md).

## <a name="linked-services"></a>Verknüpfte Dienste
Verknüpfte Dienste definieren die Informationen, die Data Factory zum Herstellen einer Verbindung mit externen Ressourcen benötigt. (Beispiele: Azure Storage, lokale SQL Server-Instanz, Azure HDInsight) Verknüpfte Dienste haben in Data Factory zwei Zwecke:

* Sie stellen einen **Datenspeicher** dar (beispielsweise eine lokale SQL Server-Instanz, eine Oracle-Datenbank, eine Dateifreigabe oder ein Azure Blob Storage-Konto). Eine Liste mit unterstützten Datenspeichern finden Sie im Abschnitt [Datenverschiebungsaktivitäten](#data-movement-activities) .
* Sie stellen eine **Computeressource** dar, die die Ausführung einer Aktivität hosten kann. So wird beispielsweise die HDInsightHive-Aktivität in einem HDInsight-Hadoop-Cluster ausgeführt. Eine Liste mit unterstützten Computeumgebungen finden Sie unter [Datentransformationsaktivitäten](#data-transformation-activities).

## <a name="datasets"></a>DATASETS
Verknüpfte Dienste verknüpfen Datenspeicher mit einer Azure-Data Factory. Datasets stellen Datenstrukturen in den Datenspeichern dar. So stellt etwa ein verknüpfter Azure Storage-Dienst Verbindungsinformationen für Data Factory bereit, um eine Verbindung mit einem Azure Storage-Konto herstellen zu können. Ein Azure-Blob-Dataset gibt den Blobcontainer und -ordner in Azure Blob Storage an, aus dem die Pipeline die Daten lesen soll. Analog dazu stellt ein verknüpfter Azure SQL-Dienst Verbindungsinformationen für eine Azure SQL-Datenbank bereit, und ein Azure SQL-Dataset gibt die Tabelle mit den Daten an.   

## <a name="relationship-between-data-factory-entities"></a>Beziehung zwischen Data Factory-Entitäten
Data Factory verfügt über einige wichtige Entitäten, die zusammenarbeiten, um Ein- und Ausgabedaten, Verarbeitungsereignisse sowie den Zeitplan und die Ressourcen zu bestimmen, die für den Ablauf des gewünschten Datenflusses erforderlich sind.

![Diagramm: Data Factory, ein Clouddaten-Integrationsdienst – Grundlegende Konzepte](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Abbildung 2.** Beziehungen zwischen Dataset, Aktivität, Pipeline und verknüpftem Dienst

Nachdem Sie sich mit den vier einfachen Konzepten (verknüpfte Dienste, Datasets, Aktivitäten und Pipelines) vertraut gemacht haben, kann es auch schon losgehen: [Erstellen Sie Ihre erste Pipeline.](data-factory-build-your-first-pipeline.md)

## <a name="supported-regions"></a>Unterstützte Regionen
Derzeit können Data Factorys in den Regionen **USA, Westen**, **USA, Osten** und **Europa, Norden** erstellt werden. Eine Data Factory kann jedoch auf Datenspeicher und Compute Services in anderen Azure-Regionen zugreifen, um Daten zwischen Datenspeichern zu verschieben oder Daten mithilfe von Computediensten zu verarbeiten.

In Azure Data Factory selbst werden keine Daten gespeichert. Der Dienst ermöglicht das Erstellen von Datenflüssen, um die Verschiebung von Daten zwischen [unterstützten Datenspeichern](data-factory-data-movement-activities.md#supported-data-stores-and-formats) und die Verarbeitung von Daten mithilfe von [Compute Services](data-factory-compute-linked-services.md) in anderen Regionen oder in einer lokalen Umgebung zu orchestrieren. Außerdem können Sie mit programmgesteuerten und UI-basierten Mechanismen [Workflows überwachen und verwalten](data-factory-monitor-manage-pipelines.md) .

Data Factory ist zwar nur in den Regionen **USA, Westen**, **USA, Osten** und **Europa, Norden** verfügbar, der Datenverschiebungsdienst für Data Factory steht jedoch [global](data-factory-data-movement-activities.md#global) in verschiedenen Regionen zur Verfügung. Falls sich ein Datenspeicher hinter einer Firewall befindet, werden die Daten stattdessen von einem in der lokalen Umgebung installierten [Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) verschoben.

Nehmen wir beispielsweise an, dass Ihre Computeumgebungen wie Azure HDInsight-Cluster und Azure Machine Learning außerhalb der Region „Europa, Westen“ ausgeführt werden. Sie können eine Azure Data Factory-Instanz in der Region „Europa, Norden“ erstellen und mit ihr Aufträge in Ihren Computeumgebungen in der Region „Europa, Westen“ planen. Es dauert ein paar Millisekunden, bis Data Factory den Auftrag in Ihrer Computeumgebung ausgelöst hat. Die Dauer für die Ausführung des Auftrags in Ihrer Computeumgebung ändert sich aber nicht.

Es ist geplant, Azure Data Factory in Zukunft in allen von Azure unterstützten Gebieten bereitzustellen.

## <a name="next-steps"></a>Nächste Schritte
Informationen zum Erstellen von Data Factorys mit Datenpipelines erhalten Sie anhand von Schritt-für-Schritt-Anleitungen in den folgenden Tutorials:

| Lernprogramm: | Beschreibung |
| --- | --- |
| [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |In diesem Tutorial erstellen Sie eine Data Factory mit einer Pipeline, mit der **Daten aus Blob Storage in SQL-Datenbank verschoben werden** . |
| [Tutorial: Erstellen der ersten Pipeline zum Verarbeiten von Daten mithilfe eines Hadoop-Clusters](data-factory-build-your-first-pipeline.md) |In diesem Tutorial erstellen Sie Ihre erste Azure Data Factory mit einer Datenpipeline zur **Datenverarbeitung** , indem ein Hive-Skript in einem Azure HDInsight (Hadoop)-Cluster ausgeführt wird. |
| [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) |In diesem Tutorial erstellen Sie eine Data Factory mit einer Pipeline, die Daten aus einer **lokalen** SQL Server-Datenbank in ein Azure-Blob **verschiebt**. Im Rahmen der exemplarischen Vorgehensweise installieren und konfigurieren Sie das Datenverwaltungsgateway auf Ihrem Computer. |



<!--HONumber=Jan17_HO4-->


