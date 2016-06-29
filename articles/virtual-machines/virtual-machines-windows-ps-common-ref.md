<properties 
   pageTitle="Häufige PowerShell-Befehle für VMs | Microsoft Azure"
   description="Enthält häufig verwendete PowerShell-Befehle als Einstiegshilfe zum Erstellen und Verwalten Ihrer VMs in Azure unter Windows."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="davidmu1" 
   manager="timlt" 
   editor="tysonn" 
   tags="azure-resource-manager"/>
   
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="06/07/2016"
   ms.author="davidmu" />

# Häufige PowerShell-Befehle zum Erstellen und Verwalten von VMs

In diesem Artikel werden einige Azure PowerShell-Befehle beschrieben, die Sie zum Erstellen und Verwalten virtueller Computer Ihres Azure-Abonnements verwenden können. Eine ausführlichere Hilfe mit speziellen Befehlszeilenswitches und -optionen erhalten Sie, indem Sie den *Befehl* **Get-Help** verwenden.

## Erstellen von Ressourcen mit Azure PowerShell

Informationen dazu, wie Sie die aktuelle Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich beim Azure-Konto anmelden, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

Aufgabe | Befehl
-------------- | -------------------------
Erstellen einer VM-Konfiguration | $vm = [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) -VMName "vm\_name" -VMSize "vm\_size"<BR></BR><BR></BR>Die VM-Konfiguration wird verwendet, um Einstellungen für die VM zu definieren oder zu aktualisieren. Die Konfiguration wird mit dem Namen der VM und ihrer [Größe](virtual-machines-windows-sizes.md) initialisiert.
Hinzufügen von Konfigurationseinstellungen | $vm = [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) -VM $vm -Windows -ComputerName "computer\_name" -Credential $cred -ProvisionVMAgent -EnableAutoUpdate<BR></BR><BR></BR>Betriebssystemeinstellungen einschließlich der [Anmeldeinformationen](https://technet.microsoft.com/library/hh849815.aspx) werden dem Konfigurationsobjekt hinzugefügt, das Sie zuvor mit New-AzureRmVMConfig erstellt haben.
Hinzufügen einer Netzwerkschnittstelle | $vm = [Add-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) -VM $vm -Id $nic.Id<BR></BR><BR></BR>Eine VM muss über eine [Netzwerkschnittstelle](virtual-machines-windows-ps-create.md) für die Kommunikation in einem virtuellen Netzwerk verfügen. Sie können auch [Get-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619434.aspx) verwenden, um ein vorhandenes Netzwerkschnittstellenobjekt abzurufen.
Angeben eines Plattformimage | $vm = [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) -VM $vm -PublisherName "publisher\_name" -Offer "publisher\_offer" -Skus "product\_sku" -Version "latest"<BR></BR><BR></BR>[Imageinformationen](virtual-machines-windows-cli-ps-findimage.md) werden dem Konfigurationsobjekt hinzugefügt, das Sie zuvor mit New-AzureRmVMConfig erstellt haben. Das von diesem Befehl zurückgegebene Objekt wird nur verwendet, wenn Sie für den Betriebssystem-Datenträger die Verwendung eines Plattformimage festlegen.
Festlegen der Verwendung eines Plattformimage für den Betriebssystem-Datenträger | $vm = [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) -VM $vm -Name "disk\_name" -VhdUri "http://mystore1.blob.core.windows.net/vhds/disk\_name.vhd" -CreateOption fromImage<BR></BR><BR></BR>Der Name des Betriebssystem-Datenträgers und die Anordnung im [Speicher](../storage/storage-powershell-guide-full.md) werden dem zuvor erstellten Konfigurationsobjekt hinzugefügt.
Festlegen der Verwendung eines generalisierten Image für den Betriebssystem-Datenträger | $vm = Set-AzureRmVMOSDisk -VM $vm -Name "disk\_name" -SourceImageUri "https://mystore1.blob.core.windows.net/system/Microsoft.Compute/Images/myimages/myprefix-osDisk.{guid}.vhd" -VhdUri "https://mystore1.blob.core.windows.net/vhds/disk\_name.vhd" -CreateOption fromImage -Windows<BR></BR><BR></BR>Der Name des Betriebssystem-Datenträgers, der Speicherort des Quellimage und die Anordnung des Datenträgers im [Speicher](../storage/storage-powershell-guide-full.md) werden dem zuvor erstellten Konfigurationsobjekt hinzugefügt.
Festlegen der Verwendung eines spezialisierten Image für den Betriebssystem-Datenträger | $vm = Set-AzureRmVMOSDisk -VM $vm -Name "name\_of\_disk" -VhdUri "http://mystore1.blob.core.windows.net/vhds/" -CreateOption Attach -Windows
Erstellen einer VM | [New-AzureRmVM]() -ResourceGroupName "resource\_group\_name" -Location "location\_name" -VM $vm<BR></BR><BR></BR>Alle Ressourcen werden in einer [Ressourcengruppe](../powershell-azure-resource-manager.md) erstellt. Die VM muss an demselben [Speicherort](https://msdn.microsoft.com/library/azure/dn495177.aspx) wie die Ressourcengruppe erstellt werden. Führen Sie vor dem Ausführen dieses Befehls New-AzureRmVMConfig, Set-AzureRmVMOperatingSystem, Set-AzureRmVMSourceImage, Add-AzureRmVMNetworkInterface und Set-AzureRmVMOSDisk aus.
Auflisten der VMs eines Abonnements| [Get-AzureRmVM](https://msdn.microsoft.com/library/mt603718.aspx)
Auflisten der VMs einer Ressourcengruppe | Get-AzureRmVM -ResourceGroupName "resource\_group\_name"<BR></BR><BR></BR>Verwenden Sie zum Abrufen einer Liste mit Ressourcengruppen Ihres Abonnements den Befehl [Get-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt679016.aspx).
Abrufen von Informationen zu einem virtuellen Computer | Get-AzureRmVM -ResourceGroupName "resource\_group\_name" -Name "vm\_name"
Starten eines virtuellen Computers | [Start-AzureRmVM](https://msdn.microsoft.com/library/mt603453.aspx) -ResourceGroupName "resource\_group\_name" -Name "vm\_name"
Anhalten eines virtuellen Computers | [Stop-AzureRmVM](https://msdn.microsoft.com/library/mt603483.aspx) -ResourceGroupName "resource\_group\_name" -Name "vm\_name"
Neustarten einer ausgeführten VM | [Restart-AzureRmVM](https://msdn.microsoft.com/library/mt603775.aspx) -ResourceGroupName "resource\_group\_name" -Name "vm\_name"
Löschen eines virtuellen Computers | [Remove-AzureRmVM](https://msdn.microsoft.com/library/mt603641.aspx) -ResourceGroupName "resource\_group\_name" -Name "vm\_name"
Generalisieren einer VM | [Set-AzureRmVm](https://msdn.microsoft.com/library/mt603688.aspx) -ResourceGroupName YourResourceGroup -Name "vm\_name" -Generalized<BR></BR><BR></BR>Sie müssen diesen Befehl ausführen, bevor Sie Save-AzureRmVMImage ausführen.
Erfassen eines virtuellen Computers | [Save-AzureRmVMImage](https://msdn.microsoft.com/library/mt619423.aspx) -ResourceGroupName "resource\_group\_name" -VMName "vm\_name" -DestinationContainerName "image\_container" -VHDNamePrefix "image\_name\_prefix" -Path "C:\\filepath\\filename.json"<BR></BR><BR></BR>Ein virtueller Computer muss [heruntergefahren und generalisiert](virtual-machines-windows-capture-image.md) werden, damit er zum Erstellen eines Image verwendet werden kann. Führen Sie Set-AzureRmVm aus, bevor Sie diesen Befehl ausführen.
Aktualisieren einer VM | [Update-AzureRmVM](https://msdn.microsoft.com/library/mt603662.aspx) -ResourceGroupName "resource\_group\_name" -VM $vm<BR></BR><BR></BR>Rufen Sie die aktuelle VM-Konfiguration ab, indem Sie Get-AzureRmVM verwenden, ändern Sie die Konfigurationseinstellungen auf dem VM-Objekt, und führen Sie dann diesen Befehl aus.
Hinzufügen eines Datenträgers zu einem virtuellen Computer | [Add-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603673.aspx) -VM $vm -Name "disk\_name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/disk\_name.vhd" -LUN # -Caching ReadWrite -DiskSizeinGB # -CreateOption Empty<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM, um das VM-Objekt abzurufen. Geben Sie die LUN-Nummer und die Größe des Datenträgers an. Führen Sie Update-AzureRmVM aus, um die Konfigurationsänderungen auf die VM anzuwenden. Der Datenträger, den Sie hinzufügen, ist nicht initialisiert. Informationen hierzu finden Sie unter [Verwalten von virtuellen Azure-Computern mit Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md).
Entfernen eines Datenträgers aus einem virtuellen Computer | [Remove-AzureRmVMDataDisk](https://msdn.microsoft.com/library/mt603614.aspx) -VM $vm -Name "disk\_name"<BR></BR><BR></BR>Verwenden Sie Get-AzureRmVM, um das VM-Objekt abzurufen. Führen Sie Update-AzureRmVM aus, um die Konfigurationsänderungen auf die VM anzuwenden.
Hinzufügen einer Erweiterung zu einer VM | [Set-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603745.aspx) -ResourceGroupName "resource\_group\_name" -Location "azure\_location" -VMName "vm\_name" -Name "extension\_name" -Publisher "publisher\_name" -Type "extension\_type" -TypeHandlerVersion "#.#" -Settings $Settings -ProtectedSettings $ProtectedSettings<BR></BR><BR></BR>Führen Sie diesen Befehl mit den richtigen [Konfigurationsinformationen](virtual-machines-windows-extensions-configuration-samples.md) für die Erweiterung aus, die Sie installieren möchten.
Entfernen einer VM-Erweiterung | [Remove-AzureRmVMExtension](https://msdn.microsoft.com/library/mt603782.aspx) -ResourceGroupName "resource\_group\_name" -Name "extension\_name" -VMName "vm\_name"

## Nächste Schritte

- Sehen Sie sich die grundlegenden Schritte zum Erstellen eines virtuellen Computers unter [Erstellen einer Windows-VM mit dem Resource Manager und PowerShell](virtual-machines-windows-ps-create.md) an.

<!---HONumber=AcomDC_0615_2016-->