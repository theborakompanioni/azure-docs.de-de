---
title: Migrieren virtueller AWS-Computer zu Azure | Microsoft-Dokumentation
description: Migrieren Sie eine Amazon Web Services-EC2-Instanz (AWS) zu Azure Virtual Machines. Dieses Szenario verwendet Managed Disks zum Vereinfachen des Cloudspeichers.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/08/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 204fa369dd6db618ec5340317188681b0a2988e3
ms.openlocfilehash: 6854ebe781220dfdf957fa1d89ec19d32e6febea


---

# <a name="migrate-from-amazon-web-services-aws-to-azure-managed-disks"></a>Migrieren von Amazon Web Services (AWS) zu Azure Managed Disks

Sie können eine Amazon Web Services-EC2-Instanz (AWS) zu Azure migrieren, indem Sie die VHD hochladen. Wenn Sie mehrere virtuelle Computer aus dem gleichen Image in Azure erstellen möchten, müssen Sie den virtuellen Computer zuerst generalisieren und dann die generalisierte VHD in ein lokales Verzeichnis exportieren. Sobald die VHD hochgeladen wurde, können Sie einen neuen virtuellen Azure-Computer erstellen, der [Managed Disks](../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) zum Speichern verwendet. Dank Azure Managed Disks ist es nicht mehr erforderlich, Speicherkonten für Azure-IaaS-VMs zu verwalten. Sie müssen nur den Typ (Premium oder Standard) und die benötigte Größe des Datenträgers angeben, der anschließend von Azure erstellt und verwaltet wird. 

Bevor Sie den Vorgang starten, lesen Sie die Informationen in diesem Artikel: [Planen der Migration zu Managed Disks](virtual-machines-windows-on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

Bevor Sie eine VHD in Azure hochladen, befolgen Sie die Anweisungen unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="before-you-begin"></a>Voraussetzungen
Wenn Sie PowerShell verwenden, vergewissern Sie sich, dass Sie die neueste Version des AzureRM.Compute-PowerShell-Moduls verwenden. Führen Sie den folgenden Befehl aus, um es zu installieren.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell-Versionsverwaltung](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/#azure-powershell-versioning).


## <a name="generalize-the-windows-vm-using-sysprep"></a>Generalisieren des virtuellen Windows-Computers mithilfe von Sysprep

Beim Generalisieren eines virtuellen Computers mithilfe von Sysprep werden alle computerspezifischen und persönlichen Informationen von der VHD entfernt, und der Computer wird auf die Verwendung als Image vorbereitet. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx)(in englischer Sprache).

Stellen Sie sicher, dass die auf dem Computer ausgeführten Serverrollen von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Wenn Sie Sysprep vor dem Hochladen der virtuellen Festplatte in Azure zum ersten Mal ausführen, stellen Sie sicher, dass Sie vor dem Ausführen von Sysprep [Ihren virtuellen Computer vorbereitet](virtual-machines-windows-prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) haben. 
> 
> 

1. Melden Sie sich bei dem virtuellen Windows-Computer an.
2. Öffnen Sie das Eingabeaufforderungsfenster als Administrator. Wechseln Sie in das Verzeichnis **%windir%\system32\sysprep**, und führen Sie anschließend `sysprep.exe` aus.
3. Wählen Sie unter **Systemvorbereitungsprogramm** die Option **Out-of-Box-Experience (OOBE) für System aktivieren**, und vergewissern Sie sich, dass das Kontrollkästchen **Verallgemeinern** aktiviert ist.
4. Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren**.
5. Klicken Sie auf **OK**.
   
    ![Starten von Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)
6. Nach Abschluss von Sysprep wird die virtuelle Maschine heruntergefahren. Starten Sie den virtuellen Computer nicht neu.



## <a name="export-the-vhd-from-an-ec2-instance"></a>Exportieren der VHD aus einer EC2-Instanz

1.    Bei Verwendung von Amazon Web Services (AWS) exportieren Sie die EC2-Instanz in eine VHD in einem Amazon S3-Bucket. Befolgen Sie die in der Amazon-Dokumentation unter „Exporting Amazon EC2 Instances (Exportieren von Amazon EC2-Instanzen)“ beschriebenen Schritte, um die Amazon EC2-CLI (Befehlszeilenschnittstelle) zu installieren, und führen Sie den Befehl „create-instance-export-task“ zum Exportieren der EC2-Instanz in eine VHD-Datei aus. Verwenden Sie für die Variable „DISK_IMAGE_FORMAT“ unbedingt „VHD“, wenn Sie den Befehl „create-instance-export-task“ ausführen. Die exportierte VHD-Datei wird im Amazon S3-Bucket gespeichert, der während dieses Prozesses angegeben wurde.

    ```
    aws ec2 create-instance-export-task --instance-id ID --target-environment TARGET_ENVIRONMENT '
    --export-to-s3-task DiskImageFormat=DISK_IMAGE_FORMAT,ContainerFormat=ova,S3Bucket=BUCKET,S3Prefix=PREFIX
    ```

2.    Laden Sie die VHD-Datei aus dem S3-Bucket herunter. Wählen Sie die VHD-Datei und dann **Aktionen** > **Herunterladen** aus.



## <a name="log-in-to-azure"></a>Anmelden an Azure
Wenn Sie PowerShell noch nicht installiert haben, finden Sie entsprechende Informationen unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

1. Öffnen Sie Azure PowerShell, und melden Sie sich bei Ihrem Azure-Konto an. Ein Popupfenster wird geöffnet, in das Sie die Anmeldeinformationen für Ihr Azure-Konto eingeben können.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Rufen Sie die Abonnement-IDs für Ihre verfügbaren Abonnements ab.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Legen Sie das richtige Abonnement mit der Abonnement-ID fest. Ersetzen Sie `<subscriptionID>` durch die ID des richtigen Abonnements.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-the-storage-account"></a>Abrufen des Speicherkontos
Sie benötigen ein Speicherkonto in Azure, in dem das hochgeladene VM-Image gespeichert wird. Sie können ein vorhandenes Speicherkonto auswählen oder ein neues erstellen. 

Wenn Sie die VHD zum Erstellen eines verwalteten Datenträgers für eine VM verwenden, muss der Standort des Speicherkontos dem Standort entsprechen, an dem Sie die VM erstellen.

Geben Sie zum Anzeigen der verfügbaren Speicherkonten Folgendes ein:

```powershell
Get-AzureRmStorageAccount
```

Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, fahren Sie mit dem Abschnitt [Hochladen des VM-Images](#upload-the-vm-vhd-to-your-storage-account) fort.

Wenn Sie ein neues Speicherkonto erstellen möchten, gehen Sie wie folgt vor:

1. Sie benötigen den Namen der Ressourcengruppe, in der das Speicherkonto erstellt werden soll. Geben Sie den folgenden Befehl ein, um alle in Ihrem Abonnement enthaltenen Ressourcengruppen zu ermitteln:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    Zum Erstellen einer Ressourcengruppe mit dem Namen **myResourceGroup** in der Region **USA, Westen** geben Sie den folgenden Befehl ein:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Erstellen Sie mit dem Cmdlet [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) das Speicherkonto **mystorageaccount** in dieser Ressourcengruppe:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    Gültige Werte für „-SkuName“ sind:
   
   * **Standard_LRS:** lokal redundanter Speicher 
   * **Standard_ZRS:** zonenredundanter Speicher
   * **Standard_GRS:** georedundanter Speicher 
   * **Standard_RAGRS:** georedundanter Speicher mit Lesezugriff 
   * **Premium_LRS:** lokal redundanter Premium-Speicher 

## <a name="upload-the-vhd-to-your-storage-account"></a>Hochladen der VHD in Ihr Speicherkonto

Verwenden Sie das Cmdlet [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx), um die VHD in einen Container in Ihrem Speicherkonto hochzuladen. In diesem Beispiel wird die Datei **myVHD.vhd** aus `"C:\Users\Public\Documents\Virtual hard disks\"` in das Speicherkonto **mystorageaccount** in der Ressourcengruppe **myResourceGroup** hochgeladen. Die Datei wird im Container **mycontainer** abgelegt. Der neue Dateiname lautet **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Bei erfolgreicher Ausführung erhalten Sie eine Antwort, die etwa wie folgt aussieht:

```powershell
MD5 hash is being calculated for the file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for the operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Abhängig von Ihrer Netzwerkverbindung und der Größe Ihrer VHD-Datei kann die Ausführung dieses Befehls einige Zeit in Anspruch nehmen.

Speichern Sie den Pfad des **Ziel-URI** zur späteren Verwendung, wenn Sie beabsichtigen, einen verwalteten Datenträger oder einen neuen virtuellen Computer mithilfe der hochgeladenen VHD zu erstellen.

### <a name="other-options-for-uploading-a-vhd"></a>Weitere Optionen zum Hochladen einer virtuellen Festplatte

Sie können eine VHD zudem mit den folgenden Tools in ein Speicherkonto hochladen:

-   [Azure Storage Copy Blob-API](https://msdn.microsoft.com/library/azure/dd894037.aspx)

-   [Azure-Speicher-Explorer: Hochladen von Blobs](https://azurestorageexplorer.codeplex.com/)

-   [Speicher-Import/Export Service REST-API-Referenz](https://msdn.microsoft.com/library/dn529096.aspx)

    Wir empfehlen das Verwenden des Import/Export-Diensts, wenn die geschätzte Hochladedauer sieben Tage überschreitet. Sie können mithilfe von [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) die Dauer anhand der Datengröße und Übertragungseinheit schätzen. 

    Der Import/Export-Dienst kann zum Kopieren in oder aus dem Standardspeicherkonto verwendet werden. Sie benötigen ein Tool wie AzCopy zum Kopieren zwischen Standardspeicherkonto und Premium-Speicherkonto.

## <a name="create-a-managed-image-from-the-uploaded-vhd"></a>Erstellen eines verwalteten Images der hochgeladenen VHD 

Erstellen Sie ein verwaltetes Image mithilfe Ihrer generalisierten Betriebssystem-VHD.


1.  Legen Sie zunächst die allgemeinen Parameter fest:

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```

4.  Erstellen Sie das Image mithilfe Ihrer generalisierten Betriebssystem-VHD.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="setup-some-variables-for-the-image"></a>Erstellen einiger Variablen für das Image

Zunächst müssen wir grundlegende Informationen zum Image erfassen und eine Variable für das Image erstellen. In diesem Beispiel wird ein verwaltetes VM-Image mit dem Namen **myImage** verwendet, das sich in der Ressourcengruppe **myResourceGroup** in der Region **USA, Westen-Mitte** befindet. 

```powershell
$rgName = "myResourceGroup"
$location = "West Central US"
$imageName = "myImage"
$image = Get-AzureRMImage -ImageName $imageName -ResourceGroupName $rgName
```

## <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie das VNet und das Subnetz des [virtuellen Netzwerks](../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie das Subnetz. Mit diesem Beispielbefehl wird ein Subnetz namens **mySubnet** mit dem Adresspräfix **10.0.0.0/24** erstellt.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Erstellen des virtuellen Netzwerks Im folgenden Beispiel wird ein virtuelles Netzwerk namens **myVnet** mit dem Adresspräfix **10.0.0.0/16** erstellt.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Erstellen einer öffentlichen IP-Adresse und einer Netzwerkschnittstelle

Sie benötigen eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle, um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk zu ermöglichen.

1. Erstellen einer öffentlichen IP-Adresse Dieses Beispiel erstellt eine öffentliche IP-Adresse mit dem Namen **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Erstellen der NIC Dieses Beispiel erstellt eine NIC mit dem Namen **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Erstellen einer Netzwerksicherheitsgruppe und einer RDP-Regel

Damit Sie sich über RDP bei Ihrem virtuellen Computer anmelden können, benötigen Sie eine Netzwerksicherheitsregel (Network Security Rule, NSG), die RDP-Zugriff auf Port 3389 zulässt. 

Dieses Beispiel erstellt eine NSG namens **myNsg**, das eine Regel namens **myRdpRule** enthält, die RDP-Datenverkehr über Port 3389 zulässt. Weitere Informationen zu NSGs finden Sie unter [Öffnen von Ports für einen virtuellen Computer in Azure mithilfe von PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-the-virtual-network"></a>Erstellen einer Variablen für das virtuelle Netzwerk

Erstellen einer Variablen für das abgeschlossene virtuelle Netzwerk 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-the-credentials-for-the-vm"></a>Abrufen der Anmeldeinformationen für die VM

Mit dem folgenden Cmdlet wird ein Fenster geöffnet, in dem Sie einen neuen Benutzernamen und ein Kennwort als lokales Administratorkonto für den Remotezugriff auf die VM eingeben. 

```powershell
$cred = Get-Credential
```

## <a name="set-variables-for-the-vm-name-computer-name-and-the-size-of-the-vm"></a>Festlegen von Variablen für den VM-Namen, Computernamen und die VM-Größe

1. Erstellen Sie Variablen für den VM- und Computernamen. In diesem Beispiel wird der VM-Name als **myVM** und der Name des Computers als **myComputer** festgelegt.

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    ```
2. Legen Sie die Größe des virtuellen Computers fest. In diesem Beispiel wird eine VM der Größe **Standard_DS1_v2** erstellt. Weitere Informationen zu den [VM-Größen](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/) finden Sie in der Dokumentation.

    ```powershell
    $vmSize = "Standard_DS1_v2"
    ```

3. Fügen Sie der VM-Konfiguration den VM-Namen und die VM-Größe hinzu.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-the-vm-image-as-source-image-for-the-new-vm"></a>Festlegen des VM-Images als Quellimage für die neue VM

Legen Sie das Quellimage mithilfe der ID des Images der verwalteten VM fest.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-the-os-configuration-and-add-the-nic"></a>Legen Sie die Betriebssystemkonfiguration fest, und fügen Sie die Netzwerkkarte hinzu.

Geben Sie den Speichertyp (PremiumLRS oder StandardLRS) und die Größe des Betriebssystemdatenträgers ein. Bei diesem Beispiel wird der Kontotyp auf **PremiumLRS**, die Datenträgergröße auf **128 GB** und das Datenträgercaching auf **ReadWrite** festgelegt.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm  -ManagedDiskStorageAccountType PremiumLRS -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie die neue VM mithilfe der Konfiguration, die Sie erstellt und in der Variablen **$vm** gespeichert haben.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-the-vm-was-created"></a>Stellen Sie sicher, dass der virtuelle Computer erstellt wurde
Anschließend müsste der neu erstellte virtuelle Computer im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden. Alternativ können Sie ihn auch mit den folgenden PowerShell-Befehlen anzeigen:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte

Melden Sie sich beim neuen virtuellen Computer an, wechseln Sie zum virtuellen Computer im [Portal](https://portal.azure.com), klicken Sie auf **Verbinden**, und öffnen Sie die Remotedesktop-RDP-Datei. Verwenden Sie die Kontoanmeldeinformationen des ursprünglichen virtuellen Computers für die Anmeldung bei Ihrem neuen virtuellen Computer. Anweisungen dazu finden Sie unter [Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden bei diesem Computer](virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 
 



<!--HONumber=Feb17_HO2-->


