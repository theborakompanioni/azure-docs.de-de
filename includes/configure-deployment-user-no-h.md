Erstellen von Anmeldeinformationen für die Bereitstellung mit dem Befehl [az webapp deployment user set](/cli/azure/webapp/deployment/user#set).

Bei der FTP- und der lokalen Git-Bereitstellung für eine Web-App ist ein Bereitstellungsbenutzer erforderlich. Der Benutzername und das Kennwort gelten auf der Kontoebene. Sie unterscheiden sich von den Anmeldeinformationen Ihres Azure-Abonnements.

Ersetzen Sie im folgenden Befehl *\<user-name>* und *\<password>* durch einen neuen Benutzernamen und ein neues Kennwort.

```azurecli-interactive
az webapp deployment user set --user-name <username> --password <password>
```

Der Benutzername muss eindeutig sein. Das Kennwort muss mindestens acht Zeichen lang sein und zwei der folgenden drei Elemente enthalten: Buchstaben, Zahlen, Symbole. Wenn Sie den Fehler ` 'Conflict'. Details: 409` erhalten, müssen Sie den Benutzernamen ändern. Wenn Sie den Fehler ` 'Bad Request'. Details: 400` erhalten, müssen Sie ein sichereres Kennwort verwenden.

Sie müssen diesen Bereitstellungsbenutzer nur einmal erstellen und können ihn dann für alle Azure-Bereitstellungen verwenden.

> [!NOTE]
> Notieren Sie sich den Benutzernamen und das Kennwort. Sie benötigen sie später für die Bereitstellung der Web-App.
>
>