<properties
pageTitle="So wird's gemacht: Verwenden des Azure Service Bus-Connectors in Ihren Logik-Apps | Microsoft Azure"
description="Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine Verbindung mit Azure Service Bus her, um Nachrichten zu senden und zu empfangen. Sie können Aktionen ausführen wie Senden an eine Warteschlange, Senden an ein Thema, Empfangen aus der Warteschlange, Empfangen aus Abonnements usw."
services="logic-apps"	
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
ms.date="08/02/2016"
ms.author="deonhe"/>

# Erste Schritte mit dem Azure Service Bus-Connector

Stellen Sie eine Verbindung mit Azure Service Bus her, um Nachrichten zu senden und zu empfangen. Sie können Aktionen ausführen wie Senden an eine Warteschlange, Senden an ein Thema, Empfangen aus der Warteschlange, Empfangen aus Abonnements usw.

Wenn Sie einen [Connector](./apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## Herstellen einer Verbindung mit Azure Service Bus

Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine [Verbindung](./connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.

### Erstellen einer Verbindung mit Azure Service Bus

>[AZURE.INCLUDE [Schritte zum Erstellen einer Verbindung mit Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## Verwenden eines Azure Service Bus-Triggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

>[AZURE.INCLUDE [Schritte zum Erstellen eines Service Bus-Triggers](../../includes/connectors-create-api-servicebus-trigger.md)]

## Verwenden einer Azure Service Bus-Aktion

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Schritte zum Erstellen einer Service Bus-Aktion](../../includes/connectors-create-api-servicebus-action.md)]

## Technische Details

Im Anschluss finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden:

## Azure Service Bus-Trigger

Für Azure Service Bus stehen folgende Trigger zur Verfügung:

|Trigger | Beschreibung|
|--- | ---|
|[When a message is received in a queue](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue) (Wenn bei einer Warteschlange eine Nachricht eingeht)|Dieser Vorgang löst einen Ablauf aus, wenn bei einer Warteschlange eine Nachricht eingeht.|
|[When a message is received in a topic subscription](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription) (Wenn bei einem Themenabonnement eine Nachricht eingeht)|Dieser Vorgang löst einen Ablauf aus, wenn bei einem Themenabonnement eine Nachricht eingeht.|


## Azure Service Bus-Aktionen

Für Azure Service Bus stehen folgende Aktionen zur Verfügung:


|Aktion|Beschreibung|
|--- | ---|
|[Nachricht senden](connectors-create-api-servicebus.md#send-message)|Dieser Vorgang sendet eine Nachricht an eine Warteschlange oder an ein Thema.|
### Aktionsdetails

Im Anschluss finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten:



### Nachricht senden
Dieser Vorgang sendet eine Nachricht an eine Warteschlange oder an ein Thema.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|ContentData*|Inhalt|Inhalt der Nachricht|
|ContentType|Inhaltstyp|Inhaltstyp der Nachricht|
|Eigenschaften|Eigenschaften|Schlüssel-Wert-Paare für die einzelnen vermittelten Eigenschaften|
|entityName*|Warteschlangen-/Themenname|Name der Warteschlange oder des Themas|

Die folgenden erweiterten Parameter sind ebenfalls verfügbar:

|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|MessageId|Nachrichten-ID|Benutzerdefinierter Wert, mit dem Service Bus doppelte Nachrichten ermitteln kann (sofern aktiviert)|
|To  
|To  
|Zieladresse|
|ReplyTo|Antworten an|Warteschlangenadresse für die Antwort|
|ReplyToSessionId|Antwort an Sitzungs-ID|Sitzungs-ID für die Antwort|
|Bezeichnung|Bezeichnung|Anwendungsspezifische Bezeichnung|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Zeitpunkt, zu dem die Nachricht der Warteschlange hinzugefügt wird (Datum und Uhrzeit im UTC-Format)|
|SessionId|Sitzungs-ID|ID der Sitzung|
|CorrelationId|Korrelations-ID|ID der Korrelation|
|TimeToLive|Gültigkeitsdauer (Time To Live, TTL)|Die Gültigkeitsdauer einer Nachricht (in Ticks). Der Zeitraum beginnt, wenn die Nachricht an den Service Bus gesendet wird.|



Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.




### When a message is received in a queue (Wenn bei einer Warteschlange eine Nachricht eingeht)
Dieser Vorgang löst einen Ablauf aus, wenn bei einer Warteschlange eine Nachricht eingeht.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|queueName*|Warteschlangenname|Name der Warteschlange|


Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

ServiceBusMessage: Dieses Objekt enthält den Inhalt und die Eigenschaften einer Service Bus-Nachricht.


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ContentData|string|Inhalt der Nachricht|
|ContentType|string|Inhaltstyp der Nachricht|
|Eigenschaften|Objekt|Schlüssel-Wert-Paare für die einzelnen vermittelten Eigenschaften|
|MessageId|string|Benutzerdefinierter Wert, mit dem Service Bus doppelte Nachrichten ermitteln kann (sofern aktiviert)|
|To  
|string|Zieladresse|
|ReplyTo|string|Warteschlangenadresse für die Antwort|
|ReplyToSessionId|string|Sitzungs-ID für die Antwort|
|Bezeichnung|string|Anwendungsspezifische Bezeichnung|
|ScheduledEnqueueTimeUtc|string|Zeitpunkt, zu dem die Nachricht der Warteschlange hinzugefügt wird (Datum und Uhrzeit im UTC-Format)|
|SessionId|string|ID der Sitzung|
|CorrelationId|string|ID der Korrelation|
|TimeToLive|string|Die Gültigkeitsdauer einer Nachricht (in Ticks). Der Zeitraum beginnt, wenn die Nachricht an den Service Bus gesendet wird.|




### When a message is received in a topic subscription (Wenn bei einem Themenabonnement eine Nachricht eingeht)
Dieser Vorgang löst einen Ablauf aus, wenn bei einem Themenabonnement eine Nachricht eingeht.


|Eigenschaftenname| Anzeigename|Beschreibung|
| ---|---|---|
|topicName*|Themenname|Name des Themas|
|subscriptionName*|Topic subscription name (Themenabonnementname)|Name des Themenabonnements|


Ein Sternchen gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### Ausgabedetails

ServiceBusMessage: Dieses Objekt enthält den Inhalt und die Eigenschaften einer Service Bus-Nachricht.


| Eigenschaftenname | Datentyp | Beschreibung |
|---|---|---|
|ContentData|string|Inhalt der Nachricht|
|ContentType|string|Inhaltstyp der Nachricht|
|Eigenschaften|Objekt|Schlüssel-Wert-Paare für die einzelnen vermittelten Eigenschaften|
|MessageId|string|Benutzerdefinierter Wert, mit dem Service Bus doppelte Nachrichten ermitteln kann (sofern aktiviert)|
|To  
|string|Zieladresse|
|ReplyTo|string|Warteschlangenadresse für die Antwort|
|ReplyToSessionId|string|Sitzungs-ID für die Antwort|
|Bezeichnung|string|Anwendungsspezifische Bezeichnung|
|ScheduledEnqueueTimeUtc|string|Zeitpunkt, zu dem die Nachricht der Warteschlange hinzugefügt wird (Datum und Uhrzeit im UTC-Format)|
|SessionId|string|ID der Sitzung|
|CorrelationId|string|ID der Korrelation|
|TimeToLive|string|Die Gültigkeitsdauer einer Nachricht (in Ticks). Der Zeitraum beginnt, wenn die Nachricht an den Service Bus gesendet wird.|



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

<!---HONumber=AcomDC_0803_2016-->