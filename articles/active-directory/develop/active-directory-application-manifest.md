---
title: Grundlegendes zum Azure Active Directory-Anwendungsmanifest | Microsoft Docs
description: "Sie erhalten ausführliche Informationen über das Azure Active Directory-Anwendungsmanifest, das die Identitätskonfiguration einer Anwendung in einem Azure AD-Mandanten darstellt, und für die OAuth-Autorisierung, den Zustimmungsprozess und andere Zwecke genutzt wird."
services: active-directory
documentationcenter: 
author: sureshja
manager: mbaldwin
editor: 
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2017
ms.author: sureshja
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: 7d6525f4614c6301f0ddb621b0483da70842a71b
ms.openlocfilehash: 2dc166a346c58d43e9ed60332f47619c1de89816
ms.contentlocale: de-de
ms.lasthandoff: 02/11/2017


---
# <a name="understanding-the-azure-active-directory-application-manifest"></a>Grundlegendes zum Azure Active Directory-Anwendungsmanifest
Anwendungen, die in Azure Active Directory (AD) integriert werden, müssen bei einem Azure AD-Mandanten registriert werden, um eine dauerhafte Identitätskonfiguration für die Anwendung bereitzustellen. Diese Konfiguration wird zur Laufzeit geprüft und ermöglicht so Szenarien, in denen eine Anwendung die Authentifizierung/Autorisierung über Azure AD abwickelt/vermittelt. Weitere Informationen zum Azure AD-Anwendungsmodell finden Sie im Artikel [Hinzufügen, Aktualisieren und Entfernen einer Anwendung][ADD-UPD-RMV-APP].

## <a name="updating-an-applications-identity-configuration"></a>Aktualisieren der Identitätskonfiguration für eine Anwendung
Es gibt verschiedene Optionen zum Aktualisieren der Eigenschaften für die Identitätskonfiguration einer Anwendung, die sich im Hinblick auf Funktionen und Komplexität unterscheiden. Es stehen u. a. folgende Optionen zur Verfügung:

* Die **Webbenutzeroberfläche im [Azure-Portal][AZURE-PORTAL]** ermöglicht es Ihnen, die gängigsten Eigenschaften einer Anwendung zu aktualisieren. Dies ist die schnellste und am wenigsten fehleranfällige Möglichkeit zum Aktualisieren der Anwendungseigenschaften, sie bietet jedoch im Gegensatz zu den zwei weiteren Methoden keinen vollständigen Zugriff auf alle Eigenschaften.
* Für einige erweiterte Szenarien, in denen Sie Eigenschaften aktualisieren müssen, die nicht im klassischen Azure-Portal verfügbar gemacht werden, können Sie das **Anwendungsmanifest**ändern. Dies ist der Schwerpunkt des vorliegenden Artikels, ausführliche Informationen erhalten Sie ab dem nächsten Abschnitt.
* Sie können zum Aktualisieren Ihrer Anwendung auch **eine Anwendung schreiben, die die [Graph-API][GRAPH-API]** nutzt. Dieser Ansatz erfordert den meisten Aufwand. Dies kann eine attraktive Option sein, wenn Sie Verwaltungssoftware schreiben oder die Anwendungseigenschaften regelmäßig und in automatisierter Weise aktualisieren müssen.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Verwenden des Anwendungsmanifests zum Aktualisieren der Identitätskonfiguration einer Anwendung
Über das [Azure-Portal][AZURE-PORTAL] können Sie die Identitätskonfiguration Ihrer Anwendung verwalten, indem Sie das Anwendungsmanifest mit dem Inline-Manifest-Editor aktualisieren. Sie können das Anwendungsmanifest auch als JSON-Datei herunter- und hochladen. Im Verzeichnis wird keine wirkliche Datei gespeichert. Das Anwendungsmanifest ist lediglich ein für die Azure AD-Graph-API-Anwendungsentität ausgeführter HTTP-GET-Vorgang, und das Hochladen ist ein HTTP-PATCH-Vorgang für die Anwendungsentität.

Um deshalb das Format und die Eigenschaften des Anwendungsmanifests verstehen zu können, müssen Sie die Graph-API-Dokumentation zur [Anwendungsentität][APPLICATION-ENTITY] zu Rate ziehen. Nachfolgend werden einige Beispiele für Aktualisierungen aufgeführt, die Sie über ein Anwendungsmanifest durchführen können:

* **Deklarieren Sie Berechtigungsbereiche (oauth2Permissions)**, die von Ihrer Web-API verfügbar gemacht werden. Informationen zum Implementieren von Benutzeridentitätswechseln mithilfe des delegierten Berechtigungsbereichs „oauth2Permissions“ finden Sie im Thema „Bereitstellen von Web-APIs für andere Anwendungen“ des Artikels [Integrieren von Anwendungen in Azure Active Directory][INTEGRATING-APPLICATIONS-AAD]. Wie bereits erwähnt, werden Eigenschaften von Anwendungsentitäten im Graph-API-Referenzartikel [Entity and complex type reference][APPLICATION-ENTITY] (Entität und komplexer Typ) dokumentiert, darunter auch die oauth2Permissions-Eigenschaft. Hierbei handelt es sich um eine Auflistung des Typs [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
* **Deklarieren Sie Anwendungsrollen (appRoles), die von Ihrer App verfügbar gemacht werden**. Die appRoles-Eigenschaft der Anwendungsentität ist eine Auflistung vom Typ [AppRole][APPLICATION-ENTITY-APP-ROLE]. Ein Implementierungsbeispiel finden Sie unter im Artikel zur [rollenbasierten Zugriffssteuerung in Cloudanwendungen mit Azure AD][RBAC-CLOUD-APPS-AZUREAD].
* **Deklarieren bekannter Clientanwendungen (knownClientApplications)**, mit deren Hilfe Sie die Zustimmung der angegebenen Clientanwendungen logisch an die Ressourcen-/Web-API binden können
* **Fordern Sie bei Azure AD die Ausgabe eines Anspruchs für die Gruppenmitgliedschaften** des angemeldeten Benutzers an (groupMembershipClaims).  Durch eine Konfiguration kann ein Anspruch für die Verzeichnisrollenmitgliedschaften des Benutzers ausgegeben werden. Ein Implementierungsbeispiel finden Sie im Artikel [Autorisierung in Cloudanwendungen mit AD-Gruppen][AAD-GROUPS-FOR-AUTHORIZATION].
* **Ermöglichen Sie für Ihre Anwendung die Unterstützung von impliziten OAuth 2.0-Gewährungsworkflows** (oauth2AllowImplicitFlow). Diese Art von Code Grant-Datenflüssen wird mit eingebetteten JavaScript-Webseiten oder Single-Page-Anwendungen (SPA) verwendet. Weitere Informationen zur impliziten Gewährung der Autorisierung finden Sie unter [Grundlegendes zum Ablauf der impliziten OAuth2-Gewährung in Azure Active Directory (AD)][IMPLICIT-GRANT].
* **Aktivieren Sie die Verwendung von X509-Zertifikaten als geheimer Schlüssel** (keyCredentials). Beispiele für die Implementierung finden Sie in den Artikeln [Builddienst und Daemon-Apps in Office 365][O365-SERVICE-DAEMON-APPS] und [Entwicklerhandbuch für die Authentifizierung mit Azure Resource Manager-API][DEV-GUIDE-TO-AUTH-WITH-ARM].
* **Fügen Sie einen neuen App-ID-URI** für Ihre Anwendung hinzu (identifierURIs[]). Mit App-ID-URIs wird eine Anwendung in ihrem Azure AD-Mandanten eindeutig identifiziert (bzw. in Szenarios mit mehreren Mandanten wird die Anwendung in mehreren Azure AD-Mandanten identifiziert, wenn sie über eine überprüfte benutzerdefinierte Domäne qualifiziert wurde). Sie werden beim Anfordern von Berechtigungen für eine Ressourcenanwendung oder beim Abrufen eines Zugriffstokens für eine Ressourcenanwendung genutzt. Bei der Aktualisierung dieses Elements wird auch die entsprechende Sammlung „servicePrincipalNames []“ des Dienstprinzipals aktualisiert, die sich im Home-Mandanten der Anwendung befindet.

Das Anwendungsmanifest bietet außerdem eine gute Möglichkeit, den Status Ihrer Anwendungsregistrierung nachzuverfolgen. Da dieser im JSON-Format verfügbar ist, kann die Dateidarstellung in Ihrer Quellcodeverwaltung geprüft werden, gemeinsam mit dem Quellcode der Anwendung.

## <a name="step-by-step-example"></a>Schritt-für-Schritt-Beispiel
Nun gehen wir die erforderlichen Schritte zum Aktualisieren der Identitätskonfiguration Ihrer Anwendung über das Anwendungsmanifest durch. Anhand eines der obigen Beispiele zeigen wir, wie Sie für eine Ressourcenanwendung einen neuen Berechtigungsbereich deklarieren:

1. Melden Sie sich beim [Azure-Portal][AZURE-PORTAL] an.
2. Wählen Sie nach der Authentifizierung Ihren Azure AD-Mandanten aus, indem Sie ihn in der oberen rechten Ecke der Seite auswählen.
3. Wählen Sie im Navigationsbereich auf der linken Seite die **Azure Active Directory**-Erweiterung aus, und klicken Sie auf **App-Registrierungen**.
4. Suchen Sie in der Liste nach der Anwendung, die Sie aktualisieren möchten, und klicken Sie darauf.
5. Klicken Sie auf der Anwendungsseite auf **Manifest**, um den Inline-Manifest-Editor zu öffnen. 
6. Das Manifest kann mit diesem Editor direkt bearbeitet werden. Beachten Sie, dass das Manifest dem Schema für die [Anwendungsentität][APPLICATION-ENTITY] folgt, wie zuvor bereits erwähnt: Angenommen, wir möchten eine neue Berechtigung mit dem Namen „Employees.Read.All“ für unsere Ressourcenanwendung (API) implementieren oder verfügbar machen. Hierfür fügen Sie einfach der Auflistung „oauth2Permissions“ ein neues bzw. zweites Element hinzu. Beispiel:
   
        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],
   
    Der Eintrag muss eindeutig sein, sodass Sie eine neue globale eindeutige ID (GUID) für die `"id"`-Eigenschaft generieren müssen. Da wir `"type": "User"` angegeben haben, kann diese Berechtigung hier die Zustimmung über jedes Konto erhalten, das mit dem Azure AD-Mandanten authentifiziert wurde, unter dem die Ressourcen-/API-Anwendung registriert ist. Der Clientanwendung wird so im Namen des Kontos die Berechtigung für den Zugriff gewährt. Die Zeichenfolgen für die Beschreibung und den Anzeigenamen werden während des Zustimmungsvorgangs und für die Anzeige im Azure-Portal verwendet.
6. Wenn Sie das Manifest aktualisiert haben, klicken Sie auf **Speichern**, um das Manifest zu speichern.  
   
Nachdem das Manifest gespeichert wurde, können Sie einer registrierten Clientanwendung Zugriff auf die neue Berechtigung gewähren, die wir oben hinzugefügt haben. Dieses Mal können Sie die Webbenutzeroberfläche des Azure-Portals verwenden, anstatt das Manifest der Clientanwendung zu bearbeiten:  

1. Wechseln Sie zunächst zum Blatt **Einstellungen** der Clientanwendung, der Sie Zugriff auf die neue API hinzufügen möchten. Klicken Sie auf **Erforderliche Berechtigungen**, und wählen Sie **API auswählen**.
2. Anschließend wird die Liste mit den registrierten Ressourcenanwendungen (APIs) des Mandanten angezeigt. Klicken Sie auf die Ressourcenanwendung, um sie auszuwählen, oder geben Sie den Namen der Anwendung im Suchfeld ein. Wenn Sie die Anwendung gefunden haben, klicken Sie auf **Auswählen**.  
3. Dadurch gelangen Sie zur Seite **Berechtigungen auswählen**, in der die Liste der Anwendungsberechtigungen und der delegierten Berechtigungen angezeigt wird, die für die Ressourcenanwendung verfügbar sind. Wählen Sie die neue Berechtigung aus, um sie der angeforderten Liste mit den Berechtigungen des Clients hinzuzufügen. Diese neue Berechtigung wird in der Identitätskonfiguration der Clientanwendung in der Auflistungseigenschaft „requiredResourceAccess“ gespeichert.


Das ist alles. Ihre Anwendungen werden jetzt mit der neuen Identitätskonfiguration ausgeführt.

## <a name="next-steps"></a>Nächste Schritte
* Ausführliche Informationen zu den Beziehungen zwischen Anwendungsobjekten und Dienstprinzipalobjekten der Anwendung finden Sie unter [Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory][AAD-APP-OBJECTS].
* Unter [Azure AD-Entwicklerglossar][AAD-DEVELOPER-GLOSSARY] finden Sie Definitionen für einige der wichtigsten Konzepte für Azure Active Directory-Entwickler (AD).

Verwenden Sie den unten angezeigten Kommentarabschnitt, um uns Feedback zu senden und uns bei der Verbesserung unserer Inhalte zu unterstützen.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/


