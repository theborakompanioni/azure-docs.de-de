---
title: Installieren von MongoDB auf einem virtuellen Linux-Computer mithilfe von Azure CLI 1.0 | Microsoft Docs
description: Hier erfahren Sie, wie Sie mit dem Resource Manager-Bereitstellungsmodell MongoDB auf einem virtuellen Linux-Computer in Azure installieren und konfigurieren.
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
ms.date: 05/11/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: c97ade0a3d95824f723aad55776de861fe49441f
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="how-to-install-and-configure-mongodb-on-a-linux-vm-using-the-azure-cli-10"></a>Installieren und Konfigurieren von MongoDB auf einem virtuellen Linux-Computer mithilfe von Azure CLI 1.0
[MongoDB](http://www.mongodb.org) ist eine beliebte, leistungsfähige Open Source-NoSQL-Datenbank. In diesem Artikel wird erläutert, wie Sie mit dem Resource Manager-Bereitstellungsmodell MongoDB auf einem virtuellen Linux-Computer in Azure installieren und konfigurieren. Hier finden Sie Beispiele mit Informationen zu den folgenden Schritten:

* [Manuelles Installieren und Konfigurieren einer einfachen MongoDB-Instanz](#manually-install-and-configure-mongodb-on-a-vm)
* [Erstellen einer einfachen MongoDB-Instanz mithilfe einer Resource Manager-Vorlage](#create-basic-mongodb-instance-on-centos-using-a-template)
* [Erstellen eines komplexen MongoDB-Shardclusters mit Replikatgruppen mithilfe einer Resource Manager-Vorlage](#create-a-complex-mongodb-sharded-cluster-on-centos-using-a-template)


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- Azure-CLI 1.0: Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](create-cli-complete-nodejs.md) Unsere CLI der nächsten Generation für das Resource Manager-Bereitstellungsmodell


## <a name="manually-install-and-configure-mongodb-on-a-vm"></a>Manuelles Installieren und Konfigurieren von MongoDB auf einem virtuellen Computer
Für MongoDB stehen [Installationsanweisungen](https://docs.mongodb.com/manual/administration/install-on-linux/) für Linux-Distributionen zur Verfügung, u.a. für Red Hat/CentOS, SUSE, Ubuntu und Debian. Im folgenden Beispiel wird eine *CentOS*-VM mit einem SSH-Schlüssel unter *~/.ssh/id_rsa.pub* erstellt. Beantworten Sie die Fragen nach Speicherkontoname, DNS-Name und Administratoranmeldeinformationen:

```azurecli
azure vm quick-create \
    --image-urn CentOS \
    --ssh-publickey-file ~/.ssh/id_rsa.pub 
```

Melden Sie sich mit der IP-Adresse, die am Ende des vorherigen Schritts zur Erstellung des virtuellen Computers angezeigt wird, beim virtuellen Computer an:

```bash
ssh azureuser@40.78.23.145
```

Erstellen Sie zum Hinzufügen der Installationsquellen für MongoDB wie folgt eine **yum**-Repositorydatei:

```bash
sudo touch /etc/yum.repos.d/mongodb-org-3.4.repo
```

Öffnen Sie die MongoDB-Repositorydatei zur Bearbeitung. Fügen Sie die folgenden Zeilen hinzu:

```sh
[mongodb-org-3.4]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.4/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.4.asc
```

Installieren Sie MongoDB wie folgt per **yum**:

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
Sie können mithilfe der folgenden Azure-Schnellstartvorlage von GitHub eine einfache MongoDB-Instanz auf einem einzelnen virtuellen CentOS-Computer erstellen. Diese Vorlage verwendet die benutzerdefinierte Skripterweiterung für Linux, um Ihrem neu erstellten virtuellen CentOS-Computer ein `yum`-Repository hinzuzufügen und MongoDB zu installieren.

* [Basic MongoDB instance on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-on-centos) (Einfache MongoDB-Instanz unter CentOS) – https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json

Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` in der Region `eastus` erstellt. Geben Sie Ihre eigenen Werte wie folgt ein:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-on-centos/azuredeploy.json
```

> [!NOTE]
> Die Azure-Befehlszeilenschnittstelle zeigt innerhalb weniger Sekunden nach der Bereitstellungserstellung wieder eine Eingabeaufforderung an, Installation und Konfiguration dauern jedoch einige Minuten. Überprüfen Sie den Status der Bereitstellung mit `azure group deployment show myResourceGroup`. Geben Sie dabei den Namen der Ressourcengruppe entsprechend ein. Warten Sie, bis für **ProvisioningState** der Status *Succeeded* (Erfolgreich) angezeigt wird, bevor Sie versuchen, eine SSH-Verbindung mit der VM herzustellen.

Stellen Sie nach Abschluss der Bereitstellung eine SSH-Verbindung mit dem virtuellen Computer her. Rufen Sie die IP-Adresse des virtuellen Computers wie im folgenden Beispiel mit dem Befehl `azure vm show` ab:

```azurecli
azure vm show --resource-group myResourceGroup --name myLinuxVM
```

Im unteren Bereich der Ausgabe wird die öffentliche IP-Adresse angezeigt. Stellen Sie unter Verwendung der IP-Adresse des virtuellen Computers eine SSH-Verbindung mit dem virtuellen Computer her:

```bash
ssh azureuser@138.91.149.74
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
Sie können mithilfe der folgenden Azure-Schnellstartvorlage von GitHub einen komplexen MongoDB-Shardcluster erstellen. Diese Vorlage basiert auf den [bewährten Methoden für MongoDB-Shardcluster](https://docs.mongodb.com/manual/core/sharded-cluster-components/), um Redundanz und hohe Verfügbarkeit zu gewährleisten. Die Vorlage erstellt zwei Shards mit drei Knoten in jeder Replikatgruppe. Darüber hinaus werden eine Konfigurationsserver-Replikatgruppe mit drei Knoten sowie zwei **mongos**-Routerserver erstellt, um Konsistenz für Anwendungen auf allen Shards zu ermöglichen.

* [MongoDB Sharding Cluster on CentOS](https://github.com/Azure/azure-quickstart-templates/tree/master/mongodb-sharding-centos) (MongoDB-Shardcluster unter CentOS) – https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json

> [!WARNING]
> Für die Bereitstellung dieses komplexen MongoDB-Shardclusters sind mehr als 20 Kerne erforderlich. Dies ist in der Regel die Standardanzahl von Kernen pro Region für ein Abonnement. Stellen Sie eine Azure-Supportanfrage zum Erhöhen der Kernanzahl.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* in der Region *eastus* erstellt. Geben Sie Ihre eigenen Werte wie folgt ein:

```azurecli
azure group create \
    --name myResourceGroup \
    --location eastus \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/mongodb-sharding-centos/azuredeploy.json
```

> [!NOTE]
> Die Azure-Befehlszeilenschnittstelle zeigt innerhalb weniger Sekunden nach der Bereitstellungserstellung wieder eine Eingabeaufforderung an, es dauert jedoch unter Umständen länger als eine Stunde, bis Installation und Konfiguration abgeschlossen sind. Überprüfen Sie den Status der Bereitstellung mit `azure group deployment show myResourceGroup`. Passen Sie dabei den Namen der Ressourcengruppe entsprechend an. Warten Sie, bis für **ProvisioningState** der Status *Succeeded* (Erfolgreich) angezeigt wird, bevor Sie eine Verbindung mit den virtuellen Computern herstellen.


## <a name="next-steps"></a>Nächste Schritte
In diesen Beispielen wird vom virtuellen Computer eine lokale Verbindung mit der MongoDB-Instanz hergestellt. Wenn Sie von einem anderen virtuellen Computer oder Netzwerk eine Verbindung mit der MongoDB-Instanz herstellen möchten, stellen Sie sicher, dass die entsprechenden [Netzwerksicherheitsgruppen-Regeln](nsg-quickstart.md) erstellt werden.

Weitere Informationen zur Erstellung mithilfe von Vorlagen finden Sie in der [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).

Die Azure Resource Manager-Vorlagen verwenden die benutzerdefinierte Skripterweiterung zum Herunterladen und Ausführen von Skripts auf Ihren virtuellen Computern. Weitere Informationen finden Sie unter [Verwenden der benutzerdefinierten Skripterweiterung von Azure mit virtuellen Linux-Computern](extensions-customscript.md).


