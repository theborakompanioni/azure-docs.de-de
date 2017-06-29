---
title: Worum handelt es sich beim Azure SQL-Datenbankdienst? | Microsoft Docs
description: Lernen Sie die technischen Details und Funktionen von SQL-Datenbank kennen, des relationalen Datenbankmanagementsystems von Microsoft in der Cloud.
keywords: "Einführung in SQL, Was ist SQL-Datenbank?"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: cgronlun
ms.assetid: c561f600-a292-4e3b-b1d4-8ab89b81db48
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/19/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 3b4706f50498616df64f1924cc83ceccec70f92b
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---
# <a name="what-is-the-azure-sql-database-service"></a>Worum handelt es sich beim Azure SQL-Datenbankdienst? 

Azure SQL-Datenbank ist eine vollständig verwaltete, relationale Database-as-a-Service (DBaaS) in der Microsoft Cloud („Azure“). Bei diesem Platform-as-a-Service-Angebot kümmert sich Microsoft um sämtliche Patches und Updates der SQL-Codebasis und übernimmt damit die Verwaltung der gesamten zugrunde liegenden Infrastruktur. SQL-Datenbank nutzt diese Codebasis gemeinsam mit dem [Microsoft SQL Server-Datenbankmodul](https://docs.microsoft.com/sql/sql-server/sql-server-technical-documentation). 

Microsoft verwaltet zurzeit Millionen von Produktionsdatenbanken mit einer Vielzahl von Anwendungen und Workloads – von einfachen Transaktionsdaten bis zu datenintensiven, unternehmenskritischen Anwendungen, die eine erweiterte Datenverarbeitung auf globaler Ebene erfordern. Der SQL-Datenbank-Dienst wird derzeit in 38 Rechenzentren auf der ganzen Welt ausgeführt – und regelmäßig werden neue online geschaltet.  

SQL-Datenbank ist eine relationale Datenbank für allgemeine Zwecke, die Strukturen wie relationale Daten, JSON, räumliche Daten und XML unterstützt. Sie bietet eine [dynamisch skalierbare Leistung](sql-database-service-tiers.md) und stellt Optionen wie z.B. [Columnstore-Indizes](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) für extrem umfangreiche Analysen und Berichte und [In-Memory-OLTP](sql-database-in-memory.md) für aufwendigste Transaktionsverarbeitungen bereit. 

Mit der Cloud-First-Strategie von Microsoft werden die neuesten Funktionen von SQL Server zunächst in SQL-Datenbank und erst dann in SQL Server selbst veröffentlicht. Durch diesen Ansatz verfügen Sie immer über die neuesten Features von SQL Server – ohne den Mehraufwand für Patches oder Updates –, die in Millionen von Datenbanken getestet wurden. Informationen zu angekündigten neuen Funktionen finden Sie unter:

- **[Azure-Roadmap für SQL-Datenbank:](https://azure.microsoft.com/roadmap/?category=databases)** Hier können Sie herausfinden, welche neuen Funktionen bereitstehen und was als Nächstes geplant ist. 
- **[Azure SQL-Datenbank-Blog:](https://azure.microsoft.com/blog/topics/database)** Hier schreiben die Teammitglieder von SQL Server-Produkten Beiträge zu News und Features von SQL-Datenbank. 

SQL-Datenbank bietet eine vorhersagbare Leistung mit mehreren Serviceleveln, die eine dynamische Skalierung ohne Ausfallzeit, integrierte intelligente Optimierung, globale Skalierbarkeit und Verfügbarkeit sowie erweiterte Sicherheitsoptionen bieten – alles mit nahezu keinem Verwaltungsaufwand. Mit diesen Funktionen können Sie sich auf die schnelle Entwicklung von Apps und die Verkürzung des Zeitraums bis zur Markteinführung konzentrieren, anstatt wertvolle Zeit und Ressourcen für die Verwaltung von virtuellen Computern und der Infrastruktur aufwenden zu müssen. 

> [!NOTE]
> Informationen zur Sicherheit der Azure-Plattform finden Sie im [Azure Trust Center](https://azure.microsoft.com/support/trust-center/security/).
>

## <a name="scalable-performance-and-pools"></a>Skalierbare Leistung und Pools

Bei SQL-Datenbank ist jede Datenbank von den anderen isoliert und damit portabel, außerdem besitzt jede eine eigene [Dienstebene](sql-database-service-tiers.md) mit einer garantierten Leistungsebene. SQL-Datenbank bietet unterschiedliche Leistungsebenen für unterschiedliche Anforderungen. Darüber hinaus können Datenbanken in Pools zusammengefasst werden, um die Verwendung von Ressourcen zu maximieren und damit Geld zu sparen.

### <a name="adjust-performance-and-scale-without-downtime"></a>Anpassen von Leistung und Skalierung ohne Ausfallzeiten

SQL-Datenbank bietet vier Dienstebenen zur Unterstützung von geringeren und aufwendigeren Datenbank-Workloads: Basic, Standard, Premium und Premium RS. Sie können zu geringen monatlichen Kosten Ihre erste App in einer kleinen Einzeldatenbank erstellen und dann manuell oder programmgesteuert jederzeit die Dienstebene (Tarif) ändern, um die Anforderungen Ihrer Lösung zu erfüllen. Die Leistungsanpassung ist möglich, ohne dass es für die App oder für Ihre Kunden zu Ausfallzeiten kommt. Dank der dynamischen Skalierbarkeit kann Ihre Datenbank in transparenter Form auf sich schnell ändernde Ressourcenanforderungen reagieren, und Sie zahlen nur für die Ressourcen, die Sie jeweils benötigen.

   ![Skalieren](./media/sql-database-what-is-a-dtu/single_db_dtus.png)

### <a name="elastic-pools-to-maximize-resource-utilization"></a>Elastische Pools zum Maximieren der Ressourcenverwendung

Für viele Unternehmen und Anwendungen genügt es, wenn Einzeldatenbanken erstellt werden können und sich die Leistung nach oben oder unten anpassen lässt – insbesondere, wenn die Nutzungsmuster relativ gut vorhersagbar sind. Bei unvorhersagbaren Nutzungsmustern kann es jedoch schwer sein, die Kosten und Ihr Geschäftsmodell zu verwalten. [Elastische Pools](sql-database-elastic-pool.md) sind dafür ausgelegt, dieses Problem zu beheben. Das Konzept ist denkbar einfach. Sie ordnen Leistungsressourcen nicht einer Einzeldatenbank, sondern einem Pool zu, sodass Sie für die gesamten Leistungsressourcen des Pools und nicht für die Leistung einer Einzeldatenbank zahlen. 

   ![Elastische Pools](./media/sql-database-what-is-a-dtu/sqldb_elastic_pools.png)

Bei elastischen Pools müssen Sie sich nicht darauf konzentrieren, die Datenbankleistung nach oben oder unten anzupassen, wenn der Ressourcenbedarf schwankt. Die im Pool zusammengefassten Datenbanken nutzen die Leistungsressourcen des elastischen Pools je nach Bedarf. In einem Pool zusammengefasste Datenbanken nutzen die Grenzwerte des Pools, überschreiten sie aber nicht. Ihre Kosten bleiben also vorhersagbar, auch wenn sich die Nutzung der einzelnen Datenbanken nicht prognostizieren lässt. Darüber hinaus können Sie [Datenbanken im Pool hinzufügen und entfernen](sql-database-elastic-pool-manage-portal.md)und so Ihre App von einigen wenigen auf Tausende Datenbanken skalieren, und das in einem kontrollierten Kostenrahmen. Sie können außerdem die minimalen und maximalen Ressourcen steuern, die für die Datenbanken im Pool verfügbar sind. So können Sie sicherstellen, dass keine Datenbank im Pool alle Poolressourcen verbraucht und dass jede Datenbank des Pools über ein garantiertes Minimum an Ressourcen verfügt. Weitere Informationen zu Entwurfsmustern für SaaS-Anwendungen, für die Pools für elastische Datenbanken verwendet werden, finden Sie unter [Entwurfsmuster für SaaS-Anwendungen mit mehreren Mandanten und SQL-Datenbank](sql-database-design-patterns-multi-tenancy-saas-applications.md).

### <a name="blend-single-databases-with-pooled-databases"></a>Mischen von Einzeldatenbanken mit Datenbanken in Pools

Sie können weiterhin flexibel vorgehen – unabhängig davon, ob Sie sich für Einzeldatenbanken oder elastische Pools entscheiden. Sie haben die Möglichkeit, Einzeldatenbanken mit elastischen Pools zu kombinieren und die Dienstebenen von Einzeldatenbanken und elastischen Pools schnell und einfach an Ihre Bedürfnisse anzupassen. Mit der Leistungsstärke und Reichweite von Azure können Sie andere Azure-Dienste mit SQL-Datenbank kombinieren, um Ihre individuellen Anforderungen bei der App-Entwicklung zu erfüllen, Kosten- und Ressourceneffizienz zu fördern und neue Geschäftsmöglichkeiten zu erschließen.

### <a name="extensive-monitoring-and-alerting-capabilities"></a>Umfassende Funktionen für Überwachung und Warnung

Aber wie können Sie nun die relative Leistung von Einzeldatenbanken und elastischen Pools vergleichen? Wie entscheiden Sie sich beim Anpassen für die richtige Lösung? Sie verwenden die [integrierten Leistungsüberwachungs-](sql-database-performance.md) und [Warntools](sql-database-insights-alerts-portal.md) in Kombination mit den Leistungsbewertungen, die auf [Datenbanktransaktionseinheiten (DTUs) für Einzeldatenbanken und elastischen DTUs (eDTUs) für elastische Pools](sql-database-what-is-a-dtu.md) basieren. Mit diesen Tools können Sie die Auswirkungen des zentralen Hoch- oder Herunterskalierens je nach Ihren derzeitigen bzw. projektbezogenen Leistungsanforderungen schnell bewerten. Ausführliche Informationen finden Sie unter [SQL-Datenbankoptionen und -leistung: Grundlegendes zum Angebot in den einzelnen Tarifen](sql-database-service-tiers.md) .

Darüber hinaus kann SQL-Datenbank zur einfacheren Überwachung [Metriken und Diagnoseprotokolle ausgeben](sql-database-metrics-diag-logging.md). Sie können SQL-Datenbank zum Speichern von Ressourcenverbrauch, Workern und Sitzungen sowie Verbindungen in einer der folgenden Azure-Ressourcen konfigurieren:

- **Azure Storage:** für die Archivierung großer Mengen von Telemetriedaten zu einem kleinen Preis
- **Azure Event Hub:** für die Integration von Telemetriedaten von SQL-Datenbank in Ihrer benutzerdefinierte Überwachungslösung oder Hotpipelines
- **Azure Log Analytics:** für eine integrierte Überwachungslösung mit Funktionen für Berichterstellung, Warnungen und Problemlösung

    ![Architektur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="availability-capabilities"></a>Verfügbarkeitsfunktionen

Durch die Unterstützung eines globalen Netzwerks von Microsoft-verwalteten Rechenzentren stellt die in der Branche führende Verfügbarkeit von Azure mit einer Vereinbarung zum Servicelevel [(SLA)](http://azure.microsoft.com/support/legal/sla/) von 99,99% sicher, dass Ihre Apps rund um die Uhr ausgeführt werden. SQL-Datenbank bietet außerdem integrierte Features für die [Geschäftskontinuität und globale Skalierbarkeit](sql-database-business-continuity.md). Dazu gehören u.a.:

- **[Automatische Sicherungen:](sql-database-automated-backups.md)** SQL-Datenbank führt automatisch vollständige, differenzielle und Transaktionsprotokollsicherungen durch.
- **[Point-in-Time-Wiederherstellungen:](sql-database-recovery-using-backups.md)** SQL-Datenbank unterstützt die Wiederherstellung zu einem beliebigen Zeitpunkt innerhalb der Vermerkdauer für automatische Sicherungen.
- **[Aktive Georeplikation:](sql-database-geo-replication-overview.md)** Mit SQL-Datenbank können Sie bis zu vier lesbare sekundäre Datenbanken entweder im gleichen oder in weltweit verteilten Azure-Rechenzentren konfigurieren.  Wenn Sie beispielsweise über eine SaaS-Anwendung mit einer Katalogdatenbank verfügen, die eine große Anzahl gleichzeitiger schreibgeschützter Transaktionen umfasst, können Sie mithilfe der aktiven Georeplikation eine globale Skalierung für das Lesen aktivieren und so Engpässe in der primären Datenbank beseitigen, die durch hohe Workloads aufgrund der Lesevorgänge verursacht werden. 
- **[Failovergruppen:](sql-database-geo-replication-overview.md)** SQL-Datenbank bietet eine hohe Verfügbarkeit und einen Lastenausgleich auf globaler Ebene, einschließlich transparenter Georeplikation und von Failovern großer Mengen von Datenbanken und Pools für elastische Datenbanken. Failovergruppen und die aktive Georeplikation erlauben das Erstellen von weltweit verteilten SaaS-Anwendungen mit minimalem Verwaltungsaufwand, bei dem alle komplexen Aufgaben für Überwachung, Weiterleitung und Failoverorchestrierung von SQL-Datenbank erledigt werden.

## <a name="built-in-intelligence"></a>Integrierte Logik

SQL-Datenbanken stellt integrierte Logik bereit, die Ihnen hilft, den Aufwand für die Ausführung und Verwaltung von Datenbanken erheblich zu senken und die Leistung und Sicherheit Ihrer Anwendung zu maximieren. Bei der Ausführung von Millionen von Kundenworkloads rund um die Uhr sammelt und verarbeitet SQL-Datenbank eine riesige Menge an Telemetriedaten. Dabei bleiben die Kundendaten im Hintergrund jedoch permanent geschützt. Verschiedene Algorithmen werten fortlaufend die Telemetriedaten aus, damit der Dienst mit Ihrer Anwendung lernen und sich anpassen kann. Auf Grundlage dieser Analyse schlägt der Dienst Empfehlungen zur Verbesserung der Leistung vor, die auf Ihre Workload zugeschnitten sind. 

### <a name="automatic-performance-tuning"></a>Automatische Leistungsoptimierung

SQL-Datenbank bietet detaillierte Einblicke in die Abfragen, die Sie überwachen sollten. SQL-Datenbank erlernt Ihre Datenbankmuster und ermöglicht Ihnen das Anpassen Ihres Datenbankschemas an Ihre Workload. SQL-Datenbank stellt mithilfe von [SQL Database Advisor](sql-database-advisor.md) Empfehlungen zur Leistungsoptimierung bereit, mit denen Sie Optimierungsschritte überprüfen und anwenden können. Eine kontinuierliche Überwachung der Datenbank ist jedoch eine schwierige und aufwendige Aufgabe, insbesondere bei sehr vielen Datenbanken. Das Verwalten einer großen Anzahl von Datenbanken ist eventuell nicht möglich, selbst mit allen verfügbaren Tools und Berichten, die SQL-Datenbank und das Azure-Portal bereitstellen. Anstelle der manuellen Überwachung und Optimierung der Datenbank sollten Sie erwägen, einige der Überwachungs- und Optimierungsaktionen mithilfe des Features zur automatischen Optimierung an SQL-Datenbank zu delegieren. SQL-Datenbank wendet Empfehlungen automatisch an und testet und überprüft jede der Optimierungsaktionen, um sicherzustellen, dass sich die Leistung auch wirklich verbessert. Auf diese Weise passt sich SQL-Datenbank automatisch auf kontrollierte und sichere Weise an Ihre Workload an. Die automatische Optimierung bedeutet, dass die Leistung Ihrer Datenbank vor und nach jeder Optimierungsaktion sorgfältig überwacht und verglichen wird – und wenn sich die Leistung nicht verbessert, wird die Optimierungsaktion zurückgesetzt.

Bereits heute verlassen sich viele unserer Partner, die [mehrinstanzenfähige SaaS-Apps](sql-database-design-patterns-multi-tenancy-saas-applications.md) auf Grundlage von SQL-Datenbank ausführen, auf die automatische Leistungsoptimierung, um sicherzustellen, dass ihre Anwendungen immer eine stabile und zuverlässige Leistung zeigen. Für sie verringert dieses Feature das Risiko, dass mitten in der Nacht ein Leistungsproblem auftritt, enorm. Da darüber hinaus auch ein Teil ihrer Kundenbasis SQL Server verwendet, wenden sie dieselben Indizierungsempfehlungen von SQL-Datenbank an, um ihre SQL Server-Kunden zu unterstützen.

Es gibt zwei Aspekte der automatischen Optimierung, die in SQL-Datenbank verfügbar sind:

- **[Automatische Indexverwaltung:](sql-database-automatic-tuning.md#automatic-index-management)** Diese Funktion identifiziert Indizes, die der Datenbank hinzugefügt, und solche, die entfernt werden sollten.
- **[Automatische Plankorrektur:](sql-database-automatic-tuning.md#automatic-plan-choice-correction)** Diese Funktion erkennt problematische Pläne und korrigiert Leistungsprobleme mit SQL-Plänen (in Kürze, in SQL Server 2017 bereits verfügbar).

### <a name="adaptive-query-processing"></a>Adaptive Abfrageverarbeitung

Wir fügen darüber hinaus auch einen Featuresatz zur adaptiven Abfrageverarbeitung in SQL-Datenbank hinzu. Dieser schließt die [überlappende Ausführung für Tabellenwertfunktionen mit mehreren Anweisungen, Feedback zur Speicherzuweisung im Batchmodus](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/04/19/introducing-interleaved-execution-for-multi-statement-table-valued-functions/) und [adaptive Joins im Batchmodus](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/04/19/introducing-batch-mode-adaptive-joins/) ein. Jedes dieser Features zur adaptiven Abfrageverarbeitung wendet ähnliche Techniken zum Lernen und Anpassen an, um weitere Leistungsprobleme zu beheben, die durch traditionell schwierig zu lösende Probleme bei der Abfrageoptimierung verursacht werden.

### <a name="intelligent-threat-detection"></a>Intelligente Bedrohungserkennung

 Die [SQL-Bedrohungserkennung](sql-database-threat-detection.md) nutzt die [Überwachung von SQL-Datenbank](sql-database-auditing.md) für die kontinuierliche Überwachung von Azure SQL-Datenbank-Instanzen auf potenziell schädliche Zugriffsversuche auf sensibler Daten. Die SQL-Bedrohungserkennung bietet eine neue Sicherheitsebene, die es den Kunden ermöglicht, auf erkannte potenzielle Bedrohungen zu reagieren. Zu diesem Zweck werden Sicherheitshinweise zu anomalen Aktivitäten bereitgestellt. Benutzer erhalten Warnungen zu verdächtigen Datenbankaktivitäten, potenziellen Sicherheitsrisiken sowie Angriffen mit Einschleusung von SQL-Befehlen und anomalen Datenbank-Zugriffsmustern. Die Warnungen der SQL-Bedrohungserkennung enthalten Details zu verdächtigen Aktivitäten sowie Empfehlungen, wie die Bedrohung untersucht und abgewendet werden kann. Benutzer können verdächtige Ereignisse untersuchen, um zu ermitteln, ob sie auf einen Zugriffsversuch, die Verletzung der Datensicherheit oder den Missbrauch von Daten in der Datenbank zurückzuführen sind. Die Bedrohungserkennung vereinfacht den Umgang mit potenziellen Bedrohungen für die Datenbank, ohne das Fachwissen eines Sicherheitsexperten besitzen oder komplexe Sicherheitsüberwachungssysteme verwalten zu müssen.

## <a name="advanced-security-and-compliance"></a>Erweiterte Sicherheit und Konformität

SQL-Datenbank bietet eine Reihe von [integrierten Sicherheits- und Konformitätsfeatures](sql-database-security-overview.md), mit der Sie Ihre Anwendung an verschiedene Sicherheits- und Konformitätsanforderungen anpassen können. 

### <a name="auditing-for-compliance-and-security"></a>Überwachung auf Konformität und Sicherheit

Die [SQL-Datenbank-Überprüfung](sql-database-auditing.md) verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in Ihrem Azure Storage-Konto. Die Überwachung kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.

### <a name="data-encryption-at-rest"></a>Datenverschlüsselung ruhender Daten

Die [Transparent Data Encryption](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database) von SQL-Datenbank bietet Schutz vor der Bedrohung durch böswillige Aktivitäten. Hierzu werden die Datenbank, die zugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit ver- und entschlüsselt, ohne dass Änderungen an der Anwendung erforderlich sind. Ab Mai 2017 werden alle neu erstellten Azure SQL-Datenbank-Instanzen automatisch mit Transparent Data Encryption (TDE) geschützt. TDE ist die bewährte SQL-Verschlüsselungstechnologie für ruhende Daten, die für viele Konformitätsstandards zum Schutz vor Diebstahl von Speichermedien erforderlich ist. Kunden können die TDE-Verschlüsselungsschlüssel und andere Geheimnisse auf sichere und konforme Weise mit Azure Key Vault verwalten.

### <a name="data-encryption-in-motion"></a>Datenverschlüsselung in Aktion

SQL-Datenbank ist das einzige Datenbanksystem, das mit [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine) einen Schutz für sensible Daten bei der Übertragung, im Ruhezustand und während der Abfrageverarbeitung bietet. Always Encrypted ist das branchenweit erste System, das einen beispiellosen Schutz von Daten gegen Sicherheitsverletzungen wie dem Diebstahl wichtiger Daten bietet. Beispielsweise werden mit Always Encrypted die Kreditkartennummern von Kunden immer verschlüsselt in der Datenbank gespeichert, selbst während der Abfrageverarbeitung. Gleichzeitig wird autorisierten Mitarbeitern oder Anwendungen, die diese Daten verarbeiten müssen, die Entschlüsselung zum Zeitpunkt der Verwendung erlaubt.

### <a name="dynamic-data-masking"></a>Dynamische Datenmaskierung

Die [dynamische Datenmaskierung in SQL-Datenbanken](sql-database-dynamic-data-masking-get-started.md) schränkt die Offenlegung sensibler Daten ein, indem diese für nicht berechtigte Benutzer maskiert werden. Die dynamische Datenmaskierung hilft beim Verhindern des unbefugten Zugriffs auf sensible Daten, indem Kunden festlegen dürfen, welcher Anteil der sensiblen Daten mit minimalen Auswirkungen auf die Anwendungsschicht offengelegt wird. Es handelt sich um eine richtlinienbasierte Sicherheitsfunktion, die die sensiblen Daten im Resultset einer Abfrage in festgelegten Datenbankfeldern ausblendet, ohne dass die Daten in der Datenbank geändert werden.

### <a name="row-level-security"></a>Sicherheit auf Zeilenebene

Bei der [Sicherheit auf Zeilenebene](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) können Kunden den Zugriff auf Zeilen in einer Datenbanktabelle basierend auf den Merkmalen des Benutzers steuern, der eine Abfrage ausführt (z.B. Gruppenmitgliedschaft oder Ausführungskontext). Die Sicherheit auf Zeilenebene (Row-Level Security, RLS) vereinfacht das Entwerfen und Programmieren der Sicherheit in Ihrer Anwendung. Mit RLS können Sie den Zugriff auf Datenzeilen einschränken. So können Sie beispielsweise sicherstellen, dass Mitarbeiter nur auf Datenzeilen zugreifen können, die für ihre Abteilung relevant sind, oder den Datenzugriff von Kunden auf Daten beschränken, die für das Unternehmen des jeweiligen Kunden relevant sind.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory-Integration und Multi-Factor Authentication

SQL-Datenbank ermöglicht über die [Azure Active Directory-Integration](sql-database-aad-authentication.md) eine zentrale Verwaltung von Identitäten der Datenbankbenutzer und anderer Microsoft-Dienste. Diese Funktion vereinfacht die Berechtigungsverwaltung und erhöht die Sicherheit. Azure Active Directory unterstützt die [Multi-Factor Authentication](sql-database-ssms-mfa-authentication.md) (MFA) zur Verbesserung der Daten- und Anwendungssicherheit, während gleichzeitig das einmalige Anmelden unterstützt wird.

### <a name="compliance-certification"></a>Konformitätszertifizierung

SQL-Datenbank ist an regulären Überwachungen beteiligt und wurde für mehrere Konformitätsstandards zertifiziert. Weitere Informationen finden Sie im [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/), wo die aktuellste Liste von [Compliance-Zertifizierungen für SQL-Datenbank](https://azure.microsoft.com/support/trust-center/services/) angezeigt wird.

## <a name="easy-to-use-tools"></a>Benutzerfreundliche Tools

SQL-Datenbank sorgt für einfachere und produktivere Abläufe beim Erstellen und Verwalten von Anwendungen. Mit SQL-Datenbank können Sie sich auf das konzentrieren, was Sie am besten können: erstklassige Apps entwickeln. Sie können in SQL-Datenbank Verwaltungs- und Entwicklungsaufgaben mithilfe von Tools und Fertigkeiten durchführen, über die Sie bereits verfügen.

- **[Azure-Portal:](https://portal.azure.com/)** eine webbasierte Anwendung für die Verwaltung aller Azure-Dienste 
- **[SQL Server Management Studio:](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)** eine kostenlose, herunterladbare Clientanwendung für die Verwaltung beliebiger SQL-Infrastrukturen – von SQL Server bis SQL-Datenbank
- **[SQL Server Data Tools in Visual Studio:](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt)** eine kostenlose, herunterladbare Clientanwendung, mit der Sie relationale SQL Server-Datenbanken, Azure SQL-Datenbanken, Integration Services-Pakete, Analysis Services-Datenmodelle und Reporting Services-Berichte erstellen können
- **[Visual Studio Code:](https://code.visualstudio.com/docs)** ein kostenloser, herunterladbarer Open-Source-Code-Editor für Windows, macOS und Linux, der Erweiterungen unterstützt, z.B. die [mssql-Erweiterung](https://aka.ms/mssql-marketplace) zum Abfragen von Microsoft SQL Server, Azure SQL-Datenbank und SQL Data Warehouse

SQL-Datenbank unterstützt das Erstellen von Anwendungen mit Python, Java, Node.js, PHP, Ruby und .NET unter macOS, Linux und Windows. SQL-Datenbank unterstützt dieselben [Verbindungsbibliotheken](sql-database-libraries.md) wie SQL Server.

## <a name="next-steps"></a>Nächste Schritte

- Unter [Preise](https://azure.microsoft.com/pricing/details/sql-database/) finden Sie Preisvergleiche und Rechner für Einzeldatenbanken und elastische Pools.

- Diese Schnellstarts erleichtern Ihnen den Einstieg:

  - [Create a SQL database in the Azure portal](sql-database-get-started-portal.md) (Erstellen einer SQL-Datenbank im Azure-Portal)  
  - [Erstellen einer SQL-Datenbank mit der Azure CLI](sql-database-get-started-cli.md)
  - [Erstellen einer SQL-Datenbank mithilfe von PowerShell](sql-database-get-started-powershell.md)

- Azure CLI- und PowerShell-Beispiele finden Sie unter:
  - [Azure CLI-Beispiele für SQL-Datenbank](sql-database-cli-samples.md)
  - [Azure PowerShell-Beispiele für SQL-Datenbank](sql-database-powershell-samples.md)
