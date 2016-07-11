<properties 
   pageTitle="Azure SQL-Datenbankratgeber" 
   description="Der Azure SQL-Datenbankratgeber stellt Empfehlungen für Ihre vorhandenen SQL-Datenbanken bereit, die die aktuelle Abfrageleistung verbessern können." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="06/23/2016"
   ms.author="sstein"/>

# SQL-Datenbankratgeber

> [AZURE.SELECTOR]
- [SQL-Datenbankratgeber – Übersicht](sql-database-advisor.md)
- [Portal](sql-database-advisor-portal.md)

Azure SQL-Datenbank stellt Empfehlungen zum Erstellen und Löschen von Indizes, zum Parametrisieren von Abfragen und zum Beheben von Schemaproblemen bereit. Der SQL-Datenbankratgeber bewertet die Leistung durch eine Analyse des Nutzungsverlaufs Ihrer SQL-Datenbank. Die Empfehlungen, die sich am besten für die Ausführung der typischen Workload Ihrer Datenbank eignen, werden angezeigt.

Die folgenden Empfehlungen sind für V12-Server verfügbar. (Für V11-Server stehen keine Empfehlungen zur Verfügung.) Derzeit können Sie festlegen, dass die Empfehlungen zum Erstellen und Löschen von Indizes automatisch angewendet werden. Informationen hierzu finden Sie unter [Automatische Indexverwaltung](sql-database-advisor-portal.md#enable-automatic-index-management).

## Empfehlungen zum Erstellen eines Index 

Empfehlungen zum **Erstellen eines Index** werden angezeigt, wenn der SQL-Datenbankdienst erkennt, dass ein Index fehlt, von dem die Workload Ihrer Datenbanken profitieren könnte (nur nicht gruppierte Indizes).

## Empfehlungen zum Löschen eines Index

Empfehlungen zum **Löschen eines Index** werden angezeigt, wenn der SQL-Datenbankdienst doppelt vorhandene Indizes erkennt (derzeit in der Vorschau, gilt nur für doppelt vorhandene Indizes).

## Empfehlungen zum Parametrisieren von Abfragen

Empfehlungen zum **Parametrisieren von Abfragen** werden angezeigt, wenn der SQL-Datenbankdienst erkennt, dass eine oder mehrere Abfragen ständig neu kompiliert werden, aber am Ende immer derselbe Abfrageausführungsplan daraus hervorgeht. Dadurch ergibt sich die Möglichkeit zum Anwenden einer erzwungenen Parametrisierung, sodass Abfragepläne für eine verbesserte Leistung und eine geringere Ressourcennutzung zwischengespeichert und wiederverwendet werden.

Jede an SQL Server gerichtete Abfrage muss zunächst kompiliert werden, um einen Ausführungsplan zu erstellen, der zum Ausführen der Abfrage verwendet wird. Alle generierten Pläne werden dem Plancache hinzugefügt, und bei erneuter Ausführung derselben Abfrage kann dieser Plan aus dem Cache wiederverwendet werden. Dadurch wird eine erneute Kompilierung überflüssig.

Anwendungen, die Abfragen mit nicht parametrisierten Werten enthalten, können den nötigen Leistungsaufwand erhöhen, weil für jede dieser Abfragen mit unterschiedlichen Parameterwerten jeweils ein neuer Ausführungsplan kompiliert werden muss. In vielen Fällen werden für die gleichen Abfragen mit unterschiedlichen Parameterwerten die gleichen Ausführungspläne generiert, aber diese Pläne werden alle separat dem Plancache hinzugefügt. Diese erneuten Kompilierungsvorgänge nutzen Datenbankressourcen, erhöhen die Abfragedauer und überfluten den Plancache, wodurch Pläne aus dem Cache verdrängt werden. Sie können das entsprechende Verhalten von SQL Server ändern, indem Sie für die Datenbank die Option für erzwungene Parametrisierung festlegen.

Damit Sie die Auswirkungen dieser Empfehlung besser abschätzen können, wird Ihnen ein Vergleich zwischen der tatsächlichen CPU-Auslastung und der prognostizierten CPU-Auslastung nach Anwendung der Empfehlung angezeigt. Neben der geringeren CPU-Auslastung profitieren Sie auch von einer Verkürzung der Abfragedauer um die Zeit, die normalerweise für die Kompilierung benötigt würde. Außerdem wird auch der Plancache deutlich weniger beansprucht, sodass die meisten Pläne im Cache bleiben und wiederverwendet werden können. Sie können diese Empfehlung schnell und einfach umsetzen, indem Sie auf den Befehl „Anwenden“ klicken.

Nach dem Anwenden dieser Empfehlung wird die erzwungene Parametrisierung innerhalb weniger Minuten für Ihre Datenbank aktiviert. Anschließend startet ein Überwachungsprozess, der ungefähr 24 Stunden läuft. Wenn dieser Zeitraum verstrichen ist, können Sie einen Überprüfungsbericht anzeigen. Dieser enthält Daten zur CPU-Auslastung für die 24 Stunden vor und nach dem Anwenden der Empfehlung. Der SQL-Datenbankratgeber verfügt über einen Sicherheitsmechanismus, der die angewendete Empfehlung wieder zurücknimmt, falls eine Leistungsregression beobachtet wird.

## Empfehlungen zum Beheben von Schemaproblemen

Empfehlungen zum **Beheben von Schemaproblemen** werden angezeigt, wenn der SQL-Datenbankdienst eine Anomalie in der Anzahl von schemabezogenen SQL-Fehlern erkennt, die in Ihrer Azure SQL-Datenbank auftreten. Diese Empfehlung tritt in der Regel auf, wenn Ihre Datenbank innerhalb einer Stunde mehrere schemabezogene Fehler erkennt (ungültiger Spaltenname, ungültiger Objektname usw.).

Bei Schemaproblemen handelt es sich um eine Klasse von Syntaxproblemen in SQL Server, die auftreten, wenn die Definition einer SQL-Abfrage nicht auf die Definition des Datenbankschemas abgestimmt ist (z.B. weil eine der von der Abfrage erwarteten Spalten in der Zieltabelle fehlt oder umgekehrt).

Empfehlungen zum Beheben von Schemaproblemen werden angezeigt, wenn der Azure SQL-Datenbankdienst eine Anomalie in der Anzahl von schemabezogenen SQL-Fehlern erkennt, die in Ihrer Azure SQL-Datenbank auftreten. In der folgenden Tabelle sind Fehler mit Bezug zu Schemaproblemen aufgeführt:

|SQL-Fehlercode|Message|
|--------------|-------|
|201|Die *'*'*-Prozedur oder -Funktion erwartet den '*'-Parameter, der nicht bereitgestellt wurde.|
|207|Ungültiger Spaltenname '*'.|
|208|Ungültiger Objektname '*'. |
|213|Der Spaltenname oder die Anzahl der bereitgestellten Werte entspricht nicht der Tabellendefinition. |
|2812|Die gespeicherte Prozedur '*' wurde nicht gefunden. |
|8144|Für die '*'-Prozedur oder -Funktion wurden zu viele Argumente angegeben. |

## Nächste Schritte

Überwachen Sie Ihre Empfehlungen, und wenden Sie sie weiterhin an, um die Leistung zu optimieren. Datenbankworkloads sind dynamisch und ändern sich ständig. Der SQL-Datenbankratgeber setzt die Überwachung fort und bietet Empfehlungen, durch die die Leistung Ihrer Datenbank erhöht werden kann.

 - Unter [SQL-Datenbankratgeber im Azure-Portal](sql-database-advisor-portal.md) finden Sie Schritte zum Verwenden des SQL-Datenbankratgebers im Azure-Portal.
 - Unter [Query Performance Insight](sql-database-query-performance.md) erfahren Sie, wie Sie die Auswirkungen Ihrer wichtigsten Abfragen auf die Leistung untersuchen können.

## Zusätzliche Ressourcen

- [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)
- [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
- [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md)

<!---HONumber=AcomDC_0629_2016-->