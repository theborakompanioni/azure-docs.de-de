---
title: Wie funktioniert die Hyper-V-Replikation in Azure Site Recovery? | Microsoft Docs
description: "Dieser Artikel enthält eine Übersicht über die Funktionsweise der Hyper-V-Replikation in Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/05/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 9adf266c6a2ac00c3aaa34e2a29aefe34abe2871
ms.lasthandoff: 04/18/2017


---
# <a name="how-does-hyper-v-replication-to-azure-work"></a>Wie funktioniert die Hyper-V-Replikation in Azure?

Dieser Artikel enthält Informationen zur Architektur und zu Workflows für die Hyper-V-Replikation in Azure mit dem Dienst [Azure Site Recovery](site-recovery-overview.md).

Kommentare können Sie am Ende dieses Artikels eingeben oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

Sie können Folgendes in Azure replizieren:
- **Hyper-V mit VMM:** Virtuelle Computer auf lokalen Hyper-V-Hosts, die in System Center Virtual Machine Manager-Clouds (VMM-Clouds) verwaltet werden. Auf den Hosts kann ein beliebiges [unterstütztes Betriebssystem](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) ausgeführt werden. Sie können virtuelle Hyper-V-Computer unter einem beliebigen Gastbetriebssystem replizieren, das [von Hyper-V und Azure unterstützt](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) wird.
- **Hyper-V ohne VMM:** Lokale virtuelle Computer auf Hyper-V-Hosts, die nicht in VMM-Clouds verwaltet werden. Auf den Hosts kann ein beliebiges [unterstütztes Betriebssystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) ausgeführt werden. Sie können virtuelle Hyper-V-Computer unter einem beliebigen Gastbetriebssystem replizieren, das [von Hyper-V und Azure unterstützt](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) wird.


## <a name="architectural-components"></a>Komponenten der Architektur

**Bereich** | **Komponente** | **Details**
--- | --- | ---
**Azure** | In Azure benötigen Sie ein Microsoft Azure-Konto, ein Azure-Speicherkonto und ein Azure-Netzwerk. | Bei Speicher und Netzwerk kann es sich um Resource Manager-basierte Konten oder klassische Konten handeln.<br/><br/> Replizierte Daten werden im Speicherkonto gespeichert, und Azure-VMs werden mit den replizierten Daten erstellt, wenn ein Failover von Ihrem lokalen Standort durchgeführt wird.<br/><br/> Für die Azure-VMs wird eine Verbindung mit dem virtuellen Azure-Netzwerk hergestellt, wenn diese erstellt werden.
**VMM-Server** | Hyper-V-Hosts in VMM-Clouds | Wenn Hyper-V-Hosts in VMM-Clouds verwaltet werden, registrieren Sie den VMM-Server im Recovery Services-Tresor.<br/><br/> Auf dem VMM-Server installieren Sie den Site Recovery-Anbieter, um die Replikation in Azure zu orchestrieren.<br/><br/> Zum Konfigurieren der Netzwerkzuordnung müssen logische Netzwerke und VM-Netzwerke eingerichtet sein. Ein VM-Netzwerk sollte mit einem logischen Netzwerk verbunden sein, das der Cloud zugeordnet ist.
**Hyper-V-Host** | Hyper-V-Server können mit oder ohne VMM-Server bereitgestellt werden. | Falls kein VMM-Server vorhanden ist, wird der Site Recovery-Anbieter auf dem Host installiert, um die Replikation mit Site Recovery über das Internet zu orchestrieren. Ist ein VMM-Server vorhanden, wird der Anbieter nicht auf dem Host, sondern auf dem VMM-Server installiert.<br/><br/> Der Recovery Services-Agent wird auf dem Host installiert und wickelt die Datenreplikation ab.<br/><br/> Sowohl die Kommunikation vom Anbieter als auch vom Agent ist sicher und verschlüsselt. Die replizierten Daten im Azure-Speicher werden ebenfalls verschlüsselt.
**Virtuelle Hyper-V-Computer** | Sie benötigen mindestens eine VM auf dem Hyper-V-Hostserver. | Auf virtuellen Computern muss nichts explizit installiert werden.

## <a name="deployment-steps"></a>Bereitstellungsschritte

1. **Azure:** Richten Sie die Azure-Komponenten ein. Wir empfehlen Ihnen, Speicher- und Netzwerkkonten einzurichten, bevor Sie mit der Site Recovery-Bereitstellung beginnen.
2. **Tresor:** Erstellen Sie einen Recovery Services-Tresor für Site Recovery, und konfigurieren Sie Tresoreinstellungen (einschließlich Quell-und Zieleinstellungen, Einrichtung einer Replikationsrichtlinie und Aktivierung der Replikation).
3. **Quelle und Ziel:**
    - **Hyper-V-Hosts in VMM-Clouds:** Laden Sie im Rahmen der Angabe der Quelleinstellungen den Azure Site Recovery-Anbieter herunter, und installieren Sie ihn auf dem VMM-Server. Laden Sie außerdem den Azure Recovery Services-Agent herunter, und installieren Sie ihn auf den einzelnen Hyper-V-Hosts. Die Quelle ist der VMM-Server. Das Ziel ist Azure.
    - Hyper-V-Hosts ohne VMM: Laden Sie im Rahmen der Angabe der Quelleinstellungen den Anbieter und den Agent herunter, und installieren Sie sie auf den einzelnen Hyper-V-Hosts. Fassen Sie die Hosts bei der Bereitstellung zu einer Hyper-V-Site zusammen, und geben Sie sie als Quelle an. Das Ziel ist Azure.

    ![Hyper-V-/VMM-Replikation in Azure](./media/site-recovery-components/arch-onprem-onprem-azure-vmm.png)
    ![Hyper-V-Site-Replikation in Azure](./media/site-recovery-components/arch-onprem-azure-hypervsite.png)


4. **Replikationsrichtlinie:** Erstellen Sie eine Replikationsrichtlinie für die Hyper-V-Site oder die VMM-Cloud. Die Richtlinie wird auf alle VMs angewendet, die sich auf Hosts am Standort oder in der Cloud befinden.
5. **Replikation aktivieren:** Aktivieren Sie die Replikation für virtuelle Hyper-V-Computer. Die erste Replikation wird gemäß den Einstellungen der Replikationsrichtlinie durchgeführt. Datenänderungen werden nachverfolgt, und die Replikation von Deltaänderungen in Azure beginnt, nachdem die erste Replikation abgeschlossen wurde. Nachverfolgte Änderungen für eine Komponente werden in einer HRL-Datei gespeichert.
6. **Testfailover:** Führen Sie ein Testfailover aus, um sicherzustellen, dass alles wie erwartet funktioniert.

Weitere Informationen zur Bereitstellung finden Sie hier:
- [Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds in Azure per Site Recovery im Azure-Portal](site-recovery-vmm-to-azure.md)
- [Replizieren von virtuellen Hyper-V-Computern (ohne VMM) nach Azure per Azure Site Recovery über das Azure-Portal](site-recovery-hyper-v-site-to-azure.md)

## <a name="hyper-v-replication-workflow"></a>Workflow der Hyper-V-Replikation

### <a name="enable-protection"></a>Schutz aktivieren

1. Nachdem Sie den Schutz für einen virtuellen Hyper-V-Computer über das Azure-Portal oder lokal aktiviert haben, wird der Auftrag **Schutz aktivieren** gestartet.
2. Im Rahmen des Auftrags wird überprüft, ob der Computer die Voraussetzungen erfüllt. Anschließend wird [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) aufgerufen, um die Replikation mit den konfigurierten Einstellungen einzurichten.
3. Der Auftrag startet die erste Replikation durch Aufrufen der [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx)-Methode, um eine vollständige VM-Replikation zu initiieren, und übermittelt die virtuellen Datenträger des virtuellen Computers an Azure.
4. Sie können den Auftrag auf der Registerkarte **Aufträge** überwachen.
        ![Auftragsliste](media/site-recovery-hyper-v-azure-architecture/image1.png)
        ![Drilldown für „Schutz aktivieren“](media/site-recovery-hyper-v-azure-architecture/image2.png)

### <a name="initial-replication"></a>Erste Replikation

1. Wenn die erste Replikation ausgelöst wird, wird eine [Hyper-V-Momentaufnahme für den virtuellen Computer](https://technet.microsoft.com/library/dd560637.aspx) erstellt.
2. Virtuelle Festplatten werden nacheinander repliziert, bis alle in Azure kopiert wurden. Die Dauer ist abhängig von der Größe des virtuellen Computers und von der Netzwerkbandbreite. Informationen zum Optimieren der Netzwerknutzung finden Sie unter [How to manage on-premises to Azure protection network bandwidth usage](https://support.microsoft.com/kb/3056159)(Verwalten der Netzwerkbandbreitennutzung für den Schutz lokaler Elemente in Azure).
3. Falls während der ersten Replikation Datenträgeränderungen auftreten, werden diese Änderungen mit dem Replication Tracker für Hyper-V-Replikate in Form von Hyper-V-Replikationsprotokollen (HRL-Dateien) nachverfolgt. Diese Dateien befinden sich im gleichen Ordner wie die Datenträger. Jeder Datenträger verfügt über eine zugeordnete HRL-Datei, die an den sekundären Speicher gesendet wird.
4. Beachten Sie, dass die Momentaufnahme- und Protokolldateien Festplattenressourcen belegen, während die anfängliche Replikation durchgeführt wird.
5. Nach Abschluss der ersten Replikation wird die Momentaufnahme des virtuellen Computers gelöscht. Die im Protokoll erfassten Datenträgeränderungen werden synchronisiert und mit dem übergeordneten Datenträger zusammengeführt.


### <a name="finalize-protection"></a>Abschließen des Schutzes

1. Nach Abschluss der ersten Replikation werden mit dem Auftrag **Schutz auf virtuellem Computer abschließen** das Netzwerk und andere Einstellungen für die Zeit nach der Replikation konfiguriert, sodass der virtuelle Computer geschützt ist.
    ![Auftrag zum Abschließen des Schutzes](media/site-recovery-hyper-v-azure-architecture/image3.png)
2. Wenn Sie eine Replikation zu Azure durchführen, müssen Sie die Einstellungen für den virtuellen Computer unter Umständen so optimieren, dass er bereit für das Failover ist. An diesem Punkt können Sie ein Testfailover durchführen, um zu überprüfen, ob alles wie erwartet funktioniert.

### <a name="delta-replication"></a>Deltareplikation

1. Nach der ersten Replikation beginnt die Deltasynchronisierung gemäß den Replikationseinstellungen.
2. Der Replication Tracker für Hyper-V-Replikate verfolgt die Änderungen einer virtuellen Festplatte mithilfe von HRL-Dateien nach. Jedem für die Replikation konfigurierten Datenträger ist eine HRL-Datei zugeordnet. Dieses Protokoll wird nach Abschluss der ersten Replikation an das Speicherkonto des Kunden gesendet. Beim Übermitteln eines Protokolls an Azure werden Änderungen am primären Datenträger in einer anderen Protokolldatei im gleichen Verzeichnis nachverfolgt.
3. Während der Erst- und Deltareplikation können Sie den virtuellen Computer in der Ansicht des virtuellen Computers überwachen. [Weitere Informationen](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines).  

### <a name="replication-synchronization"></a>Replikationssynchronisierung

1. Falls die Deltareplikation nicht erfolgreich ist und eine vollständige Replikation viel Bandbreite oder Zeit beanspruchen würde, wird der virtuelle Computer für eine Neusynchronisierung markiert. Wenn die HRL-Dateien beispielsweise 50% des Festplattenspeichers füllen, wird die VM für die Neusynchronisierung gekennzeichnet.
2.  Bei der Neusynchronisierung werden Prüfsummen für die virtuellen Quell- und Zielcomputer berechnet und nur die Deltadaten gesendet, um die Menge der gesendeten Daten zu minimieren. Die Neusynchronisierung verwendet einen Blockerstellungsalgorithmus mit festen Blöcken, durch den Quell-und Zieldateien in feste Blöcke unterteilt werden. Für die einzelnen Blöcke werden Prüfsummen generiert und anschließend verglichen, um zu ermitteln, welche Blöcke aus der Quelle auf das Ziel angewendet werden müssen.
3. Nach Abschluss der Neusynchronisierung wird die normale Deltareplikation fortgesetzt. Standardmäßig ist die Neusynchronisierung so geplant, dass sie automatisch außerhalb der Geschäftszeiten durchgeführt wird, aber Sie können eine virtuelle Maschine auch manuell neu synchronisieren. Die Neusynchronisierung kann beispielsweise nach einem Netzwerkausfall oder einem anderen Ausfall fortgesetzt werden. Wählen Sie hierzu im Portal den virtuellen Computer und anschließend **Resynchronisieren** aus.

    ![Manuelle Neusynchronisierung](media/site-recovery-hyper-v-azure-architecture/image4.png)


### <a name="retries"></a>Wiederholungsversuche

Im Falle eines Replikationsfehlers wird die integrierte Wiederholungsfunktion verwendet. Diese Logik lässt sich in zwei Kategorien unterteilen:

**Kategorie** | **Details**
--- | ---
**Nicht behebbare Fehler** | Es wird kein erneuter Versuch unternommen. Der Status des virtuellen Computers lautet **Kritisch**, und ein Administrator muss eingreifen. Beispiele: unterbrochene VHD-Kette, ungültiger Zustand des virtuellen Replikatcomputers, Netzwerkauthentifizierungsfehler, Autorisierungsfehler, Fehler aufgrund eines nicht gefundenen virtuellen Computers (bei eigenständigen Hyper-V-Servern)
**Behebbare Fehler** | In jedem Replikationsintervall wird ein weiterer erneuter Versuch unternommen. Dabei wird ein exponentielles Backoff verwendet, durch das sich das Wiederholungsintervall ab dem ersten Versuch immer weiter verlängert (1, 2, 4, 8 und 10 Minute(n)). Wenn ein Fehler auftritt, versuchen Sie es jede halbe Stunde erneut. Beispiele: Netzwerkfehler, Fehler aufgrund von geringem Speicherplatz, wenig Arbeitsspeicher |

## <a name="protection-and-recovery-lifecycle"></a>Lebenszyklus von Schutz und Wiederherstellung

![Workflow](./media/site-recovery-components/arch-hyperv-azure-workflow.png)

## <a name="next-steps"></a>Nächste Schritte

- [Überprüfen Sie die Bereitstellungsvoraussetzungen.](site-recovery-prereq.md)
- Problembehandlung:
    - [Überwachung und Problembehandlung für den Schutz von virtuellen Computern und physischen Servern](site-recovery-monitoring-and-troubleshooting.md)
    - [Hilfe vom Microsoft-Support](site-recovery-monitoring-and-troubleshooting.md#reach-out-for-microsoft-support)
    - [Häufige Fehler bei der automatischen Systemwiederherstellung und deren Lösungen](site-recovery-monitoring-and-troubleshooting.md#common-azure-site-recovery-errors-and-their-resolutions)

