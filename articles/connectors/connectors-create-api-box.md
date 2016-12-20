---
title: "Hinzufügen des Box-Connectors zu Ihren Logik-Apps | Microsoft Docs"
description: "Übersicht über den Box-Connector mit REST-API-Parametern"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 949579cf-a81c-4790-9ef5-fe39b4fbd0c5
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 71f9dd111ebdbe885f33d162b2ea320dfaa167bb
ms.openlocfilehash: e211d0878a7f9bd43c95c727935ad883fae0db3a


---
# <a name="get-started-with-the-box-connector"></a>Erste Schritte mit dem Box-Connector
Verbinden Sie sich mit Box, um Dateien zu erstellen, zu löschen usw. 

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Box ermöglicht Folgendes:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus Box abgerufen werden. 
* Verwenden von Triggern, wenn eine Datei erstellt oder aktualisiert wird.
* Verwenden von Aktionen, um eine Datei zu kopieren, zu löschen usw. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn z. B. in Box eine Datei geändert wird, können Sie diese Datei auswählen und mithilfe von Office 365 per E-Mail senden.

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Box weist die folgenden Trigger und Aktionen auf.

| Trigger | Aktionen |
| --- | --- |
| <ul><li>Wenn eine Datei erstellt wird</li><li>Wenn eine Datei geändert wird</li></ul> |<ul><li>Datei erstellen</li><li>Wenn eine Datei erstellt wird</li><li>Datei kopieren</li><li>Datei löschen</li><li>Archiv in Ordner extrahieren</li><li>Dateiinhalt anhand der ID abrufen</li><li>Dateiinhalt anhand des Pfads abrufen</li><li>Dateimetadaten anhand der ID abrufen</li><li>Dateimetadaten anhand des Pfads abrufen</li><li>Datei aktualisieren</li><li>Wenn eine Datei geändert wird</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## <a name="create-a-connection-to-box"></a>Herstellen einer Verbindung mit Box
Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, müssen Sie ihnen das Herstellen einer Verbindung mit Ihrer Box erlauben.

> [!INCLUDE [Steps to create a connection to box](../../includes/connectors-create-api-box.md)]
> 
> 

Nachdem Sie die Verbindung hergestellt haben, geben Sie die Box-Eigenschaften ein. In der **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

> [!TIP]
> Sie können dieselbe Box-Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="swagger-rest-api-reference"></a>Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### <a name="create-file"></a>Datei erstellen
Lädt eine Datei in Box hoch.  
```POST: /datasets/default/files```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| folderPath |string |Ja |query |Keine |Ordnerpfad zum Hochladen der Datei in Box |
| Name |string |Ja |query |Keine |Name der Datei, die in Box erstellt werden soll |
| body |string(binary) |Ja |body |Keine |Inhalt der Datei, die in Box hochgeladen werden soll |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="when-a-file-is-created"></a>Wenn eine Datei erstellt wird
Löst einen Datenfluss aus, wenn in einem Box-Ordner eine neue Datei erstellt wird.  
```GET: /datasets/default/triggers/onnewfile```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| folderId |string |Ja |query |Keine |Eindeutiger Bezeichner des Ordners in Box |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="copy-file"></a>Datei kopieren
Kopiert eine Datei in Box.  
```POST: /datasets/default/copyFile```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Quelle |string |Ja |query |Keine |URL zur Quelldatei |
| Ziel |string |Ja |query |Keine |Zieldateipfad in Box, einschließlich Zieldateiname |
| overwrite |Boolescher Wert |Nein |query |Keine |Überschreibt die Zieldatei, falls auf „True“ festgelegt |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="delete-file"></a>Datei löschen
Löscht eine Datei aus Box.  
```DELETE: /datasets/default/files/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der aus Box zu löschenden Datei |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="extract-archive-to-folder"></a>Archiv in Ordner extrahieren
Extrahiert eine Archivdatei in einen Ordner in Box (Beispiel: ZIP).  
```POST: /datasets/default/extractFolderV2```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Quelle |string |Ja |query | |Pfad zur Archivdatei |
| Ziel |string |Ja |query | |Pfad in Box, in den der Archivinhalt extrahiert wird |
| overwrite |Boolescher Wert |Nein |query | |Überschreibt die Zieldateien, falls auf „True“ festgelegt |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-file-content-using-id"></a>Dateiinhalt anhand der ID abrufen
Ruft den Dateiinhalt anhand der ID aus Box ab.  
```GET: /datasets/default/files/{id}/content```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der Datei in Box |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-file-content-using-path"></a>Dateiinhalt anhand des Pfads abrufen
Ruft den Dateiinhalt anhand des Pfads aus Box ab.  
```GET: /datasets/default/GetFileContentByPath```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| path |string |Ja |query |Keine |Eindeutiger Pfad zur Datei in Box |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-file-metadata-using-id"></a>Dateimetadaten anhand der ID abrufen
Ruft Dateimetadaten aus Box anhand der Datei-ID ab.  
```GET: /datasets/default/files/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der Datei in Box |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-file-metadata-using-path"></a>Dateimetadaten anhand des Pfads abrufen
Ruft Dateimetadaten aus Box anhand des Pfads ab.  
```GET: /datasets/default/GetFileByPath```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| path |string |Ja |query |Keine |Eindeutiger Pfad zur Datei in Box |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="update-file"></a>Datei aktualisieren
Aktualisiert eine Datei in Box.  
```PUT: /datasets/default/files/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der Datei, die in Box aktualisiert werden soll |
| body |string(binary) |Ja |body |Keine |Inhalt der Datei, die in Box aktualisiert werden soll |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="when-a-file-is-modified"></a>Wenn eine Datei geändert wird
Löst einen Datenfluss aus, wenn in einem Box-Ordner eine Datei geändert wird.  
```GET: /datasets/default/triggers/onupdatedfile```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| folderId |string |Ja |query |Keine |Eindeutiger Bezeichner des Ordners in Box |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="object-definitions"></a>Objektdefinitionen
#### <a name="datasetsmetadata"></a>DataSetsMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| tabular |nicht definiert |no |
| Blob |nicht definiert |no |

#### <a name="tabulardatasetsmetadata"></a>TabularDataSetsMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Quelle |string |no |
| displayName |string |no |
| urlEncoding |string |no |
| tableDisplayName |string |no |
| tablePluralName |string |no |

#### <a name="blobdatasetsmetadata"></a>BlobDataSetsMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Quelle |string |no |
| displayName |string |no |
| urlEncoding |string |no |

#### <a name="blobmetadata"></a>BlobMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| id |string |no |
| Name |string |no |
| displayName |string |no |
| path |string |no |
| LastModified |string |no |
| Größe |integer |no |
| MediaType |string |no |
| IsFolder |Boolescher Wert |no |
| ETag |string |no |
| FileLocator |string |no |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)




<!--HONumber=Nov16_HO3-->


