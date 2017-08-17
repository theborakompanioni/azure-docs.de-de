---
title: "Fehlertoleranz der Kopieraktivität von Azure Data Factory – Überspringen inkompatibler Zeilen | Microsoft-Dokumentation"
description: "Hier finden Sie Informationen zur Fehlertoleranz durch Überspringen der inkompatiblen Zeilen beim Kopieren unter Verwendung von Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: d613537657af3bbe379a53e92532bf6b184d762b
ms.contentlocale: de-de
ms.lasthandoff: 07/20/2017

---
# <a name="copy-activity-fault-tolerance---skip-incompatible-rows"></a>Fehlertoleranz der Kopieraktivität – Überspringen inkompatibler Zeilen

Beim Kopieren von Daten zwischen Quell- und Senkendatenspeichern bietet die [Kopieraktivität](data-factory-data-movement-activities.md) verschiedene Optionen für den Umgang mit inkompatiblen Zeilen. Die Kopieraktivität kann entweder mit einem Fehler abgebrochen werden, wenn sie auf inkompatible Daten stößt (Standardverhalten), oder sie kann den Kopiervorgang für die übrigen Daten fortsetzen und die inkompatiblen Zeilen überspringen. Darüber hinaus können die inkompatiblen Zeilen in Azure Blob Storage protokolliert werden, damit Sie die Fehlerursache ermitteln, die Daten in der Datenquelle korrigieren und den Vorgang wiederholen können.

## <a name="supported-scenarios"></a>Unterstützte Szenarien
Die Kopieraktivität unterstützt momentan die Erkennung, Überspringung und Protokollierung der folgenden inkompatiblen Situationen beim Kopieren:

- **Datentypinkompatibilität zwischen nativen Quell- und Senkentypen**

    Beispiel: Sie möchten Daten aus einer CSV-Datei in Azure Blob Storage an Azure SQL-Datenbank kopieren, und das in Azure SQL-Datenbank definierte Schema enthält drei Spalten vom Typ *INT*. In diesem Fall werden Zeilen mit numerischen Daten (etwa `123,456,789`) erfolgreich aus der CSV-Quelldatei kopiert. Zeilen mit nicht numerischen Wert (etwa `123,456,abc`) werden dagegen als nicht kompatible Zeilen übersprungen.

- **Nicht übereinstimmende Spaltenanzahl zwischen Quelle und Senke**

    Beispiel: Sie möchten Daten aus einer CSV-Datei in Azure Blob Storage an Azure SQL-Datenbank kopieren, und das in SQL Azure definierte Schema umfasst sechs Spalten. In diesem Fall werden Zeilen mit sechs Spalten erfolgreich aus der CSV-Quelldatei kopiert. Zeilen mit einer anderen Spaltenanzahl werden dagegen als nicht kompatible Zeilen übersprungen.

- **Primärschlüsselverletzung beim Schreiben in die relationale Datenbank**

    Beispiel: Sie möchten Daten aus SQL Server in Azure SQL-Datenbank kopieren, und in der Senke (Azure SQL-Datenbank) ist ein Primärschlüssel definiert, in der Quelle (SQL Server) jedoch nicht. Die doppelten Zeilen, die in der Quelle vorhanden sein können, sind beim Schreiben in die Senke nicht zulässig. In diesem Fall kopiert die Kopieraktivität nur die erste Zeile in die Senke und überspringt alle weiteren Zeilen mit doppeltem Primärschlüsselwert aus der Quelle.

## <a name="configuration"></a>Konfiguration
Das folgende Beispiel bietet eine JSON-Definition zum Konfigurieren des Überspringens inkompatibler Datenzeilen in der Kopieraktivität:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Eigenschaft | Beschreibung | Zulässige Werte | Erforderlich |
| --- | --- | --- | --- |
| enableSkipIncompatibleRow | Gibt an, ob inkompatible Zeilen beim Kopieren übersprungen werden sollen. | true<br/>False (Standardwert) | Nein |
| redirectIncompatibleRowSettings | Eine Gruppe von Eigenschaften, die angegeben werden können, wenn Sie die inkompatiblen Zeilen protokollieren möchten. | &nbsp; | Nein |
| linkedServiceName | Der verknüpfte Azure Storage-Dienst zum Speichern des Protokolls mit allen übersprungenen Zeilen. | Geben Sie den Namen eines verknüpften Diensts vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) oder [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) an, um auf die Storage-Instanz zu verweisen, in der die Protokolldatei gespeichert wird. | Nein |
| path | Der Pfad der Protokolldatei mit allen übersprungenen Zeilen. | Geben Sie den gewünschten Blob Storage-Pfad für die Protokollierung der inkompatiblen Daten an. Wenn Sie keinen Pfad angeben, erstellt der Dienst automatisch einen Container. | Nein |

## <a name="monitoring"></a>Überwachung
Nach Abschluss der Kopieraktivität wird die Anzahl übersprungener Zeilen im Überwachungsabschnitt angezeigt:

![Überwachung des Überspringens inkompatibler Zeilen](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Wenn Sie das Protokollieren inkompatibler Zeilen konfiguriert haben, können Sie anhand der Protokolldatei unter `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` ermitteln, was übersprungen wurde und aus welchem Grund.

In der Datei werden sowohl die ursprünglichen Daten als auch der entsprechende Fehler protokolliert. Der Inhalt der Protokolldatei kann beispielsweise wie folgt aussehen:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).,
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Kopieraktivität von Azure Data Factory finden Sie unter [Verschieben von Daten mit der Kopieraktivität](data-factory-data-movement-activities.md).
