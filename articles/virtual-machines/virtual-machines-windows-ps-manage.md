---
title: Verwalten von virtuellen Computern mit Resource Manager und PowerShell | Microsoft Docs
description: Verwalten Sie virtuelle Computer mit Azure Resource Manager und PowerShell
services: virtual-machines-windows
documentationcenter: ''
author: davidmu1
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/07/2016
ms.author: davidmu

---
# Verwalten von virtuellen Azure-Computern mit Resource Manager und PowerShell
## Installieren von Azure PowerShell
Unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) finden Sie Informationen dazu, wie Sie die neueste Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich bei Ihrem Azure-Konto anmelden.

## Festlegen von Variablen
Alle Befehle in diesem Artikel benötigen den Namen der Ressourcengruppe, in der sich der virtuelle Computer befindet, und den Name des zu verwaltenden virtuellen Computers. Ersetzen Sie den Wert von **$rgName** durch den Namen der Ressourcengruppe, die den virtuellen Computer enthält. Ersetzen Sie den Wert von **$vmName** durch den Namen des virtuellen Computers. Erstellen Sie die Variablen.

    $rgName = "resource-group-name"
    $vmName = "VM-name"

## Anzeigen von Informationen zu einem virtuellen Computer
Rufen Sie die Informationen zum virtuellen Computers ab.

    Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Folgendes sollte angezeigt werden:

    ResourceGroupName        : rg1
    Id                       : /subscriptions/{subscription-id}/resourceGroups/
                               rg1/providers/Microsoft.Compute/virtualMachines/vm1
    Name                     : vm1
    Type                     : Microsoft.Compute/virtualMachines
    Location                 : centralus
    Tags                     : {}
    AvailabilitySetReference : {
                                  "id": "/subscriptions/{subscription-id}/resourceGroups/
                                  rg1/providers/Microsoft.Compute/availabilitySets/av1"
                               }
    Extensions               : []
    HardwareProfile          : {
                                  "vmSize": "Standard_A0"
                               }
    InstanceView             : null
    NetworkProfile           : {
                                  "networkInterfaces": [
                                    {
                                      "properties.primary": null,
                                      "id": "/subscriptions/{subscription-id}/resourceGroups/
                                      rg1/providers/Microsoft.Network/networkInterfaces/nc1"
                                    }
                                  ]
                               }
    OSProfile                : {
                                  "computerName": "vm1",
                                  "adminUsername": "myaccount1",
                                  "adminPassword": null,
                                  "customData": null,
                                  "windowsConfiguration": {
                                    "provisionVMAgent": true,
                                    "enableAutomaticUpdates": true,
                                    "timeZone": null,
                                    "additionalUnattendContents": [],
                                    "winRM": null
                                  },
                                  "linuxConfiguration": null,
                                  "secrets": []
                               }
    Plan                     : null
    ProvisioningState        : Succeeded
    StorageProfile           : {
                                  "imageReference": {
                                    "publisher": "MicrosoftWindowsServer",
                                    "offer": "WindowsServer",
                                    "sku": "2012-R2-Datacenter",
                                    "version": "latest"
                                  },
                                  "osDisk": {
                                    "osType": "Windows",
                                    "encryptionSettings": null,
                                    "name": "osdisk",
                                    "vhd": {
                                      "Uri": "http://sa1.blob.core.windows.net/vhds/osdisk1.vhd"
                                    }
                                    "image": null,
                                    "caching": "ReadWrite",
                                    "createOption": "FromImage"
                                  }
                                  "dataDisks": [],
                               }
    DataDiskNames            : {}
    NetworkInterfaceIDs      : {/subscriptions/{subscription-id}/resourceGroups/
                                rg1/providers/Microsoft.Network/networkInterfaces/nc1}

## Starten eines virtuellen Computers
Starten Sie den virtuellen Computer.

    Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Nach ein paar Minuten sollte eine Ausgabe ähnlich der folgenden angezeigt werden:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## Beenden eines virtuellen Computers
Beenden Sie den virtuellen Computer.

    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Sie werden aufgefordert, den Vorgang zu bestätigen:

    Virtual machine stopping operation
    This cmdlet will stop the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Geben Sie **Y** ein, um den virtuellen Computer zu beenden.

Nach ein paar Minuten sollte eine Ausgabe ähnlich der folgenden angezeigt werden:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## Neustarten eines virtuellen Computers
Starten Sie den virtuellen Computer neu.

    Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Folgendes sollte angezeigt werden:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## Löschen eines virtuellen Computers
Löschen Sie den virtuellen Computer.

    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [!NOTE]
> Mit dem Parameter **- Force** können Sie die Bestätigungsaufforderung überspringen.
> 
> 

Wenn Sie den Parameter "-Force" nicht verwendet haben, werden Sie aufgefordert, den Vorgang zu bestätigen:

    Virtual machine removal operation
    This cmdlet will remove the specified virtual machine. Do you want to continue?
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Folgendes sollte angezeigt werden:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

## Ändern der Größe eines virtuellen Computers
Dieses Beispiel zeigt, wie Sie die Größe des virtuellen Computers aktualisieren.

    $vmSize = "Standard_A1"
    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    $vm.HardwareProfile.vmSize = $vmSize
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Folgendes sollte angezeigt werden:

    RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
    ---------  -------------------  ----------  ------------
                              True          OK  OK

Eine Liste mit den verfügbaren Größen für einen virtuellen Computer finden Sie unter [Größen für virtuelle Computer in Azure](virtual-machines-windows-sizes.md).

## Hinzufügen eines Datenträgers zu einem virtuellen Computer
Dieses Beispiel zeigt, wie Sie einem vorhandenen virtuellen Computer einen Datenträger hinzufügen.

    $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    Add-AzureRmVMDataDisk -VM $vm -Name "disk-name" -VhdUri "https://mystore1.blob.core.windows.net/vhds/datadisk1.vhd" -LUN 0 -Caching ReadWrite -DiskSizeinGB 1 -CreateOption Empty
    Update-AzureRmVM -ResourceGroupName $rgName -VM $vm

Der hinzugefügte Datenträger ist nicht initialisiert. Wenn Sie den Datenträger initialisieren möchten, können Sie sich bei dem Datenträger anmelden und die Datenträgerverwaltung verwenden. Wenn Sie bei der Erstellung WinRM und ein Zertifikat darauf installiert haben, können Sie den Datenträger mit einem PowerShell-Remoteaufruf initialisieren. Alternativ können Sie auch eine benutzerdefinierte Skripterweiterung verwenden:

    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"

Der Skriptdateiinhalt für die Datenträgerinitialisierung kann etwa wie folgt aussehen:

    $disks = Get-Disk |   Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { ([char]$_) }
    $count = 0
    $labels = @("data1","data2")

    foreach($d in $disks) {
        $driveLetter = $letters[$count].ToString()
        $d | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] `
            -Confirm:$false -Force 
        $count++
    }

## Nächste Schritte
Sollten bei einer Bereitstellung Probleme aufgetreten sein, helfen Ihnen ggf. die Informationen unter [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) weiter.

<!---HONumber=AcomDC_0907_2016-->