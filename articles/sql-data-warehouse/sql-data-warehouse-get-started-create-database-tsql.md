<properties
   pageTitle="Erstellen eines SQL Data Warehouse mit TSQL | Microsoft Azure"
   description="Erfahren Sie, wie Sie ein Azure SQL Data Warehouse mit TSQL erstellen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="04/20/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Erstellen einer SQL Data Warehouse-Datenbank mithilfe von Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure-Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In diesem Artikel erfahren Sie, wie Sie eine SQL Data Warehouse-Datenbank mithilfe von Transact-SQL (TSQL) erstellen.

## Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

- Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, müssen Sie lediglich oben auf dieser Seite auf den Link **Kostenlose Testversion** klicken. Lesen Sie anschließend den Artikel weiter.
- Einen logischen V12-Server. Sie benötigen einen V12-SQL-Server zum Erstellen des SQL Data Warehouse. Wenn Sie über keinen logischen V12-SQL-Server verfügen, finden Sie unter **Konfigurieren und Erstellen eines Servers** im Artikel zum [Erstellen eines SQL Data Warehouse im Azure-Portal][] weitere Informationen.
- Visual Studio. Eine kostenlose Version von Visual Studio finden Sie auf der Seite [Visual Studio-Downloads][].


> [AZURE.NOTE] Wenn Sie ein neues SQL Data Warehouse erstellen, wird dadurch unter Umständen auch ein neuer abrechenbarer Dienst erstellt. Unter [SQL Data Warehouse – Preise][] finden Sie weitere Informationen zu den Preisen.

## Erstellen einer Datenbank mit Visual Studio

Wenn Sie mit Visual Studio noch nicht vertraut sind, finden Sie im Artikel [Connect to SQL Data Warehouse with Visual Studio][] (Verbinden mit SQL Data Warehouse mithilfe von Visual Studio) weitere Informationen. Zum Starten öffnen Sie den SQL Server-Objekt-Explorer in Visual Studio und stellen eine Verbindung mit dem Server her, der Ihre SQL Data Warehouse-Datenbank hosten soll. Wenn die Verbindung hergestellt wurde, können Sie ein SQL Data Warehouse erstellen, indem Sie den folgenden SQL-Befehl auf die Datenbank **master** anwenden. Dieser Befehl erstellt die Datenbank „MySqlDwDb“ mit dem Dienstziel DW400, und die Datenbank kann maximal 10 TB groß sein.

```sql
CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## Erstellen Sie eine Datenbank mit SQLCMD

Alternativ können Sie den gleichen Befehl mit „sqlcmd“ ausführen, wenn Sie bei einer Eingabeaufforderung Folgendes ausführen.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

Die Parameter **MAXSIZE** und **SERVICE\_OBJECTIVE** legen den maximalen Speicherplatz fest, den die Datenbank auf einem Datenträger einnehmen kann, sowie die Computeressourcen, die Ihrer Data Warehouse-Instanz zugewiesen ist. Das Dienstziel liegt im Wesentlichen im Zuweisen von CPU und Arbeitsspeicher mit linearer Skalierung in Bezug auf die DWU-Größe.

MAXSIZE kann zwischen 250 GB und 60 TB liegen. Das Dienstziel kann zwischen DW100 und DW2000 liegen. Eine vollständige Liste aller gültigen Werte für MAXSIZE und SERVICE\_OBJECTIVE finden Sie in der MSDN-Dokumentation für [CREATE DATABASE][]. Sowohl MAXSIZE als auch SERVICE\_OBJECTIVE können auch mit einem [ALTER DATABASE][]-T-SQL-Befehl geändert werden. Seien Sie beim Ändern von SERVICE-OBJECTIVE vorsichtig, da dies einen Neustart des Diensts zur Folge hat, was alle aktuellen Abfragen abbricht. MAXSIZE ist ein einfacher Metadatenvorgang und beinhaltet somit weniger Risiken.

## Nächste Schritte
Nach der SQL Data Warehouse-Bereitstellung können Sie [Beispieldaten laden][] oder die Schritte zum [Entwickeln][], [Laden][] oder [Migrieren][] lernen.

<!--Article references-->
[Erstellen eines SQL Data Warehouse im Azure-Portal]: sql-data-warehouse-get-started-provision.md
[Connect to SQL Data Warehouse with Visual Studio]: sql-data-warehouse-get-started-connect.md
[Migrieren]: sql-data-warehouse-overview-migrate.md
[Entwickeln]: sql-data-warehouse-overview-develop.md
[Laden]: sql-data-warehouse-overview-load.md
[Beispieldaten laden]: sql-data-warehouse-get-started-manually-load-samples.md

<!--MSDN references--> 
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[SQL Data Warehouse – Preise]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Visual Studio-Downloads]: https://www.visualstudio.com/downloads/download-visual-studio-vs

<!---HONumber=AcomDC_0511_2016-->