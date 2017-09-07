---
title: "Aktivieren der Replikation in Azure für virtuelle Hyper-V-Computer in VMM-Clouds mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt, wie die Replikation in Azure für virtuelle Hyper-V-Computer in VMM-Clouds mit dem Azure Site Recovery-Dienst aktiviert wird."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 89a2c4fc-7e03-4a86-a2c0-52831ccebc1a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 96a817e43a830e836f2faa4603fc88ed9c0b1828
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="step-11-enable-replication-to-azure-for-hyper-v-vms-in-vmm-clouds"></a>Schritt 11: Aktivieren der Replikation in Azure für virtuelle Hyper-V-Computer in VMM-Clouds

Nach dem Einrichten einer Replikationsrichtlinie aktivieren Sie anhand der Informationen in diesem Artikel die Replikation für lokale virtuelle Hyper-V-Computer, die in System Center Virtual Machine Manager-Clouds (VMM) verwaltet werden, mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="before-you-start"></a>Vorbereitung

Stellen Sie sicher, dass Ihr Azure-Konto über die entsprechenden [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Erstellen virtueller Azure-Computer verfügt. [Weitere Informationen](../active-directory/role-based-access-built-in-roles.md) zur rollenbasierten Zugriffssteuerung in Azure.

## <a name="exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

Standardmäßig werden alle Datenträger auf einem Computer repliziert. Sie können Datenträger von der Replikation ausschließen. Beispielsweise möchten Sie möglicherweise nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart des Computers oder der Anwendung aktualisiert werden (z.B. pagefile.sys oder SQL Server tempdb), repliziert werden. [Weitere Informationen](site-recovery-exclude-disk.md)

## <a name="replicate-vms"></a>Replizieren von VMs

Aktivieren Sie die Replikation für VMs wie folgt:  

1. Klicken Sie auf **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**. Klicken Sie nach der erstmaligen Aktivierung der Replikation im Tresor auf **+Replizieren**, um die Replikation für weitere Computer zu aktivieren.

    ![Replikation aktivieren](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication1.png)
2. Wählen Sie auf dem Blatt **Quelle** den VMM-Server und die Cloud aus, in der sich die Hyper-V-Hosts befinden. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-source.png)
3. Wählen Sie unter **Ziel** das Abonnement, das Bereitstellungsmodell für die Zeit nach dem Failover und das Speicherkonto für die replizierten Daten aus.

    ![Replikation aktivieren](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication-target.png)
4. Wählen Sie das Speicherkonto aus, das Sie verwenden möchten. Falls Sie keines der bereits vorhandenen Speicherkonten verwenden möchten, können Sie ein [Speicherkonto erstellen](#set-up-an-azure-storage-account). Wenn Sie ein Storage Premium-Konto für replizierte Daten verwenden, müssen Sie ein weiteres Standardspeicherkonto zum Speichern von Replikationsprotokollen einrichten, mit denen laufende Änderungen lokaler Daten erfasst werden. Um mit dem Resource Manager-Modell ein Speicherkonto zu erstellen, klicken Sie auf **Neu erstellen**. Wenn Sie ein Speicherkonto mit dem klassischen Modell erstellen möchten, verwenden Sie das [Azure-Portal](../storage/common/storage-create-storage-account.md). Klicken Sie dann auf **OK**.
5. Wählen Sie das Azure-Netzwerk und das Subnetz aus, mit dem virtuelle Azure-Computer, die nach einem Failover erstellt werden, eine Verbindung herstellen. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Falls Sie keines der bereits vorhandenen Netzwerke verwenden möchten, können Sie [ein Netzwerk erstellen](#set-up-an-azure-network). Klicken Sie zum Erstellen eines Netzwerks mit dem Resource Manager-Modell auf **Neu erstellen**. Falls Sie ein Netzwerk mit dem klassischen Modell erstellen möchten, verwenden Sie hierfür das [Azure-Portal](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Wählen Sie, falls zutreffend, ein Subnetz aus. Klicken Sie dann auf **OK**.
6. Klicken Sie auf **Virtuelle Computer** > **Virtuelle Computer auswählen**, und wählen Sie die Computer aus, die Sie replizieren möchten. Sie können nur Computer auswählen, für die die Replikation aktiviert werden kann. Klicken Sie dann auf **OK**.

    ![Replikation aktivieren](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication5.png)

7. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Betriebssystem für die ausgewählten VMs und den Betriebssystem-Datenträger aus.

    - Stellen Sie sicher, dass der Name des virtuellen Azure-Computers (Zielname) den [Anforderungen für virtuelle Azure-Computer](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) entspricht.   
    - Standardmäßig werden alle Datenträger des virtuellen Computers für die Replikation ausgewählt, doch Sie können Datenträger löschen, um sie auszuschließen.

        - Möglicherweise möchten Sie Datenträger ausschließen, um die Replikationsbandbreite zu verringern. Beispielsweise möchten Sie vielleicht nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart eines Computers oder einer App aktualisiert werden (etwa „pagefile.sys“ oder Microsoft SQL Server tempdb), repliziert werden. Durch Aufheben der Auswahl eines Datenträgers können Sie ihn von der Replikation ausschließen.
        - Nur Basisdatenträger können ausgeschlossen werden. Betriebssystemdatenträger können Sie nicht ausschließen.
        - Es wird empfohlen, keine dynamischen Datenträger auszuschließen. Site Recovery kann nicht ermitteln, ob eine virtuelle Festplatte in einer Gast-VM einfach oder dynamisch ist. Wenn keiner der abhängigen dynamischen Volumendatenträger ausgeschlossen wird, werden geschützte dynamische Datenträger bei einem VM-Failover als fehlerhaft angesehen, und auf die Daten des Datenträgers kann nicht zugegriffen werden.
        - Nach Aktivierung der Replikation können Sie keine Datenträger für die Replikation hinzufügen oder entfernen. Wenn Sie einen Datenträger hinzufügen oder entfernen möchten, müssen Sie den Schutz für den virtuellen Computer deaktivieren und anschließend wieder aktivieren.
        - Für Datenträger, die Sie manuell in Azure erstellen, wird kein Failback durchgeführt. Wenn Sie also beispielsweise ein Failover für drei Datenträger durchführen und zwei Datenträger direkt auf dem virtuellen Azure-Computer erstellen, wird nur für die drei Datenträger, für die das Failover ausgeführt wurde, ein Failback von Azure zu Hyper-V durchgeführt. Manuell erstellte Datenträger können nicht in das Failback oder in die umgekehrte Replikation von Hyper-V zu Azure einbezogen werden.
        - Wenn Sie einen Datenträger ausschließen, der für den Betrieb einer Anwendung erforderlich ist, müssen Sie ihn nach dem Failover zu Azure manuell in Azure erstellen, damit die replizierte Anwendung ausgeführt werden kann. Alternativ können Sie Azure Automation in einen Wiederherstellungsplan integrieren, um den Datenträger während des Failovers des Computers zu erstellen.

    Klicken Sie zum Speichern der Änderungen auf **OK**. Sie können später weitere Eigenschaften festlegen.

    ![Replikation aktivieren](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication6-with-exclude-disk.png)

8. Wählen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren** die Replikationsrichtlinie aus, die Sie für die geschützten VMs anwenden möchten. Klicken Sie dann auf **OK**. Sie können die Replikationsrichtlinie unter **Replikationsrichtlinien** > <Richtlinienname> > **Einstellungen bearbeiten** ändern. Von Ihnen vorgenommene Änderungen werden für Computer, die bereits repliziert werden, und für neue Computer verwendet.

   ![Replikation aktivieren](./media/vmm-to-azure-walkthrough-enable-replication/enable-replication7.png)

Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 12: Ausführen eines Testfailovers](vmm-to-azure-walkthrough-test-failover.md).

