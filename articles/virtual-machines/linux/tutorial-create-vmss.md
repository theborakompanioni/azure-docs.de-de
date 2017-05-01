---
title: "Erstellen einer hoch verfügbaren App in Azure mithilfe von VM-Skalierungsgruppen | Microsoft-Dokumentation"
description: "Erstellen Sie mit einer VM-Skalierungsgruppe und der Azure CLI eine hoch verfügbare Anwendung auf virtuellen Linux-Computern, und stellen Sie sie bereit."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 04/05/2017
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 4c76fb202f501f671504646395b800aeb90d8e69
ms.lasthandoff: 04/07/2017

---

# <a name="create-a-highly-available-application-on-linux-with-virtual-machine-scale-sets"></a>Erstellen einer hoch verfügbaren Anwendung unter Linux mit VM-Skalierungsgruppen
In diesem Tutorial erfahren Sie, wie Sie eine hoch verfügbare Anwendung in einer VM-Skalierungsgruppe erstellen. Sie erfahren außerdem, wie Sie die Konfiguration der virtuellen Computer in der Skalierungsgruppe automatisieren können. 


## <a name="step-1---create-a-resource-group"></a>Schritt 1: Erstellen einer Ressourcengruppe
Für dieses Tutorial muss die aktuelle Version von [Azure CLI 2.0](/cli/azure/install-azure-cli) installiert sein. Falls Sie noch nicht bei Ihrem Azure-Abonnement angemeldet sind, melden Sie sich mit [az login](/cli/azure/#login) an, und befolgen Sie die Anweisungen auf dem Bildschirm.

Erstellen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen `myResourceGroupVMSS` am Standort `westus`:

```azurecli
az group create --name myResourceGroupVMSS --location westus
```


## <a name="step-2---define-your-app"></a>Schritt 2: Definieren der App
Verwenden Sie die **cloud-init**-Konfiguration aus dem Tutorial, in dem Sie eine hoch verfügbare App mit Lastenausgleich erstellt haben. Weitere Informationen zur Verwendung von **cloud-init** finden Sie unter [Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

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


## <a name="step-3---create-scale-set"></a>Schritt 3: Erstellen der Skalierungsgruppe
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. In Skalierungsgruppen werden die gleichen Komponenten verwendet, die Sie im Tutorial [Erstellen einer hoch verfügbaren App in Azure](tutorial-load-balance-nodejs.md) kennen gelernt haben. Zu diesen Komponenten gehören Verfügbarkeitsgruppen, Fehler- und Updatedomänen sowie Lastenausgleichsmodule.

Mit einer Skalierungsgruppe werden diese Ressourcen für Sie erstellt und verwaltet. Die Anzahl von virtuellen Computern in Ihrer Skalierungsgruppe kann basierend auf definierten Regeln automatisch erhöht oder verringert werden. Sie können [ein benutzerdefiniertes Image](capture-image.md) als Quelle für den virtuellen Computer verwenden oder die virtuellen Computer wie in diesem Tutorial während der Bereitstellung mit **cloud-init** konfigurieren.

Erstellen Sie mit [az vmss create](/cli/azure/vmss#create) eine VM-Skalierungsgruppe. Im folgenden Beispiel wird eine Skalierungsgruppe namens `myScaleSet` erstellt:

```azurecli
az vmss create \
  --resource-group myResourceGroupVMSS \
  --name myScaleSet \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys      
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe dauert einige Minuten.


## <a name="step-4---configure-firewall"></a>Schritt 4: Konfigurieren der Firewall
Ein Lastenausgleichsmodul wurde automatisch als Teil der VM-Skalierungsgruppe erstellt. Das Lastenausgleichsmodul verteilt den Datenverkehr auf der Grundlage von Lastenausgleichsregeln auf eine Gruppe definierter virtueller Computer. Damit Datenverkehr die Web-App erreicht, erstellen Sie mit [az network lb probe create](/cli/azure/network/lb/probe#create) eine Regel. Im folgenden Beispiel wird eine Regel namens `myLoadBalancerRuleWeb` erstellt:

```azurecli
az network lb rule create \
  --resource-group myResourceGroupVMSS \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="step-5---test-your-app"></a>Schritt 5: Testen der App
Rufen Sie mit [az network public-ip show](/cli/azure/network/public-ip#show) die öffentliche IP-Adresse Ihres Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für `myScaleSetLBPublicIP` abgerufen, die als Teil der Skalierungsgruppe erstellt wurde:

```azurecli
az network public-ip show \
    --resource-group myResourceGroupVMSS \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Geben Sie die öffentliche IP-Adresse in einen Webbrowser ein. Die App wird mit dem Hostnamen des virtuellen Computers angezeigt, an den der Load Balancer den Datenverkehr verteilt hat:

![Ausgeführte Node.js-App](./media/tutorial-load-balance-nodejs/running-nodejs-app.png)

Führen Sie eine erzwungene Aktualisierung Ihres Webbrowsers durch, um zu verfolgen, wie das Lastenausgleichsmodul den Datenverkehr auf alle virtuellen Computer in der Skalierungsgruppe verteilt, auf denen Ihre App ausgeführt wird.


## <a name="step-6---management-tasks"></a>Schritt 6: Verwaltungsaufgaben
Während des Lebenszyklus der Skalierungsgruppe müssen Sie möglicherweise eine oder mehrere Verwaltungsaufgaben ausführen. Darüber hinaus empfiehlt es sich, Skripts zum Automatisieren von verschiedenen Aufgaben im Lebenszyklus zu erstellen. Azure CLI 2.0 bietet eine schnelle Möglichkeit, um diese Aufgaben auszuführen. Im Folgenden sind einige allgemeine Aufgaben aufgeführt.

### <a name="increase-or-decrease-vm-instances"></a>VM-Instanzen erhöhen oder verringern
Sie können die Anzahl der virtuellen Computer in der Skalierungsgruppe mit [az vmss scale](/cli/azure/vmss#scale) manuell erhöhen oder verringern. Im folgenden Beispiel wird die Anzahl von virtuellen Computern in Ihrer Skalierungsgruppe auf `5` erhöht:

```azurecli
az vmss scale --resource-group myResourceGroupVMSS --name myScaleSet --new-capacity 5
```

Mit Regeln zur automatischen Skalierung können Sie definieren, wie die Anzahl von virtuellen Computern in Ihrer Skalierungsgruppe hoch- oder herunterskaliert werden soll, um auf den Bedarf z.B. durch Netzwerkdatenverkehr oder CPU-Auslastung zu reagieren. Derzeit können diese Regeln nicht in Azure CLI 2.0 festgelegt werden. Verwenden Sie das [Azure-Portal](https://portal.azure.com) zum Konfigurieren der automatischen Skalierung.

### <a name="get-connection-info"></a>Verbindungsinformationen abrufen
Verwenden Sie zum Abrufen der Verbindungsinformationen für die virtuellen Computer in Ihren Skalierungsgruppen [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info). Durch diesen Befehl werden die öffentliche IP-Adresse und die Ports für alle virtuellen Computer ausgegeben, die eine Verbindung mit SSH ermöglichen:

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroupVMSS --name myScaleSet
```

### <a name="delete-resource-group"></a>Ressourcengruppe löschen
Beim Löschen einer Ressourcengruppe werden auch alle darin enthaltenen Ressourcen gelöscht.

```azurecli
az group delete --name myResourceGroupVMSS
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben wir mit **cloud-init** eine Web-App definiert und alle virtuellen Computer während der Bereitstellung konfiguriert. Informationen zum Erfassen eines virtuellen Computers, um ihn als Quellimage in Ihrer Skalierungsgruppe zu verwenden, finden Sie unter [Generalisieren und Erfassen eines virtuellen Linux-Computers](capture-image.md).

Weitere Informationen zu einigen der Features von VM-Skalierungsgruppen aus diesem Tutorial finden Sie in den folgenden Themen:

- [Übersicht über Azure-VM-Skalierungsgruppen](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [Übersicht über Azure Load Balancer](../../load-balancer/load-balancer-overview.md)
- [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../../virtual-network/virtual-networks-nsg.md)
