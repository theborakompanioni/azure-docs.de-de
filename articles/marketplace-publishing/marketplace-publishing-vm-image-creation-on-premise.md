---
title: "Erstellen eines lokalen Images eines virtuellen Computers für den Azure Marketplace | Microsoft Docs"
description: "Informationen über die Schritte zum Erstellen eines lokalen Images eines virtuellen Computers und zur Bereitstellung für den Kauf im Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 26dfbd5a-8685-4b19-987e-c20ca60540ec
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 04/29/2016
ms.author: hascipio; v-divte
translationtype: Human Translation
ms.sourcegitcommit: 3136b8345d0c851c29a9498089da73c8564549d1
ms.openlocfilehash: 101d0b2733a43b99b32efae8a9510ffca2a2e467
ms.lasthandoff: 01/31/2017


---
# <a name="develop-an-on-premises-virtual-machine-image-for-the-azure-marketplace"></a>Entwickeln eines lokalen Images eines virtuellen Computers für Azure Marketplace
Es wird dringend empfohlen, virtuelle Azure-Festplatten (Azure-VHDs) direkt in der Cloud mithilfe des Remotedesktopprotokolls zu entwickeln. Bei Bedarf ist es jedoch auch möglich, eine VHD herunterzuladen und mithilfe lokaler Infrastruktur zu entwickeln.  

Für die lokale Entwicklung müssen Sie die Betriebssystem-VHD des erstellten virtuellen Computers herunterladen. Diese Schritte werden als Teil von Schritt 3.3 oben ausgeführt.  

## <a name="download-a-vhd-image"></a>Herunterladen eines VHD-Images
### <a name="locate-a-blob-url"></a>Suchen einer Blob-URL
Um die VHD-Datei herunterzuladen, müssen Sie zuerst nach der Blob-URL für den Betriebssystem-Datenträger suchen.

Suchen Sie die Blob-URL im neuen [Microsoft Azure-Portal](https://portal.azure.com):

1. Wechseln Sie zu **Durchsuchen** > **VMs**, und wählen Sie dann den bereitgestellten virtuellen Computer aus.
2. Wählen Sie unter **Konfigurieren** die Kachel **Datenträger** aus. Dadurch wird das Blatt „Datenträger“ geöffnet.
   
   ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img01.png)
3. Wählen Sie den **Betriebssystemdatenträger** aus. Dadurch wird ein anderes Blatt mit Datenträgereigenschaften angezeigt, einschließlich des Speicherorts der VHD-Datei.
4. Kopieren Sie die BLOB-URL.
   
   ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img02.png)
5. Löschen Sie jetzt den bereitgestellten virtuellen Computer, ohne die zugrunde liegenden Datenträger zu löschen. Sie können den virtuellen Computer auch beenden, anstatt ihn zu löschen. Laden Sie die Betriebssystem-VHD-Datei nicht herunter, wenn der virtuelle Computer ausgeführt wird.
   
   ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img03.png)

### <a name="download-a-vhd"></a>Herunterladen einer VHD-Datei
Nachdem Sie die Blob-URL kennen, können Sie die virtuelle Festplatte über das [Azure-Portal](http://manage.windowsazure.com/) oder per PowerShell herunterladen.  

> [!NOTE]
> Zum Zeitpunkt der Erstellung dieser Anleitung ist die Funktionalität zum Herunterladen einer VHD-Datei noch nicht im neuen Microsoft Azure-Portal vorhanden.  
> 
> 

**Herunterladen der Betriebssystem-VHD-Datei über das aktuelle [Azure-Portal](http://manage.windowsazure.com/)**

1. Melden Sie sich beim Azure-Portal an, sofern Sie noch nicht angemeldet sind.
2. Klicken Sie auf die Registerkarte **Speicher** .
3. Wählen Sie das Speicherkonto aus, in dem die VHD-Datei gespeichert ist.
   
   ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img04.png)
4. Dadurch werden die Eigenschaften des Speicherkontos angezeigt. Wählen Sie die Registerkarte **Container** aus.
   
   ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img05.png)
5. Wählen Sie den Container, in dem die VHD-Datei gespeichert ist. Standardmäßig wird die VHD-Datei beim Erstellen über das Portal in einem virtuellen Festplattencontainer gespeichert.
   
   ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img06.png)
6. Wählen Sie die VHD-Datei mit dem richtigen Betriebssystem aus, indem Sie die URL mit der gespeicherten URL vergleichen.
7. Klicken Sie auf **Download**.
   
   ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img07.png)

### <a name="download-a-vhd-by-using-powershell"></a>Herunterladen einer VHD-Datei mithilfe von PowerShell
Zusätzlich zur Verwendung des Azure-Portals können Sie das Cmdlet [Save-AzureVhd](http://msdn.microsoft.com/library/dn495297.aspx) zum Herunterladen der Betriebssystem-VHD-Datei verwenden.

        Save-AzureVhd –Source <storageURIOfVhd> `
        -LocalFilePath <diskLocationOnWorkstation> `
        -StorageKey <keyForStorageAccount>
Beispiel: Save-AzureVhd -Source „https://baseimagevm.blob.core.windows.net/vhds/BaseImageVM-6820cq00-BaseImageVM-os-1411003770191.vhd“ -LocalFilePath „C:\Users\Administrator\Desktop\baseimagevm.vhd“ -StorageKey <String>

> [!NOTE]
> **Save-AzureVhd** verfügt auch über eine **NumberOfThreads**-Option, mit der die Parallelität so erhöht wird, dass die verfügbare Bandbreite optimal für den Download eingesetzt wird.
> 
> 

## <a name="upload-vhds-to-an-azure-storage-account"></a>Hochladen von VHD-Dateien in ein Azure-Speicherkonto
Wenn Sie die virtuellen Festplatten lokal vorbereitet haben, müssen Sie sie in ein Speicherkonto in Azure hochladen. Dieser Schritt erfolgt, nachdem die VHD-Datei lokal erstellt wurde, jedoch bevor eine Zertifizierung für das VM-Image abgerufen wird.

### <a name="create-a-storage-account-and-container"></a>Erstellen eines Speicherkontos und Containers
Es wird empfohlen, virtuelle Festplatten in ein Speicherkonto in einer Region in den USA hochzuladen. Alle virtuellen Festplatten für eine einzelne SKU sollten in einem einzelnen Container innerhalb eines einzelnen Speicherkontos platziert werden.

Um ein Speicherkonto zu erstellen, können Sie das [Microsoft Azure-Portal](https://portal.azure.com/), PowerShell oder das Linux-Befehlszeilentool verwenden.  

**Erstellen eines Speicherkontos im Microsoft Azure-Portal**

1. Klicken Sie auf **Neu**.
2. Wählen Sie **Speicher**.
3. Geben Sie den Namen des Speicherkontos ein, und wählen Sie dann einen Speicherort aus.
   
   ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img08.png)
4. Klicken Sie auf **Erstellen**.
5. Das Blatt für das erstellte Speicherkonto sollte geöffnet werden. Wählen Sie andernfalls **Durchsuchen** > **Speicherkonten** aus. Wählen Sie auf dem Blatt „Speicherkonto“ das erstellte Speicherkonto aus.
6. Wählen Sie **Container**aus.
   
   ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img09.png) 
7. Wählen Sie auf dem Blatt „Container“ die Option **Hinzufügen**aus, und geben Sie dann einen Containernamen und die Berechtigungen für den Container ein. Wählen Sie **Privat** als Containerberechtigung aus.

> [!TIP]
> Es wird empfohlen, einen Container pro SKU zu erstellen, die Sie veröffentlichen möchten.
> 
> 

  ![Abbildung](media/marketplace-publishing-vm-image-creation-on-premise/img10.png)

### <a name="create-a-storage-account-by-using-powershell"></a>Erstellen eines Speicherkontos mithilfe von PowerShell
Erstellen Sie per PowerShell mit dem Cmdlet [New-AzureStorageAccount](http://msdn.microsoft.com/library/dn495115.aspx) ein Speicherkonto.

        New-AzureStorageAccount -StorageAccountName “mystorageaccount” -Location “West US”

Erstellen Sie anschließend mit dem Cmdlet [NewAzureStorageContainer](http://msdn.microsoft.com/library/dn495291.aspx) einen Container in diesem Speicherkonto.

        New-AzureStorageContainer -Name “containername” -Permission “Off”

> [!NOTE]
> Bei diesen Befehlen wird davon ausgegangen, dass der aktuelle Kontext des Speicherkontos in PowerShell bereits festgelegt wurde.   Weitere Informationen zur Einrichtung von PowerShell finden Sie unter [Einrichten von Azure PowerShell](marketplace-publishing-powershell-setup.md) .  
> 
> ### <a name="create-a-storage-account-by-using-the-command-line-tool-for-mac-and-linux"></a>Erstellen eines Speicherkontos mit dem Befehlszeilentool für Mac und Linux
> Erstellen Sie wie folgt über das [Linux-Befehlszeilentool](../virtual-machines/virtual-machines-linux-cli-manage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)ein Speicherkonto.
> 
> 

        azure storage account create mystorageaccount --location "West US"

Erstellen Sie wie folgt einen Container.

        azure storage container create containername --account-name mystorageaccount --accountkey <accountKey>

## <a name="upload-a-vhd"></a>Hochladen einer VHD-Datei
Nachdem das Speicherkonto und der Container erstellt wurden, können Sie Ihre vorbereiteten virtuellen Festplatten hochladen. Dazu können Sie PowerShell, das Befehlszeilentool für Linux oder andere Azure-Speicherverwaltungstools verwenden.

### <a name="upload-a-vhd-via-powershell"></a>Hochladen einer virtuellen Festplatte über PowerShell
Verwenden Sie das Cmdlet [Add-AzureVhd](http://msdn.microsoft.com/library/dn495173.aspx) .

        Add-AzureVhd –Destination “http://mystorageaccount.blob.core.windows.net/containername/vmsku.vhd” -LocalFilePath “C:\Users\Administrator\Desktop\vmsku.vhd”

### <a name="upload-a-vhd-by-using-the-command-line-tool-for-mac-and-linux"></a>Hochladen einer virtuellen Festplatte über das Befehlszeilentool für Mac und Linux
Verwenden Sie folgenden Befehl im [Linux-Befehlszeilentool](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2): azure vm image create <image name> --location <Location of the data center> --OS Linux <LocationOfLocalVHD>

## <a name="see-also"></a>Weitere Informationen
* [Erstellen eines Images eines virtuellen Computers für den Marketplace](marketplace-publishing-vm-image-creation.md)
* [Einrichten von Azure PowerShell](marketplace-publishing-powershell-setup.md)


