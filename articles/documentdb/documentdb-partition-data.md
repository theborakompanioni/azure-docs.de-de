<properties 
	pageTitle="Partitionieren und Skalieren in Azure DocumentDB | Microsoft Azure"      
	description="Erfahren Sie, wie die Partitionierung in Azure DocumentDB funktioniert, wie die Partitionierung und Partitionsschlüssel konfiguriert werden und wie Sie den richtigen Partitionsschlüssel für Ihre Anwendung auswählen."         
	services="documentdb" 
	authors="arramac" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/20/2016" 
	ms.author="arramac"/>

# Partitionieren und Skalieren von Daten in DocumentDB
[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) wurde entwickelt, damit Sie schnelle, vorhersagbare Leistung für Ihre Anwendung erzielen und sie nahtlos skalieren können, wenn sie wächst. Dieser Artikel bietet einen Überblick darüber, wie Partitionierung in DocumentDB funktioniert. Er beschreibt, wie Sie DocumentDB-Sammlungen konfigurieren können, um Ihre Anwendungen effektiv skalieren zu können.

Nach dem Lesen dieses Artikels können Sie die folgenden Fragen beantworten:

- Wie funktioniert die Partitionierung in Azure DocumentDB?
- Wie konfiguriere ich die Partitionierung in DocumentDB?
- Was sind Partitionsschlüssel, und wie kann ich den richtigen Partitionsschlüssel für meine Anwendung auswählen?

Sie können das Projekt aus dem [DocumentDB-Treiberbeispiel zur Leistungsüberprüfung](https://github.com/Azure/azure-documentdb-dotnet/tree/a2d61ddb53f8ab2a23d3ce323c77afcf5a608f52/samples/documentdb-benchmark) herunterladen, um mit dem Programmieren loszulegen.

## Partitionieren in DocumentDB

In DocumentDB können Sie schemalose JSON-Dokumente jeder Größe speichern und innerhalb von Millisekunden abfragen. DocumentDB stellt Container für das Speichern von Daten bereit, die als **Sammlungen** bezeichnet werden. Sammlungen sind logische Ressourcen und können eine oder mehrere physische Partitionen oder einen oder mehrere Server umfassen. Die Anzahl der Partitionen wird von DocumentDB auf Basis der Speichergröße und dem bereitgestellten Durchsatz der Sammlung bestimmt. Jede Partition in DocumentDB verfügt über eine feste Menge an SSD-gestütztem Speicher, der dieser zugeordnet ist. Ebenso ist sie für hohe eine Verfügbarkeit repliziert. Die Partitionsverwaltung wird komplett von Azure DocumentDB verwaltet, Sie müssen also keine komplexen Codes schreiben oder Ihre Partitionen verwalten. DocumentDB-Sammlungen sind im Hinblick auf Speicher und Durchsatz **praktisch unbegrenzt**.

Partitionierung ist für Ihre Anwendung völlig transparent. DocumentDB unterstützt schnelle Lese- und Schreibvorgänge, SQL und LINQ-Abfragen, JavaScript-basierter Transaktionslogik, Konsistenzebenen sowie präzise Zugriffssteuerung per REST-API-Aufrufe auf eine einzelne Auflistungsressource. Der Dienst übernimmt die Verteilung von Daten über Partitionen sowie das Routing von Abfrage-Anforderungen an die richtige Partition.

Wie funktioniert das? Wenn Sie eine Sammlung in DocumentDB erstellen, können Sie einen Konfigurationswert für die **Partitionsschlüsseleigenschaft** angeben. Dies ist die JSON-Eigenschaft (oder der JSON-Pfad) in Ihren Dokumenten, die von DocumentDB verwendet werden kann, um Ihre Daten auf mehrere Server oder Partitionen zu verteilen. DocumentDB ermittelt den Hashwert des Partitionsschlüsselwerts und nutzt diesen, um die Partition zu bestimmen, in der das JSON-Dokument gespeichert wird. Alle Dokumente mit demselben Partitionsschlüssel werden in der gleichen Partition gespeichert.

Stellen Sie sich z.B. eine Anwendung vor, die Daten über Mitarbeiter und deren Abteilungen in DocumentDB speichert. Wir wählen `"department"` als Partitionsschlüsseleigenschaft aus, um Daten nach Abteilung horizontal hochzuskalieren. Jedes Dokument in DocumentDB muss eine obligatorische `"id"`-Eigenschaft aufweisen, die für jedes Dokument mit dem gleichen Partitionsschlüssel eindeutig sein muss, z.B. `"Marketing`". Jedes Dokument, das in einer Sammlung gespeichert ist, muss eine eindeutige Kombination aus Partitionsschlüssel und ID enthalten, z.B. `{ "Department": "Marketing", "id": "0001" }`, `{ "Department": "Marketing", "id": "0002" }` und `{ "Department": "Sales", "id": "0001" }`. In anderen Worten gilt die zusammengesetzte Eigenschaft aus (Partitionsschlüssel, id) als Primärschlüssel für Ihre Auflistung.

### Partitionsschlüssel
Die Auswahl des Partitionsschlüssels ist eine wichtige Entscheidung, die Sie zur Entwurfszeit treffen müssen. Sie müssen einen JSON-Eigenschaftennamen auswählen, der eine große Spanne von Werten sowie wahrscheinlich gleichmäßig verteilte Zugriffsmuster besitzt. Der Partitionsschlüssel wird als JSON-Pfad angegeben. So stellt z.B. `/department` die Eigenschaft „Abteilung“ dar.

Die folgende Tabelle zeigt Beispiele für Definitionen der Partitionsschlüssel und die entsprechenden JSON-Werte.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Partitionsschlüsselpfad</strong></p></td>
            <td valign="top"><p><strong>Beschreibung</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>/department</p></td>
            <td valign="top"><p>Entspricht dem JSON-Wert doc.department, wobei doc das Dokument ist.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/properties/name</p></td>
            <td valign="top"><p>Entspricht dem JSON-Wert doc.properties.name, wobei doc das Dokument (verschachtelte Eigenschaft) ist.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/id</p></td>
            <td valign="top"><p>Entspricht dem JSON-Wert doc.id (id und partition key sind dieselbe Eigenschaft).</p></td>
        </tr>
        <tr>
            <td valign="top"><p>/"Abteilungsname"</p></td>
            <td valign="top"><p>Entspricht dem JSON-Wert doc["Abteilungsname"], wobei doc das Dokument ist.</p></td>
        </tr>
    </tbody>
</table>

> [AZURE.NOTE] Die Syntax für den Partitionsschlüsselpfad ähnelt der Pfadspezifikation für die Indizierung von Richtlinienpfaden. Der wichtigste Unterschied besteht darin, dass der Pfad der Eigenschaft anstelle des Werts entspricht, d.h., es gibt keinen Platzhalter am Ende. Beispielsweise würden Sie /department/? angeben, um die Werte unter department zu indizieren, jedoch /department als Definition des Partitionsschlüssels. Der Partitionsschlüsselpfad wird implizit indiziert und kann nicht mithilfe von Überschreibungen für Indizierungsrichtlinien von der Indizierung ausgeschlossen werden.

Sehen wir uns an, wie sich die Auswahl der Partitionsschlüssel auf die Leistung Ihrer Anwendung auswirkt.

### Partitionierung und bereitgestellter Durchsatz
DocumentDB wurde für die vorhersagbare Leistung entwickelt. Wenn Sie eine Sammlung erstellen, reservieren Sie Durchsatz hinsichtlich der **[Anforderungseinheiten](documentdb-request-units.md) (request units; RUs) pro Sekunde**. Jeder Anforderung wird eine Gebühr für Anforderungseinheiten zugewiesen, die proportional zur Menge der Systemressourcen wie CPU und E/A ist, die vom Vorgang genutzt werden. Der Lesevorgang eines Dokuments mit der Größe von 1 KB und mit einer Sitzungskonsistenz beansprucht eine Anforderungseinheit. Ein Lesevorgang entspricht einer RU, unabhängig von der Anzahl der Elemente, die gespeichert sind oder der Anzahl gleichzeitiger Anforderungen, die parallel ausgeführt werden. Abhängig von der Größe erfordern größere Dokumente höhere Anforderungseinheiten. Wenn Sie die Größe Ihrer Entitäten sowie die von Ihrer Anwendung benötigte Anzahl an Lesevorgängen kennen, können Sie Ihrer Anwendung für den Lesevorgang exakt den benötigten Durchsatz bereitstellen.

Wenn DocumentDB Dokumente speichert, verteilt es diese anhand des Partitionsschlüsselwerts gleichmäßig zwischen Partitionen. Der Durchsatz wird auch gleichmäßig zwischen den verfügbaren Partitionen verteilt, z.B. der Durchsatz pro Partition = (gesamter Durchsatz pro Sammlung) / (Anzahl der Partitionen).

>[AZURE.NOTE] Um den gesamten Durchsatz der Sammlung zu erzielen, müssen Sie einen Partitionsschlüssel auswählen, der Ihnen ermöglicht, Anforderungen gleichmäßig zwischen einer Anzahl unterschiedlicher Partitionsschlüsselwerten zu verteilen.

## Einzelne Partitionen und partitionierte Sammlungen
DocumentDB unterstützt die Erstellung von einzelnen Partitionen und partitionierte Sammlungen.

- **Partitionierte Sammlungen** können mehrere Partitionen umfassen und sehr große Mengen an Speicher und Durchsatz unterstützen. Sie müssen einen Partitionsschlüssel für die Sammlung angeben.
- **Einzelne partitionierte Sammlungen** haben niedrigere Preisoptionen und die Fähigkeit, Transaktionen über alle Sammlungsdaten hinweg abzufragen und durchzuführen. Sie haben das Skalierbarkeitslimit und die Speichergrenzwerte einer einzelnen Partition. Sie müssen keinen Partitionsschlüssel für diese Sammlungen angeben.

![Partitionierte Sammlungen in DocumentDB][2]

Für Szenarios, die keine große Mengen an Speicher oder Durchsatz benötigen, sind Sammlungen mit nur einer Partition gut geeignet. Beachten Sie, dass Sammlungen mit nur einer Partition das Skalierbarkeitslimit und die Speichergrenzwerte einer einzelnen Partition besitzen, d.h. bis zu 10 GB Speicher und bis zu 10.000 Anforderungseinheiten pro Sekunde.

Partitionierte Sammlungen können sehr große Mengen an Speicher und Durchsatz unterstützen. Die Standardangebote sind jedoch dafür konfiguriert, bis zu 250 GB Speicher zu speichern und bis zu 250.000 Anforderungseinheiten pro Sekunde zentral hochzuskalieren. Wenn Sie mehr Speicher oder Durchsatz pro Sammlung benötigen, wenden Sie sich an den [Azure-Support](documentdb-increase-limits.md), um diese für Ihr Konto vergrößern zu lassen.

In der folgenden Tabelle sind die Unterschiede zwischen der Arbeit mit einer Sammlung mit einer Partition und der mit partitionierten Sammlungen aufgeführt:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Sammlung mit einer Partition</strong></p></td>
            <td valign="top"><p><strong>Partitionierte Sammlung</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Partitionsschlüssel</p></td>
            <td valign="top"><p>Keine</p></td>
            <td valign="top"><p>Erforderlich</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Primärschlüssel für Dokument</p></td>
            <td valign="top"><p>"id"</p></td>
            <td valign="top"><p>Verbundschlüssel &lt;Partitionsschlüssel> und "id"</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimale Speichergröße</p></td>
            <td valign="top"><p>0 GB</p></td>
            <td valign="top"><p>0 GB</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximale Speichergröße</p></td>
            <td valign="top"><p>10 GB</p></td>
            <td valign="top"><p>Unbegrenzt (250 GB standardmäßig)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Minimaler Durchsatz</p></td>
            <td valign="top"><p>400 Anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>10.000 Anforderungseinheiten pro Sekunde</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Maximaler Durchsatz</p></td>
            <td valign="top"><p>10.000 Anforderungseinheiten pro Sekunde</p></td>
            <td valign="top"><p>Unbegrenzt (250.000 Anforderungseinheiten pro Sekunde in der Standardeinstellung)</p></td>
        </tr>
        <tr>
            <td valign="top"><p>API-Versionen</p></td>
            <td valign="top"><p>Alle</p></td>
            <td valign="top"><p>API 2015-12-16 und neuer</p></td>
        </tr>
    </tbody>
</table>

## Arbeiten mit den SDKs

Azure DocumentDB bietet nun zusätzliche Unterstützung für die automatische Partitionierung durch [REST-API-Version 2015-12-16](https://msdn.microsoft.com/library/azure/dn781481.aspx). Um partitionierte Sammlungen erstellen zu können, müssen Sie die SDK-Versionen 1.6.0 oder neuer in einer der unterstützten SDK Plattformen (.NET, Node.js, Java, Python) herunterladen.

### Erstellen von partitionierten Sammlungen

Das folgende Beispiel zeigt einen Ausschnitt für .NET, zum Erstellen einer Sammlung, um Gerätetelemetriedaten von 20.000 Anforderungseinheiten pro Durchsatzsekunde zu speichern. Das SDK legt den Wert für „OfferThroughput“ fest (der wiederum den Anforderungsheader `x-ms-offer-throughput` in der REST-API festlegt). Hier legen wir `/deviceId` als Partitionsschlüssel fest. Die Wahl des Partitionsschlüssels wird zusammen mit dem Rest der Metadaten der Sammlung wie Name und Indizierungsrichtlinie gespeichert.

Für dieses Beispiel haben wir `deviceId` ausgewählt, da es (a) möglich ist, die Schreibvorgänge aufgrund der großen Anzahl von Geräten gleichmäßig auf Partitionen zu verteilen, und wir so die Datenbanken hochskalieren können, um riesige Datenmengen erfassen zu können, und da (b) viele der Anforderungen, wie das Abrufen der letzten Lesevorgänge für ein Gerät, einer einzelnen deviceId zugeordnet sind und von einer einzelnen Partition abgerufen werden können.

    DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
    await client.CreateDatabaseAsync(new Database { Id = "db" });

    // Collection for device telemetry. Here the JSON property deviceId will be used as the partition key to 
    // spread across partitions. Configured for 10K RU/s throughput and an indexing policy that supports 
    // sorting against any number or string property.
    DocumentCollection myCollection = new DocumentCollection();
    myCollection.Id = "coll";
    myCollection.PartitionKey.Paths.Add("/deviceId");

    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri("db"),
        myCollection,
        new RequestOptions { OfferThroughput = 20000 });
        

> [AZURE.NOTE] Um partitionierte Sammlungen zu erstellen, müssen Sie einen Durchsatzwert von mehr als 10.000 Anforderungseinheiten pro Sekunde angeben. Da der Durchsatz in Vielfachen von 100 angegeben wird, muss dies 10.100 oder höher sein.

Auf diese Weise wird ein REST-API-Aufruf in DocumentDB durchgeführt, und der Dienst stellt eine Anzahl von Partitionen basierend auf dem angeforderten Durchsatz bereit. Sie können den Durchsatz einer Sammlung ändern, wenn Sie eine höhere Leistung benötigen. Weitere Informationen finden Sie unter [Leistungsstufen](documentdb-performance-levels.md).

### Lesen und Schreiben von Dokumenten

Jetzt fügen wir Daten in DocumentDB ein. Nachstehend finden Sie eine Beispielklasse, die eine Geräteanzeige enthält sowie einen Aufruf von CreateDocumentAsync, um ein neues Gerät mit Lesevorgang einer Sammlung hinzuzufügen.

    public class DeviceReading
    {
        [JsonProperty("id")]
        public string Id;

        [JsonProperty("deviceId")]
        public string DeviceId;

        [JsonConverter(typeof(IsoDateTimeConverter))]
        [JsonProperty("readingTime")]
        public DateTime ReadingTime;

        [JsonProperty("metricType")]
        public string MetricType;

        [JsonProperty("unit")]
        public string Unit;

        [JsonProperty("metricValue")]
        public double MetricValue;
      }

    // Create a document. Here the partition key is extracted as "XMS-0001" based on the collection definition
    await client.CreateDocumentAsync(
        UriFactory.CreateDocumentCollectionUri("db", "coll"),
        new DeviceReading
        {
            Id = "XMS-001-FE24C",
            DeviceId = "XMS-0001",
            MetricType = "Temperature",
            MetricValue = 105.00,
            Unit = "Fahrenheit",
            ReadingTime = DateTime.UtcNow
        });


Nun rufen wir das Dokument mithilfe seines Partitionsschlüssels und seiner id auf, aktualisieren es und löschen es schließlich unter Angabe dieser beiden Werte. Beachten Sie, dass der Lesevorgang einen Wert für „PartitionKey“ enthält (entsprechend dem Anforderungsheader `x-ms-documentdb-partitionkey` in der REST-API).

    // Read document. Needs the partition key and the ID to be specified
    Document result = await client.ReadDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });

    DeviceReading reading = (DeviceReading)(dynamic)result;

    // Update the document. Partition key is not required, again extracted from the document
    reading.MetricValue = 104;
    reading.ReadingTime = DateTime.UtcNow;

    await client.ReplaceDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      reading);

    // Delete document. Needs partition key
    await client.DeleteDocumentAsync(
      UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
      new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });



### Abfragen von partitionierten Sammlungen

Beim Abfragen von Daten in partitionierten Sammlungen leitet DocumentDB die Abfrage automatisch an die Partitionen weiter, die den im Filter angegebenen Partitionsschlüsselwerten entsprechen (sofern vorhanden). Diese Abfrage wird z.B. nur an die Partition weitergeleitet, die den Partitionsschlüssel „XMS-0001“ enthält.

    // Query using partition key
    IQueryable<DeviceReading> query = client.CreateDocumentQuery<DeviceReading>(
    	UriFactory.CreateDocumentCollectionUri("db", "coll"))
        .Where(m => m.MetricType == "Temperature" && m.DeviceId == "XMS-0001");

Die folgende Abfrage verfügt nicht über einen Filter für den Partitionsschlüssel (DeviceId) und wird an alle Partitionen verteilt, wo sie auf dem Partitionsindex ausgeführt wird. Beachten Sie, dass Sie „EnableCrossPartitionQuery“ (`x-ms-documentdb-query-enablecrosspartition` in der REST-API) angeben müssen, damit das SDK eine partitionsübergreifende Abfrage ausführen kann.

    // Query across partition keys
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true })
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100);

### Ausführung paralleler Abfragen

Ab dem DocumentDB SDK 1.9.0 werden Optionen zur parallelen Ausführung von Abfragen unterstützt. Dadurch können Sie Abfragen mit niedriger Latenz auf partitionierte Sammlungen anwenden, auch wenn eine große Anzahl von Sammlungen berücksichtigt werden muss. Die folgende Abfrage ist z.B. so konfiguriert, dass sie partitionsübergreifend parallel ausgeführt wird.

    // Cross-partition Order By Queries
    IQueryable<DeviceReading> crossPartitionQuery = client.CreateDocumentQuery<DeviceReading>(
        UriFactory.CreateDocumentCollectionUri("db", "coll"), 
        new FeedOptions { EnableCrossPartitionQuery = true, MaxDegreeOfParallelism = 10, MaxBufferedItemCount = 100})
        .Where(m => m.MetricType == "Temperature" && m.MetricValue > 100)
        .OrderBy(m => m.MetricValue);

Sie können die parallele Ausführung von Abfragen verwalten, indem Sie die folgenden Parameter optimieren:

- Durch Festlegen von `MaxDegreeOfParallelism` können Sie den Grad der Parallelität steuern, d.h. die maximale Anzahl gleichzeitiger Verbindungen mit den Partitionen der Sammlung. Wenn Sie diese Einstellung auf „-1“ festlegen, wird der Grad der Parallelität vom SDK verwaltet. Falls `MaxDegreeOfParallelism` nicht angegeben oder auf 0 (Standardwert) festgelegt wurde, besteht eine einzelne Netzwerkverbindung mit den Partitionen der Sammlung.
- Durch Festlegen von `MaxBufferedItemCount` können Sie eine Abstimmung zwischen Abfragelatenz und Speicherauslastung auf Clientseite ermöglichen. Wenn Sie diesen Parameter weglassen oder auf „-1“ festlegen, wird die Anzahl der Elemente, die während der Ausführung paralleler Abfragen gepuffert wird, vom SDK verwaltet.

Bei gleichem Zustand der Sammlung gibt eine parallele Abfrage Ergebnisse in der gleichen Reihenfolge wie bei einer seriellen Ausführung zurück. Beim Durchführen einer partitionsübergreifenden Abfrage mit Sortierung (ORDER BY und/oder TOP) führt das DocumentDB SDK die Abfrage parallel auf den Partitionen durch und führt teilweise sortierte Ergebnisse auf Clientseite zusammen, um global sortierte Ergebnisse zu generieren.

### Ausführen von gespeicherten Prozeduren

Sie können auch atomarische Transaktionen für Dokumente mit derselben Geräte-ID ausführen, z.B. wenn Sie Aggregate oder den aktuellen Status eines Geräts in einem einzelnen Dokument verwalten.

    await client.ExecuteStoredProcedureAsync<DeviceReading>(
        UriFactory.CreateStoredProcedureUri("db", "coll", "SetLatestStateAcrossReadings"),
        new RequestOptions { PartitionKey = new PartitionKey("XMS-001") }, 
        "XMS-001-FE24C");

Im nächsten Abschnitt untersuchen wir, wie partitionierte Sammlungen aus Sammlungen mit nur einer Partition verschoben werden können.

<a name="migrating-from-single-partition"></a>
### Migrieren von Sammlungen mit nur einer Partitionen zu partitionierten Sammlungen
Wenn eine Anwendung, die eine Sammlung mit nur einer Partition verwendet, einen höheren Durchsatz (> 10.000 RU/s) oder mehr Speicher (> 10 GB) benötigt, können Sie mit dem [DocumentDB-Datenmigrationstool](http://www.microsoft.com/downloads/details.aspx?FamilyID=cda7703a-2774-4c07-adcc-ad02ddc1a44d) die Daten aus der Sammlung mit nur einer Partition zu einer partitionierten Sammlung migrieren.

So führen Sie eine Migration aus einer Sammlung mit nur einer Partition in eine partitionierte Sammlung durch

1. Exportieren Sie Daten aus der Sammlung mit nur einer Partition nach JSON. Weitere Informationen finden Sie unter [Exportieren in eine JSON-Datei](documentdb-import-data.md#export-to-json-file).
2. Importieren Sie die Daten in eine partitionierte Sammlung, die mit einer Partitionsschlüsseldefinition erstellt wurde und mehr als 10.000 Anforderungseinheiten pro Sekunde aufweist, wie im nachstehenden Beispiel gezeigt. Weitere Informationen finden Sie unter [Importieren in DocumentDB](documentdb-import-data.md#DocumentDBSeqTarget).

![Migrieren von Daten in eine partitionierte Sammlung in DocumentDB][3]

>[AZURE.TIP] Erhöhen Sie für schnellere Importvorgänge die Anzahl der parallelen Anforderungen auf 100 oder höher, um den höheren verfügbaren Durchsatz für partitionierte Sammlungen zu nutzen.

Nun, da wir die Grundlagen abgeschlossen haben, sehen wir uns einige wichtige Entwurfsüberlegungen bei der Arbeit mit Partitionsschlüsseln in DocumentDB an.

## Entwerfen für Partitionierung
Die Auswahl des Partitionsschlüssels ist eine wichtige Entscheidung, die Sie zur Entwurfszeit treffen müssen. Dieser Abschnitt beschreibt einige der Vor-und Nachteile bei der Auswahl der Partitionsschlüssel für Ihre Sammlung.

### Partitionsschlüssel als Transaktionsgrenze
Sie sollten Ihren Partitionsschlüssel so wählen, dass Transaktionen vorgenommen werden können und zugleich die Skalierbarkeit der Lösung gegeben ist (durch Verteilung Ihrer Entitäten auf mehrere Partitionsschlüssel). In einem Extremfall können Sie denselben Partitionsschlüssel für alle Dokumente festlegen, aber dies kann die Skalierbarkeit Ihrer Lösung beeinträchtigen. Im anderen Extremfall können Sie jedem Dokument einen eindeutigen Partitionsschlüssel zuweisen. Dies ermöglicht zwar eine hohe Skalierbarkeit, aber es wird auch verhindert, dass Sie dokumentübergreifende Transaktionen über gespeicherte Prozeduren und Trigger verwenden. Ein idealer Partitionsschlüssel ermöglicht die Verwendung von effizienten Abfragen und verfügt über eine ausreichende Kardinalität, um sicherzustellen, dass Ihre Lösung skalierbar ist.

### Vermeiden von Speicher- und Leistungsengpässen 
Es ist auch wichtig, eine Eigenschaft auszuwählen, mit der Schreibvorgänge über eine Anzahl von unterschiedlichen Werten verteilt werden können. Anforderungen an den gleichen Partitionsschlüssel können den Durchsatz einer einzelnen Partition nicht überschreiten und werden gedrosselt. Daher ist es wichtig, einen Partitionsschlüssel auszuwählen, der nicht zu **„Hotspots“** innerhalb Ihrer Anwendung führt. Die gesamte Speichergröße für Dokumente mit demselben Partitionsschlüssel kann ebenso 10 GB an Speicherplatz nicht überschreiten.

### Beispiele für gute Partitionsschlüssel
Nachstehend finden Sie einige Beispiele dafür, wie Sie den Partitionsschlüssel für Ihre Anwendung auswählen:

* Wenn Sie ein Benutzerprofil-Back-End implementieren, ist die Benutzer-ID eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie IoT-Daten speichern, z.B. Gerätestatus, ist eine Geräte-ID eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie DocumentDB für die Protokollierung von Zeitreihendaten verwenden, ist der Hostname oder die Prozess-ID eine gute Wahl für den Partitionsschlüssel.
* Wenn Sie über eine mehrinstanzenfähige Architektur verfügen, ist die Mandanten-ID eine gute Wahl für Partitionsschlüssel.

Beachten Sie, dass in einigen Fällen (z.B. bei IoT und Benutzerprofilen wie oben beschrieben) der Partitionsschlüssel mit Ihrer ID (Dokumentenschlüssel) identisch sein kann. In anderen Fällen, wie etwa bei den Zeitreihendaten, kann sich der Partitionsschlüssel von der ID unterscheiden.

### Partitionierung und Protokollierung/Zeitreihendaten
Einen der häufigsten Anwendungsfälle von DocumentDB stellen Protokollierung und Telemetrie dar. Es ist wichtig, einen guten Partitionsschlüssel auswählen, da Sie möglicherweise große Datenmengen lesen oder schreiben müssen. Die Auswahl hängt von Ihren Lese- und Schreibraten und den zu erwartenden Abfragen ab. Im Folgenden finden Sie einige Tipps zum Auswählen eines guten Partitionsschlüssels.

- Wenn Ihr Anwendungsfall eine geringe Rate von Schreibvorgängen umfasst, die sich über längere Zeit ansammeln, und Abfragen nach Zeitstempelbereichen und anderen Filtern durchführen müssen, ist ein Rollup des Zeitstempels, z.B. mit dem Datum als Partitionsschlüssel, ein guter Ansatz. Dadurch können Sie Abfragen über alle Daten für ein Datum von einer einzigen Partition ausführen.
- Wenn Ihre Workload schreiblastig ist (im Allgemeinen der häufigere Fall), sollten Sie einen Partitionsschlüssel verwenden, der nicht auf Zeitstempeln basiert, sodass DocumentDB Schreibvorgänge gleichmäßig über eine Anzahl von Partitionen verteilen kann. Hierbei sind Hostname, Prozess-ID, Aktivitäts-ID oder eine andere Eigenschaft mit hoher Kardinalität eine gute Wahl.
- Ein dritter Ansatz ist eine Mischung, bei der für jeden Tag/Monat eine Sammlung vorhanden ist und der Partitionsschlüssel eine differenzierte Eigenschaft wie Hostname ist. Dies hat den Vorteil, dass Sie unterschiedliche Leistungsstufen entsprechend dem Zeitfenster festlegen können, z.B. kann die Sammlung für den aktuellen Monat mit höherem Durchsatz bereitgestellt werden, da sie für Lese- und Schreibvorgänge verwendet wird, während in vorherigen Monaten ein geringerer Durchsatz ausreichend war, da nur Lesevorgänge ausgeführt wurden.

### Partitionierung und Mehrinstanzenfähigkeit
Wenn Sie eine mehrinstanzenfähige Anwendung mithilfe von DocumentDB implementieren, gibt es zwei wichtige Muster für die Implementierung von Mandanten mit DocumentDB – einen Partitionsschlüssel pro Mandant und eine Auflistung pro Mandant. Hier sind ihre Vor- und Nachteile auflistet:

* Ein Partitionsschlüssel pro Mandant: In diesem Modell sind Mandanten innerhalb einer einzelnen Sammlung verbunden. Abfragen und Einfügungen können jedoch für Dokumente innerhalb eines einzelnen Mandanten für eine einzelne Partition ausgeführt werden. Sie können auch die Transaktionslogik über alle Dokumenten innerhalb eines Mandanten hinweg implementieren. Da mehrere Mandanten eine Sammlung gemeinsam nutzen, können Sie die Kosten für Speicher und Durchsatz sparen, indem Sie Ressourcen für Mandanten in einer einzelnen Sammlung zusammenfassen, anstatt zusätzlichen Toleranzbereich für jeden Mandanten bereitzustellen. Der Nachteil darin besteht, dass Sie über keine Leistungsisolation pro Mandant verfügen. Im Vergleich zu gezielten Steigerungen für Mandanten werden Erhöhungen von Leistung/Durchsatz auf die komplette Sammlung angewendet.
* Eine Auflistung pro Mandant: jeder Mandant verfügt über seine eigene Sammlung. In diesem Modell können Sie die Leistung pro Mandant reservieren. Mit dem neuen, nutzungsbasierten Preismodell von DocumentDB, ist dieses Modell kosteneffektiver für mehrinstanzenfähige Anwendungen mit einer kleinen Anzahl von Mandanten.

Sie können auch eine Kombination/einen abgestuften Ansatz verwenden. Hier werden kleine Mandanten zusammengefasst und größere Mandanten zu ihrer eigenen Sammlung migriert.

## Nächste Schritte
In diesem Artikel haben wir beschrieben, wie Partitionierung in Azure DocumentDB funktioniert, wie Sie partitionierte Sammlungen erstellen können, und wie Sie einen guten Partitionsschlüssel für Ihre Anwendung auswählen können.

-   Nutzen Sie DocumentDB, um die Skalierung und die Leistung zu testen. Weitere Informationen und ein Beispiel finden Sie unter [Leistungs- und Skalierungstests mit Azure DocumentDB](documentdb-performance-testing.md).
-   Erste Schritte bei der Codierung mit den [SDKs](documentdb-sdk-dotnet.md) oder der [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx)
-   Erfahren Sie mehr über den [bereitgestellten Durchsatz in DocumentDB](documentdb-performance-levels.md).
-   Wenn Sie selbst anpassen möchten, wie Ihre Anwendung die Partitionierung ausführt, können Sie Ihre eigene clientseitige Partitionierungsimplementierung verwenden. Siehe [Unterstützung für clientseitige Partitionierung](documentdb-sharding.md).

[1]: ./media/documentdb-partition-data/partitioning.png
[2]: ./media/documentdb-partition-data/single-and-partitioned.png
[3]: ./media/documentdb-partition-data/documentdb-migration-partitioned-collection.png

 

<!---HONumber=AcomDC_0921_2016-->