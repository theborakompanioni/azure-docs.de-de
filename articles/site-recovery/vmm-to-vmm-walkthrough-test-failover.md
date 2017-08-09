---
title: "Ausführen eines Testfailovers für die Replikation virtueller Hyper-V-Computer an einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt, wie ein Testfailover für die Replikation virtueller Hyper-V-Computer an einem sekundären System Center VMM-Standort mit Azure Site Recovery ausgeführt wird."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a3fd11ce-65a1-4308-8435-45cf954353ef
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 23d235d326273e7ec59feee6588a39f685401e52
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-10-run-a-test-failover-for-hyper-v-replication-to-a-secondary-site"></a>Schritt 10: Ausführen eines Testfailovers für die Hyper-V-Replikation an einem sekundären Standort


Nachdem Sie die Replikation für virtuelle Hyper-V-Computer mit [Azure Site Recovery](site-recovery-overview.md) aktiviert haben, führen Sie anhand der Informationen in diesem Artikel ein Testfailover aus. Bei einem Testfailover wird überprüft, ob die Replikation funktioniert, ohne dass sich dies auf die Produktionsumgebung auswirkt. 


Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.


## <a name="before-you-start"></a>Vorbereitung

- Beim Auslösen eines Testfailovers können Sie das Netzwerk angeben, mit dem die virtuellen Testreplikatcomputer eine Verbindung herstellen. [Erfahren Sie mehr](site-recovery-test-failover-vmm-to-vmm.md#network-options-in-site-recovery) über die Netzwerkoptionen.
- Es wird empfohlen, dass Sie kein Netzwerk auswählen, das Sie während der Netzwerkzuordnung ausgewählt haben.
- Die Anweisungen in diesem Artikel beziehen sich auf das Failover eines einzelnen virtuellen Computers. Wenn Sie ein Failover für mehrere virtuelle Computer zusammen ausführen möchten, finden Sie entsprechende Informationen unter [Erstellen von Wiederherstellungsplänen](site-recovery-create-recovery-plans.md).
- Machen Sie sich mit den [Einschränkungen für Testfailover](site-recovery-test-failover-vmm-to-vmm.md#things-to-note) vertraut.
- Die IP-Adresse, die einem virtuellen Computer bei einem Testfailover zugewiesen wird, ist mit der IP-Adresse identisch, die dem virtuellen Computer bei einem geplanten oder nicht geplanten Failover zugewiesen wird (sofern die IP-Adresse im Testfailovernetzwerk verfügbar ist). Wenn die IP-Adresse nicht im Testfailovernetzwerk verfügbar ist, wird dem virtuellen Computer eine andere IP-Adresse zugewiesen, die im Testfailovernetzwerk verfügbar ist.
- Wenn Sie ein Testfailover in der Produktionsumgebung ausführen möchten, ist für die vollständige Überprüfung der End-to-End-Netzwerkkonnektivität der Computer Folgendes zu beachten:
    - Der primäre virtuelle Computer sollte beim Ausführen des Testfailovers heruntergefahren werden. Andernfalls werden zwei virtuelle Computer mit derselben Identität gleichzeitig im selben Netzwerk ausgeführt. 
    - Wenn Sie Änderungen auf virtuellen Testcomputern vornehmen, gehen diese Änderungen beim Bereinigen des Testfailovers verloren. Diese Änderungen werden nicht wieder auf dem primären virtuellen Computer repliziert.
    - Das Testen eines Produktionsnetzwerks führt zu einem Ausfall der Produktionsworkloads. Bitten Sie Benutzer, die App während des Notfallwiederherstellungsverfahrens nicht zu verwenden.  


## <a name="run-a-test-failover-for-a-vm"></a>Ausführen eines Testfailovers für einen virtuellen Computer

1. Klicken Sie zum Ausführen eines Failovers für einen einzelnen virtuellen Computer unter **Replizierte Elemente** auf den virtuellen Computer und anschließend auf **Testfailover**.
2. Geben Sie in **Testfailover** an, wie die virtuellen Computer nach dem Testfailover mit Netzwerken verbunden werden. 
3. Klicken Sie auf **OK**, um den Failovervorgang zu starten. Überwachen Sie den Fortschritt auf der Registerkarte **Aufträge** .
5. Vergewissern Sie sich nach Abschluss des Failovers, dass die virtuellen Testcomputer erfolgreich gestartet werden.
6. Klicken Sie anschließend auf dem Wiederherstellungsplan auf **Cleanup test failover** (Testfailover bereinigen).
7. Erfassen und speichern Sie unter **Notizen** alle Beobachtungen im Zusammenhang mit dem Testfailover. Durch diesen Schritt werden die während des Testfailovers erstellten virtuellen Computer und Netzwerke gelöscht.


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Bereitstellung getestet haben, können Sie sich [hier](site-recovery-failover.md) ausführlicher über andere Failoverarten informieren.

