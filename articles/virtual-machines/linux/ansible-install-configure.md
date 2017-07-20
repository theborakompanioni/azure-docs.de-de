---
title: "Installieren und Konfigurieren von Ansible für die Verwendung mit virtuellen Azure-Computern | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ansible für die Verwaltung von Azure-Ressourcen unter Ubuntu, CentOS und SLES installieren und konfigurieren."
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
ms.openlocfilehash: 52b763274437961dccfc862c8a45fbd57ea9fc4e
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017

---

# <a name="install-and-configure-ansible-to-manage-virtual-machines-in-azure"></a>Installieren und Konfigurieren von Ansible für das Verwalten von virtuellen Computern in Azure
In diesem Artikel wird erläutert, wie Sie Ansible und die erforderlichen Python-SDK-Module für Azure unter einigen der am häufigsten verwendeten Linux-Distributionen installieren. Sie können Ansible auf anderen Distributionen installieren, indem Sie die installierten Pakete entsprechend an Ihre Plattform anpassen. Damit Sie Azure-Ressourcen auf sichere Weise erstellen können, erfahren Sie auch, wie Sie Anmeldeinformationen für Ansible erstellen und definieren. 

Weitere Installationsoptionen und Anleitungen für zusätzliche Plattformen finden Sie im [Ansible-Installationshandbuch](https://docs.ansible.com/ansible/intro_installation.html).


## <a name="install-ansible"></a>Installieren von Ansible
Erstellen Sie zunächst mit [az group create](/cli/azure/group#create) eine Ressourcengruppe. Das folgende Beispiel erstellt die Ressourcengruppe *myResourceGroupAnsible* am Standort *eastus*:

```azurecli
az group create --name myResourceGroupAnsible --location eastus
```

Nun erstellen Sie einen virtuellen Computer und installieren Ansible für eine der folgenden Distributionen:

- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [CentOS 7.3](#centos-73)
- [SLES 12.2 SP2](#sles-122-sp2)

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS
Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer. Im folgenden Beispiel wird der virtuelle Computer *myVMAnsible* erstellt:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Stellen Sie per SSH mithilfe der `publicIpAddress` aus der Ausgabe von der Erstellung des virtuellen Computers eine Verbindung mit diesem her:

```bash
ssh azureuser@<publicIpAddress>
```

Installieren Sie die erforderlichen Pakete für die Python-SDK-Module für Azure sowie Ansible wie folgt auf dem virtuellen Computer:

```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Azure SDKs via pip
pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via apt
sudo apt-get install -y software-properties-common
sudo apt-add-repository -y ppa:ansible/ansible
sudo apt-get update && sudo apt-get install -y ansible
```

Fahren Sie anschließend mit dem [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials) fort.


### <a name="centos-73"></a>CentOS 7.3
Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer. Im folgenden Beispiel wird der virtuelle Computer *myVMAnsible* erstellt:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image CentOS \
    --admin-username azureuser \
    --generate-ssh-keys
```

Stellen Sie per SSH mithilfe der `publicIpAddress` aus der Ausgabe von der Erstellung des virtuellen Computers eine Verbindung mit diesem her:

```bash
ssh azureuser@<publicIpAddress>
```

Installieren Sie die erforderlichen Pakete für die Python-SDK-Module für Azure sowie Ansible wie folgt auf dem virtuellen Computer:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Azure SDKs via pip
sudo pip install "azure==2.0.0rc5" msrestazure

## Install Ansible via yum
sudo yum install -y ansible
```

Fahren Sie anschließend mit dem [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials) fort.


### <a name="sles-122-sp2"></a>SLES 12.2 SP2
Erstellen Sie mit [az vm create](/cli/azure/vm#create) einen virtuellen Computer. Im folgenden Beispiel wird der virtuelle Computer *myVMAnsible* erstellt:

```bash
az vm create \
    --name myVMAnsible \
    --resource-group myResourceGroupAnsible \
    --image SLES \
    --admin-username azureuser \
    --generate-ssh-keys
```

Stellen Sie per SSH mithilfe der `publicIpAddress` aus der Ausgabe von der Erstellung des virtuellen Computers eine Verbindung mit diesem her:

```bash
ssh azureuser@<publicIpAddress>
```

Installieren Sie die erforderlichen Pakete für die Python-SDK-Module für Azure sowie Ansible wie folgt auf dem virtuellen Computer:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 python-devel \
    libopenssl-devel python-pip python-setuptools python-azure-sdk

## Install Ansible via zypper
sudo zypper addrepo http://download.opensuse.org/repositories/systemsmanagement/SLE_12_SP2/systemsmanagement.repo
sudo zypper refresh && sudo zypper install ansible
```

Fahren Sie anschließend mit dem [Erstellen von Azure-Anmeldeinformationen](#create-azure-credentials) fort.


## <a name="create-azure-credentials"></a>Erstellen von Azure-Anmeldeinformationen
Ansible kommuniziert mit Azure unter Verwendung von Benutzername und Kennwort oder eines Dienstprinzipals. Ein Azure-Dienstprinzipal ist eine Sicherheitsidentität, die Sie mit Apps, Diensten und Automatisierungstools wie Ansible verwenden können. Sie steuern und definieren die Berechtigungen hinsichtlich der Vorgänge, die der Dienstprinzipal in Azure ausführen können soll. Zur Erhöhung der Sicherheit über die Bereitstellung eines Benutzernamens und Kennworts hinaus wird in diesem Beispiel ein einfacher Dienstprinzipal erstellt.

Erstellen Sie mit [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) einen Dienstprinzipalnamen, und geben Sie die Anmeldeinformationen aus, die Ansible benötigt:

```azurecli
az ad sp create-for-rbac --query [appId,password,tenant]
```

Ein Beispiel der Ausgabe der vorherigen Befehle lautet wie folgt:

```json
[
  "eec5624a-90f8-4386-8a87-02730b5410d5",
  "531dcffa-3aff-4488-99bb-4816c395ea3f",
  "72f988bf-86f1-41af-91ab-2d7cd011db47"
]
```

Zur Authentifizierung bei Azure müssen Sie auch Ihre Azure-Abonnement-ID mit [az account show](/cli/azure/account#show) abrufen:

```azurecli
az account show --query [id] --output tsv
```

Die Ausgabe dieser beiden Befehle verwenden Sie im nächsten Schritt.


## <a name="create-ansible-credentials-file"></a>Erstellen einer Ansible-Anmeldeinformationendatei
Zum Angeben von Anmeldeinformationen für Ansible definieren Sie Umgebungsvariablen oder erstellen eine lokale Datei mit den Anmeldeinformationen. Weitere Informationen zum Definieren von Ansible-Anmeldeinformationen finden Sie unter [Bereitstellen von Anmeldeinformationen für Azure-Module](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules). 

Erstellen Sie für eine Entwicklungsumgebung wie folgt eine Datei mit *Anmeldeinformationen* für Ansible auf der Host-VM:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

Die Datei mit den *Anmeldeinformationen* selbst kombiniert die Abonnement-ID mit der Ausgabe der Erstellung eines Dienstprinzipals. Die Ausgabe aus dem vorherigen Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) entspricht der erforderlichen Reihenfolge von *client_id*, *secret* und *tenant*. Das folgende Beispiel zeigt die Werte aus der vorherigen Ausgabe in der Datei mit den *Anmeldeinformationen*. Geben Sie Ihre eigenen Werte wie folgt ein:

```bash
[default]
subscription_id=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
client_id=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
secret=b8326643-f7e9-48fb-b0d5-952b68ab3def
tenant=72f988bf-86f1-41af-91ab-2d7cd011db47
```


## <a name="use-ansible-environment-variables"></a>Verwenden von Ansible-Umgebungsvariablen
Wenn Sie Tools wie Ansible Tower oder Jenkins verwenden möchten, können Sie wie folgt Umgebungsvariablen definieren. Diese Variablen kombinieren die Abonnement-ID mit der Ausgabe aus der Erstellung eines Dienstprinzipals. Die Ausgabe aus dem vorherigen Befehl [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) entspricht der erforderlichen Reihenfolge von *AZURE_CLIENT_ID*, *AZURE_SECRET* und *AZURE_TENANT*. 

```bash
export AZURE_SUBSCRIPTION_ID=xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
export AZURE_CLIENT_ID=66cf7166-dd13-40f9-bca2-3e9a43f2b3a4
export AZURE_SECRET=8326643-f7e9-48fb-b0d5-952b68ab3def
export AZURE_TENANT=72f988bf-86f1-41af-91ab-2d7cd011db47
```

## <a name="next-steps"></a>Nächste Schritte
Sie haben nun Ansible und die erforderlichen Python-SDK-Module für Azure installiert und Anmeldeinformationen für Ansible definiert. Erfahren Sie, wie Sie [einen virtuellen Computer mit Ansible erstellen](ansible-create-vm.md). Sie können sich auch darüber informieren, wie Sie [eine vollständige Azure-VM mit den zugehörigen Ressourcen mit Ansible erstellen](ansible-create-complete-vm.md).
