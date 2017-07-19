---
title: "Einrichten einer Replikationsrichtlinie für die Replikation virtueller Hyper-V-Computer (ohne System Center VMM) in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Schritte zusammengefasst, die Sie zum Aufstellen einer Replikationsrichtlinie beim Replizieren von virtuellen Hyper-V-Computern im Azure-Speicher benötigen."
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1777e0eb-accb-42b5-a747-11272e131a52
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: 42a0421b4589b95c0da806be0ec8875b7d62c91d
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="step-9-set-up-a-replication-policy-for-hyper-v-vm-replication-to-azure"></a>Schritt 9: Aufstellen einer Replikationsrichtlinie für die Replikation virtueller Hyper-V-Computer in Azure

In diesem Artikel wird beschrieben, wie Sie bei der Replikation von virtuellen Hyper-V-Computern in Azure (ohne System Center VMM) mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal eine Replikationsrichtlinie erstellen.


Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="about-snapshots"></a>Informationen zu Momentaufnahmen

Hyper-V verwendet zwei Momentaufnahmen: eine Standard-Momentaufnahme, die eine inkrementelle Momentaufnahme des gesamten virtuellen Computers bereitstellt, und eine anwendungskonsistente Momentaufnahme, die eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers erfasst.
    - Anwendungskonsistente Momentaufnahmen verwenden den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um sicherzustellen, dass Anwendungen sich bei der Erstellung der Momentaufnahme in einem konsistenten Zustand befinden.
    - Die Leistung von Anwendungen auf virtuellen Quellcomputern wird durch die Aktivierung anwendungskonsistenter Momentaufnahmen beeinträchtigt. Stellen Sie sicher, dass der festgelegte Wert kleiner als die konfigurierte Anzahl der zusätzlichen Wiederherstellungspunkte ist.

## <a name="set-up-a-replication-policy"></a>Einrichten einer Replikationsrichtlinie

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.

    ![Netzwerk](./media/hyper-v-site-walkthrough-replication/gs-replication.png)
2. Geben Sie unter **Richtlinie erstellen und zuordnen**einen Richtliniennamen an.
3. Geben Sie unter **Kopierhäufigkeit**an, wie oft Sie Deltadaten nach der ersten Replikation replizieren möchten (alle 30 Sekunden, nach 5 Minuten oder nach 15 Minuten).

    > [!NOTE]
    > Eine Häufigkeit von 30 Sekunden wird bei der Replikation nach Storage Premium nicht unterstützt. Die Einschränkung richtet sich nach der Anzahl von Momentaufnahmen pro Blob (100), die von Storage Premium unterstützt wird. [Weitere Informationen](../storage/storage-premium-storage.md#snapshots-and-copy-blob).

4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. VMs können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (1 bis 12 Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten.
6. Geben Sie unter **Startzeit der ersten Replikation** an, wann die erste Replikation starten soll. Da die Replikation über Ihre Internetbandbreite durchgeführt wird, ist es ratsam, sie außerhalb der Zeiten mit der höchsten Arbeitsbelastung einzuplanen. Klicken Sie dann auf **OK**.

    ![Replikationsrichtlinie](./media/hyper-v-site-walkthrough-replication/gs-replication2.png)

Wenn Sie eine neue Richtlinie erstellen, wird sie der Hyper-V-Site automatisch zugeordnet. Sie können die Hyper-V-Site (und die darin enthaltenen VMs) über **Replikation** > Richtlinienname > **Associate Hyper-V Site** (Hyper-V-Site zuordnen) mehreren Replikationsrichtlinien zuordnen.



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 10: Aktivieren der Replikation](hyper-v-site-walkthrough-enable-replication.md).

