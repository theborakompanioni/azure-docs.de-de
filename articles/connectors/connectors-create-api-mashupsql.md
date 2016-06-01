<properties
pageTitle="SQL Connector | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. Der SQL-Connector stellt eine API zum Arbeiten mit SQL-Datenbanken bereit."
services="app-servicelogic"	
documentationCenter=".net,nodejs,java" 	
authors="msftman"	
manager="erikre"	
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="05/17/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem SQL-Connector



Der SQL-Connector kann verwendet werden in:

- [Logik-Apps](../app-service-logic/app-service-logic-what-are-logic-apps.md)  
- [PowerApps](http://powerapps.microsoft.com)  
- [Flow](http://flows.microsoft.com)  

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.

Erstellen Sie zu Beginn eine Logik-App, wie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Trigger und Aktionen

Der SQL-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der SQL-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### Aktionen des SQL-Connectors
Sie können diese Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|[GetTables](connectors-create-api-sqlconnector.md#gettables)|Ruft Tabellen aus einer SQL-Datenbank ab.|
|[PostItem](connectors-create-api-sqlconnector.md#postitem)|Fügt eine neue Zeile in eine SQL-Tabelle ein.|
|[GetItem](connectors-create-api-sqlconnector.md#getitem)|Ruft eine einzelne Zeile aus einer SQL-Tabelle ab.|
|[DeleteItem](connectors-create-api-sqlconnector.md#deleteitem)|Löscht eine Zeile aus einer SQL-Tabelle.|
|[PatchItem](connectors-create-api-sqlconnector.md#patchitem)|Aktualisiert eine vorhandene Zeile in einer SQL-Tabelle.|
### Trigger des SQL-Connectors
Sie können auf diese Ereignisse lauschen:

|Trigger | Beschreibung|
|--- | ---|


## Herstellen einer Verbindung mit dem SQL-Connector
Um Logik-Apps mit dem SQL-Connector zu erstellen, müssen Sie zuerst eine **Verbindung** herstellen und anschließend die Details für die folgenden Eigenschaften angeben:

|Eigenschaft| Erforderlich|Beschreibung|
| ---|---|---|
|SqlConnectionString|Ja|Angeben Ihrer SQL-Verbindungszeichenfolge|
Nachdem Sie die Verbindung erstellt haben, können Sie sie zum Ausführen der Aktionen und zum Lauschen auf die in diesem Artikel beschriebenen Trigger verwenden.

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit Mashup SQL](../../includes/connectors-create-api-mashupsql.md)]

>[AZURE.TIP] Sie können diese Verbindung in anderen Logik-Apps verwenden.

## Referenz für den SQL-Connector
Gilt für Version: 1.0.

## GetTables
Tabellen abrufen: Ruft Tabellen aus einer SQL-Datenbank ab.

```GET: /datasets/default/tables```

Es gibt keine Parameter für diesen Aufruf
#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## PostItem
Zeile einfügen: Fügt eine neue Zeile in eine SQL-Tabelle ein.

```POST: /datasets/default/tables/{table}/items```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Name der SQL-Tabelle|
|item| |Ja|body|(Keine)|In der angegebenen SQL-Tabelle einzufügende Zeile|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## GetItem
Zeile abrufen: Ruft eine einzelne Zeile aus einer SQL-Tabelle ab.

```GET: /datasets/default/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Name der SQL-Tabelle|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der abzurufenden Zeile|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## DeleteItem
Zeile löschen: Löscht eine Zeile aus einer SQL-Tabelle.

```DELETE: /datasets/default/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Name der SQL-Tabelle|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der zu löschenden Zeile|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## PatchItem
Zeile aktualisieren: Aktualisiert eine vorhandene Zeile in einer SQL-Tabelle.

```PATCH: /datasets/default/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Name der SQL-Tabelle|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der zu aktualisierenden Zeile|
|item| |Ja|body|(Keine)|Zeile mit aktualisierten Werten|

#### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


## Objektdefinitionen 

### DataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|tabular|nicht definiert|Nein |
|Blob|nicht definiert|Nein |



### TabularDataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|source|string|Nein |
|displayName|string|Nein |
|urlEncoding|string|Nein |
|tableDisplayName|string|Nein |
|tablePluralName|string|Nein |



### BlobDataSetsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|source|string|Nein |
|displayName|string|Nein |
|urlEncoding|string|Nein |



### TableMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|name|string|Nein |
|title|string|Nein |
|x-ms-permission|string|Nein |
|x-ms-capabilities|nicht definiert|Nein |
|schema|nicht definiert|Nein |



### TableCapabilitiesMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|sortRestrictions|nicht definiert|Nein |
|filterRestrictions|nicht definiert|Nein |
|filterFunctions|array|Nein |



### Objekt


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|



### TableSortRestrictionsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|sortable|Boolescher Wert|Nein |
|unsortableProperties|array|Nein |
|ascendingOnlyProperties|array|Nein |



### TableFilterRestrictionsMetadata


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|filterable|Boolescher Wert|Nein |
|nonFilterableProperties|array|Nein |
|requiredProperties|array|Nein |



### DataSetsList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|value|array|Nein |



### DataSet


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|string|Nein |
|DisplayName|string|Nein |



### Table


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|Name|string|Nein |
|DisplayName|string|Nein |



### Item


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|ItemInternalId|string|Nein |



### TablesList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|value|array|Nein |



### ItemsList


| Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|value|array|Nein |


## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0518_2016-->