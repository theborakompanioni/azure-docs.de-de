---
title: Benutzerdefinierte Skripterweiterung auf einem virtuellen Windows-Computer | Microsoft Docs
description: "Automatisieren Sie Konfigurationsaufgaben für virtuelle Azure-Computer mit der benutzerdefinierten Skripterweiterung zum Ausführen von PowerShell-Skripts auf einem virtuellen Remote-Computer unter Windows."
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: ebb7340a-8f61-4d3c-a290-d7bf8de2d0bd
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/06/2015
ms.author: kundanap
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 5a66b7a5454ae84c656e5e38e6e7072a5de49ef0


---
# <a name="custom-script-extension-for-windows-virtual-machines"></a>Benutzerdefinierte Skripterweiterung für virtuelle Windows-Computer
Dieser Artikel bietet einen Überblick über die Verwendung der benutzerdefinierten Skripterweiterung auf virtuellen Windows-Computern mithilfe von Azure PowerShell-Cmdlets mit Azure-Dienstverwaltungs-APIs.

Mit den Erweiterungen virtueller Computer, die von Microsoft und vertrauenswürdigen Drittanbietern entwickelt werden, wird die Funktionalität des virtuellen Computers erweitert. Einen Überblick über die Erweiterungen virtueller Computer finden Sie unter [Erweiterungen virtueller Azure-Computer und Features](virtual-machines-windows-extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Modell ausführen](virtual-machines-windows-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="custom-script-extension-overview"></a>Übersicht über benutzerdefinierte Skripterweiterungen
Die benutzerdefinierte Skripterweiterung für Windows ermöglicht Ihnen das Ausführen von PowerShell-Skripts auf einem virtuellen Remotecomputer, ohne sich bei diesem anzumelden. Sie können die Skripts nach Bereitstellung des virtuellen Computers oder zu einem beliebigen Zeitpunkt im Lebenszyklus des virtuellen Computers ausführen, ohne zusätzliche Ports öffnen zu müssen. Die häufigsten Anwendungsfälle für benutzerdefinierte Skripterweiterungen sind das Ausführen, Installieren und Konfigurieren zusätzlicher Software auf dem virtuellen Computer, nachdem dieser bereitgestellt wurde.

### <a name="prerequisites-for-running-the-custom-script-extension"></a>Voraussetzungen für das Ausführen der benutzerdefinierten Skripterweiterung
1. Installieren Sie <a href="http://azure.microsoft.com/downloads" target="_blank">Azure PowerShell-Cmdlets</a> der Version 0.8.0 oder höher.
2. Wenn Sie die Skripts auf einem vorhandenen virtuellen Computer ausführen möchten, müssen Sie sicherstellen, dass der Agent des virtuellen Computers darauf aktiviert ist. Wenn er nicht installiert ist, befolgen Sie diese [Schritte](virtual-machines-windows-classic-agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Wenn die VM aus dem Azure-Portal erstellt wird, wird der VM-Agent standardmäßig installiert.
3. Laden Sie die Skripts, die Sie auf der VM ausführen möchten, nach Azure Storage hoch. Die Skripts können aus einem einzelnen oder mehreren Speichercontainern stammen.
4. Das Skript sollte so geschrieben sein, dass das Eingangsskript, das durch die Erweiterung gestartet wird, weitere Skripts startet.

## <a name="custom-script-extension-scenarios"></a>Szenarien für benutzerdefinierte Skripterweiterungen
### <a name="upload-files-to-the-default-container"></a>Hochladen von Dateien in den Standardcontainer
Das folgende Beispiel zeigt, wie Sie Ihre Skripts auf dem virtuellen Computer ausführen können, wenn sie im Speichercontainer des Standardkontos Ihres Abonnements vorliegen. Sie können Ihre Skripts in ContainerName hochladen. Sie können das standardmäßige Speicherkonto mit dem Befehl **Get-AzureSubscription –Default** überprüfen.

Im folgenden Beispiel wird ein virtueller Computer erstellt. Sie können das gleiche Szenario jedoch auch auf einem vorhandenen virtuellen Computer ausführen.

    # Create a new VM in Azure.
    $vm = New-AzureVMConfig -Name $name -InstanceSize Small -ImageName $imagename
    $vm = Add-AzureProvisioningConfig -VM $vm -Windows -AdminUsername $username -Password $password
    // Add Custom Script extension to the VM. The container name refers to the storage container that contains the file.
    $vm = Set-AzureVMCustomScriptExtension -VM $vm -ContainerName $container -FileName 'start.ps1'
    New-AzureVM -ServiceName $servicename -Location $location -VMs $vm
    #  After the VM is created, the extension downloads the script from the storage location and executes it on the VM.

    # Viewing the  script execution output.
    $vm = Get-AzureVM -ServiceName $servicename -Name $name
    # Use the position of the extension in the output as index.
    $vm.ResourceExtensionStatusList[i].ExtensionSettingStatus.SubStatusList

### <a name="upload-files-to-a-non-default-storage-container"></a>Hochladen von Dateien in einen anderen Ordner als den Standardordner
Dieses Szenario zeigt, wie ein Nicht-Standardspeichercontainer verwendet wird – entweder innerhalb des gleichen Abonnements oder in einem anderen Abonnement – um Skripts und Dateien hochzuladen. In diesem Beispiel wird eine vorhandene VM verwendet, aber die gleichen Operationen können auch beim Erstellen einer neuen VM verwendet werden.

        Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileName 'file1.ps1','file2.ps1' -Run 'file.ps1' | Update-AzureVM

### <a name="upload-scripts-to-multiple-containers-across-different-storage-accounts"></a>Hochladen von Skripts in mehrere Container über verschiedene Speicherkonten hinweg
  Wenn die Skriptdateien in mehreren Containern gespeichert sind, müssen Sie die vollständige SAS-URL (Shared Access Signatures) für die Dateien bereitstellen, um die Skripts auszuführen.

      Get-AzureVM -Name $name -ServiceName $servicename | Set-AzureVMCustomScriptExtension -StorageAccountName $storageaccount -StorageAccountKey $storagekey -ContainerName $container -FileUri $fileUrl1, $fileUrl2 -Run 'file.ps1' | Update-AzureVM


### <a name="add-the-custom-script-extension-from-the-azure-portal"></a>Hinzufügen der benutzerdefinierten Skripterweiterung aus dem Azure-Portal
Navigieren Sie im <a href="https://portal.azure.com/ " target="_blank">Azure-Portal</a> zum virtuellen Computer, und fügen Sie die Erweiterung hinzu, indem Sie die auszuführende Skriptdatei angeben.

  ![Angeben der Skriptdatei][5]

### <a name="uninstall-the-custom-script-extension"></a>Deinstallieren der benutzerdefinierten Skripterweiterung
Sie können die benutzerdefinierte Skripterweiterung mithilfe des folgenden Befehls vom virtuellen Computer deinstallieren.

      get-azureVM -ServiceName KPTRDemo -Name KPTRDemo | Set-AzureVMCustomScriptExtension -Uninstall | Update-AzureVM

### <a name="use-the-custom-script-extension-with-templates"></a>Verwenden der benutzerdefinierten Skripterweiterung mit Vorlagen
Informationen zum Verwenden der benutzerdefinierten Skripterweiterung mit Azure Resource Manager-Vorlagen finden Sie unter [Verwenden der benutzerdefinierten Skripterweiterung für virtuelle Windows-Computer mit Azure Resource Manager-Vorlagen](virtual-machines-windows-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<!--Image references-->
[5]: ./media/virtual-machines-windows-classic-extensions-customscript/addcse.png



<!--HONumber=Nov16_HO3-->


