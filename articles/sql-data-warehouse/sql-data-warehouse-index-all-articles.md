---
title: Alle Themen zum SQL Data Warehouse-Dienst | Microsoft Docs
description: "Tabelle mit allen Themen für den Azure-Dienst namens „SQL Data Warehouse“ in https://azure.microsoft.com/de-de/documentation/articles/, Titel und Beschreibung."
services: sql-data-warehouse
documentationcenter: 
author: barbkess
manager: jhubbard
editor: 
ms.assetid: a26a6dec-9c08-4415-8f58-4ee1dd41f718
ms.service: sql-data-warehouse
ms.workload: sql-data-warehouse
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: reference
ms.date: 03/30/2017
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 9fe41f12960dc099700e01573b4f03ebf63f8749
ms.lasthandoff: 04/03/2017


---
# <a name="all-topics-for-azure-sql-data-warehouse-service"></a>Alle Themen für den Azure SQL Data Warehouse-Dienst
Dieses Thema führt alle Themen mit direktem Bezug zum **SQL Data Warehouse-Dienst** in Azure auf. Sie können auf dieser Webseite mit **STRG+F**nach Schlüsselwörtern suchen, um aktuell interessante Themen zu finden.

## <a name="new"></a>Neu
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 1 |[SQL Data Warehouse-Sicherungen](sql-data-warehouse-backups.md) |Erfahren Sie, wie integrierte SQL Data Warehouse-Datenbanksicherungen Ihnen ermöglichen, ein Azure SQL Data Warehouse auf einen Wiederherstellungspunkt in einer anderen geografischen Region wiederherzustellen. |

## <a name="updated-articles-sql-data-warehouse"></a>Aktualisierte Artikel zu SQL Data Warehouse
In diesem Abschnitt werden Artikel aufgelistet, die kürzlich mit einem sehr umfangreichen oder wesentlichen Update aktualisiert wurden. Für jeden aktualisierten Artikel wird ein grober Ausschnitt des hinzugefügten Markdowntexts angezeigt. Die Artikel wurden zwischen dem **22.08.2016** und dem **05.10.2016** aktualisiert.

| &nbsp; | Artikel | Aktualisierter Text, Ausschnitt | Aktualisierungsdatum |
| ---:|:--- |:--- |:--- |
| 2 |[Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) |/- To track bytes and files SELECT  r.command,  s.request_id,  r.status,  count(distinct input_name) as nbr_files,  sum(s.bytes_processed)/1024/1024 as gb_processed FROM  sys.dm_pdw_exec_requests r  inner join sys.dm_pdw_dms_external_work s  on r.request_id = s.request_id WHERE  r. label  = 'CTAS : Load  cso . DimProduct  '  OR r. label  = 'CTAS : Load  cso . FactOnlineSales  ' GROUP BY  r.command,  s.request_id,  r.status ORDER BY  nbr_files desc,  gb_processed desc; |2016-09-07 |
| 3 |[SQL Data Warehouse-Wiederherstellung](sql-data-warehouse-restore-database-overview.md) |** Kann ich ein angehaltenes Data Warehouse wiederherstellen?** Um ein angehaltenes Data Warehouse wiederherzustellen, müssen Sie das Data Warehouse zuerst wieder online schalten. Sobald das Data Warehouse wieder online geschaltet wurde, stehen Ihnen Wiederherstellungspunkte aus sieben Tage zur Auswahl. **Wiederherstellen in einer georedundanten Region** Wenn Sie georedundanten Speicher verwenden, können Sie das Data Warehouse im gekoppelten Rechenzentrum in einer anderen geografischen Region wiederherstellen. Das Data Warehouse wird aus der letzten täglichen Sicherung wiederhergestellt. **Zeitachse der Wiederherstellung** Sie können eine Datenbank auf jeden Wiederherstellungspunkt innerhalb der letzten sieben Tage wiederherstellen. Momentaufnahmen werden alle vier bis acht Stunden gestartet und sind sieben Tage lang verfügbar. Wenn eine Momentaufnahme älter als sieben Tage ist, läuft sie ab, und ihr Wiederherstellungspunkt ist nicht mehr verfügbar. **Kosten für die Wiederherstellung** Die Speichergebühren für das wiederhergestellte Data Warehouse werden zum Azure Storage Premium-Tarif abgerechnet. Wenn Sie ein wiederhergestelltes Data Warehouse anhalten, wird Ihnen der Speicher zum Azure Storage Premium-Tarif in Rechnung gestellt. Der Vorteil des Anhaltens besteht darin, dass keine Gebühren für die DWU-Computeressourcen anfallen. |2016-09-29 |

## <a name="get-started"></a>Erste Schritte
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 4 |[Authentifizierung in Azure SQL Data Warehouse](sql-data-warehouse-authentication.md) |Azure Active Directory- (AAD) und SQL Server-Authentifizierung in Azure SQL Data Warehouse. |
| 5 |[Bewährte Methoden für Azure SQL Data Warehouse](sql-data-warehouse-best-practices.md) |Enthält Empfehlungen und bewährte Methoden, die Sie kennen sollten, wenn Sie Lösungen für Azure SQL Data Warehouse entwickeln. Sie tragen dazu bei, dass Sie erfolgreich sind. |
| 6 |[Treiber für Azure SQL Data Warehouse](sql-data-warehouse-connection-strings.md) |Verbindungszeichenfolgen und Treiber für SQL Data Warehouse |
| 7 |[Verbinden mit Azure SQL Data Warehouse](sql-data-warehouse-connect-overview.md) |Gewusst wie: Suchen des Servernamens und der Verbindungszeichenfolge für Azure SQL Data Warehouse |
| 8 |[Analysieren von Daten mit Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md) |Azure Machine Learning wird verwendet, um ein Predictive Machine Learning-Modell basierend auf in Azure SQL Data Warehouse gespeicherten Daten zu erstellen. |
| 9 |[Abfragen von Azure SQL Data Warehouse (sqlcmd)](sql-data-warehouse-get-started-connect-sqlcmd.md) |Es wird beschrieben, wie Sie Azure SQL Data Warehouse mit dem Befehlszeilenprogramm sqlcmd abfragen. |
| 10 |[Erstellen einer SQL Data Warehouse-Datenbank mithilfe von Transact-SQL (TSQL)](sql-data-warehouse-get-started-create-database-tsql.md) |Erfahren Sie, wie Sie ein Azure SQL Data Warehouse mit TSQL erstellen. |
| 11 |[Gewusst wie: Erstellen eines Supporttickets für SQL Data Warehouse](sql-data-warehouse-get-started-create-support-ticket.md) |Sie erfahren, wie Sie in Azure SQL Data Warehouse ein Supportticket erstellen. |
| 12 |[Laden von Daten mit Azure Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md) |Informationen zum Laden von Daten mit Azure Data Factory |
| 13 |[Laden von Daten mit PolyBase in SQL Data Warehouse](sql-data-warehouse-get-started-load-with-polybase.md) |Informationen zu PolyBase und Hinweise zur Verwendung in Data Warehouse-Szenarios. |
| 14 |[Erstellen einer Azure SQL Data Warehouse-Instanz](sql-data-warehouse-get-started-provision.md) |Erfahren Sie, wie Sie ein Azure SQL Data Warehouse im Azure-Portal erstellen. |
| 15 |[Erstellen von SQL Data Warehouse mithilfe von PowerShell](sql-data-warehouse-get-started-provision-powershell.md) |Erstellen von SQL Data Warehouse mithilfe von PowerShell |
| 16 |[Visualisieren von Daten mit Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) |Visualisieren von SQL Data Warehouse-Daten mit Power BI |
| 17 |[Abfragen von Azure SQL Data Warehouse (Visual Studio)](sql-data-warehouse-query-visual-studio.md) |Es wird beschrieben, wie Sie SQL Data Warehouse mit Visual Studio abfragen. |

## <a name="develop"></a>Entwickeln
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 18 |[Optimieren von Transaktionen für SQL Data Warehouse](sql-data-warehouse-develop-best-practices-transactions.md) |Bewährte Methoden zum Schreiben von effizienten Transaktionsupdates in Azure SQL Data Warehouse |
| 19 |[Parallelitäts- und Workloadverwaltung in SQL Data Warehouse](sql-data-warehouse-develop-concurrency.md) |Grundlagen der Parallelitäts- und Workloadverwaltung in Azure SQL Data Warehouse zum Entwickeln von Lösungen |
| 20 |[Create Table As Select (CTAS) in SQL Data Warehouse](sql-data-warehouse-develop-ctas.md) |Tipps für die Codierung mit der Anweisung „Create Table As Select“ (CTAS) in Azure SQL Data Warehouse zum Entwickeln von Lösungen |
| 21 |[Dynamischer SQL-Code in SQL Data Warehouse](sql-data-warehouse-develop-dynamic-sql.md) |Tipps zur Verwendung von dynamischem SQL-Code in Azure SQL Data Warehouse für die Entwickelung von Lösungen |
| 22 |[Group By-Optionen in SQL Data Warehouse](sql-data-warehouse-develop-group-by-options.md) |Tipps zum Implementieren von Group By-Optionen in Azure SQL Data Warehouse für die Entwicklung von Lösungen |
| 23 |[Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in SQL Data Warehouse](sql-data-warehouse-develop-label.md) |Tipps zum Verwenden von Bezeichnungen zum Instrumentieren von Abfragen in Azure SQL Data Warehouse für die Entwicklung von Lösungen |
| 24 |[Schleifen in SQL Data Warehouse](sql-data-warehouse-develop-loops.md) |Tipps zu Transact-SQL-Schleifen und zum Ersetzen von Cursorn in Azure SQL Data Warehouse für die Entwicklung von Lösungen |
| 25 |[Gespeicherte Prozeduren in SQL Data Warehouse](sql-data-warehouse-develop-stored-procedures.md) |Tipps zum Implementieren von gespeicherten Prozeduren in Azure SQL Data Warehouse für die Entwicklung von Lösungen |
| 26 |[Transaktionen in SQL Data Warehouse](sql-data-warehouse-develop-transactions.md) |Tipps zum Implementieren von Transaktionen in Azure SQL Data Warehouse zum Entwickeln von Lösungen |
| 27 |[Benutzerdefinierte Schemas in SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md) |Tipps für die Verwendung von Transact-SQL-Schemas in Azure SQL Data Warehouse zum Entwickeln von Lösungen. |
| 28 |[Zuweisen von Variablen in SQL Data Warehouse](sql-data-warehouse-develop-variable-assignment.md) |Tipps zum Zuweisen von Transact-SQL-Variablen in Azure SQL Data Warehouse für die Entwicklung von Lösungen. |
| 29 |[Sichten in SQL Data Warehouse](sql-data-warehouse-develop-views.md) |Tipps für die Verwendung von Transact-SQL-Sichten in Azure SQL Data Warehouse zum Entwickeln von Lösungen. |
| 30 |[Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse](sql-data-warehouse-overview-develop.md) |Entwicklungskonzepte, Entwurfsentscheidungen, Empfehlungen und Programmiertechniken für SQL Data Warehouse. |

## <a name="manage"></a>Verwalten
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 31 |[Verwalten von Computeleistung in Azure SQL Data Warehouse (Übersicht)](sql-data-warehouse-manage-compute-overview.md) |Funktionen zur horizontalen Leistungshochskalierung in Azure SQL Data Warehouse. Führen Sie eine horizontale Skalierung durch, indem Sie DWUs anpassen oder Computeressourcen anhalten und fortsetzen, um Kosten zu sparen. |
| 32 |[Verwalten von Computeleistung in Azure SQL Data Warehouse (Azure-Portal)](sql-data-warehouse-manage-compute-portal.md) |Azure-Portal-Aufgaben zum Verwalten von Computeleistung. Skalieren von Computeressourcen durch Anpassen der DWUs Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen. |
| 33 |[Verwalten von Computeleistung für Azure SQL Data Warehouse (PowerShell)](sql-data-warehouse-manage-compute-powershell.md) |PowerShell-Aufgaben zum Verwalten von Computeleistung. Skalieren von Computeressourcen durch Anpassen der DWUs Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen. |
| 34 |[Verwalten von Computeleistung in Azure SQL Data Warehouse (REST)](sql-data-warehouse-manage-compute-rest-api.md) |PowerShell-Aufgaben zum Verwalten von Computeleistung. Skalieren von Computeressourcen durch Anpassen der DWUs Sie können Computeressourcen auch nach Bedarf anhalten und fortsetzen, um Kosten zu sparen. |
| 35 |[Verwalten von Computeleistung in Azure SQL Data Warehouse (T-SQL)](sql-data-warehouse-manage-compute-tsql.md) |Transact-SQL-Aufgaben (T-SQL) zur horizontalen Hochskalierung durch Anpassen der DWUs Sparen Sie Kosten, indem Sie während Nicht-Spitzenzeiten herunterskalieren. |
| 36 |[Überwachen Ihrer Workload mit dynamischen Verwaltungssichten](sql-data-warehouse-manage-monitor.md) |Informationen zum Überwachen Ihrer Workload mit dynamischen Verwaltungssichten. |
| 37 |[Verwalten von Datenbanken in Azure SQL Data Warehouse](sql-data-warehouse-overview-manage.md) |Übersicht über die Verwaltung von SQL Data Warehouse-Datenbanken. Enthält Verwaltungstools, DWUs und horizontale Leistungsskalierung, Problembehandlung für die Abfrageleistung, sinnvolle Sicherheitsrichtlinien und das Wiederherstellen einer Datenbank nach Datenbeschädigung oder einem regionalen Ausfall. |
| 38 |[Überwachen von Benutzeranfragen in Azure SQL Data Warehouse](sql-data-warehouse-overview-manage-user-queries.md) |Übersicht über Überlegungen, bewährte Methoden und Aufgaben für die Überwachung von Benutzeranfragen in Azure SQL Data Warehouse. |
| 11,9 |[SQL Data Warehouse-Wiederherstellung](sql-data-warehouse-restore-database-overview.md) |Übersicht über die Wiederherstellungsoptionen zur Wiederherstellung einer Datenbank in Azure SQL Data Warehouse. |
| 40 |[Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Portal)](sql-data-warehouse-restore-database-portal.md) |Azure-Portal-Aufgaben zum Wiederherstellen einer Azure SQL Data Warehouse-Instanz. |
| 41 |[Wiederherstellen einer Azure SQL Data Warehouse-Instanz (PowerShell)](sql-data-warehouse-restore-database-powershell.md) |PowerShell-Aufgaben zum Wiederherstellen einer Azure SQL Data Warehouse-Instanz. |
| 42 |[Wiederherstellen einer Azure SQL Data Warehouse-Instanz (REST-API)](sql-data-warehouse-restore-database-rest-api.md) |REST-API-Aufgaben zum Wiederherstellen einer Azure SQL Data Warehouse-Instanz. |

## <a name="tables-and-indexes"></a>Tabellen und Indizes
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 43 |[Datentypen für Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-data-types.md) |Es werden die ersten Schritte für die Verwendung von Datentypen für Azure SQL Data Warehouse-Tabellen beschrieben. |
| 44 |[Verteilen von Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-distribute.md) |Enthält Informationen zu den ersten Schritten bei der Verteilung von Tabellen in Azure SQL Data Warehouse. |
| 45 |[Indizieren von Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-index.md) |Enthält Informationen zu den ersten Schritten bei der Tabellenindizierung in Azure SQL Data Warehouse. |
| 46 |[Übersicht über Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-overview.md) |Enthält Informationen zu den ersten Schritten mit Azure SQL Data Warehouse-Tabellen. |
| 47 |[Partitionieren von Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-partition.md) |Enthält Informationen zu den ersten Schritten bei der Tabellenpartitionierung in Azure SQL Data Warehouse. |
| 48 |[Verwalten von Statistiken für Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-statistics.md) |Enthält Informationen zu den ersten Schritten zu Statistiken von Tabellen in Azure SQL Data Warehouse. |
| 49 |[Temporäre Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-temporary.md) |Enthält Informationen zu den ersten Schritten mit temporären Tabellen in Azure SQL Data Warehouse. |

## <a name="integrate"></a>Integrieren
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 50 |[Verwenden von Azure Data Factory mit SQL Data Warehouse](sql-data-warehouse-integrate-azure-data-factory.md) |Tipps für die Verwendung von Azure Data Factory (ADF) mit Azure SQL Data Warehouse für die Entwicklung von Lösungen. |
| 51 |[Verwenden von Azure Machine Learning mit SQL Data Warehouse](sql-data-warehouse-integrate-azure-machine-learning.md) |Tipps für die Verwendung von Azure Machine Learning mit Azure SQL Data Warehouse für die Entwicklung von Lösungen. |
| 52 |[Verwenden von Azure Stream Analytics mit SQL Data Warehouse](sql-data-warehouse-integrate-azure-stream-analytics.md) |Tipps für die Verwendung von Azure Stream Analytics mit Azure SQL Data Warehouse für die Entwicklung von Lösungen. |
| 53 |[Verwenden von Power BI mit SQL Data Warehouse](sql-data-warehouse-integrate-power-bi.md) |Tipps zur Verwendung von Power BI in Azure SQL Data Warehouse zum Entwickeln von Lösungen. |
| 54 |[Nutzen anderer Dienste mit SQL Data Warehouse](sql-data-warehouse-overview-integrate.md) |Tools und Partner mit Lösungen, die in SQL Data Warehouse integriert werden können. |

## <a name="load"></a>Laden
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 55 |[Laden von Daten aus dem Azure-Blobspeicher in Azure SQL Data Warehouse (Azure Data Factory)](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md) |Informationen zum Laden von Daten mit Azure Data Factory |
| 56 |[Laden von Daten aus Azure Blob Storage in SQL Data Warehouse (PolyBase)](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md) |Erfahren Sie, wie Sie PolyBase verwenden, um Daten aus Azure Blob Storage in SQL Data Warehouse zu laden. Laden Sie einige Tabellen aus öffentlichen Daten in das Contoso Retail-Data Warehouse-Schema. |
| 57 |[Laden von Daten aus SQL Server in Azure SQL Data Warehouse (AZCopy)](sql-data-warehouse-load-from-sql-server-with-azcopy.md) |Verwendet BCP zum Exportieren von Daten aus SQL Server in Flatfiles, AZCopy zum Importieren von Daten in Azure-Blobspeicher und PolyBase zum Erfassen der Daten in Azure SQL Data Warehouse. |
| 58 |[Laden von Daten aus SQL Server in Azure SQL Data Warehouse (Flatfiles)](sql-data-warehouse-load-from-sql-server-with-bcp.md) |Kleinere Datenmengen können mithilfe von bcp aus SQL Server in Flatfiles exportiert und direkt in Azure SQL Data Warehouse importiert werden. |
| 59 |[Laden von Daten aus SQL Server in Azure SQL Data Warehouse (SSIS)](sql-data-warehouse-load-from-sql-server-with-integration-services.md) |Hier erfahren Sie, wie Sie ein SQL Server Integration Services (SSIS)-Paket erstellen, um Daten aus verschiedensten Datenquellen in SQL Data Warehouse zu verschieben. |
| 60 |[Laden von Daten mit PolyBase in SQL Data Warehouse](sql-data-warehouse-load-from-sql-server-with-polybase.md) |Verwendet BCP zum Exportieren von Daten aus SQL Server in Flatfiles, AZCopy zum Importieren von Daten in Azure-Blobspeicher und PolyBase zum Erfassen der Daten in Azure SQL Data Warehouse. |
| 61 |[Anleitung für die Verwendung von PolyBase in SQL Data Warehouse](sql-data-warehouse-load-polybase-guide.md) |Richtlinien und Empfehlungen zur Verwendung von PolyBase in SQL Data Warehouse-Szenarios. |
| 62 |[Laden von Beispieldaten in SQL Data Warehouse](sql-data-warehouse-load-sample-databases.md) |Laden von Beispieldaten in SQL Data Warehouse |
| 63 |[Laden von Daten mit bcp](sql-data-warehouse-load-with-bcp.md) |Erfahren Sie, was bcp ist und wie es in Data Warehouse-Szenarios verwendet wird. |
| 64 |[Laden von Daten in Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md) |Erfahren Sie mehr über die üblichen Szenarien für das Laden von Daten in SQL Data Warehouse. Dazu gehören die Verwendung von PolyBase, Azure Blob Storage, Flatfiles und Datenträgerversand. Sie können auch Drittanbietertools verwenden. |

## <a name="migrate"></a>Migrieren
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 65 |[Migrieren von SQL-Code nach SQL Data Warehouse](sql-data-warehouse-migrate-code.md) |Tipps für die Migration des SQL-Codes in Azure SQL Data Warehouse zum Entwickeln von Lösungen. |
| 66 |[Migrieren von Daten](sql-data-warehouse-migrate-data.md) |Tipps für die Migration von Daten in Azure SQL Data Warehouse zum Entwickeln von Lösungen. |
| 67 |[Data Warehouse-Dienstprogramm für die Migration (Vorschau)](sql-data-warehouse-migrate-migration-utility.md) |Migrieren nach SQL Data Warehouse. |
| 68 |[Migrieren Ihres Schemas nach SQL Data Warehouse](sql-data-warehouse-migrate-schema.md) |Tipps für die Migration eines Schemas in Azure SQL Data Warehouse zum Entwickeln von Lösungen. |
| 69 |[Migrieren Ihrer Lösung nach SQL Data Warehouse](sql-data-warehouse-overview-migrate.md) |Migrationshinweise zum Übertragen Ihrer Lösung auf die Plattform Azure SQL Data Warehouse. |

## <a name="partners"></a>Partner
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 70 |[Business Intelligence-Partner für SQL Data Warehouse](sql-data-warehouse-partner-business-intelligence.md) |Liste der externen Business Intelligence-Partner mit Lösungen, die SQL Data Warehouse unterstützen. |
| 71 |[Partner für die Datenintegration bei SQL Data Warehouse](sql-data-warehouse-partner-data-integration.md) |Liste der externen Partner mit Lösungen für die Datenintegration, die Azure SQL Data Warehouse unterstützen. |
| 72 |[Partner für die Datenverwaltung bei SQL Data Warehouse](sql-data-warehouse-partner-data-management.md) |Liste der externen Partner für die Datenverwaltung mit Lösungen, die SQL Data Warehouse unterstützen. |

## <a name="reference"></a>Referenz
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 73 |[Referenzthemen für SQL Data Warehouse](sql-data-warehouse-overview-reference.md) |Links zu Referenzinhalten für SQL Data Warehouse. |
| 74 |[PowerShell-Cmdlets und REST-APIs für SQL Data Warehouse](sql-data-warehouse-reference-powershell-cmdlets.md) |Hier finden Sie die wichtigsten PowerShell-Cmdlets für Azure SQL Data Warehouse, einschließlich der Cmdlets zum Anhalten und Fortsetzen einer Datenbank. |
| 75 |[Sprachelemente](sql-data-warehouse-reference-tsql-language-elements.md) |Eine Liste mit Links zu Referenzinhalten für die Transact-SQL-Sprachelemente für SQL Data Warehouse. |
| 76 |[Transact-SQL-Themen](sql-data-warehouse-reference-tsql-statements.md) |Links zu Referenzinhalten für Transact-SQL-Themen für SQL Data Warehouse. |
| 77 |[Systemsichten](sql-data-warehouse-reference-tsql-system-views.md) |Links zum Inhalt von Systemsichten für SQL Data Warehouse |

## <a name="security"></a>Sicherheit
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 78 |[SQL Data Warehouse – Unterstützung von kompatiblen Clients für Überwachung und dynamische Datenmaskierung](sql-data-warehouse-auditing-downlevel-clients.md) |Sie erhalten Informationen zur Unterstützung von Vorgängerversionsclients in SQL Data Warehouse für die Datenüberwachung. |
| 79 |[Überwachung in Azure SQL Data Warehouse](sql-data-warehouse-auditing-overview.md) |Erste Schritte mit der Überwachung in Azure SQL Data Warehouse |
| 80 |[Erste Schritte mit Transparent Data Encryption (TDE) in SQL Data Warehouse](sql-data-warehouse-encryption-tde.md) |Transparent Data Encryption (TDE) in SQL Data Warehouse |
| 81 |[Erste Schritte mit Transparent Data Encryption (TDE)](sql-data-warehouse-encryption-tde-tsql.md) |Transparent Data Encryption (TDE) in SQL Data Warehouse (T-SQL) |
| 82 |[Sichern einer Datenbank in SQL Data Warehouse](sql-data-warehouse-overview-manage-security.md) |Tipps für das Sichern einer Datenbank in Azure SQL Data Warehouse für die Entwicklung von Lösungen. |

## <a name="miscellaneous"></a>Verschiedenes
| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 83 |[Installieren von Visual Studio und SSDT für SQL Data Warehouse](sql-data-warehouse-install-visual-studio.md) |Installieren von Visual Studio und SQL Server Data Tools (SSDT) für Azure SQL Data Warehouse |
| 84 |[Details zur Migration zu Storage Premium](sql-data-warehouse-migrate-to-premium-storage.md) |Anweisungen zum Migrieren eines vorhandenen SQL Data Warehouse in Storage Premium |
| 85 |[Erste Schritte mit der Bedrohungserkennung](sql-data-warehouse-security-threat-detection.md) |Erste Schritte mit der Bedrohungserkennung |
| 86 |[Kapazitätsgrenzen von SQL Data Warehouse](sql-data-warehouse-service-capacity-limits.md) |Maximale Werte für Verbindungen, Datenbanken, Tabellen und Abfragen für SQL Data Warehouse. |
| 87 |[Problembehandlung bei Azure SQL Data Warehouse](sql-data-warehouse-troubleshoot.md) |Problembehandlung bei Azure SQL Data Warehouse. |


