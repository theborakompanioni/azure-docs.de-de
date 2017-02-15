---
title: Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure-Clouddienste | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie im Fall einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure-Clouddienste vorgehen.
services: cloud-services
documentationcenter: 
author: kmouss
manager: drewm
editor: 
ms.assetid: e52634ab-003d-4f1e-85fa-794f6cd12ce4
ms.service: cloud-services
ms.workload: cloud-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2016
ms.author: kmouss;aglick
translationtype: Human Translation
ms.sourcegitcommit: e7d3c82e235d691c4ab329be3b168dcccc19774f
ms.openlocfilehash: 5db1864f5fb04b30a7b8ce59932e826fcef792f0


---
# <a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure-Clouddienste
Bei Microsoft setzen wir uns mit großen Engagement dafür ein, dass unsere Dienste immer verfügbar sind, wenn Sie sie benötigen. Aufgrund von höherer Gewalt können jedoch gelegentlich ungeplante Dienstausfälle auftreten.

Microsoft stellt für seine Dienste Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) bereit, um dem Engagement für Verfügbarkeit und Konnektivität Nachdruck zu verleihen. Die SLAs für einzelne Azure-Dienste finden Sie unter [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/).

Azure weist bereits viele integrierte Plattformfunktionen auf, die hoch verfügbare Anwendungen unterstützen. Weitere Informationen zu diesen Diensten finden Sie unter [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Dieser Artikel behandelt ein echtes Szenario der Notfallwiederherstellung für den Fall, dass eine ganze Region aufgrund einer Naturkatastrophe oder einer umfangreichen Dienstunterbrechung von einem Ausfall betroffen ist. So etwas kommt zwar äußerst selten vor, dennoch müssen Sie für den Ausfall einer gesamten Region vorbereitet sein. Falls eine ganze Region von einer Dienstunterbrechung betroffen ist, sind die lokal redundanten Kopien Ihrer Daten vorübergehend nicht verfügbar. Bei aktivierter Georeplikation sind drei zusätzliche Kopien Ihrer Azure Storage-Blobs und -Tabellen in einer anderen Region gespeichert. Sollte eine gesamte Region ausfallen oder die primäre Region aufgrund einer Katastrophe nicht wiederherstellbar sein, ordnet Azure alle DNS-Einträge der georeplizierten Region neu zu.

> [!NOTE]
> Beachten Sie, dass Sie keine Kontrolle über diesen Prozess haben. Er tritt nur bei rechenzentrumsweiten Dienstunterbrechungen auf. Setzen Sie daher auch auf andere anwendungsspezifische Sicherungsstrategien, um eine möglichst hohe Verfügbarkeit zu erreichen. Weitere Informationen finden Sie unter [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md). Wenn Sie das Failover selbst beeinflussen möchten, können Sie beispielsweise [georedundanten Speicher mit Lesezugriff](../storage/storage-redundancy.md#read-access-geo-redundant-storage) verwenden, um eine schreibgeschützte Kopie Ihrer Daten in einer anderen Region zu erstellen.
>
>

In den folgenden Informationen für virtuelle Azure-Computer erfahren Sie, welche Möglichkeiten Sie haben, wenn die gesamte Region, in der Ihre virtuelle Azure-Computeranwendung bereitgestellt ist, von einer Dienstunterbrechung betroffen ist.

## <a name="option-1-wait-for-recovery"></a>Option 1: Warten auf die Wiederherstellung
In diesem Fall ist keine weitere Aktion erforderlich. Die Azure-Teams arbeiten intensiv daran, die Verfügbarkeit des Diensts wiederherzustellen. Sie können den aktuellen Dienststatus auf unserem [Dashboard zur Azure-Dienstintegrität](https://azure.microsoft.com/status/)anzeigen.

> [!NOTE]
> Dies ist die beste Option, wenn der Kunde Azure Site Recovery nicht eingerichtet hat oder über eine sekundäre Bereitstellung in einer anderen Region verfügt.
>
>

Für Kunden, die umgehend Zugriff auf ihre bereitgestellten Clouddienste wünschen, stehen die folgenden Optionen zur Verfügung.

> [!NOTE]
> Beachten Sie ,dass bei diesen Optionen die Gefahr von Datenverlusten besteht.     
>
>

## <a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Option 2: Erneutes Bereitstellen Ihrer Clouddienstkonfiguration in einer neuen Region
Wenn Sie über den ursprünglichen Code verfügen, können Sie einfach die Anwendung, die zugehörige Konfiguration und die zugehörigen Ressourcen in einem neuen Clouddienst in einer neuen Region bereitstellen.  

Ausführlichere Informationen zum Erstellen und Bereitstellen einer Clouddienstanwendung finden Sie unter [Erstellen und Bereitstellen eines Clouddiensts](cloud-services-how-to-create-deploy-portal.md).

Abhängig von Ihren Anwendungsdatenquellen müssen Sie möglicherweise die Wiederherstellungsvorgänge für Ihre Anwendungsdatenquelle überprüfen.

* Informationen zu Azure Storage-Datenquellen finden Sie unter [Azure Storage-Replikation](../storage/storage-redundancy.md#read-access-geo-redundant-storage). Dort können Sie basierend auf dem ausgewählten Replikationsmodell für Ihre Anwendung prüfen, welche Optionen Ihnen zur Verfügung stehen.
* Informationen zu SQL-Datenbankquellen finden Sie unter [Übersicht: Geschäftskontinuität für die Cloud und Notfallwiederherstellung für Datenbanken mit SQL-Datenbank](../sql-database/sql-database-business-continuity.md). Dort können Sie basierend auf dem ausgewählten Replikationsmodell für Ihre Anwendung prüfen, welche Optionen Ihnen zur Verfügung stehen.

## <a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Option 3: Verwenden einer Sicherungsbereitstellung über Azure Traffic Manager
Bei dieser Option wird davon ausgegangen, dass Sie bereits beim Entwurf Ihrer Anwendungslösung die regionale Notfallwiederherstellung beachtet haben. Sie können diese Option verwenden, wenn Sie bereits eine sekundäre Clouddienst-Anwendungsbereitstellung in einer anderen Region ausführen und diese über einen Traffic Manager-Kanal verbunden ist. Überprüfen Sie in diesem Fall die Integrität der sekundären Bereitstellung. Wenn diese fehlerfrei ist, können Sie den Datenverkehr über Azure Traffic Manager dorthin umleiten. Mit dieser Strategie können Sie von den Methoden für das Datenverkehrsrouting und den Konfigurationen der Failoverreihenfolge in Azure Traffic Manager profitieren. Weitere Informationen finden Sie unter [Was ist Traffic Manager?](../traffic-manager/traffic-manager-overview.md).

![Lastenausgleich von Azure Cloud Services zwischen Regionen mit Azure Traffic Manager](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Implementierung einer Strategie für Notfallwiederherstellung und hohe Verfügbarkeit finden Sie unter [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Im [technischen Leitfaden zur Resilienz in Azure](../resiliency/resiliency-technical-guidance.md)können Sie detaillierte technische Kenntnisse hinsichtlich der Funktionen einer Cloudplattform erwerben.

Falls die Anweisungen nicht klar sein sollten oder Microsoft die Schritte für Sie ausführen soll, wenden Sie sich an den [Kundensupport](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).



<!--HONumber=Nov16_HO3-->


