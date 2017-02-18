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
ms.date: 01/19/2017
ms.author: sutalasi
translationtype: Human Translation
ms.sourcegitcommit: 74d51269cdd55e39eaa2963fec2b409decb47d0a
ms.openlocfilehash: a279fdc35c62c76e6f0858d52c64240032669f15


---
# <a name="manage-replication-policy-for-vmware-to-azure"></a>Verwalten der Replikationsrichtlinie für VMware in Azure


## <a name="create-a-new-replication-policy"></a>Erstellen einer neuen Replikationsrichtlinie

1. Klicken Sie im Menü auf der linken Seite auf „Verwalten -> Site Recovery-Infrastruktur“. 
2. Wählen Sie im Abschnitt „For VMware and Physical machines“ (Für VMware und physische Computer) die Option „Replikationsrichtlinien“ aus.
3. Klicken Sie oben auf „+Replikationsrichtlinie“.

    ![Erstellen einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Geben Sie den Namen der Richtlinie ein.

5. Geben Sie unter „RPO-Schwellenwert“ den RPO-Grenzwert an. Warnungen werden generiert, wenn die fortlaufende Replikation diesen Grenzwert überschreitet.
6. Geben Sie unter „Aufbewahrungszeitraum des Wiederherstellungspunkts“ das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden. 

    > [!NOTE] 
    > Für Computer, die in Storage Premium repliziert werden, wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt. Für Computer, die im Standardspeicher repliziert werden, wird eine Aufbewahrungsdauer von 72 Stunden unterstützt.
    
    > [!NOTE] 
    > Eine Replikationsrichtlinie für ein Failback wird automatisch erstellt.

7. Geben Sie unter „App-konsistente Momentaufnahmehäufigkeit“ an, wie häufig (in Minuten) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten.

8. Klicken Sie auf „OK“. Die Richtlinie sollte in etwa 30 bis 60 Sekunden erstellt werden.

![Erstellen einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-configuration-server-with-replication-policy"></a>Zuordnen des Konfigurationsservers zur Replikationsrichtlinie
1. Klicken Sie auf die Replikationsrichtlinie, der der Konfigurationsserver zugeordnet werden soll.
2. Klicken Sie oben auf „Zuordnen“.
![Erstellen einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Wählen Sie aus der Liste der Server „Konfigurationsserver“ aus.
4. Klicken Sie auf OK. Der Konfigurationsserver sollte in etwa ein bis zwei Minuten zugeordnet sein.

![Erstellen einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-replication-policy"></a>Bearbeiten einer Replikationsrichtlinie
1. Klicken Sie auf die Replikationsrichtlinie, für die Sie die Replikationseinstellungen bearbeiten möchten.
![Erstellen einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Klicken Sie oben auf „Einstellungen bearbeiten“.
![Erstellen einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Ändern Sie die Einstellungen abhängig von Ihren Anforderungen.
4. Klicken Sie oben auf „Speichern“. Die Richtlinie sollte in etwa zwei bis fünf Minuten gespeichert werden, je nachdem, wie viele virtuelle Computer diese Replikationsrichtlinie verwenden.

![Erstellen einer Replikationsrichtlinie](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-configuration-server-from-replication-policy"></a>Aufheben der Zuordnung des Konfigurationsservers zur Replikationsrichtlinie
1. Klicken Sie auf die Replikationsrichtlinie, der der Konfigurationsserver zugeordnet werden soll.
2. Klicken Sie oben auf „Zuordnung aufheben“.
3. Wählen Sie aus der Liste der Server „Konfigurationsserver“ aus.
4. Klicken Sie auf OK. Die Zuordnung des Konfigurationsservers sollte in etwa ein bis zwei Minuten aufgehoben sein.
    
    > [!NOTE] 
    > Sie können die Zuordnung eines Konfigurationsservers nicht aufheben, wenn mindestens ein mithilfe der Richtlinie repliziertes Element vorhanden ist. Stellen Sie sicher, dass keine mithilfe der Richtlinie replizierten Elemente vorhanden sind, ehe Sie die Zuordnung des Konfigurationsservers aufheben.

## <a name="delete-replication-policy"></a>Löschen einer Replikationsrichtlinie 

1. Klicken Sie auf die Replikationsrichtlinie, die Sie löschen möchten.
2. Klicken Sie auf „Löschen“. Die Richtlinie sollte in etwa 30 bis 60 Sekunden gelöscht werden.

    > [!NOTE] 
    > Sie können eine Replikationsrichtlinie nicht löschen, wenn ihr mindestens ein Konfigurationsserver zugeordnet ist. Stellen Sie sicher, dass keine mithilfe der Richtlinie replizierten Elemente vorhanden sind, und löschen Sie alle zugeordneten Konfigurationsserver, bevor Sie die Richtlinie löschen.


<!--HONumber=Jan17_HO4-->


