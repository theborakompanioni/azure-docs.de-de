Verwenden Sie die Azure CLI, um die Remotebereitstellungs-URL für Ihre API-App abzurufen. Ersetzen Sie im folgenden Befehl *\<app_name>* durch den Namen Ihrer Web-App.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group myResourceGroup --query url --output tsv
```

Konfigurieren Sie die lokale Git-Bereitstellung, um mithilfe von Push an die Remoteinstanz übertragen zu können.

```bash
git remote add azure <URI from previous step>
```

Führen Sie einen Pushvorgang zur Azure-Remoteinstanz durch, um Ihre App bereitzustellen. Sie werden zum Eingeben des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers erstellt haben. Stellen Sie sicher, dass Sie das Kennwort eingeben, das Sie zuvor erstellt haben, und nicht das Kennwort, das Sie für die Anmeldung im Azure-Portal verwenden.

```bash
git push azure master
```
