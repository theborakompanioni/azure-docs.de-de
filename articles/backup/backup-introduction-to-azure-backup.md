---
title: Was ist Azure Backup? | Microsoft Docs
description: "Mithilfe von Azure Backup und Recovery Services können Sie Daten und Anwendungen von Windows-Servern, Windows-Clientcomputern, System Center DPM-Servern oder virtuellen Azure-VMs sichern und wiederherstellen."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: tysonn
keywords: "Sichern und Wiederherstellen; Wiederherstellungsdienste; Sicherungslösungen"
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2016
ms.author: jimpark; trinadhk
translationtype: Human Translation
ms.sourcegitcommit: cf3930f209e84ee9b14b56566ca19d31382946aa
ms.openlocfilehash: cefb405b4f30ca5fe20f6acfaee5ebba2690990b


---
# <a name="what-is-azure-backup"></a>Was ist Azure Backup?
Azure Backup ist der Azure-basierte Dienst, den Sie zum Sichern (bzw. Schützen) und Wiederherstellen Ihrer Daten in der Microsoft Cloud verwenden können. Azure Backup ersetzt Ihre vorhandene lokale bzw. standortexterne Lösung durch eine zuverlässige, sichere und wirtschaftliche Cloudlösung. Azure Backup verfügt über mehrere Komponenten, die Sie herunterladen und auf dem jeweiligen Computer, Server oder in der Cloud bereitstellen. Die Komponente (der Agent), die Sie bereitstellen, richtet sich danach, was geschützt werden soll. Alle Azure Backup-Komponenten (unabhängig davon, ob Daten lokal oder in der Cloud geschützt werden sollen) können genutzt werden, um Daten in einem Backup-Tresor in Azure zu sichern. Informationen dazu, welche Komponente zum Schützen bestimmter Daten, Anwendungen oder Workloads geeignet ist, finden Sie in der [Tabelle mit den Azure Backup-Komponenten](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (weiter unten in diesem Artikel).

[Video mit einem Überblick über Azure Backup](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Gründe für Azure Backup
Herkömmliche Sicherungslösungen haben sich dahingehend entwickelt, dass die Cloud ähnlich wie bei einer Festplatte oder einem Band als Endpunkt bzw. statisches Speicherziel behandelt wird. Dieser Ansatz ist zwar einfach, aber er unterliegt auch Beschränkungen, und es werden nicht alle Vorteile einer zugrunde liegenden Cloudplattform genutzt. Dies führt zu einer teuren und ineffizienten Lösung. Andere Lösungen sind kostenintensiv, da Sie für die falsche Art von Speicher oder für nicht benötigten Speicher zahlen. Andere Lösungen sind häufig ineffizient, da Sie nicht die Art oder die Menge an benötigtem Speicher erhalten oder weil zu viel Zeit für Verwaltungsaufgaben aufgewendet werden muss. Im Gegensatz dazu bietet Azure Backup diese wichtigen Vorteile:

**Automatische Speicherverwaltung**: Für Hybridumgebungen ist häufig heterogener Speicher erforderlich – teilweise lokal und teilweise in der Cloud. Bei Azure Backup fallen keine Kosten für die Verwendung von lokalen Speichergeräten an. Azure Backup sorgt im Rahmen eines Modells mit nutzungsbasierter Bezahlung für die automatische Zuteilung und Verwaltung von Sicherungsspeicher. Nutzungsbasierte Bezahlung bedeutet, dass Sie nur für den Speicher zahlen, den Sie verbrauchen. Weitere Informationen finden Sie im [Artikel zu den Preisen von Azure](https://azure.microsoft.com/pricing/details/backup).

**Unbegrenzte Skalierung**: Für Azure Backup wird die Leistungsstärke und unbegrenzte Skalierung der Azure-Cloud genutzt, um eine hohe Verfügbarkeit sicherzustellen – ohne Wartungs- oder Überwachungsaufwand. Sie können Warnungen einrichten, um Informationen zu Ereignissen bereitzustellen, aber Sie müssen sich nicht um die hohe Verfügbarkeit für Ihre Daten in der Cloud kümmern.

**Mehrere Speicheroptionen**: Ein Aspekt der hohen Verfügbarkeit ist die Speicherreplikation. Azure Backup bietet zwei Arten der Replikation: mit [lokal redundantem Speicher](../storage/storage-redundancy.md#locally-redundant-storage) und mit [georepliziertem Speicher](../storage/storage-redundancy.md#geo-redundant-storage). Wählen Sie die Sicherungsspeicheroption aus, die zu Ihren Anforderungen passt:

* Lokal redundanter Speicher (Locally Redundant Storage, LRS) repliziert Ihre Daten dreimal in einem gekoppelten Datencenter in der gleichen Region. (Es werden also drei Kopien Ihrer Daten erstellt.) LRS ist eine kostengünstige Option und ideal für preisbewusste Kunden geeignet, da Daten vor lokalen Hardwareausfällen geschützt werden.
* Beim Speicher mit geografischer Replikation (GRS) werden Ihre Daten in eine sekundäre Region repliziert (Hunderte von Kilometern entfernt vom primären Standort der Quelldaten). GRS führt zu höheren Kosten als bei LRS, bietet aber eine höhere Dauerhaftigkeit für Ihre Daten, und zwar auch im Falle eines regionalen Ausfalls.

**Unbegrenzte Datenübertragungen**: Bei Azure Backup ist die Menge der übertragenen eingehenden und ausgehenden Daten nicht beschränkt. Außerdem fallen bei Azure Backup keine Gebühren für die übertragenen Daten an. Aber wenn Sie den Azure Import/Export-Dienst nutzen, um große Datenmengen zu importieren, werden für eingehende Daten Kosten berechnet. Weitere Informationen zu diesen Kosten finden Sie unter [Workflow zur Offlinesicherung in Azure Backup](backup-azure-backup-import-export.md). Ausgehende Daten sind Daten, die während eines Wiederherstellungsvorgangs aus einem Backup-Tresor übertragen wurden.

**Datenverschlüsselung**: Die Datenverschlüsselung ermöglicht eine sichere Übertragung und Speicherung Ihrer Daten in der öffentlichen Cloud. Sie speichern die Passphrase für die Verschlüsselung lokal, und sie wird niemals in Azure übertragen oder gespeichert. Wenn Daten wiederhergestellt werden sollen, sind nur Sie im Besitz der Passphrase für die Verschlüsselung bzw. des Schlüssels.

**Anwendungskonsistente Sicherungen**: Unabhängig davon, ob Sie einen Dateiserver, einen virtuellen Computer oder eine SQL-Datenbank sichern, müssen Sie wissen, dass ein Wiederherstellungspunkt über alle erforderlichen Daten zum Wiederherstellen der Sicherungskopie verfügt. Azure Backup umfasst anwendungskonsistente Sicherungen, sodass sichergestellt ist, dass zum Wiederherstellen der Daten keine zusätzlichen Fixes benötigt werden. Durch die Wiederherstellung von anwendungskonsistenten Daten wird die Wiederherstellungsdauer reduziert, sodass Sie schnell zum Zustand der normalen Ausführung zurückkehren können.

**Langfristige Aufbewahrung**: In Azure können Sie Daten für eine Dauer von bis zu 99 Jahren sichern. Anstatt Sicherungskopien vom Datenträger auf Band zu verschieben und das Band dann zur langfristigen Speicherung an einen anderen Standort zu bringen, können Sie Azure für die kurz- und langfristige Aufbewahrung nutzen.

## <a name="which-azure-backup-components-should-i-use"></a>Welche Azure Backup-Komponenten sollte ich verwenden?
Falls Sie unsicher sind, welche Azure Backup-Komponente für Ihre Anforderungen geeignet ist, helfen Ihnen die Informationen in der folgenden Tabelle weiter. Es ist jeweils angegeben, was Sie mit einer Komponente schützen können. Das Azure-Portal enthält einen Assistenten, der in das Portal integriert ist und Sie bei der Auswahl der Komponente unterstützt, die heruntergeladen und bereitgestellt werden soll. Mit dem Assistenten, der Teil der Erstellung des Recovery Services-Tresors ist, werden Sie schrittweise durch die Auswahl eines Sicherungsziels geführt, und Sie können die zu schützenden Daten oder die Anwendung auswählen.

| Komponente | Vorteile | Einschränkungen | Was wird geschützt? | Wo werden Sicherungen gespeichert? |
| --- | --- | --- | --- | --- |
| Azure Backup-Agent (MARS) |<li>Sicherung von Dateien und Ordnern auf physischen oder virtuellen Windows-Computern (virtuelle Computer können sich am lokalen Standort oder in Azure befinden)<li>Kein separater Sicherungsserver erforderlich |<li>Drei Sicherungen pro Tag <li>Nicht anwendungsorientiert, nur Wiederherstellung auf Datei-, Ordner- und Volumeebene <li>  Keine Unterstützung für Linux |<li>Dateien <li>Ordner |Azure Backup-Tresor |
| System Center DPM |<li>App-fähige Momentaufnahmen (VSS)<li>Vollständige Flexibilität in Bezug auf Sicherungszeitpunkt<li>Wiederherstellungsgranularität (alle)<li>Verwendung des Azure Backup-Tresors möglich<li>Linux-Unterstützung (bei Hosting unter Hyper-V) <li>Schutz von virtuellen VMware-Computern mit DPM 2012 R2 |Die Sicherung von Oracle-Workloads wird nicht unterstützt. |<li>Dateien <li>Ordner<li> Volumes <li>VMs<li> Anwendungen<li> Workloads |<li>Azure Backup-Tresor<li> Lokal angefügter Datenträger<li>  Band (nur lokal) |
| Azure Backup Server |<li>App-fähige Momentaufnahmen (VSS)<li>Vollständige Flexibilität in Bezug auf Sicherungszeitpunkt<li>Wiederherstellungsgranularität (alle)<li>Verwendung des Azure Backup-Tresors möglich<li>Linux-Unterstützung (bei Hosting unter Hyper-V)<li>Keine System Center-Lizenz erforderlich |<li>Fehlende heterogene Unterstützung (VMware-VM-Sicherung, Oracle-Workloadsicherung)<li>Aktives Azure-Abonnement immer erforderlich<li>Keine Unterstützung der Bandsicherung |<li>Dateien <li>Ordner<li> Volumes <li>VMs<li> Anwendungen<li> Workloads |<li>Azure Backup-Tresor<li> Lokal angefügter Datenträger |
| Azure IaaS-VM-Sicherung |<li>Native Sicherungen für Windows/Linux<li>Keine bestimmte Agent-Installation erforderlich<li>Sicherung auf Fabric-Ebene ohne Sicherungsinfrastruktur |<li>Tägliche Sicherung von virtuellen Computern <li>Wiederherstellung von virtuellen Computern nur auf Datenträgerebene<li>Keine lokale Sicherung möglich |<li>VMs <li>Alle Datenträger (mit PowerShell) |<p>Azure Backup-Tresor</p> |

## <a name="what-are-the-deployment-scenarios-for-each-component"></a>Was sind die Bereitstellungsszenarien für jede Komponente?
| Komponente | Bereitstellung in Azure möglich? | Lokale Bereitstellung möglich? | Unterstützter Zielspeicher |
| --- | --- | --- | --- |
| Azure Backup-Agent (MARS) |<p>**Ja**</p> <p>Der Azure Backup-Agent kann auf allen virtuellen Windows Server-Computern bereitgestellt werden, die in Azure ausgeführt werden.</p> |<p>**Ja**</p> <p>Der Azure Backup-Agent kann auf allen virtuellen Windows Server-Computern oder physischen Computern bereitgestellt werden.</p> |<p>Azure Backup-Tresor</p> |
| System Center DPM |<p>**Ja**</p><p>Erfahren Sie mehr über den [Schutz von Workloads in Azure mithilfe von System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Ja**</p> <p>Erfahren Sie mehr zum [Schutz von Workloads und virtuellen Computern im Datencenter](https://technet.microsoft.com/en-us/system-center-docs/dpm/data-protection-manager).</p> |<p>Lokal angefügter Datenträger</p> <p>Azure Backup-Tresor</p> <p>Band (nur lokal)</p> |
| Azure Backup Server |<p>**Ja**</p><p>Erfahren Sie mehr zum [Schutz von Workloads in Azure mithilfe von Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>**Ja**</p> <p>Erfahren Sie mehr zum [Schutz von Workloads in Azure mithilfe von Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>Lokal angefügter Datenträger</p> <p>Azure Backup-Tresor</p> |
| Azure IaaS-VM-Sicherung |<p>**Ja**</p><p>Teil der Azure-Fabric</p><p>Speziell für die [Sicherung von Azure IaaS-VMs (Infrastructure as a Service)](backup-azure-vms-introduction.md).</p> |<p>**Nein**</p> <p>Sichern Sie virtuelle Computer in Ihrem Datencenter mit System Center DPM.</p> |<p>Azure Backup-Tresor</p> |

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
| System Center DPM |Nur dateikonsistente Sicherungen auf Hyper-V<br/> (nicht für Azure-VM verfügbar) |
| Azure Backup Server |Nur dateikonsistente Sicherungen auf Hyper-V<br/> (nicht für Azure-VM verfügbar) |
| Azure IaaS-VM-Sicherung |Ja |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Verwenden von Storage Premium-VMs mit Azure Backup
Azure Backup schützt Storage Premium-VMs. Azure Storage Premium ist ein SSD-basierter Speicher (Solid State Drive, Festkörperlaufwerk), der auf die Unterstützung E/A-intensiver Workloads ausgelegt ist. Storage Premium ist gut für Workloads von virtuellen Computern (VMs) geeignet. Weitere Informationen zu Storage Premium finden Sie im Artikel [Premium-Speicher: Hochleistungsspeicher für Workloads auf virtuellen Azure-Computern](../storage/storage-premium-storage.md).

### <a name="back-up-premium-storage-vms"></a>Sichern virtueller Storage Premium-Computer
Beim Sichern virtueller Storage Premium-Computer erstellt der Backup-Dienst einen temporären Stagingspeicherort im Storage Premium-Konto. Der Stagingspeicherort namens „AzureBackup-“ entspricht der Gesamtdatengröße der Premium-Datenträger, die an den virtuellen Computer angefügt sind.

> [!NOTE]
> Ändern oder bearbeiten Sie den Stagingspeicherort nicht.
>
>

Wenn der Sicherungsauftrag abgeschlossen ist, wird der Stagingspeicherort gelöscht. Der Preis für den Speicher, der für den Stagingspeicherort genutzt wird, entspricht den [Preisen für Storage Premium](../storage/storage-premium-storage.md#pricing-and-billing).

### <a name="restore-premium-storage-vms"></a>Wiederherstellen virtueller Storage Premium-Computer
Virtuelle Storage Premium-Computer können entweder unter Storage Premium oder in einem normalen Speicher wiederhergestellt werden. In der Regel wird der Wiederherstellungspunkt eines virtuellen Storage Premium-Computers auf Storage Premium wiederhergestellt. Unter Umständen ist es jedoch kostengünstiger, den Wiederherstellungspunkt eines virtuellen Storage Premium-Computers auf dem Standardspeicher wiederherzustellen. Diese Art der Wiederherstellung ist möglich, wenn Sie einen Teil der Dateien des virtuellen Computers benötigen.

## <a name="what-are-the-features-of-each-backup-component"></a>Welche Features haben die einzelnen Backup-Komponenten?
Die folgenden Abschnitte enthalten Tabellen, in denen die Verfügbarkeit bzw. die Unterstützung verschiedener Features der einzelnen Azure Backup-Komponenten zusammengefasst ist. Weitere Informationen zur Unterstützung bzw. weitere Details sind jeweils unterhalb der Tabelle zu finden.

### <a name="storage"></a>Speicher
| Feature | Azure Backup-Agent | System Center DPM | Azure Backup Server | Azure IaaS-VM-Sicherung |
| --- | --- | --- | --- | --- |
| Azure Backup-Tresor |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| Datenträgerspeicher | |![Ja][green] |![Ja][green] | |
| Bandspeicher | |![Ja][green] | | |
| Komprimierung <br/>(im Backup-Tresor) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Inkrementelle Sicherung |![Ja][green] |![Ja][green] |![Ja][green] |![Ja][green] |
| Datenträgerdeduplizierung | |![Teilweise][yellow] |![Teilweise][yellow] | |

![Tabellenschlüssel](./media/backup-introduction-to-azure-backup/table-key.png)

Der Azure Backup-Tresor ist das bevorzugte Speicherziel aller Komponenten. System Center DPM und Azure Backup Server ermöglichen auch die Verwendung einer lokalen Datenträgerkopie. Nur System Center DPM bietet aber die Möglichkeit, Daten auf ein Bandspeichergerät zu schreiben.

#### <a name="compression"></a>Komprimierung
Sicherungen werden komprimiert, um den erforderlichen Speicherplatz zu reduzieren. Die einzige Komponente, für die keine Komprimierung verwendet wird, ist die VM-Erweiterung. Die VM-Erweiterung kopiert alle Sicherungsdaten aus Ihrem Speicherkonto in den Sicherungstresor in der gleichen Region. Die Daten werden unkomprimiert übertragen. Dadurch erhöht sich geringfügig der verwendete Speicherplatz. Unkomprimiert gespeicherte Daten lassen sich im Bedarfsfall allerdings auch schneller wiederherstellen.

#### <a name="incremental-backup"></a>Inkrementelle Sicherung
Unabhängig vom Zielspeicher (Datenträger, Band, Sicherungstresor) unterstützen alle Komponenten inkrementelle Sicherungen. Mit der inkrementellen Sicherung wird sichergestellt, dass Sicherungen speicher- und zeiteffizient sind, da nur die seit der letzten Sicherung vorgenommenen Änderungen übertragen werden.

#### <a name="disk-deduplication"></a>Datenträgerdeduplizierung
Sie können die Deduplizierung nutzen, wenn Sie System Center DPM oder Azure Backup Server [auf einem virtuellen Hyper-V-Computer](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx) bereitstellen. Windows Server führt die Datendeduplizierung (auf Hostebene) auf den virtuellen Festplatten (VHDs) durch, die als Sicherungsspeicher an den virtuellen Computer angefügt sind.

> [!NOTE]
> Die Deduplizierung ist in Azure für keine der Backup-Komponenten verfügbar. Wenn System Center DPM und Azure Backup Server in Azure bereitgestellt werden, können an die VM angefügte Speicherdatenträger nicht dedupliziert werden.
>
>

### <a name="security"></a>Sicherheit
| Feature | Azure Backup-Agent | System Center DPM | Azure Backup Server | Azure IaaS-VM-Sicherung |
| --- | --- | --- | --- | --- |
| Netzwerksicherheit<br/> (in Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Teilweise][yellow] |
| Datensicherheit<br/> (in Azure) |![Ja][green] |![Ja][green] |![Ja][green] |![Teilweise][yellow] |

![Tabellenschlüssel](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Netzwerksicherheit
Sämtlicher Sicherungsdatenverkehr von Ihren Servern in den Azure Backup-Tresor wird mit AES 256 (Advanced Encryption Standard) verschlüsselt. Die Sicherungsdaten werden über eine sichere HTTPS-Verbindung übertragen. Die Sicherungsdaten werden auch im Sicherungstresor in verschlüsselter Form gespeichert. Nur Sie, der Azure-Kunde, sind im Besitz der Passphrase zum Entsperren dieser Daten. Die Sicherungsdaten können zu keinem Zeitpunkt von Microsoft entschlüsselt werden.

> [!WARNING]
> Nachdem Sie den Backup-Tresor eingerichtet haben, haben nur Sie Zugriff auf den Verschlüsselungsschlüssel. Microsoft bewahrt keine Kopie Ihres Verschlüsselungsschlüssels auf und hat auch keinen Zugriff auf den Schlüssel. Wenn der Schlüssel verlegt wird, kann Microsoft die gesicherten Daten nicht wiederherstellen.
>
>

#### <a name="data-security"></a>Datensicherheit
Für das Sichern virtueller Azure-Computer ist das Einrichten der Verschlüsselung *im* virtuellen Computer erforderlich. Verwenden Sie BitLocker auf virtuellen Windows-Computern und **dm-crypt** auf virtuellen Linux-Computern. Daten, die auf diesem Weg eingehen, werden von Azure Backup nicht automatisch verschlüsselt.

### <a name="network"></a>Netzwerk
| Feature | Azure Backup-Agent | System Center DPM | Azure Backup Server | Azure IaaS-VM-Sicherung |
| --- | --- | --- | --- | --- |
| Netzwerkkomprimierung <br/>(auf **Sicherungsserver**) | |![Ja][green] |![Ja][green] | |
| Netzwerkkomprimierung <br/>(in **Sicherungstresor**) |![Ja][green] |![Ja][green] |![Ja][green] | |
| Netzwerkprotokoll <br/>(auf **Sicherungsserver**) | |TCP |TCP | |
| Netzwerkprotokoll <br/>(in **Sicherungstresor**) |HTTPS |HTTPS |HTTPS |HTTPS |

![Tabellenschlüssel](./media/backup-introduction-to-azure-backup/table-key-2.png)

Die VM-Erweiterung (auf der IaaS-VM) liest die Daten über das Speichernetzwerk direkt aus dem Azure-Speicherkonto, sodass es nicht erforderlich ist, diesen Datenverkehr zu komprimieren.

Wenn Sie Ihre Daten auf einer System Center DPM- oder Azure Backup Server-Einheit sichern, ist es ratsam, die Daten für den Weg vom primären Server zum Sicherungsserver zu komprimieren. Wenn die Daten vor dem Sichern in DPM oder Azure Backup Server komprimiert werden, wird weniger Bandbreite beansprucht.

#### <a name="network-throttling"></a>Netzwerkdrosselung
Der Azure Backup-Agent verfügt über die Netzwerkdrosselung, mit der Sie steuern können, wie die Netzwerkbandbreite während der Datenübertragung verwendet wird. Die Drosselung kann hilfreich sein, wenn Sie Daten während der Geschäftszeiten sichern möchten, der Sicherungsprozess aber keine Auswirkung auf den anderen Internetdatenverkehr haben soll. Die Drosselung der Datenübertragung gilt für Sicherungs- und Wiederherstellungsaktivitäten.

### <a name="backup-and-retention"></a>Sicherung und Aufbewahrung
|  | Azure Backup-Agent | System Center DPM | Azure Backup Server | Azure IaaS-VM-Sicherung |
| --- | --- | --- | --- | --- |
| Sicherungshäufigkeit<br/> (im Backup-Tresor) |Drei Sicherungen pro Tag |Zwei Sicherungen pro Tag |Zwei Sicherungen pro Tag |Eine Sicherung pro Tag |
| Sicherungshäufigkeit<br/> (auf Datenträger) |Nicht zutreffend |<li>Alle 15 Minuten für SQL Server <li>Stündlich für andere Workloads |<li>Alle 15 Minuten für SQL Server <li>Stündlich für andere Workloads</p> |Nicht zutreffend |
| Aufbewahrungsoptionen |Täglich, wöchentlich, monatlich, jährlich |Täglich, wöchentlich, monatlich, jährlich |Täglich, wöchentlich, monatlich, jährlich |Täglich, wöchentlich, monatlich, jährlich |
| Aufbewahrungszeitraum |Bis zu 99 Jahre |Bis zu 99 Jahre |Bis zu 99 Jahre |Bis zu 99 Jahre |
| Wiederherstellungspunkte in Azure Backup-Tresor |Unbegrenzt |Unbegrenzt |Unbegrenzt |Unbegrenzt |
| Wiederherstellungspunkte auf lokalem Datenträger |Nicht zutreffend |<li>64 für Dateiserver,<li>448 für Anwendungsserver |<li>64 für Dateiserver,<li>448 für Anwendungsserver |Nicht zutreffend |
| Wiederherstellungspunkte auf Band |Nicht zutreffend |Unbegrenzt |Nicht zutreffend |Nicht zutreffend |

## <a name="what-is-the-vault-credential-file"></a>Was ist die Datei mit Tresoranmeldeinformationen?
Die Datei mit Tresoranmeldeinformationen ist ein Zertifikat, das vom Portal für jeden Backup-Tresor generiert wird. Das Portal lädt anschließend den öffentlichen Schlüssel in den Access Control Service (ACS) hoch. Der private Schlüssel wird beim Herunterladen von Anmeldeinformationen für Sie bereitgestellt. Verwenden Sie ihn, um die zu schützenden Computer zu registrieren. Mit dem privaten Schlüssel können Sie die Server oder Computer für das Senden von Sicherungsdaten an einen bestimmten Backup-Tresor authentifizieren.

Sie verwenden die Tresoranmeldeinformationen nur, um die Server oder Computer zu registrieren. Gehen Sie mit den Tresoranmeldeinformationen aber sorgfältig um. Wenn sie verloren gehen oder in den Besitz anderer Personen gelangen, können die Tresoranmeldeinformationen zum Registrieren von anderen Computern für denselben Tresor verwendet werden. Da die Sicherungsdaten mit einer Passphrase verschlüsselt werden, auf die nur Sie Zugriff haben, können vorhandene Sicherungsdaten nicht kompromittiert werden. Tresoranmeldeinformationen laufen nach 48 Stunden ab. Sie können die Tresoranmeldeinformationen des Backup-Tresors zwar so häufig wie gewünscht herunterladen, aber nur die jeweils letzten Anmeldeinformationen können für die Registrierung verwendet werden.

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Wie unterscheidet sich Azure Backup von Azure Site Recovery?
Azure Backup und Azure Site Recovery ähneln sich, da mit beiden Diensten Daten gesichert und dann wiederhergestellt werden können, aber sie unterscheiden sich in Bezug auf den Hauptzweck.

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

[grün]: ./media/backup-introduction-to-azure-backup/green.png
[gelb]: ./media/backup-introduction-to-azure-backup/yellow.png
[rot]: ./media/backup-introduction-to-azure-backup/red.png



<!--HONumber=Nov16_HO3-->


