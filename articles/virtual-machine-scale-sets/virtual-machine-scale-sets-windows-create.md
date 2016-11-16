---
title: Erstellen einer VM-Skalierungsgruppe mit PowerShell | Microsoft Docs
description: Erstellen einer VM-Skalierungsgruppe mit PowerShell
services: virtual-machine-scale-sets
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7bb03323-8bcc-4ee4-9a3e-144ca6d644e2
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/18/2016
ms.author: davidmu
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 6d70338ebf918a3f9178a4f633dd46a607d72b1c


---
# <a name="create-a-windows-virtual-machine-scale-set-using-azure-powershell"></a>Erstellen einer Windows-VM-Skalierungsgruppe mithilfe von Azure PowerShell
Diese Schritte folgen einem lückenfüllenden Ansatz zur Erstellung einer Azure-VM-Skalierungsgruppe. Weitere Informationen über Skalierungsgruppen finden Sie unter [Übersicht über VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md) .

Die Ausführung der Schritte im Artikel dauert ungefähr 30 Minuten.

## <a name="step-1-install-azure-powershell"></a>Schritt 1: Installieren von Azure PowerShell
Unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md) erfahren Sie, wie Sie die neueste Version von Azure PowerShell installieren, Ihr Abonnement auswählen und sich bei Ihrem Konto anmelden.

## <a name="step-2-create-resources"></a>Schritt 2: Erstellen von Ressourcen
Erstellen Sie die Ressourcen, die für die neue Skalierungsgruppe benötigt wird.

### <a name="resource-group"></a>Ressourcengruppe
Eine VM-Skalierungsgruppe muss in einer Ressourcengruppe enthalten sein.

1. Rufen Sie eine Liste mit den verfügbaren Standorten ab, an denen Ressourcen erstellt werden können:
   
        Get-AzureLocation | Sort Name | Select Name
2. Wählen Sie einen Standort aus, der für Sie am besten geeignet ist, ersetzen Sie den Wert von **$locName** durch diesen Standortnamen, und erstellen Sie dann die Variable:
   
        $locName = "location name from the list, such as Central US"
3. Ersetzen Sie den Wert von **$rgName** durch den Namen, den Sie für die neue Ressourcengruppe verwenden möchten, und erstellen Sie dann die Variable: 
   
        $rgName = "resource group name"
4. Erstellen Sie die Ressourcengruppe:
   
        New-AzureRmResourceGroup -Name $rgName -Location $locName
   
    Die Ausgabe sollte in etwa wie das folgende Beispiel aussehen:
   
        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### <a name="storage-account"></a>Speicherkonto
Ein Speicherkonto wird von einem virtuellen Computer verwendet, um den Betriebssystemdatenträger und die Diagnosedaten für die Skalierung zu speichern. Nach Möglichkeit sollte in einer Skalierungsgruppe für jeden virtuellen Computer ein Speicherkonto erstellt werden. Sollte dies nicht möglich sein, planen Sie mit maximal 20 virtuellen Computern pro Speicherkonto. In dem Beispiel in diesem Artikel werden für drei virtuelle Computer drei Speicherkonten erstellt.

1. Ersetzen Sie den Wert von **$saName** durch den Namen des Speicherkontos. Testen Sie den Namen auf Eindeutigkeit. 
   
        $saName = "storage account name"
        Get-AzureRmStorageAccountNameAvailability $saName
   
    Lautet die Antwort **True**, ist der vorgeschlagene Name eindeutig.
2. Ersetzen Sie den Wert von **$saType** durch den Typ des Speicherkontos, und erstellen Sie dann die Variable:  
   
        $saType = "storage account type"
   
    Mögliche Werte: Standard_LRS, Standard_GRS, Standard_RAGRS oder Premium_LRS.
3. Erstellen Sie das Konto:
   
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName
   
    Die Ausgabe sollte in etwa wie das folgende Beispiel aussehen:
   
        ResourceGroupName   : myrg1
        StorageAccountName  : myst1
        Id                  : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft
                              .Storage/storageAccounts/myst1
        Location            : centralus
        AccountType         : StandardLRS
        CreationTime        : 3/15/2016 4:51:52 PM
        CustomDomain        :
        LastGeoFailoverTime :
        PrimaryEndpoints    : Microsoft.Azure.Management.Storage.Models.Endpoints
        PrimaryLocation     : centralus
        ProvisioningState   : Succeeded
        SecondaryEndpoints  :
        SecondaryLocation   :
        StatusOfPrimary     : Available
        StatusOfSecondary   :
        Tags                : {}
        Context             : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext
4. Wiederholen Sie die Schritte 1 bis 4, um drei Speicherkonten (beispielsweise „myst1“, „myst2“ und „myst3“) zu erstellen.

### <a name="virtual-network"></a>virtuelles Netzwerk
Für die virtuellen Computer in der Skalierungsgruppe ist ein virtuelles Netzwerk erforderlich.

1. Ersetzen Sie den Wert von **$subnetName** durch den Namen, den Sie für das Subnetz im virtuellen Netzwerk verwenden möchten, und erstellen Sie dann die Variable: 
   
        $subnetName = "subnet name"
2. Erstellen Sie die Subnetzkonfiguration:
   
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   
    Das Adresspräfix kann in Ihrem virtuellen Netzwerk anders sein.
3. Ersetzen Sie den Wert von **$netName** durch den Namen, den Sie für das virtuelle Netzwerk verwenden möchten, und erstellen Sie dann die Variable: 
   
        $netName = "virtual network name"
4. Erstellen Sie das virtuelle Netzwerk:
   
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="configuration-of-the-scale-set"></a>Konfiguration der Skalierungsgruppe
Sie haben alle Ressourcen, die Sie für die Skalierungsgruppenkonfiguration benötigen, also können Sie sie erstellen.  

1. Ersetzen Sie den Wert von **$ipName** durch den Namen, den Sie für die IP-Konfiguration verwenden möchten, und erstellen Sie dann die Variable: 
   
        $ipName = "IP configuration name"
2. Erstellen Sie die IP-Konfiguration:
   
        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id
3. Ersetzen Sie den Wert von **$vmssConfig** durch den Namen, den Sie für die Skalierungsgruppenkonfiguration verwenden möchten, und erstellen Sie dann die Variable:   
   
        $vmssConfig = "Scale set configuration name"
4. Erstellen Sie die Konfiguration für die Skaliserungsgruppe:
   
        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
   
    In diesem Beispiel wird veranschaulicht, wie eine Skalierungsgruppe mit drei virtuellen Computern erstellt wird. Weitere Informationen zur Kapazität von Skalierungsgruppen finden Sie unter [Übersicht über VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md) . In diesem Schritt wird auch die Größe der virtuellen Computer in der Gruppe festgelegt (wird als „SkuName“ bezeichnet). Ermitteln Sie unter [Größen für virtuelle Computer](../virtual-machines/virtual-machines-windows-sizes.md) die passende Größe für Ihre Anforderungen.
5. Fügen Sie die Konfiguration der Netzwerkschnittstelle der Skalierungsgruppenkonfiguration hinzu:
   
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
   
    Die Ausgabe sollte in etwa wie das folgende Beispiel aussehen:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     :
        OverProvision         :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags                  :

#### <a name="operating-system-profile"></a>Betriebssystemprofil
1. Ersetzen Sie den Wert von **$computerName** durch das Computernamenpräfix, das Sie verwenden möchten, und erstellen Sie dann die Variable: 
   
        $computerName = "computer name prefix"
2. Ersetzen Sie den Wert von **$adminName** durch den Namen des Administratorkontos auf den virtuellen Computern, und erstellen Sie dann die Variable:
   
        $adminName = "administrator account name"
3. Ersetzen Sie den Wert von **$adminPassword** durch das Kontokennwort, und erstellen Sie dann die Variable:
   
        $adminPassword = "password for administrator accounts"
4. Erstellen Sie das Betriebssystemprofil:
   
        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### <a name="storage-profile"></a>Speicherprofil
1. Ersetzen Sie den Wert von **$storageProfile** durch den Namen, den Sie für das Speicherprofil verwenden möchten, und erstellen Sie dann die Variable:  
   
        $storageProfile = "storage profile name"
2. Erstellen Sie die Variablen, die das zu verwendende Image definieren:  
   
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
   
    Weitere Informationen zu anderen zu verwendenden Images finden Sie unter [Navigate and select Azure virtual machine images with Windows PowerShell and the Azure CLI](../virtual-machines/virtual-machines-windows-cli-ps-findimage.md) (Navigieren zwischen und Auswählen von Images virtueller Azure-Computer mit Windows PowerShell und der Azure-Befehlszeilenschnittstelle).
3. Ersetzen Sie den Wert von **$vhdContainers** durch eine Liste mit den Pfaden, an denen die virtuellen Festplatten gespeichert sind (beispielsweise „https://mystorage.blob.core.windows.net/vhds), und erstellen Sie dann die Variable:
   
        $vhdContainers = @("https://myst1.blob.core.windows.net/vhds","https://myst2.blob.core.windows.net/vhds","https://myst3.blob.core.windows.net/vhds")
4. Erstellen Sie das Speicherprofil:
   
        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storageProfile -VhdContainer $vhdContainers -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### <a name="virtual-machine-scale-set"></a>VM-Skalierungsgruppe
Jetzt können Sie die Skalierungsgruppe erstellen.

1. Ersetzen Sie den Wert von **$vmssName** durch den Namen der VM-Skalierungsgruppe, und erstellen Sie dann die Variable:
   
        $vmssName = "scale set name"
2. Erstellen Sie die Skalierungsgruppe:
   
        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss
   
    Daraufhin sollte etwa folgendes Beispiel angezeigt werden, das auf eine erfolgreiche Bereitstellung hinweist:
   
        Sku                   : Microsoft.Azure.Management.Compute.Models.Sku
        UpgradePolicy         : Microsoft.Azure.Management.Compute.Models.UpgradePolicy
        VirtualMachineProfile : Microsoft.Azure.Management.Compute.Models.VirtualMachineScaleSetVMProfile
        ProvisioningState     : Updating
        OverProvision         :
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microso
                                ft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags                  :

## <a name="step-3-explore-resources"></a>Schritt 3: Untersuchen von Ressourcen
Untersuchen Sie die erstellte VM-Skalierungsgruppe mithilfe der folgenden Ressourcen:

* Azure-Portal: Eine begrenzte Menge an Informationen steht im Portal zur Verfügung.
* [Azure-Ressourcen-Explorer](https://resources.azure.com/): Dieses Tool eignet sich perfekt zum Untersuchen des aktuellen Zustands Ihrer Skalierungsgruppe.
* Azure PowerShell: Verwenden Sie diesen Befehl, um Informationen zu erhalten:
  
        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
  
        Or 
  
        Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

## <a name="next-steps"></a>Nächste Schritte
* Verwalten Sie die Skalierungsgruppe, die Sie gerade erstellt haben, mithilfe der Informationen unter [Verwalten virtueller Computer in einer VM-Skalierungsgruppe](virtual-machine-scale-sets-windows-manage.md)
* Ziehen Sie die automatische Skalierung Ihrer Skalierungsgruppe in Betracht. Lesen Sie dazu die Informationen unter [Automatische Skalierung und Skalierungsgruppen für virtuelle Computer](virtual-machine-scale-sets-autoscale-overview.md).
* Informieren Sie sich unter [Vertikale automatische Skalierung mit VM-Skalierungsgruppen](virtual-machine-scale-sets-vertical-scale-reprovision.md)




<!--HONumber=Nov16_HO2-->


