<properties
pageTitle="Verwenden des Azure Service Bus-Connectors in Ihren Logik-Apps | Microsoft Azure"
description="Erste Schritte mit dem Azure Service Bus-Connector in Ihren Microsoft Azure App Service-Logik-Apps"
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
ms.date="05/23/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem Azure Service Bus-Connector 

Stellen Sie eine Verbindung mit Azure Service Bus her, um Nachrichten zu senden und zu empfangen. Sie können Aktionen ausführen wie Senden an eine Warteschlange, Senden an ein Thema, Empfangen aus der Warteschlange, Empfangen aus Abonnements usw.

>[AZURE.NOTE] Diese Version des Artikels gilt für die Schemaversion 2015-08-01-preview für Logik-Apps.

Mit Azure Service Bus können Sie folgende Aktionen ausführen:

* Erstellen von Logik-Apps  

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Trigger und Aktionen

Der Azure Service Bus-Connector kann als Aktion verwendet werden. Er verfügt über Trigger. Alle Connectors unterstützen Daten im JSON- und XML-Format.

 Der Azure Service Bus-Connector verfügt über die folgenden Aktionen und/oder Trigger:

### Azure Service Bus-Aktionen
Sie können die folgenden Aktionen ausführen:

|Aktion|Beschreibung|
|--- | ---|
|SendMessage|Sendet eine Nachricht an ein Azure Service Bus-Warteschlange oder -Thema.|
### Azure Service Bus-Trigger
Sie können auf diese Ereignisse lauschen:

|Trigger | Beschreibung|
|--- | ---|
|GetMessageFromQueue|Ruft eine neue Nachricht aus der Azure Service Bus-Warteschlange ab.|
|GetMessageFromTopic|Ruft eine neue Nachricht aus dem Azure Service Bus-Themenabonnement ab.|


## Erstellen einer Verbindung mit Azure Service Bus
Zur Verwendung des Azure Service Bus-Connectors stellen Sie zunächst eine **Verbindung** her und geben anschließend die Details für diese Eigenschaften an:

>[AZURE.INCLUDE [Schritte zum Herstellen einer Verbindung mit ServiceBus](../../includes/connectors-create-api-servicebus.md)]

>[AZURE.TIP] Sie können diese Verbindung in anderen Logik-Apps verwenden.

## Azure Service Bus-REST-API-Referenz
#### Diese Dokumentation gilt für Version 1.0.


### Sendet eine Nachricht an ein Azure Service Bus-Warteschlange oder -Thema.
**```POST: /{entityName}/messages```**



| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|message| |Ja|body|(Keine)|Service Bus-Nachricht|
|entityName|string|Ja|path|(Keine)|Name der Warteschlange oder des Themas|


### Im Folgenden sind die möglichen Antworten aufgeführt:

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|
------



### Ruft eine neue Nachricht aus der Azure Service Bus-Warteschlange ab.
**```GET: /{queueName}/messages/head```**



| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|queueName|string|Ja|path|(Keine)|Name der Warteschlange.|


### Im Folgenden sind die möglichen Antworten aufgeführt:

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|
------



### Ruft eine neue Nachricht aus dem Azure Service Bus-Themenabonnement ab.
**```GET: /{topicName}/subscriptions/{subscriptionName}/messages/head```**



| Name| Datentyp|Erforderlich|Enthalten in|Standardwert|Beschreibung|
| ---|---|---|---|---|---|
|topicName|string|Ja|path|(Keine)|Name des Themas.|
|subscriptionName|string|Ja|path|(Keine)|Name des Themenabonnements.|


### Im Folgenden sind die möglichen Antworten aufgeführt:

|Name|Beschreibung|
|---|---|
|200|OK|
|default|Fehler beim Vorgang.|
------



## Objektdefinitionen: 

 **ServiceBusMessage:** Nachricht besteht aus Inhalt und Eigenschaften

Erforderliche Eigenschaften für ServiceBusMessage:

ContentTransferEncoding

**Alle Eigenschaften**:


| Name | Datentyp |
|---|---|
|ContentData|string|
|ContentType|string|
|ContentTransferEncoding|string|
|Eigenschaften|Objekt|


## Nächste Schritte
[Erstellen einer Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

<!---HONumber=AcomDC_0525_2016-->