---
title: Verwenden von Ansible zum Erstellen einer einfachen Linux-VM in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ansible zum Erstellen und Verwalten eines einfachen virtuellen Linux-Computers in Azure verwenden.
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
ms.openlocfilehash: 526f3522334450564500c4ba0e401a683cae55f6
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="create-a-basic-virtual-machine-in-azure-with-ansible"></a>Erstellen eines einfachen virtuellen Computers in Azure mit Ansible
Ansible ermöglicht die Automatisierung der Bereitstellung und Konfiguration von Ressourcen in Ihrer Umgebung. Sie können mit Ansible Ihre virtuellen Computer (VMs) in Azure wie jede andere Ressource verwalten. In diesem Artikel wird gezeigt, wie Sie einen einfachen virtuellen Computer mit Ansible erstellen. Sie können sich auch darüber informieren, wie Sie [eine vollständige Umgebung mit virtuellen Computern mit Ansible erstellen](ansible-create-complete-vm.md).


## <a name="prerequisites"></a>Voraussetzungen
Um Azure-Ressourcen mit Ansible verwalten zu können, benötigen Sie Folgendes:

- Ansible und die Azure Python-SDK-Module auf dem Hostsystem
    - Installieren von Ansible unter [Ubuntu 16.04 LTS](ansible-install-configure.md#ubuntu-1604-lts), [CentOS 7.3](ansible-install-configure.md#centos-73) und [SLES 12.2 SP2](ansible-install-configure.md#sles-122-sp2)
- Azure-Anmeldeinformationen und eine für deren Verwendung konfigurierte Ansible-Instanz
    - [Erstellen von Azure-Anmeldeinformationen und Konfigurieren von Ansible](ansible-install-configure.md#create-azure-credentials)
- Azure-Befehlszeilenschnittstelle, Version 2.0.4 oder höher. Führen Sie `az --version` aus, um die Version zu finden. 
    - Wenn Sie ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. Sie können auch [Cloud Shell](/azure/cloud-shell/quickstart) in Ihrem Browser verwenden.


## <a name="create-supporting-azure-resources"></a>Erstellen unterstützender Azure-Ressourcen
In diesem Beispiel erstellen wir ein Runbook, das einen virtuellen Computer in einer vorhandenen Infrastruktur bereitstellt. Erstellen Sie zunächst mit [az group create](/cli/azure/vm#create) eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurecli
az group create --name myResourceGroup --location eastus
```

Erstellen Sie mit [az network vnet create](/cli/azure/network/vnet#create) ein virtuelles Netzwerk für den virtuellen Computer. Im folgenden Beispiel werden ein virtuelles Netzwerk mit dem Namen *myVnet* und ein Subnetz mit dem Namen *mySubnet* erstellt:

```azurecli
az network vnet create \
  --resource-group myResourceGroup \
  --name myVnet \
  --address-prefix 10.0.0.0/16 \
  --subnet-name mySubnet \
  --subnet-prefix 10.0.1.0/24
```


## <a name="create-and-run-ansible-playbook"></a>Erstellen und Ausführen von Ansible-Playbooks
Erstellen Sie das Ansible-Playbook **azure_create_vm.yml**, und fügen Sie den folgenden Inhalt ein. In diesem Beispiel wird ein einzelner virtueller Computer erstellt, und es werden SSH-Anmeldeinformationen konfiguriert. Geben Sie Ihre eigenen öffentlichen Schlüsseldaten wie folgt im *key_data*-Paar ein:

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
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
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.3'
        version: latest
```

Um den virtuellen Computer mit Ansible zu erstellen, führen Sie das Playbook wie folgt aus:

```bash
ansible-playbook azure_create_vm.yml
```

Die Ausgabe sieht etwa wie im folgenden Beispiel aus, das zeigt, dass der virtuelle Computer erfolgreich erstellt wurde:

```bash
PLAY [Create Azure VM] ****************************************************

TASK [Gathering Facts] ****************************************************
ok: [localhost]

TASK [Create VM] **********************************************************
changed: [localhost]

PLAY RECAP ****************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
```


## <a name="next-steps"></a>Nächste Schritte
Dieses Beispiel erstellt einen virtuellen Computer in einer vorhandener Ressourcengruppe und mit einem bereits bereitgestellten virtuellen Netzwerk. Ein ausführlicheres Beispiel zur Verwendung von Ansible zum Erstellen der unterstützenden Ressourcen, wie z.B. eines virtuellen Netzwerks und der Regeln für die Netzwerksicherheitsgruppe, finden Sie unter [Erstellen einer vollständigen VM-Umgebung mit Ansible](ansible-create-complete-vm.md).
