---
title: "SQL-Datenbank – Automatische Optimierung | Microsoft-Dokumentation"
description: "SQL-Datenbank analysiert die SQL-Abfrage und führt eine automatische Anpassung an die Benutzerworkload durch."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/05/2017
ms.author: jovanpop
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: f5552793db2d89542782b7d9e8f996314f62e429
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="automatic-tuning"></a>Automatische Optimierung

Azure SQL-Datenbank ist ein vollständig verwalteter Datendienst zum Überwachen der Abfragen, die in der Datenbank ausgeführt werden, und kann die Leistung bei der Bewältigung der Workload automatisch verbessern. Azure SQL-Datenbank verfügt über ein integriertes Intelligence-Verfahren, mit dem die Leistung Ihrer Abfragen automatisch optimiert und verbessert werden kann, indem die Datenbank dynamisch an Ihre Workload angepasst wird. Die automatische Optimierung in Azure SQL-Datenbank ist unter Umständen eines der wichtigsten Features, das Sie in Azure SQL-Datenbank aktivieren können, um die Leistung Ihrer Abfragen zu optimieren.

Die erforderlichen Schritte für das Azure-Portal finden Sie im Artikel [Aktivieren der automatischen Optimierung](sql-database-automatic-tuning-enable.md).

## <a name="why-automatic-tuning"></a>Gründe für die automatische Optimierung

Die Hauptaufgabe bei der Verwaltung von klassischen Datenbanken ist das Überwachen der Workload, wobei kritische SQL-Abfragen, zur Steigerung der Leistung hinzuzufügende Indizes und selten verwendete Indizes identifiziert werden. Azure SQL-Datenbank bietet detaillierte Einblicke in die Abfragen und Indizes, die Sie überwachen sollten. Eine kontinuierliche Überwachung der Datenbank ist jedoch eine schwierige und aufwendige Aufgabe, insbesondere bei sehr vielen Datenbanken. Das effiziente Verwalten einer großen Anzahl von Datenbanken ist unter Umständen selbst mit allen verfügbaren Tools und Berichten, die von Azure SQL-Datenbank und vom Azure-Portal bereitgestellt werden, nicht möglich. Anstelle der manuellen Überwachung und Optimierung der Datenbank sollten Sie erwägen, einige der Überwachungs- und Optimierungsaktionen mithilfe des Features zur automatischen Optimierung an Azure SQL-Datenbank zu delegieren. 


> [!VIDEO https://channel9.msdn.com/Blogs/Azure-in-the-Enterprise/Enabling-Azure-SQL-Database-Auto-Tuning-at-Scale-for-Microsoft-IT/player]
>

## <a name="how-does-automatic-tuning-work"></a>Wie funktioniert die automatische Optimierung?

Azure SQL-Datenbank verfügt über einen fortlaufenden Prozess zum Überwachen und Analysieren der Leistung, bei dem die Merkmale Ihrer Workload ständig verfolgt und potenzielle Probleme und Verbesserungsmöglichkeiten identifiziert werden.

![Automatischer Optimierungsprozess](./media/sql-database-automatic-tuning/tuning-process.png)

Dieser Prozess ermöglicht Azure SQL-Datenbank eine dynamische Anpassung Ihrer Workload, indem ermittelt wird, welche Indizes und Pläne die Leistung Ihrer Workloads verbessern können und welche Indizes sich auf Ihre Workloads auswirken. Basierend auf diesen ermittelten Informationen werden bei der automatischen Optimierung Optimierungsaktionen angewendet, um die Leistung Ihrer Workload zu verbessern. Außerdem überwacht Azure SQL-Datenbank die Leistung fortlaufend nach allen Änderungen, die von der automatischen Optimierung vorgenommen wurden. So wird sichergestellt, dass die Leistung Ihrer Workload auch wirklich verbessert wird. Alle Aktionen, die nicht zu einer Leistungsverbesserung führen, werden automatisch rückgängig gemacht. Diese Überprüfung ist ein wichtiges Feature, mit dem dafür gesorgt wird, dass Änderungen, die von der automatischen Optimierung vorgenommen werden, für Ihre Workload keine Leistungsverschlechterung bewirken.

Es gibt zwei Aspekte der automatischen Optimierung, die in Azure SQL-Datenbank verfügbar sind:

 -  **Automatische Indexverwaltung:** Diese Funktion identifiziert Indizes, die der Datenbank hinzugefügt, und solche, die entfernt werden sollten.
 -  **Automatische Plankorrektur:** Diese Funktion erkennt problematische Pläne und korrigiert Leistungsprobleme für SQL-Pläne (in Kürze verfügbar, in SQL Server 2017 bereits enthalten).

## <a name="automatic-index-management"></a>Automatische Indexverwaltung

In Azure SQL-Datenbank ist die Indexverwaltung einfach, weil Azure SQL-Datenbank Informationen zu Ihrer Workload erhält und sicherstellt, dass Ihre Daten immer optimal indiziert sind. Das richtige Indexdesign ist von entscheidender Bedeutung für die optimale Leistung Ihrer Workload, und die automatische Indexverwaltung kann Ihnen bei der Optimierung Ihrer Indizes behilflich sein. Mit der automatischen Indexverwaltung können entweder Leistungsprobleme in falsch indizierten Datenbanken behoben werden, oder es können Indizes im vorhandenen Datenbankschema verwaltet und verbessert werden. 

### <a name="why-do-you-need-index-management"></a>Gründe für die Nutzung der Indexverwaltung

Mit Indizes werden einige Ihrer Abfragen beschleunigt, mit denen Daten aus Ihren Tabellen gelesen werden. Für die Abfragen, mit denen Daten aktualisiert werden, können sie aber eine Verlangsamung bewirken. Sie müssen sorgfältig analysieren, wann Sie einen Index erstellen und welche Spalten Sie in den Index einbinden müssen. Einige Indizes werden nach einiger Zeit unter Umständen nicht mehr benötigt. Aus diesem Grund müssen Sie die Indizes, die nicht zu Vorteilen führen, regelmäßig identifizieren und verwerfen. Wenn Sie die nicht genutzten Indizes ignorieren, verringert sich die Leistung der Abfragen, mit denen Daten aktualisiert werden, ohne dass sich Vorteile für die Abfragen zum Lesen von Daten ergeben. Nicht verwendete Indizes wirken sich außerdem auf die Gesamtleistung des Systems aus, da für zusätzliche Updates eine unnötige Protokollierung erforderlich ist.

Zur Ermittlung des optimalen Satzes mit Indizes, mit denen die Leistung der Abfragen zum Lesen von Daten aus Ihren Tabellen verbessert wird und die eine minimale Auswirkung auf Updates hat, ist ggf. eine fortlaufende und komplexe Analyse erforderlich.

Azure SQL-Datenbank verwendet integrierte Intelligence und erweiterte Regeln, mit denen Ihre Abfragen analysiert und die Indizes identifiziert werden, die für Ihre aktuellen Workloads optimal geeignet wären bzw. die ggf. entfernt werden können. Mit Azure SQL-Datenbank wird sichergestellt, dass Sie über einen erforderlichen Mindestsatz von Indizes verfügen, mit denen die Abfragen zum Lesen von Daten optimiert werden und sich nur geringe Auswirkungen auf andere Abfragen ergeben.

### <a name="how-to-identify-indexes-that-need-to-be-changed-in-your-database"></a>Wie können Indizes identifiziert werden, die in Ihrer Datenbank geändert werden müssen?

Mit Azure SQL-Datenbank ist der Prozess zum Verwalten von Indizes einfach. Anstelle des langwierigen Prozesses zum manuellen Analysieren von Workloads und Überwachen von Indizes führt Azure SQL-Datenbank eine Analyse Ihrer Workload durch, identifiziert die Abfragen, die mit einem neuen Index ggf. schneller ausgeführt werden können, und identifiziert nicht verwendete oder duplizierte Indizes. Weitere Informationen zur Identifikation von Indizes, die geändert werden sollten, finden Sie unter [Azure SQL Database Advisor im Azure-Portal](sql-database-advisor-portal.md).

### <a name="automatic-index-management-considerations"></a>Aspekte der automatischen Indexverwaltung

Wenn Sie herausfinden, dass die integrierten Regeln die Leistung Ihrer Datenbank verbessern, können Sie sich für die automatische Verwaltung Ihrer Indizes durch Azure SQL-Datenbank entscheiden.

Aktionen, die zum Erstellen von erforderlichen Indizes in Azure SQL-Datenbanken durchgeführt werden müssen, verbrauchen unter Umständen Ressourcen und wirken sich vorübergehend negativ auf die Workloadleistung aus. Zur Verringerung der Auswirkung von Indexerstellungen auf die Workloadleistung ermittelt Azure SQL-Datenbank für alle Vorgänge der Indexverwaltung das passende Zeitfenster. Ein Optimierungsvorgang wird verschoben, wenn die Datenbank Ressourcen zum Ausführen Ihrer Workload benötigt, und gestartet, wenn die Datenbank über genügend ungenutzte Ressourcen verfügt, die für die Wartungsaufgabe verwendet werden können. Ein wichtiges Feature der automatischen Indexverwaltung ist die Überprüfung der Aktionen. Wenn Azure SQL-Datenbank einen Index erstellt oder verwirft, analysiert ein Überwachungsprozess die Leistung Ihrer Workload, um zu überprüfen, ob die Leistung durch die Aktion verbessert wurde. Falls keine signifikante Verbesserung erkennbar ist, wird die Aktion sofort rückgängig gemacht. Auf diese Weise stellt Azure SQL-Datenbank sicher, dass sich automatische Aktionen nicht negativ auf die Leistung Ihrer Workload auswirken. Indizes, die mit der automatischen Optimierung erstellt werden, sind für den Wartungsvorgang des zugrunde liegenden Schemas transparent. Schemaänderungen, wie das Verwerfen oder Umbenennen von Spalten, werden durch das Vorhandensein von automatisch erstellten Indizes nicht blockiert. Von Azure SQL-Datenbank automatisch erstellte Indizes werden sofort verworfen, wenn dazugehörige Tabellen oder Spalten verworfen werden.

## <a name="automatic-plan-choice-correction"></a>Automatische Korrektur der Planauswahl

Die automatische Plankorrektur ist ein neues Feature der automatischen Optimierung in SQL Server 2017 CTP2.0, mit dem SQL-Pläne mit unerwünschtem Verhalten identifiziert werden. Diese automatische Optimierungsoption ist demnächst für Azure SQL-Datenbank verfügbar. Weitere Informationen finden Sie unter [Automatic tuning](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning) (Automatische Optimierung).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur Aktivierung der automatischen Optimierung in Azure SQL-Datenbank und zur Verwendung für die vollständige Verwaltung Ihrer Workload finden Sie unter [Aktivieren der automatischen Optimierung](sql-database-automatic-tuning-enable.md).
- Bei Verwendung der manuellen Optimierung helfen Ihnen die Informationen unter [Azure SQL Database Advisor im Azure-Portal](sql-database-advisor-portal.md) weiter, und Sie können die Empfehlungen manuell anwenden, die zu einer Verbesserung der Leistung Ihrer Abfragen führen.

