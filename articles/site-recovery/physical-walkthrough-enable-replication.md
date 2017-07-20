---
title: Aktivieren der Replikation physischer Server nach Azure mit Azure Site Recovery| Microsoft-Dokumentation
description: "Hier erhalten Sie eine Zusammenfassung der Schritte, die für physische Server zum Aktivieren der Replikation nach Azure mithilfe des Azure Site Recovery-Diensts ausgeführt werden müssen"
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
ms.openlocfilehash: 42f35c53eec06a346281fd90c97aecfd2269307d
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-10-enable-replication-for-physical-servers-to-azure"></a>Schritt 10: Aktivieren der Replikation für physische Server nach Azure


In diesem Artikel wird beschrieben, wie die Replikation von lokalen Windows/Linux-Servern nach Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal aktiviert wird.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="before-you-start"></a>Vorbereitung

- Auf Servern muss die [Mobility-Dienstkomponente installiert sein](physical-walkthrough-install-mobility.md).
- Wenn eine VM für die Pushinstallation vorbereitet wurde, installiert der Prozessserver automatisch den Mobilitätsdienst, sobald Sie die Replikation aktivieren.
- Wenn Sie einen Computer für die Replikation aktivieren, benötigt Ihr Azure-Benutzerkonto spezielle [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines), damit sichergestellt ist, dass Sie Azure-Speicher nutzen und Azure-VMs erstellen können.
- Beim Hinzufügen oder Ändern von IP-Adressen für Server kann es 15 Minuten oder auch länger dauern, bis Änderungen wirksam werden und im Portal sichtbar sind.


## <a name="exclude-disks-from-replication"></a>Ausschließen von Datenträgern von der Replikation

Standardmäßig werden alle Datenträger auf einem Computer repliziert. Sie können Datenträger von der Replikation ausschließen. Beispielsweise möchten Sie möglicherweise nicht, dass Datenträger mit temporären Daten oder Daten, die bei jedem Neustart des Computers oder der Anwendung aktualisiert werden (z.B. pagefile.sys oder SQL Server tempdb), repliziert werden. [Weitere Informationen](site-recovery-exclude-disk.md)

## <a name="replicate-servers"></a>Replizieren von Servern

1. Klicken Sie auf **Schritt 2: Replizieren Sie die Anwendung** > **Quelle**.
2. Wählen Sie unter **Quelle** den Konfigurationsserver aus.
3. Wählen Sie unter **Computertyp** die Option **Physische Computer**.
4. Wählen Sie den Prozessserver aus. Wenn Sie keine zusätzlichen Prozessserver erstellt haben, ist dies der Konfigurationsserver. Klicken Sie dann auf **OK**.
5. Wählen Sie unter **Ziel** das Abonnement und die Ressourcengruppe aus, in dem bzw. der Sie die VMs erstellen möchten, für die ein Failover durchgeführt wurde. Wählen Sie das Bereitstellungsmodell aus, das in Azure für die VMs verwendet werden soll, für die ein Failover durchgeführt wurde (klassisch oder Resource Manager).
6. Wählen Sie das Azure-Speicherkonto aus, das Sie für die Replikation von Daten verwenden möchten. Falls Sie kein bereits eingerichtetes Konto verwenden möchten, können Sie ein neues Konto erstellen.
7. Wählen Sie das **Azure-Netzwerk** und das **Subnetz** aus, mit dem virtuelle Azure-Computer nach dem Failover eine Verbindung herstellen. Wählen Sie die Option **Jetzt für die ausgewählten Computer konfigurieren** aus, um die Netzwerkeinstellung auf alle Computer anzuwenden, die geschützt werden sollen. Wählen Sie **Später konfigurieren** aus, um das Azure-Netzwerk pro Computer auszuwählen. Falls Sie kein vorhandenes Netzwerk verwenden möchten, können Sie ein Netzwerk erstellen.

    ![Replikation aktivieren](./media/physical-walkthrough-enable-replication/targetsettings.png)

8. Klicken Sie unter **Physische Computer** auf **+Physischer Computer**, und geben Sie den **Namen** und die **IP-Adresse** ein. Wählen Sie das Betriebssystem des Computers, den Sie replizieren möchten. Es dauert einige Minuten, bis die Computer ermittelt und in der Liste angezeigt werden.
9. Geben Sie unter **Eigenschaften** > **Eigenschaften konfigurieren**das Konto aus, das der Prozessserver zum automatischen Installieren des Mobilitätsdiensts auf dem Computer verwenden soll.
10. Standardmäßig werden alle Datenträger repliziert. Klicken Sie auf **Alle Datenträger** , und entfernen Sie alle Datenträger, die Sie nicht replizieren möchten. Klicken Sie dann auf **OK**. Sie können später weitere VM-Eigenschaften festlegen.

    ![Replikation aktivieren](./media/physical-walkthrough-enable-replication/enable-replication6.png)
11. Überprüfen Sie unter **Replikationseinstellungen** > **Replikationseinstellungen konfigurieren**, ob die richtige Replikationsrichtlinie ausgewählt ist. Wenn Sie eine Richtlinie ändern, werden die Änderungen auf den replizierenden Computer und neue Computer angewendet.
12. Aktivieren Sie **Multi-VM-Konsistenz** , wenn Sie Computer in einer Replikationsgruppe zusammenfassen möchten, und geben Sie einen Namen für die Gruppe an. Klicken Sie dann auf **OK**. Beachten Sie Folgendes:

    * Computer in Replikationsgruppen werden gemeinsam repliziert und verfügen beim Failover über gemeinsame ausfallsichere und anwendungskonsistente Wiederherstellungspunkte.
    * Es wird empfohlen, dass Sie physische Server zusammenfassen, damit sie Ihre Workloads widerspiegeln. Das Aktivieren von Multi-VM-Konsistenz kann sich auf die Leistung der Workload auswirken und sollte nur verwendet werden, wenn Computer die gleiche Workload ausführen und Sie Konsistenz benötigen.

13. Klicken Sie auf **Replikation aktivieren**. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen. Nachdem der Auftrag **Schutz abschließen** ausgeführt wurde, ist der Computer bereit für das Failover.

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 11: Ausführen eines Testfailovers](physical-walkthrough-test-failover.md).

