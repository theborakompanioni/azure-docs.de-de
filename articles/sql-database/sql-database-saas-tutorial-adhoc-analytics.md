---
title: "Ausführen von Ad-hoc-Analyseabfragen für mehrere Azure SQL-Datenbanken | Microsoft-Dokumentation"
description: "Ausführen von Ad-hoc-Analyseabfragen für mehrere SQL-Datenbanken in einem mehrinstanzenfähigen App-Beispiel."
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: billgib; sstein
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: a27a65627fecf35b59122110250a40c6fe8077b5
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="run-ad-hoc-analytics-queries-across-multiple-azure-sql-databases"></a>Ausführen von Ad-hoc-Analyseabfragen für mehrere Azure SQL-Datenbanken

In diesem Tutorial können Sie Abfragen über einen Satz von Datenbanken mit mehreren Mandanten verteilen, um Ad-hoc-Analysen möglich zu machen. Mit Elastic Query können Sie verteilte Abfragen durchführen. Dazu ist die Bereitstellung einer weiteren Analysedatenbank (im Katalogserver) erforderlich. Diese Abfragen können Einblicke geben, die sonst in den tagtäglichen operativen Daten der Wingtip-SaaS-App verborgen bleiben.


In diesem Tutorial lernen Sie Folgendes kennen:

> [!div class="checklist"]

> * Informationen zu den globalen Ansichten in jeder Datenbank, die effiziente mandantenübergreifende Abfragen ermöglichen
> * Bereitstellen einer Ad-hoc-Analysedatenbank
> * Ausführen verteilter Abfragen für alle Mandantendatenbanken



Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die Wingtip-SaaS-App wird bereitgestellt. Unter [Bereitstellen und Kennenlernen einer mehrinstanzenfähigen SaaS-Anwendung, die Azure SQL-Datenbank verwendet](sql-database-saas-tutorial.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* SQL Server Management Studio (SSMS) muss installiert sein. Sie können SSMS unter [Herunterladen von SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) herunterladen und installieren.


## <a name="ad-hoc-analytics-pattern"></a>Ad-hoc-Analysemuster

Einer der großen Vorteile von SaaS-Anwendungen ist, dass Sie die riesige Menge von zentral gespeicherten Mandantendaten in der Cloud verwenden können. Mit diesen Daten können Sie Einblicke in den Betrieb und die Verwendung Ihrer Anwendung erhalten. An diesen gewonnenen Erkenntnissen können Sie sich bei der Entwicklung von Funktionen, Verbesserungen der Benutzerfreundlichkeit und anderen Investitionen in Ihre Apps und Dienste orientieren.

Es ist einfach, auf diese Daten in einer Datenbank mit mehreren Mandanten zuzugreifen, aber weniger einfach bei einer Verteilung über potentiell Tausende von Datenbanken. Ein Ansatz besteht in der Verwendung von [Elastic Query](sql-database-elastic-query-overview.md), wodurch Abfragen für einen verteilten Satz von Datenbanken mit gemeinsamem Schema ermöglicht werden. Flexible Abfragen verwenden eine einzige *Hauptdatenbank*, in der externe Tabellen definiert werden, die Tabellen oder Sichten in den verteilten Datenbanken (Mandantendatenbanken) spiegeln. Abfragen, die an diese Hauptdatenbank übermittelt werden, werden kompiliert, um einen Plan der verteilten Abfrage zu erzeugen, bei dem Teile der Abfrage nach Bedarf per Push an die Mandantendatenbanken übertragen werden. Flexible Abfragen verwenden die Shardzuordnung in der Katalogdatenbank, um den Speicherort der Mandantendatenbanken bereitzustellen. Für Setup und Abfragen wird einfach gewöhnlicher [Transact-SQL](https://docs.microsoft.com/sql/t-sql/language-reference)-Code verwendet, und Ad-hoc-Abfragen von Tools wie Power BI und Excel werden unterstützt.

Durch das Verteilen von Abfragen über Mandantendatenbanken bietet Elastic Query schnellen Einblick in Liveproduktionsdaten. Dadurch dass es möglich ist, das Elastic Query Daten aus vielen Datenbanken abruft, kann die Abfragewartezeit manchmal länger sein als für äquivalente Abfragen, die an eine einzelne Datenbank mit mehreren Mandanten ausgestellt werden. Achten Sie darauf, Ihre Abfragen so zu entwerfen, dass ein Minimum an Daten zurückgegeben wird. Elastic Query ist häufig optimal für das Abfragen kleiner Mengen von Echtzeitdaten und weniger für das Erstellen von häufig verwendeten oder komplexen Analyseabfragen oder -berichten. Wenn Abfragen nicht optimal ausgeführt werden, schauen Sie sich den [Ausführungsplan](https://docs.microsoft.com/sql/relational-databases/performance/display-an-actual-execution-plan) an, um zu sehen, welcher Teil der Abfrage per Push in die remote Datenbank übertragen wurde und wie viele Daten zurückgegeben werden. Abfragen, die eine komplexe analytische Verarbeitung erfordern, sind möglicherweise in einigen Fällen passender, da sie Mandantendaten in eine dedizierte Datenbank oder ein Data Warehouse extrahieren, die für Analyseabfragen optimal geeignet sind. Dieses Muster wird im [Tutorial zu Mandantenanalysen](sql-database-saas-tutorial-tenant-analytics.md) erklärt. 

## <a name="get-the-wingtip-application-scripts"></a>Abrufen des Wingtip-Anwendungsskripts

Die Wingtip-SaaS-Skripts und der Quellcode der Anwendung stehen im GitHub-Repository [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) zur Verfügung. [Schritte zum Herunterladen der Wingtip-SaaS-Skripts](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts)

## <a name="create-ticket-sales-data"></a>Erstellen von Ticketverkaufsdaten

Um Abfragen mit einem interessanteren Dataset auszuführen, erstellen Sie Ticketverkaufsdaten mit dem Ticketgenerator.

1. Öffnen Sie nun \\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1* in der *PowerShell ISE*, und legen Sie folgende Werte fest:
   * **$DemoScenario** = 1, **Tickets für Events an allen Veranstaltungsorten kaufen**.
2. Drücken Sie **F5**, um das Skript auszuführen und Ticketverkaufsdaten zu generieren. Während das Skript ausgeführt wird, können Sie die nächsten Schritte des Tutorials ausführen. Sie können die Ticketdaten im Abschnitt *Ausführen von verteilten Ad-hoc-Abfragen* abfragen. Warten Sie zunächst, bis der Ticketgenerator abgeschlossen wurde.

## <a name="explore-the-global-views"></a>Untersuchen der globalen Sichten

Die Wingtip-SaaS-Anwendung wird basierend auf dem Modell „ein Mandant pro Datenbank“ erstellt, sodass das Mandantendatenbank-Schema aus Sicht eines einzelnen Mandanten definiert wird. Die mandantenspezifischen Informationen sind in einer Tabelle (*Venue*) vorhanden, die immer über eine einzelne Zeile verfügt und als Heap ohne Primärschlüssel implementiert wird. Andere Tabellen im Schema müssen nicht mit der Tabelle *Venue* verknüpft werden, da bei normaler Nutzung kein Zweifel besteht, zu welchem Mandanten die Daten gehören.

Wenn Sie alle Datenbanken abfragen, ist es wichtig, dass Elastic Query die Daten so behandeln kann, als seinen Sie Teil einer einzigen logischen Datenbank, die vom Mandanten freigegeben wurde. Dieses Muster können Sie simulieren, indem Sie einen Satz von „globalen“ Ansichten der Mandantendatenbank hinzufügen, die eine Mandanten-ID in jede der global abgefragten Tabellen projizieren. Beispielsweise fügt die Ansicht *VenueEvents* eine berechnete *VenueId* in den Spalten ein, die von der Tabelle *Events* projiziert wurden. Durch das Definieren der externen Tabelle in der Hauptdatenbank mit *VenueEvents* (statt der zugrundeliegenden Tabelle *Events*) kann Elastic Query Verknüpfungen basierend auf der *VenueId* per Push übertragen, sodass diese parallel in jeder remoten Datenbank ausgeführt werden können (statt in der Hauptdatenbank). Dadurch wird die Menge an zurückgegebenen Daten deutlich verringert, sodass die Leistung von vielen Abfragen wesentlich gesteigert wird. Globale Ansichten wurden in allen Mandantendatenbanken (und in *basetenantdb*) vorgefertigt.

1. Öffnen Sie SSMS, und [stellen Sie eine Verbindung mit dem Server „tenants1-&lt;BENUTZER&gt;“ her](sql-database-wtp-overview.md#explore-database-schema-and-execute-sql-queries-using-ssms).
2. Erweitern Sie die Option **Datenbanken**, klicken Sie mit der rechten Maustaste auf **contosoconcerthall**, und wählen Sie **Neue Abfrage**.
3. Führen Sie die folgenden Abfragen aus, um den Unterschied zwischen den Tabellen mit einem einzelnen Mandanten und den globalen Sichten zu untersuchen:

   ```T-SQL
   -- The base Venue table, that has no VenueId associated.
   SELECT * FROM Venue

   -- Notice the plural name 'Venues'. This view projects a VenueId column.
   SELECT * FROM Venues

   -- The base Events table, which has no VenueId column.
   SELECT * FROM Events

   -- This view projects the VenueId retrieved from the Venues table.
   SELECT * FROM VenueEvents
   ```

In diesen Ansichten wurde *VenueId* als Hash des Veranstaltungsorts berechnet. Für die Einführung eines eindeutigen Werts kann jedoch jeder Ansatz verwendet werden. Dieser Ansatz ähnelt der Berechnung des Mandantenschlüssels zur Verwendung im Katalog.

So können Sie die Definition der Ansicht *Venues* untersuchen:

1. Erweitern Sie im **Objekt-Explorer** die Option **contosoconcethall** > **Sichten**:

   ![Sichten](media/sql-database-saas-tutorial-adhoc-analytics/views.png)

2. Klicken Sie mit der rechten Maustaste auf **dbo.Venues**.
3. Wählen Sie **Skript für Sicht als** > **CREATE in** > **Neues Abfrage-Editor-Fenster**.

Skripten Sie jede beliebige *Venue*-Ansicht, um zu sehen, wie die *VenueId* hinzugefügt wird.

## <a name="deploy-the-database-used-for-ad-hoc-distributed-queries"></a>Bereitstellen der für verteilte Ad-hoc-Abfragen verwendeten Datenbank

In dieser Übung wird die Datenbank *adhocanalytics* bereitgestellt. Dies ist die Hauptdatenbank, die das Schema enthält, das zum Abfragen aller Mandantendatenbanken verwendet wird. Die Datenbank wird auf dem vorhandenen Katalogserver bereitgestellt. Dies ist der Server, der für alle verwaltungsbezogenen Datenbanken in der Beispielanwendung verwendet wird.

1. Öffnen Sie ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalytics.ps1* in der *PowerShell ISE*, und legen Sie folgende Werte fest:
   * **$DemoScenario** = 2, **Ad-hoc-Analysedatenbank bereitstellen**.

2. Drücken Sie **F5**, um das Skript auszuführen und die Datenbank *adhocanalytics* zu erstellen.

Im nächsten Abschnitt fügen Sie das Schema in der Datenbank hinzu, damit es zum Ausführen von verteilten Abfragen verwendet werden kann.

## <a name="configure-the-database-for-running-distributed-queries"></a>Konfigurieren der Datenbank für verteilte Abfragen

In dieser Übung wird ein Schema (die externe Datenquelle und die externen Tabellendefinitionen) in die Ad-hoc-Analysedatenbank eingefügt, wodurch die Abfrage aller Mandantendatenbanken ermöglicht wird.

1. Öffnen Sie SQL Server Management Studio, und stellen Sie eine Verbindung zur Ad-hoc-Analysedatenbank her, die Sie im vorherigen Schritt erstellt haben. Der Name der Datenbank ist *adhocanalytics*.
2. Öffnen Sie ...\Learning Modules\Operational Analytics\Adhoc Analytics\ *Initialize-AdhocAnalyticsDB.sql* in SSMS.
3. Prüfen Sie das SQL-Skript, und achten Sie auf Folgendes:

   Für die elastische Abfrage werden datenbankbezogene Anmeldeinformationen verwendet, um auf die einzelnen Mandantendatenbanken zuzugreifen. Diese Anmeldeinformationen müssen in allen Datenbanken verfügbar sein, und im Normalfall sollten die Rechte gewährt werden, die zum Aktivieren dieser Ad-hoc-Abfragen mindestens erforderlich sind.

    ![erstellen von anmeldeinformationen](media/sql-database-saas-tutorial-adhoc-analytics/create-credential.png)

   Die externe Datenquelle, die für die Verwendung der Mandanten-Shardzuordnung in der Katalogdatenbank definiert ist. Indem sie als externe Datenquelle verwendet wird, werden Abfragen auf alle Datenbanken verteilt, die bei Ausgabe der Abfrage im Katalog registriert sind. Da sich Servernamen bei jeder Bereitstellung unterscheiden, ruft dieses Initialisierungsskript den Speicherort der Katalogdatenbank ab, indem es den aktuellen Server (@@servername) abruft, auf dem das Skript ausgeführt wird.

    ![erstellen einer externen datenquelle](media/sql-database-saas-tutorial-adhoc-analytics/create-external-data-source.png)

   Die externen Tabellen, die auf die globalen Sichten aus dem vorherigen Abschnitt verweisen, und die mit **DISTRIBUTION = SHARDED(VenueId)** definiert werden. Da jede *VenueId* auf eine einzelne Datenbank verweist, wird so die Leistung in vielen Szenarios verbessert, wie im nächsten Abschnitt gezeigt.

    ![erstellen externer tabellen](media/sql-database-saas-tutorial-adhoc-analytics/external-tables.png)

   Die lokale Tabelle *VenueTypes*, die erstellt und mit Daten gefüllt wird. Diese Verweisdatentabelle kommt in allen Mandantendatenbanken vor. Daher kann sie hier als lokale Tabelle dargestellt werden, die mit den gemeinsamen Daten aufgefüllt ist. Für einige Abfragen kann dies die Menge an zwischen den Mandantendatenbanken und der Datenbank *adhocanalytics* bewegten Daten verringern.

    ![tabelle erstellen](media/sql-database-saas-tutorial-adhoc-analytics/create-table.png)

   Wenn Sie so Verweistabellen einbeziehen, achten Sie darauf, dass Sie das Tabellenschema und die Tabellendaten aktualisieren, immer wenn Sie die Mandantendatenbank aktualisieren.

4. Drücken Sie **F5**, um das Skript auszuführen und die Datenbank *adhocanalytics* zu initialisieren. 

Jetzt können Sie verteilte Abfragen ausführen und Einblicke in alle Mandanten erhalten.

## <a name="run-ad-hoc-distributed-queries"></a>Ausführen von verteilten Ad-hoc-Abfragen

Jetzt wo die Datenbank *adhocanalytics* erstellt ist, können Sie testweise einige verteilte Abfragen ausführen. Ziehen Sie den Ausführungsplan mit ein, um einen besseren Überblick zu haben, wo die Verarbeitung von Abfragen stattfindet. 

Um ausführlichere Informationen zu erhalten, können Sie im Ausführungsplan auf die Plansymbole zeigen. 

Beachten Sie, dass durch das Festlegen von **DISTRIBUTION = SHARDED(VenueId)** beim Definieren der externen Datenquelle die Leistung für viele Szenarios verbessert wird. Da jede *VenueId* auf eine einzelne Datenbank verweist, können Sie leicht remote filtern, sodass nur die benötigten Daten zurückgegeben werden.

1. Öffnen Sie ...\\Learning Modules\\Operational Analytics\\Adhoc Analytics\\*Demo-AdhocAnalyticsQueries.sql* in SSMS.
2. Stellen Sie sicher, dass Sie mit der Datenbank **adhocanalytics** verbunden sind.
3. Klicken Sie auf das Menü **Abfrage** und dann auf **Include Actual Execution Plan** (Tatsächlichen Ausführungsplan einschließen).
4. Markieren Sie die Abfrage *Which venues are currently registered?* (Welche Veranstaltungsorte sind aktuell registriert?), und drücken Sie **F5**.

   Die Abfrage gibt die gesamte Liste an Veranstaltungsorten zurück. Dies zeigt, wie schnell und unkompliziert Sie alle Mandanten abfragen und Daten jedes Mandanten zurückgeben können.

   Schauen Sie sich den Plan an, und beachten Sie, dass die Gesamtkosten die remote Abfrage ist, weil wir einfach zu jeder Mandantendatenbank gehen und die Informationen zum Veranstaltungsort auswählen.

   ![SELECT * FROM dbo.Venues](media/sql-database-saas-tutorial-adhoc-analytics/query1-plan.png)

5. Wählen Sie die nächste Abfrage aus, und drücken Sie **F5**.

   Diese Abfrage verknüpft Daten der Mandantendatenbank mit Daten der lokalen *VenueTypes*-Tabelle (lokal, da es sich dabei um eine Tabelle in der Datenbank *adhocanalytics* handelt).

   Schauen Sie sich den Plan an, und beachten Sie, dass der Hauptteil der Kosten die remote Abfrage ist, weil wir die Informationen zum Veranstaltungsort jedes Mandanten abfragen und anschließend eine schnelle lokale Verknüpfung mit der lokalen *VenueTypes*-Tabelle durchführen, um den Anzeigenamen anzuzeigen.

   ![Verknüpfung von remoten und lokalen Daten](media/sql-database-saas-tutorial-adhoc-analytics/query2-plan.png)

6. Wählen Sie nun die Abfrage *On which day were the most tickets sold?* (An welchem Tag wurden die meisten Tickets verkauft?) aus, und drücken Sie **F5**.

   Diese Abfrage führt etwas komplexere Verknüpfungen und Aggregationen durch. Wesentlich ist zu beachten, dass der Hauptteil der Verarbeitung remote erfolgt. Erneut erhalten wir nur die Zeilen, die wir benötigen. Für die aggregierten Ticketverkaufszahlen jedes Veranstaltungsorts pro Tag wird jeweils nur eine Zeile zurückgegeben.

   ![query](media/sql-database-saas-tutorial-adhoc-analytics/query3-plan.png)


## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]

> * Ausführen verteilter Abfragen für alle Mandantendatenbanken
> * Stellen Sie eine Ad-hoc-Analysedatenbank bereit, und fügen Sie Schemas hinzu, um verteilte Abfragen auszuführen.


Probieren Sie das [Tutorial zu Mandantenanalysen](sql-database-saas-tutorial-tenant-analytics.md), um sich mit dem Extrahieren von Daten in eine separate Analysedatenbank für komplexere Analyseverarbeitungen vertraut zu machen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* Zusätzliche [Tutorials, die auf der Wingtip-SaaS-Anwendung aufbauen](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastische Abfrage](sql-database-elastic-query-overview.md)

