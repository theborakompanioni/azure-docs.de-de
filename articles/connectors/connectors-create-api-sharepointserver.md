---
title: Verwenden des SharePoint Online-Connectors in Ihren Logik-Apps | Microsoft Docs
description: Erste Schritte mit dem SharePoint Online-Connector in Ihren Logik-Apps
services: 
documentationcenter: 
author: msftman
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 9c74b25a2ac5e2088a841d97920035376b7f3f11
ms.openlocfilehash: c462c98e2ecef1afe62ef921265b8842b6ae717d


---
# <a name="get-started-with-the-sharepoint-online-connector"></a>Erste Schritte mit dem SharePoint Online-Connector
Der SharePoint-Connector bietet eine Methode zum Arbeiten mit Listen in SharePoint.

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="lets-talk-about-triggers-and-actions"></a>Trigger und Aktionen
Der SharePoint-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format. 

Der SharePoint-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### <a name="sharepoint-actions"></a>SharePoint-Aktionen
Sie können diese Aktionen ausführen:

| Aktion | Beschreibung |
| --- | --- |
| GetFileMetadata |Dient zum Abrufen der Metadaten einer Datei in der Dokumentbibliothek |
| UpdateFile |Dient zum Aktualisieren einer Datei in der Dokumentbibliothek |
| DeleteFile |Dient zum Löschen einer Datei aus der Dokumentbibliothek |
| GetFileMetadataByPath |Dient zum Abrufen der Metadaten einer Datei in der Dokumentbibliothek |
| GetFileContentByPath |Dient zum Abrufen einer Datei aus der Dokumentbibliothek |
| GetFileContent |Dient zum Abrufen einer Datei aus der Dokumentbibliothek |
| CreateFile |Dient zum Hochladen einer Datei in die Dokumentbibliothek |
| CopyFile |Dient zum Kopieren einer Datei in der Dokumentbibliothek |
| ExtractFolderV2 |Dient zum Extrahieren eines Ordners in der Dokumentbibliothek |
| PostItem |Erstellt ein neues Element in einer SharePoint-Liste |
| GetItem |Ruft ein einzelnes Element aus einer SharePoint-Liste ab |
| DeleteItem |Löscht ein Element aus einer SharePoint-Liste |
| PatchItem |Aktualisiert ein Element in einer SharePoint-Liste |

### <a name="sharepoint-triggers"></a>SharePoint-Trigger
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| OnNewFile |Löst einen Datenfluss aus, wenn in einem SharePoint-Ordner eine neue Datei erstellt wird |
| OnUpdatedFile |Löst einen Datenfluss aus, wenn in einem SharePoint-Ordner eine Datei geändert wird |
| GetOnNewItems |Wenn ein neues Element in einer SharePoint-Liste erstellt wird |
| GetOnUpdatedItems |Wenn ein vorhandenes Element in einer SharePoint-Liste geändert wird |

## <a name="create-a-connection-to-sharepoint"></a>Herstellen einer Verbindung mit SharePoint
Erstellen Sie zum Verwenden des SharePoint-Connectors zunächst eine **Verbindung** , und geben Sie anschließend die Details für die folgenden Eigenschaften an: 

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Tokenverschlüsselung |Ja |Bereitstellen von SharePoint-Anmeldeinformationen |

Um eine Verbindung mit **SharePoint Online** herzustellen, müssen Sie Ihre Identität (Benutzername und Kennwort, Smartcard-Anmeldeinformationen usw.) für SharePoint Online bereitstellen. Nach der Authentifizierung können Sie den SharePoint Online-Connector in Ihrer Logik-App verwenden. 

Führen Sie im Designer der Logik-App die folgenden Schritte durch, um sich bei SharePoint anzumelden und die **Verbindung** zu erstellen, die Sie in Ihrer Logik-App verwenden können:

1. Geben Sie in das Suchfeld „SharePoint“ ein, und warten Sie, bis die Suche alle Einträge mit „SharePoint“ im Namen zurückgibt:    
   ![SharePoint konfigurieren][1]  
2. Wählen Sie **SharePoint Online – Wenn eine Datei erstellt wird**   
3. Wählen Sie **Sign in to SharePoint Online** (Bei SharePoint Online anmelden) aus:   
   ![SharePoint konfigurieren][2]    
4. Geben Sie Ihre SharePoint-Anmeldeinformationen ein, um sich bei SharePoint zu authentifizieren.   
   ![SharePoint konfigurieren][3]     
5. Nach Abschluss der Authentifizierung werden Sie zu Ihrer Logik-App umgeleitet, damit Sie diese vervollständigen können. Dazu konfigurieren Sie das SharePoint-Dialogfeld **Wenn eine Datei erstellt wird**.          
   ![SharePoint konfigurieren][4]  
6. Sie können dann weitere Trigger und Aktionen hinzufügen, die Sie benötigen, um Ihre Logik-App abzuschließen.   
7. Wählen Sie auf der oberen Menüleiste die Option **Speichern** aus, um Ihre Arbeit zu speichern.  

## <a name="sharepoint-rest-api-reference"></a>REST-API-Referenz für SharePoint
#### <a name="this-documentation-is-for-version-10"></a>Diese Dokumentation gilt für Version 1.0.
### <a name="used-for-getting-a-file-metadata-on-document-library"></a>Dient zum Abrufen der Metadaten einer Datei in der Dokumentbibliothek
**```GET: /datasets/{dataset}/files/{id}```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner der Datei |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="used-for-updating-a-file-on-document-library"></a>Dient zum Aktualisieren einer Datei in der Dokumentbibliothek
**```PUT: /datasets/{dataset}/files/{id}```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner der Datei |
| body | |Ja |body |(Keine) |Der Inhalt der Datei |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="used-for-deleting-a-file-on-document-library"></a>Dient zum Löschen einer Datei aus der Dokumentbibliothek
**```DELETE: /datasets/{dataset}/files/{id}```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner der Datei |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="used-for-getting-a-file-metadata-on-document-library"></a>Dient zum Abrufen der Metadaten einer Datei in der Dokumentbibliothek
**```GET: /datasets/{dataset}/GetFileByPath```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| path |string |Ja |query |(Keine) |Pfad der Datei |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="used-for-getting-a-file-on-document-library"></a>Dient zum Abrufen einer Datei aus der Dokumentbibliothek
**```GET: /datasets/{dataset}/GetFileContentByPath```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| path |string |Ja |query |(Keine) |Pfad der Datei |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="used-for-getting-a-file-on-document-library"></a>Dient zum Abrufen einer Datei aus der Dokumentbibliothek
**```GET: /datasets/{dataset}/files/{id}/content```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner der Datei |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="used-for-uploading-a-file-on-document-library"></a>Dient zum Hochladen einer Datei in die Dokumentbibliothek
**```POST: /datasets/{dataset}/files```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| folderPath |string |Ja |query |(Keine) |Der Pfad zum Ordner |
| Name |string |Ja |query |(Keine) |Name der Datei |
| body | |Ja |body |(Keine) |Der Inhalt der Datei |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="used-for-copying-a-file-on-document-library"></a>Dient zum Kopieren einer Datei in der Dokumentbibliothek
**```POST: /datasets/{dataset}/copyFile```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| Quelle |string |Ja |query |(Keine) |Pfad zur Quelldatei |
| Ziel |string |Ja |query |(Keine) |Pfad zur Zieldatei |
| overwrite |Boolescher Wert |no |query |false |Soll die vorhandene Datei überschrieben werden oder nicht? |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="triggers-a-flow-when-a-new-file-is-created-in-a-sharepoint-folder"></a>Löst einen Datenfluss aus, wenn in einem SharePoint-Ordner eine neue Datei erstellt wird
**```GET: /datasets/{dataset}/triggers/onnewfile```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL |
| folderId |string |Ja |query |(Keine) |Eindeutiger Bezeichner des Ordners in SharePoint |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="triggers-a-flow-when-a-file-is-modified-in-a-sharepoint-folder"></a>Löst einen Datenfluss aus, wenn in einem SharePoint-Ordner eine Datei geändert wird
**```GET: /datasets/{dataset}/triggers/onupdatedfile```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL |
| folderId |string |Ja |query |(Keine) |Eindeutiger Bezeichner des Ordners in SharePoint |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="used-for-extracting-a-folder-on-document-library"></a>Dient zum Extrahieren eines Ordners in der Dokumentbibliothek
**```POST: /datasets/{dataset}/extractFolderV2```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| Quelle |string |Ja |query |(Keine) |Pfad zur Quelldatei |
| Ziel |string |Ja |query |(Keine) |Pfad zum Zielordner |
| overwrite |Boolescher Wert |no |query |false |Soll die vorhandene Datei überschrieben werden oder nicht? |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="when-a-new-item-is-created-in-a-sharepoint-list"></a>Wenn ein neues Element in einer SharePoint-Liste erstellt wird
**```GET: /datasets/{dataset}/tables/{table}/onnewitems```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| Tabelle |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| $skip |integer |no |query |Keine |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |integer |no |query |Keine |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |
| $filter |string |no |query |Keine |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |string |no |query |Keine |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="when-an-existing-item-is-modified-in-a-sharepoint-list"></a>Wenn ein vorhandenes Element in einer SharePoint-Liste geändert wird
**```GET: /datasets/{dataset}/tables/{table}/onupdateditems```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| Tabelle |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| $skip |integer |no |query |Keine |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |integer |no |query |Keine |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |
| $filter |string |no |query |Keine |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |string |no |query |Keine |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="creates-a-new-item-in-a-sharepoint-list"></a>Erstellt ein neues Element in einer SharePoint-Liste
**```POST: /datasets/{dataset}/tables/{table}/items```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| Tabelle |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| item | |Ja |body |(Keine) |Zu erstellendes Element |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="retrieves-a-single-item-from-a-sharepoint-list"></a>Ruft ein einzelnes Element aus einer SharePoint-Liste ab
**```GET: /datasets/{dataset}/tables/{table}/items/{id}```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| Tabelle |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| id |integer |Ja |path |(Keine) |Eindeutiger Bezeichner des abzurufenden Elements |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="deletes-an-item-from-a-sharepoint-list"></a>Löscht ein Element aus einer SharePoint-Liste
**```DELETE: /datasets/{dataset}/tables/{table}/items/{id}```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| Tabelle |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| id |integer |Ja |path |(Keine) |Eindeutiger Bezeichner des zu löschenden Elements |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### <a name="updates-an-item-in-a-sharepoint-list"></a>Aktualisiert ein Element in einer SharePoint-Liste
**```PATCH: /datasets/{dataset}/tables/{table}/items/{id}```** 

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |URL der SharePoint-Website (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| Tabelle |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| id |integer |Ja |path |(Keine) |Eindeutiger Bezeichner des zu aktualisierenden Elements |
| item | |Ja |body |(Keine) |Element mit geänderten Eigenschaften |

### <a name="here-are-the-possible-responses"></a>Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
## <a name="object-definitions"></a>Objektdefinitionen:
 **DataSetsMetadata:**

Erforderliche Eigenschaften für DataSetsMetadata:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| tabular |nicht definiert |
| Blob |nicht definiert |

 **TabularDataSetsMetadata:**

Erforderliche Eigenschaften für TabularDataSetsMetadata:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Quelle |string |
| displayName |string |
| urlEncoding |string |
| tableDisplayName |string |
| tablePluralName |string |

 **BlobDataSetsMetadata:**

Erforderliche Eigenschaften für BlobDataSetsMetadata:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Quelle |string |
| displayName |string |
| urlEncoding |string |

 **BlobMetadata:**

Erforderliche Eigenschaften für BlobMetadata:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| id |string |
| Name |string |
| displayName |string |
| path |string |
| LastModified |string |
| Größe |integer |
| MediaType |string |
| IsFolder |Boolescher Wert |
| ETag |string |
| FileLocator |string |

 **Object:**

Erforderliche Eigenschaften für „Object“:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
|  | |

 **TableMetadata:**

Erforderliche Eigenschaften für TableMetadata:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Name |string |
| title |string |
| x-ms-permission |string |
| schema |nicht definiert |

 **DataSetsList:**

Erforderliche Eigenschaften für DataSetsList:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| value |array |

 **DataSet:**

Erforderliche Eigenschaften für DataSet:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Name |string |
| displayName |string |

 **Tabelle**:

Erforderliche Eigenschaften für „Table“:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| Name |string |
| displayName |string |

 **Item:**

Erforderliche Eigenschaften für „Item“:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| ItemInternalId |string |

 **ItemsList:**

Erforderliche Eigenschaften für ItemsList:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| value |array |

 **TablesList:**

Erforderliche Eigenschaften für TablesList:

Keine der Eigenschaften ist erforderlich. 

**Alle Eigenschaften:** 

| Name | Datentyp |
| --- | --- |
| value |array |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)  

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png



<!--HONumber=Jan17_HO3-->


