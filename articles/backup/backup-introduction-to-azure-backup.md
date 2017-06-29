---
title: Was ist Azure Backup? | Microsoft Docs
description: "Mithilfe von Azure Backup können Sie Daten und Workloads auf Windows Server-Computern, Windows-Arbeitsstationen, System Center DPM-Servern und virtuellen Azure-Computern sichern und wiederherstellen."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "Sichern und Wiederherstellen; Wiederherstellungsdienste; Sicherungslösungen"
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 6/14/2017
ms.author: markgal;trinadhk;anuragm
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 629b3c54a35f939845349e6f2ed7743885e41f3b
ms.contentlocale: de-de
ms.lasthandoff: 06/16/2017


---
# <a name="overview-of-the-features-in-azure-backup"></a>Übersicht über die Funktionen in Azure Backup
Azure Backup ist der Azure-basierte Dienst, den Sie zum Sichern (bzw. Schützen) und Wiederherstellen Ihrer Daten in der Microsoft Cloud verwenden können. Azure Backup ersetzt Ihre vorhandene lokale bzw. standortexterne Lösung durch eine zuverlässige, sichere und wirtschaftliche Cloudlösung. Azure Backup verfügt über mehrere Komponenten, die Sie herunterladen und auf dem jeweiligen Computer, Server oder in der Cloud bereitstellen. Die Komponente (der Agent), die Sie bereitstellen, richtet sich danach, was geschützt werden soll. Alle Azure Backup-Komponenten (unabhängig davon, ob Daten lokal oder in der Cloud geschützt werden sollen) können genutzt werden, um Daten in einem Recovery Services-Tresor in Azure zu sichern. Informationen dazu, welche Komponente zum Schützen bestimmter Daten, Anwendungen oder Workloads geeignet ist, finden Sie in der [Tabelle mit den Azure Backup-Komponenten](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (weiter unten in diesem Artikel).

[Video mit einem Überblick über Azure Backup](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Gründe für Azure Backup
Herkömmliche Sicherungslösungen haben sich dahingehend entwickelt, dass die Cloud ähnlich wie bei einer Festplatte oder einem Band als Endpunkt bzw. statisches Speicherziel behandelt wird. Dieser Ansatz ist zwar einfach, aber er unterliegt auch Beschränkungen, und es werden nicht alle Vorteile einer zugrunde liegenden Cloudplattform genutzt. Dies führt zu einer teuren und ineffizienten Lösung. Andere Lösungen sind kostenintensiv, da Sie für die falsche Art von Speicher oder für nicht benötigten Speicher zahlen. Andere Lösungen sind häufig ineffizient, da Sie nicht die Art oder die Menge an benötigtem Speicher erhalten oder weil zu viel Zeit für Verwaltungsaufgaben aufgewendet werden muss. Im Gegensatz dazu bietet Azure Backup diese wichtigen Vorteile:

**Automatische Speicherverwaltung**: Für Hybridumgebungen ist häufig heterogener Speicher erforderlich – teilweise lokal und teilweise in der Cloud. Bei Azure Backup fallen keine Kosten für die Verwendung von lokalen Speichergeräten an. Azure Backup sorgt im Rahmen eines Modells mit nutzungsbasierter Bezahlung für die automatische Zuteilung und Verwaltung von Sicherungsspeicher. Nutzungsbasierte Bezahlung bedeutet, dass Sie nur für den Speicher zahlen, den Sie verbrauchen. Weitere Informationen finden Sie im [Artikel zu den Preisen von Azure](https://azure.microsoft.com/pricing/details/backup).

**Unbegrenzte Skalierung**: Für Azure Backup wird die Leistungsstärke und unbegrenzte Skalierung der Azure-Cloud genutzt, um eine hohe Verfügbarkeit sicherzustellen – ohne Wartungs- oder Überwachungsaufwand. Sie können Warnungen einrichten, um Informationen zu Ereignissen bereitzustellen, aber Sie müssen sich nicht um die hohe Verfügbarkeit für Ihre Daten in der Cloud kümmern.

**Mehrere Speicheroptionen**: Ein Aspekt der hohen Verfügbarkeit ist die Speicherreplikation. Azure Backup bietet zwei Arten der Replikation: mit [lokal redundantem Speicher](../storage/storage-redundancy.md#locally-redundant-storage) und mit [georedundantem Speicher](../storage/storage-redundancy.md#geo-redundant-storage). Wählen Sie die Sicherungsspeicheroption aus, die zu Ihren Anforderungen passt:

* Lokal redundanter Speicher (Locally Redundant Storage, LRS) repliziert Ihre Daten dreimal in einem gekoppelten Datencenter in der gleichen Region. (Es werden also drei Kopien Ihrer Daten erstellt.) LRS ist eine kostengünstige Möglichkeit, um Daten vor lokalen Hardwarefehlern zu schützen.

* Georedundanter Speicher (GRS) repliziert Ihre Daten in eine sekundäre Region, die mehrere hundert Kilometer vom primären Speicherort der Quelldaten entfernt ist. GRS führt zu höheren Kosten als LRS, bietet aber eine höhere Haltbarkeit Ihrer Daten (auch im Falle eines regionalen Ausfalls).

**Unbegrenzte Datenübertragungen**: Bei Azure Backup ist die Menge der übertragenen eingehenden und ausgehenden Daten nicht beschränkt. Außerdem fallen bei Azure Backup keine Gebühren für die übertragenen Daten an. Aber wenn Sie den Azure Import/Export-Dienst nutzen, um große Datenmengen zu importieren, werden für eingehende Daten Kosten berechnet. Weitere Informationen zu diesen Kosten finden Sie unter [Workflow zur Offlinesicherung in Azure Backup](backup-azure-backup-import-export.md). Ausgehende Daten sind Daten, die während eines Wiederherstellungsvorgangs aus einem Recovery Services-Tresor übertragen werden.

**Datenverschlüsselung**: Die Datenverschlüsselung ermöglicht eine sichere Übertragung und Speicherung Ihrer Daten in der öffentlichen Cloud. Sie speichern die Passphrase für die Verschlüsselung lokal, und sie wird niemals in Azure übertragen oder gespeichert. Wenn Daten wiederhergestellt werden sollen, sind nur Sie im Besitz der Passphrase für die Verschlüsselung bzw. des Schlüssels.

**Anwendungskonsistente Sicherungen**: Unabhängig davon, ob Sie einen Dateiserver, einen virtuellen Computer oder eine SQL-Datenbank sichern, müssen Sie wissen, dass ein Wiederherstellungspunkt über alle erforderlichen Daten zum Wiederherstellen der Sicherungskopie verfügt. Azure Backup umfasst anwendungskonsistente Sicherungen, sodass sichergestellt ist, dass zum Wiederherstellen der Daten keine zusätzlichen Fixes benötigt werden. Durch die Wiederherstellung von anwendungskonsistenten Daten wird die Wiederherstellungsdauer reduziert, sodass Sie schnell zum Zustand der normalen Ausführung zurückkehren können.

**Langfristige Aufbewahrung:** Anstatt Sicherungskopien auf Band zu erstellen und das Band dann an einen anderen Ort zu bringen, können Sie Azure für die kurz- und langfristige Aufbewahrung nutzen. Die Zeit, für die Sie Daten in einem Backup- oder Recovery Services-Tresor aufbewahren, wird von Azure nicht begrenzt. Sie können Daten also beliebig lange in einem Tresor aufbewahren. Bei Azure Backup gilt pro geschützter Instanz ein Limit von 9999 Wiederherstellungspunkten. Informationen zu den möglichen Auswirkungen auf Ihre Sicherungsanforderungen finden Sie im Abschnitt [Sicherung und Aufbewahrung](backup-introduction-to-azure-backup.md#backup-and-retention).  

## <a name="which-azure-backup-components-should-i-use"></a>Welche Azure Backup-Komponenten sollte ich verwenden?
Falls Sie unsicher sind, welche Azure Backup-Komponente für Ihre Anforderungen geeignet ist, helfen Ihnen die Informationen in der folgenden Tabelle weiter. Es ist jeweils angegeben, was Sie mit einer Komponente schützen können. Das Azure-Portal enthält einen Assistenten, der in das Portal integriert ist und Sie bei der Auswahl der Komponente unterstützt, die heruntergeladen und bereitgestellt werden soll. Mit dem Assistenten, der Teil der Erstellung des Recovery Services-Tresors ist, werden Sie schrittweise durch die Auswahl eines Sicherungsziels geführt, und Sie können die zu schützenden Daten oder die Anwendung auswählen.

| Komponente | Vorteile | Einschränkungen | Was wird geschützt? | Wo werden Sicherungen gespeichert? |
| --- | --- | --- | --- | --- |
| Azure Backup-Agent (MARS) |<li>Sicherung von Dateien und Ordnern auf physischen oder virtuellen Windows-Computern (virtuelle Computer können sich am lokalen Standort oder in Azure befinden)<li>Kein separater Sicherungsserver erforderlich |<li>Drei Sicherungen pro Tag <li>Nicht anwendungsorientiert, nur Wiederherstellung auf Datei-, Ordner- und Volumeebene <li>  Keine Unterstützung für Linux |<li>Dateien <li>Ordner |Recovery Services-Tresor |
| System Center DPM |<li>Anwendungsabhängige Momentaufnahmen (VSS)<li>Vollständige Flexibilität in Bezug auf Sicherungszeitpunkt<li>Wiederherstellungsgranularität (alle)<li>Kann einen Recovery Services-Tresor verwenden<li>Linux-Unterstützung für Hyper-V- und VMware-VMs <li>Sichern und Wiederherstellen von virtuellen VMware-Computern mit DPM 2012 R2 |Oracle-Workloads können nicht gesichert werden.|<li>Dateien <li>Ordner<li> Volumes <li>VMs<li> Anwendungen<li> Workloads |<li>Recovery Services-Tresor<li> Lokal angefügter Datenträger<li>  Band (nur lokal) |
| Azure Backup Server |<li>App-fähige Momentaufnahmen (VSS)<li>Vollständige Flexibilität in Bezug auf Sicherungszeitpunkt<li>Wiederherstellungsgranularität (alle)<li>Kann einen Recovery Services-Tresor verwenden<li>Linux-Unterstützung für Hyper-V- und VMware-VMs<li>Sichern und Wiederherstellen von virtuellen VMware-Computern <li>Keine System Center-Lizenz erforderlich |<li>Oracle-Workloads können nicht gesichert werden.<li>Aktives Azure-Abonnement immer erforderlich<li>Keine Unterstützung der Bandsicherung |<li>Dateien <li>Ordner<li> Volumes <li>VMs<li> Anwendungen<li> Workloads |<li>Recovery Services-Tresor<li> Lokal angefügter Datenträger |
| Azure IaaS-VM-Sicherung |<li>Native Sicherungen für Windows/Linux<li>Keine bestimmte Agent-Installation erforderlich<li>Sicherung auf Fabric-Ebene ohne Sicherungsinfrastruktur |<li>Tägliche Sicherung von virtuellen Computern <li>Wiederherstellung von virtuellen Computern nur auf Datenträgerebene<li>Keine lokale Sicherung möglich |<li>VMs <li>Alle Datenträger (mit PowerShell) |<p>Recovery Services-Tresor</p> |

## <a name="what-are-the-deployment-scenarios-for-each-component"></a>Was sind die Bereitstellungsszenarien für jede Komponente?
| Komponente | Bereitstellung in Azure möglich? | Lokale Bereitstellung möglich? | Unterstützter Zielspeicher |
| --- | --- | --- | --- |
| Azure Backup-Agent (MARS) |<p>**Ja**</p> <p>Der Azure Backup-Agent kann auf allen virtuellen Windows Server-Computern bereitgestellt werden, die in Azure ausgeführt werden.</p> |<p>**Ja**</p> <p>Der Azure Backup-Agent kann auf allen virtuellen Windows Server-Computern oder physischen Computern bereitgestellt werden.</p> |<p>Recovery Services-Tresor</p> |
| System Center DPM |<p>**Ja**</p><p>Erfahren Sie mehr über den [Schutz von Workloads in Azure mithilfe von System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Ja**</p> <p>Erfahren Sie mehr zum [Schutz von Workloads und virtuellen Computern im Datencenter](https://technet.microsoft.com/system-center-docs/dpm/data-protection-manager).</p> |<p>Lokal angefügter Datenträger</p> <p>Recovery Services-Tresor</p> <p>Band (nur lokal)</p> |
| Azure Backup Server |<p>**Ja**</p><p>Erfahren Sie mehr zum [Schutz von Workloads in Azure mithilfe von Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>**Ja**</p> <p>Erfahren Sie mehr zum [Schutz von Workloads in Azure mithilfe von Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>Lokal angefügter Datenträger</p> <p>Recovery Services-Tresor</p> |
| Azure IaaS-VM-Sicherung |<p>**Ja**</p><p>Teil der Azure-Fabric</p><p>Speziell für die [Sicherung von Azure IaaS-VMs (Infrastructure as a Service)](backup-azure-vms-introduction.md).</p> |<p>**Nein**</p> <p>Sichern Sie virtuelle Computer in Ihrem Datencenter mit System Center DPM.</p> |<p>Recovery Services-Tresor</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Welche Anwendungen und Workloads kann ich sichern?
Die folgende Tabelle enthält eine Matrix mit den Daten und Workloads, die mit Azure Backup geschützt werden können. Die Spalte „Azure Backup-Lösung“ enthält Links zur Bereitstellungsdokumentation für die Lösung. Jede Azure Backup-Komponente kann in einer Umgebung mit einem klassischen Bereitstellungsmodell (Service Manager-Bereitstellung) oder Resource Manager-Bereitstellungsmodell bereitgestellt werden.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

| Daten oder Workload | Quellumgebung | Azure Backup-Lösung |
| --- | --- | --- |
| Dateien und Ordner |Windows Server |<p>[Azure Backup-Agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ Azure Backup-Agent),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (enthält den Azure Backup-Agent)</p> |
| Dateien und Ordner |Windows-Computer |<p>[Azure Backup-Agent](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ Azure Backup-Agent),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (enthält den Azure Backup-Agent)</p> |
| Virtueller Hyper-V-Computer (Windows) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-Agent),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (enthält den Azure Backup-Agent)</p> |
| Virtueller Hyper-V-Computer (Linux) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-Agent),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (enthält den Azure Backup-Agent)</p> |
| Microsoft SQL Server |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-Agent),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (enthält den Azure Backup-Agent)</p> |
| Microsoft SharePoint |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-Agent),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (enthält den Azure Backup-Agent)</p> |
| Microsoft Exchange |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ Azure Backup-Agent),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (enthält den Azure Backup-Agent)</p> |
| Azure IaaS-VMs (Windows) |Ausführung in Azure |[Azure Backup (VM-Erweiterung)](backup-azure-vms-introduction.md) |
| Azure IaaS-VMs (Linux) |Ausführung in Azure |[Azure Backup (VM-Erweiterung)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Linux-Unterstützung
In der folgende Tabellen sind die Azure Backup-Komponenten angegeben, die über Unterstützung für Linux verfügen.  

| Komponente | Linux-Unterstützung (von Azure unterstützt) |
| --- | --- |
| Azure Backup-Agent (MARS) |Keine (nur Windows-basierter Agent) |
| System Center DPM |<li> Dateikonsistente Sicherung von Linux-Gast-VMs unter Hyper-V und VMWare<br/> <li> VM-Wiederherstellung von Hyper-V- und VMWare-Linux-Gast-VMs </br> </br>  *Dateikonsistente Sicherungen sind für virtuelle Azure-Computer nicht verfügbar.* <br/> |
| Azure Backup Server |<li>Dateikonsistente Sicherung von Linux-Gast-VMs unter Hyper-V und VMWare<br/> <li> VM-Wiederherstellung von Hyper-V- und VMWare-Linux-Gast-VMs </br></br> *Dateikonsistente Sicherungen sind für virtuelle Azure-Computer nicht verfügbar.*  |
| Azure IaaS-VM-Sicherung |Anwendungskonsistente Sicherung per [Pre-Skript- und Post-Skript-Framework](backup-azure-linux-app-consistent.md)<br/> [Präzise Dateiwiederherstellung](backup-azure-restore-files-from-vm.md)<br/> [Wiederherstellen aller VM-Datenträger](backup-azure-arm-restore-vms.md#restore-backed-up-disks)<br/> [VM-Wiederherstellung](backup-azure-arm-restore-vms.md#create-a-new-vm-from-restore-point) |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Verwenden von Storage Premium-VMs mit Azure Backup
Azure Backup schützt Storage Premium-VMs. Azure Storage Premium ist ein SSD-basierter Speicher (Solid State Drive, Festkörperlaufwerk), der auf die Unterstützung E/A-intensiver Workloads ausgelegt ist. Storage Premium ist gut für Workloads von virtuellen Computern (VMs) geeignet. Weitere Informationen zu Storage Premium finden Sie im Artikel [Storage Premium: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../storage/storage-premium-storage.md).

### <a name="back-up-premium-storage-vms"></a>Sichern virtueller Storage Premium-Computer
Beim Sichern virtueller Storage Premium-Computer erstellt der Backup-Dienst einen temporären Stagingspeicherort namens „AzureBackup-“ im Storage Premium-Konto. Die Größe des Stagingspeicherorts entspricht der Größe der Momentaufnahme des Wiederherstellungspunkts. Stellen Sie sicher, dass im Storage Premium-Konto ausreichend freier Speicherplatz für den temporären Stagingspeicherort zur Verfügung steht. Weitere Informationen finden Sie im Artikel zu den [Storage Premium-Einschränkungen](../storage/storage-premium-storage.md#scalability-and-performance-targets). Wenn der Sicherungsauftrag abgeschlossen ist, wird der Stagingspeicherort gelöscht. Der Preis für den Speicher, der für den Stagingspeicherort genutzt wird, entspricht den [Preisen für Storage Premium](../storage/storage-premium-storage.md#pricing-and-billing).

> [!NOTE]
> Ändern oder bearbeiten Sie den Stagingspeicherort nicht.
>
>

### <a name="restore-premium-storage-vms"></a>Wiederherstellen virtueller Storage Premium-Computer
Virtuelle Storage Premium-Computer können entweder unter Storage Premium oder in einem normalen Speicher wiederhergestellt werden. In der Regel wird der Wiederherstellungspunkt eines virtuellen Storage Premium-Computers auf Storage Premium wiederhergestellt. Unter Umständen ist es jedoch kostengünstiger, den Wiederherstellungspunkt eines virtuellen Storage Premium-Computers auf dem Standardspeicher wiederherzustellen. Diese Art der Wiederherstellung ist möglich, wenn Sie einen Teil der Dateien des virtuellen Computers benötigen.

## <a name="using-managed-disk-vms-with-azure-backup"></a>Verwendung von virtuellen Computer auf verwalteten Datenträgern
Azure Backup schützt virtuelle Computer auf verwalteten Datenträgern. Bei verwalteten Datenträgern entfällt die Verwaltung von Speicherkonten für virtuelle Computer, und die Bereitstellung virtueller Computer wird erheblich vereinfacht.

### <a name="back-up-managed-disk-vms"></a>Sichern von virtuellen Computer auf verwalteten Datenträgern
Das Sichern virtueller Computer auf verwalteten Datenträgern funktioniert genauso wie das Sichern virtueller Resource Manager-Computer. Im Azure-Portal können Sie den Sicherungsauftrag direkt in der Ansicht des virtuellen Computers oder in der Ansicht des Recovery Services-Tresors konfigurieren. Virtuelle Computer können auf verwalteten Datenträgern über RestorePoint-Sammlungen gesichert werden, die auf verwalteten Datenträgern aufbauen. Das Sichern von virtuellen Computern auf verwalteten Datenträgern mit Azure Disk Encryption (ADE) wird von Azure Backup ebenfalls unterstützt.

### <a name="restore-managed-disk-vms"></a>Wiederherstellen von virtuellen Computer auf verwalteten Datenträgern
Mit Azure Backup können Sie einen vollständigen virtuellen Computer mit verwalteten Datenträgern oder verwaltete Datenträger in einem Speicherkonto wiederherstellen. Während der Wiederherstellung werden die verwalteten Datenträger von Azure verwaltet. Sie als Kunde verwalten das Speicherkonto, das im Rahmen der Wiederherstellung erstellt wurde. Beim Wiederherstellen verwalteter verschlüsselter VMs müssen die Schlüssel und Geheimnisse des virtuellen Computers vor dem Starten des Wiederherstellungsvorgangs bereits im Schlüsseltresor vorhanden sein.

## <a name="what-are-the-features-of-each-backup-component"></a>Welche Features haben die einzelnen Backup-Komponenten?
Die folgenden Abschnitte enthalten Tabellen, in denen die Verfügbarkeit bzw. die Unterstützung verschiedener Features der einzelnen Azure Backup-Komponenten zusammengefasst ist. Weitere Informationen zur Unterstützung bzw. weitere Details sind jeweils unterhalb der Tabelle zu finden.

### <a name="storage"></a>Speicher
| Feature | Azure Backup-Agent | System Center DPM | Azure Backup Server | Azure IaaS-VM-Sicherung |
| --- | --- | --- | --- | --- |
| Recovery Services-Tresor |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| Datenträgerspeicher | |![Ja][green] |![Ja][green] | |
| Bandspeicher | |![Ja][green] | | |
| Komprimierung <br/>(im Recovery Services-Tresor) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Inkrementelle Sicherung |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| Datenträgerdeduplizierung | |![Teilweise][yellow] |![Teilweise][yellow] | | |

![Tabellenschlüssel](./media/backup-introduction-to-azure-backup/table-key.png)

Der Recovery Services-Tresor ist das bevorzugte Speicherziel aller Komponenten. System Center DPM und Azure Backup Server ermöglichen auch die Verwendung einer lokalen Datenträgerkopie. Nur System Center DPM bietet aber die Möglichkeit, Daten auf ein Bandspeichergerät zu schreiben.

#### <a name="compression"></a>Komprimierung
Sicherungen werden komprimiert, um den erforderlichen Speicherplatz zu reduzieren. Die einzige Komponente, für die keine Komprimierung verwendet wird, ist die VM-Erweiterung. Die VM-Erweiterung kopiert alle Sicherungsdaten aus Ihrem Speicherkonto in den Recovery Services-Tresor in der gleichen Region. Die Daten werden unkomprimiert übertragen. Dadurch erhöht sich geringfügig der verwendete Speicherplatz. Unkomprimiert gespeicherte Daten lassen sich im Bedarfsfall allerdings auch schneller wiederherstellen.


#### <a name="disk-deduplication"></a>Datenträgerdeduplizierung
Sie können die Deduplizierung nutzen, wenn Sie System Center DPM oder Azure Backup Server [auf einem virtuellen Hyper-V-Computer](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx) bereitstellen. Windows Server führt die Datendeduplizierung (auf Hostebene) auf den virtuellen Festplatten (VHDs) durch, die als Sicherungsspeicher an den virtuellen Computer angefügt sind.

> [!NOTE]
> Die Deduplizierung ist in Azure für keine der Backup-Komponenten verfügbar. Wenn System Center DPM und Azure Backup Server in Azure bereitgestellt werden, können an die VM angefügte Speicherdatenträger nicht dedupliziert werden.
>
>

### <a name="incremental-backup-explained"></a>Erläuterung der inkrementellen Sicherung
Die inkrementelle Sicherung wird unabhängig vom Zielspeicher (Datenträger, Band, Recovery Services-Tresor) von allen Azure Backup-Komponenten unterstützt. Mit der inkrementellen Sicherung wird sichergestellt, dass Sicherungen speicher- und zeiteffizient sind, da nur die seit der letzten Sicherung vorgenommenen Änderungen übertragen werden.

#### <a name="comparing-full-differential-and-incremental-backup"></a>Vergleich zwischen vollständiger, differenzieller und inkrementeller Sicherung

Speicherverbrauch, RTO (Recovery Time Objective) und Netzwerkauslastung variieren je nach Art der Sicherungsmethode. Um die Gesamtkosten der Sicherung möglichst niedrig zu halten, müssen Sie verstehen, welche Sicherungslösung am besten für Sie geeignet ist. Die folgende Abbildung zeigt einen Vergleich zwischen vollständiger, differenzieller und inkrementeller Sicherung. In der Abbildung besteht die Datenquelle A aus zehn Speicherblöcken (A1–A10), die monatlich gesichert werden. Die Blöcke A2, A3, A4 und A9 ändern sich im ersten Monat, der Block A5 ändert sich im nächsten Monat.

![Vergleichsdarstellung von Sicherungsmethoden](./media/backup-introduction-to-azure-backup/backup-method-comparison.png)

Bei der **vollständigen Sicherung** enthält jede Sicherungskopie die gesamte Datenquelle. Die vollständige Sicherung beansprucht bei jeder Übertragung einer Sicherungskopie große Mengen an Netzwerkbandbreite und Speicherplatz.

Bei der **differenziellen Sicherung** werden nur die Blöcke gespeichert, die sich seit der ersten vollständigen Sicherung geändert haben. Das führt zu einer geringeren Netzwerk- und Speicherauslastung. Differenzielle Sicherungen enthalten keine redundanten Kopien von Daten, die sich nicht geändert haben. Da jedoch die Datenblöcke, die sich zwischen Folgesicherungen nicht geändert haben, übertragen und gespeichert werden, sind differenzielle Sicherungen ineffizient. Im zweiten Monat werden die geänderten Blöcke A2, A3, A4 und A9 gesichert. Im dritten Monat werden die gleichen Blöcke erneut gesichert – zusammen mit dem geänderten Block A5. Die geänderten Blöcke werden bis zur nächsten vollständigen Sicherung immer wieder gesichert.

Die **inkrementelle Sicherung** bietet eine hohe Speicher- und Netzwerkeffizienz, da hier nur die Datenblöcke gespeichert werden, die sich seit der vorherigen Sicherung geändert haben. Bei der inkrementellen Sicherung müssen nicht regelmäßig vollständige Sicherungen erstellt werden. In dem Beispiel werden nach der vollständigen Sicherung für den ersten Monat die geänderten Blöcke A2, A3, A4 und A9 als geändert gekennzeichnet und für den zweiten Monat übertragen. Im dritten Monat wird nur der geänderte Block A5 gekennzeichnet und übertragen. Hierbei werden weniger Daten bewegt, was sich positiv auf die Auslastung der Speicher- und Netzwerkressourcen auswirkt und zu geringeren Gesamtkosten führt.   

### <a name="security"></a>Sicherheit
| Feature | Azure Backup-Agent | System Center DPM | Azure Backup Server | Azure IaaS-VM-Sicherung |
| --- | --- | --- | --- | --- |
| Netzwerksicherheit<br/> (in Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Teilweise][yellow] |
| Datensicherheit<br/> (in Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Teilweise][yellow] |

![Tabellenschlüssel](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Netzwerksicherheit
Sämtlicher Sicherungsdatenverkehr von Ihren Servern in den Recovery Services-Tresor wird mit AES 256 (Advanced Encryption Standard) verschlüsselt. Die Sicherungsdaten werden über eine sichere HTTPS-Verbindung übertragen. Die Sicherungsdaten werden auch im Recovery Services-Tresor in verschlüsselter Form gespeichert. Nur Sie, der Azure-Kunde, sind im Besitz der Passphrase zum Entsperren dieser Daten. Die Sicherungsdaten können zu keinem Zeitpunkt von Microsoft entschlüsselt werden.

> [!WARNING]
> Nachdem Sie den Recovery Services-Tresor eingerichtet haben, haben nur Sie Zugriff auf den Verschlüsselungsschlüssel. Microsoft bewahrt keine Kopie Ihres Verschlüsselungsschlüssels auf und hat auch keinen Zugriff auf den Schlüssel. Wenn der Schlüssel verlegt wird, kann Microsoft die gesicherten Daten nicht wiederherstellen.
>
>

#### <a name="data-security"></a>Datensicherheit
Für das Sichern virtueller Azure-Computer ist das Einrichten der Verschlüsselung *im* virtuellen Computer erforderlich. Verwenden Sie BitLocker auf virtuellen Windows-Computern und **dm-crypt** auf virtuellen Linux-Computern. Daten, die auf diesem Weg eingehen, werden von Azure Backup nicht automatisch verschlüsselt.

### <a name="network"></a>Netzwerk
| Feature | Azure Backup-Agent | System Center DPM | Azure Backup Server | Azure IaaS-VM-Sicherung |
| --- | --- | --- | --- | --- |
| Netzwerkkomprimierung <br/>(auf **Sicherungsserver**) | |![Ja][green] |![Ja][green] | |
| Netzwerkkomprimierung <br/>(zum **Recovery Services-Tresor**) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Netzwerkprotokoll <br/>(auf **Sicherungsserver**) | |TCP |TCP | |
| Netzwerkprotokoll <br/>(zum **Recovery Services-Tresor**) |HTTPS |HTTPS |HTTPS |HTTPS |

![Tabellenschlüssel](./media/backup-introduction-to-azure-backup/table-key-2.png)

Die VM-Erweiterung (auf der IaaS-VM) liest die Daten über das Speichernetzwerk direkt aus dem Azure-Speicherkonto, sodass es nicht erforderlich ist, diesen Datenverkehr zu komprimieren.

Wenn Sie einen System Center DPM-Server oder eine Azure Backup Server-Instanz als sekundären Sicherungsserver verwenden, empfiehlt es sich, die Daten zu komprimieren, die vom primären Server an den Sicherungsserver gesendet werden. Wenn die Daten vor dem Sichern in DPM oder Azure Backup Server komprimiert werden, wird weniger Bandbreite beansprucht.

#### <a name="network-throttling"></a>Netzwerkdrosselung
Der Azure Backup-Agent verfügt über die Netzwerkdrosselung, mit der Sie steuern können, wie die Netzwerkbandbreite während der Datenübertragung verwendet wird. Die Drosselung kann hilfreich sein, wenn Sie Daten während der Geschäftszeiten sichern möchten, der Sicherungsprozess aber keine Auswirkung auf den anderen Internetdatenverkehr haben soll. Die Drosselung der Datenübertragung gilt für Sicherungs- und Wiederherstellungsaktivitäten.

## <a name="backup-and-retention"></a>Sicherung und Aufbewahrung

Für Azure Backup gilt pro *geschützter Instanz* eine Obergrenze von 9999 Wiederherstellungspunkten (auch Sicherungskopien oder Momentaufnahmen genannt). Geschützte Instanzen sind Computer, Server (physisch oder virtuell) oder Workloads, die Daten in Azure sichern. Weitere Informationen finden Sie im Abschnitt [Was ist eine geschützte Instanz?](backup-introduction-to-azure-backup.md#what-is-a-protected-instance). Eine Instanz ist geschützt, sobald eine Sicherungskopie der Daten gespeichert wurde. Die Sicherungskopie der Daten ist der Schutz. Wenn die Quelldaten verloren gehen oder beschädigt werden, können sie mithilfe der Sicherungskopie wiederhergestellt werden. In der folgenden Tabelle ist die maximale Sicherungshäufigkeit für die einzelnen Komponenten angegeben. Ihre Konfiguration der Sicherungsrichtlinie bestimmt, wie schnell die Wiederherstellungspunkte verbraucht werden. Wenn Sie beispielsweise jeden Tag einen Wiederherstellungspunkt erstellen, können Sie Wiederherstellungspunkte 27 Jahre lang nutzen, bevor der Vorrat erschöpft ist. Wenn Sie einen monatlichen Wiederherstellungspunkt verwenden, reicht der Vorrat für 833 Jahre. Der Backup-Dienst legt keine Ablaufzeitgrenze für einen Wiederherstellungspunkt fest.

|  | Azure Backup-Agent | System Center DPM | Azure Backup Server | Azure IaaS-VM-Sicherung |
| --- | --- | --- | --- | --- |
| Sicherungshäufigkeit<br/> (zum Recovery Services-Tresor) |Drei Sicherungen pro Tag |Zwei Sicherungen pro Tag |Zwei Sicherungen pro Tag |Eine Sicherung pro Tag |
| Sicherungshäufigkeit<br/> (auf Datenträger) |Nicht zutreffend |<li>Alle 15 Minuten für SQL Server <li>Stündlich für andere Workloads |<li>Alle 15 Minuten für SQL Server <li>Stündlich für andere Workloads</p> |Nicht zutreffend |
| Aufbewahrungsoptionen |Täglich, wöchentlich, monatlich, jährlich |Täglich, wöchentlich, monatlich, jährlich |Täglich, wöchentlich, monatlich, jährlich |Täglich, wöchentlich, monatlich, jährlich |
| Maximale Wiederherstellungspunkte pro geschützter Instanz |9999|9999|9999|9999|
| Maximale Aufbewahrungsdauer |Abhängig von der Sicherungshäufigkeit |Abhängig von der Sicherungshäufigkeit |Abhängig von der Sicherungshäufigkeit |Abhängig von der Sicherungshäufigkeit |
| Wiederherstellungspunkte auf lokalem Datenträger |Nicht zutreffend |<li>64 für Dateiserver,<li>448 für Anwendungsserver |<li>64 für Dateiserver,<li>448 für Anwendungsserver |Nicht zutreffend |
| Wiederherstellungspunkte auf Band |Nicht zutreffend |Unbegrenzt |Nicht zutreffend |Nicht zutreffend |

## <a name="what-is-a-protected-instance"></a>Was ist eine geschützte Instanz?
Eine geschützte Instanz ist ein generischer Verweis auf einen Windows-Computer, einen (physischen oder virtuellen) Server oder auf eine SQL-Datenbank, der bzw. die Daten in Azure sichert. Eine Instanz ist geschützt, sobald Sie eine Sicherungsrichtlinie für den Computer, den Server oder die Datenbank konfigurieren und eine Sicherungskopie der Daten erstellen. Durch nachfolgende Kopien der Sicherungsdaten für die geschützte Instanz (so genannte Wiederherstellungspunkte) erhöht sich der beanspruchte Speicherplatz. Für eine geschützte Instanz können bis zu 9999 Wiederherstellungspunkte erstellt werden. Aus dem Speicher gelöschte Wiederherstellungspunkte werden nicht in die Gesamtanzahl von 9999 Wiederherstellungspunkten einbezogen.
Allgemeine Beispiele für geschützte Instanzen sind virtuelle Computer, Anwendungsserver, Datenbanken und PCs unter Windows. Beispiel:

* Ein virtueller Computer, auf dem die Hyper-V- oder die Azure IaaS-Hypervisor-Fabric ausgeführt wird. Mögliche Gastbetriebssysteme für den virtuellen Computer sind Windows Server und Linux.
* Ein Anwendungsserver: Dabei kann es sich um einen physischen oder virtuellen Computer handeln, auf dem Windows Server und Workloads mit zu sichernden Daten ausgeführt werden. Gängige Workloads sind Microsoft SQL Server, Microsoft Exchange Server, Microsoft SharePoint Server und die Dateiserverrolle unter Windows Server. Zum Sichern dieser Workloads benötigen Sie System Center Data Protection Manager (DPM) oder Azure Backup Server.
* Ein PC, eine Arbeitsstation oder ein Laptop unter dem Windows-Betriebssystem.


## <a name="what-is-a-recovery-services-vault"></a>Was ist ein Recovery Services-Tresor?
Ein Recovery Services-Tresor ist eine Onlinespeicherentität in Azure, die zum Speichern von Daten wie Sicherungskopien, Wiederherstellungspunkten und Sicherungsrichtlinien verwendet wird. Sie können Recovery Services-Tresore zum Speichern von Sicherungsdaten für Azure-Dienste sowie lokale Server und Arbeitsstationen verwenden. Recovery Services-Tresore vereinfachen die Organisation Ihrer Sicherungsdaten und minimieren gleichzeitig den Verwaltungsaufwand. Sie können innerhalb eine Abonnements beliebig viele Recovery Services-Tresore erstellen.

Sicherungstresore, die auf Azure Service Manager basieren, waren die erste Version eines Tresors. Recovery Services-Tresore, die um die Azure Resource Manager-Modellfunktionen ergänzt wurden, sind die zweite Version. Eine vollständige Beschreibung der Funktionsunterschiede finden Sie im [Übersichtsartikel zu Recovery Services-Tresoren](backup-azure-recovery-services-vault-overview.md). Sicherungstresors werden weiterhin unterstützt, Sie können aber keine neuen Sicherungstresore im Azure-Portal mehr erstellen.

> [!IMPORTANT]
> Sie können nun ein Upgrade für Ihre Sicherungstresore auf Recovery Services-Tresore durchführen. Detaillierte Informationen finden Sie im Artikel [Durchführen eines Upgrades für einen Sicherungstresor auf einen Recovery Services-Tresor](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft empfiehlt, ein Upgrade für Ihre Sicherungstresore auf Recovery Services-Tresore durchzuführen.<br/> **Ab dem 1. November 2017**:
>- Für alle verbleibenden Sicherungstresore wird automatisch ein Upgrade auf Recovery Services-Tresore durchgeführt.
>- Der Zugriff auf Ihre Sicherungsdaten im klassischen Portal wird nicht möglich sein. Verwenden Sie stattdessen das Azure-Portal, um auf Ihre Sicherungsdaten in Recovery Services-Tresoren zuzugreifen.
>

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Wie unterscheidet sich Azure Backup von Azure Site Recovery?
Azure Backup und Azure Site Recovery ähneln sich insofern als mit beiden Diensten Daten gesichert und dann wiederhergestellt werden können. Beide Dienste bieten aber unterschiedliche Funktionen hinsichtlich der Geschäftskontinuität und Notfallwiederherstellung in Ihrem Unternehmen. Verwenden Sie Azure Backup, um Daten auf differenzierte Weise zu schützen und wiederherzustellen. Wenn beispielsweise die Darstellung eines Laptops beschädigt wurde, verwenden Sie Azure Backup, um die Darstellung wiederherzustellen. Wenn Sie die Konfiguration und die Daten eines virtuellen Computers in einem anderen Rechenzentrum replizieren möchten, verwenden Sie Azure Site Recovery.

Mit Azure Backup werden Daten lokal und in der Cloud geschützt. Azure Site Recovery koordiniert Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server. Beide Dienste sind wichtig, da die Daten im Rahmen Ihrer Notfallwiederherstellungsstrategie sicher und wiederherstellbar gespeichert werden müssen (Backup) *und* Ihre Workloads verfügbar sein müssen (Site Recovery), falls es zu Ausfällen kommen sollte.

Die folgenden Konzepte können Ihnen als Unterstützung beim Treffen wichtiger Entscheidungen in Bezug auf die Sicherung und Notfallwiederherstellung dienen.

| Konzept | Details | Sicherung | Notfallwiederherstellung |
| --- | --- | --- | --- |
| Recovery Point Objective (RPO) |Dies ist der Umfang des zulässigen Datenverlusts, falls eine Wiederherstellung durchgeführt werden muss. |Sicherungslösungen weisen eine große Varianz beim akzeptablen RPO auf. Sicherungen virtueller Computer haben zumeist ein RPO von einem Tag, während Datenbanksicherungskopien RPOs von bis zu 15 Minuten aufweisen. |Lösungen für die Notfallwiederherstellung haben niedrige RPO-Werte. Die Kopie für die Notfallwiederherstellung kann einige Sekunden bzw. einige Minuten hinterherhinken. |
| Recovery Time Objective (RTO) |Der Zeitraum, der für eine Wiederherstellung erforderlich ist. |Aufgrund des höheren RPO-Werts ist die Datenmenge, die eine Sicherungslösung verarbeiten muss, meist wesentlich größer, was zu längeren RTOs führt. Beispielsweise kann das Wiederherstellen von Daten von Bändern Tage dauern, was davon abhängt, wie lange der Transport des Bands von einem standortexternen Aufbewahrungsort dauert. |Lösungen für die Notfallwiederherstellung weisen kleinere RTOs auf, da sie synchroner mit der Quelle sind. Weniger Änderungen müssen verarbeitet werden. |
| Aufbewahrung |Die Dauer der Datenspeicherung. |Für Szenarien, für die eine operative Wiederherstellung (Beschädigung von Daten, versehentliches Löschen von Dateien, Ausfall des Betriebssystems) erforderlich ist, werden die Sicherungsdaten normalerweise maximal 30 Tage aufbewahrt.<br>Aus Gründen der Compliance kann es sein, dass Daten mehrere Monate oder sogar Jahre gespeichert werden müssen. In solchen Fällen sind Sicherungsdaten ideal für die Archivierung geeignet. |Für die Notfallwiederherstellung werden nur betriebliche Wiederherstellungsdaten benötigt. Dies dauert normalerweise einige Stunden oder bis zu einem Tag. Aufgrund der differenzierten Datensammlung in Lösungen für die Notfallwiederherstellung empfehlen sich Notfallwiederherstellungsdaten nicht für eine langfristige Aufbewahrung. |

## <a name="next-steps"></a>Nächste Schritte
Verwenden Sie eines der folgenden Tutorials mit ausführlichen Schritt-für-Schritt-Anleitungen zum Schützen von Daten unter Windows Server bzw. Schützen eines virtuellen Computers (VM) in Azure:

* [Sichern von Dateien und Ordnern](backup-try-azure-backup-in-10-mins.md)
* [Sichern von virtuellen Azure-Computern](backup-azure-vms-first-look-arm.md)

Weitere Informationen zum Schützen anderer Workloads finden Sie in diesen Artikeln:

* [Sichern eines Windows-Servers oder -Clients in Azure unter Verwendung des Resource Manager-Bereitstellungsmodells](backup-configure-vault.md)
* [Sichern von Anwendungsworkloads](backup-azure-microsoft-azure-backup.md)
* [Sichern von Azure IaaS-VMs](backup-azure-vms-prepare.md)

[green]: ./media/backup-introduction-to-azure-backup/green.png
[yellow]: ./media/backup-introduction-to-azure-backup/yellow.png
[red]: ./media/backup-introduction-to-azure-backup/red.png

