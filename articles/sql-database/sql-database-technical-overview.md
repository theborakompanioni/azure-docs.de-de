---
title: Worum handelt es sich beim Azure SQL-Datenbankdienst? | Microsoft Docs
description: 'Get an introduction to SQL Database: technical details and capabilities of Microsoft''s relational database management system (RDBMS) in the cloud.'
keywords: "Einführung in SQL, Was ist SQL-Datenbank?"
services: sql-database
documentationcenter: 
author: shontnew
manager: jhubbard
editor: cgronlun
ms.assetid: c561f600-a292-4e3b-b1d4-8ab89b81db48
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 03/17/2017
ms.author: shkurhek
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: a139b97e20685a5ced513c1fd180a74e91c497a7
ms.lasthandoff: 04/15/2017

---
# <a name="what-is-sql-database-introduction-to-sql-database"></a>Was ist SQL Database? Einführung in SQL-Datenbank
SQL-Datenbank ist ein relationaler Datenbankdienst in der Microsoft Cloud, der auf dem marktführenden Microsoft SQL Server-Modul basiert und unternehmenswichtige Workloads verarbeiten kann. SQL-Datenbank bietet vorhersagbare Leistung auf mehreren Dienstebenen, dynamische Skalierbarkeit ohne Ausfallzeiten, integrierte Geschäftskontinuität und Datenschutz – nahezu ohne Verwaltungsaufwand. Mit diesen Funktionen können Sie sich auf die schnelle Entwicklung von Apps und die Verkürzung des Zeitraums bis zur Markteinführung konzentrieren, anstatt wertvolle Zeit und Ressourcen für die Verwaltung von virtuellen Computern und der Infrastruktur aufwenden zu müssen. Da SQL-Datenbank auf dem [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx)-Modul basiert, unterstützt SQL-Datenbank vorhandene SQL Server-Tools, -Bibliotheken und -APIs. Es ist für Sie also einfach, neue Lösungen zu entwickeln und vorhandene SQL Server-Lösungen zu verschieben und in die Microsoft Cloud zu erweitern, ohne dass Sie hierfür neue Fähigkeiten erlernen müssen.

Dieser Artikel bietet eine Einführung in die wesentlichen Konzepte und Features von SQL-Datenbank im Hinblick auf Leistung, Skalierbarkeit und Verwaltbarkeit sowie Links zu detaillierten Informationen. Diese Schnellstarts erleichtern Ihnen den Einstieg:
 - [Create a SQL database in the Azure portal](sql-database-get-started-portal.md) (Erstellen einer SQL-Datenbank im Azure-Portal)  
 - [Erstellen einer SQL-Datenbank mit der Azure CLI](sql-database-get-started-cli.md)
 - [Erstellen einer SQL-Datenbank mithilfe von PowerShell](sql-database-get-started-powershell.md)

Azure CLI- und PowerShell-Beispiele finden Sie unter:
 - [Azure CLI-Beispiele für Azure SQL-Datenbank](sql-database-cli-samples.md)
 - [Azure PowerShell-Beispiele für Azure SQL-Datenbank](sql-database-powershell-samples.md)

## <a name="adjust-performance-and-scale-without-downtime"></a>Anpassen von Leistung und Skalierung ohne Ausfallzeiten
Der SQL-Datenbank-Dienst verfügt über vier Dienstebenen: Basic, Standard, Premium und Premium RS. Jede Dienstebene bietet [verschiedene Leistungsstufen und Funktionen](sql-database-service-tiers.md) zur Unterstützung geringer und hoher Datenbankworkloads. Sie können zu geringen monatlichen Kosten Ihre erste App in einer kleinen Datenbank erstellen und dann manuell oder programmgesteuert jederzeit die [Dienstebene (Tarif) ändern](sql-database-service-tiers.md), um die Anforderungen Ihrer Lösung zu erfüllen. Dies ist möglich, ohne dass es für die App oder für Ihre Kunden zu Ausfallzeiten kommt. Dank der dynamischen Skalierbarkeit kann Ihre Datenbank in transparenter Form auf sich schnell ändernde Ressourcenanforderungen reagieren, und Sie zahlen nur für die Ressourcen, die Sie jeweils benötigen.

## <a name="elastic-pools-to-maximize-resource-utilization"></a>Elastische Pools zum Maximieren der Ressourcenverwendung
Für viele Unternehmen und Apps genügt es, wenn Einzeldatenbanken erstellt werden können und sich die Leistung nach oben oder unten anpassen lässt – insbesondere, wenn die Nutzungsmuster relativ gut vorhersagbar sind. Bei unvorhersagbaren Nutzungsmustern kann es jedoch schwer sein, die Kosten und Ihr Geschäftsmodell zu verwalten. [Elastische Pools](sql-database-elastic-pool.md) sind dafür ausgelegt, dieses Problem zu beheben. Das Konzept ist denkbar einfach. Sie ordnen Leistungsressourcen nicht einer Einzeldatenbank, sondern einem Pool zu, sodass Sie für die gesamten Leistungsressourcen des Pools und nicht für die Leistung einer Einzeldatenbank zahlen. Bei elastischen Pools müssen Sie sich nicht darauf konzentrieren, die Datenbankleistung nach oben oder unten anzupassen, wenn der Ressourcenbedarf schwankt. Die im Pool zusammengefassten Datenbanken nutzen die Leistungsressourcen des elastischen Pools je nach Bedarf. In einem Pool zusammengefasste Datenbanken nutzen die Grenzwerte des Pools, überschreiten sie aber nicht. Ihre Kosten bleiben also vorhersagbar, auch wenn sich die Nutzung der einzelnen Datenbanken nicht prognostizieren lässt. Darüber hinaus können Sie [Datenbanken im Pool hinzufügen und entfernen](sql-database-elastic-pool-manage-portal.md)und so Ihre App von einigen wenigen auf Tausende Datenbanken skalieren, und das in einem kontrollierten Kostenrahmen. Schließlich können Sie noch die minimalen und maximalen Ressourcen steuern, die für die Datenbanken im Pool verfügbar sind. So können Sie sicherstellen, dass keine Datenbank im Pool alle Poolressourcen verbraucht und dass jede Datenbank des Pools über ein garantiertes Minimum an Ressourcen verfügt. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die elastische Pools verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="blend-single-databases-with-pooled-databases"></a>Mischen von Einzeldatenbanken mit Datenbanken in Pools
Sie können weiterhin flexibel vorgehen – unabhängig davon, ob Sie sich für Einzeldatenbanken oder elastische Pools entscheiden. Sie haben die Möglichkeit, Einzeldatenbanken mit elastischen Pools zu kombinieren und die Dienstebenen von Einzeldatenbanken und elastischen Pools schnell und einfach an Ihre Bedürfnisse anzupassen. Außerdem können Sie mit der Leistungsstärke und Reichweite von Azure andere Azure-Dienste mit der SQL-Datenbank kombinieren, um Ihre individuellen Anforderungen an App-Design zu erfüllen, Kosten- und Ressourceneffizienz zu fördern und neue Geschäftsmöglichkeiten zu erschließen.

## <a name="monitoring-and-alerting"></a>Überwachung und Warnung
Aber wie können Sie nun die relative Leistung von Einzeldatenbanken und elastischen Pools vergleichen? Wie entscheiden Sie sich beim Anpassen für die richtige Lösung? Sie verwenden die [integrierten Leistungsüberwachungs-](sql-database-performance.md) und [Warntools](sql-database-insights-alerts-portal.md) in Kombination mit den Leistungsbewertungen, die auf [Datenbanktransaktionseinheiten (DTUs) für Einzeldatenbanken und elastischen DTUs (eDTUs) für elastische Pools](sql-database-what-is-a-dtu.md) basieren. Mit diesen Tools können Sie die Auswirkungen des zentralen Hoch- oder Herunterskalierens je nach Ihren derzeitigen bzw. projektbezogenen Leistungsanforderungen schnell bewerten. Ausführliche Informationen finden Sie unter [SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen](sql-database-service-tiers.md) .

## <a name="keep-your-app-and-business-running"></a>Aufrechterhalten von App-Ausführung und Geschäftskontinuität
Durch die Unterstützung eines globalen Netzwerks von Microsoft-verwalteten Rechenzentren stellt die in der Branche führende Verfügbarkeit von Azure mit einer Vereinbarung zum Servicelevel [(SLA)](http://azure.microsoft.com/support/legal/sla/) von 99,99% sicher, dass Ihre Apps rund um die Uhr ausgeführt werden. Mit jeder SQL-Datenbank nutzen Sie die integrierte Sicherheit, Fehlertoleranz und den [Schutz von Daten](sql-database-automated-backups.md), die Sie andernfalls entwerfen oder kaufen, erstellen und verwalten müssten. In der SQL-Datenbank bietet jede Dienstebene einen umfassenden Satz von Funktionen für Geschäftskontinuität und Optionen, die sicherstellen, dass Sie den Betrieb aufnehmen können und dieser nicht unterbrochen wird. Über die [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md) können Sie den früheren Status einer Datenbank für bis zu 35 Tage wiederherstellen. Sie können die [langfristige Sicherungsaufbewahrung](sql-database-long-term-retention.md) konfigurieren, um Sicherungen bis zu zehn Jahre lang in einem sicheren Tresor zu speichern. Ferner können Sie bei einem Ausfall des Datencenters, in dem Ihre Datenbanken gehostet werden, Ihre Datenbanken aus [georedundanten Kopien kürzlich durchgeführter Sicherungen](sql-database-recovery-using-backups.md) wiederherstellen. Darüber hinaus können Sie bei Bedarf [georedundante lesbare Replikate](sql-database-geo-replication-overview.md) in einer oder mehreren Regionen konfigurieren, um bei einem Ausfall eines Rechenzentrums ein schnelles Failover zu ermöglichen. Sie können diese Replikate auch verwenden, um eine schnellere Leseleistung in unterschiedlichen geografischen Regionen zu erzielen oder [Anwendungsupgrades ohne Ausfallzeit](sql-database-manage-application-rolling-upgrade.md) durchzuführen. 

![SQL-Datenbank-Georeplikation](./media/sql-database-technical-overview/azure_sqldb_map.png)

Ausführliche Informationen zu den unterschiedlichen Funktionen der Geschäftskontinuität bei den verschiedenen Dienstebenen finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md) .

## <a name="secure-your-data"></a>Sichern der Daten
SQL Server bietet traditionell eine hohe Datensicherheit, die SQL-Datenbank mit Funktionen zur Zugriffseinschränkung, zum Schutz von Daten und zur Überwachung von Aktivitäten fortführt. Eine kurze Übersicht über die Sicherheitsoptionen in SQL-Datenbank finden Sie unter [Sichern der SQL-Datenbank](sql-database-security-overview.md) . Ausführlichere Informationen zu den Sicherheitsfunktionen finden Sie unter [Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank](https://msdn.microsoft.com/library/bb510589) . Schließlich finden Sie im [Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/) Informationen zur Sicherheit der Azure-Plattform.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine Einführung in SQL-Datenbank gelesen und die Frage „Was ist SQL-Datenbank?“ beantwortet haben, sind Sie bereit für Folgendes:

* Unter [Preise](https://azure.microsoft.com/pricing/details/sql-database/) finden Sie Preisvergleiche und Rechner für Einzeldatenbanken und elastische Pools.
* Erfahren Sie mehr über [elastische Pools](sql-database-elastic-pool.md).
* Beginnen Sie mit dem [Erstellen einer ersten Azure SQL-Datenbank](sql-database-get-started-portal.md).
* Erstellen Sie Ihre erste App in C#, Java, Node.js, PHP, Python oder Ruby: [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)

