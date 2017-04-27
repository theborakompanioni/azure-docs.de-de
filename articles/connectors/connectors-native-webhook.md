---
title: "Webhookconnector für Azure Logic Apps | Microsoft-Dokumentation"
description: "Verwenden von Webhookaktionen und -triggern zum Ausführen von Aktionen, z.B. „Array filtern“, in Logik-Apps"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: jehollan; LADocs
translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: fbfef291334109c6dcfcde80741874549fb7929f
ms.lasthandoff: 04/10/2017


---
# <a name="get-started-with-the-webhook-connector"></a>Erste Schritte mit dem Webhookconnector

Mit der Webhookaktion und dem Webhooktrigger können Sie Flows starten, anhalten und fortsetzen, um die folgenden Aufgaben auszuführen:

* Auslösen über [Azure Event Hub](https://github.com/logicappsio/EventHubAPI), wenn ein Element empfangen wird
* Warten auf eine Genehmigung, bevor ein Workflow fortgesetzt wird

Weitere Informationen zum [Erstellen von benutzerdefinierten APIs, die einen Webhook unterstützen](../logic-apps/logic-apps-create-api-app.md).

## <a name="use-the-webhook-trigger"></a>Verwenden des Webhooktriggers

Ein [*Trigger*](connectors-overview.md) ist ein Ereignis, das einen Logik-App-Workflow startet. Webhooktrigger sind ereignisbasiert und erfordern nicht das Abrufen neuer Elemente. Die Logik-App wird wie der [Anforderungstrigger](connectors-native-reqres.md) in dem Moment ausgelöst, in dem ein Ereignis eintritt. Der Webhooktrigger registriert eine *Rückruf-URL* bei einem Dienst und verwendet diese URL, um die Logik-App ggf. auszulösen.

Im Anschluss finden Sie ein Beispiel für die Einrichtung eines HTTP-Triggers im Logik-App-Designer. In den Schritten wird davon ausgegangen, dass Sie bereits eine API bereitgestellt haben oder auf eine API zugreifen, die dem [Muster zum Abonnieren und Abbestellen von Webhooks in Logik-Apps folgt](../logic-apps/logic-apps-create-api-app.md#webhook-triggers). Der Aufruf zum Abonnieren erfolgt, wenn eine Logik-App mit einem neuen Webhook gespeichert oder von „deaktiviert“ in „aktiviert“ umgeschaltet wird. Der Aufruf zum Abbestellen erfolgt, wenn ein Logik-App-Webhooktrigger entfernt und gespeichert oder von „aktiviert“ in „deaktiviert“ umgeschaltet wird.

**So fügen Sie den Webhooktrigger hinzu**

1. Fügen Sie den Trigger **HTTP-Webhook** als ersten Schritt in einer Logik-App hinzu.
2. Geben Sie die Parameter für die Aufrufe zum Abonnieren und Abbestellen des Webhooks ein.

   Für diesen Schritt gilt dasselbe Muster wie beim Format für die [HTTP-Aktion](connectors-native-http.md).

     ![HTTP-Trigger](./media/connectors-native-webhook/using-trigger.png)

3. Fügen Sie mindestens eine Aktion hinzu.
4. Klicken Sie auf **Speichern**, um die Logik-App zu veröffentlichen. Durch diesen Schritt wird der Abonnementendpunkt mit der Rückruf-URL aufgerufen, die zum Auslösen dieser Logik-App erforderlich ist.
5. Immer wenn der Dienst `HTTP POST` an die Rückruf-URL sendet, wird die Logik-App ausgelöst, und sie enthält alle Daten, die in der Anforderung übergeben wurden.

## <a name="use-the-webhook-action"></a>Verwenden der Webhookaktion

Eine [*Aktion*](connectors-overview.md) ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Eine Webhookaktion registriert eine *Rückruf-URL* bei einem Dienst und wartet vor dem Fortsetzen auf den Aufruf der URL. [Genehmigungs-E-Mail senden](connectors-create-api-office365-outlook.md) ist ein Beispiel für einen Connector, der diesem Muster folgt. Sie können dieses Muster über die Webhookaktion auf jeden Dienst erweitern. 

Im Anschluss finden Sie ein Beispiel für die Einrichtung einer Webhookaktion im Logik-App-Designer. In diesen Schritten wird davon ausgegangen, dass Sie bereits eine API bereitgestellt haben oder auf eine API zugreifen, die dem [in Logik-Apps verwendeten Muster zum Abonnieren und Abbestellen von Webhooks Apps folgt](../logic-apps/logic-apps-create-api-app.md#webhook-actions). Der Aufruf zum Abonnieren erfolgt, wenn eine Logik-App die Webhookaktion ausführt. Der Aufruf zum Abbestellen erfolgt, wenn eine Ausführung beim Warten auf eine Antwort oder vor der Zeitüberschreitung für die Logik-App abgebrochen wird.

**So fügen Sie eine Webhookaktion hinzu**

1. Wählen Sie **Neuer Schritt** > **Aktion hinzufügen** aus.

2. Geben Sie im Suchfeld die Zeichenfolge „Webhook“ ein, um die Aktion **HTTP-Webhook** zu suchen.

    ![Auswählen der Abfrageaktion](./media/connectors-native-webhook/using-action-1.png)

3. Geben Sie die Parameter für die Aufrufe zum Abonnieren und Abbestellen des Webhooks ein.

   Für diesen Schritt gilt dasselbe Muster wie beim Format für die [HTTP-Aktion](connectors-native-http.md).

     ![Abschließen der Abfrageaktion](./media/connectors-native-webhook/using-action-2.png)
   
   Zur Laufzeit ruft die Logik-App nach Erreichen dieses Schritts den Abonnementendpunkt auf.

4. Klicken Sie auf **Speichern**, um die Logik-App zu veröffentlichen.

## <a name="technical-details"></a>Technische Details

Im Folgenden finden Sie weitere Details zu den von Webhooks unterstützten Triggern und Aktionen.

## <a name="webhook-triggers"></a>Webhooktrigger

| Aktion | Beschreibung |
| --- | --- |
| HTTP-Webhook |Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um die Logik-App nach Bedarf auszulösen. |

### <a name="trigger-details"></a>Triggerdetails

#### <a name="http-webhook"></a>HTTP-Webhook

Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um die Logik-App nach Bedarf auszulösen.
Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

| Anzeigename | Eigenschaftenname | Beschreibung |
| --- | --- | --- |
| Abonnieren: Methode* |method |Für die Abonnementanforderung zu verwendende HTTP-Methode |
| Abonnieren: URI* |uri |Für die Abonnementanforderung zu verwendender HTTP-URI |
| Abbestellen: Methode* |method |Für die Abbestellanforderung zu verwendende HTTP-Methode |
| Abbestellen: URI* |uri |Für die Abbestellanforderung zu verwendender HTTP-URI |
| Abonnieren: Text |body |HTTP-Anforderungstext für das Abonnement |
| Abonnieren: Header |headers |HTTP-Anforderungsheader für das Abonnement |
| Abonnieren: Authentifizierung |Authentifizierung |HTTP-Authentifizierung für das Abonnement. Für weitere Details siehe [HTTP-Connector](connectors-native-http.md#authentication). |
| Abbestellen: Text |body |HTTP-Anforderungstext für die Abbestellung |
| Abbestellen: Header |headers |HTTP-Anforderungsheader für die Abbestellung |
| Abbestellen: Authentifizierung |authentication |HTTP-Authentifizierung für die Abbestellung. Für weitere Details siehe [HTTP-Connector](connectors-native-http.md#authentication). |

**Ausgabedetails**

Webhookanforderung

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| headers |Objekt |Webhookanforderungsheader |
| body |Objekt |Webhookanforderungsobjekt |
| Statuscode |int |Statuscode der Webhookanforderung |

## <a name="webhook-actions"></a>Webhookaktionen

| Aktion | Beschreibung |
| --- | --- |
| HTTP-Webhook |Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um einen Workflowschritt nach Bedarf fortzusetzen. |

### <a name="action-details"></a>Aktionsdetails

#### <a name="http-webhook"></a>HTTP-Webhook

Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um einen Workflowschritt nach Bedarf fortzusetzen.
Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

| Anzeigename | Eigenschaftenname | Beschreibung |
| --- | --- | --- |
| Abonnieren: Methode* |method |Für die Abonnementanforderung zu verwendende HTTP-Methode |
| Abonnieren: URI* |uri |Für die Abonnementanforderung zu verwendender HTTP-URI |
| Abbestellen: Methode* |method |Für die Abbestellanforderung zu verwendende HTTP-Methode |
| Abbestellen: URI* |uri |Für die Abbestellanforderung zu verwendender HTTP-URI |
| Abonnieren: Text |body |HTTP-Anforderungstext für das Abonnement |
| Abonnieren: Header |headers |HTTP-Anforderungsheader für das Abonnement |
| Abonnieren: Authentifizierung |Authentifizierung |HTTP-Authentifizierung für das Abonnement. Für weitere Details siehe [HTTP-Connector](connectors-native-http.md#authentication). |
| Abbestellen: Text |body |HTTP-Anforderungstext für die Abbestellung |
| Abbestellen: Header |headers |HTTP-Anforderungsheader für die Abbestellung |
| Abbestellen: Authentifizierung |authentication |HTTP-Authentifizierung für die Abbestellung. Für weitere Details siehe [HTTP-Connector](connectors-native-http.md#authentication). |

**Ausgabedetails**

Webhookanforderung

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| headers |Objekt |Webhookanforderungsheader |
| body |Objekt |Webhookanforderungsobjekt |
| Statuscode |int |Statuscode der Webhookanforderung |

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md)
* [Andere Connectors](apis-list.md)
