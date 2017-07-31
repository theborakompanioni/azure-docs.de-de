Konfigurieren Sie die lokale Git-Bereitstellung für die Web-App mit dem Befehl [az webapp deployment source config-local-git](/cli/azure/webapp/deployment/source#config-local-git).

App Service unterstützt mehrere Möglichkeiten zum Bereitstellen von Inhalt für eine Web-App, z.B. FTP, lokales Git, GitHub, Visual Studio Team Services und Bitbucket. Für diesen Schnellstart führen Sie die Bereitstellung per lokalem Git durch. Dies bedeutet, dass Sie für die Bereitstellung einen Git-Befehl verwenden, um die Daten aus einem lokalen Repository in ein Repository in Azure zu übertragen. 

Ersetzen Sie im folgenden Befehl *\<app_name>* durch den Namen Ihrer Web-App.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Die Ausgabe hat folgendes Format:

```bash
https://<username>@<app_name>.scm.azurewebsites.net:443/<app_name>.git
```

`<username>` ist der [Bereitstellungsbenutzer](#configure-a-deployment-user), den Sie in einem der vorherigen Schritte erstellt haben.

Kopieren Sie den angezeigten URI, da Sie ihn im nächsten Schritt benötigen.
