<properties
	pageTitle="Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Computer | Microsoft Azure"
	description="Hier erhalten Sie Informationen zur Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Computer."
	services="virtual-machines"
	documentationCenter=""
	authors="kmouss"
	manager="drewm"
	editor=""/>

<tags
	ms.service="virtual-machines"
	ms.workload="virtual-machines"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/16/2016"
	ms.author="kmouss;aglick"/>

#Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Computer

Bei Microsoft setzen wir uns mit großem Engagement dafür ein, dass unsere Dienste immer verfügbar sind, wenn Sie sie benötigen. Aufgrund höherer Gewalt kann es jedoch gelegentlich zu ungeplanten Dienstausfällen kommen.

In Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) sichert Microsoft die Verfügbarkeit und Konnektivität seiner Dienste zu. Die SLAs für individuelle Azure-Dienste finden Sie unter [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/).

Azure verfügt bereits über zahlreiche integrierte Plattformfeatures zur Unterstützung hochverfügbarer Anwendungen. Weitere Informationen zu diesen Diensten finden Sie unter [Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](https://aka.ms/drtechguide).

Dieses Dokument behandelt eine echte Notfallwiederherstellung für den Fall, dass eine ganze Region aufgrund einer Naturkatastrophe oder einer umfangreichen Dienstunterbrechung von einem Ausfall betroffen ist. So etwas kommt zwar äußerst selten vor, dennoch sollten Sie auf den Ausfall der gesamten Region vorbereitet sein. Falls eine ganze Region von einer Dienstunterbrechung betroffen ist, sind die lokal redundanten Kopien Ihrer Daten vorübergehend nicht verfügbar. Bei aktivierter Georeplikation sind drei zusätzliche Kopien Ihrer Azure Storage-Blobs und Tabellen in einer anderen Region gespeichert. Sollte eine gesamte Region ausfallen oder die primäre Region aufgrund einer Katastrophe nicht wiederherstellbar sein, ordnet Azure alle DNS-Einträge der georeplizierten Region zu.
 
>[AZURE.NOTE]Auf diesen Prozess haben Sie keinen Einfluss, und er wird nur bei einem regionsweiten Ausfall durchgeführt. Setzen Sie daher auch andere anwendungsspezifische Sicherungsstrategien ein, um eine möglichst hohe Verfügbarkeit zu erreichen. Weitere Informationen finden Sie im Abschnitt zu [Datenstrategien für die Notfallwiederherstellung](https://aka.ms/drtechguide#DSDR).

In den folgenden Informationen für virtuelle Azure-Computer erfahren Sie, welche Möglichkeiten Sie haben, wenn die gesamte Region, in der Ihre virtuellen Azure-Computer bereitgestellt sind, von einer Dienstunterbrechung betroffen ist.

##Option 1: Warten auf die Wiederherstellung 
In diesem Fall ist keine weitere Aktion erforderlich. Wir arbeiten intensiv daran, die Verfügbarkeit des Diensts wiederherzustellen. Der aktuelle Dienststatus wird auf dem [Dashboard zur Azure-Dienstintegrität](https://azure.microsoft.com/status/) angezeigt.

>[AZURE.NOTE]Dies ist die beste Option, wenn Sie vor der Unterbrechung nicht ASR (Azure Site Recovery), Virtual Machines Backup, RA-GRS (georedundanter Speicher mit Lesezugriff) oder GRS (georedundanter Speicher) eingerichtet haben. Falls Sie für das Speicherkonto mit den virtuellen Festplatten Ihrer virtuellen Computer GRS (oder RA-GRS) eingerichtet haben, haben Sie die Möglichkeit, die virtuelle Festplatte mit dem Basisimage wiederherzustellen und zu versuchen, auf dessen Grundlage einen neuen virtuellen Computer bereitzustellen. Dies wird jedoch nicht empfohlen, da die Synchronisierung der Daten nur bei Verwendung von Azure Virtual Machines Backup oder ASR zuverlässig funktioniert.

Für Kunden, die umgehend Zugriff auf virtuelle Computer benötigen, stehen die beiden folgenden Optionen zur Verfügung.

>[AZURE.NOTE]Bei beiden Optionen besteht die Gefahr von Datenverlusten.

##Option 2: Wiederherstellen eines virtuellen Computers auf der Grundlage einer Sicherung 
Kunden mit konfiguriertem Virtual Machines Backup können den virtuellen Computer auf der Grundlage einer Sicherung und eines Wiederherstellungspunkts wiederherstellen.

Führen Sie die unter „Wiederherstellen virtueller Computer in Azure“ beschriebenen Schritte aus, um einen neuen virtuellen Computer auf der Grundlage einer Azure-Sicherung wiederherzustellen.

Informationen zum Planen der Infrastruktur für Azure Virtual Machines Backup finden Sie im Artikel [Planen der Sicherungsinfrastruktur für virtuelle Computer in Azure](../backup/backup-azure-vms-introduction.md).

##Option 3: Initiieren eines Failovers mit Azure Site Recovery (ASR) 
Wenn Sie Azure Site Recovery für Ihre betroffenen virtuellen Azure-Computer konfiguriert haben, können Sie Ihre virtuellen Computer auf der Grundlage ihrer Replikate wiederherstellen. Diese Replikate können sich entweder in Azure befinden oder sogar lokal vorhanden sein. In diesem Fall können Sie auf der Grundlage des vorhandenen Replikats einen neuen virtuellen Computer erstellen. Führen Sie die unter [Migrieren von virtuellen Azure-IaaS-Computern zwischen Azure-Regionen mit Azure Site Recovery](../site-recovery/site-recovery-migrate-azure-to-azure.md) beschriebenen Schritte aus, um Ihre virtuellen Computer auf der Grundlage eines Azure Site Recovery-Replikats wiederherzustellen.

>[AZURE.NOTE]Betriebssystem und Datenträger virtueller Azure-Computer werden auf eine sekundäre virtuelle Festplatte (Virtual Hard Drive, VHD) repliziert. Wenn sich diese jedoch in einem GRS- oder RA-GRS-Speicherkonto befinden, wird jede VHD unabhängig repliziert, wodurch keine übergreifende Konsistenz für die replizierten VHDs gewährleistet ist. Wenn Ihre Anwendung und/oder Ihre Datenbanken, die diese Datenträger verwenden, voneinander abhängig sind, ist nicht gewährleistet, dass alle virtuellen Festplatten als einzelne Momentaufnahme repliziert werden. Außerdem ist nicht sicher, dass das VHD-Replikat von GRS oder RA-GRS eine anwendungskonsistente Momentaufnahme zum Starten des virtuellen Computers ergibt.

##Referenzen 
[Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](https://aka.ms/drtechguide)

[Azure-Geschäftskontinuität – Technische Anleitung](http://aka.ms/bctechguide)

[Sichern von virtuellen Azure-Computern](../backup/backup-azure-vms.md)

[Azure Site Recovery](https://azure.microsoft.com/documentation/learning-paths/site-recovery/)

Falls die Anweisungen nicht klar sein sollten oder Microsoft die Schritte für Sie ausführen soll, wenden Sie sich an den [Kundensupport](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

<!---HONumber=AcomDC_0525_2016-->