<properties
	pageTitle="Logik-App-Webhookconnector | Microsoft Azure"
	description="Übersicht über die Webhookaktionen und -trigger zum Ausführen von Aktionen wie „Array filtern“."
	services=""
	documentationCenter="" 
	authors="jeffhollan"
	manager="erikre"
	editor=""
	tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na" 
   ms.date="07/21/2016"
   ms.author="jehollan"/>

# Erste Schritte mit dem Webhookconnector

Mit der Webhookaktion und dem Webhooktrigger können Sie Datenflüsse auslösen, anhalten und fortsetzen, um Folgendes zu erreichen:

- Auslösen über [Azure Event Hub, wenn ein Element](https://github.com/logicappsio/EventHubAPI) empfangen wird
- Warten auf eine Genehmigung, bevor ein Workflow fortgesetzt wird

Informationen zum Erstellen einer API, die das Abonnieren von Webhooks unterstützt, finden [in diesem Artikel zum Erstellen von Logik-App-Connectors](../app-service-logic/app-service-logic-create-api-app.md).

---

## Verwenden des Webhooktriggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md). Ein Webhooktrigger ist besonders nützlich, da dafür keine neuen Elemente abgerufen werden müssen – wie beim [Anforderungstrigger](./connectors-native-reqres.md) wird die Logik-App ausgelöst, sobald ein Ereignis auftritt. Dies geschieht durch Registrieren einer *Rückruf-URL* bei einem Dienst, der verwendet werden kann, um die Logik-App bei Bedarf auszulösen.

Im Anschluss finden Sie eine Beispielsequenz für die Einrichtung eines HTTP-Triggers im Logik-App-Designer: Dabei wird davon ausgegangen, dass Sie bereits eine API bereitgestellt haben oder auf eine API zugreifen, die dem [in Logik-Apps verwendeten Muster zum Abonnieren und Abbestellen von Webhooks folgt](../app-service-logic/app-service-logic-create-api-app.md#webhook-triggers). Der Aufruf zum Abonnieren erfolgt, wenn eine Logik-App mit einem neuen Webhook gespeichert oder von „deaktiviert“ in „aktiviert“ umgeschaltet wird. Der Aufruf zum Abbestellen erfolgt, wenn ein Logik-App-Webhooktrigger entfernt und gespeichert oder von „aktiviert“ in „deaktiviert“ umgeschaltet wird.

1. Fügen Sie den Trigger **HTTP-Webhook** als ersten Schritt in einer Logik-App hinzu.
1. Geben Sie die Parameter für die Aufrufe zum Abonnieren und Abbestellen des Webhooks ein.
	- Hier gilt dasselbe Muster wie beim Format für die [HTTP-Aktion](./connectors-native-http.md).

	![HTTP-Trigger](./media/connectors-native-webhook/using-trigger.png)

1. Fügen Sie mindestens eine Aktion hinzu.
1. Klicken Sie auf „Speichern“, um die Logik-App zu veröffentlichen. Dadurch wird der Abonnementendpunkt mit der Rückruf-URL aufgerufen, die zum Auslösen dieser Logik-App erforderlich ist.
1. Immer wenn der Dienst `HTTP POST` an die Rückruf-URL sendet, wird die Logik-App ausgelöst (und enthält alle Daten, die in der Anforderung übergeben wurden).

## Verwenden der Webhookaktion
	
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. [Weitere Informationen zu Aktionen finden Sie hier.](connectors-overview.md) Eine Webhookaktion ist besonders hilfreich, da damit eine *Rückruf-URL* bei einem Dienst registriert und vor dem Fortsetzen auf den Aufruf der URL gewartet wird. [Genehmigungs-E-Mail senden](./connectors-create-api-office365-outlook.md) ist ein Beispiel für einen Connector, der diesem Muster folgt. Sie können dieses Muster über die Webhookaktion auf jeden Dienst erweitern. Dabei wird davon ausgegangen, dass Sie bereits eine API bereitgestellt haben oder auf eine API zugreifen, die dem [in Logik-Apps verwendeten Muster zum Abonnieren und Abbestellen von Webhooks folgt](../app-service-logic/app-service-logic-create-api-app.md#webhook-actions). Der Aufruf zum Abonnieren erfolgt, wenn eine Logik-App die Webhookaktion ausgeführt. Der Aufruf zum Abbestellen erfolgt, wenn eine Ausführung beim Warten auf eine Antwort oder vor der Zeitüberschreitung für die Logik-App abgebrochen wird.

So fügen Sie eine Webhookaktion hinzu

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.
1. Wählen Sie **Aktion hinzufügen** aus.
1. Geben Sie im Aktionssuchfeld die Zeichenfolge „Webhook“ ein, um die Aktion **HTTP-Webhook** anzuzeigen.

	![Auswählen der Abfrageaktion](./media/connectors-native-webhook/using-action-1.png)

1. Geben Sie die Parameter für die Aufrufe zum Abonnieren und Abbestellen des Webhooks ein.
	- Hier gilt dasselbe Muster wie beim Format für die [HTTP-Aktion](./connectors-native-http.md).

	![Abschließen der Abfrageaktion](./media/connectors-native-webhook/using-action-2.png)

	- Zur Laufzeit ruft die Logik-App den Abonnementendpunkt auf, sobald sie den Schritt erreicht.

1. Klicken Sie links oben auf der Symbolleiste auf „Speichern“. Dadurch wird Ihre Logik-App gespeichert und veröffentlicht (aktiviert).

---

## Technische Details

Im Folgenden finden Sie Details zu den von Webhooks unterstützten Triggern und Aktionen.

## Webhooktrigger

Ein Trigger ist ein Vorgang zum Starten eines Workflows. [Weitere Informationen zu Triggern finden Sie hier.](connectors-overview.md) Dieser Connector besitzt einen Trigger.

|Aktion|Beschreibung|
|---|---|
|HTTP-Webhook|Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um die Logik-App nach Bedarf auszulösen.|

### Triggerdetails

Für den Webhookconnector steht ein möglicher Trigger zur Verfügung. Im Folgenden finden Sie Informationen zu der Aktion und den erforderlichen und optionalen Eingabefeldern sowie entsprechende Ausgabedetails in Verbindung mit deren Verwendung.

#### HTTP-Webhook
Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um die Logik-App nach Bedarf auszulösen. Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Abonnieren: Methode*|method|Für die Abonnementanforderung zu verwendende HTTP-Methode|
|Abonnieren: URI*|uri|Für die Abonnementanforderung zu verwendender HTTP-URI|
|Abbestellen: Methode*|method|Für die Abbestellanforderung zu verwendende HTTP-Methode|
|Abbestellen: URI*|uri|Für die Abbestellanforderung zu verwendender HTTP-URI|
|Abonnieren: Text|body|HTTP-Anforderungstext für das Abonnement|
|Abonnieren: Header|headers|HTTP-Anforderungsheader für das Abonnement|
|Abonnieren: Authentifizierung|authentication|HTTP-Authentifizierung für das Abonnement. Details finden Sie unter [HTTP-Connector](./connectors-native-http.md#authenication).|
|Abbestellen: Text|body|HTTP-Anforderungstext für die Abbestellung|
|Abbestellen: Header|headers|HTTP-Anforderungsheader für die Abbestellung|
|Abbestellen: Authentifizierung|authentication|HTTP-Authentifizierung für die Abbestellung. Details finden Sie unter [HTTP-Connector](./connectors-native-http.md#authenication).|
<br>

**Ausgabedetails**

Webhookanforderung

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Header|Objekt|Webhookanforderungsheader|
|Body|Objekt|Webhookanforderungsobjekt|
|Statuscode|int|Statuscode der Webhookanforderung|

## Webhookaktionen

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. [Weitere Informationen zu Aktionen finden Sie hier.](connectors-overview.md) Der Connector verfügt über eine mögliche Aktion.

|Aktion|Beschreibung|
|---|---|
|HTTP-Webhook|Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um einen Workflowschritt nach Bedarf fortzusetzen.|

### Aktionsdetails

Für den Webhookconnector steht eine mögliche Aktion zur Verfügung. Im Folgenden finden Sie Informationen zu der Aktion und den erforderlichen und optionalen Eingabefeldern sowie entsprechende Ausgabedetails in Verbindung mit deren Verwendung.

#### HTTP-Webhook
Abonnieren Sie eine Rückruf-URL für einen Dienst, der die URL aufrufen kann, um einen Workflowschritt nach Bedarf fortzusetzen. Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Abonnieren: Methode*|method|Für die Abonnementanforderung zu verwendende HTTP-Methode|
|Abonnieren: URI*|uri|Für die Abonnementanforderung zu verwendender HTTP-URI|
|Abbestellen: Methode*|method|Für die Abbestellanforderung zu verwendende HTTP-Methode|
|Abbestellen: URI*|uri|Für die Abbestellanforderung zu verwendender HTTP-URI|
|Abonnieren: Text|body|HTTP-Anforderungstext für das Abonnement|
|Abonnieren: Header|headers|HTTP-Anforderungsheader für das Abonnement|
|Abonnieren: Authentifizierung|authentication|HTTP-Authentifizierung für das Abonnement. Details finden Sie unter [HTTP-Connector](./connectors-native-http.md#authentication).|
|Abbestellen: Text|body|HTTP-Anforderungstext für die Abbestellung|
|Abbestellen: Header|headers|HTTP-Anforderungsheader für die Abbestellung|
|Abbestellen: Authentifizierung|authentication|HTTP-Authentifizierung für die Abbestellung. Details finden Sie unter [HTTP-Connector](./connectors-native-http.md#authentication).|
<br>

**Ausgabedetails**

Webhookanforderung

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Header|Objekt|Webhookanforderungsheader|
|Body|Objekt|Webhookanforderungsobjekt|
|Statuscode|int|Statuscode der Webhookanforderung|

---

## Nächste Schritte

Im Anschluss finden Sie Details zur den weiteren Schritten mit Logik-Apps und unserer Community.

## Erstellen einer Logik-App

Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

<!---HONumber=AcomDC_0727_2016-->