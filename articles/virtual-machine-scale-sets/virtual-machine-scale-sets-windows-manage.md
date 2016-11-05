---
title: Verwalten von VMs in einer VM-Skalierungsgruppe | Microsoft Docs
description: Verwalten virtueller Computer in einer VM-Skalierungsgruppe mit Azure PowerShell.
services: virtual-machine-scale-sets
documentationcenter: ''
author: davidmu1
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2016
ms.author: davidmu

---
# Verwalten virtueller Computer in einer VM-Skalierungsgruppe
Verwalten Sie mit den in diesem Artikel beschriebenen Aufgaben VM-Ressourcen in Ihrer VM-Skalierungsgruppe.

Alle Aufgaben im Zusammenhang mit dem Verwalten eines virtuellen Computers in einer Skalierungsgruppe erfordern, dass Sie die Instanz-ID des Computers kennen, den Sie verwalten möchten. Sie können mit dem [Azure-Ressourcen-Explorer](https://resources.azure.com) die Instanz-ID eines virtuellen Computers in einer Skalierungsgruppe finden. Sie können mit dem Ressourcen-Explorer auch den Status der Aufgaben überprüfen, die Sie fertig stellen.

Informationen dazu, wie Sie die aktuelle Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich beim Azure-Konto anmelden, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Anzeigen von Informationen zu einer VM-Skalierungsgruppe
Sie können allgemeine Informationen über eine Skalierungsgruppe abrufen, was auch als Instanzansicht bezeichnet wird. Oder Sie können spezifischere Informationen abrufen, z.B. Informationen zu den Ressourcen in der Gruppe.

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, und *scale set name* durch den Namen der VM-Skalierungsgruppe, und führen Sie dann den Befehl aus:

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

Folgendes sollte angezeigt werden:

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded

Ersetzen Sie in diesem Befehl *resource group name* durch den Namen der Ressourcengruppe, die die VM-Skalierungsgruppe enthält, *scale set name* durch den Namen der VM-Skalierungsgruppe und *#* durch den Instanzbezeichner der VM, über die Sie Informationen abrufen möchten, und führen Sie dann den Befehl aus:

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

Folgendes sollte angezeigt werden:

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded

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

<!---HONumber=AcomDC_0720_2016-->