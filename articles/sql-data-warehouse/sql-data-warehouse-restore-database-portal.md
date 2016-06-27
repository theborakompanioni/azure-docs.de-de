<properties
   pageTitle="Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Portal) | Microsoft Azure"
   description="Azure-Portal-Aufgaben zum Wiederherstellen einer Azure SQL Data Warehouse-Instanz."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="elfisher"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/11/2016"
   ms.author="elfish;barbkess;sonyama"/>

# Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Portal)

> [AZURE.SELECTOR]
- [Übersicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In diesem Artikel erfahren Sie, wie Sie eine Azure SQL Data Warehouse-Instanz über das Azure-Portal wiederherstellen.

## Voraussetzungen

**Überprüfen Sie Ihre DTU-Kapazität.** Jede SQL Data Warehouse-Instanz wird von einem logischen Server mit SQL Server gehostet. Dieser logische Server verfügt über ein in DTU gemessenes Kapazitätslimit. Vor dem Wiederherstellen einer SQL Data Warehouse-Instanz muss zunächst geprüft werden, ob die DTU-Kapazität des logischen Servers mit SQL Server, der als Host für Ihre Datenbank fungiert, für die wiederherzustellende Datenbank ausreicht. Weitere Informationen zum Anzeigen und Erhöhen des DTU-Kontingents finden Sie in [diesem Blogbeitrag][].


## Wiederherstellen einer aktiven oder angehaltenen Datenbank

So stellen Sie eine Datenbank wieder her:

1. Melden Sie sich beim [Azure-Portal][] an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL-Datenbanken** aus.
3. Navigieren Sie zu Ihrer Datenbank, und wählen Sie sie aus.
4. Klicken Sie oben auf dem Blatt für die Datenbank auf **Wiederherstellen**.
5. Geben Sie einen neuen Datenbanknamen an, wählen Sie einen Wiederherstellungspunkt aus, und klicken Sie anschließend auf **Erstellen**.
6. Der Datenbank-Wiederherstellungsvorgang wird gestartet und kann anhand von **BENACHRICHTIGUNGEN** überwacht werden.

## Wiederherstellen einer gelöschten Datenbank

So stellen Sie eine gelöschte Datenbank wieder her

1. Melden Sie sich beim [Azure-Portal][] an.
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** und dann **SQL Server** aus.
3. Navigieren Sie zu Ihrem Server, und wählen Sie ihn aus.
4. Scrollen Sie auf dem Blatt für Ihren Server nach unten zu „Vorgänge“, und klicken Sie auf die Kachel **Gelöschte Datenbanken**.
5. Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
5. Geben Sie einen neuen Datenbanknamen an, und klicken Sie auf **Erstellen**.
6. Der Datenbank-Wiederherstellungsvorgang wird gestartet und kann anhand von **BENACHRICHTIGUNGEN** überwacht werden.


## Nächste Schritte
Informationen zu den Geschäftskontinuitätsfeatures von Azure SQL-Datenbank-Editionen finden Sie in der [Übersicht über die Geschäftskontinuität der Azure SQL-Datenbank][].

<!--Image references-->

<!--Article references-->
[Übersicht über die Geschäftskontinuität der Azure SQL-Datenbank]: ./sql-database-business-continuity.md
[Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->

<!--Blog references-->
[diesem Blogbeitrag]: https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/

<!--Other Web references-->
[Azure-Portal]: https://portal.azure.com/

<!---HONumber=AcomDC_0615_2016-->