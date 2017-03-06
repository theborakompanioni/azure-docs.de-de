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
ms.date: 02/19/2017
ms.author: mahender
translationtype: Human Translation
ms.sourcegitcommit: facdd2abbb90a85535b8936ea655a824134c4c8b
ms.openlocfilehash: 3e8b8f9908cf24baf7a5d70521c79dbd470001f8
ms.lasthandoff: 02/22/2017


---
# <a name="working-with-azure-functions-proxies-preview"></a>Arbeiten mit Proxys in Azure Functions (Vorschau)

> [!Note] 
> Azure Functions-Proxys sind derzeit in der Vorschau verfügbar. Während der Vorschau sind sie kostenlos, aber für Proxyausführungen gilt die standardmäßige Functions-Abrechnung. Weitere Informationen hierzu finden Sie unter [Functions Preise](https://azure.microsoft.com/pricing/details/functions/).

In diesem Artikel werden das Konfigurieren von Azure Functions-Proxys und das Arbeiten mit ihnen erläutert. Dieses Feature ermöglicht Ihnen, in Ihrer Funktionen-App Endpunkte anzugeben, die von einer anderen Ressource implementiert werden. Sie können mit diesen Proxys eine große API in mehrere Funktionen-Apps aufteilen (wie in einer Microservicearchitektur), wobei Clients dennoch eine einzelne API-Oberfläche präsentiert wird.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]


## <a name="a-nameenableaenabling-azure-functions-proxies"></a><a name="enable"></a>Aktivieren von Azure Functions-Proxys

Proxys sind nicht standardmäßig aktiviert. Sie können Proxys erstellen, während das Feature deaktiviert ist, aber sie werden nicht ausgeführt. In den folgenden Schritten erfahren Sie, wie Sie Proxys aktivieren:

1. Öffnen Sie zunächst das [Azure-Portal] und navigieren Sie zu Ihrer Funktionen-App.
2. Wählen Sie **Funktionen-App-Einstellungen**.
3. Schalten Sie **Azure Functions-Proxys aktivieren (Vorschau)** auf „Ein“.

Sie können auch zu diesem Schritt zurückkehren, um die Proxylaufzeit zu aktualisieren, sobald neue Features verfügbar sind.


## <a name="creating-a-proxy"></a>Erstellen eines Proxys

In diesem Abschnitt erfahren Sie, wie Sie einen Proxy im Functions-Portal erstellen.

1. Öffnen Sie zunächst das [Azure-Portal] und navigieren Sie zu Ihrer Funktionen-App.
2. Wählen Sie im linken Navigationsbereich **Neuer Proxy**.
3. Geben Sie einen Namen für Ihren Proxy an.
4. Konfigurieren Sie den in dieser Funktionen-App verfügbar gemachten Endpunkt durch Angeben von **Routenvorlage** und **HTTP-Methoden**. Diese Parameter verhalten sich entsprechend den Regeln für [HTTP-Triggern].
5. Legen Sie für die **Back-End-URL** einen anderen Endpunkt fest. Dies könnte eine Funktion in einer anderen Funktionen-App oder eine beliebige andere API sein.
6. Klicken Sie auf Erstellen.

Ihr Proxy ist jetzt als neuer Endpunkt in Ihrer Funktionen-App vorhanden. Aus Sicht eines Clients entspricht dies einem HttpTrigger in Azure Functions. Sie können Ihren neuen Proxy ausprobieren, indem Sie die Proxy-URL kopieren und mit Ihrem bevorzugten HTTP-Client testen.


## <a name="a-namemodify-requestsamodifying-backend-requests"></a><a name="modify-requests"></a>Ändern von Back-End-Anforderungen

Der Back-End-URL-Parameter muss nicht statisch sein. Sie können ihn für die Eingabe aus der Anforderung oder aus Anwendungseinstellungen konditionieren.


### <a name="using-request-parameters"></a>Verwenden von Anforderungsparametern

In der Routenvorlage verwendete Parameter können als Eingaben für die Back-End-URL-Eigenschaft verwendet werden. Auf Werte wird mit dem in geschweifte Klammern „{}“ eingeschlossenen Namen verwiesen.

Wenn ein Proxy etwa eine Routenvorlage wie `/pets/{petId}` hat, kann die Back-End-URL den Wert von `{petId}` enthalten, wie z.B. in `https://<AnotherApp>.azurewebsites.net/api/pets/{petId}`. Wenn die Routenvorlage in einem Platzhalter wie z.B. `/api/{*restOfPath}` endet, ist der Wert `{restOfPath}` eine Zeichenfolgendarstellung der verbleibenden Pfadsegmente aus der eingehenden Anforderung.


### <a name="using-application-settings"></a>Verwenden von Anwendungseinstellungen

Sie können auch auf [Anwendungseinstellungen](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings#develop) verweisen, indem Sie den Namen der Einstellung mit Prozentzeichen '%' umgeben.

Bei der Back-End-URL `https://%ORDER_PROCESSING_HOST%/api/orders` wird z.B. „%ORDER_PROCESSING_HOST %“ durch den Wert der Einstellung ORDER_PROCESSING_HOST ersetzt.

> [!TIP] 
> Verwenden Sie Anwendungseinstellungen für Back-End-Hosts, wenn Sie mehrere Bereitstellungen oder Testumgebungen haben. Auf diese Weise können Sie sicherstellen, dass Sie immer mit dem richtigen Back-End für die jeweilige Umgebung kommunizieren.



## <a name="deployment-methods"></a>Bereitstellungsmethoden

Die Proxys, die Sie konfigurieren, werden in einer Datei „proxies.json“ gespeichert, die sich im Stamm eines Funktionen-App-Verzeichnisses befindet. Sie können diese Datei manuell bearbeiten und als Teil Ihrer App bereitstellen, wenn Sie eine der [Bereitstellungsmethoden](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) verwenden, die Functions unterstützt.

Das Feature muss aktiviert werden, damit die Datei verarbeitet wird. Hierzu können Sie die Anweisungen in [Aktivieren von Azure Functions-Proxys](#enable) befolgen.

„Proxies.json“ wird von einem Proxys-Objekt definiert, das aus benannten Proxys und ihren Definitionen besteht. Eine Beispieldatei könnte folgendermaßen aussehen:

```json
{
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
* **backendUri**: Die URL der Back-End-Ressource, zur der die Anforderung über einen Proxy gesendet werden soll. Dieser Wert kann auf Vorlagen basieren, wie in [Ändern von Back-End-Anforderungen](#modify-requests) beschrieben. Wenn diese Eigenschaft nicht enthalten ist, antwortet Azure Functions mit einem „HTTP 200 OK“.

> [!Note] 
> Die Azure Functions-Proxys berücksichtigen nicht die Eigenschaft routePrefix der Functions-Hostkonfiguration. Wenn Sie ein Präfix wie „/api“ einschließen möchten, muss es in der Eigenschaft „route“ enthalten sein.



[Azure-Portal]: https://portal.azure.com
[HTTP-Triggern]: https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#http-trigger
