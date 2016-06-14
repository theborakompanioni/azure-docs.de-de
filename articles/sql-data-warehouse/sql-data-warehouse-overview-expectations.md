<properties
   pageTitle="Erwartungen an SQL Data Warehouse Preview | Microsoft Azure"
   description="Übersicht über die Funktionen der öffentlichen Preview-Version und Ziele in Hinblick auf die allgemeine Verfügbarkeit von SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/05/2016"
   ms.author="nicw;barbkess;sonyama"/>


# Erwartungen an SQL Data Warehouse Preview

Dieser Artikel beschreibt die Funktionen von SQL Data Warehouse Preview und erläutert, welche Ziele wir mit dem Dienst in Hinblick auf die allgemeine Verfügbarkeit verfolgen. Diese Informationen werden bei Funktionsverbesserungen der öffentlichen Preview-Version fortlaufend aktualisiert.

Unsere Ziele für SQL Data Warehouse:

- Vorhersagbare Leistung und lineare Skalierbarkeit für Daten bis zum Petabytebereich
- Hohe Zuverlässigkeit für alle Data Warehouse-Vorgänge
- Kurze Zeit vom Laden der Daten bis zu den Einblicken in relationale und nicht relationale Daten

In der Vorschauphase von SQL Data Warehouse arbeiten wir kontinuierlich an der Umsetzung dieser Ziele.

## Vorhersagbare und skalierbare Leistung

Azure SQL Data Warehouse stellt erstmalig Data Warehouse-Einheiten (DWUs) als Möglichkeit zum Messen von Computerressourcen (CPUs, Arbeitsspeicher, Speicher-E/A) für das Data Warehouse bereit. Je höher die Anzahl der DWUs, desto größer die Ressourcen. Mit zunehmender Anzahl von DWUs führt SQL Data Warehouse Vorgänge parallel (z. B. Laden von Daten oder Abfrage) über stärker verteilte Ressourcen aus. Dadurch verringert sich die Latenz, und die Leistung verbessert sich.

Für jedes Data Warehouse gibt es zwei grundlegende Messwerte für die Leistung:

- Auslastungsrate. Die Anzahl der Datensätze, die pro Sekunde in das Data Warehouse geladen werden können. Wir messen speziell die Anzahl der Datensätze, die über PolyBase aus dem Azure-Blob-Speicher in eine Tabelle mit einem gruppierten Spaltenspeicherindex importiert werden können.
- Scanrate. Die Anzahl der Datensätze, die pro Sekunde sequenziell aus dem Data Warehouse abgerufen werden können. Wir messen speziell die Anzahl der Datensätze, die durch die Abfrage eines gruppierten Spaltenspeicherindexes zurückgegeben werden.

Wir haben einige wichtige Leistungsverbesserungen gemessen und werden die voraussichtlichen Raten in Kürze mitteilen. Während der Preview nehmen wir fortlaufend Verbesserungen (z. B. an Komprimierung und Caching) vor, um diese Raten zu erhöhen und eine vorhersagbare Skalierung sicherzustellen.

## Datenschutz

SQL Data Warehouse speichert alle Daten mithilfe von lokal redundantem Speicher im Azure-Speicher. Mehrere synchrone Kopien der Daten werden im lokalen Rechenzentrum behalten, um transparenten Datenschutz bei lokalen Ausfällen sicherzustellen.

## Backups

Azure SQL Data Warehouse sichert sämtliche Daten mindestens alle 8 Stunden mit Azure-Speichermomentaufnahmen. Diese Momentaufnahmen werden 7 Tage lang aufbewahrt. Dadurch können Daten an bis zu 21 Punkten innerhalb der letzten 7 Tage bis zu dem Zeitpunkt wiederhergestellt werden, zu dem die letzte Momentaufnahme erstellt wurde. Mithilfe von PowerShell oder REST-APIs können Sie die Daten aus einer Momentaufnahme wiederherstellen.

## Abfragezuverlässigkeit

SQL Data Warehouse basiert auf einer MPP-Architektur (Massively Parallel Processing). SQL Data Warehouse erkennt und migriert automatisch Ausfälle von Compute- und Steuerknoten. Ein Vorgang (z.B. Laden oder Abfragen von Daten) kann aufgrund eines Knotenausfalls oder einer Knotenmigration fehlschlagen. Während der Preview nehmen wir fortlaufend Verbesserungen vor, damit Vorgänge trotz Knotenausfällen erfolgreich abgeschlossen werden können.


## Upgrades und Downtime

SQL Data Warehouse wird in regelmäßigen Abständen aktualisiert, um neue Funktionen hinzuzufügen und wichtige Updates zu installieren. Diese Upgrades können störend sein, und derzeit werden Upgrades nicht in einem vorhersehbaren Zeitplan durchgeführt. Wenn Sie feststellen, dass dieser Prozess zu störend ist, empfehlen wir Ihnen, [ein Supportticket zu erstellen][], damit wir Ihnen helfen können, diesen Prozess zu umgehen.


## Nächste Schritte

[Erste Schritte][] mit der öffentlichen Preview-Version.

<!--Image references-->

<!--Article references-->
[ein Supportticket zu erstellen]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Erste Schritte]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other Web references-->

<!---HONumber=AcomDC_0608_2016-->