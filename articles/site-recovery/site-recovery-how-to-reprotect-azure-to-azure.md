---
title: "Erneutes Schützen von virtuellen Azure-Computern zurück zur primären Azure-Region nach einem Failover | Microsoft-Dokumentation"
description: "Nach einem Failover von virtuellen Computern aus einer Azure-Region in eine andere Region können Sie die Computer mit Azure Site Recovery in umgekehrter Richtung schützen. Hier erfahren Sie, wie Sie Computer vor einem Failover erneut schützen."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/13/2017
ms.author: ruturajd
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 92a3250a4b07a8a473bd3343181176a2623cd476
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---
# <a name="reprotect-from-failed-over-azure-region-back-to-primary-region"></a>Erneutes Schützen aus einer Azure-Region in die primäre Region nach Failover



>[!NOTE]
>
> Die Site Recovery-Replikation für virtuelle Azure-Computer ist derzeit als Vorschauversion verfügbar.


## <a name="overview"></a>Übersicht
Wenn Sie ein [Failover](site-recovery-failover.md) für virtuelle Computer aus einer Azure-Region in eine andere Region ausführen, befinden sich die virtuellen Computer in einem ungeschützten Zustand. Wenn Sie sie wieder in die primäre Region verschieben möchten, müssen Sie die virtuellen Computer zunächst schützen und dann erneut ein Failover ausführen. Das Failover ist in beide Richtungen gleich. Entsprechend gibt es nach dem Aktivieren des Schutzes für die virtuellen Computer keinen Unterschied für das erneute Schützen nach dem Failover oder nach dem Failback.
Zum Erläutern der Abläufe beim erneuten Schützen und um Verwechslungen zu vermeiden, wird als primärer Standort der geschützten Computer „Asien, Osten“ und als Wiederherstellungsstandort der Computer die Region „Asien, Südosten“ verwendet. Während des Failovers wird das Failover der virtuellen Computer in die Region „Asien, Südosten“ ausgeführt. Vor dem Failback müssen Sie die virtuellen Computer aus „Asien, Südosten“ in „Asien, Osten“ erneut schützen. Dieser Artikel beschreibt die Schritte zum erneuten Schützen.

> [!WARNING]
> Wenn Sie die [Migration abgeschlossen](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), den virtuellen Computer in eine andere Ressourcengruppe verschoben oder den virtuellen Azure-Computer gelöscht haben, ist danach kein Failback möglich.

Wenn der Schutz wiederhergestellt wurde und die geschützten virtuellen Computer repliziert werden, können Sie ein Failover für die virtuellen Computer initiieren, um sie zurück in die Region „Asien, Osten“ zu übertragen.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="prerequisites"></a>Voraussetzungen
1. Für die virtuellen Computer sollte ein Commit ausgeführt worden sein.
2. Der Zielstandort – in diesem Fall die Azure-Region „Asien, Osten“ – sollte verfügbar sein und sollte auf neue Ressourcen in dieser Region zugreifen bzw. sie erstellen können.

## <a name="steps-to-reprotect"></a>Schritte zum erneuten Schützen

Im Folgenden finden Sie die Schritte zum erneuten Schützen eines virtuellen Computers mit den Standardwerten.

1. Klicken Sie unter **Tresor** > **Replizierte Elemente** mit der rechten Maustaste auf den virtuellen Computer, für den ein Failover durchgeführt wurde, und wählen Sie dann **Erneut schützen**. Sie können auch auf den Computer klicken und die Befehlsschaltfläche **Erneut schützen** wählen.

![Zum erneuten Schützen mit der rechten Maustaste klicken](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotect.png)

2. Auf dem Blatt können Sie sehen, dass die Schutzrichtung **Asien, Südosten zu Asien, Osten** bereits ausgewählt ist.

![Blatt „Erneut schützen“](./media/site-recovery-how-to-reprotect-azure-to-azure/reprotectblade.png)

3. Überprüfen Sie die Informationen für **Ressourcengruppe, Netzwerk, Speicher und Verfügbarkeitsgruppen**, und klicken Sie auf „OK“. Wenn markierte Ressourcen (neu) vorhanden sind, werden sie während des Vorgangs zum erneuten Schützen erstellt.

Hierbei wird ein Auftrag zum erneuten Schützen ausgelöst, mit dem zunächst für den Zielstandort (in diesem Fall SEA) ein Seeding mit den aktuellen Daten ausgeführt wird. Nach Abschluss des Auftrags werden die Deltas repliziert, bevor Sie ein Failover zurück zu „Asien, Südosten“ ausführen.

### <a name="reprotect-customization"></a>Anpassen des erneuten Schutzes
Wenn Sie während des Vorgangs zum erneuten Schützen das Extraktionsspeicherkonto oder das Netzwerk auswählen möchten, ist dies über die Anpassungsoption auf dem Blatt zum erneuten Schützen möglich.

![Option zum Anpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customize.png)

Sie können die folgenden Eigenschaften des virtuellen Zielcomputers beim erneuten Schützen anpassen.

![Blatt für Anpassen](./media/site-recovery-how-to-reprotect-azure-to-azure/customizeblade.png)

|Eigenschaft |Hinweise  |
|---------|---------|
|Zielressourcengruppe     | Sie können auswählen, dass die Zielressourcengruppe geändert wird, in der der virtuelle Computer erstellt wird. Beim erneuten Schützen wird der virtuelle Zielcomputer gelöscht, daher können Sie eine neue Ressourcengruppe auswählen, in der der virtuelle Computer nach dem Failover erstellt werden kann.         |
|Virtuelles Zielnetzwerk     | Das Netzwerk kann beim erneuten Schützen nicht geändert werden. Wiederholen Sie die Netzwerkzuordnung, um das Netzwerk zu ändern.         |
|Zielspeicher     | Sie können das Speicherkonto ändern, in dem der virtuelle Computer nach dem Failover erstellt wird.         |
|Zwischenspeicher     | Sie können ein Zwischenspeicherkonto angeben, das während der Replikation verwendet wird. Wenn Sie mit den Standardeinstellungen fortfahren, wird ein neues Zwischenspeicherkonto erstellt, sofern es nicht bereits vorhanden ist.         |
|Verfügbarkeitsgruppe     |Wenn der virtuelle Computer in „Asien, Osten“ Teil einer Verfügbarkeitsgruppe ist, können Sie eine Verfügbarkeitsgruppe für den virtuellen Zielcomputer in „Asien, Südosten“ auswählen. In den Standardwerten wird die vorhandene Verfügbarkeitsgruppe für SEA übernommen, und es wird versucht, sie zu verwenden. Während der Anpassung können Sie eine völlig neue Verfügbarkeitsgruppe angeben.         |


### <a name="what-happens-during-reprotect"></a>Was geschieht beim erneuten Schützen?

Wie beim ersten Aktivieren des Schutzes werden die folgenden Artefakte erstellt, wenn Sie die Standardeinstellungen verwenden.
1. In der Region „Asien, Osten“ wird ein Zwischenspeicherkonto erstellt.
2. Wenn das Zielspeicherkonto (das ursprüngliche Speicherkonto des virtuellen Computers in „Asien, Südosten“) nicht vorhanden ist, wird ein neues Konto erstellt. Der Name entspricht dem Speicherkonto des virtuellen Computers in „Asien, Osten“ mit dem Suffix „asr“.
3. Wenn die Zielverfügbarkeitsgruppe nicht vorhanden ist und beim Verwenden der Standardwerte erkannt wird, dass eine neue Verfügbarkeitsgruppe erstellt werden muss, wird diese im Rahmen des Auftrags zum erneuten Schützen erstellt. Wenn Sie den Vorgang zum erneuten Schützen angepasst haben, wird die ausgewählte Verfügbarkeitsgruppe verwendet.
4.

Im Folgenden werden alle Schritte aufgeführt, die beim Auslösen eines Auftrags zum erneuten Schützen ausgeführt werden. Dies gilt für den Fall, dass der virtuellen Computer auf der Zielseite vorhanden ist.

1. Die erforderlichen Artefakte werden als Teil des Vorgangs zum erneuten Schützen erstellt. Wenn sie bereits vorhanden sind, werden sie wiederverwendet.
2. Der virtuelle Computer auf der Zielseite („Asien, Südosten“) wird zunächst deaktiviert, sofern er ausgeführt wird.
3. Der Datenträger des virtuellen Computers auf der Zielseite wird von Azure Site Recovery als Seedingblob in einen Container kopiert.
4. Der virtuelle Computer auf der Zielseite wird dann gelöscht.
5. Das Seedingblob wird vom virtuellen Computer auf der aktuellen Quellseite („Asien, Osten“) für die Replikation verwendet. Dadurch wird sichergestellt, dass nur Deltas repliziert werden.
6. Die wichtigsten Unterschiede zwischen dem Quelldatenträger und dem Seedingblob werden synchronisiert. Dieser Vorgang kann einige Zeit in Anspruch nehmen.
7. Wenn der Auftrag zum erneuten Schützen abgeschlossen wurde, beginnt die Deltareplikation, mit der gemäß der Richtlinie ein Wiederherstellungspunkt erstellt wird.

> [!NOTE]
> Ein Schutz auf Ebene eines Wiederherstellungsplans ist nicht möglich. Der erneute Schutz ist nur auf VM-Ebene möglich.

Nach dem erfolgreichen erneuten Schützen befindet sich der virtuelle Computer in einem geschützten Zustand.

## <a name="next-steps"></a>Nächste Schritte

Wenn sich der virtuelle Computer in einem geschützten Zustand befindet, können Sie ein Failover initiieren. Beim Failover wird der virtuelle Computer in der Azure-Region „Asien, Osten“ heruntergefahren. Danach wird der virtuelle Computer in der Region „Asien, Südosten“ erstellt und gestartet. Dadurch ergibt sich eine kurze Ausfallzeit für die Anwendung. Führen Sie das Failover daher zu einer Zeit aus, zu der dies für Ihre Anwendung kein Problem darstellt. Es wird empfohlen, dass Sie zunächst ein Testfailover für den virtuellen Computer ausführen, um sicherzustellen, dass keine Fehler auftreten, bevor Sie das Failover initiieren.

-   [Schritte zum Initiieren eines Testfailovers für den virtuellen Computer](site-recovery-test-failover-to-azure.md)

-   [Schritte zum Initiieren des Failovers für den virtuellen Computer](site-recovery-failover.md)

