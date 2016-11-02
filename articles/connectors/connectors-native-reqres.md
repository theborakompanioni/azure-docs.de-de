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

Mit den Anforderungs- und Antwortkomponenten in einer Logik-App können Sie in Echtzeit auf Ereignisse reagieren.

Dazu zählen z. B.:

- Reagieren Sie auf eine HTTP-Anforderung mit Daten aus einer lokalen Datenbank über eine Logik-App.
- Lösen Sie eine Logik-App über ein externes Webhook-Ereignis aus.
- Rufen Sie innerhalb einer Logik-App eine andere Logik-App mit einer Anforderungs- und Antwortaktion auf.

Wenn Sie Anforderungs- und Antwortaktionen in einer Logik-App verwenden möchten, müssen Sie zunächst eine [Logik-App erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Verwenden des HTTP-Anforderungstriggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md).

Im Anschluss finden Sie eine Beispielsequenz für die Einrichtung einer HTTP-Anforderung im Logik-App-Designer.

1. Fügen Sie in Ihrer Logik-App den Trigger **Request - When an HTTP request is received** (Anforderung – wenn eine HTTP-Anforderung empfangen wird) hinzu. Geben Sie ggf. (mit einem Tool wie [JSONSchema.net](http://jsonschema.net)) ein JSON-Schema für den Anforderungstext an. Dadurch kann der Designer Token für Eigenschaften in der HTTP-Anforderung generieren.
2. Fügen Sie eine weitere Aktion hinzu, um die Logik-App speichern zu können.
3. Nach dem Speichern der Logik-App können Sie die URL der HTTP-Anforderung über die Anforderungskarte ermitteln.
4. Ein (mit einem Tool wie [Postman](https://www.getpostman.com/)) an die URL gerichteter HTTP-POST-Vorgang löst die Logik-App aus.

>[AZURE.NOTE] Wenn Sie keine Antwortaktion definieren, wird umgehend eine `202 ACCEPTED`-Antwort an den Aufrufer zurückgegeben. Mithilfe der Antwortaktion können Sie die Antwort anpassen.

![Antworttrigger](./media/connectors-native-reqres/using-trigger.png)

## Verwenden der HTTP-Antwortaktion

Die HTTP-Antwortaktion ist nur gültig, wenn sie in einem durch eine HTTP-Anforderung ausgelösten Workflow verwendet wird. Wenn Sie keine Antwortaktion definieren, wird umgehend eine `202 ACCEPTED`-Antwort an den Aufrufer zurückgegeben. Sie können in jedem Schritt innerhalb des Workflows eine Antwortaktion hinzufügen. Die Logik-App wartet bei der eingehenden Anforderung nur eine Minute auf eine Antwort. Falls nach einer Minute noch keine Antwort vom Workflow gesendet wurde (und die Definition eine Antwortaktion enthält), wird `504 GATEWAY TIMEOUT` an den Aufrufer zurückgegeben.

Gehen Sie zum Hinzufügen einer HTTP-Antwortaktion wie folgt vor:

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.
2. Wählen Sie **Aktion hinzufügen** aus.
3. Geben Sie im Aktionssuchfeld die Zeichenfolge **response** ein, um die Antwortaktion anzuzeigen.

	![Auswählen der Antwortaktion](./media/connectors-native-reqres/using-action-1.png)

4. Geben Sie alle Parameter an, die Sie ggf. für die HTTP-Antwortnachricht benötigen.

	![Ausführen der Antwortaktion](./media/connectors-native-reqres/using-action-2.png)

5. Klicken Sie zum Speichern links oben auf die Symbolleiste. Dadurch wird Ihre Logik-App gespeichert und veröffentlicht (aktiviert).

## Anforderungstrigger

Hier finden Sie Details zu dem Trigger, den dieser Connector unterstützt. Ein einzelner Anforderungstrigger steht zur Verfügung.

|Trigger|Beschreibung|
|---|---|
|Request|Wird ausgeführt, wenn eine HTTP-Anforderung empfangen wird.|

## Antwortaktion

Hier finden Sie Details zu der Aktion, die dieser Connector unterstützt. Eine einzelne Antwortaktion steht zur Verfügung, und sie muss mit einem Anforderungstrigger kombiniert werden.

|Aktion|Beschreibung|
|---|---|
|Antwort|Gibt eine Antwort an die korrelierte HTTP-Anforderung zurück.|

### Trigger- und Aktionsdetails

Die folgenden Tabellen beschreiben die Eingabefelder für die Trigger und Aktionen sowie die entsprechenden Ausgabedetails.

#### Anforderungstrigger
Das Folgende ist ein Eingabefeld für den Trigger aus einer eingehenden HTTP-Anforderung.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|JSON-Schema|schema|Das JSON-Schema für den HTTP-Anforderungstext.|
<br>

**Ausgabedetails**

Im Folgenden werden die Ausgabedetails für die Anforderung angegeben.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Headers|Objekt|Anforderungsheader|
|Body|Objekt|Anforderungsobjekt|

#### Antwortaktion

Im Folgenden werden die Eingabefelder für die HTTP-Antwortaktion angegeben. Ein * bedeutet, dass es sich um ein Pflichtfeld handelt.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Statuscode*|statusCode|Der HTTP-Statuscode.|
|Header|headers|Ein JSON-Objekt für alle einzubeziehenden Antwortheader.|
|Body|body|Der Antworttext.|

## Nächste Schritte

Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

<!---HONumber=AcomDC_0810_2016-->