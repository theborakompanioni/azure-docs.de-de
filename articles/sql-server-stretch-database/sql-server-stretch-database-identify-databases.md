---
title: "Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank durch Ausführen von Stretch Database Advisor | Microsoft Docs"
description: "Erfahren Sie, wie Sie Datenbanken und Tabellen identifizieren können, die sich für Stretch-Datenbank eignen."
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 9253c5a2-f135-4782-95ec-8ff39454c2c0
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: douglasl
translationtype: Human Translation
ms.sourcegitcommit: 47b666a7f2178daebc1b2b442fd1bb8cf31e40bb
ms.openlocfilehash: 3aef66c7b80ee356faedcaaadacd15aea3438714


---
# <a name="identify-databases-and-tables-for-stretch-database-by-running-stretch-database-advisor"></a>Identifizieren von Datenbanken und Tabellen für Stretch-Datenbank durch Ausführen von Stretch Database Advisor
Laden Sie SQL Server 2016 Upgrade Advisor herunter, um Datenbanken und Tabellen zu identifizieren, die sich für Stretch-Datenbank eignen, und führen Sie Stretch Database Advisor aus. Der Stretch-Datenbank-Ratgeber erkennt auch Blockierungsprobleme.

## <a name="download-and-install-upgrade-advisor"></a>Herunterladen und Installieren von Upgrade Advisor
Laden Sie Upgrade Advisor [hier](http://go.microsoft.com/fwlink/?LinkID=613421)herunter, und installieren Sie ihn. Dieses Tool ist nicht auf dem SQL Server-Installationsmedium enthalten.

## <a name="run-the-stretch-database-advisor"></a>Ausführen von Stretch Database Advisor
1. Führen Sie den Upgrade Advisor aus.
2. Wählen Sie **Szenarien** und dann **STRETCH DATABASE ADVISOR AUSFÜHREN** aus.
3. Klicken Sie auf dem Blatt **Stretch Database Advisor ausführen** auf **DATENBANK ZUM ANALYSIEREN AUSWÄHLEN**.
4. Geben Sie auf dem Blatt **Datenbanken auswählen** den Servernamen und die Authentifizierungsinformationen ein, bzw. wählen Sie beides aus. Klicken Sie auf **Verbinden**.
5. Eine Liste der Datenbanken auf dem ausgewählten Server wird angezeigt. Wählen Sie die Datenbanken aus, die Sie analysieren möchten. Klicken Sie auf **Auswählen**.
6. Klicken Sie auf dem Blatt **Stretch Database Advisor ausführen** auf **Ausführen**.  Die Analyse wird ausgeführt.

## <a name="review-the-results"></a>Überprüfen der Ergebnisse
1. Wenn die Analyse abgeschlossen ist, wählen Sie auf dem Blatt **Analysierte Datenbanken** eine der analysierten Datenbanken aus, um das Blatt **Analyseergebnisse** anzuzeigen.
   
   Auf dem Blatt **Analyseergebnisse** werden die empfohlenen Tabellen in der ausgewählten Datenbank aufgelistet, die den standardmäßigen Empfehlungskriterien entsprechen.
2. Wählen Sie in der Liste der Tabellen auf dem Blatt **Analyseergebnisse** eine der empfohlenen Tabellen aus, um das Blatt **Tabellenergebnisse** anzuzeigen.
   
   Wenn Hindernisse vorliegen, enthält das Blatt **Tabellenergebnisse** Hindernisse für die ausgewählte Tabelle. Informationen zu den Hindernissen, die vom Stretch Database Advisor erkannt werden, finden Sie unter [Einschränkungen für Stretch-Datenbank](sql-server-stretch-database-limitations.md).
3. Wählen Sie in der Liste der Hindernisse auf dem Blatt **Tabellenergebnisse** eines der Probleme aus, um weitere Informationen über das ausgewählte Problem und mögliche Gegenmaßnahmen anzuzeigen. Implementieren Sie die empfohlenen Maßnahmen, wenn Sie die ausgewählte Tabelle für Stretch-Datenbank konfigurieren möchten.

## <a name="next-step"></a>Nächster Schritt
Aktivieren Sie Stretch-Datenbank.

* Informationen zum Aktivieren von Stretch-Datenbank für eine **Datenbank**finden Sie unter [Aktivieren von Stretch-Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md).
* Informationen zum Aktivieren von Stretch-Datenbank für eine andere **Tabelle**, wenn Stretch bereits für die Datenbank aktiviert ist, finden Sie unter [Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md).

## <a name="see-also"></a>Siehe auch
[Einschränkungen für Stretch-Datenbank](sql-server-stretch-database-limitations.md)

[Aktivieren von Stretch-Datenbank für eine Datenbank](sql-server-stretch-database-enable-database.md)

[Aktivieren von Stretch-Datenbank für eine Tabelle](sql-server-stretch-database-enable-table.md)




<!--HONumber=Jan17_HO1-->


