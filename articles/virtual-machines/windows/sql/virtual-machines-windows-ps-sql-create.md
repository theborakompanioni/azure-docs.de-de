---
title: Erstellen eines virtuellen Computers mit SQL Server in Azure PowerShell (Resource Manager) | Microsoft Docs
description: "Enthält Schritte und PowerShell-Skripts zum Erstellen eines virtuellen Azure-Computers über Images aus dem Katalog von virtuellen Computern mit SQL Server."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 98d50dd8-48ad-444f-9031-5378d8270d7b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/17/2017
ms.author: jroth
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 218d7dcf14ebbb28ac043e50e6f9248ecb942aaa
ms.lasthandoff: 03/25/2017


---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-resource-manager"></a>Bereitstellen eines virtuellen SQL Server-Computers mit Azure PowerShell (Resource Manager)
> [!div class="op_single_selector"]
> * [Portal](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
>
>

## <a name="overview"></a>Übersicht
In diesem Tutorial erfahren Sie, wie Sie einen virtuellen Azure-Computer im Rahmen des Bereitstellungsmodells **Azure Resource Manager** mithilfe von Azure PowerShell-Cmdlets erstellen. Zu diesem Zweck erstellen wir in diesem Tutorial einen einzelnen virtuellen Computer mit einem einzelnen Laufwerk. Als Grundlage dient dabei ein Image aus dem SQL-Katalog. Außerdem erstellen wir neue Anbieter für die Speicher-, Netzwerk- und Compute-Ressourcen, die von dem virtuellen Computer verwendet werden. Falls Sie bereits über Anbieter für diese Ressourcen verfügen, können Sie stattdessen diese Anbieter verwenden.

Die klassische Version dieses Themas finden Sie unter [Erstellen eines virtuellen Computers mit SQL Server in Azure PowerShell (klassisch)](../classic/ps-sql-create.md).

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Ein Azure-Konto und ein Azure-Abonnement, bevor Sie beginnen. Falls Sie diese benötigen, können Sie sich für eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)registrieren.
* [Azure PowerShell](/powershell/azureps-cmdlets-docs) (mindestens Version 1.4.0). Dieses Tutorial wurde für Version 1.5.0 geschrieben.
  * Geben Sie zum Abrufen Ihrer Version den Befehl **Get-Module Azure -ListAvailable**ein.

## <a name="configure-your-subscription"></a>Konfigurieren Ihres Abonnements
Öffnen Sie Windows PowerShell, und führen Sie das folgende Cmdlet aus, um den Zugriff auf Ihr Azure-Konto einzurichten. Geben Sie auf der angezeigten Anmeldeseite Ihre Anmeldeinformationen ein. Verwenden Sie die E-Mail-Adresse und das Kennwort für die Anmeldung beim Azure-Portal.

    Add-AzureRmAccount

Nach erfolgreicher Anmeldung werden auf dem Bildschirm einige Informationen angezeigt – unter anderem die Abonnement-ID, mit der Sie sich angemeldet haben. Hierbei handelt es sich um das Abonnement, unter dem die Ressourcen für dieses Tutorial erstellt werden, sofern Sie nicht zu einem anderen Abonnement wechseln. Wenn Sie über mehrere Abonnement-IDs verfügen, führen Sie das folgende Cmdlet aus, um eine Liste mit allen Ihren Abonnement-IDs zurückzugeben:

    Get-AzureRmSubscription

Wenn Sie zu einer anderen Abonnement-ID wechseln möchten, führen Sie das folgende Cmdlet mit der gewünschten Abonnement-ID aus:

    Select-AzureRmSubscription -SubscriptionId xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

## <a name="define-image-variables"></a>Definieren von Image-Variablen
Der Einfachheit halber und zum besseren Verständnis des fertigen Skripts aus diesem Tutorial definieren wir zunächst eine Reihe von Variablen. Die Parameterwerte können nach Belieben geändert werden. Dabei müssen allerdings die Benennungskonventionen hinsichtlich Namenslänge und Sonderzeichen eingehalten werden.

### <a name="location-and-resource-group"></a>Ort und Ressourcengruppe
Definieren Sie mithilfe zweier Variablen die Datenregion und die Ressourcengruppe, in der Sie die anderen Ressourcen für den virtuellen Computer erstellen.

Ändern Sie die Werte nach Bedarf, und führen Sie die folgenden Cmdlets aus, um die Variablen zu initialisieren:

    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"

### <a name="storage-properties"></a>Speichereigenschaften
Definieren Sie mithilfe der folgenden Variablen das Speicherkonto und die Art des Speichers für den virtuellen Computer.

Ändern Sie die Werte nach Bedarf, und führen Sie das folgende Cmdlet aus, um die Variablen zu initialisieren. In diesem Beispiel verwenden wir mit [Storage Premium](../../../storage/storage-premium-storage.md)die empfohlene Option für Produktionsworkloads. Ausführliche Informationen zu dieser Richtlinie sowie andere Empfehlungen finden Sie unter [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md).

    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

### <a name="network-properties"></a>Netzwerkeigenschaften
Definieren Sie mithilfe der folgenden Variablen die Netzwerkschnittstelle, die TCP/IP-Zuordnungsmethode, den Namen des virtuellen Netzwerks, den Namen des virtuellen Subnetzes, den IP-Adressbereich für das virtuelle Netzwerk, den IP-Adressbereich für das Subnetz und den Namen der öffentlichen Domäne für das Netzwerk des virtuellen Computers.

Ändern Sie die Werte nach Bedarf, und führen Sie das folgende Cmdlet aus, um die Variablen zu initialisieren.

    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $TCPIPAllocationMethod = "Dynamic"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $DomainName = "sqlvm1"

### <a name="virtual-machine-properties"></a>Eigenschaften für virtuelle Maschinen
Definieren Sie mithilfe der folgenden Variablen den Namen des virtuellen Computers, den Computernamen, die Größe des virtuellen Computers und den Namen des Betriebssystemdatenträgers für den virtuellen Computer.

Ändern Sie die Werte nach Bedarf, und führen Sie das folgende Cmdlet aus, um die Variablen zu initialisieren.

    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

### <a name="image-properties"></a>Image-Eigenschaften
Definieren Sie mithilfe folgender Variablen das Image für den virtuellen Computer. In diesem Beispiel wird das SQL Server 2016 Enterprise-Image verwendet.

Ändern Sie die Werte nach Bedarf, und führen Sie das folgende Cmdlet aus, um die Variablen zu initialisieren.

    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

Eine vollständige Liste mit SQL Server-Image-Angeboten können Sie mithilfe des Befehls „Get-AzureRmVMImageOffer“ abrufen:

    Get-AzureRmVMImageOffer -Location 'East US' -Publisher 'MicrosoftSQLServer'

Die für ein Angebot verfügbaren SKUs können mithilfe des Befehls „Get-AzureRmVMImageSku“ ermittelt werden. Der folgende Befehl zeigt alle verfügbaren SKUs für das Angebot **SQL2014SP1-WS2012R2** an.

    Get-AzureRmVMImageSku -Location 'East US' -Publisher 'MicrosoftSQLServer' -Offer 'SQL2014SP1-WS2012R2' | Select Skus

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Bei Verwendung des Resource Manager-Bereitstellungsmodells wird als erstes Objekt die Ressourcengruppe erstellt. Wir verwenden das Cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt759837.aspx), um eine Azure-Ressourcengruppe und ihre Ressourcen mit dem Ressourcengruppennamen und -ort zu erstellen, die durch die zuvor initialisierten Variablen definiert werden.

Führen Sie das folgende Cmdlet aus, um Ihre neue Ressourcengruppe zu erstellen:

    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

## <a name="create-a-storage-account"></a>Speicherkonto erstellen
Der virtuelle Computer benötigt Speicherressourcen für den Betriebssystemdatenträger sowie für die SQL Server-Daten und -Protokolldateien. Der Einfachheit halber erstellen wir für beides einen einzelnen Datenträger. Zusätzliche Datenträger können später mithilfe des Cmdlets [Add-Azure Disk](https://msdn.microsoft.com/library/azure/dn495252.aspx) angefügt werden, um Ihre SQL Server-Daten und -Protokolldateien auf dedizierten Datenträgern zu platzieren. Wir erstellen mithilfe des Cmdlets [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) ein Standardspeicherkonto in Ihrer neuen Ressourcengruppe mit dem Speicherkontonamen, Speichernamen und Speicherort, die durch die zuvor initialisierten Variablen definiert werden.

Führen Sie das folgende Cmdlet aus, um Ihr neues Speicherkonto zu erstellen:

    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

## <a name="create-network-resources"></a>Erstellen von Netzwerkressourcen
Der virtuelle Computer benötigt für die Netzwerkkonnektivität eine Reihe von Netzwerkressourcen.

* Jeder virtuelle Computer benötigt ein virtuelles Netzwerk.
* Ein virtuelles Netzwerk muss über mindestens ein definiertes Subnetz verfügen.
* Eine Netzwerkschnittstelle muss mit einer öffentlichen oder privaten IP-Adresse definiert werden.

### <a name="create-a-virtual-network-subnet-configuration"></a>Erstellen einer Subnetzkonfiguration für ein virtuelles Netzwerk
Wir erstellen zunächst eine Subnetzkonfiguration für unser virtuelles Netzwerk. Im Rahmen unseres Tutorials erstellen wir mithilfe des Cmdlets [New-AzureRmVirtualNetworkSubnetConfig](https://msdn.microsoft.com/library/mt619412.aspx) ein Standardsubnetz. Für die Subnetzkonfiguration unseres virtuellen Netzwerks verwenden wir den Subnetznamen und das Adresspräfix, die durch die zuvor initialisierten Variablen definiert werden.

> [!NOTE]
> Mit dem Cmdlet können auch weitere Eigenschaften der Subnetzkonfiguration für das virtuelle Netzwerk definiert werden. Darauf wird in diesem Tutorial jedoch nicht weiter eingegangen.
>
>

Führen Sie das folgende Cmdlet aus, um die Konfiguration des virtuellen Subnetzes zu erstellen:

    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Als Nächstes erstellen wir mithilfe des Cmdlets [New-AzureRmVirtualNetwork](https://msdn.microsoft.com/library/mt603657.aspx) unser virtuelles Netzwerk. Das virtuelle Netzwerk erstellen wir in Ihrer neuen Ressourcengruppe mit dem Namen, Speicherort und Adresspräfix, die durch die zuvor initialisierten Variablen definiert werden, sowie mit der im vorherigen Schritt definierten Subnetzkonfiguration.

Führen Sie das folgende Cmdlet aus, um Ihr virtuelles Netzwerk zu erstellen:

    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig

### <a name="create-the-public-ip-address"></a>Erstellen der öffentlichen IP-Adresse
Nachdem wir unser virtuelles Netzwerk definiert haben, müssen wir nun eine IP-Adresse für die Verbindung mit dem virtuellen Computer konfigurieren. In diesem Tutorial erstellen wir eine öffentliche IP-Adresse mit dynamischer IP-Adressierung, um Internetverbindungen zu unterstützen. Wir verwenden das Cmdlet [New-AzureRmPublicIpAddress](https://msdn.microsoft.com/library/mt603620.aspx) , um die öffentliche IP-Adresse in der zuvor erstellten Ressourcengruppe mit dem Namen, dem Speicherort, der Zuordnungsmethode und der DNS-Domänennamenbezeichnung zu erstellen, die durch die zuvor initialisierten Variablen definiert werden.

> [!NOTE]
> Mit dem Cmdlet können auch weitere Eigenschaften der öffentlichen IP-Adresse definiert werden. Darauf wird in diesem Einstiegstutorial jedoch nicht weiter eingegangen. Sie können auch eine private Adresse oder eine Adresse mit einer statischen Adresse erstellen, aber auch das wird in diesem Tutorial nicht behandelt.
>
>

Führen Sie das folgende Cmdlet aus, um Ihre öffentliche IP-Adresse zu erstellen:

    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName

### <a name="create-the-network-interface"></a>Erstellen der Netzwerkschnittstelle
Nun können wir die Netzwerkschnittstelle für unseren virtuellen Computer erstellen. Wir erstellen unsere Netzwerkschnittstelle mithilfe des Cmdlets [New-AzureRmNetworkInterface](https://msdn.microsoft.com/library/mt619370.aspx) in der zuvor erstellten Ressourcengruppe und mit dem Namen, Speicherort, Subnetz und der öffentlichen IP-Adresse, die wir zuvor definiert haben.

Führen Sie das folgende Cmdlet aus, um Ihre Netzwerkschnittstelle zu erstellen:

    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

## <a name="configure-a-vm-object"></a>Konfigurieren eines VM-Objekts
Nachdem wir die Speicher- und Netzwerkressourcen definiert haben, können wir nun Compute-Ressourcen für den virtuellen Computer definieren. Im Rahmen unseres Tutorials geben wir die Größe des virtuellen Computers, verschiedene Betriebssystemeigenschaften und die zuvor erstellte Netzwerkschnittstelle an, definieren Blobspeicher und geben anschließend den Betriebssystemdatenträger an.

### <a name="create-the-vm-object"></a>Erstellen des VM-Objekts
Zunächst legen wir die Größe des virtuellen Computers fest. In diesem Tutorial geben wir dazu DS13 an. Wir erstellen mithilfe des Cmdlets [New-AzureRmVMConfig](https://msdn.microsoft.com/library/mt603727.aspx) ein konfigurierbares VM-Objekt mit dem Namen und der Größe, die durch die zuvor initialisierten Variablen definiert werden.

Führen Sie das folgende Cmdlet aus, um das VM-Objekt zu erstellen:

    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize

### <a name="create-a-credential-object-to-hold-the-name-and-password-for-the-local-administrator-credentials"></a>Erstellen eines Anmeldeinformationsobjekts zum Speichern von Name und Kennwort für die lokalen Administratoranmeldeinformationen
Bevor wir die Betriebssystemeigenschaften für den virtuellen Computer festlegen können, müssen wir die Anmeldeinformationen für das lokale Administratorkonto als sichere Zeichenfolge angeben. Hierzu verwenden wir das Cmdlet [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) .

Führen Sie das folgende Cmdlet aus, und geben Sie im Windows PowerShell-Fenster mit der Anmeldeinformationsanforderung den Namen und das Kennwort für das lokale Administratorkonto des virtuellen Windows-Computers an:

    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."

### <a name="set-the-operating-system-properties-for-the-virtual-machine"></a>Festlegen der Betriebssystemeigenschaften für den virtuellen Computer
Nun können wir die Betriebssystemeigenschaften des virtuellen Computers festlegen. Hierzu legen wir mithilfe des Cmdlets [Set-AzureRmVMOperatingSystem](https://msdn.microsoft.com/library/mt603843.aspx) die Art des Betriebssystems auf Windows fest, machen die Installation des [Agents für virtuelle Computer](../classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) erforderlich, geben an, dass das Cmdlet automatische Aktualisierungen ermöglicht, und legen den Namen des virtuellen Computers, den Computernamen und die Anmeldeinformationen mithilfe der zuvor initialisierten Variablen fest.

Führen Sie das folgende Cmdlet aus, um die Betriebssystemeigenschaften für Ihren virtuellen Computer festzulegen:

    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate

### <a name="add-the-network-interface-to-the-virtual-machine"></a>Hinzufügen der Netzwerkschnittstelle zum virtuellen Computer
Im nächsten Schritt fügen wir dem virtuellen Computer die zuvor erstellte Netzwerkschnittstelle hinzu. Hierzu verwenden wir das Cmdlet [Add-AzureRmVMNetworkInterface](https://msdn.microsoft.com/library/mt619351.aspx) mit der zuvor definierten Netzwerkschnittstellen-Variablen.

Führen Sie das folgende Cmdlet aus, um die Netzwerkschnittstelle für Ihren virtuellen Computer festzulegen:

    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id

### <a name="set-the-blob-storage-location-for-the-disk-to-be-used-by-the-virtual-machine"></a>Festlegen des Blobspeicherorts für den Datenträger des virtuellen Computers
Im nächsten Schritt legen wir unter Verwendung der zuvor definierten Variablen den Blobspeicherort für den Datenträger des virtuellen Computers fest.

Führen Sie das folgende Cmdlet aus, um den Blobspeicherort festzulegen:

    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"

### <a name="set-the-operating-system-disk-properties-for-the-virtual-machine"></a>Festlegen der Eigenschaften des Betriebssystemdatenträgers für den virtuellen Computer
Als Nächstes legen wir die Eigenschaften des Betriebssystemdatenträgers für den virtuellen Computer fest. Wir geben mithilfe des Cmdlets [Set-AzureRmVMOSDisk](https://msdn.microsoft.com/library/mt603746.aspx) an, dass das Betriebssystem für den virtuellen Computer auf einem Image basiert, legen eine schreibgeschützte Zwischenspeicherung fest (da SQL Server auf dem gleichen Datenträger installiert ist) und definieren den Namen des virtuellen Computers und den Betriebssystemdatenträger mithilfe der zuvor definierten Variablen.

Führen Sie das folgende Cmdlet aus, um die Eigenschaften des Betriebssystemdatenträgers für Ihren virtuellen Computer festzulegen:

    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

### <a name="specify-the-platform-image-for-the-virtual-machine"></a>Angeben des Plattformimage für den virtuellen Computer
In unserem letzten Konfigurationsschritt geben wir das Plattformimage für den virtuellen Computer an. Im Rahmen dieses Tutorials verwenden wir das neueste SQL Server 2016 CTP-Image. Wir verwenden das Cmdlet [Set-AzureRmVMSourceImage](https://msdn.microsoft.com/library/mt619344.aspx) , um dieses Image gemäß den zuvor definierten Variablen zu verwenden.

Führen Sie das folgende Cmdlet aus, um das Plattformimage für Ihren virtuellen Computer festzulegen:

    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

## <a name="create-the-sql-vm"></a>Erstellen des virtuellen SQL-Computers
Nach Abschluss der Konfigurationsschritte können Sie den virtuellen Computer nun erstellen. Wir verwenden das Cmdlet [New-AzureRmVM](https://msdn.microsoft.com/library/mt603754.aspx) , um den virtuellen Computer unter Verwendung der zuvor definierten Variablen zu erstellen.

Führen Sie das folgende Cmdlet aus, um Ihren virtuellen Computer zu erstellen:

    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

Der virtuelle Computer wird erstellt. Beachten Sie, dass ein Standardspeicherkonto für die Startdiagnose erstellt wird, da es sich bei dem angegebenen Speicherkonto für den Datenträger des virtuellen Computers um ein Storage Premium-Konto handelt.

Der Computer steht nun im Azure-Portal zur Verfügung. Hier können Sie [die öffentliche IP-Adresse und den vollständig qualifizierten Domänennamen des Computers](virtual-machines-windows-portal-sql-server-provision.md) anzeigen.

## <a name="example-script"></a>Beispielskript
Das folgende Skript enthält das vollständige PowerShell-Skript für dieses Tutorial. Es wird davon ausgegangen, dass Sie das Azure-Abonnement bereits eingerichtet haben, das mit den Befehlen **Add-AzureRmAccount** und **Select-AzureRmSubscription** verwendet werden soll.

    # Variables
    ## Global
    $Location = "SouthCentralUS"
    $ResourceGroupName = "sqlvm1"
    ## Storage
    $StorageName = $ResourceGroupName + "storage"
    $StorageSku = "Premium_LRS"

    ## Network
    $InterfaceName = $ResourceGroupName + "ServerInterface"
    $VNetName = $ResourceGroupName + "VNet"
    $SubnetName = "Default"
    $VNetAddressPrefix = "10.0.0.0/16"
    $VNetSubnetAddressPrefix = "10.0.0.0/24"
    $TCPIPAllocationMethod = "Dynamic"
    $DomainName = "sqlvm1"

    ##Compute
    $VMName = $ResourceGroupName + "VM"
    $ComputerName = $ResourceGroupName + "Server"
    $VMSize = "Standard_DS13"
    $OSDiskName = $VMName + "OSDisk"

    ##Image
    $PublisherName = "MicrosoftSQLServer"
    $OfferName = "SQL2016-WS2016"
    $Sku = "Enterprise"
    $Version = "latest"

    # Resource Group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    # Storage
    $StorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageName -SkuName $StorageSku -Kind "Storage" -Location $Location

    # Network
    $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix $VNetSubnetAddressPrefix
    $VNet = New-AzureRmVirtualNetwork -Name $VNetName -ResourceGroupName $ResourceGroupName -Location $Location -AddressPrefix $VNetAddressPrefix -Subnet $SubnetConfig
    $PublicIp = New-AzureRmPublicIpAddress -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -AllocationMethod $TCPIPAllocationMethod -DomainNameLabel $DomainName
    $Interface = New-AzureRmNetworkInterface -Name $InterfaceName -ResourceGroupName $ResourceGroupName -Location $Location -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $PublicIp.Id

    # Compute
    $VirtualMachine = New-AzureRmVMConfig -VMName $VMName -VMSize $VMSize
    $Credential = Get-Credential -Message "Type the name and password of the local administrator account."
    $VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName $ComputerName -Credential $Credential -ProvisionVMAgent -EnableAutoUpdate #-TimeZone = $TimeZone
    $VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $Interface.Id
    $OSDiskUri = $StorageAccount.PrimaryEndpoints.Blob.ToString() + "vhds/" + $OSDiskName + ".vhd"
    $VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name $OSDiskName -VhdUri $OSDiskUri -Caching ReadOnly -CreateOption FromImage

    # Image
    $VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -PublisherName $PublisherName -Offer $OfferName -Skus $Sku -Version $Version

    ## Create the VM in Azure
    New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VirtualMachine

## <a name="next-steps"></a>Nächste Schritte
Nach Erstellung des virtuellen Computers können Sie über RDP eine Verbindung mit dem virtuellen Computer herstellen und die Konnektivität einrichten. Weitere Informationen finden Sie unter [Verbinden mit SQL Server-Instanzen auf virtuellen Azure-Maschinen (Ressourcen-Manager)](virtual-machines-windows-sql-connect.md).


