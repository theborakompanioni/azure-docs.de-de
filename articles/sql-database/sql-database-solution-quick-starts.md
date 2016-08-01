<properties
   pageTitle="Azure SQL-Datenbanklösung – Schnellstarts | Microsoft Azure"
   description="Informationen zu Azure SQL-Datenbanklösungen"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="sqldb-quickstart"
   ms.date="06/22/2016"
   ms.author="carlrab"/>

# Übersicht über die Schnellstarts zur Azure SQL-Datenbanklösung

Dieser Artikel bietet eine Übersicht über die Schnellstarts zur Azure SQL-Datenbanklösung. Diese Schnellstarts veranschaulichen den Einsatz der SQL-Datenbank in einer vollständigen Lösung anhand von Anwendungsbeispielen. Einfache Schritt-für-Schritt-Tutorials, die die Verwendung eines bestimmten Azure SQL-Datenbankfeatures veranschaulichen, finden Sie unter [Kennenlernen der SQL-Datenbank-Tutorials](sql-database-explore-tutorials.md).

## WingTipTickets-Demo und praktische Übungen

[Azure SQL Database WingTipTickets](https://github.com/microsoft/wingtiptickets)-Demo und praktische Übungen. Diese Dateien enthalten praktische Übungen, in denen eine auf Azure SQL-Datenbank und Azure Search basierende Beispielanwendung vorgestellt wird, mit der Konzerttickets verkauft werden.

## Sammeln und Überwachen von Ressourcennutzungsdaten über mehrere Pools hinweg

Dieser Lösungsschnellstart bietet eine Lösung für die Erfassung und Überwachung der Azure SQL-Datenbank-Ressourcennutzung über mehrere Pools in einem Abonnement hinweg. Wenn Sie über eine große Anzahl von Datenbanken in einem Abonnement verfügen, ist es mühselig, jeden elastischen Pool einzeln zu überwachen. Um dies zu vermeiden, können Sie PowerShell-Cmdlets für die SQL-Datenbank und T-SQL-Abfragen kombinieren, um Ressourcennutzungsdaten aus mehreren Pools und den zugehörigen Datenbanken zu sammeln und eine Überwachung und Analyse der Ressourcennutzung durchzuführen.

Unter [Verwalten mehrerer elastischer Pools in der SQL-Datenbank mithilfe von PowerShell und Power BI](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) im GitHub-Beispielrepository für SQL Server finden Sie einen Satz von Powershell-Skripts und T-SQL-Abfragen sowie Dokumentation zu Funktionen und Verwendung.

## Erste Schritte mit elastischen Pools in einem SaaS-Szenario

Dieser Lösungsschnellstart bietet eine Lösung für ein SaaS-Szenario (Software-as-a-Solution), das elastische Pools nutzt, um ein kostengünstiges, skalierbares Datenbank-Back-End einer SaaS-Anwendung bereitzustellen. Im Rahmen dieser Lösung werden Sie durch die Implementierung einer Web-App geführt, mit der Sie die durch einen Last-Generator auf einen elastischen Pool ausgeübte Last anhand eines benutzerdefinierten Dashboards, das das Azure-Portal ergänzt, visualisieren können.

Im GitHub-Beispielrepository zu SQL Server finden Sie unter [Elastic Pool Custom Dashboard for Saas](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) (Benutzerdefiniertes Dashboard für SaaS mit elastischem Pool) eine Lastgenerator- und eine Überwachungs-Web-App sowie Dokumentation zu Funktionen und Verwendung.

## Erstellen einer Azure SQL-Datenbank unter Verwendung von Entity Framework und Code First-Entwicklung

Dieses Video und das dazugehörige Beispiel bieten eine Einführung in die Code First-Entwicklung für eine neue Datenbank. Das Szenario umfasst die zielgerichtete Entwicklung für eine Datenbank, die noch nicht vorhanden ist und mittels Code First erstellt wird, bzw. für eine leere Datenbank, der mit Code First neue Tabellen hinzugefügt werden. Mit Code First können Sie Modelle unter Verwendung von C#- oder VB.NET-Klassen definieren. Optional können zusätzliche Konfigurationsschritte ausgeführt werden – beispielsweise durch Anwendung von Attributen auf Klassen und Eigenschaften oder durch die Verwendung einer Fluent-API. Weitere Informationen finden Sie unter [Code First für eine neue Datenbank](https://msdn.microsoft.com/data/jj193542.aspx).

## Integrieren von Tools für elastische Datenbanken in eine Entity Framework-Anwendung

Dieses Beispiel zeigt, welche Änderungen in einer Entity Framework-Anwendung erforderlich sind, damit diese die Funktionen der [Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md) nutzen kann. Der Schwerpunkt liegt auf der Erstellung der [Shard-Zuordnungsverwaltung](sql-database-elastic-scale-shard-map-management.md) und des [datenabhängigen Routings](sql-database-elastic-scale-data-dependent-routing.md) mit dem Code First-Ansatz von Entity Framework. Das [Code First-Beispiel für eine neue Datenbank für EF](http://msdn.microsoft.com/data/jj193542.aspx) wird im gesamten Dokument als Beispiel verwendet. Der zu diesem Dokument gehörige Beispielcode ist Teil der Beispielserie der Tools für elastische Datenbanken in den Visual Studio-Codebeispielen. Weitere Informationen finden Sie unter [Clientbibliothek für elastische Datenbanken mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md).

## Mehrinstanzenfähige Anwendungen mit elastischen Datenbanktools und zeilenbasierter Sicherheit

Dieses Beispiel zeigt, welche Änderungen in einer Entity Framework-Anwendung erforderlich sind, damit [Tools für elastische Datenbanken](sql-database-elastic-scale-get-started.md) mit [Sicherheit auf Zeilenebene](https://msdn.microsoft.com/library/dn765131) integriert werden können. Dieses Beispiel veranschaulicht, wie die Technologien zusammen verwendet werden, um eine Anwendung mit einer hochgradig skalierbaren Datenebene zu erstellen, die mehrinstanzenfähige Shards mit ADO.NET SqlClient und/oder Entity Framework unterstützt. Dieses Beispiel erweitert die [Clientbibliothek für elastische Datenbanken mit Entity Framework](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) durch Hinzufügen von Unterstützung für Shard-Datenbanken mit mehreren Mandanten. Hierzu wird eine einfache Konsolenanwendung zum Erstellen von Blogs und Beiträgen mit vier Mandanten und zwei Shard-Datenbanken mit mehreren Mandanten erstellt. Weitere Informationen finden Sie unter [Mehrinstanzenfähige Anwendungen mit elastischen Datenbanktools und zeilenbasierter Sicherheit](sql-database-elastic-tools-multi-tenant-row-level-security.md).

## Tailspin-Anwendung „Surveys“

Bei diesem Beispiel handelt es sich um eine mehrinstanzenfähige Webanwendung namens „Surveys“, die Benutzern die Erstellung von Onlineumfragen ermöglicht. Das Beispiel veranschaulicht einige wichtige Aspekte der Verwaltung von Benutzeridentitäten in einer mehrinstanzenfähigen Anwendung (einschließlich Registrierung, Authentifizierung, Autorisierung und App-Rollen). Informationen zum Ausführen dieses Beispiels finden Sie unter [How to run the Tailspin Surveys sample application](https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/docs/running-the-app.md) (Gewusst wie: Ausführen der Tailspin-Beispielanwendung „Surveys“).

## Contoso Clinic-Demoanwendung

In diesem Beispiel werden die Sicherheitsfeatures von Azure SQL-Datenbank (V12) vorgestellt. Um dieses Beispiel auszuführen, wechseln Sie zu [Contoso Clinic Demo Application](https://github.com/Microsoft/azure-sql-security-sample) (Contoso Clinic-Demoanwendung).

## Nächste Schritte

[Kennenlernen der Azure SQL-Datenbank-Tutorials](sql-database-explore-tutorials.md)

<!---HONumber=AcomDC_0720_2016-->