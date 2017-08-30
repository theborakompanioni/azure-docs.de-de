---
title: "Hinzufügen von Fehlertoleranz der Kopieraktivität von Azure Data Factory durch Überspringen inkompatibler Zeilen | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Fehlertoleranz der Kopieraktivität von Azure Data Factory durch Überspringen inkompatibler Zeilen hinzufügen."
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
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: e2a108752259d5da3b401666c6bdbaad13b7ea90
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Hinzufügen von Fehlertoleranz der Kopieraktivität durch Überspringen inkompatibler Zeilen

Die [Kopieraktivität](data-factory-data-movement-activities.md) von Azure Data Factory bietet beim Kopieren von Daten zwischen Quell- und Senkendatenspeichern zwei Optionen für den Umgang mit inkompatiblen Zeilen:

- Die Kopieraktivität kann mit einem Fehler abgebrochen werden, wenn sie auf inkompatible Daten stößt (Standardverhalten).
- Der Kopiervorgang kann für alle Daten fortgesetzt werden, indem Fehlertoleranz hinzugefügt wird und die inkompatiblen Datenzeilen übersprungen werden. Darüber hinaus können die inkompatiblen Zeilen in Azure Blob Storage protokolliert werden. Sie können dann das Protokoll überprüfen, um die Ursache des Fehlers zu ermitteln, die Daten in der Datenquelle korrigieren und die Kopieraktivität wiederholen.

## <a name="supported-scenarios"></a>Unterstützte Szenarien
Die Kopieraktivität unterstützt drei Szenarien zum Erkennen, Überspringen und Protokollieren inkompatibler Daten:

- **Inkompatibilität zwischen dem Quelldatentyp und dem nativen Senkentyp**

    Beispiel: Kopieren von Daten aus einer CSV-Datei in Blob Storage in eine SQL-Datenbank mit einer Schemadefinition, die drei Spalten vom Typ **INT** enthält. Die Zeilen der CSV-Datei, die numerische Daten wie z.B. `123,456,789` enthalten, werden erfolgreich in den Senkenspeicher kopiert. Die Zeilen mit nicht numerischen Werten, z.B. `123,456,abc`, werden dagegen als inkompatibel erkannt und übersprungen.

- **Fehlende Übereinstimmung bei der Anzahl der Spalten zwischen der Quelle und der Senke**

    Beispiel: Kopieren von Daten aus einer CSV-Datei in Blob Storage in eine SQL-Datenbank mit einer Schemadefinition, die sechs Spalten enthält. Die Zeilen der CSV-Datei, die sechs Spalten enthalten, werden erfolgreich in den Senkenspeicher kopiert. Die Zeilen der CSV-Datei mit weniger oder mehr als sechs Spalten werden als inkompatibel erkannt und übersprungen.

- **Primärschlüsselverletzung beim Schreiben in eine relationale Datenbank**

    Beispiel: Kopieren von Daten von einer SQL Server-Instanz in eine SQL-Datenbank. In der SQL-Datenbank der Senke ist ein Primärschlüssel definiert, in der SQL Server-Instanz der Quelle ist dagegen kein Primärschlüssel definiert. Die doppelten Zeilen, die in der Quelle vorhanden sind, können nicht in die Senke kopiert werden. Die Kopieraktivität kopiert nur die erste Zeile der Quelldaten in die Senke. Die nachfolgenden Quellzeilen, die den doppelten Primärschlüsselwert enthalten, werden als inkompatibel erkannt und übersprungen.

## <a name="configuration"></a>Konfiguration
Das folgende Beispiel umfasst eine JSON-Definition zum Konfigurieren des Überspringens inkompatibler Datenzeilen in der Kopieraktivität:

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
| **enableSkipIncompatibleRow** | Gibt an, ob inkompatible Zeilen beim Kopieren übersprungen werden sollen. | true<br/>False (Standardwert) | Nein |
| **redirectIncompatibleRowSettings** | Eine Gruppe von Eigenschaften, die angegeben werden können, wenn Sie die inkompatiblen Zeilen protokollieren möchten. | &nbsp; | Nein |
| **linkedServiceName** | Der verknüpfte Azure Storage-Dienst zum Speichern des Protokolls mit den übersprungenen Zeilen. | Der Name eines verknüpften Diensts vom Typ [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) oder [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service), der auf die Storage-Instanz verweist, in der die Protokolldatei gespeichert werden soll. | Nein |
| **path** | Der Pfad der Protokolldatei, die die übersprungenen Zeilen enthält. | Geben Sie den gewünschten Blob Storage-Pfad für die Protokollierung der inkompatiblen Daten an. Wenn Sie keinen Pfad angeben, erstellt der Dienst automatisch einen Container. | Nein |

## <a name="monitoring"></a>Überwachung
Nach Abschluss der Kopieraktivität wird die Anzahl übersprungener Zeilen im Überwachungsabschnitt angezeigt:

![Überwachen übersprungener inkompatibler Zeilen](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Wenn Sie das Protokollieren inkompatibler Zeilen konfiguriert haben, finden Sie die Protokolldatei unter folgendem Pfad: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` In der Protokolldatei können Sie die übersprungenen Zeilen und die Ursache der Inkompatibilität einsehen.

In der Datei werden sowohl die ursprünglichen Daten als auch der entsprechende Fehler protokolliert. Der Inhalt der Protokolldatei kann beispielsweise wie folgt aussehen:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Kopieraktivität von Azure Data Factory finden Sie unter [Verschieben von Daten mit der Kopieraktivität](data-factory-data-movement-activities.md).
