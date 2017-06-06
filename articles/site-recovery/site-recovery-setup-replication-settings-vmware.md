---
title: "Einrichten der Replikationseinstellungen für Azure Site Recovery | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie Azure Site Recovery bereitstellen, um die Replikation, das Failover und die Wiederherstellung von virtuellen Hyper-V-Computern in VMM-Clouds nach Azure zu orchestrieren.
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: sutalasi
ms.translationtype: Human Translation
ms.sourcegitcommit: 28e905a20d878eab1428a4b88113544aa742124a
ms.openlocfilehash: 9c34ea792aa561b8155a915845ffb857dfef7a90
ms.contentlocale: de-de
ms.lasthandoff: 02/21/2017


---
# <a name="manage-replication-policy-for-vmware-to-azure"></a>Verwalten der Replikationsrichtlinie für VMware in Azure


## <a name="create-a-replication-policy"></a>Erstellen einer Replikationsrichtlinie

1. Wählen Sie **Verwalten** > **Site Recovery-Infrastruktur**.
2. Wählen Sie unter **For VMware and Physical machines** (Für VMware und physische Computer) die Option **Replikationsrichtlinien**.
3. Wählen Sie die Option **+Replikationsrichtlinie**.

      ![Erstellen einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Geben Sie den Namen der Richtlinie ein.

5. Geben Sie unter **RPO-Schwellenwert** den RPO-Grenzwert an. Warnungen werden generiert, wenn die fortlaufende Replikation diesen Grenzwert überschreitet.
6. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** die Dauer des Aufbewahrungszeitfensters für die einzelnen Wiederherstellungspunkte (in Stunden) an. Geschützte Computer können innerhalb eines Aufbewahrungszeitfensters an einem beliebigen Punkt wiederhergestellt werden.

    > [!NOTE]
    > Für in Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt. Für in Standardspeicher replizierte Computer wird eine Aufbewahrungsdauer von bis zu 72 Stunden unterstützt.

    > [!NOTE]
    > Eine Replikationsrichtlinie für das Failback wird automatisch erstellt.

7. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (in Minuten) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten.

8. Klicken Sie auf **OK**. Die Richtlinie sollte innerhalb von 30 bis 60 Sekunden erstellt werden.

![Generierung der Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a>Zuordnen eines Konfigurationsservers zu einer Replikationsrichtlinie
1. Wählen Sie die Replikationsrichtlinie aus, der Sie den Konfigurationsserver zuordnen möchten.
2. Klicken Sie auf **Zuordnen**.
![Zuordnen des Konfigurationsservers](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Wählen Sie den Konfigurationsserver in der Serverliste aus.
4. Klicken Sie auf **OK**. Die Zuordnung des Konfigurationsservers sollte innerhalb von ein bis zwei Minuten abgeschlossen sein.

![Zuordnung des Konfigurationsservers](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a>Bearbeiten einer Replikationsrichtlinie
1. Wählen Sie die Replikationsrichtlinie aus, für die Sie die Replikationseinstellungen bearbeiten möchten.
![Bearbeiten einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Klicken Sie auf **Edit Settings**.
![Bearbeiten der Einstellungen für eine Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Ändern Sie die Einstellungen abhängig von Ihren Anforderungen.
4. Klicken Sie auf **Speichern**. Die Richtlinie sollte innerhalb von zwei bis fünf Minuten gespeichert werden, je nachdem, wie viele virtuelle Computer diese Replikationsrichtlinie verwenden.

![Speichern einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a>Aufheben der Zuordnung des Konfigurationsservers zu einer Replikationsrichtlinie
1. Wählen Sie die Replikationsrichtlinie aus, der Sie den Konfigurationsserver zuordnen möchten.
2. Klicken Sie auf **Zuordnung aufheben**.
3. Wählen Sie den Konfigurationsserver in der Serverliste aus.
4. Klicken Sie auf **OK**. Die Aufhebung der Zuordnung des Konfigurationsservers sollte innerhalb von ein bis zwei Minuten abgeschlossen sein.

    > [!NOTE]
    > Sie können die Zuordnung eines Konfigurationsservers nicht aufheben, wenn mindestens ein mithilfe der Richtlinie repliziertes Element vorhanden ist. Stellen Sie sicher, dass keine mithilfe der Richtlinie replizierten Elemente vorhanden sind, bevor Sie die Zuordnung des Konfigurationsservers aufheben.

## <a name="delete-a-replication-policy"></a>Löschen einer Replikationsrichtlinie

1. Wählen Sie die Replikationsrichtlinie aus, die Sie löschen möchten.
2. Klicken Sie auf **Löschen**. Die Richtlinie sollte innerhalb von 30 bis 60 Sekunden gelöscht werden.

    > [!NOTE]
    > Sie können eine Replikationsrichtlinie nicht löschen, wenn ihr mindestens ein Konfigurationsserver zugeordnet ist. Stellen Sie sicher, dass keine mithilfe der Richtlinie replizierten Elemente vorhanden sind, und löschen Sie alle zugeordneten Konfigurationsserver, bevor Sie die Richtlinie löschen.

