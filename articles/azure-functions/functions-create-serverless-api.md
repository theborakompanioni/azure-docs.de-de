---
title: Erstellen einer serverlosen API mit Azure Functions | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie eine serverlose API mit Azure Functions erstellen
services: functions
author: mattchenderson
manager: cfowler
ms.service: functions
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: tutorial
ms.date: 05/04/2017
ms.author: mahender
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: e4fe86b80d8a786da15cdea37619e54e55102e3f
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---

# <a name="create-a-serverless-api-using-azure-functions"></a>Erstellen einer serverlosen API mit Azure Functions

In diesem Tutorial erfahren Sie, wie Sie mit Azure Functions höchst flexibel skalierbare APIs erstellen können. Zu Azure Functions gehört eine Sammlung von integrierten HTTP-Triggern und -Bindungen, die Ihnen das Erstellen eines Endpunkts in einer Vielzahl von Sprachen wie z.B. Node.js und C# erleichtern. In diesem Tutorial passen Sie einen HTTP-Trigger so an, dass er bestimmte Aktionen in Ihrem API-Entwurf verarbeitet. Sie bereiten sich auch auf eine Erweiterung Ihrer API vor, indem Sie sie in Azure Functions-Proxys integrieren und Modell-APIs einrichten. All dies erfolgt auf der Grundlage der serverlosen Compute-Umgebung für Functions. Daher müssen Sie sich keine Gedanken über die Skalierung der Ressourcen machen – Sie können sich ganz auf Ihre API-Logik konzentrieren.

## <a name="prerequisites"></a>Voraussetzungen 

[!INCLUDE [Previous quickstart note](../../includes/functions-quickstart-previous-topics.md)]

Die resultierende Funktion wird für den restlichen Teil dieses Tutorials verwendet.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Öffnen Sie das Azure-Portal. Melden Sie sich hierzu mit Ihrem Azure-Konto bei [https://portal.azure.com](https://portal.azure.com) an.

## <a name="customize-your-http-function"></a>Anpassen der HTTP-Funktion

Standardmäßig ist Ihre über HTTP ausgelöste Funktion so konfiguriert, dass sie alle HTTP-Methoden akzeptiert. Es gibt auch eine Standard-URL im Format `http://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>`. Wenn Sie nach der Schnellstartanleitung vorgegangen sind, sieht `<funcname>` wahrscheinlich in etwa wie „HttpTriggerJS1“ aus. In diesem Abschnitt ändern Sie die Funktion, sodass sie stattdessen nur auf GET-Anforderungen für die Route `/api/hello` antwortet. 

1. Navigieren Sie im Azure-Portal zu Ihrer Funktion. Wählen Sie im linken Navigationsbereich **Integrieren** aus.

    ![Anpassen einer HTTP-Funktion](./media/functions-create-serverless-api/customizing-http.png)

1. Verwenden Sie die HTTP-Triggereinstellungen, wie in der Tabelle angegeben.

    | Feld | Beispielwert | Beschreibung |
    |---|---|---|
    | Zulässige HTTP-Methoden | Ausgewählte Methoden | Bestimmt, welche HTTP-Methoden verwendet werden können, um diese Funktion aufzurufen |
    | Ausgewählte HTTP-Methoden | GET | Bestimmt, dass nur ausgewählte HTTP-Methoden zum Aufrufen dieser Funktion verwendet werden können |
    | Routenvorlage | /hello | Bestimmt, welche Route verwendet wird, um diese Funktion aufzurufen |
    | Autorisierungsstufe | Anonym | Optional: Ermöglicht den Zugriff auf Ihre Funktion ohne einen API-Schlüssel |

    > [!NOTE] 
    > Beachten Sie, dass Sie das Basispfadpräfix `/api` nicht in die Routenvorlage aufgenommen haben, da es durch eine globale Einstellung festgelegt wird.

1. Klicken Sie auf **Speichern**.

Unter [HTTP- und Webhookbindungen in Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook#customizing-the-http-endpoint) erfahren Sie mehr über das Anpassen von HTTP-Funktionen.

### <a name="test-your-api"></a>Testen der API

Als Nächstes testen Sie die Funktion, um zu sehen, wie sie mit der neuen API-Oberfläche funktioniert.
1. Navigieren Sie zurück zur Entwicklungsseite, indem Sie im linken Navigationsbereich auf den Funktionsnamen klicken.
1. Klicken Sie auf **Funktions-URL abrufen**, und kopieren Sie die URL. Sie sollten sehen, dass jetzt die Route `/api/hello` verwendet wird.
1. Kopieren Sie die URL in eine neue Browserregisterkarte oder in Ihren bevorzugten REST-Client. Browser verwenden standardmäßig GET.
1. Führen Sie die Funktion aus, und vergewissern Sie sich, dass sie funktioniert. Sie müssen möglicherweise den Parameter „name“ als Abfragezeichenfolge angeben, damit der Schnellstartcode funktioniert.
1. Sie können auch versuchen, den Endpunkt mit einer anderen HTTP-Methode aufzurufen, um sicherzustellen, dass die Funktion nicht ausgeführt wird. Hierzu müssen Sie einen REST-Client wie cURL, Postman oder Fiddler verwenden.

## <a name="proxies-overview"></a>Übersicht über Proxy

Im nächsten Abschnitt zeigen Sie Ihre API über einen Proxy an. Azure Functions-Proxys sind eine Vorschaufunktion, mit der Sie Anforderungen an andere Ressourcen weiterleiten können. Sie definieren einen HTTP-Endpunkt wie für HTTP-Trigger, statt aber Code zu schreiben, der ausgeführt wird, wenn dieser Endpunkt aufgerufen wird, geben Sie eine URL für eine Remoteimplementierung an. Dadurch können Sie mehrere API-Quellen in einer einzelnen API-Oberfläche zusammenstellen, die Clients einfach nutzen können. Dies ist besonders nützlich, wenn Sie Ihre API als Microservices erstellen möchten.

Ein Proxy kann auf HTTP-Ressourcen wie die folgenden verweisen:
- Azure-Funktionen 
- API-Apps in [Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-web-overview)
- Docker-Container in [App Service unter Linux](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro)
- Andere gehostete APIs

Weitere Informationen zu Proxys finden Sie unter [Arbeiten mit Proxys in Azure Functions (Vorschau)].

## <a name="create-your-first-proxy"></a>Erstellen Ihres ersten Proxys

In diesem Abschnitt erstellen Sie einen neuen Proxy, der als Front-End für Ihre übergeordnete API dient. 

### <a name="setting-up-the-frontend-environment"></a>Einrichten der Front-End-Umgebung

Wiederholen Sie die Schritte zum [Erstellen einer Funktionen-App](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function#create-a-function-app), um eine neue Funktionen-App zu erstellen, in der Sie den Proxy erstellen. Die URL dieser neuen App dient als Front-End für unsere API, und die Funktions-App, die Sie zuvor bearbeitet haben, dient als Back-End.

1. Navigieren Sie zu Ihrer neuen Front-End-Funktionen-App im Portal.
1. Wählen Sie **Settings**aus. Schalten Sie dann **Azure Functions-Proxys aktivieren (Vorschau)** auf „Ein“.
1. Wählen Sie **Plattformeinstellungen** und dann **Anwendungseinstellungen** aus.
1. Scrollen Sie nach unten bis zu **App-Einstellungen**, und erstellen Sie eine neue Einstellung mit dem Schlüssel „HELLO_HOST“. Legen Sie deren Wert auf den Host Ihrer Back-End-Funktionen-App fest, z.B. `<YourBackendApp>.azurewebsites.net`. Dies ist Teil der URL, die Sie zuvor kopiert haben, als Sie die HTTP-Funktion getestet haben. Sie geben diese Einstellung später in der Konfiguration an.

    > [!NOTE] 
    > App-Einstellungen werden für die Hostkonfiguration empfohlen, um eine hartcodierte Umgebungsabhängigkeit für den Proxy zu verhindern. Die Verwendung dieser App-Einstellungen bedeutet, dass Sie die Proxykonfiguration zwischen Umgebungen verschieben können, und die umgebungsspezifischen App-Einstellungen werden angewendet.

1. Klicken Sie auf **Speichern**.

### <a name="creating-a-proxy-on-the-frontend"></a>Erstellen eines Proxys auf dem Front-End

1. Navigieren Sie zurück zu Ihrer Front-End-Funktionen-App im Portal.
1. Klicken Sie im linken Navigationsbereich auf das Pluszeichen „+“ neben „Proxys (Vorschau)“.
    ![Erstellen eines Proxys](./media/functions-create-serverless-api/creating-proxy.png)
1. Verwenden Sie die Proxyeinstellungen, wie in der Tabelle angegeben. 

    | Feld | Beispielwert | Beschreibung |
    |---|---|---|
    | Name | HelloProxy | Ein Anzeigename für die Verwaltung |
    | Routenvorlage | /api/hello | Bestimmt, welche Route verwendet wird, um diesen Proxy aufzurufen |
    | Back-End-URL | https://%HELLO_HOST%/api/hello | Gibt den Endpunkt an, der für die Anforderung als Proxy dienen soll |
    
1. Beachten Sie, dass Proxys das Basispfadpräfix `/api` nicht bereitstellen. Es muss in der Routenvorlage enthalten sein.
1. Die Syntax `%HELLO_HOST%` verweist auf die App-Einstellung, die Sie zuvor erstellt haben. Die aufgelöste URL verweist auf die ursprüngliche Funktion.
1. Klicken Sie auf **Erstellen**.
1. Sie können Ihren neuen Proxy ausprobieren, indem Sie die Proxy-URL kopieren und im Browser oder mit Ihrem bevorzugten HTTP-Client testen.
    1. Für eine anonyme Funktion verwenden Sie:
        1. `https://YOURPROXYAPP.azurewebsites.net/api/hello?name="Proxies"`
    1. Für eine Funktion mit Autorisierung verwenden Sie:
        1. `https://YOURPROXYAPP.azurewebsites.net/api/hello?code=YOURCODE&name="Proxies"`

## <a name="create-a-mock-api"></a>Erstellen einer Modell-API

Als Nächstes verwenden Sie einen Proxy, um eine Modell-API für die Lösung zu erstellen. Dadurch kann die Cliententwicklung fortgesetzt werden, ohne den Back-End vollständig zu implementieren. Sie können später bei der Entwicklung eine neue Funktionen-App erstellen, die diese Logik unterstützt, und den Proxy entsprechend umleiten.

Um diese Modell-API zu erstellen, erstellen wir einen neuen Proxy. Dieses Mal verwenden wir den [App Service-Editor](https://github.com/projectkudu/kudu/wiki/App-Service-Editor). Navigieren Sie zunächst zu Ihrer Funktionen-App im Portal. Wählen Sie **Plattformfeatures** aus, und suchen Sie nach **App Service-Editor**. Durch Klicken wird der App Service-Editor auf einer neuen Registerkarte geöffnet.

Wählen Sie im linken Navigationsbereich `proxies.json` aus. In dieser Datei wird die Konfiguration für alle Ihre Proxys gespeichert. Wenn Sie eine der [Functions-Bereitstellungsmethoden](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment) verwenden, ist dies die Datei, in der Sie die Quellcodeverwaltung verwalten. Weitere Informationen zu dieser Datei finden Sie unter [Erweiterte Konfiguration für Proxys](https://docs.microsoft.com/azure/azure-functions/functions-proxies#advanced-configuration).

Wenn Sie den bisherigen Schritten gefolgt sind, sollte Ihre „proxies.json“ wie folgt aussehen:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        }
    }
}
```

Als Nächstes fügen Sie Ihre Modell-API hinzu. Ersetzen Sie die Datei „proxies.json“ durch Folgendes:

```json
{
    "$schema": "http://json.schemastore.org/proxies",
    "proxies": {
        "HelloProxy": {
            "matchCondition": {
                "route": "/api/hello"
            },
            "backendUri": "https://%HELLO_HOST%/api/hello"
        },
        "GetUserByName" : {
            "matchCondition": {
                "methods": [ "GET" ],
                "route": "/api/users/{username}"
            },
            "responseOverrides": {
                "response.statusCode": "200",
                "response.headers.Content-Type" : "application/json",
                "response.body": {
                    "name": "{username}",
                    "description": "Awesome developer and master of serverless APIs",
                    "skills": [
                        "Serverless",
                        "APIs",
                        "Azure",
                        "Cloud"
                    ]
                }
            }
        }
    }
}
```

Damit wird ein neuer Proxy „GetUserByName“ ohne die Eigenschaft „backendUri“ hinzugefügt. Statt eine andere Ressource aufzurufen, wird die Standardantwort von Proxys geändert, indem die Antwort überschrieben wird. Das Überschreiben von Anforderungen und Antworten kann auch in Verbindung mit einer Back-End-URL genutzt werden. Dies ist besonders nützlich, wenn Proxyfunktionen in einem Legacysystem verwendet werden, wo Sie möglicherweise Header, Abfrageparameter usw. ändern müssen. Weitere Informationen zum Überschreiben von Anforderungen und Antworten finden Sie unter [Ändern von Anforderungen und Antworten in Proxys](https://docs.microsoft.com/azure/azure-functions/functions-proxies#a-namemodify-requests-responsesamodifying-requests-and-responses).

Testen Sie Ihre Modell-API durch Aufrufen des Endpunkts `/api/users/{username}` mit einem Browser oder Ihrem bevorzugten REST-Client. Sie müssen _{username}_ mit einem Zeichenfolgenwert eines Benutzernamens ersetzen.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie eine API für Azure Functions erstellt und angepasst wird. Außerdem haben Sie gelernt, wie Sie mehrere APIs (darunter Modelle) zu einer einheitlichen API-Oberfläche verknüpften können. Sie können diese Techniken verwenden, um APIs von beliebiger Komplexität zu erstellen, während Sie das von Azure Functions bereitgestellte serverlose Computemodell nutzen.

Die folgenden Referenzen können bei der weiteren Entwicklung Ihrer API hilfreich sein:

- [HTTP- und Webhookbindungen in Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-bindings-http-webhook)
- [Arbeiten mit Proxys in Azure Functions (Vorschau)]
- [Dokumentieren einer Azure Functions-API (Vorschau)](https://docs.microsoft.com/azure/azure-functions/functions-api-definition-getting-started)


[Create your first function]: https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function
[Arbeiten mit Proxys in Azure Functions (Vorschau)]: https://docs.microsoft.com/azure/azure-functions/functions-proxies

