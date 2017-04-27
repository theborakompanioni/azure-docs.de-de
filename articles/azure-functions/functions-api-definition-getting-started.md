---
title: Erste Schritte mit OpenAPI-Metadaten in Azure Functions | Microsoft-Dokumentation
description: "Erste Schritte mit OpenAPI-Unterstützung in Azure Functions"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 03/23/2017
ms.author: alkarche
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: ad82c1a552d9d77259c44b938975eebc44933b86
ms.lasthandoff: 04/26/2017


---
# <a name="creating-openapi-20-swagger-metadata-for-a-function-app-preview"></a>Erstellen von OpenAPI 2.0-Metadaten (Swagger-Metadaten) für eine Funktionen-App (Vorschau)

In diesem Dokument sind die Schritte aufgeführt, mit denen Sie eine OpenAPI-Definition für eine einfache API erstellen, die von Azure Functions gehostet wird.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-swagger"></a>Was ist OpenAPI (Swagger)?
[Swagger-Metadaten](http://swagger.io/) sind in einer Datei enthalten und definieren die Funktionalität und Betriebsmodi Ihrer API und ermöglichen es, dass eine Funktion, die eine REST-API hostet, von vielen anderen Softwareprodukten verwendet werden kann. Microsoft-Angebote wie PowerApps und [API-Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier) sowie von anderen Herstellern angebotene Entwicklertools wie [Postman](https://www.getpostman.com/docs/importing_swagger) und [viele weitere Pakete](http://swagger.io/tools/) ermöglichen, dass Ihre API mit einer Swagger-Definition verwendet werden kann.

## <a name="prepare-function"></a>Erstellen einer Funktion mit einer einfachen API
  Um eine OpenAPI-Definition zu erstellen, müssen Sie zunächst eine Funktion mit einer einfachen API erstellen. Wenn Sie bereits eine API haben, die in einer Funktionen-App gehostet wird, können Sie direkt zum nächsten Abschnitt wechseln.
1. Erstellen Sie eine neue Funktionen-App.
  1. [Azure-Portal](https://portal.azure.com)  >  `+ New` > nach „Funktionen-App“ suchen
1. Erstellen Sie eine neue HTTP-Triggerfunktion in Ihrer neuen Funktionen-App.
  1. Ihre Funktion enthält bereits Code, der eine sehr einfache REST-API definiert.
  1. Jede Zeichenfolge, die als Abfrageparameter oder im Körper an die Funktion übergeben wird, wird als „Hello {Eingabe}“ zurückgegeben.
1. Wechseln Sie zur Registerkarte `Integrate` Ihrer neuen HTTP-Triggerfunktion.
  1. Schalten Sie `Allowed HTTP methods` in `Selected methods` um.
  1. Deaktivieren Sie in `Selected HTTP methods` alle Verben außer POST.
    ![Ausgewählte HTTP-Methoden](./media/functions-api-definition-getting-started/selectedHTTPmethods.png)
  1. Dieser Schritt vereinfacht nachher Ihre API-Definition.

## <a name="enable"></a>Aktivieren der Unterstützung für API-Definition
1. Navigieren Sie zu `your function name` > `API Definition (preview)`.
1. Legen Sie `API Definition Source` auf `Function` fest.
  1. In diesem Schritt wird eine Reihe von OpenAPI-Optionen für Ihre Funktionen-App aktiviert. Dazu gehören ein Endpunkt, um eine OpenAPI-Datei aus der Domäne Ihrer Funktionen-App zu hosten, eine Inlinekopie des [OpenAPI-Editors](http://editor.swagger.io) und ein Schnellstart-Definitionsgenerator.
![Aktivierte Definition](./media/functions-api-definition-getting-started/enabledefinition.png)

## <a name="create-definition"></a>Erstellen Ihrer API-Definition aus einer Vorlage
1. Klicken Sie auf `Generate API Definition template`.
  1. In diesem Schritt wird in Ihrer Funktionen-App nach HTTP-Triggerfunktionen gesucht und werden die Informationen in „functions.json“ verwendet, um ein OpenAPI-Dokument zu generieren.
2. Fügen Sie ein Vorgangsobjekt (Operation Object) zu `paths: /api/yourfunctionroute post:` hinzu.
  1. Das OpenAPI-Schnellstartdokument ist eine Kurzfassung eines vollständigen OpenAPI-Dokuments. Für eine vollständige OpenAPI-Definition sind weitere Metadaten erforderlich, z.B. Vorgangsobjekte und Antwortvorlagen (response-Vorlagen).
  1. Das folgende Beispielvorgangsobjekt hat einen ausgefüllten Abschnitt für „produces/consumes“ sowie ein parameter- und ein response-Objekt.
  
  ```yaml
      post:
        operationId: /api/yourfunctionroute/post
        consumes: [application/json]
        produces: [application/json]
        parameters:
          - name: name
            in: body
            description: Your name
            x-ms-summary: Your name
            required: true
            schema:
              type: object
              properties:
                name:
                  type: string
        description: >-
          Replace with Operation Object
          #http://swagger.io/specification/#operationObject
        responses:
          '200':
            description: A Greeting
            x-ms-summary: Your name
            schema:
              type: string
        security:
          - apikeyQuery: []
  ```

> [!NOTE]
>  „x-ms-summary“ stellt einen Anzeigenamen in Logic Apps, Flow und PowerApps bereit.
>
> Weitere Informationen finden Sie unter [Customize your Swagger definition for PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).

3. Klicken Sie auf `save`, um die Änderungen zu speichern ![Vorlagendefinition hinzufügen](./media/functions-api-definition-getting-started/addingtemplate.png)

## <a name="use-definition"></a>Verwenden Ihrer API-Definition
1. Kopieren Sie Ihre `API definition URL`, und fügen Sie sie in einer neuen Browserregisterkarte ein, um das unformatierte OpenAPI Dokument anzuzeigen.
1. Sie können Ihr OpenAPI-Dokument über diese URL in beliebig viele Tools zum Testen und Einbinden importieren.
  1. Viele Azure-Ressourcen können Ihr OpenAPI-Dokument automatisch über die API-Definitions-URL importieren, die in den Einstellungen Ihrer Funktionen-App gespeichert ist. Als Teil der `Function`-API-Definitionsquelle aktualisieren wir diese URL für Sie.


## <a name="test-definition"></a>Verwenden der Swagger-Benutzeroberfläche zum Testen Ihrer API-Definition
Es gibt Testtools, die in die Benutzeroberflächenansicht des eingebetteten Editors für API-Definitionen integriert sind. Fügen Sie Ihren API-Schlüssel hinzu, und verwenden Sie dann die Schaltfläche `Try this operation` unter jeder Methode. Das Tool verwendet die API-Definition, um die Anforderungen zu formatieren, und erfolgreiche Antworten kennzeichnen, dass Ihre Definition richtig ist.

### <a name="steps"></a>Schritte:

1. Kopieren Sie den API-Schlüssel der Funktion.
  1. Den API-Schlüssel finden Sie in Ihrer HTTP-Triggerfunktion unter `function name`  >  `Keys`  >  `Function Keys` 
   ![Schlüssel für Funktion](./media/functions-api-definition-getting-started/functionkey.png)
1. Navigieren Sie zur Seite `API Definition (preview)`.
  1. Klicken Sie auf `Authenticate`, und fügen Sie den API-Schlüssel Ihrer Funktion dem oben befindlichen Sicherheitsobjekt hinzu.
  ![OpenAPI-Schlüssel](./media/functions-api-definition-getting-started/definitionTest auth.png)
1. Wählen Sie `/api/yourfunctionroute` > `POST` aus.
1. Klicken Sie auf `Try it out`, und geben Sie einen Namen ein, um zu testen.
1. Sie sollten das Ergebnis SUCCESS unter `Pretty` sehen.
 ![Test der API-Definition](./media/functions-api-definition-getting-started/definitionTest.png)

## <a name="next-steps"></a>Nächste Schritte
* [OpenAPI-Definition in Azure Functions](functions-api-definition.md)
  * Lesen Sie die vollständige Dokumentation, um weitere Informationen zur Unterstützung von OpenAPI zu erhalten.
* [Entwicklerreferenz zu Azure Functions](functions-reference.md)  
  * Referenz zum Programmieren von Funktionen sowie zum Festlegen von Triggern und Bindungen.
* [Azure Functions-GitHub-Repository](https://github.com/Azure/Azure-Functions/)
  * Testen Sie Functions-Github (Azure Functions), und geben uns Feedback zur Vorschau der Unterstützung von API-Definitionen! Erstellen Sie ein Github-Problem für jeden Punkt, für den Sie eine Aktualisierung wünschen.

