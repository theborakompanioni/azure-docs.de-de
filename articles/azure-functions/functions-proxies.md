---
title: Arbeiten mit Proxys in Azure Functions | Microsoft-Dokumentation
description: "Übersicht zum Verwenden von Azure Functions-Proxys"
services: functions
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/11/2017
ms.author: mahender
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 6a0a81a011d9a1cc1a8a81ba8ef92d90308c534d
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="working-with-azure-functions-proxies-preview"></a>Arbeiten mit Proxys in Azure Functions (Vorschau)

> [!Note] 
> Azure Functions-Proxys sind derzeit in der Vorschau verfügbar. Während der Vorschau sind sie kostenlos, aber für Proxyausführungen gilt die standardmäßige Functions-Abrechnung. Weitere Informationen hierzu finden Sie unter [Functions Preise](https://azure.microsoft.com/pricing/details/functions/).

In diesem Artikel werden das Konfigurieren von Azure Functions-Proxys und das Arbeiten mit ihnen erläutert. Dieses Feature ermöglicht Ihnen, in Ihrer Funktionen-App Endpunkte anzugeben, die von einer anderen Ressource implementiert werden. Sie können mit diesen Proxys eine große API in mehrere Funktionen-Apps aufteilen (wie in einer Microservicearchitektur), wobei Clients dennoch eine einzelne API-Oberfläche präsentiert wird.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <a name="enable"></a>Aktivieren von Azure Functions-Proxys

Proxys sind nicht standardmäßig aktiviert. Sie können Proxys erstellen, während das Feature deaktiviert ist, aber sie werden nicht ausgeführt. In den folgenden Schritten erfahren Sie, wie Sie Proxys aktivieren:

1. Öffnen Sie zunächst das [Azure-Portal] und navigieren Sie zu Ihrer Funktionen-App.
2. Wählen Sie **Funktionen-App-Einstellungen**.
3. Schalten Sie **Azure Functions-Proxys aktivieren (Vorschau)** auf „Ein“.

Sie können auch zu diesem Schritt zurückkehren, um die Proxylaufzeit zu aktualisieren, sobald neue Features verfügbar sind.


## <a name="create"></a>Erstellen eines Proxys

In diesem Abschnitt erfahren Sie, wie Sie einen Proxy im Functions-Portal erstellen.

1. Öffnen Sie zunächst das [Azure-Portal] und navigieren Sie zu Ihrer Funktionen-App.
2. Wählen Sie im linken Navigationsbereich **Neuer Proxy**.
3. Geben Sie einen Namen für Ihren Proxy an.
4. Konfigurieren Sie den in dieser Funktionen-App verfügbar gemachten Endpunkt durch Angeben von **Routenvorlage** und **HTTP-Methoden**. Diese Parameter verhalten sich entsprechend den Regeln für [HTTP-Triggern].
5. Legen Sie für die **Back-End-URL** einen anderen Endpunkt fest. Dies könnte eine Funktion in einer anderen Funktionen-App oder eine beliebige andere API sein. Der Wert muss nicht statisch sein und kann auf [Anwendungseinstellungen] und [Parameter aus der ursprünglichen Clientanforderung] verweisen.
6. Klicken Sie auf „Erstellen“.

Ihr Proxy ist jetzt als neuer Endpunkt in Ihrer Funktionen-App vorhanden. Aus Sicht eines Clients entspricht dies einem HttpTrigger in Azure Functions. Sie können Ihren neuen Proxy ausprobieren, indem Sie die Proxy-URL kopieren und mit Ihrem bevorzugten HTTP-Client testen.








## <a name="modify-requests-responses"></a>Ändern von Anforderungen und Antworten

Mit Azure Functions-Proxys können Sie Anforderungen und Antworten aus dem Back-End ändern. Diese Transformationen können Variablen gemäß Definition in [Verwenden von Variablen] verwenden.

### <a name="modify-backend-request"></a>Ändern der Back-End-Anforderung

Standardmäßig wird die Back-End-Anforderung als Kopie der ursprünglichen Anforderung initialisiert. Zusätzlich zum Festlegen der Back-End-URL können Sie auch HTTP-Methode, Header und Abfragezeichenfolgen-Parameter ändern. Die geänderten Werte können auf [Anwendungseinstellungen] und [Parameter aus der ursprünglichen Clientanforderung] verweisen.

Das Ändern von Back-End-Anforderungen ist gegenwärtig im Portal nicht möglich. Unter [Definieren eines requestOverrides-Objekts] erfahren Sie, wie Sie diese Funktion von proxies.json nutzen.

### <a name="modify-response"></a>Ändern der Antwort

Standardmäßig wird die Clientantwort als Kopie der Back-End-Antwort initialisiert. Sie können Änderungen an Statuscode, Ursachentext, Headern und Text der Antwort vornehmen. Die geänderten Werte können auf [Anwendungseinstellungen], [Parameter aus der ursprünglichen Clientanforderung] und [Parameter aus der Back-End-Antwort] verweisen.

Das Ändern von Antworten ist gegenwärtig im Portal nicht möglich. Unter [Definieren eines responseOverrides-Objekts] erfahren Sie, wie Sie diese Funktion von proxies.json nutzen.






## <a name="using-variables"></a>Verwenden von Variablen

Die Konfiguration für einen Proxy muss nicht statisch sein. Sie können ihn zur Verwendung von Variablen aus der ursprünglichen Anforderung, der Back-End-Antwort oder Anwendungseinstellungen konditionieren.

### <a name="request-parameters"></a>Verweisen auf Anforderungsparameter

Anforderungsparameter können als Eingaben für die Back-End-URL-Eigenschaft oder im Rahmen der Änderung von Anforderungen und Antworten verwendet werden. Einige Parameter können der Routenvorlage entnommen werden, die in der Basisproxykonfiguration angegeben wird, während andere von Eigenschaften der eingehenden Anforderung stammen.

#### <a name="route-template-parameters"></a>Routenvorlagenparameter
Auf Parameter, die in der Routenvorlage verwendet werden, kann mit dem in geschweiften Klammern „{}“ eingeschlossenen Namen verwiesen werden.

Wenn ein Proxy etwa eine Routenvorlage wie `/pets/{petId}` hat, kann die Back-End-URL den Wert von `{petId}` enthalten, wie z.B. in `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Wenn die Routenvorlage in einem Platzhalter wie z.B. `/api/{*restOfPath}` endet, ist der Wert `{restOfPath}` eine Zeichenfolgendarstellung der verbleibenden Pfadsegmente aus der eingehenden Anforderung.

#### <a name="additonal-request-parameters"></a>Zusätzliche Anforderungsparameter
Zusätzlich zu den Routenvorlagenparametern können die folgenden Werte in Konfigurationswerten verwendet werden:

* **{request.method}**: Die HTTP-Methode, die in der ursprünglichen Anforderung verwendet wird.
* **{request.headers.&lt;HeaderName&gt;}**: Ein Header, der aus der ursprünglichen Anforderung gelesen werden kann. Ersetzen Sie &lt;HeaderName&gt; durch den Namen des Headers, den Sie lesen möchten. Wenn der Header nicht in der Anforderung enthalten ist, wird der Wert eine leere Zeichenfolge sein.
* **{request.querystring.&lt;ParameterName&gt;}**: Ein Abfragezeichenfolgen-Parameter, der aus der ursprünglichen Anforderung gelesen werden kann. Ersetzen Sie &lt;ParameterName&gt; durch den Namen des Parameters, den Sie lesen möchten. Wenn der Parameter nicht in der Anforderung enthalten ist, wird der Wert eine leere Zeichenfolge sein.

### <a name="response-parameters"></a>Verweisen auf Back-End-Antwort-Parameter

Antwortparameter können als Teil der Änderung der Antwort an den Client verwendet werden. Die folgenden Werte können in Konfigurationswerten verwendet werden:

* **{backend.response.statusCode}**: Der HTTP-Statuscode, der in der Back-End-Antwort zurückgegeben wird.
* **{backend.response.statusReason}**: Der HTTP-Ursachentext, der in der Back-End-Antwort zurückgegeben wird.
* **{backend.response.headers.&lt;HeaderName&gt;}**: Ein Header, der aus der Back-End-Antwort gelesen werden kann. Ersetzen Sie &lt;HeaderName&gt; durch den Namen des Headers, den Sie lesen möchten. Wenn der Header nicht in der Anforderung enthalten ist, wird der Wert eine leere Zeichenfolge sein.

### <a name="use-appsettings"></a>Verweisen auf Anwendungseinstellungen

Sie können auch verweisen auf [Anwendungseinstellungen, die für die Funktionen-App definiert sind](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop), indem Sie den Namen der Einstellung in Prozentzeichen '%' einschließen.

Bei der Back-End-URL `https://%ORDER_PROCESSING_HOST%/api/orders` wird z.B. „%ORDER_PROCESSING_HOST %“ durch den Wert der Einstellung ORDER_PROCESSING_HOST ersetzt.

> [!TIP] 
> Verwenden Sie Anwendungseinstellungen für Back-End-Hosts, wenn Sie mehrere Bereitstellungen oder Testumgebungen haben. Auf diese Weise können Sie sicherstellen, dass Sie immer mit dem richtigen Back-End für die jeweilige Umgebung kommunizieren.





## <a name="advanced-configuration"></a>Erweiterte Konfiguration

Die Proxys, die Sie konfigurieren, werden in einer Datei „proxies.json“ gespeichert, die sich im Stamm eines Funktionen-App-Verzeichnisses befindet. Sie können diese Datei manuell bearbeiten und als Teil Ihrer App bereitstellen, wenn Sie eine der [Bereitstellungsmethoden](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) verwenden, die Functions unterstützt. Das Feature muss [aktiviert](#enable) werden, damit die Datei verarbeitet wird. 

> [!TIP] 
> Wenn Sie keine der Bereitstellungsmethoden eingerichtet haben, können Sie auch mit der Datei „proxies.json“ im Portal arbeiten. Navigieren Sie zu Ihrer Funktionen-App, wählen Sie „Plattformfeatures“ und dann „App Service-Editor“. So können Sie die gesamte Dateistruktur Ihrer Funktionen-App sehen und Änderungen vornehmen.

„Proxies.json“ wird von einem Proxys-Objekt definiert, das aus benannten Proxys und ihren Definitionen besteht. Sie können zur Codevervollständigung optional auf ein [JSON-Schema](http://json.schemastore.org/proxies) verweisen, wenn Ihr Editor dies unterstützt. Eine Beispieldatei könnte folgendermaßen aussehen:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>"
        }
    }
}
```

Jeder Proxy hat einen Anzeigenamen, wie z.B. „proxy1“ im obigen Beispiel. Das entsprechende Proxydefinitionsobjekt wird durch die folgenden Eigenschaften definiert:

* **matchCondition**: Erforderlich – ein Objekt, das die Anforderungen definiert, die die Ausführung dieses Proxys auslösen. Es enthält zwei Eigenschaften, die es mit [HTTP-Triggern] gemeinsam hat:
    * _methods_: Ein Array der HTTP-Methoden, denen der Proxy antwortet. Wenn es nicht angegeben wird, antwortet der Proxy auf alle HTTP-Methoden in der Route.
    * _route_: Erforderlich – definiert die Routenvorlage, mit der gesteuert wird, auf welche Anforderungs-URLs Ihr Proxy antwortet. Im Gegensatz zu HTTP-Triggern ist dies kein Standardwert.
* **backendUri**: Die URL der Back-End-Ressource, zur der die Anforderung über einen Proxy gesendet werden soll. Dieser Wert kann auf Anwendungseinstellungen und Parameter aus der ursprünglichen Clientanforderung verweisen. Wenn diese Eigenschaft nicht enthalten ist, antwortet Azure Functions mit einem „HTTP 200 OK“.
* **requestOverrides**: Ein Objekt, das Transformationen der Back-End-Anforderung definiert. Siehe [Definieren eines requestOverrides-Objekts].
* **responseOverrides**: ein Objekt, das Transformationen der Clientantwort definiert. Siehe [Definieren eines responseOverrides-Objekts].

> [!Note] 
> Die Azure Functions-Proxys berücksichtigen nicht die Eigenschaft routePrefix der Functions-Hostkonfiguration. Wenn Sie ein Präfix wie „/api“ einschließen möchten, muss es in der Eigenschaft „route“ enthalten sein.

### <a name="requestOverrides"></a>Definieren eines requestOverrides-Objekts

Das requestOverrides-Objekt definiert Änderungen an der Anforderung, wenn die Back-End-Ressource aufgerufen wird. Das Objekt wird durch folgende Eigenschaften definiert:

* **backend.request.method**: Dies ist die HTTP-Methode, mit der das Back-End aufgerufen wird.
* **backend.request.querystring.&lt;ParameterName&gt;**: Ein Abfragezeichenfolgen-Parameter, der für den Aufruf des Back-Ends festgelegt werden kann. Ersetzen Sie &lt;ParameterName&gt; durch den Namen des Parameters, den Sie festlegen möchten. Wenn die leere Zeichenfolge angegeben wird, wird der Parameter nicht in die Back-End-Anforderung einbezogen.
* **backend.request.headers.&lt;HeaderName&gt;**: Ein Header, der für den Aufruf des Back-Ends festgelegt werden kann. Ersetzen Sie &lt;HeaderName&gt; durch den Namen des Headers, den Sie festlegen möchten. Wenn die leere Zeichenfolge angegeben wird, wird der Header nicht in die Back-End-Anforderung einbezogen.

Die Werte können auf Anwendungseinstellungen und Parameter aus der ursprünglichen Clientanforderung verweisen.

Eine Beispielkonfiguration könnte folgendermaßen aussehen:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "backendUri": "https://<AnotherApp>.azurewebsites.net/api/<FunctionName>",
            "requestOverrides": {
                "backend.request.headers.Accept" : "application/xml",
                "backend.request.headers.x-functions-key" : "%ANOTHERAPP_API_KEY%"
            }
        }
    }
}
```

### <a name="responseOverrides"></a>Definieren eines responseOverrides-Objekts

Das responseOverrides-Objekt definiert Änderungen an der Antwort, die an den Client zurückgegeben wird. Das Objekt wird durch folgende Eigenschaften definiert:

* **response.statusCode**: Der HTTP-Statuscode, der an den Client zurückgegeben werden soll.
* **response.statusReason**: Der HTTP-Ursachentext, der an den Client zurückgegeben werden soll.
* **response.body**: Die Zeichenfolgendarstellung des Texts, der an den Client zurückgegeben werden soll.
* **response.headers.&lt;HeaderName&gt;**: Ein Header, der für die Antwort an den Client festgelegt werden kann. Ersetzen Sie &lt;HeaderName&gt; durch den Namen des Headers, den Sie festlegen möchten. Wenn die leere Zeichenfolge angegeben wird, wird der Header nicht in die Antwort einbezogen.

Die Werte können auf Anwendungseinstellungen, Parameter aus der ursprünglichen Clientanforderung und Parameter aus der Back-End-Antwort verweisen.

Eine Beispielkonfiguration könnte folgendermaßen aussehen:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "proxy1": {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/{test}"
            },
            "responseOverrides": {
                "response.body": "Hello, {test}",
                "response.headers.Content-Type" : "text/plain"
            }
        }
    }
}
```
> [!Note] 
> In diesem Beispiel wird der Text direkt festgelegt, sodass keine `backendUri`-Eigenschaft erforderlich ist. Dies ist ein Beispiel der Nutzung eines Azure Functions-Proxys zum Imitieren von APIs.

[Azure-Portal]: https://portal.azure.com
[HTTP-Triggern]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
[Modifying the backend request]: #modify-backend-request
[Modifying the response]: #modify-response
[Definieren eines requestOverrides-Objekts]: #requestOverrides
[Definieren eines responseOverrides-Objekts]: #responseOverrides
[Anwendungseinstellungen]: #use-appsettings
[Verwenden von Variablen]: #using-variables
[Parameter aus der ursprünglichen Clientanforderung]: #request-parameters
[Parameter aus der Back-End-Antwort]: #response-parameters
