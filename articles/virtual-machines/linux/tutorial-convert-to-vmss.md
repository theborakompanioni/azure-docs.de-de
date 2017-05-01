---
title: Konvertieren einer Azure-VM in eine Skalierungsgruppe | Microsoft-Dokumentation
description: Erstellen Sie mit der Azure CLI eine Azure-VM-Skalierungsgruppe unter Linux, und stellen Sie sie bereit.
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
ms.sourcegitcommit: 0b53a5ab59779dc16825887b3c970927f1f30821
ms.openlocfilehash: 8d3376d2791b1349298db618d475ce5573083702
ms.lasthandoff: 04/07/2017

---

# <a name="convert-an-existing-azure-virtual-machine-to-a-scale-set"></a>Konvertieren eines vorhandenen virtuellen Azure-Computers in eine Skalierungsgruppe

In diesem Tutorial wird gezeigt, wie ein virtueller Computer mit Azure CLI 2.0 in eine VM-Skalierungsgruppe konvertiert wird. Sie erfahren außerdem, wie Sie die Konfiguration der virtuellen Computer in der Skalierungsgruppe automatisieren können. Weitere Informationen zum Installieren von Azure CLI 2.0 finden Sie unter [Erste Schritte mit Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md). Weitere Informationen zu Skalierungsgruppen finden Sie unter [VM-Skalierungsgruppen](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).

## <a name="step-1---deprovision-the-vm"></a>Schritt 1: Aufheben der Bereitstellung des virtuellen Computers

Verwenden Sie SSH zum Herstellen einer Verbindung mit dem virtuellen Computer.

Heben Sie die Bereitstellung des virtuellen Computers mithilfe des Azure-VM-Agents auf, um Dateien und Daten zu löschen. Eine ausführliche Übersicht über das Aufheben der Bereitstellung finden Sie unter [Erfassen eines virtuellen Linux-Computers](capture-image.md).

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-the-vm"></a>Schritt 2: Erfassen eines Images des virtuellen Computers

Eine ausführliche Übersicht über das Erfassen finden Sie unter [Erfassen eines virtuellen Linux-Computers](capture-image.md).

Heben Sie die Zuordnung des virtuellen Computers mit [az vm deallocate](/cli/azure/vm#deallocate) auf:

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

Generalisieren Sie den virtuellen Computer mit [az vm generalize](/cli/azure/vm#generalize):

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

Erstellen Sie ein Image aus der VM-Ressource mit [az image create](/cli/azure/image#create):

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-the-scale-set"></a>Schritt 3: Erstellen der Skalierungsgruppe

Rufen Sie die **ID** des Images ab.

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

Erstellen Sie mit [az vmss create](/cli/azure/vmss#create) einen virtuellen Computer aus Ihrer Imageressource:

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

Mit diesem Befehl wurde auch ein 10-GB-Datenträger angefügt. Beachten Sie, dass abhängig von der für den virtuellen Computer ausgewählten Größe (wir haben **Standard_DS1_v2** verwendet) eine andere Anzahl von Datenträgern zulässig ist. Weitere Informationen finden Sie unter [Größen virtueller Computer](sizes.md).

Sobald die Skalierungsgruppe fertiggestellt wurde, stellen Sie eine Verbindung damit her. Rufen Sie mit [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info) eine Liste von IP-Adressen für die Instanzen für SSH ab:

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

Jetzt können Sie eine Verbindung mit der Instanz des virtuellen Computers herstellen, um den Datenträger zu initialisieren.

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-the-data-disk"></a>Schritt 4: Initialisieren des Datenträgers

Während die Verbindung mit dem virtuellen Computer besteht, partitionieren Sie den Datenträger mit `fdisk`:

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

Schreiben Sie mit dem Befehl `mkfs` ein Dateisystem auf die Partition:

```bash
sudo mkfs -t ext4 /dev/sdc1
```

Binden Sie den neuen Datenträger ein, damit im Betriebssystem darauf zugegriffen werden kann:

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

Auf den Datenträger kann jetzt über den Bereitstellungspunkt „datadrive“ zugegriffen werden. Dies kann mit `ls /datadrive/` überprüft werden.

Beenden Sie die SSH-Sitzung.


## <a name="step-5---configure-firewall"></a>Schritt 5: Konfigurieren der Firewall

Öffnen Sie in der Firewall eine Lücke zum Webserver, der von der Skalierungsgruppe gehostet wird. Als die Skalierungsgruppe erstellt wurde, wurde auch ein Lastenausgleich erstellt, und Sie haben ihn für **SSH**-Verbindungen mit den einzelnen virtuellen Computern verwendet. Zum Öffnen eines Ports benötigen Sie zwei Angaben, die Sie mithilfe der Azure CLI abrufen können.

* **Front-End-IP-Adresspool**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* **Back-End-IP-Adresspool**  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

Mit diesen beiden Namen können Sie Port **80** öffnen.

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a>Schritt 6: Automatisieren der Konfiguration

Der Datenträger muss für jede Instanz des virtuellen Computers konfiguriert werden. Wir können die Konfiguration des virtuellen Computers mit der Erweiterung **CustomScript** automatisieren.

Erstellen Sie zunächst ein *SH*-Skript, das die Befehle für das Datenträgerformat enthält.

```sh
#!/bin/bash

# Setup the data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

Laden Sie als Nächstes die Skriptdatei in einen Speicherort hoch, auf den die Erweiterung **CustomScript** zugreifen kann. Eine Kopie ist [hier](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4) verfügbar.

Erstellen Sie eine lokale Datei mit dem Namen **settings.json**, und fügen Sie den folgenden JSON-Block ein. Die `flieUris`-Eigenschaft sollte auf den Speicherort festgelegt werden, in den die Skriptdatei hochgeladen wurde.

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

Stellen Sie diesen Befehl mit der Erweiterung **CustomScript** Ihrer Skalierungsgruppe bereit. Verweisen Sie dabei auf die Datei **settings.json**, die wir soeben erstellt haben.

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

Diese Erweiterung wird automatisch auf allen aktuellen Instanzen und auf allen Instanzen, die durch Skalieren später erstellt werden, ausgeführt.

## <a name="step-7---configure-autoscale-rules"></a>Schritt 7: Konfigurieren von Regeln zur automatischen Skalierung

Derzeit können keine Regeln zur automatischen Skalierung in der Azure CLI festgelegt werden. Verwenden Sie das [Azure-Portal](https://portal.azure.com) zum Konfigurieren der automatischen Skalierung.

## <a name="step-8---management-tasks"></a>Schritt 8: Verwaltungsaufgaben

Während des Lebenszyklus der Skalierungsgruppe müssen Sie möglicherweise eine oder mehrere Verwaltungsaufgaben ausführen. Darüber hinaus könnten Sie Skripts erstellen, um verschiedene Aufgaben im Lebenszyklus zu automatisieren. Die Azure CLI bietet eine schnelle Möglichkeit, diese Aufgaben zu erledigen. Im Folgenden sind einige allgemeine Aufgaben aufgeführt.

### <a name="get-connection-info"></a>Verbindungsinformationen abrufen

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a>Die Anzahl von Instanzen festlegen (manuelles Skalieren)

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a>Ressourcengruppe löschen

Beim Löschen einer Ressourcengruppe werden auch alle darin enthaltenen Ressourcen gelöscht.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu einigen der Features von VM-Skalierungsgruppen aus diesem Tutorial finden Sie in den folgenden Themen:

- [Übersicht über Azure-VM-Skalierungsgruppen](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [Übersicht über Azure Load Balancer](../../load-balancer/load-balancer-overview.md)
- [Steuern des Netzwerkdatenverkehrs mit Netzwerksicherheitsgruppen](../../virtual-network/virtual-networks-nsg.md)
