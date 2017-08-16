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
ms.date: 08/14/2017
ms.author: shlo
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 537bdee67ed9648c3cba2099553d847399609705
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---
# <a name="introduction-to-azure-data-factory"></a>Einführung in Azure Data Factory 
## <a name="what-is-azure-data-factory"></a>Was ist Azure Data Factory?
Wie werden vorhandene Daten in Zeiten von Big Data in Unternehmen genutzt? Ist es möglich, in der Cloud generierte Daten zu erweitern, indem Referenzdaten von lokalen Datenquellen oder anderen unterschiedlichen Datenquellen verwendet werden? Eine Spiele-Unternehmen erfasst beispielsweise viele Protokolle, die von Spielen in der Cloud erstellt werden. Es möchte diese Protokolle analysieren, um Informationen zu den Vorlieben der Kunden, der Demografie, dem Nutzungsverhalten usw. zu erhalten und so Up-Selling- und Cross-Selling-Chancen zu identifizieren, neue überzeugende Features zu erstellen, die das Unternehmenswachstum fördern, und die Benutzerfreundlichkeit zu verbessern. 

Zum Analysieren dieser Protokolle benötigt das Unternehmen Referenzdaten, wie etwa Informationen zu den Kunden, zum Spiel und zur Marketingkampagne, die in einem lokalen Datenspeicher gespeichert sind. Daher möchte das Unternehmen Protokolldaten aus dem Clouddatenspeicher und Referenzdaten aus dem lokalen Datenspeicher erfassen. Anschließend werden die Daten mithilfe von Hadoop in der Cloud (Azure HDInsight) verarbeitet, und die Ergebnisdaten werden in einem Data Warehouse in der Cloud wie etwa Azure SQL Data Warehouse oder in einem lokalen Datenspeicher wie SQL Server veröffentlicht. Das Unternehmen möchte diesen Workflow einmal wöchentlich ausführen. 

Hierfür ist eine Plattform erforderlich, mit der das Unternehmen einen Workflow erstellen kann, der Daten aus lokalen Quellen sowie aus Clouddatenspeichern erfassen kann, Daten mithilfe vorhandener Compute Services wie Hadoop umwandelt oder verarbeitet und die Ergebnisse lokal oder in einem Clouddatenspeicher veröffentlicht, damit sie von BI-Anwendungen genutzt werden können. 

![Übersicht über Data Factory](media/data-factory-introduction/what-is-azure-data-factory.png) 

Azure Data Factory ist eine Plattform, die für solche Szenarien optimal geeignet ist. Sie ist ein **cloudbasierter Datenintegrationsdienst, mit dem Sie datengesteuerte Workflows in der Cloud erstellen können, um Datenverschiebungen und -transformationen zu orchestrieren und zu automatisieren**. Mit Azure Data Factory können Sie datengesteuerte Workflows (sogenannte Pipelines) erstellen und planen, die Daten aus unterschiedlichen Datenspeichern erfassen, diese Daten mithilfe von Compute Services wie Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics und Azure Machine Learning verarbeiten/transformieren und die Ausgabedaten für Datenspeicher wie Azure SQL Data Warehouse veröffentlichen, damit diese von Business Intelligence (BI)-Anwendungen genutzt werden können.  

Dies ist eher eine Extract-and-Load (EL)- und eine Transform-and-Load (TL)-Plattform als eine traditionelle Extract-Transform-and-Load (ETL)-Plattform. Die ausgeführten Transformationen dienen vielmehr zum Transformieren/Verarbeiten von Daten mithilfe von Compute Services als zum Ausführen von Transformationen wie beim Hinzufügen abgeleiteter Spalten, Zählen der Zeilenanzahl, Sortieren von Daten usw. 

Die Daten, die derzeit in Azure Data Factory von Workflows genutzt und erstellt werden, sind **Zeitsegmentdaten** (stündlich, täglich, wöchentlich usw.). Eine Pipeline kann z.B. einmal täglich Eingabedaten lesen, Daten verarbeiten und Ausgabedaten erstellen. Sie können einen Workflow auch nur einmal ausführen.  
  

## <a name="how-does-it-work"></a>Wie funktioniert Application Insights? 
Die Pipelines (datengesteuerten Workflows) in Azure Data Factory führen in der Regel die folgenden drei Schritte aus:

![Drei Phasen von Azure Data Factory](media/data-factory-introduction/three-information-production-stages.png)

### <a name="connect-and-collect"></a>Herstellen einer Verbindung und Sammeln von Daten
In Unternehmen sind verschiedene Typen von Daten in unterschiedlichen Quellen gespeichert. Der erste Schritt zur Erstellung eines Informationssystems für die Produktion umfasst das Herstellen der Verbindung mit allen erforderlichen Daten- und Verarbeitungsquellen, z.B. SaaS-Dienste, Dateifreigaben, FTP und Webdienste, und das Verschieben der Daten an einen zentralen Standort zur weiteren Verarbeitung nach Bedarf.

Ohne Data Factory müssen Unternehmen benutzerdefinierte Komponenten für die Datenverschiebung erstellen oder benutzerdefinierte Dienste schreiben, um diese Datenquellen und die Verarbeitung zu integrieren. Dies ist mit Kostenaufwand verbunden, und Systeme dieser Art lassen sich nur schwer integrieren und verwalten. Häufig fehlen für Unternehmen geeignete Überwachungs- und Warnfunktionen sowie die Steuerungsmöglichkeiten eines vollständig verwalteten Diensts.

Mit Data Factory können Sie die Kopieraktivität in einer Datenpipeline nutzen, um Daten sowohl aus lokalen als auch aus Cloud-Quelldatenspeichern zur weiteren Analyse in einen zentralen Datenspeicher in der Cloud zu verschieben. Beispielsweise können Sie Daten in einem Azure Data Lake Store sammeln und später transformieren, indem Sie einen Azure Data Lake Analytics-Computedienst verwenden. Oder sammeln Sie Daten in einer Azure Blob Storage-Instanz, und transformieren Sie sie später mit einem Azure HDInsight Hadoop-Cluster.

### <a name="transform-and-enrich"></a>Transformieren und Erweitern
Wenn Daten in einem zentralen Datenspeicher in der Cloud gespeichert sind, sollen die erfassten Daten mithilfe von Compute Services wie HDInsight Hadoop, Spark, Data Lake Analytics und Machine Learning verarbeitet oder transformiert werden. Darauf folgt das Erstellen transformierter Daten nach einem verwaltbaren und gesteuerten Zeitplan, um Produktionsumgebungen mit vertrauenswürdigen Daten zu versorgen. 

### <a name="publish"></a>Veröffentlichen 
Stellen Sie transformierte Daten aus der Cloud für lokale Quellen wie SQL Server bereit, oder behalten Sie sie in den Cloudspeicherquellen, um Sie für Business Intelligence (BI)- und Analysetools sowie andere Anwendungen zur Nutzung verfügbar zu machen.

## <a name="key-components"></a>Hauptkomponenten
Ein Azure-Abonnement kann über mindestens eine Azure Data Factory-Instanz (oder Data Factory) verfügen. Azure Data Factory besteht aus vier Hauptkomponenten, die zusammen die Plattform bereitstellen, auf der Sie datengesteuerte Workflows mit Schritten zum Verschieben und Transformieren von Daten zusammenstellen können. 

### <a name="pipeline"></a>Pipeline
Eine Data Factory kann eine oder mehrere Pipelines aufweisen. Eine Pipeline ist eine Gruppe mit Aktivitäten. Gemeinsam führen die Aktivitäten einer Pipeline eine Aufgabe durch. Eine Pipeline kann beispielsweise eine Gruppe mit Aktivitäten enthalten, die Daten aus einem Azure-Blob erfasst, und anschließend eine Hive-Abfrage in einem HDInsight-Cluster ausführen, um die Daten zu partitionieren. Der Vorteil hierbei ist, dass die Pipeline Ihnen das Verwalten der Aktivitäten als Gruppe ermöglicht – anstatt für jede Aktivität einzeln. Beispielsweise können Sie die Pipeline bereitstellen und planen, anstatt dies für die Aktivitäten jeweils individuell durchzuführen. 

### <a name="activity"></a>Aktivität
Eine Pipeline kann über eine oder mehrere Aktivitäten verfügen. Aktivitäten definieren die Aktionen, die Sie auf Ihre Daten anwenden. Beispielsweise können Sie eine Kopieraktivität verwenden, um Daten zwischen zwei Datenspeichern zu kopieren. Analog dazu können Sie eine Hive-Aktivität verwenden, die eine Hive-Abfrage für einen Azure-HDInsight-Cluster ausführt, um Ihre Daten zu transformieren oder zu analysieren. Data Factory unterstützt zwei Arten von Aktivitäten: Datenverschiebungen und Datentransformationen.

### <a name="data-movement-activities"></a>Datenverschiebungsaktivitäten
Die Kopieraktivität in Data Factory kopiert die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher. Data Factory unterstützt die folgenden Datenspeicher. Daten aus beliebigen Quellen können in beliebige Senken geschrieben werden. Klicken Sie auf einen Datenspeicher, um zu erfahren, wie Daten in diesen/aus diesem Speicher kopiert werden.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Weitere Informationen finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).

### <a name="data-transformation-activities"></a>Datentransformationsaktivitäten
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Weitere Informationen finden Sie im Artikel [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md).

### <a name="custom-net-activities"></a>Benutzerdefinierte .NET-Aktivitäten
Falls Sie Daten in einen/aus einem Datenspeicher verschieben müssen, der von der Kopieraktivität nicht unterstützt wird, oder Daten mit Ihrer eigenen Logik transformieren möchten, erstellen Sie eine **benutzerdefinierte .NET-Aktivität**. Informationen zum Erstellen und Verwenden einer benutzerdefinierten Aktivität finden Sie unter [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](data-factory-use-custom-activities.md).

### <a name="datasets"></a>Datasets
Eine Aktivität verwendet keine oder mehrere Datasets als Eingaben und erzeugt mindestens ein Dataset als Ausgabe. Datasets stellen Datenstrukturen in den Datenspeichern dar, die einfach auf die Daten zeigen bzw. verweisen, die Sie in Ihren Aktivitäten als Eingaben oder Ausgaben verwenden möchten. Ein Azure-Blob-Dataset kann beispielsweise den Blobcontainer und -ordner in Azure Blob Storage angeben, aus dem die Pipeline die Daten lesen soll. Oder ein Azure SQL-Tabellendataset gibt die Tabelle an, in die die Aktivität die Ausgabedaten schreibt. 

### <a name="linked-services"></a>Verknüpfte Dienste
Verknüpfte Dienste ähneln Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die für Data Factory zum Herstellen einer Verbindung mit externen Ressourcen erforderlich sind. Sie können sich dies wie folgt vorstellen: Ein verknüpfter Dienst definiert die Verbindung mit der Datenquelle, und ein Dataset stellt die Struktur der Daten dar. So gibt etwa ein verknüpfter Azure Storage-Dienst die Verbindungszeichenfolge an, um eine Verbindung mit einem Azure Storage-Konto herzustellen. Und ein Azure-Blobdataset gibt den Blobcontainer und den Ordner an, der die Daten enthält.   

(Beispiele: Azure Storage, lokale SQL Server-Instanz, Azure HDInsight) Verknüpfte Dienste haben in Data Factory zwei Zwecke:

* Sie stellen einen **Datenspeicher** dar (beispielsweise eine lokale SQL Server-Instanz, eine Oracle-Datenbank, eine Dateifreigabe oder ein Azure Blob Storage-Konto). Eine Liste mit unterstützten Datenspeichern finden Sie im Abschnitt [Datenverschiebungsaktivitäten](#data-movement-activities) .
* Sie stellen eine **Computeressource** dar, die die Ausführung einer Aktivität hosten kann. So wird beispielsweise die HDInsightHive-Aktivität in einem HDInsight-Hadoop-Cluster ausgeführt. Eine Liste mit unterstützten Computeumgebungen finden Sie unter [Datentransformationsaktivitäten](#data-transformation-activities).

### <a name="relationship-between-data-factory-entities"></a>Beziehung zwischen Data Factory-Entitäten
![Diagramm: Data Factory, ein Clouddaten-Integrationsdienst – Grundlegende Konzepte](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Abbildung 2.** Beziehungen zwischen Dataset, Aktivität, Pipeline und verknüpftem Dienst

## <a name="supported-regions"></a>Unterstützte Regionen
Derzeit können Data Factorys in den Regionen **USA, Westen**, **USA, Osten** und **Europa, Norden** erstellt werden. Eine Data Factory kann jedoch auf Datenspeicher und Compute Services in anderen Azure-Regionen zugreifen, um Daten zwischen Datenspeichern zu verschieben oder Daten mithilfe von Computediensten zu verarbeiten.

In Azure Data Factory selbst werden keine Daten gespeichert. Der Dienst ermöglicht das Erstellen von datengesteuerten Workflows, um die Verschiebung von Daten zwischen [unterstützten Datenspeichern](#data-movement-activities) und die Verarbeitung von Daten mithilfe von [Compute Services](#data-transformation-activities) in anderen Regionen oder in einer lokalen Umgebung zu orchestrieren. Außerdem können Sie mit programmgesteuerten und UI-basierten Mechanismen [Workflows überwachen und verwalten](data-factory-monitor-manage-pipelines.md) .

Data Factory ist zwar nur in den Regionen **USA, Westen**, **USA, Osten** und **Europa, Norden** verfügbar, der Datenverschiebungsdienst für Data Factory steht jedoch [global](data-factory-data-movement-activities.md#global) in verschiedenen Regionen zur Verfügung. Wenn sich ein Datenspeicher hinter einer Firewall befindet, werden die Daten stattdessen von einem in der lokalen Umgebung installierten [Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) verschoben.

Nehmen wir beispielsweise an, dass Ihre Computeumgebungen wie Azure HDInsight-Cluster und Azure Machine Learning außerhalb der Region „Europa, Westen“ ausgeführt werden. Sie können eine Azure Data Factory-Instanz in der Region „Europa, Norden“ erstellen und mit ihr Aufträge in Ihren Computeumgebungen in der Region „Europa, Westen“ planen. Es dauert ein paar Millisekunden, bis Data Factory den Auftrag in Ihrer Computeumgebung ausgelöst hat. Die Dauer für die Ausführung des Auftrags in Ihrer Computeumgebung ändert sich aber nicht.

## <a name="get-started-with-creating-a-pipeline"></a>Erste Schritte beim Erstellen einer Pipeline
Verwenden Sie diese Tools oder APIs, um Datenpipelines in Azure Data Factory zu erstellen: 

- Azure-Portal
- Visual Studio
- PowerShell
- .NET API
- REST-API
- Azure Resource Manager-Vorlage 

Informationen zum Erstellen von Data Factorys mit Datenpipelines erhalten Sie anhand von Schritt-für-Schritt-Anleitungen in den folgenden Tutorials:

| Lernprogramm: | Beschreibung |
| --- | --- |
| [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |In diesem Tutorial erstellen Sie eine Data Factory mit einer Pipeline, mit der **Daten aus Blob Storage in SQL-Datenbank verschoben werden** . |
| [Tutorial: Erstellen der ersten Pipeline zum Verarbeiten von Daten mithilfe eines Hadoop-Clusters](data-factory-build-your-first-pipeline.md) |In diesem Tutorial erstellen Sie Ihre erste Azure Data Factory mit einer Datenpipeline zur **Datenverarbeitung** , indem ein Hive-Skript in einem Azure HDInsight (Hadoop)-Cluster ausgeführt wird. |
| [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](data-factory-move-data-between-onprem-and-cloud.md) |In diesem Tutorial erstellen Sie eine Data Factory mit einer Pipeline, die Daten aus einer **lokalen** SQL Server-Datenbank in ein Azure-Blob **verschiebt**. Im Rahmen der exemplarischen Vorgehensweise installieren und konfigurieren Sie das Datenverwaltungsgateway auf Ihrem Computer. |

