---
title: Exportieren einer in Azure gehosteten API nach PowerApps und Microsoft Flow | Microsoft-Dokumentation
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
ms.date: 06/20/2017
ms.author: mahender
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 0d166a2e5b60c3b9f911f9fc3ad49ad7f252abb4
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017


---
# <a name="exporting-an-azure-hosted-api-to-powerapps-and-microsoft-flow"></a>Exportieren einer in Azure gehosteten API nach PowerApps und Microsoft Flow

## <a name="creating-custom-connectors-for-powerapps-and-microsoft-flow"></a>Erstellen von benutzerdefinierten Connectors für PowerApps und Microsoft Flow

[PowerApps](https://powerapps.com) ist ein Dienst zum Erstellen und Verwenden benutzerdefinierter Branchen-Apps, die eine Verbindung mit Ihren Daten herstellen und plattformübergreifend verwendet werden können. [Microsoft Flow](https://flow.microsoft.com) ermöglicht die problemlose Automatisierung von Workflows und Geschäftsprozessen zwischen Ihren bevorzugten Apps und Diensten. Sowohl PowerApps als auch Microsoft Flow verfügt über verschiedene integrierte Connectors für Datenquellen wie Office 365, Dynamics 365 und Salesforce. Benutzer müssen aber auch Datenquellen und APIs nutzen können, die von ihrer Organisation erstellt werden.

Analog dazu möchten Entwickler, die eine umfassendere Bereitstellung ihrer APIs in der Organisation anstreben, ihre APIs unter Umständen für PowerApps und Microsoft Flow-Benutzern verfügbar machen. In diesem Thema erfahren Sie, wie Sie eine mit Azure App Service oder Azure Functions erstellte API für PowerApps und Microsoft Flow verfügbar machen. [Azure App Service](https://azure.microsoft.com/services/app-service/) ist ein Platform-as-a-Service-Angebot, mit dem Entwickler schnell und einfach professionelle Web-, Mobil- und API-Anwendungen erstellen können. [Azure Functions](https://azure.microsoft.com/services/functions/) ist eine ereignisbasierte serverlose Computelösung zur schnellen Erstellung von Code, der auf andere Teile Ihres Systems reagieren und bedarfsgerecht skalieren kann.

Weitere Informationen zu diesen Diensten finden Sie unter den folgenden Links:
- [PowerApps Guided Learning](https://powerapps.microsoft.com/guided-learning/learning-introducing-powerapps/) (Angeleitetes Lernen für PowerApps) 
- [Angeleitetes Lernen für Microsoft Flow](https://flow.microsoft.com/guided-learning/learning-introducing-flow/)
- [Was ist App Service?](https://docs.microsoft.com/azure/app-service/app-service-value-prop-what-is)
- [Übersicht zu Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview)

## <a name="sharing-an-api-definition"></a>Gemeinsames Verwenden einer API-Definition

APIs werden häufig mithilfe eines [OpenAPI-Dokuments](https://www.openapis.org/) (manchmal auch „Swagger-Dokument“ genannt) beschrieben. Dieses Dokument enthält alle Informationen zu den verfügbaren Vorgängen und zur Strukturierung der Daten. PowerApps und Microsoft Flow können benutzerdefinierte Connectors für jedes beliebige OpenAPI 2.0-Dokument erstellen. Nach der Erstellung kann ein benutzerdefinierter Connector auf die gleiche Weise verwendet werden wie einer der integrierten Connectors und schnell in eine Anwendung integriert werden.

In Azure App Service und Azure Functions ist die Unterstützung für das Erstellen, Hosten und Verwalten eines OpenAPI-Dokuments bereits [integriert](https://docs.microsoft.com/azure/app-service-api/app-service-api-metadata). Um einen benutzerdefinierten Connector für eine Web-, mobile, API- oder Funktionen-App zu erstellen, müssen eine Kopie der Definition für PowerApps und Flow bereitgestellt werden.

> [!NOTE]
> Da eine Kopie der API-Definition verwendet wird, kommen Aktualisierungen oder wichtige Änderungen erst mit einer gewissen Verzögerung bei PowerApps und Microsoft Flow an. Wenn eine neue Version der API verfügbar wird, sollten diese Schritte für die neue Version wiederholt werden. 

So stellen Sie PowerApps und Microsoft Flow die gehostete API-Definition für Ihre App bereit:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und navigieren Sie zu Ihrer App Service- oder Azure Functions-Anwendung.

    Wählen Sie bei Verwendung von Azure App Service in der Liste mit den Einstellungen die Option **API-Definition** aus. 
    
    Wenn Sie Azure Functions verwenden, wählen Sie Ihre Funktionen-App aus, und klicken Sie dann auf **Plattformfeatures** und **API-Definition**. Sie können auch stattdessen die Registerkarte **API-Definition (Vorschau)** öffnen.

2. Falls eine API-Definition bereitgestellt wurde, wird eine Schaltfläche zum **Exportieren in PowerApps und Microsoft Flow** angezeigt. Klicken Sie auf diese Schaltfläche, um den Exportvorgang zu starten.

3. Wählen Sie den **Exportmodus**. Davon hängen die Schritte ab, die Sie zum Erstellen eines Connectors durchführen müssen. App Service bietet zwei Optionen zur Bereitstellung Ihrer API-Definition für PowerApps und Microsoft Flow:

    Mit **Express** können Sie innerhalb des Azure-Portals den benutzerdefinierten Connector erstellen. Für diese Option muss der aktuell angemeldete Benutzer die Berechtigung zum Erstellen von Connectors in der Zielumgebung besitzen. Dies ist die empfohlene Vorgehensweise, wenn diese Anforderung erfüllt werden kann. Wenn Sie diesen Modus verwenden, befolgen Sie die unten stehenden Anweisungen zum [Express-Export](#express).

    Über die Option **Manuell** können Sie eine Kopie der API-Definition exportieren, die über die Portale PowerApps oder Microsoft Flow importiert werden kann. Dies ist die empfohlene Vorgehensweise, wenn der Azure-Benutzer und der Benutzer mit der Berechtigung zum Erstellen von Connectors nicht dieselbe Person sind oder der Connector in einem anderen Mandanten erstellt werden muss. Wenn Sie diesen Modus verwenden, befolgen Sie die unten stehenden Anweisungen zum [manuellen Exportieren und Importieren](#manual).

<a name="express"></a>
## <a name="express-export"></a>Express-Export

In diesem Abschnitt erstellen Sie innerhalb des Azure-Portals einen neuen benutzerdefinierten Connector. Sie müssen bei dem Mandanten, auf dem der Export durchgeführt werden soll, angemeldet sein und über die Berechtigung zum Erstellen eines benutzerdefinierten Connectors in der Zielumgebung verfügen.

1. Wählen Sie die Umgebung aus, in der der Connector erstellt werden soll. Geben Sie dann einen Namen für Ihren benutzerdefinierten Connector ein.

2. Falls Ihre API-Definition Sicherheitsdefinitionen enthält, werden diese in Schritt 2 angegeben. Falls erforderlich, geben Sie die erforderlichen Sicherheitskonfigurationsdetails ein, um Benutzern Zugriff auf Ihre API zu gewähren. Weitere Informationen finden Sie unter [Authentifizierung](#auth). 

3. Klicken Sie auf **OK**, um Ihren benutzerdefinierten Connector zu erstellen.


<a name="manual"></a>
## <a name="manual-export-and-import"></a>Manuelles Exportieren und Importieren

Die Erstellung eines benutzerdefinierten Connectors für eine Web-, Mobil-, API- oder Funktionen-App umfasst zwei Schritte:

1. [Abrufen der API-Definition aus App Service oder Azure Functions](#export)
2. [Importieren der API-Definition in PowerApps und Microsoft Flow](#import)

Diese beiden Schritte müssen in einer Organisation unter Umständen von zwei verschiedenen Personen ausgeführt werden, da möglicherweise kein einzelner Benutzer vorhanden ist, der zur Durchführung beider Aktionen berechtigt ist. In diesem Fall muss ein Entwickler, der für die App Service- oder Azure Functions-Anwendung über das Zugriffsrecht „Mitwirkender“ verfügt, die API-Definition (eine einzelne JSON-Datei) oder einen entsprechenden Link abrufen. Diese Definition muss dann einem PowerApps- oder Microsoft Flow-Besitzer zur Verfügung gestellt werden. Dieser Besitzer kann dann mithilfe der Metadaten den benutzerdefinierten Connector erstellen.

<a name="export"></a>
### <a name="retrieving-the-api-definition-from-app-service-or-azure-functions"></a>Abrufen der API-Definition aus App Service oder Azure Functions

In diesem Abschnitt exportieren Sie die API-Definition für Ihre App Service-API zur späteren Verwendung im Portal PowerApps oder Microsoft Flow.

1. Sie können wählen, ob **Sie die API-Definition herunterladen** oder **einen Link** anfordern möchten. Für beide Fälle wird das Ergebnis im nächsten Abschnitt bereitgestellt. Wählen Sie eine der Optionen aus, und befolgen Sie die Anweisungen.
 
2. Falls Ihre API-Definition Sicherheitsdefinitionen enthält, werden diese in Schritt 2 angegeben. Während des Imports werden sie von PowerApps und Microsoft Flow erkannt, und Sie werden zur Angabe von Sicherheitsinformationen aufgefordert. Erfassen Sie die Anmeldeinformationen für die einzelnen Definitionen, um sie im nächsten Abschnitt verwenden zu können. Weitere Informationen finden Sie unter [Authentifizierung](#auth). 

<a name="import"></a>
### <a name="importing-the-api-definition-into-powerapps-and-microsoft-flow"></a>Importieren der API-Definition in PowerApps und Microsoft Flow

In diesem Abschnitt erstellen Sie mithilfe der zuvor abgerufenen API-Definition einen benutzerdefinierten Connector in PowerApps und Microsoft Flow. Da benutzerdefinierte Connectors von den beiden Diensten gemeinsam genutzt werden, muss die Definition nur einmal importiert werden. Weitere Informationen zu benutzerdefinierten Connectors finden Sie unter [Registrieren und Verwenden von benutzerdefinierten Connectors in PowerApps] und [Registrieren und Verwenden von benutzerdefinierten Connectors in Microsoft Flow].

1. Öffnen Sie das [PowerApps-Webportal](https://web.powerapps.com) oder das [Microsoft Flow-Webportal](https://flow.microsoft.com/), und melden Sie sich an. 

2. Klicken Sie rechts oben auf der Seite auf die Schaltfläche **Einstellungen** (Zahnradsymbol), und wählen Sie **Benutzerdefinierte Connectors** aus. 

3. Klicken Sie auf **Benutzerdefinierten Connector erstellen**.

4. Geben Sie auf der Registerkarte **Allgemein** einen Namen für Ihre API an, und laden Sie anschließend die OpenAPI-Definition hoch, oder fügen Sie die Metadaten-URL ein. Klicken Sie auf **Weiter**.

4. Geben Sie auf der Registerkarte **Sicherheit** die im vorherigen Abschnitt ermittelten Werte ein, wenn Sie zur Angabe von Authentifizierungsinformationen aufgefordert werden. Fahren Sie andernfalls mit dem nächsten Schritt fort.

5. Auf der Registerkarte **Definitionen** werden alle in der OpenAPI-Datei definierten Vorgänge automatisch ausgefüllt. Wenn alle gewünschten Vorgänge definiert sind, können Sie mit dem nächsten Schritt fortfahren. Andernfalls können Sie hier Vorgänge hinzufügen und ändern.

6. Klicken Sie auf **Connector erstellen**. Wenn Sie die API-Aufrufe testen möchten, fahren Sie mit dem nächsten Schritt fort.

7. Erstellen Sie auf der Registerkarte **Test** eine Verbindung, wählen Sie den zu testenden Vorgang aus, und geben Sie die für den Vorgang erforderlichen Daten ein.

8. Klicken Sie auf **Vorgang testen**.


<a name="auth"></a>
## <a name="authentication"></a>Authentifizierung

PowerApps und Microsoft Flow bieten eine systemeigene Unterstützung für eine Sammlung von Identitätsanbietern, die zur Anmeldung von Benutzern Ihres benutzerdefinierten Connectors verwendet werden kann. Falls Ihre API eine Authentifizierung erfordert, stellen Sie sicher, dass sie in Ihrem OpenAPI-Dokument als _Sicherheitsdefinition_ erfasst ist. Während des Exports müssen Sie Konfigurationswerte angeben, mit denen PowerApps und Microsoft Flow Anmeldeaktionen ausführen können.

In diesem Abschnitt werden die vom Express-Fluss unterstützten Authentifizierungstypen behandelt: API-Schlüssel, Azure Active Directory und generisches OAuth 2.0. Die vollständige Liste der Anbieter und die jeweils erforderlichen Anmeldeinformationen finden Sie unter [Registrieren und Verwenden von benutzerdefinierten Connectors in PowerApps] und [Registrieren und Verwenden von benutzerdefinierten Connectors in Microsoft Flow].

### <a name="api-key"></a>API-Schlüssel
Wenn dieses Sicherheitsschema verwendet wird, werden die Benutzer Ihres Connectors beim Herstellen einer Verbindung aufgefordert, den Schlüssel anzugeben. Sie können einen API-Schlüsselnamen angeben, um sie bei der Auswahl des erforderlichen Schlüssels zu unterstützen. Bei Azure Functions ist dies in der Regel einer der Hosttasten, die in der Funktionen-App verschiedene Funktionen erfüllen.

### <a name="azure-active-directory"></a>Azure Active Directory
Bei der Konfiguration eines benutzerdefinierten Connectors, der eine AAD-Anmeldung erfordert, sind zwei AAD-Anwendungsregistrierungen notwendig: eine zur Modellierung der Back-End-API und eine zur Modellierung des Connectors in PowerApps und Flow.

Ihre API sollte für die Verwendung bei der ersten Registrierung konfiguriert sein. Wenn Sie die Funktion [App Service-Authentifizierung/-Autorisierung](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication) verwendet haben, wird dies bereits automatisch durchgeführt.

Sie müssen die zweite Registrierung für den Connector manuell erstellen, indem Sie die Schritte unter [Hinzufügen einer AAD-Anwendung](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application) durchführen. Für die Registrierung muss ein delegierter Zugriff auf Ihre API und die Antwort-URL `https://msmanaged-na.consent.azure-apim.net/redirect` vorhanden sein. Ausführlichere Informationen zum Ersetzen Ihrer API für den Azure Resource Manager finden Sie in [diesem Beispiel](
https://powerapps.microsoft.com/tutorials/customapi-azure-resource-manager-tutorial/).

Die folgenden Konfigurationswerte sind erforderlich:
- **Client-ID** – Die Client-ID der AAD-Registrierung Ihres Connectors
- **Geheimer Clientschlüssel** – Der geheime Clientschlüssel der AAD-Registrierung Ihres Connectors
- **Anmelde-URL** – Die Basis-URL für AAD. Im öffentlichen Azure lautet diese in der Regel `https://login.windows.net`.
- **Mandanten-ID** – Die für die Anmeldung verwendete ID des Mandanten. Diese sollte allgemein oder die ID des Mandanten sein, in dem der Connector erstellt wird.
- **Ressourcen-URL** – Die Ressourcen-URL der AAD-Registrierung Ihres API-Back-Ends

> [!IMPORTANT]
> Falls die API-Definition im Rahmen des manuellen Flusses von einer anderen Person in PowerApps und Microsoft Flow importiert wird, stellen Sie die Client-ID und den geheimen Clientschlüssel **der Connectorregistrierung** sowie die Ressourcen-URL Ihrer API bereit. Achten Sie auf eine sichere Verwaltung dieser Geheimnisse. **Geben Sie die Sicherheitsanmeldeinformationen der API nicht weiter.**

### <a name="generic-oauth-20"></a>Generisches OAuth 2.0
Dank der Unterstützung für das generische OAuth 2.0 ist eine Integration in jedem OAuth 2.0-Anbieter möglich. Dadurch können Sie einen beliebigen benutzerdefinierten Anbieter verwenden, der systemintern nicht unterstützt wird.

Die folgenden Konfigurationswerte sind erforderlich:
- **Client-ID** – Client-ID von OAuth 2.0
- **Geheimer Clientschlüssel** – Der geheime OAuth 2.0-Clientschlüssel
- **Autorisierungs-URL** – Die OAuth 2.0-Autorisierungs-URL
- **Token-URL** – Die OAuth 2.0-Token-URL
- **Aktualisierungs-URL** – Die OAuth 2.0-Aktualisierungs-URL



[Registrieren und Verwenden von benutzerdefinierten Connectors in PowerApps]: https://powerapps.microsoft.com/tutorials/register-custom-api/
[Registrieren und Verwenden von benutzerdefinierten Connectors in Microsoft Flow]: https://flow.microsoft.com/documentation/register-custom-api/

