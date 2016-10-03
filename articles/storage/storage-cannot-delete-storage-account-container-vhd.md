<properties
	pageTitle="Problembehandlung für das Löschen von Azure-Speicherkonten, -Containern oder -VHDs | Microsoft Azure"
	description="Problembehandlung für das Löschen von Azure-Speicherkonten, -Containern oder -VHDs"
	services="storage"
	documentationCenter=""
	authors="genlin"
	manager="felixwu"
	editor="tysonn"
	tags="storage"/>

<tags
	ms.service="storage"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/20/2016"
	ms.author="minet;genli;robinsh"/>

# Problembehandlung für das Löschen von Azure-Speicherkonten, -Containern oder -VHDs

## Zusammenfassung
Möglicherweise treten Fehler beim Löschen von Azure-Speicherkonten, -Containern oder -VHDs im [Azure-Portal](https://portal.azure.com/) oder im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf. Die Probleme können durch folgende Umstände verursacht werden:

-	Wenn Sie einen virtuellen Computer löschen, werden der Datenträger und die VHD nicht automatisch gelöscht. Dies kann die Ursache für Fehler beim Löschen des Speicherkontos sein. Der Datenträger wird nicht gelöscht, damit Sie den Datenträger verwenden können, um einen anderen virtuellen Computer bereitzustellen.

-	Es besteht noch eine Lease für einen Datenträger oder für das Blob, das dem Datenträger zugeordnet ist.

Falls Sie Ihr Azure-Problem mit den Informationen in diesem Artikel nicht beheben konnten, suchen Sie in den Azure-Foren zu [MSDN und Stack Overflow](https://azure.microsoft.com/support/forums/). Sie können Ihr Problem in diesen Foren veröffentlichen oder auf Twitter an den @AzureSupport senden. Darüber hinaus können Sie eine Azure-Supportanfrage stellen, indem Sie auf der Website des [Azure-Supports](https://azure.microsoft.com/support/options/) die Option **Support erhalten** auswählen.

## Lösung
Um die häufigsten Probleme zu beheben, verwenden Sie die folgende Methode:

1. Wechseln Sie zum [klassischen Azure-Portal](https://manage.windowsazure.com/).
2. Wählen Sie **VIRTUELLER COMPUTER** > **DATENTRÄGER**.

	![Abbildung von Datenträgern auf virtuellen Computern im klassischen Azure-Portal](./media/storage-cannot-delete-storage-account-container-vhd/VMUI.png)

3. Suchen Sie die Datenträger, die dem zu löschenden Speicherkonto oder Container bzw. der zu löschenden VHD zugeordnet sind. Wenn Sie den Speicherort des Datenträgers überprüfen, finden Sie das zugeordnete Speicherkonto, den Container und die VHD.

	![Abbildung, die Speicherortinformationen für Datenträger im klassischen Azure-Portal zeigt](./media/storage-cannot-delete-storage-account-container-vhd/DiskLocation.png)

4. Stellen Sie sicher, dass kein virtueller Computer im Feld **Angeschlossen an** der Datenträger aufgeführt ist, und löschen Sie dann die Datenträger.

 	> [AZURE.NOTE] Wenn ein Datenträger mit einem virtuellen Computer verbunden ist, können Sie ihn nicht löschen. Datenträger werden asynchron von einem gelöschten virtuellen Computer getrennt. Nach dem Löschen des virtuellen Computers kann es einige Minuten dauern, bis dieses Feld geleert wird.

5. Wählen Sie **VIRTUELLER COMPUTER** > **IMAGES** aus, und löschen Sie die Images, die dem Speicherkonto, dem Container oder der VHD zugeordnet sind.

	Versuchen Sie danach erneut, das Speicherkonto, den Container oder die VHD zu löschen.

> [AZURE.WARNING] Sichern Sie alle Inhalte, die Sie speichern möchten, bevor Sie das Konto löschen. Es ist nicht möglich, ein gelöschtes Speicherkonto wiederherzustellen oder Inhalte abzurufen, die das Konto vor dem Löschen enthielt. Dies gilt auch für alle Ressourcen im Konto – gelöschte VHDs, Blobs, Tabellen, Warteschlangen oder Dateien können nicht wiederhergestellt werden. Stellen Sie sicher, dass die Ressource nicht verwendet wird.

## Häufige Probleme

Im folgenden Abschnitt werden häufige Fehler aufgeführt, die beim Versuch, Azure-Speicherkonten, -Container oder -VHDs zu löschen, auftreten können:

### Szenario 1: Speicherkonto kann nicht gelöscht werden

Wenn Sie im [Azure-Portal](https://portal.azure.com/) oder [klassischen Azure-Portal](https://manage.windowsazure.com/) zum Speicherkonto navigieren und **Löschen** wählen, wird möglicherweise die folgende Fehlermeldung angezeigt:

**Im Azure-Portal**:

*Das Speicherkonto <VM-Speicherkontoname> konnte nicht gelöscht werden. Fehler beim Löschen des Speicherkontos <VM-Speicherkontoname>: „Speicherkonto <VM-Speicherkontoname> hat einige aktive Images und/oder Datenträger. Entfernen Sie diese Bilder und/oder Datenträger, bevor Sie dieses Speicherkonto löschen.“*

**Im klassischen Azure-Portal**:

*Speicherkonto <VM-Speicherkontoname> hat einige aktive Images und/oder Datenträger, z.B. „xxxxxxxxx- xxxxxxxxx-O-209490240936090599“. Entfernen Sie diese Bilder und/oder Datenträger, bevor Sie dieses Speicherkonto löschen.*

Auch der folgende Fehler kann angezeigt werden:

**Im Azure-Portal**:

*Das Speicherkonto <VM-Speicherkontoname> verfügt über 1 Container, der/die über ein aktives Image und/oder Datenträgerartefakte verfügt/verfügen. Stellen Sie sicher, dass diese Artefakte aus dem Image-Repository entfernt werden, bevor Sie dieses Speicherkonto löschen.*

**Im klassischen Azure-Portal**:

*Übermitteln fehlgeschlagen – das Speicherkonto <VM-Speicherkontoname> verfügt über 1 Container, der/die über ein aktives Image und/oder Datenträgerartefakte verfügt/verfügen. Stellen Sie sicher, dass diese Artefakte aus dem Image-Repository entfernt werden, bevor Sie dieses Speicherkonto löschen. Wenn Sie versuchen, ein Speicherkonto zu löschen, und dem Speicherkonto aktive Datenträger zugeordnet sind, werden Sie mit einer Meldung darüber informiert, dass aktive Datenträger vorhanden sind, die gelöscht werden müssen.*

### Szenario 2: Container kann nicht gelöscht werden

Wenn Sie versuchen, den Speichercontainer zu löschen, wird u. U die folgende Fehlermeldung angezeigt:

*Speichercontainer <Containername> konnte nicht gelöscht werden. Fehler: Derzeit ist eine Lease für den Container vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.*

### Szenario 3: VHD kann nicht gelöscht werden

Nachdem Sie einen virtuellen Computer gelöscht haben und dann versuchen, die Blobs für die zugeordneten VHDs zu löschen, wird möglicherweise die folgende Meldung angezeigt:

*Fehler beim Löschen von Blob „Pfad/XXXXXX-XXXXXX-os-1447379084699.vhd“. Fehler: Derzeit ist eine Lease für das Blob vorhanden, und in der Anforderung wurde keine Lease-ID angegeben.*

## Informationen zum Status „Beendet“ (Zuordnung aufgehoben)

Virtuelle Computer, die im klassischen Bereitstellungsmodell erstellt und beibehalten wurden, haben im [Azure-Portal](https://portal.azure.com/) oder im [klassischen Azure-Portal](https://manage.windowsazure.com/) den Status **Beendet (Zuordnung aufgehoben)**.

**Klassisches Azure-Portal**:

![Status „Beendet (Zuordnung aufgehoben)“ für virtuelle Computer im Azure-Portal](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo2.png)


**Azure-Portal**:

![Status „Beendet (Zuordnung aufgehoben)“ für virtuelle Computer im klassischen Azure-Portal](./media/storage-cannot-delete-storage-account-container-vhd/moreinfo1.png)

Der Status „Beendet (Zuordnung aufgehoben)“ gibt Computerressourcen wie CPU, Arbeitsspeicher und Netzwerk frei. Die Datenträger werden jedoch weiterhin beibehalten, damit der Benutzer den virtuellen Computer bei Bedarf schnell neu erstellen kann. Diese Datenträger werden auf den VHDs erstellt, die von Azure Storage unterstützt werden. Diese VHDs gehören zum Speicherkonto, und die Datenträger verfügen über Leases für diese VHDs.

## Nächste Schritte

- [Löschen eines Speicherkontos](storage-create-storage-account.md#delete-a-storage-account)
- [How to break the locked lease of blob storage in Microsoft Azure (PowerShell) (in englischer Sprache)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)

<!---HONumber=AcomDC_0921_2016-->