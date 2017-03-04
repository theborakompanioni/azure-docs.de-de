---
title: Aufrufen benutzerdefinierter APIs in Azure Logic Apps | Microsoft-Dokumentation
description: Aufrufen Ihrer eigenen benutzerdefinierten APIs in Azure App Service mit Azure Logic Apps
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2016
ms.author: stepsic
translationtype: Human Translation
ms.sourcegitcommit: fc509ef8b30fadb6e026f346d4adbd6ef759624a
ms.openlocfilehash: 74aae9f757f56e94b583069a1fdee7efaafe467c
ms.lasthandoff: 02/16/2017


---
# <a name="call-custom-apis-hosted-on-azure-app-service-with-azure-logic-apps"></a>Aufrufen benutzerdefinierter APIs in Azure App Service mit Azure Logic Apps

Zwar bietet Azure Logic Apps mehr als 40 Connectors für eine Vielzahl von Diensten, trotzdem möchten Sie möglicherweise Ihre eigene benutzerdefinierte API aufrufen, die Ihren eigenen Code ausführen kann. Azure App Service stellt die einfachste und am besten skalierbare Methode zum Hosten Ihrer eigenen benutzerdefinierten Web-APIs bereit. In diesem Artikel wird beschrieben, wie Sie eine Web-API aufrufen, die in einer App Service-API-App, einer Web-App oder einer mobilen App gehostet wird.
Erfahren Sie, [wie APIs als Trigger oder Aktion in Logik-Apps erstellt werden](../logic-apps/logic-apps-create-api-app.md).

## <a name="deploy-your-web-app"></a>Bereitstellen Ihrer Web-App

Zunächst müssen Sie die API als Web-App in Azure App Service bereitstellen. Erfahren Sie mehr über die [grundlegende Bereitstellung beim Erstellen einer ASP.NET-Web-App](../app-service-web/web-sites-dotnet-get-started.md). Obwohl Sie jede API aus einer Logik-App aufrufen können, sollten Sie am besten Swagger-Metadaten hinzufügen, um die Integration in Logik-App-Aktionen zu vereinfachen. Erfahren Sie mehr über das [Hinzufügen von Swagger-Metadaten ](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui).

### <a name="api-settings"></a>API-Einstellungen

Damit der Logik-App-Designer Ihre Swagger-Daten analysieren kann, müssen Sie CORS aktivieren und die Eigenschaften der API-Definition für Ihre Web-App festlegen. 

1.    Wählen Sie Ihre Web-App im Azure-Portal aus.
2.    Suchen Sie auf dem geöffneten Blatt nach **API**, und wählen Sie **API-Definition** aus. Legen Sie den **Speicherort der API-Definition** auf die URL Ihrer swagger.json-Datei fest.

    Normalerweise lautet diese URL „https://{Name}.azurewebsites.net/swagger/docs/v1)“.

3.    Um Anforderungen vom Logik-App-Designer zuzulassen, wählen Sie unter **API** die Option **CORS** aus, und legen Sie eine CORS-Richtlinie für „*“ fest.

## <a name="call-into-your-custom-api"></a>Aufrufen der benutzerdefinierten API

Wenn Sie CORS und die Eigenschaften der API-Definition festlegen, sollten Sie auf einfache Weise Aktionen benutzerdefinierter APIs im Workflow im Logic Apps-Portal hinzufügen können. Sie können im Logik-Apps-Designer Ihre Abonnementwebsites durchsuchen, um die Websites aufzulisten, für die eine Swagger-URL definiert wurde. Sie können auch auf Swagger verweisen und die verfügbaren Aktionen und Eingaben auflisten, indem Sie die Aktion „HTTP + Swagger“ verwenden. Und schließlich können Sie jederzeit eine Anforderung mit der HTTP-Aktion erstellen, um beliebige APIs aufzurufen. Dazu gehören auch APIs, die nicht über ein Swagger-Dokument verfügen oder ein solches auch nicht verfügbar machen.

Zum Schützen Ihrer API gibt es verschiedene Möglichkeiten:

*    Codeänderungen sind nicht erforderlich. Sie können Azure Active Directory verwenden, um ohne Änderung des Codes oder ohne eine erneute Bereitstellung Ihre API zu schützen.
*    Erzwingen Sie im Code der API eine Standardauthentifizierung, eine Azure Active Directory-Authentifizierung oder eine Zertifikatauthentifizierung.

## <a name="secure-calls-to-your-api-without-changing-code"></a>Sichern von Aufrufen Ihrer API ohne Codeänderungen

In diesem Abschnitt erstellen Sie zwei Azure Active Directory-Anwendungen – eine für Ihre Logik-App und eine für Ihre Web-App. Sie authentifizieren Aufrufe Ihrer Web-App mit dem Dienstprinzipal (Client-ID und geheimer Schlüssel), der der Azure Active Directory-App Ihrer Logik-App zugeordnet ist. Schließlich nehmen Sie die Anwendungs-IDs in die Definition der Logik-App auf.

### <a name="part-1-set-up-an-application-identity-for-your-logic-app"></a>Teil 1: Einrichten einer Anwendungsidentität für Ihre Logik-App

Die Logik-App verwendet diese Anwendungsidentität zum Authentifizieren bei Azure Active Directory. Sie müssen diese Identität für Ihr Verzeichnis nur einmal einrichten. Sie können z.B. die gleiche Identität für alle Logik-Apps verwenden, Sie können aber auch pro Logik-App eindeutige Identitäten erstellen. Sie können diese Identitäten im Azure-Portal oder über PowerShell einrichten.

#### <a name="create-the-application-identity-in-the-azure-classic-portal"></a>Erstellen der Anwendungsidentität im klassischen Azure-Portal

1. Navigieren Sie im klassischen Azure-Portal zu [Azure Active Directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory). 
2. Wählen Sie das Verzeichnis aus, das Sie für Ihre Web-App verwenden.
3. Wählen Sie die Registerkarte **Anwendungen** aus. Wählen Sie auf der Befehlsleiste am unteren Rand der Seite **Hinzufügen** aus.
5. Geben Sie Ihrer App-Identität einen Namen, klicken Sie auf den Pfeil „Weiter“.
6. Geben Sie unter **App-Eigenschaften** eine eindeutige Zeichenfolge ein, die als Domäne formatiert ist, und klicken Sie auf das Häkchen.
7. Wählen Sie die Registerkarte **Konfigurieren** aus. Wechseln Sie zu **Client-ID**, und kopieren Sie die Client-ID zur Verwendung in Ihrer Logik-App.
8. Öffnen Sie unter **Schlüssel** die Liste **Dauer auswählen**, und wählen Sie die Dauer für den Schlüssel aus.
9. Klicken Sie unten auf der Seite auf **Speichern**. Möglicherweise müssen Sie einige Sekunden warten.
10. Sie sollten den Schlüssel kopieren, der jetzt unter **Schlüssel** angezeigt wird, damit Sie diesen Schlüssel in der Logik-App verwenden können.

#### <a name="create-the-application-identity-using-powershell"></a>Erstellen der Anwendungsidentität mithilfe von PowerShell

1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. Kopieren Sie die **Mandanten-ID**, die **Anwendungs-ID** und das Kennwort, das Sie verwendet haben.

### <a name="part-2-protect-your-web-app-with-an-azure-active-directory-app-identity"></a>Teil 2: Schützen der Web-App mit einer Azure Active Directory-App-Identität

Wenn Ihre Web-App bereits bereitgestellt wurde, können Sie die Autorisierung im Azure-Portal aktivieren. Andernfalls können Sie den Abschnitt für die Autorisierung in der Azure Resource Manager-Bereitstellung verwenden.

#### <a name="enable-authorization-in-the-azure-portal"></a>Aktivieren der Autorisierung im Azure-Portal

1. Suchen Sie Ihre Web-App, und wählen Sie sie aus. Wählen Sie unter **Einstellungen** die Option **Authentifizierung/Autorisierung** aus.
2. Legen Sie unter **App Service-Authentifizierung** die Authentifizierung auf **Ein** fest, und wählen Sie **Speichern** aus.

An diesem Punkt wird eine Anwendung automatisch für Sie erstellt. Sie benötigen die Client-ID dieser Anwendung für Teil 3, daher müssen Sie folgende Schritte ausführen:

1. Navigieren Sie im klassischen Azure-Portal zu [Azure Active Directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory).
2.    Wählen Sie Ihr Verzeichnis.
3. Suchen Sie im Suchfeld nach Ihrer App.
4. Wählen Sie in der Liste Ihre App aus.
5. Wählen Sie die Registerkarte **Konfigurieren** aus, auf der die **Client-ID** angezeigt wird.

#### <a name="deploy-your-web-app-using-an-azure-resource-manager-template"></a>Bereitstellen Ihrer Web-App mithilfe einer Azure Resource Manager-Vorlage

Zunächst müssen Sie eine Anwendung für Ihre Web-App erstellen, die sich von der Anwendung unterscheidet, die Sie für Ihre Logik-App verwenden. Folgen Sie den Schritten in Teil 1 oben, verwenden Sie aber nun für **HomePage** und **IdentifierUris** den tatsächlichen Wert für „https://**URL**“ der Web-App.

> [!NOTE]
> Wenn Sie die Anwendung für Ihre Web-App erstellen, müssen Sie das [klassische Azure-Portal](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) verwenden. Mit dem PowerShell-Cmdlet werden nicht die erforderlichen Berechtigungen für die Anmeldung der Benutzer bei einer Website eingerichtet.

Sobald Sie über die Client-ID und die Mandanten-ID verfügen, nehmen Sie diesen Teil als untergeordnete Ressource der Web-App in die Bereitstellungsvorlage auf:

```
"resources": [
    {
        "apiVersion": "2015-08-01",
        "name": "web",
        "type": "config",
        "dependsOn": ["[concat('Microsoft.Web/sites/','parameters('webAppName'))]"],
        "properties": {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

Um eine Bereitstellung automatisch auszuführen, mit der gleichzeitig eine leere Web-App und eine Logik-App bereitgestellt werden, die Azure Active Directory verwenden, klicken Sie auf die Schaltfläche **Deploy to Azure**:

[![In Azure bereitstellen](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

Die vollständige Vorlage finden Sie unter [Logik-App ruft eine benutzerdefinierte API auf, die in App Service gehostet und von Azure Active Directory geschützt wird](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json).

### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>Teil 3: Ausfüllen des Abschnitts für die Autorisierung in Ihrer Logik-App

Im Abschnitt für die **Autorisierung** der **HTTP**-Aktion:

`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| Element | Beschreibung |
| ------- | ----------- |
| type |Die Art der Authentifizierung. Für die ActiveDirectoryOAuth-Authentifizierung lautet der Wert `ActiveDirectoryOAuth`. |
| Mandant |Die Mandanten-ID zum Identifizieren des AD-Mandanten. |
| audience |Erforderlich. Die Ressource, mit der Sie eine Verbindung herstellen. |
| clientID |Die Client-ID für die Azure AD-Anwendung. |
| secret |Erforderlich. Der geheime Schlüssel des Clients, der das Token anfordert. |

In der obigen Vorlage ist der Abschnitt für die Autorisierung bereits eingerichtet, aber wenn Sie die Logik-App direkt erstellen, müssen Sie den gesamten Abschnitt für die Autorisierung aufnehmen.

## <a name="secure-your-api-in-code"></a>Sichern Ihrer API im Code

### <a name="certificate-authentication"></a>Zertifikatauthentifizierung

Clientzertifikate können zum Überprüfen der eingehenden Anforderungen bei der Web-App verwendet werden. Informationen zum Einrichten des Codes finden Sie unter [Konfigurieren der gegenseitigen TLS-Authentifizierung für eine Web-App](../app-service-web/app-service-web-configure-tls-mutual-auth.md) .

Im Abschnitt für die **Autorisierung** sollten Sie Folgendes angeben: 

`{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`

| Element | Beschreibung |
| ------- | ----------- |
| type |Erforderlich. Die Art der Authentifizierung. Für SSL-Clientzertifikate muss der Wert `ClientCertificate` lauten. |
| pfx |Erforderlich. Base64-codierte Inhalte der PFX-Datei. |
| password |Erforderlich. Kennwort für den Zugriff auf die PFX-Datei. |

### <a name="basic-authentication"></a>Standardauthentifizierung

Sie können die Standardauthentifizierung (z.B. Benutzername und Kennwort) verwenden, um die eingehenden Anforderungen zu überprüfen. Die Standardauthentifizierung ist ein gängiges Muster, und Sie können diese Authentifizierung in jeder Sprache verwenden, mit der Ihre App erstellt wird.

Im Abschnitt für die **Autorisierung** sollten Sie Folgendes angeben:

`{"type": "basic","username": "test","password": "test"}`.

| Element | Beschreibung |
| --- | --- |
| type |Erforderlich. Die Art der Authentifizierung. Für die Standardauthentifizierung muss der Wert `Basic`lauten. |
| username |Erforderlich. Der zu authentifizierende Benutzername. |
| password |Erforderlich. Das zu authentifizierende Kennwort. |

### <a name="handle-azure-active-directory-authentication-in-code"></a>Verarbeiten der Azure Active Directory-Authentifizierung im Code

Standardmäßig führt die Azure Active Directory-Authentifizierung, die Sie im Azure-Portal aktivieren, keine fein abgestufte Autorisierung durch. Beispielsweise legt diese Authentifizierung Ihre API nicht auf einen bestimmten Benutzer oder eine App fest, sondern nur auf einen bestimmten Mandanten.

Um Ihre API z.B. auf Ihre Logik-App zu beschränken, extrahieren Sie im Code den Header, der das JWT enthält. Überprüfen Sie die Identität des Aufrufers, und lehnen Sie Anforderungen ab, die nicht übereinstimmen.

Weitere Informationen dazu, wie Sie diese Authentifizierung vollständig in Ihrem eigenen Code implementieren können, ohne die Funktion im Azure-Portal zu verwenden, finden Sie unter [Authentifizieren mit lokaler Active Directory-Instanz in Ihrer Azure-App](../app-service-web/web-sites-authentication-authorization.md).
Sie müssen die obigen Schritte ausführen, um die Anwendungsidentität für Ihre Logik-App zu erstellen und diese Identität zum Aufrufen Ihrer API zu verwenden.
