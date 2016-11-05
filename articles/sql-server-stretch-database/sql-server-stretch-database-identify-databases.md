---
title: Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank durch Ausführen von Stretch Database Advisor | Microsoft Docs
description: Erfahren Sie, wie Sie Datenbanken und Tabellen identifizieren können, die sich für Stretch-Datenbank eignen.
services: sql-server-stretch-database
documentationcenter: ''
author: douglaslMS
manager: ''
editor: ''

ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl

---
# Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank durch Ausführen von Stretch Database Advisor
Laden Sie SQL Server 2016 Upgrade Advisor herunter, um Datenbanken und Tabellen zu identifizieren, die sich für Stretch-Datenbank eignen, und führen Sie Stretch Database Advisor aus. Der Stretch-Datenbank-Ratgeber erkennt auch Blockierungsprobleme.

## Herunterladen und Installieren von Upgrade Advisor
Laden Sie Upgrade Advisor [hier](http://go.microsoft.com/fwlink/?LinkID=613421) herunter, und installieren Sie ihn. Dieses Tool ist nicht auf dem SQL Server-Installationsmedium enthalten.

## Ausführen von Stretch Database Advisor
1. Führen Sie den Upgrade Advisor aus.
2. Wählen Sie **Szenarien** und dann **STRETCH DATABASE ADVISOR AUSFÜHREN** aus.
3. Klicken Sie auf dem Blatt **Stretch Database Advisor ausführen** auf **DATENBANK ZUM ANALYSIEREN AUSWÄHLEN**.
4. Geben Sie auf dem Blatt **Datenbanken auswählen** den Servernamen und die Authentifizierungsinformationen ein, bzw. wählen Sie beides aus. Klicken Sie auf **Verbinden**.
5. Eine Liste der Datenbanken auf dem ausgewählten Server wird angezeigt. Wählen Sie die Datenbanken aus, die Sie analysieren möchten. Klicken Sie auf **Auswählen**.
6. Klicken Sie auf dem Blatt **Stretch Database Advisor ausführen** auf **Ausführen**. Die Analyse wird ausgeführt.

## Überprüfen der Ergebnisse
1. Wenn die Analyse abgeschlossen ist, wählen Sie auf dem Blatt **Analysierte Datenbanken** eine der analysierten Datenbanken aus, um das Blatt **Analyseergebnisse** anzuzeigen.
   
   Auf dem Blatt **Analyseergebnisse** werden die empfohlenen Tabellen in der ausgewählten Datenbank aufgelistet, die den standardmäßigen Empfehlungskriterien entsprechen.
2. Wählen Sie in der Liste der Tabellen auf dem Blatt **Analyseergebnisse** eine der empfohlenen Tabellen aus, um das Blatt **Tabellenergebnisse** anzuzeigen.
   
   Wenn Hindernisse vorliegen, enthält das Blatt **Tabellenergebnisse** Hindernisse für die ausgewählte Tabelle. Informationen zu den Hindernissen, die vom Stretch Database Advisor erkannt werden, finden Sie unter [Einschränkungen für Stretch-Datenbank](sql-server-stretch-database-limitations.md).
3. Wählen Sie in der Liste der Hindernisse auf dem Blatt **Tabellenergebnisse** eines der Probleme aus, um weitere Informationen über das ausgewählte Problem und mögliche Gegenmaßnahmen anzuzeigen. Implementieren Sie die empfohlenen Maßnahmen, wenn Sie die ausgewählte Tabelle für Stretch-Datenbank konfigurieren möchten.

## Nächster Schritt
Aktivieren Sie Stretch-Datenbank.

* Informationen zum Aktivieren von Stretch-Datenbank für eine **Datenbank** finden Sie unter [Aktivieren von Stretch-Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md).
* Informationen zum Aktivieren von Stretch-Datenbank für eine andere **Tabelle**, wenn Stretch bereits für die Datenbank aktiviert ist, finden Sie unter [Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md).

## Siehe auch
[Einschränkungen für Stretch-Datenbank](sql-server-stretch-database-limitations.md)

[Aktivieren von Stretch-Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md)

[Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md)

[Alle Themen für den SQL Server Stretch-Datenbankdienst](sql-server-stretch-database-index-all-articles.md)

<!---HONumber=AcomDC_0817_2016-->