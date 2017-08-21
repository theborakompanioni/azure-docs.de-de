---
title: "Beheben von Fehlern beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs | Microsoft-Dokumentation"
description: "Beheben von Fehlern beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs"
services: storage
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 318d7146ea53a806baf813ff7de2fe91f18becc8
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds"></a>Beheben von Fehlern beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs

Beim Löschen von Azure-Speicherkonten, -Containern oder -VDHs (Virtual Hard Disk, virtuellen Festplatten) im [Azure-Portal](https://portal.azure.com) können Fehler auftreten. Dieser Artikel enthält Anleitungen zum Beheben von Problemen in einer Azure Resource Manager-Bereitstellung.

Wenn in diesem Artikel Ihr Problem mit Azure nicht beschrieben ist, besuchen Sie die Azure-Foren unter [MSDN-Foren und Stack Overflow](https://azure.microsoft.com/support/forums/). Sie können Ihr Problem in diesen Foren veröffentlichen oder auf Twitter an @AzureSupport senden. Darüber hinaus können Sie eine Azure-Supportanfrage stellen, indem Sie auf der Website des **Azure-Supports** die Option [Support erhalten](https://azure.microsoft.com/support/options/) auswählen.

## <a name="symptoms"></a>Symptome
### <a name="scenario-1"></a>Szenario 1
Wenn Sie versuchen, eine virtuelle Festplatte in einem Speicherkonto in einer Resource Manager-Bereitstellung zu löschen, wird möglicherweise folgende Fehlermeldung angezeigt:

**Fehler beim Löschen des Blobs 'vhds/BlobName.vhd'. Fehler: Derzeit ist eine Lease für das Blob vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.**

Dieses Problem kann auftreten, wenn ein virtueller Computer über eine Lease für die virtuelle Festplatte verfügt, die Sie löschen möchten.

### <a name="scenario-2"></a>Szenario 2:
Wenn Sie versuchen, einen Container in einem Speicherkonto in einer Resource Manager-Bereitstellung zu löschen, wird möglicherweise folgende Fehlermeldung angezeigt:

**Fehler beim Löschen von Speichercontainer 'vhds'. Fehler: Derzeit ist eine Lease für den Container vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.**

Dieses Problem kann auftreten, wenn der Container über eine virtuelle Festplatte verfügt, die im Leasezustand gesperrt ist.

### <a name="scenario-3"></a>Szenario 3
Wenn Sie versuchen, ein Speicherkonto in einer Resource Manager-Bereitstellung zu löschen, wird möglicherweise folgende Fehlermeldung angezeigt:

**Fehler beim Löschen von Speicherkonto 'StorageAccountName'. Fehler: Das Speicherkonto kann nicht gelöscht werden, da zugehörige Artefakte gerade verwendet werden.**

Dieses Problem kann auftreten, wenn das Speicherkonto über eine virtuelle Festplatte verfügt, die sich im Leasezustand befindet.

## <a name="solution"></a>Lösung 
Um diese Probleme zu beheben, müssen Sie die virtuelle Festplatte, die den Fehler verursacht, sowie den zugehörigen virtuellen Computer ausfindig machen. Trennen Sie anschließend die virtuelle Festplatte vom virtuellen Computer (für Datenträger) oder löschen Sie den virtuellen Computer, der die virtuelle Festplatte (für Betriebssystemdatenträger) verwendet. Dadurch wird die Lease für die virtuelle Festplatte gelöscht, sodass diese gelöscht werden kann. 

Verwenden Sie hierzu eine der folgenden Methoden:

### <a name="method-1---use-azure-storage-explorer"></a>Methode 1: Azure Storage Explorer

### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a>Schritt 1: Bestimmen der VHD, die das Löschen des Speicherkontos verhindert

1. Wenn Sie das Speicherkonto löschen, wird ein Meldungsdialogfeld wie das folgende angezeigt: 

    ![Meldung beim Löschen des Speicherkontos](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Überprüfen Sie die **Datenträger-URL**, um das Speicherkonto und die VHD zu bestimmen, die Sie am Löschen des Speicherkontos hindert. Im folgenden Beispiel ist die Zeichenfolge vor „.blob.core.windows.net“ der Name des Speicherkontos und „SCCM2012-2015-08-28.vhd“ der Name der VHD.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

### <a name="step-2-delete-the-vhd-by-using-azure-storage-explorer"></a>Schritt 2: Löschen der VHD mithilfe des Azure Storage Explorers

1. Laden Sie die neueste [Azure Storage Explorer](http://storageexplorer.com/)-Version herunter, und installieren Sie sie. Dieses Tool ist eine eigenständige App von Microsoft, mit der Sie unter Windows, macOS und Linux mit Azure Storage-Daten arbeiten können.
2. Öffnen Sie den Azure Storage Explorer. Klicken Sie auf der linken Leiste auf das ![Symbol für „Konto“,](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/account.png) wählen Sie Ihre Azure-Umgebung aus, und melden Sie sich dann an.

3. Wählen Sie alle Abonnements, oder das Abonnement mit dem Speicherkonto aus, das Sie löschen möchten.

    ![Abonnement hinzufügen](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/addsub.png)

4. Wechseln Sie zum Speicherkonto, das wir zuvor über die Datenträger-URL abgerufen haben. Wählen Sie **Blobcontainer** > **VHDs** aus, und suchen Sie die VHD, die Sie am Löschen des Speicherkontos hindert.
5. Wenn die VHD gefunden wird, überprüfen Sie die Spalte **VM-Name** auf die VM, die diese VHD verwendet.

    ![Überprüfen der VM](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/check-vm.png)

6. Entfernen Sie im Azure-Portal die Lease von der VHD. Weitere Informationen finden Sie unter [Entfernen der Lease von der VHD](#remove-the-lease-from-the-vhd). 

7. Klicken Sie Azure Storage Explorer mit der rechten Maustaste auf die VHD, und wählen Sie dann „Löschen“ aus.

8. Löschen Sie das Speicherkonto.

### <a name="method-2---use-azure-portal"></a>Methode 2: Azure-Portal 

#### <a name="step-1-identify-the-vhd-that-prevent-deletion-of-the-storage-account"></a>Schritt 1: Bestimmen der VHD, die das Löschen des Speicherkontos verhindert

1. Wenn Sie das Speicherkonto löschen, wird ein Meldungsdialogfeld wie das folgende angezeigt: 

    ![Meldung beim Löschen des Speicherkontos](media/storage-resource-manager-cannot-delete-storage-account-container-vhd/delete-storage-error.png) 

2. Überprüfen Sie die **Datenträger-URL**, um das Speicherkonto und die VHD zu bestimmen, die Sie am Löschen des Speicherkontos hindert. Im folgenden Beispiel ist die Zeichenfolge vor „.blob.core.windows.net“ der Name des Speicherkontos und „SCCM2012-2015-08-28.vhd“ der Name der VHD.  

        https://portalvhds73fmhrw5xkp43.blob.core.windows.net/vhds/SCCM2012-2015-08-28.vhd

2. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
3. Wählen Sie im Menü „Hub“ die Option **Alle Ressourcen** aus. Wechseln Sie zum Speicherkonto, und wählen Sie **Blobs** > **VHDs** aus.

    ![Screenshot des Portals, in dem das Speicherkonto und der Container „vhds“ hervorgehoben sind](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

4. Suchen Sie die VHD, die wir zuvor über die Datenträger-URL bestimmt haben. Ermitteln Sie anschließend, welcher virtuelle Computer die virtuelle Festplatte verwendet. In der Regel können Sie anhand des Namens der virtuellen Festplatte erkennen, auf welchem virtuellen Computer sich die virtuelle Festplatte befindet:

VM im Ressourcen-Manager-Entwicklungsmodell

   * Bei Betriebssystemdatenträgern wird in der Regel folgende Benennungskonvention befolgt: VMName-YYYY-MM-DD-HHMMSS.vhd
   * Bei Datenträgern wird in der Regel folgende Benennungskonvention befolgt: VMName-YYYY-MM-DD-HHMMSS.vhd

VM im klassischen Entwicklungsmodell

   * Bei Betriebssystemdatenträgern wird in der Regel folgende Benennungskonvention befolgt: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd
   * Bei Datenträgern wird in der Regel folgende Benennungskonvention befolgt: CloudServiceName-VMName-YYYY-MM-DD-HHMMSS.vhd

#### <a name="step-2-remove-the-lease-from-the-vhd"></a>Schritt 2: Entfernen der Lease für die virtuelle Festplatte

[Entfernen Sie die Lease von der VHD](#remove-the-lease-from-the-vhd), und löschen Sie dann das Speicherkonto.

## <a name="what-is-a-lease"></a>Was ist eine Lease?
Eine Lease ist eine Sperre, die zum Steuern des Zugriffs auf ein Blob (z.B. eine virtuelle Festplatte) verwendet werden kann. Wenn ein Blob geleast wird, können nur die Besitzer der Lease auf das Blob zugreifen. Eine Lease ist aus den folgenden Gründen wichtig:

* Sie verhindert, dass Daten beschädigt werden, wenn mehrere Besitzer gleichzeitig im gleichen Bereich des Blobs schreiben möchten.
* Sie verhindert, dass das Blob gelöscht wird, wenn es (beispielsweise von einem virtuellen Computer) aktiv verwendet wird.
* Sie verhindert, dass das Speicherkonto gelöscht wird, wenn es (beispielsweise von einem virtuellen Computer) aktiv verwendet wird.

### <a name="remove-the-lease-from-the-vhd"></a>Entfernen der Lease von der VHD
Wenn die VHD ein Betriebssystemdatenträger ist, müssen Sie die VM löschen, um die Lease zu entfernen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Wählen Sie im Menü **Hub** die Option **Virtuelle Computer** aus.
3. Wählen Sie den virtuellen Computer aus, der über eine Lease für die virtuelle Festplatte verfügt.
4. Stellen Sie sicher, dass der virtuelle Computer nicht aktiv verwendet wird und dass Sie den virtuellen Computer nicht mehr benötigen.
5. Wählen Sie oben auf dem Blatt **Details zum virtuellen Computer** die Option **Löschen** aus, und klicken Sie zur Bestätigung auf **Ja**.
6. Die VM sollte nun gelöscht, doch die VHD noch vorhanden sein. Für die virtuelle Festplatte sollte jedoch keine Lease mehr vorhanden sein. Es kann einige Minuten dauern, bis die Lease freigegeben wird. Wenn Sie sicherstellen möchten, dass die Lease freigegeben wurde, klicken Sie auf **Alle Ressourcen** > **Speicherkontoname** > **Blobs** > **vhds**. Im Bereich **Blob-Eigenschaften** sollte für **Leasestatus** der Wert **Nicht gesperrt** angezeigt werden.

Wenn die VHD ein Datenträger ist, trennen Sie die VHD von der VM, um die Lease zu entfernen:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü **Hub** die Option **Virtuelle Computer** aus.
3. Wählen Sie den virtuellen Computer aus, der über eine Lease für die virtuelle Festplatte verfügt.
4. Wählen Sie auf dem Blatt **Details zum virtuellen Computer** die Option **Datenträger** aus.
5. Wählen Sie den Datenträger aus, der über eine Lease für die virtuelle Festplatte verfügt. Anhand der URL der virtuellen Festplatte können Sie erkennen, welche virtuelle Festplatte an den Datenträger angefügt wurde.
6. Stellen Sie unbedingt sicher, dass der Datenträger nicht aktiv verwendet wird.
7. Klicken Sie auf dem Blatt **Datenträgerdetails** auf **Trennen**.
8. Der Datenträger sollte nun vom virtuellen Computer getrennt sein, und die virtuelle Festplatte sollte keine Lease für Datenträger mehr aufweisen. Es kann einige Minuten dauern, bis die Lease freigegeben wird. Wenn Sie sicherstellen möchten, dass die Lease freigegeben wurde, klicken Sie auf **Alle Ressourcen** > **Speicherkontoname** > **Blobs** > **vhds**. Im Bereich **Blob-Eigenschaften** sollte für **Leasestatus** der Wert **Nicht gesperrt** angezeigt werden.

## <a name="next-steps"></a>Nächste Schritte
* [Löschen eines Speicherkontos](storage-create-storage-account.md#delete-a-storage-account)
* [How to break the locked lease of blob storage in Microsoft Azure (PowerShell) (in englischer Sprache)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)

