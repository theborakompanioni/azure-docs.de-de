---
title: Erstellen eines VM-Image aus einer Azure-VM | Microsoft Docs
description: Es wird beschrieben, wie Sie ein generalisiertes VM-Image aus einer vorhandenen Azure-VM erstellen, die auf dem Resource Manager-Bereitstellungsmodell basiert.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 08/04/2016
ms.author: cynthn

---
# Erstellen eines VM-Image aus einer vorhandenen Azure-VM
In diesem Artikel wird gezeigt, wie Sie Azure PowerShell zum Erstellen eines generalisierten Image einer vorhandenen Azure-VM verwenden. Sie können das Image dann nutzen, um eine andere VM zu erstellen. Dieses Image umfasst den Betriebssystemdatenträger und die an den virtuellen Computer angefügten Datenträger. Das Image enthält nicht die Ressourcen des virtuellen Netzwerks. Sie müssen diese Ressourcen also einrichten, wenn Sie das Image zum Erstellen einer VM verwenden. Bei diesem Prozess wird ein [generalisiertes Windows-Image](https://technet.microsoft.com/library/hh824938.aspx) erstellt.

## Voraussetzungen
* Bei diesen Schritten wird vorausgesetzt, dass Sie bereits über einen virtuellen Azure-Computer im Resource Manager-Bereitstellungsmodell verfügen, den Sie zum Erstellen des Image verwenden möchten. Sie benötigen den VM-Namen und den Namen der Ressourcengruppe. Sie erhalten eine Liste mit den Ressourcengruppen Ihres Abonnements, indem Sie das PowerShell-Cmdlet `Get-AzureRmResourceGroup` eingeben. Sie erhalten eine Liste mit den VMs in Ihrem Abonnement, indem Sie `Get-AzureRMVM` eingeben.
* Hierfür muss Azure PowerShell Version 1.0.x installiert sein. Wenn Sie PowerShell noch nicht installiert haben, finden Sie die Installationsschritte unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
* Stellen Sie sicher, dass die auf dem Computer ausgeführten Serverrollen von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Sysprep-Unterstützung für Serverrollen).

## Vorbereiten der Quell-VM
In diesem Abschnitt erfahren Sie, wie Sie die Windows-VM generalisieren, damit sie als Image verwendet werden kann. Sichern Sie den virtuellen Computer vor dem Ausführen von Sysprep.

> [!WARNING]
> Sie können sich nicht per RDP an der VM anmelden, nachdem sie generalisiert wurde, da bei diesem Prozess alle Benutzerkonten entfernt werden. Die Änderungen können nicht rückgängig gemacht werden.
> 
> 

1. Melden Sie sich bei Ihrem virtuellen Windows-Computer an. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu **Durchsuchen** > **Virtuelle Computer** > Ihr virtueller Windows-Computer > **Verbinden**.
2. Öffnen Sie ein Eingabeaufforderungsfenster als ein Administrator.
3. Ändern Sie das Verzeichnis in `%windir%\system32\sysprep`, und führen Sie dann „sysprep.exe“ aus.
4. Führen Sie im Dialogfeld **Systemvorbereitungstool** folgende Schritte aus:
   
   * Wählen Sie unter **Systembereinigungsaktion** die Option **Out-of-Box-Experience (OOBE) für System aktivieren** und stellen Sie sicher, dass **Verallgemeinern** aktiviert ist. Weitere Informationen zur Verwendung von Sysprep finden Sie unter [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx).
   * Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren**.
   * Klicken Sie auf **OK**.
     
     ![Sysprep ausführen](./media/virtual-machines-windows-capture-image/SysprepGeneral.png)
   
   Sysprep fährt den virtuellen Computer herunter. Sein Status im Azure-Portal wird in **Beendet** geändert.

## Anmelden an Azure PowerShell
1. Öffnen Sie Azure PowerShell, und melden Sie sich bei Ihrem Azure-Konto an.
   
        Login-AzureRmAccount
   
    Ein Popupfenster wird geöffnet, in das Sie die Anmeldeinformationen für Ihr Azure-Konto eingeben können.
2. Rufen Sie die Abonnement-IDs für Ihre verfügbaren Abonnements ab.
   
        Get-AzureRmSubscription
3. Legen Sie das richtige Abonnement mit der Abonnement-ID fest.
   
        Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"

## Aufheben der VM-Zuordnung und Festlegen des Status auf „Generalisiert“
1. Heben Sie die Zuordnung der VM-Ressourcen auf.
   
        Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
   
    Der *Status* für die VM im Azure-Portal ändert sich von **Beendet** in **Beendet (Zuordnung aufgehoben)**.
2. Legen Sie den Status des virtuellen Computers auf **Generalisiert** fest.
   
        Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
3. Überprüfen Sie den Status der VM. Im Abschnitt **OSState/generalized** der VM sollte **DisplayStatus** auf **Virtueller Computer generalisiert** festgelegt sein.
   
        $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -status
        $vm.Statuses

## Erstellen des Image
1. Kopieren Sie das Image des virtuellen Computers mithilfe des folgenden Befehls in den Zielspeichercontainer: Das Image wird im demselben Speicherkonto wie der ursprüngliche virtuelle Computer erstellt. Mit der Variablen `-Path` wird eine Kopie der JSON-Vorlage lokal gespeichert. Die Variable `-DestinationContainerName` ist der Name des Containers, in dem Ihre Images aufbewahrt werden sollen. Falls der Container noch nicht vorhanden ist, wird er erstellt.
   
        Save-AzureRmVMImage -ResourceGroupName YourResourceGroup -Name YourWindowsVM -DestinationContainerName YourImagesContainer -VHDNamePrefix YourTemplatePrefix -Path Yourlocalfilepath\Filename.json
   
    Sie können die URL Ihres Image aus der JSON-Dateivorlage abrufen. Suchen Sie den Abschnitt **resources** > **storageProfile** > **osDisk** > **image** > **uri**, um den vollständigen Pfad zu Ihrem Image zu finden. Die URL des Image sieht wie folgt aus: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
   
    Sie können den URI auch im Portal überprüfen. Das Image wird in einen Container mit dem Namen **system** in Ihrem Speicherkonto kopiert.
2. Erstellen Sie eine Variable für den Pfad zum Bild.
   
        $imageURI = "<https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd>"

## Erstellen eines virtuellen Netzwerks
Erstellen Sie das vNet und das Subnetz des [virtuellen Netzwerks](../virtual-network/virtual-networks-overview.md).

1. Ersetzen Sie den Wert der Variablen durch Ihre eigenen Informationen. Geben Sie das Adresspräfix für das Subnetz im CIDR-Format an. Erstellen Sie die Variablen und das Subnetz.
   
        $rgName = "<resourceGroup>"
        $location = "<location>"
        $subnetName = "<subNetName>"
        $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix <0.0.0.0/0>
2. Ersetzen Sie den Wert von **$vnetName** durch den Namen des virtuellen Netzwerks. Geben Sie das Adresspräfix für das virtuelle Netzwerk im CIDR-Format an. Erstellen Sie die Variable und das virtuelle Netzwerk mit dem Subnetz.
   
        $vnetName = "<vnetName>"
        $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $locName -AddressPrefix <0.0.0.0/0> -Subnet $singleSubnet

## Erstellen einer öffentlichen IP-Adresse und einer Netzwerkschnittstelle
Sie benötigen eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle, um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk zu ermöglichen.

1. Ersetzen Sie den Wert von **$ipName** durch den Namen der öffentlichen IP-Adresse. Erstellen Sie die Variable und die öffentliche IP-Adresse.
   
        $ipName = "<ipName>"
        $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
2. Ersetzen Sie den Wert von **$nicName** durch den Namen der Netzwerkschnittstelle. Erstellen Sie die Variable und die Netzwerkschnittstelle.
   
        $nicName = "<nicName>"
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

## Erstellen des virtuellen Computers
Das folgende PowerShell-Skript veranschaulicht, wie Sie die Konfiguration für den virtuellen Computer einrichten und das hochgeladene VM-Image als Quelle für die neue Installation verwenden.

> [!NOTE]
> Die VM muss in demselben Speicherkonto wie die ursprüngliche VHD enthalten sein.
> 
> 

</br>

    #Create variables
    # Enter a new user name and password to use as the local administrator account for the remotely accessing the VM
    $cred = Get-Credential

    # Name of the storage account 
    $storageAccName = "<storageAccountName>"

    # Name of the virtual machine
    $vmName = "<vmName>"

    # Size of the virtual machine. See the VM sizes documentation for more information: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "<vmSize>"

    # Computer name for the VM
    $computerName = "<computerName>"

    # Name of the disk that holds the OS
    $osDiskName = "<osDiskName>"

    # Assign a SKU name
    # Valid values for -SkuName are: **Standard_LRS** - locally redundant storage, **Standard_ZRS** - zone redundant storage, **Standard_GRS** - geo redundant storage, **Standard_RAGRS** - read access geo redundant storage, **Premium_LRS** - premium locally redundant storage. 
    $skuName = "<skuName>"

    # Create a new storage account for the VM
    New-AzureRmStorageAccount -ResourceGroupName $rgName -Name $storageAccName -Location $location -SkuName $skuName -Kind "Storage"

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
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri $imageURI -Windows

    #Create the new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm



Anschließend müsste der neu erstellte virtuelle Computer im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden. Alternativ können Sie ihn auch mit den folgenden PowerShell-Befehlen anzeigen:

    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name


## Nächste Schritte
Informationen zum Verwalten des neuen virtuellen Computers mit Azure PowerShell finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md).

<!---HONumber=AcomDC_0831_2016-->