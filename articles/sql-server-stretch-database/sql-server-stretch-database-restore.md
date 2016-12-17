---
title: "Wiederherstellen von Stretch-fähigen Datenbanken | Microsoft Docs"
description: "Erfahren Sie mehr über das Wiederherstellen von Stretch\\-fähigen Datenbanken."
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: 97fdf8d4-1d91-409f-bfce-755e15c79498
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2016
ms.author: douglasl
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7f892604d863225625c8d3b05bb4c8b985be7d5f


---
# <a name="restore-stretch-enabled-databases"></a>Wiederherstellen von Stretch-fähigen Datenbanken
Nach vielen Arten von Ausfällen, Fehlern und Notfällen können Sie bei Bedarf eine gesicherte Datenbank wiederherstellen.

Weitere Informationen zur Datensicherung finden Sie unter [Sichern von Stretch-fähigen Datenbanken](sql-server-stretch-database-backup.md).

> [!NOTE]
> Die Datensicherung ist nur ein Teil einer vollständigen Lösung für hohe Verfügbarkeit und Geschäftskontinuität. Weitere Informationen zu hoher Verfügbarkeit finden Sie unter [Lösungen für hohe Verfügbarkeit](https://msdn.microsoft.com/library/ms190202.aspx).
> 
> 

## <a name="restore-your-sql-server-data"></a>Wiederherstellen von SQL Server-Daten
Nach einem Hardwarefehler oder einer Beschädigung können Sie die Stretch\-fähige SQL Server-Datenbank aus einer Sicherungskopie wiederherstellen. Zum Wiederherstellen können Sie weiterhin die bereits verwendeten SQL Server-Methoden nutzen. Weitere Informationen finden Sie unter [Übersicht über Wiederherstellungsvorgänge](https://msdn.microsoft.com/library/ms191253.aspx).

Nach der Wiederherstellung der SQL Server-Datenbank müssen Sie die gespeicherte Prozedur **sys.sp_rda_reauthorize_db** ausführen, um erneut die Verbindung zwischen der Stretch\-fähigen SQL Server-Datenbank und der Azure-Remotedatenbank herzustellen. Weitere Informationen finden Sie unter [Wiederherstellen der Verbindung zwischen SQL Server-Datenbank und der Azure-Remotedatenbank](#restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database).

## <a name="restore-your-remote-azure-data"></a>Wiederherstellen von Azure-Remotedaten
### <a name="recover-a-live-azure-database"></a>Wiederherstellen einer Azure-Livedatenbank
Der Dienst SQL Server Stretch-Datenbank erstellt mit Azure Storage mindestens alle 8 Stunden Momentaufnahmen sämtlicher Livedaten. Diese Momentaufnahmen werden 7 Tage lang aufbewahrt. Dadurch können Sie Daten an einem von mindestens 21 Punkten innerhalb der letzten 7 Tage bis zu dem Zeitpunkt wiederherstellen, zu dem die letzte Momentaufnahme erstellt wurde.

Gehen Sie folgendermaßen vor, um mithilfe des Azure-Portals eine Azure-Datenbank auf einen früheren Zeitpunkt wiederherzustellen.

1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL-Datenbanken** aus.
3. Navigieren Sie zu Ihrer Datenbank, und wählen Sie sie aus.
4. Klicken Sie oben im Blatt für die Datenbank auf **Wiederherstellen**.
5. Geben Sie einen neuen **Datenbanknamen** an, wählen Sie einen **Wiederherstellungspunkt** aus, und klicken Sie anschließend auf **Erstellen**.
6. Der Datenbank-Wiederherstellungsvorgang wird gestartet und kann mithilfe von **BENACHRICHTIGUNGEN**überwacht werden.

### <a name="recover-a-deleted-azure-database"></a>Wiederherstellen einer gelöschten Azure-Datenbank
Der Azure-Dienst SQL Server Stretch-Datenbank erstellt eine Datenbankmomentaufnahme, bevor eine Datenbank gelöscht wird, und bewahrt diese sieben Tage lang auf. Danach werden keine Momentaufnahmen der Livedatenbank mehr beibehalten. Auf diese Weise können Sie eine gelöschte Datenbank auf den Punkt wiederherstellen, zu dem sie gelöscht wurde.

Gehen Sie folgendermaßen vor, um mit dem Azure-Portal eine Azure-Datenbank auf den Zeitpunkt wiederherzustellen, zu dem sie gelöscht wurde.

1. Melden Sie sich beim Azure-Portal an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL Server** aus.
3. Navigieren Sie zu Ihrem Server, und wählen Sie ihn aus.
4. Scrollen Sie auf dem Blatt für Ihren Server nach unten zu „Vorgänge“, und klicken Sie auf die Kachel **Gelöschte Datenbanken** .
5. Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
6. Geben Sie einen neuen **Datenbanknamen** an, und klicken Sie auf **Erstellen**.
7. Der Datenbank-Wiederherstellungsvorgang wird gestartet und kann mithilfe von **BENACHRICHTIGUNGEN**überwacht werden.

## <a name="restore-the-connection-between-the-sql-server-database-and-the-remote-azure-database"></a>Wiederherstellen der Verbindung zwischen SQL Server-Datenbank und der Azure-Remotedatenbank
1. Wenn Sie eine Verbindung mit einer wiederhergestellten Azure-Datenbank mit einem anderen Namen oder in einer anderen Region herstellen möchten, führen Sie die gespeicherte Prozedur [sys.sp_rda_deauthorize_db](https://msdn.microsoft.com/library/mt703716.aspx) aus, um die Verbindung mit der vorherigen Azure-Datenbank zu trennen.  
2. Führen Sie die gespeicherte Prozedur [sys.sp_rda_reauthorize_db](https://msdn.microsoft.com/library/mt131016.aspx) aus, um die Verbindung der lokalen Stretch\-fähigen Datenbank mit der Azure-Datenbank erneut herzustellen.  
   
   * Geben Sie die vorhandenen datenbankbezogenen Anmeldeinformationen als einen Wert vom Datentyp „sysname“ oder „varchar\(128\)“ ein. \(Verwenden Sie nicht „varchar\(max\)“.\) Sie können den Anmeldenamen in der Ansicht **sys.database\_scoped\_credentials** nachschlagen.  
   * Geben Sie an, ob eine Kopie der Remotedaten erstellt und eine Verbindung mit der Kopie hergestellt werden soll (empfohlen).  
   
   ```tsql  
   USE <Stretch-enabled database name>;
   GO
   EXEC sp_rda_reauthorize_db
       @credential = N'<existing_database_scoped_credential_name>',
       @with_copy = 1 ;  
   GO
   ```  

## <a name="see-also"></a>Weitere Informationen
[Verwalten von Stretch-Datenbank und Behandeln von Problemen](sql-server-stretch-database-manage.md)

[Sys.sp_rda_reauthorize_db (Transact-SQL)](https://msdn.microsoft.com/library/mt131016.aspx)

[Sichern und Wiederherstellen von SQL Server-Datenbanken](https://msdn.microsoft.com/library/ms187048.aspx)




<!--HONumber=Nov16_HO3-->


