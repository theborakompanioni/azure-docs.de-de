---
title: Erstellen eines virtuellen Linux-Computers mithilfe der Azure-Befehlszeilenschnittstelle 2.0 (Vorschau) | Microsoft Azure
description: Erstellen Sie einen virtuellen Linux-Computer mithilfe der Azure-Befehlszeilenschnittstelle 2.0 (Vorschau).
services: virtual-machines-linux
documentationcenter: 
author: squillace
manager: timlt
editor: 
ms.assetid: 82005a05-053d-4f52-b0c2-9ae2e51f7a7e
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/13/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 1081eb18bd63b1ad580f568201e03258901e4eaf
ms.openlocfilehash: e926f22b94da30e1d3b790432ffdc229d9f4e609
ms.lasthandoff: 02/22/2017


---

# <a name="create-a-linux-vm-using-the-azure-cli-20-preview-azpy"></a>Erstellen eines virtuellen Linux-Computers mithilfe von Azure-CLI 2.0 Preview (az.py)
In diesem Artikel erfahren Sie, wie Sie mithilfe des Befehls [az vm create](/cli/azure/vm#create) über die Azure-Befehlszeilenschnittstelle 2.0 (Vorschau) im Handumdrehen einen virtuellen Linux-Computer mit verwalteten Datenträgern und mit Datenträgern in nativen Speicherkonten bereitstellen.

> [!NOTE] 
> Bei der Vorschauversion der Azure-Befehlszeilenschnittstelle 2.0 handelt es sich um die nächste Generation unserer plattformübergreifenden Befehlszeilenschnittstelle. [Probieren Sie sie aus.](https://docs.microsoft.com/cli/azure/install-az-cli2)
>
> Wenn Sie einen virtuellen Computer nicht mit der Vorschauversion der Azure-Befehlszeilenschnittstelle 2.0, sondern mit der bereits vorhandenen Azure CLI 1.0 erstellen möchten, lesen Sie unter [Erstellen einer Linux-VM von Grund auf mit der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) weiter.

Zum Erstellen eines virtuellen Computers ist Folgendes erforderlich: 

* Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))
* Installierte [Azure-Befehlszeilenschnittstelle 2.0 (Vorschau)](/cli/azure/install-az-cli2)
* Anmeldung bei Ihrem Azure-Konto ([az login](/cli/azure/#login))

(Ein virtueller Linux-Computer kann auch über das [Azure-Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bereitgestellt werden.)

Das folgende Beispiel zeigt, wie Sie einen virtuellen Debian-Computer bereitstellen und unter Verwendung eines SSH-Schlüssels (Secure Shell) eine Verbindung damit herstellen. Ihre Argumente können sich ggf. unterscheiden. Falls Sie ein anderes Image verwenden möchten, können Sie [nach einem Image suchen](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="using-managed-disks"></a>Verwenden verwalteter Datenträger

Wenn Sie Azure Managed Disks verwenden möchten, müssen Sie eine Region verwenden, die dieses Feature unterstützt. Geben Sie zunächst [az group create](/cli/azure/group#create) ein, um Ihre Ressourcengruppe zu erstellen, die alle bereitgestellten Ressourcen enthält:

```azurecli
 az group create -n myResourceGroup -l westus
```

Die Ausgabe sieht wie folgt aus. (Sie können auch eine andere `--output`-Option angeben, um ein anderes Format zu erhalten.)

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```
### <a name="create-your-vm"></a>Erstellen Ihres virtuellen Computers 
Nun können Sie Ihren virtuellen Computer und dessen Umgebung erstellen. Ersetzen Sie dabei den Wert `--public-ip-address-dns-name` durch einen eindeutigen Wert. (Der hier angegebene Wert wird möglicherweise bereits verwendet.)

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name manageddisks \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


Die Ausgabe sieht wie folgt aus. Notieren Sie sich entweder den `publicIpAddress`- oder den `fqdn`-Wert, um eine **ssh**-Verbindung mit Ihrem virtuellen Computer herzustellen.


```json
{
  "fqdn": "manageddisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-E9-41",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "104.42.127.53",
  "resourceGroup": "myResourceGroup"
}
```

Melden Sie sich bei Ihrem virtuellen Computer an. Verwenden Sie dabei entweder die öffentliche IP-Adresse oder den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) aus der Ausgabe.

```bash
ssh ops@manageddisks.westus.cloudapp.azure.com
```

Die Ausgabe ist abhängig von der gewählten Distribution, sollte aber in etwa wie folgt aussehen:

```bash
The authenticity of host 'manageddisks.westus.cloudapp.azure.com (134.42.127.53)' can't be established.
RSA key fingerprint is c9:93:f5:21:9e:33:78:d0:15:5c:b2:1a:23:fa:85:ba.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'manageddisks.westus.cloudapp.azure.com' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Fri Jan 13 14:44:21 2017 from net-37-117-240-123.cust.vodafonedsl.it
ops@myVM:~$ 
```

Weitere Möglichkeiten mit verwalteten Datenträgern für Ihren neuen virtuellen Computer finden Sie unter [Nächste Schritte](#next-steps).

## <a name="using-unmanaged-disks"></a>Verwenden nicht verwalteter Datenträger 

VMs, für die nicht verwaltete Speicherdatenträger verwendet werden, verfügen über nicht verwaltete Speicherkonten. Geben Sie zunächst [az group create](/cli/azure/group#create) ein, um Ihre Ressourcengruppe für alle bereitgestellten Ressourcen zu erstellen:

```azurecli
az group create --name nativedisks --location westus
```

Die Ausgabe sieht wie folgt aus. (Sie können auch eine andere `--output`-Option verwenden.)

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks",
  "location": "westus",
  "managedBy": null,
  "name": "nativedisks",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

### <a name="create-your-vm"></a>Erstellen Ihres virtuellen Computers 

Nun können Sie Ihren virtuellen Computer und dessen Umgebung erstellen. Verwenden Sie das Flag `--use-unmanaged-disk`, um die VM mit nicht verwalteten Datenträgern zu erstellen. Außerdem wird ein nicht verwaltetes Speicherkonto erstellt. Ersetzen Sie dabei den Wert `--public-ip-address-dns-name` durch einen eindeutigen Wert. (Der hier angegebene Wert wird möglicherweise bereits verwendet.)

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username azureuser \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name nativedisks \
--resource-group nativedisks \
--location westus \
--name myVM \
--use-unmanaged-disk
```

Die Ausgabe sieht wie folgt aus. Notieren Sie sich entweder den `publicIpAddress`- oder den `fqdn`-Wert, um eine **ssh**-Verbindung mit Ihrem virtuellen Computer herzustellen.

```json
{
  "fqdn": "nativedisks.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/nativedisks/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-33-24-3C",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.91.91.195",
  "resourceGroup": "nativedisks"
}
```

Melden Sie sich bei Ihrem virtuellen Computer an. Verwenden Sie dabei die öffentliche IP-Adresse oder den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) aus der obigen Ausgabe.

```bash
ssh ops@nativedisks.westus.cloudapp.azure.com
```

Die Ausgabe ist abhängig von der gewählten Distribution, sollte aber in etwa wie folgt aussehen:

```
The authenticity of host 'nativedisks.westus.cloudapp.azure.com (13.91.93.195)' can't be established.
RSA key fingerprint is 3f:65:22:b9:07:c9:ef:7f:8c:1b:be:65:1e:86:94:a2.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'nativedisks.westus.cloudapp.azure.com,13.91.93.195' (RSA) to the list of known hosts.
Enter passphrase for key '/home/ops/.ssh/id_rsa':

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@myVM:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Nächste Schritte
Der Befehl `az vm create` ermöglicht die schnelle Bereitstellung einer VM – Sie können sich bei einer Bash-Shell anmelden und mit der Arbeit beginnen. Bei der Verwendung von `az vm create` haben Sie jedoch weder umfassende Kontrolle, noch können Sie eine komplexere Umgebung erstellen.  Informationen zum Bereitstellen einer für Ihre Infrastruktur angepassten Linux-VM finden Sie in den Artikeln zu folgenden Themen:

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Direktes Erstellen einer benutzerdefinierten Umgebung für einen virtuellen Linux-Computer über Azure-CLI-Befehle](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer SSH-geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Sie können auch [den Azure-Treiber `docker-machine` mit verschiedenen Befehlen verwenden, um schnell einen virtuellen Linux-Computer als Docker-Host zu erstellen](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bei Verwendung von Java können Sie die [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine)-Methode verwenden.


