---
title: Erstellen einer App mit Hyperskalierung in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe verschiedener Azure-Dienste die Leistung einer ASP.NET-App in Azure maximieren.
services: app-service\web
documentationcenter: dotnet
author: cephalin
manager: erikre
editor: 
ms.assetid: a4d49ac7-0f97-4997-84c5-cdb9c4465757
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 03/23/2017
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 26d460a699e31f6c19e3b282fa589ed07ce4a068
ms.openlocfilehash: 48ce80d2a25c6ad55e0520ea4cbdbc82db23f919
ms.lasthandoff: 04/04/2017


---
# <a name="build-a-hyper-scale-web-app-in-azure"></a>Erstellen einer Web-App mit Hyperskalierung in Azure

In diesem Tutorial wird gezeigt, wie Sie eine ASP.NET-Web-App in Azure horizontal hochskalieren, um Benutzeranforderungen zu maximieren.

Stellen Sie vor der Durchführung dieses Tutorials sicher, dass auf Ihrem Computer [die Azure CLI installiert ist](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli). Darüber hinaus benötigen Sie [Visual Studio](https://www.visualstudio.com/vs/) auf dem lokalen Computer zur Ausführung der Beispielanwendung.

## <a name="step-1---get-sample-application"></a>Schritt 1: Abrufen der Beispielanwendung
In diesem Schritt richten Sie das lokale ASP.NET-Projekt ein.

### <a name="clone-the-application-repository"></a>Klonen des Anwendungsrepositorys

Öffnen Sie ein Befehlszeilenterminal Ihrer Wahl, und wechseln Sie mit `CD` in ein Arbeitsverzeichnis. Führen Sie anschließend die folgenden Befehle aus, um die Beispielanwendung zu klonen. 

```powershell
git clone https://github.com/cephalin/HighScaleApp.git
```

### <a name="run-the-sample-application-in-visual-studio"></a>Ausführen der Beispielanwendung in Visual Studio

Öffnen Sie die Projektmappe in Visual Studio.

```powershell
cd HighScaleApp
.\HighScaleApp.sln
```

Drücken Sie `F5`, um die Anwendung auszuführen.

Diese ASP.NET-Beispielwebanwendung stammt aus der Standardvorlage: Sie behält Benutzersitzungen bei und nutzt den Ausgabecache. Sehen Sie sich `HighScaleApp\Controllers\HomeController.cs` an. Die `Index()`-Methode fügt der Sitzung ein Datenelement hinzu.

```csharp
Session.Add("visited", "true"); 
```

Die Methoden `About()` und `Contact()` speichern die Ausgabe zwischen.

```csharp
[OutputCache(Duration = 60)]
```

## <a name="step-2---deploy-to-azure"></a>Schritt 2: Bereitstellen in Azure
In diesem Schritt erstellen Sie eine Azure-Web-App und stellen Ihre ASP.NET-Beispielanwendung darin bereit.

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe   
Erstellen Sie mithilfe von [az group create](https://docs.microsoft.com/cli/azure/group#create) eine [Ressourcengruppe](../azure-resource-manager/resource-group-overview.md) in der Region „Europa, Westen“. Einer Ressourcengruppe fügen Sie alle Azure-Ressourcen hinzu, die Sie zusammen verwalten möchten, z.B. die Web-App und alle SQL-Datenbank-Back-Ends.

```azurecli
az group create --location "West Europe" --name myResourceGroup
```

Welche Werte Sie für `---location` verwenden können, erfahren Sie mithilfe des Befehls [az appservice list-locations](https://docs.microsoft.com/en-us/cli/azure/appservice#list-locations).

### <a name="create-an-app-service-plan"></a>Wie erstelle ich einen Plan?
Erstellen Sie mithilfe von [az appservice plan create](https://docs.microsoft.com/en-us/cli/azure/appservice/plan#create) einen [App Service-Plan](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) vom Typ „B1“. 

```azurecli
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku B1
```

Bei einem App Service-Plan handelt es sich um eine Skalierungseinheit mit einer beliebigen Anzahl von Apps, die zusammen über die gleiche App Services-Infrastruktur zentral oder horizontal hochskaliert werden sollen. Jedem Plan wird zudem ein [Tarif](https://azure.microsoft.com/en-us/pricing/details/app-service/) zugewiesen. Höhere Tarife enthalten bessere Hardware und mehr Funktionen, beispielsweise mehr horizontal hochskalierbare Instanzen.

In diesem Tutorial ist B1 der Mindesttarif, der das horizontale Hochskalieren auf drei Instanzen ermöglicht. Durch Ausführen von [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) können Sie später jederzeit einen höheren oder niedrigeren Tarif für Ihre App festlegen. 

### <a name="create-a-web-app"></a>Erstellen einer Web-App
Erstellen Sie mit [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) eine Web-App mit einem eindeutigen Namen in `$appName`.

```azurecli
$appName = "<replace-with-a-unique-name>"
az appservice web create --name $appName --resource-group myResourceGroup --plan myAppServicePlan
```

### <a name="set-deployment-credentials"></a>Anmeldeinformationen für die Bereitstellung zurücksetzen
Verwenden Sie [az appservice web deployment user set](https://docs.microsoft.com/en-us/cli/azure/appservice/web/deployment/user#set), um die Anmeldeinformationen der Bereitstellung für App Service auf Kontoebene festzulegen.

```azurecli
az appservice web deployment user set --user-name <letters-numbers> --password <mininum-8-char-captital-lowercase-letters-numbers>
```

### <a name="configure-git-deployment"></a>Konfigurieren der Git-Bereitstellung
Konfigurieren Sie mit [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) die lokale Git-Bereitstellung.

```azurecli
az appservice web source-control config-local-git --name $appName --resource-group myResourceGroup
```

Mit diesem Befehl erhalten Sie etwa folgende Ausgabe:

```json
{
  "url": "https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git"
}
```

Verwenden Sie die zurückgegebene URL für die Konfiguration des Git-Remoteverzeichnisses. Der folgende Befehl verwendet das obige Ausgabebeispiel:

```powershell
git remote add azure https://user123@myuniqueappname.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-the-sample-application"></a>Bereitstellen der Beispielanwendung
Sie können die Beispielanwendung jetzt bereitstellen. Führen Sie `git push` aus.

```powershell
git push azure master
```

Verwenden Sie bei entsprechender Aufforderung das Kennwort, das Sie bei der Ausführung von `az appservice web deployment user set` angegeben haben.

### <a name="browse-to-azure-web-app"></a>Navigieren zur Azure-Web-App
Führen Sie den Befehl [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) aus, um die Liveausführung der App in Azure zu verfolgen.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-3---connect-to-redis"></a>Schritt 3: Herstellen einer Verbindung mit Redis
In diesem Schritt richten Sie Azure Redis Cache als externen, zusammengestellten Cache für Ihre Azure-Web-App ein. Sie können Redis schnell zum Zwischenspeichern der Seitenausgabe nutzen. Wenn Sie Ihre Web-Apps später horizontal hochskalieren, unterstützt Redis Sie darüber hinaus beim zuverlässigen Beibehalten von Benutzersitzungen über mehrere Instanzen hinweg.

### <a name="create-an-azure-redis-cache"></a>Erstellen eines Azure Redis Cache
Erstellen Sie mit [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) eine Azure Redis Cache-Instanz, und speichern Sie die JSON-Ausgabe. Verwenden Sie einen eindeutigen Namen in `$cacheName`.

```powershell
$cacheName = "<replace-with-a-unique-cache-name>"
$redis = (az redis create --name $cacheName --resource-group myResourceGroup --location "West Europe" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

### <a name="configure-the-application-to-use-redis"></a>Konfigurieren der Anwendung für die Verwendung von Redis
Formatieren Sie die Verbindungszeichenfolge für Ihren Cache.

```powershell
$connstring = "$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False"
$connstring 
```

In der zweiten Zeile sollte etwa folgende Ausgabe angezeigt werden:

```
mycachename.redis.cache.windows.net:6380,password=/rQP/TLz1mrEPpmh9b/gnfns/t9vBRXqXn3i1RwBjGA=,ssl=True,abortConnect=False
```

Erstellen Sie in Visual Studio im Projektstammverzeichnis eine Webkonfigurationsdatei namens `redis.config`, und fügen Sie in der Datei den folgenden Code ein. Verwenden Sie für `value` die Verbindungszeichenfolge aus der PowerShell-Ausgabe.

```xml
<appSettings>
  <add key="RedisConnection" value="your-azure-redis-cache-connection-string"/>
</appSettings>
```

Bei Betrachtung der Datei `.gitignore` im Git-Repository sehen Sie, dass diese Datei aus der Quellcodeverwaltung ausgeschlossen ist. So ist der Schutz Ihrer vertraulichen Daten gewährleistet. 

Öffnen Sie `Web.config`. Beachten Sie das Element `<appSettings file="redis.config">`, das die von Ihnen in `redis.config` erstellte Einstellung erhält. 

Suchen Sie den kommentierten Abschnitt, der `<sessionState>` und `<caching>` enthält. Heben Sie die Auskommentierung dieses Abschnitts auf.

![](./media/app-service-web-tutorial-hyper-scale-app/redisproviders.png)

Dieser Code sucht nach der in `RedisConnection` definierten Redis-Verbindungszeichenfolge. 

Die Anwendung verwendet nun Redis zum Verwalten von Sitzungen sowie zum Zwischenspeichern. Drücken Sie `F5`, um die Anwendung auszuführen. Sie können auch einen Redis-Verwaltungsclient herunterladen, um die nun im Cache gespeicherten Daten zu visualisieren.

### <a name="configure-the-connection-string-in-azure"></a>Konfigurieren der Verbindungszeichenfolge in Azure

Damit Ihre Anwendung in Azure verwendet werden kann, müssen Sie die gleiche Redis-Verbindungszeichenfolge in Ihrer Azure-Web-App konfigurieren. Da `redis.config` nicht in der Quellcodeverwaltung beibehalten wird, wird die Datei bei der Ausführung der Git-Bereitstellung nicht in Azure bereitgestellt.

Fügen Sie mit [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) die Verbindungszeichenfolge mit dem gleichen Namen (`RedisConnection`) hinzu.

az appservice web config appsettings update --settings "RedisConnection=$connstring" --name $appName --resource-group myResourceGroup

Beachten Sie, dass `$connstring` die formatierte Verbindungszeichenfolge enthält.

### <a name="redeploy-the-application-to-azure"></a>Erneutes Bereitstellen der Anwendung in Azure
Übertragen Sie Ihre Änderungen mithilfe von Git-Befehlen per Push an Azure:

```bash
git add .
git commit -m "now use Redis providers"
git push azure master
```

Verwenden Sie bei entsprechender Aufforderung das Kennwort, das Sie bei der Ausführung von `az appservice web deployment user set` angegeben haben.

### <a name="browse-to-the-azure-web-app"></a>Navigieren zur Azure-Web-App
Zeigen Sie mit [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) die Änderungen live in Azure an.

```azurecli
az appservice web browse --name $appName --resource-group myResourceGroup
```

## <a name="step-4---scale-to-multiple-instances"></a>Schritt 4: Skalieren auf mehrere Instanzen
Der App Service-Plan ist die Skalierungseinheit für Ihre Azure-Web-Apps. Zum horizontalen Hochskalieren Ihrer Web-App skalieren Sie den App Service-Plan.

Skalieren Sie mit [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update) den App Service-Plan horizontal auf drei Instanzen hoch. Dies ist die maximal zulässige Anzahl im B1-Tarif. B1 ist der Tarif, den Sie zuvor beim Erstellen des App Service-Plans ausgewählt haben. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --number-of-workers 3 
```

## <a name="step-5---scale-geographically"></a>Schritt 5: Geografisches Skalieren
Beim geografischen Skalieren führen Sie Ihre App in mehreren Regionen der Azure-Cloud aus. Dieses Setup führt für Ihre App basierend auf der Geografie einen weiteren Lastenausgleich aus und verringert die Reaktionszeit, indem die Entfernung der App zum Clientbrowser verringert wird.

In diesem Schritt skalieren Sie Ihre ASP.NET-Web-App mit [Azure Traffic Manager](https://docs.microsoft.com/en-us/azure/traffic-manager/) auf eine zweite Region. Anschließend besitzen Sie eine (bereits erstellte) Web-App, die in der Region „Europa, Westen“ ausgeführt wird, und eine (noch nicht erstellte) Web-App, die in der Region „Asien, Südosten“ ausgeführt wird. Beide Apps werden unter der gleichen Traffic Manager-URL bereitgestellt.

### <a name="scale-up-the-europe-app-to-standard-tier"></a>Zentrales Hochskalieren der Europa-App auf den Standard-Tarif
In App Service ist für die Integration in Azure Traffic Manager der Standard-Tarif erforderlich. Verwenden Sie [az appservice plan update](https://docs.microsoft.com/cli/azure/appservice/plan#update), um Ihren App Service-Plan zentral auf S1 hochzuskalieren. 

```azurecli
az appservice plan update --name myAppServicePlan --resource-group myResourceGroup --sku S1
```
### <a name="create-a-traffic-manager-profile"></a>Erstellen eines Traffic Manager-Profils 
Erstellen Sie mit [az network traffic-manager profile create](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#create) ein Traffic Manager-Profil, und fügen Sie es zur Ressourcengruppe hinzu. Verwenden Sie einen eindeutigen DNS-Namen in „$dnsName“.

```azurecli
$dnsName = "<replace-with-unique-dns-name>"
az network traffic-manager profile create --name myTrafficManagerProfile --resource-group myResourceGroup --routing-method Performance --unique-dns-name $dnsName
```

> [!NOTE]
> `--routing-method Performance` gibt an, dass dieses Profil [Benutzerdatenverkehr an den nächstgelegenen Endpunkt leitet](../traffic-manager/traffic-manager-routing-methods.md).

### <a name="get-the-resource-id-of-the-europe-app"></a>Abrufen der Ressourcen-ID der Europa-App
Rufen Sie mit [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) die Ressourcen-ID Ihrer Web-App ab.

```azurecli
$appId = az appservice web show --name $appName --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-europe-app"></a>Hinzufügen eines Traffic Manager-Endpunkts für die Europa-App
Fügen Sie mit [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) einen Endpunkt zu Ihrem Traffic Manager-Profil hinzu, und verwenden Sie die Ressourcen-ID Ihrer Web-Apps als Ziel.

```azurecli
az network traffic-manager endpoint create --name myWestEuropeEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appId
```

### <a name="get-the-traffic-manager-endpoint-url"></a>Abrufen der Traffic Manager-Endpunkt-URL
Das Traffic Manager-Profil enthält nun einen Endpunkt, der auf Ihre vorhandene Web-App verweist. Rufen Sie mit [az network traffic-manager profile show](https://docs.microsoft.com/cli/azure/network/traffic-manager/profile#show) die URL ab. 

```azurecli
az network traffic-manager profile show --name myTrafficManagerProfile --resource-group myResourceGroup --query dnsConfig.fqdn --output tsv
```

Kopieren Sie die Ausgabe in Ihren Browser. Ihre Web-App sollte nun wieder angezeigt werden.

### <a name="create-an-azure-redis-cache-in-asia"></a>Erstellen eines Azure Redis Cache in Asien
Jetzt replizieren Sie Ihre Azure-Web-App in der Region „Asien, Südosten“. Erstellen Sie zunächst mit [az redis create](https://docs.microsoft.com/en-us/cli/azure/redis#create) einen zweiten Azure Redis Cache in der Region „Asien, Südosten“. Dieser Cache muss mit Ihrer App in Asien zusammengestellt werden.

```powershell
$redis = (az redis create --name $cacheName-asia --resource-group myResourceGroup --location "Southeast Asia" --sku-capacity 0 --sku-family C --sku-name Basic | ConvertFrom-Json)
```

Mit `--name $cacheName-asia` erhält der Cache den Namen des Cache in der Region „Europa, Westen“ mit dem Suffix `-asia`.

### <a name="create-an-app-service-plan-in-asia"></a>Erstellen eines App Service-Plans in Asien
Erstellen Sie mit [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) einen zweiten App Service-Plan in der Region „Asien, Südosten“, und verwenden Sie dabei den gleichen S1-Tarif wie im Plan „Europa, Westen“.

```azurecli
az appservice plan create --name myAppServicePlanAsia --resource-group myResourceGroup --location "Southeast Asia" --sku S1
```

### <a name="create-a-web-app-in-asia"></a>Erstellen einer Web-App in Asien
Erstellen Sie mit [az appservice web create](https://docs.microsoft.com/en-us/cli/azure/appservice/web#create) eine zweite Web-App.

```azurecli
az appservice web create --name $appName-asia --resource-group myResourceGroup --plan myAppServicePlanAsia
```

Mit `--name $appName-asia` erhält die App den Namen der App in der Region „Europa, Westen“ mit dem Suffix `-asia`.

### <a name="configure-the-connection-string-for-redis"></a>Konfigurieren der Verbindungszeichenfolge für Redis
Fügen Sie mit [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) der Web-App die Verbindungszeichenfolge für den Cache in der Region „Asien, Südosten“ hinzu.

az appservice web config appsettings update --settings "RedisConnection=$($redis.hostname):$($redis.sslPort),password=$($redis.accessKeys.primaryKey),ssl=True,abortConnect=False" --name $appName-asia --resource-group myResourceGroup

### <a name="configure-git-deployment-for-the-asia-app"></a>Konfigurieren Sie die Git-Bereitstellung für die Asien-App.
Konfigurieren Sie mit [az appservice web source-control config-local-git](https://docs.microsoft.com/en-us/cli/azure/appservice/web/source-control#config-local-git) die lokale Git-Bereitstellung für die zweite Web-App.

```azurecli
az appservice web source-control config-local-git --name $appName-asia --resource-group myResourceGroup
```

Mit diesem Befehl erhalten Sie etwa folgende Ausgabe:

```json
{
  "url": "https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git"
}
```

Konfigurieren Sie mit der zurückgegebenen URL ein zweites Git-Verzeichnis für Ihr lokales Repository. Der folgende Befehl verwendet das obige Ausgabebeispiel:

```bash
git remote add azure-asia https://user123@myuniqueappname-asia.scm.azurewebsites.net/myuniqueappname.git
```

### <a name="deploy-your-sample-application"></a>Bereitstellen der Beispielanwendung
Führen Sie `git push` aus, um Ihre Beispielanwendung im zweiten Git-Remoteverzeichnis bereitzustellen. 

```bash
git push azure-asia master
```

Verwenden Sie bei entsprechender Aufforderung das Kennwort, das Sie bei der Ausführung von `az appservice web deployment user set` angegeben haben.

### <a name="browse-to-the-asia-app"></a>Navigieren zur Asien-App
Überprüfen Sie mit [az appservice web browse](https://docs.microsoft.com/en-us/cli/azure/appservice/web#browse) die Liveausführung der App in Azure.

```azurecli
az appservice web browse --name $appName-asia --resource-group myResourceGroup
```

### <a name="get-the-resource-id-of-the-asia-app"></a>Abrufen der Ressourcen-ID der Asien-App
Rufen Sie mit [az appservice web show](https://docs.microsoft.com/en-us/cli/azure/appservice/web#show) die Ressourcen-ID Ihrer Web-App in „Asien, Südosten“ ab.

```azurecli
$appIdAsia = az appservice web show --name $appName-asia --resource-group myResourceGroup --query id --output tsv
```

### <a name="add-a-traffic-manager-endpoint-for-the-asia-app"></a>Hinzufügen eines Traffic Manager-Endpunkts für die Asien-App
Fügen Sie mit [az network traffic-manager endpoint create](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint#create) einen zweiten Endpunkt zum Traffic Manager-Profil hinzu.

```azurecli
az network traffic-manager endpoint create --name myAsiaEndpoint --profile-name myTrafficManagerProfile --resource-group myResourceGroup --type azureEndpoints --target-resource-id $appIdAsia
```

### <a name="add-region-identifier-to-web-apps"></a>Hinzufügen einer Regions-ID zu Web-Apps
Fügen Sie mit [az appservice web config appsettings update](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings#update) eine regionsspezifische Umgebungsvariable hinzu.

```azurecli
az appservice web config appsettings update --settings "Region=West Europe" --name $appName --resource-group myResourceGroup
az appservice web config appsettings update --settings "Region=Southeast Asia" --name $appName-asia --resource-group myResourceGroup
```

Der Anwendungscode nutzt bereits diese Anwendungseinstellung. Sehen Sie sich `HighScaleApp\Views\Home\Index.cshtml` an.

### <a name="complete"></a>Fertig!

Versuchen Sie nun, von Browsern in verschiedenen geografischen Regionen auf die URL des Traffic Manager-Profils zuzugreifen. Für Clientbrowser in Europa sollte „ASP.NET West Europe“ (ASP.NET: Europa, Westen) und für Clientbrowser in Asien „ASP.NET Southeast Asia“ (ASP.NET: Asien, Südosten) angezeigt werden.

## <a name="more-resources"></a>Weitere Ressourcen

