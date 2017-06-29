Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe.

[!INCLUDE [resource group intro text](resource-group.md)]

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *westeurope*.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Verwenden Sie den Befehl `az appservice list-locations`, um die verfügbaren Speicherorte anzuzeigen. Im Allgemeinen erstellen Sie Ressourcen in einer Region in Ihrer Nähe.
