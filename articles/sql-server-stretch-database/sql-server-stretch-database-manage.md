---
title: Verwalten von Stretch-Datenbank und Behandeln von Problemen | Microsoft Docs
description: Erfahren Sie, wie Sie Stretch-Datenbank verwalten und Probleme behandeln können.
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
ms.date: 06/27/2016
ms.author: douglasl

---
# Verwalten von Stretch-Datenbank und Behandeln von Problemen
Verwenden Sie zum Verwalten von Stretch-Datenbank und zur Behandlung von Problemen, die in diesem Thema beschriebenen Tools und Methoden.

## Verwalten lokaler Daten
### <a name="LocalInfo"></a>Abrufen von Informationen zu lokalen Datenbanken und Tabellen, die für die Stretch-Datenbank aktiviert sind
Öffnen Sie die Katalogsichten **sys.databases** und **sys.tables**, um Informationen über Stretch-fähige SQL Server-Datenbanken und -Tabellen anzuzeigen. Weitere Informationen finden Sie unter [sys.databases (Transact-SQL)](https://msdn.microsoft.com/library/ms178534.aspx) und [sys.tables (Transact-SQL)](https://msdn.microsoft.com/library/ms187406.aspx).

Führen Sie die folgende Anweisung aus, um zu ermitteln, wie viel Speicherplatz eine Stretch-fähige Tabelle in SQL Server belegt.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'LOCAL_ONLY';
GO
 ```
## Verwalten der Datenmigration
### Überprüfen der auf eine Tabelle angewendeten Filterfunktion
Öffnen Sie die Katalogsicht **sys.remote\_data\_archive\_tables**, und überprüfen Sie den Wert der Spalte **filter\_predicate**, um die Funktion zu ermitteln, mit der Stretch-Datenbank die zu migrierenden Zeilen auswählt. Wenn der Wert null ist, ist die gesamte Tabelle für eine Migration berechtigt. Weitere Informationen finden Sie unter [sys.remote\_data\_archive\_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx) und [Auswählen von Zeilen für die Migration mit einer Filterfunktion](sql-server-stretch-database-predicate-function.md).

### <a name="Migration"></a>Überprüfen des Status der Datenmigration
Wählen Sie **Aufgaben | Stretch | Überwachung** für eine Datenbank in SQL Server Management Studio, um die Datenmigration im Stretch-Datenbankmonitor zu überwachen. Weitere Informationen finden Sie unter [Überwachen und Behandeln von Problemen der Datenmigration (Stretch-Datenbank)](sql-server-stretch-database-monitor.md).

Oder öffnen Sie die dynamische Verwaltungssicht **sys.dm\_db\_rda\_migration\_status**, um zu ermitteln, wie viele Batches und Zeilen mit Daten migriert wurden.

### <a name="Firewall"></a>Beheben von Problemen bei der Datenmigration
Empfehlungen zur Problembehandlung finden Sie unter [Überwachen und Behandeln von Problemen der Datenmigration (Stretch-Datenbank)](sql-server-stretch-database-monitor.md).

## Verwalten von Remotedaten
### <a name="RemoteInfo"></a>Abrufen von Informationen zu Remotedatenbanken und -tabellen, die von Stretch-Datenbank verwendet werden
Öffnen Sie die Katalogsichten **sys.remote\_data\_archive\_databases** und **sys.remote\_data\_archive\_tables**, um Informationen über die Remotedatenbanken und -tabellen anzuzeigen, in denen migrierte Daten gespeichert werden. Weitere Informationen finden Sie unter [sys.remote\_data\_archive\_databases (Transact-SQL)](https://msdn.microsoft.com/library/dn934995.aspx) und [sys.remote\_data\_archive\_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

Führen Sie die folgende Anweisung aus, um zu ermitteln, wie viel Speicherplatz eine für Stretch aktivierte Tabelle in Azure belegt.

 ```tsql
USE <Stretch-enabled database name>;
GO
EXEC sp_spaceused '<Stretch-enabled table name>', 'true', 'REMOTE_ONLY';
GO
 ```

### Löschen migrierter Daten
Wenn Sie bereits zu Azure migrierte Daten löschen möchten, führen Sie die unter [sys.sp\_rda\_reconcile\_batch](https://msdn.microsoft.com/library/mt707768.aspx) beschriebenen Schritte aus.

## Verwalten des Tabellenschemas
### Schema der Remotetabelle nicht ändern
Ändern Sie nicht das Schema einer Azure-Remotetabelle, die mit einer SQL Server-Tabelle verknüpft ist, die für Stretch-Datenbank konfiguriert wurde. Ändern Sie insbesondere nicht den Namen oder den Datentyp einer Spalte. Für die Stretch-Datenbank-Funktion gelten verschiedene Annahmen hinsichtlich des Schemas einer Remotetabelle in Bezug auf das Schema der SQL Server-Tabelle. Wenn Sie das Remoteschema ändern, funktioniert Stretch-Datenbank nicht mehr für die geänderte Tabelle.

### Abstimmen von Tabellenspalten
Wenn Sie versehentlich Spalten aus der Remotetabelle gelöscht haben, führen Sie **sp\_rda\_reconcile\_columns** aus, um der Remotetabelle Spalten hinzuzufügen, die in der für Stretch aktivierten SQL Server-Tabelle, aber nicht in der Remotetabelle vorhanden sind. Weitere Informationen finden Sie unter [sys.sp\_rda\_reconcile\_columns](https://msdn.microsoft.com/library/mt707765.aspx).

> [WICHTIG] Wenn **sp\_rda\_reconcile\_columns** erneut Spalten erstellt, die Sie versehentlich aus der Remotetabelle gelöscht haben, werden dabei nicht die Daten wiederhergestellt, die zuvor in den gelöschten Spalten enthalten waren.
> 
> 

**sp\_rda\_reconcile\_columns** löscht keine Spalten aus der Remotetabelle, die in der Remotetabelle, aber nicht in der für Stretch aktivierten SQL Server-Tabelle vorhanden sind. Wenn die Azure-Remotetabelle Spalten enthält, die in der Stretch-fähigen SQL Server-Tabelle nicht mehr vorhanden sind, verhindern diese zusätzlichen Spalten nicht die normale Funktionsweise der Stretch-Datenbank. Sie können die zusätzlichen Spalten manuell entfernen (optional).

## Verwalten von Leistung und Kosten
### Beheben von Problemen mit der Abfrageleistung
Abfragen mit Stretch-fähigen Tabellen werden erwartungsgemäß langsamer, nachdem die Tabellen für Stretch aktiviert wurden. Wenn die Abfrageleistung maßgeblich sinkt, überprüfen Sie die folgenden möglichen Problemursachen.

* Befindet sich der Azure-Server in einer anderen geografischen Region als Ihr SQL Server? Konfigurieren Sie Ihren Azure-Server so, dass er sich in der gleichen geografischen Region wie Ihr SQL Server befindet, um die Netzwerklatenz zu reduzieren.
* Möglicherweise haben sich die Netzwerkbedingungen verschlechtert. Wenden Sie sich an Ihren Netzwerkadministrator, um Informationen über aktuelle Probleme oder Ausfälle zu erhalten.

### Steigern der Azure-Leistung für ressourcenintensive Vorgänge wie z. B. die Indizierung
Wenn Sie einen Index für eine große Tabelle, die für Stretch-Datenbank konfiguriert ist, erstellen, neu erstellen oder neu organisieren, und während des Vorgangs umfangreiche Abfragen der migrierten Daten in Azure erwarten, sollten Sie die Steigerung der Leistung der entsprechenden Azure-Remotedatenbank für die Dauer des Vorgangs in Betracht ziehen. Weitere Informationen zu Leistungsstufen und Preisen finden Sie unter [SQL Server Stretch-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### Sie können den SQL Server Stretch-Datenbankdienst in Azure nicht anhalten.
 Stellen Sie sicher, dass Sie die entsprechende Leistungs- und Preisstufe auswählen. Wenn Sie die Leistungsstufe für einen ressourcenintensiven Vorgang vorübergehend erhöhen, stellen Sie nach Abschluss des Vorgangs wieder die vorherige Stufe her. Weitere Informationen zu Leistungsstufen und Preisen finden Sie unter [SQL Server Stretch-Datenbank – Preise](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

## Ändern des Abfragebereichs
 Abfragen für Stretch-fähige Tabellen geben standardmäßig sowohl lokale Daten als auch Remotedaten zurück. Sie können den Abfragebereich für alle Abfragen von allen Benutzern oder nur für eine einzelne Abfrage von einem Administrator ändern.

### Ändern des Abfragebereichs für alle Abfragen von allen Benutzern
 Um den Bereich für alle Abfragen von allen Benutzern zu ändern, führen Sie die gespeicherte Prozedur **sys.sp\_rda\_set\_query\_mode** aus. Sie können den Bereich reduzieren, um nur lokale Daten abzufragen, alle Abfragen zu deaktivieren oder die Standardeinstellungen wiederherzustellen. Weitere Informationen finden Sie unter [sys.sp\_rda\_set\_query\_mode](https://msdn.microsoft.com/library/mt703715.aspx).

### <a name="queryHints"></a>Ändern des Abfragebereichs für eine einzelne Abfrage durch einen Administrator
 Um den Bereich einer einzelnen Abfrage durch ein Mitglied der Rolle „db\_owner“ zu ändern, fügen Sie den Abfragehinweis **WITH ( REMOTE\_DATA\_ARCHIVE\_OVERRIDE = *Wert* )** zur SELECT-Anweisung hinzu. Der REMOTE\_DATA\_ARCHIVE\_OVERRIDE-Abfragehinweis kann folgende Werte haben:

* **LOCAL\_ONLY**. Nur lokale Daten werden abgefragt.
* **REMOTE\_ONLY**. Nur Remotedaten werden abgefragt.
* **STAGE\_ONLY**. Es werden nur die Daten in der Tabelle abgefragt, in der Stretch-Datenbank für die Migration berechtigte Zeilen bereitstellt und migrierte Zeilen für den angegebenen Zeitraum nach der Migration beibehält. Dieser Abfragehinweis ist die einzige Möglichkeit zum Abfragen der Stagingtabelle.

Die folgende Abfrage gibt beispielsweise nur lokale Ergebnisse zurück:

 ```tsql  
 USE <Stretch-enabled database name>;
 GO
 SELECT * FROM <Stretch_enabled table name> WITH (REMOTE_DATA_ARCHIVE_OVERRIDE = LOCAL_ONLY) WHERE ... ;
 GO
```  

## <a name="adminHints"></a>Ausführen administrativer Updates und Löschvorgänge
 Die Befehle UPDATE oder DELETE können standardmäßig nicht für zur Migration berechtigte Zeilen oder für bereits migrierte Zeilen in einer Tabelle ausgeführt werden, für die Stretch aktiviert ist. Zum Beheben eines Problems kann ein Mitglied der Rolle „db\_owner“ einen UPDATE- oder DELETE-Vorgang durch Hinzufügen des Abfragehinweises **WITH ( REMOTE\_DATA\_ARCHIVE\_OVERRIDE = *Wert* )** zur Anweisung ausführen. Der REMOTE\_DATA\_ARCHIVE\_OVERRIDE-Abfragehinweis kann folgende Werte haben:

* **LOCAL\_ONLY**. Nur lokale Daten werden aktualisiert oder gelöscht.
* **REMOTE\_ONLY**. Nur Remotedaten werden aktualisiert oder gelöscht.
* **STAGE\_ONLY**. Es werden nur die Daten in der Tabelle aktualisiert oder gelöscht, in der Stretch-Datenbank für die Migration berechtigte Zeilen bereitstellt und migrierte Zeilen für den angegebenen Zeitraum nach der Migration beibehält.

## Weitere Informationen
[Überwachen von Stretch-Datenbank](sql-server-stretch-database-monitor.md)

[Sichern von Stretch-fähigen Datenbanken](sql-server-stretch-database-backup.md)

[Wiederherstellen von Stretch-fähigen Datenbanken](sql-server-stretch-database-restore.md)

<!---HONumber=AcomDC_0629_2016-->