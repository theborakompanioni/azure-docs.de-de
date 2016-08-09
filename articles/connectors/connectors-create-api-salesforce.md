<properties
pageTitle="Verwenden des Salesforce-Connectors in Ihren Logik-Apps | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. Der Salesforce-Connector bietet eine API zum Verwenden von Salesforce-Objekten."
services="app-servicelogic"	
documentationCenter=".net,nodejs,java" 	
authors="msftman"	
manager="erikre"	
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/22/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem Salesforce-Connector

Der Salesforce-Connector bietet eine API zum Verwenden von Salesforce-Objekten.

Wenn Sie einen [Connector](./apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Herstellen einer Verbindung mit dem Salesforce-Connector

Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine [Verbindung](./connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.

### Erstellen einer Verbindung mit dem Salesforce-Connector

>[AZURE.INCLUDE [Schritte zum Erstellen einer Verbindung mit dem Salesforce-Connector](../../includes/connectors-create-api-salesforce.md)]

## Verwenden eines Triggers des Salesforce-Connectors

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

>[AZURE.INCLUDE [Schritte zum Erstellen eines Salesforce-Triggers](../../includes/connectors-create-api-salesforce-trigger.md)]

## Hinzufügen einer Bedingung 
>[AZURE.INCLUDE [Schritte zum Hinzufügen einer Salesforce-Bedingung](../../includes/connectors-create-api-salesforce-condition.md)]

## Verwenden einer Aktion des Salesforce-Connectors

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

>[AZURE.INCLUDE [Schritte zum Erstellen einer Salesforce-Aktion](../../includes/connectors-create-api-salesforce-action.md)]

## Technische Details

Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## Trigger des Salesforce-Connectors

Der Salesforce-Connector verfügt über folgende Trigger:

|Trigger | Beschreibung|
|--- | ---|
|[Wenn ein Objekt erstellt wird](connectors-create-api-salesforceconnector.md#when-an-object-is-created)|Dieser Vorgang löst einen Ablauf aus, wenn ein Objekt erstellt wird.|
|[Wenn ein Objekt geändert wird](connectors-create-api-salesforceconnector.md#when-an-object-is-modified)|Dieser Vorgang löst einen Ablauf aus, wenn ein Objekt geändert wird.|


## Aktion des Salesforce-Connectors

Der Salesforce-Connector verfügt über folgende Aktionen:


|Aktion|Beschreibung|
|--- | ---|
|[Get objects](connectors-create-api-salesforceconnector.md#get-objects) (Objekte abrufen)|Dieser Vorgang ruft Objekte eines bestimmten Objekttyps (beispielsweise „Lead“) ab.|
|[Objekt erstellen](connectors-create-api-salesforceconnector.md#create-object)|Dieser Vorgang erstellt ein Objekt.|
|[Objekt abrufen](connectors-create-api-salesforceconnector.md#get-object)|Dieser Vorgang ruft ein Objekt ab.|
|[Objekt löschen](connectors-create-api-salesforceconnector.md#delete-object)|Dieser Vorgang löscht ein Objekt.|
|[Objekt aktualisieren](connectors-create-api-salesforceconnector.md#update-object)|Dieser Vorgang aktualisiert ein Objekt.|
|[Get object types](connectors-create-api-salesforceconnector.md#get-object-types) (Objekttypen abrufen)|Dieser Vorgang listet die verfügbaren Objekttypen auf.|
### Aktionsdetails

Im Anschluss finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten:



### Get objects (Objekte abrufen)
Dieser Vorgang ruft Objekte eines bestimmten Objekttyps (beispielsweise „Lead“) ab.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|table*|Objekttyp|Salesforce-Objekttyp (beispielsweise „Lead“)|
|$filter|Filterabfrage|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|Sortieren nach|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|Auslassungsanzahl|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|Maximale Anzahl von Get-Anforderungen|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

ItemsList


| Eigenschaftenname | Datentyp |
|---|---|
|value|array|




### Objekt erstellen
Dieser Vorgang erstellt ein Objekt.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|table*|Objekttyp|Objekttyp (beispielsweise „Lead“)|
|item*|Object|Zu erstellendes Objekt|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

Item


| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|string|




### Objekt abrufen
Dieser Vorgang ruft ein Objekt ab.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|table*|Objekttyp|Salesforce-Objekttyp (beispielsweise „Lead“)|
|id*|Objekt-ID|Bezeichner des abzurufenden Objekts|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

Item


| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|string|




### Objekt löschen
Dieser Vorgang löscht ein Objekt.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|table*|Objekttyp|Objekttyp (beispielsweise „Lead“)|
|id*|Objekt-ID|Bezeichner des zu löschenden Objekts|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### Objekt aktualisieren
Dieser Vorgang aktualisiert ein Objekt.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|table*|Objekttyp|Objekttyp (beispielsweise „Lead“)|
|id*|Objekt-ID|Bezeichner des zu aktualisierenden Objekts|
|item*|Object|Objekt mit geänderten Eigenschaften|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

Item


| Eigenschaftenname | Datentyp |
|---|---|
|ItemInternalId|string|




### Wenn ein Objekt erstellt wird
Dieser Vorgang löst einen Ablauf aus, wenn ein Objekt erstellt wird.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|table*|Objekttyp|Objekttyp (beispielsweise „Lead“)|
|$filter|Filterabfrage|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|Sortieren nach|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|Auslassungsanzahl|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|Maximale Anzahl von Get-Anforderungen|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

ItemsList


| Eigenschaftenname | Datentyp |
|---|---|
|value|array|




### Wenn ein Objekt geändert wird
Dieser Vorgang löst einen Ablauf aus, wenn ein Objekt geändert wird.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|table*|Objekttyp|Objekttyp (beispielsweise „Lead“)|
|$filter|Filterabfrage|Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge|
|$orderby|Sortieren nach|Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen|
|$skip|Auslassungsanzahl|Anzahl der zu überspringenden Einträge (Standardeinstellung = 0)|
|$top|Maximale Anzahl von Get-Anforderungen|Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256)|

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

ItemsList


| Eigenschaftenname | Datentyp |
|---|---|
|value|array|




### Get object types (Objekttypen abrufen)
Dieser Vorgang listet die verfügbaren Objekttypen auf.


Es gibt keine Parameter für diesen Aufruf

#### Ausgabedetails

TablesList


| Eigenschaftenname | Datentyp | 
|---|---|
|value|array|



## HTTP-Antworten

Von den oben angegebenen Aktionen und Triggern können folgende HTTP-Statuscodes zurückgegeben werden:

|Name|Beschreibung|
|---|---|
|200|OK|
|202|Zulässig|
|400|Ungültige Anforderung|
|401|Nicht autorisiert|
|403|Verboten|
|404|Nicht gefunden|
|500|Interner Serverfehler. Unbekannter Fehler.|
|die Standardeinstellung|Fehler beim Vorgang.|






## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0727_2016-->