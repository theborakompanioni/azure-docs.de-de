---
title: Bereitstellen von Azure File Storage auf virtuellen Linux-Computern per SMB mit der Azure CLI 1.0 | Microsoft-Dokumentation
description: Bereitstellen von Azure File Storage auf virtuellen Linux-Computern per SMB
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
ms.date: 12/07/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 4951860630f0aad107d0846d52ebe4423ee0b91c
ms.lasthandoff: 04/03/2017


---

# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a>Bereitstellen von Azure File Storage auf virtuellen Linux-Computern per SMB mit der Azure CLI 1.0

Dieser Artikel zeigt, wie Sie Azure File Storage mithilfe des SMB-Protokolls (Server Message Block) auf einem virtuellen Linux-Computer bereitstellen. File Storage bietet Dateifreigaben in der Cloud über das standardmäßige SMB-Protokoll. Folgende Anforderungen müssen erfüllt sein:

* Ein [Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)
* [Dateien mit den öffentlichen und privaten SSH-Schlüsseln (Secure Shell)](mac-create-ssh-keys.md)

## <a name="cli-versions-to-use"></a>Zu verwendende Versionen der Befehlszeilenschnittstelle
Für diese Aufgabe können Sie eine der folgenden Versionen der Befehlszeilenschnittstelle (Command-Line Interface, CLI) verwenden:

- [Azure-CLI 1.0](#quick-commands): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0:](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell


## <a name="quick-commands"></a>Schnellbefehle
Führen Sie die Schritte in diesem Abschnitt aus, um die Aufgabe schnell zu erledigen. Wenn Sie genauere Informationen und weiteren Kontext erhalten möchten, beginnen Sie mit dem Abschnitt [Ausführliche exemplarische Vorgehensweise](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough).

### <a name="prerequisites"></a>Voraussetzungen
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
Fügen Sie folgende Zeile zu `/etc/fstab` hinzu:

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

File Storage bietet Dateifreigaben in der Cloud, die das standardmäßige SMB-Protokoll verwenden. Mit der neuesten Version von File Storage können Sie außerdem eine Dateifreigabe aus einem beliebigen Betriebssystem bereitstellen, das SMB 3.0 unterstützt. Wenn Sie eine SMB-Bereitstellung unter Linux verwenden, erhalten Sie einfache Sicherungen an einem robusten, dauerhaften Speicherort für die Archivierung, der durch eine Vereinbarung zum Servicelevel (SLA) unterstützt wird.

Das Verschieben von Dateien von einem virtuellen Computer auf eine in File Storage gehostete SMB-Bereitstellung ist eine gute Möglichkeit zum Debuggen von Protokollen. Dies liegt daran, dass ein und dieselbe SMB-Freigabe lokal auf Ihrer Mac-, Linux- oder Windows-Arbeitsstation bereitgestellt werden kann. SMB ist nicht die ideale Lösung zum Streamen von Linux- oder Anwendungsprotokollen in Echtzeit, da das SMB-Protokoll nicht auf so anspruchsvolle Protokollierungsaufgaben ausgelegt ist. Ein dediziertes Tool wie Fluentd mit vereinheitlichter Protokollierungsebene ist eine bessere Wahl als SMB, wenn es um das Erfassen der Linux- und Anwendungsprotokollausgabe geht.

Für diese ausführliche exemplarische Vorgehensweise schaffen wir die Voraussetzungen für die Erstellung der File Storage-Freigabe und die anschließende Bereitstellung per SMB auf einem virtuellen Linux-Computer.

1. Erstellen Sie mit folgendem Code ein Azure-Speicherkonto:

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. Zeigen Sie die Speicherkontoschlüssel an.

    Beim Erstellen eines Speicherkontos werden die Speicherkontoschlüssel als Paar erstellt, sodass eine Rotation der Schlüssel ohne Unterbrechung des Diensts möglich ist. Wenn Sie zum zweiten Schlüssel des Paars wechseln, erstellen Sie ein neues Schlüsselpaar. Neue Speicherkontoschlüssel werden stets als Paar erstellt, damit sichergestellt ist, dass immer mindestens ein ungenutzter Speicherkontoschlüssel vorhanden ist, zu dem gewechselt werden kann. Zeigen Sie mit folgendem Code die Speicherkontoschlüssel an:

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. Erstellen Sie die File Storage-Freigabe.

    Die File Storage-Freigabe enthält die SMB-Freigabe. Das Kontingent wird immer in Gigabyte (GB) angegeben. Erstellen Sie mit folgendem Code die File Storage-Freigabe:

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. Erstellen Sie das Bereitstellungspunktverzeichnis.

    Sie müssen ein lokales Verzeichnis in dem Linux-Dateisystem erstellen, in dem die SMB-Freigabe bereitgestellt werden soll. Alle Elemente, die im lokalen Bereitstellungsverzeichnis geschrieben oder gelesen werden, werden an die in File Storage gehostete SMB-Freigabe weitergeleitet. Erstellen Sie mit folgendem Code das Verzeichnis:

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. Stellen Sie mit folgendem Code die SMB-Freigabe bereit:

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. Legen Sie fest, dass die SMB-Bereitstellung bei Neustarts beibehalten wird.

    Beim Neustart eines virtuellen Linux-Computers wird die Bereitstellung der SMB-Freigabe während des Herunterfahrens aufgehoben. Damit die SMB-Freigabe beim Starten erneut bereitgestellt wird, müssen Sie der Linux-Datei „etc/fstab“ eine Zeile hinzufügen. Linux verwendet die fstab-Datei, um die Dateisysteme aufzulisten, die während des Startvorgangs bereitgestellt werden müssen. Durch Hinzufügen der SMB-Freigabe wird sichergestellt, dass die File Storage-Freigabe für den virtuellen Linux-Computer ein dauerhaft bereitgestelltes Dateisystem ist. Das Hinzufügen der File Storage-SMB-Freigabe zu einem neuen virtuellen Computer ist möglich, wenn Sie „cloud-init“ verwenden.

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Verschlüsseln von Datenträgern auf einem virtuellen Linux-Computer mithilfe der Azure-Befehlszeilenschnittstelle](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

