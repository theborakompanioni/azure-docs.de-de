---
title: "Notfallwiederherstellungsszenarien für Azure-VMs | Microsoft Docs"
description: Hier erhalten Sie Informationen zur Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Computer.
services: virtual-machines
documentationcenter: 
author: kmouss
manager: timlt
editor: 
ms.assetid: 65272148-ff06-4bce-91f1-851d706d4d40
ms.service: virtual-machines
ms.workload: virtual-machines
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: kmouss;aglick
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: fb986a41e33501ee71c93a48457ac4114e33c671
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="what-to-do-in-the-event-that-an-azure-service-disruption-impacts-azure-vms"></a>Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf virtuelle Azure-Computer
Bei Microsoft setzen wir uns mit großem Engagement dafür ein, dass unsere Dienste immer verfügbar sind, wenn Sie sie benötigen. Aufgrund von höherer Gewalt können jedoch gelegentlich ungeplante Dienstausfälle auftreten.

Microsoft stellt für seine Dienste Vereinbarungen zum Servicelevel (Service Level Agreements, SLAs) bereit, um dem Engagement für Verfügbarkeit und Konnektivität Nachdruck zu verleihen. Die SLAs für einzelne Azure-Dienste finden Sie unter [Vereinbarungen zum Servicelevel (SLAs)](https://azure.microsoft.com/support/legal/sla/).

Azure weist bereits viele integrierte Plattformfunktionen auf, die hoch verfügbare Anwendungen unterstützen. Weitere Informationen zu diesen Diensten finden Sie unter [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Dieser Artikel behandelt ein echtes Szenario der Notfallwiederherstellung für den Fall, dass eine ganze Region aufgrund einer Naturkatastrophe oder einer umfangreichen Dienstunterbrechung von einem Ausfall betroffen ist. So etwas kommt zwar äußerst selten vor, dennoch müssen Sie für den Ausfall einer gesamten Region vorbereitet sein. Falls eine ganze Region von einer Dienstunterbrechung betroffen ist, sind die lokal redundanten Kopien Ihrer Daten vorübergehend nicht verfügbar. Bei aktivierter Georeplikation sind drei zusätzliche Kopien Ihrer Azure Storage-Blobs und -Tabellen in einer anderen Region gespeichert. Sollte eine gesamte Region ausfallen oder die primäre Region aufgrund einer Katastrophe nicht wiederherstellbar sein, ordnet Azure alle DNS-Einträge der georeplizierten Region zu.

In den folgenden Informationen zu virtuellen Azure-Computern erfahren Sie, welche Möglichkeiten Sie haben, wenn die gesamte Region, in der Ihre virtuelle Azure-Computeranwendung bereitgestellt wurde, von einer Dienstunterbrechung betroffen ist.

## <a name="option-1-initiate-a-failover-by-using-azure-site-recovery"></a>Option 1: Initiieren eines Failovers mit Azure Site Recovery
Sie können Azure Site Recovery für Ihre virtuellen Computer konfigurieren, damit Sie Ihre Anwendung mit einem einfachen Mausklick innerhalb weniger Minuten wiederherstellen können. Sie können in die Azure-Region Ihrer Wahl replizieren und sind nicht auf gekoppelte Regionen beschränkt. Als ersten Schritt können Sie [Ihre virtuellen Computer replizieren](https://aka.ms/a2a-getting-started). Sie können [einen Wiederherstellungsplan erstellen](../site-recovery/site-recovery-create-recovery-plans.md), damit Sie den gesamten Failovervorgang für Ihre Anwendung automatisieren können. Sie können [Ihre Failover vorher testen](../site-recovery/site-recovery-test-failover-to-azure.md), ohne die Produktionsanwendung oder die fortlaufende Replikation zu beeinträchtigen. Im Fall einer Unterbrechung der primären Region [initiieren Sie einfach ein Failover](../site-recovery/site-recovery-failover.md) und verschieben Ihre Anwendung in die Zielregion.


## <a name="option-2-wait-for-recovery"></a>Option 2: Warten auf die Wiederherstellung
In diesem Fall ist keine weitere Aktion erforderlich. Wir arbeiten intensiv daran, die Verfügbarkeit des Diensts wiederherzustellen. Sie können den aktuellen Dienststatus auf unserem [Dashboard zur Azure-Dienstintegrität](https://azure.microsoft.com/status/)anzeigen.

Dies ist die beste Option, wenn Sie vor der Unterbrechung nicht Azure Site Recovery, georedundanten Speicher mit Lesezugriff oder georedundanten Speicher eingerichtet haben. Wenn Sie den georedundanten Speicher oder den georedundanten Speicher mit Lesezugriff für das Speicherkonto eingerichtet haben, unter dem die virtuellen Festplatten (VHDs) der VM gespeichert sind, können Sie versuchen, die Festplatte mit dem Basisimage wiederherzustellen und darüber eine neue VM bereitzustellen. Dabei handelt es sich nicht um die bevorzugte Option, da die Synchronisierung der Daten nicht gewährleistet werden kann. Daher kann nicht garantiert werden, dass diese Option funktioniert.


> [!NOTE]
> Denken Sie daran, dass Sie keine Kontrolle über diesen Prozess haben. Er tritt nur bei einer regionsweiten Dienstunterbrechung auf. Setzen Sie daher auch auf andere anwendungsspezifische Sicherungsstrategien, um eine möglichst hohe Verfügbarkeit zu erreichen. Weitere Informationen finden Sie im Abschnitt zu [Datenstrategien für die Notfallwiederherstellung](https://docs.microsoft.com/azure/architecture/resiliency/disaster-recovery-azure-applications#data-strategies-for-disaster-recovery).
>
>

## <a name="next-steps"></a>Nächste Schritte

- Beginnen Sie damit, mithilfe von Azure Site Recovery [Ihre Anwendungen auf virtuellen Azure-Computern zu schützen](https://aka.ms/a2a-getting-started).

- Weitere Informationen zur Implementierung einer Strategie für Notfallwiederherstellung und hohe Verfügbarkeit finden Sie unter [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

- Im [technischen Leitfaden zur Resilienz in Azure](../resiliency/resiliency-technical-guidance.md)können Sie detaillierte technische Kenntnisse hinsichtlich der Funktionen einer Cloudplattform erwerben.


- Wenden Sie sich an den [Kundensupport](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade), falls die Anweisungen nicht klar sein sollten oder Microsoft die Schritte für Sie ausführen soll.

