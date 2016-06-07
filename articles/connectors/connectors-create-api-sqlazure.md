<properties
    pageTitle="Hinzufügen des SQL Azure-Connectors zu Ihren Logik-Apps | Microsoft Azure"
    description="Übersicht über den SQL Azure-Connector mit REST-API-Parametern"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="multiple"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="05/19/2016"
   ms.author="mandia"/>


# Erste Schritte mit dem SQL Azure-Connector
Stellen Sie eine Verbindung mit der Azure SQL-Datenbank her, um Tabellen und Zeilen zu verwalten, z.B. Einfügen von Zeilen, Abrufen von Tabellen usw.

Der Azure SQL-Datenbank-Connector kann verwendet werden in:

- Logik-Apps (in diesem Thema erläutert)
- PowerApps (eine vollständige Liste finden Sie in der [PowerApps-Verbindungsliste](https://powerapps.microsoft.com/tutorials/connections-list/))

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.

Die Azure SQL-Datenbank bietet Ihnen folgende Möglichkeiten:

- Erstellen Sie einen Geschäftsworkflow auf der Basis der Daten, die Sie von der Azure SQL-Datenbank erhalten. 
- Verwenden Sie Aktionen zum Abrufen einer Zeile, Einfügen einer Zeile usw. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Sie können z.B. eine Datenzeile aus der Azure SQL-Datenbank abrufen und dann die Daten in Excel hinzufügen. 

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../app-service-logic/app-service-logic-create-a-logic-app.md).


## Trigger und Aktionen
SQL bietet die folgenden Aktionen. Es gibt keine Trigger.

Trigger | Aktionen
--- | ---
Keine | <ul><li>Zeile abrufen</li><li>Zeilen abrufen</li><li>Zeile einfügen</li><li>Zeile löschen</li><li>Tabellen abrufen</li><li>Zeile aktualisieren</li></ul>

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## Herstellen der SQL-Verbindung

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit SQL](../../includes/connectors-create-api-sqlazure.md)]


Nachdem Sie die Verbindung hergestellt haben, geben Sie die SQL-Eigenschaften ein, z.B. den Tabellennamen. In der **REST-API-Referenz** in diesem Thema werden diese Eigenschaften beschrieben.

>[AZURE.TIP] Sie können diese Verbindung in anderen Logik-Apps verwenden.

## Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### Zeile abrufen 
Ruft eine einzelne Zeile aus einer SQL-Tabelle ab. ```GET: /datasets/default/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Name der SQL-Tabelle|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der abzurufenden Zeile|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Zeilen abrufen 
Ruft Zeilen aus einer SQL-Tabelle ab. ```GET: /datasets/default/tables/{table}/items```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Name der SQL-Tabelle|
|$skip|integer|no|query|(Keine)|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|integer|no|query|(Keine)|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|
|$filter|string|no|query|(Keine)|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|string|no|query|(Keine)|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Zeile einfügen 
Fügt eine neue Zeile in eine SQL-Tabelle ein. ```POST: /datasets/default/tables/{table}/items```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Name der SQL-Tabelle|
|item|ItemInternalId: string|Ja|body|(Keine)|In der angegebenen SQL-Tabelle einzufügende Zeile|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Zeile löschen 
Löscht eine Zeile aus einer SQL-Tabelle. ```DELETE: /datasets/default/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Name der SQL-Tabelle|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der zu löschenden Zeile|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Tabellen abrufen 
Ruft Tabellen aus einer SQL-Datenbank ab. ```GET: /datasets/default/tables```

Es gibt keine Parameter für diesen Aufruf.

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|


### Zeile aktualisieren 
Aktualisiert eine vorhandene Zeile in einer SQL-Tabelle. ```PATCH: /datasets/default/tables/{table}/items/{id}```

| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|table|string|Ja|path|(Keine)|Name der SQL-Tabelle|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der zu aktualisierenden Zeile|
|item|ItemInternalId: string|Ja|body|(Keine)|Zeile mit aktualisierten Werten|

#### Antwort
|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|

## Objektdefinitionen

#### DataSetsMetadata

|Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|tabular|nicht definiert|no|
|Blob|nicht definiert|no|

#### TabularDataSetsMetadata

|Eigenschaftenname | Datentyp | Erforderlich |
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|
|tableDisplayName|string|no|
|tablePluralName|string|no|

#### BlobDataSetsMetadata

|Eigenschaftenname | Datentyp |Erforderlich |
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|

#### TableMetadata

|Eigenschaftenname | Datentyp |Erforderlich |
|---|---|---|
|name|string|no|
|title|string|no|
|x-ms-permission|string|no|
|schema|nicht definiert|no|

#### DataSetsList

|Eigenschaftenname | Datentyp |Erforderlich |
|---|---|---|
|value|array|no|

#### DataSet

|Eigenschaftenname | Datentyp |Erforderlich |
|---|---|---|
|Name|string|no|
|DisplayName|string|no|

#### Tabelle

|Eigenschaftenname | Datentyp |Erforderlich |
|---|---|---|
|Name|string|no|
|DisplayName|string|no|

#### Item

|Eigenschaftenname | Datentyp |Erforderlich |
|---|---|---|
|ItemInternalId|string|no|

#### ItemsList

|Eigenschaftenname | Datentyp |Erforderlich |
|---|---|---|
|value|array|no|

#### TablesList

|Eigenschaftenname | Datentyp |Erforderlich |
|---|---|---|
|value|array|no|


## Nächste Schritte

[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

<!---HONumber=AcomDC_0525_2016-->