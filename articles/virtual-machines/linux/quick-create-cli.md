---
title: "Azure-Schnellstart – Erstellen einer VM mit der CLI | Microsoft-Dokumentation"
description: Hier lernen Sie schnell, virtuelle Computer mit der Azure CLI zu erstellen.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/03/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 08fcde4f5bddccb9de5564455937a637054ebb60
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---

# <a name="create-a-linux-virtual-machine-with-the-azure-cli"></a>Erstellen einer Linux-VM mit der Azure CLI

Die Azure CLI dient zum Erstellen und Verwalten von Azure-Ressourcen über die Befehlszeile oder mit Skripts. In dieser Anleitung wird die Bereitstellung einer VM mit Ubuntu 16.04 LTS mithilfe der Azure CLI ausführlich beschrieben. Nachdem der Server bereitgestellt wurde, stellen wir eine SSH-Verbindung mit dem virtuellen Computer her, um NGINX zu installieren. 

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

Stellen Sie auch sicher, dass die Azure-CLI installiert wurde. Weitere Informationen finden Sie im [Azure CLI-Installationshandbuch](https://docs.microsoft.com/cli/azure/install-azure-cli). 

## <a name="log-in-to-azure"></a>Anmelden an Azure 

Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm.

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Das folgende Beispiel erstellt eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *westeurope*.

```azurecli
az group create --name myResourceGroup --location westeurope
```

## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers

Erstellen Sie mit dem Befehl [az vm create](/cli/azure/vm#create) einen virtuellen Computer. 

Das folgende Beispiel erstellt einen virtuellen Computer mit dem Namen *myVM* und SSH-Schlüssel, falls sie nicht bereits an einem Standardschlüsselspeicherort vorhanden sind. Um einen bestimmten Satz von Schlüsseln zu verwenden, nutzen Sie die Option `--ssh-key-value`.  

```azurecli
az vm create --resource-group myResourceGroup --name myVM --image UbuntuLTS --generate-ssh-keys
```

Nach dem Erstellen der VM zeigt die Azure CLI ähnliche Informationen wie im folgenden Beispiel an. Notieren Sie sich den Wert von `publicIpAddress`. Diese Adresse wird verwendet, um auf den virtuellen Computer zuzugreifen.

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westeurope",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="open-port-80-for-web-traffic"></a>Öffnen von Port 80 für Webdatenverkehr 

Standardmäßig sind für in Azure bereitgestellte virtuelle Linux-Computer nur eingehende SSH-Verbindungen zulässig. Wenn dieser virtuelle Computer als Webserver fungieren soll, muss Port 80 für Datenverkehr aus dem Internet geöffnet werden. Verwenden Sie zum Öffnen des gewünschten Ports den Befehl [az vm open-port](/cli/azure/vm#open-port).  
 
 ```azurecli 
az vm open-port --port 80 --resource-group myResourceGroup --name myVM
```

## <a name="ssh-into-your-vm"></a>Herstellen einer SSH-Verbindung mit Ihrem virtuellen Computer

Erstellen Sie mit dem folgenden Befehl eine SSH-Sitzung mit dem virtuellen Computer. Ersetzen Sie *<publicIpAddress>* durch die korrekte öffentliche IP-Adresse Ihres virtuellen Computers.  In unserem Beispiel von weiter oben lautete die IP-Adresse *40.68.254.142*.

```bash 
ssh <publicIpAddress>
```

## <a name="install-nginx"></a>Installieren von NGINX

Verwenden Sie das folgende Bash-Skript, um Paketquellen zu aktualisieren und das neueste NGINX-Paket zu installieren. 

```bash 
#!/bin/bash

# update package source
apt-get -y update

# install NGINX
apt-get -y install nginx
```

## <a name="view-the-ngix-welcome-page"></a>Anzeigen der NGIX-Willkommensseite

Nachdem Sie NGINX installiert und Port 80 auf Ihrem virtuellen Computer für den Zugriff über das Internet geöffnet haben, können Sie nun mit einem Webbrowser Ihrer Wahl die NGINX-Standardwillkommensseite anzeigen. Verwenden Sie dabei den weiter oben dokumentierten *publicIpAddress*-Wert, um die Standardseite zu besuchen. 

![NGINX-Standardwebsite](./media/quick-create-cli/nginx.png) 


## <a name="delete-virtual-machine"></a>Löschen des virtuellen Computers

Wenn Ressourcengruppe, virtueller Computer und alle zugehörigen Ressourcen nicht mehr benötigt werden, können Sie sie mit dem Befehl [az group delete](/cli/azure/group#delete) entfernen.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nächste Schritte

[Erstellen einer vollständigen Linux-Umgebung mit der Azure CLI 2.0](create-cli-complete.md)

[Azure CLI-Beispiele für Linux-VMs](cli-samples.md)

