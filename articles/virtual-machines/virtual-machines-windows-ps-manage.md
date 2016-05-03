<properties
	pageTitle="Verwalten von virtuellen Computern mit Resource Manager und PowerShell | Microsoft Azure"
	description="Verwalten Sie virtuelle Computer mit Azure Resource Manager und PowerShell"
	services="virtual-machines-windows"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="na"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/18/2016"
	ms.author="davidmu"/>

# Verwalten von virtuellen Azure-Computern mit Resource Manager und PowerShell

## Installieren von Azure PowerShell
 
Informationen dazu, wie Sie die aktuelle Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich am Azure-Konto anmelden, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Festlegen von Variablen

Alle Befehle in diesem Artikel benötigen den Namen der Ressourcengruppe, in der sich der virtuelle Computer befindet, und den Name des zu verwaltenden virtuellen Computers. Ersetzen Sie den Wert von **$rgName** durch den Namen der Ressourcengruppe, die den virtuellen Computer enthält. Ersetzen Sie den Wert von **$vmName** durch den Namen des virtuellen Computers. Erstellen Sie die Variablen.

        $rgName = "resource group name"
        $vmName = "VM name"

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

        $rgName = "resource group name"
        $vmName = "VM name"
        Restart-AzureRmVM -ResourceGroupName $rgName -Name $vmName

Folgendes sollte angezeigt werden:

        RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
        ---------  -------------------  ----------  ------------
                                  True          OK  OK

## Löschen eines virtuellen Computers

Löschen Sie den virtuellen Computer.

        $rgName = "resource group name"
        $vmName = "VM name"
	    Remove-AzureRmVM -ResourceGroupName $rgName –Name $vmName

> [AZURE.NOTE] Mit dem Parameter **- Force** können Sie die Bestätigungsaufforderung überspringen.

Wenn Sie den Parameter "-Force" nicht verwendet haben, werden Sie aufgefordert, den Vorgang zu bestätigen:

	    Virtual machine removal operation
	    This cmdlet will remove the specified virtual machine. Do you want to continue?
	    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Folgendes sollte angezeigt werden:

        RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
        ---------  -------------------  ----------  ------------
                                  True          OK  OK

## Aktualisieren eines virtuellen Computers

Dieses Beispiel zeigt, wie Sie die Größe des virtuellen Computers aktualisieren.
        
        $vmSize = "Standard_A1"
        $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
        $vm.HardwareProfile.vmSize = $vmSize
        Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
    
    See [Sizes for virtual machines in Azure](virtual-machines-windows-sizes.md) for a list of available sizes for a virtual machine.

Folgendes sollte angezeigt werden:

        RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
        ---------  -------------------  ----------  ------------
                                  True          OK  OK

## Nächste Schritte

Falls bei der Bereitstellung Probleme aufgetreten sind, finden Sie womöglich Hilfe unter [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md).

<!---HONumber=AcomDC_0420_2016-->