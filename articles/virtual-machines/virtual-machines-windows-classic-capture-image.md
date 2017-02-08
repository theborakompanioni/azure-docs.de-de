---
title: Erfassen eines Images eines virtuellen Azure Windows-Computers | Microsoft Docs
description: Erfassen Sie ein Image eines virtuellen Azure Windows-Computers, der mit dem klassischen Bereitstellungsmodell erstellt wurde.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: a5986eac-4cf3-40bd-9b79-7c811806b880
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: 6b68d41daeea780d70b5ce1389d05f1f4fdf65ea


---
# <a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Erfassen Sie ein Image eines virtuellen Azure Windows-Computers, der mit dem klassischen Bereitstellungsmodell erstellt wurde.
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Resource Manager-Modells. Informationen zum Resource Manager-Modell finden Sie unter [Erstellen einer Kopie eines virtuellen Windows-Computers in Azure](virtual-machines-windows-vhd-copy.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

In diesem Artikel wird erläutert, wie Sie einen virtuellen Azure-Computer erfassen, auf dem Windows läuft, um ihn als Image zum Erstellen anderer virtueller Computer zu verwenden. Dieses Image enthält den Betriebssystem-Datenträger und alle Datenträger, die an den virtuellen Computer angefügt sind. Da das Image keine Netzwerkkonfiguration enthält, muss diese später erfolgen, wenn Sie die anderen auf diesem Image basierenden virtuellen Computer erstellen.

Azure speichert das Image unter **Eigene Images**. Hier werden auch sämtliche Images abgelegt, die Sie hochladen. Näheres zu Images finden Sie unter [Informationen zu Images für virtuelle Computer](virtual-machines-linux-classic-about-images.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="before-you-begin"></a>Voraussetzungen
Diese Schritte setzen voraus, dass Sie bereits einen virtuellen Azure-Computer erstellt, das Betriebssystem konfiguriert und beliebige Datenträger angefügt haben. Falls dies noch nicht geschehen ist, finden Sie hier Anweisungen:

* [Erstellen eines virtuellen Computers von einem Image](virtual-machines-windows-classic-createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Anfügen eines Datenträgers an einen virtuellen Computer](virtual-machines-windows-classic-attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* Stellen Sie sicher, dass die Serverrollen von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)(Sysprep-Unterstützung für Serverrollen).

> [!WARNING]
> Mit diesem Vorgang wird die ursprüngliche virtuelle Maschine nach der Erfassung gelöscht. 
> 
> 

Es empfiehlt sich, vor der Erfassung eines Images eines Azure virtuellen Computers, den virtuellen Zielcomputer durch ein Backup zu sichern. Azure virtuelle Computer können durch den Azure Backup entsprechend gesichert werden. Weitere Informationen finden Sie unter [Sichern eines virtuellen Azure-Computers](../backup/backup-azure-vms.md). Andere Lösungen sind von zertifizierten Partnern erhältlich. Aktuelle Angebote finden Sie auf dem Azure Marketplace.

## <a name="capture-the-virtual-machine"></a>Erfassen des virtuellen Computers
1. Stellen Sie im [klassischen Azure-Portal](http://manage.windowsazure.com)eine **Verbindung** mit dem virtuellen Computer her. Ausführliche Informationen finden Sie unter [Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird][How to sign in to a virtual machine running Windows Server].
2. Öffnen Sie ein Eingabeaufforderungsfenster als ein Administrator.
3. Ändern Sie das Verzeichnis in `%windir%\system32\sysprep`, und führen Sie dann „sysprep.exe“ aus.
4. Das Dialogfeld **Systemvorbereitungstool** wird angezeigt. Gehen Sie wie folgt vor:
   
   * Wählen Sie unter **Systembereinigungsaktion** die Option **Out-of-Box-Experience (OOBE) für System aktivieren** aus, und stellen Sie sicher, dass **Verallgemeinern** aktiviert ist. Weitere Informationen zur Verwendung von Sysprep finden Sie unter [Gewusst wie: Verwenden von Sysprep – Einführung][How to Use Sysprep: An Introduction].
   * Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren** aus.
   * Klicken Sie auf **OK**.
   
   ![Sysprep ausführen](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)
5. Sysprep fährt den virtuellen Computer herunter. Dadurch wird der Status des virtuellen Computers im klassischen Azure-Portal in **Angehalten** geändert.
6. Klicken Sie im klassischen Azure-Portal auf **Virtuelle Computer** , und wählen Sie den virtuellen Computer aus, den Sie erfassen möchten.
7. Klicken Sie in der Befehlsleiste auf **Aufnehmen**.
   
   ![Virtuellen Computer erfassen](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)
   
   Das Dialogfeld **Virtuellen Computer erfassen** wird angezeigt.
8. Geben Sie unter **Imagename**den Namen für das neue Image ein.
9. Bevor Sie ein Windows Server-Image zu Ihren benutzerdefinierten Images hinzufügen, muss es durch Ausführen von Sysprep, wie in den vorherigen Schritten beschrieben, verallgemeinert werden. Klicken Sie auf **Ich habe Sysprep auf dem virtuellen Computer ausgeführt** , um anzugeben, dass Sie diesen Schritt ausgeführt haben.
10. Aktivieren Sie das Kontrollkästchen, um das Image zu erfassen. Das neue Image steht nun unter **Images**zur Verfügung.
    
    ![Image-Erfassung erfolgreich](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Nächste Schritte
Das Image kann jetzt zum Erstellen virtueller Computer verwendet werden. Dazu erstellen Sie mithilfe des Menüeintrags **Aus Katalog** einen virtuellen Computer und wählen das gerade erstellte Image aus. Anweisungen dazu finden Sie unter [Erstellen eines virtuellen Computers von einem Image](virtual-machines-windows-classic-createportal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

[How to sign in to a virtual machine running Windows Server]: virtual-machines-windows-classic-connect-logon.md
[How to Use Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Run Sysprep.exe]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Enter Sysprep.exe options]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[The virtual machine is stopped]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Capture an image of the virtual machine]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Enter the image name]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image capture successful]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Use the captured image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png



<!--HONumber=Dec16_HO1-->


