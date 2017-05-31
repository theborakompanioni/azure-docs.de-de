Es folgt ein kurzer Überblick zu den Ereignissen in der App. Öffnen Sie die Datei „DocumentDBRepository.cs“. Sie stellen fest, dass mit diesen Codezeilen die DocumentDB-Ressourcen erstellt werden. 

* Der DocumentClient wird initialisiert.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);`
    ```

* Es wird eine neue Datenbank erstellt.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Es wird eine neue Sammlung erstellt.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new RequestOptions { OfferThroughput = 1000 });
    ```
