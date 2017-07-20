---
title: "Verwenden von Ansible zum Erstellen einer vollständigen Linux-VM in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ansible zum Erstellen und Verwalten einer vollständigen Linux-VM-Umgebung in Azure verwenden."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: b2fcc288b40c12a9b3f966156ee2eedf4acca313
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="create-a-complete-linux-virtual-machine-environment-in-azure-with-ansible"></a>Erstellen einer vollständigen Linux-VM-Umgebung in Azure mit Ansible
Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können mit Ansible Ihre virtuellen Computer (VMs) in Azure wie jede andere Ressource verwalten. In diesem Artikel wird gezeigt, wie Sie eine vollständige Linux-Umgebung erstellen und Ressourcen mit Ansible unterstützen. Sie können sich auch darüber informieren, wie Sie [einen einfachen virtuellen Computer mit Ansible erstellen](ansible-create-vm.md).


## <a name="prerequisites"></a>Voraussetzungen
Um Azure-Ressourcen mit Ansible verwalten zu können, benötigen Sie Folgendes:

- Ansible und die Azure Python-SDK-Module auf dem Hostsystem
    - Installieren von Ansible unter [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73) und [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)
- Azure-Anmeldeinformationen und eine für deren Verwendung konfigurierte Ansible-Instanz
    - [Erstellen von Azure-Anmeldeinformationen und Konfigurieren von Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure-Befehlszeilenschnittstelle, Version 2.0.4 oder höher. Führen Sie `az --version` aus, um die Version zu finden. 
    - Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. Sie können auch [Cloud Shell](/azure/cloud-shell/quickstart) in Ihrem Browser verwenden.


## <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen
Der folgende Abschnitt in einem Ansible-Playbook erstellt ein virtuelles Netzwerk mit dem Namen *myVnet* im Adressbereich *10.0.0.0/16*:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.10.0.0/16"
```

Um ein Subnetz hinzuzufügen, erstellt der folgende Abschnitt das Subnetz *mySubnet* im virtuellen Netzwerk *myVnet*:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```


## <a name="create-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse
Für den Zugriff auf Ressourcen über das Internet erstellen Sie eine öffentliche IP-Adresse für Ihren virtuellen Computer und weisen sie zu. Im folgenden Abschnitt eines Ansible-Playbooks wird die öffentliche IP-Adresse *myPublicIP* erstellt:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```


## <a name="create-network-security-group"></a>Erstellen einer Netzwerksicherheitsgruppe
Netzwerksicherheitsgruppen steuern den ein- und ausgehenden Netzwerkdatenverkehr des virtuellen Computers. Im folgenden Abschnitt eines Ansible-Playbooks wird die Netzwerksicherheitsgruppe *myNetworkSecurityGroup* erstellt und eine Regel zum Zulassen von SSH-Datenverkehr über den TCP-Port 22 definiert:

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: TCP
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-virtual-network-interface-card"></a>Erstellen virtueller Netzwerkschnittstellenkarten
Eine virtuelle Netzwerkschnittstellenkarte (NIC) verbindet Ihren virtuellen Computer mit einem angegebenen virtuellen Netzwerk, einer öffentlichen IP-Adresse und einer Netzwerksicherheitsgruppe. Im folgenden Abschnitt eines Ansible-Playbooks wird die virtuelle Netzwerkkarte *myNIC* erstellt, die mit den von Ihnen erstellen virtuellen Netzwerkressourcen verbunden ist:

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```


## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Der letzte Schritt besteht im Erstellen eines virtuellen Computers und dem Verwenden der erstellten Ressourcen. Im folgenden Abschnitt eines Ansible-Playbooks wird der virtuelle Computer *myVM* erstellt und die virtuelle NIC *myNIC* angefügt. Geben Sie Ihre eigenen öffentlichen Schlüsseldaten wie folgt im *key_data*-Paar ein:

```yaml
- name: Create VM
  azure_rm_virtualmachine:
    resource_group: myResourceGroup
    name: myVM
    vm_size: Standard_DS1_v2
    admin_username: azureuser
    ssh_password_enabled: false
    ssh_public_keys: 
      - path: /home/azureuser/.ssh/authorized_keys
        key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.3'
      version: latest
```

## <a name="complete-ansible-playbook"></a>Vollständiges Ansible-Playbook
Um alle diese Abschnitte zusammenzuführen, erstellen Sie das Ansible-Playbook *azure_create_complete_vm.yml* und fügen den folgenden Inhalt ein:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys: 
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

Ansible benötigt eine Ressourcengruppe, in der alle Ressourcen bereitgestellt werden. Erstellen Sie mit [az group create](/cli/azure/vm#create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli
az group create --name myResourceGroup --location eastus
```

Um die vollständige VM-Umgebung mit Ansible zu erstellen, führen Sie das Playbook wie folgt aus:

```bash
ansible-playbook azure_create_complete_vm.yml
```

Die Ausgabe sieht etwa wie im folgenden Beispiel aus, das zeigt, dass der virtuelle Computer erfolgreich erstellt wurde:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create virtual network] *********************************************
changed: [localhost]

TASK [Add subnet] *********************************************************
changed: [localhost]

TASK [Create public IP address] *******************************************
changed: [localhost]

TASK [Create Network Security Group that allows SSH] **********************
changed: [localhost]

TASK [Create virtual network inteface card] *******************************
changed: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=7    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Nächste Schritte
In diesem Beispiel wird eine vollständige VM-Umgebung einschließlich der erforderlichen Ressourcen für das virtuelle Netzwerk erstellt. Ein noch direkteres Beispiel für das Erstellen eines virtuellen Computers in vorhandenen Netzwerkressourcen mit Standardoptionen finden Sie unter [Erstellen eines virtuellen Computers](ansible-create-vm.md).
