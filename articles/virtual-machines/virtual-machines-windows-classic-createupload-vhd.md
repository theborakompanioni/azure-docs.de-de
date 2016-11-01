<properties
	pageTitle="Erstellen und Hochladen eines VM-Images mit PowerShell | Microsoft Azure"
	description="Hier finden Sie Informationen Sie zum Erstellen und Hochladen eines generalisierten Windows Server-Image (VHD) mit dem klassischen Bereitstellungsmodell und Azure PowerShell."
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
	ms.date="07/21/2016"
	ms.author="cynthn"/>

# Erstellen und Hochladen einer Windows Server-VHD nach Azure

Dieser Artikel erläutert, wie Sie Ihr eigenes generalisiertes VM-Image als virtuelle Festplatte (Virtual Hard Disk, VHD) hochladen, um es zum Erstellen virtueller Computer zu nutzen. Weitere Details zu Datenträgern und VHDs in Microsoft Azure finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md).


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]. Sie können einen virtuellen Computer auch mit dem Resource Manager-Modell [erfassen](virtual-machines-windows-capture-image.md) und [hochladen](virtual-machines-windows-upload-image.md).

## Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über Folgendes verfügen:

- **Ein Azure-Abonnement** – wenn Sie keines besitzen, können Sie [ein kostenloses Azure-Konto eröffnen](/pricing/free-trial/?WT.mc_id=A261C142F).

- **[Microsoft Azure PowerShell](../powershell-install-configure.md)** – Sie haben das Microsoft Azure PowerShell-Modul installiert und für die Verwendung Ihres Abonnements konfiguriert.

- **Eine VHD-Datei** – ein unterstütztes Windows-Betriebssystem, das in einer VHD-Datei gespeichert und einem virtuellen Computer angefügt ist. Prüfen Sie auch, ob die auf der virtuellen Festplatte ausgeführten Serverrollen von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Sysprep-Unterstützung für Serverrollen).

> [AZURE.IMPORTANT] Das VHDX-Format wird in Microsoft Azure nicht unterstützt. Sie können den Datenträger mit dem Hyper-V-Manager oder dem [Convert-VHD-Cmdlet](http://technet.microsoft.com/library/hh848454.aspx) in das VHD-Format konvertieren. Ausführliche Informationen finden Sie in diesem [Blogbeitrag](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## Schritt 1: Vorbereiten der VHD-Datei 

Bevor Sie die VHD-Datei nach Azure hochladen, muss sie mit dem Sysprep-Tool generalisiert werden. Durch diesen Vorgang wird die VHD-Datei auf eine Verwendung als Image vorbereitet. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx) (in englischer Sprache). Sichern Sie den virtuellen Computer vor dem Ausführen von Sysprep.

Führen Sie auf dem virtuellen Computer, auf dem das Betriebssystem installiert wurde, die folgende Prozedur aus:

1. Melden Sie sich beim Betriebssystem an.

2. Öffnen Sie ein Eingabeaufforderungsfenster als Administrator. Wechseln Sie in das Verzeichnis **%windir%\\system32\\sysprep**, und führen Sie dann `sysprep.exe` aus.

	![Öffnen eines Eingabeaufforderungsfensters](./media/virtual-machines-windows-classic-createupload-vhd/sysprep_commandprompt.png)

3.	Das Dialogfeld **Systemvorbereitungstool** wird angezeigt.

	![Starten von Sysprep](./media/virtual-machines-windows-classic-createupload-vhd/sysprepgeneral.png)

4.  Wählen Sie unter **Systemvorbereitungstool** die Option **Out-of-Box-Experience (OOBE) für System aktivieren**, und stellen Sie sicher, dass **Verallgemeinern** aktiviert ist.

5.  Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren**.

6.  Klicken Sie auf **OK**.

## Schritt 2: Erstellen eines Speicherkontos und eines Containers

Sie benötigen ein Speicherkonto in Azure, um über einen Ort zum Hochladen der VHD-Datei zu verfügen. In diesem Schritt wird gezeigt, wie Sie ein Konto erstellen oder Informationen zu einem vorhandenen Konto abrufen. Ersetzen Sie die Variablen in &lsaquo; Klammern &rsaquo; durch Ihre eigenen Daten.

1. Anmeldung

		Add-AzureAccount

1. Legen Sie Ihr Azure-Abonnement fest.

    	Select-AzureSubscription -SubscriptionName <SubscriptionName> 

2. Erstellen Sie ein neues Speicherkonto Der Name des Speicherkontos muss eindeutig und 3 bis 24 Zeichen lang sein. Der Name kann eine beliebige Kombination aus Buchstaben und Zahlen sein. Sie müssen auch einen Speicherort angeben, z.B. „USA, Osten“.
    	
		New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>

3. Legen Sie das neue Speicherkonto als Standard fest.
    	
		Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>

4. Erstellen Sie einen neuen Container.

    	New-AzureStorageContainer -Name <ContainerName> -Permission Off

 

## Schritt 3: Hochladen der VHD-Datei

Verwenden Sie [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) zum Hochladen der VHD.

Geben Sie in dem Azure PowerShell-Fenster, das Sie im vorherigen Schritt verwendet haben, den folgenden Befehl ein, und ersetzen Sie die Variablen in &lsaquo; Klammern &rsaquo; durch Ihre eigenen Daten.

		Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>


## Schritt 4: Hinzufügen des Image zur Liste der benutzerdefinierten Images

Fügen Sie das Image mit dem Cmdlet [Add-AzureVMImage](https://msdn.microsoft.com/library/mt589167.aspx) der Liste Ihrer benutzerdefinierten Images hinzu.

		Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"


## Nächste Schritte

Sie können nun mit dem Image, das Sie hochgeladen haben, [einen benutzerdefinierten virtuellen Computer erstellen](virtual-machines-windows-classic-createportal.md).

<!---HONumber=AcomDC_0907_2016-->