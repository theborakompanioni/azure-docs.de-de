---
title: Hyper-V-Replikation mit Azure Site Recovery | Microsoft Docs
description: Verwenden Sie diesen Artikel, um sich mit den technischen Konzepten vertraut zu machen, die Sie beim Installieren, Konfigurieren und Verwalten von Azure Site Recovery unterstützen.
services: site-recovery
documentationcenter: ''
author: Rajani-Janaki-Ram
manager: mkjain
editor: ''

ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/12/2016
ms.author: rajanaki

---
# <a name="hyper-v-replication-with-azure-site-recovery"></a>Hyper-V-Replikation mit Azure Site Recovery
Dieser Artikel beschreibt die technischen Konzepte, die Sie beim Konfigurieren und Verwalten eines Hyper-V-Standorts oder eines System Center Virtual Machine Manager-Standorts (VMM-Standort) mit Azure-Schutz unter Verwendung von Azure Site Recovery unterstützen.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>Einrichten der Quellumgebung für die Replikation zwischen dem lokalen Standort und Azure
Laden Sie im Zuge der Einrichtung der Notfallwiederherstellung zwischen dem lokalen Standort und Azure den Azure Site Recovery-Anbieter herunter, und installieren Sie ihn auf dem VMM-Server. Installieren Sie den Azure Recovery Services-Agent auf den einzelnen Hyper-V-Hosts.

![VMM-Standortbereitstellung für die Replikation zwischen lokalem Standort und Azure](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Die Quellumgebung in einer verwalteten Hyper-V-Infrastruktur wird auf ähnliche Weise eingerichtet wie die Quellumgebung in einer verwalteten VMM-Infrastruktur. Der einzige Unterschied: Anbieter und Agent werden auf dem Hyper-V-Host installiert. In der VMM-Umgebung wird der Anbieter auf dem VMM-Server und der Agent auf den Hyper-V-Hosts installiert (bei Verwendung der Replikation in Azure).

## <a name="workflows"></a>Workflows
### <a name="enable-protection"></a>Schutz aktivieren
Wenn ein virtueller Computer über das Azure-Portal oder lokal geschützt wird, wird ein Site Recovery-Auftrag namens **Schutz aktivieren** gestartet. Dieser kann auf der Registerkarte **Aufträge** überwacht werden.

![Auftrag „Schutz aktivieren“ in der Auftragsliste](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

Der Auftrag **Schutz aktivieren** überprüft zunächst, ob die Voraussetzungen erfüllt sind, und ruft dann die [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) -Methode auf. Diese Methode erstellt die Replikation in Azure auf der Grundlage von Eingaben, die während des Schutzes konfiguriert werden.

Der Auftrag **Schutz aktivieren** startet die erste Replikation lokal durch Aufrufen der [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) -Methode. Diese Methode sendet die virtuellen Datenträger des virtuellen Computers an Azure.

![Details zum Auftrag „Schutz aktivieren“](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>Abschließen des Schutzes für den virtuellen Computer
Wenn die erste Replikation ausgelöst wird, wird eine [Hyper-V-Momentaufnahme für den virtuellen Computer](https://technet.microsoft.com/library/dd560637.aspx) erstellt. Virtuelle Festplatten werden nacheinander verarbeitet, bis alle Datenträger in Azure hochgeladen wurden. Die Dauer dieses Vorgangs ist abhängig von der Größe des Datenträgers und von der Bandbreite. Informationen zum Optimieren der Netzwerknutzung finden Sie unter [How to manage on-premises to Azure protection network bandwidth usage](https://support.microsoft.com/kb/3056159)(Verwalten der Netzwerkbandbreitennutzung für den Schutz lokaler Elemente in Azure).

Nach Abschluss der ersten Replikation konfiguriert der Auftrag **Schutz auf dem virtuellen Computer abschließen** die Einstellungen für das Netzwerk und für die Zeit nach der Replikation. Während der Durchführung der ersten Replikation gilt:

* Alle Änderungen an den Datenträgern werden nachverfolgt. 
* Die Dateien für die Momentaufnahme und für das Hyper-V-Replikatprotokoll (Hyper-V Replica Log, HRL) beanspruchen zusätzlichen Speicherplatz.

Nach Abschluss der ersten Replikation wird die Hyper-V-Momentaufnahme für den virtuellen Computer gelöscht. Im Zuge dieser Löschung werden Datenänderungen zusammengeführt, die nach der ersten Replikation am übergeordneten Datenträger vorgenommen wurden.

![Auftrag „Abschließen des Schutzes für den virtuellen Computer“ in der Auftragsliste](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Deltareplikation
Die im Replikationsmodul des Hyper-V-Replikats enthaltene Hyper-V-Replikat-Replikationsverfolgung verfolgt die Änderungen an einer virtuellen Festplatte mithilfe von Hyper-V-Replikatprotokollen (HRL-Dateien) nach. HRL-Dateien befinden sich im gleichen Verzeichnis wie die dazugehörigen Datenträger.

Jedem für die Replikation konfigurierten Datenträger ist eine HRL-Datei zugeordnet. Dieses Protokoll wird nach Abschluss der ersten Replikation an das Speicherkonto des Kunden gesendet. Beim Übermitteln eines Protokolls an Azure werden Änderungen am primären Element in einer anderen Protokolldatei im gleichen Verzeichnis nachverfolgt.

Während der ersten Replikation oder einer Deltareplikation können Sie die Replikationsintegrität des virtuellen Computers wie unter [Überwachen der Replikationsintegrität für virtuelle Computer](site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine)beschrieben in der Ansicht des virtuellen Computers überwachen.  

### <a name="resynchronization"></a>Neusynchronisierung
Ein virtueller Computer wird für eine Neusynchronisierung markiert, wenn die Deltareplikation nicht erfolgreich war und eine vollständige Erstreplikation die Netzwerkbandbreite zu stark beansprucht oder zu zeitaufwendig ist. Wenn also beispielsweise eine HRL-Datei 50 Prozent des gesamten Datenträgers einnimmt, wird der virtuelle Computer für eine Neusynchronisierung markiert. Bei der Neusynchronisierung wird die über das Netzwerk übertragene Datenmenge minimiert, indem Prüfsummen der Datenträger des virtuellen Quell- und Zielcomputers berechnet werden und nur die Differenz gesendet wird.

Nach Abschluss der Neusynchronisierung wird die normale Deltareplikation fortgesetzt. Die Neusynchronisierung kann im Falle eines Netzwerkausfalls oder eines anderen Ausfalls fortgesetzt werden.

Die automatisch geplante Neusynchronisierung wird standardmäßig so konfiguriert, dass sie nicht während der Arbeitszeit stattfindet. Wenn der virtuelle Computer manuell neu synchronisiert werden muss, wählen Sie den virtuellen Computer im Portal aus, und klicken Sie anschließend auf **Resynchronisieren**.

![Manuelle Neusynchronisierung](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Die Neusynchronisierung verwendet einen Blockerstellungsalgorithmus mit festen Blöcken, durch den Quell-und Zieldateien in feste Blöcke unterteilt werden. Für die einzelnen Blöcke werden Prüfsummen generiert und anschließend verglichen, um zu ermitteln, welche Blöcke aus der Quelle auf das Ziel angewendet werden müssen.

### <a name="retry-logic"></a>Wiederholungslogik
Für Replikationsfehler steht eine integrierte Wiederholungslogik zur Verfügung. Diese Logik lässt sich in zwei Kategorien unterteilen:

| Kategorie | Szenarien |
| --- | --- |
| Nicht behebbarer Fehler |Es wird kein erneuter Versuch unternommen. Der Replikationsstatus des virtuellen Computers lautet **Kritisch**, und ein Administrator muss eingreifen. Beispiele:  <ul><li>Unterbrochene VHD-Kette</li><li>Ungültiger Status für den virtuellen Replikatcomputer</li><li>Netzwerkauthentifizierungsfehler</li><li>Autorisierungsfehler</li><li>Virtueller Computer, der bei einem eigenständigen Hyper-V-Server nicht gefunden wird</li></ul> |
| Behebbarer Fehler |In jedem Replikationsintervall wird ein weiterer erneuter Versuch unternommen. Dabei wird ein exponentielles Backoff verwendet, durch das sich das Wiederholungsintervall ab dem ersten Versuch immer weiter verlängert (1, 2, 4, 8, 10 Minuten). Wenn ein Fehler auftritt, versuchen Sie es jede halbe Stunde erneut. Beispiele:  <ul><li>Netzwerkfehler</li><li>Wenig freier Speicherplatz</li><li>Wenig Arbeitsspeicher verfügbar</li></ul> |

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>Schutz- und Wiederherstellungslebenszyklus für virtuelle Hyper-V-Computer
![Schutz- und Wiederherstellungslebenszyklus für virtuelle Hyper-V-Computer](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Andere Referenzen
* [Überwachung und Problembehandlung für den Schutz von virtuellen Computern und physischen Servern](site-recovery-monitoring-and-troubleshooting.md)
* [Microsoft-Support](site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
* [Häufige Fehler bei der automatischen Systemwiederherstellung und deren Lösungen](site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)

<!--HONumber=Oct16_HO2-->


