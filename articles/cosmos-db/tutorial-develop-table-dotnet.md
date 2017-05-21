---
title: 'Azure Cosmos DB: Entwickeln mit der Tabellen-API in .NET | Microsoft-Dokumentation'
description: "Erfahren Sie, wie Sie mit der Tabellen-API von Azure Cosmos DB unter Verwendung von .NET entwickeln können."
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 4b22cb49-8ea2-483d-bc95-1172cd009498
ms.service: cosmosdb
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: fff65034ea725fced95a291a8f206b993e8404a4
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-develop-with-the-table-api-in-net"></a>Azure Cosmos DB: Entwickeln mit der Tabellen-API in .NET

Azure Cosmos DB ist ein global verteilter Datenbankdienst von Microsoft mit mehreren Modellen. Sie können schnell Dokument-, Schlüssel/Wert- und Graph-Datenbanken erstellen und abfragen und dabei stets von den Vorteilen der globalen Verteilung und der horizontalen Skalierung profitieren, die Azure Cosmos DB zugrunde liegen.

Dieses Tutorial enthält die folgenden Aufgaben: 

> [!div class="checklist"] 
> * Erstellen eines Azure Cosmos DB-Kontos 
> * Aktivieren der Funktionalität in der Datei „app.config“ 
> * Erstellen einer Tabelle mithilfe der [Tabellen-API](table-introduction.md) (Vorschau)
> * Hinzufügen einer Entität zu einer Tabelle 
> * Einfügen eines Entitätsbatchs 
> * Abrufen einer einzelnen Entität 
> * Abfragen von Entitäten mit automatischen sekundären Indizes 
> * Ersetzen einer Entität 
> * Löschen einer Entität 
> * Löschen einer Tabelle
 
## <a name="tables-in-azure-cosmos-db"></a>Tabellen in Azure Cosmos DB 

Azure Cosmos DB bietet die [Tabellen-API](table-introduction.md) (Vorschau) für Anwendungen, die einen Schlüssel-Wert-Speicher mit schemafreiem Design benötigen. Die SDKs und REST-APIs von [Azure Table Storage](../storage/storage-introduction.md) können zum Arbeiten mit Azure Cosmos DB verwendet werden. Mit Azure Cosmos DB können Sie Tabellen mit hohen Durchsatzanforderungen erstellen. Azure Cosmos DB unterstützt durchsatzoptimierte Tabellen (informell als „Premium Tables“ bezeichnet), die derzeit in der öffentlichen Vorschau verfügbar sind. 

Sie können Azure Table Storage weiterhin für Tabellen mit hohen Speicheranforderungen und niedrigeren Durchsatzanforderungen verwenden. In einem zukünftigen Update von Azure Cosmos DB wird die Unterstützung speicheroptimierter Tabellen eingeführt, und für vorhandene und neue Azure Table Storage-Konten wird ein nahtloses Upgrade auf Azure Cosmos DB durchgeführt.

Wenn Sie derzeit Azure Table Storage verwenden, bietet Ihnen der Wechsel zur „Premium Table“-Vorschau folgende Vorteile:

- Sofort einsetzbare [globale Verteilung](../documentdb/documentdb-distribute-data-globally.md) mit Multihosting und [automatischen und manuellen Failovers](../documentdb/documentdb-regional-failovers.md)
- Unterstützung für automatische schemaunabhängige Indizierung aller Eigenschaften („sekundäre Indizes“) und schnelle Abfragen 
- Unterstützung für eine beliebige Anzahl von Regionen übergreifende [unabhängige Skalierung von Speicher und Durchsatz](partition-data.md)
- Unterstützung für [dedizierten Durchsatz pro Tabelle](../documentdb/documentdb-request-units.md), der von Hundertern auf Millionen Anforderungen pro Sekunde skaliert werden kann
- Unterstützung für [fünf optimierbare Konsistenzebenen](../documentdb/documentdb-consistency-levels.md) zur Abstimmung von Verfügbarkeit, Latenz und Konsistenz basierend auf Ihren Anwendungsanforderungen
- 99,99 % Verfügbarkeit innerhalb einer einzelnen Region sowie die Möglichkeit zum Hinzufügen weiterer Regionen für höhere Verfügbarkeit und [branchenführende umfassende SLAs](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) zu allgemeiner Verfügbarkeit
- Arbeiten mit dem vorhandenen Azure Storage-.NET-SDK und keine Änderungen am Code der Anwendung

Während der Vorschau unterstützt Azure Cosmos DB die Tabellen-API mit dem .NET-SDK. Sie können das [Azure Storage Preview-SDK](https://aka.ms/premiumtablenuget) von NuGet herunterladen. Das SDK hat die gleichen Klassen- und Methodensignaturen wie das [Azure Storage-SDK](https://www.nuget.org/packages/WindowsAzure.Storage), kann aber auch mithilfe der Tabellen-API eine Verbindung mit Konten von Azure Cosmos DB herstellen.

Weitere Informationen zu komplexen Azure Table Storage-Aufgaben finden Sie unter:

* [Introduction to Azure Cosmos DB: Table API](table-introduction.md) (Einführung in Azure Cosmos DB: Tabellen-API)
* Vollständige Informationen zu verfügbaren APIs finden Sie in der Referenzdokumentation zum Tabellendienst unter [WindowsAzure.Storage API Reference](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409) (API-Referenz zu WindowsAzure.Storage).

### <a name="about-this-tutorial"></a>Informationen zu diesem Lernprogramm
Dieses Tutorial wendet sich an Entwickler, die mit dem Azure Table Storage-SDK vertraut sind und die verfügbaren Premium-Features mit Azure Cosmos DB verwenden möchten. Es basiert auf [Erste Schritte mit Azure Table Storage mit .NET](../storage/storage-dotnet-how-to-use-tables.md) und zeigt, wie zusätzliche Funktionen wie sekundäre Indizes, bereitgestellter Durchsatz und Multihosting genutzt werden können. Wir behandeln die Verwendung des Azure-Portals zum Erstellen eines Azure Cosmos DB-Kontos, erstellen anschließend eine Tabellenanwendung und stellen sie bereit. Wir zeigen Ihnen auch .NET-Beispiele zum Erstellen und Löschen einer Tabelle sowie zum Einfügen, Aktualisieren, Löschen und Abfragen von Tabellendaten. 

Falls Sie Visual Studio 2017 noch nicht installiert haben, können Sie die **kostenlose** [Visual Studio 2017 Community-Edition](https://www.visualstudio.com/downloads/) herunterladen und verwenden. Aktivieren Sie beim Setup von Visual Studio die Option **Azure-Entwicklung**.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Erstellen eines Datenbankkontos

Zunächst erstellen wir ein Azure Cosmos DB-Konto im Azure-Portal.  

> [!TIP]
> * Besitzen Sie bereits ein Azure Cosmos DB-Konto? Wenn dies der Fall ist, fahren Sie mit [Einrichten Ihrer Visual Studio-Projektmappe](#SetupVS) fort.
> * Besaßen Sie einmal ein Azure DocumentDB-Konto? Wenn dies der Fall ist, ist Ihr Konto jetzt ein Azure Cosmos DB-Konto, und Sie können mit [Einrichten Ihrer Visual Studio-Projektmappe](#SetupVS) fortfahren.  
> * Wenn Sie den Azure Cosmos DB-Emulator verwenden, führen Sie bitte die Schritte unter [Azure Cosmos DB-Emulator](../documentdb/documentdb-nosql-local-emulator.md) zum Einrichten des Emulators aus, und fahren Sie dann mit [Einrichten Ihrer Visual Studio-Projektmappe](#SetupVS) fort. 
>
>

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmosdb-create-dbaccount-table.md)] 

## <a name="clone-the-sample-application"></a>Klonen der Beispielanwendung

Klonen Sie jetzt eine Tabellen-App aus GitHub, legen Sie die Verbindungszeichenfolge fest und führen Sie diese aus.

1. Öffnen Sie ein Git-Terminalfenster, z.B. ein Git Bash, und `cd` in einem Arbeitsverzeichnis.  

2. Führen Sie den folgenden Befehl aus, um das Beispielrepository zu klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started
    ```

3. Öffnen Sie anschließend die Projektmappendatei in Visual Studio.

## <a name="update-your-connection-string"></a>Aktualisieren der Verbindungszeichenfolge

Wechseln Sie nun zurück zum Azure-Portal, um die Informationen der Verbindungszeichenfolge abzurufen und in die App zu kopieren.

1. Klicken Sie im [Azure-Portal](http://portal.azure.com/) in Ihrem Azure Cosmos DB-Konto im linken Navigationsbereich auf **Schlüssel**, und klicken Sie anschließend auf **Lese-/Schreibschlüssel**. Kopieren Sie im nächsten Schritt mithilfe der Schaltflächen zum Kopieren auf der rechten Seite des Bildschirms die Verbindungszeichenfolge in die Datei „app.config“.

2. Öffnen Sie in Visual Studio die Datei „app.config“. 

3. Kopieren Sie den URI-Wert aus dem Portal (mithilfe der Schaltfläche zum Kopieren), und legen Sie ihn in „app.config“ als Wert von „account-key“ fest. Verwenden Sie den zuvor erstellten Kontonamen in „app.config“ für „account-name“.
  
```
<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;TableEndpoint=https://account-name.documents.azure.com" />
```

> [!NOTE]
> Um diese App mit standardmäßigem Azure Table Storage verwenden zu können, müssen Sie die Verbindungszeichenfolge in `app.config file` ändern. Verwenden Sie den Kontonamen als Tabellenkontonamen und den Schlüssel als Azure Storage-Primärschlüssel. <br>
>`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key;EndpointSuffix=core.windows.net" />`
> 
>

## <a name="build-and-deploy-the-app"></a>Erstellen und Bereitstellen der App
1. Klicken Sie in Visual Studio mit der rechten Maustaste auf das Projekt im **Projektmappen-Explorer**, und klicken Sie anschließend auf **NuGet-Pakete verwalten**. 

2. Geben Sie im NuGet-Feld **Durchsuchen** ***WindowsAzure.Storage PremiumTable*** ein. Aktivieren Sie **Vorabversion einbeziehen**.

3. Installieren Sie aus den Ergebnissen **WindowsAzure.Storage PremiumTable**, und wählen Sie die Vorabversion `0.0.1-preview`. Mit dieser Aktion werden das Azure Table Storage-Paket und alle Abhängigkeiten installiert.

4. Drücken Sie STRG + F5, um die Anwendung auszuführen. 

Jetzt können Sie zum Daten-Explorer zurückkehren, um diese Tabellendaten anzuzeigen, abzufragen, anzupassen und mit ihnen zu arbeiten. 

> [!NOTE]
> Um diese App mit einem Azure Cosmos DB-Emulator verwenden zu können, müssen Sie nur die Verbindungszeichenfolge in `app.config file` ändern. Verwenden Sie den folgenden Wert für den Emulator. <br>
>`<add key="StorageConnectionString" value=DefaultEndpointsProtocol=https;AccountName=localhost;AccountKey=<insertkey>==;TableEndpoint=https://localhost -->`
> 
>

## <a name="azure-cosmos-db-capabilities"></a>Azure Cosmos DB-Funktionen
Azure Cosmos DB unterstützt eine Reihe von Funktionen, die nicht in der Azure Table Storage-API verfügbar sind. Die neue Funktionalität kann über die folgenden `appSettings`-Konfigurationswerte aktiviert werden. Wir haben in der Vorschauversion des Azure Storage-SDK keine neuen Signaturen oder Überladungen eingeführt. So können Sie sowohl mit Standard- als auch Premium-Tabellen Verbindungen herstellen und mit anderen Azure Storage-Diensten wie Blobs und Warteschlangen arbeiten. 


| Schlüssel | Beschreibung |
| --- | --- |
| TableConnectionMode  | Azure Cosmos DB unterstützt zwei Konnektivitätsmodi. Im `Gateway`-Modus werden Anforderungen immer an das Azure Cosmos DB-Gateway gerichtet, das sie an die entsprechenden Datenpartitionen weiterleitet. Im `Direct`-Konnektivitätsmodus ruft der Client die Zuordnung von Tabellen zu Partitionen ab, und Anforderungen werden direkt an Datenpartitionen gerichtet. `Direct` (Standard) wird empfohlen.  |
| TableConnectionProtocol | Azure Cosmos DB unterstützt zwei Verbindungsprotokolle – `Https` und `Tcp`. `Tcp` ist der Standard und wird empfohlen, da es einfacher ist. |
| TablePreferredLocations | Durch Trennzeichen getrennte Liste der bevorzugten Standorte (Multihosting) für Lesevorgänge. Jedem Azure Cosmos DB-Konto können 1-30+ Regionen zugeordnet werden. Jede Clientinstanz kann eine Teilmenge dieser Regionen in der bevorzugten Reihenfolge für Lesevorgänge mit geringer Latenz angeben. Die Regionen müssen mit ihren [Anzeigenamen](https://msdn.microsoft.com/library/azure/gg441293.aspx) benannt sein, z.B. `West US`. Siehe auch [How to setup Azure Cosmos DB global distribution using the Table API](tutorial-global-distribution-table.md) (Einrichten der globalen Verteilung für Azure Cosmos DB mit der Tabellen-API).
| TableConsistencyLevel | Um Latenz, Konsistenz und Verfügbarkeit aufeinander abzustimmen, haben Sie fünf klar definierte Konsistenzebenen zur Auswahl: `Strong`, `Session`, `Bounded-Staleness`, `ConsistentPrefix` und `Eventual`. Der Standardwert ist `Session`. Die Auswahl der Konsistenzebene sorgt in Setups mit mehreren Regionen für einen deutlichen Leistungsunterschied. Weitere Informationen finden Sie unter [Einstellbare Datenkonsistenzebenen in DocumentDB](../documentdb/documentdb-consistency-levels.md). |
| TableThroughput | Reservierter Durchsatz für die Tabelle, ausgedrückt in Anforderungseinheiten (RU) pro Sekunde. Einzelne Tabellen können Hunderte Millionen von RU/s unterstützen. Weitere Informationen finden Sie unter [Anforderungseinheiten in DocumentDB](../documentdb/documentdb-request-units.md). Der Standardwert ist `400`. |
| TableIndexingPolicy | Konsistente und automatische sekundäre Indizierung aller Spalten in Tabellen | Der Spezifikation der Indizierungsrichtlinie entsprechende JSON-Zeichenfolge. Unter [Indizierungsrichtlinien für DocumentDB](../documentdb/documentdb-indexing-policies.md) erfahren Sie, wie Sie die Indizierungsrichtlinie ändern können, um bestimmte Spalten ein- bzw. auszuschließen. | Automatische Indizierung aller Eigenschaften (Hashwert für Zeichenfolgen und Bereich für Zahlen) |
| TableQueryMaxItemCount | Konfigurieren Sie die maximale Anzahl von Elementen, die in einem einzelnen Roundtrip pro Tabellenabfrage zurückgegeben werden. Der Standardwert ist `-1`, sodass Azure Cosmos DB den Wert zur Laufzeit dynamisch bestimmen kann. |
| TableQueryEnableScan | Wenn die Abfrage den Index für keinen Filter verwenden kann, führen Sie sie dennoch über eine Überprüfung aus. Der Standardwert ist `false`.|
| TableQueryMaxDegreeOfParallelism | Der Grad der Parallelität für die Ausführung einer partitionsübergreifenden Abfrage. `0` ist seriell ohne Vorabruf, `1` ist seriell mit Vorabruf, und höhere Werte steigern den Grad der Parallelität. Der Standardwert ist `-1`, sodass Azure Cosmos DB den Wert zur Laufzeit dynamisch bestimmen kann. |

Um den Standardwert zu ändern, öffnen Sie die Datei `app.config` in Visual Studio im Projektmappen-Explorer. Fügen Sie den Inhalt des `<appSettings>` -Elements hinzu (wie unten dargestellt). Ersetzen Sie `account-name` durch den Namen Ihres Speicherkontos und `account-key` durch den Zugriffsschlüssel des Kontos. 

```xml
<configuration>
    <startup> 
        <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
    </startup>
    <appSettings>
      <!-- Client options -->
      <add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key; TableEndpoint=https://account-name.documents.azure.com" />
      <add key="TableConnectionMode" value="Direct"/>
      <add key="TableConnectionProtocol" value="Tcp"/>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>
      <add key="TableConsistencyLevel" value="Eventual"/>

      <!--Table creation options -->
      <add key="TableThroughput" value="700"/>
      <add key="TableIndexingPolicy" value="{""indexingMode"": ""Consistent""}">

      <!-- Table query options -->
      <add key="TableQueryMaxItemCount" value="-1"/>
      <add key="TableQueryEnableScan" value="false"/>
      <add key="TableQueryMaxDegreeOfParallelism" value="-1"/>
      <add key="TableQueryContinuationTokenLimitInKb" value="16"/>
            
    </appSettings>
</configuration>
```

Es folgt ein kurzer Überblick zu dem, was in der App geschieht. Öffnen Sie die Datei `Program.cs`. Sie stellen fest, dass mit diesen Codezeilen die Tabellenressourcen erstellt werden. 

## <a name="create-the-table-client"></a>Erstellen des Tabellenclients
Sie initialisieren einen `CloudTableClient`, um eine Verbindung mit dem Tabellenkonto herzustellen.

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```
Dieser Client wird mithilfe der Konfigurationswerte `TableConnectionMode`, `TableConnectionProtocol`, `TableConsistencyLevel` und `TablePreferredLocations` initialisiert, sofern in den App-Einstellungen angegeben.
    
## <a name="create-a-table"></a>Erstellen einer Tabelle
Dann erstellen Sie eine Tabelle mit `CloudTable`. Tabellen können in Azure Cosmos DB unabhängig voneinander im Hinblick auf Speicher und Durchsatz skaliert werden, und die Partitionierung wird automatisch vom Dienst durchgeführt. Azure Cosmos DB unterstützt sowohl Tabellen mit fester Größe als auch unbegrenzte Tabellen. Weitere Informationen finden Sie unter [How to partition and scale in Azure Cosmos DB](partition-data.md) (Partitionieren und Skalieren in Azure Cosmos DB). 

```csharp
CloudTable table = tableClient.GetTableReference("people");

table.CreateIfNotExists();
```

Bei den Methoden zum Erstellen von Tabellen gibt es einen wichtigen Unterschied. Im Gegensatz zu dem verbrauchsbasierten Modell für Transaktionen von Azure Storage reserviert Azure Cosmos DB Durchsatz. Das Reservierungsmodell hat zwei wichtige Vorteile:

* Ihr Durchsatz ist dediziert/reserviert, damit Sie nie gedrosselt werden, wenn Ihre Anforderungsrate höchstens dem bereitgestellten Durchsatz entspricht.
* Das Reservierungsmodell ist [kostengünstiger für Workloads mit hohem Durchsatz](../documentdb/documentdb-key-value-store-cost.md).

Sie können den Standarddurchsatz konfigurieren, indem Sie die Einstellung für `TableThroughput` im Hinblick auf RUs (Anforderungseinheiten) pro Sekunde konfigurieren. 

Ein Lesevorgang für eine 1-KB-Entität ist als 1 RU normalisiert, und andere Vorgänge sind basierend auf ihrer CPU-, Arbeitsspeicher- und IOPS-Auslastung auf einen festen RU-Wert normalisiert. Weitere Informationen finden Sie unter [Anforderungseinheiten in DocumentDB](../documentdb/documentdb-request-units.md).

> [!NOTE]
> Das Table Storage-SDK unterstützt das Ändern des Durchsatzes derzeit zwar nicht, doch Sie können den Durchsatz sofort jederzeit mittels Azure-Portal oder Azure CLI ändern.

Als Nächstes betrachten wir die einfachen Lese- und Schreibvorgänge (CRUD) mit dem Azure Table Storage-SDK. Dieses Tutorial zeigt vorhersehbare niedrige Latenzen im einstelligen Millisekundenbereich und schnelle Abfragen mit Azure Cosmos DB.

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle
In Azure Table Storage sind Entitäten Erweiterungen der `TableEntity`-Klasse und müssen über die Eigenschaften `PartitionKey` und `RowKey` verfügen. Hier ist eine Beispieldefinition für eine Kundenentität.

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

Der folgende Codeausschnitt zeigt, wie eine Entität mit dem Azure Storage-SDK eingefügt wird. Azure Cosmos DB ist für eine weltweit garantierte geringe Latenz bei jeder Skalierung ausgelegt.

Schreibvorgänge werden für Anwendungen, die in der gleichen Region ausgeführt werden, in der sich auch das Azure Cosmos DB-Konto befindet, mit <15ms bei p99 und ~6ms bei p50 abgeschlossen. Diese Dauer ist darauf zurückzuführen, dass Schreibvorgänge dem Client erst dann bestätigt werden, nachdem sie synchron repliziert, dauerhaft festgeschrieben und alle Inhalte indiziert sind.

Die Tabellen-API für Azure Cosmos DB ist in der Vorschau. Bei allgemeiner Verfügbarkeit werden die p99-Latenzgarantien wie andere Azure Cosmos DB-APIs durch SLAs unterstützt. 

```csharp
// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a>Einfügen eines Entitätsbatchs
Azure Table Storage unterstützt eine Batchvorgang-API, mit der Sie Updates, Löschungen und Einfügungen in einem einzelnen Batchvorgang kombinieren können. Manche Einschränkungen der Batch-API von Azure Table Storage treffen auf Azure Cosmos DB nicht zu. Beispielsweise können Sie innerhalb eines Batches mehrere Lesevorgänge ausführen, mehrere Schreibvorgänge auf der gleichen Entität innerhalb eines Batches ausführen, und es gibt keine Einschränkung auf 100 Vorgänge pro Batch. 

```csharp
// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```
## <a name="retrieve-a-single-entity"></a>Abrufen einer einzelnen Entität
Abrufe (GETs) in der gleichen Azure-Region werden in Azure Cosmos DB in <10ms bei p99 und ~ 1ms bei p50 ausgeführt. Sie können Ihrem Konto beliebig viele Regionen für Lesevorgänge mit geringer Latenz hinzufügen und Anwendungen zum Lesen aus ihrer Region bereitstellen („mehrfach vernetzt“), indem Sie `TablePreferredLocations` festlegen. 

Mit folgendem Codeausschnitt können Sie eine einzelne Entität abrufen:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
> [!TIP]
> Weitere Informationen über Multihosting-APIs finden Sie unter [How to setup Azure Cosmos DB global distribution using the Table API](tutorial-global-distribution-table.md) (Einrichten der globalen Verteilung für Azure Cosmos DB mit der Tabellen-API).
>

## <a name="query-entities-using-automatic-secondary-indexes"></a>Abfragen von Entitäten mit automatischen sekundären Indizes
Tabellen können mithilfe der `TableQuery`-Klasse abgefragt werden. Azure Cosmos DB besitzt ein schreiboptimiertes Datenbankmodul, das alle Spalten in der Tabelle automatisch indiziert. Indizierung ist in Azure Cosmos DB unabhängig von Schemas. Aus diesem Grund wird Ihr Schema auch dann automatisch indiziert, wenn es von Zeile zu Zeile unterschiedlich ist oder sich im Laufe der Zeit weiterentwickelt. Da Azure Cosmos DB automatische sekundäre Indizes unterstützt, können Abfragen von Eigenschaften den Index nutzen und effizient verarbeitet werden.

```csharp
CloudTable table = tableClient.GetTableReference("people");

// Filter against a property that's not partition key or row key
TableQuery<CustomerEntity> emailQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.GenerateFilterCondition("Email", QueryComparisons.Equal, "Ben@contoso.com"));

foreach (CustomerEntity entity in table.ExecuteQuery(emailQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

In der Vorschau unterstützt Azure Cosmos DB die gleiche Abfragefunktionalität wie Azure Table Storage für die Tabellen-API. Azure Cosmos DB unterstützt auch Sortieren, Aggregieren, räumliche Abfrage, Hierarchie und eine Vielzahl von integrierten Funktionen. Die zusätzliche Funktionalität wird in einem zukünftigen Dienstupdate in der Tabellen-API bereitgestellt. Unter [SQL-Abfrage und SQL-Syntax in DocumentDB](../documentdb/documentdb-sql-query.md) finden Sie eine Übersicht dieser Funktionen. 

## <a name="replace-an-entity"></a>Ersetzen einer Entität
Um eine Entität zu aktualisieren, rufen Sie sie aus dem Tabellendienst ab, ändern Sie das Entitätsobjekt, und speichern Sie die Änderungen dann im Tabellendienst. Mit dem folgenden Code wird die Telefonnummer eines vorhandenen Kunden geändert. 

```csharp
TableOperation updateOperation = TableOperation.Replace(updateEntity);
table.Execute(updateOperation);
```
Auf ähnliche Weise können Sie die Vorgänge `InsertOrMerge` oder `Merge` ausführen.  

## <a name="delete-an-entity"></a>Löschen einer Entität
Eine Entität kann nach dem Abrufen problemlos gelöscht werden. Verwenden Sie dazu das gleiche Muster wie beim Aktualisieren einer Entität. Durch den nachstehenden Code wird eine Kundenentität aufgerufen und gelöscht.

```csharp
TableOperation deleteOperation = TableOperation.Delete(deleteEntity);
table.Execute(deleteOperation);
```

## <a name="delete-a-table"></a>Löschen einer Tabelle
Schließlich wird mit dem folgenden Codebeispiel eine Tabelle aus einem Speicherkonto gelöscht. Mit Azure Cosmos DB können Sie eine Tabelle löschen und sofort neu erstellen.

```csharp
CloudTable table = tableClient.GetTableReference("people");
table.DeleteIfExists();
```

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen 

Wenn Sie diese App nicht weiterhin verwenden, löschen Sie mit den folgenden Schritten im Azure-Portal sämtliche Ressourcen, die mit diesem Tutorial erstellt wurden.   

1. Klicken Sie im Azure-Portal im Menü auf der linken Seite auf **Ressourcengruppen**, und klicken Sie auf den Namen der erstellten Ressource.  
2. Klicken Sie auf der Seite mit Ihrer Ressourcengruppe auf **Löschen**, geben Sie im Textfeld den Namen der zu löschenden Ressource ein, und klicken Sie dann auf **Löschen**. 

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben wir die ersten Schritte mit der Tabellen-API in Azure Cosmos DB behandelt, und Sie haben die folgenden Aufgaben ausgeführt: 

> [!div class="checklist"] 
> * Erstellen eines Azure Cosmos DB-Kontos 
> * Aktivieren der Funktionalität in der Datei „app.config“ 
> * Erstellen einer Tabelle 
> * Hinzufügen einer Entität zu einer Tabelle 
> * Einfügen eines Entitätsbatches 
> * Abrufen einer einzelnen Entität 
> * Abfragen von Entitäten mit automatischen sekundären Indizes 
> * Ersetzen einer Entität 
> * Löschen einer Entität 
> * Löschen einer Tabelle  

Sie können jetzt mit dem nächsten Tutorial fortfahren und mehr über das Abfragen von Tabellendaten erfahren. 

> [!div class="nextstepaction"]
> [Abfragen mit der Tabellen-API](tutorial-query-table.md)

