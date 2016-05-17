<properties
	pageTitle="Erstellen einer VM-Skalierungsgruppe | Microsoft Azure"
	description="Erstellen einer VM-Skalierungsgruppe mit PowerShell"
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

# Erstellen einer Windows-VM-Skalierungsgruppe mithilfe von Azure PowerShell

Diese Schritte folgen einem lückenfüllenden Ansatz zur Erstellung einer Azure-VM-Skalierungsgruppe. Weitere Informationen über Skalierungsgruppen finden Sie unter [Übersicht über VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md).

Die Ausführung der Schritte im Artikel dauert ungefähr 30 Minuten.

## Schritt 1: Installieren von Azure PowerShell

Informationen dazu, wie Sie die aktuelle Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich beim Azure-Konto anmelden, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

## Schritt 2: Erstellen von Ressourcen

Erstellen Sie die Ressourcen, die für die neue Skalierungsgruppe virtueller Computer benötigt wird.

### Ressourcengruppe

Eine VM-Skalierungsgruppe muss in einer Ressourcengruppe enthalten sein.

1.  Rufen Sie eine Liste der verfügbaren Standorte und der Dienste ab, die unterstützt werden:

        Get-AzureLocation | Sort Name | Select Name, AvailableServices

    Die Ausgabe sollte folgendermaßen aussehen:

        Name                AvailableServices
        ----                -----------------
        Australia East      {Compute, Storage, PersistentVMRole, HighMemory}
        Australia Southeast {Compute, Storage, PersistentVMRole, HighMemory}
        Brazil South        {Compute, Storage, PersistentVMRole, HighMemory}
        Central India       {Compute, Storage, PersistentVMRole, HighMemory}
        Central US          {Compute, Storage, PersistentVMRole, HighMemory}
        East Asia           {Compute, Storage, PersistentVMRole, HighMemory}
        East US             {Compute, Storage, PersistentVMRole, HighMemory}
        East US 2           {Compute, Storage, PersistentVMRole, HighMemory}
        Japan East          {Compute, Storage, PersistentVMRole, HighMemory}
        Japan West          {Compute, Storage, PersistentVMRole, HighMemory}
        North Central US    {Compute, Storage, PersistentVMRole, HighMemory}
        North Europe        {Compute, Storage, PersistentVMRole, HighMemory}
        South Central US    {Compute, Storage, PersistentVMRole, HighMemory}
        South India         {Compute, Storage, PersistentVMRole, HighMemory}
        Southeast Asia      {Compute, Storage, PersistentVMRole, HighMemory}
        West Europe         {Compute, Storage, PersistentVMRole, HighMemory}
        West India          {Compute, Storage, PersistentVMRole, HighMemory}
        West US             {Compute, Storage, PersistentVMRole, HighMemory}

2. Wählen Sie einen Standort aus, der für Sie am besten geeignet ist, ersetzen Sie den Wert von **$locName** durch diesen Standortnamen, und erstellen Sie dann die Variable:

        $locName = "location name from the list, such as Central US"

3. Ersetzen Sie den Wert von **$rgName** durch den Namen, den Sie für die neue Ressourcengruppe verwenden möchten, und erstellen Sie dann die Variable:

        $rgName = "resource group name"
        
4. Erstellen Sie die Ressourcengruppe:
    
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Die Ausgabe sollte folgendermaßen aussehen:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### Speicherkonto

Für die in einer Skalierungsgruppe erstellten virtuellen Computer ist ein Speicherkonto zum Speichern der zugehörigen Datenträger erforderlich.

1. Ersetzen Sie den Wert von **saName** durch den Namen, den Sie für das Speicherkonto verwenden möchten, und erstellen Sie dann die Variable: 

        $saName = "storage account name"
        
2. Testen Sie, ob der ausgewählte Name eindeutig ist:
    
        Test-AzureName -Storage $saName

    Ist die Antwort **False**, ist der vorgeschlagene Name eindeutig.

3. Ersetzen Sie den Wert von **$saType** durch den Typ des Speicherkontos, und erstellen Sie dann die Variable:

        $saType = "storage account type"
        
    Mögliche Werte: Standard\_LRS, Standard\_GRS, Standard\_RAGRS oder Premium\_LRS.
        
4. Erstellen Sie das Konto:
    
        New-AzureRmStorageAccount -Name $saName -ResourceGroupName $rgName –Type $saType -Location $locName

    Die Ausgabe sollte folgendermaßen aussehen:

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

### Virtuelles Netzwerk

Für die virtuellen Computer in der Skalierungsgruppe ist ein virtuelles Netzwerk erforderlich.

1. Ersetzen Sie den Wert von **$subName** durch den Namen, den Sie für das Subnetz im virtuellen Netzwerk verwenden möchten, und erstellen Sie dann die Variable: 

        $subName = "subnet name"
        
2. Erstellen Sie die Subnetzkonfiguration:
    
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subName -AddressPrefix 10.0.0.0/24
        
    Das Adresspräfix kann in Ihrem virtuellen Netzwerk anders sein.

3. Ersetzen Sie den Wert von **$netName** durch den Namen, den Sie für das virtuelle Netzwerk verwenden möchten, und erstellen Sie dann die Variable:

        $netName = "virtual network name"
        
4. Erstellen Sie das virtuelle Netzwerk:
    
        $vnet = New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### Öffentliche IP-Adresse

Bevor eine Netzwerkschnittstelle erstellt werden kann, müssen Sie eine öffentliche IP-Adresse erstellen.

1. Ersetzen Sie den Wert von **$domName** durch die Domänennamenbezeichnung, die Sie mit der öffentlichen IP-Adresse verwenden möchten, und erstellen Sie dann die Variable:  

        $domName = "domain name label"
        
    Die Bezeichnung darf nur Buchstaben, Zahlen und Bindestriche enthalten. Das letzte Zeichen muss ein Buchstabe oder eine Zahl sein:
    
2. Testen Sie, ob der Name eindeutig ist:
    
        Test-AzureRmDnsAvailability -DomainQualifiedName $domName -Location $locName

    Ist die Antwort **True**, ist der vorgeschlagene Name eindeutig.

3. Ersetzen Sie den Wert von **$pipName** durch den Namen, den Sie für die öffentliche IP-Adresse verwenden möchten, und erstellen Sie dann die Variable.

        $pipName = "public ip address name"
        
4. Erstellen Sie die öffentliche IP-Adresse:
    
        $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic -DomainNameLabel $domName

### Netzwerkschnittstelle

Sie verfügen jetzt über die öffentliche IP-Adresse und können die Netzwerkschnittstelle erstellen.

1. Ersetzen Sie den Wert von **$nicName** durch den Namen, den Sie für die Netzwerkschnittstelle verwenden möchten, und erstellen Sie dann die Variable: 

        $nicName = "network interface name"
        
2. Erstellen Sie die Netzwerkschnittstelle:
    
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

### Konfiguration der Skalierungsgruppe

Sie haben alle Ressourcen, die Sie für die Skalierungsgruppenkonfiguration benötigen, also können Sie sie erstellen.

1. Ersetzen Sie den Wert von **$ipName** durch den Namen, den Sie für die IP-Konfiguration verwenden möchten, und erstellen Sie dann die Variable: 

        $ipName = "IP configuration name"
        
2. Erstellen Sie die IP-Konfiguration:

        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Ersetzen Sie den Wert von **$vmssConfig** durch den Namen, den Sie für die Skalierungsgruppenkonfiguration verwenden möchten, und erstellen Sie dann die Variable:

        $vmssConfig = "Scale set configuration name"
        
3. Erstellen Sie die Konfiguration für die Skaliserungsgruppe:

        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0" -UpgradePolicyMode "manual"
        
    In diesem Beispiel wird veranschaulicht, wie eine Skalierungsgruppe mit drei virtuellen Computern erstellt wird. Weitere Informationen über die Kapazität von Skalierungsgruppen finden Sie unter [Übersicht über VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md). In diesem Schritt wird auch die Größe der virtuellen Computer in der Gruppe festgelegt (wird als „SkuName“ bezeichnet). Suchen Sie unter [Größen für virtuelle Computer](..\virtual-machines\virtual-machines-windows-sizes.md) die Größe, die Ihre Anforderungen erfüllt.
    
4. Fügen Sie die Konfiguration der Netzwerkschnittstelle der Skalierungsgruppenkonfiguration hinzu:
        
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssConfig -Primary $true -IPConfiguration $ipConfig
        
    Die Ausgabe sollte folgendermaßen aussehen:

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

#### Betriebssystemprofil

1. Ersetzen Sie den Wert von **$computerName** durch das Computernamenpräfix, das Sie verwenden möchten, und erstellen Sie dann die Variable: 

        $computerName = "computer name prefix"
        
2. Ersetzen Sie den Wert von **$adminName** durch den Namen des Administratorkontos auf den virtuellen Computern, und erstellen Sie dann die Variable:

        $adminName = "administrator account name"
        
3. Ersetzen Sie den Wert von **$adminPassword** durch das Kontokennwort, und erstellen Sie dann die Variable:

        $adminPassword = "password for administrator accounts"
        
4. Erstellen Sie das Betriebssystemprofil:

        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

#### Speicherprofil

1. Ersetzen Sie den Wert von **$storageProfile** durch den Namen, den Sie für das Speicherprofil verwenden möchten, und erstellen Sie dann die Variable:  

        $storeProfile = "storage profile name"
        
2. Erstellen Sie die Variablen, die das zu verwendende Image definieren:
      
        $imagePublisher = "MicrosoftWindowsServer"
        $imageOffer = "WindowsServer"
        $imageSku = "2012-R2-Datacenter"
        
    Weitere Informationen zum zu verwendenden Image finden Sie unter [Navigieren zwischen und Auswählen von Images virtueller Azure-Computer mit Windows PowerShell und der Azure-Befehlszeilenschnittstelle](..\virtual-machines\virtual-machines-windows-cli-ps-findimage.md).
        
3. Ersetzen Sie den Wert von **$vhdContainer** durch den Pfad, in dem die virtuellen Festplatten gespeichert sind, z.B. „https://mystorage.blob.core.windows.net/vhds“, und erstellen Sie dann die Variable:
       
        $vhdContainer = "URI of storage container"
        
4. Erstellen Sie das Speicherprofil:

        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storeProfile -VhdContainer $vhdContainer -OsDiskCreateOption "FromImage" -OsDiskCaching "None"  

### VM-Skalierungsgruppe

Jetzt können Sie die Skalierungsgruppe erstellen.

1. Ersetzen Sie den Wert von **$vmssName** durch den Namen der VM-Skalierungsgruppe, und erstellen Sie dann die Variable:

        $vmssName = "scale set name"
        
2. Erstellen Sie die Skalierungsgruppe:

        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Daraufhin sollte etwa folgende Ausgabe angezeigt werden, die darauf hinweist, dass die Bereitstellung erfolgreich war:

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

## Schritt 3: Untersuchen von Ressourcen

Untersuchen Sie die eben erstellte VM-Skalierungsgruppe mithilfe der folgenden Ressourcen:

- Azure-Portal: Eine begrenzte Menge an Informationen steht im Portal zur Verfügung.
- [Azure-Ressourcen-Explorer](https://resources.azure.com/): Dies ist das Tool, das zum Untersuchen des aktuellen Zustands Ihrer Skalierungsgruppe am besten geeignet ist.
- Azure PowerShell: Verwenden Sie den folgenden Befehl, um Informationen zu erhalten:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

## Nächste Schritte

- Verwalten Sie die Skalierungsgruppe, die Sie gerade erstellt haben, mithilfe der Informationen unter [Verwalten virtueller Computer in einer VM-Skalierungsgruppe](virtual-machine-scale-sets-windows-manage.md).
- Ziehen Sie die automatische Skalierung Ihrer Skalierungsgruppe in Betracht. Lesen Sie dazu die Informationen unter [Automatic scaling and virtual machine scale sets](virtual-machine-scale-sets-autoscale-overview.md) (Automatische Skalierung und VM-Skalierungsgruppen).
- Erfahren Sie mehr über die vertikale Skalierung, indem Sie [Vertikale automatische Skalierung mit VM-Skalierungsgruppen](virtual-machine-scale-sets-vertical-scale-reprovision.md) lesen.

<!---HONumber=AcomDC_0504_2016-->