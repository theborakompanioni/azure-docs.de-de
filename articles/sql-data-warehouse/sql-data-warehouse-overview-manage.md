<properties
   pageTitle="Verwalten von Datenbanken in Azure SQL Data Warehouse | Microsoft Azure"
   description="Übersicht über die Verwaltung von SQL Data Warehouse-Datenbanken. Enthält Verwaltungstools, DWUs und horizontale Leistungsskalierung, Problembehandlung für die Abfrageleistung, sinnvolle Sicherheitsrichtlinien und das Wiederherstellen einer Datenbank nach Datenbeschädigung oder einem regionalen Ausfall."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/16/2016"
   ms.author="barbkess;sonyama;"/>

# Verwalten von Datenbanken in Azure SQL Data Warehouse

Mit SQL Data Warehouse werden viele Aspekte der Datenbankverwaltung automatisiert. Beispielsweise müssen Sie zum Skalieren der Leistung nur die Menge der Computeressourcen anpassen und dafür zahlen. SQL Data Warehouse übernimmt dann alle Aufgaben zum horizontalen Hoch- und Herunterskalieren.

Sie möchten sicherlich Ihre Workload überwachen, um Leistungsanforderungen zu ermitteln und Probleme mit lang andauernden Abfragen zu behandeln. Außerdem werden Sie auch einige Sicherheitsaufgaben durchführen müssen, um Berechtigungen für Benutzer und Anmeldungen zu verwalten.

In dieser Übersicht werden diese Aspekte der Verwaltung von SQL Data Warehouse behandelt.

- Verwaltungstools
- Skalieren von Computeressourcen
- Anhalten und Fortsetzen von Ressourcen
- Bewährte Methoden für die Leistung
- Abfrageüberwachung
- Sicherheit
- Sichern und Wiederherstellen

## Verwaltungstools

Sie können eine Vielzahl von Tools zum Verwalten von Datenbanken in SQL Data Warehouse verwenden. Bei der Verwaltung von Datenbanken werden Sie Vorlieben für Tools entwickeln, die Sie für die einzelnen Aufgabentypen einsetzen.

### Azure-Portal
Das [Azure-Portal][] ist ein webbasiertes Portal, in dem Sie Datenbanken erstellen, aktualisieren und löschen sowie Datenbankressourcen überwachen können. Dieses Tool eignet sich hervorragend, wenn Sie gerade erst mit der Verwendung von Azure begonnen haben, nur eine kleine Anzahl von Data Warehouse-Datenbanken verwalten oder bestimmte Aktionen schnell ausführen müssen.

Informationen zu den ersten Schritten mit dem Azure-Portal finden Sie unter [Erstellen eines SQL Data Warehouse (Azure-Portal)][].

### SQL Server Data Tools in Visual Studio
Mit [SQL Server Data Tools][] \(SSDT) in Visual Studio können Sie eine Verbindung mit Ihren Datenbanken herstellen und diese verwalten und entwickeln. Wenn Sie ein Anwendungsentwickler sind, der mit Visual Studio oder anderen integrierten Entwicklungsumgebungen (IDEs) vertraut ist, sollten Sie SSDT in Visual Studio ausprobieren.

SSDT enthält den SQL Server-Objekt-Explorer, mit dem Sie SQL Data Warehouse-Datenbanken visuell darstellen, Verbindungen zu diesen herstellen und Skripts für sie ausführen können. Um eine schnelle Verbindung mit SQL Data Warehouse herzustellen, klicken Sie einfach in der Befehlsleiste auf die Schaltfläche **In Visual Studio öffnen**, während Sie die Datenbankdetails im klassischen Azure-Portal anzeigen.

Informationen zum Einstieg mit SSDT in Visual Studio finden Sie unter [Abfragen von Azure SQL Data Warehouse mit Visual Studio][].

### Befehlszeilentools
Befehlszeilentools eignen sich ideal für die Automatisierung Ihrer Workloads. PowerShell und SQLCMD sind zwei großartige Möglichkeiten, Ihre Prozesse zu automatisieren. Diese Tools werden für die Verwaltung einer großen Anzahl von logischen Servern und die Bereitstellung von Ressourcenänderungen in einer Produktionsumgebung empfohlen, da die erforderlichen Aufgaben durch Skripts ausgeführt und automatisiert werden können.

### Dynamische Verwaltungssichten 

Dynamische Verwaltungssichten (DMVs) sind die Grundbestandteile für die Verwaltung von SQL Data Warehouse. Fast alle Informationen, die im Portal angezeigt werden, beruhen auf DMVs. Eine Liste der SQL Data Warehouse-DMVs finden Sie unter [SQL Data Warehouse-Systemsichten][].

Informationen zum Einstieg finden Sie unter [Verbinden und Abfragen mit SQLCMD][] und unter [Erstellen einer Datenbank (PowerShell)][].

## Skalieren von Computeressourcen

In SQL Data Warehouse können Sie die Leistung schnell horizontal hoch- und wieder herunterskalieren, indem Sie die Computeressourcen für die CPU, den Arbeitsspeicher oder die E/A-Bandbreite erhöhen bzw. verringern. Um die Leistung zu skalieren, müssen Sie lediglich die Anzahl der DWUs (Data Warehouse-Einheiten) anpassen, die Ihrer Datenbank von SQL Data Warehouse zugewiesen werden. SQL Data Warehouse nimmt die Änderungen schnell vor und kümmert sich um alle zugrunde liegenden Änderungen an Hardware oder Software.

Weitere Informationen zum Skalieren von DWUs finden Sie unter [Skalieren der Leistung][].

##  Anhalten und Fortsetzen von Ressourcen

Um Kosten zu sparen, können Sie Computeressourcen bei Bedarf anhalten und fortsetzen. Wenn Sie die Datenbank z.B. nachts oder am Wochenende nicht verwenden, können Sie sie während dieser Zeiträume anhalten und während des Tages wieder fortsetzen. DWUs werden Ihnen nicht in Rechnung gestellt, während die Datenbank angehalten wird.

Weitere Informationen finden Sie unter [Anhalten von Computeressourcen][] und unter [Fortsetzen von Computeressourcen][].

## Bewährte Methoden für die Leistung

Beim Einstieg in eine neue Technologie können Sie viel Zeit sparen, wenn Sie gleich zu Beginn die Tipps und Tricks beherzigen. Bewährte Methoden finden Sie in vielen unserer Themen.

Eine Zusammenfassung der wichtigsten Aspekte bei der Entwicklung Ihrer Workload finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

## Abfrageüberwachung

Manchmal wird eine Abfrage zu lang ausgeführt, aber Sie sind nicht sicher, wo das Problem liegt. SQL Data Warehouse verfügt über dynamische Verwaltungssichten (DMVs), mit deren Hilfe Sie herausfinden können, welche Abfrage zu lange dauert.

Informationen zu lang andauernden Abfragen finden Sie unter [Überwachen Ihrer Workload mit DMVs][].

## Sicherheit

Um ein sicheres System zu gewährleisten, müssen Sie vor jeder Art von unbefugtem Zugriff auf der Hut sein. Ein Sicherheitssystem muss sicherstellen, dass Firewallregeln eingerichtet sind, sodass nur autorisierte IP-Adressen eine Verbindung herstellen können. Eine ordnungsgemäße Authentifizierung der Benutzeranmeldeinformationen ist erforderlich. Nachdem ein Benutzer eine Verbindung mit der Datenbank hergestellt hat, sollte der Benutzer nur Berechtigungen zum Ausführen einer minimalen Anzahl von Aktionen besitzen. Um Daten zu schützen, können Sie die Verschlüsselung verwenden. Es ist außerdem wichtig, Funktionen zur Überwachung und Nachverfolgung einzusetzen, damit Sie Ereignisse zurückverfolgen können, wenn verdächtige Aktivitäten vorliegen.

Weitere Informationen zum Verwalten der Sicherheit finden Sie in der [Sicherheitsübersicht][].

## Sichern und Wiederherstellen

Zuverlässige Sicherungen Ihrer Daten sind ein wesentlicher Bestandteil jeder Produktionsdatenbank. SQL Data Warehouse schützt Ihre Daten, indem Ihre aktiven Datenbanken in regelmäßigen Abständen automatisch gesichert werden. Diese Sicherungen ermöglichen das Wiederherstellen in Fällen, in denen Sie Ihre Daten beschädigt oder Ihre Daten oder Datenbank versehentlich gelöscht haben. Informationen zum Datensicherungszeitplan, zur Aufbewahrungsrichtlinie und zur Wiederherstellung einer Datenbank finden Sie unter [Wiederherstellung von einer Momentaufnahme][].

## Nächste Schritte
Ein sinnvoller Datenbankentwurf erleichtert die Verwaltung Ihrer Datenbanken in SQL Data Warehouse. Weitere Informationen finden Sie in der [Entwicklungsübersicht][].

<!--Image references-->

<!--Article references-->
[Erstellen eines SQL Data Warehouse (Azure-Portal)]: sql-data-warehouse-get-started-provision.md
[Erstellen einer Datenbank (PowerShell)]: sql-data-warehouse-get-started-provision-powershell
[connection]: sql-data-warehouse-develop-connections.md
[Abfragen von Azure SQL Data Warehouse mit Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Verbinden und Abfragen mit SQLCMD]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Entwicklungsübersicht]: sql-data-warehouse-overview-develop.md
[Überwachen Ihrer Workload mit DMVs]: sql-data-warehouse-manage-monitor.md
[Anhalten von Computeressourcen]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Wiederherstellung von einer Momentaufnahme]: sql-data-warehouse-restore-database-overview.md
[Fortsetzen von Computeressourcen]: sql-data-warehouse-manage-compute-overview.md#resume-compute-performance-bk
[Skalieren der Leistung]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Sicherheitsübersicht]: sql-data-warehouse-overview-manage-security.md
[Bewährte Methoden für SQL Data Warehouse]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse-Systemsichten]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure-Portal]: http://portal.azure.com/

<!---HONumber=AcomDC_0817_2016-->