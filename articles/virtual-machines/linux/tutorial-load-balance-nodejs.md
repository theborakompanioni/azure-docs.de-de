---
title: "Tutorial: Erstellen einer hochverfügbaren Anwendung auf virtuellen Azure-Computern | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie eine hochverfügbare und sichere Anwendung für drei virtuelle Linux-Computer mit einem Load Balancer erstellen."
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
ms.date: 03/27/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 65ff0e03af6b43478f645274adfeb46d28418894
ms.lasthandoff: 04/07/2017

---

# <a name="build-a-load-balanced-highly-available-application-on-linux-virtual-machines-in-azure"></a>Erstellen einer hochverfügbaren Anwendung mit Load Balancer auf virtuellen Linux-Computern in Azure
In diesem Tutorial erstellen Sie eine hoch verfügbare, wartungsresistente Anwendung. Die App verwendet einen Load Balancer, eine Verfügbarkeitsgruppe und drei virtuelle-Linux-Computer (Virtual Machines, VMs). In diesem Tutorial wird zwar eine Node.js-App erstellt, Sie können das Tutorial aber auch zum Bereitstellen eines anderen Anwendungsframeworks mit den gleichen Hochverfügbarkeitskomponenten und Richtlinien verwenden.

## <a name="step-1---azure-prerequisites"></a>Schritt 1: Azure-Voraussetzungen
Öffnen Sie für dieses Tutorial ein Terminalfenster, und vergewissern Sie sich, dass die neueste Version von [Azure CLI 2.0](/cli/azure/install-azure-cli) installiert ist. Falls Sie noch nicht bei Ihrem Azure-Abonnement angemeldet sind, melden Sie sich mit [az login](/cli/azure/#login) an, und befolgen Sie die Anweisungen auf dem Bildschirm:

```azurecli
az login
```

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Vor der Erstellung anderer Azure-Ressourcen müssen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroup` am Standort `westus`:

```azurecli
az group create --name myResourceGroup --location westus
```


## <a name="step-2---create-availability-set"></a>Schritt 2: Erstellen der Verfügbarkeitsgruppe
Virtuelle Computer können übergreifend für logische Fehler- und Updatedomänen erstellt werden. Jede logische Domäne stellt einen Teil der Hardware im zugrunde liegenden Azure-Datencenter dar. Wenn Sie mehrere virtuelle Computer erstellen, werden Ihre Compute- und Speicherressourcen auf diese Domänen verteilt. Diese Verteilung gewährleistet die Verfügbarkeit Ihrer App, wenn eine der Hardwarekomponenten gewartet wird. Diese logischen Fehler- und Updatedomänen können mithilfe von Verfügbarkeitsgruppen definiert werden.

Erstellen Sie mithilfe von [az vm availability-set create](/cli/azure/vm/availability-set#create) eine Verfügbarkeitsgruppe. Im folgenden Beispiel wird eine Verfügbarkeitsgruppe namens `myAvailabilitySet` erstellt:

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 3 \
    --platform-update-domain-count 2
```


## <a name="step-3---create-load-balancer"></a>Schritt 3: Erstellen des Load Balancers
Ein Azure Load Balancer verteilt Datenverkehr auf der Grundlage von Load Balancer-Regeln auf eine Gruppe definierter virtueller Computer. Ein Integritätstest überwacht einen bestimmten Port auf jedem virtuellen Computer und verteilt Datenverkehr nur an einen betriebsbereiten virtuellen Computer.

### <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse
Um über das Internet auf Ihre App zugreifen zu können, müssen Sie dem Load Balancer eine öffentliche IP-Adresse zuweisen. Erstellen Sie mit [az network public-ip create](/cli/azure/public-ip#create) eine öffentliche IP-Adresse. Im folgenden Beispiel wird eine öffentliche IP-Adresse namens `myPublicIP` erstellt:

```azurecli
az network public-ip create --resource-group myResourceGroup --name myPublicIP
```

### <a name="create-a-load-balancer"></a>Einrichten eines Load Balancers
Erstellen Sie mit [az network lb create](/cli/azure/network/lb#create) einen Load Balancer. Im folgenden Beispiel wird unter Verwendung der `myPublicIP`-Adresse ein Load Balancer namens `myLoadBalancer` erstellt:

```azurecli
az network lb create \
    --resource-group myResourceGroup \
    --name myLoadBalancer \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --public-ip-address myPublicIP
```

### <a name="create-a-health-probe"></a>Erstellen eines Integritätstests
Damit der Load Balancer den Status Ihrer App überwachen kann, verwenden Sie einen Integritätstest. Abhängig von der Reaktion auf Integritätsüberprüfungen werden der Load Balancer-Rotation durch den Integritätstest dynamisch virtuelle Computer hinzugefügt oder daraus entfernt. Falls bei einem virtuellen Computer innerhalb eines Intervalls von 15 Sekunden zwei aufeinanderfolgende Fehler auftreten, wird er standardmäßig aus der Load Balancer-Verteilung entfernt. Integritätstests werden auf der Grundlage eines Protokolls oder einer spezifischen Integritätsprüfungsseite für Ihre App erstellt. Im folgenden Beispiel wird ein TCP-Test erstellt. Sie können dieses Beispiel allerdings auch erweitern und beispielsweise `--path healthcheck.js` hinzufügen. `healthcheck.js` muss erstellt werden und die Antwort **HTTP 200 OK** zurückgeben, damit der Load Balancer den Host nicht aus der Rotation entfernt.

Erstellen Sie mit [az network lb probe create](/cli/azure/network/lb/probe#create) den Integritätstest. Im folgenden Beispiel wird ein benannter Integritätstest namens `myHealthProbe` erstellt, der die einzelnen virtuellen Computer überwacht:

```azurecli
az network lb probe create \
    --resource-group myResourceGroup \
    --lb-name myLoadBalancer \
    --name myHealthProbe \
    --protocol tcp \
    --port 80
```

### <a name="create-a-load-balancer-rule"></a>Erstellen einer Load Balancer-Regel
Mithilfe einer Load Balancer-Regel wird definiert, wie Datenverkehr auf die virtuellen Computer verteilt werden soll.

Erstellen Sie mit [az network lb rule create](/cli/azure/network/lb/rule#create) eine Load Balancer-Regel. Das folgende Beispiel erstellt eine Regel namens `myLoadBalancerRule`, verwendet den Integritätstest `myHealthProbe` und führt einen Lastenausgleich für den Datenverkehr an Port `80` aus:

```azurecli
az network lb rule create \
    --resource-group myResourceGroup \
    --lb-name myLoadBalancer \
    --name myLoadBalancerRule \
    --protocol tcp \
    --frontend-port 80 \
    --backend-port 80 \
    --frontend-ip-name myFrontEndPool \
    --backend-pool-name myBackEndPool \
    --probe-name myHealthProbe
```


## <a name="step-4---configure-networking"></a>Schritt 4: Konfigurieren des Netzwerks
Jeder virtuelle Computer verfügt über mindestens eine virtuelle Netzwerkschnittstellenkarte (Network Interface Card, NIC), die eine Verbindung mit einem virtuellen Netzwerk herstellt. Dieses virtuelle Netzwerk ist geschützt, um Datenverkehr auf der Grundlage definierter Zugriffsregeln zu filtern.

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie mit [az network vnet create](/cli/azure/vnet#create) ein virtuelles Netzwerk, um Netzwerkkonnektivität für Ihre virtuellen Computer bereitzustellen. Im folgenden Beispiel wird ein virtuelles Netzwerk namens `myVnet` mit einem Subnetz namens `mySubnet` erstellt:

```azurecli
az network vnet create --resource-group myResourceGroup --name myVnet --subnet-name mySubnet
```

### <a name="configure-network-security"></a>Konfigurieren der Netzwerksicherheit
Netzwerksicherheitsgruppen verwenden Regeln, um den Datenverkehr für virtuelle Computer und von virtuellen Computern zu steuern. Diese Regeln werden von Ihnen definiert – etwa, um Datenverkehr an Port 80 zuzulassen. Während der Load Balancer den Datenverkehr auf die virtuellen Computer verteilt, sorgen die Netzwerksicherheitsgruppen-Regeln dafür, dass nur erlaubter Datenverkehr zugelassen wird.

Erstellen Sie mit [az network nsg create](/cli/azure/network/nsg#create) eine Netzwerksicherheitsgruppe. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppe namens `myNetworkSecurityGroup` erstellt:

```azurecli
az network nsg create --resource-group myResourceGroup --name myNetworkSecurityGroup
```

Erstellen Sie mit [az network nsg rule create](/cli/azure/network/nsg/rule#create) eine Netzwerksicherheitsgruppen-Regel, damit Ihre App für Webdatenverkehr erreichbar ist. Im folgenden Beispiel wird eine Netzwerksicherheitsgruppen-Regel namens `myNetworkSecurityGroupRule` erstellt:

```azurecli
az network nsg rule create \
    --resource-group myResourceGroup \
    --nsg-name myNetworkSecurityGroup \
    --name myNetworkSecurityGroupRule \
    --priority 1001 \
    --protocol tcp \
    --destination-port-range 80
```

### <a name="create-virtual-network-interface-cards"></a>Erstellen virtueller Netzwerkschnittstellenkarten 
Load Balancer arbeiten mit der virtuellen NIC-Ressource und nicht mit dem eigentlichen virtuellen Computer. Die virtuelle NIC wird mit dem Load Balancer verbunden und anschließend an einen virtuellen Computer angefügt.

Erstellen Sie mit [az network nic create](/cli/azure/network/nic#create) eine virtuelle NIC. Im folgenden Beispiel werden drei virtuelle NICs erstellt (jeweils eine virtuelle NIC pro virtuellem Computer, den Sie in den folgenden Schritten für Ihre App erstellen):

```bash
for i in `seq 1 3`; do
    az network nic create \
        --resource-group myResourceGroup \
        --name myNic$i \
        --vnet-name myVnet \
        --subnet mySubnet \
        --network-security-group myNetworkSecurityGroup \
        --lb-name myLoadBalancer \
        --lb-address-pools myBackEndPool
done
```


## <a name="step-5---build-your-app"></a>Schritt 5: Erstellen Ihrer App
**cloud-init** ist eine gängige Methode zum Anpassen eines virtuellen Computers. Mit **cloud-init** können Sie Pakete installieren und Dateien schreiben. Da **cloud-init** während der ersten Bereitstellung ausgeführt wird, sind keine weiteren Schritte erforderlich, um Ihre App einsatzbereit zu machen. Der Load Balancer beginnt mit der Verteilung des Datenverkehrs, sobald der virtuelle Computer bereitgestellt wurde und die App ausgeführt wird. Weitere Informationen zur Verwendung von **cloud-init** finden Sie unter [Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Die folgende **cloud-init**-Konfiguration installiert **nodejs** und **npm** und installiert und konfiguriert **nginx** als Webproxy für Ihre App. Darüber hinaus erstellt die Konfiguration eine einfache Node.js-App namens „Hello World“ und initialisiert und startet sie anschließend mit **Express**. Wenn Sie ein anderes Anwendungsframework verwenden möchten, passen Sie die Pakete und die bereitgestellte Anwendung entsprechend an.

Erstellen Sie eine Datei namens `cloud-init.txt`, und fügen Sie die folgende Konfiguration ein:

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
  - nginx -s reload
  - cd "/home/azureuser/myapp"
  - npm init
  - npm install express -y
  - nodejs index.js
```


## <a name="step-6---create-virtual-machines"></a>Schritt 6: Erstellen virtueller Computer
Nachdem alle zugrunde liegenden Komponenten eingerichtet sind, können Sie nun hochverfügbare virtuelle Computer zur Ausführung Ihrer App erstellen.

### <a name="create-vms"></a>Virtuelle Computer erstellen
Erstellen Sie mit [az vm create](/cli/azure/vm#create) die virtuellen Computer. Im folgenden Beispiel werden drei virtuelle Computer erstellt und SSH-Schlüssel generiert, sofern sie noch nicht vorhanden sind:

```bash
for i in `seq 1 3`; do
    az vm create \
        --resource-group myResourceGroup \
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

### <a name="test-your-app"></a>Testen Ihrer App
Rufen Sie mit [az network public-ip show](/cli/azure/network/public-ip#show) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für `myPublicIP` abgerufen, die wir zuvor erstellt haben:

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name myPublicIP \
    --query [ipAddress] \
    --output tsv
```

Geben Sie die öffentliche IP-Adresse in einen Webbrowser ein. Die App wird mit dem Hostnamen des virtuellen Computers angezeigt, an den der Load Balancer den Datenverkehr verteilt hat:

![Ausgeführte Node.js-App](./media/tutorial-load-balance-nodejs/running-nodejs-app.png)

Führen Sie eine erzwungene Aktualisierung Ihres Webbrowsers durch, um zu verfolgen, wie der Load Balancer den Datenverkehr auf alle drei virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird.


## <a name="step-7---management-tasks"></a>Schritt 7: Verwaltungsaufgaben
Für die virtuellen Computer, auf denen Ihre App ausgeführt wird, sind unter Umständen gelegentlich Wartungsarbeiten erforderlich (etwa die Installation von Betriebssystemupdates). Zur Bewältigung eines höheren Datenverkehrsaufkommens für Ihre App müssen gegebenenfalls weitere virtuelle Computer hinzugefügt werden. In diesem Abschnitt erfahren Sie, wie Sie einen virtuellen Computer aus dem Load Balancer entfernen oder dem Load Balancer einen virtuellen Computer hinzufügen.

### <a name="remove-a-vm-from-the-load-balancer"></a>Entfernen eines virtuellen Computers aus dem Load Balancer
Verwenden Sie [az network nic ip-config address-pool remove](/cli/azure/network/nic/ip-config/address-pool#remove), um einen virtuellen Computer aus dem Back-End-Adresspool zu entfernen. Im folgenden Beispiel wird die virtuelle NIC für **myVM2** aus `myLoadBalancer` entfernt:

```azurecli
az network nic ip-config address-pool remove \
    --resource-group myResourceGroup \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool 
```

Führen Sie eine erzwungene Aktualisierung Ihres Webbrowsers durch, um zu verfolgen, wie der Load Balancer den Datenverkehr auf die beiden verbleibenden virtuellen Computer verteilt, auf denen Ihre App ausgeführt wird. Nun können Sie Wartungsarbeiten für den virtuellen Computer durchführen und beispielsweise Betriebssystemupdates installieren oder den virtuellen Computer neu starten.

### <a name="add-a-vm-to-the-load-balancer"></a>Hinzufügen eines virtuellen Computers zum Load Balancer
Wenn Sie die Wartungsarbeiten für den virtuellen Computer abgeschlossen haben oder die Kapazität erweitern möchten, können Sie dem Back-End-Adresspool mit [az network nic ip-config address-pool add](/cli/azure/network/nic/ip-config/address-pool#add) einen virtuellen Computer hinzufügen. Im folgenden Beispiel wird die virtuelle NIC für **myVM2** zu `myLoadBalancer` hinzugefügt:

```azurecli
az network nic ip-config address-pool add \
    --resource-group myResourceGroup \
    --nic-name myNic2 \
    --ip-config-name ipConfig1 \
    --lb-name myLoadBalancer \
    --address-pool myBackEndPool
```


## <a name="next-steps"></a>Nächste Schritte
Dieses Lernprogramm baut auf einer hochverfügbaren Anwendungsstruktur auf, die die einzelnen Azure-Ressourcen nutzt. Sie können auch VM-Skalierungsgruppen verwenden, um die Anzahl der virtuellen Computer, auf denen Ihre App ausgeführt wird, automatisch zentral hochzuskalieren oder zentral herunterzuskalieren. Fahren Sie mit dem nächsten Lernprogramm fort: [Erstellen einer hochverfügbaren Anwendung unter Linux mit VM-Skalierungsgruppen](tutorial-convert-to-vmss.md).

Weitere Informationen zu einigen der Hochverfügbarkeitsfeatures aus diesem Tutorial finden Sie in den folgenden Themen:

- [Verwalten der Verfügbarkeit virtueller Linux-Computer](../windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Übersicht über Azure Load Balancer](../../load-balancer/load-balancer-overview.md)
- [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../../virtual-network/virtual-networks-nsg.md)
- [Azure CLI-Beispiele für Linux-VMs](../windows/cli-samples.md)
