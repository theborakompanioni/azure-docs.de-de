---
title: Installieren von MongoDB auf einem virtuellen Linux-Computer mithilfe von Azure CLI 2.0 | Microsoft Docs
description: Informationen zum Installieren und Konfigurieren von MongoDB auf einem virtuellen Linux-Computer mithilfe von Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 3f55b546-86df-4442-9ef4-8a25fae7b96e
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/14/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 67d4fee2fc59651903d4c02d1fce84c7b81e5da1
ms.openlocfilehash: c3be1b3459e10281bdbe949b0d303d117d48979f
ms.lasthandoff: 02/27/2017


---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm"></a>Installieren und Konfigurieren von MongoDB auf einem virtuellen Linux-Computer
[MongoDB](http://www.mongodb.org) ist eine beliebte, leistungsfähige Open Source-NoSQL-Datenbank. In diesem Artikel wird das Installieren und Konfigurieren von MongoDB auf einem virtuellen Linux-Computer mithilfe von Azure CLI 2.0 beschrieben. Sie können diese Schritte auch mit [Azure CLI 1.0](virtual-machines-linux-install-mongodb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen. Hier finden Sie Beispiele mit Informationen zu den folgenden Schritten:

* [Manuelles Installieren und Konfigurieren einer einfachen MongoDB-Instanz](#manually-install-and-configure-mongodb-on-a-vm)
* [Erstellen einer einfachen MongoDB-Instanz mithilfe einer Resource Manager-Vorlage](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Erstellen eines komplexen MongoDB-Shardclusters mit Replikatgruppen mithilfe einer Resource Manager-Vorlage](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Manuelles Installieren und Konfigurieren von MongoDB auf einem virtuellen Computer
Für MongoDB stehen [Installationsanweisungen](https://docs.mongodb.com/manual/administration/install-on-linux/) für Linux-Distributionen zur Verfügung, u.a. für Red Hat/CentOS, SUSE, Ubuntu und Debian. Das folgende Beispiel erstellt mit einem unter `~/.ssh/id_rsa.pub` gespeicherten SSH-Schlüssel einen virtuellen `CentOS`-Computer: Zum Erstellen dieser Umgebung muss die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein.

Erstellen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `West US`:

```azurecli
 az group create --name myResourceGroup --location westus
```

Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer. Im folgenden Beispiel wird ein virtueller Computer mit dem Namen `myVM` mit einem Benutzer namens `azureuser` mithilfe der Authentifizierung mit einem öffentlichen SSH-Schlüssel und dem öffentlichen DNS-Eintrag `mypublicdns` erstellt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image CentOS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --public-ip-address-dns-name mypublicdns
```

Melden Sie sich bei dem virtuellen Computer mit der öffentlichen DNS-Adresse des virtuellen Computers an. Sie können die öffentliche DNS-Adresse mit dem Befehl [az vm show](/cli/azure/vm#show) anzeigen:

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

Stellen Sie eine SSH-Verbindung mit dem virtuellen Computer unter Verwendung Ihres eigenen Benutzernamens und der öffentlichen DNS-Adresse her:

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

Erstellen Sie zum Hinzufügen der Installationsquellen für MongoDB wie folgt eine `yum`-Repositorydatei:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.2.repo
```

Öffnen Sie die MongoDB-Repositorydatei zur Bearbeitung. Fügen Sie die folgenden Zeilen hinzu:

```sh
[mongodb-org-3.2]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.2/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.2.asc
```

Installieren Sie MongoDB folgendermaßen mithilfe von `yum`:

```bash
sudo yum install -y mongodb-org
```

Standardmäßig wird SELinux für CentOS-Images erzwungen. Dadurch wird Ihr Zugriff auf MongoDB verhindert. Installieren Sie Richtlinienverwaltungstools, und konfigurieren Sie SELinux wie folgt, um MongoDB die Nutzung des TCP-Standardports 27017 zu ermöglichen. 

```bash
sudo yum install -y policycoreutils-python
sudo semanage port -a -t mongod_port_t -p tcp 27017
```

Starten Sie den MongoDB-Dienst wie folgt:

```bash
sudo service mongod start
```

Überprüfen Sie die MongoDB-Installation durch Herstellen einer Verbindung mit dem lokalen `mongo`-Client:

```bash
mongo
```

Testen Sie nun die MongoDB-Instanz, indem Sie einige Daten hinzufügen und anschließend eine Suche ausführen:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```

Konfigurieren Sie bei Bedarf den automatischen Start von MongoDB bei einem Systemneustart:

```bash
sudo chkconfig mongod on
```


## <a name="create-basic-mongodb-instance-on-centos-using-a-template"></a>Erstellen einer einfachen MongoDB-Instanz unter CentOS mithilfe einer Vorlage
Sie können mithilfe der folgenden Azure-Schnellstartvorlage von Github eine einfache MongoDB-Instanz auf einem einzelnen virtuellen CentOS-Computer erstellen. Diese Vorlage verwendet die benutzerdefinierte Skripterweiterung für Linux, um Ihrem neu erstellten virtuellen CentOS-Computer ein `yum`-Repository hinzuzufügen und MongoDB zu installieren.

* [Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) (Einfache MongoDB-Instanz unter CentOS) – https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Zum Erstellen dieser Umgebung muss die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein. Erstellen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `West US`:

```azurecli
az group create --name myResourceGroup --location westus
```

Stellen Sie als Nächstes mit dem Befehl [az group deployment create](/cli/azure/group/deployment#create) die MongoDB-Vorlage bereit. Definieren Sie bei Bedarf Ihre eigenen Ressourcennamen und -größen, z.B. für `newStorageAccountName`, `virtualNetworkName` und `vmSize`:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"newStorageAccountName": {"value": "mystorageaccount"},
    "adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "dnsNameForPublicIP": {"value": "mypublicdns"},
    "virtualNetworkName": {"value": "myVnet"},
    "vmSize": {"value": "Standard_DS1_v2"},
    "vmName": {"value": "myVM"},
    "publicIPAddressName": {"value": "myPublicIP"},
    "nicName": {"value": "myNic"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

Melden Sie sich bei dem virtuellen Computer mit der öffentlichen DNS-Adresse des virtuellen Computers an. Sie können die öffentliche DNS-Adresse mit dem Befehl [az vm show](/cli/azure/vm#show) anzeigen:

```azurecli
az vm show -g myResourceGroup -n myVM -d --query [fqdns] -o tsv
```

Stellen Sie eine SSH-Verbindung mit dem virtuellen Computer unter Verwendung Ihres eigenen Benutzernamens und der öffentlichen DNS-Adresse her:

```bash
ssh azureuser@mypublicdns.westus.cloudapp.azure.com
```

Überprüfen Sie wie folgt die MongoDB-Installation durch Herstellen einer Verbindung mit dem lokalen `mongo`-Client:

```bash
mongo
```

Testen Sie nun die Instanz, indem Sie folgendermaßen einige Daten hinzufügen und anschließend eine Suche ausführen:

```sh
> db
test
> db.foo.insert( { a : 1 } )  
> db.foo.find()  
{ "_id" : ObjectId("57ec477cd639891710b90727"), "a" : 1 }
> exit
```


## <a name="create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template"></a>Erstellen eines komplexen MongoDB-Shardclusters unter CentOS mithilfe einer Vorlage
Sie können mithilfe der folgenden Azure-Schnellstartvorlage von Github einen komplexen MongoDB-Shardcluster erstellen. Diese Vorlage basiert auf den [bewährten Methoden für MongoDB-Shardcluster](https://docs.mongodb.com/manual/core/sharded-cluster-components/), um Redundanz und hohe Verfügbarkeit zu gewährleisten. Die Vorlage erstellt zwei Shards mit drei Knoten in jeder Replikatgruppe. Darüber hinaus werden eine Konfigurationsserver-Replikatgruppe mit drei Knoten sowie zwei `mongos`-Routerserver erstellt, um Konsistenz für Anwendungen auf allen Shards zu ermöglichen.

* [MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) (MongoDB-Shardcluster unter CentOS) – https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Für die Bereitstellung dieses komplexen MongoDB-Shardclusters sind mehr als 20 Kerne erforderlich. Dies ist in der Regel die Standardanzahl von Kernen pro Region für ein Abonnement. Stellen Sie eine Azure-Supportanfrage zum Erhöhen der Kernanzahl.

Zum Erstellen dieser Umgebung muss die neueste Version von [Azure CLI 2.0](/cli/azure/install-az-cli2) installiert sein, und Sie müssen mithilfe von [az login](/cli/azure/#login) bei einem Azure-Konto angemeldet sein. Erstellen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `West US`:

```azurecli
az group create --name myResourceGroup --location westus
```

Stellen Sie als Nächstes mit dem Befehl [az group deployment create](/cli/azure/group/deployment#create) die MongoDB-Vorlage bereit. Definieren Sie bei Bedarf Ihre eigenen Ressourcennamen und -größen, z.B. für `mongoAdminUsername`, `sizeOfDataDiskInGB` und `configNodeVmSize`:

```azurecli
az group deployment create --resource-group myResourceGroup \
  --parameters '{"adminUsername": {"value": "azureuser"},
    "adminPassword": {"value": "P@ssw0rd!"},
    "mongoAdminUsername": {"value": "mongoadmin"},
    "mongoAdminPassword": {"value": "P@ssw0rd!"},
    "dnsNamePrefix": {"value": "mypublicdns"},
    "environment": {"value": "AzureCloud"},
    "numDataDisks": {"value": "4"},
    "sizeOfDataDiskInGB": {"value": 20},
    "centOsVersion": {"value": "7.0"},
    "routerNodeVmSize": {"value": "Standard_DS3_v2"},
    "configNodeVmSize": {"value": "Standard_DS3_v2"},
    "replicaNodeVmSize": {"value": "Standard_DS3_v2"},
    "zabbixServerIPAddress": {"value": "Null"}}' \
  --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json \
  --name myMongoDBCluster --no-wait
```

Es kann über eine Stunde dauern, bis die Bereitstellung und die Konfiguration aller VM-Instanzen abgeschlossen ist. Das Flag `--no-wait` wird am Ende des vorherigen Befehls verwendet, sodass die Steuerung an die Eingabeaufforderung zurückgegeben wird, nachdem die Vorlagenbereitstellung auf der Azure-Plattform akzeptiert wurde. Sie können dann mit dem Befehl [az group deployment show](/cli/azure/group/deployment#show) den Bereitstellungsstatus anzeigen. Im folgenden Beispiel wird der Status für die Bereitstellung `myMongoDBCluster` in der Ressourcengruppe `myResourceGroup` angezeigt:

```azurecli
az group deployment show --resource-group myResourceGroup --name myMongoDBCluster \
    --query [properties.provisioningState] --output tsv
```

## <a name="next-steps"></a>Nächste Schritte
In diesen Beispielen wird vom virtuellen Computer eine lokale Verbindung mit der MongoDB-Instanz hergestellt. Wenn Sie von einem anderen virtuellen Computer oder Netzwerk eine Verbindung mit der MongoDB-Instanz herstellen möchten, stellen Sie sicher, dass die entsprechenden [Netzwerksicherheitsgruppen-Regeln](virtual-machines-linux-nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) erstellt werden.

Weitere Informationen zur Erstellung mithilfe von Vorlagen finden Sie in der [Übersicht über den Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

Die Azure Resource Manager-Vorlagen verwenden die benutzerdefinierte Skripterweiterung zum Herunterladen und Ausführen von Skripts auf Ihren virtuellen Computern. Weitere Informationen finden Sie unter [Verwenden der benutzerdefinierten Skripterweiterung von Azure mit virtuellen Linux-Computern](virtual-machines-linux-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).


