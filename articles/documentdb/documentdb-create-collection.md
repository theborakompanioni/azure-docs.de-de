<properties 
	pageTitle="Erstellen einer DocumentDB-Datenbanksammlung | Microsoft Azure" 
	description="Erfahren Sie, wie Sie JSON-Dokumentsammlungen mit dem Onlinedienstportal für die cloudbasierte NoSQL-Dokumentendatenbank Azure DocumentDB erstellen. Holen Sie sich noch heute eine kostenlose Testversion." 
	services="documentdb" 
	authors="mimig1" 
	manager="jhubbard" 
	editor="monicar" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="08/15/2016" 
	ms.author="mimig"/>

# So erstellen Sie eine DocumentDB-Sammlung über das Azure-Portal

Für die Verwendung von Microsoft Azure DocumentDB benötigen Sie ein [DocumentDB-Konto](documentdb-create-account.md), eine [Datenbank](documentdb-create-database.md), eine Sammlung und Dokumente. In diesem Thema wird die Erstellung einer DocumentDB-Sammlung im Azure-Portal beschrieben.

Sie wissen nicht, was eine Sammlung ist? Dann lesen Sie den Abschnitt [Was ist eine DocumentDB-Sammlung?](#what-is-a-documentdb-collection).

1.  Klicken Sie im [Azure-Portal](https://portal.azure.com/) auf der Navigationsleiste auf **DocumentDB-Konten**, und wählen Sie anschließend auf dem Blatt **DocumentDB-Konten** das Konto aus, dem Sie eine Sammlung hinzufügen möchten.

    ![Screenshot mit „DocumentDB-Konten“ in der Navigationsleiste, mit dem Konto im Blatt „DocumentDB-Konten“ und mit der Datenbank im Blatt „DocumentDB-Konten“ im Fokus „Datenbanken“](./media/documentdb-create-collection/docdb-database-creation-1-2.png)
    
    Wenn **DocumentDB-Konten** nicht angezeigt wird, klicken Sie auf **Weitere Dienste** und dann auf **DocumentDB-Konten**. Wenn keine Konten aufgeführt sind, müssen Sie [ein DocumentDB-Konto erstellen](documentdb-create-account.md).

2. Wählen Sie auf dem Blatt **DocumentDB-Konto** für das ausgewählte Konto im Fokus **Datenbanken** die Datenbank aus, in der eine Sammlung hinzugefügt werden soll.

    ![Screenshot mit „DocumentDB-Konten“ in der Navigationsleiste, mit dem Konto im Blatt „DocumentDB-Konten“ und mit der Datenbank im Blatt „DocumentDB-Konten“ im Fokus „Datenbanken“](./media/documentdb-create-collection/docdb-database-creation-3.png)

3. Klicken Sie auf dem Blatt **Datenbank** auf **Sammlung hinzufügen**.

	![Screenshot mit Schaltfläche „Sammlung hinzufügen“ auf dem Blatt „Datenbank“, mit den Einstellungen im Blatt „Sammlung hinzufügen“ und Schaltfläche „OK“ – Azure-Portal für DocumentDB – Cloudbasierte Datenbankerstellung für NoSQL-JSON-Datenbanken](./media/documentdb-create-collection/docdb-collection-creation-4.png)

4. Geben Sie auf dem Blatt **Sammlung hinzufügen** im Feld **ID** die ID für Ihre neue Sammlung ein. Der Namen einer Sammlung muss zwischen 1 und 255 Zeichen lang sein und darf weder `/ \ # ?` noch nachstehende Leerzeichen enthalten. Wenn der Name überprüft wurde, wird im ID-Feld ein grünes Häkchen angezeigt.

	![Screenshot mit Schaltfläche „Sammlung hinzufügen“ auf dem Blatt „Datenbank“, mit den Einstellungen im Blatt „Sammlung hinzufügen“ und Schaltfläche „OK“ – Azure-Portal für DocumentDB – Cloudbasierte Datenbankerstellung für NoSQL-JSON-Datenbanken](./media/documentdb-create-collection/docdb-collection-creation-5-8.png)

5. Per Voreinstellung ist **Tarif** auf **Standard** festgelegt, damit Sie Durchsatz und Speicher für Ihre Sammlung anpassen können. Weitere Informationen zu den Tarifen finden Sie unter [Leistungsebenen in DocumentDB](documentdb-performance-levels.md).

6. Wählen Sie einen **Partitionierungsmodus** für die Sammlung aus, entweder **Einzelne Partition** oder **Partitioniert**.

    Eine **einzelne Partition** verfügt über eine reservierte Speicherkapazität von 10 GB und kann Durchsatzebenen von 400 bis 10.000 Anforderungseinheiten pro Sekunde (RU/Sek.) unterstützen. Eine Anforderungseinheit (Request Unit, RU) entspricht dem Durchsatz des Lesevorgangs eines Dokuments mit 1 KB. Weitere Informationen zu Anforderungseinheiten finden Sie unter [Anforderungseinheiten](documentdb-request-units.md).

    Eine **partitionierte Sammlung** kann skaliert werden, um eine unbegrenzte Menge an Speicher über mehrere Partitionen verteilt zu verarbeiten. Es werden Durchsatzebenen ab 10.100 Anforderungseinheiten pro Sekunde unterstützt. Im Portal kann eine Speichermenge von maximal 250 GB und ein Durchsatz von maximal 250.000 Anforderungseinheiten pro Sekunde reserviert werden. Um eines der beiden Kontingente zu erhöhen, müssen Sie eine Anfrage stellen, wie beschrieben unter [Anfordern von erhöhten DocumentDB-Kontolimits](documentdb-increase-limits.md). Weitere Informationen zu partitionierten Sammlungen finden Sie unter [Partitionieren und Skalieren von Daten in DocumentDB](documentdb-partition-data.md#single-partition-and-partitioned-collections).

    Wenn Sie den Tarif „Standard“ verwenden, können Sie jederzeit zu diesem Blatt wechseln und den Durchsatz für Ihre Sammlung ändern.

7. Wählen Sie den **Durchsatz** für die Sammlung aus. Informationen dazu, wie Sie den benötigten Durchsatz einschätzen können, finden Sie unter [Schätzen der Durchsatzanforderungen](documentdb-request-units.md#estimating-throughput-needs) und im [Capacity Planner](https://www.documentdb.com/capacityplanner).

8. Wenn Sie eine partitionierte Sammlung erstellen, wählen Sie den **Partitionsschlüssel** für die Sammlung. Die Auswahl des richtigen Partitionsschlüssels ist wichtig für die Erstellung einer leistungsfähigen Sammlung. Weitere Informationen zum Auswählen eines Partitionsschlüssels finden Sie unter [Entwerfen für Partitionierung](documentdb-partition-data.md#designing-for-partitioning).

9. Klicken Sie am unteren Bildschirmrand auf **OK**, um die neue Sammlung zu erstellen.

10. Die neue Sammlung wird jetzt im Fokus **Auflistungen** auf dem Blatt **Datenbank** angezeigt.
 
	![Screenshot mit der neuen Sammlung in Blatt „Datenbank“ – Azure-Portal für DocumentDB – Cloudbasierte Datenbankerstellung für NoSQL-JSON-Datenbanken](./media/documentdb-create-collection/docdb-collection-creation-9.png)

## Was ist eine DocumentDB-Sammlung? 

Eine Sammlung ist ein Container für JSON-Dokumente und die zugehörige JavaScript-Anwendungslogik. Eine Sammlung ist eine abrechnungsfähige Entität, bei der die [Kosten](documentdb-performance-levels.md) durch den bereitgestellten Durchsatz der Sammlung bestimmt werden. Sammlungen können eine/n oder mehrere Partitionen oder Server umfassen und können skaliert werden, um praktisch unbegrenzte Mengen an Speicher oder Durchsatz zu verarbeiten.

Sammlungen werden von DocumentDB automatisch in einen oder mehrere physische Server partitioniert. Wenn Sie eine Sammlung erstellen, können Sie den bereitgestellten Durchsatz in Form von Anforderungseinheiten pro Sekunde und einer Partitionsschlüsseleigenschaft angeben. Der Wert dieser Eigenschaft wird von DocumentDB zum Verteilen von Dokumenten zwischen Partitionen und zum Weiterleiten von Anforderungen wie Abfragen verwendet. Der Partitionsschlüssel fungiert außerdem als die Transaktionsgrenze für gespeicherte Prozeduren und Trigger. Jede Sammlung hat eine reservierte Durchsatzmenge, die für diese Sammlung spezifisch ist und die nicht mit anderen Sammlungen im gleichen Konto gemeinsam genutzt wird. Daher können Sie Ihre Anwendung sowohl im Hinblick auf Speicher als auch Durchsatz horizontal hochskalieren.

Sammlungen sind nicht identisch mit Tabellen in relationalen Datenbanken. Sammlungen erzwingen kein Schema. Als Datenbank erzwingt DocumentDB grundsätzlich keine Schemas, es handelt sich um eine schemafreie Datenbank. Daher können Sie unterschiedliche Arten von Dokumenten mit unterschiedlichen Schemas in derselben Sammlung speichern. Falls gewünscht, können Sie Sammlungen auch verwenden, um wie bei Tabellen Objekte eines einzelnen Typs zu speichern. Das geeignetste Modell ist nur davon abhängig, wie die Daten zusammen in Abfragen und Transaktionen angezeigt werden.

## Weitere Methoden zum Erstellen einer DocumentDB-Sammlung

Sammlungen müssen nicht über das Portal erstellt werden, sondern können auch mithilfe der [DocumentDB SDKs](documentdb-sdk-dotnet.md) und der REST-API erstellt werden.

- Ein C#-Codebeispiel finden Sie in den [C#-Sammlungsbeispielen](documentdb-dotnet-samples.md#collection-examples).
- Ein Node.js-Codebeispiel finden Sie in den [Node.js-Sammlungsbeispielen](documentdb-nodejs-samples.md#collection-examples).
- Ein Python-Codebeispiel finden Sie in den [Python-Sammlungsbeispielen](documentdb-python-samples.md#collection-examples).
- Ein REST-API-Beispiel finden Sie unter [Erstellen einer Sammlung](https://msdn.microsoft.com/library/azure/mt489078.aspx).

## Problembehandlung

Wenn **Sammlung hinzufügen** im Azure-Portal deaktiviert ist, bedeutet das, dass Ihr Konto derzeit deaktiviert ist. Das passiert normalerweise, wenn alle Leistungsguthaben für den Monat aufgebraucht sind.

## Nächste Schritte

Nachdem Sie nun über eine Sammlung verfügen, besteht der nächste Schritt darin, Dokumente zur Sammlung hinzufügen oder in die Sammlung zu importieren. Zum Hinzufügen von Dokumenten zu einer Sammlung stehen Ihnen verschiedene Möglichkeiten zur Verfügung:

- Sie können [Dokumente hinzufügen](documentdb-view-json-document-explorer.md), indem Sie den Dokument-Explorer im Portal verwenden.
- Sie können [Dokumente und Daten importieren](documentdb-import-data.md), indem Sie das Datenmigrationstool für DocumentDB verwenden, mit dem Sie JSON- und CSV-Dateien importieren können. Sie können auch Daten aus SQL Server, MongoDB, dem Azure-Tabellenspeicher und anderen DocumentDB-Sammlungen importieren.
- Sie können Dokumente auch mithilfe eines der [DocumentDB-SDKs](documentdb-sdk-dotnet.md) hinzufügen. DocumentDB verfügt über SDKs für .NET, Java, Python, Node.js und JavaScript-API. C#-Codebeispiele, die die Verwendung von Dokumenten mithilfe des .NET SDK für DocumentDB veranschaulichen, finden Sie unter [C#-Dokumentbeispiele](documentdb-dotnet-samples.md#document-examples). Node.js-Codebeispiele, die die Verwendung von Dokumenten mithilfe des Node.js SDK für DocumentDB veranschaulichen, finden Sie unter [Node.js-Dokumentbeispiele](documentdb-nodejs-samples.md#document-examples).

Wenn eine Sammlung Dokumente enthält, können Sie in [DocumentDB SQL](documentdb-sql-query.md) an den Dokumenten [Abfragen ausführen](documentdb-sql-query.md#executing-queries), indem Sie den [Abfrage-Explorer](documentdb-query-collections-query-explorer.md) im Portal, die [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) oder eines der [SDKs](documentdb-sdk-dotnet.md) verwenden.

<!---HONumber=AcomDC_0817_2016-->