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
	ms.date="03/22/2016"
	ms.author="davidmu"/>

# Erstellen einer Windows-VM-Skalierungsgruppe mithilfe von Azure PowerShell

Diese Schritte folgen einem lückenfüllenden Ansatz zur Erstellung einer Azure-VM-Skalierungsgruppe. In diesem Artikel kommen Variablen zum Einsatz, für die Sie Werte angeben müssen. Ersetzen Sie alles innerhalb der Anführungszeichen durch Werte, die für Ihr Abonnement und Ihre Anwendung sinnvoll sind.

## Schritt 1: Installieren von Azure PowerShell

[AZURE.INCLUDE [powershell-preview](../../includes/powershell-preview-inline-include.md)]

## Schritt 2: Festlegen des Abonnements

1. Starten Sie eine PowerShell-Eingabeaufforderung.

2. Melden Sie sich in Ihrem Konto an:

        Login-AzureRmAccount

3. Beziehen Sie Ihr Abonnement:

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName

4. Legen Sie das Abonnement fest, das Sie als aktuelles Abonnement verwenden möchten:

        $subscr = "subscription name"
        Select-AzureSubscription -SubscriptionName $subscr –Current


## Schritt 2: Erstellen von Ressourcen

Erstellen Sie die Ressourcen, die für die neue Skalierungsgruppe virtueller Computer benötigt wird.

### Ressourcengruppe

Eine VM-Skalierungsgruppe muss in einer Ressourcengruppe enthalten sein.

1.  Führen Sie den folgenden Befehl aus, um eine Liste der verfügbaren Standorte und Dienste abzurufen, die unterstützt werden:

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

    Wählen Sie den für Sie am besten geeigneten Standort aus, und ersetzen Sie den Text in Anführungszeichen durch diesen Standortnamen:

        $locName = "location name from the list, such as Central US"

4. Ersetzen Sie den Text in Anführungszeichen durch den Namen, den Sie für die neue Ressourcengruppe verwenden möchten, und erstellen Sie sie dann an dem zuvor ausgewählten Standort:

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName

    Die Ausgabe sollte folgendermaßen aussehen:

        ResourceGroupName : myrg1
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/########-####-####-####-############/resourceGroups/myrg1

### Speicherkonto

Für die in einer Skalierungsgruppe erstellten virtuellen Computer ist ein Speicherkonto zum Speichern der zugehörigen Datenträger erforderlich.

1. Ersetzen Sie den Text in Anführungszeichen durch den Namen, den Sie für das Speicherkonto verwenden möchten, und testen Sie dann, ob er eindeutig ist:

        $saName = "storage account name"
        Test-AzureName -Storage $saName

    Ist die Antwort **False**, ist der vorgeschlagene Name eindeutig.

2. Ersetzen Sie den Text in Anführungszeichen durch den Typ des Speicherkontos, und erstellen Sie dann das Konto mit dem zuvor festgelegten Namen und Speicherort. Mögliche Werte: Standard\_LRS, Standard\_GRS, Standard\_RAGRS oder Premium\_LRS:

        $saType = "storage account type"
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

1. Ersetzen Sie den Text in Anführungszeichen durch den Namen, den Sie für das Subnetz im virtuellen Netzwerk verwenden möchten, und erstellen Sie die Konfiguration:

        $subName = "subnet name"
        $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subName -AddressPrefix 10.0.0.0/24

2. Ersetzen Sie den Text in Anführungszeichen durch den Namen, den Sie für das virtuelle Netzwerk verwenden möchten, und erstellen Sie es dann unter Verwendung der zuvor festgelegten Informationen und Ressourcen:

        $netName="virtual network name"
        $vnet=New-AzureRmVirtualNetwork -Name $netName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### Öffentliche IP-Adresse

Bevor eine Netzwerkschnittstelle erstellt werden kann, müssen Sie eine öffentliche IP-Adresse erstellen.

1. Ersetzen Sie den Text in Anführungszeichen durch die Domänennamenbezeichnung, die Sie mit der öffentlichen IP-Adresse verwenden möchten, und testen Sie dann, ob der Name eindeutig ist. Die Bezeichnung darf nur Buchstaben, Zahlen und Bindestriche enthalten. Das letzte Zeichen muss jeweils ein Buchstabe oder eine Zahl sein:

        $domName = "domain name label"
        Test-AzureRmDnsAvailability -DomainQualifiedName $domName -Location $locName

    Ist die Antwort **False**, ist der vorgeschlagene Name eindeutig.

2. Ersetzen Sie den Text in Anführungszeichen durch den Namen, den Sie für die öffentliche IP-Adresse verwenden möchten, und erstellen Sie sie:

        $pipName = "public ip address name"
        $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic -DomainNameLabel $domName

### Netzwerkschnittstelle

Sie verfügen jetzt über die öffentliche IP-Adresse und können die Netzwerkschnittstelle erstellen.

1. Ersetzen Sie den Text in Anführungszeichen durch den Namen, den Sie für die Netzwerkschnittstelle verwenden möchten, und erstellen Sie sie dann unter Verwendung der zuvor erstellten Ressourcen:

        $nicName = "network interface name"
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id


### Erstellen der VM-Skalierungsgruppe

Sie verfügen nun über alle Ressourcen, die Sie benötigen, und können die Skalierungsgruppe erstellen.

1. Ersetzen Sie den Text in Anführungszeichen durch den Namen, den Sie für die IP-Konfiguration verwenden möchten, und erstellen Sie sie:

        $ipName = "IP configuration name"
        $ipConfig = New-AzureRmVmssIpConfig -Name $ipName -LoadBalancerBackendAddressPoolsId $null -SubnetId $vnet.Subnets[0].Id

2. Ersetzen Sie den Text in Anführungszeichen durch den Namen, den Sie für die Skalierungsgruppenkonfiguration verwenden möchten, und erstellen Sie sie: In diesem Schritt wird auch die Größe der virtuellen Computer in der Gruppe festgelegt (als „SkuName“ bezeichnet) . Suchen Sie unter [Größen für virtuelle Computer](..\virtual-machines\virtual-machines-windows-sizes.md) die Größe, die Ihre Anforderungen am besten erfüllt. In diesem Beispiel wird die Verwendung von „Standard\_A0“ empfohlen:

        $vmssName = "Scale set configuration name"
        $vmss = New-AzureRmVmssConfig -Location $locName -SkuCapacity 3 -SkuName "Standard_A0"
        Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmss -Name $vmssName -Primary $true -IPConfiguration $ipConfig

    Die Ausgabe sollte folgendermaßen aussehen:

        Sku                   : {
                                  "name": "Standard_A0",
                                  "tier": null,
                                  "capacity": 3
        						}
        UpgradePolicy         : {
                                  "mode": "automatic"
                                }
        VirtualMachineProfile : {
                                  "osProfile": null,
                                  "storageProfile": null,
                                  "networkProfile": {
                                    "networkInterfaceConfigurations": [
                                      {
                                        "name": "myniccfg1",
                                        "properties.primary": true,
                                        "properties.ipConfigurations": [
                                          {
                                            "name": "myipconfig1",
                                            "properties.subnet": {
                                              "id": "/subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1"
                                            },
                                            "properties.loadBalancerBackendAddressPools": [],
                                            "properties.loadBalancerInboundNatPools": [],
                                            "id": null
                                          }
                                        ],
                                        "id": null
                                      }
                                    ]
                                  },
                                  "extensionProfile": {
                                    "extensions": null
                                  }
                                }
        ProvisioningState     :
        Id                    :
        Name                  :
        Type                  :
        Location              : Central US
        Tags.Count            : 0
        Tags                  :

3. Ersetzen Sie den Text in Anführungszeichen für das Computernamenpräfix, das Sie verwenden möchten, für den Namen des Administratorkontos auf den virtuellen Computern und für das Kontokennwort. Erstellen Sie anschließend das Betriebssystemprofil:

        $computerName = "computer name prefix"
        $adminName = "administrator account name"
        $adminPassword = "password for administrator accounts"
        Set-AzureRmVmssOsProfile -VirtualMachineScaleSet $vmss -ComputerNamePrefix $computerName -AdminUsername $adminName -AdminPassword $adminPassword

    Im Abschnitt „OsProfile“ wird etwa Folgendes angezeigt:

        "osProfile": {
          "computerNamePrefix": "myvmss1",
          "adminUsername": "########",
          "adminPassword": "########",
          "customData": null,
          "windowsConfiguration": null,
          "linuxConfiguration": null,
          "secrets": null
        },

4. Ersetzen Sie den Text in Anführungszeichen durch den Namen, den Sie für das Speicherprofil, die Imageinformationen und den Speicherpfad für die Datenträger der virtuellen Computer verwenden möchten, und erstellen Sie dann das Profil. Weitere Informationen finden Sie unter [Navigieren zwischen und Auswählen von Images virtueller Azure-Computer mit Windows PowerShell und der Azure-Befehlszeilenschnittstelle](..\virtual-machines\virtual-machines-windows-cli-ps-findimage.md).

        $storeProfile = "storage profile name"
        $imagePublisher = "image publisher name, such as MicrosoftWindowsServer"
        $imageOffer = "offer from publisher, such as WindowsServer"
        $imageSku = "sku of image, such as 2012-R2-Datacenter"
        $vhdContainer = "URI of storage container"
        Set-AzureRmVmssStorageProfile -VirtualMachineScaleSet $vmss -ImageReferencePublisher $imagePublisher -ImageReferenceOffer $imageOffer -ImageReferenceSku $imageSku -ImageReferenceVersion "latest" -Name $storeProfile -VhdContainer $vhdContainer -OsDiskCreateOption "FromImage" -OsDiskCaching "None"

    Die Ausgabe im Abschnitt „StorageProfile“ sollte folgendermaßen aussehen:

        "storageProfile": {
          "imageReference": {
            "publisher": "MicrosoftWindowsServer",
            "offer": "WindowsServer",
            "sku": "2012-R2-Datacenter",
            "version": "latest"
          },
          "osDisk": {
            "name": "mystore1",
            "caching": "None",
            "createOption": "FromImage",
            "osType": null,
            "image": null,
            "vhdContainers": {
              "http://myst1.blob.core.windows.net/vhds"
            }
          }
        },

5. Ersetzen Sie den Text in Anführungszeichen durch den Namen der VM-Skalierungsgruppe, und erstellen Sie sie:

        $vmssName = "scale set name"
        New-AzureRmVmss -ResourceGroupName $rgName -Name $vmssName -VirtualMachineScaleSet $vmss

    Daraufhin sollte etwa folgende Ausgabe angezeigt werden, die darauf hinweist, dass die Bereitstellung erfolgreich war:

        ProvisioningState     : Succeeded
        Id                    : /subscriptions/########-####-####-####-############/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
        Name                  : myvmss1
        Type                  : Microsoft.Compute/virtualMachineScaleSets
        Location              : centralus
        Tags.Count            : 0
        Tags                  :

## Schritt 3: Untersuchen von Ressourcen

Untersuchen Sie die eben erstellte VM-Skalierungsgruppe mithilfe der folgenden Ressourcen:

- Azure-Portal: Eine begrenzte Menge an Informationen steht im Portal zur Verfügung.
- [Azure-Ressourcen-Explorer](https://resources.azure.com/): Dies ist das Tool, das zum Untersuchen des aktuellen Zustands Ihrer Skalierungsgruppe am besten geeignet ist.
- Azure PowerShell: Verwenden Sie den folgenden Befehl, um Informationen zu erhalten:

        Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

## Nächste Schritte

1. Weitere Informationen finden Sie unter [Virtual Machine Scale Sets Overview](virtual-machine-scale-sets-overview.md) (Übersicht über VM-Skalierungsgruppen).

2. Ziehen Sie die automatische Skalierung Ihrer Skalierungsgruppe in Betracht. Lesen Sie dazu die Informationen unter [Automatic scaling and virtual machine scale sets](virtual-machine-scale-sets-autoscale-overview.md) (Automatische Skalierung und VM-Skalierungsgruppen).

<!---HONumber=AcomDC_0427_2016-->