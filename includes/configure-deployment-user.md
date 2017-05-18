## <a name="configure-a-deployment-user"></a>Konfigurieren eines Bereitstellungsbenutzers  

Für FTP und lokales Git muss zur Authentifizierung Ihrer Bereitstellung ein Bereitstellungsbenutzer auf dem Server konfiguriert werden.

> [!NOTE]
> Ein Bereitstellungsbenutzer wird bei FTP- und lokalen Git-Bereitstellungen für eine Web-App benötigt.
> `username` und `password` gelten für die Kontoebene. Sie unterscheiden sich von den Anmeldeinformationen Ihres Azure-Abonnements.
>

Führen Sie den Befehl [az appservice web deployment user set](/cli/azure/appservice/web/deployment/user#set) aus, um Ihre Anmeldeinformationen für die Bereitstellung zu erstellen.

```azurecli
az appservice web deployment user set --user-name <username> --password <password>
```

Der Benutzername muss eindeutig und das Kennwort sicher sein. Wenn Sie den Fehler ` 'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler ` 'Bad Request'. Details: 400` erhalten, müssen Sie ein stärkeres Kennwort verwenden.

Sie müssen diesen Bereitstellungsbenutzer nur einmal erstellen und können ihn dann für alle Azure-Bereitstellungen verwenden.

Notieren Sie sich den Benutzernamen und das Kennwort für die spätere Bereitstellung der Anwendung.