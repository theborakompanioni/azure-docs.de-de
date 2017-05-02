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
ms.date: 04/21/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: 260208e7c7a08110eb3c885ef86ec4c18ff42fc9
ms.openlocfilehash: 40552b5d3cea5b04826c08e7b4b1d046a9fcefba
ms.lasthandoff: 04/23/2017


---
# <a name="introduction-to-azure-data-factory-service-a-data-integration-service-in-the-cloud"></a>Einführung in den Azure Data Factory-Dienst (Datenintegrationsdienst in der Cloud)
## <a name="what-is-azure-data-factory"></a>Was ist Azure Data Factory?
Wie werden vorhandene Daten in Zeiten von Big Data in Unternehmen genutzt? Ist es möglich, in der Cloud generierte Daten zu erweitern, indem Referenzdaten von lokalen Datenquellen oder anderen unterschiedlichen Datenquellen verwendet werden? Hierfür wird eine Plattform zum Aggregieren und Verarbeiten von Daten einer Vielzahl von Quellen benötigt. Azure Data Factory ist ein cloudbasierter Datenintegrationsdienst, der das **Verschieben** und **Transformieren** von Daten orchestriert und automatisiert. Sie können Lösungen für die Datenintegration erstellen, mit denen Eingabedaten aus unterschiedlichen Datenspeichern erfasst, die Daten transformiert bzw. verarbeitet und Ausgabedaten für andere Datenspeicher veröffentlicht werden können. 

![Diagramm: Übersicht über Data Factory (Datenintegrationsdienst)](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Abbildung 1:** Erfassen Sie Daten aus unterschiedlichen Datenquellen, bereiten Sie sie vor, transformieren und analysieren Sie sie, und veröffentlichen Sie nutzbare Daten.


## <a name="what-does-it-offer"></a>Was ist hierbei möglich? 
Bisher ging es bei Datenintegrationsprojekten immer um die Erstellung von ETL-Prozessen (Extrahieren, Transformieren und Laden), bei denen Daten aus verschiedenen Datenquellen einer Organisation extrahiert werden. Als Nächstes werden die Daten transformiert, damit sie mit dem Zielschema eines Enterprise Data Warehouse (EDW) konform sind, und in ein EDW geladen. Dies ist in der folgenden Abbildung dargestellt. Anschließend wird auf das EDW als alleingültige Quelle für BI-Analyselösungen zugegriffen.

![Herkömmliches ETL](media/data-factory-introduction/traditional-etl.png)
**Herkömmliches ETL**

Wie in der folgenden Abbildung dargestellt, verändert sich die Geschäftslandschaft für Unternehmen heutzutage in Bezug auf Volumen, Vielseitigkeit und Komplexität immer stärker. Sie ist vielfältiger als jemals zuvor, und es werden lokale Daten und Clouddaten unterschiedlicher Form und Geschwindigkeit genutzt. Daten müssen übergreifend für mehrere geografische Standorte verarbeitet werden, und es wird eine Kombination aus Open-Source-Software, kommerziellen Lösungen und benutzerdefinierten Verarbeitungsdiensten verwendet, die teuer sind und deren Integration und Wartung aufwändig ist. Die Flexibilität, die zur Anpassung an diese sich ständig verändernde Big Data-Landschaft erforderlich ist, ist eine Chance zur Zusammenführung des herkömmlichen EDW-Ansatzes mit Funktionen, die für ein modernes Informationssystem in der Produktion benötigt werden. Diese Plattform für die Zusammenführung ist Azure Data Factory. Hiermit können herkömmliche EDWs und die sich verändernde Datenlandschaft abgedeckt werden, und Unternehmen können alle verfügbaren Daten für die datengestützte Entscheidungsfindung nutzen.

![Neue Big Data-Landschaft](media/data-factory-introduction/new-big-data-landscape.png)
**Neue Big Data-Landschaft**

Mithilfe des Azure Data Factory-Diensts erhalten Unternehmen die Möglichkeit, die vielfältigen Anforderungen zu bewältigen, indem eine Plattform zum **Zusammenführen der Datenverarbeitung, -speicherung und -verschiebung in Informationspipelines für die Produktion** und zum Verwalten von vertrauenswürdigen Datenassets bereitgestellt wird.

Mit dem Azure Data Factory-Dienst haben Sie folgende Möglichkeiten:
- **Einfaches Arbeiten mit unterschiedlichen Datenspeicher- und -verarbeitungssystemen** 

    In Unternehmen sind Daten verschiedener Art in unterschiedlichen Quellen gespeichert. Der erste Schritt zur Erstellung eines Informationssystems für die Produktion umfasst das Herstellen der Verbindung mit allen erforderlichen Daten- und Verarbeitungsquellen, z.B. SaaS-Dienste, Dateifreigaben, FTP und Webdienste, und das Verschieben der Daten an einen zentralen Standort zur weiteren Verarbeitung nach Bedarf.

    Ohne Data Factory müssen Unternehmen benutzerdefinierte Komponenten für die Datenverschiebung erstellen oder benutzerdefinierte Dienste schreiben, um diese Datenquellen und die Verarbeitung zu integrieren. Dies ist mit Kostenaufwand verbunden, und Systeme dieser Art lassen sich nur schwer integrieren und verwalten. Häufig fehlen für Unternehmen geeignete Überwachungs- und Warnfunktionen sowie die Steuerelemente eines vollständig verwalteten Diensts.

    Mit Data Factory können Sie die Kopieraktivität in einer Datenpipeline nutzen, um Daten sowohl aus lokalen als auch aus Cloud-Quelldatenspeichern zur weiteren Analyse in einen zentralen Datenspeicher in der Cloud zu verschieben. Beispielsweise können Sie Daten in einem Azure Data Lake Store sammeln und später transformieren, indem Sie einen Azure Data Lake Analytics-Computedienst verwenden. Oder sammeln Sie Daten in einer Azure Blob Storage-Instanz, und transformieren Sie sie später mit einem Azure HDInsight Hadoop-Cluster.
- **Transformieren von Daten in vertrauenswürdige Informationen** 

    Nachdem Daten in einem zentralen Datenspeicher in der Cloud vorhanden sind, folgt als Nächstes die Erstellung und Bereitstellung von Datenpipelines zur zuverlässigen Generierung von transformierten Daten nach einem verwaltbaren und gesteuerten Zeitplan, um Produktionsumgebungen mit vertrauenswürdigen Daten zu versorgen. Die Datentransformation wird in Azure Data Factory mit Transformationsaktivitäten wie Hive, Pig, MapReduce, Azure Machine Learning Batch Execution und benutzerdefinierten C#-Aktivitäten in einem Azure HDInsight Hadoop-Cluster, auf Azure Machine Learning-VMs oder in einem Azure Batch-Pool mit VMs durchgeführt.
- **Überwachen der Datenpipelines an einem zentralen Ort**

    Bei einem vielfältigen Datenportfolio ist es wichtig, dass Sie einen zuverlässigen und vollständigen Überblick über Ihre Speicher-, Verarbeitungs- und Datenverschiebungsdienste haben. Mit Data Factory können Sie den Status der Datenpipeline schnell umfassend ermitteln, Probleme identifizieren und die entsprechenden Maßnahmen ergreifen, falls dies notwendig ist. Verfolgen Sie die Datenherkunft sowie die Beziehungen zwischen den Daten über verschiedene Quellen hinweg visuell. Über ein einziges Überwachungsdashboard können Sie den vollständigen Verlauf der Auftragsausführung, den Systemstatus und die Abhängigkeiten verfolgen.

## <a name="how-does-it-work"></a>Wie funktioniert Application Insights? 
Eine Azure Data Factory-Instanz enthält drei Phasen zur Generierung von Informationen:

![Drei Phasen der Informationsgenerierung](media/data-factory-introduction/three-information-production-stages.png)

- **Verbinden und Sammeln**: In dieser Phase werden Daten aus unterschiedlichen Datenquellen an einem Ort zusammengefasst.
- **Transformieren und Erweitern**: In dieser Phase werden die gesammelten Daten verarbeitet oder transformiert.
- **Veröffentlichen**: In dieser Phase werden die Daten veröffentlicht, damit sie von BI-Tools, Analysetools und anderen Anwendungen genutzt werden können.

## <a name="key-components"></a>Hauptkomponenten
Ein Azure-Abonnement kann über mindestens eine Azure Data Factory-Instanz (oder Data Factory) verfügen. Azure Data Factory besteht aus vier Hauptkomponenten, die zusammen die Plattform bereitstellen, auf der Sie einfache und komplexe Datenverschiebungen und Transformationsorchestrierungen für Ihren Datenfluss zusammenstellen können.

### <a name="activity"></a>Aktivität
Aktivitäten definieren die Aktionen, die Sie auf Ihre Daten anwenden. Beispielsweise können Sie eine Kopieraktivität verwenden, um Daten zwischen zwei Datenspeichern zu kopieren. Analog dazu können Sie eine Hive-Aktivität verwenden, die eine Hive-Abfrage für einen Azure-HDInsight-Cluster ausführt, um Ihre Daten zu transformieren oder zu analysieren. Data Factory unterstützt zwei Arten von Aktivitäten: Datenverschiebungen und Datentransformationen.

Jede Aktivität kann über null oder mehr Eingabedatasets verfügen und ein oder mehrere Ausgabedatasets erstellen. 


### <a name="data-movement-activities"></a>Datenverschiebungsaktivitäten
Die Kopieraktivität in Data Factory kopiert die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher. Data Factory unterstützt die folgenden Datenspeicher. Daten aus beliebigen Quellen können in beliebige Senken geschrieben werden. Klicken Sie auf einen Datenspeicher, um zu erfahren, wie Daten in diesen/aus diesem Speicher kopiert werden.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Weitere Informationen finden Sie im Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md).

### <a name="data-transformation-activities"></a>Datentransformationsaktivitäten
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Weitere Informationen finden Sie im Artikel [Datentransformationsaktivitäten](data-factory-data-transformation-activities.md).

Falls Sie Daten in einen/aus einem Datenspeicher verschieben müssen, der von der Kopieraktivität nicht unterstützt wird, oder Daten mit Ihrer eigenen Logik transformieren möchten, erstellen Sie eine **benutzerdefinierte .NET-Aktivität**. Informationen zum Erstellen und Verwenden einer benutzerdefinierten Aktivität finden Sie unter [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](data-factory-use-custom-activities.md).

### <a name="pipeline"></a>Pipeline
Eine Pipeline ist eine Gruppe mit Aktivitäten. Gemeinsam führen die Aktivitäten einer Pipeline eine Aufgabe durch. Eine Pipeline kann beispielsweise eine Gruppe mit Aktivitäten enthalten, die Daten aus einem Azure-Blob erfasst, und anschließend eine Hive-Abfrage in einem HDInsight-Cluster ausführen, um die Protokolldaten zu partitionieren. Der Vorteil hierbei ist, dass die Pipeline Ihnen das Verwalten der Aktivitäten als Gruppe ermöglicht – anstatt für jede Aktivität einzeln. Beispielsweise können Sie die Pipeline bereitstellen und planen, anstatt dies für die Aktivitäten jeweils individuell durchzuführen.

### <a name="datasets"></a>Datasets
Datasets stellen Datenstrukturen in den Datenspeichern dar, die einfach auf die Daten zeigen bzw. verweisen, die Sie in Ihren Aktivitäten als Eingaben oder Ausgaben verwenden möchten. Ein Azure-Blob-Dataset kann beispielsweise den Blobcontainer und -ordner in Azure Blob Storage angeben, aus dem die Pipeline die Daten lesen soll. 

### <a name="linked-services"></a>Verknüpfte Dienste
Verknüpfte Dienste ähneln Verbindungszeichenfolgen, mit denen die Verbindungsinformationen definiert werden, die für Data Factory zum Herstellen einer Verbindung mit externen Ressourcen erforderlich sind. Sie können sich dies wie folgt vorstellen: Das Dataset stellt die Struktur der Daten dar, und der verknüpfte Dienst definiert die Verbindung mit der Datenquelle.  (Beispiele: Azure Storage, lokale SQL Server-Instanz, Azure HDInsight) Verknüpfte Dienste haben in Data Factory zwei Zwecke:

* Sie stellen einen **Datenspeicher** dar (beispielsweise eine lokale SQL Server-Instanz, eine Oracle-Datenbank, eine Dateifreigabe oder ein Azure Blob Storage-Konto). Eine Liste mit unterstützten Datenspeichern finden Sie im Abschnitt [Datenverschiebungsaktivitäten](#data-movement-activities) .
* Sie stellen eine **Computeressource** dar, die die Ausführung einer Aktivität hosten kann. So wird beispielsweise die HDInsightHive-Aktivität in einem HDInsight-Hadoop-Cluster ausgeführt. Eine Liste mit unterstützten Computeumgebungen finden Sie unter [Datentransformationsaktivitäten](#data-transformation-activities).

### <a name="relationship-between-data-factory-entities"></a>Beziehung zwischen Data Factory-Entitäten
![Diagramm: Data Factory, ein Clouddaten-Integrationsdienst – Grundlegende Konzepte](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Abbildung 2.** Beziehungen zwischen Dataset, Aktivität, Pipeline und verknüpftem Dienst

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

