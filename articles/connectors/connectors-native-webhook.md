---
title: Logik-App-Webhookconnector | Microsoft Docs
description: "Übersicht über die Webhookaktionen und -trigger zum Ausführen von Aktionen wie „Array filtern“."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 71775384-6c3a-482c-a484-6624cbe4fcc7
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/21/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: c0edbe421050ad46f6d31fd6416df4b344b233ad
ms.openlocfilehash: 2c24699f6253fc7952b331ebcce4b937f1c83603
ms.lasthandoff: 02/16/2017


---
# <a name="get-started-with-the-webhook-connector"></a>Erste Schritte mit dem Webhookconnector
Mit der Webhookaktion und dem Webhooktrigger können Sie Datenflüsse auslösen, anhalten und fortsetzen, um Folgendes zu erreichen:

* Auslösen über [Azure Event Hub, wenn ein Element](https://github.com/logicappsio/EventHubAPI) empfangen wird
* Warten auf eine Genehmigung, bevor ein Workflow fortgesetzt wird

Informationen zum Erstellen einer API, die das Abonnieren von Webhooks unterstützt, finden [in diesem Artikel zum Erstellen von Logik-App-Connectors](../logic-apps/logic-apps-create-api-app.md).

- - -
## <a name="use-the-webhook-trigger"></a>Verwenden des Webhooktriggers
Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md).  Ein Webhooktrigger ist besonders nützlich, da dafür keine neuen Elemente abgerufen werden müssen – wie beim [Anforderungstrigger](connectors-native-reqres.md) wird die Logik-App ausgelöst, sobald ein Ereignis auftritt.  Dies geschieht durch Registrieren einer *Rückruf-URL* bei einem Dienst, der verwendet werden kann, um die Logik-App bei Bedarf auszulösen.

Im Anschluss finden Sie eine Beispielsequenz für die Einrichtung eines HTTP-Triggers im Logik-App-Designer:  Dabei wird davon ausgegangen, dass Sie bereits eine API bereitgestellt haben oder auf eine API zugreifen, die dem [in Logik-Apps verwendeten Muster zum Abonnieren und Abbestellen von Webhooks folgt](../logic-apps/logic-apps-create-api-app.md#webhook-triggers).  Der Aufruf zum Abonnieren erfolgt, wenn eine Logik-App mit einem neuen Webhook gespeichert oder von „deaktiviert“ in „aktiviert“ umgeschaltet wird.  Der Aufruf zum Abbestellen erfolgt, wenn ein Logik-App-Webhooktrigger entfernt und gespeichert oder von „aktiviert“ in „deaktiviert“ umgeschaltet wird.

1. Fügen Sie den Trigger **HTTP-Webhook** als ersten Schritt in einer Logik-App hinzu.
2. Geben Sie die Parameter für die Aufrufe zum Abonnieren und Abbestellen des Webhooks ein.

   * Hier gilt dasselbe Muster wie beim Format für die [HTTP-Aktion](connectors-native-http.md).

     ![HTTP-Trigger](./media/connectors-native-webhook/using-trigger.png)
3. Fügen Sie mindestens eine Aktion hinzu.
4. Klicken Sie auf „Speichern“, um die Logik-App zu veröffentlichen. Dadurch wird der Abonnementendpunkt mit der Rückruf-URL aufgerufen, die zum Auslösen dieser Logik-App erforderlich ist.
5. Immer wenn der Dienst `HTTP POST` an die Rückruf-URL sendet, wird die Logik-App ausgelöst (und enthält alle Daten, die in der Anforderung übergeben wurden).

## <a name="use-the-webhook-action"></a>Verwenden der Webhookaktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. [Weitere Informationen zu Aktionen finden Sie hier.](connectors-overview.md)  Eine Webhookaktion ist besonders hilfreich, da damit eine *Rückruf-URL* bei einem Dienst registriert und vor dem Fortsetzen auf den Aufruf der URL gewartet wird.  [Genehmigungs-E-Mail senden](connectors-create-api-office365-outlook.md) ist ein Beispiel für einen Connector, der diesem Muster folgt.  Sie können dieses Muster über die Webhookaktion auf jeden Dienst erweitern.  Dabei wird davon ausgegangen, dass Sie bereits eine API bereitgestellt haben oder auf eine API zugreifen, die dem [in Logik-Apps verwendeten Muster zum Abonnieren und Abbestellen von Webhooks folgt](../logic-apps/logic-apps-create-api-app.md#webhook-actions).  Der Aufruf zum Abonnieren erfolgt, wenn eine Logik-App die Webhookaktion ausgeführt.  Der Aufruf zum Abbestellen erfolgt, wenn eine Ausführung beim Warten auf eine Antwort oder vor der Zeitüberschreitung für die Logik-App abgebrochen wird.

So fügen Sie eine Webhookaktion hinzu

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.
2. Wählen Sie **Aktion hinzufügen**
3. Geben Sie im Aktionssuchfeld die Zeichenfolge „Webhook“ ein, um die Aktion **HTTP-Webhook** anzuzeigen.

    ![Auswählen der Abfrageaktion](./media/connectors-native-webhook/using-action-1.png)
4. Geben Sie die Parameter für die Aufrufe zum Abonnieren und Abbestellen des Webhooks ein.

   * Hier gilt dasselbe Muster wie beim Format für die [HTTP-Aktion](connectors-native-http.md).

     ![Abschließen der Abfrageaktion](./media/connectors-native-webhook/using-action-2.png)
   * Zur Laufzeit ruft die Logik-App den Abonnementendpunkt auf, sobald sie den Schritt erreicht.
5. Klicken Sie links oben auf der Symbolleiste auf „Speichern“. Dadurch wird Ihre Logik-App gespeichert und veröffentlicht (aktiviert).

- - -
## <a name="technical-details"></a>Technische Details
Im Folgenden finden Sie Details zu den von Webhooks unterstützten Triggern und Aktionen.

## <a name="webhook-triggers"></a>Webhooktrigger
Ein Trigger ist ein Vorgang zum Starten eines Workflows. Weitere Informationen zu Triggern finden Sie [hier.](connectors-overview.md) Dieser Connector besitzt einen Trigger.

| anzuzeigen. | Beschreibung |
| --- | --- |
| HTTP-Webhook |Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um die Logik-App nach Bedarf auszulösen. |

### <a name="trigger-details"></a>Triggerdetails
Für den Webhookconnector steht ein möglicher Trigger zur Verfügung. Im Folgenden finden Sie Informationen zu der Aktion und den erforderlichen und optionalen Eingabefeldern sowie entsprechende Ausgabedetails in Verbindung mit deren Verwendung.

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
| Abonnieren: Authentifizierung |authentication |HTTP-Authentifizierung für das Abonnement. Für weitere Details siehe [HTTP-Connector](connectors-native-http.md#authentication). |
| Abbestellen: Text |body |HTTP-Anforderungstext für die Abbestellung |
| Abbestellen: Header |headers |HTTP-Anforderungsheader für die Abbestellung |
| Abbestellen: Authentifizierung |authentication |HTTP-Authentifizierung für die Abbestellung. Für weitere Details siehe [HTTP-Connector](connectors-native-http.md#authentication). |

<br>

**Ausgabedetails**

Webhookanforderung

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| headers |Objekt |Webhookanforderungsheader |
| body |Objekt |Webhookanforderungsobjekt |
| Statuscode |int |Statuscode der Webhookanforderung |

## <a name="webhook-actions"></a>Webhookaktionen
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. [Weitere Informationen zu Aktionen finden Sie hier.](connectors-overview.md) Der Connector verfügt über eine mögliche Aktion.

| anzuzeigen. | Beschreibung |
| --- | --- |
| HTTP-Webhook |Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um einen Workflowschritt nach Bedarf fortzusetzen. |

### <a name="action-details"></a>Aktionsdetails
Für den Webhookconnector steht eine mögliche Aktion zur Verfügung. Im Folgenden finden Sie Informationen zu der Aktion und den erforderlichen und optionalen Eingabefeldern sowie entsprechende Ausgabedetails in Verbindung mit deren Verwendung.

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
| Abonnieren: Authentifizierung |authentication |HTTP-Authentifizierung für das Abonnement. Für weitere Details siehe [HTTP-Connector](connectors-native-http.md#authentication). |
| Abbestellen: Text |body |HTTP-Anforderungstext für die Abbestellung |
| Abbestellen: Header |headers |HTTP-Anforderungsheader für die Abbestellung |
| Abbestellen: Authentifizierung |authentication |HTTP-Authentifizierung für die Abbestellung. Für weitere Details siehe [HTTP-Connector](connectors-native-http.md#authentication). |

<br>

**Ausgabedetails**

Webhookanforderung

| Eigenschaftenname | Datentyp | Beschreibung |
| --- | --- | --- |
| headers |Objekt |Webhookanforderungsheader |
| body |Objekt |Webhookanforderungsobjekt |
| Statuscode |int |Statuscode der Webhookanforderung |

- - -
## <a name="next-steps"></a>Nächste Schritte
Im Anschluss finden Sie Details zur den weiteren Schritten mit Logik-Apps und unserer Community.

## <a name="create-a-logic-app"></a>Erstellen einer Logik-App
Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

