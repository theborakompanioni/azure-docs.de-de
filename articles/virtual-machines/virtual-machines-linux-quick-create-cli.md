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
ms.date: 09/26/2016
ms.author: rasquill
translationtype: Human Translation
ms.sourcegitcommit: 2bd363e3c22f4cf4daf2e0fa352fd4a131d1675f
ms.openlocfilehash: 89db2c9f388b8a5496a306ba0a152ab57481ea50


---

# <a name="create-a-linux-vm-using-the-azure-cli-20-preview"></a>Erstellen eines virtuellen Linux-Computers mithilfe der Azure-Befehlszeilenschnittstelle 2.0 (Vorschau)
In diesem Artikel erfahren Sie, wie Sie mithilfe des Befehls [az vm create](/cli/azure/vm#create) über die Azure-Befehlszeilenschnittstelle 2.0 (Vorschau) im Handumdrehen einen virtuellen Linux-Computer bereitstellen. 

> [!NOTE] 
> Bei der Vorschauversion der Azure-Befehlszeilenschnittstelle 2.0 handelt es sich um die nächste Generation unserer plattformübergreifenden Befehlszeilenschnittstelle. Probieren Sie sie aus, und teilen Sie uns auf der [GitHub-Projektseite](https://github.com/Azure/azure-cli) Ihre Meinung mit.
>
> In der übrigen Dokumentation wird die bereits vorhandene Azure-Befehlszeilenschnittstelle verwendet. Wenn Sie einen virtuellen Computer nicht mit der Vorschauversion der Befehlszeilenschnittstelle 2.0, sondern mit der bereits vorhandenen Azure-Befehlszeilenschnittstelle erstellen möchten, lesen Sie unter [Create a VM with the Azure CLI](virtual-machines-linux-quick-create-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Erstellen eines virtuellen Computers mit der Azure-Befehlszeilenschnittstelle) weiter.

Zum Erstellen eines virtuellen Computers ist Folgendes erforderlich: 

* Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))
* Installierte [Azure-Befehlszeilenschnittstelle 2.0 (Vorschau)](https://github.com/Azure/azure-cli#installation)
* Anmeldung bei Ihrem Azure-Konto ([az login](/cli/azure/#login))

(Virtuelle Computer können auch schnell über das [Azure-Portal](virtual-machines-linux-quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bereitgestellt werden.)

Das folgende Beispiel zeigt, wie Sie einen virtuellen Debian-Computer bereitstellen und Ihren SSH-Schlüssel (Secure Shell) anfügen. (Ihre Argumente können von den hier angegebenen Argumenten abweichen. Falls Sie ein anderes Image verwenden möchten, können Sie [nach einem Image suchen](virtual-machines-linux-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).)

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Geben Sie zunächst [az resource group create](/cli/azure/resource/group#create) ein, um Ihre Ressourcengruppe zu erstellen, die alle bereitgestellten Ressourcen enthält:

```azurecli
az resource group create -n myResourceGroup -l westus
```

Die Ausgabe sieht wie folgt aus. (Sie können auch eine andere `--output`-Option verwenden.)

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "westus",
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="create-your-vm-using-the-latest-debian-image"></a>Erstellen Ihres virtuellen Computers unter Verwendung des neuesten Debian-Images

Nun können Sie Ihren virtuellen Computer und dessen Umgebung erstellen. Ersetzen Sie dabei den Wert `----public-ip-address-dns-name` durch einen eindeutigen Wert. (Der hier angegebene Wert wird möglicherweise bereits verwendet.)

```azurecli
az vm create \
--image credativ:Debian:8:latest \
--admin-username ops \
--ssh-key-value ~/.ssh/id_rsa.pub \
--public-ip-address-dns-name mydns \
--resource-group myResourceGroup \
--location westus \
--name myVM
```


Die Ausgabe sieht wie folgt aus. Notieren Sie sich entweder den `publicIpAddress`- oder den `fqdn`-Wert, um eine **ssh**-Verbindung mit Ihrem virtuellen Computer herzustellen.


```json
{
  "fqdn": "mydns.westus.cloudapp.azure.com",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "macAddress": "00-0D-3A-32-05-07",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.112.217.29",
  "resourceGroup": "myResourceGroup"
}
```

Melden Sie sich mit der in der Ausgabe aufgeführten öffentlichen IP-Adresse bei Ihrer VM an. Sie können auch den vollständig qualifizierten Domänennamen (FQDN) verwenden, der in der Ausgabe angegeben ist.

```bash
ssh ops@mydns.westus.cloudapp.azure.com
```

Die Ausgabe ist abhängig von der gewählten Distribution, sollte aber in etwa wie folgt aussehen:

```
The authenticity of host 'mydns.westus.cloudapp.azure.com (40.112.217.29)' can't be established.
RSA key fingerprint is SHA256:xbVC//lciRvKild64lvup2qIRimr/GB8C43j0tSHWnY.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'mydns.westus.cloudapp.azure.com,40.112.217.29' (RSA) to the list of known hosts.

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
ops@mynewvm:~$ ls /
bin  boot  dev  etc  home  initrd.img  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vmlinuz
```

## <a name="next-steps"></a>Nächste Schritte
Der Befehl `az vm create` ermöglicht die schnelle Bereitstellung einer VM – Sie können sich bei einer Bash-Shell anmelden und mit der Arbeit beginnen. Bei der Verwendung von `az vm create` haben Sie jedoch weder umfassende Kontrolle, noch können Sie eine komplexere Umgebung erstellen.  Informationen zum Bereitstellen einer für Ihre Infrastruktur angepassten Linux-VM finden Sie in den Artikeln zu folgenden Themen:

* [Bereitstellen und Verwalten von virtuellen Computern mit Azure-Ressourcen-Manager-Vorlagen und der Azure-CLI](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Direktes Erstellen einer benutzerdefinierten Umgebung für einen virtuellen Linux-Computer über Azure-CLI-Befehle](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Erstellen einer SSH-geschützten Linux-VM mit einer Azure-Vorlage](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Sie können auch [den Azure-Treiber `docker-machine` mit verschiedenen Befehlen verwenden, um schnell einen virtuellen Linux-Computer als Docker-Host zu erstellen](virtual-machines-linux-docker-machine.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Bei Verwendung von Java können Sie die [create()](/java/api/com.microsoft.azure.management.compute._virtual_machine)-Methode verwenden.




<!--HONumber=Nov16_HO4-->


