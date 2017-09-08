---
title: Azure SQL-Datenbank-Dienst | Microsoft-Dokumentation
description: Lernen Sie die Dienstebenen von SQL-Datenbank kennen.
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/20/2017
ms.author: carlrab
ms.translationtype: HT
ms.sourcegitcommit: 48dfc0fa4c9ad28c4c64c96ae2fc8a16cd63865c
ms.openlocfilehash: 0183e9c60da7c216a6268dfec3874eb76270f71d
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="what-are-azure-sql-database-service-tiers"></a>Was sind die Dienstebenen von SQL-Datenbank?

[Azure SQL-Datenbank](sql-database-technical-overview.md) bietet die Dienstebenen **Basic**, **Standard**, **Premium** und **Premium RS** sowohl für [einzelne Datenbanken](sql-database-single-database-resources.md) als auch [elastische Pools](sql-database-elastic-pool.md). Dienstebenen unterscheiden sich in erster Linie durch eine Reihe zur Wahl stehender Leistungsstufen und Speichergrößen sowie den Preis.  Alle Dienstebenen bieten Flexibilität beim Wechsel von Leistungsstufe und Speichergröße.  Einzelne Datenbanken und elastische Pools werden stündlich basierend auf Dienstebene, Leistungsstufe und Speichergröße abgerechnet.   

## <a name="choosing-a-service-tier"></a>Auswählen einer Dienstebene

Die Auswahl einer Dienstebene hängt in erster Linie von den Anforderungen an Geschäftskontinuität, Speicher und Leistung ab.
| | **Basic** | **Standard** |**Premium** |**Premium RS** |
| :-- | --: |--:| --:| --:| 
|Zielworkload|Entwicklung und Produktion|Entwicklung und Produktion|Entwicklung und Produktion|Workload, die Datenverlust aufgrund von Dienstfehlern bis zu 5 Minuten tolerieren kann|
|Betriebszeit-SLA|99,99 %|99,99 %|99,99 %|Während der Vorschau nicht zutreffend|
|Sicherungsaufbewahrung|7 Tage|35 Tage|35 Tage|35 Tage|
|CPU|Niedrig|Niedrig, Mittel, Hoch|Mittel, Hoch|Mittel|
|E/A-Durchsatz|Niedrig  | Mittel | Größenordnung höher als Standard|Identisch mit Premium|
|E/A-Latenz|Höher als Premium|Höher als Premium|Niedriger als Basic und Standard|Identisch mit Premium|
|Columnstore-Indizierung und In-Memory-OLTP|N/V|N/V|Unterstützt|Unterstützt|
|||||

## <a name="performance-level-and-storage-size-limits"></a>Einschränkungen von Leistungsstufe und Speichergröße

Leistungsstufen werden für Einzeldatenbanken als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) und für Pools für elastische Datenbanken als elastische Datenbanktransaktionseinheiten (elastic Database Transaction Units, eDTUs) bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind DTUs und eDTUs?](sql-database-what-is-a-dtu.md).

### <a name="single-databases"></a>Einzeldatenbanken

|  | **Basic** | **Standard** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße* | 2 GB | 1 TB | 4 TB  | 1 TB  |
| Maximale DTU-Anzahl | 5 | 3000 | 4000 | 1000 |
||||||

### <a name="elastic-pools"></a>Elastische Pools

| | **Basic** | **Standard** | **Premium** | **Premium RS**|
| :-- | --: | --: | --: | --: |
| Maximale Speichergröße pro Datenbank*  | 2 GB | 1 TB | 1 TB | 1 TB |
| Maximale Speichergröße pro Pool* | 156 GB | 4 TB | 4 TB | 1 TB |
| Maximale Anzahl von eDTUs pro Datenbank | 5 | 3000 | 4000 | 1000 |
| Maximale Anzahl von eDTUs pro Pool | 1600 | 3000 | 4000 | 1000 |
| Maximale Anzahl von Datenbanken pro Pool | 500  | 500 | 100 | 100 |
||||||

> [!IMPORTANT]
> \* Speichergrößen, die den integrierten Speicher überschreiten, befinden sich in der Vorschauphase und werden gegen Aufpreis bereitgestellt. Weitere Informationen finden Sie unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/). 
>
> \* Im Premium-Tarif ist eine Speicherkapazität von mehr als 1 TB in folgenden Regionen verfügbar: „USA, Osten 2“, „USA, Westen“, „USA Gov Virginia“, „Europa, Westen“, „Deutschland, Mitte“, „Asien, Südosten“, „Japan, Osten“, „Australien, Osten“, „Kanada, Mitte“ und „Kanada, Osten“. Siehe [Einzeldatenbank: Einschränkungen von P11 und P15 bei Maximalgrößen über 1 TB](sql-database-resource-limits.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  
> 

Ausführliche Informationen zu Auswahlmöglichkeiten bestimmter Leistungsstufen und Speichergrößen finden Sie unter [Ressourceneinschränkungen für Azure SQL-Datenbank](sql-database-resource-limits.md).


## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über das [Verwalten von Ressourcen für eine einzelne Datenbank in Azure SQL-Datenbank](sql-database-single-database-resources.md).
- Informationen zu elastischen Pools finden Sie unter [Elastische Pools als Hilfe beim Verwalten und Skalieren mehrerer SQL-Datenbanken](sql-database-elastic-pool.md).
- Erfahren Sie mehr über [Einschränkungen für Azure-Abonnements und -Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
* Informieren Sie sich über [Datenbanktransaktionseinheiten (DTUs) und elastische Datenbanktransaktionseinheiten (eDTUs)](sql-database-what-is-a-dtu.md).
* Informationen zur Überwachung der DTU-Nutzung finden Sie unter [Tipps zur Optimierung der SQL-Datenbankleistung](sql-database-troubleshoot-performance.md).


