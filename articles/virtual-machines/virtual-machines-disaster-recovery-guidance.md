---
title: Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Computer | Microsoft Docs
description: Hier erhalten Sie Informationen zur Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Computer.
services: virtual-machines
documentationcenter: ''
author: kmouss
manager: timlt
editor: ''

ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: kmouss;aglick

---
# Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Computer
Bei Microsoft setzen wir uns mit großem Engagement dafür ein, dass unsere Dienste immer verfügbar sind, wenn Sie sie benötigen. Aufgrund von höherer Gewalt können jedoch gelegentlich ungeplante Dienstausfälle auftreten.

Microsoft stellt für seine Dienste Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) bereit, um dem Engagement für Verfügbarkeit und Konnektivität Nachdruck zu verleihen. Die SLAs für einzelne Azure-Dienste finden Sie unter [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/).

Azure weist bereits viele integrierte Plattformfunktionen auf, die hoch verfügbare Anwendungen unterstützen. Weitere Informationen zu diesen Diensten finden Sie unter [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Dieser Artikel behandelt ein echtes Szenario der Notfallwiederherstellung für den Fall, dass eine ganze Region aufgrund einer Naturkatastrophe oder einer umfangreichen Dienstunterbrechung von einem Ausfall betroffen ist. So etwas kommt zwar äußerst selten vor, dennoch müssen Sie für den Ausfall einer gesamten Region vorbereitet sein. Falls eine ganze Region von einer Dienstunterbrechung betroffen ist, sind die lokal redundanten Kopien Ihrer Daten vorübergehend nicht verfügbar. Bei aktivierter Georeplikation sind drei zusätzliche Kopien Ihrer Azure Storage-Blobs und -Tabellen in einer anderen Region gespeichert. Sollte eine gesamte Region ausfallen oder die primäre Region aufgrund einer Katastrophe nicht wiederherstellbar sein, ordnet Azure alle DNS-Einträge der georeplizierten Region zu.

> [!NOTE]
> Denken Sie daran, dass Sie keine Kontrolle über diesen Prozess haben. Er tritt nur bei einer regionsweiten Dienstunterbrechung auf. Setzen Sie daher auch auf andere anwendungsspezifische Sicherungsstrategien, um eine möglichst hohe Verfügbarkeit zu erreichen. Weitere Informationen finden Sie im Abschnitt zu [Datenstrategien für die Notfallwiederherstellung](../resiliency/resiliency-disaster-recovery-azure-applications.md#data-strategies-for-disaster-recovery).
> 
> 

In den folgenden Informationen zu virtuellen Azure-Computern erfahren Sie, welche Möglichkeiten Sie haben, wenn die gesamte Region, in der Ihre virtuelle Azure-Computeranwendung bereitgestellt wurde, von einer Dienstunterbrechung betroffen ist.

## Option 1: Warten auf die Wiederherstellung
In diesem Fall ist keine weitere Aktion erforderlich. Wir arbeiten intensiv daran, die Verfügbarkeit des Diensts wiederherzustellen. Der aktuelle Dienststatus wird auf dem [Dashboard zur Azure-Dienstintegrität](https://azure.microsoft.com/status/) angezeigt.

> [!NOTE]
> Dies ist die beste Option, wenn Sie vor der Unterbrechung nicht Azure Site Recovery, die Sicherung virtueller Computer, georedundanten Speicher mit Lesezugriff oder georedundanten Speicher eingerichtet haben. Wenn Sie den georedundanten Speicher oder den georedundanten Speicher mit Lesezugriff für das Speicherkonto eingerichtet haben, unter dem die virtuellen Festplatten (VHDs) der VM gespeichert sind, können Sie versuchen, die Festplatte mit dem Basisimage wiederherzustellen und darüber eine neue VM bereitzustellen. Dies ist nicht die bevorzugte Option, da die Synchronisierung der Daten nur dann sichergestellt ist, wenn eine Sicherung von Azure-VMs oder Azure Site Recovery verwendet wird. Daher kann nicht garantiert werden, dass diese Option funktioniert.
> 
> 

Für Kunden, die umgehend Zugriff auf virtuelle Computer benötigen, stehen die beiden folgenden Optionen zur Verfügung.

> [!NOTE]
> Bei beiden Optionen besteht die Gefahr von Datenverlusten.
> 
> 

## Option 2: Wiederherstellen eines virtuellen Computers aus einer Sicherung
Für Kunden, die eine VM-Sicherung konfiguriert haben, können Sie den virtuellen Computer auf der Grundlage einer Sicherung und eines Wiederherstellungspunkts wiederherstellen.

Informationen zum Wiederherstellen einer neuen VM aus Azure Backup finden Sie unter [Wiederherstellen virtueller Computer in Azure](../backup/backup-azure-restore-vms.md).

Informationen zum Planen der Infrastruktur für die Sicherung von virtuellen Azure-Computern finden Sie unter [Planen der Sicherungsinfrastruktur für virtuelle Computer in Azure](../backup/backup-azure-vms-introduction.md).

## Option 3: Initiieren eines Failovers per Azure Site Recovery
Wenn Sie Azure Site Recovery für Ihre betroffenen virtuellen Azure-Computer konfiguriert haben, können Sie Ihre virtuellen Computer auf der Grundlage ihrer Replikate wiederherstellen. Diese Replikate können sich entweder in Azure befinden oder lokal vorhanden sein. In diesem Fall können Sie auf der Grundlage des vorhandenen Replikats einen neuen virtuellen Computer erstellen. Informationen zum Wiederherstellen Ihrer virtuellen Computer auf der Grundlage eines Azure Site Recovery-Replikats finden Sie unter [Migrieren von virtuellen Azure-IaaS-Computern zwischen Azure-Regionen mit Azure Site Recovery](../site-recovery/site-recovery-migrate-azure-to-azure.md).

> [!NOTE]
> Datenträger mit Betriebssystemen oder mit Daten von virtuellen Azure-Computern werden zwar auf einer sekundären VHD repliziert, aber wenn sie in einem georedundanten Speicher oder einem georedundanten Speicherkonto mit Lesezugriff vorliegen, wird jede VHD separat repliziert. Diese Replikationsebene garantiert keine Konsistenz über die replizierten VHDs hinweg. Wenn Ihre Anwendung und/oder Ihre Datenbanken, die diese Datenträger verwenden, voneinander abhängig sind, ist nicht gewährleistet, dass alle virtuellen Festplatten als einzelne Momentaufnahme repliziert werden. Außerdem ist nicht garantiert, dass das VHD-Replikat aus georedundantem Speicher oder georedundantem Speicher mit Lesezugriff zu einer für Anwendungen konsistenten Momentaufnahme zum Starten der VM führt.
> 
> 

## Nächste Schritte
Weitere Informationen zur Implementierung einer Strategie für Notfallwiederherstellung und hohe Verfügbarkeit finden Sie unter [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Im [technischen Leitfaden zur Resilienz in Azure](../resiliency/resiliency-technical-guidance.md) können Sie detaillierte technische Kenntnisse hinsichtlich der Funktionen einer Cloudplattform erwerben.

Informationen zum Sichern von VMs finden Sie unter [Sichern von virtuellen Azure-Computern](../backup/backup-azure-vms.md).

Informationen zur Verwendung von Azure Site Recovery zum Orchestrieren und Automatisieren des Schutzes Ihrer physischen (und virtuellen) Windows- und Linux-Computer, die auf VMWare- und Hyper-V-VMs ausgeführt werden, finden Sie unter [Azure Site Recovery](https://azure.microsoft.com/documentation/learning-paths/site-recovery/).

Wenden Sie sich an den [Kundensupport](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls die Anweisungen nicht klar sein sollten oder Microsoft die Schritte für Sie ausführen soll.

<!---HONumber=AcomDC_0824_2016-->