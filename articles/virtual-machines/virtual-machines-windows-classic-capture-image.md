<properties
    pageTitle="Erfassen eines Images eines virtuellen Azure Windows-Computers | Microsoft Azure"
    description="Erfassen Sie ein Image eines virtuellen Azure Windows-Computers, der mit dem klassischen Bereitstellungsmodell erstellt wurde."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="cynthn"/>


#<a name="capture-an-image-of-an-azure-windows-virtual-machine-created-with-the-classic-deployment-model."></a>Erfassen Sie ein Image eines virtuellen Azure Windows-Computers, der mit dem klassischen Bereitstellungsmodell erstellt wurde.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] Informationen zum Resource Manager-Modell finden Sie unter [Create a copy of a specialized Windows VM running in Azure](virtual-machines-windows-vhd-copy.md) (Erstellen einer Kopie eines spezialisierten virtuellen Windows-Computers in Azure).


In diesem Artikel wird erläutert, wie Sie einen virtuellen Azure-Computer erfassen, auf dem Windows läuft, um ihn als Image zum Erstellen anderer virtueller Computer zu verwenden. Dieses Image enthält den Betriebssystem-Datenträger und alle Datenträger, die an den virtuellen Computer angefügt sind. Da das Image keine Netzwerkkonfiguration enthält, muss diese später erfolgen, wenn Sie die anderen auf diesem Image basierenden virtuellen Computer erstellen.

Azure speichert das Image unter **Eigene Images**. Hier werden auch sämtliche Images abgelegt, die Sie hochladen. Näheres zu Images finden Sie unter [Informationen zu Images für virtuelle Computer](virtual-machines-linux-classic-about-images.md).

##<a name="before-you-begin##"></a>Voraussetzungen##

Diese Schritte setzen voraus, dass Sie bereits einen virtuellen Azure-Computer erstellt, das Betriebssystem konfiguriert und beliebige Datenträger angefügt haben. Falls dies noch nicht geschehen ist, finden Sie hier Anweisungen:

- [Erstellen eines virtuellen Computers von einem Image](virtual-machines-windows-classic-createportal.md)
- [Anfügen eines Datenträgers an einen virtuellen Computer](virtual-machines-windows-classic-attach-disk.md)
- Stellen Sie sicher, dass die Serverrollen von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)(Sysprep-Unterstützung für Serverrollen).

> [AZURE.WARNING] Mit diesem Vorgang wird die ursprüngliche virtuelle Maschine nach der Erfassung gelöscht. 

Es empfiehlt sich, vor der Erfassung eines Images eines Azure virtuellen Computers, den virtuellen Zielcomputer durch ein Backup zu sichern. Azure virtuelle Computer können durch den Azure Backup entsprechend gesichert werden. Weitere Informationen finden Sie unter [Sichern eines virtuellen Azure-Computers](../backup/backup-azure-vms.md). Andere Lösungen sind von zertifizierten Partnern erhältlich. Aktuelle Angebote finden Sie auf dem Azure Marketplace.


##<a name="capture-the-virtual-machine"></a>Erfassen des virtuellen Computers

1. Stellen Sie im [klassischen Azure-Portal](http://manage.windowsazure.com)eine **Verbindung** mit dem virtuellen Computer her. Ausführliche Informationen finden Sie unter [Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird] [].

2.  Öffnen Sie ein Eingabeaufforderungsfenster als ein Administrator.

3.  Ändern Sie das Verzeichnis in `%windir%\system32\sysprep`, und führen Sie dann „sysprep.exe“ aus.

4.  Das Dialogfeld **Systemvorbereitungstool** wird angezeigt. Gehen Sie wie folgt vor:

    - Wählen Sie unter **Systembereinigungsaktion** die Option **Out-of-Box-Experience (OOBE) für System aktivieren**, und stellen Sie sicher, dass **Verallgemeinern** aktiviert ist. Weitere Informationen zur Verwendung von Sysprep finden Sie unter [How to Use Sysprep: An Introduction][].

    - Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren**.

    - Klicken Sie auf **OK**.

    ![Sysprep ausführen](./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png)

7.  Sysprep fährt den virtuellen Computer herunter. Dadurch wird der Status des virtuellen Computers im klassischen Azure-Portal in **Angehalten** geändert.

8.  Klicken Sie im klassischen Azure-Portal auf **Virtuelle Computer** , und wählen Sie den virtuellen Computer aus, den Sie erfassen möchten.

9.  Klicken Sie in der Befehlsleiste auf **Aufnehmen**.

    ![Virtuellen Computer erfassen](./media/virtual-machines-windows-classic-capture-image/CaptureVM.png)

    Das Dialogfeld **Virtuellen Computer erfassen** wird angezeigt.

10. Geben Sie unter **Imagename**den Namen für das neue Image ein.

11. Bevor Sie ein Windows Server-Image zu Ihren benutzerdefinierten Images hinzufügen, muss es durch Ausführen von Sysprep, wie in den vorherigen Schritten beschrieben, verallgemeinert werden. Klicken Sie auf **Ich habe Sysprep auf dem virtuellen Computer ausgeführt** , um anzugeben, dass Sie diesen Schritt ausgeführt haben.

12. Aktivieren Sie das Kontrollkästchen, um das Image zu erfassen. Das neue Image steht nun unter **Images**zur Verfügung.

    ![Image-Erfassung erfolgreich](./media/virtual-machines-windows-classic-capture-image/VMCapturedImageAvailable.png)

##<a name="next-steps"></a>Nächste Schritte

Das Image kann jetzt zum Erstellen virtueller Computer verwendet werden. Dazu erstellen Sie mithilfe des Menüeintrags **Aus Katalog** einen virtuellen Computer und wählen das gerade erstellte Image aus. Anweisungen dazu finden Sie unter [Erstellen eines virtuellen Computers von einem Image](virtual-machines-windows-classic-createportal.md).



[Anmelden bei einem virtuellen Computer, auf dem Windows Server ausgeführt wird]: virtual-machines-windows-classic-connect-logon.md
[How to Use Sysprep: An Introduction]: http://technet.microsoft.com/library/bb457073.aspx
[Ausführen von „Sysprep.exe“]: ./media/virtual-machines-capture-image-windows-server/SysprepCommand.png
[Eingeben der Optionen für „Sysprep.exe“]: ./media/virtual-machines-windows-classic-capture-image/SysprepGeneral.png
[Der virtuelle Computer wird angehalten.]: ./media/virtual-machines-capture-image-windows-server/SysprepStopped.png
[Erfassen eines Images des virtuellen Computers]: ./media/virtual-machines-windows-classic-capture-image/CaptureVM.png
[Eingeben des Imagenamens]: ./media/virtual-machines-capture-image-windows-server/Capture.png
[Image-Erfassung erfolgreich]: ./media/virtual-machines-capture-image-windows-server/CaptureSuccess.png
[Verwenden des erfassten Image]: ./media/virtual-machines-capture-image-windows-server/MyImagesWindows.png



<!--HONumber=Oct16_HO2-->


