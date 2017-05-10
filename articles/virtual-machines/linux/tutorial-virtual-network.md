---
title: Virtuelle Azure-Netzwerke und virtuelle Linux-Computer | Microsoft-Dokumentation
description: 'Tutorial: Verwalten von virtuellen Azure-Netzwerken und virtuellen Linux-Computern mit der Azure-Befehlszeilenschnittstelle'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/18/2017
ms.author: davidmu
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 1ac628b606a198bb437c02d00467d48462c34334
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---

# <a name="manage-azure-virtual-networks-and-linux-virtual-machines-with-the-azure-cli"></a>Verwalten von virtuellen Azure-Netzwerken und virtuellen Linux-Computern mit der Azure-Befehlszeilenschnittstelle

In diesem Tutorial erfahren Sie, wie Sie mehrere virtuelle Computer (VMs) in einem virtuellen Netzwerk erstellen und die Netzwerkverbindung zwischen ihnen konfigurieren. Nach Abschluss des Tutorials kann über das Internet über Port 22 für SSH-Verbindungen und Port 80 für HTTP-Verbindungen auf einen virtuellen Front-End-Computer zugegriffen werden. Ein virtueller Back-End-Computer mit einer MySQL -Datenbank ist isoliert und der Zugriff nur über den virtuellen Front-End-Computer an Port 3306 möglich.

Die Schritte in diesem Tutorial können mit der neuesten Version von [Azure CLI 2.0](/cli/azure/install-azure-cli) ausgeführt werden.

## <a name="create-vm-and-vnet"></a>Erstellen des virtuellen Computers und des virtuellen Netzwerks

Ein Azure Virtual Network (VNet) ist eine Darstellung Ihres eigenen Netzwerks in der Cloud. Bei einem VNet handelt es sich um eine logische Isolation von der dedizierten Azure-Cloud für Ihr Abonnement. Ein VNET umfasst Subnetze, Regeln für die Verbindung mit diesen Subnetzen und Verbindungen von den virtuellen Computern zu den Subnetzen. Mit der Azure-Befehlszeilenschnittstelle können Sie mühelos alle netzwerkbezogenen Ressourcen erstellen, die für die Unterstützung des Zugriffs auf die virtuellen Computer erforderlich sind. 

Vor der Erstellung anderer Azure-Ressourcen müssen Sie zunächst mit „az group create“ eine Ressourcengruppe erstellen. Das folgende Beispiel erstellt am Standort *westus* eine Ressourcengruppe mit dem Namen *myRGNetwork*:

```azurecli
az group create --name myRGNetwork --location westus
```

Wenn Sie einen virtuellen Computer mithilfe der Azure-Befehlszeilenschnittstelle erstellen, werden die erforderlichen Netzwerkressourcen automatisch zeitgleich erstellt. Erstellen Sie *myFrontendVM* und die unterstützenden Netzwerkressourcen mit [az vm create](https://docs.microsoft.com/cli/azure/vm#create):

```azurecli
az vm create \
  --resource-group myRGNetwork \
  --name myFrontendVM \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Notieren Sie sich die öffentliche IP-Adresse, nachdem der virtuelle Computer erstellt wurde. Diese Adresse wird in späteren Schritten in diesem Tutorial verwendet:

```bash
{
  "fqdns": "",
  "id": "/subscriptions/{id}/resourceGroups/myRGNetwork/providers/Microsoft.Compute/virtualMachines/myFrontendVM",
  "location": "westus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myRGNetwork"
}
```

Folgende Netzwerkressourcen wurden erstellt:

- **myFrontendVMNSG**: Die Netzwerksicherheitsgruppe, die eingehenden Datenverkehr für *myFrontendVM* schützt.
- **myVMPublicIP**: Die öffentliche IP-Adresse, die den Internetzugriff auf *myFrontendVM* ermöglicht.
- **myVMVMNic**: Die virtuelle Netzwerkschnittstelle, die die Netzwerkverbindung für *myFrontendVM* bereitstellt.
- **myVMVNET**: Das virtuelle Netzwerk, mit dem *myFrontendVM* verbunden ist.

## <a name="install-web-server"></a>Installieren des Webservers

Erstellen Sie eine SSH-Verbindung mit *myFrontendVM*. Ersetzen Sie die IP-Beispieladresse durch die öffentliche IP-Adresse des virtuellen Computers:

```bash
ssh 40.68.254.142
```

Führen Sie die folgenden Befehle zum Installieren von NGINX aus:

```bash
sudo apt-get -y update && sudo apt-get -y install nginx
```

Schließen Sie die SSH-Sitzung:

```bash
exit
```

## <a name="manage-internet-traffic"></a>Verwalten des Internetdatenverkehrs

Eine Netzwerksicherheitsgruppe (NSG) enthält eine Liste mit Sicherheitsregeln, mit denen Netzwerkdatenverkehr für Ressourcen, die mit einem virtuellen Netzwerk verbunden sind, zugelassen oder abgelehnt wird. Netzwerksicherheitsgruppen können Subnetzen oder einzelnen an virtuelle Computer angefügte NICs zugeordnet werden. Das Öffnen oder Schließen des Zugriffs auf virtuelle Computer über Ports erfolgt mithilfe von NSG-Regeln. Beim Erstellen von *myFrontendVM* wurde der eingehende Port 22 automatisch für SSH-Verbindungen geöffnet.

Öffnen Sie Port 80 mit [az vm open-port](https://docs.microsoft.com/cli/azure/vm#open-port) auf *myFrontendVM*:

```azurecli
az vm open-port --resource-group myRGNetwork --name myFrontendVM --port 80
```

Nun können Sie zur öffentlichen IP-Adresse des virtuellen Computers navigieren und die NGINX-Website anzeigen.

![NGINX-Standardwebsite](./media/quick-create-cli/nginx.png)

## <a name="manage-internal-traffic"></a>Verwalten des internen Datenverkehrs

Die interne Kommunikation von virtuellen Computern kann auch über eine Netzwerksicherheitsgruppe konfiguriert werden. In diesem Abschnitt erfahren Sie, wie ein weiteres Subnetz im Netzwerk erstellt und dem Subnetz eine Netzwerksicherheitsgruppe zugewiesen wird, sodass eine Verbindung von *myFrontendVM* mit *myBackendVM* über Port 3306 zulässig ist. Das Subnetz wird dann bei der Erstellung dem virtuellen Computer zugewiesen.

Fügen Sie mit [az network nsg create](https://docs.microsoft.com/cli/azure/network/nsg#create) die Netzwerksicherheitsgruppe *myBackendNSG* hinzu. 

```azurecli
az network nsg create \
 --resource-group myRGNetwork \
 --name myBackendNSG
```

Richten Sie einen Port für die Kommunikation zwischen *myFrontendVM* und *myBackendVM* im virtuellen Netzwerk ein. Fügen Sie mit [az network rule create](/cli/azure/network/rule#create) eine NSG-Regel hinzu, die Datenverkehr zu *myBackendSubnet* nur aus *myVMSubnet* zulässt:

```azurecli
az network nsg rule create \
 --resource-group myRGNetwork \
 --nsg-name myBackendNSG \
 --name com-rule \
 --access Allow \
 --protocol Tcp \
 --direction Inbound \
 --priority 100 \
 --source-address-prefix 10.0.0.0/24 \
 --source-port-range "*" \
 --destination-address-prefix "*" \
 --destination-port-range 3306
```

## <a name="add-back-end-subnet"></a>Hinzufügen des Back-End-Subnetzes

Ein Subnetz ist eine untergeordnete Ressource eines VNet und hilft, die Segmente von Adressräumen innerhalb eines CIDR-Blocks mithilfe von IP-Adressenpräfixen zu definieren. NICs können zu Subnetzen hinzugefügt und mit virtuellen Computern verbunden werden, sodass sie Konnektivität für verschiedene Workloads bereitstellen.

Fügen Sie *myBackEndSubnet* mit [az network vnet subnet create](https://docs.microsoft.com/cli/azure/network/vnet/subnet#create) zu *myFrontendVMVNet* hinzu:

```azurecli
az network vnet subnet create \
 --address-prefix 10.0.1.0/24 \
 --name myBackendSubnet \
 --resource-group myRGNetwork \
 --vnet-name myFrontendVMVNET \
 --network-security-group myBackendNSG
```

## <a name="create-back-end-vm"></a>Erstellen des virtuellen Back-End-Computers

Erstellen Sie *myBackendVM* mit [az vm create](/cli/azure/vm#create) unter Verwendung von *myBackendSubnet*:

```azurecli
az vm create \
  --resource-group myRGNetwork \
  --name myBackendVM \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --subnet myBackendSubnet \
  --vnet-name myFrontendVMVNET \
  --public-ip-address ""

```

## <a name="install-database"></a>Installieren der Datenbank

In diesem Tutorial kopieren Sie den privaten Schlüssel von Ihrem virtuellen Entwicklungscomputer in *myFrontendVM*. In einer Produktionsumgebung wird empfohlen, anstelle der Verwendung von „--generate-ssh-keys“ beim Erstellen der virtuellen Computer spezifische Schlüssel für die Verwendung auf den virtuellen Computern zu erstellen. 

Auf den virtuellen Back-End-Computer soll kein öffentlicher Zugriff erfolgen. In diesem Abschnitt erfahren Sie, wie Sie sich über SSH bei *myFrontendVM* anmelden und dann über SSH über *myFrontendVM* bei *myBackendVM* anmelden.

Ersetzen Sie die IP-Beispieladresse durch die öffentliche IP-Adresse von *myFrontendVM*:

```bash
scp ~/.ssh/id_rsa 40.68.254.142:~/.ssh/id_rsa
```

Erstellen Sie eine SSH-Verbindung mit *myFrontendVM*. Ersetzen Sie die IP-Beispieladresse durch die öffentliche IP-Adresse von *myFrontendVM*:

```bash
ssh 40.68.254.142
```

Stellen Sie auf *myFrontendVM* eine Verbindung mit *myBackendVM* her:

```bash
ssh myBackendVM
```

Führen Sie den folgenden Befehl aus, um MySQL zu installieren:

```bash
sudo apt-get -y install mysql-server
```

Befolgen Sie die Anweisungen zum Einrichten von MySQL.

Schließen Sie die SSH-Sitzungen:

```bash
exit
```

MySQL wird installiert, um zu zeigen, wie eine Anwendung auf *myBackendVM* installiert werden kann, in diesem Tutorial jedoch nicht verwendet.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Informationen zum Erstellen und Schützen von Azure-Netzwerken in Bezug auf virtuelle Computer erhalten. Im nächsten Tutorial erfahren Sie mehr über das Überwachen der VM-Sicherheit mit Azure Security Center.

[Verwalten der Sicherheit virtueller Computer](./tutorial-azure-security.md)
