---
title: Erste Schritte mit Table Storage und verbundenen Visual Studio-Diensten (ASP.NET Core) | Microsoft-Dokumentation
description: Machen Sie sich mit den ersten Schritten mit Azure Table Storage in einem ASP.NET Core-Projekt in Visual Studio vertraut, nachdem Sie mithilfe von verbundenen Visual Studio-Diensten eine Verbindung mit einem Speicherkonto hergestellt haben.
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 63f208c091b0fc5ce569460c133d84eb68b8f362
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Erste Schritte mit Azure-Tabellenspeicher und verbundenen Visual Studio-Diensten
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht
Dieser Artikel beschreibt, wie die ersten Schritte beim Verwenden von Azure Table Storage in Visual Studio aussehen, nachdem Sie über das Visual Studio-Dialogfeld **Verbundene Dienste hinzufügen** in einem ASP.NET Core-Projekt ein Azure Storage-Konto erstellt oder auf ein solches Konto verwiesen haben.

Der Azure-Tabellenspeicherdienst ermöglicht das Speichern großer Mengen von strukturierten Daten. Der Dienst ist ein NoSQL-Datenspeicher, der authentifizierte Aufrufe von innerhalb und außerhalb der Azure-Cloud akzeptiert. Azure-Tabellen sind hervorragend zur Speicherung strukturierter nicht relationaler Daten geeignet.

Beim Vorgang **Verbundene Dienste hinzufügen** werden die entsprechenden NuGet-Pakete installiert, um auf Azure-Speicher in Ihrem Projekt zuzugreifen, und Ihren Projektkonfigurationsdateien die Verbindungszeichenfolge für das Speicherkonto hinzugefügt.

Weitere allgemeine Informationen zur Verwendung von Azure Table Storage finden Sie unter [Erste Schritte mit Azure Table Storage mit .NET](../storage/storage-dotnet-how-to-use-tables.md).

Damit Sie beginnen können, müssen Sie zuerst eine Tabelle in Ihrem Speicherkonto erstellen. Wir zeigen Ihnen, wie Sie eine Azure-Tabelle in Code erstellen. Außerdem wird gezeigt, wie Sie grundlegende Tabellen- und Entitätsvorgänge, etwa Hinzufügen, Ändern und Lesen von Tabellenentitäten, ausführen. Die Beispiele sind in C\# geschrieben und greifen auf die Azure Storage-Clientbibliothek für .NET zurück.

**HINWEIS**: Einige der APIs, die Aufrufe zum Azure Storage in ASP.NET Core ausführen, arbeiten asynchron. Unter [Asynchrone Programmierung mit Async und Await](http://msdn.microsoft.com/library/hh191443.aspx) finden Sie weitere Informationen. Im nachstehenden Code wird die Programmierung mit Async-Methoden angenommen.

## <a name="access-tables-in-code"></a>Zugreifen auf Tabellen im Code
Um auf Tabellen in ASP.NET Core-Projekten zuzugreifen, müssen Sie jeder C#-Quelldatei, in der auf Azure Table Storage zugegriffen wird, die folgenden Elemente hinzufügen.

1. Vergewissern Sie sich, dass die Namespacedeklarationen am Anfang der C#-Datei diese **using** -Anweisungen enthalten.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um Ihre Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    **HINWEIS** : Verwenden Sie den gesamten obigen Code vor dem Code in den folgenden Beispielen.
3. Rufen Sie ein **CloudTableClient** -Objekt ab, um auf die Tabellenobjekte in Ihrem Speicherkonto zu verweisen.  
   
        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Rufen Sie ein **CloudTable** -Verweisobjekt ab, um auf eine bestimmte Tabelle und auf bestimmte Entitäten zu verweisen.
   
        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Erstellen einer Tabelle in Code
Fügen Sie zum Erstellen der Azure-Tabelle einfach einen Aufruf von **CreateIfNotExistsAsync()**hinzu.

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle
Sie erstellen eine Klasse, mit der die Eigenschaften der Entität definiert werden, um einer Tabelle eine Entität hinzuzufügen. Der folgenden Code definiert eine Entitätsklasse namens **CustomerEntity** , die den Vornamen des Kunden als Zeilenschlüssel und den Nachnamen als Partitionsschlüssel verwendet.

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

Tabellenvorgänge, die Entitäten betreffen, werden mit dem **CloudTable** -Objekt ausgeführt, das Sie zuvor unter "Zugreifen auf Tabellen in Code" erstellt haben. Das **TableOperation** -Objekt stellt den auszuführenden Vorgang dar. Das folgende Codebeispiel zeigt das Erstellen eines **CloudTable**-Objekts und eines **CustomerEntity**-Objekts. Um den Vorgang vorzubereiten, wird eine **TableOperation** zum Einfügen der Kundenentität in die Tabelle erstellt. Schließlich wird der Vorgang durch einen Aufruf von CloudTable.ExecuteAsync ausgeführt.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Einfügen eines Entitätsbatchs
Sie können mehrere Entitäten mit einem Schreibvorgang in eine Tabelle einfügen. Das folgende Codebeispiel erstellt zwei Entitätsobjekte („Jeff Smith“ und „Ben Smith“), fügt diese mithilfe der Insert-Methode einem **TableBatchOperation**-Objekt hinzu und startet den Vorgang dann durch einen Aufruf von **CloudTable.ExecuteBatchAsync**.

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a>Abrufen aller Entitäten in einer Partition
Verwenden Sie ein **TableQuery** -Objekt, um eine Tabelle nach allen Entitäten in einer Partition abzufragen. Im folgenden Codebeispiel wird ein Filter für Entitäten erstellt, wobei "Smith" der Partitionsschlüssel ist. In diesem Beispiel werden die Felder der einzelnen Entitäten in den Abfrageergebnissen an die Konsole ausgegeben.

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

## <a name="get-a-single-entity"></a>Abrufen einer einzelnen Entität
Sie können eine Abfrage schreiben, um eine bestimmte Entität abzurufen. Der folgende Code verwendet ein **TableOperation** -Objekt, um einen Kunden namens "Ben Smith" anzugeben. Diese Methode gibt anstelle einer Sammlung nur eine Entität zurück, und bei dem zurückgegebenen Wert in **TableResult.Result** handelt es sich um ein **CustomerEntity**-Objekt. Die Angabe beider Schlüssel (für Partition und Zeile) in einer Abfrage ist die schnellste Möglichkeit, um eine einzelne Entität aus dem **Tabellenspeicherdienst** abzurufen.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Löschen einer Entität
Sie können eine Entität nach dem Abrufen löschen. Der folgende Code sucht nach einer Kundenentität namens "Ben Smith". Wenn diese gefunden wird, wird sie gelöscht.

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]


