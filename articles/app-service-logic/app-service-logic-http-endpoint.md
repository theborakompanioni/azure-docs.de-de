<properties
   pageTitle="Logik-Apps als aufrufbare Endpunkte"
   description="Gewusst wie: Erstellen und Konfigurieren von Trigger-Endpunkten und deren Verwendung in einer Logik-App in Azure App Service"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/10/2016"
   ms.author="jehollan"/>


# Logik-Apps als aufrufbare Endpunkte

Logik-Apps können einen synchronen HTTP-Endpunkt nativ als Trigger verfügbar machen. Sie können auch das Muster aufrufbarer Endpunkte verwenden, um Logik-Apps als geschachtelten Workflow über die Aktion „Workflow“ in einer Logik-App aufzurufen.

Es gibt drei Arten von Triggern, die Anforderungen erhalten können:

* Request
* ApiConnectionWebhook
* HttpWebhook

Im weiteren Verlauf des Artikels verwenden wir **request** als Beispiel, wobei sämtliche Prinzipien auch genauso für die anderen beiden Triggerarten gelten.

## Hinzufügen eines Triggers zur Definition
Im ersten Schritt wird der Definition Ihrer Logik-App ein Trigger hinzugefügt, der eingehende Anforderungen empfangen kann. Sie können im Designer nach „HTTP-Anforderung“ suchen, um die Trigger-Karte hinzuzufügen. Sie können ein JSON-Schema für den Anforderungstext definieren, sodass der Designer Token generiert, mit denen Sie Daten analysieren und vom manuellen Trigger durch den Workflow übergeben können. Empfohlen wird die Verwendung eines Tools wie z.B. [jsonschema.net](http://jsonschema.net), um ein JSON-Schema aus einer Beispieltextnutzlast zu generieren.

![Anforderungstriggerkarte][2]

Nach dem Speichern der Logik-App-Definition wird eine Rückruf-URL ähnlich der folgenden generiert:
 
``` text
https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?...
```

Diese URL enthält einen SAS-Schlüssel in den für die Authentifizierung verwendeten Abfrageparametern.

Sie können diesen Endpunkt auch im Azure-Portal abrufen:

![][1]

Oder durch Aufrufen von:

``` text
POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
```

## Aufrufen des Endpunkts des Triggers der Logik-App

Nachdem Sie den Endpunkt des Triggers erstellt haben, können Sie ihn über einen `POST`-Befehl mit der vollständigen URL aufrufen. Sie können zusätzliche Header hinzufügen und Inhalte in den Hauptteil einfügen.

Wenn „content-type“ `application/json` ist, können Sie innerhalb der Anforderung auf Eigenschaften verweisen. Andernfalls wird sie als einzelne binäre Einheit behandelt, die an andere APIs übergeben werden kann, auf die jedoch innerhalb des Workflows nicht verwiesen werden kann, ohne dass der Inhalt konvertiert wird. Wenn Sie beispielsweise `application/xml`-Inhalt übergeben, können Sie mit `@xpath()` eine XPath-Extraktion durchführen oder mit `@json()` XML in JSON konvertieren. Weitere Informationen zum Arbeiten mit Inhaltstypen [finden Sie hier](app-service-logic-content-type.md).

Darüber hinaus können Sie in der Definition ein JSON-Schema angeben. Dies bewirkt, dass der Designer Token generiert, die dann in den Schritten übergeben werden können. Im folgenden Beispiel werden ein `title`- und ein `name`-Token im Designer zur Verfügung gestellt:

```
{
    "properties":{
        "title": {
            "type": "string"
        },
        "name": {
            "type": "string"
        }
    },
    "required": [
        "title",
        "name"
    ],
    "type": "object"
}
```

## Verweisen auf den Inhalt der eingehenden Anforderung

Die `@triggerOutputs()`-Funktion gibt den Inhalt der eingehenden Anforderung aus. Dieser kann beispielsweise so aussehen:

```
{
    "headers" : {
        "content-type" : "application/json"
    },
    "body" : {
        "myprop" : "a value"
    }
}
```

Sie können über die Verknüpfung `@triggerBody()` speziell auf die `body`-Eigenschaft zugreifen.

## Reagieren auf die Anforderung

Bei einige Anforderungen, die eine Logik-App starten, können Sie mit Inhalten auf den Aufrufer reagieren. Es gibt einen neuen Aktionstyp **response**, der verwendet werden kann, um Statuscode, Hauptteil und Header für die Antwort zu erstellen. Beachten Sie Folgendes: Wenn kein **response**-Shape vorhanden ist, antwortet der Logik-App-Endpunkt *sofort* mit **202 – Zulässig**.

![HRRP-Antwortaktion][3]

``` json
"Response": {
            "conditions": [],
            "inputs": {
                "body": {
                    "name": "@{triggerBody()['name']}",
                    "title": "@{triggerBody()['title']}"
                },
                "headers": {
                    "content-type": "application/json"
                },
                "statusCode": 200
            },
            "type": "Response"
        }
```

Antworten haben die folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| statusCode | Der HTTP-Statuscode, mit dem auf die eingehende Anforderung reagiert wird. Möglich sind alle gültigen Statuscodes, die mit 2xx, 4xx oder 5xx beginnen. Mit 3xx beginnende Statuscodes sind nicht zulässig. | 
| body | Ein Hauptteilobjekt, das eine Zeichenfolge, ein JSON-Objekt oder sogar binäre Inhalte aufweisen kann, auf die in einem vorherigen Schritt verwiesen wird. | 
| headers | Sie können eine beliebige Anzahl von Headern definieren, die in der Antwort enthalten sein können. | 

Alle Schritte in der Logik-App, die für die Antwort erforderlich sind, müssen innerhalb von *60 Sekunden* abgeschlossen sein, damit die ursprüngliche Anforderung die Antwort empfängt, **es sei denn, der Workflow wird als geschachtelte Logik-App aufgerufen**. Wenn innerhalb von 60 Sekunden keine Antwortaktion erfolgt, kommt es bei der eingehenden Anforderung zu einem Timeout mit der HTTP-Antwort **408 – Clienttimeout**. Bei geschachtelten Logik-Apps wartet die übergeordnete Logik-App unabhängig von der Zeitdauer, bis die Antwort abgeschlossen ist.

## Erweiterte Endpunktkonfiguration

Logik-Apps bieten eine integrierte Unterstützung für den Direktzugriffsendpunkt und verwenden stets die `POST`-Methode, um die Ausführung der Logik-App zu starten. Die API-App **HTTP-Listener** unterstützte zuvor auch das Ändern der URL-Segmente und der HTTP-Methode. Sie konnten sogar eine zusätzliche Sicherheits- oder benutzerdefinierte Domäne einrichten, indem Sie sie dem API-App-Host hinzufügten (der Web-App, die die API-App gehostet hat).

Diese Funktionalität ist über **API Management** verfügbar:
* [Ändern der Methode der Anforderung ](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [Ändern der URL-Segmente der Anforderung ](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* Einrichten von API Management-Domänen auf der Registerkarte **Konfigurieren** im klassischen Azure-Portal
* Einrichten der Richtlinie zum Überprüfen auf Standardauthentifizierung (**Link erforderlich**)

## Vergleich der beiden Versionen hinsichtlich einer Migration

| 2014-12-01-preview | 2016-06-01 |
|---------------------|--------------------|
| Klicken auf die API-App **HTTP-Listener** | Klicken auf **Manueller Trigger** (keine API-App erforderlich) |
| HTTP-Listener-Einstellung *Sendet Antwort automatisch* | Hinzufügen oder Nichthinzufügen einer **response**-Aktion zur Workflowdefinition |
| Konfigurieren der Standard- oder OAuth-Authentifizierung | Über API Management |
| Konfigurieren der HTTP-Methode | Über API Management |
| Konfigurieren des relativen Pfads | Über API Management |
| Verweisen auf den eingehenden Hauptteil über `@triggerOutputs().body.Content` | Verweisen über `@triggerOutputs().body` |
| Aktion **HTTP-Antwort senden** im HTTP-Listener | Klicken auf **Auf HTTP-Anforderung reagieren** (keine API-App erforderlich)


[1]: ./media/app-service-logic-http-endpoint/manualtriggerurl.png
[2]: ./media/app-service-logic-http-endpoint/manualtrigger.png
[3]: ./media/app-service-logic-http-endpoint/response.png

<!---HONumber=AcomDC_0810_2016-->