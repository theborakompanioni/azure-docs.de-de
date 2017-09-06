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
ms.date: 7/18/2017
ms.author: markgal;trinadhk
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 0c930c7413b24a811707c3a1ff3d7d70585bc528
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="plan-your-vm-backup-infrastructure-in-azure"></a>Planen der Sicherungsinfrastruktur für virtuelle Computer in Azure
Dieser Artikel enthält leistungs- und ressourcenbezogene Vorschläge, um Ihnen bei der Planung Ihrer Sicherungsinfrastruktur für virtuelle Computer helfen. Darüber hinaus werden in diesem Artikel zentrale Aspekte des Backup-Diensts definiert, die für Ihre Architektur sowie für die Kapazitäts- und Zeitplanung entscheidend sein können. Wenn Sie [Ihre Umgebung vorbereitet](backup-azure-vms-prepare.md) haben, ist die Planung der nächste Schritt, bevor Sie mit dem [Sichern Ihrer virtuellen Computer](backup-azure-vms.md) beginnen. Weitere Informationen zu virtuellen Azure-Computern finden Sie in der [Dokumentation zu Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="how-does-azure-back-up-virtual-machines"></a>Wie werden virtuelle Computer in Azure gesichert?
Wenn der Azure Backup-Dienst einen Sicherungsauftrag zur geplanten Zeit initiiert, löst er die Sicherungserweiterung zum Erstellen einer Momentaufnahme aus. Der Azure Backup-Dienst verwendet unter Windows die Erweiterung _VMSnapshot_ und unter Linux die Erweiterung _VMSnapshotLinux_. Die Erweiterung wird während der ersten VM-Sicherung installiert. Um die Erweiterung zu installieren, muss die VM ausgeführt werden. Wenn die VM nicht ausgeführt wird, erstellt der Backup-Dienst eine Momentaufnahme des zugrunde liegenden Speichers (da keine Schreibvorgänge der Anwendung erfolgen, während die VM beendet ist).

Beim Erstellen einer Momentaufnahme von Windows-VMs koordiniert sich der Backup-Dienst mit dem Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS), um eine konsistente Momentaufnahme der Datenträger des virtuellen Computers zu erhalten. Wenn Sie Linux-VMs sichern, können Sie eigene benutzerdefinierte Skripts zur Gewährleistung der Konsistenz beim Erstellen einer VM-Momentaufnahme schreiben. Einzelheiten zum Aufrufen dieser Skripts finden Sie weiter unten in diesem Artikel.

Nachdem der Azure Backup-Dienst die Momentaufnahme erstellt hat, werden die Daten in den Sicherungstresor übertragen. Um die Effizienz zu maximieren, werden vom Dienst nur diejenigen Datenblöcke bestimmt und übertragen, die seit der vorherigen Sicherung geändert wurden.

![Architektur der Sicherung von virtuellen Azure-Computern](./media/backup-azure-vms-introduction/vmbackup-architecture.png)

Wenn die Datenübertragung abgeschlossen ist, wird die Momentaufnahme entfernt und ein Wiederherstellungspunkt erstellt.

> [!NOTE]
> 1. Während des Sicherungsvorgangs berücksichtigt Azure Backup nicht den temporären Datenträger, der an den virtuellen Computer angefügt ist. Weitere Informationen finden Sie im Blogbeitrag zur [temporären Speicherung](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).
> 2. Da Azure Backup eine Momentaufnahme auf Speicherebene erstellt und diese Momentaufnahme in den Tresor überträgt, ändern Sie die Speicherkontoschlüssel erst nach Abschluss des Sicherungsauftrags.
> 3. Bei virtuellen Premium-Computern kopieren wir die Momentaufnahme in das Speicherkonto. Dadurch wird sichergestellt, dass dem Azure Backup-Dienst ausreichend IOPS für die Übertragung von Daten in den Tresor zur Verfügung stehen. Diese zusätzliche Kopie des Speichers wird gemäß der zugeteilten VM-Größe in Rechnung gestellt. 
>

### <a name="data-consistency"></a>Datenkonsistenz
Das Sichern und Wiederherstellen unternehmenskritischer Daten wird dadurch erschwert, dass die Daten während der Ausführung der Anwendungen gesichert werden müssen, von denen die Daten generiert werden. Dazu unterstützt Azure Backup anwendungskonsistente Sicherungen für virtuelle Windows- und Linux-Computer.
#### <a name="windows-vm"></a>Windows-VM
Azure Backup erstellt vollständige VSS-Sicherungen auf virtuellen Windows-Computern (weitere Informationen hierzu finden Sie unter [VSS Full Backup](http://blogs.technet.com/b/filecab/archive/2008/05/21/what-is-the-difference-between-vss-full-backup-and-vss-copy-backup-in-windows-server-2008.aspx)). Um VSS-Kopiesicherungen zu aktivieren, müssen die unten aufgeführten Registrierungsschlüssel auf dem virtuellen Computer festgelegt werden.

```
[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
"USEVSSCOPYBACKUP"="TRUE"
```

#### <a name="linux-vms"></a>Virtuelle Linux-Computer
Azure Backup bietet ein Skripterstellungsframework. Zur Gewährleistung der Anwendungskonsistenz beim Sichern von Linux-VMs erstellen Sie benutzerdefinierte Pre- und Post-Skripts zum Steuern des Workflows und der Umgebung von Sicherungen. Azure Backup ruft das Pre-Skript vor Erstellen der VM-Momentaufnahme und das Post-Skript nach Abschluss des Auftrags zur Erstellung der VM-Momentaufnahme auf. Weitere Informationen finden Sie unter [Anwendungskonsistente Sicherung von Azure Linux-VMs](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).
> [!NOTE]
> Azure Backup ruft nur vom Kunden geschriebene Pre- und Post-Skripts auf. Wenn das Pre-Skript und das Post-Skript erfolgreich ausgeführt werden, markiert Azure Backup den Wiederherstellungspunkt als anwendungskonsistent. Allerdings ist der Kunde bei Verwendung benutzerdefinierter Skripts letztendlich für die Anwendungskonsistenz verantwortlich.
>


In dieser Tabelle werden die Konsistenztypen aufgeführt und die Umstände erläutert, unter denen sie während der Sicherungs- und Wiederherstellungsvorgänge für virtuelle Azure-Computer auftreten.

| Konsistenz | VSS-basiert | Erklärung und Details |
| --- | --- | --- |
| Anwendungskonsistenz |Ja, für Windows|Anwendungskonsistenz ist ideal für Workloads, da Folgendes gewährleistet wird:<ol><li> Der virtuelle Computer wird *hochgefahren*. <li>Es gibt *keine Datenbeschädigung*. <li>Es gibt *keine Datenverluste*.<li> Die Daten sind mit der Anwendung konsistent, in der die Daten verwendet werden, da die Anwendung zum Zeitpunkt der Sicherung über den VSS oder das Pre-Skript und Post-Skript einbezogen wird.</ol> <li>*Virtuelle Windows-Computer:* Die meisten Microsoft-Workloads verfügen über VSS Writer, die workloadspezifische Aktionen im Hinblick auf die Datenkonsistenz ausführen. Beispielsweise umfasst Microsoft SQL Server einen VSS Writer, der die ordnungsgemäße Ausführung der Schreibvorgänge in die Transaktionsprotokolldatei und die Datenbank gewährleistet. Bei Azure-Sicherungen von virtuellen Windows-Computern muss zum Erstellen eines anwendungskonsistenten Wiederherstellungspunkts die Sicherungserweiterung den VSS-Workflow aufrufen und abschließen, bevor die Momentaufnahme des virtuellen Computers erstellt wird. Damit Momentaufnahmen von virtuellen Azure-Computern akkurat sind, müssen die VSS-Writer aller Azure VM-Anwendungen ebenfalls abgeschlossen werden. (Informieren Sie sich über die [Grundlagen von VSS](http://blogs.technet.com/b/josebda/archive/2007/10/10/the-basics-of-the-volume-shadow-copy-service-vss.aspx), und beschäftigen Sie sich ausführlicher mit der [Funktionsweise](https://technet.microsoft.com/library/cc785914%28v=ws.10%29.aspx).) </li> <li> *Linux-VMs:* Kunden können [ein benutzerdefiniertes Pre-Skript und Post-Skript zur Sicherstellung der Anwendungskonsistenz](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent) ausführen. </li> |
| Dateisystemkonsistenz |Ja, bei Windows-basierten Computern |Es gibt zwei Szenarien, in denen der Wiederherstellungspunkt *dateisystemkonsistent* sein kann:<ul><li>Sicherungen virtueller Linux-Computer in Azure ohne Pre-Skript und Post-Skript oder bei Fehlern des Pre-Skripts oder Post-Skripts. <li>VSS-Fehler bei der Sicherung für virtuelle Windows-Computer in Azure.</li></ul> In beiden Fällen muss Folgendes sichergestellt werden: <ol><li> Der virtuelle Computer wird *hochgefahren*. <li>Es gibt *keine Datenbeschädigung*.<li>Es gibt *keine Datenverluste*.</ol> Anwendungen müssen einen eigenen Reparaturmechanismus für die wiederhergestellten Daten implementieren. |
| Absturzkonsistenz |Nein |Diese Situation entspricht dem Absturz eines virtuellen Computers (durch eine Teil- oder Vollrückstellung). Absturzkonsistenz geschieht normalerweise, wenn der virtuelle Azure-Computer zum Zeitpunkt der Sicherung heruntergefahren wird. Ein ausfallsicherer Wiederherstellungspunkt gewährt keine Garantie für die Konsistenz der Daten auf dem Speichermedium – weder im Hinblick auf das Betriebssystem noch auf die Anwendung. Nur die Daten, die zum Zeitpunkt der Sicherung bereits auf dem Datenträger vorhanden sind, werden erfasst und gesichert. <br/> <br/> Auch wenn es keine Garantie gibt, wird das Betriebssystem normalerweise gestartet.In der Regel folgt ein Vorgang zur Datenträgerüberprüfung wie „chkdsk“, um eine eventuelle Datenbeschädigung zu reparieren. Alle Daten im Arbeitsspeicher oder Schreibvorgänge, die nicht vollständig auf den Datenträger übertragen wurden, gehen verloren. Die Anwendung folgt normalerweise mit einem eigenen Überprüfungsmechanismus, falls ein Datenrollback durchgeführt werden muss. <br><br>Falls das Transaktionsprotokoll beispielsweise Einträge enthält, die nicht in der Datenbank vorhanden sind, führt die Datenbanksoftware ein Rollback durch, bis die Daten konsistent sind. Wenn Daten über mehrere virtuelle Laufwerke verteilt sind (z. B. übergreifende Volumes), bietet ein absturzkonsistenter Wiederherstellungspunkt keine Garantie für die Richtigkeit der Daten. |

## <a name="performance-and-resource-utilization"></a>Leistung und Ressourcenverwendung
Genau wie bei lokal bereitgestellter Sicherungssoftware muss auch bei der Planung der Sicherung von virtuellen Computern in Azure der Kapazitäts- und Ressourcenbedarf berücksichtigt werden. Die [Azure Storage-Begrenzungen](../azure-subscription-service-limits.md#storage-limits) definieren, wie die VM-Bereitstellungen strukturiert werden, um maximale Leistung bei minimaler Beeinträchtigung der ausgeführten Workloads zu erzielen.

Achten Sie bei der Planung der Sicherungsleistung insbesondere auf die folgenden Grenzwerte von Azure Storage:

* Max. Ausgang pro Speicherkonto
* Gesamtanforderungsrate pro Speicherkonto

### <a name="storage-account-limits"></a>Speicherkontobegrenzungen
Beim Kopieren von Sicherungsdaten aus einem Speicherkonto werden diese auf die Metriken des Speicherkontos für IOPS (Input/Output Operations Per Second, Eingabe-/Ausgabevorgänge pro Sekunde) und Ausgang (oder Durchsatz) angerechnet. Zur gleichen Zeit benötigen die virtuellen Computer ebenfalls IOPS und Durchsatz. Ziel ist, dafür zu sorgen, dass der Datenverkehr der Sicherung und virtuellen Computer die Speicherkontogrenzwerte nicht überschreitet.

### <a name="number-of-disks"></a>Anzahl der Datenträger
Bei der Sicherung wird versucht, einen Sicherungsauftrag so schnell wie möglich abzuschließen. Dabei werden so viele Ressourcen wie möglich genutzt. Sämtliche E/A-Vorgänge sind jedoch begrenzt durch den *Zieldurchsatz pro Einzelblob*, der auf 60 MB pro Sekunde beschränkt ist. Zur Maximierung der Geschwindigkeit wird versucht, die einzelnen VM-Datenträger *parallel*zu sichern. Bei einem virtuellen Computer mit vier Datenträgern versucht der Dienst, alle vier Datenträger parallel zu sichern. Die **Anzahl von Datenträgern**, die gesichert werden, ist der entscheidende Faktor beim Bestimmen des Sicherungsdatenverkehr eines Speicherkontos.

### <a name="backup-schedule"></a>Sicherungszeitplan
Ein weiterer Faktor, der sich auf die Leistung auswirkt, ist der **Sicherungszeitplan**. Wenn Sie die Richtlinien so konfigurieren, dass alle virtuellen Computer gleichzeitig gesichert werden, haben Sie einen Datenverkehrsstau geplant. Bei der Sicherung wird versucht, alle Datenträger parallel zu sichern. Um den Sicherungsdatenverkehr eines Speicherkontos zu reduzieren, sichern Sie verschiedene VM zu unterschiedlichen Tageszeiten ohne Überschneidungen.

## <a name="capacity-planning"></a>Kapazitätsplanung
Zusammenfassend folgt aus diesen Faktoren, dass die Speicherkontoverwendung der Planung bedarf. Laden Sie das [Excel-Arbeitsblatt zur Kapazitätsplanung für VM-Sicherungen](https://gallery.technet.microsoft.com/Azure-Backup-Storage-a46d7e33) herunter, um die Auswirkungen Ihrer Datenträger- und Sicherungszeitplanauswahl zu überprüfen.

### <a name="backup-throughput"></a>Sicherungsdurchsatz
Für jeden zu sichernden Datenträger liest Azure Backup die Blöcke auf dem Datenträger und speichert nur die geänderten Daten (inkrementelle Sicherung). Die folgende Tabelle zeigt die durchschnittlichen Durchsatzwerte eines Backup-Diensts. Anhand der folgenden Daten können Sie die Zeit einschätzen, die zur Sicherung eines Datenträgers mit einer bestimmten Größe benötigt wird.

| Sicherungsvorgang | Optimaler Durchsatz |
| --- | --- |
| Erste Sicherung |160 MBit/s |
| Inkrementelle Sicherung (DR) |640 MBit/s  <br><br> Der Durchsatz sinkt erheblich, wenn die geänderten Daten (die gesichert werden müssen) gleichmäßig über den Datenträger verteilt sind.|

## <a name="total-vm-backup-time"></a>Gesamtdauer der VM-Sicherung
Obgleich die Mehrheit der Sicherungszeit für das Lesen und Kopieren von Daten aufgewendet wird, wirken sich andere Vorgänge ebenfalls auf die Gesamtdauer einer VM-Sicherung aus:

* Die Zeit, die zum [Installieren oder Aktualisieren der Sicherungserweiterung](backup-azure-vms.md)benötigt wird.
* Zeit für Momentaufnahme: Die zum Auslösen einer Momentaufnahme erforderliche Zeit. Momentaufnahmen werden kurz vor der geplanten Sicherungszeit ausgelöst.
* Wartezeit in der Warteschlange. Da der Backup-Dienst Sicherungen von mehreren Kunden verarbeitet, werden die Sicherungsdaten der Momentaufnahme möglicherweise nicht sofort in die Sicherung oder in den Recovery Services-Tresor kopiert. Zu Spitzenzeiten kann die Wartezeit aufgrund der Anzahl zu verarbeitender Sicherungen bis zu acht Stunden betragen. Die Gesamtdauer der VM-Sicherung wird jedoch bei täglichen Sicherungsrichtlinien weniger als 24 Stunden betragen.
* Die Datenübertragungszeit ist die Zeit, die der Sicherungsdienst zum Berechnen von inkrementellen Änderungen aus der vorherigen Sicherung und der Übertragung dieser Änderungen in den Tresorspeicher benötigt.

### <a name="why-am-i-observing-longer12-hours-backup-time"></a>Warum stelle ich eine längere Sicherungsdauer fest (> 12 Stunden)?
Die Sicherung besteht aus zwei Phasen: Erstellen von Momentaufnahmen und Übertragen der Momentaufnahmen in den Tresor. Der Backup-Dienst ist für Speicher optimiert. Wenn die Daten einer Momentaufnahme in den Tresor übertragen werden, überträgt der Dienst nur inkrementelle Änderungen aus den vorherigen Momentaufnahmen.  Um inkrementelle Änderungen zu bestimmen, berechnet der Dienst die Prüfsumme der Blöcke. Wenn ein Block geändert wird, wird der Block als Block identifiziert, der in den Tresor gesendet werden soll. Der Dienst untersucht jeden identifizierten Block noch näher und sucht nach Möglichkeiten, die Menge der zu übertragenden Daten zu verringern. Nach dem Evaluieren aller geänderten Blöcke fügt der Dienst die Änderungen wieder zusammen und sendet sie an den Tresor. Bei einigen Legacyanwendungen sind kleine, fragmentierte Schreibvorgänge nicht optimal für die Speicherung. Wenn die Momentaufnahme viele kleine, fragmentierte Schreibvorgänge enthält, verbringt der Dienst zusätzliche Zeit mit dem Verarbeiten der Daten, die von den Anwendungen geschrieben wurden. Der von Azure empfohlene Anwendungsschreibblock für Anwendungen, die innerhalb des virtuellen Computers ausgeführt werden, ist mindestens 8 KB groß. Wenn Ihre Anwendung einen Block von weniger als 8 KB verwendet, wird die Sicherungsleistung beeinflusst. Hilfe bei der Optimierung Ihrer Anwendung zur Verbesserung der Sicherungsleistung finden Sie unter [Anwendungen für die optimale Leistung mit Azure Storage optimieren](../storage/common/storage-premium-storage-performance.md). Obwohl der Artikel über die Sicherungsleistung Storage Premium-Beispiele verwendet, kann dieser Leitfaden auch für Standard-Datenträger angewendet werden.

## <a name="total-restore-time"></a>Gesamtzeit der Wiederherstellung
Ein Wiederherstellungsvorgang umfasst zwei zentrale Teilvorgänge: das Kopieren von Daten aus dem Tresor in das ausgewählte Kundenspeicherkonto und das Erstellen des virtuellen Computers. Das Kopieren von Daten aus dem Tresor hängt vom internen Speicherort von Sicherungen in Azure sowie vom Speicherort des Kundenspeicherkontos ab. Die Dauer des Datenkopiervorgangs hängt von folgenden Faktoren ab:
* Wartezeit in der Warteschlange: Da der Dienst Wiederherstellungsaufträge von mehreren Kunden gleichzeitig verarbeitet, werden Wiederherstellungsanforderungen in eine Warteschlange eingereiht.
* Zeit für das Kopieren von Daten: Daten werden zuerst aus dem Tresor in das Speicherkonto des Kunden kopiert. Die Wiederherstellungsdauer hängt vom IOPS-Wert und Durchsatz ab, den der Azure Backup-Dienst für das Speicherkonto des ausgewählten Kunden erhält. Wählen Sie ein Speicherkonto aus, das nicht mit anderen Schreib- und Lesevorgängen anderer Anwendungen geladen ist, um die Zeit für das Kopieren zu verringern.

## <a name="best-practices"></a>Bewährte Methoden
Berücksichtigen Sie beim Konfigurieren von Sicherungen für virtuelle Computer die folgenden bewährten Methoden:

* Planen Sie niemals die gleichzeitige Sicherung von mehr als 10 klassischen virtuellen Computern aus dem gleichen Clouddienst. Staffeln Sie die Sicherungszeitpläne mit einem Versatz von jeweils einer Stunde, wenn Sie mehrere VMs des gleichen Clouddiensts sichern möchten.
* Planen Sie keine gleichzeitige Sicherung von mehr als 40 virtuellen Computern.
* Planen Sie VM-Sicherungen für Nebenzeiten ein. Auf diese Weise nutzt der Backup-Dienst den IOPS-Wert zum Übertragen von Daten aus dem Speicherkonto des Kunden in den Tresor.
* Sorgen Sie dafür, dass eine Richtlinie für virtuelle Computer angewendet wird, die auf verschiedene Speicherkonten verteilt sind. Mit einem Sicherungszeitplan sollten insgesamt maximal 20 Datenträger eines einzelnen Speicherkontos geschützt werden. Wenn ein Speicherkonto mehr als 20 Datenträger enthält, verteilen Sie die entsprechenden virtuellen Computer auf mehrere Richtlinien, damit in der Übertragungsphase der Sicherung die erforderlichen IOPS zur Verfügung stehen.
* Stellen Sie einen virtuellen Computer unter Storage Premium nicht unter dem gleichen Speicherkonto wieder her. Wenn der Wiederherstellungsvorgang zur gleichen Zeit stattfindet wie der Sicherungsvorgang, verringern sich dadurch die für die Sicherung verfügbaren IOPS.
* Vergewissern Sie sich bei der VM-Premium-Sicherung, dass das Speicherkonto, das Premium-Datenträger hostet, mindestens über 50 % freien Staging-Speicherplatz für Momentaufnahmen für eine erfolgreiche Sicherung verfügt. 
* Stellen Sie sicher, dass die für die Sicherung aktivierten virtuellen Linux-Computer die Python-Version 2.7 enthalten.

## <a name="data-encryption"></a>Datenverschlüsselung
Azure Backup verschlüsselt die Daten während des Sicherungsvorgangs nicht. Sie können jedoch die Daten auf dem virtuellen Computer verschlüsseln und dann die geschützten Daten nahtlos sichern (erfahren Sie mehr über das [Sichern verschlüsselter Daten](backup-azure-vms-encryption.md)).

## <a name="calculating-the-cost-of-protected-instances"></a>Berechnen der Kosten von geschützten Instanzen
Für virtuelle Azure-Computer, die über Azure Backup gesichert werden, gelten die [Azure Backup-Preise](https://azure.microsoft.com/pricing/details/backup/). Die Berechnung der geschützten Instanzen basiert auf der *tatsächlichen* Größe des virtuellen Computers (also auf der Summe aller Daten auf dem virtuellen Computer – mit Ausnahme des „Ressourcendatenträgers“).

Die Kosten der Sicherung virtueller Computer basieren *nicht* auf der maximal unterstützten Größe für jeden an den virtuellen Computer angefügten Datenträger. Die Kosten basieren auf der tatsächlichen Menge der Daten, die auf dem Datenträger gespeichert sind. Gleichermaßen basiert die Abrechnung der Sicherungsspeicherung auf der Menge der in Azure Backup gespeicherten Daten, d.h. auf der Summe der tatsächlichen Daten an jedem Wiederherstellungspunkt.

Beispiel: virtueller Computer mit A2-Standardgröße und zwei zusätzlichen Datenträgern mit einer maximalen Größe von jeweils 1 TB. In der folgenden Tabelle sind die auf jedem Datenträger tatsächlich gespeicherten Daten aufgeführt:

| Datenträgertyp | Max. Größe | Tatsächliche Daten |
| --- | --- | --- |
| Betriebssystem-Datenträger |1023 GB |17 GB |
| Lokaler Datenträger/Ressourcendatenträger |135 GB |5 GB (bei der Sicherung nicht enthalten) |
| Datenträger 1 |1023 GB |30 GB |
| Datenträger 2 |1023 GB |0 GB |

Die *tatsächliche* Größe des virtuellen Computers in diesem Fall ist 17 GB + 30 GB + 0 GB = 47 GB. Diese Größe der geschützten Instanz (47 GB) wird zur Basis für die monatliche Rechnung. Mit zunehmender Datenmenge auf dem virtuellen Computer ändert sich entsprechend auch die Größe der geschützten Instanz, die für die Abrechnung verwendet wird.

Die Abrechnung erfolgt erst nach dem erfolgreichen Abschluss der ersten Sicherung. Zu diesem Zeitpunkt beginnt die Abrechnung für die Speicherung und die geschützten Instanzen. Die Abrechnung erfolgt so lange, wie *Sicherungsdaten für den virtuellen Computer in einem Tresor gespeichert* werden. Wenn Sie den Schutz des virtuellen Computers beenden, aber VM-Sicherungsdaten in einem Tresor vorhanden sind, wird die Abrechnung fortgesetzt.

Die Abrechnung für einen bestimmten virtuellen Computer endet erst, wenn der Schutz beendet wird *und* alle Sicherungsdaten gelöscht werden. Wenn der Schutz beendet wird und es keine aktiven Sicherungsaufträge gibt, wird die Größe der letzten erfolgreichen VM-Sicherung zur Größe der geschützten Instanz für die monatliche Rechnung.

## <a name="questions"></a>Fragen?
Wenn Sie Fragen haben oder Anregungen zu gewünschten Funktionen mitteilen möchten, [senden Sie uns Ihr Feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nächste Schritte
* [Sichern virtueller Computer](backup-azure-vms.md)
* [Verwalten der Sicherung virtueller Computer](backup-azure-manage-vms.md)
* [Wiederherstellen virtueller Computer](backup-azure-restore-vms.md)
* [Problembehandlung bei der Sicherung virtueller Computer](backup-azure-vms-troubleshoot.md)

