---
title: Bereitstellen von Azure File Storage auf Linux-VMs per SMB | Microsoft-Dokumentation
description: Bereitstellen von Azure File Storage auf virtuellen Linux-Computern per SMB mithilfe der Azure CLI 2.0
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2017
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 279b165cbdb924dcae0cb95ad9f9d187aed1a1b3
ms.lasthandoff: 04/03/2017


---

# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>Bereitstellen von Azure File Storage auf Linux-VMs per SMB

In diesem Artikel wird beschrieben, wie Sie den Azure File Storage-Dienst auf einem virtuellen Linux-Computer über eine SMB-Bereitstellung mit der Azure CLI 2.0 verwenden. Der Azure-Dateispeicher verfügt über Dateifreigaben in der Cloud unter Verwendung des standardmäßigen SMB-Protokolls. Sie können diese Schritte auch mit der [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen. Folgende Anforderungen müssen erfüllt sein:

- [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)
- [Dateien mit den öffentlichen und privaten SSH-Schlüsseln](mac-create-ssh-keys.md)

## <a name="quick-commands"></a>Schnellbefehle

* Eine Ressourcengruppe
* Ein virtuelles Azure-Netzwerk
* Eine Netzwerksicherheitsgruppe mit eingehendem SSH-Datenverkehr
* Ein Subnetz
* Ein Azure-Speicherkonto
* Azure-Speicherkontoschlüssel
* Eine Azure File Storage-Freigabe
* Einen virtuellen Linux-Computer

Ersetzen Sie alle Beispiele durch Ihre eigenen Einstellungen.

### <a name="create-a-directory-for-the-local-mount"></a>Erstellen Sie ein Verzeichnis für die lokale Bereitstellung.

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-the-file-storage-smb-share-to-the-mount-point"></a>Bereitstellen der File Storage-SMB-Freigabe im Bereitstellungspunkt

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-the-mount-after-a-reboot"></a>Beibehalten der Bereitstellung nach einem Neustart
Fügen Sie zu diesem Zweck folgende Zeile zu `/etc/fstab` hinzu:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

File Storage bietet Dateifreigaben in der Cloud, die das standardmäßige SMB-Protokoll verwenden. Mit der neuesten Version von File Storage können Sie außerdem eine Dateifreigabe aus einem beliebigen Betriebssystem bereitstellen, das SMB 3.0 unterstützt. Wenn Sie eine SMB-Bereitstellung unter Linux verwenden, erhalten Sie einfache Sicherungen an einem robusten, dauerhaften Speicherort für die Archivierung, der durch eine Vereinbarung zum Servicelevel (SLA) unterstützt wird.

Das Verschieben von Dateien von einem virtuellen Computer auf eine in File Storage gehostete SMB-Bereitstellung ist eine gute Möglichkeit zum Debuggen von Protokollen. Dies liegt daran, dass ein und dieselbe SMB-Freigabe lokal auf Ihrer Mac-, Linux- oder Windows-Arbeitsstation bereitgestellt werden kann. SMB ist nicht die ideale Lösung zum Streamen von Linux- oder Anwendungsprotokollen in Echtzeit, da das SMB-Protokoll nicht auf so anspruchsvolle Protokollierungsaufgaben ausgelegt ist. Ein dediziertes Tool wie Fluentd mit vereinheitlichter Protokollierungsebene ist eine bessere Wahl als SMB, wenn es um das Erfassen der Linux- und Anwendungsprotokollausgabe geht.

Für diese ausführliche exemplarische Vorgehensweise schaffen wir die Voraussetzungen für die Erstellung der File Storage-Freigabe und die anschließende Bereitstellung per SMB auf einem virtuellen Linux-Computer.

1. Erstellen Sie mit [az group create](/cli/azure/group#create) eine Ressourcengruppe für die Dateifreigabe.

    Verwenden Sie das folgende Beispiel, um eine Ressourcengruppe mit dem Namen `myResourceGroup` in der Region „USA, Westen“ zu erstellen:

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. Erstellen Sie mit [az storage account create](/cli/azure/storage/account#create) ein Azure-Speicherkonto zum Speichern der eigentlichen Dateien.

    Um mithilfe der Standard_LRS-Speicher-SKU ein Speicherkonto namens „mystorageaccount“ zu erstellen, verwenden Sie folgendes Beispiel:

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. Zeigen Sie die Speicherkontoschlüssel an.

    Beim Erstellen eines Speicherkontos werden die Speicherkontoschlüssel als Paar erstellt, sodass eine Rotation der Schlüssel ohne Unterbrechung des Diensts möglich ist. Wenn Sie zum zweiten Schlüssel des Paars wechseln, erstellen Sie ein neues Schlüsselpaar. Neue Speicherkontoschlüssel werden stets als Paar erstellt, damit sichergestellt ist, dass immer mindestens ein ungenutzter Speicherkontoschlüssel vorhanden ist, zu dem gewechselt werden kann.

    Zeigen Sie mit [az storage account keys list](/cli/azure/storage/account/keys#list) die Speicherkontoschlüssel an. Im folgenden Beispiel werden die Speicherkontoschlüssel für das Speicherkonto `mystorageaccount` aufgelistet:

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    Verwenden Sie das Flag `--query`, um einen einzelnen Schlüssel zu extrahieren. Im folgenden Beispiel wird der erste Schlüssel (`[0]`) extrahiert:

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. Erstellen Sie die File Storage-Freigabe.

    Erstellen Sie mit [az storage share create](/cli/azure/storage/share#create) die File Storage-Freigabe, die die SMB-Freigabe enthält. Das Kontingent wird immer in Gigabyte (GB) angegeben. Übergeben Sie einen der Schlüssel aus dem vorherigen `az storage account keys list`-Befehl. Erstellen Sie mithilfe des folgenden Beispiels eine Freigabe namens „mystorageshare“ mit einem Kontingent von 10 GB:

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. Erstellen Sie ein Bereitstellungspunktverzeichnis.

    Erstellen Sie ein lokales Verzeichnis in dem Linux-Dateisystem, in dem die SMB-Freigabe bereitgestellt werden soll. Alle Elemente, die im lokalen Bereitstellungsverzeichnis geschrieben oder gelesen werden, werden an die in File Storage gehostete SMB-Freigabe weitergeleitet. Verwenden Sie das folgende Beispiel, um ein lokales Verzeichnis unter „/mnt/mymountdirectory“ zu erstellen:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. Stellen Sie die SMB-Freigabe in dem lokalen Verzeichnis bereit.

    Geben Sie wie folgt Ihren eigenen Benutzernamen und Speicherkontoschlüssel für das Speicherkonto als Anmeldeinformationen für die Bereitstellung an:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. Legen Sie fest, dass die SMB-Bereitstellung bei Neustarts beibehalten wird.

    Beim Neustart eines virtuellen Linux-Computers wird die Bereitstellung der SMB-Freigabe während des Herunterfahrens aufgehoben. Fügen Sie der Linux-Datei „etc/fstab“ eine Zeile hinzu, damit die SMB-Freigabe beim Starten erneut bereitgestellt wird. Linux verwendet die fstab-Datei, um die Dateisysteme aufzulisten, die während des Startvorgangs bereitgestellt werden müssen. Durch Hinzufügen der SMB-Freigabe wird sichergestellt, dass die File Storage-Freigabe für den virtuellen Linux-Computer ein dauerhaft bereitgestelltes Dateisystem ist. Das Hinzufügen der File Storage-SMB-Freigabe zu einem neuen virtuellen Computer ist möglich, wenn Sie „cloud-init“ verwenden.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Verschlüsseln von Datenträgern auf einem virtuellen Linux-Computer mithilfe der Azure-Befehlszeilenschnittstelle](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

