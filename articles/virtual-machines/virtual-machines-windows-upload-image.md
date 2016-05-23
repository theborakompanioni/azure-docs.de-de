<properties
	pageTitle="Hochladen einer Windows-VHD für den Ressourcen-Manager | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie das Image eines virtuellen Windows-Computers zur Verwendung mit dem Resource Manager-Bereitstellungsmodell hochladen."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/06/2016"
	ms.author="dkshir"/>

# Hochladen eines Windows-VM-Images an Azure für Resource Manager-Bereitstellungen


In diesem Artikel erfahren Sie, wie Sie eine virtuelle Festplatte (Virtual Hard Disk, VHD) mit einem Windows-Betriebssystem hochladen, um damit unter Verwendung des Azure Resource Manager-Bereitstellungsmodells neue virtuelle Windows-Computer (Windows-VMs) zu erstellen. Ausführlichere Informationen zu Datenträgern und VHDs in Azure finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](virtual-machines-linux-about-disks-vhds.md).



## Voraussetzungen

In diesem Artikel wird davon ausgegangen, dass Sie über Folgendes verfügen:

- **Ein Azure-Abonnement**: Falls Sie noch nicht über ein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto erstellen](/pricing/free-trial/?WT.mc_id=A261C142F) und [MSDN-Abonnentenvorteile aktivieren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

- **Azure PowerShell ab Version 1.0**: Informationen zum Installieren finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

- **Virtueller Windows-Computer**: Virtuelle Computer können mit verschiedensten Tools lokal erstellt werden. Informationen hierzu finden Sie beispielsweise unter [Installieren der Hyper-V-Rolle und Konfigurieren eines virtuellen Computers](http://technet.microsoft.com/library/hh846766.aspx). Informationen zu von Azure unterstützen Windows-Betriebssystemen finden Sie unter [Microsoft Server Software-Support für virtuelle Microsoft Azure-Computer](https://support.microsoft.com/kb/2721672).


## Sicherstellen, dass für den virtuellen Computer das richtige Dateiformat verwendet wird

Azure akzeptiert Images nur für [virtuelle Computer der Generation 1](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx), die im VHD-Dateiformat gespeichert sind. Die Größe der virtuellen Festplatte muss statisch sein und als ganze, durch Acht teilbare Anzahl von Megabytes angegeben werden. Die maximal zulässige Größe für die virtuelle Festplatte beträgt 1.023 GB.

- Wenn Sie über ein Windows-VM-Image im VHDX-Format verfügen, konvertieren Sie es auf eine der folgenden Arten in eine virtuelle Festplatte:

	- Hyper-V: Öffnen Sie Hyper-V, und wählen Sie auf der linken Seite Ihren lokalen Computer aus. Klicken Sie im Menü darüber auf **Aktion** > **Datenträger bearbeiten...**. Klicken Sie auf **Weiter**, um durch die Bildschirme zu navigieren, und geben Sie dabei folgende Optionen ein: *Pfad für die VHDX-Datei* > **Konvertieren** > **VHD** > **Feste Größe** > *Pfad für die neue VHD-Datei*. Klicken Sie zum Abschluss auf **Fertig stellen**.

	- [Convert-VHD (PowerShell-Cmdlet)](http://technet.microsoft.com/library/hh848454.aspx): Weitere Informationen finden Sie im Blogbeitrag [Converting Hyper-V .vhdx to .vhd file formats](https://blogs.technet.microsoft.com/cbernier/2013/08/29/converting-hyper-v-vhdx-to-vhd-file-formats-for-use-in-windows-azure/) (So wird's gemacht: Konvertieren des Hyper-V-VHDX-Dateiformats in das VHD-Dateiformat).

- Wenn Sie über ein Windows-VM-Image im [VMDK-Dateiformat](https://en.wikipedia.org/wiki/VMDK) verfügen, konvertieren Sie es mit dem [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) in eine virtuelle Festplatte. Weitere Informationen finden Sie im Blog [How to Convert a VMWare VMDK to Hyper-V VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) (Konvertieren von VMWare-VMDK in Hyper-V-VHD).


## Vorbereiten des VHD-Images für den Upload

In diesem Abschnitt wird erläutert, wie Sie den virtuellen Windows-Computer generalisieren. Dadurch werden unter anderem alle persönlichen Kontoinformationen entfernt. Sie sollten diesen Schritt üblicherweise ausführen, wenn Sie ein VM-Image zum schnellen Bereitstellen ähnlicher virtueller Computer verwenden möchten. Weitere Informationen zu Sysprep finden Sie unter [How to Use Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx) (in englischer Sprache).

1. Melden Sie sich bei dem virtuellen Windows-Computer an.

2. Öffnen Sie das Eingabeaufforderungsfenster als Administrator. Wechseln Sie in das Verzeichnis **%windir%\\system32\\sysprep**, und führen Sie dann `sysprep.exe` aus.

3. Führen Sie im Dialogfeld **Systemvorbereitungstool** folgende Schritte aus:

	1. Wählen Sie unter **Systembereinigungsaktion** die Option **Out-of-Box-Experience (OOBE) für System aktivieren** und vergewissern Sie sich, dass das Kontrollkästchen **Verallgemeinern** aktiviert ist.

	2. Wählen Sie unter **Optionen für Herunterfahren** die Option **Herunterfahren**.

	3. Klicken Sie auf **OK**.

	![Starten von Sysprep](./media/virtual-machines-windows-upload-image/sysprepgeneral.png)

</br> <a id="createstorage"></a>
## Erstellen oder Suchen eines Azure-Speicherkontos

Sie benötigen ein Speicherkonto in Azure, um das VM-Image hochzuladen. Sie können ein vorhandenes Speicherkonto auswählen oder ein neues erstellen. Für diesen Schritt kann entweder das Azure-Portal oder PowerShell verwendet werden.

### So erstellen oder finden Sie ein Speicherkonto mithilfe des Azure-Portals

1. Melden Sie sich beim [Portal](https://portal.azure.com) an.

2. Klicken Sie auf **Durchsuchen** > **Speicherkonten**.

3. Prüfen Sie, ob ein Speicherkonto vorhanden ist, das Sie zum Hochladen des Images verwenden möchten. Notieren Sie den Namen dieses Speicherkontos. Bei Verwendung eines vorhandenen Speicherkontos können Sie mit dem Abschnitt [Hochladen des VM-Images auf Ihr Speicherkonto](#uploadvm) fortfahren.

4. Wenn Sie ein neues Speicherkonto erstellen möchten, klicken Sie auf **Hinzufügen**, und geben Sie folgende Informationen ein:

	1. Geben Sie den Namen für das Speicherkonto ein. Er sollte nur zwischen 3 und 24 Kleinbuchstaben und Ziffern enthalten. Dieser Name wird Teil der URL, über die Sie auf Blobs, Dateien und andere Ressourcen des Speicherkontos zugreifen.
	
	2. Wählen Sie als Bereitstellungsmodell die Option *Resource Manager* aus.

	3. Wählen Sie passende Werte für Kontoart, Leistung und Replikation aus. Zeigen Sie bei Bedarf auf die Informationssymbole, um weitere Informationen zu den Werten zu erhalten.

	4. Wählen Sie unter **Ressourcengruppe** entweder *+ Neu* oder eine vorhandene Ressourcengruppe aus. Falls Sie eine neue Ressourcengruppe erstellen möchten, geben Sie den Namen der neuen Ressourcengruppe ein.

	5. Wählen Sie den Standort für das Speicherkonto aus, und klicken Sie anschließend auf **Erstellen**. Das Konto wird nun im Bereich **Speicherkonten** angezeigt.

		![Speicherkontodetails eingeben](./media/virtual-machines-windows-upload-image/portal_create_storage_account.png)

	6. Dieser und die nächsten Schritte zeigen die Vorgehensweise zum Erstellen eines Blobcontainers in diesem Speicherkonto. Dies ist optional, da Sie auch den PowerShell-Befehl zum Hochladen des Images verwenden können, um einen neuen Blobcontainer für Ihr Image zu erstellen. Wenn Sie den Container nicht selbst erstellen möchten, fahren Sie mit dem Abschnitt [Hochladen des VM-Images auf Ihr Speicherkonto](#uploadvm) fort. Klicken Sie andernfalls auf der Kachel **Dienste** auf **Blobs**.

		![Blob-Dienst](./media/virtual-machines-windows-upload-image/portal_create_blob.png)

	7. Wenn der Blobbereich angezeigt wird, klicken Sie auf **+ Container**, um einen neuen Blobspeichercontainer zu erstellen. Geben Sie den Namen des Containers und den Zugriffstyp ein.

		![Erstellen eines neuen Blobs](./media/virtual-machines-windows-upload-image/portal_create_container.png)

  		> [AZURE.NOTE] Der Container ist standardmäßig privat, und der Containerzugriff ist auf den Kontobesitzer eingeschränkt. Mit der Option **Blob** können Sie eine öffentliche Leseberechtigung für die im Container enthaltenen Blobs, jedoch nicht für die Containereigenschaften und -metadaten gewähren. Eine uneingeschränkte öffentliche Leseberechtigung für den Container und die Blobs kann mithilfe der Option **Container** gewährt werden.

	8. Der neue Blobcontainer wird im Bereich **Blob-Dienst** angezeigt. Notieren Sie sich die URL dieses Containers; sie wird für den PowerShell-Befehl zum Hochladen des Images benötigt. Je nach Bildschirmauflösung und Länge der URL wird die URL unter Umständen nicht vollständig angezeigt. Maximieren Sie in diesem Fall den Bereich, indem Sie in der rechten oberen Ecke auf das Symbol **Maximieren** klicken.


### So erstellen oder finden Sie ein Speicherkonto mithilfe von PowerShell

1. Öffnen Sie Azure PowerShell 1.0.x, und melden Sie sich bei Ihrem Azure-Konto an.

		Login-AzureRmAccount

	Dieser Befehl öffnet ein Popupfenster, in dem Sie Ihre Azure-Anmeldeinformationen eingeben können.

2. Falls die standardmäßig ausgewählte Abonnement-ID nicht der gewünschten ID entspricht, verwenden Sie einen der folgenden Befehle, um das gewünschte Abonnement festzulegen:

		Set-AzureRmContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"

	oder

		Select-AzureRmSubscription -SubscriptionId "xxxx-xxxx-xxxx-xxxx"

	Die in Ihrem Azure-Konto enthaltenen Abonnements können mithilfe des Befehls `Get-AzureRmSubscription` ermittelt werden.

3. Ermitteln Sie die unter diesem Abonnement verfügbaren Speicherkonten.

		Get-AzureRmStorageAccount

	Wenn Sie ein vorhandenes Speicherkonto verwenden möchten, fahren Sie mit dem Abschnitt [Hochladen des VM-Images auf Ihr Speicherkonto](#uploadvm) fort.

4. Gehen Sie andernfalls wie folgt vor, um ein neues Speicherkonto zum Speichern dieses Images zu erstellen:

	1. Vergewissern Sie sich, dass Sie über eine Ressourcengruppe für dieses Speicherkonto verfügen. Verwenden Sie den folgenden Befehl, um alle in Ihrem Abonnement enthaltenen Ressourcengruppen zu ermitteln:

			Get-AzureRmResourceGroup

	2. Wenn Sie eine neue Ressourcengruppe erstellen möchten, verwenden Sie diesen Befehl:

			New-AzureRmResourceGroup -Name YourResourceGroup -Location "West US"

	3. Erstellen Sie ein neues Speicherkonto in dieser Ressourcengruppe mit:

			New-AzureRmStorageAccount -ResourceGroupName YourResourceGroup -Name YourStorageAccountName -Location "West US" -Type "Standard_GRS"


</br> <a id="uploadvm"></a>

## Hochladen des VM-Images auf Ihr Speicherkonto

Führen Sie in Azure PowerShell die folgenden Schritte aus, um das VM-Image in Ihr Speicherkonto hochzuladen. Das Image wird in einen Blob-Speichercontainer des Kontos hochgeladen. Sie können entweder einen vorhandenen Container verwenden oder einen neuen Container erstellen.

1. Melden Sie mithilfe des folgenden Befehls bei Azure PowerShell 1.0.x an: `Login-AzureRmAccount`. Vergewissern Sie sich mithilfe des Befehls `Set-AzureRmContext -SubscriptionId "xxxx-xxxx-xxxx-xxxx"`, dass Sie das richtige Abonnement verwenden (wie im vorherigen Abschnitt beschrieben).

2. Fügen Sie die generalisierte virtuelle Azure-Festplatte mithilfe des Cmdlets [Add-AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) dem Speicherkonto hinzu:

		Add-AzureRmVhd -ResourceGroupName YourResourceGroup -Destination "<StorageAccountURL>/<BlobContainer>/<TargetVHDName>.vhd" -LocalFilePath <LocalPathOfVHDFile>

	Hierbei gilt:
	- **StorageAccountURL** ist die URL für das Speicherkonto. Sie hat in der Regel folgendes Format: `https://YourStorageAccountName.blob.core.windows.net`. Wichtig: Ersetzen Sie *YourStorageAccountName* durch den Namen des vorhandenen oder neuen Speicherkontos.
	- **BlobContainer** ist der BLOB-Container, in dem Sie Ihre Images speichern möchten. Wenn das Cmdlet keinen vorhandenen BLOB-Container dieses Namens findet, erstellt es einen neuen für Sie.
	- **TargetVHDName** ist der Name, unter dem Sie das Image speichern möchten.
	- **LocalPathOfVHDFile** ist der vollständige Pfad und Name der VHD-Datei auf Ihrem lokalen Computer.

	Eine erfolgreiche Ausführung von `Add-AzureRmVhd` führt zu einem Ergebnis wie dem folgenden:

		C:\> Add-AzureRmVhd -ResourceGroupName testUpldRG -Destination https://testupldstore2.blob.core.windows.net/testblobs/WinServer12.vhd -LocalFilePath "C:\temp\WinServer12.vhd"
		MD5 hash is being calculated for the file C:\temp\WinServer12.vhd.
		MD5 hash calculation is completed.
		Elapsed time for the operation: 00:03:35
		Creating new page blob of size 53687091712...
		Elapsed time for upload: 01:12:49

		LocalFilePath           DestinationUri
		-------------           --------------
		C:\temp\WinServer12.vhd https://testupldstore2.blob.core.windows.net/testblobs/WinServer12.vhd

	Die Ausführung dieses Befehls nimmt – abhängig von Ihrer Netzwerkverbindung und der Größe Ihrer VHD-Datei – einige Zeit in Anspruch.

</br>
## Bereitstellen eines neuen virtuellen Computers vom hochgeladenen Image aus

Jetzt können Sie das hochgeladene Image verwenden, um eine neue Windows-VM zu erstellen. In den folgenden Schritten erfahren Sie, wie Sie mit Azure PowerShell und dem in den vorherigen Schritten hochgeladenen VM-Image einen virtuellen Computer in einem neuen virtuellen Netzwerk erstellen.

>[AZURE.NOTE] Das VM-Image und der zu erstellende virtuelle Computer müssen sich in dem gleichen Speicherkonto befinden.

### Erstellen von Netzwerkressourcen

Verwenden Sie das folgende PowerShell-Beispielskript, um ein virtuelles Netzwerk und eine Netzwerkschnittstellenkarte (Network Interface Card, NIC) für den neuen virtuellen Computer zu erstellen. Verwenden Sie für die durch **$** gekennzeichneten Variablen geeignete Werte für Ihre Anwendung.

	$pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic

	$subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnet1Name -AddressPrefix $vnetSubnetAddressPrefix

	$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix $vnetAddressPrefix -Subnet $subnetconfig

	$nic = New-AzureRmNetworkInterface -Name $nicname -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id

### Erstellen eines neuen virtuellen Computers

Das folgende PowerShell-Skript veranschaulicht, wie Sie die Konfiguration für den virtuellen Computer einrichten und das hochgeladene VM-Image als Quelle für die neue Installation verwenden: </br>

	#Enter a new user name and password in the pop-up window for the following
	$cred = Get-Credential

	#Get the storage account where the uploaded image is stored
	$storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

	#Set the VM name and size
	#Use "Get-Help New-AzureRmVMConfig" to know the available options for -VMsize
	$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A4"

	#Set the Windows operating system configuration and add the NIC
	$vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate

	$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

	#Create the OS disk URI
	$osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

	#Configure the OS disk to be created from the image (-CreateOption fromImage), and give the URL of the uploaded image VHD for the -SourceImageUri parameter
	#You can find this URL in the result of the Add-AzureRmVhd cmdlet above
	$vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri $urlOfUploadedImageVhd -Windows

	#Create the new VM
	New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm

Der Workflow kann etwa wie folgt aussehen:

		C:\> $pipName = "testpip6"
		C:\> $pip = New-AzureRmPublicIpAddress -Name $pipName -ResourceGroupName $rgName -Location $location -AllocationMethod Dynamic
		C:\> $subnet1Name = "testsub6"
		C:\> $nicname = "testnic6"
		C:\> $vnetName = "testvnet6"
		C:\> $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name $subnet1Name -AddressPrefix $vnetSubnetAddressPrefix
		C:\> $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location -AddressPrefix $vnetAddressPrefix -Subnet $subnetconfig
		C:\> $nic = New-AzureRmNetworkInterface -Name $nicname -ResourceGroupName $rgName -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
		C:\> $vmName = "testupldvm6"
		C:\> $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A4"
		C:\> $computerName = "testupldcomp6"
		C:\> $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
		C:\> $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
		C:\> $osDiskName = "testupos6"
		C:\> $osDiskUri = '{0}vhds/{1}{2}.vhd' -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName
		C:\> $urlOfUploadedImageVhd = "https://testupldstore2.blob.core.windows.net/testblobs/WinServer12.vhd"
		C:\> $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption fromImage -SourceImageUri $urlOfUploadedImageVhd -Windows
		C:\> $result = New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
		C:\> $result
		RequestId IsSuccessStatusCode StatusCode ReasonPhrase
		--------- ------------------- ---------- ------------
		                         True         OK OK

Der neu erstellte virtuelle Computer müsste nun im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden. Alternativ können Sie ihn auch mit den folgenden PowerShell-Befehlen anzeigen:

	$vmList = Get-AzureRmVM -ResourceGroupName $rgName
	$vmList.Name


## Nächste Schritte

Informationen zum Verwalten des neuen virtuellen Computers mithilfe von Azure PowerShell finden Sie unter [Verwalten von virtuellen Azure-Computern mit Resource Manager und PowerShell](virtual-machines-windows-ps-manage.md).

<!---HONumber=AcomDC_0511_2016-->