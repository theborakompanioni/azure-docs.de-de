## <a name="push-to-azure-from-git"></a>Übertragen von Git an Azure mithilfe von Push

Fügen Sie Ihrem lokalen Git-Repository eine Azure-Remoteinstanz hinzu.

```bash
git remote add azure <URI from previous step>
```

Führen Sie einen Pushvorgang zur Azure-Remoteinstanz durch, um Ihre App bereitzustellen. Sie werden zum Eingeben des Kennworts aufgefordert, das Sie zuvor bei der Erstellung des Bereitstellungsbenutzers erstellt haben. Stellen Sie sicher, dass Sie das Kennwort eingeben, das Sie unter [Konfigurieren eines Bereitstellungsbenutzers](#configure-a-deployment-user) erstellt haben, und nicht das Kennwort, das Sie für die Anmeldung im Azure-Portal verwenden.

```bash
git push azure master
```

Mit dem vorherigen Befehl werden Informationen angezeigt, die den Informationen im folgenden Beispiel ähneln:
