---
title: Bereitstellen von Azure File Storage auf virtuellen Linux-Computern per SMB mithilfe von Azure CLI 1.0 | Microsoft Docs
description: Es wird beschrieben, wie Sie Azure File Storage per SMB auf Linux-VMs bereitstellen.
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
ms.sourcegitcommit: 4d547f854df5d4801779e37ac5f22fb9551bd6bb
ms.openlocfilehash: 73151dba33e2c05a66176ef6b895339bce167128
ms.lasthandoff: 02/15/2017


---

# <a name="mount-azure-file-storage-on-linux-vms-using-smb-using-the-azure-cli-10"></a>Bereitstellen von Azure File Storage auf virtuellen Linux-Computern per SMB mithilfe von Azure CLI 1.0

In diesem Artikel wird beschrieben, wie Sie den Azure File Storage-Dienst auf einer Linux-VM mithilfe einer SMB-Bereitstellung verwenden.  Der Azure-Dateispeicher verfügt über Dateifreigaben in der Cloud unter Verwendung des standardmäßigen SMB-Protokolls.  Folgende Anforderungen müssen erfüllt sein:

- [ein Azure-Konto](https://azure.microsoft.com/pricing/free-trial/)

- [Dateien mit den öffentlichen und privaten SSH-Schlüsseln](virtual-machines-linux-mac-create-ssh-keys.md)


## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe
Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure-CLI 1.0](#quick-commands): Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell (in diesem Artikel)
- [Azure CLI 2.0 (Vorschau)](virtual-machines-linux-mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json): Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell


## <a name="quick-commands"></a>Schnellbefehle

Falls Sie die Aufgabe schnell durchführen müssen, finden Sie im folgenden Abschnitt eine Erläuterung der erforderlichen Befehle. Ausführlichere Informationen und Kontext für die einzelnen Schritte finden Sie im übrigen Dokument ([ab hier](virtual-machines-linux-mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough)).

Voraussetzungen: Ressourcengruppe, VNet, NSG mit SSH eingehend, Subnetz, Azure Storage-Konto, Azure Storage-Kontoschlüssel, Azure File Storage-Freigabe und eine Linux-VM. Ersetzen Sie alle Beispiele durch Ihre eigenen Einstellungen.

Erstellen Sie ein Verzeichnis für die lokale Bereitstellung.

```bash
mkdir -p /mnt/mymountpoint
```

Stellen Sie die Azure File Storage-SMB-Freigabe für den Bereitstellungspunkt bereit.

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

Fügen Sie `/etc/fstab` eine Zeile hinzu, um die Bereitstellung nach einem Neustart persistent zu machen.

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise

Der Azure-Dateispeicher verfügt über Dateifreigaben in der Cloud unter Verwendung des standardmäßigen SMB-Protokolls.  Mit der neuesten Version des Dateispeichers können Sie außerdem eine Dateifreigabe eines beliebigen Betriebssystems einbinden, das SMB 3.0 unterstützt.  Die Verwendung einer SMB-Bereitstellung unter Linux ermöglicht einfache Sicherungen an einem robusten, dauerhaften Speicherort für die Archivierung, der durch eine Vereinbarung zum Servicelevel (SLA) unterstützt wird.  

Das Verschieben von Dateien von einer VM auf eine SMB-Bereitstellung, die in Azure File Storage gehostet wird, ist eine gute Möglichkeit zum Debuggen von Protokollen. Dieselbe SMB-Freigabe kann nämlich lokal auf Ihrer Mac-, Linux- oder Windows-Arbeitsstation bereitgestellt werden.  SMB ist nicht die am besten geeignete Lösung zum Streamen von Linux- oder Anwendungsprotokollen in Echtzeit, da das SMB-Protokoll nicht für anspruchsvollere Protokollierungsaufgaben wie diese ausgelegt ist.  Ein dediziertes Tool wie Fluentd mit vereinheitlichter Protokollierungsebene wäre eine bessere Wahl als SMB, wenn es um das Erfassen der Linux- und Anwendungsprotokollierungsausgabe geht.

Für diese ausführliche exemplarische Vorgehensweise erstellen wir die erforderlichen Voraussetzungen für die Erstellung der Azure File Storage-Freigabe und die anschließende Bereitstellung per SMB auf einer Linux-VM.

## <a name="create-the-azure-storage-account"></a>Erstellen des Azure Storage-Kontos

```azurecli
azure storage account create myStorageAccount \
--sku-name lrs \
--kind storage \
-l westus \
-g myResourceGroup
```

## <a name="show-the-storage-account-keys"></a>Anzeigen der Schlüssel für das Storage-Konto

Die Schlüssel des Azure Storage-Kontos werden beim Anlegen des Speicherkontos als Paare erstellt.  Der Grund für die Erstellung der Speicherkontoschlüssel als Paar ist, dass dann eine Rotation der Schlüssel ohne Unterbrechung des Diensts möglich ist.  Nachdem Sie die Rotation zum zweiten Schlüssel des Paars durchgeführt haben, erstellen Sie eine neues Schlüsselpaar.  Neue Speicherkontoschlüssel werden stets als Paar erstellt, damit sichergestellt ist, dass immer mindestens ein ungenutzter Speicherschlüssel für die Rotation vorhanden ist.

```azurecli
azure storage account keys list myStorageAccount \
--resource-group myResourceGroup
```

## <a name="create-the-azure-file-storage-share"></a>Erstellen der Azure File Storage-Freigabe

Erstellen Sie die File Storage-Freigabe, auf der die SMB-Freigabe vorhanden ist.  Das Kontingent wird immer in Gigabyte (GB) angegeben.

```azurecli
azure storage share create mystorageshare \
--quota 10 \
--account-name myStorageAccount \
--account-key nPOgPR<--snip-->4Q==
```

## <a name="create-the-mount-point-directory"></a>Erstellen des Bereitstellungspunktverzeichnisses

Für das Linux-Dateisystem wird ein lokales Verzeichnis benötigt, in dem die SMB-Freigabe bereitgestellt werden kann.  Alle Elemente, die für das lokale Bereitstellungsverzeichnis geschrieben oder gelesen werden, werden an die unter Azure File Storage gehostete SMB-Freigabe weitergeleitet.

```bash
sudo mkdir -p /mnt/mymountdirectory
```

## <a name="mount-the-smb-share"></a>Bereitstellen der SMB-Freigabe

```azurecli
sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
```

## <a name="persist-the-smb-mount-through-reboots"></a>Festlegen der SMB-Bereitstellung als persistent mithilfe von Neustarts

Beim Neustart der Linux-VM wird die Bereitstellung der SMB-Freigabe während des Herunterfahrens aufgehoben.  Damit die SMB-Freigabe beim Starten erneut bereitgestellt wird, müssen Sie der Linux-Datei `/etc/fstab` eine Zeile hinzufügen.  In Linux wird die Datei `fstab` verwendet, um aufzulisten, welche Dateisysteme während des Startvorgangs bereitgestellt werden müssen.  Durch das Hinzufügen der SMB-Freigabe wird sichergestellt, dass die Azure File Storage-Freigabe für die Linux-VM ein dauerhaft bereitgestelltes Dateisystem ist.  Das Hinzufügen der SMB-Freigabe von Azure File Storage zu einer neuen VM wird durch `cloud-init` ermöglicht.

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung](virtual-machines-linux-using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Hinzufügen eines Datenträgers zu einem virtuellen Linux-Computer](virtual-machines-linux-add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [Verschlüsseln von Datenträgern auf einem virtuellen Linux-Computer mithilfe der Azure-Befehlszeilenschnittstelle](virtual-machines-linux-encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

