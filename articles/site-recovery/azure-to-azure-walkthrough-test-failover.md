---
title: "Ausführen eines Testfailovers für die Replikation virtueller Azure-Computer mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Fasst die erforderlichen Schritte zum Ausführen eines Testfailovers für die Replikation virtueller Azure-Computer in eine andere Azure-Region mithilfe des Azure Site Recovery-Diensts zusammen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: e15c1b0c-5d75-4fdf-acb0-e61def9e9339
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: 8babb0d016729f318442af93596d206c38d91206
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---

# <a name="step-6-run-a-test-failover-for-azure-vm-replication"></a>Schritt 6: Ausführen eines Testfailovers für die Replikation virtueller Azure-Computer

Nachdem Sie die Replikation für virtuelle Azure-Computer (Virtual Machines, VMs) aktiviert haben, befolgen Sie die Schritte in diesem Artikel, um mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal ein Testfailover aus einer Azure-Region in eine andere auszuführen.

- Am Ende des Artikels sollten Sie mit einem Testfailover überprüft haben, dass zumindest für einen virtuellen Azure-Computer ein Failover in Ihre sekundäre Azure-Region möglich ist. 
- Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.

>[!NOTE]
>
> Die Azure-VM-Replikation ist derzeit als Vorschau verfügbar.


## <a name="before-you-start"></a>Vorbereitung

- Bevor Sie ein Testfailover ausführen, empfehlen wir Ihnen, die VM-Eigenschaften zu überprüfen und die erforderlichen Änderungen daran vorzunehmen. Sie finden die VM-Einstellungen unter **Replizierte Elemente**. Das Blatt **Zusammenfassung** enthält Informationen zu den Einstellungen und zum Status der Computer.
- Es wird empfohlen, ein separates Azure-VM-Netzwerk für das Testfailover zu nutzen und nicht das Netzwerk (Standard oder angepasst), das für das Produktionsfailover eingerichtet wurde.
- Das Testfailover führt Failover virtueller Azure-Computer (und deren Speicher) in die sekundäre Azure-Region aus. Es repliziert keine abhängigen Anwendungen oder Ressourcen. Wenn Anwendungen, die auf Failover-VMs ausgeführt werden, von anderen Ressourcen wie Active Directory oder DNS abhängig sind, müssen Sie auch diese replizieren, wenn sie nicht bereits in Ihrer sekundären Region verfügbar sind. [Weitere Informationen](site-recovery-test-failover-to-azure.md#prepare-active-directory-and-dns)
- Wenn Sie nach dem Failover auf replizierte VMs von einem lokalen Standort aus zugreifen wollen, müssen Sie die Verbindung zu diesen VMs [vorbereiten](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).

## <a name="run-a-test-failover"></a>Durchführen eines Test-Failovers

1. Klicken Sie unter **Einstellungen** > **Replizierte Elemente** auf das VM-Symbol **+Testfailover**. 

2. Wählen Sie unter **Testfailover** einen Wiederherstellungspunkt für das Failover aus:

    - **Letzte Verarbeitung**: Führt ein Failover des virtuellen Computers auf den letzten Wiederherstellungspunkt aus, der vom Site Recovery-Dienst verarbeitet wurde. Der Zeitstempel wird angezeigt. Mit dieser Option wird keine Zeit für die Verarbeitung von Daten verwendet und die Recovery Time Objective (RTO) niedrig gehalten.
    - **Letzte App-Konsistenz**: Diese Option führt ein Failover aller virtuellen Computer auf den letzten App-konsistenten Wiederherstellungspunkt aus. Der Zeitstempel wird angezeigt. 
    - **Benutzerdefiniert**: Wählen Sie einen beliebigen Wiederherstellungspunkt aus.
 
3. Wählen Sie das virtuelle Azure-Zielnetzwerk aus, mit dem Azure-VMs in der sekundären Region nach dem Failover verbunden werden.
4. Um das Failover zu starten, klicken Sie auf **OK**. Klicken Sie zum Überwachen des Fortschritts auf die VM, um die Eigenschaften aufzurufen. Alternativ können Sie auf den Auftrag **Testfailover** unter dem Tresornamen und dann auf **Einstellungen** > **Aufträge** > **Site Recovery-Aufträge** klicken.
5. Nach Abschluss des Failovers wird der virtuelle Azure-Replikatcomputer im Azure-Portal unter **Virtuelle Computer** angezeigt. Stellen Sie sicher, dass die VM die richtige Größe hat, mit dem richtigen Netzwerk verbunden ist und ausgeführt wird.
6. Um die VMs zu löschen, die beim Testfailover erstellt wurden, klicken Sie im replizierten Element oder im Wiederherstellungsplan auf **Testfailover bereinigen**. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover. 

[Erfahren Sie mehr](site-recovery-test-failover-to-azure.md) über Testfailover.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie das Failover getestet haben, ist diese exemplarische Vorgehensweise abgeschlossen. Erfahren Sie nun, wie Sie Failover in der Produktion ausführen:

- [Informieren Sie sich](site-recovery-failover.md) über die unterschiedlichen Failoverarten und deren Durchführung.
- Erfahren Sie mehr über das Failover mehrerer VMs [mithilfe eines Wiederherstellungsplans](site-recovery-create-recovery-plans.md).
- Erfahren Sie mehr über die [Verwendung von Wiederherstellungsplänen](site-recovery-create-recovery-plans.md).
- Erfahren Sie mehr über das [erneute Schützen von virtuellen Azure-Computern](site-recovery-how-to-reprotect.md) nach einem Failover.


