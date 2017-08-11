---
title: Aktivieren der Replikation von Azure-VMs in einer anderen Azure-Region mit Azure Site Recovery | Microsoft-Dokumentation
description: "In diesem Artikel wird zusammengefasst, wie Sie die Replikation von Azure-VMs in einer anderen Azure-Region mit dem Azure Site Recovery-Dienst aktivieren können."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a309644f-d36b-4188-bba7-ad45a2d9bede
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 8/01/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: f426ba4341e61d3c7da820d7d5097b217e94ca0e
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---


# <a name="step-5-enable-replication-for-azure-vms"></a>Schritt 5: Aktivieren der Replikation für Azure-VMs


Nachdem Sie einen [Recovery Services-Tresor](azure-to-azure-walkthrough-vault.md) eingerichtet haben, aktivieren mithilfe der Anweisungen in diesem Artikel die Replikation von virtuellen Computern (VMs) in einer anderen Azure-Region mit [Azure Site Recovery](site-recovery-overview.md). Um die Replikation zu ermöglichen, richten Sie die Quell- und Zieleinstellungen ein, überprüfen Sie die Replikationsrichtlinie, und wählen Sie die VMs aus, die Sie replizieren möchten.

- Wenn Sie diesen Artikel durchgearbeitet haben, sollten Ihre virtuellen Azure-Computer in der sekundären Azure-Region repliziert werden.
- Geben Sie am Ende dieses Artikels Kommentare ein, oder stellen Sie Fragen im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

>[!NOTE]
>
> Die Azure-VM-Replikation befindet sich derzeit in der Vorschau.


## <a name="select-the-source"></a>Auswählen der Quelle 

1. Klicken Sie unter „Recovery Services-Tresore“ auf den Tresornamen > **+Replizieren**.
2. Wählen Sie unter **Quelle** die Option **Azure – VORSCHAU**.
2. Wählen Sie unter **Quellstandort** die Azure-Quellregion aus, in der Ihre VMs derzeit ausgeführt werden.
3. Wählen Sie das **Azure-VM-Bereitstellungsmodell** für Ihre VMs: **Resource Manager** oder **Klassisch**.
4. Wählen Sie für Resource Manager-VMs die **Quellressourcengruppe** oder für klassische VMs den **Clouddienst**.
5. Klicken Sie auf **OK** , um die Einstellungen zu speichern.

    ![Konfigurieren der Quelle](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-the-vms"></a>Auswählen der virtuellen Computer

Site Recovery ruft eine Liste der virtuellen Computer ab, die dem Abonnement und der Ressourcengruppe bzw. dem Clouddienst zugeordnet sind.

1. Wählen Sie unter **Virtuelle Computer** die VMs aus, die Sie replizieren möchten.
2. Klicken Sie auf **OK**.

    ![Auswählen von VMs](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a>Konfigurieren von Einstellungen

Site Recovery stellt Standardeinstellungen für die Zielregion (basierend auf den Einstellungen der Quellregion) und die Replikationsrichtlinie bereit:

   - **Zielspeicherort**: Die Zielregion, die Sie zur Notfallwiederherstellung verwenden möchten. Der Zielspeicherort sollte mit dem Speicherort des Site Recovery-Tresors übereinstimmen.
   - **Zielressourcengruppe**: Die Ressourcengruppe, zu der Azure-VMs in der Zielregion nach einem Failover gehören werden. Site Recovery erstellt standardmäßig in der Zielregion eine neue Ressourcengruppe mit dem Suffix „asr“. 
   - **Virtuelles Zielnetzwerk**: Das Netzwerk, in dem sich Azure-VMs nach einem Failover in der Zielregion befinden werden. Site Recovery erstellt standardmäßig in der Zielregion ein neues virtuelles Netzwerk (und Subnetze) mit dem Suffix „asr“. Dieses Netzwerk wird Ihrem Quellnetzwerk zugeordnet. Beachten Sie, dass Sie einem virtuellen Computer nach einem Failover eine bestimmte IP-Adresse zuweisen können, wenn Sie die gleiche IP-Adresse an Quell- und Zielspeicherort beibehalten müssen. 
   - **Cachespeicherkonten**: Site Recovery verwendet ein Speicherkonto in der Quellregion. Änderungen an Quell-VMs werden vor der Replikation am Zielspeicherort an dieses Konto gesendet. 
   - **Zielspeicherkonten**: Standardmäßig erstellt Site Recovery ein neues Speicherkonto in der Zielregion, um das Quell-VM-Speicherkonto zu spiegeln.
   -  **Zielverfügbarkeitsgruppen**: Standardmäßig erstellt Site Recovery in der Zielregion eine neue Verfügbarkeitsgruppe mit dem Suffix „asr“. 
   - **Replikationsrichtlinienname**: Name der Richtlinie.
   - **Aufbewahrungszeitraum des Wiederherstellungspunkts**: Standardmäßig behält Site Recovery Wiederherstellungspunkte 24 Stunden lang bei. Sie können einen Wert zwischen 1 und 72 Stunden konfigurieren.
   - **App-konsistente Momentaufnahmenhäufigkeit**: Standardmäßig erstellt Site Recovery alle 4 Stunden eine App-konsistente Momentaufnahme. Sie können einen Wert zwischen 1 und 12 Stunden konfigurieren. Daten werden fortlaufend repliziert:
    - Wenn absturzkonsistente Wiederherstellungspunkte erstellt werden, behalten sie eine konsistente Schreibreihenfolge für Daten bei. Diese Art von Wiederherstellungspunkt ist in der Regel ausreichend, wenn Ihre App für die Wiederherstellung nach einem Absturz ohne Dateninkonsistenzen konzipiert ist.
    - Absturzkonsistente Wiederherstellungspunkte werden alle paar Minuten generiert. Die Verwendung dieser Wiederherstellungspunkte für Failover und Wiederherstellung Ihrer virtuellen Computer stellt innerhalb von Minuten eine Recovery Point Objective (RPO) bereit.
    - App-konsistente Wiederherstellungspunkte (zusätzlich zur Konsistenz der Schreibreihenfolge) stellen sicher, dass ausgeführte Apps alle Vorgänge abschließen und Puffer auf Datenträger leeren (Stilllegen der Anwendung). Sie sollten diese Wiederherstellungspunkte für Datenbank-Apps wie SQL Server, Oracle und Exchange verwenden.
        
    ![Konfigurieren von Einstellungen](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a>Ändern von Einstellungen

Wenn Sie Ziel- und Replikationsrichtlinieneinstellungen ändern möchten, führen Sie folgende Schritte aus:

1. Klicken Sie zum Anzeigen oder Ändern von Zieleinstellungen auf **Einstellungen**.
2. Um die Standardeinstellungen für das Ziel außer Kraft zu setzen, klicken Sie auf **Anpassen**. Sie können Zielressourcengruppe, virtuelles Netzwerk, Verfügbarkeitsgruppe und Zielspeicherkonto angeben. Sie können Verfügbarkeitsgruppen nur dann hinzufügen, wenn virtuelle Computer zu einer Gruppe in der Quellregion gehören.

    ![Konfigurieren von Einstellungen](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. Um Replikationseinstellungen für Wiederherstellungspunkte und App-konsistente Momentaufnahmen außer Kraft zu setzen, klicken Sie neben **Replikationsrichtlinie** auf **Anpassen**.
 
    ![Konfigurieren von Einstellungen](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. Um mit der Bereitstellung der Zielressourcen zu beginnen, klicken Sie auf **Zielressourcen erstellen**. Die Bereitstellung dauert ungefähr einige Minuten. Schließen Sie das Blatt während der Bereitstellung nicht, da Sie sonst von vorne anfangen müssen.




## <a name="enable-replication"></a>Replikation aktivieren

1. Klicken Sie in **Einstellungen** auf **Replikation aktivieren**. Dies ermöglicht die erste Replikation der virtuellen Computer, die Sie ausgewählt haben. Die Aktualisierung des Status der Erstreplikation kann einige Zeit dauern. Klicken Sie zum Abrufen des aktuellen Status auf **Aktualisieren**.

2. Sie können den Fortschritt des Auftrags **Schutz aktivieren** unter **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** verfolgen.

3. Unter **Einstellungen** > **Replizierte Elemente** können Sie den Status der VMs und den der ersten Replikation einsehen. Klicken Sie auf die VM, um ein Drilldown auf die zugehörigen Einstellungen auszuführen.



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 6: Ausführen eines Testfailovers](azure-to-azure-walkthrough-test-failover.md).

