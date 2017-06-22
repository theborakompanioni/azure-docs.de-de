---
title: "Ausführen von Ad-hoc-Analyseabfragen für mehrere Azure SQL-Datenbanken | Microsoft-Dokumentation"
description: "Führen Sie in der mehrinstanzenfähigen Wingtip-SaaS-App Ad-hoc-Analyseabfragen über mehrere SQL-Datenbanken hinweg durch."
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: bf003a3677ed27bc833de59ef61f7637a6899d37
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---
# <a name="run-ad-hoc-analytics-queries-across-all-wingtip-saas-tenants"></a>Ausführen von Ad-hoc-Analyseabfragen über alle Wingtip-SaaS-Mandanten hinweg

In diesem Tutorial erstellen Sie eine Datenbank für Ad-hoc-Analysen und führen mehrere Abfragen für alle Mandanten aus. Diese Abfragen können Einblicke geben, die sonst in den tagtäglichen operativen Daten der Wingtip-SaaS-App verborgen bleiben.

Zum Ausführen von Ad-hoc-Analyseabfragen (für mehrere Mandanten) verwendet die Wingtip-SaaS-App eine [elastische Abfrage](sql-database-elastic-query-overview.md) zusammen mit einer Analysedatenbank.


In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]

> * Informationen zu den globalen Sichten in jeder Datenbank, die mandantenübergreifende Abfragen ermöglichen
> * Bereitstellen einer Ad-hoc-Analysedatenbank
> * Ausführen verteilter Abfragen für alle Mandantendatenbanken



Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Wingtip-SaaS-App wird bereitgestellt. Unter [Bereitstellen und Kennenlernen einer mehrinstanzenfähigen SaaS-Anwendung, die Azure SQL-Datenbank verwendet](sql-database-saas-tutorial.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).


## <a name="ad-hoc-analytics-pattern"></a>Ad-hoc-Analysemuster

Einer der großen Vorteile von SaaS-Anwendungen ist, dass Sie die riesige Menge von zentral gespeicherten Mandantendaten in der Cloud verwenden können. Sie können diese Daten nutzen, um Erkenntnisse über den Betrieb und die Verwendung Ihrer Anwendungen, Ihre Mandanten, die dazugehörigen Benutzer und Einstellungen, das Verhalten usw. zu gewinnen. An diesen gewonnenen Erkenntnissen können Sie sich bei der Entwicklung von Funktionen, Verbesserungen der Benutzerfreundlichkeit und anderen Investitionen in Ihre Apps und Dienste orientieren.

Es ist einfach, auf diese Daten in einer Datenbank mit mehreren Mandanten zuzugreifen, aber weniger einfach bei einer Verteilung über potentiell Tausende von Datenbanken. Ein Ansatz besteht in der Verwendung flexibler Abfragen, wodurch Ad-hoc-Abfragen für einen verteilten Satz von Datenbanken mit gemeinsamem Schema ermöglicht werden. Flexible Abfragen verwenden eine einzige *Hauptdatenbank*, in der externe Tabellen definiert werden, die Tabellen oder Sichten in den verteilten Datenbanken (Mandantendatenbanken) spiegeln. Abfragen, die an diese Hauptdatenbank übermittelt werden, werden kompiliert, um einen Plan der verteilten Abfrage zu erzeugen, bei dem Teile der Abfrage nach Bedarf per Push an die Mandantendatenbanken übertragen werden. Flexible Abfragen verwenden die Shardzuordnung in der Katalogdatenbank, um den Speicherort der Mandantendatenbanken bereitzustellen. Für Setup und Abfragen wird einfach gewöhnlicher [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)-Code verwendet, und Ad-hoc-Abfragen von Tools wie Power BI und Excel werden unterstützt.

## <a name="get-the-wingtip-application-scripts"></a>Abrufen des Wingtip-Anwendungsskripts

Die Wingtip-SaaS-Skripts und der Quellcode der Anwendung stehen im GitHub-Repository [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) zur Verfügung. [Schritte zum Herunterladen der Wingtip-SaaS-Skripts](sql-database-wtp-overview.md#download-the-wingtip-saas-scripts)


## <a name="explore-the-global-views"></a>Untersuchen der globalen Sichten

Die Wingtip-SaaS-Anwendung wird basierend auf dem Modell „ein Mandant pro Datenbank“ erstellt, sodass das Mandantendatenbank-Schema aus Sicht eines einzelnen Mandanten definiert wird. Die mandantenspezifischen Informationen sind in einer Tabelle (*Venue*) vorhanden, die immer über eine einzelne Zeile verfügt und außerdem als Heap ohne Primärschlüssel konzipiert ist.  Andere Tabellen im Schema müssen nicht mit der Tabelle *Venue* verknüpft werden, da bei normaler Nutzung kein Zweifel besteht, zu welchem Mandanten die Daten gehören.  Beim Durchführen von Abfragen übergreifend für alle Datenbanken ist das Korrelieren von Daten aus Tabellen in der Datenbank auf einem bestimmten Mandanten aber wichtig. Zur Vereinfachung dieses Vorgangs wird der Mandantendatenbank eine Gruppe von Sichten hinzugefügt, die eine „globale“ Sicht jedes Mandanten ermöglichen. Diese globalen Sichten projizieren eine Mandanten-ID in jede Tabelle, für die globale Abfragen durchgeführt werden. Auf diese Weise können Daten jedes Mandanten leicht identifiziert werden. Zur Vereinfachung wurden diese Sichten in allen Mandantendatenbanken vorab erstellt (sowie auch die „goldene“ Datenbank, damit diese globalen Sichten als neue Mandanten verfügbar sind).

1. Öffnen Sie SSMS, und [stellen Sie eine Verbindung mit dem Server „tenants1-&lt;BENUTZER&gt;“ her](sql-database-wtp-overview.md#explore-database-schema-and-execute-sql-queries-using-ssms).
1. Erweitern Sie die Option **Datenbanken**, klicken Sie mit der rechten Maustaste auf **contosoconcerthall**, und wählen Sie **Neue Abfrage**.
1. Führen Sie die folgenden Abfragen aus, um den Unterschied zwischen den Tabellen mit einem einzelnen Mandanten und den globalen Sichten zu untersuchen:

   ```T-SQL
   -- This is the base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice the plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- The base Events table which has no VenueId column.
   SELECT * FROM Events

   -- This view projects the VenueId retrieved from the Venues table.
   SELECT * FROM VenueEvents
   ```

In der Beispieldatenbank haben wir eine ganzzahlige ID aus einem Hash des Namens des Veranstaltungsorts errechnet. Für die Einführung eines eindeutigen Werts kann jedoch jeder Ansatz verwendet werden. Dieser Vorgang ähnelt der Erstellung des Mandantenschlüssels im Katalog. Es ist jedoch keine Voraussetzung, dass Katalogschlüssel und Mandanten-ID in der *adhocanalytics*-Datenbank identisch sind.

Gehen Sie wie folgt vor, um eine *Sicht* zu untersuchen und sich über die Erstellung zu informieren:

1. Erweitern Sie im **Objekt-Explorer** die Option **contosoconcethall** > **Sichten**:

   ![Sichten](media/sql-database-saas-tutorial-adhoc-analytics/views.png)

1. Klicken Sie mit der rechten Maustaste auf **dbo.Venues**.
1. Wählen Sie **Skript für Sicht als** > **CREATE in** > **Neues Abfrage-Editor-Fenster**.

Führen Sie dies für alle *Sichten* durch, um Informationen zur Erstellung zu erhalten.

## <a name="deploy-the-database-used-for-ad-hoc-analytics-queries"></a>Bereitstellen der für Ad-hoc-Analyseabfragen verwendeten Datenbank

In dieser Übung wird die Datenbank *adhocanalytics* bereitgestellt. Diese Datenbank enthält das Schema, das zum Abfragen aller Mandantendatenbanken verwendet wird. Die Datenbank wird auf dem vorhandenen Katalogserver bereitgestellt. Dies ist der Server, der alle verwaltungsbezogenen Datenbanken enthält.

1. Öffnen Sie „...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Deploy-AdhocAnalyticsDB.ps1*“.
1. Scrollen Sie nach unten zum Abschnitt, in dem `$commandText` dem SQL-Skript zugewiesen wird. Prüfen Sie das Skript, und achten Sie auf Folgendes:

   1. Für die elastische Abfrage werden datenbankbezogene Anmeldeinformationen verwendet, um auf die einzelnen Mandantendatenbanken zuzugreifen. Diese Anmeldeinformationen müssen in allen Datenbanken verfügbar sein, und im Normalfall sollten die Rechte gewährt werden, die zum Aktivieren dieser Ad-hoc-Abfragen mindestens erforderlich sind.
   1. Die externe Datenquelle, die für die Verwendung der Mandanten-Shardzuordnung in der Katalogdatenbank definiert ist.  Indem sie als externe Datenquelle verwendet wird, werden Abfragen auf alle Datenbanken verteilt, die bei Ausgabe der Abfrage im Katalog registriert sind.
   1. Die externen Tabellen, die auf die globalen Sichten aus dem vorherigen Abschnitt verweisen.
   1. Die lokale Tabelle *VenueTypes*, die erstellt und mit Daten gefüllt wird.  Da diese Referenzdatentabelle in allen Mandantendatenbanken enthalten ist, kann sie hier als lokale Tabelle dargestellt werden. Für einige Abfragen kann dies zu einer Verringerung der Datenmenge führen, die zwischen den Mandantendatenbanken und der Datenbank *adhocanalytics* verschoben wird.


1. Öffnen Sie nun „...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1*“ in der *PowerShell ISE*, und legen Sie folgende Werte fest:
   * **$DemoScenario** = 2, **Ad-hoc-Analysedatenbank bereitstellen**.

1. Drücken Sie **F5**, um das Skript auszuführen und die Datenbank *adhocanalytics* zu erstellen.

   Es ist in Ordnung, hier Warnungen vom Typ *Der RPC-Server ist nicht verfügbar* zu ignorieren.

Sie verfügen nun über eine *adhocanalytics*-Datenbank, mit der Sie verteilte Abfragen ausführen und Einblicke in alle Mandanten erhalten können.

![adhocanalytics-Datenbank](media/sql-database-saas-tutorial-adhoc-analytics/adhocanalytics.png)

## <a name="run-ad-hoc-analytics-queries"></a>Ausführen von Ad-hoc-Analyseabfragen

Führen Sie nach der Einrichtung der *adhocanalytics*-Datenbank einige Ad-hob-Abfragen aus:

1. Öffnen Sie ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* in SSMS.
1. Stellen Sie sicher, dass Sie mit der **adhocanalytics**-Datenbank verbunden sind.
1. Wählen Sie die Abfrage aus, die Sie ausführen möchten, und drücken Sie **F5**:

    ![query](media/sql-database-saas-tutorial-adhoc-analytics/query.png)




## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Bereitstellen einer Ad-hoc-Analysedatenbank
> * Ausführen verteilter Abfragen für alle Mandantendatenbanken

Absolvieren des [Tutorials zu Mandantenanalysen](sql-database-saas-tutorial-tenant-analytics.md)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* Zusätzliche [Tutorials, die auf der Wingtip-SaaS-Anwendung aufbauen](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastische Abfrage](sql-database-elastic-query-overview.md)

