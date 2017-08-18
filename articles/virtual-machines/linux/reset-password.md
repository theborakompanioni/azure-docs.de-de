---
title: "Zurücksetzen des lokalen Linux-Kennworts auf Azure-VMs | Microsoft-Dokumentation"
description: "Führen Sie die Schritte zum Zurücksetzen des lokalen Linux-Kennworts auf der Azure-VM ein"
services: virtual-machines-linux
documentationcenter: 
author: Deland-Han
manager: cshepard
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 7/3/2017
ms.author: delhan
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: bd48128a078821b7a4baa02d5d7ceecc6de99608
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---

# <a name="how-to-reset-local-linux-password-on-azure-vms"></a>Zurücksetzen des lokalen Linux-Kennworts auf Azure-VMs

In diesem Artikel werden mehrere Methoden zum Zurücksetzen des lokalen Kennworts des virtuellen Linux-Computers (VM) vorgestellt. Wenn das Benutzerkonto abgelaufen ist oder Sie ein neues Konto erstellen möchten, können Sie die folgenden Methoden nutzen, um ein neues lokales Administratorkonto zu erstellen und erneut Zugriff auf den virtuellen Computer zu erhalten.

## <a name="symptoms"></a>Symptome

Sie können sich nicht am virtuellen Computer anmelden, und Sie erhalten eine Nachricht, die angibt, dass das von Ihnen verwendete Kennwort falsch ist. Darüber hinaus können Sie VMAgent nicht verwenden, um Ihr Kennwort im Azure-Portal zurückzusetzen. 

## <a name="manual-password-reset-procedure"></a>Verfahren zur manuellen Kennwortzurücksetzung

1.  Löschen Sie den virtuellen Computer und behalten Sie den angefügten Datenträger.

2.  Fügen Sie das Betriebssystemdatenträger als Datenträger einem anderen temporären virtuellen Computer an demselben Standort hinzu.

3.  Führen Sie den folgenden SSH-Befehl auf dem temporären virtuellen Computer aus, um ein Administrator zu werden.


    ~~~~
    sudo su
    ~~~~

4.  Führen Sie **fdisk -l** aus, oder sehen Sie sich die Systemprotokolle an, um den neu angefügten Datenträger zu finden. Suchen Sie den bereitzustellenden Laufwerknamen. Schauen Sie sich anschließend auf dem temporären virtuellen Computer die entsprechende Protokolldatei an.

    ~~~~
    grep SCSI /var/log/kern.log (ubuntu)
    grep SCSI /var/log/messages (centos, suse, oracle)
    ~~~~

    Im Folgenden finden Sie eine Beispielausgabe des „grep“-Befehls:

    ~~~~
    kernel: [ 9707.100572] sd 3:0:0:0: [sdc] Attached SCSI disk
    ~~~~

5.  Erstellen Sie einen Bereitstellungspunkt mit dem Namen **tempmount**.

    ~~~~
    mkdir /tempmount
    ~~~~

6.  Stellen Sie den Betriebssystemdatenträger auf dem Bereitstellungspunkt bereit. Sie müssen in der Regel sdc1 oder sdc2 bereitstellen. Dies hängt von der Hostpartition im Verzeichnis „/etc“ auf der fehlerhaften VM-Festplatte ab.

    ~~~~
    mount /dev/sdc1 /tempmount
    ~~~~

7.  Führen Sie eine Sicherung durch, bevor Sie Änderungen vornehmen:

    ~~~~
    cp /etc/passwd /etc/passwd_orig    
    cp /etc/shadow /etc/shadow_orig    
    cp /tempmount/etc/passwd /etc/passwd
    cp /tempmount/etc/shadow /etc/shadow 
    cp /tempmount/etc/passwd /tempmount/etc/passwd_orig
    cp /tempmount/etc/shadow /tempmount/etc/shadow_orig
    ~~~~

8.  Setzen Sie das Kennwort des Benutzers zurück, das Sie benötigen:

    ~~~~
    passwd <<USER>> 
    ~~~~

9.  Verschieben Sie die bearbeiteten Dateien an den richtigen Ort auf der Festplatte des fehlerhaften Computers.

    ~~~~
    cp /etc/passwd /tempmount/etc/passwd
    cp /etc/shadow /tempmount/etc/shadow
    cp /etc/passwd_orig /etc/passwd
    cp /etc/shadow_orig /etc/shadow
    
10. Go back to the root and unmount the disk.

    ~~~~
    cd / umount /tempmount
    ~~~~

11. Detach the disk from the management portal.

12. Recreate the VM.

## Next steps

* [Troubleshoot Azure VM by attaching OS disk to another Azure VM](http://social.technet.microsoft.com/wiki/contents/articles/18710.troubleshoot-azure-vm-by-attaching-os-disk-to-another-azure-vm.aspx)

* [Azure CLI: How to delete and re-deploy a VM from VHD](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/azure-cli-how-to-delete-and-re-deploy-a-vm-from-vhd/)
