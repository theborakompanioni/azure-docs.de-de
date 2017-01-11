---
title: "Häufige PowerShell-Befehle für VMs | Microsoft Docs"
description: "Enthält häufig verwendete PowerShell-Befehle als Einstiegshilfe zum Erstellen und Verwalten Ihrer VMs in Azure unter Windows."
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
ms.date: 09/27/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 45a45b616b4de005da66562c69eef83f2f48cc79
ms.openlocfilehash: d86db0f0487c8c3627fa91b656db6a4c1576348c


---
# <a name="common-powershell-commands-for-creating-and-managing-vms"></a>Häufige PowerShell-Befehle zum Erstellen und Verwalten von VMs
In diesem Artikel werden einige Azure PowerShell-Befehle beschrieben, die Sie zum Erstellen und Verwalten virtueller Computer Ihres Azure-Abonnements verwenden können.  Eine ausführlichere Hilfe mit speziellen Befehlszeilenschaltern und -optionen erhalten Sie mit dem *Befehl* **Get-Help**.

Unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

## <a name="create-a-vm"></a>Erstellen einer VM
| Aufgabe | Get-Help |
| --- | --- |
| Erstellen einer VM-Konfiguration |$vm = [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) -VMName "vm_name" -VMSize "vm_size"<BR></BR><BR></BR>Die VM-Konfiguration wird verwendet, um Einstellungen für die VM zu definieren oder zu aktualisieren. Die Konfiguration wird mit dem Namen der VM und ihrer [Größe](virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) initialisiert. |
| Hinzufügen von Konfigurationseinstellungen |$vm = [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) -VM $vm -Windows -ComputerName "computer_name" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate<BR></BR><BR></BR>Betriebssystemeinstellungen einschließlich der [Anmeldeinformationen](https://technet.microsoft.com/library/hh849815.aspx) werden dem Konfigurationsobjekt hinzugefügt, das Sie zuvor mit New-AzureRmVMConfig erstellt haben. |
| Hinzufügen einer Netzwerkschnittstelle |$vm = [Add-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) -VM $vm -Id $nic.Id<BR></BR><BR></BR>Eine VM muss über eine [Netzwerkschnittstelle](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) für die Kommunikation in einem virtuellen Netzwerk verfügen. Sie können auch [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) verwenden, um ein vorhandenes Netzwerkschnittstellenobjekt abzurufen. |
| Angeben eines Plattformimage |$vm = [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) -VM $vm -PublisherName "publisher_name" -Offer "publisher_offer" -Skus "product_sku" -Version "latest"<BR></BR><BR></BR>[Imageinformationen](virtual-machines-windows-cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) werden dem Konfigurationsobjekt hinzugefügt, das Sie zuvor mit New-AzureRmVMConfig erstellt haben. Das von diesem Befehl zurückgegebene Objekt wird nur verwendet, wenn Sie für den Betriebssystem-Datenträger die Verwendung eines Plattformimage festlegen. |
| Festlegen der Verwendung eines Plattformimage für den Betriebssystem-Datenträger |$vm = [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) -VM $vm -Name "disk_name" -VhdUri "http://mystore1.blob.core.windows.net/vhds/disk_name.vhd" -CreateOption FromImage<BR></BR><BR></BR>Der Name des Betriebssystem-Datenträgers und seine Anordnung im [Speicher](../storage/storage-powershell-guide-full.md) werden dem zuvor erstellten Konfigurationsobjekt hinzugefügt. |
| Festlegen der Verwendung eines generalisierten Image für den Betriebssystem-Datenträger |$vm = Set-AzureRmVMOSDisk -VM $vm -Name "disk_name" -SourceImageUri "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk.{guid}.vhd" -VhdUri "https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" -CreateOption FromImage -Windows<BR></BR><BR></BR>Der Name des Betriebssystem-Datenträgers, der Speicherort des Quellimages und die Anordnung des Datenträgers im [Speicher](../storage/storage-powershell-guide-full.md) werden dem Konfigurationsobjekt hinzugefügt. |
| Festlegen der Verwendung eines spezialisierten Image für den Betriebssystem-Datenträger |$vm = Set-AzureRmVMOSDisk -VM $vm -Name "name_of_disk" -VhdUri "http://mystore1.blob.core.windows.net/vhds/" -CreateOption Attach -Windows |
| Erstellen einer VM |[New-AzureRmVM]() -ResourceGroupName "resource_group_name" -Location "location_name" -VM $vm<BR></BR><BR></BR>Alle Ressourcen werden in einer [Ressourcengruppe](../powershell-azure-resource-manager.md) erstellt. Die VM muss an demselben [Speicherort](https://msdn.microsoft.com/library/azure/dn495177.aspx) wie die Ressourcengruppe erstellt werden. Führen Sie vor dem Ausführen dieses Befehls New-AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Add-AzureRmVMNetworkInterface und Set-AzureRmVMOSDisk aus. |

## <a name="get-information-about-vms"></a>Abrufen von Informationen zu virtuellen Computern
| Aufgabe | Befehl |
| --- | --- |
| Auflisten der VMs eines Abonnements |[Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx) |
| Auflisten der VMs einer Ressourcengruppe |Get-AzureRmVM -ResourceGroupName "resource_group_name"<BR></BR><BR></BR>Verwenden Sie zum Abrufen einer Liste mit Ressourcengruppen Ihres Abonnements den Befehl [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx)verwenden. |
| Abrufen von Informationen zu einem virtuellen Computer |Get-AzureRmVM -ResourceGroupName "resource_group_name" -Name "vm_name" |

## <a name="manage-vms"></a>Verwalten von VMs
| Aufgabe | Befehl |
| --- | --- |
| Starten eines virtuellen Computers |[Start-AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) -ResourceGroupName "resource_group_name" -Name "vm_name" |
| Anhalten eines virtuellen Computers |[Stop-AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) -ResourceGroupName "resource_group_name" -Name "vm_name" |
| Neustarten einer ausgeführten VM |[Restart-AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) -ResourceGroupName "resource_group_name" -Name "vm_name" |
| Löschen eines virtuellen Computers |[Remove-AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) -ResourceGroupName "resource_group_name" -Name "vm_name" |
| Generalisieren einer VM |[Set-AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) -ResourceGroupName YourResourceGroup -Name "vm_name" -Generalized<BR></BR><BR></BR>Führen Sie diesen Befehl aus, bevor Sie Save-AzureRmVMImage ausführen. |
| Erfassen eines virtuellen Computers |[Save-AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) -ResourceGroupName "resource_group_name" -VMName "vm_name" -DestinationContainerName "image_container" -VHDNamePrefix "image_name_prefix" -Path "C:\filepath\filename.json"<BR></BR><BR></BR>Ein virtueller Computer muss [heruntergefahren und generalisiert](virtual-machines-windows-generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) werden, damit er zum Erstellen eines Images verwendet werden kann. Führen Sie Set-AzureRmVm aus, bevor Sie diesen Befehl ausführen. |
| Aktualisieren einer VM |[Update-AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) -ResourceGroupName "resource_group_name" -VM $vm<BR></BR><BR></BR>Rufen Sie die aktuelle VM-Konfiguration ab, indem Sie Get-AzureRmVM verwenden, ändern Sie die Konfigurationseinstellungen auf dem VM-Objekt, und führen Sie dann diesen Befehl aus. |
| Hinzufügen eines Datenträgers zu einem virtuellen Computer |[Add-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) -VM $vm -Name "disk_name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/disk_name.vhd" -LUN # -Caching ReadWrite -DiskSizeinGB # -CreateOption Empty<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM, um das VM-Objekt abzurufen. Geben Sie die LUN-Nummer und die Größe des Datenträgers an. Führen Sie Update-AzureRmVM aus, um die Konfigurationsänderungen auf die VM anzuwenden. Der hinzugefügte Datenträger ist nicht initialisiert. Informationen zum Initialisieren von Datenträgern während des Hinzufügens finden Sie unter [Verwalten von virtuellen Azure-Computern mit Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). |
| Entfernen eines Datenträgers aus einem virtuellen Computer |[Remove-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) -VM $vm -Name "disk_name"<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM, um das VM-Objekt abzurufen. Führen Sie Update-AzureRmVM aus, um die Konfigurationsänderungen auf die VM anzuwenden. |
| Hinzufügen einer Erweiterung zu einer VM |[Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) -ResourceGroupName "resource_group_name" -Location "azure_location" -VMName "vm_name" -Name "extension_name" -Publisher "publisher_name" -Type "extension_type" -TypeHandlerVersion "#.#" -Settings $Settings -ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Führen Sie diesen Befehl mit den entsprechenden [Konfigurationsinformationen](virtual-machines-windows-extensions-configuration-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) für die Erweiterung aus, die Sie installieren möchten. |
| Entfernen einer VM-Erweiterung |[Remove-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) -ResourceGroupName "resource_group_name" -Name "extension_name" -VMName "vm_name" |

## <a name="next-steps"></a>Nächste Schritte
* Die grundlegenden Schritte zum Erstellen eines virtuellen Computers finden Sie unter [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).




<!--HONumber=Dec16_HO2-->


