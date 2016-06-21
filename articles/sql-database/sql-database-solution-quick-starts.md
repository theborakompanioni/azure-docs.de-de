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
   ms.date="06/01/2016"
   ms.author="carlrab"/>

# Übersicht über die Schnellstarts zur Azure SQL-Datenbanklösung

Dieser Artikel bietet eine Übersicht über die Schnellstarts zur Azure SQL-Datenbanklösung. Diese Schnellstarts veranschaulichen den Einsatz der SQL-Datenbank in einer vollständigen Lösung anhand von Anwendungsbeispielen. Einfache Schritt-für-Schritt-Tutorials, die die Verwendung eines bestimmten Azure SQL-Datenbankfeatures veranschaulichen, finden Sie unter [Kennenlernen der SQL-Datenbank-Tutorials](sql-database-explore-tutorials.md).

## Sammeln und Überwachen von Ressourcennutzungsdaten über mehrere Pools hinweg

Dieser Lösungsschnellstart bietet eine Lösung für die Erfassung und Überwachung der Azure SQL-Datenbank-Ressourcennutzung über mehrere Pools in einem Abonnement hinweg. Wenn Sie über eine große Anzahl von Datenbanken in einem Abonnement verfügen, ist es mühselig, jeden elastischen Pool einzeln zu überwachen. Um dies zu vermeiden, können Sie PowerShell-Cmdlets für die SQL-Datenbank und T-SQL-Abfragen kombinieren, um Ressourcennutzungsdaten aus mehreren Pools und den zugehörigen Datenbanken zu sammeln und eine Überwachung und Analyse der Ressourcennutzung durchzuführen.

Unter [Verwalten mehrerer elastischer Pools in der SQL-Datenbank mithilfe von PowerShell und Power BI](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) im GitHub-Beispielrepository für SQL Server finden Sie einen Satz von Powershell-Skripts und T-SQL-Abfragen sowie Dokumentation zu Funktionen und Verwendung.

## Erste Schritte mit elastischen Pools in einem SaaS-Szenario

Dieser Lösungsschnellstart bietet eine Lösung für ein SaaS-Szenario (Software-as-a-Solution), das elastische Pools nutzt, um ein kostengünstiges, skalierbares Datenbank-Back-End einer SaaS-Anwendung bereitzustellen. Im Rahmen dieser Lösung werden Sie durch die Implementierung einer Web-App geführt, mit der Sie die durch einen Last-Generator auf einen elastischen Pool ausgeübte Last anhand eines benutzerdefinierten Dashboards, das das Azure-Portal ergänzt, visualisieren können.

Unter [Saas-scenario-with-elastic-pools](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools) im GitHub-Beispielrepository zu SQL Server finden Sie eine Last-Generator- und eine Überwachungs-Web-App sowie Dokumentation zu Funktionen und Verwendung.

## Nächste Schritte

[Kennenlernen der Azure SQL-Datenbank-Tutorials](sql-database-explore-tutorials.md)

<!---HONumber=AcomDC_0608_2016-->