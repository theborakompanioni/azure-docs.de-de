---
title: "Empfehlungen zur Leistung – Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Azure SQL-Datenbankratgeber gibt Empfehlungen für Ihre SQL-Datenbanken, die die aktuelle Abfrageleistung verbessern können."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 1db441ff-58f5-45da-8d38-b54dc2aa6145
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 357a25a665894c86ddb0f93beeb4dd59d8837489
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="performance-recommendations"></a>Empfehlungen zur Leistung

Azure SQL-Datenbank lernt mit Ihrer Anwendung und passt sich daran an. Darüber hinaus bietet der Dienst benutzerdefinierte Empfehlungen, mit denen Sie die Leistung Ihrer SQL-Datenbanken maximieren können. Die Leistung kann kontinuierlich durch die Analyse Ihres Nutzungsverlauf von SQL-Datenbank bewertet werden. Die Empfehlungen, die gegeben werden, basieren auf einem für die Datenbank eindeutigen Workloadmuster und tragen zur Verbesserung der Leistung bei.

> [!NOTE]
> Es wird empfohlen, die Empfehlungen durch das Aktivieren von „Automatische Optimierung“ in Ihrer Datenbank zu verwenden. Weitere Informationen finden Sie unter [Automatic tuning (Automatische Optimierung)](sql-database-automatic-tuning.md).
>

## <a name="create-index-recommendations"></a>Empfehlungen zum Erstellen eines Index
Azure SQL-Datenbank überwacht kontinuierlich die ausgeführten Abfragen und erkennt die Indizes, die die Leistung verbessern könnten. Sobald es genügend Vertrauen gibt, das ein bestimmter Index fehlt, wird eine neue Empfehlung **Index erstellen** erstellt. Azure SQL-Datenbank baut Vertrauen auf, indem berechnet wird, welche Leistungsgewinn der Index mit der Zeit bringen würde. Je nach geschätztem Leistungsgewinn werden Empfehlungen als Hoch, Mittel oder Niedrig eingestuft. 

Indizes, die mit Empfehlungen erstellt wurden, sind immer als „auto_created“-Indizes gekennzeichnet. Sie können in der Ansicht „sys.indexes“ sehen, welche Indizes „auto_created“ sind. Automatisch erstellte Indizes blockieren keine ALTER/RENAME-Befehle. Wenn Sie versuchen, eine Spalte zu löschen, die einen automatisch erstellten Index hat, läuft der Befehl weiter und der automatisch erstellte Index wird ebenso durch den Befehl gelöscht. Reguläre Indizes würden den Befehl ALTER/RENAME für Spalten blockieren, die indiziert sind.

Nachdem die Empfehlung zum Erstellen eines Indexes angewendet wurde, vergleicht Azure SQL-Datenbank die Leistung der Abfragen mit der Baselineleistung. Wenn der neue Index die Leistung verbessert hat, wird die Empfehlung als erfolgreich gekennzeichnet. Zudem wird ein Auswirkungsbericht erstellt. Wenn der Index nicht zum gewünschten Ergebnis geführt hat, wird er automatisch rückgängig gemacht. Auf diese Weise stellt Azure SQL-Datenbank sicher, dass Empfehlungen die Leistung der Datenbank nur verbessern.

Jede **Index erstellen**-Empfehlung hat eine Backoffrichtlinie, die verhindert, dass die Empfehlung angewendet wird, wenn der DTU-Verbrauch der Datenbank oder des Pools in den letzten 20 Minuten über 80 % gelegen hat, oder wenn der Speicher über 90 % ausgelastet ist. In diesem Fall wird die Empfehlung verschoben.

## <a name="drop-index-recommendations"></a>Empfehlungen zum Löschen eines Indexes
Zusätzlich zum Erkennen eines fehlenden Indexes analysiert die Azure SQL-Datenbank kontinuierlich die Leistung der vorhandenen Indizes. Wenn der Index nicht verwendet wird, empfiehlt die Azure SQL-Datenbank, dass dieser gelöscht wird. Das Löschen eines Indexes wird in zwei Fällen empfohlen:
* Der Index ist ein Duplikat eines anderes Indexes (gleiche indizierte und enthaltene Spalte, gleiches Partitionsschema und gleiche Filter)
* Der Index wurde über einen längeren Zeitraum nicht verwendet (93 Tage)

„Index löschen“-Empfehlungen durchlaufen auch die Überprüfung nach der Implementierung. Wenn die Leistung verbessert wurde, wird ein Auswirkungsbericht erstellt. Für den Fall, dass eine Verringerung der Leistung erkannt wird, wird die Empfehlung rückgängig gemacht.


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

* Weiter Informationen zur Verwendung von Leistungsempfehlungen im Azure-Portal finden Sie unter [Performance recommendations in the Azure portal (Leistungsempfehlungen im Azure-Portal)](sql-database-advisor-portal.md).
* Unter [Query Performance Insight](sql-database-query-performance.md) erfahren Sie, wie Sie die Auswirkungen Ihrer wichtigsten Abfragen auf die Leistung anzeigen und untersuchen können.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx)
* [CREATE INDEX](https://msdn.microsoft.com/library/ms188783.aspx)
* [Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md)


