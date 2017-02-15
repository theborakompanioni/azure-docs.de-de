---
title: "Beheben von Fehlern beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs in einer Resource Manager-Bereitstellung | Microsoft Docs"
description: "Beheben von Fehlern beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs in einer Resource Manager-Bereitstellung"
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: na
tags: storage
ms.assetid: 17403aa1-fe8d-45ec-bc33-2c0b61126286
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 75d5e82e2f4e747747f24376239e23f6512f916a


---
# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds-in-a-resource-manager-deployment"></a>Beheben von Fehlern beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs in einer Resource Manager-Bereitstellung
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

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

### <a name="step-1-identify-the-problem-vhd-and-the-associated-vm"></a>Schritt 1: Ausfindigmachen der virtuellen Festplatte, die das Problem verursacht, sowie des zugehörigen virtuellen Computers
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü **Hub** die Option **Alle Ressourcen** aus. Wechseln Sie zu dem Speicherkonto, das Sie löschen möchten, und wählen Sie **Blobs** > **vhds** aus.

    ![Screenshot des Portals, in dem das Speicherkonto und der Container „vhds“ hervorgehoben sind](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)
3. Überprüfen Sie die Eigenschaften für jede virtuelle Festplatte im Container. Machen Sie die virtuelle Festplatte ausfindig, die sich im Zustand **Geleast** befindet. Ermitteln Sie anschließend, welcher virtuelle Computer die virtuelle Festplatte verwendet. In der Regel können Sie anhand des Namens der virtuellen Festplatte erkennen, auf welchem virtuellen Computer sich die virtuelle Festplatte befindet:

   * Bei Betriebssystemdatenträgern wird in der Regel folgende Benennungskonvention angewendet: VMNameYYYYMMDDHHMMSS.vhd
   * Bei Datenträgern wird in der Regel folgende Benennungskonvention angewendet: VMName-YYYYMMDD-HHMMSS.vhd

     ![Screenshot der Containerinformationen im Portal; der Name des virtuellen Computers, der Leasestatus „Gesperrt“ und der Leasezustand „Geleast“ sind hervorgehoben](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/locatevm.png)

### <a name="step-2-remove-the-lease-from-the-vhd"></a>Schritt 2: Entfernen der Lease für die virtuelle Festplatte
Gehen Sie wie folgt vor, um den virtuellen Computer zu löschen, der die virtuelle Festplatte (für Betriebssystemdatenträger) verwendet:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü **Hub** die Option **Virtuelle Computer** aus.
3. Wählen Sie den virtuellen Computer aus, der über eine Lease für die virtuelle Festplatte verfügt.
4. Stellen Sie sicher, dass der virtuelle Computer nicht aktiv verwendet wird und dass Sie den virtuellen Computer nicht mehr benötigen.
5. Wählen Sie oben auf dem Blatt **Details zum virtuellen Computer** die Option **Löschen** aus, und klicken Sie zur Bestätigung auf **Ja**.
6. Der virtuelle Computer sollte nun gelöscht sein, die virtuelle Festplatte sollte jedoch noch vorhanden sein. Für die virtuelle Festplatte sollte jedoch keine Lease mehr vorhanden sein. Es kann einige Minuten dauern, bis die Lease freigegeben wird. Wenn Sie sicherstellen möchten, dass die Lease freigegeben wurde, klicken Sie auf **Alle Ressourcen** > **Speicherkontoname** > **Blobs** > **vhds**. Im Bereich **Blob-Eigenschaften** sollte für **Leasestatus** der Wert **Nicht gesperrt** angezeigt werden.

Gehen Sie wie folgt vor, um die virtuelle Festplatte von dem virtuellen Computer zu trennen, von dem die virtuelle Festplatte (für Datenträger) verwendet wird:

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü **Hub** die Option **Virtuelle Computer** aus.
3. Wählen Sie den virtuellen Computer aus, der über eine Lease für die virtuelle Festplatte verfügt.
4. Wählen Sie auf dem Blatt **Details zum virtuellen Computer** die Option **Datenträger** aus.
5. Wählen Sie den Datenträger aus, der über eine Lease für die virtuelle Festplatte verfügt. Anhand der URL der virtuellen Festplatte können Sie erkennen, welche virtuelle Festplatte an den Datenträger angefügt wurde.
6. Stellen Sie unbedingt sicher, dass der Datenträger nicht aktiv verwendet wird.
7. Klicken Sie auf dem Blatt **Datenträgerdetails** auf **Trennen**.
8. Der Datenträger sollte nun vom virtuellen Computer getrennt sein, und die virtuelle Festplatte sollte keine Lease für Datenträger mehr aufweisen. Es kann einige Minuten dauern, bis die Lease freigegeben wird. Wenn Sie sicherstellen möchten, dass die Lease freigegeben wurde, klicken Sie auf **Alle Ressourcen** > **Speicherkontoname** > **Blobs** > **vhds**. Im Bereich **Blob-Eigenschaften** sollte für **Leasestatus** der Wert **Nicht gesperrt** angezeigt werden.

## <a name="what-is-a-lease"></a>Was ist eine Lease?
Eine Lease ist eine Sperre, die zum Steuern des Zugriffs auf ein Blob (z.B. eine virtuelle Festplatte) verwendet werden kann. Wenn ein Blob geleast wird, können nur die Besitzer der Lease auf das Blob zugreifen. Eine Lease ist aus den folgenden Gründen wichtig:

* Sie verhindert, dass Daten beschädigt werden, wenn mehrere Besitzer gleichzeitig im gleichen Bereich des Blobs schreiben möchten.
* Sie verhindert, dass das Blob gelöscht wird, wenn es (beispielsweise von einem virtuellen Computer) aktiv verwendet wird.
* Sie verhindert, dass das Speicherkonto gelöscht wird, wenn es (beispielsweise von einem virtuellen Computer) aktiv verwendet wird.

## <a name="next-steps"></a>Nächste Schritte
* [Löschen eines Speicherkontos](storage-create-storage-account.md#delete-a-storage-account)
* [How to break the locked lease of blob storage in Microsoft Azure (PowerShell) (in englischer Sprache)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)



<!--HONumber=Nov16_HO3-->


