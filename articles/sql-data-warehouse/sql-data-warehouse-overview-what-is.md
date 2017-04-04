---
title: Was ist Azure SQL Data Warehouse? | Microsoft Docs
description: "Verteilte Datenbank der Enterprise-Klasse, die mehrere Petabyte an relationalen und nicht relationalen Daten verarbeiten kann Sie ist das branchenweit erste Cloud-Data Warehouse mit Funktionen zum Vergrößern, Verkleinern und Anhalten innerhalb weniger Sekunden."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 2/28/2017
ms.author: jrj;mausher;kevin;barbkess;elbutter
translationtype: Human Translation
ms.sourcegitcommit: bf73ad830226626ddf41cc4ae80e714abf8bcfc2
ms.openlocfilehash: 19e87c61493bd4620120b39a533e9e4b64517538
ms.lasthandoff: 03/01/2017


---
# <a name="what-is-azure-sql-data-warehouse"></a>Was ist Azure SQL Data Warehouse?
Azure SQL Data Warehouse ist eine cloudbasierte, horizontal hochskalierbare, relationale MPP-Datenbank (Massively Parallel Processing), mit der sehr große Datenvolumen verarbeitet werden können. 

Für SQL Data Warehouse gilt Folgendes:

* Die relationale SQL Server-Datenbank ist darin mit Azure-Cloudfunktionen für das horizontale Hochskalieren kombiniert. 
* Entkoppelt Speicher und Compute.
* Ermöglicht das Erhöhen, Verringern, Anhalten und Fortsetzen von Computevorgängen. 
* Ist in die gesamte Azure-Plattform integriert.
* Verwendet SQL Server Transact-SQL (T-SQL) und Tools.
* Erfüllt verschiedene rechtliche Vorgaben und geschäftliche Sicherheitsanforderungen wie SOC und ISO.

In diesem Artikel werden die wichtigen Funktionen von SQL Data Warehouse beschrieben.

## <a name="massively-parallel-processing-architecture"></a>Massively Parallel Processing-Architektur
SQL Data Warehouse ist ein verteiltes Datenbanksystem mit paralleler Massenverarbeitung (Massively Parallel Processing, MPP). Im Hintergrund verteilt SQL Data Warehouse Ihre Daten auf viele Shared-Nothing-Speicher - und -Verarbeitungseinheiten. Die Daten werden auf einer lokal redundanten Premium-Speicherebene gespeichert, der dynamisch verknüpfte Computeknoten übergeordnet sind, die Abfragen ausführen. SQL Data Warehouse verfolgt bei der Ausführung von Lasten und komplexen T-SQL-Abfragen das Konzept „Teile und herrsche“. Anforderungen werden von einem Steuerknoten empfangen, für die Verteilung optimiert und dann an Computeknoten übergeben, die die Schritte parallel ausführen.

Die Entkoppelung von Speicher und Compute ermöglicht SQL Data Warehouse Folgendes:

* Erhöhen oder Verringern der Speichergröße unabhängig vom Computevorgang
* Erhöhen oder Verringern der Computeleistung ohne Datenverschiebung
* Anhalten der Computekapazität ohne Beeinträchtigung der Daten (und nur Bezahlung für den Speicher)
* Fortsetzen der Computekapazität während der Betriebszeiten

Im folgenden Diagramm ist die Architektur ausführlicher dargestellt:

![SQL Data Warehouse-Architektur][1]

**Steuerknoten:** Der Steuerknoten verwaltet und optimiert Abfragen. Dies ist das Front-End, das mit allen Anwendungen und Verbindungen interagiert. In SQL Data Warehouse basiert der Steuerknoten auf SQL-Datenbank, und die Verbindungsherstellung läuft genauso ab. Im Hintergrund koordiniert der Steuerknoten alle Datenverschiebungen und -berechnungen, die zum Ausführen von parallelen Abfragen für Ihre verteilten Daten erforderlich sind. Wenn Sie eine T-SQL-Abfrage an SQL Data Warehouse übermitteln, wird sie vom Steuerknoten in separate Abfragen aufgeteilt, die auf allen Computeknoten parallel ausgeführt werden.

**Computeknoten:** Die Computeknoten fungieren quasi als Antriebsaggregate von SQL Data Warehouse. Es handelt sich dabei um SQL-Datenbanken, die Ihre Daten speichern und Ihre Abfrage verarbeiten. Wenn Sie Daten hinzufügen, werden die Zeilen von SQL Data Warehouse an Ihre Computeknoten verteilt. Die Computeknoten führen die parallelen Abfragen für Ihre Daten durch. Nach der Verarbeitung übergeben sie die Ergebnisse zurück an den Steuerknoten. Um die Abfrage zu beenden, aggregiert der Steuerknoten die Ergebnisse und gibt das endgültige Ergebnis zurück.

**Speicher:** Ihre Daten werden in Azure-Blobspeicher gespeichert. Wenn Computeknoten mit Ihren Daten interagieren, werden Schreib- und Lesevorgänge direkt mit dem Blobspeicher durchgeführt. Da Azure-Speicher transparent und umfassend erweitert werden kann, ist dies auch für SQL Data Warehouse möglich. Da Compute- und Speichervorgänge unabhängig voneinander sind, kann in SQL Data Warehouse die Skalierung von Speicher automatisch separat von der Computeskalierung durchgeführt werden (und umgekehrt). Azure-Blobspeicher ist außerdem vollständig fehlertolerant und führt zu einer Optimierung des Sicherungs- und Wiederherstellungsprozesses.

**Datenverschiebungsdienst:** Mit dem Datenverschiebungsdienst (Data Movement Service, DMS) werden Daten zwischen Knoten verschoben. Per DMS erhalten die Computeknoten Zugriff auf Daten, die sie für Verknüpfungen und Aggregationen benötigen. DMS ist kein Azure-Dienst. Es ist ein Windows-Dienst, der auf allen Knoten zusammen mit SQL-Datenbank ausgeführt wird. DMS ist ein Hintergrundprozess, mit dem nicht direkt interagiert werden kann. In Abfrageplänen sehen Sie jedoch, wann DMS-Vorgänge auftreten, da zur parallelen Ausführung der einzelnen Abfragen eine Datenverschiebung erforderlich ist.

## <a name="optimized-for-data-warehouse-workloads"></a>Für Data Warehouse-Workloads optimiert
Das MPP-Konzept wird durch mehrere Data Warehousing-spezifische Leistungsoptimierungen unterstützt:

* Einen verteilten Abfrageoptimierer und einen Satz mit komplexen Statistiken, die datenübergreifend gelten. Mit den Informationen zur Datengröße und -verteilung kann der Dienst Abfragen optimieren, indem die Kosten bestimmter Vorgänge für verteilte Abfragen bewertet werden.
* Erweiterte Algorithmen und Techniken, die in den Datenverschiebungsprozess integriert sind, um Daten bei Bedarf effizient zwischen Computingressourcen zu verschieben und so die Abfrage durchzuführen. Diese Datenverschiebungsvorgänge sind integriert, und alle Optimierungen des Datenverschiebungsdiensts werden automatisch durchgeführt.
* Gruppierte **Columnstore** -Indizes werden standardmäßig verwendet. Mithilfe des spaltenbasierten Speichers erreicht SQL Data Warehouse durchschnittlich eine fünfmal höhere Komprimierung als mit dem herkömmlichen zeilenorientierten Speicher und eine bis zu zehnmal höhere Abfrageleistung (oder mehr). Analytics-Abfragen, bei denen eine große Zahl von Zeilen überprüft werden muss, funktionieren besser mit Columnstore-Indizes.


## <a name="predictable-and-scalable-performance-with-data-warehouse-units"></a>Planbare und skalierbare Leistung mit Data Warehouse-Einheiten
Da SQL Data Warehouse auf ähnlichen Technologien basiert wie SQL-Datenbank, können Benutzer bei analytischen Abfragen eine konsistente, planbare Leistung erwarten. Beim Hinzufügen oder Entfernen von Computeknoten ist mit einer linearen Skalierung der Leistung zu rechnen. Die Zuordnung von Ressourcen zur SQL Data Warehouse-Instanz wird in Data Warehouse-Einheiten (Data Warehouse Units, DWUs) gemessen. DWUs sind ein Maß für zugrunde liegende Ressourcen wie CPU, Arbeitsspeicher und IOPS, die der SQL Data Warehouse-Instanz zugeordnet werden. Wenn Sie die Anzahl von DWUs erhöhen, erhöht sich auch die Anzahl von Ressourcen, und die Leistung wird gesteigert. Mit DWUs kann vor allem Folgendes sichergestellt werden:

* Sie können Ihr Data Warehouse skalieren, ohne sich um die zugrunde liegende Hardware oder Software kümmern zu müssen.
* Sie können die Leistungsverbesserung für eine DWU-Ebene prognostizieren, bevor Sie die Computekapazität Ihrer Data Warehouse-Instanz ändern.
* Die zugrunde liegende Hardware und Software Ihrer Instanz kann sich ändern oder verschoben werden, ohne dass sich dies auf Ihre Workloadleistung auswirkt.
* Microsoft kann die zugrunde liegende Architektur des Diensts optimieren, ohne die Leistung Ihrer Workload zu beeinträchtigen.
* Microsoft kann die Leistung in SQL Data Warehouse schnell verbessern und so erreichen, dass die Skalierbarkeit und eine gleichmäßige Auswirkung auf das System sichergestellt sind.

Data Warehouse-Einheiten stellen ein Maß für drei Metriken bereit, die eng mit der Data Warehousing-Workloadleistung zusammenhängen. Die folgenden zentralen Workloadmetriken werden linear mit den DWUs skaliert:

**Scan/Aggregation**: Eine Data Warehousing-Standardabfrage, mit der eine große Zahl von Zeilen überprüft und anschließend eine komplexe Aggregation durchgeführt wird. Dies ist ein Vorgang mit hohem E/A- und CPU-Aufwand.

**Laden**: Die Fähigkeit, Daten im Dienst zu erfassen. Ladevorgänge werden am besten mit PolyBase aus Azure Storage Blobs oder aus Azure Data Lake ausgeführt. Diese Metrik ist dafür konzipiert, die Netzwerk- und CPU-Aspekte des Diensts zu verdeutlichen.

**Create Table As Select (CTAS)** : Mit CTAS wird die Fähigkeit zum Kopieren einer Tabelle gemessen. Dies umfasst das Lesen von Daten aus dem Speicher, das Verteilen auf die Knoten der Anwendung und das erneute Schreiben in den Speicher. Dies ist ein Vorgang mit hohem CPU-, E/A- und Netzwerkaufwand.

## <a name="built-on-sql-server"></a>SQL Server als Grundlage
SQL Data Warehouse basiert auf dem relationalen Datenbankmodul von SQL Server und enthält viele Features, die Sie von einem Data Warehouse für Unternehmen erwarten. Wenn Sie T-SQL bereits kennen, ist es einfach, Ihr Wissen auf SQL Data Warehouse zu übertragen. Die Beispiele in der gesamten Dokumentation stellen für Fortgeschrittene und Anfänger gleichermaßen eine gute Starthilfe dar. Sie können sich den Aufbau der Sprachelemente von SQL Data Warehouse generell wie folgt vorstellen:

* Für SQL Data Warehouse wird die T-SQL-Syntax für viele Vorgänge verwendet. Außerdem werden viele herkömmliche SQL-Konstrukte unterstützt, z.B. gespeicherte Prozeduren, benutzerdefinierte Funktionen, Tabellenpartitionierung, Indizes und Sortierungen.
* SQL Data Warehouse enthält auch mehrere neuere SQL Server-Features – etwa gruppierte **Columnstore**-Indizes, PolyBase-Integration und Datenüberwachung (mit Bedrohungsbewertung).
* Bestimmte T-SQL-Sprachelemente, die für Data Warehousing-Workloads weniger üblich oder für SQL Server neu sind, sind unter Umständen derzeit nicht verfügbar. Weitere Informationen finden Sie in der [Dokumentation zur Migration][Migration documentation].

Mit Transact-SQL und der Funktionskompatibilität zwischen SQL Server, SQL Data Warehouse, SQL-Datenbank und Analytic Platform System können Sie eine Lösung entwickeln, die Ihren Bedürfnissen entspricht. Sie können entscheiden, wo Ihre Daten basierend auf der Leistung, der Sicherheit und den Skalierungsanforderungen gespeichert werden sollen, und dann Daten bei Bedarf zwischen verschiedenen Systemen übertragen.

## <a name="data-protection"></a>Datenschutz
SQL Data Warehouse speichert alle Daten mithilfe von lokal redundantem Speicher in Azure Premium. Mehrere synchrone Kopien der Daten werden im lokalen Rechenzentrum gepflegt, um transparenten Datenschutz bei lokalen Ausfällen zu gewährleisten. Außerdem sichert SQL Data Warehouse in regelmäßigen Abständen automatisch Ihre aktiven (nicht angehaltenen) Datenbanken mithilfe von Azure Storage-Momentaufnahmen. Weitere Informationen zum Sichern und Wiederherstellen finden Sie unter [Sichern und Wiederherstellen – Übersicht][Backup and restore overview].

## <a name="integrated-with-microsoft-tools"></a>Integration in Microsoft-Tools
In SQL Data Warehouse sind außerdem viele Tools integriert, mit denen SQL Server-Benutzer häufig vertraut sind. Zu diesen Tools zählen:

**Herkömmliche SQL Server-Tools** : SQL Data Warehouse ist vollständig in SQL Server Analysis Services, Integration Services und Reporting Services integriert.

**Cloudbasierte Tools**: SQL Data Warehouse kann in verschiedene Dienste in Azure integriert werden. Hierzu zählen beispielsweise Data Factory, Stream Analytics, Machine Learning und Power BI. Eine vollständige Liste finden Sie in der [Übersicht über die integrierten Tools][Integrated tools overview].

**Drittanbietertools**: Viele Anbieter von Drittanbietertools haben sich die Integration ihrer Tools in SQL Data Warehouse zertifizieren lassen. Eine vollständige Liste finden Sie unter [SQL Data Warehouse-Lösungspartner][SQL Data Warehouse solution partners].

## <a name="hybrid-data-sources-scenarios"></a>Szenarien für Hybriddatenquellen
PolyBase ermöglicht es Ihnen, Ihre Daten aus unterschiedlichen Quellen mithilfe von vertrauten T-SQL-Befehlen zu nutzen. PolyBase ermöglicht Ihnen das Abfragen nicht relationaler Daten im Azure-Blobspeicher wie in einer normalen Tabelle. Verwenden Sie PolyBase, um nicht relationale Daten abzufragen oder nicht relationale Daten in SQL Data Warehouse zu importieren.

* PolyBase verwendet externe Tabellen für den Zugriff auf nicht relationale Daten. Die Tabellendefinitionen werden im SQL Data Warehouse gespeichert, und Sie können wie bei normalen relationalen Daten auch per SQL und mit Tools darauf zugreifen.
* PolyBase verhält sich bei der Integration agnostisch. Für alle unterstützten Quellen werden die gleichen Features und Funktionen bereitgestellt. Die von PolyBase gelesenen Daten können in verschiedenen Formaten vorliegen – unter anderem als durch Trennzeichen getrennte Dateien oder als ORC-Dateien.
* PolyBase kann verwendet werden, um auf Blobspeicher zuzugreifen, der auch als Speicher für einen HDInsight-Cluster verwendet wird. Hierdurch erhalten Sie mit relationalen und nicht relationalen Tools Zugriff auf die gleichen Daten.

## <a name="sla"></a>SLA
SQL Data Warehouse bietet eine Vereinbarung zum Servicelevel (SLA) auf Produktebene als Teil des Microsoft Online Services-SLA. Weitere Informationen finden Sie unter [SLA für SQL Data Warehouse][SLA for SQL Data Warehouse]. Weitere Informationen zum SLA für alle anderen Produkte finden Sie auf der Azure-Seite [Service Level Agreements]. Sie können die SLAs auch auf der Seite [Volumenlizenzierung][Volume Licensing] herunterladen. 

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit den Grundlagen von SQL Data Warehouse vertraut gemacht haben, können Sie sich nun darüber informieren, wie Sie schnell [eine SQL Data Warehouse-Instanz erstellen][create a SQL Data Warehouse] und [Beispieldaten][load sample data] laden. Falls Sie mit Azure noch nicht vertraut sind und auf neue Terminologie stoßen, ist das [Azure-Glossar][Azure glossary] sehr nützlich. Weitere SQL Data Warehouse-Ressourcen:  

* [Kundenerfolgsgeschichten]
* [Blogs]
* [Funktionsanfragen]
* [Videos]
* [Customer Advisory Team-Blogs]
* [Erstellen eines Supporttickets]
* [MSDN-Forum]
* [Stack Overflow-Forum]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Erstellen eines Supporttickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Kundenerfolgsgeschichten]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Customer Advisory Team-Blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funktionsanfragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-Forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow-Forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/en-us/support/legal/sla/

