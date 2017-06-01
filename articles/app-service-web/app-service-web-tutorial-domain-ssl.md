---
title: "Hinzufügen einer benutzerdefinierten Domäne und eines benutzerdefinierten SSL-Zertifikats zu einer Azure-Web-App | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihre Azure-Web-App auf die Produktion vorbereiten, indem Sie Ihr unternehmenseigenes Branding hinzufügen. Ordnen Sie den benutzerdefinierten Domänennamen zu Ihrer Web-App zu, und sichern Sie die App mit einem benutzerdefinierten SSL-Zertifikat."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: dc446e0e-0958-48ea-8d99-441d2b947a7c
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 03/29/2017
ms.author: cephalin
ms.translationtype: Human Translation
ms.sourcegitcommit: a30a90682948b657fb31dd14101172282988cbf0
ms.openlocfilehash: 535a7dbe24a1badc8539b61d34c09bdeda41ad40
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---
# <a name="add-custom-domain-and-ssl-to-an-azure-web-app"></a>Hinzufügen einer benutzerdefinierten Domäne und eines benutzerdefinierten SSL-Zertifikats zu einer Azure-Web-App

Dieses Tutorial veranschaulicht, wie Sie Ihrer Azure Web-App schnell einen benutzerdefinierten Domänennamen zuordnen und die App dann mit einem benutzerdefinierten SSL-Zertifikat sichern. 

## <a name="before-you-begin"></a>Voraussetzungen

Installieren Sie vor dem Ausführen dieses Beispiels die [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) lokal.

Sie benötigen auch Administratorzugriff auf die DNS-Konfigurationsseite für Ihren jeweiligen Domänenanbieter. Um beispielsweise `www.contoso.com` hinzuzufügen, müssen Sie DNS-Einträge für `contoso.com` konfigurieren können.

Und schließlich benötigen Sie eine gültige PFX-Datei _und_ das zugehörige Kennwort für das SSL-Zertifikat, das Sie hochladen und binden möchten. Dieses SSL-Zertifikat sollte für die Sicherung des gewünschten Domänennamens konfiguriert werden. Im obigen Beispiel sollte Ihre SSL-Zertifikat `www.contoso.com` sichern. 

## <a name="step-1---create-an-azure-web-app"></a>Schritt 1: Erstellen einer Azure-Web-App

### <a name="log-in-to-azure"></a>Anmelden an Azure

Als Nächstes verwenden wir die Azure CLI 2.0 in einem Terminalfenster, um die Ressourcen zu erstellen, die zum Hosten unserer Node.js-App in Azure benötigt werden.  Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm. 

```azurecli 
az login 
``` 
   
### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe   
Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen wie Web-Apps, Datenbanken und Speicherkonten bereitgestellt und verwaltet werden. 

```azurecli
az group create --name myResourceGroup --location westeurope 
```

Welche Werte Sie für `---location` verwenden können, erfahren Sie mithilfe des Azure CLI-Befehls `az appservice list-locations`.

## <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?

Erstellen Sie mit dem Befehl [az appservice plan create](/cli/azure/appservice/plan#create) einen App Service-Plan. 

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Das folgende Beispiel erstellt einen App Service-Plan namens `myAppServicePlan` mit dem Tarif **Basic**.

az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1

Nach Erstellung des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an. 

```json 
{ 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "kind": "app", 
    "location": "West Europe", 
    "sku": { 
    "capacity": 1, 
    "family": "B", 
    "name": "B1", 
    "tier": "Basic" 
    }, 
    "status": "Ready", 
    "type": "Microsoft.Web/serverfarms" 
} 
``` 

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Nach dem Erstellen eines App Service-Plans können Sie nun innerhalb des App Service-Plans `myAppServicePlan` eine Web-App erstellen. Die Web-App bietet Ihnen eine Hostingumgebung zum Bereitstellen Ihres Codes sowie eine URL, unter der Sie sich die bereitgestellte Anwendung ansehen können. Erstellen Sie die Web-App mit dem Befehl [az appservice web create](/cli/azure/appservice/web#create). 

Ersetzen Sie im unten stehenden Befehl den Platzhalter `<app_name>` durch Ihren eigenen eindeutigen App-Namen. Da dieser eindeutige Name als Teil des Standarddomänennamens für die Web-App verwendet wird, muss er für alle Apps in Azure eindeutig sein. Später können Sie der Web-App einen beliebigen benutzerdefinierten DNS-Eintrag zuordnen, bevor Sie sie für Ihre Benutzer verfügbar machen. 

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan 
```

Nach Erstellung der Web-App zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an. 

```json 
{ 
    "clientAffinityEnabled": true, 
    "defaultHostName": "<app_name>.azurewebsites.net", 
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/sites/<app_name>", 
    "isDefaultContainer": null, 
    "kind": "app", 
    "location": "West Europe", 
    "name": "<app_name>", 
    "repositorySiteName": "<app_name>", 
    "reserved": true, 
    "resourceGroup": "myResourceGroup", 
    "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
    "state": "Running", 
    "type": "Microsoft.Web/sites", 
} 
```

In der JSON-Ausgabe zeigt `defaultHostName` den Standarddomänennamen Ihrer Web-App an. Navigieren Sie in Ihrem Browser zu dieser Adresse.

```
http://<app_name>.azurewebsites.net 
``` 
 
![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-created.png)  

## <a name="step-2---configure-dns-mapping"></a>Schritt 2: Konfigurieren der DNS-Zuordnung

In diesem Schritt fügen Sie eine Zuordnung zwischen einer benutzerdefinierten Domäne und den Standarddomänennamen `<app_name>.azurewebsites.net` Ihrer Web-App hinzu. Diesen Schritt führen Sie in der Regel auf der Website Ihres Domänenanbieters aus. Da sich die Websites der einzelnen Domänenregistrierungsstellen leicht unterscheiden, ist es ratsam, jeweils in der Dokumentation des Anbieters nachzulesen. Hier sind aber bereits einige allgemeine Richtlinien aufgeführt. 

### <a name="navigate-to-to-dns-management-page"></a>Navigieren zur Seite für die DNS-Verwaltung

Melden Sie sich zunächst bei der Website Ihrer Domänenregistrierungsstelle an.  

Suchen Sie dann die Seite für die Verwaltung von DNS-Einträgen. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. Häufig finden Sie in Ihren Kontoinformationen einen Link auf diese Seite. Suchen Sie anschließend nach einem Link wie etwa **Eigene Domänen**.

Sobald Sie die Seite gefunden haben, suchen Sie nach einem Link, mit dem Sie DNS-Einträge hinzufügen oder bearbeiten können. Dieser Link kann als **Zone file** (Zonendatei), **DNS Records** (DNS-Einträge) oder **Advanced Configuration** (Erweiterte Konfiguration) bezeichnet werden.

### <a name="create-a-cname-record"></a>Erstellen eines CNAME-Eintrags

Fügen Sie einen CNAME-Eintrag hinzu, der den gewünschten Unterdomänennamen zum Standarddomänennamen Ihrer Web-App zuordnet (`<app_name>.azurewebsites.net`, wobei `<app_name>` der eindeutige Name Ihrer App ist).

Für das `www.contoso.com`-Beispiel erstellen Sie einen CNAME-Eintrag, der den `www`-Hostnamen zu `<app_name>.azurewebsites.net` zuordnet.

## <a name="step-3---configure-the-custom-domain-on-your-web-app"></a>Schritt 3: Konfigurieren der benutzerdefinierten Domäne in Ihrer Web-App

Wenn Sie die Konfiguration der Hostnamenzuordnung auf der Website Ihres Domänenanbieters abgeschlossen haben, können Sie die benutzerdefinierte Domäne in Ihrer Web-App konfigurieren. Verwenden Sie den Befehl [az appservice web config hostname add](/cli/azure/appservice/web/config/hostname#add), um diese Konfiguration hinzuzufügen. 

Ersetzen Sie im unten stehenden Befehl `<app_name>` durch Ihren eindeutigen App-Namen und <your_custom_domain> durch den vollqualifizierten Namen Ihrer benutzerdefinierten Domäne (z.B. `www.contoso.com`). 

```azurecli
az appservice web config hostname add --webapp <app_name> --resource-group myResourceGroup --name <your_custom_domain>
```

Die benutzerdefinierte Domäne ist jetzt vollständig Ihrer Web-App zugeordnet. Navigieren Sie in Ihrem Browser zum Namen der benutzerdefinierten Domäne. Beispiel:

```
http://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-custom-domain.png)  

## <a name="step-4---bind-a-custom-ssl-certificate-to-your-web-app"></a>Schritt 4: Binden eines benutzerdefinierten SSL-Zertifikats an Ihre Web-App

Sie verfügen jetzt über eine Azure-Web-App mit dem gewünschten Domänennamen in der Adressleiste Ihres Browsers. Wenn Sie jedoch jetzt zu `https://<your_custom_domain>` navigieren, erhalten Sie einen Zertifikatfehler. 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-cert-error.png)  

Dieser Fehler tritt auf, weil Ihre Web-App noch nicht über eine SSL-Zertifikatbindung entsprechend dem Namen Ihrer benutzerdefinierten Domäne verfügt. Sie erhalten jedoch keinen Fehler, wenn Sie zu `https://<app_name>.azurewebsites.net` navigieren. Dies liegt daran, dass Ihre App – wie alle Azure App Service-Apps – standardmäßig mit dem SSL-Zertifikat für die Platzhalterdomäne `*.azurewebsites.net` gesichert ist. 

Um über den benutzerdefinierten Domänennamen auf Ihre Web-App zuzugreifen, müssen Sie das SSL-Zertifikat für Ihre benutzerdefinierte Domäne an die Web-App binden. Dies erledigen Sie im vorliegenden Schritt. 

### <a name="upload-the-ssl-certificate"></a>Hochladen des SSL-Zertifikats

Laden Sie das SSL-Zertifikat für Ihre benutzerdefinierte Domäne mithilfe des Befehls [az appservice web config ssl upload](/cli/azure/appservice/web/config/ssl#upload) in Ihre Web-App hoch.

Ersetzen Sie im unten stehenden Befehl `<app_name>` durch den eindeutigen Namen Ihrer App, `<path_to_ptx_file>` durch den Pfad zu Ihrer PFX-Datei und `<password>` durch das Kennwort für Ihr Zertifikat. 

```azurecli
az appservice web config ssl upload --name <app_name> --resource-group myResourceGroup --certificate-file <path_to_pfx_file> --certificate-password <password> 
```

Nach dem Hochladen des Zertifikats zeigt die Azure-Befehlszeilenschnittstelle ähnliche Informationen wie im folgenden Beispiel an:

```json
{
  "cerBlob": null,
  "expirationDate": "2018-03-29T14:12:57+00:00",
  "friendlyName": "",
  "hostNames": [
    "www.contoso.com"
  ],
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/cert
ificates/9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "issueDate": "2017-03-29T14:12:57+00:00",
  "issuer": "www.contoso.com",
  "keyVaultId": null,
  "keyVaultSecretName": null,
  "keyVaultSecretStatus": "Initialized",
  "kind": null,
  "location": "West Europe",
  "name": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00__West Europe_myResourceGroup",
  "password": null,
 "pfxBlob": null,
  "publicKeyHash": null,
  "resourceGroup": "myResourceGroup",
  "selfLink": null,
  "serverFarmId": null,
  "siteName": null,
  "subjectName": "www.contoso.com",
  "tags": null,
  "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00",
  "type": "Microsoft.Web/certificates",
  "valid": null
}
```

In der JSON-Ausgabe zeigt `thumbprint` Ihren hochgeladenen Zertifikatfingerabdruck an. Kopieren Sie diesen für den nächsten Schritt.

### <a name="bind-the-uploaded-ssl-certificate-to-the-web-app"></a>Binden des hochgeladenen SSL-Zertifikats an die Web-App

Ihre Web-App verfügt jetzt über den von Ihnen gewünschten benutzerdefinierten Domänennamen sowie über ein SSL-Zertifikat, das die benutzerdefinierte Domäne sichert. Jetzt fehlt nur noch ein Schritt: das Binden des hochgeladenen Zertifikats an die Web-App. Hierzu verwenden Sie den Befehl [az appservice web config ssl bind](/cli/azure/appservice/web/config/ssl#bind).

Ersetzen Sie im unten stehenden Befehl `<app_name>` durch den eindeutigen Namen Ihrer App und `<thumbprint-from-previous-output>` durch den Zertifikatfingerabdruck, den Sie aus dem vorherigen Befehl kopiert haben. 

az appservice web config ssl bind --name <App-Name> --resource-group myResourceGroup --certificate-thumbprint <Fingerabdruck-aus-vorheriger-Ausgabe> --ssl-type SNI

Nach dem Binden des Zertifikats an die Web-App zeigt die Azure-Befehlszeilenschnittstelle ähnliche Informationen wie im folgenden Beispiel an:

{ "availabilityState": "Normal", "clientAffinityEnabled": true, "clientCertEnabled": false, "cloningInfo": null, "containerSize": 0, "dailyMemoryTimeQuota": 0, "defaultHostName": "<App-Name>.azurewebsites.net", "enabled": true, "enabledHostNames": [ "www.contoso.com", "<App-Name>.azurewebsites.net", "<App-Name>.scm.azurewebsites.net" ], "gatewaySiteName": null, "hostNameSslStates": [ { "hostType": "Standard", "name": "<App-Name>.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Repository", "name": "<App-Name>.scm.azurewebsites.net", "sslState": "Disabled", "thumbprint": null, "toUpdate": null, "virtualIp": null }, { "hostType": "Standard", "name": "www.contoso.com", "sslState": "SniEnabled", "thumbprint": "9FD1D2D06E2293673E2A8D1CA484A092BD016D00", "toUpdate": null, "virtualIp": null } ], "hostNames": [ "www.contoso.com", "<App-Name>.azurewebsites.net" ], "hostNamesDisabled": false, "hostingEnvironmentProfile": null, "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/site s/<App-Name>", "isDefaultContainer": null, "kind": "WebApp", "lastModifiedTimeUtc": "2017-03-29T14:36:18.803333", "location": "West Europe", "maxNumberOfWorkers": null, "microService": "false", "name": "<App-Name>", "outboundIpAddresses": "13.94.143.57,13.94.136.57,40.68.199.146,13.94.138.55,13.94.140.1", "premiumAppDeployed": null, "repositorySiteName": "<App-Name>", "reserved": false, "resourceGroup": "myResourceGroup", "scmSiteAlsoStopped": false, "serverFarmId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsof t.Web/serverfarms/myAppServicePlan", "siteConfig": null, "slotSwapStatus": null, "state": "Running", "suspendedTill": null, "tags": null, "targetSwapSlot": null, "trafficManagerHostNames": null, "type": "Microsoft.Web/sites", "usageState": "Normal" }

Navigieren Sie in Ihrem Browser zum HTTPS-Endpunkt Ihres benutzerdefinierten Domänennamens. Beispiel:

```
https://www.contoso.com 
``` 

![app-service-web-service-created](media/app-service-web-tutorial-domain-ssl/web-app-ssl-success.png)  

## <a name="more-resources"></a>Weitere Ressourcen

[Kaufen und Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service](custom-dns-web-site-buydomains-web-app.md)
[Kaufen und Konfigurieren eines SSL-Zertifikats für Ihren Azure App Service](web-sites-purchase-ssl-web-site.md)

