---
title: Hochladen einer Windows-VHD für den Ressourcen-Manager | Microsoft Docs
description: Hier erfahren Sie, wie Sie das Image eines virtuellen Windows-Computers zur Verwendung mit dem Resource Manager-Bereitstellungsmodell hochladen.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/02/2016
ms.author: cynthn

---
# Hochladen eines Windows-VM-Images an Azure für Resource Manager-Bereitstellungen
In diesem Artikel erfahren Sie, wie Sie ein Windows-VHD-Image (Virtual Hard Disk, virtuelle Festplatte) erstellen und hochladen, damit Sie schnell virtuelle Computer erstellen können. Ausführlichere Informationen zu Datenträgern und VHDs in Azure finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md).

## Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie über Folgendes verfügen:

* **Ein Azure-Abonnement**: Falls Sie noch nicht über ein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto erstellen](/pricing/free-trial/?WT.mc_id=A261C142F) oder [MSDN-Abonnentenvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).
* **Azure PowerShell ab Version 1.4**: Informationen zum Installieren finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
* **Virtueller Windows-Computer**: Virtuelle Computer können mit verschiedensten Tools lokal erstellt werden. Informationen hierzu finden Sie beispielsweise unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx). Informationen zu den in Azure unterstützen Windows-Betriebssystemen finden Sie unter [Microsoft-Serversoftwaresupport für virtuelle Microsoft Azure-Computer](https://support.microsoft.com/kb/2721672).
* Stellen Sie sicher, dass die auf dem virtuellen Computer ausgeführten Serverrollen Sysprep unterstützen. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Sysprep-Unterstützung für Serverrollen). Sichern Sie den virtuellen Computer vor dem Ausführen von Sysprep.

## Sicherstellen, dass für den virtuellen Computer das richtige Dateiformat verwendet wird
In Azure können Sie nur [virtuelle Computer der 1. Generation](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) verwenden, die das VHD-Dateiformat aufweisen. Die Größe der VHD muss statisch sein und als ganze, durch 8 teilbare Anzahl von Megabytes angegeben werden. Die maximal zulässige Größe für die virtuelle Festplatte beträgt 1.023 GB.

* Wenn Sie über ein Windows-VM-Image im VHDX-Format verfügen, konvertieren Sie es auf eine der folgenden Arten in eine virtuelle Festplatte:
  
  * Hyper-V: Öffnen Sie Hyper-V, und wählen Sie auf der linken Seite Ihren lokalen Computer aus. Klicken Sie im Menü darüber auf **Aktion** > **Datenträger bearbeiten...**. Klicken Sie auf **Weiter**, um durch die Bildschirme zu navigieren, und geben Sie dabei folgende Optionen ein: *Pfad für die VHDX-Datei* > **Konvertieren** > **VHD** > **Feste Größe** > *Pfad für die neue VHD-Datei*. Klicken Sie zum Abschluss auf **Fertig stellen**.
  * [Convert-VHD (PowerShell-Cmdlet)](http://technet.microsoft.com/library/hh848454.aspx): Weitere Informationen finden Sie im Blogbeitrag [Converting Hyper-V .vhdx to .vhd file formats](https://blogs.technet.microsoft.com/cbernier/2013/08/29/converting-hyper-v-vhdx-to-vhd-file-formats-for-use-in-windows-azure/) (So wird's gemacht: Konvertieren des Hyper-V-VHDX-Dateiformats in das VHD-Dateiformat).
* Wenn Sie über ein Windows-VM-Image im [VMDK-Dateiformat](https://en.wikipedia.org/wiki/VMDK) verfügen, konvertieren Sie es mit dem [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) in eine virtuelle Festplatte. Weitere Informationen finden Sie im Blog [How to Convert a VMWare VMDK to Hyper-V VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) (Konvertieren von VMWare-VMDK in Hyper-V-VHD).

## Vorbereiten des VHD-Images für den Upload
In diesem Abschnitt wird erläutert, wie Sie den virtuellen Windows-Computer generalisieren. Sysprep entfernt unter anderem alle persönlichen Kontoinformationen. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx) (in englischer Sprache).

1. Melden Sie sich bei dem virtuellen Windows-Computer an.
2. Öffnen Sie das Eingabeaufforderungsfenster als Administrator. Wechseln Sie in das Verzeichnis **%windir%\\system32\\sysprep**, und führen Sie dann `sysprep.exe` aus.
3. Führen Sie im Dialogfeld **Systemvorbereitungstool** folgende Schritte aus:
   
   1. Wählen Sie unter **Systembereinigungsaktion** die Option **Out-of-Box-Experience (OOBE) für System aktivieren** und vergewissern Sie sich, dass das Kontrollkästchen **Verallgemeinern** aktiviert ist.
   2. Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren**.
   3. Klicken Sie auf **OK**.
      
      ![Starten von Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

</br>

## Anmelden an Azure
1. Öffnen Sie Azure PowerShell, und melden Sie sich bei Ihrem Azure-Konto an.
   
        Login-AzureRmAccount
   
    Ein Popupfenster wird geöffnet, in das Sie die Anmeldeinformationen für Ihr Azure-Konto eingeben können.
2. Rufen Sie die Abonnement-IDs für Ihre verfügbaren Abonnements ab.
   
        Get-AzureRmSubscription
3. Legen Sie das richtige Abonnement mit der Abonnement-ID fest.
   
        Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"

## Abrufen des Speicherkontos
Sie benötigen ein Speicherkonto in Azure, in das das VM-Image hochgeladen wird. Sie können ein vorhandenes Speicherkonto auswählen oder ein neues erstellen.

Zeigen Sie die verfügbaren Speicherkonten an.

        Get-AzureRmStorageAccount

Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, fahren Sie mit dem Abschnitt [Hochladen des VM-Images auf Ihr Speicherkonto](#upload-the-vm-image-to-your-storage-account) fort.

Gehen Sie andernfalls wie folgt vor, wenn Sie ein neues Speicherkonto erstellen möchten:

1. Vergewissern Sie sich, dass Sie über eine Ressourcengruppe für dieses Speicherkonto verfügen. Verwenden Sie den folgenden Befehl, um alle in Ihrem Abonnement enthaltenen Ressourcengruppen zu ermitteln:
   
        Get-AzureRmResourceGroup
2. Um eine Ressourcengruppe zu erstellen, verwenden Sie diesen Befehl:
   
        New-AzureRmResourceGroup -Name <resourceGroupName> -Location <location>
3. Erstellen Sie mit dem Cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) ein Speicherkonto in dieser Ressourcengruppe:
   
        New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Location "<location>" -SkuName "<skuName>" -Kind "Storage"

Gültige Werte für „-SkuName“ sind:

* **Standard\_LRS**: Lokal redundanter Speicher
* **Standard\_ZRS**: Zonenredundanter Speicher
* **Standard\_GRS**: Georedundanter Speicher
* **Standard\_RAGRS**: Georedundanter Speicher mit Lesezugriff
* **Premium\_LRS**: Lokal redundanter Premium-Speicher

## Hochladen des VM-Images auf Ihr Speicherkonto
Verwenden Sie das Cmdlet [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx), um das Image in einen Container in Ihrem Speicherkonto hochzuladen:

        $rgName = "<resourceGroupName>"
        $urlOfUploadedImageVhd = "<storageAccount>/<blobContainer>/<targetVHDName>.vhd"
        Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd -LocalFilePath <localPathOfVHDFile>

Hierbei gilt:

* **storageAccount** ist der Name des Speicherkontos für das Image.
* **blobContainer** ist der Blobcontainer, in dem Sie Ihr Image speichern möchten. Wenn kein vorhandener Blobcontainer mit diesem Namen gefunden wird, wird er für Sie erstellt.
* **targetVHDName** ist der Name, den Sie für die hochgeladene VHD-Datei verwenden möchten.
* **localPathOfVHDFile** ist der vollständige Pfad und Name der VHD-Datei auf Ihrem lokalen Computer.

Bei erfolgreicher Ausführung erhalten Sie eine Antwort, die etwa wie folgt aussieht:

        C:\> Add-AzureRmVhd -ResourceGroupName testUpldRG -Destination https://testupldstore2.blob.core.windows.net/testblobs/WinServer12.vhd -LocalFilePath "C:\temp\WinServer12.vhd"
        MD5 hash is being calculated for the file C:\temp\WinServer12.vhd.
        MD5 hash calculation is completed.
        Elapsed time for the operation: 00:03:35
        Creating new page blob of size 53687091712...
        Elapsed time for upload: 01:12:49

        LocalFilePath           DestinationUri
        -------------           --------------
        C:\temp\WinServer12.vhd https://testupldstore2.blob.core.windows.net/testblobs/WinServer12.vhd

Die Ausführung dieses Befehls kann – abhängig von Ihrer Netzwerkverbindung und der Größe Ihrer VHD-Datei – einige Zeit in Anspruch nehmen.

## Erstellen eines virtuellen Netzwerks
Erstellen Sie das vNet und das Subnetz des [virtuellen Netzwerks](../virtual-network/virtual-networks-overview.md).

1. Ersetzen Sie den Wert der Variablen durch Ihre eigenen Informationen. Geben Sie das Adresspräfix für das Subnetz im CIDR-Format an. Erstellen Sie die Variablen und das Subnetz.
   
        $rgName = "<resourceGroup>"
        $location = "<location>"
        $subnetName = "<subNetName>"
        $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix <0.0.0.0/0>
2. Ersetzen Sie den Wert von **$vnetName** durch den Namen des virtuellen Netzwerks. Geben Sie das Adresspräfix für das virtuelle Netzwerk im CIDR-Format an. Erstellen Sie die Variable und das virtuelle Netzwerk mit dem Subnetz.
   
        $vnetName = "<vnetName>"
        $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix <0.0.0.0/0> -Subnet $singleSubnet

## Erstellen einer öffentlichen IP-Adresse und einer Netzwerkschnittstelle
Sie benötigen eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle, um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk zu ermöglichen.

1. Ersetzen Sie den Wert von **$ipName** durch den Namen der öffentlichen IP-Adresse. Erstellen Sie die Variable und die öffentliche IP-Adresse.
   
        $ipName = "<ipName>"
        $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
2. Ersetzen Sie den Wert von **$nicName** durch den Namen der Netzwerkschnittstelle. Erstellen Sie die Variable und die Netzwerkschnittstelle.
   
        $nicName = "<nicName>"
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

## Erstellen des virtuellen Computers
Das folgende PowerShell-Skript veranschaulicht, wie Sie die Konfiguration für den virtuellen Computer einrichten und das hochgeladene VM-Image als Quelle für die neue Installation verwenden.

> [!NOTE]
> Der virtuelle Computer muss im gleichen Speicherkonto wie die hochgeladene VHD-Datei enthalten sein.
> 
> 

</br>

    #Create variables
    # Enter a new user name and password to use as the local administrator account for the remotely accessing the VM
    $cred = Get-Credential

    # Name of the storage account where the VHD file is and where the OS disk will be created
    $storageAccName = "<storageAccountName>"

    # Name of the virtual machine
    $vmName = "<vmName>"

    # Size of the virtual machine. See the VM sizes documentation for more information: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "<vmSize>"

    # Computer name for the VM
    $computerName = "<computerName>"

    # Name of the disk that holds the OS
    $osDiskName = "<osDiskName>"

    #Get the storage account where the uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    #Set the VM name and size
    #Use "Get-Help New-AzureRmVMConfig" to know the available options for -VMsize
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set the Windows operating system configuration and add the NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate

    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    #Create the OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    #Configure the OS disk to be created from the image (-CreateOption fromImage), and give the URL of the uploaded image VHD for the -SourceImageUri parameter
    #You set this variable when you uploaded the VHD
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri $urlOfUploadedImageVhd -Windows

    #Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm



Anschließend müsste der neu erstellte virtuelle Computer im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden. Alternativ können Sie ihn auch mit den folgenden PowerShell-Befehlen anzeigen:

    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name


## Nächste Schritte
Informationen zum Verwalten des neuen virtuellen Computers mithilfe von Azure PowerShell finden Sie unter [Verwalten von virtuellen Azure-Computern mit Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md).

<!---HONumber=AcomDC_0831_2016-->