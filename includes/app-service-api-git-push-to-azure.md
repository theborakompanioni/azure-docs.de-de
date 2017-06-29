Mit der Azure CLI können Sie die URL für die Remotebereitstellung für die API-App abrufen. Konfigurieren Sie dann die lokale Git-Bereitstellung, um mithilfe von Push an die Remoteinstanz übertragen können.

```bash
giturl=$(az webapp deployment source config-local-git -n $app_name \ -g myResourceGroup --query [url] -o tsv)

git remote add azure $giturl
```

Führen Sie einen Pushvorgang zur Azure-Remoteinstanz durch, um Ihre App bereitzustellen. Sie werden zum Eingeben des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers erstellt haben. Stellen Sie sicher, dass Sie das Kennwort eingeben, das Sie zuvor erstellt haben, und nicht das Kennwort, das Sie für die Anmeldung im Azure-Portal verwenden.

```bash
git push azure master
```
