---
title: "Ausführen eines Testfailovers für die Replikation physischer Server zu Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Fasst die erforderlichen Schritte zum Ausführen eines Testfailovers für die Replikation physischer Server in Azure mithilfe des Azure Site Recovery-Diensts zusammen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a640e139-3a09-4ad8-8283-8fa92544f4c6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: e6ebab3e4d7deeefbab395b0a898fbf441d75b5d
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-11-run-a-test-failover-of-physical-servers-to-azure"></a>Schritt 11: Ausführen eines Testfailovers physischer Server zu Azure

In diesem Artikel erfahren Sie, wie Sie ein Testfailover von lokalen physischen Servern zu Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal ausführen.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="before-you-start"></a>Vorbereitung

Bevor Sie ein Testfailover ausführen, empfehlen wir Ihnen, die Servereigenschaften zu überprüfen und die erforderlichen Änderungen daran vorzunehmen. Sie finden die VM-Einstellungen unter **Replizierte Elemente**. Das Blatt **Zusammenfassung** enthält Informationen zu den Einstellungen und zum Status der Computer.

## <a name="managed-disk-considerations"></a>Überlegungen zu verwalteten Datenträgern

[Verwaltete Datenträger](../storage/storage-managed-disks-overview.md) vereinfachen die Datenträgerverwaltung für virtuelle Azure-Computer durch die Verwaltung der Speicherkonten, die den VM-Datenträgern zugeordnet sind. 

- Wenn Sie den Schutz für einen Server aktivieren, werden die Daten des virtuellen Computers in ein Speicherkonto repliziert. Verwaltete Datenträger werden erstellt und nur an den virtuellen Computer angefügt, wenn ein Failover stattfindet.
- Verwaltete Datenträger können nur für virtuelle Computer erstellt werden, die mithilfe des Resource Manager-Modells bereitgestellt wurden.  
- Wenn diese Einstellung aktiviert ist, können nur Verfügbarkeitsgruppen in Ressourcengruppen ausgewählt werden, für die **Verwaltete Datenträger verwenden** aktiviert ist. Virtuelle Computer mit verwalteten Datenträgern müssen sich in Verfügbarkeitsgruppen befinden, wobei **Verwaltete Datenträger verwenden** auf **Ja** festgelegt sein muss. Wenn die Einstellung für virtuelle Computer nicht aktiviert ist, können nur Verfügbarkeitsgruppen in Ressourcengruppen ohne aktivierte verwaltete Datenträger ausgewählt werden.
- [Hier](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set) finden Sie weitere Informationen zu verwalteten Datenträgern und Verfügbarkeitsgruppen.
- Wenn das Speicherkonto, das Sie für die Replikation verwenden, mit Storage Service Encryption verschlüsselt wurde, können verwaltete Datenträger während eines Failovers nicht erstellt werden. In diesem Fall aktivieren Sie die Verwendung verwalteter Datenträger nicht, oder Sie deaktivieren den Schutz für den virtuellen Computer, und aktivieren Sie ihn erneut, um ein Speicherkonto zu nutzen, für das die Verschlüsselung nicht aktiviert ist. [Weitere Informationen](https://docs.microsoft.com/azure/storage/storage-managed-disks-overview#managed-disks-and-encryption)


## <a name="network-considerations"></a>Netzwerküberlegungen

Sie können die Ziel-IP-Adresse für einen nach einem Failover erstellten virtuellen Azure-Computer festlegen.

- Wenn Sie keine Adresse angeben, wird für den Computer, für den das Failover durchgeführt wurde, DHCP verwendet.
- Wenn Sie eine Adresse festlegen, die beim Failover nicht verfügbar ist, funktioniert das Failover nicht.
- Die gleiche Ziel-IP-Adresse kann für das Testfailover verwendet werden, wenn die Adresse im Testfailover-Netzwerk verfügbar ist.
- Die Anzahl der Netzwerkkarten hängt von der Größe ab, die Sie für den virtuellen Zielcomputer angeben:

     - Wenn die Anzahl der Netzwerkkarten des Quellcomputers maximal der Anzahl der Netzwerkkarten entspricht, die für die Größe des Zielcomputers zulässig ist, hat der Zielcomputer die gleiche Anzahl von Netzwerkkarten wie der Quellcomputer.
     - Wenn die Anzahl der Netzwerkadapter für den virtuellen Quellcomputer die maximal zulässige Anzahl für die Größe des Zielcomputers übersteigt, wird die Anzahl verwendet, die maximal für die Größe des Zielcomputers zulässig ist.
     - Wenn beispielsweise ein Quellcomputer zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe vier Netzwerkadapter unterstützt, erhält der Zielcomputer zwei Netzwerkadapter. Wenn der Quellcomputer dagegen zwei Netzwerkadapter besitzt und der Zielcomputer aufgrund seiner Größe nur einen Adapter unterstützt, erhält der Zielcomputer nur einen Adapter.     
   - Wenn der virtuelle Computer über mehrere Netzwerkadapter verfügt, werden alle mit dem gleichen Netzwerk verbunden.
   - Wenn der virtuelle Computer über mehrere Netzwerkadapter verfügt, wird der erste, der in der Liste angezeigt wird, zum *Standard*-Netzwerkadapter im virtuellen Azure-Computer.
 - [Hier](vmware-walkthrough-network.md) finden Sie weitere Informationen zur IP-Adressierung.



## <a name="view-and-modify-vm-settings"></a>Anzeigen und Ändern von Einstellungen virtueller Computer

Es wird empfohlen, dass Sie die Eigenschaften des Quellservers überprüfen, bevor Sie ein Failover ausführen.

1. Klicken Sie unter **Geschützte Elemente** auf **Replizierte Elemente**, und klicken Sie auf den Computer.
2. Im Bereich **Repliziertes Element** können Sie eine Zusammenfassung der Computerinformationen, den Integritätsstatus sowie die aktuellen verfügbaren Wiederherstellungspunkte anzeigen. Klicken Sie auf **Eigenschaften**, um weitere Details anzuzeigen.
3. Unter **Compute und Netzwerk** können Sie die folgenden Aktionen ausführen:
    - Ändern des Namens des virtuellen Azure-Computers. Der Name muss die [Azure-Anforderungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllen.
    - Angeben einer [Ressourcengruppe](../virtual-machines/windows/infrastructure-resource-groups-guidelines.md) nach dem Failover
    - Angeben einer Zielgröße für den virtuellen Azure-Computer
    - Auswählen einer [Verfügbarkeitsgruppe](../virtual-machines/windows/infrastructure-availability-sets-guidelines.md)
    - Angeben, ob [verwaltete Datenträger](#managed-disk-considerations) verwendet werden sollen. Wählen Sie **Ja** aus, wenn Sie Ihrem Computer bei der Migration zu Azure verwaltete Datenträger hinzufügen möchten.
    - Anzeigen oder Ändern von Netzwerkeinstellungen, einschließlich des Netzwerks/Subnetzes, in dem der virtuelle Azure-Computer nach dem Failover platziert wird, sowie die IP-Adressen, die ihm zugewiesen werden.
4. Unter **Datenträger** finden Sie Informationen über das Betriebssystem und die Datenträger auf dem virtuellen Computer.

## <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers

Führen Sie nach Abschluss der Einrichtung ein Testfailover durch, um sicherzustellen, dass alles wie erwartet funktioniert.

- Wenn Sie die Verbindung mit virtuellen Azure-Computern nach dem Failover per RDP herstellen möchten, [bereiten Sie die Verbindungsherstellung vor](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).
 - Für den vollständigen Test müssen Sie Active Directory und DNS in Ihre Testumgebung kopieren. [Weitere Informationen](site-recovery-active-directory.md#test-failover-considerations)
 - Vollständige Informationen zum Testfailover finden Sie [in diesem Artikel](site-recovery-test-failover-to-azure.md).
- Bevor Sie beginnen, verschaffen Sie sich schnell einen Überblick per Video:

     
     >[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video4-Recovery-Plan-DR-Drill-and-Failover/player]

Führen Sie nun ein Failover aus:

1. Klicken Sie zum Durchführen eines Failovers für einen einzelnen Computer unter **Einstellungen** > **Replizierte Elemente** auf den Computer und dann auf das Symbol **+Testfailover**.

    ![Testfailover](./media/physical-walkthrough-test-failover/test-failover.png)

2. Klicken Sie zum Durchführen eines Failovers für einen Wiederherstellungsplan unter **Einstellungen** > **Wiederherstellungspläne** mit der rechten Maustaste auf den Plan, und klicken Sie dann auf **Testfailover**. Eine Anleitung zum Erstellen eines Wiederherstellungsplans finden Sie [hier](site-recovery-create-recovery-plans.md).  

3. Wählen Sie unter **Testfailover** das Azure-Netzwerk aus, mit dem Azure-VMs nach dem Failover verbunden werden.

4. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Sie können den Fortschritt nachverfolgen, indem Sie auf den virtuellen Computer klicken, um die Eigenschaften zu öffnen. Alternativ können Sie unter „Tresorname“ > **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** auf den Auftrag **Testfailover** klicken.

5. Nach Abschluss des Failovers sollte der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt werden. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.

6. Wenn Sie die Vorbereitung für Verbindungen nach dem Failover durchgeführt haben, sollten Sie eine Verbindung mit dem virtuellen Azure-Computer herstellen können.

### <a name="delete-test-failover-vms"></a>Löschen der für das Testfailover erstellten virtuellen Computer

1. Klicken Sie anschließend auf dem Wiederherstellungsplan oder dem Computer auf **Cleanup Test Failover** (Testfailover bereinigen).
2. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover.
3. Durch die Bereinigung werden die während des Testfailovers erstellten virtuellen Computer gelöscht.

## <a name="summary"></a>Zusammenfassung

Wenn Sie das Testfailover erfolgreich abgeschlossen haben, werden Ihre physischen Server repliziert, und Sie haben sichergestellt, dass ein Failover auf Azure möglich ist. Nun können Sie Failover in Übereinstimmung mit den Anforderungen Ihres Unternehmens ausführen. 

Bitte beachten Sie, dass ein Failback von Azure auf einen physischen Server derzeit nicht möglich ist. Das Failback muss auf einen virtuellen VWware-Computer ausgeführt werden. Daher benötigen Sie eine lokale VMware-Infrastruktur, damit Sie ein Failback ausführen können. [Hier](site-recovery-failback-azure-to-vmware.md) finden Sie weitere Informationen zu Failbacks von virtuellen Azure-Computern auf VMware.


## <a name="next-steps"></a>Nächste Schritte

- Führen Sie [Failover](site-recovery-failover.md) nach Bedarf durch.

