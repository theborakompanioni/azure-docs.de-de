<properties
	pageTitle="Erstellen einer Kopie Ihres virtuellen Linux-Computers | Microsoft Azure"
	description="Erfahren Sie, wie Sie eine Kopie des virtuellen Azure-Computers unter Linux im Resource Manager-Bereitstellungsmodell erstellen, indem Sie ein *spezialisiertes Image* erstellen."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="dsk-2015"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/26/2016"
	ms.author="dkshir"/>

# Erstellen einer Kopie eines virtuellen Linux-Computers im Resource Manager-Bereitstellungsmodell



In diesem Artikel erfahren Sie, wie Sie mit der Azure-Befehlszeilenschnittstelle und dem Azure-Portal eine Kopie des virtuellen Azure-Computers (VM) unter Linux im Resource Manager-Bereitstellungsmodell erstellen können. Er zeigt, wie ein **_spezialisiertes_** Image Ihrer Azure-VM erstellt wird, das die Benutzerkonten und andere Statusdaten des ursprünglichen virtuellen Computers enthält. Ein spezialisiertes Image eignet sich z.B. für folgende Szenarien: Portieren der Linux-VM aus dem klassischen Bereitstellungsmodell in das Resource Manager-Bereitstellungsmodell oder Erstellen einer Sicherungskopie Ihrer im Resource Manager-Bereitstellungsmodell erstellten Linux-VM. Sie können die Datenträger für das Betriebssystem und die Daten auf diese Weise kopieren und dann die Netzwerkressourcen einrichten, um den neuen virtuellen Computer zu erstellen.

Wenn Sie Massenbereitstellungen ähnlicher Linux-VMs erstellen müssen, benötigen Sie ein *generalisiertes* Image. Weitere Informationen finden Sie unter [Erfassen eines virtuellen Linux-Computers](virtual-machines-linux-capture-image.md).



## Vorbereitungen

In diesem Artikel wird davon ausgegangen, dass die folgenden Voraussetzungen erfüllt sind, bevor Sie mit den Schritten beginnen:

1. Sie haben einen virtuellen Azure-Computer unter Linux, der mit dem klassischen Bereitstellungsmodell oder dem Resource Manager-Bereitstellungsmodell erstellt wurde. Sie haben das Betriebssystem konfiguriert, Datenträger angefügt und andere Anpassungen vorgenommen, wie die Installation erforderlicher Anwendungen. Wir verwenden diesen virtuellen Computer, um die Kopie zu erstellen. Wenn Sie Hilfe bei der Erstellung des virtuellen Quellcomputers benötigen, lesen Sie [Erstellen eines virtuellen Linux-Computers in Azure](virtual-machines-linux-quick-create-cli.md). 

1. Sie haben die Azure-Befehlszeilenschnittstelle heruntergeladen und auf Ihrem Computer installiert und sind bei Ihrem Azure-Abonnement angemeldet. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md).

1. Sie haben eine Ressourcengruppe, ein Speicherkonto und einen Blobcontainer in der Ressourcengruppe erstellt, in die die VHDs kopiert werden. Weitere Informationen zum Erstellen von Speicherkonten und Blobcontainern mit der Azure-Befehlszeilenschnittstelle finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle mit Azure-Speicher](../storage/storage-azure-cli.md).


> [AZURE.NOTE] Ähnliche Schritte gelten für einen virtuellen Computer, der mit einem der beiden Bereitstellungsmodelle als Quellimage erstellt wurde. Wir werden bei Bedarf auf kleinere Unterschiede hinweisen.


## Kopieren von VHDs in Ihr Resource Manager-Speicherkonto


1. Geben Sie zuerst die VHDs frei, die vom virtuellen Quellcomputer verwendet werden, indem Sie eine der beiden folgenden Optionen ausführen:

	- Wenn Sie den virtuellen Quellcomputer **_kopieren_** möchten, **beenden** Sie ihn, und **heben Sie dessen Zuordnung auf**. Klicken Sie im Portal auf **Durchsuchen** > **Virtuelle Computer** oder **Virtuelle Computer (klassisch)** > *Ihr virtueller Computer* > **Beenden**. Für mit dem Resource Manager-Bereitstellungsmodell erstellte virtuelle Computer können Sie auch den Befehl der Azure-Befehlszeilenschnittstelle `azure vm stop <yourResourceGroup> <yourVmName>` gefolgt von `azure vm deallocate <yourResourceGroup> <yourVmName>` verwenden. Beachten Sie, dass der *Status* des virtuellen Computers im Portal von **Wird ausgeführt** in **Beendet (Zuordnung aufgehoben)** geändert wird.
	
	- Wenn Sie Ihren virtuellen Quellcomputer **_migrieren_** möchten, **löschen** Sie den virtuellen Computer, und verwenden Sie die verbliebene VHD. **Suchen** Sie nach dem virtuellen Computer im [Portal](https://portal.azure.com), und klicken Sie auf **Löschen**.
	
1. Suchen Sie den Zugriffsschlüssel für das Speicherkonto, das Ihre Quell-VHD enthält. Weitere Informationen zu Zugriffsschlüsseln finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md).

	- Wenn Ihre Quell-VM mit dem klassischen Bereitstellungsmodell erstellt wurde, klicken Sie auf **Durchsuchen** > **Speicherkonten (klassisch)** > *Ihr Speicherkonto* > **Alle Einstellungen** > **Schlüssel**, und kopieren Sie den Schlüssel mit der Bezeichnung **PRIMÄRER ZUGRIFFSSCHLÜSSEL**. Oder wechseln Sie in der Azure-Befehlszeilenschnittstelle mit `azure config mode asm` zum klassischen Modus, und verwenden Sie dann `azure storage account keys list <yourSourceStorageAccountName>`.

	- Für einen mit dem Resource Manager-Bereitstellungsmodell erstellten virtuellen Computer klicken Sie auf **Durchsuchen** > **Speicherkonten** > *Ihr Speicherkonto* > **Alle Einstellungen** > **Zugriffsschlüssel**, und kopieren Sie den Text mit der Bezeichnung **key1**. Oder stellen Sie in der Azure-Befehlszeilenschnittstelle mit `azure config mode arm` sicher, dass Sie im Resource Manager-Modus sind, und verwenden Sie dann `azure storage account keys list -g <yourDestinationResourceGroup> <yourDestinationStorageAccount>`.

1. Kopieren Sie die VHD-Dateien mit den [Befehlen der Azure-Befehlszeilenschnittstelle für Speicher](../storage/storage-azure-cli.md), wie in den folgenden Schritten beschrieben. Wenn Sie über die Benutzeroberfläche die gleichen Ergebnisse erzielen möchten, können stattdessen auch den [Microsoft Azure Storage Explorer](http://storageexplorer.com/) verwenden. </br>
	1. Richten Sie die Verbindungszeichenfolge für das Zielspeicherkonto ein. Diese Verbindungszeichenfolge enthält den Zugriffsschlüssel für dieses Speicherkonto.
	
			$azure storage account connectionstring show -g <yourDestinationResourceGroup> <yourDestinationStorageAccount>
			$export AZURE_STORAGE_CONNECTION_STRING=<the_connectionstring_output_from_above_command>
	
	2. Erstellen Sie eine [Shared Access Signature](../storage/storage-dotnet-shared-access-signature-part-1.md) (SAS) für die VHD-Datei im Quellspeicherkonto. Notieren Sie sich die Ausgabe für die **SAS-URL** des folgenden Befehls.
	
			$azure storage blob sas create  --account-name <yourSourceStorageAccountName> --account-key <SourceStorageAccessKey> --container <SourceStorageContainerName> --blob <FileNameOfTheVHDtoCopy> --permissions "r" --expiry <mm/dd/yyyy_when_you_want_theSAS_to_expire>
	
	3. Verwenden Sie den folgenden Befehl, um die VHD-Datei aus dem Quellspeicher zum Ziel zu kopieren.
	
			$azure storage blob copy start <SharedAccessURL_ofTheSourceVHD> <DestinationContainerName>
	
	4. Die VHD-Datei wird asynchron kopiert. Sie können den Fortschritt mithilfe des folgenden Befehls überprüfen.
	
			$azure storage blob copy show <DestinationContainerName> <FileNameOfTheVHDtoCopy>
		
</br>

>[AZURE.NOTE] Sie müssen die Datenträger für das Betriebssystem und die Daten getrennt kopieren, wie oben beschrieben.


## Erstellen eines virtuellen Computers mit der kopierten VHD

Diese Schritte zeigen, wie Sie die Azure-Befehlszeilenschnittstelle verwenden, um eine auf Resource Manager basierende Linux-VM in einem neuen virtuellen Netzwerk mit der in den obigen Schritten kopierten VHD zu erstellen. Die VHD und der neu zu erstellende virtuelle Computer müssen sich im gleichen Speicherkonto befinden.


Richten Sie zuerst ein virtuelles Netzwerk und eine Netzwerkschnittstellenkarte (Network Interface Card, NIC) für den neuen virtuellen Computer mit einem Skript wie dem folgenden ein. Verwenden Sie für die Variablen Werte, die für Ihre Anwendung geeignet sind.

	$azure network vnet create <yourResourceGroup> <yourVnetName> -l <yourLocation>

	$azure network vnet subnet create <yourResourceGroup> <yourVnetName> <yourSubnetName>

	$azure network public-ip create <yourResourceGroup> <yourIpName> -l <yourLocation>

	$azure network nic create <yourResourceGroup> <yourNicName> -k <yourSubnetName> -m <yourVnetName> -p <yourIpName> -l <yourLocation>


Erstellen Sie jetzt mit dem folgenden Befehl den neuen virtuellen Computer mit den kopierten VHDs. </br>

	$azure vm create -g <yourResourceGroup> -n <yourVmName> -f <yourNicName> -d <UriOfYourOsDisk> -x <UriOfYourDataDisk> -e <DataDiskSizeGB> -Y -l <yourLocation> -y Linux -z "Standard_A1" -o <DestinationStorageAccountName> -R <DestinationStorageAccountBlobContainer>

	
Die URLs der Daten- und Betriebssystemdatenträger sehen wie folgt aus: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Sie können dies im Portal herausfinden, indem Sie zum Speichercontainer wechseln, auf die kopierte Betriebssystem- oder Daten-VHD klicken und dann den Inhalt der **URL** kopieren.
	
	
War dieser Befehl erfolgreich, erscheint eine Ausgabe wie die folgende:

	$azure vm create -g "testcopyRG" -n "redhatcopy" -f "LinCopyNic" -d https://testcopystore.blob.core.windows.net/testcopyblob/RedHat201631816334.vhd -x https://testcopystore.blob.core.windows.net/testcopyblob/RedHat-data.vhd -e 10 -Y -l "West US" -y Linux -z "Standard_A1" -o "testcopystore" -R "testcopyblob"
	info:    Executing command vm create
	+ Looking up the VM "redhatcopy"
	info:    Using the VM Size "Standard_A1"
	+ Looking up the NIC "LinCopyNic"
	info:    Found an existing NIC "LinCopyNic"
	info:    Found an IP configuration with virtual network subnet id "/subscriptions/b8e6a92b-d6b7-4dbb-87a8-3c8dfe248156/resourceGroups/testcopyRG/providers/Microsoft.Network/virtualNetworks/LinCopyVnet/subnets/LinCopySub" in the NIC "LinCopyNic"
	info:    This NIC IP configuration has a public ip already configured "/subscriptions/b8e6a92b-d6b7-4dbb-87a8-3c8dfe248156/resourcegroups/testcopyrg/providers/microsoft.network/publicipaddresses/lincopyip", any public ip parameters if provided, will be ignored.
	+ Looking up the storage account testcopystore
	info:    The storage URI 'https://testcopystore.blob.core.windows.net/' will be used for boot diagnostics settings, and it can be overwritten by the parameter input of '--boot-diagnostics-storage-uri'.
	+ Creating VM "redhatcopy"
	info:    vm create command OK

Der neu erstellte virtuelle Computer sollte im [Azure-Portal](https://portal.azure.com) unter **Durchsuchen** > **Virtuelle Computer** angezeigt werden.

Stellen Sie eine Verbindung mit Ihrem neuen virtuellen Computer mit einem SSH-Client Ihrer Wahl her, und verwenden Sie die Kontoanmeldeinformationen des ursprünglichen virtuellen Computers, z.B. `ssh OldAdminUser@<IPaddressOfYourNewVM>`. Weitere Informationen über SSH-Verbindungen mit einem virtuellen Linux-Computer finden Sie unter [Verwenden von SSH mit Linux und Mac in Azure](virtual-machines-linux-ssh-from-linux.md).


## Nächste Schritte

Informationen zur Verwendung der Azure-Befehlszeilenschnittstelle, um den neuen virtuellen Computer zu verwalten, finden Sie unter [Azure-CLI-Befehle im Azure Resource Manager-Modus](azure-cli-arm-commands.md).

<!---HONumber=AcomDC_0504_2016-->