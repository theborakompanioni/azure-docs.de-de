---
title: Hochladen von VHD-Dateien nach Azure DevTest Labs mit AzCopy | Microsoft Docs
description: Hochladen von VHD-Dateien in das Lab-Speicherkonto mit AzCopy
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: cc14f7747c4a24acea434f62b7615d10819bd619
ms.openlocfilehash: f79e76d2efbd71cdd828dbdf43353076c063f23b


---

# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a>Hochladen von VHD-Dateien in das Lab-Speicherkonto mit AzCopy

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

In Azure DevTest Labs können VHD-Dateien verwendet werden, um benutzerdefinierte Images zu erstellen. Diese können wiederum verwendet werden, um virtuelle Computer bereitzustellen. Die folgenden Schritte führen Sie durch die Verwendung des Befehlszeilenprogramms AzCopy für das Hochladen einer VHD-Datei in das Speicherkonto eines Labs. Nachdem Sie die VHD-Datei hochgeladen haben, finden Sie im Abschnitt [Nächste Schritte](#next-steps) einige Artikel, die veranschaulichen, wie aus der hochgeladenen VHD-Datei ein benutzerdefiniertes Image erstellt wird. Ausführlichere Informationen zu Datenträgern und VHDs in Azure finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](../storage/storage-about-disks-and-vhds-linux.md).

> [!NOTE] 
>  
> Das Befehlszeilenprogramm AzCopy steht nur unter Windows zur Verfügung.

## <a name="step-by-step-instructions"></a>Schrittweise Anweisungen

Die folgenden Schritte führen Sie durch das Hochladen einer VHD-Datei nach Azure DevTest Labs mit [AzCopy](http://aka.ms/downloadazcopy). 

1. Rufen Sie den Namen des Lab-Speicherkontos im Azure-Portal ab:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.

1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs**.

1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.  

1. Wählen Sie auf dem Blatt des Labs **Konfiguration**aus. 

1. Wählen Sie auf dem Blatt **Konfiguration** für das Lab die Option **Benutzerdefinierte Images (VHDs)** aus.

1. Wählen Sie auf dem Blatt **Benutzerdefinierte Images** die Option **+ Hinzufügen** aus. 

1. Wählen Sie auf dem Blatt **Benutzerdefinierte Images** die Option **VHD** aus.

1. Wählen Sie auf dem Blatt **VHD** die Option **Hochladen einer VHD mithilfe von PowerShell** aus.

    ![Hochladen der VHD mithilfe von PowerShell](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. Auf dem Blatt **Upload an image using PowerShell** (Ein Abbild mit PowerShell hochladen) wird ein Aufruf des Cmdlets **Add-AzureVhd** angezeigt. Der erste Parameter (*Ziel*) enthält den URI für einen Blobcontainer (*uploads*) im folgenden Format:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Notieren Sie sich den vollständigen URI, da Sie ihn in späteren Schritten verwenden.

1. Laden Sie die VHD-Datei mit AzCopy hoch:
 
1. [Laden Sie die aktuelle Version von AzCopy herunter, und installieren Sie sie](http://aka.ms/downloadazcopy).

1. Öffnen Sie ein Befehlsfenster, und navigieren Sie zum Installationsverzeichnis von AzCopy. Sie können den Speicherort für die AzCopy-Installation auch dem Systempfad hinzufügen. AzCopy wird standardmäßig im folgenden Verzeichnis installiert:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Führen Sie unter Verwendung des Speicherkontoschlüssels und des Blobcontainer-URI den folgenden Befehl an der Eingabeaufforderung aus. Der Wert von *vhdFileName* muss in Anführungszeichen eingeschlossen werden. Der Vorgang zum Hochladen kann abhängig von der Größe der VHD-Datei und der Geschwindigkeit Ihrer Verbindung lange dauern.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Nächste Schritte

- [Erstellen eines benutzerdefinierten Images in Azure DevTest Labs aus einer VHD-Datei mit dem Azure-Portal](devtest-lab-create-template.md)
- [Erstellen eines benutzerdefinierten Azure DevTest Labs-Images aus einer VHD-Datei mithilfe von PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)


<!--HONumber=Feb17_HO3-->


