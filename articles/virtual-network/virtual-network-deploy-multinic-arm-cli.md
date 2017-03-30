---
title: "Erstellen einer VM mit mehreren Netzwerkkarten – Azure CLI 2.0 | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie eine VM mit mehreren Netzwerkkarten mithilfe der Azure CLI 2.0 erstellen.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: 4f5eaf5f6ba56709b69d97c1f646f71396fd031b
ms.lasthandoff: 03/22/2017


---
# <a name="create-a-vm-with-multiple-nics-using-the-azure-cli-20"></a>Erstellen einer VM mit mehreren Netzwerkkarten mithilfe der Azure CLI 2.0

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md).  Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des [klassischen Bereitstellungsmodells](virtual-network-deploy-multinic-classic-cli.md) empfohlen wird.
>

## <a name="create"></a>Erstellen der VM

Sie können diese Aufgabe mithilfe der Azure CLI 2.0 (dieser Artikel) oder mit der [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md) ausführen. Mithilfe der Werte in Anführungszeichen ("") in den folgenden Schritten werden Ressourcen mit Einstellungen aus dem Szenario erstellt. Ersetzen Sie die Werte ggf. durch entsprechende Werte für Ihre Umgebung.

1. Installieren Sie die [Azure CLI 2.0](/cli/azure/install-az-cli2), sofern noch nicht geschehen.
2. Erstellen Sie ein Paar aus einem öffentlichen und privaten SSH-Schlüssel für Linux-VMs, indem Sie die Schritte im Artikel [Erstellen eines öffentlich-privaten SSH-Schlüsselpaars für virtuelle Linux-Computer](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ausführen.
3. Melden Sie sich über eine Befehlsshell mit dem Befehl `az login` an.
4. Erstellen Sie die VM, indem Sie das folgende Skript auf einem Linux- oder Mac-Computer ausführen. Das Skript erstellt eine Ressourcengruppe, ein virtuelles Netzwerk (VNET) mit zwei Subnetzen, zwei Netzwerkkarten und eine VM, an die die zwei Netzwerkkarten angefügt sind. Eine der Netzwerkkarten (NICs) ist mit einem Subnetz verbunden, und der NIC ist eine statische öffentliche und eine private IP-Adresse zugewiesen. Die andere Netzwerkkarte ist mit dem anderen Subnetz verbunden. Ihr ist eine statische private IP-Adresse und keine öffentliche IP-Adresse zugewiesen. Die Netzwerkkarte, die öffentliche IP-Adresse, das virtuelle Netzwerk und die VM-Ressourcen müssen sich alle am selben Standort und im selben Abonnement befinden. Wenngleich sich die Ressourcen nicht zwingend in derselben Ressourcengruppe befinden müssen, ist dies im folgenden Skript der Fall.

```bash
#!/bin/sh

RgName="Multi-NIC-VM"
Location="westus"

# Create a resource group.
az group create \
--name $RgName \
--location $Location

# The address is assigned to the resource from a pool of IP adresses unique to each Azure region. 
# Download and view the file from https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists
# the ranges for each region.

PipName="PIP-WEB"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static

# Create a virtual network with one subnet

VnetName="VNet1"
VnetPrefix="10.0.0.0/16"
VnetSubnet1Name="Front-End"
VnetSubnet1Prefix="10.0.0.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnet1Name \
--subnet-prefix $VnetSubnet1Prefix

# Create a second subnet within the VNet

VnetSubnet2Name="Back-end"
VnetSubnet2Prefix="10.0.1.0/24"
az network vnet subnet create \
--vnet-name $VnetName \
--resource-group $RgName \
--name $VnetSubnet2Name \
--address-prefix $VnetSubnet2Prefix

# Create a network interface connected to one of the subnets. The NIC is assigned a single dynamic private and
# public IP address by default, but you can instead, assign static addresses, or no public IP address at all.
# You can also assign multiple private or public IP addresses to each NIC. To learn more about IP addressing
# options for NICs, enter the az network nic create -h command.

Nic1Name="NIC-FE"
PrivateIpAddress1="10.0.0.5"

az network nic create \
--name $Nic1Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress1 \
--public-ip-address $PipName

# Create a second network interface and connect it to the other subnet. Though multiple NICs attached to the same
# VM can be connected to different subnets, the subnets must all be within the same VNet. Add additional NICs as necessary.

Nic2Name="NIC-BE"
PrivateIpAddress2="10.0.1.5"

az network nic create \
--name $Nic2Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet2Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress2

# Create a VM and attach the two NICs.

VmName="WEB"

# Replace the value for the following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article. Not all VM sizes support
# more than one NIC, so be sure to select a VM size that supports the number of NICs you want to attach to the VM.
# You must create the VM with at least two NICs if you want to add more after VM creation. If you create a VM with
# only one NIC, you can't add additional NICs to the VM after VM creation, regardless of how many NICs the VM supports.
# The VM size specified in the following variable supports two NICs.

VmSize="Standard_DS2"

# Replace the value for the OsImage variable value with a value for *urn* from the output returned by entering the
# az vm image list command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace the following value with the path to your public key file.

SshKeyValue="~/.ssh/id_rsa.pub"

# Before executing the following command, add variable names of additional NICs you may have added to the script that
# you want to attach to the VM. If creating a Windows VM, remove the **ssh-key-value** line and you'll be prompted for
# the password you want to configure for the VM.

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $Nic1Name $Nic2Name \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

Zusätzlich zur VM mit den zwei NICs werden mit dem Skript folgende Komponenten erstellt:
- Ein einzelner verwalteter Premium-Datenträger. Dies ist die Standardeinstellung, aber Sie können auch einen anderen Datenträgertyp für die Erstellung auswählen. Ausführliche Informationen finden Sie im Artikel [Erstellen einer Linux-VM mithilfe der Azure CLI 2.0](../virtual-machines/virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Ein virtuelles Netzwerk mit zwei Subnetzen und einer einzigen öffentlichen IP-Adresse. Alternativ können Sie ein *vorhandenes* Netzwerk, ein Subnetz, eine Netzwerkkarte oder öffentliche IP-Adressressourcen verwenden. Um zu erfahren, wie Sie vorhandene Netzwerkressourcen verwenden, statt zusätzliche Ressourcen zu erstellen, geben Sie `az vm create -h` ein.

## <a name = "validate"></a>Überprüfen von VM-Erstellung und NICs

1. Geben Sie den Befehl `az resource list --resouce-group Multi-NIC-VM --output table` ein, um eine Liste der Ressourcen anzuzeigen, die vom Skript erstellt wurden. Die zurückgegebene Ausgabe sollte sechs Ressourcen umfassen: zwei Netzwerkkarten, einen Datenträger, eine öffentliche IP-Adresse, ein virtuelles Netzwerk und eine VM.
2. Geben Sie den Befehl `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table` ein. Beachten Sie in der zurückgegebenen Ausgabe den Wert von **IpAddress**. Beachten Sie außerdem, dass der Wert von **PublicIpAllocationMethod** auf *Static* festgelegt ist.
3. Bevor Sie den folgenden Befehl ausführen, entfernen Sie die spitzen Klammern (<>), ersetzen Sie *Username* durch den Namen, den Sie im Skript für die Variable **Username** verwendet haben, und ersetzen Sie *ipAddress* durch den Wert für **ipAddress** aus dem letzten Schritt. Führen Sie den folgenden Befehl aus, um eine Verbindung mit der VM herzustellen: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 
4. Führen Sie nach der Verbindungsherstellung mit der VM den Befehl `sudo ifconfig` aus, um die Schnittstellen *eth0* und *eth1* anzuzeigen. Jeder Netzwerkkarte wurden durch die Azure-DHCP-Server die im Skript angegebenen statischen privaten IP-Adressen zugewiesen. Die IP- und MAC-Adressen, die den Netzwerkkarten zugewiesen werden, ändern sich erst beim Löschen der VM. Es wird empfohlen, die IP-Adressen innerhalb eines Betriebssystems nicht zu ändern, da dies die Konnektivität des Computers deaktivieren kann. Öffentliche IP-Adressen werden im Betriebssystem nicht angezeigt, da sie per NAT über die Azure-Infrastruktur von der und in die private IP-Adresse übersetzt werden.

## <a name= "clean-up"></a>Entfernen der VM und zugehöriger Ressourcen

Es wird empfohlen, die in dieser Übung erstellten Ressourcen zu löschen, sofern Sie sie nicht in der Produktion verwenden möchten. Durch die VM, die öffentliche IP-Adresse und die Datenträgerressourcen werden Kosten verursacht, solange sie bereitgestellt sind. Führen Sie zum Entfernen der während dieser Übung erstellten Ressourcen die folgenden Schritte aus:
1. Führen Sie den Befehl `az resource list --resource-group Multi-NIC-VM` aus, um die Ressourcen in der Ressourcengruppe anzuzeigen.
2. Vergewissern Sie sich, dass sich keine weiteren Ressourcen in der Ressourcengruppe befinden als die, die mithilfe des Skripts in diesem Artikel erstellt wurden. 
3. Führen Sie zum Löschen aller Ressourcen, die in dieser Übung erstellt wurden, den Befehl `az group delete --name Multi-NIC-VM` aus. Der Befehl löscht die Ressourcengruppe und alle darin enthaltenen Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

Netzwerkdatenverkehr kann bei der in diesem Artikel erstellten VM ein- oder ausgehen. Sie können innerhalb einer Netzwerksicherheitsgruppe (NSG) Regeln für den eingehenden und ausgehenden Datenverkehr definieren, der von und zur Netzwerkschnittstelle, dem Netzwerk oder beiden Komponenten fließt. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie im Artikel [Übersicht über NSGs](virtual-networks-nsg.md).
