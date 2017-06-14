---
title: "Problembehandlung beim Löschen von Azure Storage-Konten, -Containern oder -VHDs in einer klassischen Bereitstellung | Microsoft Docs"
description: "Problembehandlung beim Löschen von Azure Storage-Konten, -Containern oder -VHDs in einer klassischen Bereitstellung"
services: storage
documentationcenter: 
author: genlin
manager: felixwu
editor: tysonn
tags: storage
ms.assetid: 0f7a8243-d8dc-432a-9d37-1272a0cb3a5c
ms.service: storage
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: 64f875581dccd2ad1215a82eba1e2659fe01d191
ms.contentlocale: de-de
ms.lasthandoff: 04/06/2017


---
# <a name="troubleshoot-deleting-azure-storage-accounts-containers-or-vhds-in-a-classic-deployment"></a>Problembehandlung beim Löschen von Azure Storage-Konten, -Containern oder -VHDs in einer klassischen Bereitstellung
[!INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Beim Löschen von Azure Storage-Konten, -Containern oder -VHDs im [Azure-Portal](https://portal.azure.com/) oder im [klassischen Azure-Portal](https://manage.windowsazure.com/) können Fehler auftreten. Die Probleme können durch folgende Umstände verursacht werden:

* Wenn Sie einen virtuellen Computer löschen, werden der Datenträger und die VHD nicht automatisch gelöscht. Dies kann die Ursache für Fehler beim Löschen des Speicherkontos sein. Der Datenträger wird nicht gelöscht, damit Sie den Datenträger verwenden können, um einen anderen virtuellen Computer bereitzustellen.
* Es besteht noch eine Lease für einen Datenträger oder für das dem Datenträger zugeordnete Blob.
* Es gibt immer noch ein VM-Image, das ein Blob, einen Container oder ein Speicherkonto verwendet.

Suchen Sie in den Azure-Foren zu [MSDN und Stack Overflow](https://azure.microsoft.com/support/forums/), falls Sie Ihr Azure-Problem mit diesem Artikel nicht beheben konnten. Sie können Ihr Problem in diesen Foren veröffentlichen oder auf Twitter an @AzureSupport senden. Darüber hinaus können Sie eine Azure-Supportanfrage stellen, indem Sie auf der Website des **Azure-Supports** die Option [Support erhalten](https://azure.microsoft.com/support/options/) auswählen.

## <a name="symptoms"></a>Symptome
Im folgenden Abschnitt werden häufige Fehler aufgeführt, die beim Versuch, Azure-Speicherkonten, -Container oder -VHDs zu löschen, auftreten können:

### <a name="scenario-1-unable-to-delete-a-storage-account"></a>Szenario 1: Speicherkonto kann nicht gelöscht werden
Wenn Sie im [Azure-Portal](https://portal.azure.com/) zum klassischen Speicherkonto navigieren und **löschen** auswählen, wird Ihnen möglicherweise eine Liste mit Objekten angezeigt, die das Löschen des Speicherkontos verhindern:

  ![Abbildung des Fehlers beim Löschen des Speicherkontos](./media/storage-cannot-delete-storage-account-container-vhd/newerror.png)

Wenn Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) zum Speicherkonto navigieren und **Löschen** auswählen, wird möglicherweise eine der folgenden Fehlermeldungen angezeigt:

- *Speicherkonto "StorageAccountName" enthält VM-Images. Stellen Sie sicher, dass diese VM-Images entfernt werden, bevor dieses Speicherkonto gelöscht wird.*

- *Das Speicherkonto <VM-Speicherkontoname> konnte nicht gelöscht werden. Fehler beim Löschen des Speicherkontos <VM-Speicherkontoname>: „Speicherkonto <VM-Speicherkontoname> hat einige aktive Images und/oder Datenträger. Entfernen Sie diese Bilder und/oder Datenträger, bevor Sie dieses Speicherkonto löschen.“*

- *Speicherkonto <VM-Speicherkontoname> hat einige aktive Images und/oder Datenträger, z.B. „xxxxxxxxx- xxxxxxxxx-O-209490240936090599“. Entfernen Sie diese Bilder und/oder Datenträger, bevor Sie dieses Speicherkonto löschen.*

- *Das Speicherkonto <VM-Speicherkontoname> verfügt über 1 Container, der/die über ein aktives Image und/oder Datenträgerartefakte verfügt/verfügen. Stellen Sie sicher, dass diese Artefakte aus dem Image-Repository entfernt werden, bevor Sie dieses Speicherkonto löschen.*

-  *Übermitteln fehlgeschlagen – das Speicherkonto <VM-Speicherkontoname> verfügt über 1 Container, der/die über ein aktives Image und/oder Datenträgerartefakte verfügt/verfügen. Stellen Sie sicher, dass diese Artefakte aus dem Image-Repository entfernt werden, bevor Sie dieses Speicherkonto löschen. Wenn Sie versuchen, ein Speicherkonto zu löschen, und dem Speicherkonto aktive Datenträger zugeordnet sind, werden Sie mit einer Meldung darüber informiert, dass aktive Datenträger vorhanden sind, die gelöscht werden müssen.*

### <a name="scenario-2-unable-to-delete-a-container"></a>Szenario 2: Container kann nicht gelöscht werden
Wenn Sie versuchen, den Speichercontainer zu löschen, wird u. U die folgende Fehlermeldung angezeigt:

*Fehler beim Löschen des Speichercontainers <container name>. Fehler: Derzeit ist eine Lease für den Container vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.*

oder

*Die folgenden virtuellen Computerfestplatten verwenden derzeit Blobs in diesem Container, sodass der Container nicht gelöscht werden kann: VirtualMachineDiskName1, VirtualMachineDiskName2, ...*

### <a name="scenario-3-unable-to-delete-a-vhd"></a>Szenario 3: VHD kann nicht gelöscht werden
Nachdem Sie einen virtuellen Computer gelöscht haben und dann versuchen, die Blobs für die zugeordneten VHDs zu löschen, wird möglicherweise die folgende Meldung angezeigt:

*Fehler beim Löschen von Blob „Pfad/XXXXXX-XXXXXX-os-1447379084699.vhd“. Fehler: Derzeit ist eine Lease für das Blob vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.*

oder

*Blob „BlobName.vhd“ wird als virtuelle Computerfestplatte „VirtualMachineDiskName“ verwendet, sodass das Blob nicht gelöscht werden kann.*

## <a name="solution"></a>Lösung
Um die häufigsten Probleme zu beheben, verwenden Sie die folgende Methode:

### <a name="step-1-delete-any-disks-that-are-preventing-deletion-of-the-storage-account-container-or-vhd"></a>Schritt 1: Löschen Sie alle Datenträger, die das Löschen des Speicherkontos, des Speichercontainers oder der Speicher-VHD verhindern.
1. Wechseln Sie zum [klassischen Azure-Portal](https://manage.windowsazure.com/).
2. Wählen Sie **VIRTUELLER COMPUTER** > **DATENTRÄGER** aus.

    ![Abbildung von Datenträgern auf virtuellen Computern im klassischen Azure-Portal](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)
3. Suchen Sie die Datenträger, die dem zu löschenden Speicherkonto oder Container bzw. der zu löschenden VHD zugeordnet sind. Wenn Sie den Speicherort des Datenträgers überprüfen, finden Sie das zugeordnete Speicherkonto, den Container oder die VHD.

    ![Abbildung, die Speicherortinformationen für Datenträger im klassischen Azure-Portal zeigt](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)
4. Löschen Sie die Datenträger mit einer der folgenden Methoden:

  - Wenn kein virtueller Computer im Feld **Angeschlossen an** des Datenträgers aufgelistet ist, können Sie den Datenträger direkt löschen.

  - Wenn der Datenträger zur Datenspeicherung dient, gehen Sie folgendermaßen vor:

    1. Aktivieren Sie den Namen des virtuellen Computers, dem der Datenträger angefügt ist.
    2. Wechseln Sie zu **Virtuelle Computer** > **Instanzen**, und suchen Sie den virtuellen Computer.
    3. Stellen Sie sicher, dass der Datenträger nicht aktiv verwendet wird.
    4. Wählen Sie **Datenträger trennen** unten im Portal, um den Datenträger zu trennen.
    5. Wechseln Sie zu **Virtuelle Computer** > **Datenträger**, und warten Sie darauf, dass das Feld **Angeschlossen an** leer ist. Dies zeigt an, dass der Datenträger erfolgreich vom virtuellen Computer getrennt wurde.
    6. Wählen Sie **Löschen** am unteren Rand von **Virtuelle Computer** > **Datenträger**, um den Datenträger zu löschen.

  - Wenn der Datenträger ein Betriebssystem-Datenträger ist (das Feld **Enthält Betriebssystem** weist einen Wert wie z.B. „Windows“ auf) und an einen virtuellen Computer angefügt ist, befolgen Sie diese Schritte, um den virtuellen Computer löschen. Da der Betriebssystemdatenträger nicht getrennt werden kann, muss der virtuelle Computer zum Freigeben der Lease gelöscht werden.

    1. Aktivieren Sie den Namen des virtuellen Computers, dem der Datenträger angefügt ist.  
    2. Wechseln Sie zu **Virtuelle Computer** > **Instanzen**, und wählen Sie dann den virtuellen Computer, dem der Datenträger angefügt ist.
    3. Stellen Sie sicher, dass der virtuelle Computer nicht aktiv verwendet wird und dass Sie den virtuellen Computer nicht mehr benötigen.
    4. Wählen Sie den virtuellen Computer, dem der Datenträger angefügt ist, und wählen Sie dann **Löschen** > **Angefügte Datenträger löschen**.
    5. Wechseln Sie zu **Virtuelle Computer** > **Datenträger**, und warten Sie, bis der Datenträger nicht mehr angezeigt wird.  Es dauert möglicherweise einige Minuten, bis dies geschieht, und Sie müssen vielleicht die Seite aktualisieren.
    6. Wenn der Datenträger nicht ausgeblendet wird, warten Sie, bis das Feld **Angeschlossen an** leer ist. Dies zeigt an, dass der Datenträger vollständig vom virtuellen Computer getrennt wurde.  Wählen Sie dann den Datenträger, und wählen Sie **Löschen** am unteren Rand der Seite, um den Datenträger zu löschen.


   > [!NOTE]
   > Wenn ein Datenträger mit einem virtuellen Computer verbunden ist, können Sie ihn nicht löschen. Datenträger werden asynchron von einem gelöschten virtuellen Computer getrennt. Nach dem Löschen des virtuellen Computers kann es einige Minuten dauern, bis dieses Feld geleert wird.
   >
   >


### <a name="step-2-delete-any-vm-images-that-are-preventing-deletion-of-the-storage-account-or-container"></a>Schritt 2: Löschen Sie alle VM-Images, die das Löschen des Speicherkontos oder des Speichercontainers verhindern.
1. Wechseln Sie zum [klassischen Azure-Portal](https://manage.windowsazure.com/).
2. Wählen Sie **VIRTUELLER COMPUTER** > **IMAGES** aus, und löschen Sie die Images, die dem Speicherkonto, dem Speichercontainer oder der Speicher-VHD zugeordnet sind.

    Versuchen Sie danach erneut, das Speicherkonto, den Container oder die VHD zu löschen.

> [!WARNING]
> Sichern Sie alle Inhalte, die Sie speichern möchten, bevor Sie das Konto löschen. Wenn Sie eine VHD, ein Blob, eine Tabelle, Warteschlange oder Datei löschen, ist der Löschvorgang nicht umkehrbar. Stellen Sie sicher, dass die Ressource nicht verwendet wird.
>
>

## <a name="about-the-stopped-deallocated-status"></a>Informationen zum Status „Beendet“ (Zuordnung aufgehoben)
Virtuelle Computer, die im klassischen Bereitstellungsmodell erstellt und beibehalten wurden, weisen sowohl im [Azure-Portal](https://portal.azure.com/) als auch im [klassischen Azure-Portal](https://manage.windowsazure.com/) den Status **Beendet (Zuordnung aufgehoben)** auf.

**Klassisches Azure-Portal**:

![Status „Beendet (Zuordnung aufgehoben)“ für virtuelle Computer im Azure-Portal](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)

**Azure-Portal**:

![Status „Beendet (Zuordnung aufgehoben)“ für virtuelle Computer im klassischen Azure-Portal](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

Der Status „Beendet (Zuordnung aufgehoben)“ gibt Computerressourcen wie CPU, Arbeitsspeicher und Netzwerk frei. Die Datenträger werden jedoch weiterhin beibehalten, damit Sie den virtuellen Computer bei Bedarf schnell neu erstellen können. Diese Datenträger werden auf den VHDs erstellt, die von Azure Storage unterstützt werden. Diese VHDs gehören zum Speicherkonto, und die Datenträger verfügen über Leases für diese VHDs.

## <a name="next-steps"></a>Nächste Schritte
* [Löschen eines Speicherkontos](storage-create-storage-account.md#delete-a-storage-account)
* [How to break the locked lease of blob storage in Microsoft Azure (PowerShell) (in englischer Sprache)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)

