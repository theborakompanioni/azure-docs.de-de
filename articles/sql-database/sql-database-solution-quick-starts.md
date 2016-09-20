<properties
   pageTitle="Azure SQL-Datenbanklösung – Schnellstarts | Microsoft Azure"
   description="Informationen zu Azure SQL-Datenbanklösungen"
   services="sql-database"
   documentationCenter=""
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-quickstart"
   ms.date="09/06/2016"
   ms.author="carlrab"/>

# Übersicht über die Schnellstarts zur Azure SQL-Datenbanklösung

Dieser Artikel bietet eine Übersicht über die Schnellstarts zur Azure SQL-Datenbanklösung. Diese Schnellstarts befinden sich im GitHub-Repository mit SQL Server-Beispielen und zeigen die Verwendung von SQL-Datenbank in einer vollständigen Lösung basierend auf realen Szenarien. Einfache Schritt-für-Schritt-Tutorials, die die Verwendung eines bestimmten SQL-Datenbankfeatures veranschaulichen, finden Sie unter [Entdecken Sie Azure SQL-Datenbank-Tutorials](sql-database-explore-tutorials.md).

## Ausprobieren der WingTipTickets-Demo und praktische Übungen

Die [Azure SQL Database WingTipTickets](https://github.com/microsoft/wingtiptickets)-Demo und die zugehörigen praktischen Übungen veranschaulichen eine auf Azure SQL-Datenbank und Azure Search basierende Beispielanwendung, mit der Konzerttickets verkauft werden.


## Sammeln und Überwachen von Ressourcennutzungsdaten über mehrere Pools hinweg

[Solution Quick Start: Elastic Pool telemetry using PowerShell](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) (Lösungsschnellstart: Telemetrie in elastischen Pools mithilfe von PowerShell) bietet eine Lösung für die Erfassung und Überwachung der SQL-Datenbank-Ressourcenverwendung über mehrere Pools in einem Abonnement hinweg. Wenn Sie über eine große Anzahl von Datenbanken in einem Abonnement verfügen, ist es mühselig, jeden elastischen Pool einzeln zu überwachen.

Um dieses Problem zu lösen, können Sie PowerShell-Cmdlets für SQL-Datenbank und T-SQL-Abfragen kombinieren, um Ressourcennutzungsdaten aus mehreren Pools und den zugehörigen Datenbanken zu sammeln. So können Sie die Ressourcenverwendung effizienter überwachen und analysieren.

Dieser Schnellstart bietet eine Reihe von PowerShell-Skripts und T-SQL-Abfragen sowie Informationen zur Funktionsweise und zur Implementierung der Lösung.

## Erste Schritte mit elastischen Datenbanken in einem SaaS-Szenario

 Unter [Solution Quick Start: Elastic Pool Custom Dashboard for SaaS](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) (Lösungsschnellstart: Benutzerdefiniertes Dashboard für SaaS mit elastischem Pool) finden Sie eine Beispiellösung für ein SaaS-Szenario (Software-as-a-Service), bei dem mithilfe des SQL-Datenbank-Features für elastische Datenbanken ein kostengünstiges und skalierbares Datenbank-Back-End für eine SaaS-Anwendung bereitgestellt wird.

Diese Lösung führt Sie Schritt für Schritt durch die Implementierung einer Web-App. Mit dieser Web-App können Sie die Last visualisieren, die in einer elastischen Datenbank von einem Lastengenerator erzeugt wird, der ein benutzerdefiniertes Dashboard als Ergänzung zum Azure-Portal verwendet.

Dieser Schnellstart bietet einen Web-App für die Generierung und Überwachung von Lasten sowie Informationen zur Funktionsweise und Verwendung der App.

## Erstellen einer Azure SQL-Datenbank unter Verwendung von Code First-Entwicklung und Entity Framework

Das Video und dazugehörige Beispiel zur [Code First-Entwicklung](https://msdn.microsoft.com/data/jj193542.aspx) bieten eine Einführung in die Code First-Entwicklung für eine neue Datenbank. In diesem Szenario wird eine Datenbank verwendet, die noch nicht vorhanden ist, sondern von Code First erstellt wird. Alternativ dazu erstellt das Szenario eine leere Datenbank, der Code First neue Tabellen hinzufügt.

Mit Code First können Sie Ihr Modell mithilfe von C#- oder Visual Basic .NET-Klassen definieren. Sie können optional zusätzliche Konfigurationsschritte ausführen – beispielsweise durch Anwendung von Attributen auf Klassen und Eigenschaften oder durch Verwendung einer Fluent-API.

## Integrieren von Tools für elastische Datenbanken in eine Entity Framework-Anwendung

Das Beispiel [Clientbibliothek für elastische Datenbanken mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) zeigt die Änderungen, die Sie an einer Entity Framework-Anwendung vornehmen müssen, um sie in [Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md) zu integrieren. Der Schwerpunkt liegt auf der Erstellung der [Shardzuordnungsverwaltung](sql-database-elastic-scale-shard-map-management.md) und des [datenabhängigen Routings](sql-database-elastic-scale-data-dependent-routing.md) mit dem Code First-Ansatz von Entity Framework.

Das [Code First-Beispiel für eine neue Datenbank für EF](http://msdn.microsoft.com/data/jj193542.aspx) wird im gesamten Dokument als Beispiel verwendet. Der zu diesem Dokument gehörige Beispielcode ist Teil der Beispielserie der Tools für elastische Datenbanken in den Visual Studio-Codebeispielen.

## Integrieren der Tools für elastische Datenbank in die Sicherheit auf Zeilenebene

Der Artikel [Mehrinstanzenfähige Anwendungen mit elastischen Datenbanktools und zeilenbasierter Sicherheit](sql-database-elastic-tools-multi-tenant-row-level-security.md) zeigt die Änderungen, die Sie an einer Entity Framework-Anwendung vornehmen müssen, um [Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md) in die [Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131) zu integrieren. Dieses Beispiel veranschaulicht, wie diese Technologien zusammen verwendet werden, um eine Anwendung mit einer hochgradig skalierbaren Datenebene zu erstellen, die mehrinstanzenfähige Shards unterstützt.

Sie können hierfür mit ADO.NET SqlClient oder Entity Framework arbeiten. Dieses Beispiel erweitert die [Clientbibliothek für elastische Datenbanken mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) durch Hinzufügen von Unterstützung für mehrinstanzenfähige Sharddatenbanken. Hierzu wird eine einfache Konsolenanwendung mit vier Mandanten und zwei mehrinstanzenfähigen Sharddatenbanken zum Erstellen von Blogs und Beiträgen erstellt.

## Erstellen von Onlineumfragen mit der Tailspin Surveys-Anwendung

Bei der [Tailspin Surveys-Beispielanwendung](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) handelt es sich um eine mehrinstanzenfähige Webanwendung namens „Surveys“, die Benutzern die Erstellung von Onlineumfragen ermöglicht. Das Beispiel veranschaulicht einige wichtige Aspekte der Verwaltung von Benutzeridentitäten in einer mehrinstanzenfähigen Anwendung, einschließlich Registrierung, Authentifizierung, Autorisierung und App-Rollen.

## Informationen zu den neuesten Sicherheitsfunktionen von SQL-Datenbank mit der Contoso Clinic-Demoanwendung

Diese [Contoso Clinic-Demoanwendung](https://github.com/Microsoft/azure-sql-security-sample) zeigt die neuesten Sicherheitsfunktionen von SQL-Datenbank.

## Nächste Schritte

[Entdecken Sie Azure SQL-Datenbank-Tutorials](sql-database-explore-tutorials.md)

<!---HONumber=AcomDC_0907_2016-->