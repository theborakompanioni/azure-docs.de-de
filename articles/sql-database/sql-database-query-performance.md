---
title: Einblicke in die Abfrageleistung der Azure SQL-Datenbank | Microsoft-Dokumentation
description: "Mit der Überwachung der Abfrageleistung werden für eine Azure SQL-Datenbank die Abfragen mit der höchsten CPU-Auslastung identifiziert."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: c2f580b2-3835-453f-89f5-140e02dd2ea7
ms.service: sql-database
ms.custom: monitor & manage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 08/09/2016
ms.author: sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: ddf93a0db278fffa2aa45fdc73a1c7f57a84fb39
ms.openlocfilehash: 14140a6aa8c8cccf658f8e98305659e8ae76dcad
ms.contentlocale: de-de
ms.lasthandoff: 01/31/2017


---
# <a name="azure-sql-database-query-performance-insight"></a>Query Performance Insight für Azure SQL-Datenbank
Das Verwalten und Abstimmen der Leistung von relationalen Datenbanken ist eine anspruchsvolle Aufgabe, die ein erhebliches Maß an Wissen und Zeit erfordert. Mithilfe von Query Performance Insight können Sie den Zeitaufwand für die Problembehandlung der Datenbankleistung reduzieren. Dieses Tool stellt Folgendes bereit:

* Tiefere Einblicke in den Verbrauch von Datenbankressourcen (DTU). 
* Die häufigsten Abfragen nach den Werten von CPU-Nutzung/Dauer/Ausführungshäufigkeit, bei denen unter Umständen eine Leistungssteigerung erzielt werden kann.
* Die Fähigkeit, die Details einer Abfrage, den Abfragetext und den Verlauf der Ressourcenverwendung anzuzeigen. 
* Anmerkungen zur Leistungsoptimierung, die die vom [Azure SQL-Datenbankratgeber](sql-database-advisor.md)  



## <a name="prerequisites"></a>Voraussetzungen
* Um Query Performance Insight nutzen zu können, muss der [Abfragespeicher](https://msdn.microsoft.com/library/dn817826.aspx) für Ihre Datenbank aktiv sein. Wenn der Abfragespeicher nicht ausgeführt wird, werden Sie vom Portal zum Aktivieren aufgefordert.

## <a name="permissions"></a>Berechtigungen
Die folgenden Berechtigungen der [rollenbasierten Zugriffssteuerung](../active-directory/role-based-access-control-configure.md) sind für die Verwendung von Query Performance Insight erforderlich: 

* Die Berechtigungen **Leser**, **Besitzer**, **Mitwirkender**, **Mitwirkender von SQL DB** oder **Mitwirkender von SQL Server** sind erforderlich, um die Abfragen und Diagramme mit dem höchsten Ressourcenverbrauch anzuzeigen. 
* Die Berechtigungen **Besitzer**, **Mitwirkender**, **Mitwirkender von SQL DB** oder **Mitwirkender von SQL Server** sind zum Anzeigen von Abfragetext erforderlich.

## <a name="using-query-performance-insight"></a>Verwenden von Query Performance Insight
Query Performance Insight ist einfach zu verwenden:

* Öffnen Sie das [Azure-Portal](https://portal.azure.com/) , und suchen Sie die Datenbank, die Sie untersuchen möchten. 
  * Wählen Sie im Menü links unter „Support und Problembehandlung“ die Option „Query Performance Insight“.
* Überprüfen Sie auf der ersten Registerkarte die Liste mit den Abfragen, die den höchsten Ressourcenverbrauch aufweisen.
* Wählen Sie eine einzelne Abfrage aus, um die Details anzuzeigen.
* Öffnen Sie den [Azure SQL-Datenbankratgeber](sql-database-advisor.md) , und überprüfen Sie, ob Empfehlungen verfügbar sind.
* Verwenden Sie Schieberegler oder Zoomsymbole, um das beobachtete Intervall zu ändern.
  
    ![Leistungsdashboard](./media/sql-database-query-performance/performance.png)

> [!NOTE]
> Der Abfragespeicher für SQL-Datenbank muss einige Stunden lang Daten erfasst haben, um Statistiken zur Abfrageleistung bereitstellen zu können. Wenn die Datenbank keine Aktivität aufweist oder Query Store während eines bestimmten Zeitraums nicht aktiv war, sind die Diagramme beim Anzeigen dieses Zeitraums leer. Sie können Query Store jederzeit aktivieren, wenn die Anwendung nicht ausgeführt wird.   
> 
> 

## <a name="review-top-cpu-consuming-queries"></a>Überprüfen von Abfragen mit der höchster CPU-Auslastung
Gehen Sie im [Portal](http://portal.azure.com) wie folgt vor:

1. Navigieren Sie zu einer SQL-Datenbank, und klicken Sie auf **Alle Einstellungen** > **Support und Problembehandlung** > **Query Performance Insight**. 
   
    ![Query Performance Insight][1]
   
    Die Ansicht der Abfragen mit höchstem Verbrauch wird geöffnet, und die Abfragen mit der höchsten CPU-Auslastung werden aufgelistet.
2. Klicken Sie an verschiedenen Stellen auf das Diagramm, um Details anzuzeigen.<br>In der obersten Zeile wird der Gesamtwert der DTUs für die Datenbank in Prozent angezeigt, die Balken zeigen die Auslastung der ausgewählten Abfragen während des ausgewählten Intervalls in Prozent (Beispiel: bei Auswahl von **Letzte Woche** entspricht jeder Balken einem Tag).
   
    ![Abfragen mit höchstem Verbrauch][2]
   
    Das untere Raster zeigt zusammengefasste Informationen für die sichtbaren Abfragen.
   
   * Abfrage-ID: eindeutiger Bezeichner der Abfrage in der Datenbank.
   * CPU-Auslastung pro Abfrage während des beobachteten Intervalls (je nach Aggregationsfunktion).
   * Dauer pro Abfrage (je nach Aggregationsfunktion).
   * Gesamtzahl der Ausführungen für eine bestimmte Abfrage.
     
     Aktivieren oder deaktivieren Sie mithilfe der Kontrollkästchen einzelne Abfragen, um sie in das Diagramm einzubeziehen oder davon auszuschließen.
3. Wenn Ihre Daten veraltet sind, klicken Sie auf die Schaltfläche **Aktualisieren** .
4. Sie können Schieberegler und Zoomschaltflächen verwenden, um das Beobachtungsintervall zu ändern und Spitzenwerte zu untersuchen:  ![Einstellung](./media/sql-database-query-performance/zoom.png)
5. Optional können Sie die Ansicht wechseln, indem Sie die Registerkarte **Benutzerdefiniert** auswählen und Folgendes festlegen:
   
   * Metrik (CPU, Dauer, Anzahl von Ausführungen)
   * Zeitintervall (letzte 24 Stunden, letzte Woche, letzter Monat) 
   * Anzahl von Abfragen
   * Aggregationsfunktion
     
     ![Einstellungen](./media/sql-database-query-performance/custom-tab.png)

## <a name="viewing-individual-query-details"></a>Anzeigen von Details einzelner Abfragen
So zeigen Sie die Details von Abfragen an

1. Klicken Sie in der Liste mit den Abfragen mit der höchsten Auslastung auf eine beliebige Abfrage.
   
    ![Details](./media/sql-database-query-performance/details.png)
2. Die Detailansicht wird geöffnet, und CPU-Auslastung sowie Dauer und Anzahl von Ausführungen der Abfragen werden im zeitlichen Verlauf aufgeführt.
3. Klicken Sie an verschiedenen Stellen auf das Diagramm, um Details anzuzeigen.
   
   * Im obersten Diagramm zeigt die Linie die Gesamtanzahl von DTUs in Prozent, die Balken zeigen den CPU-Verbrauch der ausgewählten Abfrage in Prozent.
   * Das zweite Diagramm zeigt die Gesamtdauer der ausgewählten Abfrage.
   * Das untere Diagramm zeigt die Gesamtanzahl von Ausführungen der ausgewählten Abfrage.
     
     ![Abfragedetails][3]
4. Optional können Sie Schieberegler oder Zoomschaltflächen verwenden oder auf **Einstellungen** klicken, um die Anzeige der Abfragedaten zu ändern oder einen anderen Zeitraum auszuwählen.

## <a name="review-top-queries-per-duration"></a>Überprüfen der Abfragen mit der längsten Ausführungsdauer
Im letzten Update von Query Performance Insight wurden zwei neue Metriken eingeführt, mit denen Sie potenzielle Engpässe ermitteln können: Ausführungsdauer und Anzahl von Ausführungen.<br>

Abfragen mit langer Ausführungsdauer besitzen das größte Potenzial, Ressourcen länger zu blockieren, andere Benutzer zu blockieren und die Skalierbarkeit einzuschränken. Sie sind auch die besten Kandidaten für eine Optimierung.<br>

So ermitteln Sie Abfragen mit langer Ausführungsdauer:

1. Öffnen Sie in Query Performance Insight die Registerkarte **Benutzerdefiniert** für die ausgewählte Datenbank.
2. Ändern Sie die Metrik zu **Dauer**
3. Wählen Sie die Anzahl von Abfragen und das Beobachtungsintervall aus.
4. Wählen Sie die Aggregationsfunktion aus.
   
   * **Sum** addiert die Gesamtausführungsdauer aller Abfragen während des gesamten Beobachtungsintervalls.
   * **Max** sucht nach Abfragen, deren Ausführungsdauer während des gesamten Beobachtungsintervalls beim Maximalwert lag.
   * **Avg** sucht die durchschnittliche Ausführungsdauer aller Abfrageausführungen und zeigt die höchsten dieser Durchschnittswerte an. 
     
     ![Abfragedauer][4]

## <a name="review-top-queries-per-execution-count"></a>Überprüfen der Abfragen mit der höchsten Anzahl von Ausführungen
Eine hohe Anzahl von Ausführungen wirkt sich möglicherweise nicht auf die Datenbank selbst aus, und die Ressourcenverwendung kann niedrig sein. Dennoch kann die Anwendung insgesamt verlangsamt werden.

In einigen Fällen kann eine sehr hohe Anzahl von Ausführungen eine Erhöhung von Netzwerkroundtrips nach sich ziehen. Roundtrips wirken sich erheblich auf die Leistung aus. Sie richten sich nach der Netzwerklatenz und der Serverlatenz bei Downstreams. 

Viele datengesteuerte Websites greifen beispielsweise bei jeder Benutzeranforderung umfassend auf die Datenbank zu. Verbindungspools verbessern die Situation zwar, dennoch können sich der erhöhte Netzwerkverkehr und die gesteigerte Verarbeitungslast auf dem Datenbankserver negativ auf die Leistung auswirken.  Im Allgemeinen empfiehlt es sich, Roundtrips auf ein absolutes Minimum zu reduzieren.

So identifizieren Sie häufig ausgeführte Abfragen:

1. Öffnen Sie in Query Performance Insight die Registerkarte **Benutzerdefiniert** für die ausgewählte Datenbank.
2. Ändern Sie die Metrik zu **Ausführungsanzahl**
3. Wählen Sie die Anzahl von Abfragen und das Beobachtungsintervall aus.
   
    ![Anzahl von Abfrageausführungen][5]

## <a name="understanding-performance-tuning-annotations"></a>Grundlegendes zu Anmerkungen der Leistungsoptimierung
Während Sie die Workload in Query Performance Insight untersuchen, bemerken Sie möglicherweise Symbole mit einem senkrechten Strich oberhalb des Diagramms.<br>

Diese Symbole sind Anmerkungen und stehen für Aktionen des [Azure SQL-Datenbankratgebers](sql-database-advisor.md), die sich auf die Leistung auswirken. Indem Sie auf eine Anmerkung zeigen, erhalten Sie grundlegende Informationen zu der Aktion:

![Anmerkung zur Abfrage][6]

Wenn Sie mehr über die Empfehlung erfahren oder die Empfehlung anwenden möchten, klicken Sie auf das Symbol. Dadurch wird die Detailansicht der Aktion geöffnet. Wenn es sich um eine aktive Empfehlung handelt, können Sie sie mit einem Befehl sofort anwenden.

![Details der Anmerkung zur Abfrage][7]

### <a name="multiple-annotations"></a>Mehrere Anmerkungen
Es ist möglich, dass aufgrund des Zoomfaktors Anmerkungen, die dicht nebeneinander liegen, in eine Anmerkung zusammengeführt werden. Diese Anmerkung wird durch ein besonderes Symbol dargestellt. Wenn Sie auf dieses Symbol klicken, öffnet sich ein neues Blatt, auf dem eine Liste der gruppierten Anmerkungen angezeigt wird.
Durch Korrelieren der Abfragen und der Aktionen zur Leistungsoptimierung erhalten Sie einen besseren Einblick in Ihre Workload. 

## <a name="optimizing-the-query-store-configuration-for-query-performance-insight"></a>Optimierung der Abfragespeicherkonfiguration für Query Performance Insight
Während der Verwendung von Query Performance Insight können die folgenden Abfragespeichermeldungen angezeigt werden:

* „Der Abfragespeicher ist für diese Datenbank nicht ordnungsgemäß konfiguriert. Klicken Sie hier, um weitere Informationen zu erhalten.“
* „Der Abfragespeicher ist für diese Datenbank nicht ordnungsgemäß konfiguriert. Klicken Sie hier, um die Einstellungen zu ändern.“ 

Diese Meldungen werden in der Regel angezeigt, wenn der Abfragespeicher keine neuen Daten sammeln kann. 

Der erste Fall tritt ein, wenn sich der Abfragespeicher im schreibgeschützten Modus befindet und die Parameter optimal festgelegt sind. Sie können dies beheben, indem Sie die Größe des Abfragespeichers erhöhen oder den Abfragespeicher löschen.

![QDS-Schaltfläche][8]

Der zweite Fall tritt ein, wenn der Abfragespeicher deaktiviert ist oder die Parameter nicht optimal festgelegt sind. <br>Sie können das Portal verwenden oder die unten stehenden Befehle ausführen, um die Richtlinie für Aufbewahrung und Erfassung zu ändern und den Abfragespeicher zu aktivieren:

![QDS-Schaltfläche][9]

### <a name="recommended-retention-and-capture-policy"></a>Empfohlene Aufbewahrungs- und Erfassungsrichtlinie
Es gibt zwei Arten von Aufbewahrungsrichtlinien:

* Größenbasiert: Bei Festlegung auf AUTO werden Daten automatisch bereinigt, wenn die maximale Größe fast erreicht ist.
* Zeitbasiert – standardmäßig legen wir ein Limit von 30 Tagen fest, d.h., wenn der Speicherplatz im Abfragespeicher zur Neige geht, werden Abfragedaten gelöscht, die älter als 30 Tage sind.

Die Erfassungsrichtlinie sollte festgelegt werden auf:

* **Alle:** Alle Abfragen werden erfasst.
* **Auto:** Seltene Abfragen und Abfragen mit unbedeutender Erstellungs- und Ausführungsdauer werden ignoriert. Die Schwellenwerte für Ausführungszahl, Erstellungs- und Ausführungsdauer werden intern bestimmt. Dies ist die Standardoption.
* **Keine:** Der Abfragespeicher beendet die Erfassung neuer Abfragen, die Laufzeitstatistiken für bereits erfasste Abfragen werden jedoch weiter gesammelt.

Sie sollten alle Richtlinien auf AUTO und die Bereinigungsrichtlinie auf 30 Tage festlegen:

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30));

    ALTER DATABASE [YourDB] 
    SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);

Erhöhen Sie die Größe des Abfragespeichers. Hierzu könnten Sie eine Verbindung mit einer Datenbank herstellen und folgende Abfrage ausführen:

    ALTER DATABASE [YourDB]
    SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);

Durch Anwenden dieser Einstellungen sammelt der Abfragespeicher schlussendlich neue Abfragen. Wenn Sie jedoch nicht warten möchten, können Sie den Abfragespeicher auch löschen. 

> [!NOTE]
> Durch Ausführen der folgenden Abfrage werden alle aktuellen Informationen im Abfragespeicher gelöscht. 
> 
> 

    ALTER DATABASE [YourDB] SET QUERY_STORE CLEAR;


## <a name="summary"></a>Zusammenfassung
Dank Query Performance Insight können Sie die Auswirkungen der Abfragen-Workload und die Wechselwirkung mit dem Verbrauch von Datenbankressourcen besser verstehen. Mit diesem Feature erhalten Sie Informationen zu den Abfragen mit dem höchsten Verbrauch und können die Abfragen ermitteln, die angepasst werden müssen, bevor sie ein Problem verursachen.

## <a name="next-steps"></a>Nächste Schritte
Um weitere Empfehlungen zur Verbesserung der Leistung Ihrer SQL-Datenbank zu erhalten, klicken Sie auf dem Blatt [Query Performance Insight](sql-database-advisor.md) auf **Empfehlungen** .

![Leistungsratgeber](./media/sql-database-query-performance/ia.png)

<!--Image references-->
[1]: ./media/sql-database-query-performance/tile.png
[2]: ./media/sql-database-query-performance/top-queries.png
[3]: ./media/sql-database-query-performance/query-details.png
[4]: ./media/sql-database-query-performance/top-duration.png
[5]: ./media/sql-database-query-performance/top-execution.png
[6]: ./media/sql-database-query-performance/annotation.png
[7]: ./media/sql-database-query-performance/annotation-details.png
[8]: ./media/sql-database-query-performance/qds-off.png
[9]: ./media/sql-database-query-performance/qds-button.png


