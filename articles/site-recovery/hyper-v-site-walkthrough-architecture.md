---
title: "Überprüfen der Architektur für die Hyper-V-Replikation (ohne System Center VMM) in Azure mit Azure Site Recovery? | Microsoft Docs"
description: "Dieser Artikel bietet einen Überblick über die Komponenten und Architektur, die beim Replizieren von lokalen virtuellen Hyper-V-Computern (ohne VMM) in Azure mit dem Azure Site Recovery-Dienst verwendet werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/22/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: 044dfe686de36c56703d126db94d0b4382b85886
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---


<a id="step-1-review-the-architecture-for-hyper-v-replication-to-azure" class="xliff"></a>

# Schritt 1: Überprüfen der Architektur für die Hyper-V-Replikation in Azure


Dieser Artikel beschreibt die Komponenten und Prozesse bei der Replikation lokaler virtueller Hyper-V-Computer (die nicht von System Center VMM verwaltet werden) in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts.

Kommentare können Sie am Ende dieses Artikels eingeben oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.



<a id="architectural-components" class="xliff"></a>

## Komponenten der Architektur

Beim Replizieren von virtuellen Hyper-V-Computern in Azure ohne VMM sind eine Reihe von Komponenten beteiligt.

**Komponente** | **Standort** | **Details**
--- | --- | ---
**Azure** | In Azure benötigen Sie ein Microsoft Azure-Konto, ein Azure-Speicherkonto und ein Azure-Netzwerk. | Replizierte Daten werden im Speicherkonto gespeichert, und Azure-VMs werden mit den replizierten Daten erstellt, wenn ein Failover von Ihrem lokalen Standort durchgeführt wird.<br/><br/> Für die Azure-VMs wird eine Verbindung mit dem virtuellen Azure-Netzwerk hergestellt, wenn diese erstellt werden.
**Hyper-V** | Hyper-V-Hosts und -Cluster werden zu Hyper-V-Standorten zusammengefasst. Der Azure Site Recovery-Anbieter und der Recovery Services-Agent auf jedem Hyper-V-Host installiert. | Der Anbieter orchestriert die Replikation mit Site Recovery über das Internet. Der Recovery Services-Agent verarbeitet die Datenreplikation.<br/><br/> Sowohl die Kommunikation vom Anbieter als auch vom Agent ist sicher und verschlüsselt. Die replizierten Daten im Azure-Speicher werden ebenfalls verschlüsselt.
**Virtuelle Hyper-V-Computer** | Sie benötigen mindestens einen virtuellen Computer, der auf dem Hyper-V-Hostserver ausgeführt wird. | Auf virtuellen Computern muss nichts explizit installiert werden.

Weitere Informationen zu Voraussetzungen für die Bereitstellung und Anforderungen für die Komponenten finden Sie in der [Supportmatrix](site-recovery-support-matrix-to-azure.md).

**Abbildung 1: Replikation von Hyper-V-Sites in Azure**

![Komponenten](./media/hyper-v-site-walkthrough-architecture/arch-onprem-azure-hypervsite.png)


<a id="replication-process" class="xliff"></a>

## Replikationsprozess

**Abbildung 2: Replikations- und Wiederherstellungsvorgang für die Hyper-V-Replikation in Azure**

![Workflow](./media/hyper-v-site-walkthrough-architecture/arch-hyperv-azure-workflow.png)

<a id="enable-protection" class="xliff"></a>

### Schutz aktivieren

1. Nachdem Sie den Schutz für einen virtuellen Hyper-V-Computer über das Azure-Portal oder lokal aktiviert haben, wird der Auftrag **Schutz aktivieren** gestartet.
2. Im Rahmen des Auftrags wird überprüft, ob der Computer die Voraussetzungen erfüllt. Anschließend wird [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) aufgerufen, um die Replikation mit den konfigurierten Einstellungen einzurichten.
3. Der Auftrag startet die erste Replikation durch Aufrufen der [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx)-Methode, um eine vollständige VM-Replikation zu initiieren, und übermittelt die virtuellen Datenträger des virtuellen Computers an Azure.
4. Sie können den Auftrag auf der Registerkarte **Aufträge** überwachen.
 
<a id="replicate-the-initial-data" class="xliff"></a>

### Replizieren der ursprünglichen Daten

1. Wenn die erste Replikation ausgelöst wird, wird eine [Hyper-V-Momentaufnahme für den virtuellen Computer](https://technet.microsoft.com/library/dd560637.aspx) erstellt.
2. Virtuelle Festplatten werden nacheinander repliziert, bis alle in Azure kopiert wurden. Die Dauer ist abhängig von der Größe des virtuellen Computers und von der Netzwerkbandbreite. Informationen zum Optimieren der Netzwerknutzung finden Sie unter [How to manage on-premises to Azure protection network bandwidth usage](https://support.microsoft.com/kb/3056159)(Verwalten der Netzwerkbandbreitennutzung für den Schutz lokaler Elemente in Azure).
3. Falls während der ersten Replikation Datenträgeränderungen auftreten, werden diese Änderungen mit dem Replication Tracker für Hyper-V-Replikate in Form von Hyper-V-Replikationsprotokollen (HRL-Dateien) nachverfolgt. Diese Dateien befinden sich im gleichen Ordner wie die Datenträger. Jeder Datenträger verfügt über eine zugeordnete HRL-Datei, die an den sekundären Speicher gesendet wird.
4. Beachten Sie, dass die Momentaufnahme- und Protokolldateien Festplattenressourcen belegen, während die anfängliche Replikation durchgeführt wird.
5. Nach Abschluss der ersten Replikation wird die Momentaufnahme des virtuellen Computers gelöscht. Die im Protokoll erfassten Datenträgeränderungen werden synchronisiert und mit dem übergeordneten Datenträger zusammengeführt.


<a id="finalize-protection" class="xliff"></a>

### Abschließen des Schutzes

1. Nach Abschluss der ersten Replikation werden mit dem Auftrag **Schutz auf virtuellem Computer abschließen** das Netzwerk und andere Einstellungen für die Zeit nach der Replikation konfiguriert, sodass der virtuelle Computer geschützt ist.
2. Wenn Sie eine Replikation zu Azure durchführen, müssen Sie die Einstellungen für den virtuellen Computer unter Umständen so optimieren, dass er bereit für das Failover ist. An diesem Punkt können Sie ein Testfailover durchführen, um zu überprüfen, ob alles wie erwartet funktioniert.

<a id="replicate-the-delta" class="xliff"></a>

### Replizieren des Deltas

1. Nach der ersten Replikation beginnt die Deltasynchronisierung gemäß den Replikationseinstellungen.
2. Der Replication Tracker für Hyper-V-Replikate verfolgt die Änderungen einer virtuellen Festplatte mithilfe von HRL-Dateien nach. Jedem für die Replikation konfigurierten Datenträger ist eine HRL-Datei zugeordnet. Dieses Protokoll wird nach Abschluss der ersten Replikation an das Speicherkonto des Kunden gesendet. Beim Übermitteln eines Protokolls an Azure werden Änderungen am primären Datenträger in einer anderen Protokolldatei im gleichen Verzeichnis nachverfolgt.
3. Während der Erst- und Deltareplikation können Sie den virtuellen Computer in der Ansicht des virtuellen Computers überwachen. [detaillierte Kapazitätsplanung](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machines)  

<a id="synchronize-replication" class="xliff"></a>

### Synchronisieren der Replikation

1. Falls die Deltareplikation nicht erfolgreich ist und eine vollständige Replikation viel Bandbreite oder Zeit beanspruchen würde, wird der virtuelle Computer für eine Neusynchronisierung markiert. Wenn die HRL-Dateien beispielsweise 50% des Festplattenspeichers füllen, wird die VM für die Neusynchronisierung gekennzeichnet.
2.  Bei der Neusynchronisierung werden Prüfsummen für die virtuellen Quell- und Zielcomputer berechnet und nur die Deltadaten gesendet, um die Menge der gesendeten Daten zu minimieren. Die Neusynchronisierung verwendet einen Blockerstellungsalgorithmus mit festen Blöcken, durch den Quell-und Zieldateien in feste Blöcke unterteilt werden. Für die einzelnen Blöcke werden Prüfsummen generiert und anschließend verglichen, um zu ermitteln, welche Blöcke aus der Quelle auf das Ziel angewendet werden müssen.
3. Nach Abschluss der Neusynchronisierung wird die normale Deltareplikation fortgesetzt. Standardmäßig ist die Neusynchronisierung so geplant, dass sie automatisch außerhalb der Geschäftszeiten durchgeführt wird, aber Sie können eine virtuelle Maschine auch manuell neu synchronisieren. Die Neusynchronisierung kann beispielsweise nach einem Netzwerkausfall oder einem anderen Ausfall fortgesetzt werden. Wählen Sie hierzu im Portal den virtuellen Computer und anschließend **Resynchronisieren** aus.

    ![Manuelle Neusynchronisierung](media/hyper-v-site-walkthrough-architecture/image4.png)


<a id="retry-logic" class="xliff"></a>

### Wiederholungslogik

Im Falle eines Replikationsfehlers wird die integrierte Wiederholungsfunktion verwendet. Diese Logik lässt sich in zwei Kategorien unterteilen:

**Kategorie** | **Details**
--- | ---
**Nicht behebbare Fehler** | Es wird kein erneuter Versuch unternommen. Der Status des virtuellen Computers lautet **Kritisch**, und ein Administrator muss eingreifen. Beispiele: unterbrochene VHD-Kette, ungültiger Zustand des virtuellen Replikatcomputers, Netzwerkauthentifizierungsfehler, Autorisierungsfehler, Fehler aufgrund eines nicht gefundenen virtuellen Computers (bei eigenständigen Hyper-V-Servern)
**Behebbare Fehler** | In jedem Replikationsintervall wird ein weiterer erneuter Versuch unternommen. Dabei wird ein exponentielles Backoff verwendet, durch das sich das Wiederholungsintervall ab dem ersten Versuch immer weiter verlängert (1, 2, 4, 8 und 10 Minute(n)). Wenn ein Fehler auftritt, versuchen Sie es jede halbe Stunde erneut. Beispiele: Netzwerkfehler, Fehler aufgrund von geringem Speicherplatz, wenig Arbeitsspeicher |



<a id="failover-and-failback-process" class="xliff"></a>

## Failover- und Failbackprozesse

1. Sie können ein geplantes oder ungeplantes [Failover](site-recovery-failover.md) von lokalen Hyper-V-VMs nach Azure ausführen. Wenn Sie ein geplantes Failover durchführen, werden die Quell-VMs heruntergefahren, um sicherzustellen, dass kein Datenverlust auftritt.
2. Sie können ein Failover für einen einzelnen Computer durchführen oder [Wiederherstellungspläne](site-recovery-create-recovery-plans.md) erstellen, um das Failover von mehreren Computern zu orchestrieren.
4. Nachdem Sie das Failover ausgeführt haben, sollten Sie die erstellten Replikat-VMs in Azure sehen können. Sie können der VM bei Bedarf eine öffentliche IP-Adresse zuweisen.
5. Anschließend führen Sie ein Commit für das Failover durch, um von der Replikat-VM in Azure auf die Workload zuzugreifen.
6. Wenn Ihr primärer lokaler Standort wieder verfügbar ist, können Sie ein [Failback](site-recovery-failback-from-azure-to-hyper-v.md) durchführen. Sie lösen ein geplantes Failover von Azure zum primären Standort aus. Für ein geplantes Failover können Sie wählen, ob ein Failback auf dieselbe VM oder an einen anderen Standort durchgeführt werden soll, und Sie können die Änderungen zwischen Azure und dem lokalen Standort synchronisieren, um Datenverluste zu verhindern. Wenn die VMs lokal erstellt werden, führen Sie ein Commit für das Failover aus.




<a id="next-steps" class="xliff"></a>

## Nächste Schritte

Wechseln Sie zu [Schritt 2: Überprüfen der Voraussetzungen für die Bereitstellung](hyper-v-site-walkthrough-prerequisites.md)

