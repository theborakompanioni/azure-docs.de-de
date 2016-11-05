---
title: Verwenden des SharePoint Online-Connectors in Ihren Logik-Apps | Microsoft Docs
description: Erste Schritte mit dem SharePoint Online-Connector für Azure App Service in Ihren Logik-Apps.
services: ''
documentationcenter: ''
author: msftman
manager: erikre
editor: ''
tags: connectors

ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: deonhe

---
# Erste Schritte mit dem SharePoint Online-Connector
Der SharePoint-Connector bietet eine Methode zum Arbeiten mit Listen in SharePoint.

> [!NOTE]
> Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.
> 
> 

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Trigger und Aktionen
Der SharePoint-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

Der SharePoint-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### SharePoint-Aktionen
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

### SharePoint-Trigger
Sie können auf diese Ereignisse lauschen:

| Trigger | Beschreibung |
| --- | --- |
| OnNewFile |Löst einen Datenfluss aus, wenn in einem SharePoint-Ordner eine neue Datei erstellt wird |
| OnUpdatedFile |Löst einen Datenfluss aus, wenn in einem SharePoint-Ordner eine Datei geändert wird |
| GetOnNewItems |Wenn ein neues Element in einer SharePoint-Liste erstellt wird |
| GetOnUpdatedItems |Wenn ein vorhandenes Element in einer SharePoint-Liste geändert wird |

## Herstellen einer Verbindung mit SharePoint
Erstellen Sie zum Verwenden des SharePoint-Connectors zunächst eine **Verbindung**, und geben Sie anschließend die Details für die folgenden Eigenschaften an:

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Token |Ja |Bereitstellen von SharePoint-Anmeldeinformationen |

Um eine Verbindung mit **SharePoint Online** herzustellen, müssen Sie Ihre Identität (Benutzername und Kennwort, Smartcard-Anmeldeinformationen usw.) für SharePoint Online bereitstellen. Nachdem Sie authentifiziert wurden, können Sie den SharePoint Online-Connector in Ihrer Logik-App verwenden.

Führen Sie im Designer der Logik-App die folgenden Schritte aus, um sich bei SharePoint anzumelden und die **Verbindung** für Ihre Logik-App zu erstellen:

1. Geben Sie „SharePoint“ in das Suchfeld ein, und warten Sie, bis die Suche alle Einträge mit „SharePoint“ im Namen zurückgegeben hat: ![SharePoint konfigurieren][1]
2. Wählen Sie **SharePoint Online - When a file is created** (SharePoint Online – Wenn eine Datei erstellt wird) aus.
3. Wählen Sie **Sign in to SharePoint Online** (Bei SharePoint Online anmelden) aus: ![SharePoint konfigurieren][2]
4. Geben Sie Ihre SharePoint-Anmeldeinformationen ein, um sich bei SharePoint zu authentifizieren. ![SharePoint konfigurieren][3]
5. Nach Abschluss der Authentifizierung werden Sie zu Ihrer Logik-App umgeleitet, damit Sie diese vervollständigen können. Dazu konfigurieren Sie das SharePoint-Dialogfeld **When a file is created** (Wenn eine Datei erstellt wird). ![SharePoint konfigurieren][4]
6. Sie können dann weitere Trigger und Aktionen hinzufügen, die Sie benötigen, um Ihre Logik-App abzuschließen.
7. Wählen Sie auf der oberen Menüleiste die Option **Speichern** aus, um Ihre Arbeit zu speichern.

## REST-API-Referenz für SharePoint
#### Diese Dokumentation gilt für Version 1.0.
### Dient zum Abrufen der Metadaten einer Datei in der Dokumentbibliothek
**```GET: /datasets/{dataset}/files/{id}```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner der Datei |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Dient zum Aktualisieren einer Datei in der Dokumentbibliothek
**```PUT: /datasets/{dataset}/files/{id}```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner der Datei |
| body | |Ja |body |(Keine) |Der Inhalt der Datei |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Dient zum Löschen einer Datei aus der Dokumentbibliothek
**```DELETE: /datasets/{dataset}/files/{id}```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner der Datei |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Dient zum Abrufen der Metadaten einer Datei in der Dokumentbibliothek
**```GET: /datasets/{dataset}/GetFileByPath```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| path |string |Ja |query |(Keine) |Pfad der Datei |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Dient zum Abrufen einer Datei aus der Dokumentbibliothek
**```GET: /datasets/{dataset}/GetFileContentByPath```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| path |string |Ja |query |(Keine) |Pfad der Datei |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Dient zum Abrufen einer Datei aus der Dokumentbibliothek
**```GET: /datasets/{dataset}/files/{id}/content```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| id |string |Ja |path |(Keine) |Eindeutiger Bezeichner der Datei |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Dient zum Hochladen einer Datei in die Dokumentbibliothek
**```POST: /datasets/{dataset}/files```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| folderPath |string |Ja |query |(Keine) |Der Pfad zum Ordner |
| Name |string |Ja |query |(Keine) |Name der Datei |
| body | |Ja |body |(Keine) |Der Inhalt der Datei |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Dient zum Kopieren einer Datei in der Dokumentbibliothek
**```POST: /datasets/{dataset}/copyFile```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| source |string |Ja |query |(Keine) |Pfad zur Quelldatei |
| destination |string |Ja |query |(Keine) |Pfad zur Zieldatei |
| overwrite |Boolescher Wert |no |query |false |Soll die vorhandene Datei überschrieben werden oder nicht? |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Löst einen Datenfluss aus, wenn in einem SharePoint-Ordner eine neue Datei erstellt wird
**```GET: /datasets/{dataset}/triggers/onnewfile```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL |
| folderId |string |Ja |query |(Keine) |Eindeutiger Bezeichner des Ordners in SharePoint |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Löst einen Datenfluss aus, wenn in einem SharePoint-Ordner eine Datei geändert wird
**```GET: /datasets/{dataset}/triggers/onupdatedfile```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL |
| folderId |string |Ja |query |(Keine) |Eindeutiger Bezeichner des Ordners in SharePoint |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Dient zum Extrahieren eines Ordners in der Dokumentbibliothek
**```POST: /datasets/{dataset}/extractFolderV2```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL. Beispiel: http://contoso.sharepoint.com/sites/mysite |
| source |string |Ja |query |(Keine) |Pfad zur Quelldatei |
| destination |string |Ja |query |(Keine) |Pfad zum Zielordner |
| overwrite |Boolescher Wert |no |query |false |Soll die vorhandene Datei überschrieben werden oder nicht? |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Wenn ein neues Element in einer SharePoint-Liste erstellt wird
**```GET: /datasets/{dataset}/tables/{table}/onnewitems```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| $skip |integer |no |query |(Keine) |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |integer |no |query |(Keine) |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |
| $filter |string |no |query |(Keine) |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |string |no |query |(Keine) |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Wenn ein vorhandenes Element in einer SharePoint-Liste geändert wird
**```GET: /datasets/{dataset}/tables/{table}/onupdateditems```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| $skip |integer |no |query |(Keine) |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |integer |no |query |(Keine) |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |
| $filter |string |no |query |(Keine) |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |string |no |query |(Keine) |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Erstellt ein neues Element in einer SharePoint-Liste
**```POST: /datasets/{dataset}/tables/{table}/items```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| item | |Ja |body |(Keine) |Zu erstellendes Element |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Ruft ein einzelnes Element aus einer SharePoint-Liste ab
**```GET: /datasets/{dataset}/tables/{table}/items/{id}```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| id |integer |Ja |path |(Keine) |Eindeutiger Bezeichner des abzurufenden Elements |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Löscht ein Element aus einer SharePoint-Liste
**```DELETE: /datasets/{dataset}/tables/{table}/items/{id}```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| id |integer |Ja |path |(Keine) |Eindeutiger Bezeichner des zu löschenden Elements |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
### Aktualisiert ein Element in einer SharePoint-Liste
**```PATCH: /datasets/{dataset}/tables/{table}/items/{id}```**

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| dataset |string |Ja |path |(Keine) |SharePoint-Website-URL (Beispiel: http://contoso.sharepoint.com/sites/mysite) |
| table |string |Ja |path |(Keine) |Name der SharePoint-Liste |
| id |integer |Ja |path |(Keine) |Eindeutiger Bezeichner des zu aktualisierenden Elements |
| item | |Ja |body |(Keine) |Element mit geänderten Eigenschaften |

### Im Folgenden sind die möglichen Antworten aufgeführt:
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

- - -
## Objektdefinitionen:
 **DataSetsMetadata**:

Erforderliche Eigenschaften für DataSetsMetadata:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| tabular |nicht definiert |
| Blob |nicht definiert |

 **TabularDataSetsMetadata**:

Erforderliche Eigenschaften für TabularDataSetsMetadata:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| source |string |
| displayName |string |
| urlEncoding |string |
| tableDisplayName |string |
| tablePluralName |string |

 **BlobDataSetsMetadata**:

Erforderliche Eigenschaften für BlobDataSetsMetadata:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| source |string |
| displayName |string |
| urlEncoding |string |

 **BlobMetadata**:

Erforderliche Eigenschaften für BlobMetadata:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| ID |string |
| Name |string |
| DisplayName |string |
| Pfad |string |
| LastModified |string |
| Größe |integer |
| MediaType |string |
| IsFolder |Boolescher Wert |
| ETag |string |
| FileLocator |string |

 **Object**:

Erforderliche Eigenschaften für „Object“:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
|  | |

 **TableMetadata**:

Erforderliche Eigenschaften für TableMetadata:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| name |string |
| title |string |
| x-ms-permission |string |
| schema |nicht definiert |

 **DataSetsList**:

Erforderliche Eigenschaften für DataSetsList:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| value |array |

 **DataSet**:

Erforderliche Eigenschaften für DataSet:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| Name |string |
| DisplayName |string |

 **Tabelle**:

Erforderliche Eigenschaften für „Table“:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| Name |string |
| DisplayName |string |

 **Item**:

Erforderliche Eigenschaften für „Item“:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| ItemInternalId |string |

 **ItemsList**:

Erforderliche Eigenschaften für ItemsList:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| value |array |

 **TablesList**:

Erforderliche Eigenschaften für TablesList:

Keine der Eigenschaften ist erforderlich.

**Alle Eigenschaften**:

| Name | Datentyp |
| --- | --- |
| value |array |

## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png

<!---HONumber=AcomDC_0824_2016-->