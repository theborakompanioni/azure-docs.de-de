### <a name="app-service-plan"></a>App Service-Plan
Erstellt den Dienstplan zum Hosten der Web-App. Geben Sie den Namen des Plans über den **hostingPlanName** -Parameter an. Der Speicherort des Plans ist mit demjenigen für die Ressourcengruppe identisch. Der Tarif und die Workergröße werden in den Parametern **sku** und **workerSize** angegeben.

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

