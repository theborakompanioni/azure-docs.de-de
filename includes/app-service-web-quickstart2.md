Erstellen Sie die Web-App mit dem Befehl [az appservice web create](/cli/azure/appservice/web#create). Ersetzen Sie im folgenden Befehl den Platzhalter `<app_name>` durch einen eindeutigen App-Namen. `<app_name>` wird auf der DNS-Standardwebsite für die Web-App verwendet. Wenn `<app_name>` nicht eindeutig ist, wird die Fehlermeldung „Eine Website mit dem Namen <App-Name> ist bereits vorhanden“ angezeigt.

```azurecli
az appservice web create --name <app_name> --resource-group myResourceGroup --plan quickStartPlan
```

Nach der Erstellung der Web-App zeigt die Azure-Befehlszeilenschnittstelle Informationen wie im folgenden Beispiel an:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "<app_name>.azurewebsites.net",
    "<app_name>.scm.azurewebsites.net"
  ],
  "gatewaySiteName": null,
  "hostNameSslStates": [
    {
      "hostType": "Standard",
      "name": "<app_name>.azurewebsites.net",
      "sslState": "Disabled",
      "thumbprint": null,
      "toUpdate": null,
      "virtualIp": null
    }
    < JSON data removed for brevity. >
}
```

Navigieren Sie zu der Website (`http://<app_name>.azurewebsites.net`), um sich Ihre neu erstellte Web-App anzusehen.

![app-service-web-service-created](../articles/app-service-web/media/app-service-web-get-started-nodejs-poc/app-service-web-service-created.png)


## <a name="configure-local-git-deployment"></a>Konfigurieren der lokalen Git-Bereitstellung

App Service unterstützt mehrere Möglichkeiten zum Bereitstellen von Inhalt für eine Web-App, z.B. FTP, lokales Git, GitHub, Visual Studio Team Services und Bitbucket. 

Für diesen Schnellstart führen Sie die Bereitstellung per lokalem Git durch. Dies bedeutet, dass Sie für die Bereitstellung einen Git-Befehl verwenden, um die Daten aus einem lokalen Repository in ein Repository in Azure zu übertragen. 

Konfigurieren Sie mit dem Befehl [az appservice web source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git) den lokalen Git-Zugriff auf die Web-App.

```azurecli
az appservice web source-control config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Die Ausgabe hat folgendes Format:

```
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

Speichern Sie den angezeigten URI. Sie verwenden ihn im nächsten Schritt. `<username>` ist der [Bereitstellungsbenutzer](#configure-a-deployment-user), den Sie in einem der vorherigen Schritte erstellt haben.

## <a name="push-to-azure-from-git"></a>Übertragen von Daten aus Git an Azure per Pushvorgang

Fügen Sie Ihrem lokalen Git-Repository eine Azure-Remoteinstanz hinzu.

```bash
git remote add azure <URI from previous step>
```

Führen Sie einen Pushvorgang zur Azure-Remoteinstanz durch, um Ihre App bereitzustellen. Sie werden zum Eingeben des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers erstellt haben. Stellen Sie sicher, dass Sie das Kennwort eingeben, das Sie unter [Konfigurieren eines Bereitstellungsbenutzers](#configure-a-deployment-user) erstellt haben, und nicht das Kennwort, das Sie für die Anmeldung im Azure-Portal verwenden.

```azurecli
git push azure master
```

Mit dem vorherigen Befehl werden Informationen angezeigt, die den Informationen im folgenden Beispiel ähneln:

```bash
Counting objects: 13, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (13/13), 2.07 KiB | 0 bytes/s, done.
Total 13 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'cc39b1e4cb'.
remote: Generating deployment script.
remote: Generating deployment script for Web Site
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling Basic Web Site deployment.
remote: KuduSync.NET from: 'D:\home\site\repository' to: 'D:\home\site\wwwroot'
remote: Deleting file: 'hostingstart.html'
remote: Copying file: '.gitignore'
remote: Copying file: 'LICENSE'
remote: Copying file: 'README.md'
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-app"></a>Navigieren zur App


Navigieren Sie zur bereitgestellten App:

```
http://<app_name>.azurewebsites.net
```

