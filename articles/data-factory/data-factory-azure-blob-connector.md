---
title: Kopieren von Daten in und aus Azure Blob Storage | Microsoft Docs
description: Hier erfahren Sie, wie Sie Blobdaten in Azure Data Factory kopieren. Verwenden Sie unser Beispiel zum Kopieren von Daten aus Azure-Blobspeicher in die Azure SQL-Datenbank (und umgekehrt).
keywords: Blobdaten, Azure-Blobkopie
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: jingwang
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: 5e724a03048ea2faaed9659ec3e4aace5d9339c6
ms.lasthandoff: 03/29/2017


---
# <a name="move-data-to-or-from-azure-blob-storage-using-azure-data-factory"></a>Verschieben von Daten in oder aus Azure Blob Storage mithilfe von Azure Data Factory
Dieser Artikel beschreibt, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten in und aus Azure Blob Storage zu verschieben. Dieser Artikel baut auf dem Artikel zu [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) auf, der eine allgemeine Übersicht zur Datenverschiebung mit der Kopieraktivität bietet. 

Sie können Daten aus einem beliebigen unterstützten Quelldatenspeicher in Azure Blob Storage bzw. aus Azure Blob Storage in einen beliebigen unterstützten Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](data-factory-data-movement-activities.md#supported-data-stores-and-formats). 

Die Kopieraktivität unterstützt das Kopieren von Daten in und aus Azure Storage-Konten für allgemeine Zwecke und Blob Storage (Hot/Cool). Die Aktivität unterstützt das Lesen aus Block-, Anfüge- und Seitenblobs. Das Schreiben wird jedoch nur für Blockblobs unterstützt. Azure Premium Storage wird als Senke nicht unterstützt, da er vor dem Hintergrund von Seitenblobs funktioniert.

## <a name="getting-started"></a>Erste Schritte
Sie können eine Pipeline mit einer Kopieraktivität erstellen, die Daten mithilfe verschiedener Tools/APIs in und aus Azure Blob Storage verschiebt.

Am einfachsten erstellen Sie eine Pipeline mit dem **Kopier-Assistenten**. Unter [Tutorial: Erstellen einer Pipeline mit dem Assistenten zum Kopieren](data-factory-copy-data-wizard-tutorial.md) finden Sie eine kurze exemplarische Vorgehensweise zum Erstellen einer Pipeline mithilfe des Assistenten zum Kopieren von Daten.

Sie können auch die folgenden Tools für das Erstellen einer Pipeline verwenden: **Azure-Portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-Vorlagen**, **.NET-API** und **REST-API**. Im [Tutorial zur Kopieraktivität](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können. 

Unabhängig davon, ob Sie Tools oder APIs verwenden, führen Sie die folgenden Schritte aus, um eine Pipeline zu erstellen, die Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher verschiebt: 

1. Erstellen **verknüpfter Dienste** zum Verknüpfen von Eingabe- und Ausgabedatenspeichern mit Ihrer Data Factory.
2. Erstellen von **Datasets** zur Darstellung von Eingabe- und Ausgabedaten für den Kopiervorgang. 
3. Erstellen einer **Pipeline** mit einer Kopieraktivität, die ein Dataset als Eingabe und ein Dataset als Ausgabe akzeptiert. 

Wenn Sie den Assistenten verwenden, werden automatisch JSON-Definitionen für diese Data Factory-Entitäten (verknüpfte Diensten, Datasets und die Pipeline) erstellt. Bei Verwendung von Tools und APIs (mit Ausnahme der .NET-API) definieren Sie diese Data Factory-Entitäten im JSON-Format.  Beispiele mit JSON-Definitionen für Data Factory-Entitäten für das Kopieren von Daten in und aus Azure Blob Storage finden Sie in diesem Artikel im Abschnitt [JSON-Beispiele](#json-examples). 

Die folgenden Abschnitte enthalten Details zu JSON-Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Azure Blob Storage verwendet werden: 

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts
Es gibt zwei Arten von verknüpften Diensten, die Sie verwenden können, um einen Azure Storage mit einer Azure Data Factory zu verknüpfen. **AzureStorage** und **AzureStorageSas**. Dagegen bietet der mit Azure Storage SAS (Shared Access Signature) verknüpfte Dienst der Data Factory einen eingeschränkten bzw. zeitgebundenen Zugriff auf Azure-Speicher. Es gibt keine weitere Unterschiede zwischen diesen beiden verknüpften Diensten. Wählen Sie den verknüpften Dienst, der Ihren Anforderungen entspricht. Die folgenden Abschnitte bieten weitere Informationen zu diesen beiden verknüpften Diensten.

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Dataset-Eigenschaften
Um ein Dataset zur Darstellung von Eingabe- oder Ausgabedaten in einem Azure Blob Storage anzugeben, legen Sie die Typeigenschaft des Datasets auf **AzureBlob** fest. Legen Sie die **linkedServiceName**-Eigenschaft des Datasets auf den Namen des mit Azure Storage oder Azure Storage SAS verknüpften Diensts fest.  Die Typeigenschaften des Datasets geben den **Blobcontainer** und den **Ordner** im Blobspeicher an.

Eine vollständige Liste der JSON-Abschnitte und -Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel [Erstellen von Datasets](data-factory-create-datasets.md). Abschnitte wie „structure“, „availability“ und „policy“ des JSON-Codes eines Datasets sind bei allen Dataset-Typen (Azure SQL, Azure-Blob, Azure-Tabelle usw.) ähnlich.

Data Factory unterstützt für das Schema von Lesedatenquellen wie Azure-Blob die folgenden CLS-kompatiblen auf .NET basierenden Typwerte für die Bereitstellung von Typinformationen in „structure“: Int16, Int32, Int64, Single, Double, Decimal, Byte[], Bool, String, Guid, Datetime, Datetimeoffset, Timespan. Data Factory führt beim Verschieben von Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher automatisch Typkonvertierungen durch. 

Der Abschnitt **typeProperties** unterscheidet sich bei jeder Art von Dataset und enthält unter anderem Informationen zum Speicherort und Format der Daten im Datenspeicher. Der Abschnitt „typeProperties“ für ein Dataset des Typs **AzureBlob** hat die folgenden Eigenschaften:

| Eigenschaft | Beschreibung | Erforderlich |
| --- | --- | --- |
| folderPath |Der Pfad zum Container und Ordner im Blobspeicher. Beispiel: myblobcontainer\myblobfolder\ |Ja |
| fileName |Der Name des Blobs. fileName ist optional, wobei seine Groß- und Kleinschreibung beachtet werden muss.<br/><br/>Wenn Sie einen Dateinamen angeben, funktioniert die Aktivität (einschließlich Kopieren) für das jeweilige Blob.<br/><br/>Wenn „fileName“ nicht angegeben ist, werden alle Blobs in folderPath für das Eingabedataset kopiert.<br/><br/>Wenn „fileName“ für ein Ausgabedataset nicht angegeben ist, hat der Name der generierten Datei folgendes Format: Data.<Guid>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt). |Nein |
| partitionedBy |"partitionedBy" ist eine optionale Eigenschaft. "partitionedBy" kann genutzt werden, um einen dynamischen Wert für "folderPath" oder "fileName" für Zeitreihendaten anzugeben. Beispiel: "folderPath" kann für jedes stündliche Datenaufkommen parametrisiert werden. Im Abschnitt [Nutzen der Eigenschaft „partitionedBy“](#using-partitionedBy-property) finden Sie Details und Beispiele. |Nein |
| format | Die folgenden Formattypen werden unterstützt: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**. Sie müssen die **type** -Eigenschaft unter „format“ auf einen dieser Werte festlegen. Weitere Informationen finden Sie in den Abschnitten [Textformat](data-factory-supported-file-and-compression-formats.md#text-format), [JSON-Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro-Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc-Format](data-factory-supported-file-and-compression-formats.md#orc-format) und [Parquet-Format](data-factory-supported-file-and-compression-formats.md#parquet-format). <br><br> Wenn Sie **Dateien unverändert zwischen dateibasierten Speichern kopieren** möchten (binäre Kopie), können Sie den Formatabschnitt bei den Definitionen von Eingabe- und Ausgabedatasets überspringen. |Nein |
| Komprimierung | Geben Sie den Typ und den Grad der Komprimierung für die Daten an. Unterstützte Typen sind: **Gzip**, **Deflate**, **bzip2** und **ZipDeflate**. Unterstützte Grade sind: **Optimal** und **Schnellste**. Weitere Informationen finden Sie unter [Datei- und Komprimierungsformate in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nein |

### <a name="using-partitionedby-property"></a>Nutzen der partitionedBy-Eigenschaft
Wie im vorherigen Abschnitt erwähnt, können die **partitionedBy**-Eigenschaft, [Data Factory-Funktionen und Systemvariablen](data-factory-functions-variables.md) verwendet werden, um für Zeitreihendaten einen dynamischen Wert für folderPath und filename anzugeben.

In den Artikeln [Erstellen von Datasets](data-factory-create-datasets.md) und [Planung und Ausführung](data-factory-scheduling-and-execution.md) finden Sie weitere Informationen zu Zeitreihen-Datasets, Planung und Slices.

#### <a name="sample-1"></a>Beispiel 1

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

Im obigen Beispiel wird {Slice} durch den Wert der Data Factory-Systemvariablen SliceStart ersetzt, die im Format (JJJJMMTTHH) angegeben ist. "SliceStart" bezieht sich auf die Startzeit des Slices. "folderPath" unterscheidet sich für jeden Slice. Beispiel: "wikidatagateway/wikisampledataout/2014100103" oder "wikidatagateway/wikisampledataout/2014100104".

#### <a name="sample-2"></a>Beispiel 2

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

Im Beispiel oben werden Jahr, Monat, Tag und Uhrzeit von SliceStart in separate Variablen extrahiert, die von den Eigenschaften „folderPath“ und „fileName“ verwendet werden.

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität
Eine vollständige Liste der Abschnitte und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Erstellen von Pipelines](data-factory-create-pipelines.md). Eigenschaften wie Name, Beschreibung, Eingabe- und Ausgabedatasets und Richtlinien sind für alle Arten von Aktivitäten verfügbar. Eigenschaften im Abschnitt **typeProperties** der Aktivität können dagegen je nach Aktivitätstyp variieren. Für die Kopieraktivität variieren die Eigenschaften je nach Art der Quellen und Senken. Wenn Sie Daten aus Azure Blob Storage verschieben, legen Sie den Quelltyp in der Kopieraktivität auf **BlobSource**fest. Wenn Sie hingegen Daten in Azure Blob Storage verschieben, legen Sie den Senkentyp in der Kopieraktivität auf **BlobSink**fest. Dieser Abschnitt enthält eine Liste der Eigenschaften, die von „BlobSource“ und „BlobSink“ unterstützt werden.

**BlobSource** unterstützt die folgenden Eigenschaften im Abschnitt **typeProperties**:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| recursive |Gibt an, ob die Daten rekursiv aus den Unterordnern oder nur aus dem angegebenen Ordner gelesen werden. |True (Standardwert), False |Nein |

**BlobSink** unterstützt die folgenden Eigenschaften im Abschnitt **typeProperties**:

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| copyBehavior |Definiert das Verhalten beim Kopieren, wenn die Quelle "BlobSource" oder "FileSystem" ist. |<b>PreserveHierarchy:</b> behält die Dateihierarchie im Zielordner bei. Der relative Pfad der Quelldatei zum Quellordner entspricht dem relativen Pfad der Zieldatei zum Zielordner.<br/><br/><b>FlattenHierarchy</b>: Alle Dateien aus dem Quellordner befinden sich in der ersten Ebene des Zielordners. Für die Zieldateien wird ein automatisch ein Name erzeugt. <br/><br/><b>MergeFiles</b> (Standardwert): Führt alle Dateien aus dem Quellordner in einer Datei zusammen. Wenn der Datei-/Blob-Name angegeben wurde, entspricht der Name dem angegebenen Namen, andernfalls dem automatisch generierten Dateinamen. |Nein |

**BlobSource** unterstützt darüber hinaus zum Zweck der Abwärtskompatibilität die beiden folgenden Eigenschaften:

* **treatEmptyAsNull**: Gibt an, ob Null oder eine leere Zeichenfolge als NULL-Wert behandelt wird.
* **skipHeaderLineCount** : Gibt an, wie viele Zeilen übersprungen werden müssen. Nur anwendbar, wenn das Eingabedataset „TextFormat“ verwendet.

**BlobSink** unterstützt zum Zweck der Abwärtskompatibilität die folgende Eigenschaft:

* **blobWriterAddHeader**: Gibt an, ob beim Schreiben in ein Ausgabedataset ein Header der Spaltendefinitionen hinzugefügt werden soll.

Datasets unterstützen jetzt die folgenden Eigenschaften, die die gleichen Funktionen implementieren: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.

Die folgende Tabelle enthält Anweisungen zur Verwendung der neuen Dataseteigenschaften anstelle dieser Blobquellen-/Senkeneigenschaften.

| Eigenschaft der Kopieraktivität | Dataseteigenschaft |
|:--- |:--- |
| „skipHeaderLineCount“ für „BlobSource“ |„skipLineCount“ und „firstRowAsHeader“. Zeilen werden zunächst übersprungen. Anschließend wird die erste Zeile als Header gelesen. |
| „treatEmptyAsNull“ für „BlobSource“ |„treatEmptyAsNull“ für Eingabedataset |
| „blobWriterAddHeader“ für „BlobSink“ |„firstRowAsHeader“ für Ausgabedataset |

Ausführliche Informationen zu diesen Eigenschaften finden Sie im Abschnitt [Angeben von „TextFormat“](data-factory-supported-file-and-compression-formats.md#text-format) .    

### <a name="recursive-and-copybehavior-examples"></a>Beispiele für "recursive" und "copyBehavior"
Dieser Abschnitt beschreibt das resultierende Verhalten des Kopiervorgangs für verschiedene Kombinationen von rekursiven und CopyBehavior-Werten.

| recursive | copyBehavior | Resultierendes Verhalten |
| --- | --- | --- |
| true |preserveHierarchy |Für den Quellordner „Ordner1“ mit der folgenden Struktur:  <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der gleichen Struktur erstellt wie die Quelle<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5 |
| true |flattenHierarchy |Für den Quellordner „Ordner1“ mit der folgenden Struktur:  <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei5 |
| true |mergeFiles |Für den Quellordner „Ordner1“ mit der folgenden Struktur:  <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp; Inhalte von Datei1 + Datei2 + Datei3 + Datei4 + Datei5 werden in einer Datei mit einem automatisch generierten Namen zusammengeführt. |
| false |preserveHierarchy |Für den Quellordner „Ordner1“ mit der folgenden Struktur:  <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/><br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |flattenHierarchy |Für den Quellordner „Ordner1“ mit der folgenden Struktur: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Automatisch generierter Name für Datei2<br/><br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |
| false |mergeFiles |Für den Quellordner „Ordner1“ mit der folgenden Struktur: <br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Datei2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Unterordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Datei5<br/><br/>wird der Zielordner „Ordner1“ mit der folgenden Struktur erstellt:<br/><br/>Ordner1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Inhalte von Datei1 + Datei2 werden zu einer Datei mit einem automatisch generierten Namen zusammengeführt. Automatisch generierter Name für Datei1<br/><br/>Unterordner1 mit Datei3, Datei4 und Datei5 wird nicht übernommen. |

## <a name="json-examples"></a>JSON-Beispiele
Die folgenden Beispiele zeigen JSON-Beispieldefinitionen, die Sie zum Erstellen einer Pipeline mit dem [Azure-Portal](data-factory-copy-activity-tutorial-using-azure-portal.md), mit [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) oder mit [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md) verwenden können. Sie zeigen Ihnen das Kopieren von Daten aus Azure-Blobspeicher in die Azure SQL-Datenbank (und umgekehrt). Daten können jedoch mithilfe der Kopieraktivität in Azure Data Factory **direkt** aus beliebigen Quellen in die [hier](data-factory-data-movement-activities.md#supported-data-stores-and-formats) aufgeführten Senken kopiert werden.

## <a name="example-copy-data-from-blob-storage-to-sql-database"></a>Beispiel: Kopieren von Daten aus Blob Storage in SQL-Datenbank
Dieses Beispiel zeigt Folgendes:

1. Einen verknüpften Dienst des Typs [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](#linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](#dataset-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [BlobSource](#copy-activity-properties) und [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties) verwendet

Im Beispiel werden Zeitreihendaten aus einem Azure-Blob stündlich in eine Azure SQL-Tabelle kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure SQL verknüpfter Dienst:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Mit Azure Storage verknüpfter Dienst:**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Azure Data Factory unterstützt zwei Arten von mit Azure Storage verknüpften Diensten: **AzureStorage** und **AzureStorageSas**. Für den ersten geben Sie die Verbindungszeichenfolge an, die den Kontoschlüssel enthält. Für den zweiten geben Sie den SAS-URI (Shared Access Signature) an. Weitere Informationen finden Sie unter [Verknüpfte Dienste](#linked-service-properties).  

**Azure-Blob-Eingabedataset:**

Daten werden stündlich aus einem neuen Blob übernommen ("frequency": "hour", "interval": 1). Ordnerpfad und Dateiname des Blobs werden basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Der Ordnerpfad verwendet die Bestandteile Jahr, Monat und Tag der Startzeit, und der Dateiname verwendet die Stunde der Startzeit. Die Festlegung von „external“ auf „true“ teilt dem Data Factory-Dienst mit, dass diese Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
**Azure SQL-Ausgabedataset:**

Das Beispiel kopiert Daten in eine Tabelle namens "MyTable" in einer Azure SQL-Datenbank. Erstellen Sie die Tabelle in der Azure SQL-Datenbank mit der gleichen Anzahl von Spalten, die Sie in der Blob-CSV-Datei erwarten. Neue Zeilen werden der Tabelle stündlich hinzugefügt.

```json
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
**Eine Kopieraktivität in einer Pipeline mit Blobquelle und SQL-Senke:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **BlobSource** und der Typ **sink** auf **SqlSink** festgelegt.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
## <a name="example-copy-data-from-azure-sql-to-azure-blob"></a>Beispiel: Kopieren von Daten aus Azure SQL nach Azure-Blob
Dieses Beispiel zeigt Folgendes:

1. Einen verknüpften Dienst des Typs [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties)
2. Einen verknüpften Dienst des Typs [AzureStorage](#linked-service-properties)
3. Ein [Eingabedataset](data-factory-create-datasets.md) des Typs [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties)
4. Ein [Ausgabedataset](data-factory-create-datasets.md) des Typs [AzureBlob](#dataset-properties)
5. Eine [Pipeline](data-factory-create-pipelines.md) mit Kopieraktivität, die [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) und [BlobSink](#copy-activity-properties) verwendet

Im Beispiel werden Zeitreihendaten aus einer Azure SQL-Tabelle stündlich in einen Azure-Blob kopiert. Die bei diesen Beispielen verwendeten JSON-Eigenschaften werden in den Abschnitten beschrieben, die auf die Beispiele folgen.

**Mit Azure SQL verknüpfter Dienst:**

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
**Mit Azure Storage verknüpfter Dienst:**

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Azure Data Factory unterstützt zwei Arten von mit Azure Storage verknüpften Diensten: **AzureStorage** und **AzureStorageSas**. Für den ersten geben Sie die Verbindungszeichenfolge an, die den Kontoschlüssel enthält. Für den zweiten geben Sie den SAS-URI (Shared Access Signature) an. Weitere Informationen finden Sie unter [Verknüpfte Dienste](#linked-service-properties).  

**Azure SQL-Eingabedataset:**

Im Beispiel wird vorausgesetzt, dass Sie die Tabelle "MyTable" in Azure SQL erstellt haben, die für Zeitreihendaten eine Spalte namens "timestampcolumn" enthält.

Durch Festlegen von „external“ auf „true“ wird dem Data Factory-Dienst mitgeteilt, dass die Tabelle für die Data Factory extern ist und nicht durch eine Aktivität in der Data Factory erzeugt wird.

```json
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Azure-Blob-Ausgabedataset:**

Daten werden stündlich in ein neues Blob geschrieben ("frequency": "hour", "interval": 1). Der Ordnerpfad des Blobs wird basierend auf der Startzeit des Slices, der verarbeitet wird, dynamisch ausgewertet. Im Ordnerpfad werden Jahr, Monat, Tag und die Stundenteile der Startzeit verwendet.

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**Eine Kopieraktivität in einer Pipeline mit einer SQL-Quelle und einer Blobsenke:**

Die Pipeline enthält eine Kopieraktivität, die für die Verwendung der Ein- und Ausgabedatasets und für eine stündliche Ausführung konfiguriert ist. In der JSON-Definition der Pipeline ist der Typ **source** auf **SqlSource** und der Typ **sink** auf **BlobSink** festgelegt. Die für die **SqlReaderQuery** -Eigenschaft angegebene SQL-Abfrage wählt die Daten der letzten Stunde zum Kopieren aus.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureSQLInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                },
                   "scheduler": {
                      "frequency": "Hour",
                      "interval": 1
                },
                "policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "OldestFirst",
                      "retry": 0,
                      "timeout": "01:00:00"
                }
              }
         ]
    }
}
```

> [!NOTE]
> Weitere Informationen zum Zuordnen von Spalten im Quelldataset zu Spalten im Senkendataset finden Sie unter [Zuordnen von Datasetspalten in Azure Data Factory](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Leistung und Optimierung
Der Artikel [Handbuch zur Leistung und Optimierung der Kopieraktivität](data-factory-copy-activity-performance.md) beschreibt wichtige Faktoren, die sich auf die Leistung der Datenverschiebung (Kopieraktivität) in Azure Data Factory auswirken, sowie verschiedene Möglichkeiten zur Leistungsoptimierung.

