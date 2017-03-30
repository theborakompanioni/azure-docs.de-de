---
title: Erfassen eines Images eines virtuellen Linux-Computers | Microsoft Docs
description: "Erfahren Sie, wie Sie ein Image eines mit dem klassischen Bereitstellungsmodell erstellten virtuellen Azure-Computers unter Linux erfassen können."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 59a88a9f2db745db22007f6b528f8d41016bde16
ms.lasthandoff: 03/21/2017


---
# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Erfassen eines klassischen virtuellen Linux-Computers als Image
> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Modell ausführen](virtual-machines-linux-capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

In diesem Artikel erfahren Sie, wie Sie einen klassischen virtuellen Azure-Computer, auf dem Linux ausgeführt wird, als Image erfassen, um weitere virtuelle Computer zu erstellen. Dieses Image umfasst den Betriebssystemdatenträger und die an den virtuellen Computer angefügten Datenträger. Da das Image keine Netzwerkkonfiguration enthält, müssen Sie die Konfiguration später vornehmen, wenn Sie die anderen virtuellen Computer auf der Grundlage dieses Images erstellen.

Azure speichert das Image unter **Images**, zusammen mit allen Images, die Sie hochgeladen haben. Weitere Informationen zu Images finden Sie unter [Informationen zu Images von virtuellen Computern in Azure][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Voraussetzungen
Diese Schritte setzen voraus, dass Sie bereits mithilfe des klassischen Bereitstellungsmodells einen virtuellen Azure-Computer erstellt, das Betriebssystem konfiguriert und Datenträger angefügt haben. Falls Sie einen virtuellen Computer erstellen müssen, lesen Sie sich [Erstellen eines benutzerdefinierten virtuellen Linux-Computers][How to Create a Linux Virtual Machine] durch.

## <a name="capture-the-virtual-machine"></a>Erfassen des virtuellen Computers
1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – unter Verwendung eines SSH-Clients Ihrer Wahl.
2. Geben Sie im SSH-Fenster den folgenden Befehl ein. Die Ausgabe von `waagent` kann je nach Version dieses Hilfsprogramms geringfügig abweichen:

    ```bash
    sudo waagent -deprovision+user
    ```

    Der vorherige Befehl versucht, das System zu bereinigen und für eine erneute Bereitstellung vorzubereiten. Bei diesem Vorgang werden die folgenden Aufgaben ausgeführt:

   * Entfernen von SSH-Hostschlüsseln (sofern "Provisioning.RegenerateSshHostKeyPair" in der Konfigurationsdatei auf "y" festgelegt ist)
   * Löschen der Namenserverkonfiguration in "/etc/resolv.conf"
   * Entfernen des Kennworts des `root` -Benutzers aus „/etc/shadow“ (sofern „Provisioning.DeleteRootPassword“ in der Konfigurationsdatei auf „y“ festgelegt ist)
   * Entfernen von zwischengespeicherten DHCP-Clientleases
   * Setzt den Hostnamen auf "localhost.localdomain" zurück
   * Löschen des zuletzt bereitgestellten Benutzerkontos (aus "/var/lib/waagent" abgerufen) **und der zugehörigen Daten**

     > [!NOTE]
     > Beim Aufheben der Bereitstellung werden Dateien und Daten gelöscht, um das Image zu „generalisieren“. Führen Sie diesen Befehl nur auf einem virtuellen Computer aus, den Sie als neue Imagevorlage erfassen möchten. Dies garantiert nicht, dass alle vertraulichen Informationen aus dem Image gelöscht werden oder dass es für eine erneute Verteilung an Dritte genutzt werden kann.

3. Geben Sie **y** ein, um fortzufahren. Sie können den Parameter `-force` hinzufügen, um diesen Bestätigungsschritt zu vermeiden.
4. Geben Sie **Exit** ein, um den SSH-Client zu schließen.

   > [!NOTE]
   > Bei den nächsten Schritten wird davon ausgegangen, dass Sie [die Azure-Befehlszeilenschnittstelle auf dem Clientcomputer installiert haben](../cli-install-nodejs.md) . Die folgenden Schritte können Sie auch im [klassischen Azure-Portal][Azure classic portal] ausführen.

5. Öffnen Sie auf dem Clientcomputer die Azure-CLI, und melden Sie sich bei Ihrem Azure-Abonnement an. Weitere Informationen hierzu finden Sie unter [Herstellen einer Verbindung mit einem Azure-Abonnement von der Azure-Befehlszeilenschnittstelle](../xplat-cli-connect.md).
6. Stellen Sie sicher, dass der Dienstverwaltungsmodus aktiviert ist:

    ```azurecli
    azure config mode asm
    ```

7. Fahren Sie den virtuellen Computer herunter, dessen Bereitstellung bereits aufgehoben wurde. Im folgenden Beispiel wird der virtuelle Computer namens `myVM` heruntergefahren:

    ```azurecli
    azure vm shutdown myVM
    ```

   > [!NOTE]
   > Mit `azure vm list` können Sie eine Liste aller virtuellen Computer anzeigen, die in Ihrem Abonnement erstellt wurden.

8. Erfassen Sie nach dem Beenden des virtuellen Computers das Image. Im folgenden Beispiel wird der virtuelle Computer namens `myVM` erfasst und ein generalisiertes Image namens `myNewVM` erstellt:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    Der Unterbefehl `-t` löscht den ursprünglichen virtuellen Computer.

9. Das neue Image ist jetzt in der Liste mit den Images verfügbar, die zum Konfigurieren neuer virtueller Computer verwendet werden können. Sie können es mit dem folgenden Befehl anzeigen:

   ```azurecli
   azure vm image list
   ```

   Im [Azure-Portal](http://portal.azure.com) wird das neue Image in **VM-Images (klassisch)** angezeigt, das zu den **Compute**-Diensten gehört. Sie finden **VM-Images (klassisch)** durch Klicken auf _Weitere Dienste_ am unteren Rand der Azure-Dienstliste. Suchen Sie dann in den **Compute**-Diensten.   

   ![Image-Erfassung erfolgreich](./media/virtual-machines-linux-classic-capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Nächste Schritte
Das Image kann jetzt zum Erstellen virtueller Computer verwendet werden. Sie können den Azure-CLI-Befehl `azure vm create` verwenden und den Namen des Images angeben, das Sie erstellt haben. Weitere Informationen finden Sie unter [Befehle der Azure-Befehlszeilenschnittstelle im Modus „Azure-Dienstverwaltung“ (Azure Service Management, ASM)](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

Alternativ können Sie einen benutzerdefinierten virtuellen Computer über das [klassische Azure-Portal][Azure classic portal] erstellen, indem Sie die Methode **Aus Katalog** verwenden und das Image auswählen, das Sie erstellt haben. Weitere Informationen finden Sie unter [Erstellen eines benutzerdefinierten virtuellen Linux-Computers][How to Create a Custom Virtual Machine].

**Siehe auch** [Benutzerhandbuch für Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Azure classic portal]: http://manage.windowsazure.com
[About Virtual Machine Images in Azure]: virtual-machines-linux-classic-about-images.md
[How to Create a Custom Virtual Machine]: virtual-machines-linux-classic-create-custom.md
[How to Attach a Data Disk to a Virtual Machine]:windows/classic/attach-disk.md
[How to Create a Linux Virtual Machine]: virtual-machines-linux-classic-create-custom.md

