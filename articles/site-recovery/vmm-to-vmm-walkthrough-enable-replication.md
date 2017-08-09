---
title: "Aktivieren der Hyper-V-Replikation an einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt, wie die Replikation virtueller Hyper-V-Computer an einem sekundären System Center VMM-Standort mit Azure Site Recovery aktiviert wird."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d8782d14-9fef-4396-8912-ff945efc851b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 6673d192dbc18bfc955d9e7e3c55893512511ffb
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-9-enable-replication-to-a-secondary-site-for-hyper-v-vms"></a>Schritt 9: Aktivieren der Replikation an einem sekundären Standort für virtuelle Hyper-V-Computer


Nach dem Einrichten einer Replikationsrichtlinie aktivieren Sie anhand der Informationen in diesem Artikel die Replikation an einem sekundären System Center Virtual Machine Manager-Standort (VMM) für lokale virtuelle Hyper-V-Computer mithilfe von [Azure Site Recovery](site-recovery-overview.md).

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.



## <a name="select-vms-to-replicate"></a>Auswählen von zu replizierenden virtuellen Computern

1. Klicken Sie auf **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**. 

    ![Replikation aktivieren](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication1.png)

2. Wählen Sie auf dem Blatt **Quelle** den VMM-Server und die Cloud aus, in denen sich die Hyper-V-Hosts befinden, die Sie replizieren möchten. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication2.png)
3. Überprüfen Sie in **Ziel** den sekundären VMM-Server und die Cloud.
4. Wählen Sie unter **Virtuelle Computer** in der Liste die VMs aus, die Sie schützen möchten.

    ![Schutz für virtuelle Computer aktivieren](./media/vmm-to-vmm-walkthrough-enable-replication/enable-replication5.png)

Sie können den Fortschritt der Aktion **Schutz aktivieren** unter **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nach der Ausführung des Auftrags **Schutz abschließen** ist die erste Replikation abgeschlossen und der virtuelle Computer bereit zum Failover.

Beachten Sie Folgendes:

- Sie können auch Schutz für virtuelle Maschinen in der VMM-Konsole aktivieren. Klicken Sie in den Eigenschaften des virtuellen Computers auf der Symbolleiste auf **Schutz aktivieren** und anschließend auf die Registerkarte **Azure Site Recovery**.
- Nachdem Sie die Replikation aktiviert haben, können Sie unter **Replizierte Elemente** die Eigenschaften für die VM anzeigen. Das Dashboard **Zusammenfassung** enthält Informationen zur Replikationsrichtlinie der VM und ihrem Status. Klicken Sie auf **Eigenschaften** , um weitere Details anzuzeigen.

## <a name="onboard-existing-vms"></a>Integrierte vorhandene virtuelle Computer

Wenn in VMM virtuelle Computer vorhanden sind, die über Hyper-V-Replikate repliziert werden, können Sie diese für die Replikation durch Azure Site Recovery wie folgt integrieren:

1. Stellen Sie sicher, dass sich der Hyper-V-Hostserver der vorhandenen VM in der primären Cloud und der Hyper-V-Hostserver der Replikat-VM in der sekundären Cloud befindet.
2. Stellen Sie sicher, dass eine Replikationsrichtlinie für die primäre VMM-Cloud konfiguriert ist.
3. Aktivieren Sie die Replikation für den primären virtuellen Computer. Azure Site Recovery und VMM stellen sicher, dass der gleiche Replikathost und virtuelle Computer erkannt werden. Azure Site Recovery verwendet die Replikation wieder und stellt sie anhand der festgelegten Einstellungen erneut her.


## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 10: Ausführen eines Testfailovers](vmm-to-vmm-walkthrough-test-failover.md).

