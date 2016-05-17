<properties
	pageTitle="Verwalten von VMs in einer VM-Skalierungsgruppe | Microsoft Azure"
	description="Verwalten virtueller Computer in einer VM-Skalierungsgruppe mit Azure PowerShell."
	services="virtual-machine-scale-sets"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machine-scale-sets"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/26/2016"
	ms.author="davidmu"/>

# Verwalten virtueller Computer in einer VM-Skalierungsgruppe

Verwalten Sie mit den in diesem Artikel beschriebenen Aufgaben VM-Ressourcen in Ihrer VM-Skalierungsgruppe.

Alle Aufgaben im Zusammenhang mit dem Verwalten eines virtuellen Computers in einer Skalierungsgruppe erfordern, dass Sie die Instanz-ID des Computers kennen, den Sie verwalten möchten. Sie können mit dem [Azure-Ressourcen-Explorer](https://resources.azure.com) die Instanz-ID eines virtuellen Computers in einer Skalierungsgruppe finden. Sie können mit dem Ressourcen-Explorer auch den Status der Aufgaben überprüfen, die Sie fertig stellen.

Informationen dazu, wie Sie die aktuelle Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich beim Azure-Konto anmelden, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Anzeigen von Informationen zu einer VM-Skalierungsgruppe

Sie können allgemeine Informationen über eine Skalierungsgruppe abrufen, was auch als Instanzansicht bezeichnet wird. Oder Sie können spezifischere Informationen abrufen, z.B. Informationen zu den Ressourcen in der Gruppe.

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, und *scale set name* durch den Namen der VM-Skalierungsgruppe, und führen Sie dann den Befehl aus:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Folgendes sollte angezeigt werden:

    Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
    UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
    VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
    ProvisioningState     : Succeeded
    OverProvision         :
    Id                    : /subscriptions/{subscription-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                  : myvmss1
    Type                  : Microsoft.Compute/virtualMachineScaleSets
    Location              : centralus
    Tags                  :

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *scale set name* durch den Namen der VM-Skalierungsgruppe und *#* durch den Instanzbezeichner der VM, über die Sie Informationen abrufen möchten, und führen Sie dann den Befehl aus:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
        
Folgendes sollte angezeigt werden:

    InstanceId         : 1
    Sku                : Microsoft.Azure.Management.Compute.Models.Sku
    LatestModelApplied : True
    InstanceView       :
    HardwareProfile    :
    StorageProfile     : Microsoft.Azure.Management.Compute.Models.StorageProfile
    OsProfile          : Microsoft.Azure.Management.Compute.Models.OSProfile
    NetworkProfile     : Microsoft.Azure.Management.Compute.Models.NetworkProfile
    DiagnosticsProfile :
    AvailabilitySet    :
    ProvisioningState  : Succeeded
    LicenseType        :
    Plan               :
    Resources          :
    Id                 : /subscriptions/{subscription-id}/resourceGroups/myrg1/providers/Microsoft.
                         Compute/virtualMachineScaleSets/myvmss1/virtualMachines/1
    Name               : myvmss1_1
    Type               : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location           : centralus
    Tags               :
        
## Starten eines virtuellen Computers in einer Skalierungsgruppe

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *scale set name* durch den Namen der Skalierungsgruppe und *#* durch den Bezeichner der VM, die Sie starten möchten, und führen Sie dann den Befehl aus:

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Im Ressourcen-Explorer sehen wir, dass der Status der Instanz **running** ist:

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

Sie können alle virtuellen Computer in der Gruppe starten, indem Sie nicht den -InstanceId-Parameter verwenden.
    
## Stoppen eines virtuellen Computers in einer Skalierungsgruppe

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *scale set name* durch den Namen der Skalierungsgruppe und *#* durch den Bezeichner der VM, die Sie beenden möchten, und führen Sie dann den Befehl aus:

	Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Im Ressourcen-Explorer sehen wir, dass der Status der Instanz **deallocated** ist:

	"statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]
    
Um einen virtuellen Computer zu beenden und dessen Zuordnung nicht aufzuheben, verwenden Sie den -StayProvisioned-Parameter. Sie können alle virtuellen Computer in der Gruppe beenden, indem Sie nicht den -InstanceId-Parameter verwenden.
    
## Neustarten eines virtuellen Computers in einer Skalierungsgruppe

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *scale set name* durch den Namen der Skalierungsgruppe und *#* durch den Bezeichner der VM, die Sie neu starten möchten, und führen Sie dann den Befehl aus:

	Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #
    
Sie können alle virtuellen Computer in der Gruppe neu starten, indem Sie nicht den -InstanceId-Parameter verwenden.

## Entfernen eines virtuellen Computers aus einer Skalierungsgruppe

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *scale set name* durch den Namen der Skalierungsgruppe und *#* durch den Bezeichner der VM, die Sie aus der Skalierungsgruppe entfernen möchten, und führen Sie dann den Befehl aus:

	Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

Sie können die gesamte VM-Skalierungsgruppe entfernen, indem Sie nicht den -InstanceId-Parameter verwenden.

<!---HONumber=AcomDC_0504_2016-->