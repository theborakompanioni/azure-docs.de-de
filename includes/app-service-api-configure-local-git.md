Konfigurieren Sie die lokale Git-Bereitstellung für die API-App mit dem Befehl [az webapp source-control config-local-git](/cli/azure/appservice/web/source-control#config-local-git).   

App Service unterstützt mehrere Möglichkeiten zum Bereitstellen von Inhalt für eine Web-App, z.B. FTP, lokales Git, GitHub, Visual Studio Team Services und Bitbucket. Für diesen Schnellstart führen Sie die Bereitstellung per lokalem Git durch. Dies bedeutet, dass Sie für die Bereitstellung einen Git-Befehl verwenden, um die Daten aus einem lokalen Repository in ein Repository in Azure zu übertragen.  

Legen Sie mit dem Skript unten die Anmeldeinformationen für die Bereitstellung auf Kontoebene fest, die Sie beim Senden des Codes per Push verwenden. Achten Sie darauf, eigene Werte für Benutzernamen und Kennwort einzugeben.   

```azurecli-interactive
az webapp deployment user set --user-name <desired user name> --password <desired password>
```
