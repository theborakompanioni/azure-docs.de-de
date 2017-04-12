---

title: Verwenden eines virtuellen Windows-Problembehandlungscomputers mit Azure PowerShell | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie Probleme mit virtuellen Windows-Computern (VMs) in Azure behandeln, indem Sie den Betriebssystemdatenträger per Azure PowerShell mit einer Wiederherstellungs-VM verbinden."
services: virtual-machines-windows
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/13/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 50e9982fbf33a39b69effed193a4bd137c07a14d
ms.lasthandoff: 03/31/2017


---

# <a name="troubleshoot-a-windows-vm-by-attaching-the-os-disk-to-a-recovery-vm-using-azure-powershell"></a>Beheben von Problemen mit einer Windows-VM durch Hinzufügen des Betriebssystemdatenträgers zu einer Wiederherstellungs-VM per Azure PowerShell
Wenn für Ihren virtuellen Windows-Computer in Azure ein Start- oder Datenträgerfehler auftritt, müssen Sie unter Umständen Schritte zur Problembehebung auf der virtuellen Festplatte selbst ausführen. Ein gängiges Beispiel wäre ein ungültiges Anwendungsupdate, das den erfolgreichen Start der VM verhindert. In diesem Artikel wird erläutert, wie Sie Azure PowerShell zum Herstellen der Verbindung zwischen Ihrer virtuellen Festplatte und einer anderen Windows-VM verwenden, um alle Fehler zu beheben und dann Ihre ursprüngliche VM neu zu erstellen.


## <a name="recovery-process-overview"></a>Übersicht über den Wiederherstellungsprozess
Der Problembehebungsprozess sieht wie folgt aus:

1. Löschen Sie die VM, auf der Probleme auftreten, und behalten Sie die virtuellen Festplatten bei.
2. Fügen Sie einer anderen Windows-Problembehebungs-VM die virtuelle Festplatte hinzu, und stellen Sie sie bereit.
3. Stellen Sie eine Verbindung mit der Problembehebungs-VM her. Bearbeiten Sie Dateien, oder führen Sie ein beliebiges Tool zum Beheben von Problemen auf der ursprünglichen virtuellen Festplatte aus.
4. Heben Sie die Bereitstellung auf, und trennen Sie die virtuelle Festplatte von der Problembehebungs-VM.
5. Erstellen Sie eine VM mithilfe der ursprünglichen virtuellen Festplatte.

Stellen Sie sicher, dass Sie die [aktuelle Version von Azure PowerShell](/powershell/azureps-cmdlets-docs) installiert haben und an Ihrem Abonnement angemeldet sind:

```powershell
Login-AzureRMAccount
```

Ersetzen Sie in den folgenden Beispielen die Beispielparameternamen durch Ihre eigenen Werte. Als Beispielparameternamen werden `myResourceGroup`, `mystorageaccount` und `myVM` verwendet.


## <a name="determine-boot-issues"></a>Bestimmen von Problemen beim Start
Sie können einen Screenshot Ihrer VM in Azure anzeigen, die Ihnen als Hilfe beim Behandeln von Startproblemen dient. Anhand dieses Screenshots können Sie ermitteln, warum eine VM nicht erfolgreich gestartet werden kann. Im folgenden Beispiel stammt der Screenshot von der Windows-VM mit dem Namen `myVM` in der Ressourcengruppe mit dem Namen `myResourceGroup`:

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Überprüfen Sie den Screenshot, um zu bestimmen, warum die VM nicht startet. Beachten Sie alle angegebenen spezifischen Fehlermeldungen oder Fehlercodes.


## <a name="view-existing-virtual-hard-disk-details"></a>Anzeigen von Details vorhandener virtueller Festplatten
Bevor Sie Ihre virtuelle Festplatte zu einer anderen VM hinzufügen können, müssen Sie den Namen der virtuellen Festplatte (VHD) identifizieren.

Im folgenden Beispiel werden Informationen der VM namens `myVM` in der Ressourcengruppe namens `myResourceGroup` abgerufen:

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Suchen Sie in der Ausgabe des vorherigen Befehls im Abschnitt `StorageProfile` nach `Vhd URI`. In der folgenden gekürzten Beispielausgabe befindet sich die `Vhd URI` am Ende des Codeblocks:

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a>Löschen einer vorhandener VM
Virtuelle Festplatten und VMs sind zwei unterschiedliche Ressourcen in Azure. Eine virtuelle Festplatte ist der Ort, an dem das Betriebssystem selbst, Anwendungen und Konfigurationen gespeichert werden. Die VM selbst besteht nur aus Metadaten, die die Größe oder Position definieren und auf Ressourcen verweisen, z.B. eine virtuelle Festplatte oder virtuelle Netzwerkschnittstellenkarte (NIC). Jede virtuelle Festplatte verfügt über eine zugewiesene Lease, wenn sie mit einer VM verknüpft ist. Obwohl Datenträger auch bei laufendem Betrieb der VM hinzugefügt und getrennt werden können, kann der Betriebssystemdatenträger nicht getrennt werden, es sei denn, die VM-Ressource wird gelöscht. Die Lease ordnet den Betriebssystemdatenträger weiterhin einer VM zu, selbst wenn diese VM beendet und die Zuordnung aufgehoben ist.

Der erste Schritt beim Wiederherstellen Ihrer VM ist das Löschen der VM-Ressource selbst. Das Löschen der VM belässt die virtuellen Festplatten in Ihrem Speicherkonto. Nachdem die VM gelöscht wird, fügen Sie die virtuelle Festplatte zu einer anderen VM hinzu, um die Fehler zu beheben.

Im folgenden Beispiel wird die VM namens `myVM` in der Ressourcengruppe namens `myResourceGroup` gelöscht:

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Warten Sie, bis die VM gelöscht wurde, bevor Sie die virtuelle Festplatte einer anderen VM hinzufügen. Die Lease auf der virtuellen Festplatte, die sie zur VM zuordnet, muss freigegeben werden, bevor Sie die virtuelle Festplatte einer anderen VM hinzufügen können.


## <a name="attach-existing-virtual-hard-disk-to-another-vm"></a>Hinzufügen einer vorhandenen virtuellen Festplatte zu einer anderen VM
Verwenden Sie eine andere Problembehebungs-VM für die nächsten Schritte. Fügen Sie die vorhandene virtuelle Festplatte zu dieser Problembehebungs-VM hinzu, um den Inhalt des Datenträgers zu durchsuchen und zu bearbeiten. Durch diesen Prozess können Sie z.B. alle Konfigurationsfehler beheben oder zusätzliche Anwendungs- oder Systemprotokolldateien überprüfen. Wählen Sie eine andere Problembehebungs-VM aus, oder erstellen Sie eine.

Wenn Sie die vorhandene virtuelle Festplatte hinzufügen, geben Sie die URL des Datenträgers an, die im vorherigen Befehl `Get-AzureRmVM` abgerufen wurde. Das folgende Beispiel fügt eine vorhandene virtuelle Festplatte zur Problembehebungs-VM namens `myVMRecovery` in der Ressourcengruppe mit dem Namen `myResourceGroup` hinzu:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> Für das Hinzufügen eines Datenträgers müssen Sie seine Größe angeben. Da hier ein vorhandener Datenträger angefügt wird, wird `-DiskSizeInGB` als `$null` angegeben. Mit diesem Wert wird sichergestellt, dass der Datenträger richtig angefügt ist, und die wirkliche Größe des Datenträgers muss nicht ermittelt werden.


## <a name="mount-the-attached-data-disk"></a>Bereitstellen des hinzugefügten Datenträgers

1. Stellen Sie per RDP und den entsprechenden Anmeldeinformationen eine Verbindung mit Ihrer Problembehebungs-VM her. Im folgenden Beispiel wird die RDP-Verbindungsdatei für die VM mit dem Namen `myVMRecovery` in der Ressourcengruppe mit dem Namen `myResourceGroup` heruntergeladen und in `C:\Users\ops\Documents` abgelegt.

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. Der Datenträger wird automatisch erkannt und angefügt. Zeigen Sie die Liste mit den angefügten Volumes an, um den Laufwerkbuchstaben wie folgt zu ermitteln:

    ```powershell
    Get-Disk
    ```

    In der folgenden Beispielausgabe ist die virtuelle Festplatte mit dem Datenträger **2** verbunden. (Sie können auch `Get-Volume` verwenden, um den Laufwerkbuchstaben anzuzeigen.):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Beheben von Problemen auf der ursprünglichen virtuellen Festplatte
Nun, da die vorhandene virtuelle Festplatte bereitgestellt ist, können Sie jetzt alle Schritte zur Wartung und Problembehebung ausführen. Fahren Sie mit den folgenden Schritte fort, nachdem Sie die Probleme behoben haben.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Aufheben der Bereitstellung und Trennen der ursprünglichen virtuellen Festplatte
Sobald Ihre Fehler behoben sind, heben Sie die Bereitstellung auf, und trennen Sie die vorhandene virtuelle Festplatte von Ihrer Problembehebungs-VM. Sie können Ihre virtuelle Festplatte nicht mit einer anderen VM nutzen, bis die Lease freigegeben wird, die die virtuelle Festplatte zur Problembehebungs-VM hinzufügt.

1. Heben Sie in der RDP-Sitzung die Bereitstellung des Datenträgers auf der Wiederherstellungs-VM auf. Sie benötigen die Datenträgernummer aus dem vorherigen `Get-Disk`-Cmdlet. Verwenden Sie anschließend `Set-Disk`, um für den Datenträger den Offlinezustand festzulegen:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Vergewissern Sie sich, dass der Datenträger jetzt offline ist, indem Sie erneut `Get-Disk` verwenden. Die folgende Beispielausgabe zeigt, dass der Datenträger jetzt offline ist:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. Beenden Sie die RDP-Sitzung. Entfernen Sie die virtuelle Festplatte in der Azure PowerShell-Sitzung von der Problembehebungs-VM.

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a>Erstellen einer VM von der ursprünglichen Festplatte
Verwenden Sie zum Erstellen einer VM von Ihrer ursprünglichen virtuellen Festplatte [diese Azure Resource Manager-Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). Die tatsächliche JSON-Vorlage ist unter dem folgenden Link verfügbar:

- https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json

Die Vorlage stellt eine VM in einem vorhandenen virtuellen Netzwerk bereit und nutzt die VHD-URL aus dem früheren Befehl. Im folgenden Beispiel wird die Vorlage in der Ressourcengruppe namens `myResourceGroup` bereitgestellt:

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

Befolgen Sie die Eingabeaufforderungen der Vorlage, z.B. VM-Name, Betriebssystemtyp und VM-Größe. `osDiskVhdUri` wurde bereits zuvor verwendet, als die vorhandene virtuelle Festplatte zur Problembehebungs-VM hinzugefügt wurde.


## <a name="re-enable-boot-diagnostics"></a>Erneutes Aktivieren der Startdiagnose

Wenn Sie Ihre VM aus der vorhandenen virtuellen Festplatte erstellen, werden Startdiagnoseeinstellungen möglicherweise nicht automatisch aktiviert. Im folgenden Beispiel wird die Diagnoseerweiterung in der VM namens `myVMDeployed` in der Ressourcengruppe namens `myResourceGroup` aktiviert:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Nächste Schritte
Wenn Probleme beim Herstellen einer Verbindung mit Ihrer VM auftreten, helfen Ihnen die Informationen unter [Problembehandlung bei Remotedesktopverbindungen mit einem Windows-basierten virtuellen Azure-Computer](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) weiter. Konsultieren Sie [Problembehandlung beim Zugriff auf eine Anwendung, die auf einem virtuellen Azure-Computer ausgeführt wird](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) bei Problemen mit dem Zugriff auf Anwendungen, die auf Ihrer VM ausgeführt werden.

Weitere Informationen zu Resource Manager finden Sie unter [Übersicht über den Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
