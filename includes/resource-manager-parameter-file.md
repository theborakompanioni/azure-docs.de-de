## Parameterdatei

Bei der Verwendung einer Parameterdatei zum Übergeben von Parameterwerten an Ihre Vorlage während der Bereitstellung müssen Sie eine JSON-Datei in einem Format wie im folgenden Beispiel erstellen.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "webSiteName": {
                "value": "ExampleSite"
            },
            "webSiteHostingPlanName": {
                "value": "DefaultPlan"
            },
            "webSiteLocation": {
                "value": "West US"
            },
            "adminPassword": {
                "reference": {
                   "keyVault": {
                      "id": "/subscriptions/{guid}/resourceGroups/{group-name}/providers/Microsoft.KeyVault/vaults/{vault-name}"
                   }, 
                   "secretName": "sqlAdminPassword" 
                }   
            }
       }
    }

Die Parameterdatei darf nicht größer als 64 KB sein.

<!---HONumber=AcomDC_0615_2016-->