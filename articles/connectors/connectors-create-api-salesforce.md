---
title: Verwenden des Salesforce-Connectors in Ihren Logik-Apps | Microsoft Docs
description: Erstellen Sie Logik-Apps mit Azure App Service. Der Salesforce-Connector bietet eine API zum Verwenden von Salesforce-Objekten.
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: 54fe5af8-7d2a-4da8-94e7-15d029e029bf
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/05/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f2f9a4d182527d2772e78d78a0c10bd94742d0fe


---
# <a name="get-started-with-the-salesforce-connector"></a>Erste Schritte mit dem Salesforce-Connector
Der Salesforce-Connector bietet eine API zum Verwenden von Salesforce-Objekten.

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## <a name="connect-to-salesforce-connector"></a>Herstellen einer Verbindung mit dem Salesforce-Connector
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine *Verbindung* mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.  

### <a name="create-a-connection-to-salesforce-connector"></a>Erstellen einer Verbindung mit dem Salesforce-Connector
> [!INCLUDE [Steps to create a connection to Salesforce Connector](../../includes/connectors-create-api-salesforce.md)]
> 
> 

## <a name="use-a-salesforce-connector-trigger"></a>Verwenden eines Triggers des Salesforce-Connectors
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

> [!INCLUDE [Steps to create a Salesforce trigger](../../includes/connectors-create-api-salesforce-trigger.md)]
> 
> 

## <a name="add-a-condition"></a>Eine Bedingung hinzufügen
> [!INCLUDE [Steps to create a Salesforce condition](../../includes/connectors-create-api-salesforce-condition.md)]
> 
> 

## <a name="use-a-salesforce-connector-action"></a>Verwenden einer Aktion des Salesforce-Connectors
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

> [!INCLUDE [Steps to create a Salesforce action](../../includes/connectors-create-api-salesforce-action.md)]
> 
> 

## <a name="technical-details"></a>Technische Details
Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## <a name="salesforce-connector-triggers"></a>Trigger des Salesforce-Connectors
Der Salesforce-Connector verfügt über folgende Trigger:  

| Trigger | Beschreibung |
| --- | --- |
| [Wenn ein Objekt erstellt wird](connectors-create-api-salesforce.md#when-an-object-is-created) |Dieser Vorgang löst einen Ablauf aus, wenn ein Objekt erstellt wird. |
| [Wenn ein Objekt geändert wird](connectors-create-api-salesforce.md#when-an-object-is-modified) |Dieser Vorgang löst einen Ablauf aus, wenn ein Objekt geändert wird. |

## <a name="salesforce-connector-actions"></a>Aktion des Salesforce-Connectors
Der Salesforce-Connector verfügt über folgende Aktionen:

| Aktion | Beschreibung |
| --- | --- |
| [Get objects](connectors-create-api-salesforce.md#get-objects) (Objekte abrufen) |Dieser Vorgang ruft Objekte eines bestimmten Objekttyps (beispielsweise „Lead“) ab. |
| [Create object](connectors-create-api-salesforce.md#create-object) (Objekt erstellen) |Dieser Vorgang erstellt ein Objekt. |
| [Get object](connectors-create-api-salesforce.md#get-object) (Objekt abrufen) |Dieser Vorgang ruft ein Objekt ab. |
| [Delete object](connectors-create-api-salesforce.md#delete-object) (Objekt löschen) |Dieser Vorgang löscht ein Objekt. |
| [Update object](connectors-create-api-salesforce.md#update-object) (Objekt aktualisieren) |Dieser Vorgang aktualisiert ein Objekt. |
| [Get object types](connectors-create-api-salesforce.md#get-object-types) (Objekttypen abrufen) |Dieser Vorgang listet die verfügbaren Objekttypen auf. |

### <a name="action-details"></a>Aktionsdetails
Im Anschluss finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten:

### <a name="get-objects"></a>Get objects (Objekte abrufen)
Dieser Vorgang ruft Objekte eines bestimmten Objekttyps (beispielsweise „Lead“) ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Objekttyp |Salesforce-Objekttyp (beispielsweise „Lead“) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

### <a name="create-object"></a>Objekt erstellen
Dieser Vorgang erstellt ein Objekt. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Objekttyp |Objekttyp (beispielsweise „Lead“) |
| item* |Objekt |Zu erstellendes Objekt |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
Element

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

### <a name="get-object"></a>Objekt abrufen
Dieser Vorgang ruft ein Objekt ab. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Objekttyp |Salesforce-Objekttyp (beispielsweise „Lead“) |
| id* |Objekt-ID |Bezeichner des abzurufenden Objekts |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
Element

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

### <a name="delete-object"></a>Objekt löschen
Dieser Vorgang löscht ein Objekt. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Objekttyp |Objekttyp (beispielsweise „Lead“) |
| id* |Objekt-ID |Bezeichner des zu löschenden Objekts |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

### <a name="update-object"></a>Objekt aktualisieren
Dieser Vorgang aktualisiert ein Objekt. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Objekttyp |Objekttyp (beispielsweise „Lead“) |
| id* |Objekt-ID |Bezeichner des zu aktualisierenden Objekts |
| item* |Objekt |Objekt mit geänderten Eigenschaften |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
Element

| Eigenschaftenname | Datentyp |
| --- | --- |
| ItemInternalId |string |

### <a name="when-an-object-is-created"></a>Wenn ein Objekt erstellt wird
Dieser Vorgang löst einen Ablauf aus, wenn ein Objekt erstellt wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Objekttyp |Objekttyp (beispielsweise „Lead“) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

### <a name="when-an-object-is-modified"></a>Wenn ein Objekt geändert wird
Dieser Vorgang löst einen Ablauf aus, wenn ein Objekt geändert wird. 

| Eigenschaftenname | Display Name | Beschreibung |
| --- | --- | --- |
| table* |Objekttyp |Objekttyp (beispielsweise „Lead“) |
| $filter |Filterabfrage |Eine ODATA-Filterabfrage zum Einschränken der Anzahl der Einträge |
| $orderby |Sortieren nach |Eine ODATA-orderBy-Abfrage zum Angeben der Reihenfolge von Einträgen |
| $skip |Auslassungsanzahl |Anzahl der zu überspringenden Einträge (Standardeinstellung = 0) |
| $top |Maximale Anzahl von Get-Anforderungen |Maximale Anzahl abzurufender Einträge (Standardeinstellung = 256) |

Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="output-details"></a>Ausgabedetails
ItemsList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

### <a name="get-object-types"></a>Get object types (Objekttypen abrufen)
Dieser Vorgang listet die verfügbaren Objekttypen auf. 

Es gibt keine Parameter für diesen Aufruf

#### <a name="output-details"></a>Ausgabedetails
TablesList

| Eigenschaftenname | Datentyp |
| --- | --- |
| value |array |

## <a name="http-responses"></a>HTTP-Antworten
Von den oben angegebenen Aktionen und Triggern können folgende HTTP-Statuscodes zurückgegeben werden: 

| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| 202 |Zulässig |
| 400 |Ungültige Anforderung |
| 401 |Nicht autorisiert |
| 403 |Verboten (403) |
| 404 |Nicht gefunden |
| 500 |Interner Serverfehler. Unbekannter Fehler. |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md)




<!--HONumber=Nov16_HO3-->


