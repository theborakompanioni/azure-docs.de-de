---
title: Problembehandlung bei Azure SQL Data Warehouse | Microsoft-Dokumentation
description: Problembehandlung bei Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 51f1e444-9ef7-4e30-9a88-598946c45196
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 5d3bcc3c1434b16279778573ccf3034f9ac28a4d
ms.openlocfilehash: ecc008469ba754af12583d350706f626ec6690b5


---
# <a name="troubleshooting-azure-sql-data-warehouse"></a>Problembehandlung bei Azure SQL Data Warehouse
In diesem Thema sind einige häufige Fragen zur Problembehandlung aufgeführt, die von Kunden gestellt werden.

## <a name="connecting"></a>Herstellen einer Verbindung
| Problem | Lösung |
|:--- |:--- |
| Fehler bei der Anmeldung für den Benutzer 'NT-AUTORITÄT\ANONYME ANMELDUNG'. (Microsoft SQL Server, Fehler: 18456) |Dieser Fehler tritt auf, wenn ein AAD-Benutzer versucht, eine Verbindung mit der Masterdatenbank herzustellen, aber nicht über einen Masterdatenbank-Benutzer verfügt.  Zum Beheben dieses Problems geben Sie entweder das SQL Data Warehouse an, mit dem Sie gerade eine Verbindung herstellen möchten, oder fügen Sie den Benutzer der Masterdatenbank hinzu.  Weitere Informationen finden Sie im Artikel [Sichern einer Datenbank in SQL Data Warehouse][Security overview]. |
| Der Serverprinzipal „MeinBenutzername“ kann unter dem aktuellen Sicherheitskontext nicht auf die Datenbank „Master“ zugreifen. Standardbenutzerdatenbank kann nicht geöffnet werden. Anmeldefehler. Fehler bei der Anmeldung für den Benutzer 'MeinBenutzername'. (Microsoft SQL Server, Fehler: 916) |Dieser Fehler tritt auf, wenn ein AAD-Benutzer versucht, eine Verbindung mit der Masterdatenbank herzustellen, aber nicht über einen Masterdatenbank-Benutzer verfügt.  Zum Beheben dieses Problems geben Sie entweder das SQL Data Warehouse an, mit dem Sie gerade eine Verbindung herstellen möchten, oder fügen Sie den Benutzer der Masterdatenbank hinzu.  Weitere Informationen finden Sie im Artikel [Sichern einer Datenbank in SQL Data Warehouse][Security overview]. |
| CTAIP-Fehler |Dieser Fehler kann auftreten, wenn zwar eine Anmeldung für die SQL Server-Masterdatenbank erstellt wurde, dies jedoch in der SQL Data Warehouse-Datenbank unterlassen wurde.  Lesen Sie in diesem Fall den Artikel [Sichern einer Datenbank in SQL Data Warehouse][Security overview].  In diesem Artikel wird erläutert, wie Sie zunächst eine Anmeldung und einen Benutzer für die Masterdatenbank erstellen und anschließend in der SQL Data Warehouse-Datenbank einen Benutzer erstellen. |
| Von der Firewall blockiert |Azure SQL-Datenbanken sind durch Firewalls auf Server- und Datenbankebene geschützt, um sicherzustellen, dass nur bekannte IP-Adressen Zugriff auf eine Datenbank haben. Firewalls sind standardmäßig sicher. Sie müssen daher eine IP-Adresse oder einen Adressbereich explizit aktivieren, bevor Sie eine Verbindung herstellen können.  Führen Sie die Schritte aus dem Abschnitt [Erstellen einer Firewallregel auf Serverebene im Azure-Portal][Configure server firewall access for your client IP] der [Bereitstellungsanweisungen][Provisioning instructions] aus, um Ihre Firewall für den Zugriff zu konfigurieren. |
| Verbindung mit Tool oder Treiber kann nicht hergestellt werden |SQL Data Warehouse empfiehlt, zum Abfragen von Daten [SSMS][SSMS], [SSDT für Visual Studio 2015][SSDT for Visual Studio 2015] oder [sqlcmd][sqlcmd] zu verwenden. Ausführlichere Informationen zu Treibern und zum Herstellen einer Verbindung mit SQL Data Warehouse finden Sie in den Artikeln [Treiber für Azure SQL Data Warehouse][Drivers for Azure SQL Data Warehouse] und [Verbinden mit Azure SQL Data Warehouse][Connect to Azure SQL Data Warehouse]. |

## <a name="tools"></a>Tools
| Problem | Lösung |
|:--- |:--- |
| In Visual Studio-Objekt-Explorer fehlen AAD-Benutzer |Dies ist ein bekanntes Problem.  Sie können die Benutzer in [ys.database_principals][sys.database_principals] anzeigen, um dieses Problem zu umgehen.  Weitere Informationen zur Verwendung von Azure Active Directory mit SQL Data Warehouse finden Sie unter [Authentifizierung in Azure SQL Data Warehouse][Authentication to Azure SQL Data Warehouse]. |

## <a name="performance"></a>Leistung
| Problem | Lösung |
|:--- |:--- |
| Behandlung von Problemen mit der Abfrageleistung |Wenn Sie die Problembehandlung für eine bestimmte Abfrage durchführen möchten, sollten Sie sich zunächst über das [Untersuchen der Ausführung von Abfragen][Learning how to monitor your queries] informieren. |
| Schlechte Abfrageleistung und Planung ist häufig das Ergebnis fehlender Statistiken |Die häufigste Ursache für schlechte Leistung ist das Fehlen von Statistiken für Ihre Tabellen.  Ausführliche Informationen dazu, wie Sie Statistiken erstellen und warum sie für die Leistung wichtig sind, finden Sie unter [Verwalten von Statistiken für Tabellen in SQL Data Warehouse][Statistics]. |
| Geringe Parallelität/Abfragen in der Warteschlange |Das Verständnis der [Workloadverwaltung][Workload management] ist wichtig zur Abwägung von Speicherbelegung und Parallelität. |
| Implementieren von bewährten Methoden |Wenn Sie die Leistung bei Ihren Abfragen verbessern möchten, ist der Artikel [Bewährte Methoden für SQL Data Warehouse][SQL Data Warehouse best practices] ein idealer Ausgangspunkt. |
| Verbessern der Leistung mit der Skalierung |Mitunter besteht der Weg zum Verbessern der Leistung einfach darin, mehr Computeleistung für Abfragen zur Verfügung zu stellen, indem Sie [SQL Data Warehouse skalieren][Scaling your SQL Data Warehouse]. |
| Schlechte Leistung aufgrund von schlechter Indexqualität |Es kann vorkommen, dass sich Abfragen verlangsamen, da eine [schlechte Qualität der Columnstore-Indizes][Poor columnstore index quality] vorliegt.  Weitere Informationen finden Sie unter [Neuerstellen von Indizes zur Verbesserung der Segmentqualität][Rebuild indexes to improve segment quality]. |

## <a name="system-management"></a>Systemverwaltung
| Problem | Lösung |
|:--- |:--- |
| Msg 40847: Der Vorgang konnte nicht ausgeführt werden, da der Server das zulässige Datenbanktransaktionseinheit-Kontingent von 45.000 überschreiten würde. |Reduzieren Sie entweder die [DWU][DWU] der Datenbank, die Sie erstellen möchten, oder [fordern Sie eine Erhöhung des Kontingents an][request a quota increase]. |
| Untersuchen der Speicherauslastung |Informationen zu den Grundlagen der Speicherauslastung des Systems finden Sie unter [Tabellengrößen][Table sizes]. |
| Hilfe beim Verwalten von Tabellen |Hilfe zur Verwaltung von Tabellen finden Sie in der [Übersicht über Tabellen][Overview].  Dieser Artikel enthält auch Links zu ausführlicheren Themen. Hierzu zählen beispielsweise [Tabellendatentypen][Data types], [Verteilen einer Tabelle][Distribute], [Indizieren einer Tabelle][Index], [Partitionieren einer Tabelle][Partition], [Verwalten von Tabellenstatistiken][Statistics] und [Temporäre Tabellen][Temporary]. |

## <a name="polybase"></a>PolyBase
| Problem | Lösung |
|:--- |:--- |
| UTF-8-Fehler |Derzeit unterstützt PolyBase nur das Laden von Datendateien mit UTF-8-Codierung.  Eine Anleitung zur Umgehung dieser Beschränkung finden Sie unter [Umgehen der UTF-8-Anforderung von PolyBase][Working around the PolyBase UTF-8 requirement]. |
| Fehler beim Laden aufgrund von umfangreichen Zeilen |Umfangreiche Zeilen werden für PolyBase derzeit nicht unterstützt.  Dies bedeutet, dass keine externen Tabellen zum Laden der Daten verwendet werden können, wenn die Tabelle VARCHAR(MAX), NVARCHAR(MAX) oder VARBINARY(MAX) enthält.  Ladevorgänge für umfangreiche Zeilen werden derzeit nur von Azure Data Factory (mit BCP), Azure Stream Analytics, SSIS, BCP oder der SQLBulkCopy-.NET-Klasse unterstützt. Die PolyBase-Unterstützung für umfangreiche Zeilen wird in einer zukünftigen Version hinzugefügt. |
| Fehler beim BCP-Ladevorgang einer Tabelle mit MAX-Datentyp |Es besteht das folgende bekannte Problem: VARCHAR(MAX), NVARCHAR(MAX) oder VARBINARY(MAX) müssen in manchen Szenarien am Ende der Tabelle angeordnet werden.  Versuchen Sie, die MAX-Spalten an das Ende der Tabelle zu verschieben. |

## <a name="differences-from-sql-database"></a>Unterschiede zu SQL-Datenbank
| Problem | Lösung |
|:--- |:--- |
| Nicht unterstützte Funktionen von SQL-Datenbank |Siehe [Nicht unterstützte Tabellenfunktionen][Unsupported table features]. |
| Nicht unterstützte SQL-Datenbank-Datentypen |Siehe [Nicht unterstützte Datentypen][Unsupported data types]. |
| DELETE- und UPDATE-Einschränkungen |Siehe [UPDATE-Problemumgehungen][UPDATE workarounds], [DELETE-Problemumgehungen][DELETE workarounds] und [Umgehen von nicht unterstützten Funktionen mit CTAS][Using CTAS to work around unsupported UPDATE and DELETE syntax]. |
| MERGE-Anweisung wird nicht unterstützt |Siehe [MERGE-Problemumgehungen][MERGE workarounds]. |
| Einschränkungen für gespeicherte Prozeduren |In „Gespeicherte Prozeduren in SQL Data Warehouse“ werden unter [Einschränkungen][Stored procedure limitations] einige Einschränkungen für gespeicherte Prozeduren beschrieben. |
| UDFs unterstützen keine SELECT-Anweisungen |Dies ist eine aktuelle Beschränkung unserer benutzerdefinierten Funktionen (User-Defined Functions, UDFs).  Informationen zur unterstützten Syntax finden Sie unter der [CREATE-FUNKTION][CREATE FUNCTION]. |

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie bisher keine Lösung für Ihr Problem gefunden haben, können Sie folgende Ressourcen nutzen:

* [Blogs]
* [Funktionsanfragen]
* [Videos]
* [CAT Team-Blogs]
* [Erstellen eines Supporttickets]
* [MSDN-Forum]
* [Stack Overflow-Forum]
* [Twitter]

<!--Image references-->

<!--Article references-->
[Security overview]: ./sql-data-warehouse-overview-manage-security.md
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx
[SSDT for Visual Studio 2015]: ./sql-data-warehouse-install-visual-studio.md
[Drivers for Azure SQL Data Warehouse]: ./sql-data-warehouse-connection-strings.md
[Connect to Azure SQL Data Warehouse]: ./sql-data-warehouse-connect-overview.md
[Erstellen eines Supporttickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Scaling your SQL Data Warehouse]: ./sql-data-warehouse-manage-compute-overview.md
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[request a quota increase]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change
[Learning how to monitor your queries]: ./sql-data-warehouse-manage-monitor.md
[Provisioning instructions]: ./sql-data-warehouse-get-started-provision.md
[Configure server firewall access for your client IP]: ./sql-data-warehouse-get-started-provision.md#create-a-server-level-firewall-rule-in-the-azure-portal
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[Table sizes]: ./sql-data-warehouse-tables-overview.md#table-size-queries
[Unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[Poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuild indexes to improve segment quality]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Workload management]: ./sql-data-warehouse-develop-concurrency.md
[Using CTAS to work around unsupported UPDATE and DELETE syntax]: ./sql-data-warehouse-develop-ctas.md#using-ctas-to-work-around-unsupported-features
[UPDATE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements
[DELETE workarounds]: ./sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements
[MERGE workarounds]: ./sql-data-warehouse-develop-ctas.md#replace-merge-statements
[Stored procedure limitations]: ./sql-data-warehouse-develop-stored-procedures.md#limitations
[Authentication to Azure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md
[Working around the PolyBase UTF-8 requirement]: ./sql-data-warehouse-load-polybase-guide.md#working-around-the-polybase-utf-8-requirement

<!--MSDN references-->
[sys.database_principals]: https://msdn.microsoft.com/library/ms187328.aspx
[CREATE FUNCTION]: https://msdn.microsoft.com/library/mt203952.aspx
[sqlcmd]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-connect-sqlcmd/

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[CAT Team-Blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funktionsanfragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-Forum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse
[Stack Overflow-Forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse



<!--HONumber=Feb17_HO2-->


