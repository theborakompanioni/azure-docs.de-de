## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

 Eine [Azure-Ressourcengruppe](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups) ist ein logischer Container, in dem Ressourcen wie Web-Apps und Datenbanken verwaltet werden. In einer Ressourcengruppe stellen Sie die Ressourcen bereit, aktualisieren und löschen diese.

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Verwenden Sie den Befehl `az appservice list-locations`, um die verfügbaren Speicherorte anzuzeigen. Im Allgemeinen erstellen Sie Ressourcen in einer Region in Ihrer Nähe.

## <a name="create-an-azure-app-service-plan"></a>Erstellen eines Azure App Service-Plans

Erstellen Sie mit dem Befehl [az appservice plan create](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) einen kostenlosen [App Service-Plan](/cli/azure/appservice/plan#create).

[!INCLUDE [app-service-plan](app-service-plan.md)]

Mit dem folgenden Befehl wird ein App Service-Plan mit dem Namen `quickStartPlan` mit dem Tarif **Free** erstellt.

```azurecli
az appservice plan create --name quickStartPlan --resource-group myResourceGroup --sku FREE
```

Nach Erstellung des App Service-Plans zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "quickStartPlan",
  "geoRegion": "West Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/quickStartPlan",
  "kind": "app",
  "location": "West Europe",
  "maximumNumberOfWorkers": 1,
  "name": "quickStartPlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
} 
```

## <a name="create-a-web-app"></a>Erstellen einer Web-App