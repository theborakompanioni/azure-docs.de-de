---
title: OpenAPI-Metadaten in Azure Functions | Microsoft-Dokumentation
description: "Übersicht über OpenAPI-Unterstützung in Azure Functions"
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
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: aac362f5123038cc39e0d2b32c10f7747a702cfe
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="openapi-20-metadata-support-in-azure-functions-preview"></a>OpenAPI 2.0-Metadatenunterstützung in Azure Functions (Vorschau)
Dieses Vorschaufeature ermöglicht es Ihnen, eine OpenAPI 2.0-Definition (früher Swagger) in einer Funktionen-App zu schreiben und diese Datei über die Funktionen-App zu hosten.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

### <a name="what-is-openapi-metadata"></a>Was sind OpenAPI-Metadaten?
[OpenAPI-Metadaten](http://swagger.io/) ermöglichen, dass eine Funktion, die eine REST-API hostet, von vielen anderen Softwareprodukten verwendet werden kann. Dies reicht von Microsoft-Angeboten wie PowerApps und [API-Apps](https://docs.microsoft.com/azure/app-service-api/app-service-api-dotnet-get-started#a-idcodegena-generate-client-code-for-the-data-tier) bis zu Entwicklertools wie [Postman](https://www.getpostman.com/docs/importing_swagger) eines anderen Anbieters und [vielen weiteren Paketen](http://swagger.io/tools/).

>[!TIP]
>Es empfiehlt sich, mit dem [Erste-Schritte-Tutorial](./functions-api-definition-getting-started.md) zu beginnen und dann zu diesem Dokument zurückzukehren, um das Wissen über bestimmte Features zu vertiefen.

## <a name="enable"></a>Aktivieren der Unterstützung für eine OpenAPI-Definition
* Sämtliche OpenAPI-Einstellungen können auf der Seite `API Definition (preview)` in Ihren Funktionen-App-Einstellungen konfiguriert werden.
* Legen Sie `API definition source` auf `Function` fest, um eine gehostete OpenAPI-Definition und eine Schnellstart-Definitionsgenerierung zu aktivieren.
  * `External URL` ermöglicht, dass Sie in Ihrer Funktion eine OpenAPI-Definition verwenden, die andernorts gehostet wird.

## <a name="generate-definition"></a>Generieren eines Swagger-Gerüsts aus Ihren Funktionsmetadaten
Eine Vorlage ist ein optimaler Einstiegspunkt für das Schreiben Ihrer ersten OpenAPI-Definition. Das Feature für Definitionsvorlagen erstellt eine spärlich konfigurierte OpenAPI-Definition anhand aller Metadaten aus „function.json“ für jede Ihrer HTTP-Triggerfunktionen. **Sie müssen weitere Informationen zu Ihrer API aus der [OpenAPI-Spezifikation](http://swagger.io/specification/) eintragen, z.B. Vorlagen für Anforderungen (request) und Antworten (response).**

[In diesem Erste-Schritte-Tutorial finden Sie schrittweise Anweisungen.](./functions-api-definition-getting-started.md)

### <a name="templates"></a>Verfügbare Vorlagen

|Name| Beschreibung |
|:-----|:-----|
|Generated Definition|Eine OpenAPI-Definition mit sämtlichen Informationen, die aus den vorhandenen Metadaten den Funktion abgeleitet werden können|

### <a name="quickstart-details"></a>Enthaltene Metadaten in „Generated Definition“

In der folgenden Tabelle sind die Portaleinstellungen und die entsprechenden Daten aus „function.json“ aufgeführt, wie sie dem generierte Swagger-Gerüst zugeordnet sind.

|Swagger.json|Portal-Benutzeroberfläche|Function.json|
|:----|:-----|:-----|
|[Host](http://swagger.io/specification/#fixed-fields-15)|`Function app settings` > `Go to App Service Settings` > `Overview` > `URL`|*Nicht vorhanden*
|[Paths](http://swagger.io/specification/#paths-object-29)|`Integrate` > `Selected HTTP methods`|Bindungen: Route
|[Path Item](http://swagger.io/specification/#path-item-object-32)|`Integrate` > `Route template`|Bindungen: Methoden
|[Sicherheit](http://swagger.io/specification/#security-scheme-object-112)|Schlüssel|*Nicht vorhanden*|
|operationID*|„Route“ und „Zulässige Verben“|„Route“ und „Allowed Verbs“|

\*operationID ist nur für Einbinden in PowerApps und Flow erforderlich.
> [!NOTE]
>  „x-ms-summary“ stellt einen Anzeigenamen in Logic Apps, Flow und PowerApps bereit.
>
> Weitere Informationen finden Sie unter [Customize your Swagger definition for PowerApps](https://powerapps.microsoft.com/tutorials/customapi-how-to-swagger/).

## <a name="CICD"></a>Verwenden von CI/CD, um eine API-Definition festzulegen

 Sie müssen das Hosten von API-Definitionen im Portal aktivieren, bevor Sie es ermöglichen, Ihre API-Definition aus der Quellcodeverwaltung ändern zu können. Befolgen Sie die nachstehenden Anweisungen.

1. Navigieren Sie zu `API Definition (preview)` in den Einstellungen Ihrer Funktionen-App.
  1. Legen Sie `API definition source` auf `Function` fest.
  1. Klicken Sie auf `Generate API definition template` und dann auf `Save`, um eine Vorlagendefinition zu erstellen, die Sie später ändern können.
  1. Notieren Sie Ihre `API definition URL` und Ihren `key`.
1. [Richten Sie CI/CD ein.](https://docs.microsoft.com/azure/azure-functions/functions-continuous-deployment#continuous-deployment-requirements)
2. Ändern Sie Ihre `swagger.json`-Datei in der Quellcodeverwaltung zu `\site\wwwroot\.azurefunctions\swagger\swagger.json`.

Änderungen an `swagger.json` in Ihrem Repository werden nun von Ihrer Funktionen-App mit der `API definition URL` und dem `key` gehostet, die Sie im Schritt 1.3 notiert haben.

## <a name="next-steps"></a>Nächste Schritte
* [Erste-Schritte-Tutorial](functions-api-definition-getting-started.md)
  * Probieren Sie unsere exemplarische Vorgehensweise aus, um eine OpenAPI-Definition in Aktion zu sehen!
* [Azure Functions-GitHub-Repository](https://github.com/Azure/Azure-Functions/)
  * Testen Sie Functions-Github (Azure Functions), und geben uns Feedback zur Vorschau der Unterstützung von API-Definitionen! Erstellen Sie ein Github-Problem für jeden Punkt, für den Sie eine Aktualisierung wünschen.
* [Entwicklerreferenz zu Azure Functions](functions-reference.md)  
  * Referenz zum Programmieren von Funktionen sowie zum Festlegen von Triggern und Bindungen.

