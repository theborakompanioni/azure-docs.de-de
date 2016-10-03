<properties
   pageTitle="Problembehandlung bei Azure SQL Data Warehouse | Microsoft Azure"
   description="Problembehandlung bei Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="sonyama;barbkess"/>

# Problembehandlung bei Azure SQL Data Warehouse

In diesem Thema sind einige häufige Fragen zur Problembehandlung aufgeführt, die von Kunden gestellt werden.

## Herstellen einer Verbindung

| Problem | Lösung |
| :----------------------------------| :---------------------------------------------- |
| Fehler bei der Anmeldung für den Benutzer 'NT-AUTORITÄT\\ANONYME ANMELDUNG'. (Microsoft SQL Server, Fehler: 18456) | Dieser Fehler tritt auf, wenn ein AAD-Benutzer versucht, eine Verbindung mit der Masterdatenbank herzustellen, aber nicht über einen Masterdatenbank-Benutzer verfügt. Zum Beheben dieses Problems geben Sie entweder das SQL Data Warehouse an, mit dem Sie gerade eine Verbindung herstellen möchten, oder fügen Sie den Benutzer der Masterdatenbank hinzu. Weitere Informationen finden Sie im Artikel [Sichern einer Datenbank in SQL Data Warehouse][].|
|Der Serverprinzipal „MeinBenutzername“ kann unter dem aktuellen Sicherheitskontext nicht auf die Datenbank „Master“ zugreifen. Standardbenutzerdatenbank kann nicht geöffnet werden. Anmeldefehler. Fehler bei der Anmeldung für den Benutzer 'MeinBenutzername'. (Microsoft SQL Server, Fehler: 916) | Dieser Fehler tritt auf, wenn ein AAD-Benutzer versucht, eine Verbindung mit der Masterdatenbank herzustellen, aber nicht über einen Masterdatenbank-Benutzer verfügt. Zum Beheben dieses Problems geben Sie entweder das SQL Data Warehouse an, mit dem Sie gerade eine Verbindung herstellen möchten, oder fügen Sie den Benutzer der Masterdatenbank hinzu. Weitere Informationen finden Sie im Artikel [Sichern einer Datenbank in SQL Data Warehouse][].|
| CTAIP-Fehler | Dieser Fehler kann auftreten, wenn zwar eine Anmeldung für die SQL Server-Masterdatenbank erstellt wurde, dies jedoch in der SQL Data Warehouse-Datenbank unterlassen wurde. Lesen Sie den [Übersichtsartikel zur Sicherheit][], wenn dieser Fehler auftritt. In diesem Artikel wird erläutert, wie Sie zunächst eine Anmeldung und einen Benutzer für die Masterdatenbank erstellen und anschließend in der SQL Data Warehouse-Datenbank einen Benutzer erstellen.|
| Von der Firewall blockiert |Azure SQL-Datenbanken sind durch Firewalls auf Server- und Datenbankebene geschützt, um sicherzustellen, dass nur bekannte IP-Adressen Zugriff auf eine Datenbank haben. Firewalls sind standardmäßig sicher. Sie müssen daher eine IP-Adresse oder einen Adressbereich explizit aktivieren, bevor Sie eine Verbindung herstellen können. Führen Sie die Schritte im Abschnitt [Erstellen einer neuen Azure SQL-Firewall auf Serverebene][] des Artikels [Erstellen einer Azure SQL Data Warehouse-Instanz][] aus, um Ihre Firewall für den Zugriff zu konfigurieren.|
| Verbindung mit Tool oder Treiber kann nicht hergestellt werden | Für SQL Data Warehouse wird empfohlen, [SSMS][], [SSDT für Visual Studio 2015][] oder [sqlcmd][] zum Abfragen von Daten zu verwenden. Weitere Informationen zu Treibern und zum Herstellen einer Verbindung mit SQL Data Warehouse finden Sie in den Artikeln [Treiber für Azure SQL Data Warehouse][] und [Verbinden mit Azure SQL Data Warehouse][].|


## Tools

| Problem | Lösung |
| :----------------------------------| :---------------------------------------------- |
| In Visual Studio-Objekt-Explorer fehlen AAD-Benutzer | Dies ist ein bekanntes Problem. Die Benutzer können aber in [sys.database\_principals][] angezeigt werden. Weitere Informationen zur Verwendung von Azure Active Directory mit SQL Data Warehouse finden Sie unter [Authentifizierung in Azure SQL Data Warehouse][].|

## Leistung

| Problem | Lösung |
| :----------------------------------| :---------------------------------------------- |
| Behandlung von Problemen mit der Abfrageleistung | Wenn Sie die Problembehandlung für eine bestimmte Abfrage durchführen möchten, sollten Sie sich zunächst über das [Untersuchen der Ausführung von Abfragen][] informieren.|
| Schlechte Abfrageleistung und Planung ist häufig das Ergebnis fehlender Statistiken | Die häufigste Ursache für schlechte Leistung ist das Fehlen von Statistiken für Ihre Tabellen. Ausführliche Informationen dazu, wie Sie Statistiken erstellen und warum sie für die Leistung wichtig sind, finden Sie unter [Managing statistics on tables in SQL Data Warehouse][Statistics] \(Verwalten von Statistiken für Tabellen in SQL Data Warehouse).|
| Geringe Parallelität/Abfragen in der Warteschlange | Das Verständnis der [Workloadverwaltung][] ist wichtig, damit Sie wissen, wie Sie die Speicherbelegung und die Parallelität abwägen sollen.|
| Implementieren von bewährten Methoden | Wenn Sie die Leistung bei Ihren Abfragen verbessern möchten, ist der Artikel [Bewährte Methoden für SQL Data Warehouse][] ein idealer Ausgangspunkt.|
| Verbessern der Leistung mit der Skalierung | Mitunter besteht der Weg zum Verbessern der Leistung einfach darin, den Abfragen mehr Computeleistung hinzuzufügen, indem Sie Ihr [SQL Data Warehouse skalieren][].|
| Schlechte Leistung aufgrund von schlechter Indexqualität | Es kann vorkommen, dass sich Abfragen verlangsamen, weil eine [schlechte Qualität der Columnstore-Indizes][] vorliegt. Weitere Informationen finden Sie unter [Rebuild indexes to improve segment quality][] \(Neuerstellen von Indizes zum Verbessern der Segmentqualität).|

## Systemverwaltung

| Problem | Lösung |
| :----------------------------------| :---------------------------------------------- |
| Msg 40847: Der Vorgang konnte nicht ausgeführt werden, da der Server das zulässige Datenbanktransaktionseinheit-Kontingent von 45.000 überschreiten würde. | Reduzieren Sie entweder die [DWU][] der Datenbank, die Sie erstellen möchten, oder [fordern Sie eine Erhöhung des Kontingents][] an.|
| Untersuchen der Speicherauslastung | Informationen zu den Grundlagen der Speicherauslastung des Systems finden Sie unter [Tabellengrößen][].|
| Hilfe beim Verwalten von Tabellen | Hilfe zur Verwaltung von Tabellen finden Sie unter [Übersicht über Tabellen][Overview]. Dieser Artikel enthält auch Links zu ausführlicheren Themen, z.B. [Tabellendatentypen][Data types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition], [Verwalten von Tabellenstatistiken][Statistics] und [Temporäre Tabellen][Temporary].|

## PolyBase

| Problem | Lösung |
| :----------------------------------| :---------------------------------------------- |
| UTF-8-Fehler | Derzeit unterstützt PolyBase nur das Laden von Datendateien mit UTF-8-Codierung. Eine Anleitung zur Umgehung dieser Beschränkung finden Sie unter [Umgehen der UTF-8-Anforderung von PolyBase][].|
| Fehler beim Laden aufgrund von umfangreichen Zeilen | Umfangreiche Zeilen werden für PolyBase derzeit nicht unterstützt. Dies bedeutet, dass keine externen Tabellen zum Laden der Daten verwendet werden können, wenn die Tabelle VARCHAR(MAX), NVARCHAR(MAX) oder VARBINARY(MAX) enthält. Ladevorgänge für umfangreiche Zeilen werden derzeit nur von Azure Data Factory (mit BCP), Azure Stream Analytics, SSIS, BCP oder der SQLBulkCopy-.NET-Klasse unterstützt. Die PolyBase-Unterstützung für umfangreiche Zeilen wird in einer zukünftigen Version hinzugefügt.|
| Fehler beim BCP-Ladevorgang einer Tabelle mit MAX-Datentyp | Es besteht das folgende bekannte Problem: VARCHAR(MAX), NVARCHAR(MAX) oder VARBINARY(MAX) müssen in manchen Szenarien am Ende der Tabelle angeordnet werden. Versuchen Sie, die MAX-Spalten an das Ende der Tabelle zu verschieben.|

## Unterschiede zu SQL-Datenbank

| Problem | Lösung |
| :----------------------------------| :---------------------------------------------- |
| Nicht unterstützte Funktionen von SQL-Datenbank | Siehe [Nicht unterstützte Tabellenfunktionen][].|
| Nicht unterstützte SQL-Datenbank-Datentypen | Siehe [Nicht unterstützte Datentypen][].|
| DELETE- und UPDATE-Einschränkungen | Siehe Informationen zu [UPDATE-Problemumgehungen][], [DELETE-Problemumgehungen][] und zum [Verwenden von CTAS als Problemumgehung für nicht unterstützte UPDATE- und DELETE-Syntax][]. |
| MERGE-Anweisung wird nicht unterstützt | Siehe [MERGE-Problemumgehungen][].|
| Einschränkungen für gespeicherte Prozeduren | In „Gespeicherte Prozeduren in SQL Data Warehouse“ werden unter [Einschränkungen][] einige Einschränkungen für gespeicherte Prozeduren beschrieben.|
| UDFs unterstützen keine SELECT-Anweisungen | Dies ist eine aktuelle Beschränkung unserer benutzerdefinierten Funktionen (User-Defined Functions, UDFs). Informationen zur unterstützten Syntax finden Sie unter [CREATE FUNCTION][]. |

## Nächste Schritte

Wenn Sie bisher keine Lösung für Ihr Problem gefunden haben, können Sie folgende Ressourcen nutzen:

- [Blogs]
- [Funktionsanfragen]
- [Videos]
- [CAT Team-Blogs]
- [Erstellen eines Supporttickets]
- [MSDN-Forum]
- [Stack Overflow-Forum]
- [Twitter]

<!--Image references-->

<!--Article references-->
[Sichern einer Datenbank in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Übersichtsartikel zur Sicherheit]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT für Visual Studio 2015]: ./sql-data-warehouse-install-visual-studio.md
[Treiber für Azure SQL Data Warehouse]: ./sql-data-warehouse-connection-strings.md
[Verbinden mit Azure SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Erstellen eines Supporttickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[SQL Data Warehouse skalieren]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[fordern Sie eine Erhöhung des Kontingents]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Untersuchen der Ausführung von Abfragen]: ./sql-data-warehouse-manage-monitor.md
[Erstellen einer Azure SQL Data Warehouse-Instanz]: ./sql-data-warehouse-get-started-provision.md
[Erstellen einer neuen Azure SQL-Firewall auf Serverebene]: ./sql-data-warehouse-get-started-provision.md#create-a-new-azure-sql-server-level-firewall
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Tabellengrößen]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Nicht unterstützte Tabellenfunktionen]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Nicht unterstützte Datentypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[schlechte Qualität der Columnstore-Indizes]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workloadverwaltung]: ./sql-data-warehouse-develop-concurrency.md
[Verwenden von CTAS als Problemumgehung für nicht unterstützte UPDATE- und DELETE-Syntax]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[UPDATE-Problemumgehungen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[DELETE-Problemumgehungen]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE-Problemumgehungen]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Einschränkungen]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentifizierung in Azure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md
[Umgehen der UTF-8-Anforderung von PolyBase]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database\_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[CREATE FUNCTION]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT Team-Blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funktionsanfragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-Forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stack Overflow-Forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

<!---HONumber=AcomDC_0907_2016-->