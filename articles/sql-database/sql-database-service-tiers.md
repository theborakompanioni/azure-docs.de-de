<properties
	pageTitle="SQL-Datenbank-Leistung und -optionen: Tarife | Microsoft Azure"
	description="Vergleich Sie die verschiedenen Leistungs- und Geschäftskontinuitätsoptionen der SQL-Datenbanktarife, um beim Skalieren Kosten und Funktionalität im Blick zu behalten."
	keywords="Datenbankoptionen, Datenbankleistung"
	services="sql-database"
	documentationCenter=""
	authors="CarlRabeler"
	manager="jhubbard"
	editor="CarlRabeler"/>

<tags
	ms.service="sql-database"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="na"
	ms.workload="data-management"
	ms.date="08/10/2016"
	ms.author="carlrab"/>

# SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen

[Azure SQL-Datenbank](sql-database-technical-overview.md) bietet drei Tarife mit mehreren Leistungsstufen für unterschiedliche Workloads. Jede Leistungsstufe bietet einen zunehmenden Umfang an Ressourcen, um einen zunehmend höheren Durchsatz zu ermöglichen. Sie können jede Datenbank in ihrer eigenen [Dienstebene](sql-database-service-tiers.md#standalone-database-service-tiers-and-performance-levels) mit eigenen Leistungsstufen verwalten. Darüber hinaus besteht die Möglichkeit, mehrere Datenbanken in einem [elastischen Pool](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus) mit gemeinsam genutzten Ressourcen zu verwalten. Die für eigenständige Datenbanken verfügbaren Ressourcen werden als Datenbanktransaktionseinheiten (Database Transaction Units, DTUs) und die für elastische Pools verfügbaren Ressourcen als elastische DTUs bzw. eDTUs bezeichnet. Weitere Informationen zu DTUs und eDTUs finden Sie unter [What is a DTU](sql-database-what-is-a-DTU.md) (Informationen zu DTUs).

In beiden Fällen sind die Dienstebenen **Basic**, **Standard** und **Premium** verfügbar. Die Datenbankoptionen dieser Ebenen sind bei eigenständigen Datenbanken und elastischen Pools ähnlich, aber bei elastischen Pools sind noch weitere Aspekte zu beachten. Dieser Artikel enthält ausführliche Informationen zu den Dienstebenen für eigenständige Datenbanken und elastische Pools.

## Tarife und Datenbankoptionen
Die Dienstebenen "Basic", "Standard" und "Premium" haben alle eine Betriebszeit-SLA von 99,99 % und bieten vorhersagbare Leistung, flexible Optionen für Geschäftskontinuität, Sicherheitsfeatures und stündliche Abrechnung. In der folgenden Tabelle sind Beispiele für Dienstebenen aufgeführt, die sich für unterschiedliche Anwendungsworkloads am besten eignen.

| Dienstebene | Zielworkloads |
|---|---|
| **Basic** | Eignet sich am besten für eine kleine Datenbank, in der normalerweise ein einzelner aktiver Vorgang zu einem bestimmten Zeitpunkt unterstützt wird. Beispiele hierfür sind Datenbanken, die für die Entwicklung, für Tests oder für kleine, selten verwendete Anwendungen verwendet werden. |
| **Standard** | Die richtige Wahl für die meisten Cloudanwendungen mit Unterstützung mehrerer gleichzeitiger Abfragen. Beispiele hierfür sind Arbeitsgruppen-oder Webanwendungen. |
| **Premium** | Konzipiert für hohe Transaktionsvolumen, unterstützt viele gleichzeitige Benutzer und erfordert die höchste Stufe der Funktionen für Geschäftskontinuität. Beispiele hierfür sind Datenbanken für unternehmenskritische Anwendungen. |

>[AZURE.NOTE] Web Edition und Business Edition sind nicht mehr verfügbar. Informieren Sie sich unter [Häufig gestellte Fragen zur Einstellung von Web und Business Edition](https://azure.microsoft.com/pricing/details/sql-database/web-business/), wenn Sie Web- und Business-Editionen weiterhin verwenden möchten.

## Tarife und Leistungsstufen für eigenständige Datenbanken
Bei eigenständigen Datenbanken gibt es mehrere Leistungsstufen auf den einzelnen Dienstebenen. Sie können die Ebene auswählen, die Ihren Anforderungen an Workloads am besten entspricht. Wenn Sie zentral hoch- oder herunterskalieren müssen, können Sie die Ebenen Ihrer Datenbank leicht ändern. Ausführlichere Informationen finden Sie unter [Ändern der Dienstebenen und -Leistungsstufen von Datenbanken](sql-database-scale-up.md).

Die hier aufgeführten Leistungsmerkmale gelten für Datenbanken, die mit [SQL-Datenbank V12](sql-database-v12-whats-new.md) erstellt wurden. Unabhängig von der Anzahl von gehosteten Datenbanken erhält Ihre Datenbank einen garantierten Ressourcensatz, und die erwarteten Leistungsmerkmale Ihrer Datenbank werden nicht beeinträchtigt.

[AZURE.INCLUDE [Tarife für SQL-Datenbank](../../includes/sql-database-service-tiers-table.md)]

>[AZURE.NOTE] Eine ausführliche Beschreibung aller anderen Zeilen in dieser Dienstebenentabelle finden Sie unter [Service tier capabilities and limits](sql-database-performance-guidance.md#service-tier-capabilities-and-limits) (Funktionen und Limits von Dienstebenen; in englischer Sprache).

## Dienstebenen und Leistung für elastische Pools in eDTUs
Neben dem Erstellen und Skalieren einer eigenständigen Datenbank haben Sie die Möglichkeit, mehrere Datenbanken in einem [elastischen Pool](sql-database-elastic-pool.md) zu verwalten. Alle Datenbanken in einem elastischen Pool nutzen einen gemeinsamen Satz von Ressourcen. Die Leistungsmerkmale werden in *Transaktionseinheiten für elastische Datenbanken* (eDTUs) gemessen. Wie bei eigenständigen Datenbanken auch, verfügen Pools über drei Dienstebenen: **Basic**, **Standard** und **Premium**. Auch für Pools werden mit diesen drei Dienstebenen die Leistungsgrenzen und verschiedene Funktionen definiert.

Durch Pools können Datenbanken DTU-Ressourcen freigeben und gemeinsam nutzen, ohne dass jeder Datenbank im Pool eine bestimmte Leistungsstufe zugewiesen werden muss. Beispielsweise kann eine eigenständige Datenbank in einem Pool der Dienstebene „Standard“ 0 eDTU bis hin zum maximalen eDTU-Wert für Datenbanken nutzen, den Sie beim Konfigurieren des Pools festlegen. Bei Pools können mehrere Datenbanken mit unterschiedlichen Workloads die für den gesamten Pool verfügbaren eDTU-Ressourcen effizient nutzen. Weitere Informationen finden Sie unter den [Überlegungen zum Preis und zur Leistung eines elastischen Pools](sql-database-elastic-pool-guidance.md).

In der folgenden Tabelle sind die Merkmale der Dienstebenen eines Pools beschrieben.

[AZURE.INCLUDE [Tabelle mit den SQL-Datenbank-Dienstebenen für elastische Pools](../../includes/sql-database-service-tiers-table-elastic-db-pools.md)]

Für jede Datenbank in einem Pool gelten auch die Merkmale für eigenständige Datenbanken für die entsprechende Dienstebene. Der Basic-Pool verfügt in Bezug auf die maximalen Sitzungen pro Pool beispielsweise über eine Beschränkung auf den Bereich von 4.800 bis 28.800. Eine eigenständige Datenbank in einem Basic-Pool verfügt dagegen über eine Datenbankbeschränkung von 300 Sitzungen.

## Auswählen einer Dienstebene

Ermitteln Sie zum Auswählen einer Dienstebene zunächst, ob die Datenbank eine eigenständige Datenbank oder Teil eines elastischen Pools sein soll.

### Auswählen einer Dienstebene für eine eigenständige Datenbank

Beginnen Sie bei einer Dienstebene für eine eigenständige Datenbank damit, die benötigten Datenbankfeatures zu bestimmen, um die SQL-Datenbank-Edition auswählen zu können:

- Datenbankgröße (maximal 2 GB für Basic, maximal 250 GB für Standard und maximal 500 GB bis 1 TB für Premium, je nach Leistungsebene)
- Datenbank-Aufbewahrungszeitraum (7 Tage bei Basic und 35 Tage bei Standard und Premium)

Nach der Ermittlung der SQL-Datenbank-Edition können Sie die Leistungsebene für die Datenbank (Anzahl von DTUs) bestimmen. Sie können dies schätzen und dann anhand der tatsächlichen Erfahrungen [dynamisch zentral hoch- oder herunterskalieren](sql-database-scale-up.md). Sie können auch den [DTU Calculator](http://dtucalculator.azurewebsites.net/) (DTU-Rechner) verwenden, um die geschätzte Anzahl von erforderlichen DTUs zu ermitteln.

### Auswählen einer Dienstebene für einen Pool für elastische Datenbanken

Ermitteln Sie bei der Dienstebene für einen Pool für elastische Datenbanken zunächst die benötigten Datenbankfeatures, um die Dienstebene für den Pool auswählen zu können:

- Datenbankgröße (2 GB für Basic, 250 GB für Standard und 500 GB für Premium)
- Datenbank-Aufbewahrungszeitraum (7 Tage bei Basic und 35 Tage bei Standard und Premium)
- Anzahl von Datenbanken pro Pool (400 für Basic, 400 für Standard und 50 für Premium)
- Maximaler Speicher pro Pool (117 GB für Basic, 1.200 für Standard und 750 für Premium)

Nachdem Sie die Dienstebene für Ihren Pool ermittelt haben, können Sie die Leistungsebene für den Pool (eDTUs) bestimmen. Sie können dies schätzen und dann anhand der tatsächlichen Erfahrungen [dynamisch zentral hoch- oder herunterskalieren](sql-database-elastic-pool-manage-portal.md#change-performance-settings-of-a-pool). Sie können den [DTU Calculator](http://dtucalculator.azurewebsites.net/) (DTU-Rechner) verwenden, um die geschätzte Anzahl von erforderlichen DTUs für jede Datenbank eines Pools zu ermitteln und die Obergrenze für den Pool festzulegen.

## Nächste Schritte
- Erfahren Sie mehr über die Preise für diese Ebenen unter [SQL-Datenbank Preise](https://azure.microsoft.com/pricing/details/sql-database/).
- Erfahren Sie mehr über [elastische Pools](sql-database-elastic-pool-guidance.md), und lesen Sie sich den Artikel [Wo sollte ein Pool für elastische Datenbanken verwendet werden?](sql-database-elastic-pool-guidance.md) durch.
- Erfahren Sie, wie Sie [einen elastischen Pool überwachen und verwalten und seine Größe ändern](sql-database-elastic-pool-manage-portal.md) und [die Leistung von eigenständigen Datenbanken überwachen](sql-database-single-database-monitor.md).
- Nachdem Sie jetzt die Ebenen der SQL-Datenbank kennen, können Sie sie mit einem [kostenlosen Konto](https://azure.microsoft.com/pricing/free-trial/) ausprobieren und sich mit der [Erstellung Ihrer ersten SQL-Datenbank](sql-database-get-started.md) befassen.

## Zusätzliche Ressourcen

Informationen zu gängigen Datenarchitekturmustern von mehrinstanzenfähigen SaaS-Datenbankanwendungen (Software-as-a-Service) finden Sie unter [Entwurfsmuster für mehrinstanzenfähige SaaS-Anwendungen und Azure SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

<!----HONumber=AcomDC_0921_2016--->