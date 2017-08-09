---
title: "Einrichten einer Replikationsrichtlinie für die Hyper-V-Replikation an einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt, wie eine Richtlinie für die Replikation virtueller Hyper-V-Computer an einem sekundären VMM-Standort mit Azure Site Recovery eingerichtet wird."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5d9b79cf-89f2-4af9-ac8e-3a32ad8c6c4d
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: a3ecc555e049914d7bffdddce7b0522f09362d24
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-8-set-up-a-replication-policy"></a>Schritt 8: Einrichten einer Replikationsrichtlinie

Nach dem Konfigurieren der [Netzwerkzuordnung](vmm-to-vmm-walkthrough-network-mapping.md) richten Sie anhand der Informationen in diesem Artikel eine Replikationsrichtlinie für die Replikation virtueller Hyper-V-Computer an einem sekundären Standort mit [Azure Site Recovery](site-recovery-overview.md) ein.

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.


## <a name="before-you-start"></a>Vorbereitung

- Wenn Sie eine Replikationsrichtlinie erstellen, müssen alle Hosts, die die Richtlinie verwenden, über das gleiche Betriebssystem verfügen. Die VMM-Cloud kann zwar Hyper-V-Hosts mit unterschiedlichen Windows Server-Versionen enthalten, in diesem Fall benötigen Sie jedoch mehrere Replikationsrichtlinien.
- Die anfängliche Replikation kann offline durchgeführt werden.

## <a name="configure-replication-settings"></a>Konfigurieren der Replikationseinstellungen

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Infrastruktur vorbereiten** > **Replikationseinstellungen** > **+Erstellen und zuordnen**.

    ![Netzwerk](./media/vmm-to-vmm-walkthrough-replication/gs-replication.png)
2. Geben Sie unter **Richtlinie erstellen und zuordnen**einen Richtliniennamen an. Quell- und Zieltyp müssen jeweils **Hyper-V**lauten.
3. Wählen Sie unter **Hyper-V-Hostversion** aus, welches Betriebssystem auf dem Host ausgeführt wird.
4. Geben Sie unter **Authentifizierungstyp** und **Authentifizierungsport** an, wie der Datenverkehr zwischen dem primären und dem Hyper-V-Wiederherstellungs-Hostserver authentifiziert wird. Wählen Sie **Zertifikat** aus, sofern Sie nicht über eine funktionsfähige Kerberos-Umgebung verfügen. Azure Site Recovery konfiguriert automatisch Zertifikate für die HTTPS-Authentifizierung. Sie müssen gar nichts manuell durchführen. Standardmäßig werden die Ports 8083 und 8084 (für Zertifikate) in der Windows-Firewall auf den Hyper-V-Hostservern geöffnet. Wenn Sie **Kerberos**auswählen, wird ein Kerberos-Ticket für die gegenseitige Authentifizierung der Hostserver verwendet. Bitte beachten Sie, dass diese Einstellung nur für Hyper-V-Hostserver relevant ist, die unter Windows Server 2012 R2 laufen.
5. Geben Sie unter **Kopierhäufigkeit**an, wie oft Sie Deltadaten nach der ersten Replikation replizieren möchten (alle 30 Sekunden, nach 5 Minuten oder nach 15 Minuten).
6. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts**das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte in Stunden an. Geschützte Computer können innerhalb eines Zeitfensters an einem beliebigen Punkt wiederhergestellt werden.
7. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit** an, wie häufig (1 bis 12 Stunden) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Hyper-V verwendet zwei Momentaufnahmen: eine Standard-Momentaufnahme, die eine inkrementelle Momentaufnahme des gesamten virtuellen Computers bereitstellt, und eine anwendungskonsistente Momentaufnahme, die eine Zeitpunkt-Momentaufnahme der Anwendungsdaten innerhalb des virtuellen Computers erfasst. Anwendungskonsistente Momentaufnahmen verwenden den Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um sicherzustellen, dass Anwendungen sich bei der Erstellung der Momentaufnahme in einem konsistenten Zustand befinden. Die Leistung von Anwendungen auf virtuellen Quellcomputern wird durch die Aktivierung anwendungskonsistenter Momentaufnahmen beeinträchtigt. Stellen Sie sicher, dass der festgelegte Wert kleiner als die konfigurierte Anzahl der zusätzlichen Wiederherstellungspunkte ist.
8. Geben Sie unter **Datenübertragungskomprimierung**an, ob replizierte Daten, die übertragen werden, komprimiert werden sollen.
9. Wählen Sie **Replikat-VM löschen** aus, um anzugeben, dass die Replikat-VM gelöscht werden sollte, wenn Sie den Schutz für die Quell-VM deaktivieren. Wenn Sie diese Einstellung aktivieren, und Sie deaktivieren den Schutz für die Quell-VMs, wird der virtuelle Computer aus der Site Recovery-Konsole entfernt, die Site Recovery-Einstellungen für die VMM werden aus der VMM-Konsole entfernt, und das Replikat wird gelöscht.
10. Wenn Sie über das Netzwerk replizieren, geben Sie unter **Methode für anfängliche Replikation** an, ob die erste Replikation sofort gestartet werden soll oder ob Sie sie planen möchten. Um Netzwerkbandbreite zu sparen, sollte der Zeitraum außerhalb Ihrer Spitzenzeiten liegen. Klicken Sie dann auf **OK**.

     ![Replikationsrichtlinie](./media/vmm-to-vmm-walkthrough-replication/gs-replication2.png)
11. Wenn Sie eine neue Richtlinie erstellen, wird sie der VMM-Cloud automatisch zugeordnet. Klicken Sie unter **Replikationsrichtlinie** auf **OK**. Sie können dieser Replikationsrichtlinie unter **Replikation** > „Richtlinienname“ > **VMM-Cloud zuordnen** zusätzliche VMM-Clouds (und die darin enthaltenen VMs) zuordnen.

     ![Replikationsrichtlinie](./media/vmm-to-vmm-walkthrough-replication/policy-associate.png)



## <a name="prepare-for-offline-initial-replication"></a>Vorbereiten der ersten Offlinereplikation

Sie können eine Offlinereplikation für die erstmalige Datenkopie durchführen. Sie können dies wie folgt vorbereiten:

* Geben Sie auf dem Quellserver einen Pfad an, von dem der Datenexport stattfinden soll. Weisen Sie Vollzugriff für NTFS und Freigabeberechtigungen für den VMM-Dienst auf dem Exportpfad zu. Auf dem Zielserver geben Sie einen Pfad an, von dem der Datenimport stattfinden soll. Weisen Sie für diesen Importpfad die gleichen Berechtigungen zu.
* Wenn der Import- oder Exportpfad freigegeben ist, weisen Sie Mitgliedschaften in den Gruppen "Administratoren", "Hauptbenutzer", "Druck-Operatoren" oder "Server-Operatoren" für das VMM-Dienstkonto auf dem Remotecomputer zu, auf dem sich die Freigabe befindet.
* Wenn Sie zum Hinzufügen von Hosts ausführende Konten verwenden, weisen Sie auf dem Import- und Exportpfad Lese- und Schreibberechtigungen für die ausführenden Konten in VMM zu.
* Die Import- und Exportfreigaben sollten sich nicht auf Computern befinden, die als Hyper-V-Hostserver eingesetzt werden, da die Loopbackkonfiguration von Hyper-V nicht unterstützt wird.
* Aktivieren und konfigurieren Sie die eingeschränkte Delegierung in Active Directory auf jedem Hyper-V-Hostserver mit virtuellen Computern, die Sie schützen möchten, um den Remotecomputern zu vertrauen, auf denen sich der Import- und der Exportpfad befinden. Gehen Sie dabei wie folgt vor:
  1. Öffnen Sie auf dem Domänencontroller **Active Directory-Benutzer und-Computer**.
  2. Klicken Sie in der Konsolenstruktur auf **Domänenname** > **Computer**.
  3. Klicken Sie mit der rechten Maustaste auf den Namen des Hyper-V-Hostservers, und klicken Sie anschließend auf **Eigenschaften**.
  4. Klicken Sie auf der Registerkarte **Delegierung** auf **Computer nur bei Delegierungen angegebener Dienste vertrauen**.
  5. Klicken Sie auf **Beliebiges Authentifizierungsprotokoll verwenden**.
  6. Klicken Sie auf **Hinzufügen** > **Benutzer und Computer**können Sie technische Fragen stellen.
  7. Geben Sie den Namen des Computers an, der den Importpfad hostet, und klicken Sie auf **OK**. Halten Sie in der Liste der verfügbaren Dienste die STRG-Taste gedrückt, und klicken Sie auf **CIFS** > **OK**. Wiederholen Sie diesen Vorgang für den Namen des Computers, der den Importpfad hostet. Wiederholen Sie den Vorgang bei Bedarf für zusätzliche Hyper-V-Hostserver.



## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 9: Aktivieren der Replikation](vmm-to-vmm-walkthrough-enable-replication.md).

