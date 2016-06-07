<properties
	pageTitle="Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Cloud Services | Microsoft Azure"
	description="Hier erhalten Sie Informationen zur Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Cloud Services."
	services="cloud-services"
	documentationCenter=""
	authors="kmouss"
	manager="drewm"
	editor=""/>

<tags
	ms.service="cloud-services"
	ms.workload="cloud-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/16/2016"
	ms.author="kmouss;aglick"/>

#Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Cloud Services

Bei Microsoft setzen wir uns mit großen Engagement dafür ein, dass unsere Dienste immer verfügbar sind, wenn Sie sie benötigen. Aufgrund höherer Gewalt können jedoch gelegentlich ungeplante Dienstausfälle auftreten.

Microsoft stellt für seine Dienste Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) bereit, um dem Engagement für Verfügbarkeit und Konnektivität Nachdruck zu verleihen. Die SLAs für individuelle Azure-Dienste finden Sie unter [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/).

Azure verfügt bereits über zahlreiche integrierte Plattformfeatures zur Unterstützung hochverfügbarer Anwendungen. Weitere Informationen zu diesen Diensten finden Sie unter [Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](https://aka.ms/drtechguide).

Dieses Dokument behandelt eine echte Notfallwiederherstellung für den Fall, dass eine ganze Region aufgrund einer Naturkatastrophe oder einer umfangreichen Dienstunterbrechung von einem Ausfall betroffen ist. So etwas kommt zwar äußerst selten vor, dennoch müssen Sie für den Ausfall der gesamten Region vorbereitet sein. Falls eine ganze Region von einer Dienstunterbrechung betroffen ist, sind die lokal redundanten Kopien Ihrer Daten vorübergehend nicht verfügbar. Bei aktivierter Georeplikation sind drei zusätzliche Kopien Ihrer Azure Storage-Blobs und Tabellen in einer anderen Region gespeichert. Sollte eine gesamte Region ausfallen oder die primäre Region aufgrund einer Katastrophe nicht wiederherstellbar sein, ordnet Azure alle DNS-Einträge der georeplizierten Region zu.
 
>[AZURE.NOTE]Beachten Sie, dass Sie keinerlei Einfluss auf diesen Prozess haben.Er wird nur bei einem Ausfall eines gesamten Datencenters durchgeführt. Setzen Sie daher auch auf andere anwendungsspezifische Sicherungsstrategien, um eine möglichst hohe Verfügbarkeit zu erreichen. Weitere Informationen finden Sie im Abschnitt zu [Datenstrategien für die Notfallwiederherstellung](https://aka.ms/drtechguide#DSDR) (in englischer Sprache). Wenn Sie Ihr eigenes Failover beeinflussen möchten, können Sie beispielsweise [Georedundanter Speicher mit Lesezugriff (RA-GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage) verwenden, um eine schreibgeschützte Kopie Ihrer Daten in einer andere Region zu erstellen.

In den folgenden Informationen für virtuelle Azure-Computer erfahren Sie, welche Möglichkeiten Sie haben, wenn die gesamte Region, in der Ihre virtuelle Azure-Computeranwendung bereitgestellt sind, von einer Dienstunterbrechung betroffen ist.

##Option 1: Warten auf die Wiederherstellung 
In diesem Fall ist keine weitere Aktion erforderlich. Die Azure-Teams arbeiten intensiv daran, die Verfügbarkeit des Diensts wiederherzustellen. Sie können den aktuellen Dienststatus auf unserem [Dashboard zur Azure-Dienstintegrität](https://azure.microsoft.com/status/) anzeigen.

>[AZURE.NOTE]Dies ist die beste Option, wenn der Kunde Azure Site Recovery (ASR) noch nicht eingerichtet hat oder über eine sekundäre Bereitstellung in einer anderen Region verfügt.

Für Kunden, die umgehend Zugriff auf ihre bereitgestellten Clouddienste wünschen, stehen die folgenden Optionen zur Verfügung.

>[AZURE.NOTE]Beachten Sie ,dass bei diesen Optionen die Gefahr von Datenverlusten besteht.

##Option 2: Erneutes Bereitstellen Ihrer Clouddienstkonfiguration in einer neuen Region 

Wenn Sie über den ursprünglichen Code verfügen, können Sie einfach die Anwendung, die zugehörige Konfiguration und die zugehörigen Ressourcen in einem neuen Clouddienst in einer neuen Region bereitstellen.

Ausführlichere Informationen zum Erstellen und Bereitstellen einer Clouddienstanwendung finden Sie unter [Erstellen und Bereitstellen eines Clouddiensts](./cloud-services-how-to-create-deploy-portal.md).

Abhängig von Ihren Anwendungsdatenquellen müssen Sie möglicherweise die Wiederherstellungsvorgänge für Ihre Anwendungsdatenquelle überprüfen.
  * Informationen zu Azure Storage-Datenquellen finden Sie unter [Azure Storage-Replikation](../storage/storage-redundancy.md#read-access-geo-redundant-storage). Damit können Sie die verfügbaren Optionen auf Grundlage des Replikationsmodells überprüfen, das für Ihre Anwendung ausgewählt wurde.
  * Informationen zu SQL-Datenbankquellen finden Sie unter [Übersicht: Geschäftskontinuität für die Cloud und Notfallwiederherstellung für Datenbanken mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md). Damit können Sie die verfügbaren Optionen auf Grundlage des Replikationsmodells überprüfen, das für Ihre Anwendung ausgewählt wurde. 

##Option 3: Verwenden einer Sicherungsbereitstellung über einen Traffic Manager 
Bei dieser Option wird davon ausgegangen, dass Sie bereits beim Entwurf Ihrer Anwendungslösung die regionale Notfallwiederherstellung beachtet haben. Diese Option kann verwendet werden, wenn Sie bereits eine sekundäre Clouddienst-Anwendungsbereitstellung in einer anderen Region ausführen und diese über Traffic Manager-Kanal verbunden ist. In diesem Fall überprüfen Sie die Integrität der sekundären Bereitstellung und wenn diese fehlerfrei ist, können Sie den Datenverkehr mithilfe von Azure Traffic Manager dahin umleiten und die Routingmethode für Datenverkehr sowie die Failoverreihenfolgen-Konfiguration in ATM nutzen. Weitere Informationen finden sie unter [Konfigurieren von Traffic Manager-Einstellungen](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Lastenausgleich von Azure Cloud Services zwischen Regionen mit Azure Traffic Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##Referenzen 

[Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](https://aka.ms/drtechguide)

[Azure-Geschäftskontinuität – Technische Anleitung](https://aka.ms/bctechguide)
 
Falls die Anweisungen nicht klar sein sollten oder Microsoft die Schritte für Sie ausführen soll, wenden Sie sich an den [Kundensupport](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

<!---HONumber=AcomDC_0525_2016-->