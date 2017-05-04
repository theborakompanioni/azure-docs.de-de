---
title: "Häufige PowerShell-Befehle für virtuelle Azure-Computer | Microsoft-Dokumentation"
description: "Enthält häufig verwendete PowerShell-Befehle als Einstiegshilfe zum Erstellen und Verwalten Ihrer Windows-VMs in Azure."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ba3839a2-f3d5-4e19-a5de-95bfb1c0e61e
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/02/2017
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: d0a92475258176ad20cbf8e9cf34f35038d2133d
ms.lasthandoff: 04/27/2017


---
# <a name="common-powershell-commands-for-creating-and-managing-azure-virtual-machines"></a>Häufige PowerShell-Befehle zum Erstellen und Verwalten virtueller Azure-Computer

In diesem Artikel werden einige Azure PowerShell-Befehle beschrieben, die Sie zum Erstellen und Verwalten virtueller Computer Ihres Azure-Abonnements verwenden können.  Eine ausführlichere Hilfe mit speziellen Befehlszeilenschaltern und -optionen erhalten Sie mit dem *Befehl* **Get-Help**.

Unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

Diese Variablen könnten hilfreich sein, wenn Sie mehr als einen der Befehle aus diesem Artikel ausführen:

- $location – Der Speicherort des virtuellen Computers. Sie können [Get-AzureRmLocation](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.5.0/get-azurermlocation) verwenden, um eine [geografische Region](https://azure.microsoft.com/regions/) zu finden, die für Sie funktioniert.
- $myResourceGroup – Der Name der Ressourcengruppe, die den virtuellen Computer enthält.
- $myVM – Der Name des virtuellen Computers.

## <a name="create-a-vm"></a>Erstellen einer VM

| Aufgabe | Get-Help |
| ---- | ------- |
| Erstellen einer VM-Konfiguration |$vm = [New-AzureRmVMConfig](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/New-AzureRmVMConfig) -VMName $myVM -VMSize "Standard_D1_v1"<BR></BR><BR></BR>Die VM-Konfiguration wird verwendet, um Einstellungen für die VM zu definieren oder zu aktualisieren. Die Konfiguration wird mit dem Namen der VM und ihrer [Größe](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) initialisiert. |
| Hinzufügen von Konfigurationseinstellungen |$vm = [Set-AzureRmVMOperatingSystem](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Set-AzureRmVMOperatingSystem) -VM $vm -Windows -ComputerName $myVM -Credential $cred -ProvisionVMAgent -EnableAutoUpdate<BR></BR><BR></BR>Betriebssystemeinstellungen einschließlich der [Anmeldeinformationen](https://technet.microsoft.com/library/hh849815.aspx) werden dem Konfigurationsobjekt hinzugefügt, das Sie zuvor mit New-AzureRmVMConfig erstellt haben. |
| Hinzufügen einer Netzwerkschnittstelle |$vm = [Add-AzureRmVMNetworkInterface](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMNetworkInterface) -VM $vm -Id $nic.Id<BR></BR><BR></BR>Eine VM muss über eine [Netzwerkschnittstelle](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) für die Kommunikation in einem virtuellen Netzwerk verfügen. Sie können auch [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface) verwenden, um ein vorhandenes Netzwerkschnittstellenobjekt abzurufen. |
| Angeben eines Plattformimage |$vm = [Set-AzureRmVMSourceImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Set-AzureRmVMSourceImage) -VM $vm -PublisherName "publisher_name" -Offer "publisher_offer" -Skus "product_sku" -Version "latest"<BR></BR><BR></BR>[Imageinformationen](cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) werden dem Konfigurationsobjekt hinzugefügt, das Sie zuvor mit New-AzureRmVMConfig erstellt haben. Das von diesem Befehl zurückgegebene Objekt wird nur verwendet, wenn Sie für den Betriebssystem-Datenträger die Verwendung eines Plattformimage festlegen. |
| Festlegen der Verwendung eines Plattformimage für den Betriebssystem-Datenträger |$vm = [Set-AzureRmVMOSDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Set-AzureRmVMOSDisk) -VM $vm -Name "myOSDisk" -VhdUri "http://mystore1.blob.core.windows.net/vhds/myOSDisk.vhd" -CreateOption FromImage<BR></BR><BR></BR>Der Name des Betriebssystem-Datenträgers und seine Anordnung im [Speicher](../../storage/storage-powershell-guide-full.md) werden dem zuvor erstellten Konfigurationsobjekt hinzugefügt. |
| Festlegen der Verwendung eines generalisierten Image für den Betriebssystem-Datenträger |$vm = Set-AzureRmVMOSDisk -VM $vm -Name "myOSDisk" -SourceImageUri "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk.{guid}.vhd" -VhdUri "https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" -CreateOption FromImage -Windows<BR></BR><BR></BR>Der Name des Betriebssystem-Datenträgers, der Speicherort des Quellimages und die Anordnung des Datenträgers im [Speicher](../../storage/storage-powershell-guide-full.md) werden dem Konfigurationsobjekt hinzugefügt. |
| Festlegen der Verwendung eines spezialisierten Image für den Betriebssystem-Datenträger |$vm = Set-AzureRmVMOSDisk -VM $vm -Name "myOSDisk" -VhdUri "http://mystore1.blob.core.windows.net/vhds/" -CreateOption Attach -Windows |
| Erstellen einer VM |[New-AzureRmVM]() -ResourceGroupName $myResourceGroup -Location $location -VM $vm<BR></BR><BR></BR>Alle Ressourcen werden in einer [Ressourcengruppe](../../azure-resource-manager/powershell-azure-resource-manager.md) erstellt. Führen Sie vor dem Ausführen dieses Befehls New-AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Add-AzureRmVMNetworkInterface und Set-AzureRmVMOSDisk aus. |

## <a name="get-information-about-vms"></a>Abrufen von Informationen zu virtuellen Computern

| Aufgabe | Befehl |
| ---- | ------- |
| Auflisten der VMs eines Abonnements |[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) |
| Auflisten der VMs einer Ressourcengruppe |Get-AzureRmVM -ResourceGroupName $myResourceGroup<BR></BR><BR></BR>Verwenden Sie zum Abrufen einer Liste mit Ressourcengruppen Ihres Abonnements den Befehl [Get-AzureRmResourceGroup](/powershell/module/azurerm.resources/get-azurermresourcegroup)verwenden. |
| Abrufen von Informationen zu einem virtuellen Computer |Get-AzureRmVM -ResourceGroupName $myResourceGroup -Name $myVM |

## <a name="manage-vms"></a>Verwalten von VMs
| Aufgabe | Befehl |
| --- | --- |
| Starten eines virtuellen Computers |[Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Anhalten eines virtuellen Computers |[Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Neustarten einer ausgeführten VM |[Restart-AzureRmVM](/powershell/module/azurerm.compute/restart-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Löschen eines virtuellen Computers |[Remove-AzureRmVM](/powershell/module/azurerm.compute/remove-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM |
| Generalisieren einer VM |[Set-AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm) -ResourceGroupName $myResourceGroup -Name $myVM -Generalized<BR></BR><BR></BR>Führen Sie diesen Befehl aus, bevor Sie Save-AzureRmVMImage ausführen. |
| Erfassen eines virtuellen Computers |[Save-AzureRmVMImage](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Save-AzureRmVMImage) -ResourceGroupName $myResourceGroup -VMName $myVM -DestinationContainerName "myImageContainer" -VHDNamePrefix "myImagePrefix" -Path "C:\filepath\filename.json"<BR></BR><BR></BR>Ein virtueller Computer muss [heruntergefahren und generalisiert](generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) werden, damit er zum Erstellen eines Images verwendet werden kann. Führen Sie Set-AzureRmVm aus, bevor Sie diesen Befehl ausführen. |
| Aktualisieren einer VM |[Update-AzureRmVM](/powershell/module/azurerm.compute/update-azurermvm) -ResourceGroupName $myResourceGroup -VM $vm<BR></BR><BR></BR>Rufen Sie die aktuelle VM-Konfiguration ab, indem Sie Get-AzureRmVM verwenden, ändern Sie die Konfigurationseinstellungen auf dem VM-Objekt, und führen Sie dann diesen Befehl aus. |
| Hinzufügen eines Datenträgers zu einem virtuellen Computer |[Add-AzureRmVMDataDisk](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Add-AzureRmVMDataDisk) -VM $vm -Name "myDataDisk" -VhdUri "https://mystore1.blob.core.windows.net/vhds/myDataDisk.vhd" -LUN # -Caching ReadWrite -DiskSizeinGB # -CreateOption Empty<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM, um das VM-Objekt abzurufen. Geben Sie die LUN-Nummer und die Größe des Datenträgers an. Führen Sie Update-AzureRmVM aus, um die Konfigurationsänderungen auf die VM anzuwenden. Der hinzugefügte Datenträger ist nicht initialisiert. Informationen zum Initialisieren von Datenträgern während des Hinzufügens finden Sie unter [Verwalten von virtuellen Azure-Computern mit Resource Manager und PowerShell](ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Entfernen eines Datenträgers aus einem virtuellen Computer |[Remove-AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk) -VM $vm -Name "myDataDisk"<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM, um das VM-Objekt abzurufen. Führen Sie Update-AzureRmVM aus, um die Konfigurationsänderungen auf die VM anzuwenden. |
| Hinzufügen einer Erweiterung zu einer VM |[Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/resourcemanager/azurerm.compute/v2.5.0/Set-AzureRmVMExtension) -ResourceGroupName $myResourceGroup -Location $location -VMName $myVM -Name "extensionName" -Publisher "publisherName" -Type "extensionType" -TypeHandlerVersion "#.#" -Settings $Settings -ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Führen Sie diesen Befehl mit den entsprechenden [Konfigurationsinformationen](extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) für die Erweiterung aus, die Sie installieren möchten. |
| Entfernen einer VM-Erweiterung |[Remove-AzureRmVMExtension](/powershell/module/azurerm.compute/remove-azurermvmextension) -ResourceGroupName $myResourceGroup -Name "extensionName" -VMName $myVM |

## <a name="next-steps"></a>Nächste Schritte
* Die grundlegenden Schritte zum Erstellen eines virtuellen Computers finden Sie unter [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


