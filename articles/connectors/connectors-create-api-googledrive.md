---
title: "Hinzufügen des Google Drive-Connectors in Logik-Apps | Microsoft Docs"
description: "Übersicht über den Google Drive-Connector mit REST-API-Parametern"
services: 
suite: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2bcebc5-02d2-435b-b0da-ef53bc51c4b6
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 71f9dd111ebdbe885f33d162b2ea320dfaa167bb
ms.openlocfilehash: 5738c78d65ed0394028b90d634e5ebd4b1d5c5bc


---
# <a name="get-started-with-the-google-drive-connector"></a>Erste Schritte mit dem Google Drive-Connector
Verbinden Sie sich mit Google Drive, um Dateien zu erstellen, Zeilen abzurufen usw. Google Drive ermöglicht Folgendes: 

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die aus einer Suche abgerufen werden. 
* Verwenden von Aktionen, um Bilder, Nachrichten und mehr zu suchen. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Sie können z. B. ein Video suchen und dann Twitter verwenden, um das Video in einem Twitter-Feed zu posten.

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Google Drive bietet die folgenden Aktionen. Es gibt keine Trigger. 

| Trigger | Aktionen |
| --- | --- |
| Keine |<ul><li>Datei erstellen</li><li>Zeile einfügen</li><li>Datei kopieren</li><li>Datei löschen</li><li>Zeile löschen</li><li>Archiv in Ordner extrahieren</li><li>Dateiinhalt anhand der ID abrufen</li><li>Dateiinhalt anhand des Pfads abrufen</li><li>Dateimetadaten anhand der ID abrufen</li><li>Dateimetadaten anhand des Pfads abrufen</li><li>Zeile abrufen</li><li>Datei aktualisieren</li><li>Zeile aktualisieren</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## <a name="create-the-connection-to-google-drive"></a>Herstellen der Verbindung mit Google Drive
Wenn Sie diesen Connector Ihren Logik-Apps hinzufügen, müssen Sie ihnen das Herstellen einer Verbindung mit Ihrem Google Drive erlauben.

> [!INCLUDE [Steps to create a connection to googledrive](../../includes/connectors-create-api-googledrive.md)]
> 
> 

Nachdem Sie eine Verbindung hergestellt haben, geben Sie die Google Drive-Eigenschaften ein, z. B. Ordnerpfad oder Dateiname. In der **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

> [!TIP]
> Sie können dieselbe Google Drive-Verbindung in anderen Logik-Apps verwenden.
> 
> 

## <a name="swagger-rest-api-reference"></a>Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### <a name="create-file"></a>Datei erstellen
Lädt eine Datei in Google Drive hoch.  
```POST: /datasets/default/files```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| folderPath |string |Ja |query |Keine |Ordnerpfad zum Hochladen der Datei in Google Drive |
| Name |string |Ja |query |Keine |Name der Datei, die in Google Drive erstellt werden soll |
| body |string(binary) |Ja |body |Keine |Inhalt der Datei, die in Google Drive hochgeladen werden soll |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="insert-row"></a>Zeile einfügen
Fügt eine Zeile in eine Google-Tabelle ein.  
```POST: /datasets/{dataset}/tables/{table}/items```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |Keine |Eindeutiger Bezeichner der Datei mit der Google-Tabelle |
| Tabelle |string |Ja |path |Keine |Eindeutige ID des Arbeitsblatts |
| item |ItemInternalId: string |Ja |body |Keine |In der angegebenen Tabelle einzufügende Zeile |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="copy-file"></a>Datei kopieren
Kopiert eine Datei in Google Drive.  
```POST: /datasets/default/copyFile```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Quelle |string |Ja |query |(Keine) |URL zur Quelldatei |
| Ziel |string |Ja |query |Keine |Zieldateipfad in Google Drive, einschließlich Zieldateiname |
| overwrite |Boolescher Wert |no |query |(Keine) |Überschreibt die Zieldatei, falls auf „True“ festgelegt |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="delete-file"></a>Datei löschen
Löscht eine Datei aus Google Drive.  
```DELETE: /datasets/default/files/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der aus Google Drive zu löschenden Datei |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="delete-row"></a>Zeile löschen
Löscht eine Zeile aus einer Google-Tabelle.  
```DELETE: /datasets/{dataset}/tables/{table}/items/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |Keine |Eindeutiger Bezeichner der Datei mit der Google-Tabelle |
| Tabelle |string |Ja |path |Keine |Eindeutige ID des Arbeitsblatts |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der zu löschenden Zeile |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="extract-archive-to-folder"></a>Archiv in Ordner extrahieren
Extrahiert eine Archivdatei in einen Ordner in Google Drive (Beispiel: ZIP).  
```POST: /datasets/default/extractFolderV2```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| Quelle |string |Ja |query |(Keine) |Pfad zur Archivdatei |
| Ziel |string |Ja |query |Keine |Pfad in Google Drive, in den der Archivinhalt extrahiert wird |
| overwrite |Boolescher Wert |no |query |(Keine) |Überschreibt die Zieldateien, falls auf „True“ festgelegt |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-file-content-using-id"></a>Dateiinhalt anhand der ID abrufen
Ruft den Inhalt der Datei anhand der ID aus Google Drive ab.  
```GET: /datasets/default/files/{id}/content```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der aus Google Drive abzurufenden Datei |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-file-content-using-path"></a>Dateiinhalt anhand des Pfads abrufen
Ruft den Inhalt der Datei anhand des Pfads aus Google Drive ab.  
```GET: /datasets/default/GetFileContentByPath```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| path |string |Ja |query |Keine |Pfad der Datei in Google Drive |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-file-metadata-using-id"></a>Dateimetadaten anhand der ID abrufen
Ruft Dateimetadaten aus Google Drive anhand der ID ab.  
```GET: /datasets/default/files/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der Datei in Google Drive |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-file-metadata-using-path"></a>Dateimetadaten anhand des Pfads abrufen
Ruft Dateimetadaten anhand des Pfads aus Google Drive ab.  
```GET: /datasets/default/GetFileByPath```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| path |string |Ja |query |Keine |Pfad der Datei in Google Drive |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-row"></a>Zeile abrufen
Ruft eine einzelne Zeile aus einer Google-Tabelle ab.  
```GET: /datasets/{dataset}/tables/{table}/items/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |Keine |Eindeutiger Bezeichner der Datei mit der Google-Tabelle |
| Tabelle |string |Ja |path |Keine |Eindeutige ID des Arbeitsblatts |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der abzurufenden Zeile |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="update-file"></a>Datei aktualisieren
Aktualisiert eine Datei in Google Drive.  
```PUT: /datasets/default/files/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der in Google Drive zu aktualisierenden Datei |
| body |string(binary) |Ja |body |Keine |Inhalt der Datei, die in Google Drive hochgeladen werden soll |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="update-row"></a>Zeile aktualisieren
Aktualisiert eine Zeile in einer Google-Tabelle.  
```PATCH: /datasets/{dataset}/tables/{table}/items/{id}```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |Keine |Eindeutiger Bezeichner der Datei mit der Google-Tabelle |
| Tabelle |string |Ja |path |Keine |Eindeutige ID des Arbeitsblatts |
| id |string |Ja |path |Keine |Eindeutiger Bezeichner der zu aktualisierenden Zeile |
| item |ItemInternalId: string |Ja |body |Keine |Zeile mit aktualisierten Werten |

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

#### <a name="tablemetadata"></a>TableMetadata
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |no |
| title |string |no |
| x-ms-permission |string |no |
| schema |nicht definiert |no |

#### <a name="tableslist"></a>TablesList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |no |

#### <a name="table"></a>Tabelle
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Name |string |no |
| displayName |string |no |

#### <a name="item"></a>item
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| ItemInternalId |string |no |

#### <a name="itemslist"></a>ItemsList
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| value |array |no |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

Gehen Sie zur [Liste der APIs](apis-list.md)zurück.

<!--References-->
[5]: https://console.developers.google.com/
[6]: ./media/connectors-create-api-googledrive/google-developers-console.png
[8]: ./media/connectors-create-api-googledrive/use-google-apis.png
[9]: ./media/connectors-create-api-googledrive/googledrive-api-overview.png
[10]: ./media/connectors-create-api-googledrive/enable-googledrive-api.png
[12]: ./media/connectors-create-api-googledrive/googledrive-api-credentials-add.png
[13]: ./media/connectors-create-api-googledrive/configure-consent-screen.png
[14]: ./media/connectors-create-api-googledrive/create-client-id.png



<!--HONumber=Nov16_HO3-->


