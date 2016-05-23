<properties
	pageTitle="Erstellen einer Kopie Ihres virtuellen Windows-Computers | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine Kopie des virtuellen Azure-Computers unter Windows im Resource Manager-Bereitstellungsmodell erstellen, indem Sie ein *spezialisiertes Image* erstellen."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/26/2016"
	ms.author="dkshir"/>

# Erstellen einer Kopie eines virtuellen Windows-Computers im Resource Manager-Bereitstellungsmodell


In diesem Artikel erfahren Sie, wie Sie mit Azure PowerShell und dem Azure-Portal eine Kopie des virtuellen Azure-Computers (VM) unter Windows im Resource Manager-Bereitstellungsmodell erstellen können. Er zeigt, wie ein **_spezialisiertes_** Image Ihrer Azure-VM erstellt wird, das die Benutzerkonten und andere Statusdaten des ursprünglichen virtuellen Computers enthält. Ein spezialisiertes Image eignet sich z.B. für folgende Szenarien: Portieren der Windows-VM aus dem klassischen Bereitstellungsmodell in das Resource Manager-Bereitstellungsmodell oder Erstellen einer Sicherungskopie Ihrer im Resource Manager-Bereitstellungsmodell erstellten Windows-VM. Sie können die Datenträger für das Betriebssystem und die Daten auf diese Weise kopieren und dann die Netzwerkressourcen einrichten, um den neuen virtuellen Computer zu erstellen.

Wenn Sie Massenbereitstellungen ähnlicher Windows-VMs erstellen müssen, benötigen Sie ein *generalisiertes* Image. Weitere Informationen finden Sie unter [Erfassen eines virtuellen Windows-Computers](virtual-machines-windows-capture-image.md).



## Vorbereitungen

In diesem Artikel wird davon ausgegangen, dass Sie über Folgendes verfügen:

1. Einen **virtuellen Azure-Computer mit Windows** im klassischen oder Resource Manager-Bereitstellungsmodell mit konfiguriertem Betriebssystem, angehängten Datenträgern und installierten erforderlichen Anwendungen. Wenn Sie Hilfe beim Erstellen dieses virtuellen Computers benötigen, finden Sie Informationen unter [Erstellen eines virtuellen Windows-Computers mit Ressource Manager](virtual-machines-windows-ps-create.md). 

1. Sie haben **Azure PowerShell 1.0 oder höher** auf Ihrem Computer installiert und sind bei Ihrem Azure-Abonnement angemeldet. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

1. Das **AzCopy-Tool** ist auf Ihrem Computer installiert. Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](../storage/storage-use-azcopy.md).

1. Eine **Ressourcengruppe** mit einem **Speicherkonto** und einem darin erstellten **Blob-Container**, in den die virtuellen Festplatten kopiert werden. Im Abschnitt [Erstellen oder Suchen eines Azure-Speicherkontos](virtual-machines-windows-upload-image.md#createstorage) sind die Schritte aufgeführt, wenn Sie ein vorhandenes Speicherkonto verwenden oder ein neues Speicherkonto erstellen möchten.



> [AZURE.NOTE] Ähnliche Schritte gelten für einen virtuellen Computer, der mit einem der beiden Bereitstellungsmodelle als Quellimage erstellt wurde. Wir werden bei Bedarf auf kleinere Unterschiede hinweisen.


## Kopieren von VHDs in Ihr Resource Manager-Speicherkonto


1. Geben Sie zuerst die VHDs frei, die vom virtuellen Quellcomputer verwendet werden, indem Sie eine der beiden folgenden Optionen ausführen:

	- Wenn Sie den virtuellen Quellcomputer **_kopieren_** möchten, **beenden** Sie ihn, und **heben Sie dessen Zuordnung auf**.
	
		- Für einen mit dem klassischen Bereitstellungsmodell erstellten virtuellen Computer können Sie das [Portal](https://portal.azure.com) verwenden und auf **Durchsuchen** > **Virtuelle Computer (klassisch)** > *Ihr virtueller Computer* > **Beenden** klicken. Sie können aber auch den PowerShell-Befehl `Stop-AzureVM -ServiceName <yourServiceName> -Name <yourVmName>` verwenden. 
		
		- Für einen virtuellen Computer im Resource Manager-Bereitstellungsmodell können Sie sich beim Portal anmelden und auf **Durchsuchen** > **Virtuelle Computer** > *Ihr virtueller Computer* > **Beenden** klicken. Sie können aber auch den PowerShell-Befehl `Stop-AzureRmVM -ResourceGroupName <yourResourceGroup> -Name <yourVmName>` verwenden. Beachten Sie, dass der *Status* des virtuellen Computers im Portal von **Wird ausgeführt** in **Beendet (Zuordnung aufgehoben)** geändert wird.

	
	- Wenn Sie Ihren virtuellen Quellcomputer **_migrieren_** möchten, **löschen** Sie den virtuellen Computer, und verwenden Sie die verbliebene VHD. **Suchen** Sie nach dem virtuellen Computer im [Portal](https://portal.azure.com), und klicken Sie auf **Löschen**.
	
1. Suchen Sie nach dem Zugriffsschlüssel für das Speicherkonto, das Ihre Quell-VHD enthält, und nach dem Zugriffsschlüssel für das Speicherkonto, in das Sie Ihre VHD zum Erstellen des neuen virtuellen Computers kopieren. Der Schlüssel für das Konto, aus dem die VHD kopiert wird, ist der *Quellschlüssel*, und der Schlüssel für das Konto, in das sie kopiert wird, ist der *Zielschlüssel*. Weitere Informationen zu Zugriffsschlüsseln finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md).

	- Wenn Ihre Quell-VM mit dem klassischen Bereitstellungsmodell erstellt wurde, klicken Sie auf **Durchsuchen** > **Speicherkonten (klassisch)** > *Ihr Speicherkonto* > **Alle Einstellungen** > **Schlüssel**, und kopieren Sie den Schlüssel mit der Bezeichnung **PRIMÄRER ZUGRIFFSSCHLÜSSEL**. 
	
	- Für einen mit dem Resource Manager-Bereitstellungsmodell erstellten virtuellen Computer oder für das Speicherkonto, das Sie für den neuen virtuellen Computer verwenden möchten, klicken Sie auf **Durchsuchen** > **Speicherkonten** > *Ihr Speicherkonto* > **Alle Einstellungen** > **Zugriffsschlüssel**, und kopieren Sie den Schlüssel mit der Bezeichnung **key1**.

1. Rufen Sie die URLs ab, um auf die Quell- und Zielspeicherkonten zuzugreifen. **Wechseln** Sie im Portal zu Ihrem Speicherkonto, und klicken Sie auf **Blobs**. Klicken Sie dann auf den Container, der die Quell-VHD hostet (z.B. *vhds* für das klassische Bereitstellungsmodell), oder auf den Container, in den die VHD kopiert werden soll. Klicken Sie für den Container auf **Eigenschaften**, und kopieren Sie den Text mit der Bezeichnung **URL**. Wir benötigen die URLs des Quell- und des Zielcontainers. Die URLs sehen etwa wie folgt aus: `https://myaccount.blob.core.windows.net/mycontainer`.

1. Öffnen Sie auf dem lokalen Computer ein Befehlsfenster, und navigieren Sie zum Ordner, in dem AzCopy installiert ist. Dies sollte ein ähnlicher Ordner wie *C:\\Program Files (x86) \\Microsoft SDKs\\Azure\\AzCopy* sein. Führen Sie hier den folgenden Befehl aus: </br>

		AzCopy /Source:<URL_of_the_source_blob_container> /Dest:<URL_of_the_destination_blob_container> /SourceKey:<Access_key_for_the_source_storage> /DestKey:<Access_key_for_the_destination_storage> /Pattern:<File_name_of_the_VHD_you_are_copying>
</br>

>[AZURE.NOTE] Sie müssen die Datenträger für das Betriebssystem und die Daten mit AzCopy getrennt kopieren, wie oben beschrieben.


## Erstellen eines virtuellen Computers mit der kopierten VHD

Diese Schritte zeigen, wie Sie Azure PowerShell verwenden, um eine auf Resource Manager basierende Windows-VM in einem neuen virtuellen Netzwerk mit der in den obigen Schritten kopierten VHD zu erstellen. Die VHD und der neu zu erstellende virtuelle Computer müssen sich im gleichen Speicherkonto befinden.


Richten Sie zuerst ein virtuelles Netzwerk und eine Netzwerkschnittstellenkarte (Network Interface Card, NIC) für den neuen virtuellen Computer mit einem Skript wie dem folgenden ein. Verwenden Sie für die durch das **$**-Zeichen dargestellten Variablen Werte, die für Ihre Anwendung geeignet sind.

	$pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic

	$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnet1Name -AddressPrefix $vnetSubnetAddressPrefix

	$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix $vnetAddressPrefix -Subnet $subnetconfig

	$nic = New-AzureRmNetworkInterface -Name $nicname -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id


Richten Sie jetzt die VM-Konfigurationen ein, und verwenden Sie die kopierten VHDs, um einen neuen virtuellen Computer zu erstellen, wie unten dargestellt. </br>

	#Set the VM name and size
	$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"

	#Add the NIC
	$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

	#Add the OS disk using the URL of the copied OS VHD
	$osDiskName = $vmName + "osDisk"
	$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
	
	#Add data disks using the URLs of the copied data VHDs at the appropriate Logical Unit Number (Lun)
	$dataDiskName = $vmName + "dataDisk"
	$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 0 -CreateOption attach
	
Die URLs der Daten- und Betriebssystemdatenträger sehen wie folgt aus: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Sie können dies im Portal herausfinden, indem Sie zum Zielspeichercontainer wechseln, auf die kopierte Betriebssystem- oder Daten-VHD klicken und dann den Inhalt der **URL** kopieren.
	
	#Create the new VM
	New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
	
War dieser Befehl erfolgreich, erscheint eine Ausgabe wie die folgende:

	RequestId IsSuccessStatusCode StatusCode ReasonPhrase
	--------- ------------------- ---------- ------------
	                         True         OK OK


Sie sollten nun den neu erstellten virtuellen Computer im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** sehen. Alternativ können Sie ihn auch mit den folgenden PowerShell-Befehlen ermitteln:

	$vmList = Get-AzureRmVM -ResourceGroupName $rgName
	$vmList.Name

Melden Sie sich beim neuen virtuellen Computer an, **wechseln** Sie zum virtuellen Computer im [Portal](https://portal.azure.com), klicken Sie auf **Verbinden**, und öffnen Sie die *Remotedesktop*-RDP-Datei. Verwenden Sie die Kontoanmeldeinformationen des ursprünglichen virtuellen Computers für die Anmeldung bei Ihrem neuen virtuellen Computer.


## Nächste Schritte

Informationen zum Verwalten des neuen virtuellen Computers mit Azure PowerShell finden Sie unter [Verwalten von virtuellen Computern mit Azure Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md).

<!---HONumber=AcomDC_0511_2016-->