---
title: "Einrichten einer Richtlinie für die Replikation von Hyper-V-VMs (mit VMM) nach Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt die Festlegung einer Richtlinie für die Replikation von Hyper-V-VMs (mit VMM) nach Azure mit Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ee808b20-324b-4198-b831-edb65b95e8b7
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 1bf66eaa272ad1e2bf400707929243662eb1f140
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="step-10-set-up-a-replication-policy-for-hyper-v-vm-replication-with-vmm-to-azure"></a>Schritt 10: Einrichten einer Richtlinie für die Replikation virtueller Hyper-V-Computer (mit VMM) nach Azure


Nachdem Sie die [Netzwerkzuordnung](vmm-to-azure-walkthrough-network-mapping.md) eingerichtet haben, konfigurieren Sie anhand der Informationen in diesem Artikel eine Replikationsrichtlinie, um lokale virtuelle Hyper-V-Computer, die in System Center Virtual Machine Manager-Clouds (VMM) verwaltet werden, mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal nach Azure zu replizieren.

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.



## <a name="create-a-policy"></a>Erstellen einer Richtlinie

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.

    ![Netzwerk](./media/vmm-to-azure-walkthrough-replication/gs-replication.png)
2. Geben Sie unter **Richtlinie erstellen und zuordnen**einen Richtliniennamen an.
3. Geben Sie unter **Kopierhäufigkeit**an, wie oft Sie Deltadaten nach der ersten Replikation replizieren möchten (alle 30 Sekunden, nach 5 Minuten oder nach 15 Minuten).

    > [!NOTE]
    >  Eine Häufigkeit von 30 Sekunden wird bei der Replikation nach Storage Premium nicht unterstützt. Die Einschränkung richtet sich nach der Anzahl von Momentaufnahmen pro Blob (100), die von Storage Premium unterstützt wird. [Weitere Informationen](../storage/storage-premium-storage.md#snapshots-and-copy-blob)

4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts**das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit**an, wie häufig (1 bis 12 Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Hyper-V verwendet zwei Momentaufnahmen: eine Standard-Momentaufnahme, die eine inkrementelle Momentaufnahme des gesamten virtuellen Computers bereitstellt, und eine anwendungskonsistente Momentaufnahme, die eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers erfasst. Anwendungskonsistente Momentaufnahmen verwenden den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um sicherzustellen, dass Anwendungen sich bei der Erstellung der Momentaufnahme in einem konsistenten Zustand befinden. Beachten Sie, dass die Leistung von Anwendungen auf virtuellen Quellcomputern durch die Aktivierung anwendungskonsistenter Momentaufnahmen beeinträchtigt wird. Stellen Sie sicher, dass der festgelegte Wert kleiner als die konfigurierte Anzahl der zusätzlichen Wiederherstellungspunkte ist.
6. Geben Sie unter **Startzeit der ersten Replikation**an, wann die erste Replikation starten soll. Da die Replikation über Ihre Internetbandbreite durchgeführt wird, ist es ratsam, sie außerhalb der Zeiten mit der höchsten Arbeitsbelastung einzuplanen.
7. Geben Sie unter **In Azure gespeicherte Daten verschlüsseln**an, ob ruhende Daten im Azure-Speicher verschlüsselt werden sollen. Klicken Sie dann auf **OK**.

    ![Replikationsrichtlinie](./media/vmm-to-azure-walkthrough-replication/gs-replication2.png)
8. Wenn Sie eine neue Richtlinie erstellen, wird sie der VMM-Cloud automatisch zugeordnet. Klicken Sie auf **OK**. Sie können dieser Replikationsrichtlinie unter **Replikation** > <Richtlinienname> > **VMM-Cloud zuordnen** zusätzliche VMM-Clouds (und die darin enthaltenen virtuellen Computer) zuordnen.

    ![Replikationsrichtlinie](./media/vmm-to-azure-walkthrough-replication/policy-associate.png)



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 11: Aktivieren der Replikation](vmm-to-azure-walkthrough-enable-replication.md).

