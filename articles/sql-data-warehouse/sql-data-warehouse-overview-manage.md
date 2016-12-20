---
title: Verwalten von Datenbanken in Azure SQL Data Warehouse | Microsoft Docs
description: "Übersicht über die Verwaltung von SQL Data Warehouse-Datenbanken. Enthält Verwaltungstools, DWUs und horizontale Leistungsskalierung, Problembehandlung für die Abfrageleistung, sinnvolle Sicherheitsrichtlinien und das Wiederherstellen einer Datenbank nach Datenbeschädigung oder einem regionalen Ausfall."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 8576fdb3-71fe-4b3b-a4e0-5e8a7f148acf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 5a101aa78dbac4f1a0edb7f414b44c14db392652
ms.openlocfilehash: f421f2e333b0725fe4561997d44ed61b20b3f1d6


---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Verwalten von Datenbanken in Azure SQL Data Warehouse
Mit SQL Data Warehouse werden viele Aspekte der Datenbankverwaltung automatisiert. Beispielsweise müssen Sie zum Skalieren der Leistung nur die Menge der Computeressourcen anpassen und dafür zahlen. SQL Data Warehouse übernimmt dann alle Aufgaben zum horizontalen Hoch- und Herunterskalieren.

Sie möchten sicherlich Ihre Workload überwachen, um Leistungsanforderungen zu ermitteln und Probleme mit lang andauernden Abfragen zu behandeln. Außerdem werden Sie auch einige Sicherheitsaufgaben durchführen müssen, um Berechtigungen für Benutzer und Anmeldungen zu verwalten.

In dieser Übersicht werden diese Aspekte der Verwaltung von SQL Data Warehouse behandelt.

* Verwaltungstools
* Skalieren von Computeressourcen
* Anhalten und Fortsetzen von Ressourcen
* Bewährte Methoden für die Leistung
* Abfrageüberwachung
* Sicherheit
* Sichern und Wiederherstellen

## <a name="management-tools"></a>Verwaltungstools
Sie können eine Vielzahl von Tools zum Verwalten von Datenbanken in SQL Data Warehouse verwenden. Bei der Verwaltung von Datenbanken werden Sie Vorlieben für Tools entwickeln, die Sie für die einzelnen Aufgabentypen einsetzen.

### <a name="azure-portal"></a>Azure-Portal
Das [Azure-Portal][Azure-Portal] ist ein webbasiertes Portal, in dem Sie Datenbanken erstellen, aktualisieren und löschen sowie Datenbankressourcen überwachen können. Dieses Tool eignet sich hervorragend, wenn Sie gerade erst mit der Verwendung von Azure begonnen haben, nur eine kleine Anzahl von Data Warehouse-Datenbanken verwalten oder bestimmte Aktionen schnell ausführen müssen.

Informationen zu den ersten Schritten mit dem Azure-Portal finden Sie unter Erstellen eines [SQL Data Warehouse (Azure-Portal)][Erstellen eines SQL Data Warehouse (Azure-Portal)].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools in Visual Studio
Mit [SQL Server Data Tools][SQL Server Data Tools] (SSDT) in Visual Studio können Sie eine Verbindung mit Ihren Datenbanken herstellen und diese verwalten und entwickeln. Wenn Sie ein Anwendungsentwickler sind, der mit Visual Studio oder anderen integrierten Entwicklungsumgebungen (IDEs) vertraut ist, sollten Sie SSDT in Visual Studio ausprobieren.

SSDT enthält den SQL Server-Objekt-Explorer, mit dem Sie SQL Data Warehouse-Datenbanken visuell darstellen, Verbindungen zu diesen herstellen und Skripts für sie ausführen können. Um eine schnelle Verbindung mit SQL Data Warehouse herzustellen, klicken Sie einfach in der Befehlsleiste auf die Schaltfläche **In Visual Studio öffnen** , während Sie die Datenbankdetails im klassischen Azure-Portal anzeigen.  

Informationen zum Einstieg mit SSDT in Visual Studio finden Sie unter [Abfragen von Azure SQL Data Warehouse mit Visual Studio][Abfragen von Azure SQL Data Warehouse mit Visual Studio].

### <a name="command-line-tools"></a>Befehlszeilentools
Befehlszeilentools eignen sich ideal für die Automatisierung Ihrer Workloads.  PowerShell und SQLCMD sind zwei großartige Möglichkeiten, Ihre Prozesse zu automatisieren.  Diese Tools werden für die Verwaltung einer großen Anzahl von logischen Servern und die Bereitstellung von Ressourcenänderungen in einer Produktionsumgebung empfohlen, da die erforderlichen Aufgaben durch Skripts ausgeführt und automatisiert werden können.

### <a name="dynamic-management-views"></a>Dynamische Verwaltungssichten
Dynamische Verwaltungssichten (DMVs) sind die Grundbestandteile für die Verwaltung von SQL Data Warehouse. Fast alle Informationen, die im Portal angezeigt werden, beruhen auf DMVs. Eine Liste der SQL Data Warehouse-DMVs finden Sie unter [SQL Data Warehouse-Systemsichten][SQL Data Warehouse-Systemsichten].

Informationen zum Einstieg finden Sie unter [Verbinden und Abfragen mit sqlcmd][Verbinden und Abfragen mit sqlcmd] und [Erstellen einer Datenbank (PowerShell)][Erstellen einer Datenbank (PowerShell)].

## <a name="scale-compute"></a>Skalieren von Computeressourcen
In SQL Data Warehouse können Sie die Leistung schnell horizontal hoch- und wieder herunterskalieren, indem Sie die Computeressourcen für die CPU, den Arbeitsspeicher oder die E/A-Bandbreite erhöhen bzw. verringern. Um die Leistung zu skalieren, müssen Sie lediglich die Anzahl der DWUs (Data Warehouse-Einheiten) anpassen, die Ihrer Datenbank von SQL Data Warehouse zugewiesen werden. SQL Data Warehouse nimmt die Änderungen schnell vor und kümmert sich um alle zugrunde liegenden Änderungen an Hardware oder Software.

Weitere Informationen zum Skalieren von DWUs finden Sie unter [Skalieren der Leistung][Skalieren der Leistung].

## <a name="pause-and-resume"></a>Anhalten und Fortsetzen von Ressourcen
Um Kosten zu sparen, können Sie Computeressourcen bei Bedarf anhalten und fortsetzen. Wenn Sie die Datenbank z.B. nachts oder am Wochenende nicht verwenden, können Sie sie während dieser Zeiträume anhalten und während des Tages wieder fortsetzen. DWUs werden Ihnen nicht in Rechnung gestellt, während die Datenbank angehalten wird.

Weitere Informationen finden Sie unter [Anhalten von Computeressourcen][Anhalten von Computeressourcen] und [Fortsetzen von Computeressourcen][Fortsetzen von Computeressourcen].

## <a name="performance-best-practices"></a>Bewährte Methoden für die Leistung
Beim Einstieg in eine neue Technologie können Sie viel Zeit sparen, wenn Sie gleich zu Beginn die Tipps und Tricks beherzigen.  Bewährte Methoden finden Sie in vielen unserer Themen.

Eine Zusammenfassung der wichtigsten Aspekte bei der Entwicklung Ihrer Workload finden Sie unter [Bewährte Methoden für SQL Data Warehouse][Bewährte Methoden für SQL Data Warehouse].

## <a name="query-monitoring"></a>Abfrageüberwachung
Manchmal wird eine Abfrage zu lang ausgeführt, aber Sie sind nicht sicher, wo das Problem liegt. SQL Data Warehouse verfügt über dynamische Verwaltungssichten (DMVs), mit deren Hilfe Sie herausfinden können, welche Abfrage zu lange dauert.

Informationen zu Abfragen mit langer Laufzeit finden Sie unter [Überwachen Ihrer Workload mit dynamischen Verwaltungssichten][Überwachen Ihrer Workload mit dynamischen Verwaltungssichten].

## <a name="security"></a>Sicherheit
Um ein sicheres System zu gewährleisten, müssen Sie vor jeder Art von unbefugtem Zugriff auf der Hut sein. Ein Sicherheitssystem muss sicherstellen, dass Firewallregeln eingerichtet sind, sodass nur autorisierte IP-Adressen eine Verbindung herstellen können. Eine ordnungsgemäße Authentifizierung der Benutzeranmeldeinformationen ist erforderlich. Nachdem ein Benutzer eine Verbindung mit der Datenbank hergestellt hat, sollte der Benutzer nur Berechtigungen zum Ausführen einer minimalen Anzahl von Aktionen besitzen. Um Daten zu schützen, können Sie die Verschlüsselung verwenden. Es ist außerdem wichtig, Funktionen zur Überwachung und Nachverfolgung einzusetzen, damit Sie Ereignisse zurückverfolgen können, wenn verdächtige Aktivitäten vorliegen.

Weitere Informationen zum Verwalten der Sicherheit finden Sie in der [Sicherheitsübersicht][Sicherheitsübersicht].

## <a name="backup-and-restore"></a>Sichern und Wiederherstellen
Zuverlässige Sicherungen Ihrer Daten sind ein wesentlicher Bestandteil jeder Produktionsdatenbank. SQL Data Warehouse schützt Ihre Daten, indem Ihre aktiven Datenbanken in regelmäßigen Abständen automatisch gesichert werden. Diese Sicherungen ermöglichen das Wiederherstellen in Fällen, in denen Sie Ihre Daten beschädigt oder Ihre Daten oder Datenbank versehentlich gelöscht haben.  Informationen zum Datensicherungszeitplan, zur Aufbewahrungsrichtlinie und zur Wiederherstellung einer Datenbank finden Sie unter [Wiederherstellung von einer Momentaufnahme][Wiederherstellung von einer Momentaufnahme].

## <a name="next-steps"></a>Nächste Schritte
Ein sinnvoller Datenbankentwurf erleichtert die Verwaltung Ihrer Datenbanken in SQL Data Warehouse. Weitere Informationen finden Sie in der [Entwicklungsübersicht][Entwicklungsübersicht].

<!--Image references-->

<!--Article references-->
[Erstellen eines SQL Data Warehouse (Azure-Portal)]: sql-data-warehouse-get-started-provision.md
[Erstellen einer Datenbank (PowerShell)]: sql-data-warehouse-get-started-provision-powershell.md
[Verbindung]: sql-data-warehouse-develop-connections.md
[Abfragen von Azure SQL Data Warehouse mit Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Verbinden und Abfragen mit sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Entwicklungsübersicht]: sql-data-warehouse-overview-develop.md
[Überwachen Ihrer Workload mit dynamischen Verwaltungssichten]: sql-data-warehouse-manage-monitor.md
[Anhalten von Computeressourcen]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Wiederherstellung von einer Momentaufnahme]: sql-data-warehouse-restore-database-overview.md
[Fortsetzen von Computeressourcen]: sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Skalieren der Leistung]: sql-data-warehouse-manage-compute-overview.md#scale-performance-bk
[Sicherheitsübersicht]: sql-data-warehouse-overview-manage-security.md
[Bewährte Methoden für SQL Data Warehouse]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse-Systemsichten]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure-Portal]: http://portal.azure.com/



<!--HONumber=Nov16_HO3-->


