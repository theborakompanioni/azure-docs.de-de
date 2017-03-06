---
title: "Planen der Sicherungsinfrastruktur für virtuelle Computer in Azure | Microsoft Docs"
description: Wichtige Aspekte beim Planen der Sicherung virtueller Computer in Azure
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: Sichern virtueller Computer, Sichern von VMs
ms.assetid: 19d2cf82-1f60-43e1-b089-9238042887a9
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: markgal;trinadhk
translationtype: Human Translation
ms.sourcegitcommit: d7a2b9c13b2c3372ba2e83f726c7bf5cc7e98c02
ms.openlocfilehash: 4fae07988dea260776368162c03374d83bc55664
ms.lasthandoff: 02/17/2017


---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>Planen der Sicherungsinfrastruktur für virtuelle Computer in Azure
Dieser Artikel enthält leistungs- und ressourcenbezogene Vorschläge, um Ihnen bei der Planung Ihrer Sicherungsinfrastruktur für virtuelle Computer helfen. Darüber hinaus werden in diesem Artikel zentrale Aspekte des Backup-Diensts definiert, die für Ihre Architektur sowie für die Kapazitäts- und Zeitplanung entscheidend sein können. Wenn Sie [Ihre Umgebung vorbereitet](backup-azure-vms-prepare.md) haben, ist dies der nächste Schritt, bevor Sie mit dem [Sichern Ihrer virtuellen Computer](backup-azure-vms.md) beginnen. Weitere Informationen zu virtuellen Azure-Computern finden Sie in der [Dokumentation zu Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="how-does-azure-back-up-virtual-machines"></a>Wie werden virtuelle Computer in Azure gesichert?
Wenn der Azure Backup-Dienst einen Sicherungsauftrag zur geplanten Zeit initiiert, löst er die Sicherungserweiterung zum Erstellen einer Momentaufnahme aus. Diese Momentaufnahme wird in Koordination mit dem Volumeschattenkopie-Dienst (VSS) erstellt, um eine konsistente Momentaufnahme der Datenträger auf dem virtuellen Computer zu erhalten, ohne dass dieser heruntergefahren werden muss.

Nachdem die Momentaufnahme erstellt wurde, werden die Daten vom Azure Backup-Dienst an den Sicherungstresor übertragen. Um den Sicherungsvorgang effizienter zu gestalten, werden vom Dienst nur diejenigen Datenblöcke identifiziert und übertragen, die seit der letzten Sicherung geändert wurden.

![Architektur der Sicherung von virtuellen Azure-Computern](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Wenn die Datenübertragung abgeschlossen ist, wird die Momentaufnahme entfernt und ein Wiederherstellungspunkt erstellt.

> [!NOTE]
> Azure Backup berücksichtigt bei der Durchführung der Sicherung keine temporären Datenträger, die dem virtuellen Computer angefügt sind. Erfahren Sie mehr über [temporäre Datenträger](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
>
>

### <a name="data-consistency"></a>Datenkonsistenz
Das Sichern und Wiederherstellen unternehmenskritischer Daten wird dadurch erschwert, dass die Daten während der Ausführung der Anwendungen gesichert werden müssen, von denen die Daten generiert werden. Hierfür bietet Azure Backup anwendungskonsistente Sicherungen für Microsoft-Workloads mithilfe des Volumeschattenkopie-Diensts, um sicherzustellen, dass die Daten ordnungsgemäß in den Speicher geschrieben werden.

> [!NOTE]
> Für virtuelle Linux-Computer sind nur dateikonsistente Sicherungen möglich, da Linux keine dem VSS entsprechende Plattform umfasst.
>
>

Azure Backup erstellt vollständige VSS-Sicherungen auf virtuellen Windows-Computern (weitere Informationen hierzu finden Sie unter [VSS Full Backup](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). Um VSS-Kopiesicherungen zu aktivieren, müssen die folgenden Registrierungsschlüssel auf dem virtuellen Computer festgelegt werden.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```


In dieser Tabelle werden die Konsistenztypen aufgeführt und die Umstände erläutert, unter denen sie während der Sicherungs- und Wiederherstellungsvorgänge für virtuelle Azure-Computer auftreten.

| Konsistenz | VSS-basiert | Erklärung und Details |
| --- | --- | --- |
| Anwendungskonsistenz |Ja |Dies ist der ideale Konsistenztyp für Microsoft-Workloads, da Folgendes gewährleistet wird:<ol><li> Der virtuelle Computer wird *hochgefahren*. <li>Es gibt *keine Datenbeschädigung*. <li>Es gibt *keine Datenverluste*.<li> Die Daten sind mit der Anwendung konsistent, in der die Daten verwendet werden, da die Anwendung zum Zeitpunkt der Sicherung über den VSS einbezogen wird.</ol> Die meisten Microsoft-Workloads verfügen über VSS Writer, die workloadspezifische Aktionen im Zusammenhang mit der Datenkonsistenz ausführen. Beispielsweise umfasst Microsoft SQL Server einen VSS Writer, der die ordnungsgemäße Ausführung der Schreibvorgänge in die Transaktionsprotokolldatei und die Datenbank gewährleistet.<br><br> Bei Azure-VM-Sicherungen bedeutet das Erhalten eines anwendungskonsistenten Wiederherstellungspunkts, dass die Sicherungserweiterung den VSS-Workflow aufrufen und *ordnungsgemäß* abschließen konnte, bevor die Momentaufnahme des virtuellen Computers erstellt wurde. Das bedeutet, dass auch die VSS Writer aller Anwendungen auf dem virtuellen Azure-Computer aufgerufen wurden.<br><br>(Informieren Sie sich über die [Grundlagen von VSS](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx), und beschäftigen Sie sich ausführlicher mit der [Funktionsweise](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx).) |
| Dateisystemkonsistenz |Ja, bei Windows-basierten Computern |Es gibt zwei Szenarien, in denen der Wiederherstellungspunkt *dateisystemkonsistent* sein kann:<ul><li>Sicherung virtueller Linux-Computer in Azure, da Linux über keine entsprechende VSS-Plattform verfügt.<li>VSS-Fehler bei der Sicherung für virtuelle Windows-Computer in Azure.</li></ul> In beiden Fällen muss Folgendes sichergestellt werden: <ol><li> Der virtuelle Computer wird *hochgefahren*. <li>Es gibt *keine Datenbeschädigung*.<li>Es gibt *keine Datenverluste*.</ol> Anwendungen müssen einen eigenen Reparaturmechanismus für die wiederhergestellten Daten implementieren. |
| Absturzkonsistenz |Nein |Diese Situation entspricht dem Absturz eines virtuellen Computers (durch eine Teil- oder Vollrückstellung). Dies geschieht normalerweise, wenn zum Zeitpunkt der Sicherung der virtuelle Azure-Computer heruntergefahren wird. Bei Azure-VM-Sicherungen bedeutet das Erhalten eines absturzkonsistenten Wiederherstellungspunkts, dass Azure Backup keine Garantie für die Konsistenz der Daten auf dem Speichermedium gewährt – weder im Hinblick auf das Betriebssystem noch im Hinblick auf die Anwendung. Nur Daten, die zum Zeitpunkt der Sicherung bereits auf dem Datenträger vorhanden sind, werden erfasst und gesichert. <br/> <br/> Das Betriebssystem wird in den meisten Fällen gestartet. In der Regel folgt eine Prozedur zur Datenträgerüberprüfung wie „chkdsk“, um eine mögliche Datenbeschädigung zu reparieren. Alle Daten im Arbeitsspeicher oder Schreibvorgänge, die nicht vollständig auf den Datenträger übertragen wurden, gehen verloren. Die Anwendung folgt normalerweise mit einem eigenen Überprüfungsmechanismus, falls ein Datenrollback durchgeführt werden muss. <br><br>Falls das Transaktionsprotokoll beispielsweise Einträge enthält, die nicht in der Datenbank vorhanden sind, führt die Datenbanksoftware ein Rollback durch, bis die Daten konsistent sind. Wenn Daten über mehrere virtuelle Laufwerke verteilt sind (z. B. übergreifende Volumes), bietet ein absturzkonsistenter Wiederherstellungspunkt keine Garantie für die Richtigkeit der Daten. |

## <a name="performance-and-resource-utilization"></a>Leistung und Ressourcenverwendung
Genau wie bei lokal bereitgestellter Sicherungssoftware muss auch bei der Planung der Sicherung von virtuellen Computern in Azure der Kapazitäts- und Ressourcenbedarf berücksichtigt werden. Die [Azure Storage-Begrenzungen](../azure-subscription-service-limits.md#storage-limits) definieren, wie die VM-Bereitstellungen strukturiert werden, um maximale Leistung bei minimaler Beeinträchtigung der ausgeführten Workloads zu erzielen.

Achten Sie bei der Planung der Sicherungsleistung insbesondere auf die folgenden Grenzwerte von Azure Storage:

* Max. Ausgang pro Speicherkonto
* Gesamtanforderungsrate pro Speicherkonto

### <a name="storage-account-limits"></a>Speicherkontobegrenzungen
Beim Kopieren von Sicherungsdaten aus einem Speicherkonto werden diese auf die Metriken des Speicherkontos für IOPS (Input/Output Operations Per Second, Eingabe-/Ausgabevorgänge pro Sekunde) und Ausgang (oder Durchsatz) angerechnet. Zur gleichen Zeit werden die virtuellen Computer ausgeführt und verbrauchen IOPS und Durchsatz. Das Ziel ist, dafür zu sorgen, dass der Datenverkehr insgesamt (Sicherung und virtueller Computer) die Speicherkontogrenzwerte nicht überschreitet.

### <a name="number-of-disks"></a>Anzahl der Datenträger
Bei der Sicherung wird versucht, einen Sicherungsauftrag so schnell wie möglich abzuschließen. Dabei werden so viele Ressourcen wie möglich genutzt. Sämtliche E/A-Vorgänge sind jedoch begrenzt durch den *Zieldurchsatz pro Einzelblob*, der auf 60 MB pro Sekunde beschränkt ist. Zur Maximierung der Geschwindigkeit wird versucht, die einzelnen VM-Datenträger *parallel*zu sichern. Bei einem virtuellen Computer mit vier Datenträgern versucht Azure Backup also, alle vier Datenträger parallel zu sichern. Der entscheidende Faktor für den ausgehenden Sicherungsdatenverkehr eines Kundenspeicherkonto ist daher die **Anzahl von Datenträgern** , die über das Speicherkonto gesichert werden.

### <a name="backup-schedule"></a>Sicherungszeitplan
Ein weiterer Faktor, der sich auf die Leistung auswirkt, ist der **Sicherungszeitplan**. Wenn Sie die Richtlinien so konfigurieren, dass alle virtuellen Computer gleichzeitig gesichert werden, haben Sie einen Datenverkehrsstau geplant. Bei der Sicherung wird versucht, alle Datenträger parallel zu sichern. Eine Möglichkeit zur Reduzierung des Sicherungsdatenverkehrs für ein Speicherkonto wäre, die einzelnen virtuellen Computer zu unterschiedlichen Tageszeiten zu sichern und Überschneidungen zu vermeiden.

## <a name="capacity-planning"></a>Kapazitätsplanung
Zusammenfassend folgt aus diesen Faktoren, dass die Speicherkontoverwendung einer gründlichen Planung bedarf. Laden Sie das [Excel-Arbeitsblatt zur Kapazitätsplanung für VM-Sicherungen](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) herunter, um die Auswirkungen Ihrer Datenträger- und Sicherungszeitplanauswahl zu überprüfen.

### <a name="backup-throughput"></a>Sicherungsdurchsatz
Für jeden zu sichernden Datenträger liest Azure Backup die Blöcke auf dem Datenträger und speichert nur die geänderten Daten (inkrementelle Sicherung). Diese Tabelle zeigt die durchschnittlichen Durchsatzwerte, die Sie von Azure Backup erwarten können. Anhand dieser Informationen können Sie die Zeitdauer schätzen, die zur Sicherung eines Datenträgers mit einer bestimmten Größe benötigt wird.

| Sicherungsvorgang | Optimaler Durchsatz |
| --- | --- |
| Erste Sicherung |160 MBit/s |
| Inkrementelle Sicherung (DR) |640 MBit/s  <br><br> Dieser Durchsatz kann erheblich sinken, wenn auf dem Datenträger viele verstreute Daten gesichert werden müssen. |

## <a name="total-vm-backup-time"></a>Gesamtdauer der VM-Sicherung
Obgleich ein Großteil der Sicherungszeit für das Lesen und Kopieren von Daten aufgewendet wird, gibt es noch andere Vorgänge, die sich auf die Gesamtdauer einer VM-Sicherung auswirken:

* Die Zeit, die zum [Installieren oder Aktualisieren der Sicherungserweiterung](backup-azure-vms.md)benötigt wird.
* Zeit für Momentaufnahme: Die zum Auslösen einer Momentaufnahme erforderliche Zeit. Momentaufnahmen werden kurz vor der geplanten Sicherungszeit ausgelöst.
* Wartezeit in der Warteschlange. Da der Backup-Dienst Sicherungen von mehreren Kunden verarbeitet, werden die Sicherungsdaten der Momentaufnahme möglicherweise nicht sofort in die Sicherung oder in den Recovery Services-Tresor kopiert. Zu Spitzenzeiten kann die Wartezeit aufgrund der Anzahl zu verarbeiteter Sicherungen bis zu acht Stunden betragen. Die Gesamtdauer der VM-Sicherung wird jedoch bei täglichen Sicherungsrichtlinien weniger als 24 Stunden betragen.
* Die Datenübertragungszeit ist die Zeit, die der Sicherungsdienst zum Berechnen von inkrementellen Änderungen aus der vorherigen Sicherung und der Übertragung dieser Änderungen in den Tresorspeicher benötigt.

### <a name="why-am-i-observing-longer15-hours-backup-time"></a>Warum stelle ich eine längere Sicherungszeit fest (>&15; Stunden)?
Die Sicherung besteht aus zwei Phasen: Erstellen der Momentaufnahme und Übertragen der Momentaufnahme in den Tresor. In der zweiten Phase, der Übertragung von Daten in den Tresor, um die Nutzung des zur Sicherung verwendeten Speichers zu optimieren, übertragen wir nur inkrementelle Änderungen aus der vorherigen Momentaufnahme. Um dies zu erreichen, wird die Prüfsumme der Blöcke berechnet, und wenn ein Block geändert wird, wird er zum Senden an den Tresor gekennzeichnet. Auch hier wird der Block näher untersucht, um festzustellen, ob die Menge der zu übertragenden Daten minimiert werden kann, und alle geänderten Blöcke werden zusammengefügt und an den Tresor gesendet. Bei einigen Legacyanwendungen wurde beobachtet, dass Schreibvorgänge von Anwendungen im Hinblick auf den Speicher nicht optimal sind, da es sich um kleine und fragmentierte Schreibvorgänge handelt. Darum nimmt das Verarbeiten von Daten, die von diesen Anwendungen geschrieben wurden, zusätzliche Zeit in Anspruch. Der von Azure empfohlene Anwendungsschreibblock für Anwendungen, die von virtuellen Computern ausgeführt werden, ist mindestens 8KB groß. Wenn Ihre Anwendung einen Block von weniger als 8KB verwendet, wird die Sicherungsleistung beeinträchtigt, da dies außerhalb der Azure-Empfehlungen liegt. Wir empfehlen Ihnen, sich über die [Optimierung von Anwendungen für eine optimale Leistung mit Azure-Speicher](../storage/storage-premium-storage-performance.md) zu informieren und herauszufinden, ob Sie Ihre Anwendung so optimieren können, dass sie optimal schreibt, um die Sicherungsleistung zu verbessern. Dieser Artikel berücksichtigt zwar in erster Linie Storage Premium, doch gilt er auch für Datenträger mit Standardspeicher.

## <a name="total-restore-time"></a>Gesamtzeit der Wiederherstellung
Ein Wiederherstellungsvorgang umfasst zwei zentrale Teilvorgänge: das Kopieren von Daten aus dem Tresor in das ausgewählte Kundenspeicherkonto und das Erstellen des virtuellen Computers. Das Kopieren von Daten aus dem Tresor hängt vom internen Speicherort von Sicherungen in Azure sowie vom Speicherort des Kundenspeicherkontos ab. Die Dauer des Datenkopiervorgangs hängt von folgenden Faktoren ab:
* Wartezeit in der Warteschlange: Da der Dienst Wiederherstellungen von mehreren Kunden gleichzeitig verarbeitet, werden Wiederherstellungsanforderungen in eine Warteschlange eingereiht.
* Zeit für das Kopieren von Daten: Daten werden ähnlich wie bei einem ersten Sicherungsprozess aus dem Tresor in das Kundenspeicherkonto kopiert. Wenn das Kundenspeicherkonto, in das der Sicherungsdienst Daten aus dem Tresor schreiben muss, geladen ist, kann das Kopieren länger dauern. Achten Sie also darauf, ein Speicherkonto auszuwählen, das während der Wiederherstellung nicht für andere Schreib- und Lesevorgänge der Anwendung geladen ist, um die Kopierdauer zu optimieren.

## <a name="best-practices"></a>Bewährte Methoden
Berücksichtigen Sie beim Konfigurieren von Sicherungen für virtuelle Computer die folgenden bewährten Methoden:

* Planen Sie niemals die gleichzeitige Sicherung von mehr als 10 klassischen virtuellen Computern aus dem gleichen Clouddienst. Staffeln Sie die Sicherungszeitpläne mit einem Versatz von jeweils einer Stunde, wenn Sie mehrere virtuelle Computer des gleichen Clouddiensts sichern möchten.
* Planen Sie keine gleichzeitige Sicherung von mehr als 40 virtuellen Computern.
* Planen Sie Sicherungen für virtuelle Computer außerhalb der Spitzenzeiten, damit der Sicherungsdienst die Daten aus dem Kundenspeicherkonto mithilfe von IOPS an die Sicherung oder den Recovery Services-Tresor überträgt.
* Sorgen Sie dafür, dass eine Richtlinie für virtuelle Computer angewendet wird, die auf verschiedene Speicherkonten verteilt sind. Mit einem Sicherungszeitplan sollten insgesamt maximal 20 Datenträger eines einzelnen Speicherkontos geschützt werden. Wenn ein Speicherkonto mehr als 20 Datenträger enthält, verteilen Sie die entsprechenden virtuellen Computer auf mehrere Richtlinien, damit in der Übertragungsphase der Sicherung die erforderlichen IOPS zur Verfügung stehen.
* Stellen Sie einen virtuellen Computer unter Storage Premium nicht unter dem gleichen Speicherkonto wieder her. Wenn der Wiederherstellungsvorgang zur gleichen Zeit stattfindet wie der Sicherungsvorgang, verringern sich dadurch die für die Sicherung verfügbaren IOPS.
* Wir empfehlen, jeden virtuellen Premium-Computer unter einem anderen Storage Premium-Konto auszuführen, um die Leistung bei der Sicherung nicht zu beeinträchtigen.

## <a name="data-encryption"></a>Datenverschlüsselung
Azure Backup verschlüsselt die Daten während des Sicherungsvorgangs nicht. Sie können jedoch die Daten auf dem virtuellen Computer verschlüsseln und dann die geschützten Daten nahtlos sichern (erfahren Sie mehr über das [Sichern verschlüsselter Daten](backup-azure-vms-encryption.md)).

## <a name="how-are-protected-instances-calculated"></a>Wie werden geschützte Instanzen berechnet?
Für virtuelle Azure-Computer, die über Azure Backup gesichert werden, gelten die [Azure Backup-Preise](https://azure.microsoft.com/pricing/details/backup/). Die Berechnung der geschützten Instanzen basiert auf der *tatsächlichen* Größe des virtuellen Computers (also auf der Summe aller Daten auf dem virtuellen Computer – mit Ausnahme des Ressourcendatenträgers).

Ihnen wird *nicht* die maximal unterstützte Größe für jeden mit dem virtuellen Computer verknüpften Datenträger, sondern die Größe der tatsächlich auf dem jeweiligen Datenträger gespeicherten Daten berechnet. Gleichermaßen basiert die Berechnung der Sicherungsspeicherung auf der Menge der mit Azure Backup gespeicherten Daten, d. h. auf der Summe der tatsächlichen Daten in jedem Wiederherstellungspunkt.

Beispiel: virtueller Computer mit A2-Standardgröße und zwei zusätzlichen Datenträgern mit einer maximalen Größe von jeweils 1 TB. In der folgenden Tabelle sind die auf jedem Datenträger tatsächlich gespeicherten Daten aufgeführt:

| Datenträgertyp | Max. Größe | Tatsächliche Daten |
| --- | --- | --- |
| Betriebssystem-Datenträger |1023 GB |17 GB |
| Lokaler Datenträger/Ressourcendatenträger |135 GB |5 GB (bei der Sicherung nicht enthalten) |
| Datenträger 1 |1023 GB |30 GB |
| Datenträger 2 |1023 GB |0 GB |

Die *tatsächliche* Größe des virtuellen Computers in diesem Fall ist 17 GB + 30 GB + 0 GB = 47 GB. Dies wird in der monatlichen Rechnung als Größe der geschützten Instanz abgerechnet. Mit zunehmender Datenmenge auf dem virtuellen Computer ändert sich entsprechend auch die Größe der geschützten Instanz, die für die Abrechnung verwendet wird.

Die Abrechnung erfolgt erst nach dem erfolgreichen Abschluss der ersten Sicherung. Zu diesem Zeitpunkt beginnt die Abrechnung für die Speicherung und die geschützten Instanzen. Die Abrechnung erfolgt so lange, wie *Sicherungsdaten mit Azure Backup für den virtuellen Computer gespeichert* werden. Durch Durchführen des Vorgangs "Schutz beenden" wird die Abrechnung nicht beendet, wenn Sicherungsdaten weiterhin beibehalten werden.

Die Abrechnung für einen bestimmten virtuellen Computer endet nur, wenn der Schutz beendet wird *und* alle Sicherungsdaten gelöscht werden. Wenn keine aktiven Sicherungsaufträge vorhanden sind (wenn der Schutz beendet wurde), wird die Größe des virtuellen Computers zum Zeitpunkt der letzten erfolgreichen Sicherung in der monatlichen Rechnung als Größe der geschützten Instanz angesetzt.

## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nächste Schritte
* [Sichern virtueller Computer](backup-azure-vms.md)
* [Verwalten der Sicherung virtueller Computer](backup-azure-manage-vms.md)
* [Wiederherstellen virtueller Computer](backup-azure-restore-vms.md)
* [Problembehandlung bei der Sicherung virtueller Computer](backup-azure-vms-troubleshoot.md)

