---
title: Hochladen einer spezialisierten VHD in Azure zur Erstellung eines neuen virtuellen Computers | Microsoft-Dokumentation
description: Laden Sie eine spezialisierte VHD in Azure hoch, um damit einen neuen virtuellen Computer zu erstellen.
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
ms.date: 02/05/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 204fa369dd6db618ec5340317188681b0a2988e3
ms.openlocfilehash: fc8cb82e952a05b161f00ef9ebfbd4852d3987d4


---

# <a name="upload-a-specialized-vhd-to-azure-to-use-for-creating-a-new-vm"></a>Hochladen einer spezialisierten VHD in Azure zur Erstellung eines neuen virtuellen Computers

Auf einer spezialisierten VHD werden die Benutzerkonten, Anwendungen und andere Zustandsdaten Ihres ursprünglichen virtuellen Computers beibehalten. Sie können eine spezialisierte VHD in Azure hochladen und sie zum Erstellen eines virtuellen Computers verwenden, der Managed Disks oder ein nicht verwaltetes Speicherkonto nutzt. Wir empfehlen die Verwendung von [Managed Disks](../storage/storage-managed-disks-overview.md), damit Sie die vereinfachte Verwaltung und weitere Features von Managed Disks nutzen können.


> [!IMPORTANT]
> Bevor Sie eine VHD in Azure hochladen, befolgen Sie die Anweisungen unter [Vorbereiten einer Windows-VHD oder -VHDX zum Hochladen in Azure](virtual-machines-windows-prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
>
>


* Informationen zu den Preisen der unterschiedlichen VM-Größen finden Sie unter [Virtuelle Linux-Computer – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).
* Informationen zu Speicherpreisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/). 
* Informationen zur Verfügbarkeit von VM-Größen in Azure-Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).
* Informationen zu den allgemeinen Einschränkungen von virtuellen Azure-Computern finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).

## <a name="before-you-begin"></a>Voraussetzungen
Wenn Sie PowerShell verwenden, stellen Sie sicher, dass Sie über die neueste Version des AzureRM.Compute-PowerShell-Moduls verfügen. Führen Sie den folgenden Befehl aus, um es zu installieren.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Weitere Informationen finden Sie unter [Azure PowerShell Versioning](https://docs.microsoft.com/powershell/azureps-cmdlets-docs/#azure-powershell-versioning) (Azure PowerShell-Versionsverwaltung).


## <a name="prepare-the-vm"></a>Vorbereiten des virtuellen Computers
 
Wenn Sie die spezialisierte VHD unverändert zum Erstellen eines neuen virtuellen Computers verwenden möchten, führen Sie auf jeden Fall die folgenden Schritte aus. 
  * Wenn Sie Managed Disks verwenden möchten, lesen Sie die Informationen unter [Plan for the migration to Managed Disks](virtual-machines-windows-on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) (Plan für die Migration zu Managed Disks).
  * [Vorbereiten einer Windows-VHD für das Hochladen in Azure.](virtual-machines-windows-prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Generalisieren Sie den virtuellen Computer **nicht** mithilfe von Sysprep.
  * Entfernen Sie alle auf dem virtuellen Computer installierten Gast-Virtualisierungstools und Agents (d.h. VMware-Tools).
  * Stellen Sie sicher, dass der virtuelle Computer so konfiguriert ist, dass die zugehörigen IP-Adressen und DNS-Einstellungen per Pullvorgang über DHCP übertragen werden. Dadurch wird sichergestellt, dass der Server beim Starten eine IP-Adresse innerhalb des VNET bezieht. 
  * Fahren Sie den virtuellen Computer herunter, bevor Sie fortfahren.

## <a name="log-in-to-azure"></a>Anmelden an Azure
Wenn Sie PowerShell 1.4 oder eine höhere Version noch nicht installiert haben, finden Sie entsprechende Informationen unter [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

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

Wenn Sie die VHD zum Erstellen eines verwalteten Datenträgers für einen virtuellen Computer verwenden, muss der Standort des Speicherkontos dem Standort entsprechen, an dem Sie den virtuellen Computer erstellen.

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

    
## <a name="create-the-subnet-and-vnet"></a>Erstellen des Subnetzes und des virtuellen Netzwerks

Erstellen Sie das vNet und das Subnetz des [virtuellen Netzwerks](../virtual-network/virtual-networks-overview.md).

1. Erstellen Sie das Subnetz. Dieses Beispiel erstellt ein Subnetz mit dem Namen **mySubNet** in der Ressourcengruppe **myResourceGroup** und legt das Adresspräfix **10.0.0.0/24** fest.
   
    ```powershell
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Erstellen Sie das virtuelle Netzwerk. Dieses Beispiel legt den Namen des virtuellen Netzwerks auf **myVnetName**, den Standort auf **USA, Westen** und das Adresspräfix für das virtuelle Netzwerk auf **10.0.0.0/16** fest. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-nic"></a>Erstellen einer öffentlichen IP-Adresse und einer NIC
Sie benötigen eine [öffentliche IP-Adresse](../virtual-network/virtual-network-ip-addresses-overview-arm.md) und eine Netzwerkschnittstelle, um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk zu ermöglichen.

1. Erstellen Sie die öffentliche IP-Adresse. In diesem Beispiel ist der Name der öffentlichen IP-Adresse auf **myIP** festgelegt.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Erstellen Sie die NIC. In diesem Beispiel ist der Name der NIC auf **myNicName** festgelegt.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

## <a name="create-the-network-security-group-and-an-rdp-rule"></a>Erstellen einer Netzwerksicherheitsgruppe und einer RDP-Regel
Damit Sie sich über RDP bei Ihrem virtuellen Computer anmelden können, benötigen Sie eine Sicherheitsregel, die RDP-Zugriff auf Port 3389 zulässt. Da die VHD für den neuen virtuellen Computer aus einer vorhandenen spezialisierten VM erstellt wurde, können Sie nach dem Erstellen des virtuellen Computers ein vorhandenes Konto des virtuellen Quellcomputers verwenden, der über die Berechtigung zum Anmelden über RDP verfügte.

In diesem Beispiel wird der Name der Netzwerksicherheitsgruppe auf **myNsg** und der Name der RDP-Regel auf **myRdpRule** festgelegt.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Weitere Informationen zu Endpunkten und Netzwerksicherheitsgruppen finden Sie unter [Öffnen von Ports für einen virtuellen Computer in Azure mithilfe von PowerShell](virtual-machines-windows-nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="set-the-vm-name-and-size"></a>Festlegen von Name und Größe des virtuellen Computers

In diesem Beispiel wird der Name des virtuellen Computers auf „myVM“ und ihre Größe auf „Standard_A2“ festgelegt.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

## <a name="add-the-nic"></a>Hinzufügen der Netzwerkkarte
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
## <a name="configure-the-os-disk"></a>Konfigurieren des Betriebssystem-Datenträgers

Der spezialisierte Betriebssystem-Datenträger könnte eine virtuelle Festplatte sein, die Sie [in Azure hochgeladen](virtual-machines-windows-upload-image.md) haben, oder eine [Kopie der VHD einer vorhandenen Azure-VM](virtual-machines-windows-vhd-copy.md). 

Sie können eine dieser beiden Optionen wählen:
- **Option 1:** Erstellen Sie einen spezialisierten verwalteten Datenträger auf der Grundlage einer spezialisierten VHD in einem vorhandenen Speicherkonto zur Verwendung als Betriebssystem-Datenträger.

oder 

- **Option 2:** Verwenden Sie eine spezialisierte virtuelle Festplatte, die in Ihrem eigenen Speicherkonto gespeichert ist (ein nicht verwalteter Datenträger). 

### <a name="option-1-create-a-managed-disk-from-an-unmanaged-specialized-disk"></a>Option 1: Erstellen eines verwalteten Datenträgers auf der Grundlage eines nicht verwalteten spezialisierten Datenträgers

1. Erstellen Sie einen verwalteten Datenträger auf der Grundlage der vorhandenen spezialisierten VHD in Ihrem Speicherkonto. In diesem Beispiel wird **myOSDisk1** als Name des Datenträgers verwendet, der Datenträger im **StandardLRS**-Speicher gespeichert und **https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vh.vhd** als URI für die Quell-VHD verwendet.

    ```powershell
    $osDisk = New-AzureRmDisk -DiskName "myOSDisk1" -Disk (New-AzureRmDiskConfig `
    -AccountType StandardLRS  -Location $location -CreationDataCreateOption Import `
    -SourceUri $urlOfUploadedImageVhd -ResourceGroupName $rgName
    ```

2. Fügen Sie den Betriebssystem-Datenträger der Konfiguration hinzu. In diesem Beispiel wird die Größe des Datenträgers auf **128 GB** festgelegt und der verwaltete Datenträger als **Windows**-Betriebssystem-Datenträger angefügt.
    
    ```powershell
    $vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -ManagedDiskStorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
    ```

Optional: Fügen Sie zusätzliche verwaltete Datenträger als Datenträger für Daten an. Diese Option setzt voraus, dass Sie Ihre verwalteten Datenträger für Daten mit [Verwaltete Datenträger für Daten erstellen](virtual-machines-windows-create-managed-disk-ps.md) erstellt haben. 

```powershell
$vm = Add-AzureRmVMDataDisk -VM $VirtualMachine -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1
```


### <a name="option-2-attach-a-vhd-that-is-in-an-existing-storage-account"></a>Option 2: Anfügen einer VHD, die in einem vorhandenen Speicherkonto gespeichert ist

1. Legen Sie den URI für die VHD fest, die Sie verwenden möchten. In diesem Beispiel wird die VHD-Datei mit dem Namen **myOsDisk.vhd** in einem Speicherkonto namens **myStorageAccount** in einem Container mit dem Namen **myContainer** gespeichert.

    ```powershell
    $osDiskUri = $urlOfUploadedImageVhd
    ```
2. Fügen Sie den Betriebssystem-Datenträger mithilfe der URL der kopierten Betriebssystem-VHD hinzu. In diesem Beispiel wird beim Erstellen des Betriebssystem-Datenträgers der Begriff „osDisk“ dem Namen des virtuellen Computers angefügt, um den Namen des Betriebssystem-Datenträgers zu erstellen. In diesem Beispiel wird auch angegeben, dass diese Windows-basierte VHD dem virtuellen Computer als Betriebssystem-Datenträger angefügt werden soll.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

Optional: Wenn Sie über Datenträger für Daten verfügen, die dem virtuellen Computer angefügt werden müssen, fügen Sie die Datenträger für Daten mit den URLs von Daten-VHDs und den entsprechenden logischen Gerätenummern (Logical Unit Number, LUN) hinzu.

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

Bei Verwendung eines Speicherkontos sehen die URLs der Daten- und Betriebssystem-Datenträger wie folgt aus: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Sie finden dies im Portal, indem Sie zum Zielspeichercontainer wechseln, auf die kopierte Betriebssystem- oder Daten-VHD klicken und dann den Inhalt der URL kopieren.


## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie den virtuellen Computer mithilfe der soeben erstellten Konfigurationen.

```powershell
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

War dieser Befehl erfolgreich, erscheint eine Ausgabe wie die folgende:

```
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

## <a name="verify-that-the-vm-was-created"></a>Sicherstellen, dass der virtuelle Computer erstellt wurde
Der neu erstellte virtuelle Computer müsste nun im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden. Alternativ dazu können Sie ihn mit den folgenden PowerShell-Befehlen anzeigen:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Nächste Schritte
Melden Sie sich beim neuen virtuellen Computer an, wechseln Sie zum virtuellen Computer im [Portal](https://portal.azure.com), klicken Sie auf **Verbinden**, und öffnen Sie die Remotedesktop-RDP-Datei. Verwenden Sie die Kontoanmeldeinformationen des ursprünglichen virtuellen Computers für die Anmeldung bei Ihrem neuen virtuellen Computer. Anweisungen dazu finden Sie unter [Herstellen einer Verbindung mit einem virtuellen Azure-Computer unter Windows und Anmelden bei diesem Computer](virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



<!--HONumber=Feb17_HO2-->


