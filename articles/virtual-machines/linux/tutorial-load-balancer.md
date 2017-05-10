---
title: "Lastenausgleich für virtuelle Linux-Computer in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mit dem Azure Load Balancer eine hoch verfügbare und sichere Anwendung für drei virtuelle Linux-Computer erstellen."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/17/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 5ff735100132e8571871b41ac2309334662adb7f
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---

# <a name="how-to-load-balance-linux-virtual-machines-in-azure-to-create-a-highly-available-application"></a>Lastenausgleich für virtuelle Linux-Computer in Azure zum Erstellen einer hoch verfügbaren Anwendung
In diesem Tutorial erhalten Sie Informationen zu den verschiedenen Komponenten des Azure Load Balancers, mit denen der Datenverkehr verteilt und hohe Verfügbarkeit bereitgestellt wird. Um den Load Balancer in Aktion zu sehen, erstellen Sie eine Node.js-App, die auf drei virtuellen Linux-Computern (VMs) ausgeführt wird.

Die Schritte in diesem Tutorial können mit der neuesten Version von [Azure CLI 2.0](/cli/azure/install-azure-cli) ausgeführt werden.


## <a name="azure-load-balancer-overview"></a>Übersicht über den Azure Load Balancer
Ein Azure Load Balancer ist ein Load Balancer der Schicht 4 (TCP, UDP), der hohe Verfügbarkeit durch Verteilen des eingehenden Datenverkehrs auf fehlerfreie virtuelle Computer bietet. Der Integritätstest eines Load Balancers überwacht einen bestimmten Port auf jedem virtuellen Computer und verteilt Datenverkehr nur an einen betriebsbereiten virtuellen Computer.

Sie definieren eine Front-End-IP-Konfiguration, die eine oder mehrere öffentliche IP-Adressen enthält. Mit dieser Front-End-IP-Konfiguration sind der Load Balancer und Ihre Anwendungen über das Internet zugänglich. 

Virtuelle Computer werden über die zugehörige virtuelle Netzwerkschnittstelle (NIC) mit einem Load Balancer verbunden. Zum Verteilen von Datenverkehr auf die virtuellen Computer enthält ein Back-End-Adresspool die IP-Adressen der virtuellen NICs, die mit dem Load Balancer verbunden sind.

Zur Steuerung des Datenverkehrsflusses definieren Sie Load Balancer-Regeln für spezifische Ports und Protokolle, die den virtuellen Computern zugeordnet sind.

Wenn Sie die Schritte im vorherigen Tutorial zum [Erstellen einer VM-Skalierungsgruppe](tutorial-create-vmss.md) ausgeführt haben, wurde bereits ein Load Balancer erstellt. Alle diese Komponenten wurden für Sie als Teil der Skalierungsgruppe konfiguriert.


## <a name="create-azure-load-balancer"></a>Erstellen eines Azure Load Balancers
In diesem Abschnitt wird erläutert, wie Sie die einzelnen Komponenten des Load Balancers erstellen und konfigurieren können. Vor der Erstellung des Load Balancers müssen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt am Standort *westus* eine Ressourcengruppe mit dem Namen *myRGLoadBalancer*:

```azurecli
az group create --name myRGLoadBalancer --location westus
```

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse
Um über das Internet auf Ihre App zugreifen zu können, benötigen Sie eine öffentliche IP-Adresse für den Load Balancer. Erstellen Sie mit [az network public-ip create](/cli/azure/public-ip#create) eine öffentliche IP-Adresse. Das folgende Beispiel erstellt in der Ressourcengruppe *myRGLoadBalancer* eine öffentliche IP-Adresse mit dem Namen *MyPublicIP*:

```azurecli
az network public-ip create \
    --resource-group myRGLoadBalancer \
    --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers
Erstellen Sie mit [az network lb create](/cli/azure/network/lb#create) einen Load Balancer. Im folgenden Beispiel wird der Load Balancer *myLoadBalancer* erstellt und die Adresse *myPublicIP* der Front-End-IP-Konfiguration zugewiesen:

```azurecli
az network lb create \
    --resource-group myRGLoadBalancer \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests
Damit der Load Balancer den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Falls bei einem virtuellen Computer innerhalb eines Intervalls von 15 Sekunden zwei aufeinanderfolgende Fehler auftreten, wird er standardmäßig aus der Load Balancer-Verteilung entfernt. Integritätstests werden auf der Grundlage eines Protokolls oder einer spezifischen Integritätsprüfungsseite für Ihre App erstellt. 

Im folgenden Beispiel wird ein TCP-Test erstellt. Sie können auch benutzerdefinierte HTTP-Tests für differenzierte Integritätsprüfungen erstellen. Bei Verwendung eines benutzerdefinierten HTTP-Tests müssen Sie die Integritätsprüfungsseite erstellen, z.B. *healthcheck.js*. Der Test muss die HTTP-Antwort **200 OK** zurückgeben, damit der Load Balancer den Host nicht aus der Rotation entfernt.

Zum Erstellen eines TCP-Integritätstests verwenden Sie [az network lb probe create](/cli/azure/network/lb/probe#create). Im folgenden Beispiel wird ein Integritätstest mit dem Namen *myHealthProbe* erstellt:

```azurecli
az network lb probe create \
    --resource-group myRGLoadBalancer \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel
Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll. Sie definieren die Front-End-IP-Konfiguration für den eingehenden Datenverkehr und den Back-End-IP-Pool zum Empfangen des Datenverkehrs zusammen mit dem erforderlichen Quell- und Zielport. Um sicherzustellen, dass nur fehlerfreie virtuelle Computer Datenverkehr empfangen, definieren Sie zudem den zu verwendenden Integritätstest.

Erstellen Sie mit [az network lb rule create](/cli/azure/network/lb/rule#create) eine Load Balancer-Regel. Das folgende Beispiel erstellt eine Regel namens *myLoadBalancerRule*, verwendet den Integritätstest *myHealthProbe* und führt einen Lastenausgleich für den Datenverkehr an Port *80* aus:

```azurecli
az network lb rule create \
    --resource-group myRGLoadBalancer \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="configure-virtual-network"></a>Konfigurieren des virtuellen Netzwerks
Vor der Bereitstellung mehrerer virtueller Computer und dem Testen des Load Balancers müssen Sie zunächst die unterstützenden virtuellen Netzwerkressourcen erstellen. Weitere Informationen zu virtuellen Netzwerken finden Sie im Tutorial [Verwalten von virtuellen Azure-Netzwerken](tutorial-virtual-network.md).

### <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen
Erstellen Sie mit [az network vnet create](/cli/azure/vnet#create) ein virtuelles Netzwerk. Im folgenden Beispiel werden ein virtuelles Netzwerk mit dem Namen *myVnet* und einem Subnetz mit dem Namen *mySubnet* erstellt:

```azurecli
az network vnet create \
    --resource-group myRGLoadBalancer \
    --name myVnet \
    --subnet-name mySubnet
```

Zum Hinzufügen einer Netzwerksicherheitsgruppe verwenden Sie [az network nsg create](/cli/azure/network/nsg#create). Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens *myNetworkSecurityGroup* erstellt:

```azurecli
az network nsg create \
    --resource-group myRGLoadBalancer \
    --name myNetworkSecurityGroup
```

Erstellen Sie mit [az network nsg rule create](/cli/azure/network/nsg/rule#create) eine Netzwerksicherheitsgruppen-Regel. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppen-Regel namens *myNetworkSecurityGroupRule* erstellt:

```azurecli
az network nsg rule create \
    --resource-group myRGLoadBalancer \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

Virtuelle NICs werden mit [az network nic create](/cli/azure/network/nic#create) erstellt. Im folgenden Beispiel werden drei virtuelle NICs erstellt (jeweils eine virtuelle NIC pro virtuellem Computer, den Sie in den folgenden Schritten für Ihre App erstellen). Sie können jederzeit weitere virtuelle NICs und virtuelle Computer erstellen und dem Load Balancer hinzufügen:

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myRGLoadBalancer \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```

## <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern

### <a name="create-cloud-init-config"></a>Erstellen der cloud-init-Konfiguration
In einem vorherigen Tutorial zum [Anpassen eines virtuellen Linux-Computers beim ersten Start](tutorial-automate-vm-deployment.md) haben Sie erfahren, wie die Anpassung für virtuelle Computer mit cloud-init automatisiert wird. Mithilfe der gleichen cloud-init-Konfigurationsdatei können Sie NGINX installieren und eine einfache Node.js-App „Hello World“ ausführen. Erstellen Sie eine Datei namens *cloud-init.txt*, und fügen Sie die folgende Konfiguration ein:

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
  - nodejs
  - npm
write_files:
  - owner: www-data:www-data
  - path: /etc/nginx/sites-available/default
    content: |
      server {
        listen 80;
        location / {
          proxy_pass http://localhost:3000;
          proxy_http_version 1.1;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection keep-alive;
          proxy_set_header Host $host;
          proxy_cache_bypass $http_upgrade;
        }
      }
  - owner: azureuser:azureuser
  - path: /home/azureuser/myapp/index.js
    content: |
      var express = require('express')
      var app = express()
      var os = require('os');
      app.get('/', function (req, res) {
        res.send('Hello World from host ' + os.hostname() + '!')
      })
      app.listen(3000, function () {
        console.log('Hello world app listening on port 3000!')
      })
runcmd:
  - service nginx restart
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```

### <a name="create-virtual-machines"></a>Erstellen von virtuellen Computern
Fügen Sie die virtuellen Computer in eine Verfügbarkeitsgruppe ein, um die hohe Verfügbarkeit Ihrer App zu optimieren. Weitere Informationen zu Verfügbarkeitsgruppen finden Sie im vorherigen Tutorial [Erstellen von hoch verfügbaren virtuellen Computern](tutorial-availability-sets.md).

Erstellen Sie mithilfe von [az vm availability-set create](/cli/azure/vm/availability-set#create) eine Verfügbarkeitsgruppe. Im folgenden Beispiel wird eine Verfügbarkeitsgruppe namens *myAvailabilitySet* erstellt:

```azurecli
az vm availability-set create \
    --resource-group myRGLoadBalancer \
    --name myAvailabilitySet \
    --platform-fault-domain-count 3 \
    --platform-update-domain-count 2
```

Jetzt können Sie mit [az vm create](/cli/azure/vm#create) die virtuellen Computer erstellen. Im folgenden Beispiel werden drei virtuelle Computer erstellt und SSH-Schlüssel generiert, sofern sie noch nicht vorhanden sind:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myRGLoadBalancer \
        --name myVM$i \
        --availability-set myAvailabilitySet \
        --nics myNic$i \
        --image Canonical:UbuntuServer:14.04.4-LTS:latest \
        --admin-username azureuser \
        --generate-ssh-keys \
        --custom-data cloud-init.txt \
        --no-wait
done
```

Die Erstellung und Konfiguration aller drei virtuellen Computer dauert einige Minuten. Der Integritätstest des Load Balancers erkennt automatisch, wenn die App auf den einzelnen virtuellen Computern ausgeführt wird. Sobald die App ausgeführt wird, wird der Datenverkehr auf der Grundlage der Load Balancer-Regel verteilt.


## <a name="test-load-balancer"></a>Testen des Load Balancers
Rufen Sie mit [az network public-ip show](/cli/azure/network/public-ip#show) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für *myPublicIP* abgerufen, die wir zuvor erstellt haben:

```azurecli
az network public-ip show \
    --resource-group myRGLoadBalancer \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Geben Sie die öffentliche IP-Adresse in einem Webbrowser ein. Die App wird wie im folgenden Beispiel mit dem Hostnamen des virtuellen Computers angezeigt, an den der Load Balancer den Datenverkehr verteilt hat:

![Ausgeführte Node.js-App](./media/tutorial-load-balancer/running-nodejs-app.png)

Sie können eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Load Balancer den Datenverkehr auf alle drei virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird.


## <a name="add-and-remove-vms"></a>VMs hinzufügen und entfernen
Für die virtuellen Computer, auf denen Ihre App ausgeführt wird, sind unter Umständen gelegentlich Wartungsarbeiten erforderlich (etwa die Installation von Betriebssystemupdates). Zur Bewältigung eines höheren Datenverkehrsaufkommens für Ihre App müssen gegebenenfalls weitere virtuelle Computer hinzugefügt werden. In diesem Abschnitt erfahren Sie, wie Sie einen virtuellen Computer aus dem Load Balancer entfernen oder dem Load Balancer einen virtuellen Computer hinzufügen.

### <a name="remove-a-vm-from-the-load-balancer"></a>Entfernen eines virtuellen Computers aus dem Load Balancer
Mit [az network nic ip-config address-pool remove](/cli/azure/network/nic/ip-config/address-pool#remove) können Sie einen virtuellen Computer aus dem Back-End-Adresspool entfernen. Im folgenden Beispiel wird die virtuelle NIC für **myVM2** aus *myLoadBalancer* entfernt:

```azurecli
az network nic ip-config address-pool remove \
    --resource-group myRGLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

Sie können eine erzwungene Aktualisierung Ihres Webbrowsers durchführen, um zu verfolgen, wie der Load Balancer den Datenverkehr auf die beiden verbleibenden virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird. Nun können Sie Wartungsarbeiten für den virtuellen Computer durchführen und beispielsweise Betriebssystemupdates installieren oder den virtuellen Computer neu starten.

### <a name="add-a-vm-to-the-load-balancer"></a>Hinzufügen eines virtuellen Computers zum Load Balancer
Wenn Sie die Wartungsarbeiten für den virtuellen Computer abgeschlossen haben oder die Kapazität erweitern möchten, können Sie dem Back-End-Adresspool mit [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#add) einen virtuellen Computer hinzufügen. Im folgenden Beispiel wird die virtuelle NIC für **myVM2** zu *myLoadBalancer* hinzugefügt:

```azurecli
az network nic ip-config address-pool add \
    --resource-group myRGLoadBalancer \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie erfahren, wie Sie einen Load Balancer für virtuelle Computer erstellen. Im nächsten Tutorial erhalten Sie weitere Informationen zu virtuellen Azure-Netzwerkkomponenten.

[Verwalten von VM-Netzwerken](tutorial-virtual-network.md)

