Erstellen Sie in der Cloud Shell mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe.

[!INCLUDE [resource group intro text](resource-group.md)]

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *Europa, Westen*.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

Im Allgemeinen erstellen Sie Ressourcengruppen und Ressourcen in einer Region in Ihrer Nähe. Wenn Sie alle unterstützten Speicherorte für die Azure-Web-Apps anzeigen möchten, führen Sie den Befehl `az appservice list-locations` aus. 