---
title: "Aufrufen, Auslösen oder Schachteln von Logik-Apps über HTTP-Endpunkte – Azure Logic Apps | Microsoft-Dokumentation"
description: "Hinzufügen und Konfigurieren von HTTP-Endpunkten zum Aufrufen, Auslösen oder Schachteln von Workflows für Logik-Apps in Azure"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 73ba2a70-03e9-4982-bfc8-ebfaad798bc2
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.custom: H1Hack27Feb2017
ms.date: 10/18/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 1e6ae31b3ef2d9baf578b199233e61936aa3528e
ms.openlocfilehash: 8c4cb0aefcd7241b17c6afcf33496c1fc2e6f675
ms.lasthandoff: 03/03/2017

---

# <a name="add-http-endpoints-to-call-trigger-or-nest-logic-app-workflows"></a>Hinzufügen von HTTP-Endpunkten zum Aufrufen, Auslösen, oder Schachteln von Logik-App-Workflows

Logik-Apps können synchrone HTTP-Endpunkte nativ als Auslöser verfügbar machen, sodass Sie Ihre Logik-Apps aufrufen können. Sie können auch ein Muster aufrufbarer Endpunkte verwenden, um Logik-Apps als geschachtelten Workflow über die Aktion **Logic Apps-Workflow wählen** in Ihrer Logik-App aufzurufen.

Sie können folgende Auslöser zum Erhalten von Anfragen verwenden:

* Request
* ApiConnectionWebhook
* HttpWebhook

Dieses Thema verwendet den Auslöser **Request** (Anforderung) als Beispiele, jedoch sind alle Prinzipien identisch mit den anderen Triggertypen.

## <a name="add-a-trigger-to-your-logic-app-definition"></a>Hinzufügen eines Triggers zu Ihrer Logik-App-Definition

1. Fügen Sie im Logik-App-Designer einen Trigger hinzu, der eingehende Anfragen für Ihre Logik-App-Definition empfangen kann. Fügen Sie Ihrer Logik-App z.B. den **Anforderungstrigger** hinzu.

2.    Sie können unter **ein JSON-Schema für den Anforderungstext** ein JSON-Schema für die Nutzlast eingeben, die Sie erwarten. Wenn Sie kein Schema haben, können Sie **Beispielnutzlast zum Generieren eines Schemas verwenden** auswählen, um ein JSON-Schema für eine Beispielnutzlast zu generieren.

    Der Designer verwendet dieses Schema zum Generieren von Token, die Ihnen beim Nutzen, Analysieren und Übergeben von Daten vom manuellen Trigger über Ihren Workflow helfen.

    ![Hinzufügen der Anforderungsaktion][2]

2.    Nachdem Sie Ihre Logik-App-Definition unter **HTTP POST an diese URL** gespeichert haben, erhalten Sie eine generierte Rückruf-URL, wie in diesem Beispiel gezeigt:

    ``` text
    https://prod-03.eastus.logic.azure.com:443/workflows/080cb66c52ea4e9cabe0abf4e197deff/triggers/myendpointtrigger?*signature*...
    ```

    Diese URL enthält einen Shared Access Signature-Schlüssel (SAS) in den für die Authentifizierung verwendeten Abfrageparametern. 
    Sie können diesen Endpunkt auch im Azure-Portal abrufen:

    ![URL-Endpunkt][1]

    Oder durch Aufrufen von:

    ``` text
    POST https://management.azure.com/{resourceID of your logic app}/triggers/myendpointtrigger/listCallbackURL?api-version=2015-08-01-preview
    ```

### <a name="change-the-http-method-for-your-trigger"></a>Ändern der HTTP-Methode für Ihren Trigger

Standardmäßig erwartet der Trigger eine HTTP POST-Anforderung. Um eine andere HTTP-Methode zu konfigurieren, wählen Sie **Erweiterte Optionen anzeigen** aus.

> [!NOTE]
> Es ist nur eine Methode zulässig.

### <a name="customize-the-relative-trigger-url"></a>Anpassen der Relativen Trigger-URL

Sie können auch den relativen Pfad für die Anforderungs-URL anpassen, um Parameter zu übernehmen.

1. Wählen Sie auf dem **Anforderungstrigger** **Erweiterte Optionen anzeigen** aus. Geben Sie unter **Relativer Pfad** `customer/{customerId}` ein.

    ![Relativer URL-Trigger](./media/logic-apps-http-endpoint/relativeurl.png)

2.    Um den Parameter zu verwenden, aktualisieren Sie die **Response**-Aktion (Antwortaktion).

    *    Daraufhin sollte `customerId` in der Tokenauswahl angezeigt werden.
    *    Aktualisieren Sie den Text der **Antwort**aktion, damit `Hello {customerId}` zurückgegeben wird.

    ![Relative URL-Antwort](./media/logic-apps-http-endpoint/relativeurlresponse.png)

3. Speichern Sie Ihre Logik-App. Die Anforderungs-URL wird mit dem relativen Pfad aktualisiert.

4. Kopieren Sie die neue Anforderungs-URL, und fügen Sie sie in einem neuen Browserfenster ein. Ersetzen Sie `{customerId}` durch `123`, und drücken Sie die EINGABETASTE.

    Folgender Text sollte zurückgegeben werden: `Your customer Id is 123`

### <a name="security-for-the-trigger-url"></a>Sicherheit für die Trigger-URL

Rückruf-URLs für Logik-Apps werden sicher über eine Shared Access Signature (SAS) generiert. Die Signatur wird als Abfrageparameter übergeben und muss geprüft werden, bevor Ihre Logik-App ausgelöst werden kann. Die Signatur wird durch eine eindeutige Kombination aus einem geheimen Schlüssel pro Logik-App, dem Namen des Triggers und dem ausgeführten Vorgang generiert. Nur wenn ein Benutzer Zugriff auf den geheimen Logik-App-Schlüssel hat, kann er eine gültige Signatur generieren.

## <a name="call-your-logic-app-triggers-endpoint"></a>Aufrufen des Endpunkts des Triggers der Logik-App

Nachdem Sie den Endpunkt des Triggers erstellt haben, können Sie Ihre Logik-App über einen `POST`-Befehl mit der vollständigen URL aufrufen. Sie können weitere Header hinzufügen und Inhalte in den Hauptteil einfügen. Wenn „content-type“ `application/json` ist, können Sie innerhalb der Anforderung auf Eigenschaften verweisen. Andernfalls wird der Inhalt als einzelne binäre Einheit behandelt, die an andere APIs übergeben werden kann, auf die jedoch innerhalb des Workflows nicht verwiesen werden kann, ohne dass der Inhalt konvertiert wird. Wenn Sie beispielsweise `application/xml`-Inhalt übergeben, können Sie mit `@xpath()` eine XPath-Extraktion durchführen oder mit `@json()` XML in JSON konvertieren. Erfahren Sie mehr über das [Arbeiten mit Inhaltstypen](../logic-apps/logic-apps-content-type.md).

Darüber hinaus können Sie in der Definition ein JSON-Schema angeben. Dieses Schema bewirkt, dass der Designer Token generiert, die in den Schritten übergeben werden können. Im folgenden Beispiel werden ein `title`- und ein `name`-Token im Designer zur Verfügung gestellt:

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

## <a name="reference-the-content-from-the-incoming-request"></a>Verweisen auf den Inhalt der eingehenden Anforderung

Die `@triggerOutputs()`-Funktion gibt den Inhalt der eingehenden Anforderung aus. Die Ausgabe sieht wie folgt aus:

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

Sie können über die Verknüpfung `body` speziell auf die `@triggerBody()`-Eigenschaft zugreifen. 

## <a name="respond-to-the-request"></a>Antworten auf die Anforderung

Bei einige Anforderungen, die eine Logik-App starten, können Sie möglicherweise mit Inhalten auf den Aufrufer reagieren. Sie können einen neuen Aktionstyp namens **response** verwenden, um Statuscode, Hauptteil und Header für die Antwort zu erstellen. Wenn jedoch keine **Antwort** enthalten ist, gibt der Logik-App-Endpunkt *sofort* **202 Zulässig** zurück.

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

Antworten haben folgende Eigenschaften:

| Eigenschaft | Beschreibung |
| --- | --- |
| statusCode |Der HTTP-Statuscode, mit dem auf die eingehende Anforderung reagiert wird. Dieser Code kann jeder gültige Statuscode sein, der mit 2xx, 4xx oder 5xx beginnt. Mit 3xx beginnende Statuscodes sind jedoch nicht zulässig. |
| body |Ein Hauptteilobjekt, das eine Zeichenfolge, ein JSON-Objekt oder sogar binäre Inhalte aufweisen kann, auf die in einem vorherigen Schritt verwiesen wird. |
| headers |Sie können eine beliebige Anzahl von Headern definieren, die in der Antwort enthalten sein können. |

In Ihrer Logik-App müssen alle erforderlichen Schritte für die Antwort innerhalb von *60 Sekunden* beendet sein, damit die ursprüngliche Anforderung die Antwort enthält, *es sei denn, Sie rufen den Workflow als geschachtelte Logik-App auf*. Wenn innerhalb von 60 Sekunden keine Antwort erfolgt, kommt es bei der eingehenden Anforderung zu einem Timeout mit der HTTP-Antwort **408 – Clienttimeout** . Bei geschachtelten Logik-Apps wartet die übergeordnete Logik-App unabhängig von der Zeitdauer, bis die Antwort abgeschlossen ist.

## <a name="advanced-endpoint-configuration"></a>Erweiterte Endpunktkonfiguration

Logik-Apps bieten eine integrierte Unterstützung für den Direktzugriffsendpunkt und verwenden stets die `POST`-Methode, um die Ausführung der Logik-App zu starten. Die API-App **HTTP-Listener** unterstützte zuvor auch das Ändern der URL-Segmente und der HTTP-Methode. Sie können sogar eine zusätzliche Sicherheits- oder benutzerdefinierte Domäne einrichten, indem Sie diese Elemente dem API-App-Host hinzufügten (der Web-App, die die API-App gehostet hat). 

Diese Funktionalität ist über **API Management**verfügbar:

* [Ändern der Anforderungsmethode](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod)
* [Ändern der URL-Segmente der Anforderung](https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL)
* Einrichten von API Management-Domänen auf der Registerkarte **Konfigurieren** im klassischen Azure-Portal
* Einrichten der Richtlinie zum Überprüfen auf Standardauthentifizierung

## <a name="summary-of-migration-from-2014-12-01-preview"></a>Vergleich der beiden Versionen hinsichtlich einer Migration

| 2014-12-01-preview | 2016-06-01 |
| --- | --- |
| Klicken auf die API-App **HTTP-Listener** |Klicken auf **Manueller Trigger** (keine API-App erforderlich) |
| HTTP-Listener-Einstellung*Sendet Antwort automatisch* |Hinzufügen oder Nichthinzufügen einer **response** -Aktion zur Workflowdefinition |
| Konfigurieren der Standard- oder OAuth-Authentifizierung |Über API Management |
| Konfigurieren der HTTP-Methode |Über API Management |
| Konfigurieren des relativen Pfads |Über API Management |
| Verweisen auf den eingehenden Hauptteil über `@triggerOutputs().body.Content` |Verweisen über `@triggerOutputs().body` |
| **HTTP-Antwort senden** im HTTP-Listener |Klicken auf **Auf HTTP-Anforderung reagieren** (keine API-App erforderlich) |

[1]: ./media/logic-apps-http-endpoint/manualtriggerurl.png
[2]: ./media/logic-apps-http-endpoint/manualtrigger.png
[3]: ./media/logic-apps-http-endpoint/response.png

