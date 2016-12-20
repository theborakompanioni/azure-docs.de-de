---
title: 'SQL-Datenbankleistung: Dienstebenen | Microsoft Docs'
description: "Vergleichen Sie die Dienstebenen für SQL-Datenbank."
keywords: Datenbankoptionen, Datenbankleistung
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 12/06/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: e8bb9e5a02a7caf95dae0101c720abac1c2deff3
ms.openlocfilehash: 7bbdbe345bd468c01e2a790610bcf6c063c11f9b


---
# <a name="sql-database-service-tiers-for-single-databases-and-elastic-database-pools"></a>Dienstebenen für SQL-Datenbank für Einzeldatenbanken und Pools für elastische Datenbanken
[Azure SQL-Datenbank](sql-database-technical-overview.md) bietet drei Dienstebenen mit mehreren Leistungsstufen für unterschiedliche Workloads. Höhere Leistungsstufen bieten einen zunehmenden Umfang an Ressourcen, um einen zunehmend höheren Durchsatz zu ermöglichen. Sie können die Tarife und Leistungsstufen dynamisch ändern. Ausführlichere Informationen finden Sie unter [Ändern der Dienstebenen und -Leistungsstufen von Datenbanken](sql-database-scale-up.md) .

Sie können jede Datenbank in ihrer eigenen [Dienstebene](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) mit eigenen Leistungsstufen verwalten. Darüber hinaus besteht die Möglichkeit, mehrere Datenbanken in einem [Pool für elastische Datenbanken](sql-database-service-tiers.md#elastic-database-pool-service-tiers-and-performance-in-edtus) mit gemeinsam genutzten Ressourcen zu verwalten. Die für Einzeldatenbanken verfügbaren Ressourcen werden als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) bezeichnet, die für Pools für elastische Datenbanken verfügbaren Ressourcen werden als elastische DTUs bzw. eDTUs bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [Was sind Datenbanktransaktionseinheiten (DTUs)?](sql-database-what-is-a-dtu.md). 

In beiden Fällen sind die Dienstebenen **Basic**, **Standard** und **Premium** verfügbar. 

## <a name="service-tiers"></a>Dienstebenen
Die Dienstebenen "Basic", "Standard" und "Premium" haben alle eine Betriebszeit-SLA von 99,99 % und bieten vorhersagbare Leistung, flexible Optionen für Geschäftskontinuität, Sicherheitsfeatures und stündliche Abrechnung. In der folgenden Tabelle sind Beispiele für Dienstebenen aufgeführt, die sich für unterschiedliche Anwendungsworkloads am besten eignen.

| Dienstebene | Zielworkloads |
| :--- | --- |
| **Basic** |Eignet sich am besten für eine kleine Datenbank, in der normalerweise ein einzelner aktiver Vorgang zu einem bestimmten Zeitpunkt unterstützt wird. Beispiele hierfür sind Datenbanken, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| **Standard** |Die richtige Wahl für die meisten Cloudanwendungen mit Unterstützung mehrerer gleichzeitiger Abfragen. Beispiele hierfür sind Arbeitsgruppen-oder Webanwendungen. |
| **Premium** |Konzipiert für hohe Transaktionsvolumen, unterstützt viele gleichzeitige Benutzer und erfordert die höchste Stufe der Funktionen für Geschäftskontinuität. Beispiele hierfür sind Datenbanken für unternehmenskritische Anwendungen. |

> [!NOTE]
> Web Edition und Business Edition sind nicht mehr verfügbar. Informieren Sie sich unter [Häufig gestellte Fragen zur Einstellung von Web und Business Edition](https://azure.microsoft.com/pricing/details/sql-database/web-business/) , wenn Sie Web- und Business-Editionen weiterhin verwenden möchten.
> 
> 

## <a name="single-database-service-tiers-and-performance-levels"></a>Tarife und Leistungsstufen für Einzeldatenbanken
Bei Einzeldatenbanken gibt es mehrere Leistungsstufen auf den einzelnen Dienstebenen. Sie können die Ebene auswählen, die Ihren Anforderungen an Workloads am besten entspricht. Wenn Sie zentral hoch- oder herunterskalieren müssen, können Sie die Leistungsstufe Ihrer Datenbank schnell ändern. Ausführlichere Informationen finden Sie unter [Ändern der Dienstebenen und -Leistungsstufen von Datenbanken](sql-database-scale-up.md) .

Die hier aufgeführten Leistungsmerkmale gelten für Datenbanken, die mit [SQL-Datenbank V12](sql-database-technical-overview.md)erstellt wurden. Unabhängig von der Anzahl von gehosteten Datenbanken erhält Ihre Datenbank einen garantierten Ressourcensatz, und die erwarteten Leistungsmerkmale Ihrer Datenbank werden nicht beeinträchtigt.

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

> [!NOTE]
> Eine ausführliche Beschreibung aller anderen Zeilen in dieser Dienstebenentabelle finden Sie unter [Service tier capabilities and limits](sql-database-performance-guidance.md#service-tier-capabilities-and-limits)(Funktionen und Limits von Dienstebenen; in englischer Sprache).
> 
> 

## <a name="elastic-database-pool-service-tiers-and-performance-in-edtus"></a>Tarife und Leistung für einen Pool für elastische Datenbanken in eDTUs
Sie können auch mehrere Datenbanken in einem [Pool für elastische Datenbanken](sql-database-elastic-pool.md) verwalten. Alle Datenbanken in einem Pool für elastische Datenbanken nutzen einen gemeinsamen Satz von Ressourcen. Die Leistungsmerkmale werden in *Transaktionseinheiten für elastische Datenbanken* (eDTUs) gemessen. Wie bei Einzeldatenbanken auch, verfügen Pools über drei Dienstebenen: **Basic**, **Standard** und **Premium**. Auch für Pools werden mit diesen drei Dienstebenen die Leistungsgrenzen und verschiedene Funktionen definiert.

Durch Pools können Datenbanken DTU-Ressourcen freigeben und gemeinsam nutzen, ohne dass jeder Datenbank im Pool eine bestimmte Leistungsstufe zugewiesen werden muss. Beispielsweise kann eine Datenbank in einem Pool der Dienstebene „Standard“ 0 eDTUs bis hin zum maximalen eDTU-Wert für Datenbanken nutzen, den Sie beim Konfigurieren des Pools festlegen. Bei Pools können mehrere Datenbanken mit unterschiedlichen Workloads die für den gesamten Pool verfügbaren eDTU-Ressourcen effizient nutzen. Weitere Informationen finden Sie unter [Überlegungen zum Preis und zur Leistung eines Pools für elastische Datenbanken](sql-database-elastic-pool-guidance.md).

In der folgenden Tabelle sind die Merkmale von elastischen Basic-, Standard- und Premium-Datenbankpools beschrieben.

[!INCLUDE [SQL DB service tiers table for elastic database pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Für jede Datenbank in einem Pool gelten auch die Merkmale für Einzeldatenbanken für die entsprechende Dienstebene. Der Basic-Pool verfügt in Bezug auf die maximalen Sitzungen pro Pool beispielsweise über eine Beschränkung auf den Bereich von 4.800 bis 28.800. Eine eigenständige Datenbank in einem Basic-Pool verfügt dagegen über eine Datenbankbeschränkung von 300 Sitzungen.

## <a name="choosing-a-service-tier"></a>Auswählen einer Dienstebene
Ermitteln Sie zum Auswählen einer Dienstebene zunächst, ob die Datenbank eine Einzeldatenbank oder Teil eines Pools für elastische Datenbanken sein soll. 

### <a name="choosing-a-service-tier-for-a-single-database"></a>Auswählen einer Dienstebene für eine Einzeldatenbank
Beginnen Sie bei der Entscheidung für eine Dienstebene für eine Einzeldatenbank damit, die benötigten Datenbankfeatures zu bestimmen, um die SQL-Datenbank-Edition auswählen zu können:

* Datenbankgröße (maximal 2 GB für Basic, maximal 250 GB für Standard und maximal 500 GB bis 1 TB für Premium, je nach Leistungsebene)
* Datenbank-Aufbewahrungszeitraum (7 Tage bei Basic und 35 Tage bei Standard und Premium)

Nach der Ermittlung der SQL-Datenbank-Edition können Sie die Leistungsebene für die Datenbank (Anzahl von DTUs) bestimmen. Sie können dies schätzen und dann anhand der tatsächlichen Erfahrungen [dynamisch zentral hoch- oder herunterskalieren](sql-database-scale-up.md) . Sie können auch den [DTU Calculator](http://dtucalculator.azurewebsites.net/) (DTU-Rechner) verwenden, um die geschätzte Anzahl von erforderlichen DTUs zu ermitteln. 

### <a name="choosing-a-service-tier-for-an-elastic-database-pool"></a>Auswählen einer Dienstebene für einen Pool für elastische Datenbanken
Ermitteln Sie bei der Dienstebene für einen Pool für elastische Datenbanken zunächst die benötigten Datenbankfeatures, um die Dienstebene für den Pool auswählen zu können:

* Datenbankgröße (2 GB für Basic, 250 GB für Standard und 500 GB für Premium)
* Datenbank-Aufbewahrungszeitraum (7 Tage bei Basic und 35 Tage bei Standard und Premium)
* Anzahl von Datenbanken pro Pool (400 für Basic, 400 für Standard und 50 für Premium)
* Maximaler Speicher pro Pool (117 GB für Basic, 1.200 für Standard und 750 für Premium)

Nachdem Sie die Dienstebene für Ihren Pool ermittelt haben, können Sie die Leistungsebene für den Pool (eDTUs) bestimmen. Sie können dies schätzen und dann anhand der tatsächlichen Erfahrungen [dynamisch zentral hoch- oder herunterskalieren](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool) . Sie können den [DTU Calculator](http://dtucalculator.azurewebsites.net/) (DTU-Rechner) verwenden, um die geschätzte Anzahl von erforderlichen DTUs für jede Datenbank eines Pools zu ermitteln und die Obergrenze für den Pool festzulegen.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über die Preise für diese Ebenen unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/).
* Erfahren Sie mehr über [Pools für elastische Datenbanken](sql-database-elastic-pool-guidance.md), und lesen Sie sich die [Überlegungen zum Preis und zur Leistung eines Pools für elastische Datenbanken](sql-database-elastic-pool-guidance.md) durch.
* Erfahren Sie, wie Sie einen [Pool für elastische Datenbanken überwachen und verwalten und seine Größe ändern](sql-database-elastic-pool-manage-portal.md) und die [Leistung von Einzeldatenbanken überwachen](sql-database-single-database-monitor.md).
* Nachdem Sie jetzt die Ebenen für SQL-Datenbank kennen, können Sie sie mit einem [kostenlosen Konto](https://azure.microsoft.com/pricing/free-trial/) ausprobieren und sich mit der [Erstellung Ihrer ersten SQL-Datenbank](sql-database-get-started.md) befassen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Microsoft Virtual Academy video course on elastic database capabilities in Azure SQL Database](https://mva.microsoft.com/en-US/training-courses/elastic-database-capabilities-with-azure-sql-db-16554) (Microsoft Virtual Academy-Videokurs zu Funktionen elastischer Datenbanken in Azure SQL-Datenbank)




<!--HONumber=Nov16_HO4-->


