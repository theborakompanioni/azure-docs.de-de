---
title: Datenbankfragen zu DocumentDB – Häufig gestellte Fragen | Microsoft Docs
description: Erhalten Sie Antworten auf häufig gestellte Fragen zum Azure DocumentDB-NoSQL-Dokumentdatenbankdienst für JSON. Beantworten Sie Datenbankfragen zu Kapazität, Leistung und Skalierung.
keywords: Datenbankfragen,häufig gestellte Fragen,DocumentDB,Azure,Microsoft Azure
services: documentdb
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: ''

ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: mimig

---
# Häufig gestellte Fragen zu DocumentDB
## Datenbankfragen zu den Grundlagen von Microsoft Azure DocumentDB
### Was ist Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB ist eine überaus schnelle global skalierbare und als Dienst verfügbare NoSQL-Dokumentdatenbank, die umfangreiche Abfragen von nicht schematisierten Daten ermöglicht, konfigurierbare, zuverlässige Leistung bietet und eine schnelle Bereitstellung ermöglicht – und all das auf einer verwalteten Plattform, die auf der Leistungsfähigkeit und Reichweite von Microsoft Azure basiert. DocumentDB ist die geeignete Lösung für Web-, Mobil-, Gaming- und IoT-Anwendungen, wenn vorhersagbarer Durchsatz, hohe Verfügbarkeit, niedrige Latenz und schemafreie Datenmodelle wichtige Anforderungen sind. DocumentDB bietet Schemaflexibilität und umfassende Indizierung über ein systemeigenes JSON-Datenmodell und beinhaltet die Transaktionsunterstützung mehrerer Dokumente durch integriertes JavaScript.

Weitere Datenbankfragen, -antworten und -anweisungen zur Bereitstellung und Verwendung des Diensts finden Sie auf der Seite [DocumentDB-Dokumentation](https://azure.microsoft.com/documentation/services/documentdb/).

### Was für eine Art Datenbank ist DocumentDB?
DocumentDB ist eine dokumentorientierte NoSQL-Datenbank, die Daten im JSON-Format speichert. DocumentDB unterstützt geschachtelte, eigenständige Datenstrukturen, die mithilfe einer umfassenden DocumentDB-[SQL-Abfragegrammatik](documentdb-sql-query.md) abgefragt werden können. DocumentDB bietet eine hochleistungsfähige Transaktionsverarbeitung von serverseitigem JavaScript durch [gespeicherte Prozeduren, Trigger und benutzerdefinierte Funktionen](documentdb-programming.md). Die Datenbank unterstützt außerdem vom Entwickler optimierbare Konsistenzebenen mit den zugehörigen [Leistungsebenen](documentdb-performance-levels.md).

### Verfügen DocumentDB-Datenbanken über Tabellen wie eine relationale Datenbank (RDBMS)?
Nein, DocumentDB speichert Daten als JSON-Dokumentsammlungen. Weitere Informationen zu den DocumentDB-Ressourcen finden Sie im Artikel [Ressourcenmodell und Konzepte von DocumentDB](documentdb-resources.md). Weitere Informationen dazu, wie sich NoSQL-Lösungen wie DocumentDB von relationalen Lösungen unterscheiden, finden Sie unter [NoSQL im Vergleich zu SQL](documentdb-nosql-vs-sql.md).

### Unterstützen DocumentDB-Datenbanken schemafreie Daten?
Ja, DocumentDB ermöglicht Anwendungen das Speichern beliebiger JSON-Dokumente ohne Schemadefinition oder -hinweise. Die Daten stehen unmittelbar zur Abfrage über die DocumentDB SQL-Abfrageschnittstelle zur Verfügung.

### Unterstützt DocumentDB ACID-Transaktionen?
Ja, DocumentDB unterstützt dokumentübergreifende Transaktionen, die in Form von gespeicherten JavaScript-Prozeduren und Triggern ausgedrückt werden. Die Transaktionen werden einer Partition in jeder Sammlung zugeordnet und mit ACID-Semantik nach dem Prinzip „alles oder nichts“ ausgeführt. Sie sind dabei von anderem gleichzeitig ausgeführtem Code und Benutzeranforderungen isoliert. Falls bei der serverseitigen Ausführung des JavaScript-Anwendungscodes ein Ausnahmefehler auftritt, wird für die gesamte Transaktion ein Rollback durchgeführt. Weitere Informationen zu Transaktionen finden Sie unter [Datenbankprogramm-Transaktionen](documentdb-programming.md#database-program-transactions).

### Was sind die typischen Anwendungsfälle für DocumentDB?
DocumentDB ist eine gute Wahl für neue Web-, Mobil-, Gaming- und IoT-Anwendungen, bei denen Dinge wie automatische Skalierung, vorhersagbare Leistung, kurze Reaktionszeiten im Millisekundenbereich und die Möglichkeit zum Abfragen von schemalosen Daten wichtig sind. DocumentDB eignet sich für schnelle Entwicklungen und die Unterstützung kontinuierlicher Iterationen von Anwendungsdatenmodellen. Anwendungen, die von Benutzern erzeugte Inhalte und Daten verwalten, sind ein [häufiger Anwendungsfall von DocumentDB](documentdb-use-cases.md).

### Wie wird bei DocumentDB für eine vorhersagbare Leistung gesorgt?
In DocumentDB wird der Durchsatz in [Anforderungseinheiten](documentdb-request-units.md) gemessen. 1 Anforderungseinheit entspricht dem Durchsatz der GET-Anforderung für ein Dokument mit einer Größe von 1 KB. Jeder Vorgang in DocumentDB (einschließlich der Ausführung von Lese- und Schreibvorgängen, SQL-Abfragen sowie gespeicherten Prozeduren) verfügt über einen deterministischen Wert für die Anforderungseinheiten, der auf dem erforderlichen Durchsatz zum Abschließen des Vorgangs basiert. Im Hinblick auf den Durchsatz Ihrer Anwendung müssen Sie sich also keine Gedanken um das Zusammenspiel von CPU, E/A-Leistung und Arbeitsspeicher machen, sondern können mit einer einzigen Kennzahl arbeiten: der Anforderungseinheit.

Jede DocumentDB-Sammlung kann mit bereitgestelltem Durchsatz reserviert werden (Anforderungseinheiten des Durchsatzes pro Sekunde). Für Anwendungen jeder Größe können Sie ein Vergleichstests einzelner Anforderungen durchführen, um deren Werte für die Anforderungseinheiten zu messen. Außerdem können Sie Sammlungen bereitstellen, um die Gesamtsumme der Anforderungseinheiten über alle Anforderungen hinweg zu verarbeiten. Sie können den Durchsatz der Sammlung auch zentral hoch- oder herunterskalieren, wenn sich die Anforderungen Ihrer Anwendung ändern. Weitere Informationen zu Anforderungseinheiten und Hilfe zum Ermitteln Ihrer Sammlungsanforderungen finden Sie unter [Verwalten der Leistung und Kapazität](documentdb-manage.md). Probieren Sie außerdem den [Durchsatzrechner](https://www.documentdb.com/capacityplanner) aus.

### Ist DocumentDB mit HIPAA kompatibel?
Ja, DocumentDB ist mit HIPAA kompatibel. HIPAA gibt Anforderungen für die Verwendung, Offenlegung und den Schutz von individuell identifizierbaren Gesundheitsinformationen vor. Weitere Informationen finden Sie im [Microsoft Trust Center](https://www.microsoft.com/de-DE/TrustCenter/Compliance/HIPAA).

### Wie lauten die Speicherbeschränkungen von DocumentDB?
Es gibt keine theoretische Beschränkung in Bezug auf die Gesamtmenge der Daten, die von einer Sammlung in DocumentDB gespeichert werden kann. Falls Sie mehr als 250 GB Daten in einer einzelnen Sammlung speichern möchten, [bitten Sie den Support](documentdb-increase-limits.md), Ihr Kontokontingent zu erhöhen.

### Wo liegen die Durchsatzgrenzwerte von DocumentDB?
Es gibt kein theoretisches Limit für den Gesamtdurchsatz, der von einer Sammlung in DocumentDB unterstützt werden kann, wenn Ihre Workload in etwa gleichmäßig auf eine ausreichend große Zahl von Partitionsschlüsseln verteilt werden kann. Falls Sie pro Sammlung oder Konto mehr als 250.000 Anforderungseinheiten pro Sekunde verwenden möchten, [bitten Sie den Support](documentdb-increase-limits.md), Ihr Kontokontingent zu erhöhen.

### Wie viel kostet Microsoft Azure DocumentDB?
Ausführliche Informationen dazu finden Sie auf der Seite [DocumentDB – Preise](https://azure.microsoft.com/pricing/details/documentdb/). Die Nutzungsgebühren für DocumentDB werden von der verwendeten Anzahl von Sammlungen, der Anzahl von Stunden, die die Sammlungen online waren, und dem genutzten Speicher und bereitgestellten Durchsatz für jede Sammlung bestimmt.

### Ist ein kostenloses Konto verfügbar?
Wenn Azure für Sie neu ist, können Sie sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) registrieren. Sie erhalten eine Gutschrift von 200 US-Dollar für 30 Tage, in denen Sie alle Azure-Dienste ausprobieren können. Wenn Sie ein Visual Studio-Abonnement besitzen, haben Sie Anspruch auf ein [kostenloses Azure-Guthaben von 150 US-Dollar pro Monat](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), das Sie für beliebige Azure-Dienste nutzen können.

### Wie kann ich zusätzliche Hilfe zu DocumentDB erhalten?
Wenn Sie Hilfe benötigen, wenden Sie sich auf [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-documentdb) oder in den [MSDN-Foren für Entwickler zu Azure DocumentDB](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureDocumentDB) an uns, oder terminieren Sie einen [1:1-Chat mit dem Entwicklungsteam von DocumentDB](http://www.askdocdb.com/). Um bei den neuesten Features von DocumentDB auf dem Laufenden zu bleiben, folgen Sie uns auf [Twitter](https://twitter.com/DocumentDB).

## Einrichten von Microsoft Azure DocumentDB
### Wie registriere ich mich für Microsoft Azure DocumentDB?
Microsoft Azure DocumentDB steht im [Azure-Portal][azure-portal] zur Verfügung. Zuerst müssen Sie sich für ein Azure-Abonnement anmelden. Nachdem Sie sich für ein Microsoft Azure-Abonnement registriert haben, können Sie Ihrem Azure-Abonnement ein DocumentDB-Konto hinzufügen. Anweisungen zum Hinzufügen eines DocumentDB-Kontos finden Sie unter [Erstellen eines DocumentDB-Datenbankkontos](documentdb-create-account.md).

### Was ist ein Hauptschlüssel?
Ein Hauptschlüssel ist ein Sicherheitstoken für den Zugriff auf alle Ressourcen eines Kontos. Personen, die über diesen Schlüssel verfügen, haben Lese- und Schreibzugriff auf alle Ressourcen im Datenbankkonto. Gehen Sie beim Verteilen der Hauptschlüssel mit Bedacht vor. Der primäre und der sekundäre Hauptschlüssel stehen auf dem Blatt **Schlüssel** im [Azure-Portal][azure-portal] zur Verfügung. Weitere Informationen zu Schlüsseln finden Sie unter [Anzeigen, Kopieren und erneutes Generieren von Zugriffsschlüsseln](documentdb-manage-account.md#keys).

### Wie erstelle ich eine Datenbank?
Datenbanken können über das [Azure-Portal]() erstellt werden, wie in [Erstellen einer DocumentDB-Datenbank](documentdb-create-database.md) beschrieben, über eines der [DocumentDB-SDKs](documentdb-sdk-dotnet.md) oder über die [REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx).

### Was ist eine Sammlung?
Eine Sammlung ist ein Container für JSON-Dokumente und die zugehörige JavaScript-Anwendungslogik. Eine Sammlung ist eine fakturierbare Entität, deren [Kosten](documentdb-performance-levels.md) vom Durchsatz und belegten Speicher bestimmt werden. Sammlungen können eine/n oder mehrere Partitionen oder Server umfassen und skaliert werden, um praktisch unbegrenzte Mengen an Speicher oder Durchsatz zu verarbeiten.

Sammlungen sind zudem die Abrechnungseinheiten für DocumentDB. Die Kosten für jede Sammlung werden basierend auf dem bereitgestellten Durchsatz und dem verwendeten Speicherplatz pro Stunde berechnet. Weitere Informationen finden Sie unter [DocumentDB-Preise](https://azure.microsoft.com/pricing/details/documentdb/).

### Wie richte ich Benutzer und Berechtigungen ein?
Sie können Benutzer und Berechtigungen mit einem des [DocumentDB SDKs](documentdb-sdk-dotnet.md) oder mithilfe der [REST-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) erstellen.

## Datenbankfragen zum Entwickeln mit Microsoft Azure DocumentDB
### Wie beginne ich beim Entwickeln mit DocumentDB?
[SDKs](documentdb-sdk-dotnet.md) sind für .NET, Python, Node.js, JavaScript und Java verfügbar. Entwickler können außerdem die [RESTful HTTP-APIs](https://msdn.microsoft.com/library/azure/dn781481.aspx) zur Interaktion mit DocumentDB-Ressourcen auf einer Vielzahl von Plattformen und mit verschiedenen Sprachen nutzen.

Beispiele der [.NET](documentdb-dotnet-samples.md), [Java](https://github.com/Azure/azure-documentdb-java), [Node.js](documentdb-nodejs-samples.md) und [Python](documentdb-python-samples.md) SDKs für DocumentDB sind auf GitHub verfügbar.

### Unterstützt DocumentDB SQL?
Die DocumentDB-SQL-Abfragesprache ist eine erweiterte Teilmenge der Abfragefunktionalität, die von SQL unterstützt wird. Die DocumentDB SQL-Abfragesprache bietet umfassende hierarchische und relationale Operatoren sowie die Erweiterbarkeit über JavaScript-basierte benutzerdefinierte Funktionen. Die JSON-Grammatik ermöglicht die Modellierung von JSON-Dokumenten als Struktur mit den Beschriftungen als Strukturknoten. Dies wird sowohl von den automatischen Indizierungstechniken als auch beim SQL-Abfragedialekt von DocumentDB verwendet. Ausführliche Informationen zur Verwendung der SQL-Grammatik finden Sie im Artikel [Abfragen von DocumentDB][query].

### Welche Datentypen werden von DocumentDB unterstützt?
Die in DocumentDB unterstützten primitiven Datentypen sind dieselben wie in JSON. JSON hat ein einfaches Typsystem, das aus Zeichenfolgen, Zahlen (IEEE754 mit doppelter Genauigkeit) und booleschen Werten – "true", "false" und NULL-Werten – besteht. Komplexere Datentypen wie DateTime, Guid, Int64 und Geometrie können sowohl in JSON als auch in DocumentDB durch das Erstellen von verschachtelten Objekten mit dem { }-Operator sowie Arrays mit dem [ ]-Operator dargestellt werden.

### Wie stellt DocumentDB Parallelität zur Verfügung?
DocumentDB unterstützt die Steuerung für optimistische Parallelität (OCC) durch HTTP-Entitätstags bzw. ETags. Jede DocumentDB-Ressource verfügt über ein ETag. Das ETag wird auf dem Server festgelegt, immer wenn ein Dokument aktualisiert wird. Der ETag-Header und aktuelle Wert sind in allen Antwortnachrichten enthalten. ETags können mit dem „If-Match“-Header verwendet werden, um den Server entscheiden zu lassen, ob eine Ressource aktualisiert werden soll. Der „If-Match“-Wert ist der ETag-Wert, für den eine Überprüfung erfolgt. Wenn der ETag-Wert mit dem ETag-Wert des Servers übereinstimmt, wird die Ressource aktualisiert. Wenn das ETag nicht mehr aktuell ist, lehnt der Server den Vorgang mit dem Antwortcode „HTTP 412 – Vorbedingungsfehler“ ab. Der Client muss anschließend die Ressource erneut abrufen, um den aktuellen ETag-Wert für die Ressource zu erhalten. Darüber hinaus können ETags mit einem „If-None-Match“-Header verwendet werden, um festzustellen, ob ein erneutes Abrufen einer Ressource erforderlich ist.

Zum Nutzen der optimistischen Nebenläufigkeit in .NET verwenden Sie die [AccessCondition](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.accesscondition.aspx)-Klasse. Ein Beispiel für .NET finden Sie unter [Program.cs](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/DocumentManagement/Program.cs) im „DocumentManagement“-Beispiel auf GitHub.

### Wie führe ich Transaktionen in DocumentDB durch?
DocumentDB unterstützt sprachintegrierte Transaktionen über gespeicherte JavaScript-Prozeduren und Trigger. Alle Datenbankvorgänge in Skripts werden per Momentaufnahmeisolation ausgeführt, für die die Sammlung als Bereich gilt, wenn es sich um eine Sammlung mit einer Partition handelt. Es kann sich auch um Dokumente innerhalb desselben Partitionsschlüsselwerts handeln, wenn die Sammlung partitioniert ist. Eine Momentaufnahme der Dokumentversionen (ETags) wird zu Beginn der Transaktion angefertigt und erst dann festgeschrieben, wenn das Skript erfolgreich ausgeführt wurde. Falls JavaScript einen Fehler ausgibt, wird für die Transaktion ein Rollback ausgeführt. Weitere Einzelheiten finden Sie unter [Serverseitige DocumentDB-Programmierung](documentdb-programming.md).

### Wie kann ich Masseneinfügung für Dokumente in DocumentDB ausführen?
Es gibt drei Möglichkeiten zur Masseneinfügung von Dokumenten in DocumentDB:

* Das Datenmigrationstool, beschrieben in [Importieren von Daten in DocumentDB](documentdb-import-data.md).
* Den Dokument-Explorer im Azure-Portal, beschrieben in [Massenhinzufügen von Dokumenten mit dem Dokument-Explorer](documentdb-view-json-document-explorer.md#BulkAdd).
* Gespeicherte Prozeduren, beschrieben in [DocumentDB, serverseitige Programmierung](documentdb-programming.md).

### Unterstützt DocumentDB Ressourcenlink-Zwischenspeicherung?
Ja. Da es sich bei DocumentDB um einen RESTful-Dienst handelt, sind Ressourcenlinks unveränderbar und können zwischengespeichert werden. DocumentDB-Clients können einen „If-None-Match“-Header für Lesevorgänge für jede Ressource wie Dokumente oder Sammlungen festlegen und ihre lokalen Kopien nur dann aktualisieren, wenn sich die Serverversion geändert hat.

### Ist eine lokale Instanz von DocumentDB verfügbar?
Derzeit ist keine lokale Instanz von DocumentDB verfügbar. Sie können den Status eines lokalen Emulators nachverfolgen und dafür im [Feedbackforum](https://feedback.azure.com/forums/263030-documentdb/suggestions/6328798-standalone-local-instance) abstimmen.

[azure-portal]: https://portal.azure.com
[query]: documentdb-sql-query.md


<!---HONumber=AcomDC_0706_2016-->