<properties
	pageTitle="Verwenden von Anforderungs- und Antwortaktionen | Microsoft Azure"
	description="Übersicht über den Anforderungs- und Antworttrigger und die Anforderungs- und Antwortaktion in einer Azure-Logik-App"
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
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# Erste Schritte mit den Anforderungs- und Antwortkomponenten

Mit den Anforderungs- und Antwortkomponenten in einer Logik-App können Sie beispielsweise in folgenden Szenarien in Echtzeit auf Ereignisse reagieren:

- Reagieren Sie auf eine HTTP-Anforderung mit Daten aus einer lokalen Datenbank über eine Logik-App.
- Lösen Sie eine Logik-App über ein externes Webhook-Ereignis aus.
- Rufen Sie innerhalb einer Logik-App eine andere Logik-App mit einer Anforderungs- und Antwortaktion auf.

Wenn Sie Anforderungs- und Antwortaktionen in einer Logik-App verwenden möchten, müssen Sie zunächst eine [Logik-App erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## Verwenden des HTTP-Anforderungstriggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md).

Im Anschluss finden Sie eine Beispielsequenz für die Einrichtung einer HTTP-Anforderung im Logik-App-Designer:

1. Fügen Sie in Ihrer Logik-App den Trigger **Request - When an HTTP request is received** (Anforderung – Wenn eine HTTP-Anforderung empfangen wird) hinzu.
	- Geben Sie ggf. mit einem Tool wie [jsonschema.net](http://jsonschema.net) ein JSON-Schema für den Anforderungstext an. Dadurch kann der Designer Token für Eigenschaften in der HTTP-Anforderung generieren.
1. Fügen Sie eine weitere Aktion hinzu, um die Logik-App speichern zu können.
1. Nach dem Speichern können Sie die URL der HTTP-Anforderung über die Anforderungskarte ermitteln.
1. Die Logik-App wird durch einen an die URL gerichteten HTTP-POST-Vorgang (mit einem Tool wie [Postman](https://www.getpostman.com/)) ausgelöst.

>[AZURE.NOTE] Wenn Sie keine Antwortaktion definieren, wird an den Aufrufer umgehend eine `202 ACCEPTED`-Antwort zurückgegeben. Mithilfe der Antwortaktion können Sie die Antwort anpassen.

![Antworttrigger](./media/connectors-native-reqres/using-trigger.png)

## Verwenden der HTTP-Antwortaktion
	
Die HTTP-Antwortaktion ist nur gültig, wenn Sie in einem durch eine HTTP-Anforderung ausgelösten Workflow verwendet wird. Wenn Sie keine Antwortaktion definieren, wird an den Aufrufer umgehend eine `202 ACCEPTED`-Antwort zurückgegeben. Eine Antwortaktion kann in jedem Schritt innerhalb des Workflows hinzugefügt werden. Die Logik-App wartet bei der eingehenden Anforderung nur eine Minute auf eine Antwort. Falls nach einer Minute noch keine Antwort vom Workflow gesendet wurde (und in der Definition eine Antwortaktion vorhanden ist) wird `504 GATEWAY TIMEOUT` an den Aufrufer zurückgegeben. Gehen Sie zum Hinzufügen einer HTTP-Antwortaktion wie folgt vor:

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.
1. Wählen Sie **Aktion hinzufügen** aus.
1. Geben Sie im Aktionssuchfeld die Zeichenfolge „Response“ ein, um die Antwortaktion anzuzeigen.

	![Auswählen der Antwortaktion](./media/connectors-native-reqres/using-action-1.png)

1. Geben Sie alle Parameter an, die Sie ggf. für die HTTP-Antwortnachricht benötigen.

	![Konfigurieren der Antwortaktion](./media/connectors-native-reqres/using-action-2.png)

1. Klicken Sie links oben auf der Symbolleiste auf „Speichern“. Dadurch wird Ihre Logik-App gespeichert und veröffentlicht (aktiviert).

---

## Technische Details

Im Anschluss finden Sie Details zu den von diesem Connector unterstützten Triggern und Aktionen.

## Anforderungstrigger

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md). Ein einzelner Anforderungstrigger steht zur Verfügung.

|Trigger|Beschreibung|
|---|---|
|Anforderung|Wenn eine HTTP-Anforderung empfangen wird|

## Antwortaktion

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](connectors-overview.md). Eine einzelne Antwortaktion steht zur Verfügung, und sie muss mit einem Anforderungstrigger kombiniert werden.

|Aktion|Beschreibung|
|---|---|
|Antwort|Antwort an die korrelierte HTTP-Anforderung zurückgeben|

### Aktionsdetails

Im Anschluss finden Sie Informationen zu den einzelnen Aktionen und den erforderlichen und optionalen Eingabefeldern sowie entsprechende Ausgabedetails in Verbindung mit deren Verwendung.

#### Anforderungstrigger
Trigger aus einer eingehenden HTTP-Anforderung. Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|JSON-Schema|schema|Das JSON-Schema für den HTTP-Anforderungstext.|
<br>

**Ausgabedetails**

Anforderung

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Header|Objekt|Anforderungsheader|
|Body|Objekt|Anforderungsobjekt|

#### Antwortaktion

HTTP-Antwort. Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Statuscode*|statusCode|Der HTTP-Statuscode.|
|Header|headers|Ein JSON-Objekt für alle einzubeziehenden Antwortheader.|
|Body|body|Der Antworttext.|

## Nächste Schritte

Im Anschluss finden Sie Details zur den weiteren Schritten mit Logik-Apps und unserer Community.

## Erstellen einer Logik-App

Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

<!---HONumber=AcomDC_0727_2016-->