<properties
   pageTitle="Was ist Azure SQL Data Warehouse? | Microsoft Azure"
   description="Verteilte Datenbank der Enterprise-Klasse, die mehrere Petabyte an relationalen und nicht relationalen Daten verarbeiten kann Sie ist das branchenweit erste Cloud-Data Warehouse mit Funktionen zum Vergrößern, Verkleinern und Anhalten innerhalb weniger Sekunden."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/27/2016"
   ms.author="lodipalm;barbkess;mausher;jrj;sonyama;kevin"/>


# Was ist Azure SQL Data Warehouse?

Azure SQL Data Warehouse ist eine cloudbasierte Datenbank für das horizontale Hochskalieren, mit der sehr große Datenvolumen verarbeitet werden können, und zwar sowohl relational als auch nicht relational. SQL Data Warehouse basiert auf unserer MPP-Architektur (Massively Parallel Processing) und ist für die Verarbeitung Ihrer Enterprise-Workloads geeignet.

Für SQL Data Warehouse gilt Folgendes:

- Die relationale SQL Server-Datenbank ist darin mit Azure-Cloudfunktionen für das horizontale Hochskalieren kombiniert. Sie können Computevorgänge innerhalb von Sekunden erhöhen, verringern, anhalten oder fortsetzen. Sie sparen Kosten, indem Sie die CPU horizontal hochskalieren, wenn es erforderlich ist, und die Nutzung außerhalb der Spitzenzeiten herunterfahren.
- Die Azure-Plattform wird genutzt. Die Anwendung kann einfach bereitgestellt und nahtlos gewartet werden und verfügt dank der automatischen Sicherungen über eine umfassende Fehlertoleranz.
- Stellt eine Ergänzung des SQL Server-Ökosystems dar. Sie können vertrauten SQL Server Transact-SQL-Code (T-SQL) und die dazugehörigen Tools für die Entwicklung nutzen.

In diesem Artikel werden die wichtigen Funktionen von SQL Data Warehouse beschrieben.

## Massively Parallel Processing-Architektur

SQL Data Warehouse ist ein verteiltes Datenbanksystem mit paralleler Massenverarbeitung (Massively Parallel Processing, MPP). Indem die Daten- und Verarbeitungsfunktionen über mehrere Knoten verteilt werden, ermöglicht SQL Data Warehouse eine deutlich umfassendere Skalierbarkeit als ein Einzelsystem. Im Hintergrund verteilt SQL Data Warehouse Ihre Daten auf viele Shared-Nothing-Speicher - und -Verarbeitungseinheiten. Die Daten werden in lokal redundantem Premium-Speicher gespeichert und zur Abfrageausführung mit Computeknoten verknüpft. Mit dieser Architektur folgt SQL Data Warehouse im Zusammenhang mit der Ausführung von Lasten und komplexen T-SQL-Abfragen dem Konzept „Teile und herrsche“. Anforderungen werden vom Steuerknoten empfangen, optimiert und dann an die Computeknoten übergeben, damit die Schritte parallel ausgeführt werden können.

Aufgrund der Kombination aus MPP-Architektur und Azure-Speicherfunktionen ist mit SQL Data Warehouse Folgendes möglich:

- Erhöhen oder Verringern des Speichers unabhängig vom Computevorgang
- Vergrößern oder Verkleinern des Computevorgangs ohne Datenverschiebung
- Anhalten der Computekapazität unter Beibehaltung der Intaktheit von Daten
- Schnelles erneutes Bereitstellen der Computekapazität

Im folgenden Diagramm ist die Architektur ausführlicher dargestellt:

![SQL Data Warehouse-Architektur][1]


**Steuerknoten:** Der Steuerknoten verwaltet und optimiert Abfragen. Dies ist das Front-End, das mit allen Anwendungen und Verbindungen interagiert. In SQL Data Warehouse basiert der Steuerknoten auf SQL-Datenbank, und die Verbindungsherstellung läuft genauso ab. Im Hintergrund koordiniert der Steuerknoten alle Datenverschiebungen und -berechnungen, die zum Ausführen von parallelen Abfragen für Ihre verteilten Daten erforderlich sind. Wenn Sie eine T-SQL-Abfrage an SQL Data Warehouse übermitteln, wird sie vom Steuerknoten in separate Abfragen aufgeteilt, die auf allen Computeknoten parallel ausgeführt werden.

**Computeknoten:** Die Computeknoten fungieren quasi als Antriebsaggregate von SQL Data Warehouse. Es handelt sich dabei um SQL-Datenbanken, die Ihre Daten speichern und Ihre Abfrage verarbeiten. Wenn Sie Daten hinzufügen, werden die Zeilen von SQL Data Warehouse an Ihre Computeknoten verteilt. Die Computeknoten führen die parallelen Abfragen für Ihre Daten durch. Nach der Verarbeitung übergeben sie die Ergebnisse zurück an den Steuerknoten. Um die Abfrage zu beenden, aggregiert der Steuerknoten die Ergebnisse und gibt das endgültige Ergebnis zurück.

**Speicher:** Ihre Daten werden in Azure-Blobspeicher gespeichert. Wenn Computeknoten mit Ihren Daten interagieren, werden Schreib- und Lesevorgänge direkt mit dem Blobspeicher durchgeführt. Da Azure-Speicher transparent und umfassend erweitert werden kann, ist dies auch für SQL Data Warehouse möglich. Da Compute- und Speichervorgänge unabhängig voneinander sind, kann in SQL Data Warehouse die Skalierung von Speicher automatisch separat von der Computeskalierung durchgeführt werden (und umgekehrt). Azure-Blobspeicher ist außerdem vollständig fehlertolerant und führt zu einer Optimierung des Sicherungs- und Wiederherstellungsprozesses.

**Datenverschiebungsdienst:** Mit dem Datenverschiebungsdienst (Data Movement Service, DMS) werden Daten zwischen Knoten verschoben. Per DMS erhalten die Computeknoten Zugriff auf Daten, die sie für Verknüpfungen und Aggregationen benötigen. DMS ist kein Azure-Dienst. Es ist ein Windows-Dienst, der auf allen Knoten zusammen mit SQL-Datenbank ausgeführt wird. Da DMS im Hintergrund ausgeführt wird, erfolgt die Interaktion nicht auf direktem Wege. Wenn Sie sich Abfragepläne ansehen, werden Sie merken, dass diese einige DMS-Vorgänge enthalten. Eine Datenverschiebung ist erforderlich, um jede Abfrage parallel ausführen zu können.


## Für Data Warehouse-Workloads optimiert

Das MPP-Konzept wird durch eine Reihe von speziellen Data Warehousing-Leistungsoptimierungen unterstützt:

- Einen verteilten Abfrageoptimierer und einen Satz mit komplexen Statistiken, die datenübergreifend gelten. Mit den Informationen zur Datengröße und -verteilung kann der Dienst Abfragen optimieren, indem die Kosten bestimmter Vorgänge für verteilte Abfragen bewertet werden.

- Erweiterte Algorithmen und Techniken, die in den Datenverschiebungsprozess integriert sind, um Daten bei Bedarf effizient zwischen Computingressourcen zu verschieben und so die Abfrage durchzuführen. Diese Datenverschiebungsvorgänge sind integriert, und alle Optimierungen des Datenverschiebungsdiensts werden automatisch durchgeführt.

- Gruppierte **Columnstore**-Indizes werden standardmäßig verwendet. Mithilfe des spaltenbasierten Speichers erreicht SQL Data Warehouse durchschnittlich eine fünfmal höhere Komprimierung als mit dem herkömmlichen zeilenorientierten Speicher und eine bis zu zehnmal höhere Abfrageleistung (oder mehr). Analytics-Abfragen, bei denen eine große Zahl von Zeilen durchsucht werden muss, funktionieren hervorragend mit Columnstore-Indizes.


## Vorhersagbare und skalierbare Leistung

Bei SQL Data Warehouse werden Speicher- und Computeressourcen voneinander getrennt, sodass diese unabhängig voneinander skaliert werden können. SQL Data Warehouse kann schnell und einfach skaliert werden, damit zusätzliche Computeressourcen sofort hinzugefügt werden können. Dies wird durch die Verwendung von Azure-Blobspeicher vervollständigt. Mit Blobs erhalten wir nicht nur einen stabilen, replizierten Speicher, sondern auch die Infrastruktur für eine mühelose Erweiterung zu niedrigen Kosten. Die Kombination aus der Speicherung auf Cloudebene und den Azure-Computeressourcen ermöglicht für SQL Data Warehouse die bedarfsabhängige Bezahlung von Abfrageleistung und Speicher. Das Ändern der Computemenge ist sehr einfach, da im Azure-Portal nur ein Schieberegler nach links oder rechts verschoben werden muss. Dieser Vorgang kann auch per T-SQL und PowerShell geplant werden.

Neben der Möglichkeit zur vollständigen Kontrolle über den Umfang der Computeleistung unabhängig vom Speicher ermöglicht Ihnen SQL Data Warehouse auch das vollständige Anhalten des Data Warehouse. Dies bedeutet, dass Sie nicht für Computeressourcen bezahlen, wenn Sie sie nicht benötigen. Während Ihr Speicher unverändert bleibt, werden alle Computeressourcen für den Hauptpool von Azure freigegeben, sodass Sie Kosten sparen. Setzen Sie die Computeressource bei Bedarf einfach fort, und halten Sie Ihre Daten und Computeleistung für Ihre Arbeitslast zur Verfügung.

## Data Warehouse-Einheiten

Die Zuordnung von Ressourcen zur SQL Data Warehouse-Instanz wird in Data Warehouse-Einheiten (Data Warehouse Units, DWUs) gemessen. DWUs sind ein Maß für zugrunde liegende Ressourcen wie CPU, Arbeitsspeicher und IOPS, die der SQL Data Warehouse-Instanz zugeordnet werden. Wenn Sie die Anzahl von DWUs erhöhen, erhöht sich auch die Anzahl von Ressourcen, und die Leistung wird gesteigert. Mit DWUs kann vor allem Folgendes sichergestellt werden:

- Sie können Ihr Data Warehouse problemlos skalieren, ohne sich um die zugrunde liegende Hardware oder Software kümmern zu müssen.

- Sie können die Leistungsverbesserung für eine DWU-Ebene prognostizieren, bevor Sie die Größe Ihrer Data Warehouse-Instanz ändern.

- Die zugrunde liegende Hardware und Software Ihrer Instanz kann sich ändern oder verschoben werden, ohne dass sich dies auf Ihre Workloadleistung auswirkt.

- Microsoft kann Anpassungen an der zugrunde liegenden Architektur des Diensts vornehmen, ohne dass sich dies auf die Leistung Ihrer Workload auswirkt.

- Microsoft kann die Leistung in SQL Data Warehouse schnell verbessern und so erreichen, dass die Skalierbarkeit und eine gleichmäßige Auswirkung auf das System sichergestellt sind.

Mit Data Warehouse-Einheiten wird ein Maß für drei präzise Metriken bereitgestellt, für die eine starke Korrelation mit der Data Warehousing-Workloadleistung besteht. Das Ziel ist, dass die folgenden wichtigen Workloadmetriken linear mit den DWUs skaliert werden können, die Sie für Ihr Data Warehouse ausgewählt haben.

**Scan/Aggregation**: Bei dieser Workload-Metrik wird eine Data Warehousing-Standardabfrage verwendet, mit der eine große Zahl von Zeilen gescannt und anschließend eine komplexe Aggregation durchgeführt wird. Dies ist ein Vorgang mit hohem E/A- und CPU-Aufwand.

**Laden**: Bei dieser Metrik wird gemessen, wie gut Daten in den Dienst aufgenommen werden können. Ladevorgänge werden mit PolyBase durchgeführt, indem ein repräsentatives Dataset aus dem Azure-Blobspeicher geladen wird. Diese Metrik ist dafür konzipiert, die Netzwerk- und CPU-Aspekte des Diensts zu verdeutlichen.

**Create Table As Select (CTAS)**: Mit CTAS wird die Fähigkeit zum Kopieren einer Tabelle gemessen. Dies umfasst das Lesen von Daten aus dem Speicher, das Verteilen auf die Knoten der Appliance und das erneute Schreiben in den Speicher. Dies ist ein Vorgang mit hohem CPU-, E/A- und Netzwerkaufwand.

## Anhalten und Skalieren nach Bedarf

Falls Sie schnellere Ergebnisse benötigen, sollten Sie Ihre DWUs erhöhen und für mehr Leistung zahlen. Falls Sie weniger Computeleistung benötigen, sollten Sie Ihre DWUs verringern und nur für die verbrauchte Leistung bezahlen. Eine Anpassung der DWUs kann etwa in folgenden Fällen angebracht sein:

- Wenn Sie keine Abfragen ausführen müssen (etwa abends oder am Wochenende), können Sie Ihre Abfragen stilllegen. Halten Sie anschließend Ihre Computeressourcen an, um nicht für nicht benötigte DWUs bezahlen zu müssen.

- Bei geringem Bedarf empfiehlt sich unter Umständen eine DWU-Verringerung. Sie können weiterhin auf die Daten zugreifen, sparen dabei aber Kosten in erheblichem Umfang.

- Wenn Sie einen umfangreichen Vorgang zum Laden oder Transformieren von Daten durchführen, sollten Sie zentral hochskalieren, damit die Daten schneller verfügbar sind.

Damit Sie verstehen, wie Ihr idealer DWU-Wert lautet, sollten Sie versuchsweise hoch- und herunterskalieren und nach dem Laden der Daten einige Abfragen ausführen. Da die Skalierung schnell geht, können Sie es mit mehreren unterschiedlichen Leistungsebenen probieren, ohne dass dies länger als eine Stunde dauert. Beachten Sie, dass SQL Data Warehouse für die Verarbeitung von großen Datenmengen ausgelegt ist. Um die wahren Skalierungsmöglichkeiten zu nutzen, vor allem im Rahmen der von uns angebotenen größeren Skalierungen, sollten Sie ein großes Dataset von 1 TB oder darüber verwenden.


## SQL Server als Grundlage

SQL Data Warehouse basiert auf dem relationalen Datenbankmodul von SQL Server und enthält viele Features, die Sie von einem Data Warehouse für Unternehmen erwarten. Wenn Sie T-SQL bereits kennen, ist es einfach, Ihr Wissen auf SQL Data Warehouse zu übertragen. Die Beispiele in der gesamten Dokumentation stellen für Fortgeschrittene und Anfänger gleichermaßen eine gute Starthilfe dar. Sie können sich den Aufbau der Sprachelemente von SQL Data Warehouse generell wie folgt vorstellen:

- Für SQL Data Warehouse wird die T-SQL-Syntax für viele Vorgänge verwendet. Außerdem werden viele herkömmliche SQL-Konstrukte unterstützt, z.B. gespeicherte Prozeduren, benutzerdefinierte Funktionen, Tabellenpartitionierung, Indizes und Sortierungen.

- SQL Data Warehouse enthält auch eine Reihe von neueren SQL Server-Features, z.B. in Clustern gruppierte **Columnstore**-Indizes, PolyBase-Integration und Datenüberwachung (mit Bedrohungsbewertung).

- Bestimmte T-SQL-Sprachelemente, die für Data Warehousing-Workloads weniger üblich oder für SQL Server neu sind, sind unter Umständen derzeit nicht verfügbar. Weitere Informationen finden Sie in der [Dokumentation zur Migration][].

Mit Transact-SQL und der Funktionskompatibilität zwischen SQL Server, SQL Data Warehouse, SQL-Datenbank und Analytic Platform System können Sie eine Lösung entwickeln, die Ihren Bedürfnissen entspricht. Sie können entscheiden, wo Ihre Daten basierend auf der Leistung, der Sicherheit und den Skalierungsanforderungen gespeichert werden sollen, und dann Daten bei Bedarf zwischen verschiedenen Systemen übertragen.

## Datenschutz

SQL Data Warehouse speichert alle Daten mithilfe von lokal redundantem Speicher in Azure Premium. Mehrere synchrone Kopien der Daten werden im lokalen Rechenzentrum behalten, um transparenten Datenschutz bei lokalen Ausfällen sicherzustellen. Außerdem sichert SQL Data Warehouse in regelmäßigen Abständen automatisch Ihre aktiven (nicht angehaltenen) Datenbanken mithilfe von Azure Storage-Momentaufnahmen. Weitere Informationen zum Sichern und Wiederherstellen finden Sie unter [Sichern und Wiederherstellen – Übersicht][].

## Integration in Microsoft-Tools

In SQL Data Warehouse sind außerdem viele Tools integriert, mit denen SQL Server-Benutzer häufig vertraut sind. Diese umfassen:

**Herkömmliche SQL Server-Tools**: SQL Data Warehouse ist vollständig in SQL Server Analysis Services, Integration Services und Reporting Services integriert.

**Cloudbasierte Tools**: SQL Data Warehouse kann zusammen mit einer Reihe von neuen Tools in Azure verwendet werden, z.B. Data Factory, Stream Analytics, Machine Learning und Power BI. Eine vollständige Liste finden Sie unter [Integrated Tools Overview][] (Übersicht über die integrierten Tools).

**Drittanbietertools**: Viele Anbieter von Drittanbietertools haben sich die Integration ihrer Tools in SQL Data Warehouse zertifizieren lassen. Eine vollständige Liste finden Sie unter [SQL Data Warehouse-Lösungspartner][].

## Szenarien für Hybriddatenquellen

Durch die Nutzung von SQL Data Warehouse mit PolyBase können Benutzer Daten jetzt noch besser in ihrem Ökosystem verschieben und erweiterte Hybridszenarien mit nicht relationalen und lokalen Datenquellen einrichten.

PolyBase ermöglicht es Ihnen, Ihre Daten aus unterschiedlichen Quellen mithilfe von vertrauten T-SQL-Befehlen zu nutzen. PolyBase ermöglicht Ihnen das Abfragen nicht relationaler Daten im Azure-Blobspeicher wie in einer normalen Tabelle. Verwenden Sie PolyBase, um nicht relationale Daten abzufragen oder nicht relationale Daten in SQL Data Warehouse zu importieren.

- PolyBase verwendet externe Tabellen für den Zugriff auf nicht relationale Daten. Die Tabellendefinitionen werden im SQL Data Warehouse gespeichert, und Sie können wie bei normalen relationalen Daten auch per SQL und mit Tools darauf zugreifen.

- PolyBase verhält sich bei der Integration agnostisch. Für alle unterstützten Quellen werden die gleichen Features und Funktionen bereitgestellt. Die von PolyBase gelesenen Daten können in einer Vielzahl von Formaten vorliegen, einschließlich Dateien mit Trennzeichen oder ORC-Dateien.

- PolyBase kann verwendet werden, um auf Blobspeicher zuzugreifen, der auch als Speicher für einen HDInsight-Cluster verwendet wird. Hierdurch erhalten Sie mit relationalen und nicht relationalen Tools Zugriff auf die gleichen Daten.

## Nächste Schritte

Nachdem Sie sich mit den Grundlagen von SQL Data Warehouse vertraut gemacht haben, können Sie sich nun darüber informieren, wie Sie schnell [eine SQL Data Warehouse-Instanz erstellen][] und [Beispieldaten laden][]. Falls Sie mit Azure noch nicht vertraut sind und auf neue Terminologie stoßen, ist das [Azure-Glossar][] sehr nützlich. Oder sehen Sie sich die folgenden SQL Data Warehouse-Ressourcen an:

- [Kundenerfolgsgeschichten]
- [Blogs]
- [Funktionsanfragen]
- [Videos]
- [Customer Advisory Team-Blogs]
- [Erstellen eines Supporttickets]
- [MSDN-Forum]
- [Stack Overflow-Forum]
- [Twitter]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Erstellen eines Supporttickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Beispieldaten laden]: ./sql-data-warehouse-load-sample-databases.md
[eine SQL Data Warehouse-Instanz erstellen]: ./sql-data-warehouse-get-started-provision.md
[Dokumentation zur Migration]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse-Lösungspartner]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Sichern und Wiederherstellen – Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Azure-Glossar]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Kundenerfolgsgeschichten]: https://customers.microsoft.com/search?sq=&ff=story_products_services%26%3EAzure%2FAzure%2FAzure%20SQL%20Data%20Warehouse%26%26story_product_families%26%3EAzure%2FAzure%26%26story_product_categories%26%3EAzure&p=0
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Customer Advisory Team-Blogs]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funktionsanfragen]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-Forum]: https://social.msdn.microsoft.com/Forums/azure/de-DE/home?forum=AzureSQLDataWarehouse
[Stack Overflow-Forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse

<!---HONumber=AcomDC_0928_2016-->