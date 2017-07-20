---
title: "Ausführen eines Testfailovers für die Hyper-V-Replikation (ohne System Center VMM) in Azure | Microsoft-Dokumentation"
description: "Fasst die erforderlichen Schritte zum Ausführen eines Testfailovers für die Replikation von Hyper-V-VMs in Azure mithilfe des Azure Site Recovery-Diensts zusammen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c9a4c3ca-84a0-4668-b700-9b0046202299
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: d62a4463bb24760e5abea7a870e6987e1275d0be
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="step-11-run-a-test-failover-for-hyper-v-replication-to-azure"></a>Schritt 11: Ausführen eines Testfailovers für die Hyper-V-Replikation in Azure

In diesem Artikel erfahren Sie, wie Sie einen Testfailover für lokale virtuelle Hyper-V-Computer (die nicht in System Center-VMM verwaltet werden) mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal ausführen.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="before-you-start"></a>Vorbereitung

Bevor Sie ein Testfailover ausführen, empfehlen wir Ihnen, die VM-Eigenschaften zu überprüfen und die erforderlichen Änderungen daran vorzunehmen. Sie finden die VM-Einstellungen unter **Replizierte Elemente**. Das Blatt **Zusammenfassung** enthält Informationen zu den Einstellungen und zum Status der Computer.

## <a name="managed-disk-considerations"></a>Überlegungen zu verwalteten Datenträgern

[Verwaltete Datenträger](../storage/storage-managed-disks-overview.md) vereinfachen die Datenträgerverwaltung für Azure-VMs durch die Verwaltung der Speicherkonten, die den VM-Datenträgern zugeordnet sind. 

- Verwaltete Datenträger werden erstellt und nur an den virtuellen Computer angefügt, wenn ein Failover in Azure stattfindet. Wenn Sie den Schutz aktivieren, werden Daten von lokalen virtuellen Computern in Speicherkonten repliziert.
- Verwaltete Datenträger können nur für virtuelle Computer erstellt werden, die über das Resource Manager-Bereitstellungsmodell bereitgestellt werden.
- Ein Failback von Azure zur lokalen Hyper-V-Umgebung wird für Computer mit verwalteten Datenträgern derzeit nicht unterstützt. Sie sollten **Verwaltete Datenträger verwenden** nur dann auf **Ja** festlegen, wenn Sie ausschließlich eine Migration (Failover in Azure ohne Failback) ausführen.
- Wenn diese Einstellung aktiviert ist, können nur Verfügbarkeitsgruppen in Ressourcengruppen ausgewählt werden, für die **Verwaltete Datenträger verwenden** aktiviert ist. Virtuelle Computer mit verwalteten Datenträgern müssen sich in Verfügbarkeitsgruppen befinden, wobei **Verwaltete Datenträger verwenden** auf **Ja** festgelegt sein muss. Wenn die Einstellung für virtuelle Computer nicht aktiviert ist, können nur Verfügbarkeitsgruppen in Ressourcengruppen ausgewählt werden, für die verwaltete Datenträger nicht aktiviert sind. [Weitere Informationen](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set)
- - Wenn das Speicherkonto, das Sie für die Replikation verwenden, mit der Speicherdienstverschlüsselung verschlüsselt wurde, können verwaltete Datenträger während eines Failovers nicht erstellt werden. In diesem Fall aktivieren Sie die Verwendung verwalteter Datenträger nicht, oder deaktivieren Sie den Schutz für den virtuellen Computer, und aktivieren Sie ihn erneut, um ein Speicherkonto zu verwenden, für das die Verschlüsselung nicht aktiviert ist. [Weitere Informationen](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption)

 
## <a name="network-considerations"></a>Netzwerküberlegungen
    
- Sie können für die IP-Zieladresse festlegen, dass sie nach dem Failover für den virtuellen Azure-Computer verwendet wird. Wenn Sie keine Adresse angeben, wird für den Computer, für den das Failover durchgeführt wurde, DHCP verwendet. Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, tritt beim Failover ein Fehler auf. Dieselbe Ziel-IP-Adresse kann für das Testfailover verwendet werden, wenn die Adresse im Testfailover-Netzwerk verfügbar ist.
- Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben. Hierbei gilt Folgendes:
    - Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
    - Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
    - Ein Beispiel: Wenn ein Quellcomputer zwei Netzwerkkarten besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkkarten unterstützt, erhält der Zielcomputer zwei Netzwerkkarten. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.     
- Wenn die VM über mehrere Netzwerkkarten verfügt, werden alle mit dem gleichen Netzwerk verbunden.
- Wenn der virtuelle Computer über mehrere Netzwerkadapter verfügt, wird der erste, der in der Liste angezeigt wird, zum *Standard*-Netzwerkadapter im virtuellen Azure-Computer.


## <a name="view-and-manage-vm-settings"></a>Anzeigen und Verwalten von VM-Einstellungen

Es wird empfohlen, dass Sie die Eigenschaften des Quellcomputers überprüfen, bevor Sie ein Failover ausführen.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente**, und klicken Sie auf den virtuellen Computer.

    ![Replikation aktivieren](./media/hyper-v-site-walkthrough-test-failover/test-failover1.png)
2. Im Bereich **Repliziertes Element** können Sie eine Zusammenfassung der Informationen über virtuelle Computer, den Integritätsstatus sowie die neu verfügbaren Wiederherstellungspunkte anzeigen. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.

    ![Replikation aktivieren](./media/hyper-v-site-walkthrough-test-failover/test-failover2.png)
3. Unter **Compute und Netzwerk** können Sie Folgendes:
    - den Namen des virtuellen Azure-Computers ändern. Der Name muss die [Azure-Anforderungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllen.
    - eine [Ressourcengruppe](../virtual-machines/windows/infrastructure-resource-groups-guidelines.md) nach dem Failover angeben
    - eine Zielgröße für den virtuellen Azure-Computer angeben
    - eine [Verfügbarkeitsgruppe](../virtual-machines/windows/infrastructure-availability-sets-guidelines.md) auswählen
    - angeben, ob [verwaltete Datenträger](#managed-disk-considerations) verwendet werden sollen. Wählen Sie **Ja** aus, wenn Sie Ihrem Computer bei der Migration zu Azure verwaltete Datenträger hinzufügen möchten.
    - Netzwerkeinstellungen, einschließlich des Netzwerks/Subnetzes anzeigen oder ändern, in dem der virtuelle Azure-Computer nach dem Failover platziert wird, sowie die IP-Adresse, die ihm zugewiesen wird.

    ![Replikation aktivieren](./media/hyper-v-site-walkthrough-test-failover/test-failover4.png)
4. Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.


## <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers

Führen Sie ein Testfailover aus, um sicherzustellen, dass alles wie erwartet funktioniert.

- Wenn Sie die Verbindung mit Azure-VMs nach dem Failover per RDP herstellen möchten, informieren Sie sich über die [Vorbereitung auf das Herstellen der Verbindung](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Für den vollständigen Test müssen Sie Active Directory und DNS in Ihre Testumgebung kopieren. [Weitere Informationen](site-recovery-active-directory.md#test-failover-considerations)
 - Vollständige Informationen zum Testfailover finden Sie [in diesem Artikel](site-recovery-test-failover-to-azure.md).
 
 Führen Sie nun ein Failover aus:

1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen Computer unter **Replizierte Elemente** auf den virtuellen Computer und dann auf das Symbol **+Testfailover**.
2. Klicken Sie zum Ausführen eines Failovers für einen Wiederherstellungsplan unter **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie anschließend auf **Testfailover**. Eine Anleitung zum Erstellen eines Wiederherstellungsplans finden Sie [hier](site-recovery-create-recovery-plans.md).
3. Wählen Sie unter **Testfailover** das Azure-Netzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.
4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt verfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ können Sie unter „Tresorname“ > > **Aufträge** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.
5. Nach Abschluss des Failovers sollte der Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
6. Wenn Sie die Vorbereitung für Verbindungen nach dem Failover durchgeführt haben, sollten Sie eine Verbindung mit dem virtuellen Azure-Computer herstellen können.
7. Klicken Sie anschließend auf dem Wiederherstellungsplan auf **Cleanup Test Failover** (Testfailover bereinigen). Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Test-Failover. Dadurch werden die während des Testfailovers erstellten virtuellen Computer gelöscht.



## <a name="next-steps"></a>Nächste Schritte

- [Informieren Sie sich](site-recovery-failover.md) über die unterschiedlichen Failoverarten und deren Durchführung.
- [Informieren Sie sich über Failbacks](site-recovery-failback-from-azure-to-hyper-v.md), um für Azure-VMs Failbacks und Replikationen zurück an den primären lokalen Hyper-V-Standort durchzuführen.


