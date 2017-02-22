---
title: 'Migration: Data Warehouse-Hilfsprogramm zur Migration | Microsoft Docs'
description: Migrieren nach SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 4d7ad981-ef31-4513-b337-50bdc4709c09
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 61adb7ae8fddc3cf423ee4558308eb9ded11fea3


---
# <a name="data-warehouse-migration-utility-preview"></a>Data Warehouse-Hilfsprogramm für die Migration (Vorschau)
> [!div class="op_single_selector"]
> * [Herunterladen des Hilfsprogramms für die Migration][Download Migration Utility]
> 
> 

Das Data Warehouse-Hilfsprogramm für die Migration ist ein Tool zum Migrieren von Schemas und Daten aus SQL Server und der Azure SQL-Datenbank nach Azure SQL Data Warehouse. Während der Migration des Schemas ordnet das Tool automatisch das entsprechende Schema aus der Quelle zum Ziel zu. Nach dem Migrieren des Schemas haben Sie mit dem Tool die Möglichkeit, Daten mit automatisch erstellten Skripts zu verschieben.

Zusätzlich zur Schema- und Datenmigration bietet dieses Tool Ihnen die Option, Kompatibilitätsberichte zu generieren, die Inkompatibilitäten zwischen den Ziel- und Quellinstanzen zusammenzufassen, die eine optimierte Migration verhindern würden.

## <a name="get-started"></a>Erste Schritte
Als Voraussetzung für die Installation benötigen Sie das BCP-Befehlszeilenhilfsprogramm zum Ausführen von Migrationsskripts und Office, um den Kompatibilitätsbericht anzuzeigen. Nach dem Starten der ausführbaren Datei, die heruntergeladen wird, werden Sie aufgefordert, eine standardmäßigen Endbenutzerlizenzvertrag zu akzeptieren, bevor das Tool installiert wird.

Sie benötigen außerdem eine der folgenden Berechtigungen für die Datenbank, die Sie migrieren möchten, um das Hilfsprogramm für die Migration auszuführen: CREATE DATABASE, ALTER ANY DATABASE oder VIEW ANY DEFINITION.

### <a name="launching-the-tool-and-connecting"></a>Starten des Tools und Herstellen einer Verbindung
Starten Sie das Tool, indem Sie auf das Desktopsymbol klicken, das nach der Installation angezeigt wird. Beim Öffnen des Tools werden Sie auf eine Seite der Erstverbindung weitergeleitet, wo Sie die Quelle und das Ziel des Migrationstools auswählen können. Zu diesem Zeitpunkt werden SQL Server und Azure SQL-Datenbank als Quellen und SQL Data Warehouse als Ziel unterstützt. Nach dieser Auswahl werden Sie dazu aufgefordert, eine Verbindung mit dem Quellserver herzustellen, indem der Servername ausgefüllt und authentifiziert wird. Klicken Sie dann auf "Verbinden".

Nach der Authentifizierung zeigt das Tool eine Liste der Datenbanken an, die auf dem Server vorhanden sind, mit dem Sie verbunden sind. Sie können mit der Migration durch das Auswählen einer Datenbank beginnen, die Sie migrieren möchten. Klicken Sie anschließend auf „Ausgewählte migrieren“.

## <a name="migration-report"></a>Migrationsbericht
Durch das Auswählen von „Überprüfen der Datenbank-Kompatibilität“ im Tool wird ein Bericht generiert, der alle Objektinkompatibilitäten in der Datenbank zusammenfasst, die Sie zum Migrieren angefordert haben. Eine umfassendere Liste der SQL Server-Funktionen, die nicht in SQL Data Warehouse vorhanden sind, finden Sie der [Migrationsdokumentation][migration documentation]. Nachdem der Bericht generiert wurde, können Sie ihn in Excel speichern und öffnen.

Beachten Sie, dass beim Generieren des Migrationsschemas die meisten als "Object" erkannte Probleme angepasst werden, um die sofortige Migration von Daten zu ermöglichen. Informieren Sie sich über die Änderungen, um sicherzustellen, dass keine zusätzliche Anpassungen vorgenommen werden sollen, bevor das Schema angewendet wird.

## <a name="migrate-schema"></a>Migrieren des Schemas
Nach dem Herstellen einer Verbindung generiert "Schema migrieren" ein Skript zur Schemamigration für die ausgewählten Tabellen. Dieses Skript portiert die Struktur der Tabelle, weist nicht-kompatible Datentypen kompatibleren Formen zu, und erstellt Sicherheitsanmeldeinformationen und ein Schema, wenn dies vom Benutzer in den Migrationseinstellungen angegeben wurde. Dieser Code kann gegen die SQL Data Warehouse-Zielinstanz ausgeführt, in einer Datei gespeichert, in die Zwischenablage kopiert oder sogar inline bearbeitet werden, bevor weitere Aktionen erfolgen.  

Wie oben erwähnt sollten Sie beim Migrieren des Schemas die Migrationsänderungen überprüfen, die durch das Tool vorgenommen wurden, um sicherzustellen, dass Sie sie vollständig verstehen.  

## <a name="migrate-data"></a>Migrieren von Daten
Durch Klicken auf die Option „Migrieren von Daten“ können Sie BCP-Skripts erstellen, die Ihre Daten zuerst in Flatfiles auf den Server verschieben und dann direkt in das SQL Data Warehouse. Es wird empfohlen, diesen Prozess zum Verschieben kleiner Datenmengen zu nutze, da Wiederholungsversuche nicht integriert sind und Fehler möglicherweise auftreten, falls die Netzwerkverbindung unterbrochen wird. Für die Ausführung muss das BCP-Befehlszeilen-Hilfsprogramm installiert und das Schema für die Daten bereits erstellt sein.

Nachdem Sie die oben aufgeführten Parameter ausgefüllt haben, müssen Sie lediglich auf „Migration ausführen“ klicken, sodass ein Satz von zwei Paketen am angegebenen Speicherort generiert wird. Führen Sie die Exportdatei zum Exportieren von Daten aus Ihrer Migrationsquelle in Flatfiles aus, und führen Sie die Importdatei zum Importieren Ihrer Daten SQL Data Warehouse aus.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun einige Daten migriert haben, schauen Sie sich an, wie das [Entwickeln][develop] funktioniert.

<!--Image references-->

<!--Article references-->
[migration documentation]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md

<!--Other Web references--> 
[Download Migration Utility]: https://migrhoststorage.blob.core.windows.net/sqldwsample/DataWarehouseMigrationUtility.zip



<!--HONumber=Feb17_HO3-->


