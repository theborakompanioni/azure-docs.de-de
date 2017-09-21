Erstellen Sie in der Cloud Shell im App Service-Plan `myAppServicePlan` mit dem Befehl [az webapp create](/cli/azure/webapp#create) eine [Web-App](../articles/app-service/containers/app-service-linux-intro.md). Vergessen Sie nicht, `<app name>` durch einen eindeutigen App-Namen zu ersetzen.

Die Runtime im folgenden Befehl ist auf `dotnetcore|1.1` festgelegt. Führen Sie zum Anzeigen aller unterstützten Runtimes den Befehl [az webapp list-runtimes](/cli/azure/webapp#list-runtimes) aus. 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> --runtime "dotnetcore|1.1" --deployment-local-git
```

Nach Erstellung der Web-App zeigt die Azure-Befehlszeilenschnittstelle eine Ausgabe wie im folgenden Beispiel an:

```json
Local git is configured with url of 'https://<username>@<app name>.scm.azurewebsites.net/<app name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app name>.scm.azurewebsites.net/<app name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Sie haben eine leere neue Web-App in einem Linux-Container mit aktivierter git-Bereitstellung erstellt.

> [!NOTE]
> Die URL des Git-Remotespeicherorts wird in der `deploymentLocalGitUrl`-Eigenschaft im Format `https://<username>@<app name>.scm.azurewebsites.net/<app name>.git` angezeigt. Speichern Sie diese URL, da Sie sie später noch benötigen.
>
