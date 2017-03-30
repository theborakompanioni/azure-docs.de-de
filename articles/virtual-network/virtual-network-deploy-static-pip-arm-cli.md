---
title: "Erstellen einer VM mit einer statischen öffentlichen IP-Adresse – Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine VM mit einer statischen öffentlichen IP-Adresse mithilfe der Azure CLI 2.0 erstellen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: 2c2442e6e0e1617dada3ba277e2478c8daa32c67
ms.lasthandoff: 03/22/2017


---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli-20"></a>Erstellen einer VM mit einer statischen öffentlichen IP-Adresse mithilfe der Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure-Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md)
> * [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [Vorlage](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassisch)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Ressourcen-Manager und klassische Bereitstellungen](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des klassischen Bereitstellungsmodells empfohlen wird.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name = "create"></a>Erstellen der VM

Sie können diese Aufgabe mithilfe der Azure CLI 2.0 (dieser Artikel) oder mit der [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md) ausführen. Mithilfe der Werte in Anführungszeichen ("") in den folgenden Schritten werden Ressourcen mit Einstellungen aus dem Szenario erstellt. Ersetzen Sie die Werte ggf. durch entsprechende Werte für Ihre Umgebung.

1. Installieren Sie die [Azure CLI 2.0](/cli/azure/install-az-cli2), sofern noch nicht geschehen.
2. Erstellen Sie ein Paar aus einem öffentlichen und privaten SSH-Schlüssel für Linux-VMs, indem Sie die Schritte im Artikel [Erstellen eines öffentlich-privaten SSH-Schlüsselpaars für virtuelle Linux-Computer](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ausführen.
3. Melden Sie sich über eine Befehlsshell mit dem Befehl `az login` an.
4. Erstellen Sie die VM, indem Sie das folgende Skript auf einem Linux- oder Mac-Computer ausführen. Die öffentliche Azure-IP-Adresse, das virtuelle Netzwerk, die Netzwerkschnittstelle und die VM-Ressourcen müssen sich alle am selben Standort befinden. Wenngleich sich die Ressourcen nicht zwingend in derselben Ressourcengruppe befinden müssen, ist dies im folgenden Skript der Fall.

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using the --allocation-method Static option.
# If you do not specify this option, the address is allocated dynamically. The address is assigned to the
# resource from a pool of IP adresses unique to each Azure region. The DnsName must be unique within the
# Azure location it's created in. Download and view the file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists the ranges for each region.

PipName="PIPWEB1"
DnsName="iaasstoryws1"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static \
--dns-name $DnsName

# Create a virtual network with one subnet

VnetName="TestVNet"
VnetPrefix="192.168.0.0/16"
SubnetName="FrontEnd"
SubnetPrefix="192.168.1.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $SubnetName \
--subnet-prefix $SubnetPrefix

# Create a network interface connected to the VNet with a static private IP address and associate the public IP address
# resource to the NIC.

NicName="NICWEB1"
PrivateIpAddress="192.168.1.101"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $SubnetName \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress \
--public-ip-address $PipName

# Create a new VM with the NIC

VmName="WEB1"

# Replace the value for the VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace the value for the OsImage variable with a value for *urn* from the output returned by entering
# the `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace the following value with the path to your public key file.
SshKeyValue="~/.ssh/id_rsa.pub"

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $NicName \
--admin-username $Username \
--ssh-key-value $SshKeyValue
# If creating a Windows VM, remove the previous line and you'll be prompted for the password you want to configure for the VM.
```

Zusätzlich zur VM werden mit dem Skript folgende Komponenten erstellt:
- Ein einzelner verwalteter Premium-Datenträger. Dies ist die Standardeinstellung, aber Sie können auch einen anderen Datenträgertyp für die Erstellung auswählen. Ausführliche Informationen finden Sie im Artikel [Erstellen einer Linux-VM mithilfe der Azure CLI 2.0](../virtual-machines/virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Virtuelles Netzwerk, Subnetz, Netzwerkkarte und öffentliche IP-Adressressourcen. Alternativ können Sie ein *vorhandenes* Netzwerk, ein Subnetz, eine Netzwerkkarte oder öffentliche IP-Adressressourcen verwenden. Um zu erfahren, wie Sie vorhandene Netzwerkressourcen verwenden, statt zusätzliche Ressourcen zu erstellen, geben Sie `az vm create -h` ein.

## <a name = "validate"></a>Überprüfen der VM-Erstellung und der öffentlichen IP-Adresse

1. Geben Sie den Befehl `az resource list --resouce-group IaaSStory --output table` ein, um eine Liste der Ressourcen anzuzeigen, die vom Skript erstellt wurden. Die zurückgegebene Ausgabe sollte fünf Ressourcen umfassen: Netzwerkschnittstelle, Datenträger, öffentliche IP-Adresse, virtuelles Netzwerk und VM.
2. Geben Sie den Befehl `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table` ein. Beachten Sie in der zurückgegebenen Ausgabe den Wert von **IpAddress**. Beachten Sie außerdem, dass der Wert von **PublicIpAllocationMethod** auf *Static* festgelegt ist.
3. Bevor Sie den folgenden Befehl ausführen, entfernen Sie die spitzen Klammern (<>), ersetzen Sie *Username* durch den Namen, den Sie im Skript für die Variable **Username** verwendet haben, und ersetzen Sie *ipAddress* durch den Wert für **ipAddress** aus dem letzten Schritt. Führen Sie den folgenden Befehl aus, um eine Verbindung mit der VM herzustellen: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 

## <a name= "clean-up"></a>Entfernen der VM und zugehöriger Ressourcen

Es wird empfohlen, die in dieser Übung erstellten Ressourcen zu löschen, sofern Sie sie nicht in der Produktion verwenden möchten. Durch die VM, die öffentliche IP-Adresse und die Datenträgerressourcen werden Kosten verursacht, solange sie bereitgestellt sind. Führen Sie zum Entfernen der während dieser Übung erstellten Ressourcen die folgenden Schritte aus:

1. Führen Sie den Befehl `az resource list --resource-group IaaSStory` aus, um die Ressourcen in der Ressourcengruppe anzuzeigen.
2. Vergewissern Sie sich, dass sich keine weiteren Ressourcen in der Ressourcengruppe befinden als die, die mithilfe des Skripts in diesem Artikel erstellt wurden. 
3. Führen Sie zum Löschen aller Ressourcen, die in dieser Übung erstellt wurden, den Befehl `az group delete -n IaaSStory` aus. Der Befehl löscht die Ressourcengruppe und alle darin enthaltenen Ressourcen.

## <a name="next-steps"></a>Nächste Schritte

Netzwerkdatenverkehr kann bei der in diesem Artikel erstellten VM ein- oder ausgehen. Sie können innerhalb einer Netzwerksicherheitsgruppe (NSG) Regeln für den eingehenden und ausgehenden Datenverkehr definieren, der von und zur Netzwerkschnittstelle, dem Netzwerk oder beiden Komponenten fließt. Weitere Informationen zu Netzwerksicherheitsgruppen finden Sie im Artikel [Übersicht über NSGs](virtual-networks-nsg.md).
