---
title: Erstellen einer Kopie einer spezialisierten VM in Azure | Microsoft Docs
description: "Erfahren Sie, wie Sie eine Kopie einer in Azure ausgeführten virtuellen Windows-VM im Resource Manager-Bereitstellungsmodell erstellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: ce7e6cd3-6a4a-4fab-bf66-52f699b1398a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 1e897d98f561b24eafb95dfb620c3b5c7622ba0a
ms.lasthandoff: 03/31/2017


---
# <a name="create-a-copy-of-a-specialized-windows-vm-running-in-azure"></a>Erstellen einer Kopie einer in Azure ausgeführten spezialisierten Windows-VM
In diesem Artikel erfahren Sie, wie Sie das AZCopy-Tool verwenden, um eine Kopie der VHD aus einer spezialisierten Windows-VM zu erstellen, die in Azure ausgeführt wird. Die Kopie der VHD können Sie dann dazu verwenden, eine neue VM zu erstellen. 

* Informationen zum Kopieren einer generalisierten VM finden Sie unter [Erstellen eines VM-Image aus einer vorhandenen Azure-VM](capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Informationen zum Hochladen einer VHD aus einer lokalen, wie eine mithilfe von Hyper-V erstellte, VM finden Sie unter [Hochladen eines Windows-VM-Images an Azure für Resource Manager-Bereitstellungen](upload-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="before-you-begin"></a>Voraussetzungen
Stellen Sie sicher, dass Sie:

* über Informationen zu den **Quell- und Zielspeicherkonten** verfügen. Für die Quell-VM benötigen Sie den Speicherkonto- und den Containernamen. Der Containername ist üblicherweise **vhds**. Außerdem müssen Sie über ein Zielspeicherkonto verfügen. Wenn Sie nicht bereits über eines verfügen, können Sie entweder mithilfe des Portals (**Weitere Dienste** > „Speicherkonten“ > „Hinzufügen“) oder des Cmdlets [New-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607148.aspx) eines erstellen. 
* Azure [PowerShell 1.0](/powershell/azureps-cmdlets-docs) (oder höher) installiert haben.
* das [AzCopy-Tool](../../storage/storage-use-azcopy.md) heruntergeladen und installiert haben. 

## <a name="deallocate-the-vm"></a>Aufheben der Zuordnung der VM
Heben Sie die Zuordnung der VM auf, wodurch die VHD zum Kopieren freigegeben wird. 

* **Portal**: Klicken Sie auf **Virtuelle Computer** > **myVM** > „Beenden“
* **PowerShell**: `Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM` hebt die Zuordnung der VM mit dem Namen **myVM** in der Ressourcengruppe **myResourceGroup** auf.

Der **Status** der VM im Azure-Portal ändert sich von **Beendet** in **Beendet (Zuordnung aufgehoben)**.

## <a name="get-the-storage-account-urls"></a>Abrufen der Speicherkonto-URLs
Sie benötigen die URLs der Quell- und Zielspeicherkonten. Die URLs sehen wie folgt aus: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Wenn Sie die Namen des Speicherkontos und des Containers bereits kennen, können Sie einfach die Informationen in Klammern ersetzen, um Ihre URL zu erstellen. 

Sie können das Azure-Portal oder Azure PowerShell verwenden, um die URL abzurufen:

* **Portal**: Klicken Sie auf **Weitere Dienste** > **Speicherkonten** > <storage account> **Blobs**, und Ihre VHD-Quelldatei befindet sich wahrscheinlich im Container **vhds**. Klicken Sie für den Container auf **Eigenschaften**, und kopieren Sie den Text mit der Bezeichnung **URL**. Sie benötigen die URLs des Quell- und des Zielcontainers. 
* **PowerShell**: `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` ruft die Informationen für die VM namens **myVM** in der Ressourcengruppe **myResourceGroup** ab. Suchen Sie in den Ergebnissen im Abschnitt **Speicherprofil** nach dem **VHD-URI**. Der erste Teil des URI ist die URL zum Container, und der zweite Teil ist der Betriebssystem-VHD-Name für die VM.

## <a name="get-the-storage-access-keys"></a>Abrufen der Speicherzugriffsschlüssel
Suchen Sie die Zugriffsschlüssel für die Quell- und Zielspeicherkonten. Weitere Informationen zu Zugriffsschlüsseln finden Sie unter [Informationen zu Azure-Speicherkonten](../../storage/storage-create-storage-account.md).

* **Portal**: Klicken Sie auf **Weitere Dienste** > **Speicherkonten** > <storage account> **Alle Einstellungen** > **Zugriffsschlüssel**. Kopieren Sie den Schlüssel mit der Bezeichnung **key1**.
* **PowerShell**: `Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup` ruft den Speicherschlüssel für das Speicherkonto **mystorageaccount** in der Ressourcengruppe **myResourceGroup** ab. Kopieren Sie den Schlüssel mit der Bezeichnung **key1**.

## <a name="copy-the-vhd"></a>Kopieren der VHD-Datei
Sie können Dateien mithilfe von AzCopy zwischen Speicherkonten kopieren. Für den Zielcontainer wird der angegebene Container für Sie erstellt, wenn er nicht vorhanden ist. 

Um AzCopy zu verwenden, öffnen Sie auf Ihrem lokalen Computer eine Eingabeaufforderung, und navigieren Sie zum Ordner, in dem AzCopy installiert ist. Dies ist ein ähnlicher Ordner wie *C:\Programme (x86)\Microsoft SDKs\Azure\AzCopy*. 

Um alle Dateien innerhalb eines Containers zu kopieren, verwenden Sie den Schalter **/S**. Dies kann verwendet werden, um die Betriebssystem-VHD und alle Datenträger zu kopieren, wenn sie sich im gleichen Container befinden. Dieses Beispiel zeigt, wie alle Dateien aus dem Container **mysourcecontainer** im Speicherkonto **mysourcestorageaccount** in den Container **mydestinationcontainer** im Speicherkonto **mydestinationstorageaccount** kopiert werden. Ersetzen Sie die Namen der Speicherkonten und Container durch Ihre eigenen. Ersetzen Sie `<sourceStorageAccountKey1>` und `<destinationStorageAccountKey1>` durch Ihre eigenen Schlüssel.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Wenn Sie nur eine bestimmte VHD in einem Container mit mehreren Dateien kopieren möchten, können Sie den Dateinamen auch mithilfe des Schalters „/Pattern“ angeben. In diesem Beispiel wird nur die Datei mit dem Namen **myFileName.vhd** kopiert.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


Wenn der Vorgang abgeschlossen ist, erhalten Sie eine Meldung ähnlich der folgenden:

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

## <a name="troubleshooting"></a>Problembehandlung
* Wenn Sie AzCopy verwenden und Ihnen die Fehlermeldung „Fehler beim Authentifizieren der Anforderung durch den Server“ angezeigt wird, stellen Sie sicher, dass der Wert des Autorisierungsheaders, einschließlich der Signatur, richtig ist. Und wenn Sie Schlüssel 2 oder den sekundären Speicherschlüssel verwenden, versuchen Sie es mit dem primären oder dem ersten Speicherschlüssel.

## <a name="next-steps"></a>Nächste Schritte
* Sie können eine neue VM erstellen, indem Sie [die Kopie der VHD als Betriebssystem-Datenträger an eine VM anfügen](create-vm-specialized.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


