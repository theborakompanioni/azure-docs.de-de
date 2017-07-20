---
title: Aktivieren der Replikation zum Replizieren virtueller Hyper-V-Computer in Azure (ohne System Center VMM) mit Azure Site Recovery | Microsoft-Dokumentation
description: "In diesem Artikel wird zusammengefasst, wie Sie die Replikation Virtueller Hyper-V-Computer in Azure mit dem Azure Site Recovery-Dienst aktivieren können."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: bd31ef01-69f1-4591-a519-e42510a6e2f4
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: aabe99dbd375b80e4a87ca7a067927008672b4ed
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="step-10-enable-replication-for-hyper-v-vms-replicating-to-azure"></a>Schritt 10: Aktivieren der Replikation zum Replizieren virtueller Hyper-V-Computer in Azure


In diesem Artikel erfahren Sie, wie Sie die Replikation lokaler virtueller Hyper-V-Computer, die nicht in System Center-VMM verwaltet werden, mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts über das Azure-Portal in Azure replizieren.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.




## <a name="before-you-start"></a>Vorbereitung

Bevor Sie starten, stellen Sie sicher, dass Ihr Azure-Benutzerkonto die erforderlichen [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Aktivieren der Replikation eines neuen virtuellen Computers in Azure besitzt.

## <a name="exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

Standardmäßig werden alle Datenträger auf einem Computer repliziert. Sie können Datenträger von der Replikation ausschließen. Beispielsweise möchten Sie möglicherweise nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart des Computers oder der Anwendung aktualisiert werden (z.B. pagefile.sys oder SQL Server tempdb), repliziert werden. [Weitere Informationen](site-recovery-exclude-disk.md)


## <a name="replicate-vms"></a>Replizieren von VMs

Aktivieren Sie die Replikation für VMs wie folgt:          

1. Klicken Sie auf **Replicate application (Anwendung replizieren)** > **Quelle**. Klicken Sie nach der erstmaligen Einrichtung der Replikation auf **+Replizieren**, um die Replikation für weitere Computer zu aktivieren.

    ![Replikation aktivieren](./media/hyper-v-site-walkthrough-enable-replication/enable-replication.png)
2. Wählen Sie in **Quelle** die Hyper-V-Site aus. Klicken Sie dann auf **OK**.
3. Wählen Sie unter **Ziel** das Tresorabonnement und das Failovermodell aus, das Sie in Azure (klassisches oder Resource Manager-Modell) nach einem Failover verwenden möchten.
4. Wählen Sie das Speicherkonto aus, das Sie verwenden möchten. Wenn Sie über kein Konto verfügen, das Sie verwenden möchten, können Sie [eines erstellen](#set-up-an-azure-storage-account). Klicken Sie dann auf **OK**.
5. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer bei einem Failover eine Verbindung herstellen.

    - Um die Netzwerkeinstellungen auf alle Computer anzuwenden, die Sie für die Replikation aktivieren, wählen Sie **Jetzt für die ausgewählten Computer konfigurieren** aus.
    - Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen.
    - Wenn Sie über kein Netzwerk verfügen, das Sie verwenden möchten, können Sie [eines erstellen](#set-up-an-azure-network). Wählen Sie, falls zutreffend, ein Subnetz aus. Klicken Sie dann auf **OK**.

   ![Replikation aktivieren](./media/hyper-v-site-walkthrough-enable-replication/enable-replication11.png)

6. Klicken Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen**, und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/hyper-v-site-walkthrough-enable-replication/enable-replication5-for-exclude-disk.png)

7. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Betriebssystem für die ausgewählten VMs und den Betriebssystem-Datenträger aus.
8. Stellen Sie sicher, dass der Name des virtuellen Azure-Computers (Zielname) den [Anforderungen für virtuelle Azure-Computer](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) entspricht.
9. Standardmäßig werden alle Datenträger des virtuellen Computers für die Replikation ausgewählt. Löschen Sie die Datenträger, um sie auszuschließen.
10. Klicken Sie zum Speichern der Änderungen auf **OK**. Sie können später weitere Eigenschaften festlegen.

    ![Replikation aktivieren](./media/hyper-v-site-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

11. Wählen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren** die Replikationsrichtlinie aus, die Sie für die geschützten VMs anwenden möchten. Klicken Sie dann auf **OK**. Sie können die Replikationsrichtlinie unter **Replikationsrichtlinien** > Richtlinienname > **Einstellungen bearbeiten** ändern. Von Ihnen vorgenommene Änderungen werden für Computer, die bereits repliziert werden, und für neue Computer verwendet.


   ![Replikation aktivieren](./media/hyper-v-site-walkthrough-enable-replication/enable-replication7.png)

Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.


## <a name="next-steps"></a>Nächste Schritte


[Schritt 11: Ausführen eines Testfailovers](hyper-v-site-walkthrough-test-failover.md)

