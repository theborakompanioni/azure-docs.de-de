---
title: "Sicherung und Notfallwiederherstellung für IaaS-Datenträger in Azure | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie die Sicherung und Notfallwiederherstellung von virtuellen IaaS-Computern und Datenträgern in Azure planen. Im Dokument werden sowohl Managed Disks als auch nicht verwaltete Datenträger behandelt."
services: storage
cloud: Azure
documentationcenter: na
author: luywang
manager: kavithag
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: luywang
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: b675daf4874a4bfe663a77b9e1097c00f00a13f4
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="backup-and-disaster-recovery-for-azure-iaas-disks"></a>Sicherung und Notfallwiederherstellung für Azure IaaS-Datenträger

In diesem Artikel wird beschrieben, wie Sie die Sicherung und Notfallwiederherstellung (Disaster Recovery, DR) von virtuellen IaaS-Computern (Virtual Machines, VMs) und Datenträgern in Azure planen. Im Dokument werden sowohl Managed Disks als auch nicht verwaltete Datenträger behandelt.

Zuerst werden die integrierten Fehlertoleranzfunktionen der Azure-Plattform behandelt, die Schutz vor lokalen Ausfällen bieten. Dann werden die Notfallszenarien behandelt, die durch die integrierten Funktionen nicht vollständig abgedeckt werden. Dies ist das Hauptthema des Dokuments. Außerdem werden einige Beispiele für Workloadszenarien angegeben, für die unterschiedliche Aspekte der Sicherung und Notfallwiederherstellung gelten können. Anschließend werden mögliche Lösungen für die Notfallwiederherstellung von IaaS-Datenträgern beschrieben. 

## <a name="introduction"></a>Einführung

Für die Azure-Plattform werden verschiedene Methoden in Bezug auf die Redundanz und Fehlertoleranz verwendet, um Kunden vor lokalen Hardwareausfällen zu schützen. Beispiele für lokale Ausfälle sind Probleme mit einem Azure Storage-Servercomputer, auf dem ein Teil der Daten für einen virtuellen Datenträger gespeichert wird, oder Ausfälle einer SSD oder HDD auf diesem Server. Solche isolierten Ausfälle von Hardwarekomponenten können während des normalen Betriebs auftreten. 

Die Azure-Plattform ist so konzipiert, dass sie diesen Ausfällen gegenüber sehr robust ist. Notfälle größeren Umfangs können zu Ausfällen oder zur Nichterreichbarkeit vieler Speicherserver oder sogar eines gesamten Rechenzentrums führen. Obwohl Ihre VMs und Datenträger im Normalfall vor lokalen Ausfällen geschützt sind, sind weitere Schritte erforderlich, um Ihre Workload vor schwerwiegenden Ausfällen ganzer Regionen (z.B. im Katastrophenfall) zu schützen, die Auswirkungen auf Ihre VMs und Datenträger haben können.

Neben möglichen Ausfällen der Plattform können auch Probleme mit der Kundenanwendung oder mit Daten auftreten. Eine neue Version Ihrer Anwendung kann beispielsweise unabsichtlich zu einer Änderung der Daten führen, die sie beschädigt. In diesem Fall sollten Sie eine vorherige Version der Anwendung und der Daten wiederherstellen, die den letzten bekannten fehlerfreien Status enthält. Dies erfordert die Durchführung von regelmäßigen Sicherungsvorgängen.

Sie müssen Ihre IaaS-VM-Datenträger in einer anderen Region sichern, um eine regionale Notfallwiederherstellung zu ermöglichen. 

Bevor wir uns mit den Optionen für die Sicherung und Notfallwiederherstellung befassen, sollen noch einmal einige Methoden für den Umgang mit lokalen Ausfällen beschrieben werden.

### <a name="azure-iaas-resiliency"></a>Azure IaaS-Resilienz

*Resilienz* ist die Toleranz in Bezug auf normale Ausfälle, die für Hardwarekomponenten auftreten. Bei der Resilienz geht es um die Möglichkeit, nach Ausfällen eine Wiederherstellung durchzuführen und die Betriebsbereitschaft sicherzustellen. Es geht nicht um das Vermeiden von Ausfällen, sondern um das Reagieren auf Ausfälle, um Ausfallzeiten oder Datenverluste zu vermeiden. Das Ziel besteht bei der Resilienz darin, für die Anwendung nach einem Ausfall wieder die volle Funktionsfähigkeit herzustellen. Virtuelle Azure-Computer und die dazugehörigen Datenträger sind so konzipiert, dass sie gegen häufig auftretende Hardwareausfälle resistent sind. Wir betrachten nun, wie diese Resilienz von der Azure IaaS-Plattform bereitgestellt wird.

Ein virtueller Computer besteht hauptsächlich aus zwei Teilen: einem Computeserver und den persistenten Datenträgern. Beides wirkt sich auf die Fehlertoleranz eines virtuellen Computers aus.

Wenn bei dem Azure-Computehostserver, auf dem sich Ihre VM-Benutzeroberflächen befinden, ein Hardwarefehler auftritt (seltener Fall), wird die VM von Azure automatisch auf einem anderen Server wiederhergestellt. Wenn dies geschieht, startet der Computer neu, und der virtuelle Computer wird nach einiger Zeit wieder hochgefahren. Hardwareausfälle dieser Art werden von Azure automatisch erkannt, und es werden Wiederherstellungen durchgeführt, um sicherzustellen, dass die VM des Kunden so schnell wie möglich wieder verfügbar ist.

In Bezug auf IaaS-Datenträger ist die Dauerhaftigkeit von Daten für die persistente Speicherplattform von entscheidender Bedeutung. Azure-Kunden nutzen wichtige Geschäftsanwendungen, die unter IaaS ausgeführt werden und von der Persistenz der Daten abhängig sind. In Azure wird der Schutz für diese IaaS-Datenträger mit drei lokal gespeicherten redundanten Kopien der Daten erreicht. Diese Kopien sorgen für hohe Widerstandsfähigkeit gegenüber lokalen Ausfällen. Falls eine der Hardwarekomponenten ausfällt, auf der sich Ihr Datenträger befindet, hat dies keine Auswirkungen auf Ihre VM. Der Grund ist, dass zwei zusätzliche Kopien zur Unterstützung von Datenträgeranforderungen vorhanden sind. Dies funktioniert auch dann gut, wenn zwei unterschiedliche Hardwarekomponenten, die einen Datenträger unterstützen, gleichzeitig ausfallen (äußerst seltener Fall). 

Damit sichergestellt ist, dass immer drei Replikate vorgehalten werden, erzeugt Azure Storage im Hintergrund automatisch eine neue Kopie der Daten für den Fall, dass eine der drei Kopien nicht mehr verfügbar ist. Daher ist es normalerweise nicht erforderlich, für Azure-Datenträger zur Erzielung der Fehlertoleranz RAID zu verwenden. Eine einfache RAID 0-Konfiguration sollte ausreichen, um bei Bedarf für die Datenträger das Striping durchzuführen, damit größere Volumes erstellt werden können.

Aufgrund dieser Architektur konnte Azure für IaaS-Datenträger durchgängig eine Dauerhaftigkeit auf Unternehmensniveau bereitstellen, mit einer branchenweit führenden [auf das Jahr umgerechneten Fehlerrate](https://en.wikipedia.org/wiki/Annualized_failure_rate) von null (0) Prozent.

Lokale Hardwareausfälle auf dem Computehost oder auf der Speicherplattform können in einigen Fällen zu einer vorübergehenden Nichtverfügbarkeit der VM führen. Dies ist durch die [Azure-Vereinbarung zum Servicelevel (SLA)](https://azure.microsoft.com/support/legal/sla/virtual-machines/) für die VM-Verfügbarkeit abgedeckt. Für Azure wird auch eine branchenführende SLA für einzelne VM-Instanzen bereitgestellt, die Storage Premium-Datenträger nutzen.

Um Anwendungsworkloads vor Ausfallzeiten aufgrund der vorübergehenden Nichtverfügbarkeit eines Datenträgers oder einer VM zu schützen, können Kunden [Verfügbarkeitsgruppen](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability) nutzen. Zwei oder mehr virtuelle Computer in einer Verfügbarkeitsgruppe sorgen für die Redundanz der Anwendung. Azure erstellt diese VMs und Datenträger dann in separaten Fehlerdomänen mit unterschiedlichen Stromversorgungs-, Netzwerk- und Serverkomponenten. 

Wegen dieser separaten Fehlerdomänen wirken sich lokale Hardwareausfälle in der Regel nicht gleichzeitig auf mehrere VMs in der Gruppe aus. Separate Fehlerdomänen gewährleisten eine hohe Verfügbarkeit Ihrer Anwendung. Eine bewährte Methode besteht darin, Verfügbarkeitsgruppen zu verwenden, wenn eine hohe Verfügbarkeit erforderlich ist. Im nächsten Abschnitt wird die Notfallwiederherstellung behandelt.

### <a name="backup-and-disaster-recovery"></a>Backup und Notfallwiederherstellung

Wiederherstellung nach einem Notfall ist die Fähigkeit zur Wiederherstellung bei seltenen, aber gravierenden Vorfällen. Dies schließt dauerhafte, weitreichende Fehler wie eine Dienstunterbrechung ein, die eine ganze Region betrifft. Die Notfallwiederherstellung umfasst die Datensicherung und -archivierung und kann auch einen manuellen Eingriff beinhalten, z.B. das Wiederherstellen einer Datenbank aus einer Sicherung.

Der integrierte Schutz der Azure-Plattform vor lokalen Ausfällen reicht für die VMs/Datenträger unter Umständen nicht aus, falls es zu größeren Katastrophen kommt, die umfassende Ausfälle nach sich ziehen. Beispiele für diese Katastrophenfälle: Ein Rechenzentrum ist von einem Hurrikan, Erdbeben, Feuer oder Ausfall von Hardwareeinheiten in größerem Umfang betroffen. Darüber hinaus kann es auch aufgrund von Anwendungs- oder Datenproblemen zu Ausfällen kommen.

Um Ihre IaaS-Workloads vor Ausfällen zu schützen, sollten Sie Redundanz- und Sicherungsmaßnahmen planen, damit eine Wiederherstellung möglich ist. Für die Notfallwiederherstellung sollten Sie Sicherungen an einem anderen geografischen Standort durchführen, der vom primären Standort weiter entfernt ist. So wird sichergestellt, dass Ihre Sicherung nicht von demselben Ereignis beeinträchtigt wird, das sich ursprünglich auf die VM oder die Datenträger ausgewirkt hat. Weitere Informationen finden Sie unter [Disaster recovery for Azure applications](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications) (Notfallwiederherstellung für Azure-Anwendungen).

Für Ihre Planung der Notfallwiederherstellung können die folgenden Aspekte wichtig sein:

- Hohe Verfügbarkeit: die Möglichkeit, dass die Anwendung ohne signifikante Ausfallzeit in einem fehlerfreien Zustand weiterbetrieben werden kann. Mit *fehlerfreier Zustand* ist gemeint, dass die Anwendung reagiert und Benutzer eine Verbindung mit der Anwendung herstellen und damit interagieren können. Bestimmte unternehmenskritische Anwendungen und Datenbanken müssen ggf. auch dann immer verfügbar sein, wenn es bei der Plattform zu Ausfällen kommt. Für diese Workloads müssen Sie ggf. die Redundanz für die Anwendung und für die Daten planen.

- Dauerhaftigkeit von Daten: In einigen Fällen muss vor allem sichergestellt werden, dass die Daten im Katastrophenfall nicht verloren gehen. Daher kann es erforderlich sein, dass Sie über eine Sicherung Ihrer Daten an einem anderen Standort verfügen. Für Workloads dieser Art ist unter Umständen keine vollständige Redundanz für die Anwendung erforderlich, sondern nur eine regelmäßige Sicherung der Datenträger.

## <a name="backup-and-dr-scenarios"></a>Szenarien für Sicherung und Notfallwiederherstellung

Wir sehen uns nun einige typische Beispiele für Anwendungsworkload-Szenarien und die wichtigen Aspekte der Planung für die Notfallwiederherstellung an.

### <a name="scenario-1-major-database-solutions"></a>Szenario 1: Größere Datenbanklösungen

Angenommen, Sie verwenden einen Produktionsdatenbankserver wie SQL Server oder Oracle, der die hohe Verfügbarkeit unterstützen kann. Von dieser Datenbank sind wichtige Produktionsanwendungen und Benutzer abhängig. Der Notfallwiederherstellungs-Plan für dieses System muss ggf. die folgenden Anforderungen unterstützen:

- Die Daten müssen geschützt und wiederherstellbar sein.
- Der Server muss für die Nutzung verfügbar sein.

Der Notfallwiederherstellungs-Plan erfordert ggf. das Vorhalten eines Replikats der Datenbank in einer anderen Region als Sicherung. Je nach den Anforderungen für die Serververfügbarkeit und Datenwiederherstellung kann die Lösung von einem Aktiv-Aktiv- oder Aktiv-Passiv-Replikatstandort bis zu regelmäßigen Offlinesicherungen der Daten reichen. Relationale Datenbanken, z.B. SQL Server und Oracle, bieten unterschiedliche Optionen für die Replikation. Nutzen Sie für SQL Server [SQL Server AlwaysOn-Verfügbarkeitsgruppen](https://msdn.microsoft.com/library/hh510230.aspx), um eine hohe Verfügbarkeit zu erzielen.

NoSQL-Datenbanken, z.B. MongoDB, unterstützen ebenfalls [Replikate](https://docs.mongodb.com/manual/replication/), um Redundanz zu erzielen. Die Replikate für hohe Verfügbarkeit werden genutzt.

### <a name="scenario-2-a-cluster-of-redundant-vms"></a>Szenario 2: Cluster mit redundanten VMs

Stellen Sie sich eine Workload vor, die von einem Cluster mit VMs verarbeitet wird, mit denen Redundanz und ein Lastenausgleich erzielt wird. Ein Beispiel hierfür ist ein Cassandra-Cluster, der in einer Region bereitgestellt wird. Mit dieser Art von Architektur wird in der Region bereits eine hohe Redundanzebene erreicht. Um die Workload vor einem Ausfall auf regionaler Ebene zu schützen, sollten Sie erwägen, den Cluster auf zwei Regionen zu verteilen oder regelmäßig Sicherungen in einer anderen Region zu erstellen.

### <a name="scenario-3-iaas-application-workload"></a>Szenario 3: IaaS-Anwendungsworkload

Wir betrachten nun die IaaS-Anwendungsworkload. Dies könnte z.B. eine typische Produktionsworkload sein, die auf einer Azure-VM ausgeführt wird. Es kann sich um einen Webserver oder Dateiserver handeln, der den Inhalt und andere Ressourcen eines Standorts enthält. Es kann sich auch um eine benutzerdefinierte Geschäftsanwendung handeln, die auf einer VM ausgeführt wird, auf der die dazugehörigen Daten, Ressourcen und der Anwendungszustand auf den VM-Datenträgern gespeichert sind. In diesem Fall muss sichergestellt werden, dass Sie regelmäßig Sicherungen erstellen. Die Sicherungshäufigkeit sollte sich nach der Art der VM-Workload richten. Wenn die Anwendung beispielsweise jeden Tag ausgeführt wird und Daten ändert, sollte einmal pro Stunde eine Sicherung erstellt werden.

Ein weiteres Beispiel ist ein Berichtsserver, der Daten aus anderen Quellen abruft und aggregierte Berichte generiert. Der Verlust dieser VM bzw. Datenträger könnte zum Verlust der Berichte führen. Es kann aber möglich sein, den Berichterstellungsvorgang erneut durchzuführen und die Ausgabe neu zu generieren. In diesem Fall haben Sie auch dann keinen Datenverlust, wenn der Berichtsserver von einem Notfall betroffen ist. Daher sollten Sie ein höheres Maß an Fehlertoleranz für den Verlust eines Teils der Daten auf dem Berichtsserver haben. Weniger häufige Sicherungen sind hierbei eine Option, um die Kosten zu reduzieren.

### <a name="scenario-4-iaas-application-data-issues"></a>Szenario 4: Datenprobleme von IaaS-Anwendungen

Datenprobleme von IaaS-Anwendungen sind eine weitere Möglichkeit. Stellen Sie sich vor, Sie würden über eine Anwendung verfügen, die wichtige Geschäftsdaten verarbeitet, verwaltet und bereitstellt, z.B. Preisinformationen. In einer neuen Version Ihrer Anwendung war ein Softwarefehler enthalten, durch den die Preise falsch berechnet und die vorhandenen Geschäftsdaten, die von der Plattform bereitgestellt wurden, beschädigt wurden. Hier besteht die beste Vorgehensweise darin, die frühere Version der Anwendung und der Daten wiederherzustellen. Erstellen Sie regelmäßige Sicherungen Ihres Systems, um dies zu ermöglichen.

## <a name="disaster-recovery-solution-azure-backup"></a>Lösung für Notfallwiederherstellung: Azure Backup 

[Azure Backup](https://azure.microsoft.com/services/backup/) kann für Sicherungen und Notfallwiederherstellungen verwendet werden und funktioniert mit [Managed Disks](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/managed-disks-overview) und [nicht verwalteten Datenträgern](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks). Sie können einen Sicherungsauftrag mit zeitbasierten Sicherungen, unkomplizierter Wiederherstellung von virtuellen Computern und Aufbewahrungsrichtlinien für Sicherungen erstellen. 

Wenn Sie [Storage Premium-Datenträger](https://docs.microsoft.com/en-us/azure/storage/common/storage-premium-storage), [Managed Disks](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/managed-disks-overview) oder andere Datenträgerarten mit der Option für [lokal redundanten Speicher](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy#locally-redundant-storage) verwenden, ist es besonders wichtig, regelmäßige Sicherungen für die Notfallwiederherstellung zu erstellen. Azure Backup speichert die Daten zur langfristigen Aufbewahrung in Ihrem Recovery Services-Tresor. Wählen Sie die Option [Georedundanter Speicher](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy#geo-redundant-storage) für den Recovery Services-Tresor von Backup. Diese Option stellt sicher, dass Sicherungen als Schutz vor regionalen Katastrophen in einer anderen Azure-Region repliziert werden.

Bei [nicht verwalteten Datenträgern](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/about-disks-and-vhds#unmanaged-disks) können Sie den lokal redundanten Speichertyp für IaaS-Datenträger verwenden. Stellen Sie aber sicher, dass Azure Backup mit der Option für georedundanten Speicher für den Recovery Services-Tresor aktiviert ist.

> [!NOTE]
> Wenn Sie den [georedundanten Speicher](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy#geo-redundant-storage) oder [georedundanten Speicher mit Lesezugriff](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy#read-access-geo-redundant-storage) als Option für Ihre nicht verwalteten Datenträger verwenden, benötigen Sie trotzdem konsistente Momentaufnahmen für die Sicherung und die Notfallwiederherstellung. Verwenden Sie entweder [Azure Backup](https://azure.microsoft.com/services/backup/) oder [konsistente Momentaufnahmen](#alternative-solution-consistent-snapshots).

 Die folgende Tabelle enthält eine Zusammenfassung der für die Notfallwiederherstellung verfügbaren Lösungen.

| Szenario | Automatische Replikation | Lösung für die Notfallwiederherstellung |
| --- | --- | --- |
| Storage Premium-Datenträger | Lokal ([lokal redundanter Speicher](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Verwaltete Datenträger | Lokal ([lokal redundanter Speicher](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Nicht verwaltete Datenträger mit lokal redundantem Speicher | Lokal ([lokal redundanter Speicher](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy#locally-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/) |
| Nicht verwaltete Datenträger mit georedundantem Speicher | Regionsübergreifend ([georedundanter Speicher](https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy#geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Konsistente Momentaufnahmen](#alternative-solution-consistent-snapshots) |
| Nicht verwaltete Datenträger mit georedundantem Speicher mit Lesezugriff | Regionsübergreifend ([georedundanter Speicher mit Lesezugriff](storage-redundancy.md#read-access-geo-redundant-storage)) | [Azure Backup](https://azure.microsoft.com/services/backup/)<br/>[Konsistente Momentaufnahmen](#alternative-solution-consistent-snapshots) |

Eine hohe Verfügbarkeit kann am besten durch die Verwendung von Managed Disks in einer Verfügbarkeitsgruppe mit Azure Backup erzielt werden. Wenn Sie nicht verwaltete Datenträger nutzen, können Sie Azure Backup trotzdem für die Notfallwiederherstellung verwenden. Falls die Verwendung von Azure Backup nicht möglich ist, ist das Erstellen von [konsistenten Momentaufnahmen](#alternative-solution-consistent-snapshots) (in einem späteren Abschnitt beschrieben) eine Alternativlösung für die Sicherung und Notfallwiederherstellung.

Ihre Auswahl in Bezug auf hohe Verfügbarkeit, Sicherung und Notfallwiederherstellung auf Anwendungs- oder Infrastrukturebene kann wie unten gezeigt dargestellt werden:

| Ebene |   Hohe Verfügbarkeit   | Sicherung oder Notfallwiederherstellung |
| --- | --- | --- |
| Anwendung | SQL Server AlwaysOn | Azure Backup |
| Infrastruktur    | Verfügbarkeitsgruppe  | Georedundanter Speicher mit konsistenten Momentaufnahmen |

### <a name="using-azure-backup"></a>Verwenden von Azure Backup 

Mit [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction) können Ihre VMs mit Windows oder Linux im Azure Recovery Services-Tresor gesichert werden. Das Sichern und Wiederherstellen unternehmenskritischer Daten wird dadurch erschwert, dass die Daten während der Ausführung der Anwendungen, von denen die Daten generiert werden, gesichert werden müssen. 

Um dieses Problem zu beheben, bietet Azure Backup anwendungskonsistente Sicherungen für Microsoft-Workloads. Dabei wird der Volumeschattenkopie-Dienst verwendet, um sicherzustellen, dass die Daten ordnungsgemäß in den Speicher geschrieben werden. Für Linux-VMs sind nur dateikonsistente Sicherungen möglich, da Linux nicht über eine mit dem Volumeschattenkopie-Dienst vergleichbare Funktionalität verfügt.

![Azure Backup-Ablauf][1]

Wenn Azure Backup einen Sicherungsauftrag zur geplanten Zeit initiiert, wird die auf der VM installierte Sicherungserweiterung zum Erstellen einer Momentaufnahme ausgelöst. Diese Momentaufnahme wird in Koordination mit dem Volumeschattenkopie-Dienst (Volume Shadow Copy Service, VSS) erstellt, um eine konsistente Momentaufnahme der Datenträger auf dem virtuellen Computer zu erhalten, ohne dass dieser heruntergefahren werden muss. Mit der Sicherungserweiterung auf der VM werden alle Schreibvorgänge geleert, bevor die konsistente Momentaufnahme für alle Datenträger erstellt wird. Nachdem die Momentaufnahme erstellt wurde, werden die Daten von Azure Backup an den Sicherungstresor übertragen. Um den Sicherungsvorgang effizienter zu gestalten, werden vom Dienst nur diejenigen Datenblöcke identifiziert und übertragen, die nach der letzten Sicherung geändert wurden.

Für die Wiederherstellung können Sie die verfügbaren Sicherungen über Azure Backup anzeigen und dann eine Wiederherstellung initiieren. Sie können Azure-Sicherungen per [Azure-Portal](https://portal.azure.com/), [PowerShell](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-automation) oder [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install) erstellen und wiederherstellen. 

### <a name="steps-to-enable-a-backup"></a>Schritte zum Aktivieren einer Sicherung

Verwenden Sie die folgenden Schritte, um das Erstellen von Sicherungen Ihrer VMs mit dem [Azure-Portal](https://portal.azure.com/) zu ermöglichen. Je nach Ihrem Szenario weichen einige Punkte ggf. ab. Ausführliche Informationen finden Sie in der Dokumentation zu [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction). Azure Backup [unterstützt auch VMs mit Managed Disks](https://azure.microsoft.com/blog/azure-managed-disk-backup/).

1.  Erstellen eines Recovery Services-Tresors für eine VM:

    a. Durchsuchen Sie im [Azure-Portal](https://portal.azure.com/) **Alle Ressourcen** nach **Recovery Services-Tresoren**.

    b. Klicken Sie im Menü **Recovery Services-Tresore** auf **Hinzufügen**, und führen Sie die Schritte zum Erstellen eines neuen Tresors in derselben Region wie die VM aus. Wenn sich Ihre VM beispielsweise in der Region „USA, Westen“ befindet, wählen Sie für den Tresor „USA, Westen“ aus.

2.  Überprüfen Sie die Speicherreplikation für den neu erstellten Tresor. Greifen Sie unter **Recovery Services-Tresore** auf den Tresor zu, und wechseln Sie zu **Einstellungen** > **Sicherungskonfiguration**. Stellen Sie sicher, dass die Option für **georedundanten Speicher** standardmäßig aktiviert ist. Dies gewährleistet, dass Ihr Tresor automatisch in einem sekundären Rechenzentrum repliziert wird. Der Tresor in „USA, Westen“ wird beispielsweise automatisch in „USA, Osten“ repliziert.

3.  Konfigurieren Sie die Sicherungsrichtlinie, und wählen Sie die VM über dieselbe Benutzeroberfläche aus.

4.  Stellen Sie sicher, dass der Backup-Agent auf der VM installiert ist. Wenn Ihre VM mit einem Azure-Katalogimage erstellt wurde, ist der Backup-Agent bereits installiert. Verwenden Sie andernfalls (also bei Verwendung eines benutzerdefinierten Image) die Anleitung unter [Installieren des VM-Agents auf dem virtuellen Computer](https://docs.microsoft.com/en-us/azure/backup/backup-azure-arm-vms-prepare#install-the-vm-agent-on-the-virtual-machine).

5.  Stellen Sie sicher, dass die VM die Netzwerkkonnektivität zulässt, damit der Sicherungsdienst funktioniert. Befolgen Sie die Anweisungen für die [Netzwerkkonnektivität](https://docs.microsoft.com/en-us/azure/backup/backup-azure-arm-vms-prepare#network-connectivity).

6.  Nachdem die obigen Schritte ausgeführt wurden, wird die Sicherung gemäß Angabe in der Sicherungsrichtlinie in regelmäßigen Abständen durchgeführt. Bei Bedarf können Sie die erste Sicherung manuell über das Tresordashboard im Azure-Portal auslösen.

Informationen zur Automatisierung von Azure Backup mithilfe von Skripts finden Sie unter [PowerShell-Cmdlets für VM-Sicherung](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-automation).

### <a name="steps-for-recovery"></a>Schritte zur Wiederherstellung

Wenn Sie eine VM reparieren oder neu erstellen müssen, können Sie sie über die Wiederherstellungspunkte der Sicherung im Tresor wiederherstellen. Es sind verschiedene Optionen zum Durchführen der Wiederherstellung vorhanden:

-   Sie können eine neue VM als Zeitpunktdarstellung Ihrer gesicherten VM erstellen.

-   Sie können die Datenträger wiederherstellen und die Vorlage dann für die VM verwenden, um die wiederhergestellte VM anzupassen und neu zu erstellen. 

Weitere Informationen hierzu finden Sie in der Anleitung zum [Wiederherstellen virtueller Computer über das Azure-Portal](https://docs.microsoft.com/en-us/azure/backup/backup-azure-arm-restore-vms#restoring-a-vm-during-azure-datacenter-disaster). In diesem Dokument werden auch die einzelnen Schritte zum Wiederherstellen von gesicherten VMs im gekoppelten Rechenzentrum beschrieben, indem Ihr georedundanter Sicherungstresor im Falle einer Katastrophe für das primäre Rechenzentrum verwendet wird. In diesem Fall verwendet Azure Backup den Computedienst der sekundären Region, um den wiederhergestellten virtuellen Computer zu erstellen.

Sie können PowerShell auch zum [Wiederherstellen einer VM](https://docs.microsoft.com/en-us/azure/backup/backup-azure-arm-restore-vms#restoring-a-vm-during-azure-datacenter-disaster) oder [Erstellen einer neuen VM aus wiederhergestellten Datenträgern](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-automation#create-a-vm-from-restored-disks) verwenden.

## <a name="alternative-solution-consistent-snapshots"></a>Alternativlösung: Konsistente Momentaufnahmen

Wenn Sie Azure Backup nicht nutzen können, können Sie Ihren eigenen Sicherungsmechanismus mithilfe von Momentaufnahmen implementieren. Es ist kompliziert, konsistente Momentaufnahmen für alle Datenträger zu erstellen, die von einer VM verwendet werden, und diese Momentaufnahmen dann in einer anderen Region zu replizieren. Aus diesem Grund ist die Nutzung des Backup-Diensts für Azure eine bessere Option als das Erstellen einer benutzerdefinierten Lösung. 

Bei Verwendung von georedundantem Speicher mit Lesezugriff / georedundantem Speicher für Datenträger werden Momentaufnahmen automatisch in einem sekundären Rechenzentrum repliziert. Wenn Sie lokal redundanten Speicher für Datenträger nutzen, müssen Sie die Daten selbst replizieren. Weitere Informationen finden Sie unter [Sichern nicht verwalteter Azure-VM-Datenträger mithilfe inkrementeller Momentaufnahmen](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/incremental-snapshots).

Eine Momentaufnahme ist eine Darstellung eines Objekts zu einem bestimmten Zeitpunkt. Eine Momentaufnahme führt dazu, dass Kosten für die inkrementelle Größe der enthaltenen Daten berechnet werden. Weitere Informationen finden Sie unter [Erstellen einer Momentaufnahme eines Blobs](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-snapshots).

### <a name="create-snapshots-while-the-vm-is-running"></a>Erstellen von Momentaufnahmen während der VM-Ausführung

Sie können zwar jederzeit eine Momentaufnahme erstellen, aber Sie sollten Folgendes beachten: Wenn die VM ausgeführt wird, werden weiter Daten auf die Datenträger gestreamt, und die Momentaufnahmen enthalten ggf. Teilvorgänge, die gerade aktiv waren. Falls mehrere Datenträger beteiligt sind, wurden die Momentaufnahmen der unterschiedlichen Datenträger außerdem ggf. zu verschiedenen Zeiten erstellt. Dies bedeutet, dass diese Momentaufnahmen möglicherweise nicht koordiniert werden können. Dies ist besonders für Stripesetvolumes problematisch, deren Dateien beschädigt werden können, falls während der Sicherung Änderungen durchgeführt werden.

Für den Sicherungsvorgang müssen die folgenden Schritte implementiert werden, um diese Situation zu vermeiden:

1.  Einfrieren aller Datenträger.

2.  Leeren aller ausstehenden Schreibvorgänge.

3.  [Erstellen Sie eine Momentaufnahme eines Blobs](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blob-snapshots) für alle Datenträger.

Einige Windows-Anwendungen, z.B. SQL Server, stellen einen Mechanismus für koordinierte Sicherungen per Volumeschattenkopie-Dienst bereit, um anwendungskonsistente Sicherungen zu erstellen. Unter Linux können Sie ein Tool wie fsfreeze zur Koordinierung der Datenträger verwenden. Dieses Tool bietet dateikonsistente Sicherungen, aber nicht anwendungskonsistente Momentaufnahmen. Dieser Prozess ist komplex, und es ist ratsam, [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/backup-azure-vms-introduction) oder eine Sicherungslösung eines Drittanbieters zu verwenden, bei der dieses Verfahren bereits implementiert ist.

Der obige Prozess führt zu einer Sammlung mit koordinierten Momentaufnahmen für alle VM-Datenträger und stellt eine VM-Ansicht für einen bestimmten Zeitpunkt dar. Dies ist ein Sicherungswiederherstellungspunkt für die VM. Sie können den Prozess in geplanten Intervallen wiederholen, um regelmäßige Sicherungen zu erstellen. Unter [Kopieren der Momentaufnahmen in eine andere Region](#copy-the-snapshots-to-another-region) finden Sie Schritte zum Kopieren der Momentaufnahmen zur Notfallwiederherstellung in eine andere Region.

### <a name="create-snapshots-while-the-vm-is-offline"></a>Erstellen von Momentaufnahmen, während die VM offline ist

Eine andere Option zum Erstellen von konsistenten Sicherungen ist das Herunterfahren der VM und Erstellen von Blobmomentaufnahmen für jeden Datenträger. Das Erstellen von Blobmomentaufnahmen ist einfacher als das Koordinieren von Momentaufnahmen einer aktiven VM, aber es sind einige Minuten Ausfallzeit erforderlich.

1. Fahren Sie die VM herunter.

2. Erstellen Sie eine Momentaufnahme von jedem Blob einer virtuellen Festplatte. Dies dauert nur wenige Sekunden.

    Zum Erstellen einer Momentaufnahme können Sie [PowerShell](https://docs.microsoft.com/en-us/azure/storage/common/storage-powershell-guide-full#how-to-manage-azure-blob-snapshots), die [Azure Storage-REST-API](https://msdn.microsoft.com/library/azure/ee691971.aspx), [Azure CLI](https://docs.microsoft.com/azure/xplat-cli-install) oder eine der Azure Storage-Clientbibliotheken, z.B. [die Storage-Clientbibliothek für .NET](https://msdn.microsoft.com/library/azure/hh488361.aspx), verwenden.

3. Starten Sie die VM, um die Ausfallzeit zu beenden. Normalerweise wird der gesamte Prozess innerhalb weniger Minuten abgeschlossen.

Bei diesem Prozess wird eine Sammlung mit den konsistenten Momentaufnahmen für alle Datenträger erstellt, um einen Sicherungswiederherstellungspunkt für die VM zu erhalten.

### <a name="copy-the-snapshots-to-another-region"></a>Kopieren der Momentaufnahmen in eine andere Region

Die Erstellung der Momentaufnahmen allein ist für die Notfallwiederherstellung ggf. nicht ausreichend. Sie müssen auch die Momentaufnahmensicherungen in eine andere Region replizieren.

Bei Verwendung von georedundantem Speicher oder georedundantem Speicher mit Lesezugriff für Ihre Datenträger werden die Momentaufnahmen automatisch in der sekundären Region repliziert. Es kann einige Minuten dauern, bis die Replikation beginnt. Wenn das primäre Rechenzentrum ausfällt, bevor die Replikation der Momentaufnahmen fertig gestellt ist, können Sie nicht vom sekundären Rechenzentrum aus auf die Momentaufnahmen zugreifen. Die Wahrscheinlichkeit hierfür ist relativ gering.

> [!NOTE] 
> Wenn sich nur die Datenträger in einem georedundanten Speicher oder georedundanten Speicher mit Lesezugriff befinden, schützt dies den virtuellen Computer nicht vor Notfällen. Sie müssen außerdem koordinierte Momentaufnahmen erstellen oder Azure Backup verwenden. Dies ist erforderlich, um für eine VM einen konsistenten Zustand wiederherzustellen.

Wenn Sie lokal redundanten Speicher verwenden, müssen Sie die Momentaufnahmen sofort nach dem Erstellen der Momentaufnahme in ein anderes Speicherkonto kopieren. Das Ziel des Kopiervorgangs könnte ein lokal redundantes Speicherkonto in einer anderen Region sein, was dazu führt, dass sich die Kopie in einer weiter entfernten Region befindet. Sie können die Momentaufnahme auch in ein georedundantes Speicherkonto mit Lesezugriff in derselben Region kopieren. In diesem Fall wird die Momentaufnahme zeitverzögert in der entfernten sekundären Region repliziert. Ihre Sicherung ist vor Katastrophen am primären Standort geschützt, nachdem der Kopiervorgang und die Replikation abgeschlossen sind.

Informationen zum effizienten Kopieren Ihrer inkrementellen Momentaufnahmen für die Notfallwiederherstellung finden Sie in der Anleitung zum [Sichern nicht verwalteter Azure-VM-Datenträger mithilfe inkrementeller Momentaufnahmen](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/incremental-snapshots).

![Sichern nicht verwalteter Azure-VM-Datenträger mithilfe inkrementeller Momentaufnahmen][2]

### <a name="recovery-from-snapshots"></a>Wiederherstellung von Daten aus Momentaufnahmen

Kopieren Sie eine Momentaufnahme, um sie abzurufen und ein neues Blob zu erstellen. Wenn Sie die Momentaufnahme aus dem primären Konto kopieren, können Sie sie in das Basisblob der Momentaufnahme kopieren. Dieser Vorgang stellt den Datenträger in der Momentaufnahme wieder her. Dieser Prozess wird als Heraufstufen der Momentaufnahme bezeichnet. Wenn Sie die Momentaufnahmensicherung aus einem sekundären Konto kopieren, müssen Sie sie im Fall eines georedundanten Speicherkontos mit Lesezugriff in ein primäres Konto kopieren. Sie können eine Momentaufnahme [mit PowerShell](https://docs.microsoft.com/en-us/azure/storage/common/storage-powershell-guide-full#how-to-copy-a-snapshot-of-a-blob) kopieren oder das Hilfsprogramm AzCopy verwenden. Weitere Informationen finden Sie unter [Übertragen von Daten mit dem Befehlszeilenprogramm AzCopy](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy).

Bei VMs mit mehreren Datenträgern müssen Sie alle Momentaufnahmen kopieren, die Teil desselben koordinierten Wiederherstellungspunkts sind. Nachdem Sie die Momentaufnahmen in schreibbare VHD-Blobs kopiert haben, können Sie die Blobs zum erneuten Erstellen Ihrer VM mithilfe der Vorlage für die VM verwenden.

## <a name="other-options"></a>Weitere Optionen

### <a name="sql-server"></a>SQL Server

Wenn SQL Server auf einer VM ausgeführt wird, sind eigene Funktionen zum Sichern Ihrer SQL Server-Datenbank im Azure-Blobspeicher oder auf einer Dateifreigabe vorhanden. Bei einem Speicherkonto des Typs georedundanter Speicher oder georedundanter Speicher mit Lesezugriff können Sie bei einem Notfall mit denselben Einschränkungen wie oben beschrieben im sekundären Rechenzentrum des Speicherkontos auf diese Sicherungen zugreifen. Weitere Informationen finden Sie unter [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery). Zusätzlich zum Sichern und Wiederherstellen können [SQL Server AlwaysOn-Verfügbarkeitsgruppen](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr) sekundäre Replikate von Datenbanken verwalten. Dies reduziert die Wiederherstellungszeit nach einem Notfall erheblich.

## <a name="other-considerations"></a>Weitere Überlegungen

In diesem Artikel wurde beschrieben, wie Sie Ihre VMs und die dazugehörigen Datenträger sichern oder Momentaufnahmen davon erstellen, um die Notfallwiederherstellung zu unterstützen, und wie Sie diese Komponenten für die Wiederherstellung Ihrer Daten verwenden. Beim Azure Resource Manager-Modell verwenden viele Benutzer Vorlagen, um ihre VMs und andere Infrastrukturelemente in Azure zu erstellen. Sie können eine Vorlage verwenden, um eine VM zu erstellen, die immer über die gleiche Konfiguration verfügt. Wenn Sie zum Erstellen Ihrer VMs benutzerdefinierte Images verwenden, müssen Sie auch sicherstellen, dass Ihre Images geschützt sind, indem Sie ein georedundantes Speicherkonto mit Lesezugriff zum Speichern verwenden.

Ihr Sicherungsprozess kann daher eine Kombination aus zwei Dingen sein:

- Sichern der Daten (Datenträger).
- Sichern der Konfiguration (Vorlagen und benutzerdefinierte Images).

Je nach gewählter Sicherungsoption müssen Sie unter Umständen die Sicherung der Daten und der Konfiguration durchführen, oder der Sicherungsdienst übernimmt diese Aufgaben für Sie.

## <a name="appendix-understanding-the-impact-of-data-redundancy"></a>Anhang: Grundlegendes zur Auswirkung der Datenredundanz

Für Speicherkonten in Azure gibt es drei Arten von Datenredundanz, die Sie in Bezug auf die Notfallwiederherstellung berücksichtigen sollten: lokal redundant, georedundant und georedundant mit Lesezugriff. 

Bei lokal redundantem Speicher werden drei Kopien der Daten in demselben Rechenzentrum aufbewahrt. Beim Schreiben der Daten durch die VM werden alle drei Kopien aktualisiert, bevor die Erfolgsmeldung an den Aufrufer zurückgegeben wird, damit Sie wissen, dass sie identisch sind. Ihr Datenträger ist gegen lokale Ausfälle geschützt, da es sehr unwahrscheinlich ist, dass alle drei Kopien gleichzeitig betroffen sind. Für lokal redundanten Speicher besteht keine Georedundanz, sodass der Datenträger nicht gegen Ausfälle aufgrund von Katastrophen geschützt ist, die ein gesamtes Rechenzentrum oder eine Speichereinheit betreffen können.

Mit georedundantem Speicher und georedundantem Speicher mit Lesezugriff werden drei Kopien Ihrer Daten in der von Ihnen ausgewählten primären Region aufbewahrt. In einer von Azure festgelegten entsprechenden sekundären Region werden drei weitere Kopien Ihrer Daten aufbewahrt. Wenn Sie Daten beispielsweise in „USA, Westen“ speichern, werden die Daten in „USA, Osten“ repliziert. Die Aufbewahrung der Kopien wird auf asynchrone Weise durchgeführt, und es kommt zwischen Updates des primären und sekundären Speicherorts zu einer kurzen Verzögerung. Replikate der Datenträger am sekundären Standort sind pro Datenträger konsistent (mit der Verzögerung), aber Replikate von mehreren aktiven Datenträgern sind unter Umständen nicht miteinander synchron. Konsistente Momentaufnahmen sind erforderlich, um konsistente Replikate über mehrere Datenträger zu erhalten.

Der Hauptunterschied zwischen georedundantem Speicher und georedundantem Speicher mit Lesezugriff ist, dass Sie mit georedundantem Speicher mit Lesezugriff jederzeit die sekundäre Kopie lesen können. Falls ein Problem auftritt, bei dem die Daten in der primären Region nicht mehr zugänglich sind, unternimmt das Azure-Team höchste Anstrengungen, um den Zugriff wiederherzustellen. Wenn die primäre Region ausgefallen ist und Sie georedundanten Speicher mit Lesezugriff aktiviert haben, können Sie auf die Daten im sekundären Rechenzentrum zugreifen. Falls Sie das Lesen des Replikats planen, während die primäre Region nicht zugänglich ist, sollten Sie darum die Verwendung von georedundantem Speicher mit Lesezugriff erwägen.

Wenn es sich um einen Ausfall größeren Umfangs handelt, kann das Azure-Team ggf. ein geografisches Failover auslösen und die primären DNS-Einträge ändern, um auf den sekundären Speicher zu verweisen. An diesem Punkt können Sie bei Aktivierung von georedundantem Speicher oder georedundantem Speicher mit Lesezugriff auf die Daten in der Region zugreifen, die bisher die sekundäre Region war. Anders ausgedrückt: Wenn Ihr Speicherkonto vom Typ georedundanter Speicher ist und ein Problem auftritt, können Sie nur dann auf den sekundären Speicher zugreifen, wenn ein geografisches Failover durchgeführt wird.

Weitere Informationen finden Sie unter [Vorgehensweise beim Ausfall von Azure Storage](https://docs.microsoft.com/en-us/azure/storage/common/storage-disaster-recovery-guidance). 

>[!NOTE] 
>Microsoft steuert, ob ein Failover erfolgt. Das Failover wird nicht pro Speicherkonto gesteuert und die Entscheidung daher nicht individuell pro Kunde getroffen. Zum Implementieren der Notfallwiederherstellung für bestimmte Speicherkonten oder VM-Datenträger müssen Sie die Verfahren verwenden, die weiter oben in diesem Artikel beschrieben wurden.



[1]: ./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-1.png
[2]: ./media/storage-backup-and-disaster-recovery-for-azure-iaas-disks/backup-and-disaster-recovery-for-azure-iaas-disks-2.png

