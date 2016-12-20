---
title: Anleitung zum Verwenden des Azure Service Bus-Connectors in Ihren Logik-Apps | Microsoft Docs
description: "Erstellen Sie Logik-Apps mit Azure App Service. Stellen Sie eine Verbindung mit Azure Service Bus her, um Nachrichten zu senden und zu empfangen. Sie können Aktionen ausführen wie Senden an eine Warteschlange, Senden an ein Thema, Empfangen aus der Warteschlange und Empfangen aus Abonnements usw."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a1ecedf9bf78e0a15908c1a03ca24d611f551875


---
# <a name="get-started-with-the-azure-service-bus-connector"></a>Erste Schritte mit dem Azure Service Bus-Connector
Stellen Sie eine Verbindung mit Azure Service Bus her, um Nachrichten zu senden und zu empfangen. Sie können Aktionen ausführen wie Senden an eine Warteschlange, Senden an ein Thema, Empfangen aus der Warteschlange und Empfangen aus Abonnements usw.

Wenn Sie [einen Connector](apis-list.md) verwenden möchten, müssen Sie zuerst eine Logik-App erstellen. Erstellen Sie daher erst einmal eine Logik-App, wie [hier](../app-service-logic/app-service-logic-create-a-logic-app.md) beschrieben.

## <a name="connect-to-service-bus"></a>Herstellen einer Verbindung mit Service Bus
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, müssen Sie zunächst eine Verbindung mit dem Dienst herstellen. Eine [Verbindung](connectors-overview.md) stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her.  

> [!INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a>Verwenden eines Service Bus-Triggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a>Verwenden einer Service Bus-Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[!INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="technical-details"></a>Technische Details
Hier finden Sie ausführliche Informationen zu den Triggern, Aktionen und Antworten, die von dieser Verbindung unterstützt werden.

### <a name="service-bus-triggers"></a>Service Bus-Trigger
Für Service Bus stehen folgende Trigger zur Verfügung:  

| Trigger | Beschreibung |
| --- | --- |
| [When a message is received in a queue](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue) (Wenn bei einer Warteschlange eine Nachricht eingeht) |Dieser Vorgang löst einen Ablauf aus, wenn bei einer Warteschlange eine Nachricht eingeht. |
| [When a message is received in a topic subscription](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription) (Wenn bei einem Themenabonnement eine Nachricht eingeht) |Dieser Vorgang löst einen Ablauf aus, wenn bei einem Themenabonnement eine Nachricht eingeht. |

### <a name="service-bus-actions"></a>Service Bus-Aktionen
Für Service Bus stehen folgende Aktionen zur Verfügung:

| Aktion | Beschreibung |
| --- | --- |
| [Nachricht senden](connectors-create-api-servicebus.md#send-message) |Dieser Vorgang sendet eine Nachricht an eine Warteschlange oder an ein Thema. |

### <a name="action-and-trigger-details"></a>Ausführliche Informationen zu Aktionen und Triggern
Im Anschluss finden Sie ausführliche Informationen zu den Aktionen und Triggern für diesen Connector sowie die jeweiligen Antworten.

#### <a name="send-message"></a>Nachricht senden
| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| ContentData* |Inhalt |Inhalt der Nachricht. |
| ContentType |Inhaltstyp |Inhaltstyp des Nachrichteninhalts. |
| Eigenschaften |Eigenschaften |Schlüssel-Wert-Paare für die einzelnen vermittelten Eigenschaften. |
| entityName* |Warteschlangen-/Themenname |Name der Warteschlange oder des Themas. |

Die folgenden erweiterten Parameter sind ebenfalls verfügbar:

| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| MessageId |Nachrichten-ID |Benutzerdefinierter Wert, mit dem Service Bus doppelte Nachrichten ermitteln kann (sofern aktiviert). |
| To
 |To
 |Zieladresse. |
| ReplyTo |Antworten an |Warteschlangenadresse für die Antwort. |
| ReplyToSessionId |Antwort an Sitzungs-ID |Sitzungs-ID für die Antwort. |
| Bezeichnung |Bezeichnung |Anwendungsspezifische Bezeichnung. |
| ScheduledEnqueueTimeUtc |ScheduledEnqueueTimeUtc |Zeitpunkt, zu dem die Nachricht der Warteschlange hinzugefügt wird (Datum und Uhrzeit im UTC-Format). |
| SessionId |Sitzungs-ID |ID der Sitzung |
| CorrelationId |Korrelations-ID |ID der Korrelation |
| TimeToLive |Gültigkeitsdauer (Time To Live, TTL) |Die Gültigkeitsdauer einer Nachricht (in Ticks). Der Zeitraum beginnt, wenn die Nachricht an den Service Bus gesendet wird. |

Ein Sternchen (*) gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

#### <a name="when-a-message-is-received-in-a-queue"></a>When a message is received in a queue (Wenn bei einer Warteschlange eine Nachricht eingeht)
| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| queueName* |Warteschlangenname |Name der Warteschlange. |

Ein Sternchen (*) gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

##### <a name="output-details"></a>Ausgabedetails
ServiceBusMessage: Dieses Objekt enthält den Inhalt und die Eigenschaften einer Service Bus-Nachricht.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ContentData |string |Inhalt der Nachricht. |
| ContentType |string |Inhaltstyp des Nachrichteninhalts. |
| Eigenschaften |Objekt |Schlüssel-Wert-Paare für die einzelnen vermittelten Eigenschaften. |
| MessageId |string |Benutzerdefinierter Wert, mit dem Service Bus doppelte Nachrichten ermitteln kann (sofern aktiviert). |
| To
 |string |Zieladresse. |
| ReplyTo |string |Warteschlangenadresse für die Antwort. |
| ReplyToSessionId |string |Sitzungs-ID für die Antwort. |
| Bezeichnung |string |Anwendungsspezifische Bezeichnung. |
| ScheduledEnqueueTimeUtc |string |Zeitpunkt, zu dem die Nachricht der Warteschlange hinzugefügt wird (Datum und Uhrzeit im UTC-Format). |
| SessionId |string |ID der Sitzung |
| CorrelationId |string |ID der Korrelation |
| TimeToLive |string |Die Gültigkeitsdauer einer Nachricht (in Ticks). Der Zeitraum beginnt, wenn die Nachricht an den Service Bus gesendet wird. |

#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>When a message is received in a topic subscription (Wenn bei einem Themenabonnement eine Nachricht eingeht)
| Eigenschaftenname | Anzeigename | Beschreibung |
| --- | --- | --- |
| topicName* |Themenname |Name des Themas. |
| subscriptionName* |Topic subscription name (Themenabonnementname) |Name des Themenabonnements. |

Ein Sternchen (*) gibt an, dass es sich um eine erforderliche Eigenschaft handelt.

##### <a name="output-details"></a>Ausgabedetails
ServiceBusMessage: Dieses Objekt enthält den Inhalt und die Eigenschaften einer Service Bus-Nachricht.

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| ContentData |string |Inhalt der Nachricht. |
| ContentType |string |Inhaltstyp des Nachrichteninhalts. |
| Eigenschaften |Objekt |Schlüssel-Wert-Paare für die einzelnen vermittelten Eigenschaften. |
| MessageId |string |Benutzerdefinierter Wert, mit dem Service Bus doppelte Nachrichten ermitteln kann (sofern aktiviert). |
| To
 |string |Zieladresse. |
| ReplyTo |string |Warteschlangenadresse für die Antwort. |
| ReplyToSessionId |string |Sitzungs-ID für die Antwort. |
| Bezeichnung |string |Anwendungsspezifische Bezeichnung. |
| ScheduledEnqueueTimeUtc |string |Zeitpunkt, zu dem die Nachricht der Warteschlange hinzugefügt wird (Datum und Uhrzeit im UTC-Format). |
| SessionId |string |ID der Sitzung |
| CorrelationId |string |ID der Korrelation |
| TimeToLive |string |Die Gültigkeitsdauer einer Nachricht (in Ticks). Der Zeitraum beginnt, wenn die Nachricht an den Service Bus gesendet wird. |

### <a name="http-responses"></a>HTTP-Antworten
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


