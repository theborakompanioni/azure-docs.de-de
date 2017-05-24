---
title: In-Memory-Technologien von Azure SQL-Datenbank | Microsoft-Dokumentation
description: In-Memory-Technologien von Azure SQL-Datenbank verbessern die Leistung von Transaktions- und Analyseworkloads erheblich. Erfahren Sie, wie Sie von diesen Technologien profitieren.
services: sql-database
documentationCenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: 250ef341-90e5-492f-b075-b4750d237c05
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: jodebrui
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: a7273c50f2619c776268406aa14f6c00dcfbfbbe
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017

---

# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>Optimieren der Leistung mithilfe von In-Memory-Technologien in SQL-Datenbank

Mithilfe von In-Memory-Technologien in Azure SQL-Datenbank können Sie für verschiedene Workloads Leistungsverbesserungen erzielen: Transaktionen (Online Transactional Processing [OLTP]), Analysen (Online Analytical Processing [OLAP]) und gemischt (Hybrid Transaction/analytical Processing [HTAP]). Aufgrund der effizienteren Abfrage- und Transaktionsverarbeitung tragen In-Memory-Technologien auch zur Kostensenkung bei. Sie müssen in der Regel nicht zu einem höheren Datenbanktarif wechseln, um Leistungsvorteile zu erzielen. In einigen Fällen können Sie möglicherweise sogar zu einem niedrigen Tarif wechseln und dennoch in den Genuss von Leistungsverbesserungen durch In-Memory-Technologien kommen.

Hier sind zwei Beispiele dafür, wie In-Memory-OLTP geholfen hat, die Leistung deutlich zu verbessern:

- Mithilfe von In-Memory-OLTP[konnte Quorum Business Solutions seine Workload verdoppeln und seine DTUs (d.h. die Ressourcenbelegung) um 70 % verbessern](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).
- Das folgende Video veranschaulicht anhand einer Beispielworkload eine erhebliche Verbesserung der Ressourcennutzung: [In-Memory-OLTP in Azure SQL-Datenbank](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB). Weitere Informationen finden Sie im Blogbeitrag [In-Memory OLTP in Azure SQL Database Blog Post](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/) (In-Memory OLTP in Azure SQL-Datenbank).

In-Memory-Technologien sind in allen Datenbanken des Tarifs „Premium“, einschließlich Datenbanken im Premium-Tarif für elastische Pools, verfügbar.

Im folgende Video werden potenzielle Leistungsvorteile mit In-Memory-Technologien in Azure SQL-Datenbank erklärt. Denken Sie daran, dass der Leistungsvorteil, den Sie erzielen, stets von vielen Faktoren abhängig ist. Dazu gehören die Art der Workload und Daten, die Zugriffsmuster der Datenbank usw.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Azure SQL-Datenbank verfügt über die folgenden In-Memory-Technologien:

- *In-Memory-OLTP* erhöht den Durchsatz und verringert die Latenz für die Verarbeitung von Transaktionen. Szenarien, die von In-Memory-OLTP profitieren sind: hoher Durchsatz bei der Transaktionsverarbeitung z.B. Handel treiben, Spielen, Datenerfassung von Ereignissen oder IoT-Geräten, Zwischenspeichern, Laden von Daten, temporäre Tabellen und Szenarien mit Tabellenvariablen.
- *Gruppierte Columnstore-Indizes* reduzieren den Speicherplatzbedarf (bis um das Zehnfache) und verbessern die Leistung für Berichts- und Analyseabfragen. Verwenden Sie sie mit Faktentabellen in Ihren Data Marts, um mehr Daten in Ihrer Datenbank zu speichern und die Leistung zu verbessern. Sie können sie auch mit Verlaufsdaten in der Betriebsdatenbank verwenden, um bis zu zehnmal mehr Daten zu archivieren und abfragen zu können.
- *Nicht gruppierte Columnstore-Indizes* für HTAP helfen beim Gewinnen von Einblicken in Echtzeit in Ihr Geschäft, indem Sie die Betriebsdatenbank direkt abfragen, ohne einen aufwendigen ETL-Prozess (Extrahieren, Transformieren, Laden) ausführen zu müssen und darauf zu warten, dass das Data Warehouse aufgefüllt wird. Nicht gruppierte Columnstore-Indizes helfen beim sehr schnellen Ausführen von Analyseabfragen in der OLTP-Datenbank, während gleichzeitig die Auswirkung auf die Betriebsworkload gering gehalten wird.
- Sie können In-Memory-OLTP und Columnstore-Indizes auch kombinieren. Sie können über eine speicheroptimierte Tabelle mit einem Columnstore-Index verfügen. Dies ermöglicht eine sehr schnelle Transaktionsverarbeitung und Anwendung von Analyseabfragen auf dieselben Daten.

Sowohl Columnstore-Indizes als auch In-Memory-OLTP gehören seit 2012 bzw. 2014 zum Funktionsumfang von SQL Server. Azure SQL-Datenbank und SQL Server weisen dieselbe Implementierung von In-Memory-Technologien auf. In Zukunft werden neue Funktionen für diese Technologien zunächst in Azure SQL-Datenbank geboten, ehe sie in SQL Server verfügbar sind.

In diesem Thema werden Aspekte von In-Memory-OLTP und Columnstore-Indizes beschrieben, die spezifisch für Azure SQL-Datenbank sind. Außerdem sind Beispiele aufgeführt. Zuerst erfahren Sie etwas über die Auswirkung dieser Technologien auf Speicher und die Grenzwerte für die Datengröße. Anschließend erfahren Sie, wie Sie das Verschieben von Datenbanken, die diese Technologien nutzen, zwischen verschiedenen Tarifen verwalten. Zum Schluss sehen Sie zwei Beispiele, die die Verwendung von In-Memory-OLTP und Columnstore-Indizes in Azure SQL-Datenbank veranschaulichen.

Weitere Informationen finden Sie in den folgenden Ressourcen.

Detaillierte Informationen zu den Technologien:

- [In-Memory-OLTP: Übersicht und Verwendungsszenarien](https://msdn.microsoft.com/library/mt774593.aspx) (einschließlich Verweisen auf Fallstudien und Informationen für Einsteiger)
- [Dokumentation für In-Memory-OLTP](http://msdn.microsoft.com/library/dn133186.aspx)
- [Beschreibung von Columnstore-Indizes](https://msdn.microsoft.com/library/gg492088.aspx)
- HTAP (Hybrid Transactional/Analytical Processing), auch als [Real-time Operational Analytics](https://msdn.microsoft.com/library/dn817827.aspx) bekannt

Schnelleinstieg in In-Memory-OLTP: [Quick Start 1: In-Memory OLTP Technologies for Faster T-SQL Performance](http://msdn.microsoft.com/library/mt694156.aspx) (Schnellstart 1: In-Memory-OLTP-Technologien für höhere T-SQL-Leistung): Dieser Artikel unterstützt Sie ebenfalls bei den ersten Schritten.

Ausführliche Videos zu den Technologien:

- [In-Memory-OLTP in Azure SQL-Datenbank](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) mit einer Veranschaulichung von Leistungsvorteilen und einer Anleitung, die zeigt, wie Sie diese Ergebnisse selbst reproduzieren können
- [In-Memory-OLTP-Videos: Was es ist und wann/wie es verwendet wird (in englischer Sprache)](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [Columnstore-Index: In-Memory-Analysen (d. h. Columnstore-Index) Videos von der Ignite 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a>Speicher- und Datengröße

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Datengröße und Speicherkapazität für In-Memory-OLTP

In-Memory-OLTP enthält speicheroptimierte Tabellen, die zum Speichern von Benutzerdaten verwendet werden. Diese Tabellen sind müssen in den Arbeitsspeicher passen. Da Sie Arbeitsspeicher direkt im SQL-Datenbank-Dienst verwalten, arbeiten wir mit dem Konzept eines Kontingents für Benutzerdaten. Dieses Konzept wird als *In-Memory-OLTP-Speicher* bezeichnet.

Jeder unterstützte eigenständige Datenbanktarif und jeder Tarif für elastische Pools sieht eine bestimmte Menge von In-Memory-OLTP-Speicher vor. Zum Zeitpunkt der Verfassung erhalten Sie ein GB Speicher für alle 125 Datenbank-Transaktionseinheiten (DTUs) bzw. elastische Datenbank-Transaktionseinheiten (eDTUs).

Der Artikel zu den [Tarifen für SQL-Datenbank](sql-database-service-tiers.md) enthält die offizielle Liste zum In-Memory-OLTP-Speicher in allen unterstützten eigenständigen Datenbanktarifen und Tarifen für elastische Pools.

Die folgenden Angaben werden bis zu Ihrer In-Memory-OLTP-Speicherkapazitätsobergrenze angerechnet:

- Aktive Benutzerdatenzeilen in speicheroptimierten Tabellen und Tabellenvariablen. Beachten Sie, dass alte Zeilenversionen nicht bis zur Kapazitätsobergrenze angerechnet werden.
- Indizes von speicheroptimierten Tabellen.
- Betriebsmehraufwand von ALTER TABLE-Vorgängen.

Wenn Sie die Kapazitätsobergrenze erreichen, erhalten Sie einen Fehler des Typs „Kontingent aufgebraucht“, woraufhin Sie keine Daten mehr einfügen oder aktualisieren können. Eine Lösung des Problems besteht darin, Daten zu löschen oder zu einem höheren Datenbank- oder Pooltarif zu wechseln.

Weitere Informationen zur Überwachung der In-Memory-OLTP-Speicherverwendung und zum Konfigurieren von Benachrichtigungen, wenn die Obergrenze fast erreicht ist, finden Sie unter [Überwachen des In-Memory-OLTP-Speichers](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>Hinweis zu elastischen Pools

Bei elastischen Pools wird der In-Memory-OLTP-Speicher von allen Datenbanken im Pool gemeinsam genutzt. Aus diesem Grund kann die Verwendung in einer Datenbank andere Datenbanken möglicherweise beeinträchtigen. Zwei Lösungen dafür sind:

- Konfigurieren Sie eine maximale eDTU-Anzahl für Datenbanken, die niedriger als die eDTU-Anzahl für den ganzen Pool ist. Dies begrenzt die In-Memory-OLTP-Speichernutzung in jeder Datenbank im Pool auf die Größe, die der eDTU-Anzahl entspricht.
- Konfigurieren Sie eine minimale eDTU-Anzahl, die größer als 0 ist. Damit wird sichergestellt, dass jede Datenbank im Pool die Menge des In-Memory-OLTPs Speichers zur Verfügung hat, die der konfigurierten minimalen eDTU-Anzahl entspricht.

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Datengröße und Speicher für Columnstore-Indizes

Columnstore-Indizes brauchen nicht in den Arbeitsspeicher zu passen. Deshalb ist die einzige Kapazität bei der Indexgröße die maximale Gesamtgröße der Datenbank, die im Artikel zu den [Tarifen von SQL-Datenbank](sql-database-service-tiers.md) dokumentiert ist.

Wenn Sie gruppierte Columnstore-Indizes verwenden, wird eine Spaltenkomprimierung für den Basistabellenspeicher verwendet. Dadurch kann der Speicherbedarf Ihrer Benutzerdaten erheblich reduziert werden, d.h. Sie können mehr Daten in der Datenbank speichern. Und dies kann mit [kolumnarer Archivierungskomprimierung](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression) noch weiter erhöht werden. Der Grad der Komprimierung, die Sie erreichen können, hängt von der Art der Daten ab, jedoch ist eine zehnfache Komprimierung nicht ungewöhnlich.

Wenn Sie z.B. eine Datenbank mit einer maximalen Größe von 1 Terabyte (TB) haben und mithilfe von Columnstore-Indizes eine zehnfache Komprimierung erreichen, können Sie insgesamt 10 TB Benutzerdaten in der Datenbank speichern.

Bei Verwendung von nicht gruppierten Columnstore-Indizes wird die Basistabelle weiterhin im herkömmlichen Rowstore-Format gespeichert. Aus diesem Grund sind die Speichereinsparungen nicht so groß wie bei gruppierten Columnstore-Indizes. Wenn Sie jedoch mehrere herkömmliche nicht gruppierte Indizes durch einen einzelnen Columnstore-Index ersetzen, können Sie dennoch allgemeine Einsparungen beim Speicherbedarf für die Tabelle erzielen.

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a>Verschieben von Datenbanken mit In-Memory-Technologien zwischen Tarifen

Für einen Wechsel zu einem höheren Tarif für eine Datenbank, die In-Memory-Technologien verwendet, sind keine speziellen Aspekte zu beachten, da ein höherer Tarif stets mehr Funktionalität und Ressourcen bietet. Das Herabstufen des Tarifs kann Auswirkungen auf die Datenbank haben. Dies gilt insbesondere, wenn Sie von Premium zu Standard oder Basic wechseln oder wenn Sie mit einer Datenbank, die In-Memory-OLTP verwendet, zu einem niedrigeren Premium-Tarif wechseln. Die gleichen Aspekte gelten, wenn Sie den Tarif eines elastischen Pools herunterstufen oder mit Datenbanken mit In-Memory-Technologien zu einem elastischen Pool im Tarif Standard oder Basic wechseln.

### <a name="in-memory-oltp"></a>In-Memory-OLTP

*Herunterstufen auf Basic/Standard*: In-Memory-OLTP wird nicht von Datenbanken im Tarif „Standard“ oder „Basic“ unterstützt. Darüber hinaus ist es nicht möglich, mit einer Datenbank mit In-Memory OLTP-Objekten zum Tarif „Standard“ oder „Basic“ zu wechseln.

Entfernen Sie alle speicheroptimierten Tabellen und Tabellentypen sowie alle nativ kompilierten T-SQL Module, bevor Sie die Datenbank auf den Standard- oder Basic-Tarif herabstufen.

Es gibt einen programmgesteuerten Weg, um zu verstehen, ob eine vorhandene Datenbank In-Memory-OLTP unterstützt. Sie können die folgende Transact-SQL-Abfrage ausführen:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

Wenn die Abfrage **1** zurückgibt, wird In-Memory-OLTP in dieser Datenbank unterstützt.


*Herunterstufen auf einen niedrigeren Premium-Tarif*: Daten in speicheroptimierten Tabellen müssen in den In-Memory-OLTP-Speicher passen, der dem Tarif der Datenbank zugeordnet oder im elastischen Pool verfügbar ist. Wenn Sie versuchen, zu einem niedrigeren Tarif zu wechseln oder die Datenbank in einen Pool zu verschieben, dem nicht genügend In-Memory-OLTP-Speicher zur Verfügung steht, misslingt der Vorgang.

### <a name="columnstore-indexes"></a>ColumnStore-Indizes

*Herunterstufen auf Basic/Standard*: Columnstore-Indizes werden nicht von Datenbanken im Tarif „Standard“ oder „Basic“ unterstützt. Wenn Sie eine Datenbank auf Standard/Basic herabstufen, sind keine Columnstore-Indizes verfügbar. Wenn Sie einen gruppierten Columnstore-Index verwenden, bedeutet dies, dass die ganze Tabelle nicht mehr verfügbar ist.

Entfernen Sie alle gruppierten Columnstore-Indizes, bevor Sie die Datenbank auf Standard/Basic herabstufen.

*Herabstufen auf einen niedrigeren Premium-Tarif*: Dies erfolgt, solange die ganze Datenbank in die maximale Datenbankgröße des Zieltarifs oder des verfügbaren Speichers im elastischen Pool passt. Es gibt keine spezifische Auswirkungen aus den Columnstore-Indizes.


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-the-in-memory-oltp-sample"></a>1. Installieren des Beispiels für In-Memory OLTP

Sie können im [Azure-Portal](https://portal.azure.com/) die Beispieldatenbank „AdventureWorksLT“ mit wenigen Klicks erstellen. Anhand der Schritte in diesem Abschnitt wird erläutert, wie Sie anschließend Ihre AdventureWorksLT-Datenbank mit In-Memory-OLTP-Objekten erweitern können, und es werden Leistungsvorteile demonstriert.

Eine einfachere, aber visuell ansprechendere Demo der Leistung von In-Memory-OLTP finden Sie hier:

- Version: [in-memory-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Quellcode: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Installationsschritte

1. Erstellen Sie im [Azure-Portal](https://portal.azure.com/) auf einem Server eine Premium-Datenbank. Legen Sie die Beispieldatenbank „AdventureWorksLT“ als **Quelle** fest. Detaillierte Anweisungen finden Sie unter [Erstellen Ihrer ersten Azure SQL-Datenbank](sql-database-get-started-portal.md).

2. Stellen Sie eine Verbindung mit der Datenbank mit SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx)her.

3. Kopieren Sie das [In-Memory OLTP Transact-SQL-Skript](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) in die Zwischenablage. Das T-SQL-Skript erstellt die erforderlichen In-Memory-Objekte in der in Schritt 1 erstellten Beispieldatenbank „AdventureWorksLT“.

4. Fügen Sie das T-SQL-Skript in SSMS ein, und führen Sie es aus. Unerlässlich ist die `MEMORY_OPTIMIZED = ON`-Klausel in den CREATE TABLE-Anweisungen. Beispiel:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Fehler 40536


Wenn bei Ausführen des T-SQL-Skripts der Fehler 40536 angezeigt wird, führen Sie das folgende T-SQL-Skript aus, um zu prüfen, ob die Datenbank In-Memory unterstützt:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Das Ergebnis **0** bedeutet, dass In-Memory nicht unterstützt wird. **1** bedeutet Unterstützung. Stellen Sie für die Problemdiagnose sicher, dass die Datenbank dem Premium-Tarif unterliegt.


#### <a name="about-the-created-memory-optimized-items"></a>Informationen zu speicheroptimierten Elementen

**Tabellen**: Das Beispiel enthält die folgenden speicheroptimierten Tabellen:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Sie können im **Objekt-Explorer** in SSMS speicheroptimierte Tabellen untersuchen. Klicken Sie mit der rechten Maustaste auf **Tabellen** > **Filter** > **Filtereinstellungen** > **Speicheroptimiert**. Der Wert ist gleich 1.


Oder Sie können die Katalogsichten abfragen wie z. B.:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Nativ kompilierte gespeicherte Prozedur** Sie können „SalesLT.usp_InsertSalesOrder_inmem“ über eine Katalogsichtabfrage untersuchen:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-the-sample-oltp-workload"></a>Ausführen der OLTP-Beispielworkload

Der einzige Unterschied zwischen den beiden folgenden *gespeicherten Prozeduren* ist, dass die erste Prozedur speicheroptimierte Versionen der Tabellen verwendet, während die zweite Prozedur die herkömmlichen Tabellen auf dem Datenträger nutzt:

- SalesLT**.**usp_InsertSalesOrder**_inmem**
- SalesLT**.**usp_InsertSalesOrder**_ondisk**


In diesem Abschnitt wird veranschaulicht, wie Sie das praktische Hilfsprogramm **ostress.exe** zum Ausführen der beiden gespeicherten Prozeduren bei hohen Belastungsgraden verwenden. Sie können vergleichen, wie lange es dauert, bis die beiden Belastungstests abgeschlossen sind.


Für das Ausführen von „ostress.exe“ wird empfohlen, dass Sie Parameterwerte übergeben, die für beide der folgenden ausgelegt sind:

- Führen Sie eine große Anzahl gleichzeitiger Verbindungen mit „-n100“ aus.
- Lassen Sie jede Verbindung eine Schleife Hunderte Male durchlaufen, indem Sie „-r500“ angeben.


Allerdings möchten Sie möglicherweise mit wesentlich kleineren Werten wie „-n10“ und „-r50“ starten, um sicherzustellen, dass alles funktioniert.


### <a name="script-for-ostressexe"></a>Skript für „ostress.exe“


Dieser Abschnitt zeigt das T-SQL-Skript, das in unsere „ostress.exe“-Befehlszeile eingebettet ist. Das Skript verwendet Elemente, die von dem zuvor von Ihnen installierten T-SQL-Skript erstellt wurden.


Das folgende Skript fügt einen Beispielauftrag mit fünf Positionen in die folgenden speicheroptimierten *Tabellen*ein:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


Um die *_ondisk*-Version des vorherigen T-SQL-Skripts für „ostress.exe“ zu erstellen, müssen Sie einfach beide Vorkommen der Teilzeichenfolge *_inmem* durch *_ondisk* ersetzen. Diese Austauschvorgänge wirken sich auf die Namen von Tabellen und gespeicherten Prozeduren aus.


### <a name="install-rml-utilities-and-ostress"></a>Installieren von RML-Hilfsprogrammen und ostress


Im Idealfall planen Sie die Ausführung von „ostress.exe“ auf einer Azure-VM. Dazu erstellen Sie eine [Azure-VM](https://azure.microsoft.com/documentation/services/virtual-machines/) in der gleichen Azure-Region, in der sich die AdventureWorksLT-Datenbank befindet. Sie können „ostress.exe“ aber auch auf Ihrem Laptop ausführen.


Installieren Sie auf der VM oder dem gewählten Host die RML-Hilfsprogramme (Replay Markup Language). Dazu zählt auch „ostress.exe“.

Weitere Informationen finden Sie unter:
- Diskussion zu „ostress.exe“ unter [Beispieldatenbank für In-Memory-OLTP](http://msdn.microsoft.com/library/mt465764.aspx).
- [Beispieldatenbank für In-Memory-OLTP](http://msdn.microsoft.com/library/mt465764.aspx).
- [Blog zur Installation von „ostress.exe“](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions to AdventureWorks to Demonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-the-inmem-stress-workload-first"></a>Zuerst Belastungsworkload *_inmem* ausführen


Sie können ein Fenster mit einer *RML-Eingabeaufforderung* zum Ausführen der „ostress.exe“-Befehlszeile verwenden. Die Befehlszeilenparameter weisen „ostress“ zu Folgendem an:

- Ausführen von 100 Verbindungen gleichzeitig (-n100).
- 50-maliges Ausführen des T-SQL-Skripts auf jeder Verbindung (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


So führen Sie die vorherige „ostress.exe“-Befehlszeile aus:


1. Setzen Sie den Dateninhalt der Datenbank durch Ausführen des folgenden Befehls in SSMS zurück, um alle Daten zu löschen, die bei vorherigen Ausführungen eingefügt wurden:
```
EXECUTE Demo.usp_DemoReset;
```

2. Kopieren Sie den Text der vorhergehenden „ostress.exe“-Befehlszeile in die Zwischenablage.

3. Ersetzen Sie `<placeholders>` für die Parameter „-S“, „-U“, „-P“ und „-d“ durch die ordnungsgemäßen tatsächlichen Werte.

4. Führen Sie die bearbeitete Befehlszeile in einem RML-Befehlsfenster aus.


#### <a name="result-is-a-duration"></a>Das Ergebnis ist eine Dauer


Wenn „ostress.exe“ abgeschlossen ist, wird die Testlaufdauer im RML-Befehlsfenster als letzte Zeile ausgegeben. Ein kürzerer Testlauf dauerte beispielsweise ca. 1,5 Minuten:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Zurücksetzen, für *_ondisk* bearbeiten, danach erneut ausführen


Sobald Sie über das Ergebnis der Ausführung von *_inmem* verfügen, führen Sie die folgenden Schritte für die Ausführung von *_ondisk* durch:


1. Setzen Sie die Datenbank durch Ausführen des folgenden Befehls in SSMS zurück, um alle Daten zu löschen, die bei der vorherigen Ausführung eingefügt wurden:
```
EXECUTE Demo.usp_DemoReset;
```

2. Bearbeiten Sie die ostress.exe-Befehlszeile so, dass alle Vorkommen von *_inmem* durch *_ondisk* ersetzt werden.

3. Führen Sie „ostress.exe“ ein zweites Mal aus, und erfassen Sie die Dauer.

4. Setzen Sie die Datenbank erneut zurück, um auf verantwortliche Weise eine u.U. große Menge an Testdaten zu löschen.


#### <a name="expected-comparison-results"></a>Erwartete Vergleichsergebnisse

Unsere In-Memory-Tests haben für diese einfache Workload eine **neunmal** höhere Leistung ergeben, wenn „ostress“ auf einem virtuellen Azure-Computer ausgeführt wird, der sich in der gleichen Azure-Region wie die Datenbank befindet.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-the-in-memory-analytics-sample"></a>2. Installieren des In-Memory Analytics-Beispiels


In diesem Abschnitt vergleichen Sie die E/A- und Statistikergebnisse mit dem Verwenden eines Columnstore-Indexes im Vergleich mit einem herkömmlichen B-Struktur-Index.


Für Echtzeitanalysen einer OLTP-Workload empfiehlt es sich häufig, einen nicht gruppierten Columnstore-Index zu verwenden. Weitere Informationen finden Sie unter [Beschreibung von Columnstore-Indizes](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-the-columnstore-analytics-test"></a>Vorbereiten des Columnstore-Analysetests


1. Erstellen Sie im Azure-Portal anhand des Beispiels eine neue AdventureWorksLT-Datenbank.
 - Verwenden Sie denselben Namen.
 - Wählen Sie den Tarif „Premium“.

2. Kopieren Sie [sql_in-memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) in die Zwischenablage.
 - Das T-SQL-Skript erstellt die erforderlichen In-Memory-Objekte in der in Schritt 1 erstellten Beispieldatenbank „AdventureWorksLT“.
 - Das Skript erstellt die Dimensionstabelle und zwei Faktentabellen. Beide Faktentabellen werden mit 3,5 Mio. Zeilen aufgefüllt.
 - Die Ausführung des Skripts kann 15 dauern.

3. Fügen Sie das T-SQL-Skript in SSMS ein, und führen Sie es aus. Unerlässlich ist das Schlüsselwort **COLUMNSTORE** in einer **CREATE INDEX**-Anweisung wie in:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Legen Sie „AdventureWorksLT“ auf den Kompatibilitätsgrad 130 fest: <br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Der Kompatibilitätsgrad 130 steht nicht in direkter Beziehung mit den In-Memory-Features  Doch der Grad 130 bietet im Allgemeinen eine höhere Abfrageleistung als 120.


#### <a name="key-tables-and-columnstore-indexes"></a>Wichtige Tabellen und Columnstore-Indizes


- „dbo.FactResellerSalesXL_CCI“ ist eine Tabelle mit einem gruppierten Columnstore-Index, der auf *Datenebene* eine erweiterte Komprimierung aufweist.

- „dbo.FactResellerSalesXL_PageCompressed“ ist eine Tabelle mit einem vergleichbaren herkömmlichen gruppierten Index, der nur auf *Seitenebene* komprimiert ist.


#### <a name="key-queries-to-compare-the-columnstore-index"></a>Wichtige Abfragen zum Vergleichen des Columnstore-Indexes


[Hier](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) finden Sie mehrere T-SQL-Abfragetypen, die Sie ausführen können, um Leistungssteigerungen zu erkennen. Achten Sie in Schritt 2 im T-SQL-Skript auf dieses Paar von Abfragen. Die beiden Abfragen unterscheiden sich nur in einer Zeile:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Für die Tabelle „FactResellerSalesXL\_CCI“ ist ein gruppierter Columnstore-Index vorhanden.

Der folgende T-SQL-Skriptauszug gibt Statistiken für E/A und Dauer der Abfrage der einzelnen Tabellen zurück.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order to see Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins the Fact Table with dimension tables
-- Note this query will run on the Page Compressed table, Note down the time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is the same Prior query on a table with a clustered columnstore index CCI
-- The comparison numbers are even more dramatic the larger the table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

In einer Datenbank mit dem P2-Tarif können Sie etwa einen neunfachen Leistungszuwachs für diese Abfrage bei Verwenden des gruppierten Columnstore-Indexes im Vergleich zum traditionellen Index erwarten. Mit P15 kann bei Verwenden des Columnstore-Indexes ein Leistungszuwachs um das 57-fache erwartet werden.



## <a name="next-steps"></a>Nächste Schritte

- [Schnellstart 1: In-Memory-OLTP-Technologien für höhere T-SQL-Leistung](http://msdn.microsoft.com/library/mt694156.aspx)

- [Verwenden von In-Memory-OLTP in einer vorhandenen Azure SQL-Anwendung](sql-database-in-memory-oltp-migration.md)

- [Überwachen des In-Memory-OLTP-Speichers](sql-database-in-memory-oltp-monitoring.md) für In-Memory-OLTP


## <a name="additional-resources"></a>Weitere Ressourcen

#### <a name="deeper-information"></a>Weiterführende Informationen

- [Erfahren Sie, wie Quorum die Workload der wichtigen Datenbank verdoppelt, während es die DTU mit In-Memory-OLTP in SQL-Datenbank um 70 % reduziert (in englischer Sprache)](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [Blogbeitrag „In-Memory OLTP in Azure SQL Database“](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/) (In-Memory OLTP in Azure SQL-Datenbank)

- [In-Memory OLTP (Arbeitsspeicheroptimierung)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Beschreibung von Columnstore-Indizes](https://msdn.microsoft.com/library/gg492088.aspx)

- [Erste Schritte mit Columnstore für operative Echtzeitanalyse](http://msdn.microsoft.com/library/dn817827.aspx)

- Das [Whitepaper zu allgemeinen Workloadmustern und Überlegungen zur Migration](http://msdn.microsoft.com/library/dn673538.aspx)beschreibt Workloadmuster, bei denen In-Memory-OLTP im Allgemeinen erhebliche Leistungssteigerungen bietet.

#### <a name="application-design"></a>Anwendungsentwurf

- [In-Memory OLTP (In-Memory Optimization)](http://msdn.microsoft.com/library/dn133186.aspx)

- [Verwenden von In-Memory-OLTP in einer vorhandenen Azure SQL-Anwendung](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Tools

- [Azure-Portal](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Data Tools (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx)

