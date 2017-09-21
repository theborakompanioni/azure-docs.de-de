---
title: "Erstellen einer VM-Skalierungsgruppe für Linux in Azure | Microsoft-Dokumentation"
description: "Erstellen und Bereitstellen einer hoch verfügbaren Anwendung auf virtuellen Linux-Computern mithilfe einer VM-Skalierungsgruppe"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 09/08/2017
ms.author: iainfou
ms.translationtype: HT
ms.sourcegitcommit: fda37c1cb0b66a8adb989473f627405ede36ab76
ms.openlocfilehash: 1f54bb04023ad61f4eae51389c6a902a029e9399
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---

# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-linux"></a>Erstellen einer VM-Skalierungsgruppe und Bereitstellen einer hoch verfügbaren App unter Linux
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Sie können die Anzahl der virtuellen Computer in der Skalierungsgruppe manuell skalieren oder basierend auf der Ressourcennutzung gemäß CPU-Auslastung, Speicherbedarf oder Netzwerkdatenverkehr Regeln für die automatische Skalierung definieren. In diesem Tutorial stellen Sie eine Skalierungsgruppe für virtuelle Computer bereit. Folgendes wird vermittelt:

> [!div class="checklist"]
> * Verwenden von cloud-init zum Erstellen einer zu skalierenden Anwendung
> * Erstellen einer Skalierungsgruppe für virtuelle Computer
> * Erhöhen oder Verringern der Anzahl der Instanzen in einer Skalierungsgruppe
> * Erstellen von Regeln zur automatischen Skalierung
> * Anzeigen von Verbindungsinformationen für die Skalierungsgruppeninstanzen
> * Verwenden von Datenträgern mit Skalierungsgruppen


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Tutorial die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="scale-set-overview"></a>Übersicht über Skalierungsgruppen
Mit einer VM-Skalierungsgruppe können Sie eine Gruppe identischer, automatisch skalierender virtueller Computer bereitstellen und verwalten. Virtuelle Computer in einer Skalierungsgruppe werden in einer oder mehreren *Platzierungsgruppen* auf logische Fehler- und Updatedomänen verteilt. Hierbei handelt es sich um Gruppen ähnlich konfigurierter virtueller Computer, vergleichbar mit [Verfügbarkeitsgruppen](tutorial-availability-sets.md).

Virtuelle Computer werden nach Bedarf in einer Skalierungsgruppe erstellt. Sie können Regeln für die automatische Skalierung definieren, um zu steuern, wie und wann virtuelle Computer in der Skalierungsgruppe hinzugefügt oder entfernt werden. Diese Regeln können basierend auf Metriken wie CPU-Auslastung, Speicherauslastung oder Netzwerkdatenverkehr ausgelöst werden.

Bei Verwendung eines Azure-Plattformimages unterstützen Skalierungsgruppen bis zu 1.000 virtuelle Computer. Für Workloads mit beträchtlichen Installations- oder VM-Anpassungsanforderungen können Sie nach Wunsch ein [benutzerdefiniertes VM-Image erstellen](tutorial-custom-images.md). Bei Verwendung eines benutzerdefinierten Images können Sie bis zu 300 virtuelle Computer in einer Skalierungsgruppe erstellen.


## <a name="create-an-app-to-scale"></a>Erstellen einer App für die Skalierung
Für die Verwendung in einer Produktionsumgebung können Sie [ein benutzerdefiniertes VM-Image erstellen](tutorial-custom-images.md), das Ihre installierte und konfigurierte Anwendung umfasst. In diesem Tutorial werden die virtuellen Computer beim ersten Start angepasst, sodass eine Skalierungsgruppe schnell in Aktion zu sehen ist.

In einem vorherigen Tutorial haben Sie erfahren, wie [ein virtueller Linux-Computer beim ersten Start mit cloud-init angepasst wird](tutorial-automate-vm-deployment.md). Mithilfe der gleichen cloud-init-Konfigurationsdatei können Sie NGINX installieren und eine einfache Node.js-App „Hello World“ ausführen. 

Erstellen Sie in der aktuellen Shell eine Datei namens *cloud-init.txt*, und fügen Sie die folgende Konfiguration ein: Erstellen Sie die Datei beispielsweise in Cloud Shell, nicht auf dem lokalen Computer. Geben Sie `sensible-editor cloud-init.txt` ein, um die Datei zu erstellen und eine Liste der verfügbaren Editoren anzuzeigen. Stellen Sie sicher, dass die gesamte Datei „cloud-init“ ordnungsgemäß kopiert wird, insbesondere die erste Zeile:

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


## <a name="create-a-scale-set"></a>Erstellen einer Skalierungsgruppe
Vor der Erstellung einer Skalierungsgruppe müssen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt am Standort *eastus* eine Ressourcengruppe mit dem Namen *myResourceGroupScaleSet*:

```azurecli-interactive 
az group create --name myResourceGroupScaleSet --location eastus
```

Erstellen Sie dann mit [az vmss create](/cli/azure/vmss#create) eine VM-Skalierungsgruppe. Im folgenden Beispiel werden eine Skalierungsgruppe mit dem Namen *myScaleSet* erstellt, der virtuelle Computer mithilfe der cloud-init-Datei angepasst und (sofern noch nicht vorhanden) SSH-Schlüssel generiert:

```azurecli-interactive 
az vmss create \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --image UbuntuLTS \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.txt \
  --admin-username azureuser \
  --generate-ssh-keys
```

Die Erstellung und Konfiguration aller Ressourcen und virtuellen Computer der Skalierungsgruppe dauert einige Minuten. Es gibt Hintergrundaufgaben, die weiterhin ausgeführt werden, wenn Ihnen von der Azure CLI wieder eine Eingabeaufforderung angezeigt wird. Unter Umständen dauert es einige Minuten, bis Sie auf die App zugreifen können.


## <a name="allow-web-traffic"></a>Zulassen von Webdatenverkehr
Ein Lastenausgleichsmodul wurde automatisch als Teil der VM-Skalierungsgruppe erstellt. Das Lastenausgleichsmodul verteilt den Datenverkehr auf der Grundlage von Lastenausgleichsregeln auf eine Gruppe definierter virtueller Computer. Weitere Informationen zu den Konzepten und der Konfiguration des Load Balancers finden Sie im nächsten Tutorial [Lastenausgleich für virtuelle Computer in Azure](tutorial-load-balancer.md).

Damit Datenverkehr die Web-App erreicht, erstellen Sie mit [az network lb rule create](/cli/azure/network/lb/rule#create) eine Regel. Im folgenden Beispiel wird eine Regel namens *myLoadBalancerRuleWeb* erstellt:

```azurecli-interactive 
az network lb rule create \
  --resource-group myResourceGroupScaleSet \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```

## <a name="test-your-app"></a>Testen Ihrer App
Um die Node.js-App im Web anzuzeigen, rufen Sie mit [az network public-ip show](/cli/azure/network/public-ip#show) die öffentliche IP-Adresse des Load Balancers ab. Im folgenden Beispiel wird die IP-Adresse für *myScaleSetLBPublicIP* abgerufen, die als Teil der Skalierungsgruppe erstellt wurde:

```azurecli-interactive 
az network public-ip show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetLBPublicIP \
    --query [ipAddress] \
    --output tsv
```

Geben Sie die öffentliche IP-Adresse in einen Webbrowser ein. Die App wird mit dem Hostnamen des virtuellen Computers angezeigt, an den der Load Balancer den Datenverkehr verteilt hat:

![Ausgeführte Node.js-App](./media/tutorial-create-vmss/running-nodejs-app.png)

Um die Skalierungsgruppe in Aktion zu sehen, führen Sie eine erzwungene Aktualisierung Ihres Webbrowsers durch, um zu verfolgen, wie der Load Balancer den Datenverkehr auf alle virtuellen Computer in der Skalierungsgruppe verteilt, auf denen Ihre App ausgeführt wird.


## <a name="management-tasks"></a>Verwaltungsaufgaben
Während des Lebenszyklus der Skalierungsgruppe müssen Sie möglicherweise eine oder mehrere Verwaltungsaufgaben ausführen. Darüber hinaus empfiehlt es sich, Skripts zum Automatisieren von verschiedenen Aufgaben im Lebenszyklus zu erstellen. Azure CLI 2.0 bietet eine schnelle Möglichkeit, um diese Aufgaben auszuführen. Im Folgenden sind einige allgemeine Aufgaben aufgeführt.

### <a name="view-vms-in-a-scale-set"></a>Anzeigen von virtuellen Computern in einer Skalierungsgruppe
Verwenden Sie [az vmss list-instances](/cli/azure/vmss#list-instances) wie folgt, um eine Liste der in der Skalierungsgruppe ausgeführten virtuellen Computer anzuzeigen:

```azurecli-interactive 
az vmss list-instances \
  --resource-group myResourceGroupScaleSet \
  --name myScaleSet \
  --output table
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```azurecli-interactive 
  InstanceId  LatestModelApplied    Location    Name          ProvisioningState    ResourceGroup            VmId
------------  --------------------  ----------  ------------  -------------------  -----------------------  ------------------------------------
           1  True                  eastus      myScaleSet_1  Succeeded            MYRESOURCEGROUPSCALESET  c72ddc34-6c41-4a53-b89e-dd24f27b30ab
           3  True                  eastus      myScaleSet_3  Succeeded            MYRESOURCEGROUPSCALESET  44266022-65c3-49c5-92dd-88ffa64f95da
```


### <a name="increase-or-decrease-vm-instances"></a>VM-Instanzen erhöhen oder verringern
Verwenden Sie [az vmss show](/cli/azure/vmss#show), und führen Sie eine Abfrage nach *sku.capacity* durch, um die Anzahl der zurzeit in einer Skalierungsgruppe vorhandenen Instanzen anzuzeigen:

```azurecli-interactive 
az vmss show \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

Sie können dann die Anzahl der virtuellen Computer in der Skalierungsgruppe mit [az vmss scale](/cli/azure/vmss#scale) manuell erhöhen oder verringern. Im folgenden Beispiel wird die Anzahl der virtuellen Computer in der Skalierungsgruppe auf *5* festgelegt:

```azurecli-interactive 
az vmss scale \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --new-capacity 5
```


### <a name="configure-autoscale-rules"></a>Konfigurieren von Regeln zur automatischen Skalierung
Anstatt die Anzahl von Instanzen in Ihrer Skalierungsgruppe manuell zu skalieren, können Sie Regeln zur automatischen Skalierung definieren. Diese Regeln überwachen die Instanzen in Ihrer Skalierungsgruppe und reagieren entsprechend basierend auf von Ihnen festgelegten Metriken und Schwellenwerten. Im folgenden Beispiel wird die Anzahl der Instanzen um eine Instanz horizontal hochskaliert, wenn die durchschnittliche CPU-Auslastung über einen Zeitraum von 5 Minuten höher als 60 % ist. Wenn die durchschnittliche CPU-Auslastung über einen Zeitraum von 5 Minuten unter 30 % fällt, werden die Instanzen um eine Instanz horizontal herunterskaliert. Ihre Abonnement-ID wird verwendet, um die Ressourcen-URIs für die verschiedenen Skalierungsgruppenkomponenten zu erstellen. Um diese Regeln mit [az monitor autoscale-settings create](/cli/azure/monitor/autoscale-settings#create) zu erstellen, kopieren Sie das folgende Profil des Befehls der automatischen Skalierung, und fügen Sie es ein:

```azurecli-interactive 
sub=$(az account show --query id -o tsv)

az monitor autoscale-settings create \
    --resource-group myResourceGroupScaleSet \
    --name autoscale \
    --parameters '{"autoscale_setting_resource_name": "autoscale",
      "enabled": true,
      "location": "East US",
      "notifications": [],
      "profiles": [
        {
          "name": "Auto created scale condition",
          "capacity": {
            "minimum": "2",
            "maximum": "10",
            "default": "2"
          },
          "rules": [
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "GreaterThan",
                "threshold": 70
              },
              "scaleAction": {
                "direction": "Increase",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            },
            {
              "metricTrigger": {
                "metricName": "Percentage CPU",
                "metricNamespace": "",
                "metricResourceUri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet",
                "metricResourceLocation": "eastus",
                "timeGrain": "PT1M",
                "statistic": "Average",
                "timeWindow": "PT5M",
                "timeAggregation": "Average",
                "operator": "LessThan",
                "threshold": 30
              },
              "scaleAction": {
                "direction": "Decrease",
                "type": "ChangeCount",
                "value": "1",
                "cooldown": "PT5M"
              }
            }
          ]
        }
      ],
      "tags": {},
      "target_resource_uri": "/subscriptions/'$sub'/resourceGroups/myResourceGroupScaleSet/providers/Microsoft.Compute/virtualMachineScaleSets/myScaleSet"
    }'
```

Wenn Sie das Profil der automatischen Skalierung wiederverwenden möchten, können Sie eine JSON-Datei (JavaScript Object Notation) erstellen und sie dem `az monitor autoscale-settings create`-Befehl mit dem `--parameters @autoscale.json`-Parameter übergeben. Weitere Entwurfsinformationen zur Verwendung der automatischen Skalierung finden Sie unter [Autoscaling](/azure/architecture/best-practices/auto-scaling) (Automatische Skalierung).


### <a name="get-connection-info"></a>Verbindungsinformationen abrufen
Verwenden Sie zum Abrufen der Verbindungsinformationen für die virtuellen Computer in Ihren Skalierungsgruppen [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info). Durch diesen Befehl werden die öffentliche IP-Adresse und der Port für alle virtuellen Computer ausgegeben, die eine Verbindung mit SSH ermöglichen:

```azurecli-interactive 
az vmss list-instance-connection-info \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet
```


## <a name="use-data-disks-with-scale-sets"></a>Verwenden von Datenträgern mit Skalierungsgruppen
Sie können Datenträger mit Skalierungsgruppen erstellen und nutzen. In einem vorherigen Tutorial haben Sie gelernt, wie [Azure-Datenträger verwaltet werden](tutorial-manage-disks.md). Erläutert wurden die bewährten Methoden und verbesserte Leistung beim Erstellen von Apps auf normalen Datenträgern statt auf dem Betriebssystem-Datenträger.

### <a name="create-scale-set-with-data-disks"></a>Erstellen einer Skalierungsgruppe mit Datenträgern
Fügen Sie zum Erstellen einer Skalierungsgruppe und zum Anfügen von Datenträgern dem Befehl [az vmss create](/cli/azure/vmss#create) den Parameter `--data-disk-sizes-gb` hinzu. Im folgenden Beispiel wird eine Skalierungsgruppe erstellt, wobei an jede Instanz ein *50*-GB-Datenträger angefügt wird:

```azurecli-interactive 
az vmss create \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSetDisks \
    --image UbuntuLTS \
    --upgrade-policy-mode automatic \
    --custom-data cloud-init.txt \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 50
```

Bei Entfernen von Instanzen aus einer Skalierungsgruppe werden angefügte Datenträger ebenfalls entfernt.

### <a name="add-data-disks"></a>Hinzufügen von Datenträgern
Verwenden Sie [az vmss disk attach](/cli/azure/vmss/disk#attach) zum Hinzufügen eines Datenträgers zu Instanzen in Ihrer Skalierungsgruppe. Im folgenden Beispiel wird jeder Instanz ein *50*-GB-Datenträger hinzugefügt:

```azurecli-interactive 
az vmss disk attach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --size-gb 50 \
    --lun 2
```

### <a name="detach-data-disks"></a>Trennen von Datenträgern
Verwenden Sie [az vmss disk detach](/cli/azure/vmss/disk#detach) zum Entfernen eines Datenträgers von Instanzen in Ihrer Skalierungsgruppe. Im folgenden Beispiel wird der Datenträger mit LUN *2* aus jeder Instanz entfernt:

```azurecli-interactive 
az vmss disk detach \
    --resource-group myResourceGroupScaleSet \
    --name myScaleSet \
    --lun 2
```


## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine Skalierungsgruppe für virtuelle Computer bereitgestellt. Es wurde Folgendes vermittelt:

> [!div class="checklist"]
> * Verwenden von cloud-init zum Erstellen einer zu skalierenden Anwendung
> * Erstellen einer Skalierungsgruppe für virtuelle Computer
> * Erhöhen oder Verringern der Anzahl der Instanzen in einer Skalierungsgruppe
> * Erstellen von Regeln zur automatischen Skalierung
> * Anzeigen von Verbindungsinformationen für die Skalierungsgruppeninstanzen
> * Verwenden von Datenträgern mit Skalierungsgruppen

Im nächsten Tutorial erhalten Sie weitere Informationen zu den Konzepten des Lastenausgleichs für virtuelle Computer.

> [!div class="nextstepaction"]
> [Lastenausgleich für virtuelle Computer](tutorial-load-balancer.md)

