<properties
	pageTitle="Ändern der Verfügbarkeitsgruppe eines virtuellen Computers | Microsoft Azure"
	description="Erfahren Sie, wie Sie mithilfe von Azure PowerShell und des Resource Manager-Bereitstellungsmodells die Verfügbarkeitsgruppe für Ihre virtuellen Computer ändern."
	keywords=""
	services="virtual-machines-windows"
	documentationCenter=""
	authors="Drewm3"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>
<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/15/2016"
	ms.author="drewm"/>



# Ändern der Verfügbarkeitsgruppe für einen virtuellen Windows-Computer

Die folgenden Schritte beschreiben, wie Sie die Verfügbarkeitsgruppe eines virtuellen Computers über Azure PowerShell ändern. Ein virtueller Computer kann nur zum Zeitpunkt der Erstellung zu einer Verfügbarkeitsgruppe hinzugefügt werden. Um die Verfügbarkeitsgruppe zu ändern, müssen Sie den virtuellen Computer löschen und neu erstellen.

## Ändern der Verfügbarkeitsgruppe über PowerShell

1. Erfassen Sie die folgenden Schlüsselinformationen des virtuellen Computers, der geändert werden soll.

	Name des virtuellen Computers
	
	```powershell
	$vm = Get-AzureRmVM -ResourceGroupName <Name-of-resource-group> -Name <name-of-VM>
	$vm.Name
	```
 
	Größe des virtuellen Computers
	
	```powershell
	$vm.HardwareProfile.VmSize
	```

	Primäre Netzwerkschnittstelle und optionale Netzwerkschnittstellen, sofern auf dem virtuellen Computer vorhanden
	
	```powershell
	$vm.NetworkProfile.NetworkInterfaces[0].Id
	```

	Profil des Betriebssystemdatenträgers

	```powershell
	$vm.StorageProfile.OsDisk.OsType
	$vm.StorageProfile.OsDisk.Name
	$vm.StorageProfile.OsDisk.Vhd.Uri
	```

	Profile für jeden Datenträger für Daten
	
	```powershell
	$vm.StorageProfile.DataDisks[<index>].Lun
	$vm.StorageProfile.DataDisks[<index>].Vhd.Uri
	```

	Installierte VM-Erweiterungen
	
	```powershell
	$vm.Extensions
	```

2. Löschen Sie den virtuellen Computer, ohne dabei einen der Datenträger oder eine der Netzwerkschnittstellen zu löschen.

	```powershell
	Remove-AzureRmVM -ResourceGroupName <resourceGroupName> -Name <vmName> 
	```

3. Erstellen Sie die Verfügbarkeitsgruppe, sofern sie noch nicht vorhanden ist.

	```powershell
	New-AzureRmAvailabilitySet -ResourceGroupName <resourceGroupName> -Name <availabilitySetName> -Location "<location>" 
	```

4. Erstellen Sie den virtuellen Computer unter Verwendung der neuen Verfügbarkeitsgruppe neu.

	```powershell
	$vm2 = New-AzureRmVMConfig -VMName <VM-name> -VMSize <vm-size> -AvailabilitySetId <availability-set-id>

	Set-AzureRmVMOSDisk -CreateOption "Attach" -VM <vmConfig> -VhdUri <osDiskURI> -Name <osDiskName> [-Windows | -Linux]

	Add-AzureRmVMNetworkInterface -VM <vmConfig> -Id  <nicId> 

	New-AzureRmVM -ResourceGroupName <resourceGroupName> -Location <location> -VM <vmConfig>
	``` 

5. Fügen Sie Datenträger und Erweiterungen hinzu. Weitere Informationen finden Sie unter [Anfügen eines Datenträgers an einen virtuellen Computer](virtual-machines-windows-attach-disk-portal.md) und [Konfigurationsbeispiele für Erweiterungen](virtual-machines-windows-extensions-configuration-samples.md). Datenträger und Erweiterungen können über PowerShell oder die Azure-Befehlszeilenschnittstelle zum virtuellen Computer hinzugefügt werden.

## Beispielskript

Das folgende Skript zeigt ein Beispiel für das Erfassen der erforderlichen Informationen, das Löschen des ursprünglichen virtuellen Computers und das erneute Erstellen des Computers in einer neuen Verfügbarkeitsgruppe.

```powershell
	#set variables
	$rg = "demo-resource-group"
	$vmName = "demo-vm"
	$newAvailSetName = "demo-as"
	$outFile = "C:\temp\outfile.txt"

	#Get VM Details
	$OriginalVM = get-azurermvm -ResourceGroupName $rg -Name $vmName

	#Output VM details to file
	"VM Name: " | Out-File -FilePath $outFile 
	$OriginalVM.Name | Out-File -FilePath $outFile -Append

	"Extensions: " | Out-File -FilePath $outFile -Append
	$OriginalVM.Extensions | Out-File -FilePath $outFile -Append

	"VMSize: " | Out-File -FilePath $outFile -Append
	$OriginalVM.HardwareProfile.VmSize | Out-File -FilePath $outFile -Append

	"NIC: " | Out-File -FilePath $outFile -Append
	$OriginalVM.NetworkProfile.NetworkInterfaces[0].Id | Out-File -FilePath $outFile -Append

	"OSType: " | Out-File -FilePath $outFile -Append
	$OriginalVM.StorageProfile.OsDisk.OsType | Out-File -FilePath $outFile -Append

	"OS Disk: " | Out-File -FilePath $outFile -Append
	$OriginalVM.StorageProfile.OsDisk.Vhd.Uri | Out-File -FilePath $outFile -Append

	if ($OriginalVM.StorageProfile.DataDisks) {
    "Data Disk(s): " | Out-File -FilePath $outFile -Append
    $OriginalVM.StorageProfile.DataDisks | Out-File -FilePath $outFile -Append
	}

	#Remove the original VM
	Remove-AzureRmVM -ResourceGroupName $rg -Name $vmName

	#Create new availability set if it does not exist
	$availSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -ErrorAction Ignore
	if (-Not $availSet) {
    $availset = New-AzureRmAvailabilitySet -ResourceGroupName $rg -Name $newAvailSetName -Location $OriginalVM.Location
	}

	#Create the basic configuration for the replacement VM
	$newVM = New-AzureRmVMConfig -VMName $OriginalVM.Name -VMSize $OriginalVM.HardwareProfile.VmSize -AvailabilitySetId $availSet.Id
	Set-AzureRmVMOSDisk -VM $NewVM -VhdUri $OriginalVM.StorageProfile.OsDisk.Vhd.Uri  -Name $OriginalVM.Name -CreateOption Attach -Windows

	#Add Data Disks
	foreach ($disk in $OriginalVM.StorageProfile.DataDisks ) { 
    Add-AzureRmVMDataDisk -VM $newVM -Name $disk.Name -VhdUri $disk.Vhd.Uri -Caching $disk.Caching -Lun $disk.Lun -CreateOption Attach -DiskSizeInGB $disk.DiskSizeGB
	}

	#Add NIC(s)
	foreach ($nic in $OriginalVM.NetworkInterfaceIDs) {
		Add-AzureRmVMNetworkInterface -VM $NewVM -Id $nic
	}

	#Create the VM
	New-AzureRmVM -ResourceGroupName $rg -Location $OriginalVM.Location -VM $NewVM -DisableBginfoExtension
```

## Nächste Schritte

Erweitern Sie den Speicher für Ihren virtuellen Computer, indem Sie einen zusätzlichen [Datenträger](virtual-machines-windows-attach-disk-portal.md) hinzufügen.

<!---HONumber=AcomDC_0921_2016-->