---
title: "Verwaltete Dienstidentität in App Service und Azure Functions | Microsoft-Dokumentation"
description: "Konzeptreferenz und Einrichtungsleitfaden zur Unterstützung der verwalteten Dienstidentität in Azure App Service und Azure Functions"
services: app-service
author: mattchenderson
manager: cfowler
editor: 
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/13/2017
ms.author: mahender
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 7aaf611a562d373a8cc1dad33963050d246b2882
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="how-to-use-azure-managed-service-identity-public-preview-in-app-service-and-azure-functions"></a>Informationen zum Verwenden der verwalteten Azure-Dienstidentität (öffentliche Vorschau) in App Service und Azure Functions

> [!NOTE] 
> Die verwaltete Dienstidentität für App Service und Azure Functions befindet sich derzeit in der Vorschauversion.

In diesem Thema erfahren Sie, wie eine verwaltete App-Identität für App Service- und Azure Functions-Anwendungen erstellt und für den Zugriff auf andere Ressourcen verwendet wird. Durch eine verwaltete Dienstidentität von Azure Active Directory kann Ihre App mühelos auf andere mit AAD geschützte Ressourcen wie Azure Key Vault zugreifen. Da die Identität von der Azure-Plattform verwaltet wird, müssen Sie keine Geheimnisse bereitstellen oder rotieren. Weitere Informationen zur verwalteten Dienstidentität finden Sie in der [Übersicht über die verwaltete Dienstidentität](../active-directory/msi-overview.md).

## <a name="creating-an-app-with-an-identity"></a>Erstellen einer App mit einer Identität

Für die Erstellung einer App mit einer Identität muss eine zusätzliche Eigenschaft in der Anwendung festgelegt werden.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals

Um eine verwaltete Dienstidentität im Portal einzurichten, erstellen Sie wie gewohnt zuerst eine Anwendung und aktivieren dann das Feature.

1. Erstellen Sie wie gewohnt eine App im Portal. Navigieren Sie im Portal zu dieser App.

2. Wenn Sie eine Funktionen-App verwenden, navigieren Sie zu **Plattformfeatures**. Scrollen Sie bei anderen App-Typen in der linken Navigationsleiste nach unten zur Gruppe **Einstellungen**.

3. Wählen Sie **Verwaltete Dienstidentität**.

4. Legen Sie **Bei Azure Active Directory registrieren** auf **Ein** fest. Klicken Sie auf **Speichern**.

![Verwaltete Dienstidentität in App Service](media/app-service-managed-service-identity/msi-blade.png)

### <a name="using-an-azure-resource-manager-template"></a>Verwenden einer Azure Resource Manager-Vorlage

Mithilfe einer Azure Resource Manager-Vorlage kann die Bereitstellung Ihrer Azure-Ressourcen automatisiert werden. Weitere Informationen zum Bereitstellen von App Service und Azure Functions finden Sie unter [Automatisieren der Ressourcenbereitstellung in App Service](../app-service-web/app-service-deploy-complex-application-predictably.md) und [Automatisieren der Ressourcenbereitstellung in Azure Functions](../azure-functions/functions-infrastructure-as-code.md).

Ressourcen vom Typ `Microsoft.Web/sites` können mit einer Identität erstellt werden, indem die folgende Eigenschaft in der Ressourcendefinition eingeschlossen wird:
```json
"identity": {
    "type": "SystemAssigned"
}    
```

Hierdurch erhält Azure die Anweisung, die Identität für Ihre Anwendung zu erstellen und zu verwalten.

Eine Web-App kann beispielsweise wie folgt aussehen:
```json
{
    "apiVersion": "2016-08-01",
    "type": "Microsoft.Web/sites",
    "name": "[variables('appName')]",
    "location": "[resourceGroup().location]",
    "identity": {
        "type": "SystemAssigned"
    },
    "properties": {
        "name": "[variables('appName')]",
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]",
        "hostingEnvironment": "",
        "clientAffinityEnabled": false,
        "alwaysOn": true
    },
    "dependsOn": [
        "[resourceId('Microsoft.Web/serverfarms', variables('hostingPlanName'))]"
    ]
}
```

Wenn die Website erstellt wurde, weist sie folgende zusätzliche Eigenschaften auf:
```json
"identity": {
    "tenantId": "<TENANTID>",
    "principalId": "<PRINCIPALID>"
}
```

Hierbei werden `<TENANTID>` und `<PRINCIPALID>` durch GUIDs ersetzt. Die Eigenschaft „tenantId“ kennzeichnet, zu welchem AAD-Mandanten die Anwendung gehört. Die Eigenschaft „principalId“ ist ein eindeutiger Bezeichner für die neue Identität der Anwendung. In AAD weist die Anwendung denselben Namen auf, den Sie für Ihre App Service- oder Azure Functions-Instanz vergeben haben.

## <a name="obtaining-tokens-for-azure-resources"></a>Abrufen von Tokens für Azure-Ressourcen

Eine App kann mithilfe ihrer Identität Tokens für andere Ressourcen abrufen, die durch AAD wie Azure Key Vault geschützt sind. Diese Tokens stellen die Anwendung dar, die auf die Ressource zugreift, nicht einen bestimmten Benutzer der Anwendung. 

> [!IMPORTANT]
> Sie müssen die Zielressource möglicherweise für den Zugriff über die Anwendung konfigurieren. Wenn Sie beispielsweise ein Token für Key Vault anfordern, müssen Sie sicherstellen, dass Sie eine Zugriffsrichtlinie hinzugefügt haben, die die Identität Ihrer Anwendung enthält. Andernfalls werden Ihre Aufrufe von Key Vault abgelehnt, auch wenn diese das Token enthalten. Informationen zu den Ressourcen, die Tokens für die verwaltete Dienstidentität unterstützen, finden Sie unter [Azure-Dienste mit Unterstützung für die Azure AD-Authentifizierung](../active-directory/msi-overview.md#which-azure-services-support-managed-service-identity).

Zum Abrufen eines Tokens in App Service und Azure Functions ist ein einfaches REST-Protokoll verfügbar. Für .NET-Anwendungen bietet die Microsoft.Azure.Services.AppAuthentication-Bibliothek eine Abstraktion über dieses Protokoll und unterstützt eine lokale Entwicklungsumgebung.

### <a name="asal"></a>Verwendung der Microsoft.Azure.Services.AppAuthentication-Bibliothek für .NET

Bei .NET-Anwendungen und -Funktionen stellt die einfachste Methode für die Arbeit mit einer verwalteten Dienstidentität das Microsoft.Azure.Services.AppAuthentication-Paket dar. Mithilfe dieser Bibliothek können Sie zudem Ihren Code lokal auf dem Entwicklungscomputer testen. Hierzu verwenden Sie Ihr Benutzerkonto aus der [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest) oder der integrierten Active Directory-Authentifizierung. In diesem Abschnitt werden die ersten Schritte mit der Bibliothek erläutert.

1. Fügen Sie einen Verweis auf das NuGet-Paket [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) zu Ihrer Anwendung hinzu.

2.  Fügen Sie Ihrer Anwendung den folgenden Code hinzu:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
// ...
var azureServiceTokenProvider = new AzureServiceTokenProvider();
string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");
// OR
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
```

Weitere Informationen zu Microsoft.Azure.Services.AppAuthentication und den zugehörigen Vorgängen finden Sie unter [App Service und KeyVault mit MSI .NET-Beispiel](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

### <a name="using-the-rest-protocol"></a>Verwenden des REST-Protokolls

Für eine App mit einer verwalteten Dienstidentität sind zwei Umgebungsvariablen definiert:
- MSI_ENDPOINT
- MSI_SECRET

Bei der Variable **MSI_ENDPOINT** handelt es sich um eine lokale URL, über die Ihre App Tokens anfordern kann. Um ein Token für eine Ressource abzurufen, senden Sie eine HTTP-GET-Anforderung mit folgenden Parametern an diesen Endpunkt:

> [!div class="mx-tdBreakAll"]
> |Parametername|Geben Sie in|Beschreibung|
> |-----|-----|-----|
> |resource|Abfrage|Der AAD-Ressourcen-URI der Ressource, für die ein Token abgerufen werden soll.|
> |api-version|Abfrage|Die Version der zu verwendenden Token-API. Die einzige derzeit unterstützte Version lautet „2017-09-01“.|
> |secret|Header|Der Wert der Umgebungsvariable „MSI_SECRET“.|


Eine erfolgreiche 200 OK-Antwort enthält einen JSON-Text mit folgenden Eigenschaften:

> [!div class="mx-tdBreakAll"]
> |Eigenschaftenname|Beschreibung|
> |-------------|----------|
> |access_token|Das angeforderte Zugriffstoken. Der aufrufende Webdienst kann dieses Token verwenden, um die Authentifizierung für den empfangenden Webdienst durchzuführen.|
> |expires_on|Die Uhrzeit, zu der das Zugriffstoken abläuft. Das Datum wird als Anzahl der Sekunden ab 1970-01-01T0:0:0Z UTC bis zur Ablaufzeit dargestellt. Dieser Wert wird verwendet, um die Lebensdauer von zwischengespeicherten Token zu bestimmen.|
> |resource|Der App-ID-URI des empfangenden Webdiensts.|
> |token_type|Gibt den Wert des Tokentyps an. Bearertoken ist der einzige Typ, den Azure AD unterstützt. Weitere Informationen zu Bearertokens finden Sie unter [OAuth 2.0-Autorisierungsframework: Verwendung von Bearertokens (RFC 6750)](http://www.rfc-editor.org/rfc/rfc6750.txt).|

Diese Antwort ist mit der [Antwort für die Zugriffstokenanforderung zwischen zwei AAD-Diensten](../active-directory/develop/active-directory-protocols-oauth-service-to-service.md#service-to-service-access-token-response) identisch.

### <a name="rest-protocol-examples"></a>Beispiele für REST-Protokolle
Eine Beispielanforderung könnte folgendermaßen aussehen:
```
GET /MSI/token?resource=https://vault.azure.net&api-version=2017-09-01 HTTP/1.1
Host: localhost:4141
Secret: 853b9a84-5bfa-4b22-a3f3-0b9a43d9ad8a
```
Eine Beispielantwort könnte folgendermaßen aussehen:
```
HTTP/1.1 200 OK
Content-Type: application/json
 
{
    "access_token": "eyJ0eXAi…",
    "expires_on": "09/14/2017 00:00:00 PM +00:00",
    "resource": "https://vault.azure.net",
    "token_type": "Bearer"
}
```

### <a name="code-examples"></a>Codebeispiele
So erstellen Sie diese Anforderung in C#
```csharp
public static async Task<HttpResponseMessage> GetToken(string resource, string apiversion)  {
    HttpClient client = new HttpClient();
    client.DefaultRequestHeaders.Add("Secret", Environment.GetEnvironmentVariable("MSI_SECRET"));
    return await client.GetAsync(String.Format("{0}/?resource={1}&api-version={2}", Environment.GetEnvironmentVariable("MSI_ENDPOINT"), resource, apiversion));
}
```
> [!TIP]
> Für .NET-Sprachen können Sie auch [Microsoft.Azure.Services.AppAuthentication](#asal) verwenden, statt diese Anforderung selbst zu entwerfen.

Gehen Sie in Node.js folgendermaßen vor:
```javascript
const rp = require('request-promise');
const getToken = function(resource, apiver, cb) {
    var options = {
        uri: `${process.env["MSI_ENDPOINT"]}/?resource=${resource}&api-version=${apiver}`,
        headers: {
            'Secret': process.env["MSI_SECRET"]
        }
    };
    rp(options)
        .then(cb);
}
```

