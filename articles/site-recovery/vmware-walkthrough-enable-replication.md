---
title: Aktivieren der Replikation zum Replizieren von VMware-VMs in Azure mit Azure Site Recovery | Microsoft-Dokumentation
description: "In diesem Artikel wird zusammengefasst, wie Sie die Replikation von VMware in Azure mit dem Azure Site Recovery-Dienst aktivieren können."
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 519c5559-7032-4954-b8b5-f24f5242a954
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 470b9ddd8df4a4e74ec7174f79020c252323e502
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-11-enable-replication-for-vmware-virtual-machines-to-azure"></a>Schritt 11: Aktivieren der Replikation für virtuelle VMware-VMs in Azure


In diesem Artikel erfahren Sie, wie Sie die Replikation für lokale virtuelle VMware-Computer mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst über das Azure-Portal in Azure aktivieren.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="before-you-start"></a>Vorbereitung

- Auf den VMware-VMs muss die [Komponente „Mobilitätsdienst“ installiert](vmware-walkthrough-install-mobility.md) sein. – Wenn eine VM für die Pushinstallation vorbereitet wurde, installiert der Prozessserver automatisch den Mobilitätsdienst, sobald Sie die Replikation aktivieren.
- Ihr Azure-Benutzerkonto muss über bestimmte [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation eines virtuellen Computers in Azure verfügen.
- Beim Hinzufügen oder Ändern von VMs kann es 15 Minuten oder auch länger dauern, bis Änderungen wirksam werden und im Portal sichtbar sind.
- Sie können den letzten Zeitpunkt der Ermittlung für VMs unter **Konfigurationsserver** > **Letzter Kontakt um** überprüfen.
- Wenn Sie VMs hinzufügen möchten, ohne auf die planmäßige Ermittlung zu warten, markieren Sie den Konfigurationsserver (nicht darauf klicken) und klicken auf **Refresh** (Aktualisieren).



## <a name="exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

Standardmäßig werden alle Datenträger auf einem Computer repliziert. Sie können Datenträger von der Replikation ausschließen. Beispielsweise möchten Sie möglicherweise nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart des Computers oder der Anwendung aktualisiert werden (z.B. pagefile.sys oder SQL Server tempdb), repliziert werden. [Weitere Informationen](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Replizieren von VMs

Bevor Sie beginnen, verschaffen Sie sich schnell einen Überblick per Video

>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video3-Protect-VMware-Virtual-Machines/player]

1. Klicken Sie auf **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**.
2. Wählen Sie unter **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Virtuelle Computer**.
4. Wählen Sie unter **vCenter-/vSphere-Hypervisor** den vCenter-Server aus, der den vSphere-Host verwaltet, oder wählen Sie den Host aus.
5. Wählen Sie den Prozessserver aus. Wenn Sie keine zusätzlichen Prozessserver erstellt haben, ist dies der Konfigurationsserver. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/vmware-walkthrough-enable-replication/enable-replication2.png)

6. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die VMs erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).


7. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden möchten. Falls Sie kein bereits eingerichtetes Konto verwenden möchten, können Sie ein neues Konto erstellen.

8. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer, die nach einem Failover erstellt werden, eine Verbindung herstellen. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Falls Sie kein vorhandenes Netzwerk verwenden möchten, können Sie ein Netzwerk erstellen.

    ![Replikation aktivieren](./media/vmware-walkthrough-enable-replication/enable-rep3.png)
9. Klicken Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen**, und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/vmware-walkthrough-enable-replication/enable-replication5.png)
10. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Konto aus, das der Prozessserver zum automatischen Installieren des Mobilitätsdiensts auf dem Computer verwenden soll.
11. Standardmäßig werden alle Datenträger repliziert. Klicken Sie auf **Alle Datenträger** , und entfernen Sie alle Datenträger, die Sie nicht replizieren möchten. Klicken Sie dann auf **OK**. Sie können später weitere VM-Eigenschaften festlegen.

    ![Replikation aktivieren](./media/vmware-walkthrough-enable-replication/enable-replication6.png)
11. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist. Wenn Sie eine Richtlinie ändern, werden die Änderungen auf den replizierenden Computer und neue Computer angewendet.
12. Aktivieren Sie **Multi-VM-Konsistenz** , wenn Sie Computer in einer Replikationsgruppe zusammenfassen möchten, und geben Sie einen Namen für die Gruppe an. Klicken Sie dann auf **OK**. Beachten Sie Folgendes:

    * Computer in Replikationsgruppen werden gemeinsam repliziert und verfügen beim Failover über gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte.
    * Es wird empfohlen, dass Sie virtuelle Computer und physische Server zusammenfassen, damit sie Ihre Workloads widerspiegeln. Das Aktivieren von Multi-VM-Konsistenz kann sich auf die Leistung der Workload auswirken und sollte nur verwendet werden, wenn Computer die gleiche Workload ausführen und Sie Konsistenz benötigen.

    ![Replikation aktivieren](./media/vmware-walkthrough-enable-replication/enable-replication7.png)
13. Klicken Sie auf **Replikation aktivieren**. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 12: Ausführen eines Testfailovers](vmware-walkthrough-test-failover.md).

