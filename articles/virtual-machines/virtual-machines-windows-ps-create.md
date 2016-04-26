<properties
	pageTitle="Erstellen einer Azure-VM mit PowerShell | Microsoft Azure"
	description="Verwenden Sie Azure PowerShell und Azure Resource Manager, um ohne großen Aufwand eine neue VM mit Windows Server zu erstellen."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="davidmu1"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="04/12/2016"
	ms.author="davidmu"/>

# Erstellen einer Windows-VM mit dem Resource Manager und PowerShell

In diesem Artikel wird beschrieben, wie Sie schnell einen virtuellen Azure-Computer mit Windows Server und den dazugehörigen Ressourcen erstellen, indem Sie den Resource Manager und PowerShell verwenden.

Die Ausführung der Schritte im Artikel dauert ungefähr 30 Minuten.

## Schritt 1: Installieren von Azure PowerShell

Informationen dazu, wie Sie die aktuelle Version von Azure PowerShell installieren, das gewünschte Abonnement auswählen und sich am Azure-Konto anmelden, finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).
        
## Schritt 2: Erstellen einer Ressourcengruppe

Alle Ressourcen müssen in einer Ressourcengruppe bereitgestellt werden. Weitere Informationen finden Sie unter [Übersicht über den Azure Resource Manager](../resource-group-overview.md).

1. Rufen Sie eine Liste mit den verfügbaren Standorten ab, an denen Ressourcen erstellt werden können.

	    Get-AzureLocation | sort Name | Select Name

2. Ersetzen Sie den Wert von **$locName** durch einen Standort aus der Liste, z.B. **USA, Mitte**. Erstellen Sie die Variable.

        $locName = "location name"
        
3. Ersetzen Sie den Wert von **$rgName** durch den Namen der neuen Ressourcengruppe. Erstellen Sie die Variable und die Ressourcengruppe.

        $rgName = "resource group name"
        New-AzureRmResourceGroup -Name $rgName -Location $locName
    
## Schritt 3: Erstellen eines Speicherkontos

Ein Speicherkonto wird zum Speichern der virtuellen Festplatte benötigt, die dem erstellten virtuellen Computer zugeordnet ist.

1. Ersetzen Sie den Wert von **$stName** (nur Kleinbuchstaben und Zahlen) durch den Namen des Speicherkontos. Testen Sie den Namen auf Eindeutigkeit.

        $stName = "storage account name"
        Test-AzureName -Storage $stName

    Wenn dieser Befehl **False** zurückgibt, ist der vorgeschlagene Name eindeutig.
    
2. Führen Sie nun den Befehl zum Erstellen des Speicherkontos aus.
    
        $storageAcc = New-AzureRmStorageAccount -ResourceGroupName $rgName -Name $stName -Type "Standard_LRS" -Location $locName
        
## Schritt 4: Erstellen eines virtuellen Netzwerks

Alle virtuellen Computer müssen einem virtuellen Netzwerk zugeordnet sein.

1. Ersetzen Sie den Wert von **$subnetName** durch den Namen des Subnetzes. Erstellen Sie die Variable und das Subnetz.
    	
        $subnetName = "subnet name"
        $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
        
2. Ersetzen Sie den Wert von **$vnetName** durch den Namen des virtuellen Netzwerks. Erstellen Sie die Variable und das virtuelle Netzwerk mit dem Subnetz.

        $vnetName = "virtual network name"
        $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $locName -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
        
## Schritt 5: Erstellen einer öffentlichen IP-Adresse und Netzwerkschnittstelle

Sie benötigen eine öffentliche IP-Adresse und eine Netzwerkschnittstelle, um die Kommunikation mit dem virtuellen Computer im virtuellen Netzwerk zu ermöglichen.

1. Ersetzen Sie den Wert von **$ipName** durch den Namen der öffentlichen IP-Adresse. Erstellen Sie die Variable und die öffentliche IP-Adresse.

        $ipName = "public IP address name"
        $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $locName -AllocationMethod Dynamic
        
2. Ersetzen Sie den Wert von **$nicName** durch den Namen der Netzwerkschnittstelle. Erstellen Sie die Variable und die Netzwerkschnittstelle.

        $nicName = "network interface name"
        $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $locName -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
        
## Schritt 6: Erstellen eines virtuellen Computers

Nachdem Sie nun alle Komponenten eingerichtet haben, können Sie den virtuellen Computer erstellen.

1. Führen Sie den Befehl zum Festlegen des Administratorkontonamens und des dazugehörigen Kennworts für den virtuellen Computer aus.

        $cred = Get-Credential -Message "Type the name and password of the local administrator account."
        
2. Ersetzen Sie den Wert von **$vmName** durch den Namen des virtuellen Computers. Erstellen Sie die Variable und die Konfiguration des virtuellen Computers.

        $vmName = "virtual machine name"
        $vm = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A1"
        
    Eine Liste mit den verfügbaren Größen für einen virtuellen Computer finden Sie unter [Größen für virtuelle Computer in Azure](virtual-machines-windows-sizes.md).
    
3. Ersetzen Sie den Wert von **$compName** durch den Computernamen des virtuellen Computers. Erstellen Sie die Variable, und fügen Sie der Konfiguration die Betriebssysteminformationen hinzu.

        $compName = "computer name"
        $vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $compName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
        
4. Definieren Sie das Image, das für die Bereitstellung des virtuellen Computers verwendet werden soll.

        $vm = Set-AzureRmVMSourceImage -VM $vm -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2012-R2-Datacenter -Version "latest"
        
    Weitere Informationen zur Auswahl von Images finden Sie unter [Navigieren zu und Auswählen von Images virtueller Windows-Computer in Azure mithilfe von PowerShell oder der Befehlszeilenschnittstelle](virtual-machines-windows-cli-ps-findimage.md).
        
5. Fügen Sie die erstellte Netzwerkschnittstelle der Konfiguration hinzu.

        $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
        
6. Ersetzen Sie den Wert von **$blobPath** durch den gespeicherten Pfad und Dateinamen der virtuellen Festplatte. Die VHD-Datei wird normalerweise in einem Container gespeichert, z.B. „vhds/WindowsVMosDisk.vhd“. Erstellen Sie die Variablen.

        $blobPath = "vhd path and file name"
        $osDiskUri = $storageAcc.PrimaryEndpoints.Blob.ToString() + $blobPath
        
7. Ersetzen Sie den Wert von **$diskName** durch den Namen des Betriebssystem-Datenträgers. Erstellen Sie die Variable, und fügen Sie der Konfiguration die Festplatteninformationen hinzu.

        $diskName = "windowsvmosdisk"
        $vm = Set-AzureRmVMOSDisk -VM $vm -Name $diskName -VhdUri $osDiskUri -CreateOption fromImage
        
8. Erstellen Sie als Letztes den virtuellen Computer.

        New-AzureRmVM -ResourceGroupName $rgName -Location $locName -VM $vm

    Im Azure-Portal sollte nun die Ressourcengruppe mit den dazugehörigen Ressourcen angezeigt werden, und im PowerShell-Fenster sollte der Status „Erfolgreich“ angegeben werden:

        RequestId  IsSuccessStatusCode  StatusCode  ReasonPhrase
        ---------  -------------------  ----------  ------------
                                  True          OK  OK
                                  
## Nächste Schritte

- Falls bei der Bereitstellung Probleme aufgetreten sind, können Sie beispielsweise mit dem Artikel [Problembehandlung beim Bereitstellen von Ressourcengruppen mit dem Azure-Portal](../resource-manager-troubleshoot-deployments-portal.md) fortfahren.
- Informationen zum Verwalten des gerade erstellten virtuellen Computers finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md).
- Nutzen Sie die Vorteile der Erstellung eines virtuellen Computers per Vorlage, indem Sie sich die Informationen unter [Erstellen eines virtuellen Windows-Computers mit einer Resource Manager-Vorlage](virtual-machines-windows-ps-template.md) durchlesen.

<!---HONumber=AcomDC_0420_2016-->