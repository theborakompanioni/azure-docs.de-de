<properties
pageTitle="Hinzufügen des Excel-Connectors zu PowerApps Enterprise | Microsoft Azure"
description="Übersicht über den Excel-Connector mit REST-API-Parametern"
services=""    
documentationCenter=""     
authors="msftman"    
manager="erikre"    
editor=""
tags="connectors"/>

<tags
ms.service="multiple"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="na"
ms.date="05/18/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem Excel-Connector

Stelen Sie eine Verbindung mit Excel her, um eine Zeile einzufügen, eine Zeile zu löschen und mehr. Der Excel-Connector kann verwendet werden in:

- PowerApps

Mit Excel können Sie folgende Aktionen ausführen:

- Fügen Sie den Excel-Connector in PowerApps Enterprise hinzu. Die Benutzer können diesen Connector anschließend in ihren Apps verwenden. 

Informationen zum Hinzufügen eines Connectors in PowerApps Enterprise finden Sie unter [Registrieren einer Microsoft-verwalteten API oder einer IT-verwalteten API](../power-apps/powerapps-register-from-available-apis.md).

## Trigger und Aktionen
Excel bietet die folgenden Aktionen. Es gibt keine Trigger.

|Trigger|Actions|
|--- | ---|
|Keine | <ul><li>Zeilen abrufen</li><li>Zeile einfügen</li><li>Zeile löschen</li><li>Zeile abrufen</li><li>Tabellen abrufen</li><li>Zeile aktualisieren</li></ul>

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### Fügt eine neue Zeile in einer Excel-Tabelle ein.
```POST: /datasets/{dataset}/tables/{table}/items```



| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|dataset|string|Ja|path|(Keine)|Excel-Dateiname|
|Tabelle|string|Ja|path|(Keine)|Name der Excel-Tabelle|
|item| |Ja|body|(Keine)|In der angegebenen Excel-Tabelle einzufügende Zeile|


### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|




### Ruft eine einzelne Zeile aus einer Excel-Tabelle ab
```GET: /datasets/{dataset}/tables/{table}/items/{id}```



| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|dataset|string|Ja|path|(Keine)|Excel-Dateiname|
|Tabelle|string|Ja|path|(Keine)|Name der Excel-Tabelle|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der abzurufenden Zeile|


### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|




### Löscht eine einzelne Zeile aus einer Excel-Tabelle
```DELETE: /datasets/{dataset}/tables/{table}/items/{id}```



| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|dataset|string|Ja|path|(Keine)|Excel-Dateiname|
|Tabelle|string|Ja|path|(Keine)|Name der Excel-Tabelle|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der zu löschenden Zeile|


### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|




### Aktualisiert eine vorhandene Zeile in einer Excel-Tabelle
```PATCH: /datasets/{dataset}/tables/{table}/items/{id}```



| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|dataset|string|Ja|path|(Keine)|Excel-Dateiname|
|Tabelle|string|Ja|path|(Keine)|Name der Excel-Tabelle|
|id|string|Ja|path|(Keine)|Eindeutiger Bezeichner der zu aktualisierenden Zeile|
|item| |Ja|body|(Keine)|Zeile mit aktualisierten Werten|


### Antwort

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|




## Objektdefinitionen

#### DataSetsMetadata

| Name | Datentyp | Erforderlich|
|---|---|---|
|tabular|nicht definiert|no|
|Blob|nicht definiert|no|

#### TabularDataSetsMetadata

| Name | Datentyp |Erforderlich|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|
|tableDisplayName|string|no|
|tablePluralName|string|no|

#### BlobDataSetsMetadata

| Name | Datentyp |Erforderlich|
|---|---|---|
|source|string|no|
|displayName|string|no|
|urlEncoding|string|no|

#### TableMetadata

| Name | Datentyp |Erforderlich|
|---|---|---|
|name|string|no|
|title|string|no|
|x-ms-permission|string|no|
|schema|nicht definiert|no|

#### DataSetsList

| Name | Datentyp |Erforderlich|
|---|---|---|
|value|array|no|

#### DataSet

| Name | Datentyp |Erforderlich|
|---|---|---|
|Name|string|no|
|DisplayName|string|no|

#### Tabelle

| Name | Datentyp |Erforderlich|
|---|---|---|
|Name|string|no|
|DisplayName|string|no|

#### Item

| Name | Datentyp |Erforderlich|
|---|---|---|
|ItemInternalId|string|no|

#### TablesList

| Name | Datentyp |Erforderlich|
|---|---|---|
|value|array|no|

#### ItemsList

| Name | Datentyp |Erforderlich|
|---|---|---|
|value|array|no|


## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md) [Erstellen einer PowerApp](../power-apps/powerapps-get-started-azure-portal.md)

<!---HONumber=AcomDC_0525_2016-->