---
title: "Einführung in Linux in Azure | Microsoft Docs"
description: Erfahren Sie, wie Sie virtuelle Linux-Computer in Azure verwenden.
services: virtual-machines-linux
documentationcenter: python
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: b13bf305-87bf-4df3-815e-e8f6337aa6ea
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: dc61f74a10fed1ba9e3959326e0020cf2b4440ea


---
# <a name="introduction-to-linux-on-azure"></a>Einführung in Linux in Azure
Dieses Thema bietet einen Überblick über einige Aspekte der Verwendung virtueller Linux-Computer in der Azure-Cloud. Die Bereitstellung eines virtuellen Linux-Computers ist ein unkomplizierter Prozess, wenn Sie ein Image aus dem Katalog verwenden.

## <a name="authentication-usernames-passwords-and-ssh-keys"></a>Authentifizierung: Benutzernamen, Kennwörter und SSH-Schlüssel
Wenn Sie mithilfe des klassischen Azure-Portals einen virtuellen Linux-Computer erstellen, werden Sie aufgefordert, einen Benutzernamen, ein Kennwort oder einen öffentlichen SSH-Schlüssel anzugeben. Bei der Auswahl eines Benutzernamens während der Bereitstellung virtueller Linux-Computer gibt es folgende Einschränkungen: bereits im virtuellen Computer vorhandene Namen von Systemkonten (UID <100) sind nicht zulässig, z.B. 'root'.

* Weitere Informationen finden Sie unter [Erstellen eines virtuellen Linux-Computers](virtual-machines-linux-quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Siehe [Verwenden von SSH mit Linux in Azure](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="obtaining-superuser-privileges-using-sudo"></a>Erlangen von Superuser-Berechtigungen mit `sudo`
Das Benutzerkonto, das während der Bereitstellung der virtuellen Computerinstanz auf Azure angegeben wird, ist ein Konto mit weit reichenden Berechtigungen. Dieses Konto wird vom Azure Linux-Agent konfiguriert, damit die Berechtigungen mithilfe des Hilfsprogramms `sudo` auf "root" (Superuser-Konto) erweitert werden können. Sobald Sie über dieses Benutzerkonto angemeldet sind, können Sie mit dieser Befehlssyntax Befehle als "root" ausführen.

    # sudo <COMMAND>

Optional können Sie über **sudo -s**eine Root-Shell abrufen.

* Siehe [Verwenden von Stammberechtigungen auf virtuellen Linux-Computern in Azure](virtual-machines-linux-use-root-privileges.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="firewall-configuration"></a>Firewall-Konfiguration
Azure bietet einen integrierten Paketfilter, der die Konnektivität auf die im klassischen Azure-Portal angegebenen Ports beschränkt. Standardmäßig ist SSH der einzige erlaubte Port. Sie können den Zugriff auf zusätzliche Ports auf Ihrem virtuellen Linux-Computer aktivieren, indem Sie im klassischen Azure-Portal Endpunkte konfigurieren.

* Siehe: [Einrichten von Endpunkten für einen virtuellen Computer](virtual-machines-windows-classic-setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Die Linux-Images im Azure-Katalog aktivieren nicht standardmäßig die *iptables* -Firewall. Bei Bedarf kann die Firewall für die Bereitstellung einer zusätzlichen Filterung konfiguriert werden.

## <a name="hostname-changes"></a>Hostnamen-Änderungen
Wenn Sie eine Instanz eines Linux-Images zum ersten Mal bereitstellen, müssen Sie einen Hostnamen für den virtuellen Computer bereitstellen. Wenn der virtuelle Computer läuft, wird dieser Hostname auf den Plattform-DNS-Servern veröffentlicht, so dass mehrere miteinander verbundene virtuelle Computer IP-Adresssuchen anhand von Hostnamen durchführen können.

Wenn nach der Bereitstellung eines virtuellen Computers Hostnamenänderungen gewünscht werden, verwenden Sie den Befehl

    # sudo hostname <newname>

Der Azure Linux Agent enthält eine Funktion zur automatischen Erkennung dieser Namensänderung und zur entsprechenden Konfiguration des virtuellen Computers für eine Beibehaltung dieser Änderung und für ihre Veröffentlichung auf DNS-Plattformservern.

* [Benutzerhandbuch für Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="cloud-init"></a>cloud-init
**Ubuntu**- und **CoreOS**-Images nutzen „cloud-init“ in Azure, was zusätzliche Funktionen für das Bootstrapping eines virtuellen Computers bietet.

* [Einfügen von benutzerdefinierten Daten](virtual-machines-windows-classic-inject-custom-data.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Custom Data and Cloud-Init on Microsoft Azure (Benutzerdefinierte Daten und Cloud-Init in Microsoft Azure, in englischer Sprache)](https://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
* [AzureSwapPartitions (in englischer Sprache)](https://wiki.ubuntu.com/AzureSwapPartitions)
* [Verwenden von CoreOS in Azure](https://coreos.com/os/docs/latest/booting-on-azure.html)

## <a name="virtual-machine-image-capture"></a>Image-Erfassung virtueller Computer
Azure bietet die Möglichkeit, den Status eines vorhandenen virtuellen Computers in einem Image zu erfassen, das danach verwendet kann, um zusätzliche virtuelle Computerinstanzen bereitzustellen. Der Azure Linux Agent kann verwendet werden, um einige während des Bereitstellungsprozesses durchgeführten Anpassungen zurückzusetzen. Sie können die Schritte unten ausführen, um einen virtuellen Computer als Image zu erfassen.

1. Führen Sie **waagent -deprovision** aus, um bei Bereitstellungen durchgeführte Anpassungen rückgängig zu machen. Oder optional **waagent -deprovision+user** , um das während der Bereitstellung angegebene Benutzerkonto und alle damit zusammenhängenden Daten zu löschen.
2. Schalten Sie den virtuellen Computer aus.
3. Klicken Sie im klassischen Azure-Portal auf *Erfassen* , oder verwenden Sie PowerShell oder Tools der Befehlszeilenschnittstelle, um den virtuellen Computer als Image zu erfassen.
   
   * Siehe [Erfassen eines virtuellen Linux-Computers zur Verwendung als Vorlage](virtual-machines-linux-classic-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="attaching-disks"></a>Anfügen von Datenträgern
An jeden virtuellen Computer ist ein temporärer lokaler *Ressourcendatenträger* angefügt. Da die Daten auf einem Ressourcendatenträger bei Neustarts verloren gehen können, werden Ressourcendatenträger oft von Anwendungen und Prozessen zur **temporären** Datenspeicherung auf dem virtuellen Computer verwendet. Außerdem dienen sie zur Speicherung von Auslagerungsdateien für das Betriebssystem.

Unter Linux wird der Ressourcendatenträger normalerweise vom Azure Linux-Agent verwaltet und automatisch in **/mnt/resource** (oder auf Ubuntu-Images in **/mnt**) eingebunden.

> [!NOTE]
> Beachten Sie, dass der Ressourcendatenträger ein **temporärer** Datenträger ist und geleert und neu formatiert werden kann, wenn der virtuelle Computer neu gestartet wird.
> 
> 

Der Datenträger unter Linux kann vom Kernel den Namen `/dev/sdc` erhalten. Die Benutzer müssen diese Ressource partitionieren, formatieren und bereitstellen. Dieser Vorgang wird Schritt für Schritt im Tutorial [Anfügen eines Datenträgers an einen virtuellen Computer](virtual-machines-linux-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) beschrieben.

* **Siehe auch**: [Konfigurieren von Software-RAID unter Linux](virtual-machines-linux-configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) & [Konfigurieren von LVM auf einem virtuellen Linux-Computer in Azure](virtual-machines-linux-configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).




<!--HONumber=Nov16_HO3-->


