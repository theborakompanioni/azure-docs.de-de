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
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: 8f1d488fd8f71bf90c8bf7b7c1544445ffbd7686
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="how-to-capture-a-vm-image-from-a-generalized-azure-vm"></a>Erfassen eines VM-Image von einem generalisierten virtuellen Azure-Computer
In diesem Artikel wird gezeigt, wie Sie Azure PowerShell zum Erstellen eines Image von einem generalisierten virtuellen Azure-Computer verwenden. Sie können das Image dann nutzen, um eine andere VM zu erstellen. Das Image umfasst den Betriebssystemdatenträger und die an den virtuellen Computer angefügten Datenträger. Das Image enthält nicht die Ressourcen des virtuellen Netzwerks. Sie müssen diese Ressourcen also einrichten, wenn Sie die neue VM erstellen. 

## <a name="prerequisites"></a>Voraussetzungen
Hierfür muss Azure PowerShell Version 1.0.x oder höher installiert sein. Wenn Sie PowerShell noch nicht installiert haben, finden Sie die Installationsschritte unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview) .

## <a name="generalize-the-windows-vm-using-sysprep"></a>Generalisieren der Windows-VM mithilfe von Sysprep

Sysprep entfernt unter anderem alle persönlichen Kontoinformationen, und bereitet den Computer darauf vor, als Image verwendet zu werden. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx)(in englischer Sprache).

Stellen Sie sicher, dass die auf dem Computer ausgeführten Serverrollen von Sysprep unterstützt werden. Weitere Informationen finden Sie unter [Sysprep Support for Server Roles](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Wenn Sie Sysprep vor dem Hochladen der virtuellen Festplatte in Azure zum ersten Mal ausführen, stellen Sie sicher, dass Sie vor dem Ausführen von Sysprep [Ihren virtuellen Computer vorbereitet](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) haben. 
> 
> 

1. Melden Sie sich bei dem virtuellen Windows-Computer an.
2. Öffnen Sie das Eingabeaufforderungsfenster als Administrator. Wechseln Sie in das Verzeichnis **%windir%\system32\sysprep**, und führen Sie anschließend `sysprep.exe` aus.
3. Wählen Sie unter **Systemvorbereitungsprogramm** die Option **Out-of-Box-Experience (OOBE) für System aktivieren**, und vergewissern Sie sich, dass das Kontrollkästchen **Verallgemeinern** aktiviert ist.
4. Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren**.
5. Klicken Sie auf **OK**.
   
    ![Starten von Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. Nach Abschluss von Sysprep wird die virtuelle Maschine heruntergefahren. Starten Sie den virtuellen Computer nicht neu.


Sie können auch einen virtuellen Linux-Computer unter Verwendung von `sudo waagent -deprovision+user` generalisieren und dann mithilfe von PowerShell erfassen. Informationen zur Verwendung der Befehlszeilenschnittstelle zum Erfassen eines virtuellen Computers finden Sie unter [Generalisieren und Erfassen eines virtuellen Linux-Computers mithilfe der Azure-Befehlszeilenschnittstelle](../linux/capture-image.md).

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


