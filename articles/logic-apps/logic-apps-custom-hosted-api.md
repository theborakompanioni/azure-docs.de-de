---
title: "Bereitstellen, Aufrufen und Authentifizieren der Web-APIs und REST-APIs für Azure Logic Apps | Microsoft-Dokumentation"
description: "Bereitstellen, Authentifizieren und Aufrufen der Web-APIs und REST-APIs in Workflows für Systemintegrationen in Azure Logic Apps"
keywords: "Web-APIs, REST-APIs, Anschlüsse, Workflows, Systemintegrationen, Authentifizierung"
services: logic-apps
author: stepsic-microsoft-com
manager: anneta
editor: 
documentationcenter: 
ms.assetid: f113005d-0ba6-496b-8230-c1eadbd6dbb9
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: LADocs; stepsic
ms.translationtype: Human Translation
ms.sourcegitcommit: 9edcaee4d051c3dc05bfe23eecc9c22818cf967c
ms.openlocfilehash: 88c62d5ab046d8cf4bce5d23b776e517bb0e1d87
ms.contentlocale: de-de
ms.lasthandoff: 06/08/2017

---

# <a name="deploy-call-and-authenticate-custom-apis-as-connectors-for-logic-apps"></a>Bereitstellen, Aufrufen und Authentifizieren benutzerdefinierter APIs als Connectors für Logik-Apps

Nachdem Sie den Vorgang [Benutzerdefinierte APIs erstellen](./logic-apps-create-api-app.md) durchgeführt haben, bei dem Aktionen oder Auslöser für Workflows in Logik-Apps bereitgestellt werden, müssen Sie Ihre APIs bereitstellen, bevor Sie sie aufrufen können. Und obwohl Sie alle APIs in einer Logik-App aufrufen können, sollten Sie für optimale Ergebnisse [Swagger-Metadaten](http://swagger.io/specification/) hinzufügen, die Ihre API-Vorgänge und -Parameter beschreiben. Mit dieser Swagger-Datei funktioniert Ihre API besser und lässt sich leichter in Logik-Apps integrieren.

Sie können Ihre APIs als [Web-Apps](../app-service-web/app-service-web-overview.md) bereitstellen, sollten jedoch in Erwägung ziehen, Ihre APIs als [API-Apps](../app-service-api/app-service-api-apps-why-best-platform.md) bereitzustellen. Dadurch wird das Erstellen, Hosten und Nutzen der APIs in der Cloud und lokal vereinfacht. Sie müssen keinen Code in Ihren APIs ändern. Stellen Sie einfach Ihren Code für eine API-App bereit. Sie können Ihre APIs in [Azure App Service](../app-service/app-service-value-prop-what-is.md) hosten, einem PaaS-Angebot (Platform-as-a-Service), das eine der besten, einfachsten und skalierbarsten Möglichkeiten zum Hosten von APIs bietet.

Sie können Azure Active Directory im Azure-Portal so einrichten, dass Sie den Code nicht aktualisieren können, um Aufrufe von Logik-Apps in den APIs authentifizieren zu können. Alternativ können Sie die Authentifizierung über den API-Code anfordern oder erzwingen.

## <a name="deploy-your-api-as-a-web-app-or-api-app"></a>Bereitstellen der API als Web-App oder API-App

Bevor Sie Ihre benutzerdefinierte API über eine Logik-App aufrufen können, müssen Sie die API im Azure App Service als Web-App oder API-App bereitstellen. Zudem müssen Sie die Eigenschaften der API-Definition festlegen und die [Ressourcenfreigabe zwischen verschiedenen Ursprüngen (Cross-Origin Resource Sharing, CORS)](../app-service-api/app-service-api-cors-consume-javascript.md#corsconfig) für Ihre Web-App oder API-App einschalten, damit das Swagger-Dokument lesbar ist.

1. Wählen Sie im Azure-Portal Ihre Web-App oder API-App aus.

2. Wählen Sie auf dem geöffneten Blatt unter **API** die Option **API-Definition** aus. Legen Sie den **Speicherort der API-Definition** auf die URL Ihrer Datei swagger.json fest.

   In der Regel wird die URL im folgenden Format dargestellt:`https://{name}.azurewebsites.net/swagger/docs/v1)`

   ![Verknüpfung mit der Swagger-Datei für Ihre benutzerdefinierte API](media/logic-apps-custom-hosted-api/custom-api-swagger-url.png)

3. Wählen Sie unter **API** den Eintrag **CORS** aus. Legen Sie die CORS-Richtlinie für **Zulässige Ursprünge** auf **„*“** (alle zulassen) fest.

   Diese Einstellung lässt Anforderungen vom Logik-App-Designer zu.

   ![Zulassen von Anforderungen des Logik-App-Designers an Ihre benutzerdefinierte API](media/logic-apps-custom-hosted-api/custom-api-cors.png)

Weitere Informationen und Beispiele finden Sie in diesen Artikeln:

* [Hinzufügen von Swagger-Metadaten für ASP.NET-Web-APIs](../app-service-api/app-service-api-dotnet-get-started.md#use-swagger-api-metadata-and-ui)
* [Bereitstellen von ASP.NET-Web-APIs für Azure App Service](../app-service-api/app-service-api-dotnet-get-started.md)

## <a name="call-your-custom-api-from-logic-app-workflows"></a>Aufrufen der benutzerdefinierten API über Workflows der Logik-App

Nachdem Sie die Eigenschaften der API-Definition und CORS eingerichtet haben, sollten die Auslöser und Aktionen Ihrer benutzerdefinierten API für Sie verfügbar werden, um in den Workflow Ihrer Logik-App aufgenommen zu werden. 

*  Durchsuchen Sie Ihre Abonnementwebsites im Logik-App-Designer, um die Websites mit Swagger-URLs anzuzeigen.

*  Verwenden Sie die [Aktion „HTTP + Swagger“ ](../connectors/connectors-native-http-swagger.md), um verfügbare Aktionen und Eingaben durch Verweis auf ein Swagger-Dokument anzuzeigen.

*  Sie können über die [HTTP-Aktion](../connectors/connectors-native-http.md) jederzeit eine Anforderung erstellen, um alle APIs aufzurufen (einschließlich der APIs, die über kein Swagger-Dokument verfügen bzw. keines verfügbar machen).

## <a name="authenticate-calls-to-your-custom-api"></a>Authentifizieren von Aufrufen der benutzerdefinierte API

Sie können Aufrufe Ihrer benutzerdefinierten API auf folgende Arten sichern:

* [Keine Änderungen am Code](#no-code): Schützen Sie Ihre API mit [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) über das Azure-Portal, damit Sie Ihren Code nicht aktualisieren bzw. Ihre API nicht bereitstellen müssen.

  > [!NOTE]
  > Bei der Azure AD-Authentifizierung, die Sie im Azure-Portal aktivieren, wird standardmäßig keine fein abgestufte Autorisierung durchgeführt. Beispielsweise sperrt diese Authentifizierung Ihre API nur für einen bestimmten Mandanten und nicht für einen bestimmten Benutzer oder eine App. 

* [Aktualisieren Ihres API-Codes](#update-code): Schützen Sie Ihre API, indem Sie die [Zertifikatsauthentifizierung](#certificate), [Standardauthentifizierung](#basic), oder [Azure AD-Authentifizierung](#azure-ad-code) über Code erzwingen.

<a name="no-code"></a>

### <a name="authenticate-calls-to-your-api-without-changing-code"></a>Authentifizieren von Aufrufen Ihrer API ohne Codeänderungen

Im Folgenden werden die allgemeinen Schritte für diese Methode aufgeführt:

1. Erstellen Sie zwei [Azure Active Directory-Anwendungsidentitäten (Azure AD)](../app-service-api/app-service-api-dotnet-service-principal-auth.md): eine für Ihre Logik-App und eine für Ihre Web-App (oder API-App).

2. Verwenden Sie für die Authentifizierung von Aufrufen Ihrer API die Anmeldeinformationen (Client-ID und Geheimnis) für den [Dienstprinzipal](../app-service-api/app-service-api-dotnet-service-principal-auth.md), welcher der Azure AD-Anwendungsidentität für Ihre Logik-App zugeordnet ist.

3. Nehmen Sie die Anwendungs-IDs in die Definition der Logik-App auf.

#### <a name="part-1-create-an-azure-ad-application-identity-for-your-logic-app"></a>Teil 1: Erstellen einer Azure AD-Anwendungsidentität für die Logik-App

Die Logik-App verwendet diese Azure AD-Anwendungsidentität zum Authentifizieren bei Azure AD. Sie müssen diese Identität für Ihr Verzeichnis nur einmal einrichten. Sie können z.B. die gleiche Identität für alle Logik-Apps verwenden, Sie können aber auch pro Logik-App eindeutige Identitäten erstellen. Sie können diese Identitäten im Azure-Portal, im [klassischen Azure-Portal](#app-identity-logic-classic), oder über [PowerShell](#powershell) einrichten.

**Erstellen der Anwendungsidentität für die Logik-App im Azure-Portal**

1. Wählen Sie im [Azure-Portal](https://portal.azure.com "https://portal.azure.com") den Eintrag **Azure Active Directory** aus. 

2. Vergewissern Sie sich, dass Sie sich in demselben Verzeichnis befinden, in dem auch die Web-App oder die API-App enthalten ist.

   > [!TIP]
   > Klicken Sie auf Ihr Profil, und wählen Sie ein anderes Verzeichnis aus, um zwischen Verzeichnissen zu wechseln. Wählen Sie alternativ **Übersicht** > **Verzeichnis wechseln** aus.

3. Wählen Sie im Verzeichnismenü unter **Verwalten** den Eintrag **App-Registrierungen** > **Neue Anwendungsregistrierung** aus.

   > [!TIP]
   > In der Liste mit den App-Registrierungen werden standardmäßig alle Registrierungen in Ihrem Verzeichnis angezeigt. Aktivieren Sie **Meine Apps** neben dem Suchfeld, um nur Ihre App-Registrierungen anzuzeigen. 

   ![Erstellen einer neuen App-Registrierung](./media/logic-apps-custom-hosted-api/new-app-registration-azure-portal.png)

4. Benennen Sie die Anwendungsidentität, lassen Sie **Anwendungstyp** auf **Web-App/API** festgelegt, stellen Sie für **Anmelde-URL** eine eindeutige, als Domäne formatierte Zeichenfolge bereit, und wählen Sie **Erstellen** aus.

   ![Bereitstellen eines Namens und einer Anmelde-URL für die Anwendungsidentität](./media/logic-apps-custom-hosted-api/logic-app-identity-azure-portal.png)

   Die Anwendungsidentität, die Sie für Ihre Logik-App erstellt haben, wird jetzt in der Liste der App-Registrierungen angezeigt.

   ![Anwendungsidentität für die Logik-App](./media/logic-apps-custom-hosted-api/logic-app-identity-created.png)

5. Wählen Sie aus der Liste der App-Registrierungen Ihre neue Anwendungsidentität aus. Kopieren und speichern Sie die **Anwendungs-ID**, die in Teil 3 als „Client-ID“ für Ihre Logik-App verwendet werden soll.

   ![Kopieren und Speichern der Anwendungs-ID für die Logik-App](./media/logic-apps-custom-hosted-api/logic-app-application-id.png)

6. Wählen Sie **Einstellungen** oder **Alle Einstellungen** aus, wenn die Einstellungen für Ihre Anwendungsidentität nicht sichtbar sind.

7. Wählen Sie unter **API-Zugriff** den Eintrag **Schlüssel** aus. Geben Sie unter **Beschreibung** einen Namen für den Schlüssel an. Wählen Sie unter **Läuft ab** eine Dauer für den Schlüssel aus.

   Der von Ihnen erstellte Schlüssel fungiert als „Geheimnis“ bzw. Kennwort der Anwendungsidentität für die Logik-App.

   ![Erstellen des Schlüssels für die Identität der Logik-App](./media/logic-apps-custom-hosted-api/create-logic-app-identity-key-secret-password.png)

8. Wählen Sie auf der Symbolleiste **Speichern** aus. Unter **Wert** wird jetzt Ihr Schlüssel angezeigt. 
**Achten Sie darauf, dass Sie Ihren Schlüssel für eine spätere Verwendung kopieren und speichern**, da der Schlüssel beim Verlassen des Schlüsselblatts ausgeblendet ist.

   Wenn Sie Ihre Logik-App in Teil 3 konfigurieren, geben Sie diesen Schlüssel als „Geheimnis“ oder Kennwort an.

   ![Kopieren und speichern Sie den Schlüssel für eine spätere Verwendung](./media/logic-apps-custom-hosted-api/logic-app-copy-key-secret-password.png)

<a name="app-identity-logic-classic"></a>

**Erstellen der Anwendungsidentität für Ihre Logik-App im klassischen Azure-Portal**

1. Wählen Sie im klassischen Azure-Portal [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) aus.

2. Wählen Sie dasselbe Verzeichnis aus, das Sie für Ihre Web-App oder API-App verwenden.

3. Wählen Sie auf der Registerkarte **Anwendungen** unten auf der Seite den Eintrag **Hinzufügen** aus.

4. Geben Sie Ihrer Anwendungsidentität einen Namen, und wählen Sie **Weiter** (Pfeil nach rechts) aus.

5. Geben Sie unter **Anwendungseigenschaften** eine eindeutige Zeichenfolge an, die als Domäne für **Anmelde-URL** und **App ID-URI** formatiert ist, und wählen Sie **Abschließen** (Häkchen) aus.

6. Kopieren und speichern Sie auf der Registerkarte **Konfigurieren** die **Client-ID** für Ihre Logik-App für eine Verwendung in Teil 3.

7. Öffnen Sie unter **Schlüssel** die Liste **Dauer auswählen**. Wählen Sie eine Dauer für den Schlüssel aus.

   Der von Ihnen erstellte Schlüssel fungiert als „Geheimnis“ bzw. Kennwort der Anwendungsidentität für die Logik-App.

8. Wählen Sie unten auf der Seite **Speichern** aus. Möglicherweise müssen Sie einige Sekunden warten.

9. Stellen Sie sicher, dass Sie unter **Schlüssel** den Schlüssel kopieren und speichern, der gerade angezeigt wird. 

   Wenn Sie Ihre Logik-App in Teil 3 konfigurieren, geben Sie diesen Schlüssel als „Geheimnis“ oder Kennwort an.

Weitere Informationen finden Sie unter [Konfigurieren Ihrer App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

<a name="powershell"></a>

**Erstellen der Anwendungsidentität für Ihre Logik-App in PowerShell**

Sie können diese Aufgabe über den Azure Ressource Manager mit PowerShell ausführen. Führen Sie in PowerShell folgende Befehle aus:

1. `Switch-AzureMode AzureResourceManager`

2. `Add-AzureAccount`

3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://mydomain.tld" -IdentifierUris "http://mydomain.tld" -Password "identity-password"`

4. Achten Sie darauf, dass Sie die **Mandanten-ID** (GUID für Ihren Azure AD-Mandanten), die **Anwendungs-ID** und das von Ihnen verwendete Kennwort kopieren.

Weitere Informationen finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md).

#### <a name="part-2-create-an-azure-ad-application-identity-for-your-web-app-or-api-app"></a>Teil 2: Erstellen einer Azure AD-Anwendungsidentität für Ihre Web-App oder API-App

Wenn Ihre Web-App oder API-App bereits bereitgestellt wurde, können Sie die Authentifizierung aktivieren und die Anwendungsidentität im Azure-Portal erstellen. Andernfalls können Sie die [Authentifizierung bei der Bereitstellung mit einer Azure Resource Manager-Vorlage aktivieren](#authen-deploy). 

**Erstellen der Anwendungsidentität und Aktivieren der Authentifizierung im Azure-Portal für bereitgestellte Apps**

1. Suchen Sie im [Azure-Portal](https://portal.azure.com "https://portal.azure.com") Ihre Web-App oder API-App und wählen diese aus. 

2. Wählen Sie unter **Einstellungen** die Option **Authentifizierung/Autorisierung** aus. Unter **App Service Authentication** können Sie die Authentifizierung **aktivieren**. Klicken Sie unter **Authentifizierungsanbieter** auf **Azure Active Directory**.

   ![Aktivieren der Authentifizierung](./media/logic-apps-custom-hosted-api/custom-web-api-app-authentication.png)

3. Nun erstellen Sie eine Anwendungsidentität für Ihre Web-App oder API-App, wie im Folgenden dargestellt. Legen Sie auf dem Blatt **Azure Active Directory-Einstellungen** den **Verwaltungsmodus** auf **Express** fest. Wählen Sie **Neue AD-App erstellen** aus. Geben Sie Ihrer Anwendungsidentität einen Namen, und wählen Sie **OK** aus. 

   ![Erstellen der Anwendungsidentität für Ihre Web-App oder API-App](./media/logic-apps-custom-hosted-api/custom-api-application-identity.png)

4. Wählen Sie auf dem Blatt **Authentifizierung/Autorisierung** die Option **Speichern** aus.

Jetzt müssen Sie die Client-ID und die Mandanten-ID für die Anwendungsidentität suchen, die Ihrer Web-App oder API-App zugeordnet ist. Sie verwenden diese IDs in Teil 3. Fahren Sie mit diesen Schritten für das Azure-Portal oder das [klassische Azure-Portal](#find-id-classic) fort.

**Suchen der Client-ID und der Mandanten-ID der Anwendungsidentität für Ihre Web-App oder API-App im Azure-Portal**

1. Klicken Sie unter **Authentifizierungsanbieter** auf **Azure Active Directory**. 

   ![Wählen Sie „Azure Active Directory“ aus](./media/logic-apps-custom-hosted-api/custom-api-app-identity-client-id-tenant-id.png)

2. Legen Sie auf dem Blatt **Azure Active Directory-Einstellungen** den **Verwaltungsmodus** auf **Erweitert** fest.

3. Kopieren Sie die **Client-ID**, und speichern Sie diese GUID für die Verwendung in Teil 3.

   > [!TIP] 
   > Wenn **Client-ID** und **Aussteller-URL** nicht angezeigt werden, versuchen Sie, das Azure-Portal zu aktualisieren, und wiederholen Sie Schritt 1.

4. Kopieren und speichern Sie unter **Aussteller-URL** nur die GUID für Teil 3. Sie können diese GUID bei Bedarf auch in der Bereitstellungsvorlage Ihrer Web-App oder API-App verwenden.

   Diese GUID ist Ihre spezifische Mandanten-GUID („Mandanten-ID“) und sollte in dieser URL angezeigt werden:`https://sts.windows.net/{GUID}`

5. Schließen Sie das Blatt **Azure Active Directory-Einstellungen**, ohne Ihre Änderungen zu speichern.

<a name="find-id-classic"></a>

**Suchen der Client-ID und der Mandanten-ID der Anwendungsidentität für Ihre Web-App oder API-App im klassischen Azure-Portal**

1. Wählen Sie im klassischen Azure-Portal [**Active Directory**](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) aus.

2.  Wählen Sie das Verzeichnis aus, das Sie für Ihre Web-App oder API-App verwenden.

3. Suchen Sie im Feld **Suchen** nach der Anwendungsidentität für Ihre Web-App oder API-App, und wählen Sie diese aus.

4. Kopieren Sie auf der Registerkarte **Konfigurieren** die **Client-ID**, und speichern Sie diese GUID für die Verwendung in Teil 3.

5. Wählen Sie unten auf der Registerkarte **Konfigurieren** die Option **Endpunkte anzeigen** aus, nachdem Sie die Client-ID abgerufen haben.

6. Kopieren Sie die URL für **Verbundmetadatendokument**, und navigieren Sie zu dieser URL.

7. Suchen Sie im geöffneten Metadatendokument nach dem Stammelement **EntityDescriptor-ID**, das über das Attribut **EntityID** in dieser Form verfügt:`https://sts.windows.net/{GUID}` 

      Die GUID in diesem Attribut ist Ihre spezifische Mandanten-GUID (Mandanten-ID).

8. Kopieren Sie die Mandanten-ID, und speichern Sie diese ID für die Verwendung in Teil 3 sowie bei Bedarf für die Verwendung in der Bereitstellungsvorlage Ihrer Web-App oder API-App.

Weitere Informationen finden Sie in den folgenden Themen:

* [Benutzerauthentifizierung für API-Apps in Azure App Service](../app-service-api/app-service-api-dotnet-user-principal-auth.md)
* [Authentifizierung und Autorisierung in Azure App Service](../app-service/app-service-authentication-overview.md)

<a name="authen-deploy"></a>

**Aktivieren der Authentifizierung bei der Bereitstellung mit einer Azure Ressource Manager-Vorlage**

Sie müssen weiterhin eine Azure AD-Anwendungsidentität für Ihre Web-App oder API-App erstellen, die sich von der Anwendungsidentität Ihrer Logik-App unterscheidet. Führen Sie zur Erstellung der Anwendungsidentität die vorherigen Schritte in Teil 2 für das Azure-Portal aus. Sie können auch die Schritte in Teil 1 ausführen. Dann sollten Sie jedoch sicherstellen, dass Ihre Web-App oder API-App tatsächlich für `https://{URL}` **Anmelde-URL** und **App-ID-URI** verwendet wird. In diesen Schritten müssen Sie die Client-ID und die Mandanten-ID für die Verwendung in der Bereitstellungsvorlage Ihrer App und auch für Teil 3 speichern.

> [!NOTE]
> Wenn Sie die Azure AD-Anwendungsidentität für Ihre Web-App oder API-App erstellen, müssen Sie anstelle von PowerShell das Azure-Portal oder das klassische Azure-Portal verwenden. Mit dem PowerShell-Cmdlet werden nicht die erforderlichen Berechtigungen für die Anmeldung der Benutzer bei einer Website eingerichtet.

Sobald Sie über die Client-ID und die Mandanten-ID verfügen, nehmen Sie diese IDs als untergeordnete Ressourcen Ihrer Web-App oder API-App in Ihre Bereitstellungsvorlage auf:

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
                 "clientId": "{client-ID}",
                 "issuer": "https://sts.windows.net/{tenant-ID}/",
               }
           }
       }
   ]
   ```

Für die automatische Bereitstellung einer leeren Web-App und einer Logik-App zusammen mit einer Azure Active Directory-Authentifizierung müssen Sie die [vollständige Vorlage hier anzeigen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-custom-api/azuredeploy.json) oder hier auf **Bereitstellung in Azure** klicken:

[![Bereitstellen in Azure](media/logic-apps-custom-hosted-api/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json)

#### <a name="part-3-populate-the-authorization-section-in-your-logic-app"></a>Teil 3: Ausfüllen des Abschnitts für die Autorisierung in Ihrer Logik-App

In der obigen Vorlage ist der Abschnitt für die Autorisierung bereits eingerichtet, aber wenn Sie die Logik-App direkt erstellen, müssen Sie den gesamten Abschnitt für die Autorisierung aufnehmen.

Öffnen Sie die Definition Ihrer Logik-App in der Codeansicht, wechseln Sie zum Abschnitt **HTTP-Aktion**, suchen Sie den Abschnitt **Autorisierung**, und fügen Sie folgende Zeile ein:

`{"tenant": "{tenant-ID}", "audience": "{client-ID-from-Part-2-web-app-or-API app}", "clientId": "{client-ID-from-Part-1-logic-app}", "secret": "{key-from-Part-1-logic-app}", "type": "ActiveDirectoryOAuth" }`

| Element | Beschreibung |
| ------- | ----------- |
| Mandant |Die GUID für den Azure AD-Mandanten |
| audience |Erforderlich. Die GUID für die Zielressource, auf die Sie zugreifen möchten – die Client-ID der Anwendungsidentität für Ihre Web-App oder API-App |
| clientId |Die GUID für den Client, der darauf zugreifen möchte – die Client-ID der Anwendungsidentität für Ihre Logik-App |
| secret |Erforderlich. Der Schlüssel oder das Kennwort der Anwendungsidentität für den Client, der das Zugriffstoken anfordert |
| Typ |Der Authentifizierungstyp. Für die ActiveDirectoryOAuth-Authentifizierung lautet der Wert `ActiveDirectoryOAuth`. |

Beispiel:

```
{
   ...
   "actions": {
      "some-action": {
         "conditions": [],
         "inputs": {
            "method": "post",
            "uri": "https://your-api-azurewebsites.net/api/your-method",
            "authentication": {
               "tenant": "tenant-ID",
               "audience": "client-ID-from-azure-ad-app-for-web-app-or-api-app",
               "clientId": "client-ID-from-azure-ad-app-for-logic-app",
               "secret": "key-from-azure-ad-app-for-logic-app",
               "type": "ActiveDirectoryOAuth"
            }
         },
      }
   }
}
```

<a name="update-code"></a>

### <a name="secure-api-calls-through-code"></a>Sichere API-Aufrufe über Code

<a name="certificate"></a>

#### <a name="certificate-authentication"></a>Zertifikatauthentifizierung

Sie können Clientzertifikate verwenden, um die eingehenden Anforderungen von Ihrer Logik-App für Ihre Web-App oder API-App zu überprüfen. Weitere Informationen zum Einrichten des Codes finden Sie unter [Konfigurieren der gegenseitigen TLS-Authentifizierung](../app-service-web/app-service-web-configure-tls-mutual-auth.md).

Schließen Sie im Abschnitt **Autorisierung** folgende Zeile ein: 

`{"type": "clientcertificate", "password": "password", "pfx": "long-pfx-key"}`

| Element | Beschreibung |
| ------- | ----------- |
| type |Erforderlich. Der Authentifizierungstyp. Für SSL-Clientzertifikate muss der Wert `ClientCertificate` lauten. |
| password |Erforderlich. Das Kennwort für den Zugriff auf das Clientzertifikat (PFX-Datei) |
| pfx |Erforderlich. Base64-codierter Inhalt des Clientzertifikats (PFX-Datei) |

<a name="basic"></a>

#### <a name="basic-authentication"></a>Standardauthentifizierung

Sie können die Standardauthentifizierung, z.B. einen Benutzernamen und ein Kennwort, verwenden, um eingehende Anforderungen von der Logik-App für Ihre Web-App oder API-App zu überprüfen. Die Standardauthentifizierung ist ein gängiges Muster, und Sie können diese Authentifizierung in jeder Sprache verwenden, mit der Ihre Web-App oder API-App erstellt wird.

Schließen Sie im Abschnitt **Autorisierung** folgende Zeile ein:

`{"type": "basic", "username": "username", "password": "password"}`.

| Element | Beschreibung |
| --- | --- |
| type |Erforderlich. Der Authentifizierungstyp. Für die Standardauthentifizierung muss der Wert `Basic` lauten. |
| username |Erforderlich. Der Benutzername für die Authentifizierung |
| password |Erforderlich. Das Kennwort für die Authentifizierung |

<a name="azure-ad-code"></a>

#### <a name="azure-active-directory-authentication-through-code"></a>Azure Active Directory-Authentifizierung über Code

Bei der Azure AD-Authentifizierung, die Sie im Azure-Portal aktivieren, wird standardmäßig keine fein abgestufte Autorisierung durchgeführt. Beispielsweise sperrt diese Authentifizierung Ihre API nur für einen bestimmten Mandanten und nicht für einen bestimmten Benutzer oder eine App. 

Extrahieren Sie den Header, der das JSON Web Token (JWT) enthält, um den API-Zugriff auf die Logik-App über Code zu beschränken. Überprüfen Sie die Identität des Aufrufers, und lehnen Sie Anforderungen ab, die nicht übereinstimmen.

Weitere Informationen dazu, wie Sie diese Authentifizierung vollständig in Ihrem eigenen Code implementieren können, ohne die Funktion im Azure-Portal zu verwenden, finden Sie unter [Authentifizieren mit lokaler Active Directory-Instanz in Ihrer Azure-App](../app-service-web/web-sites-authentication-authorization.md).

Sie müssen die obigen Schritte ausführen, um eine Anwendungsidentität für Ihre Logik-App zu erstellen und diese Identität zum Aufrufen Ihrer API zu verwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Überprüfen der Logik-App-Leistung mit Diagnoseprotokollen und -Warnungen](logic-apps-monitor-your-logic-apps.md)
