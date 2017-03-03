---
title: "Geschäftskontinuität und Notfallwiederherstellung: Azure Regionspaare | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Azure-Regionspaare sicherstellen, dass Anwendungen auch bei Rechenzentrumsausfällen stabil ausgeführt werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: c2d0a21c-2564-4d42-991a-bc31723f61a4
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 43e0bb0c921be3992321d0a202fce1fa09053f99
ms.openlocfilehash: 67edd136737e6af372c3c898c34a8bc6681a8c6d
ms.lasthandoff: 02/21/2017

---

# <a name="business-continuity-and-disaster-recovery-bcdr-azure-paired-regions"></a>Geschäftskontinuität und Notfallwiederherstellung: Azure-Regionspaare

## <a name="what-are-paired-regions"></a>Was sind Regionspaare?

Azure betreibt Datencenter in mehreren Gebieten auf der ganzen Erde. Bei einem Azure-Gebiet handelt es sich um einen definierten Bereich der Erde, der mindestens eine Azure-Region enthält. Eine Azure-Region ist ein Bereich in einem Gebiet mit mindestens einem Datencenter.

Jeder Azure-Region ist innerhalb des gleichen geografischen Gebiets eine andere Region als Regionspartner zugeordnet. Einzige Ausnahme ist „Brasilien, Süden“: Dieser Region ist eine Region außerhalb des geografischen Gebiets zugeordnet.

![Azure-Gebiet](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

Abbildung 1 – Diagramm von Azure-Regionspaaren

| Gebiet | Regionspaare |  |
|:--- |:--- |:--- |
| Nordamerika |USA (Mitte/Norden) |USA (Mitte/Süden) |
| Nordamerika |USA (Ost) |USA (West) |
| Nordamerika |USA (Ost 2) |USA, Mitte |
| Nordamerika |USA, Westen 2 |USA, Westen-Mitte |
| Europa |Nordeuropa |Westeuropa |
| Asien |Südostasien |Ostasien |
| China |Ostchina |Nordchina |
| Japan |Japan Ost |Japan (Westen) |
| Brasilien |Brasilien, Süden (1) |USA (Mitte/Süden) |
| Australien |Australien (Osten) |Australien (Südosten) |
| US Government |US Government, Iowa |US Government, Virginia |
| Indien |Indien (Mitte) |Indien, Süden |
| Kanada |Kanada, Mitte |Kanada, Osten |
| UK |UK, Westen |UK, Süden |

Tabelle 1: Übersicht über Azure-Regionspaare

> (1) Brasilien, Süden ist besonders, da dieses Gebiet ein Paar mit einer Region außerhalb des eigenen Gebiets bildet. „USA, Süden-Mitte“ fungiert als sekundäre Region von „Brasilien, Süden“, „Brasilien, Süden“ aber nicht als sekundäre Region von „USA, Süden-Mitte“.


Wir empfehlen das Replizieren von Workloads zwischen Regionalpaaren, um von Richtlinien für Isolierung und Verfügbarkeit von Azure zu profitieren. Beispielsweise werden geplante Azure-Systemupdates in Regionspaaren sequenziell (nicht gleichzeitig) bereitgestellt. Das heißt, dass selbst im seltenen Fall eines fehlerhaften Updates beide Regionen nicht gleichzeitig betroffen sind. Darüber hinaus wird im unwahrscheinlichen Fall eines umfassenden Ausfalls die Wiederherstellung mindestens einer Region aus jedem Paar priorisiert.

## <a name="an-example-of-paired-regions"></a>Beispiel für ein Regionspaar
Abbildung 2 zeigt eine hypothetische Anwendung, die das Regionspaar für die Notfallwiederherstellung verwendet. Die grünen Zahlen markieren die regionsübergreifenden Aktivitäten von drei Azure-Diensten (Azure Compute, Storage und Database) und ihre Konfiguration für eine regionsübergreifende Replikation. Die eindeutigen Vorteile einer Bereitstellung in Regionspaaren werden von den orangefarbenen Zahlen hervorgehoben.

![Übersicht über die Vorteile von Regionspaaren](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

Abbildung 2 – Hypothetisches Azure-Regionspaar

## <a name="cross-region-activities"></a>Regionsübergreifende Aktivitäten
Wie in Abbildung 2 dargestellt.

![PaaS](./media/best-practices-availability-paired-regions/1Green.png) **Azure Compute (PaaS)** – Sie müssen zusätzliche Serverressourcen im Voraus bereitstellen, um sicherzustellen, dass Ressourcen während eines Notfalls in einer anderen Region zur Verfügung stehen. Weitere Informationen finden Sie unter [Technischer Leitfaden zur Resilienz in Azure](resiliency/resiliency-technical-guidance.md).

![Storage](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** – georedundanter Speicher (GRS) wird beim Erstellen eines Azure Storage-Kontos standardmäßig konfiguriert. Mithilfe von GRS werden Ihre Daten dreimal in der primären Region und dreimal im Regionspaar automatisch repliziert. Weitere Informationen finden Sie unter [Redundanzoptionen für Azure Storage](storage/storage-redundancy.md).

![Azure SQL](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL-Datenbanken** – mithilfe der standardmäßigen Azure SQL-Georeplikation können Sie eine asynchrone Replikation von Transaktionen in einem Regionspaar konfigurieren. Bei Wahl der Option „Premium“ für die Georeplikation können Sie die Replikation in jede Region der Welt konfigurieren. Allerdings wird empfohlen, dass Sie diese Ressourcen für die meisten Notfallwiederherstellungsszenarien in einem Regionspaar bereitstellen. Weitere Informationen finden Sie unter [Georeplikation in Azure SQL-Datenbank](sql-database/sql-database-geo-replication-overview.md).

![Resource Manager](./media/best-practices-availability-paired-regions/4Green.png) **Azure Resource Manager (ARM)** – ARM bietet grundsätzlich eine regionsübergreifende logische Isolierung von Dienstverwaltungskomponenten. Dies bedeutet, dass sich logische Fehler in einer Region weniger wahrscheinlich auf eine andere auswirken.

## <a name="benefits-of-paired-regions"></a>Vorteile eines Regionspaars
Wie in Abbildung 2 dargestellt.  

![Isolierung](./media/best-practices-availability-paired-regions/5Orange.png)
**Physische Isolierung** – zwischen Azure-Rechenzentren in einem Regionspaar sollte nach Möglichkeit eine Entfernung von mindestens 480 km bestehen, was allerdings nicht in allen Gebieten zweckmäßig oder möglich ist. Durch die Trennung physischer Datencenter wird die Wahrscheinlichkeit einer gleichzeitigen Beeinträchtigung beider Regionen durch Naturkatastrophen, politische Unruhen, Stromausfälle oder physische Netzwerkausfälle verringert. Die Isolierung unterliegt den Einschränkungen des jeweiligen Gebiets (Größe, Verfügbarkeit der Energieversorgungs-/Netzwerkinfrastruktur, Vorschriften usw.).  

![Replikation](./media/best-practices-availability-paired-regions/6Orange.png)
**Von der Plattform bereitgestellte Replikation** – einige Dienste, wie z.B. georedundanter Speicher, bieten eine automatische Replikation in das Regionspaar.

![Wiederherstellung](./media/best-practices-availability-paired-regions/7Orange.png)
**Reihenfolge der Regionswiederherstellung** – im Falle eines umfassenden Ausfalls hat bei jedem Paar die Wiederherstellung einer der Regionen Priorität. Für Anwendungen, die in Regionspaaren bereitgestellt sind, wird die priorisierte Wiederherstellung einer der Regionen garantiert. Wenn eine Anwendung in Regionen bereitgestellt ist, die kein Paar bilden, kann sich die Wiederherstellung verzögern. Im schlimmsten Fall werden die gewählten Regionen ggf. zuletzt wiederhergestellt.

![Updates](./media/best-practices-availability-paired-regions/8Orange.png)
**Sequenzielle Updates** – geplante Azure-Systemupdates werden zur Minimierung von Ausfallzeit, der Auswirkungen von Fehlern und logischen Ausfällen im seltenen Fall eines fehlerhaften Updates sequenziell (nicht gleichzeitig) eingespielt.

![Daten](./media/best-practices-availability-paired-regions/9Orange.png)
**Speicherort von Daten** – eine Region befindet sich innerhalb des gleichen Gebiets wie ihr Paar (mit Ausnahme von Brasilien, Süden), um steuerliche und rechtliche Anforderungen an den Speicherort von Daten zu erfüllen.

