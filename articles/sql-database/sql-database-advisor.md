---
title: "Empfehlungen zur Optimierung der Abfrageleistung – Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Der Azure SQL-Datenbankratgeber stellt Empfehlungen für Ihre vorhandenen SQL-Datenbanken bereit, die die aktuelle Abfrageleistung verbessern können."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 1db441ff-58f5-45da-8d38-b54dc2aa6145
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/30/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: cf627b92399856af2b9a58ab155fac6730128f85
ms.openlocfilehash: a8d0b08abc7e3c688f9ab79499b3459b33f06848


---
# <a name="sql-database-advisor"></a>SQL-Datenbankratgeber

Azure SQL-Datenbank lernt mit Ihrer Anwendung und passt sich daran an. Darüber hinaus bietet der Dienst benutzerdefinierte Empfehlungen, mit denen Sie die Leistung Ihrer SQL-Datenbanken maximieren können. Der SQL Database Advisor stellt Empfehlungen zum Erstellen und Löschen von Indizes, zum Parametrisieren von Abfragen und zum Beheben von Schemaproblemen bereit. Der Ratgeber bewertet die Leistung durch eine Analyse des Nutzungsverlaufs Ihrer SQL-Datenbank. Die Empfehlungen, die sich am besten für die Ausführung der typischen Workload Ihrer Datenbank eignen, werden angezeigt. 

Die folgenden Empfehlungen sind für Azure SQL-Datenbankserver verfügbar. Derzeit können Sie festlegen, dass die Empfehlungen zum Erstellen und Löschen von Indizes automatisch angewendet werden. Informationen hierzu finden Sie unter [Automatische Indexverwaltung](sql-database-advisor-portal.md#enable-automatic-index-management).

## <a name="create-index-recommendations"></a>Empfehlungen zum Erstellen eines Index
**Erstellen eines Index** werden angezeigt, wenn der SQL-Datenbankdienst erkennt, dass ein Index fehlt, von dem die Workload Ihrer Datenbanken profitieren könnte (nur nicht gruppierte Indizes).

## <a name="drop-index-recommendations"></a>Empfehlungen zum Löschen eines Index
**Löschen eines Index** werden angezeigt, wenn der SQL-Datenbankdienst doppelt vorhandene Indizes erkennt (derzeit in der Vorschau, gilt nur für doppelt vorhandene Indizes).

## <a name="parameterize-queries-recommendations"></a>Empfehlungen zum Parametrisieren von Abfragen
Empfehlungen zum **Parametrisieren von Abfragen** werden angezeigt, wenn eine oder mehrere Abfragen ständig neu kompiliert werden, aber am Ende immer derselbe Abfrageausführungsplan daraus hervorgeht. Diese Bedingung eröffnet die Möglichkeit zum Anwenden einer erzwungenen Parametrisierung, sodass Abfragepläne zwischengespeichert und wiederverwendet werden, um die Leistung zu steigern und die Ressourcennutzung zu reduzieren. 

Jede an SQL Server gerichtete Abfrage muss zunächst kompiliert werden, um einen Ausführungsplan zu erstellen. Alle generierten Pläne werden dem Plancache hinzugefügt, und bei erneuter Ausführung derselben Abfrage kann dieser Plan aus dem Cache wiederverwendet werden. Dadurch wird eine erneute Kompilierung überflüssig. 

Anwendungen, die Abfragen mit nicht parametrisierten Werten senden, können den nötigen Leistungsaufwand erhöhen, weil für jede dieser Abfragen mit unterschiedlichen Parameterwerten jeweils ein neuer Ausführungsplan kompiliert werden muss. In vielen Fällen werden für die gleichen Abfragen mit unterschiedlichen Parameterwerten die gleichen Ausführungspläne generiert, aber diese Pläne werden dem Plancache separat hinzugefügt. Die Neukompilierung von Ausführungsplänen verbraucht Datenbankressourcen, erhöht die Abfragedauer und führt zu einem Überlauf des Plancache, wodurch Pläne aus dem Cache verdrängt werden. Sie können das entsprechende Verhalten von SQL Server ändern, indem Sie für die Datenbank die Option für erzwungene Parametrisierung festlegen. 

Damit Sie die Auswirkungen dieser Empfehlung besser abschätzen können, wird Ihnen ein Vergleich zwischen der tatsächlichen CPU-Auslastung und der prognostizierten CPU-Auslastung nach Anwendung der Empfehlung angezeigt. Neben der geringeren CPU-Auslastung profitieren Sie auch von einer Verkürzung der Abfragedauer um die Zeit, die normalerweise für die Kompilierung benötigt würde. Außerdem wird auch der Plancache deutlich weniger beansprucht, sodass die meisten Pläne im Cache bleiben und wiederverwendet werden können. Sie können diese Empfehlung schnell und einfach umsetzen, indem Sie auf den Befehl **Anwenden** klicken. 

Nach dem Anwenden dieser Empfehlung wird die erzwungene Parametrisierung innerhalb weniger Minuten für Ihre Datenbank aktiviert. Anschließend startet ein Überwachungsprozess, der ungefähr 24 Stunden ausgeführt wird. Nach Ablauf dieses Zeitraums können Sie einen Überprüfungsbericht anzeigen, der die CPU-Auslastung Ihrer Datenbank für die 24 Stunden vor und nach dem Anwenden der Empfehlung zeigt. Der SQL Database Advisor verfügt über einen Sicherheitsmechanismus, der die angewendete Empfehlung automatisch wieder zurücknimmt, falls ein Absinken der Leistung beobachtet wird.

## <a name="fix-schema-issues-recommendations"></a>Empfehlungen zum Beheben von Schemaproblemen
Empfehlungen zum **Beheben von Schemaproblemen** werden angezeigt, wenn der SQL-Datenbankdienst eine Anomalie in der Anzahl von schemabezogenen SQL-Fehlern erkennt, die in Ihrer Azure SQL-Datenbank auftreten. Diese Empfehlung wird in der Regel angezeigt, wenn in Ihrer Datenbank innerhalb einer Stunde mehrere schemabezogene Fehler auftreten (ungültiger Spaltenname, ungültiger Objektname usw.).

Bei Schemaproblemen handelt es sich um eine Klasse von Syntaxproblemen in SQL Server, die auftreten, wenn die Definition einer SQL-Abfrage nicht auf die Definition des Datenbankschemas abgestimmt ist. Dies kann z.B. der Fall sein, wenn eine der von der Abfrage erwarteten Spalten in der Zieltabelle fehlt oder umgekehrt. 

Empfehlungen zum Beheben von Schemaproblemen werden angezeigt, wenn der Azure SQL-Datenbankdienst eine Anomalie in der Anzahl von schemabezogenen SQL-Fehlern erkennt, die in Ihrer Azure SQL-Datenbank auftreten. Die folgende Tabelle zeigt die möglichen Fehler in Bezug auf Schemaprobleme:

| SQL-Fehlercode | Message |
| --- | --- |
| 201 |Die*'*'*-Prozedur oder -Funktion erwartet den '*'-Parameter, der nicht bereitgestellt wurde. |
| 207 |Ungültiger Spaltenname '*'. |
| 208 |Ungültiger Objektname '*'. |
| 213 |Der Spaltenname oder die Anzahl der bereitgestellten Werte entspricht nicht der Tabellendefinition. |
| 2812 |Die gespeicherte Prozedur '*' wurde nicht gefunden. |
| 8144 |Für die '*'-Prozedur oder -Funktion wurden zu viele Argumente angegeben. |

## <a name="next-steps"></a>Nächste Schritte
Überwachen Sie Ihre Empfehlungen, und wenden Sie sie weiterhin an, um die Leistung zu optimieren. Datenbankworkloads sind dynamisch und ändern sich ständig. Der SQL Database Advisor setzt die Überwachung fort und bietet Empfehlungen, mit denen sich die Leistung Ihrer Datenbank erhöhen lässt. 

* Unter [SQL-Datenbankratgeber im Azure-Portal](sql-database-advisor-portal.md) finden Sie Schritte zum Verwenden des SQL-Datenbankratgebers im Azure-Portal.
* Unter [Query Performance Insight](sql-database-query-performance.md) erfahren Sie, wie Sie die Auswirkungen Ihrer wichtigsten Abfragen auf die Leistung anzeigen und untersuchen können.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)
* [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-configure.md)




<!--HONumber=Feb17_HO1-->


