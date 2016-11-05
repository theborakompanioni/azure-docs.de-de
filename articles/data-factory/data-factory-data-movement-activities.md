---
title: Verschieben von Daten mit der Kopieraktivität | Microsoft Docs
description: 'Informieren Sie sich über das Verschieben von Daten in Data Factory-Pipelines: Datenmigration zwischen Cloudspeichern sowie zwischen lokalen Speichern und Cloudspeichern. Verwenden der Kopieraktivität'
keywords: Daten kopieren, Datenverschiebung, Datenmigration, Daten übertragen
services: data-factory
documentationcenter: ''
author: linda33wj
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2016
ms.author: jingwang

---
# Verschieben von Daten mit der Kopieraktivität
## Übersicht
In Azure Data Factory können Sie die Kopieraktivität verwenden, um Daten unterschiedlicher Formen von verschiedenen lokalen und Clouddatenquellen nach Azure zu kopieren. Nach dem Kopieren können die Daten weiter transformiert und analysiert werden. Sie können die Kopieraktivität auch zum Veröffentlichen von Transformations- und Analyseergebnissen für die Verwendung für Business Intelligence (BI) und in Anwendungen verwenden.

![Rolle der Kopieraktivität](media/data-factory-data-movement-activities/copy-activity.png)

Der Kopieraktivität liegt ein sicherer, zuverlässiger, skalierbarer und [global verfügbarer Dienst](#global) zugrunde. Dieser Artikel enthält Details zum Verschieben von Daten in Data Factory und zur Kopieraktivität.

Zunächst erfahren Sie, wie die Datenmigration zwischen zwei Clouddatenspeichern und zwischen einem lokalen Datenspeicher und einem Clouddatenspeicher abläuft.

> [!NOTE]
> Allgemeine Informationen zu Aktivitäten finden Sie unter [Grundlegendes zu Pipelines und Aktivitäten](data-factory-create-pipelines.md).
> 
> 

### Kopieren von Daten zwischen zwei Clouddatenspeichern
Wenn sich sowohl Quell- als auch der Senkendatenspeicher in der Cloud befinden, durchläuft die Kopieraktivität die folgenden Phasen, um die Daten aus der Quelle in die Senke zu kopieren. Folgendes gilt für den Dienst, auf dem die Kopieraktivität basiert:

1. Er liest Daten aus dem Quelldatenspeicher.
2. Er führt die Serialisierung/Deserialisierung, Komprimierung/Dekomprimierung, Spaltenzuordnung und Typumwandlung durch. Diese Vorgänge erfolgen basierend auf den Konfigurationen von Eingabedataset, Ausgabedataset und Kopieraktivität.
3. Er schreibt Daten in den Zieldatenspeicher.

Der Dienst wählt automatisch die optimale Region zum Durchführen der Datenverschiebung aus. Diese Region ist üblicherweise diejenige, die sich am nächsten am Senkendatenspeicher befindet.

![Cloud-zu-Cloud-Kopie](./media/data-factory-data-movement-activities/cloud-to-cloud.png)

### Kopieren von Daten zwischen einem lokalen Datenspeicher und einem Clouddatenspeicher
Um Daten sicher zwischen einem lokalen Datenspeicher und einem Clouddatenspeicher zu verschieben, installieren Sie auf dem lokalen Computer ein Datenverwaltungsgateway. Ein Datenverwaltungsgateway ist ein Agent, der eine hybride Datenverschiebung und -verarbeitung ermöglicht. Sie können das Gateway auf dem gleichen Computer installieren, auf dem sich der Datenspeicher selbst befindet, oder auf einem separaten Computer, der Zugriff auf den Datenspeicher hat.

In diesem Szenario führt das Datenverwaltungsgateway die Serialisierung/Deserialisierung, Komprimierung/Dekomprimierung, Spaltenzuordnung und Typumwandlung aus. Die Daten fließen nicht über den Azure Data Factory-Dienst. Das Datenverwaltungsgateway schreibt die Daten stattdessen direkt in den Zielspeicher.

![Kopie zwischen lokalem und Cloudspeicher](./media/data-factory-data-movement-activities/onprem-to-cloud.png)

Unter [Verschieben von Daten zwischen lokalen Datenspeichern und Clouddatenspeichern](data-factory-move-data-between-onprem-and-cloud.md) finden Sie eine Einführung und eine exemplarische Vorgehensweise zu diesem Thema. Ausführliche Informationen zu diesem Agent finden Sie unter [Datenverwaltungsgateway](data-factory-data-management-gateway.md).

Sie können mithilfe des Datenverwaltungsgateways auch Daten von bzw. in unterstützte Datenspeicher verschieben, die auf virtuellen Azure IaaS-Computern gehostet werden. In diesem Fall können Sie das Datenverwaltungsgateway auf dem gleichen virtuellen Computer installieren, auf dem sich der Datenspeicher selbst befindet, oder auf einem separaten virtuellen Computer, der Zugriff auf den Datenspeicher hat.

## Unterstützte Datenspeicher und Formate
[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Wenn Sie Daten in einen/aus einem Datenspeicher verschieben müssen, der von der Kopieraktivität nicht unterstützt wird, verwenden Sie die **benutzerdefinierte Aktivität** in Data Factory mit Ihrer eigenen Logik zum Kopieren/Verschieben von Daten. Informationen zum Erstellen und Verwenden einer benutzerdefinierten Aktivität finden Sie unter [Verwenden von benutzerdefinierten Aktivitäten in einer Azure Data Factory-Pipeline](data-factory-use-custom-activities.md).

### Unterstützte Dateiformate
Sie können die Kopieraktivität verwenden, um Dateien unverändert zwischen zwei dateibasierten Datenspeichern zu kopieren, wie z.B. Azure-Blob, Dateisystem und HDFS (Hadoop Distributed File System). Zu diesem Zweck können Sie in den Definitionen sowohl für das Eingabe- als auch für das Ausgabedataset den [Formatierungsabschnitt](data-factory-create-datasets.md) überspringen. Die Daten werden effizient ohne jegliche Serialisierung oder Deserialisierung kopiert.

Die Kopieraktivität liest und schreibt Dateien in den folgenden Formaten: Text, Avro, ORC, Parquet und JSON. Sie können z.B. folgende Kopieraktivitäten ausführen:

* Kopieren von Daten im Textformat (CSV) aus einem Azure-Blob und Schreiben in Azure SQL-Datenbank.
* Kopieren von Dateien im Textformat (CSV) aus dem lokalen Dateisystem und Schreiben in einen Azure-Blob im Avro-Format.
* Kopieren von Daten aus Azure SQL-Datenbank und Schreiben in ein lokales HDFS im ORC-Format.

## <a name="global"></a>Global verfügbare Datenverschiebung
Azure Data Factory ist nur in den USA (Westen), USA (Osten) und Nordeuropa verfügbar. Jedoch ist der Dienst, der die Kopieraktivität unterstützt, in den folgenden Regionen und Ländern global verfügbar. Die global verfügbare Topologie gewährleistet effiziente Datenverschiebungen, die regionsübergreifende Hops in der Regel vermeiden. Unter [Dienste nach Region](https://azure.microsoft.com/regions/#services) erfahren Sie, in welchen Regionen Data Factory und die Datenverschiebung verfügbar sind.

### Kopieren von Daten zwischen Clouddatenspeichern
Wenn sich Quell- und Senkendatenspeicher in der Cloud befinden, verwendet Data Factory zum Verschieben der Daten eine Dienstbereitstellung in der Region, die dem Gebiet der Senke am nächsten liegt. Informationen zur Zuordnung erhalten Sie in der folgenden Tabelle:

| Region des Zieldatenspeichers | Verwendete Region für die Datenverschiebung |
|:--- |:--- |
| USA, Osten |USA, Osten |
| USA, Osten 2 |USA (Ost) 2 |
| USA (West) |USA (West) |
| USA, Westen 2 |USA (West) |
| USA (Mitte) |USA (Mitte) |
| USA, Westen-Mitte |USA (Mitte) |
| USA Nord Mitte |USA, Norden-Mitte |
| USA, Süden-Mitte |USA, Süden-Mitte |
| Europa, Norden |Europa, Norden |
| Europa, Westen |Europa, Westen |
| Südostasien |Südostasien |
| Ostasien |Südostasien |
| Japan Ost |Japan, Osten |
| Japan, Westen |Japan, Osten |
| Brasilien, Süden |Brasilien, Süden |
| Australien, Osten |Australien, Osten |
| Australien, Südosten |Australien, Südosten |
| Indien, Mitte |Indien, Mitte |
| Indien, Süden |Indien, Mitte |
| Indien, Westen |Indien (Mitte) |

> [!NOTE]
> Wenn die Region des Zieldatenspeichers in der obigen Liste nicht enthalten ist, schlägt die Kopieraktivität fehl, anstatt eine Alternativregion auszuwählen.
> 
> 

### Kopieren von Daten zwischen einem lokalen Datenspeicher und einem Clouddatenspeicher
Wenn Daten zwischen lokalen Datenspeichern (oder virtuellen Azure-Computern/IaaS) und Cloudspeichern kopiert werden, wird die Datenverschiebung vom [Datenverwaltungsgateway](data-factory-data-management-gateway.md) auf einem lokalen oder einem virtuellen Computer durchgeführt. Die Daten fließen nicht durch den Dienst in der Cloud, es sei denn, Sie verwenden die Funktion des [gestaffelten Kopierens](data-factory-copy-activity-performance.md#staged-copy). In diesem Fall fließen die Daten durch den Azure-Blobstagingspeicher, bevor sie in den Senkendatenspeicher geschrieben werden.

## Erstellen einer Pipeline mit einer Kopieraktivität
Sie können eine Pipeline mit einer Kopieraktivität auf verschiedene Arten erstellen:

### Mithilfe des Assistenten zum Kopieren
Der Data Factory-Kopier-Assistent unterstützt Sie beim Erstellen einer Pipeline mit einer Kopieraktivität. Diese Pipeline ermöglicht das Kopieren von Daten aus unterstützten Quellen an Ziele *ohne das Schreiben von JSON-Definitionen* für verknüpfte Dienste, Datasets und Pipelines. Unter [Assistent zum Kopieren in Data Factory](data-factory-copy-wizard.md) finden Sie Einzelheiten zum Assistenten.

### Mithilfe von JSON-Skripts
Sie können den Data Factory-Editor im Azure-Portal, Visual Studio oder Azure PowerShell verwenden, um eine JSON-Definition für eine Pipeline (mit Kopieraktivität) zu erstellen. Anschließend können Sie diese bereitstellen, um die Pipeline in Data Factory zu erstellen. Unter [Tutorial: Verwenden der Kopieraktivität in einer Azure Data Factory-Pipeline](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie ein Tutorial mit Schrittanleitungen.

JSON-Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabetabellen und Richtlinien sind für alle Arten von Aktivitäten verfügbar. Die Eigenschaften, die im Abschnitt `typeProperties` der Aktivität verfügbar sind, variieren je nach Aktivitätstyp.

Bei der Kopieraktivität variiert der Abschnitt `typeProperties` je nach Art der Quellen und Senken. Klicken Sie im Abschnitt mit [unterstützten Quellen und Senken](#supported-data-stores) auf eine Quelle bzw. Senke, um Informationen zu den Typeigenschaften zu erhalten, die von der Kopieraktivität für diesen Datenspeicher unterstützt werden.

Dies ist eine Beispiel-JSON-Definition:

    {
      "name": "ADFTutorialPipeline",
      "properties": {
        "description": "Copy data from Azure blob to Azure SQL table",
        "activities": [
          {
            "name": "CopyFromBlobToSQL",
            "type": "Copy",
            "inputs": [
              {
                "name": "InputBlobTable"
              }
            ],
            "outputs": [
              {
                "name": "OutputSQLTable"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "SqlSink",
                "writeBatchSize": 10000,
                "writeBatchTimeout": "60:00:00"
              }
            },
            "Policy": {
              "concurrency": 1,
              "executionPriorityOrder": "NewestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
        ],
        "start": "2016-07-12T00:00:00Z",
        "end": "2016-07-13T00:00:00Z"
      }
    }

Der im Ausgabedataset definierte Zeitplan legt fest, wann die Aktivität ausgeführt wird (z.B. **täglich**, Häufigkeit: **Tag**, Intervall: **1**). Die Aktivität kopiert Daten aus einem Eingabedataset (**Quelle**) in ein Ausgabedataset (**Senke**).

Sie können mehr als ein Eingabedataset für die Kopieraktivität angeben. Diese werden verwendet, um die Abhängigkeiten zu überprüfen, bevor die Aktivität ausgeführt wird. Es werden allerdings nur die Daten aus dem ersten Dataset in das Zieldataset kopiert. Weitere Informationen finden Sie unter [Planung und Ausführung](data-factory-scheduling-and-execution.md).

## Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken. Außerdem wird die beobachtete Leistung bei internen Tests aufgeführt und es werden verschiedene Methoden zur Optimierung der Leistung der Kopieraktivität erläutert.

## Planen und sequenzielles Kopieren
Ausführliche Informationen zur Planung und Ausführung in Data Factory finden Sie unter [Planung und Ausführung](data-factory-scheduling-and-execution.md). Es ist möglich, mehrere Kopiervorgänge nacheinander sequenziell/sortiert auszuführen. Siehe Abschnitt [Sortierte Kopie](data-factory-scheduling-and-execution.md#ordered-copy).

## Typkonvertierungen
Verschiedene Datenspeicher weisen unterschiedliche systemeigene Typsysteme auf. Die Kopieraktivität führt automatische Typumwandlungen von Quelltypen in Senkentypen mithilfe des folgenden aus zwei Schritten bestehenden Ansatzes durch:

1. Konvertieren von nativen Quelltypen in einen .NET-Typ.
2. Konvertieren eines .NET-Typs in einen nativen Senkentyp.

Die Zuordnung zwischen einem nativen Typ und einem .NET-Typ für den Datenspeicher finden Sie im entsprechenden Artikel zum Datenspeicher. (Klicken Sie auf den angegebenen Link in der Tabelle [Unterstützte Datenspeicher](#supported-data-stores).) Über diese Zuordnungen können Sie beim Erstellen von Tabellen die geeigneten Typen bestimmen, damit die Kopieraktivität die richtigen Umwandlungen ausführt.

## Nächste Schritte
* Weitere Informationen über die Kopieraktivität finden Sie unter [Kopieren von Daten aus Azure Blob Storage in Azure SQL-Datenbank](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
* Unter [Verschieben von Daten zwischen lokalen Quellen und der Cloud](data-factory-move-data-between-onprem-and-cloud.md) erfahren Sie, wie Sie Daten aus einem lokalen Datenspeicher in einen Clouddatenspeicher verschieben.

<!---HONumber=AcomDC_0928_2016-->