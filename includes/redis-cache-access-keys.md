Zum Herstellen der Verbindung mit einer Azure Redis Cache-Instanz benötigen Cacheclients den Hostnamen, die Ports und die Schlüssel des Caches. Es kann sein, dass von einigen Clients unter etwas anderen Namen auf diese Elemente verwiesen wird. Sie können diese Informationen im Azure-Portal oder mit Befehlszeilentools, z.B. der Azure-CLI, abrufen.

### <a name="retrieve-host-name-ports-and-access-keys-using-the-azure-portal"></a>Abrufen von Hostname, Ports und Zugriffsschlüsseln mit dem Azure-Portal
Zum Abrufen von Hostname, Ports und Zugriffsschlüsseln mit dem Azure-Portal [navigieren](../articles/redis-cache/cache-configure.md#configure-redis-cache-settings) Sie im [Azure-Portal](https://portal.azure.com) zu Ihrem Cache und klicken im **Ressourcenmenü** auf **Zugriffsschlüssel** und **Eigenschaften**. 

![Redis Cache: Einstellungen](media/redis-cache-access-keys/redis-cache-hostname-ports-keys.png)

### <a name="retrieve-host-name-ports-and-access-keys-using-azure-cli"></a>Abrufen von Hostname, Ports und Zugriffsschlüsseln mit der Azure-CLI
Zum Abrufen des Hostnamens und der Ports per Azure CLI 2.0 können Sie [az redis show](https://docs.microsoft.com/cli/azure/redis#show) aufrufen, und zum Abrufen der Schlüssel können Sie [az redis list-keys](https://docs.microsoft.com/cli/azure/redis#list-keys) aufrufen. Mit dem folgenden Skript werden diese beiden Befehle aufgerufen und der Hostname, die Ports und die Schlüssel in der Konsole ausgegeben.

```azurecli
#/bin/bash

# Retrieve the hostname, ports, and keys for contosoCache located in contosoGroup

# Retrieve the hostname and ports for an Azure Redis Cache instance
redis=($(az redis show --name contosoCache --resource-group contosoGroup --query [hostName,enableNonSslPort,port,sslPort] --output tsv))

# Retrieve the keys for an Azure Redis Cache instance
keys=($(az redis list-keys --name contosoCache --resource-group contosoGroup --query [primaryKey,secondaryKey] --output tsv))

# Display the retrieved hostname, keys, and ports
echo "Hostname:" ${redis[0]}
echo "Non SSL Port:" ${redis[2]}
echo "Non SSL Port Enabled:" ${redis[1]}
echo "SSL Port:" ${redis[3]}
echo "Primary Key:" ${keys[0]}
echo "Secondary Key:" ${keys[1]}
```

Weitere Informationen zu diesem Skript finden Sie unter [Get the hostname, ports, and keys for Azure Redis Cache](../articles/redis-cache/scripts/cache-keys-ports.md) (Abrufen von Hostname, Ports und Schlüsseln für Azure Redis Cache). Weitere Informationen zu Azure CLI 2.0 finden Sie unter [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (Installieren von Azure CLI 2.0) und [Get started with Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) (Erste Schritte mit Azure CLI 2.0).
