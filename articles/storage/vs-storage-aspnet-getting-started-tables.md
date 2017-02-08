---
title: Erste Schritte mit Azure Table Storage und verbundenen Visual Studio-Diensten (ASP.NET) | Microsoft-Dokumentation
description: "Erfahren Sie mehr über die ersten Schritte mit Azure Table Storage in einem ASP.NET-Projekt in Visual Studio, nachdem Sie mithilfe von verbundenen Visual Studio-Diensten eine Verbindung mit einem Speicherkonto hergestellt haben."
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: af81a326-18f4-4449-bc0d-e96fba27c1f8
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 07f827e13e5f01a373e69b90e8a5a0c72081acd0
ms.openlocfilehash: 24cfb3217dbadba3a086a8b0251efe5ab0173d1f


---
# <a name="get-started-with-azure-table-storage-and-visual-studio-connected-services-aspnet"></a>Erste Schritte mit Azure Table Storage und verbundenen Visual Studio-Diensten (ASP.NET)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Übersicht

Azure-Tabellenspeicher ermöglicht das Speichern großer Mengen von strukturierten Daten. Der Dienst ist ein NoSQL-Datenspeicher, der authentifizierte Aufrufe von innerhalb und außerhalb der Azure-Cloud akzeptiert. Azure-Tabellen sind hervorragend zur Speicherung strukturierter nicht relationaler Daten geeignet.

In diesem Artikel wird beschrieben, wie Sie Azure Table Storage-Entitäten programmgesteuert verwalten, allgemeine Aufgaben wie das Erstellen und Löschen einer Tabelle ausführen und mit Tabellenentitäten arbeiten. 

> [!NOTE]
> 
> In den Codeabschnitten in diesem Artikel wird davon ausgegangen, dass Sie bereits über Verbundene Dienste eine Verbindung mit einem Azure-Speicherkonto hergestellt haben. Verbundene Dienste wird konfiguriert, indem Sie den Visual Studio Projektmappen-Explorer öffnen, mit der rechten Maustaste auf das Projekt klicken und dann im Kontextmenü **Hinzufügen->Verbundener Dienst** auswählen. Befolgen Sie die Anleitungen im eingeblendeten Dialogfeld zum Herstellen der Verbindung mit dem gewünschten Azure-Speicherkonto.      

## <a name="create-a-table-in-code"></a>Erstellen einer Tabelle in Code

Die folgenden Schritte beschreiben, wie Sie eine Tabelle programmgesteuert erstellen. In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden.

1. Fügen Sie die folgenden *using*-Direktiven hinzu:

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudTableClient**-Objekt ab, das einen Tabellenspeicherdienst-Client darstellt.

        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();


4. Rufen Sie ein **CloudTable**-Objekt ab, das einen Verweis auf den gewünschten Tabellennamen darstellt. (Ändern Sie *<table-name>* in den Namen der Tabelle, den Sie erstellen möchten.)

        CloudTable table = tableClient.GetTableReference(<table-name>);

5. Rufen Sie die **CloudTable.CreateIfNotExists**-Methode auf, um die Tabelle zu erstellen, falls noch nicht vorhanden.   
   
        table.CreateIfNotExists();

## <a name="add-an-entity-to-a-table"></a>Hinzufügen einer Entität zu einer Tabelle

Die folgenden Schritte beschreiben, wie Sie eine Entität einer Tabelle programmgesteuert hinzufügen. In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden. 

1. Fügen Sie die folgenden *using*-Direktiven hinzu:

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudTableClient**-Objekt ab, das einen Tabellenspeicherdienst-Client darstellt.

        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();


4. Rufen Sie ein **CloudTable**-Objekt ab, das einen Verweis auf den gewünschten Tabellennamen darstellt. (Ändern Sie *<table-name>* in den Namen der Tabelle, der Sie die Entität hinzufügen möchten.)

        CloudTable table = tableClient.GetTableReference(<table-name>);

5. Um einer Tabelle eine Entität hinzuzufügen, definieren Sie eine von **TableEntity** abgeleitete Klasse. Der folgenden Code definiert eine Entitätsklasse namens **CustomerEntity** , die den Vornamen des Kunden als Zeilenschlüssel und den Nachnamen als Partitionsschlüssel verwendet.

        public class CustomerEntity : TableEntity
        {
            public CustomerEntity(string lastName, string firstName)
            {
                this.PartitionKey = lastName;
                this.RowKey = firstName;
            }
    
            public CustomerEntity() { }
    
            public string Email { get; set; }
        }

6. Instanziieren Sie die Entität.

        CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
        customer1.Email = "Walter@contoso.com";

7. Erstellen Sie das **TableOperation**-Objekt, das die Kundenentität einfügt.

        TableOperation insertOperation = TableOperation.Insert(customer1);

8. Führen Sie den Einfügevorgang durch Aufrufen der **CloudTable.Execute**-Methode aus. Sie können das Ergebnis des Vorgangs überprüfen, indem Sie die **TableResult.HttpStatusCode**-Eigenschaft untersuchen. Der Statuscode 2xx gibt an, dass die vom Client angeforderte Aktion erfolgreich verarbeitet wurde. Erfolgreiche Einfügungen neuer Entitäten führen z.B. zum HTTP-Statuscode 204, was bedeutet, dass der Vorgang erfolgreich verarbeitet wurde und der Server keine Inhalte zurückgegeben hat.

        TableResult result = table.Execute(insertOperation);

        // Inspect result.HttpStatusCode for success/failure.

## <a name="add-a-batch-of-entities-to-a-table"></a>Hinzufügen eines Entitätsbatchs zu einer Tabelle

Sie können nicht nur Entitäten einzeln nacheinander einer Tabelle hinzufügen, sondern auch Entitäten als Batch hinzufügen. Dies reduziert die Anzahl von Roundtrips zwischen Ihrem Code und dem Azure-Tabellenspeicherdienst. Die folgenden Schritte beschreiben, wie Sie einer Tabelle programmgesteuert mehrere Entitäten in einem Vorgang hinzufügen. In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden.

1. Fügen Sie die folgenden *using*-Direktiven hinzu:

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudTableClient**-Objekt ab, das einen Tabellenspeicherdienst-Client darstellt.

        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();


4. Rufen Sie ein **CloudTable**-Objekt ab, das einen Verweis auf den gewünschten Tabellennamen darstellt. (Ändern Sie *<table-name>* in den Namen der Tabelle, der Sie die Entitäten hinzufügen möchten.)

        CloudTable table = tableClient.GetTableReference(<table-name>);

5. Um einer Tabelle eine Entität hinzuzufügen, definieren Sie eine von **TableEntity** abgeleitete Klasse. Der folgenden Code definiert eine Entitätsklasse namens **CustomerEntity** , die den Vornamen des Kunden als Zeilenschlüssel und den Nachnamen als Partitionsschlüssel verwendet.

        public class CustomerEntity : TableEntity
        {
            public CustomerEntity(string lastName, string firstName)
            {
                this.PartitionKey = lastName;
                this.RowKey = firstName;
            }
    
            public CustomerEntity() { }
    
            public string Email { get; set; }
        }

6. Instanziieren Sie die Entitäten.

        CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
        customer1.Email = "Jeff@contoso.com";
    
        CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
        customer2.Email = "Ben@contoso.com";

7. Rufen Sie ein **TableBatchOperation**-Objekt ab.

        TableBatchOperation batchOperation = new TableBatchOperation();

8. Fügen Sie Entitäten dem Batcheinfügevorgang hinzu.

        batchOperation.Insert(customer1);
        batchOperation.Insert(customer2);

9. Führen Sie den Batcheinfügevorgang durch Aufrufen der **CloudTable.ExecuteBatch**-Methode aus. Die **CloudTable.ExecuteBatch**-Methode gibt eine Liste von **TableResult**-Objekten zurück. Sie können das Ergebnis des Batcheinfügevorgangs überprüfen, indem Sie die **TableResult.HttpStatusCode**-Eigenschaft jedes **TableResult**-Objekts in der Liste untersuchen. Der Statuscode 2xx gibt an, dass die vom Client angeforderte Aktion erfolgreich verarbeitet wurde. Erfolgreiche Einfügungen neuer Entitäten führen z.B. zum HTTP-Statuscode 204, was bedeutet, dass der Vorgang erfolgreich verarbeitet wurde und der Server keine Inhalte zurückgegeben hat.
    
        IList<TableResult> results = table.ExecuteBatch(batchOperation);

        // Inspect the HttpStatusCode property of each TableResult object
        // in the results list for success/failure.

## <a name="get-a-single-entity"></a>Abrufen einer einzelnen Entität

Die folgenden Schritte beschreiben, wie Sie eine Entität programmgesteuert aus einer Tabelle abrufen. In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden. 

> [!NOTE]
> 
> Der Code in diesem Abschnitt verweist auf die **CustomerEntity**-Klasse und Daten, die im Abschnitt [Hinzufügen eines Entitätsbatchs zu einer Tabelle](#add-a-batch-of-entities-to-a-table) gezeigt wurden. 

1. Fügen Sie die folgenden *using*-Direktiven hinzu:

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudTableClient**-Objekt ab, das einen Tabellenspeicherdienst-Client darstellt.

        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();


4. Rufen Sie ein **CloudTable**-Objekt ab, das einen Verweis auf den gewünschten Tabellennamen darstellt. (Ändern Sie *<table-name>* in den Namen der Tabelle, der Sie die Entitäten hinzufügen möchten.)

        CloudTable table = tableClient.GetTableReference(<table-name>);

5. Erstellen Sie ein Abrufvorgangsobjekt, das ein von **TableEntity** abgeleitetes Entitätsobjekt verwendet. Der erste Parameter ist *partitionKey* und der zweite Parameter *rowKey*. Mithilfe der **CustomerEntity**-Klasse und den Daten aus dem Abschnitt [Hinzufügen eines Entitätsbatchs zu einer Tabelle](#add-a-batch-of-entities-to-a-table) fragt der folgende Codeausschnitt die Tabelle auf eine **CustomerEntity**-Entität mit dem *partitionKey*-Wert „Smith“ und *rowKey*-Wert „Ben“ ab.  

        TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

6. Führen Sie den Abrufvorgang aus.   

        TableResult retrievedResult = table.Execute(retrieveOperation);

7. Überprüfen Sie das Ergebnis des Vorgangs durch Untersuchen der **TableOperation.HttpStatusCode**-Eigenschaft, wobei der Statuscode 200 bedeutet, dass die vom Client angeforderte Aktion erfolgreich verarbeitet wurde. Sie können auch die **TableResult.Result**-Eigenschaft untersuchen, die (falls der Vorgang erfolgreich war) die zurückgegebene Entität enthält.

        CustomerEntity customer = null;

        if (retrievedResult.HttpStatusCode == 200 && retrievedResult.Result != null)
        {
            // Process the customer entity.
            customer = retrievedResult.Result as CustomerEntity;
        }

## <a name="get-all-entities-in-a-partition"></a>Abrufen aller Entitäten einer Partition

Die folgenden Schritte beschreiben, wie Sie alle Entitäten programmgesteuert aus einer Partition abrufen. In einer ASP.NET MVC-Anwendung würde sich der Code in einem Controller befinden. 

> [!NOTE]
> 
> Der Code in diesem Abschnitt verweist auf die **CustomerEntity**-Klasse und Daten, die im Abschnitt [Hinzufügen eines Entitätsbatchs zu einer Tabelle](#add-a-batch-of-entities-to-a-table) gezeigt wurden. 

1. Fügen Sie die folgenden *using*-Direktiven hinzu:

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudTableClient**-Objekt ab, das einen Tabellenspeicherdienst-Client darstellt.

        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();


4. Rufen Sie ein **CloudTable**-Objekt ab, das einen Verweis auf den gewünschten Tabellennamen darstellt. (Ändern Sie *<table-name>* in den Namen der Tabelle, der Sie die Entitäten hinzufügen möchten.)

        CloudTable table = tableClient.GetTableReference(<table-name>);

5. Instanziieren Sie ein **TableQuery**-Objekt, und geben Sie die Abfrage in der **Where**-Klausel an. Mithilfe der **CustomerEntity**-Klasse und den Daten aus dem Abschnitt [Hinzufügen eines Entitätsbatchs zu einer Tabelle](#add-a-batch-of-entities-to-a-table) fragt der folgende Codeausschnitt die Tabelle auf alle Entitäten ab, deren **PartitionKey** den Wert „Smith“ hat.

        TableQuery<CustomerEntity> query = 
            new TableQuery<CustomerEntity>()
            .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

6. Rufen Sie in einer Schleife die **CloudTable.ExecuteQuerySegmented**-Methode auf, um das Abfrageobjekt zu übergeben, das Sie im vorherigen Schritt instanziiert haben.  Die **CloudTable.ExecuteQuerySegmented**-Methode gibt ein **TableContinuationToken**-Objekt zurück. Wenn diese den Wert **null** hat, bedeutet dies, dass es keine weiteren abzurufenden Entitäten gibt. Verwenden Sie innerhalb der Schleife eine andere Schleife zum Durchlaufen der zurückgegebenen Entitäten.

        TableContinuationToken token = null;
        do
        {
            TableQuerySegment<CustomerEntity>resultSegment = table.ExecuteQuerySegmented(query, token);
            token = resultSegment.ContinuationToken;

            foreach (CustomerEntity customer in resultSegment.Results)
            {
                // Process customer entity.
            }
        } while (token != null);

## <a name="delete-an-entity"></a>Löschen einer Entität

Die folgenden Schritte veranschaulichen, wie Sie eine Entität suchen und löschen.

1. Fügen Sie die folgenden *using*-Direktiven hinzu:

         using Microsoft.Azure;
         using Microsoft.WindowsAzure.Storage;
         using Microsoft.WindowsAzure.Storage.Auth;
         using Microsoft.WindowsAzure.Storage.Table;

2. Rufen Sie ein **CloudStorageAccount** -Objekt ab, das die Informationen zu Ihrem Speicherkonto enthält. Verwenden Sie den folgenden Code, um die Speicherverbindungszeichenfolge und Speicherkontoinformationen aus der Azure-Dienstkonfiguration abzurufen. (Ändern Sie *storage-account-name>* in den Namen des Azure-Speicherkontos, auf das Sie zugreifen.)

         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));

3. Rufen Sie ein **CloudTableClient**-Objekt ab, das einen Tabellenspeicherdienst-Client darstellt.

        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();


4. Rufen Sie ein **CloudTable**-Objekt ab, das einen Verweis auf den gewünschten Tabellennamen darstellt. (Ändern Sie *<table-name>* in den Namen der Tabelle, der Sie die Entitäten hinzufügen möchten.)

        CloudTable table = tableClient.GetTableReference(<table-name>);

5. Erstellen Sie ein Abrufvorgangsobjekt, das ein von **TableEntity** abgeleitetes Entitätsobjekt verwendet. Der erste Parameter ist *partitionKey* und der zweite Parameter *rowKey*. Mithilfe der **CustomerEntity**-Klasse und den Daten aus dem Abschnitt [Hinzufügen eines Entitätsbatchs zu einer Tabelle](#add-a-batch-of-entities-to-a-table) fragt der folgende Codeausschnitt die Tabelle auf eine **CustomerEntity**-Entität mit dem *partitionKey*-Wert „Smith“ und *rowKey*-Wert „Ben“ ab.  

        TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

6. Führen Sie den Abrufvorgang aus.   

        TableResult retrievedResult = table.Execute(retrieveOperation);

7. Überprüfen Sie das Ergebnis des Vorgangs durch Untersuchen der **TableOperation.HttpStatusCode**-Eigenschaft, wobei der Statuscode 200 bedeutet, dass die vom Client angeforderte Aktion erfolgreich verarbeitet wurde. Sie können auch die **TableResult.Result**-Eigenschaft untersuchen, die (falls der Vorgang erfolgreich war) die zurückgegebene Entität enthält. Erstellen Sie innerhalb der bedingten Anweisung zum Überprüfen, ob der Vorgang erfolgreich war, einen Löschvorgang (indem Sie die von der Abfrage zurückgegebene Entität übergeben), und führen Sie den Löschvorgang aus.

        if (retrievedResult.HttpStatusCode == 200 && retrievedResult.Result != null)
        {
            CustomerEntity customer = retrievedResult.Result as CustomerEntity;

            // Create the delete operation.
            TableOperation deleteOperation = TableOperation.Delete(customer);

            // Execute the delete operation.
            table.Execute(deleteOperation);
        }

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]




<!--HONumber=Dec16_HO2-->


