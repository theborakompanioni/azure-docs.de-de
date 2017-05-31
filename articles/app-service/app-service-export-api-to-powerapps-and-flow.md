---
title: Exportieren einer in Azure gehosteten API in PowerApps und Microsoft Flow | Microsoft-Dokumentation
description: "Übersicht über das Verfügbarmachen einer in App Service gehosteten API für PowerApps und Microsoft Flow"
services: app-service
documentationcenter: 
author: mattchenderson
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 02/06/2017
ms.author: mahender
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 6a6043de57eb211c93c510cf2a139141c3950662
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Exportieren einer in Azure gehosteten API in PowerApps und Microsoft Flow

## <a name="creating-custom-connectors-for-powerapps-and-microsoft-flow"></a>Erstellen von benutzerdefinierten Connectors für PowerApps und Microsoft Flow

[PowerApps](https://powerapps.com) ist ein Dienst zum Erstellen und Verwenden benutzerdefinierter Branchen-Apps, die eine Verbindung mit Ihren Daten herstellen und plattformübergreifend verwendet werden können. [Microsoft Flow](https://flow.microsoft.com) ermöglicht die problemlose Automatisierung von Workflows und Geschäftsprozessen zwischen Ihren bevorzugten Apps und Diensten. Sowohl PowerApps als auch Microsoft Flow verfügt über verschiedene integrierte Connectors für Datenquellen wie Office 365, Dynamics 365 und Salesforce. Benutzer müssen aber auch Datenquellen und APIs nutzen können, die von ihrer Organisation erstellt werden.

Analog dazu möchten Entwickler, die eine umfassendere Bereitstellung ihrer APIs in der Organisation anstreben, ihre APIs unter Umständen für PowerApps und Microsoft Flow-Benutzern verfügbar machen. In diesem Thema erfahren Sie, wie Sie eine mit Azure App Service oder Azure Functions erstellte API für PowerApps und Microsoft Flow verfügbar machen. [Azure App Service](https://azure.microsoft.com/services/app-service/) ist ein Platform-as-a-Service-Angebot, mit dem Entwickler schnell und einfach professionelle Web-, Mobil- und API-Anwendungen erstellen können. [Azure Functions](https://azure.microsoft.com/services/functions/) ist eine ereignisbasierte serverlose Computelösung zur schnellen Erstellung von Code, der auf andere Teile Ihres Systems reagieren und bedarfsgerecht skalieren kann.

Weitere Informationen zu diesen Diensten finden Sie unter den folgenden Links:
- [PowerApps Guided Learning](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) (Angeleitetes Lernen für PowerApps) 
- [Angeleitetes Lernen für Microsoft Flow](https://flow.microsoft.com/guided-learning/learning-introducing-flow/)
- [Was ist App Service?](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- [Übersicht zu Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)

## <a name="sharing-an-api-definition"></a>Gemeinsames Verwenden einer API-Definition

APIs werden häufig mithilfe eines [Open API-Dokuments](https://www.openapis.org/) (manchmal auch „Swagger-Dokument“ genannt) beschrieben. Dieses Dokument enthält alle Informationen zu den verfügbaren Vorgängen und zur Strukturierung der Daten. PowerApps und Microsoft Flow können benutzerdefinierte Connectors für jedes beliebige Open API 2.0-Dokument erstellen. Nach der Erstellung kann ein benutzerdefinierter Connector auf die gleiche Weise verwendet werden wie einer der integrierten Connectors und schnell in eine Anwendung integriert werden.

In Azure App Service und Azure Functions ist die Unterstützung für das Erstellen, Hosten und Verwalten eines Open API-Dokuments bereits [integriert](https://docs.microsoft.com/azure/app-service-api/app-service-api-metadata). Die Erstellung eines benutzerdefinierten Connectors für eine Web-, Mobil-, API- oder Funktionen-App umfasst zwei Schritte:

1. [Abrufen der API-Definition aus App Service oder Azure Functions](#export)
2. [Importieren der API-Definition in PowerApps](#import)

Diese beiden Schritte müssen in einer Organisation unter Umständen von zwei verschiedenen Personen ausgeführt werden, da möglicherweise kein einzelner Benutzer vorhanden ist, der zur Durchführung beider Aktionen berechtigt ist. In diesem Fall muss ein Entwickler, der für die App Service- oder Azure Functions-Anwendung über das Zugriffsrecht „Mitwirkender“ verfügt, die API-Definition (eine einzelne JSON-Datei) oder einen entsprechenden Link abrufen. Diese Definition muss dann einem PowerApps- oder Microsoft Flow-Besitzer zur Verfügung gestellt werden. Dieser Besitzer kann dann mithilfe der Metadaten den benutzerdefinierten Connector erstellen.

> [!NOTE]
> Da eine Kopie der API-Definition verwendet wird, kommen Aktualisierungen oder wichtige Änderungen erst mit einer gewissen Verzögerung bei PowerApps und Microsoft Flow an. Wenn eine neue Version der API verfügbar wird, sollten diese Schritte für die neue Version wiederholt werden. 

<a name="export"></a>
## <a name="retrieving-the-api-definition-from-app-service-or-azure-functions"></a>Abrufen der API-Definition aus App Service oder Azure Functions

In diesem Abschnitt exportieren Sie die API-Definition für Ihre App Service-API zur späteren Verwendung in PowerApps.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zu Ihrer App Service- oder Azure Functions-Anwendung.

    Wählen Sie bei Verwendung von Azure App Service in der Liste mit den Einstellungen die Option **API-Definition** aus. 
    
    Wählen Sie bei Verwendung von Azure Functions die Option **Funktionen-App-Einstellungen** und anschließend **API-Metadaten konfigurieren** aus.

2. Falls eine API-Definition bereitgestellt wurde, wird eine Schaltfläche zum Exportieren in PowerApps und Microsoft Flow**** angezeigt. Klicken Sie auf diese Schaltfläche, um den Exportvorgang zu starten.

3. Sie können wählen, ob Sie die API-Definition herunterladen**** oder einen Link anfordern ****möchten. Für beide Fälle wird das Ergebnis im nächsten Abschnitt bereitgestellt. Wählen Sie eine der Optionen aus, und befolgen Sie die Anweisungen.
 
4. Falls Ihre API-Definition Sicherheitsdefinitionen enthält, werden diese in Schritt 2 angegeben. Während des Imports werden sie von PowerApps und Microsoft Flow erkannt, und Sie werden zur Angabe von Sicherheitsinformationen aufgefordert. Diese werden von den Diensten für die Anmeldung von Benutzern verwendet, damit sie auf die API zugreifen können. Falls Ihre API eine Authentifizierung erfordert, stellen Sie sicher, dass sie in Ihrem Open API-Dokument als _Sicherheitsdefinition_ erfasst ist.

    Erfassen Sie die Anmeldeinformationen für die einzelnen Definitionen, um sie im nächsten Abschnitt verwenden zu können.
 
> [!NOTE]
> Bei Verwendung der Azure Active Directory-Authentifizierung wird eine neue AAD-App-Registrierung mit delegiertem Zugriff auf Ihre API und der Antwort-URL _https://msmanaged-na.consent.azure-apim.net/redirect_ benötigt. Ausführlichere Informationen finden Sie in [diesem Beispiel](
https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/). (Ersetzen Sie Azure Resource Manager durch Ihre API.)
>
> Falls die API-Definition von einer anderen Person in PowerApps importiert wird, stellen Sie zusätzlich zur API-Definitionsdatei auch die Client-ID und den geheimen Clientschlüssel **der neuen Registrierung** sowie die Ressourcen-URL Ihrer API bereit. Achten Sie auf eine sichere Verwaltung dieser Geheimnisse. **Geben Sie die Sicherheitsanmeldeinformationen der API nicht weiter.**

<a name="import"></a>
## <a name="importing-the-api-definition-into-powerapps-and-microsoft-flow"></a>Importieren der API-Definition in PowerApps und Microsoft Flow

In diesem Abschnitt erstellen Sie mithilfe der zuvor abgerufenen API-Definition einen benutzerdefinierten Connector in PowerApps und Microsoft Flow. Da benutzerdefinierte Connectors von den beiden Diensten gemeinsam genutzt werden, muss die Definition nur einmal importiert werden. Weitere Informationen zu benutzerdefinierten Connectors finden Sie unter [Registrieren und Verwenden von benutzerdefinierten Connectors in PowerApps] und [Registrieren und Verwenden von benutzerdefinierten Connectors in Microsoft Flow].

**So importieren Sie die API-Definition in PowerApps oder Microsoft Flow**

1. Öffnen Sie das [PowerApps-Webportal](https://web.powerapps.com) oder das [Microsoft Flow-Webportal](https://flow.microsoft.com/), und melden Sie sich an. 

2. Klicken Sie rechts oben auf der Seite auf die Schaltfläche **Einstellungen** (Zahnradsymbol), und wählen Sie **Benutzerdefinierte Connectors** aus. 

3. Klicken Sie auf **Benutzerdefinierten Connector erstellen**.

4. Geben Sie auf der Registerkarte **Allgemein** einen Namen für Ihre API an, und laden Sie anschließend die OpenAPI-Definition hoch, oder fügen Sie die Metadaten-URL ein. Klicken Sie auf **Weiter**.

4. Geben Sie auf der Registerkarte **Sicherheit** die im vorherigen Abschnitt ermittelten Werte ein, wenn Sie zur Angabe von Authentifizierungsinformationen aufgefordert werden. Fahren Sie andernfalls mit dem nächsten Schritt fort.

5. Auf der Registerkarte **Definitionen** werden alle in der OpenAPI-Datei definierten Vorgänge automatisch ausgefüllt. Wenn alle gewünschten Vorgänge definiert sind, können Sie mit dem nächsten Schritt fortfahren. Andernfalls können Sie hier Vorgänge hinzufügen und ändern.

6. Klicken Sie auf **Connector erstellen**. Wenn Sie die API-Aufrufe testen möchten, fahren Sie mit dem nächsten Schritt fort.

7. Erstellen Sie auf der Registerkarte **Test** eine Verbindung, wählen Sie den zu testenden Vorgang aus, und geben Sie die für den Vorgang erforderlichen Daten ein.

8. Klicken Sie auf **Vorgang testen**.



[Registrieren und Verwenden von benutzerdefinierten Connectors in PowerApps]: https://powerapps.microsoft.com/tutorials/register-custom-api/
[Registrieren und Verwenden von benutzerdefinierten Connectors in Microsoft Flow]: https://flow.microsoft.com/documentation/register-custom-api/

