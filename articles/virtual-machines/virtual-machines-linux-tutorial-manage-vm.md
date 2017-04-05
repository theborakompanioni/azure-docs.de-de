---
title: Verwalten von virtuellen Linux-Computern mit der Azure CLI | Microsoft-Dokumentation
description: "Tutorial – Verwalten von virtuellen Linux-Computern mit der Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/28/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 14d3907d9ddbe42c9f919ce4d29ba7d58c7c5296
ms.lasthandoff: 03/28/2017

---

# <a name="manage-linux-virtual-machines-with-the-azure-cli"></a>Verwalten von virtuellen Linux-Computern mit der Azure CLI

In diesem Tutorial erstellen Sie einen virtuellen Computer und führen allgemeine Verwaltungsaufgaben durch, z.B. Hinzufügen eines Datenträgers, Automatisieren der Softwareinstallation und Erstellen einer Momentaufnahme des virtuellen Computers. 

Für dieses Tutorial muss die aktuelle Version von [Azure CLI 2.0](/cli/azure/install-azure-cli) installiert sein.

## <a name="step-1--log-in-to-azure"></a>Schritt 1: Anmelden bei Azure

Öffnen Sie zuerst ein Terminal, und melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an.

```azurecli
az login
```

## <a name="step-2--create-resource-group"></a>Schritt 2: Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](https://docs.microsoft.com/cli/azure/group#create) eine Ressourcengruppe. 

Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. Vor dem virtuellen Computer muss eine Ressourcengruppe erstellt werden. In diesem Beispiel wird eine Ressourcengruppe mit dem Namen `myResourceGroup` in der Region `westeurope` erstellt. 

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="step-3---prepare-configuration"></a>Schritt 3: Vorbereiten der Konfiguration

Beim Bereitstellen eines virtuellen Computers können mithilfe von **cloud-init** Konfigurationen wie das Installieren von Paketen, das Erstellen von Dateien und das Ausführen von Skripts automatisiert werden. In diesem Tutorial sind die Konfigurationen von zwei Elementen automatisiert:

- Installation eines NGINX-Webservers
- Bereitstellung eines zweiten Datenträgers auf dem virtuellen Computer

Da die **cloud-init**-Konfiguration zum Zeitpunkt der Bereitstellung des virtuellen Computers erfolgt, muss vor dem Erstellen des virtuellen Computers eine **cloud-init**-Konfiguration definiert werden.

Erstellen Sie eine Datei mit dem Namen `cloud-init.txt`, und kopieren Sie folgenden Inhalt hinein. Diese Konfiguration installiert das NGINX-Paket und führt Befehle aus, um den zweiten Datenträger zu formatieren und bereitzustellen.

```yaml
#cloud-config
package_upgrade: true
packages:
  - nginx
runcmd:
  - (echo n; echo p; echo 1; echo ; echo ; echo w) | sudo fdisk /dev/sdc
  - sudo mkfs -t ext4 /dev/sdc1
  - sudo mkdir /datadrive
  - sudo mount /dev/sdc1 /datadrive
```

## <a name="step-4---create-virtual-machine"></a>Schritt 4 : Erstellen eines virtuellen Computers

Erstellen Sie mit dem Befehl [az vm create](https://docs.microsoft.com/cli/azure/vm#create) einen virtuellen Computer. 

Beim Erstellen eines virtuellen Computers stehen mehrere Optionen zur Verfügung, z.B. Betriebssystemimage, Festlegen der Datenträgergröße und Administratoranmeldeinformationen. In diesem Beispiel wird ein virtueller Computer mit dem Namen `myVM` erstellt, auf dem Ubuntu ausgeführt wird. Ein Datenträger mit 50 GB wird erstellt und dem virtuellen Computer mithilfe des Arguments `--data-disk-sizes-gb` angefügt. Das Argument `--custom-data` akzeptiert, die „cloud-init“-Konfiguration und stellt sie auf dem virtuellen Computer bereit. Abschließend werden auch SSH-Schlüssel erstellt, sofern sie nicht vorhanden sind.

```azurecli
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image Canonical:UbuntuServer:14.04.4-LTS:latest \
  --generate-ssh-keys \
  --data-disk-sizes-gb 50 \
  --custom-data cloud-init.txt
```

Nachdem der virtuelle Computer erstellt wurde, gibt die Azure-CLI folgende Informationen aus. Notieren Sie die öffentliche IP-Adresse, da diese Adresse beim Zugriff auf den virtuellen Computer verwendet wird. 

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westeurope",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroup"
}
```

Der virtuelle Computer wurde zwar bereitgestellt wurde, die **cloud-init** kann jedoch noch einige Minuten in Anspruch nehmen. 

## <a name="step-5--configure-firewall"></a>Schritt 5: Konfigurieren der Firewall

Eine Azure[Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) (NSG) steuert ein- und ausgehenden Datenverkehr für einen oder mehrere virtuelle Computer. Netzwerksicherheitsgruppen-Regeln dienen zum Zulassen oder Verweigern von Netzwerkdatenverkehr an einem bestimmten Port oder in einem Portbereich. Diese Regeln können auch ein Quelladresspräfix enthalten, sodass nur Datenverkehr aus einer vordefinierten Quelle mit einem virtuellen Computer kommunizieren kann.

Im vorherigen Abschnitt wurde der NGINX-Webserver installiert. Ohne eine Netzwerksicherheitsgruppen-Regel zum Zulassen des gesamten eingehenden Datenverkehrs an Port 80 kann der Webserver nicht auf das Internet zugreifen. Dieser Schritt führt Sie durch das Erstellen der NSG-Regel, um eingehende Verbindungen an Port 80 zuzulassen.

### <a name="create-nsg-rule"></a>Erstellen der NSG-Regel

Zum Erstellen einer NSG-Regel für eingehenden Datenverkehr verwenden Sie den Befehl [az vm open-port](https://docs.microsoft.com/cli/azure/vm#open-port). Im folgenden Beispiel wird Port `80` für den virtuellen Computer geöffnet.

```azurecli
az vm open-port --port 80 --resource-group myResourceGroup --name myVM 
```

Navigieren Sie jetzt zur öffentlichen IP-Adresse des virtuellen Computers. Mit dieser NSG-Regel wird die NGINX-Standardwebsite angezeigt.

![NGINX-Standardwebsite](./media/virtual-machines-linux-tutorial-manage-vm/nginx.png)  

## <a name="step-6--snapshot-virtual-machine"></a>Schritt 6: Erstellen einer Momentaufnahme des virtuellen Computers

Bei einer Momentaufnahme des Datenträger wird eine schreibgeschützte Point-in-Time-Kopie des Datenträgers erstellt. In diesem Schritt wird eine Momentaufnahme des Betriebssystem-Datenträgers des virtuellen Computers erstellt. Mit einer Momentaufnahme des Momentaufnahme des Betriebssystem-Datenträgers kann der virtuelle Computer schnell in einem bestimmten Zustand wiederhergestellt werden, oder die Momentaufnahme kann verwendet werden, um einen neuen virtuellen Computer mit einem identischen Zustand zu erstellen.

### <a name="create-snapshot"></a>Erstellen der Momentaufnahme

Vor dem Erstellen einer Momentaufnahme wird die ID oder der Name des Datenträgers benötigt. Rufen Sie die Datenträger-ID mit dem Befehl [az vm show](https://docs.microsoft.com/cli/azure/vm#show) ab. In diesem Beispiel wird die Datenträger-ID in einer Variablen gespeichert und in einem späteren Schritt verwendet.

```azurecli
osdiskid=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
```

Nachdem Sie jetzt die ID des Datenträgers haben, wird mit dem folgenden Befehl die Momentaufnahme erstellt.

```azurcli
az snapshot create -g myResourceGroup --source "$osdiskid" --name osDisk-backup
```

### <a name="create-disk-from-snapshot"></a>Erstellen eines Datenträgers aus der Momentaufnahme

Diese Momentaufnahme kann dann in einen Datenträger konvertiert werden, mit dem wiederum der virtuelle Computer neu erstellt werden kann.

```azurecli
az disk create --resource-group myResourceGroup --name mySnapshotDisk --source osDisk-backup
```

### <a name="restore-virtual-machine-from-snapshot"></a>Wiederherstellen des virtuellen Computers aus der Momentaufnahme

Löschen Sie den vorhandenen virtuellen Computer, um die Wiederherstellung des virtuellen Computers durchzuführen. 

```azurecli
az vm delete --resource-group myResourceGroup --name myVM
```

Bei der erneuten Erstellung des virtuellen Computers wird die vorhandene Netzwerkschnittstelle wiederverwendet. So wird sichergestellt, dass Netzwerk-Sicherheitskonfigurationen beibehalten werden.

Rufen Sie mit dem Befehl [az network nic list](https://docs.microsoft.com/cli/azure/network/nic#list) den Namen der Netzwerkschnittstelle ab. In diesem Beispiel wird der Name in eine Variable namens `nic` eingefügt, die im nächsten Schritt verwendet wird.

```azurecli
nic=$(az network nic list --resource-group myResourceGroup --query "[].[name]" -o tsv)
```

Erstellen Sie einen neuen virtuellen Computer aus dem Momentaufnahmendatenträger.

```azurecli
az vm create --resource-group myResourceGroup --name myVM --attach-os-disk mySnapshotDisk --os-type linux --nics $nic
```

Notieren Sie die neue öffentliche IP-Adresse, und navigieren Sie in einem Internetbrowser zu dieser Adresse. Sie werden sehen, dass NGINX auf dem wiederhergestellten virtuellen Computer ausgeführt wird. 

### <a name="reconfigure-data-disk"></a>Neukonfiguration des Datenträgers

Der Datenträger kann jetzt erneut an den virtuellen Computer angefügt werden. 

Suchen Sie zuerst mithilfe des Befehls [az disk list](https://docs.microsoft.com/cli/azure/disk#list) den Namen des Datenträgers. In diesem Beispiel wird der Name des Datenträgers in eine Variable namens `datadisk` eingefügt, die im nächsten Schritt verwendet wird.

```azurecli
datadisk=$(az disk list -g myResourceGroup --query "[?contains(name,'myVM')].[name]" -o tsv)
```

Verwenden Sie den Befehl [az vm disk attach](https://docs.microsoft.com/cli/azure/vm/disk#attach), um den Datenträger anzufügen.

```azurecli
az vm disk attach –g myResourceGroup –-vm-name myVM –-disk $datadisk
```

Der Datenträger muss auch für das Betriebssystem bereitgestellt werden. Zum Bereitstellen des Datenträgers stellen Sie eine Verbindung mit dem virtuellen Computer her und führen dann `sudo mount /dev/sdc1 /datadrive` oder Ihren bevorzugten Vorgang zur Datenträgerbereitstellung aus. 

## <a name="step-7--management-tasks"></a>Schritt 7: Verwaltungsaufgaben

Während der Lebensdauer eines virtuellen Computers können Sie Verwaltungsaufgaben wie das Starten, Beenden oder Löschen eines virtuellen Computers ausführen. Darüber hinaus empfiehlt es sich, Skripts zum Automatisieren von wiederkehrenden oder komplexen Aufgaben zu erstellen. Mithilfe der Azure CLI können viele allgemeine Verwaltungsaufgaben über die Befehlszeile oder in Skripts ausgeführt werden. 

### <a name="get-ip-address"></a>Abrufen der IP-Adresse

Dieser Befehl gibt die privaten und öffentlichen IP-Adressen eines virtuellen Computers zurück.  

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myVM
```

### <a name="resize-virtual-machine"></a>Ändern der Größe des virtuellen Computers

Zum Ändern der Größe eines virtuellen Azure-Computers müssen Sie die Namen der verfügbaren Größen in der ausgewählten Azure-Region kennen. Diese Größen können mit dem Befehl [az vm list-sizes](https://docs.microsoft.com/cli/azure/vm#list-sizes) abgerufen werden.

```azurecli
az vm list-sizes --location westeurope --output table
```

Die Größe des virtuellen Computers kann mit dem Befehl [az vm resize](https://docs.microsoft.com/cli/azure/vm#resize) geändert werden. 

```azurecli
az vm resize -g myResourceGroup -n myVM --size Standard_F4s
```

### <a name="stop-virtual-machine"></a>Beenden des virtuellen Computers

```azurecli
az vm stop --resource-group myResourceGroup --name myVM
```

### <a name="start-virtual-machine"></a>Starten des virtuellen Computers

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

### <a name="delete-resource-group"></a>Ressourcengruppe löschen

Beim Löschen einer Ressourcengruppe werden auch alle darin enthaltenen Ressourcen gelöscht.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

Tutorial – [Create load balanced virtual machines](./virtual-machines-linux-tutorial-load-balance-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Erstellen von virtuellen Computern mit Lastenausgleich)

Beispiele – [Azure CLI-Beispielskripts](./virtual-machines-linux-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
