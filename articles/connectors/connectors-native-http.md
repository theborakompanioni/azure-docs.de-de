<properties
	pageTitle="Hinzufügen der HTTP-Aktion in Logic Apps | Microsoft Azure"
	description="Übersicht über die HTTP-Aktion mit Eigenschaften"
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
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# Erste Schritte mit der HTTP-Aktion

Mit der HTTP-Aktion können Sie Workflows für Ihre Organisation erweitern und mit einem beliebigen Endpunkt über HTTP kommunizieren.

- Erstellen Sie Logik-App-Workflows, die aktiviert (durch einen Trigger ausgelöst) werden, wenn eine von Ihnen verwaltete Website ausfällt.
- Kommunizieren Sie mit jedem beliebigen Endpunkt über HTTP, um Ihre Workflows auf andere Dienste auszuweiten.

Wenn Sie die HTTP-Aktion in einer Logik-App verwenden möchten, müssen Sie zunächst eine [Logik-App erstellen](../app-service-logic/app-service-logic-create-a-logic-app.md).

---

## Verwenden eines HTTP-Triggers

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md).

Im Anschluss finden Sie eine Beispielsequenz für die Einrichtung eines HTTP-Triggers im Logik-App-Designer:

1. Fügen Sie den HTTP-Trigger Ihrer Logik-App hinzu.
1. Geben Sie die Parameter für den abzurufenden HTTP-Endpunkt an.
1. Ändern Sie das Wiederholungsintervall, um die gewünschte Abrufhäufigkeit anzugeben.
1. Die Logik-App wird nun mit den Inhalten ausgelöst, die bei den jeweiligen Überprüfungen zurückgegeben werden.

![HTTP-Trigger](./media/connectors-native-http/using-trigger.png)

### Funktionsweise des HTTP-Triggers

Der HTTP-Trigger ruft in einem bestimmten Wiederholungsintervall einen HTTP-Endpunkt auf. Standardmäßig führt jeder HTTP-Antwortcode < 300 dazu, dass die Logik-App ausgeführt wird. In der Codeansicht können Sie eine Bedingung hinzufügen, um nach dem HTTP-Aufruf eine Überprüfung durchzuführen und zu ermitteln, ob die Logik-App ausgelöst werden soll. Im Anschluss finden Sie ein Beispiel für einen HTTP-Trigger, der ausgelöst wird, wenn der zurückgegebene Statuscode größer oder gleich `400` ist.

```javascript
"Http": 
{
	"conditions": [
		{
			"expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
		}
	],
	"inputs": {
		"method": "GET",
		"uri": "https://blogs.msdn.microsoft.com/logicapps/",
		"headers": {
			"accept-language": "en"
		}
	},
	"recurrence": {
		"frequency": "Second",
		"interval": 15
	},
	"type": "Http"
}
```

Umfassende Informationen zu den Parametern für HTTP-Trigger finden Sie auf der [MSDN-Website](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## Verwenden einer HTTP-Aktion
	
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. Weitere Informationen zu Aktionen finden Sie [hier](connectors-overview.md).

1. Wählen Sie die Schaltfläche **Neuer Schritt** aus.
1. Wählen Sie **Aktion hinzufügen** aus.
1. Geben Sie im Aktionssuchfeld die Zeichenfolge „HTTP“ ein, um die HTTP-Aktion anzuzeigen.

	![Auswählen der HTTP-Aktion](./media/connectors-native-http/using-action-1.png)

1. Geben Sie alle Parameter an, die Sie ggf. für den HTTP-Aufruf benötigen.

	![Konfigurieren der HTTP-Aktion](./media/connectors-native-http/using-action-2.png)

1. Klicken Sie links oben auf der Symbolleiste auf „Speichern“. Dadurch wird Ihre Logik-App gespeichert und veröffentlicht (aktiviert).

---

## Technische Details

Im Anschluss finden Sie Details zu den von diesem Connector unterstützten Triggern und Aktionen.

## HTTP-Trigger

Ein Trigger ist ein Ereignis, mit dem ein in einer Logik-App definierter Workflow gestartet werden kann. Weitere Informationen zu Triggern finden Sie [hier](connectors-overview.md). Der HTTP-Connector besitzt einen Trigger.

|Trigger|Beschreibung|
|---|---|
|HTTP|Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück.|

## HTTP-Aktionen

Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird. [Weitere Informationen zu Aktionen finden Sie hier.](connectors-overview.md) Der HTTP-Connector verfügt über eine mögliche Aktion.

|Aktion|Beschreibung|
|---|---|
|HTTP|Führt einen HTTP-Aufruf durch und gibt den Antwortinhalt zurück.|

## HTTP-Details

Für den HTTP-Connector steht eine mögliche Aktion zur Verfügung. Im Anschluss finden Sie Informationen zu den einzelnen Aktionen und den erforderlichen und optionalen Eingabefeldern sowie entsprechende Ausgabedetails in Verbindung mit deren Verwendung.

### HTTP-Anforderung
Führt eine ausgehende HTTP-Anforderung durch. Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Anzeigename|Eigenschaftenname|Beschreibung|
|---|---|---|
|Methode*|method|Zu verwendendes HTTP-Verb|
|URI*|uri|URI für die HTTP-Anforderung|
|Header|headers|Ein JSON-Objekt für die einzubeziehenden Header|
|Body|body|Der HTTP-Anforderungstext|
|Authentifizierung|authentication|[Details finden Sie hier](#authentication)|
<br>

**Ausgabedetails**

HTTP-Antwort

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Header|Objekt|Antwortheader|
|Body|Objekt|Antwortobjekt|
|Statuscode|int|HTTP-Statuscode|

## Authentifizierung

Durch Logik-Apps können Sie verschiedene Arten von Authentifizierung bei HTTP-Endpunkten verwenden. Diese Authentifizierung kann mit den Connectors HTTP, [HTTP + Swagger](./connectors-native-http-swagger.md) und [HTTP-Webhook](./connectors-native-webhook.md) genutzt werden. Die folgenden Arten der Authentifizierung können konfiguriert werden:

* [Standardauthentifizierung](#basic-authentication)
* [ClientCertificate-Authentifizierung](#client-certificate-authentication)
* [ActiveDirectoryOAuth-Authentifizierung](#azure-active-directory-oauth-authentication)

#### Standardauthentifizierung

Das folgende Authentifizierungsobjekt ist für die Standardauthentifizierung erforderlich: Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Typ*|Typ|Die Art der Authentifizierung. Für die Standardauthentifizierung muss der Wert `Basic` lauten.|
|Benutzername*|username|Der zu authentifizierende Benutzername|
|Kennwort*|password|Das zu authentifizierende Kennwort|

>[AZURE.TIP] Wenn Sie ein Kennwort verwenden möchten, das aus der Definition nicht abgerufen werden kann, verwenden Sie einen `securestring`-Parameter und die `@parameters()`-[Funktion für die Workflowdefinition](http://aka.ms/logicappdocs).

Sie erstellen also ein Objekt wie das folgende im Authentifizierungsfeld:

```javascript
{
	"type": "Basic",
	"username": "user",
	"password": "test"
}
```

#### Clientzertifikatsauthentifizierung

Das folgende Authentifizierungsobjekt ist für die Clientzertifikatsauthentifizierung erforderlich: Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Typ*|Typ|Die Art der Authentifizierung. Für SSL-Clientzertifikate muss der Wert `ClientCertificate` lauten.|
|PFX*|pfx|Base64-codierte Inhalte der PFX-Datei|
|Kennwort*|password|Kennwort für den Zugriff auf die PFX-Datei|

>[AZURE.TIP] Sie können einen `securestring`-Parameter und die `@parameters()`-[Funktion für die Workflowdefinition](http://aka.ms/logicappdocs) verwenden, um einen Parameter zu nutzen, der in der Definition nach dem Speichern nicht lesbar ist.

Beispiel:

```javascript
{
	"type": "ClientCertificate",
	"pfx": "aGVsbG8g...d29ybGQ=",
	"password": "@parameters('myPassword')"
}
```

#### Azure Active Directory OAuth-Authentifizierung

Das folgende Authentifizierungsobjekt ist für die Azure Active Directory OAuth-Authentifizierung erforderlich: Mit einem Sternchen gekennzeichnete Felder sind Pflichtfelder.

|Eigenschaftenname|Datentyp|Beschreibung|
|---|---|---|
|Typ*|Typ|Die Art der Authentifizierung. Für ActiveDirectoryOAuth muss der Wert `ActiveDirectoryOAuth` lauten.|
|Mandant*|tenant|Die Mandanten-ID für den Azure AD-Mandanten|
|Zielgruppe*|audience|Dieser Wert wird auf `https://management.core.windows.net/` festgelegt.|
|Client-ID*|clientId|Geben Sie die Client-ID für die Azure AD-Anwendung an|
|Geheimer Schlüssel*|secret|Der geheime Schlüssel des Clients, der das Token anfordert|

>[AZURE.TIP] Sie können einen `securestring`-Parameter und die `@parameters()`-[Funktion für die Workflowdefinition](http://aka.ms/logicappdocs) verwenden, um einen Parameter zu nutzen, der in der Definition nach dem Speichern nicht lesbar ist.

Beispiel:

```javascript
{
	"type": "ActiveDirectoryOAuth",
	"tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
	"audience": "https://management.core.windows.net/",
	"clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
	"secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

---

## Nächste Schritte

Im Anschluss finden Sie Details zur den weiteren Schritten mit Logik-Apps und unserer Community.

## Erstellen einer Logik-App

Testen Sie nun die Plattform, und [erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md). Machen Sie sich ggf. anhand unserer [API-Liste](apis-list.md) mit den anderen verfügbaren Connectors für Logik-Apps vertraut.

<!---HONumber=AcomDC_0727_2016-->