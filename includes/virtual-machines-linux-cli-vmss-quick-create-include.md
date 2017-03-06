## <a name="prerequisites"></a>Voraussetzungen

Beschaffen Sie eine [kostenlose Testversion eines Azure-Abonnements](https://azure.microsoft.com/pricing/free-trial/), und installieren Sie [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2), falls noch nicht geschehen.

## <a name="create-the-scale-set"></a>Erstellen der Skalierungsgruppe

Erstellen Sie zuerst eine Ressourcengruppe, in der die Skalierungsgruppe bereitgestellt werden kann:

```azurecli
az group create --location westus --name myResourceGroup
```

Erstellen Sie jetzt die Skalierungsgruppe mithilfe des Befehls `az vmss create`. Im folgenden Beispiel wird eine Linux-Skalierungsgruppe mit dem Namen `myvmss` in der Ressourcengruppe mit dem Namen `myrg` erstellt:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image UbuntuLTS --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

Das folgende Beispiel erstellt eine Windows-Skalierungsgruppe mit der gleichen Konfiguration:

```azurecli
az vmss create --resource-group myResourceGroup --name myVmss \
    --image Win2016Datacenter --admin-username azureuser \
    --authentication-type password --admin-password P4$$w0rd
```

Wenn Sie ein anderes Betriebssystemimage wählen möchten, können Sie die verfügbaren Images mit dem Befehl `az vm image list` oder `az vm image list --all` anzeigen. Verwenden Sie den Befehl `az vmss list_instance_connection_info`, um die Verbindungsinformationen für die VMs in der Skalierungsgruppe anzuzeigen:

```azurecli
az vmss list_instance_connection_info --resource-group myResourceGroup --name myVmss
```