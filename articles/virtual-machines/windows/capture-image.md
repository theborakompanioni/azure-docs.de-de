---
title: Erfassen eines VM-Image von einem generalisierten virtuellen Azure-Computer | Microsoft Docs
description: Es wird beschrieben, wie Sie ein VM-Image von einem generalisierten virtuellen Azure-Computer erfassen, der mit dem Resource Manager-Bereitstellungsmodell erstellt wurde.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: afdae4a1-6dfb-47b4-902a-f327f9bfe5b4
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 114e97bcd38fc69d62960c3305dc7f75ce13b66b
ms.lasthandoff: 04/03/2017


---
# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Erfassen eines VM-Image von einem generalisierten virtuellen Azure-Computer
In diesem Artikel wird gezeigt, wie Sie Azure PowerShell zum Erstellen eines Image von einem generalisierten virtuellen Azure-Computer verwenden. Sie können das Image dann nutzen, um eine andere VM zu erstellen. Das Image umfasst den Betriebssystemdatenträger und die an den virtuellen Computer angefügten Datenträger. Das Image enthält nicht die Ressourcen des virtuellen Netzwerks. Sie müssen diese Ressourcen also einrichten, wenn Sie die neue VM erstellen. 

## <a name="prerequisites"></a>Voraussetzungen
* Sie müssen [die VM bereits generalisiert haben](generalize-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Beim Generalisieren einer VM werden u.a. alle persönlichen Kontoinformationen entfernt, und der Computer wird für die Verwendung als Image vorbereitet. Sie können auch einen virtuellen Linux-Computer unter Verwendung von `sudo waagent -deprovision+user` generalisieren und dann mithilfe von PowerShell erfassen. Informationen zur Verwendung der Befehlszeilenschnittstelle zum Erfassen eines virtuellen Computers finden Sie unter [Generalisieren und Erfassen eines virtuellen Linux-Computers mithilfe der Azure-Befehlszeilenschnittstelle](../linux/capture-image.md).
* Hierfür muss Azure PowerShell Version 1.0.x oder höher installiert sein. Wenn Sie PowerShell noch nicht installiert haben, finden Sie die Installationsschritte unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs) .

## <a name="log-in-to-azure-powershell"></a>Anmelden an Azure PowerShell
1. Öffnen Sie Azure PowerShell, und melden Sie sich bei Ihrem Azure-Konto an.
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    Ein Popupfenster wird geöffnet, in das Sie die Anmeldeinformationen für Ihr Azure-Konto eingeben können.
2. Rufen Sie die Abonnement-IDs für Ihre verfügbaren Abonnements ab.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Legen Sie das richtige Abonnement mit der Abonnement-ID fest.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-the-vm-and-set-the-state-to-generalized"></a>Aufheben der VM-Zuordnung und Festlegen des Status auf „Generalisiert“
1. Heben Sie die Zuordnung der VM-Ressourcen auf.
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    Der *Status* der VM im Azure-Portal ändert sich von **Beendet** in **Beendet (Zuordnung aufgehoben)**.
2. Legen Sie den Status des virtuellen Computers auf **Generalisiert**fest. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. Überprüfen Sie den Status der VM. Im Abschnitt **OSState/generalized** der VM sollte **DisplayStatus** auf **VM generalized** festgelegt sein.  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-the-image"></a>Erstellen des Image
1. Kopieren Sie das Image des virtuellen Computers mithilfe des folgenden Befehls in den Zielspeichercontainer: Das Image wird im demselben Speicherkonto wie der ursprüngliche virtuelle Computer erstellt. Mit dem Parameter `-Path` wird eine Kopie der JSON-Vorlage lokal gespeichert. Der Parameter `-DestinationContainerName` ist der Name des Containers, in dem Ihre Images aufbewahrt werden sollen. Falls der Container noch nicht vorhanden ist, wird er erstellt.
   
    ```powershell
    Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
        -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
        -Path <C:\local\Filepath\Filename.json>
    ```
   
    Sie können die URL Ihres Image aus der JSON-Dateivorlage abrufen. Wechseln Sie zum Abschnitt **resources** > **storageProfile** > **osDisk** > **image** > **uri**, um den vollständigen Pfad zu Ihrem Image zu finden. Die URL des Image sieht wie folgt aus: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
   
    Sie können den URI auch im Portal überprüfen. Das Image wird in einen Container mit dem Namen **system** in Ihrem Speicherkonto kopiert. 

## <a name="next-steps"></a>Nächste Schritte
* Jetzt können Sie [eine VM aus dem Image erstellen](create-vm-generalized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


