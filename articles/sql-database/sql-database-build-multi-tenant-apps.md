---
title: Azure SQL-Datenbank erstellt mehrinstanzenfähige Apps mit Isolation und Effizienz
description: Erfahren Sie, wie Sie mithilfe von Azure SQL-Datenbank mehrinstanzenfähige Apps entwickeln können.
keywords: ''
services: sql-database
documentationcenter: ''
author: CarlRabeler
manager: jhubbard
editor: ''

ms.service: sql-database
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 07/19/2016
ms.author: carlrab

---
# Erstellt mehrinstanzenfähige Apps mit Azure SQL-Datenbank mit Isolation und Effizienz
## Nutzen elastischer Pools und Entwickeln von effizienteren mehrinstanzenfähigen Apps
Wenn Sie als SaaS-Entwickler eine mehrinstanzenfähige App für eine Vielzahl von Kunden erstellen, gehen Sie häufig Kompromisse im Hinblick auf Leistung, Verwaltung und Sicherheit ein. Mit den Pools für elastische Datenbanken von Azure SQL-Datenbank gehören diese Kompromisse der Vergangenheit an. Die Pools unterstützen Sie bei der Verwaltung und Überwachung mehrinstanzenfähiger Apps und ermöglichen es Ihnen, von der Isolation eines Kunden pro Datenbank zu profitieren. Informationen finden Sie unter [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) (Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank).

![build-multi-tenant-apps](./media/sql-database-build-multi-tenant-apps/sql-database-build-multi-tenant-apps.png)

## Automatische Skalierung, von Ihnen gesteuert
Pools führen basierend auf dem aktuellen Bedarf eine automatische Skalierung von Leistung und Speicherkapazität für elastische Datenbanken durch. Sie können die Leistung eines Pools steuern, elastische Datenbanken nach Bedarf hinzufügen oder entfernen und die Leistung elastischer Datenbanken definieren, ohne dass sich dies auf die Gesamtkosten des Pools auswirkt. Dies bedeutet, dass Sie sich keine Gedanken über die Verwaltung einzelner Datenbanken machen müssen.

[Dokumentation lesen](sql-database-elastic-pool.md)

## Intelligente Verwaltung Ihrer Umgebung
Integrierte Empfehlungen sorgen für eine proaktive Identifizierung von Datenbanken, die von Pools profitieren würden. Diese Empfehlungen ermöglichen Was-wäre-wenn-Analysen für eine schnelle Optimierung zum Erreichen Ihrer Leistungsziele. Leistungsstarke Dashboards zur Leistungsüberwachung und Problembehandlung unterstützen Sie dabei, die Poolnutzung im zeitlichen Verlauf zu visualisieren.

[Dokumentation lesen](sql-database-elastic-pool-guidance.md)

## Auf Ihre Anforderungen abgestimmte Leistung und Preise
Basic-, Standard- und Premium-Pools bieten eine große Auswahl an Leistungs-, Speicher- und Preisoptionen. Pools können bis zu 400 elastische Datenbanken enthalten. Elastische Datenbanken können automatisch auf bis zu 1.000 Transaktionseinheiten für elastische Datenbanken (eDTUs) skaliert werden.

[Dokumentation lesen](https://azure.microsoft.com/pricing/details/sql-database/?b=16.50)

## Elastische Tools
Zusätzlich zu den elastischen Pools stehen Features von SQL-Datenbank zur Verfügung, die Sie bei der Verwaltung operativer Aktivitäten über mehrere Datenbanken hinweg unterstützen:

** Ausführen Datenbanken übergreifender Abfragen und Berichterstattung. ** [Abfragen elastischer Datenbanken](sql-database-elastic-query-overview.md) ermöglichen Ihnen, Abfragen oder Berichte über die Datenbanken in Ihrem elastischen Pool hinweg auszuführen und in einem Arbeitsschritt auf Remotedaten zuzugreifen, die in verschiedenen Datenbanken Ihres Pools gespeichert sind.

** Datenbanktransaktionen übergreifende Ausführung. ** [Transaktionen für elastische Datenbanken](sql-database-elastic-transactions-overview.md) ermöglichen Ihnen, Transaktionen und Vorgänge über verschiedene Datenbanken in SQL-Datenbanken hinweg auszuführen (z.B. bei der Verarbeitung von Datenbanken übergreifenden Finanztransaktionen oder beim Aktualisieren des Bestands in einer Datenbank und in Bestellungen).

** Ausführen der gleichen Vorgänge für mehrere Datenbanken. ** [Aufträge für elastische Datenbanken](sql-database-elastic-jobs-overview.md) ermöglichen Ihnen das Ausführen administrativer Vorgänge, z.B. das Neuerstellen von Indizes oder eine Schemaaktualisierung für jede Datenbank in Ihrem elastischen Pool.

Wechseln Sie zur Startseite, um zu erfahren, welche weiteren Features SQL-Datenbank zu bieten hat. [Weitere Informationen](https://azure.microsoft.com/services/sql-database/)

## Nächste Schritte
Erhalten Sie ein [kostenloses Azure-Abonnement](https://azure.microsoft.com/get-started/) und [erstellen Sie Ihre erste Azure SQL-Datenbank](sql-database-get-started.md).

## Zusätzliche Ressourcen
Entdecken Sie alle [Funktionen von SQL-Datenbank](https://azure.microsoft.com/services/sql-database/).

Lesen Sie die [technische Übersicht über SQL-Datenbank](sql-database-technical-overview.md).

<!---HONumber=AcomDC_0803_2016-->