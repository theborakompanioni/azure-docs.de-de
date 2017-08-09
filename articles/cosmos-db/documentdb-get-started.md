---
title: "Azure Cosmos DB: DocumentDB-API – Tutorial zu den ersten Schritten | Microsoft-Dokumentation"
description: In diesem Tutorial werden eine Onlinedatenbank und eine C#-Konsolenanwendung mit der DocumentDB-API erstellt.
keywords: nosql tutorial, online database, c# console application
services: cosmos-db
documentationcenter: .net
author: AndrewHoh
manager: jhubbard
editor: monicar
ms.assetid: bf08e031-718a-4a2a-89d6-91e12ff8797d
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2017
ms.author: anhoh
ms.translationtype: HT
ms.sourcegitcommit: 141270c353d3fe7341dfad890162ed74495d48ac
ms.openlocfilehash: 312cb366924767cd0ad645dc4a435dd549494188
ms.contentlocale: de-de
ms.lasthandoff: 07/25/2017

---
# <a name="azure-cosmos-db-documentdb-api-getting-started-tutorial"></a>Azure Cosmos DB: DocumentDB-API – Tutorial zu den ersten Schritten
> [!div class="op_single_selector"]
> * [.NET](documentdb-get-started.md)
> * [.NET Core](documentdb-dotnetcore-get-started.md)
> * [Node.js für MongoDB](mongodb-samples.md)
> * [Node.js](documentdb-nodejs-get-started.md)
> * [Java](documentdb-java-get-started.md)
> * [C++](documentdb-cpp-get-started.md)
>  
> 

Willkommen zum Tutorial zu den ersten Schritten mit der DocumentDB-API für Azure Cosmos DB! Im Rahmen dieses Tutorials erstellen Sie eine Konsolenanwendung, mit der Azure Cosmos DB-Ressourcen erstellt und abgefragt werden können.

Folgende Themen werden behandelt:

* Erstellen eines Azure Cosmos DB-Kontos und Herstellen der Verbindung
* Konfigurieren Ihrer Visual Studio-Projektmappe
* Erstellen einer Onlinedatenbank
* Erstellen einer Sammlung
* Erstellen von JSON-Dokumenten
* Abfragen der Sammlung
* Ersetzen eines Dokuments
* Löschen eines Dokuments
* Löschen der Datenbank

Sie haben nicht genügend Zeit? Keine Sorge! Die vollständige Lösung ist auf [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started)verfügbar. Im Abschnitt [Abrufen der vollständigen Projektmappe für das NoSQL-Tutorial](#GetSolution) finden Sie eine Kurzanleitung.

Bitte verwenden Sie nach Abschluss des Lernprogramms die Abstimmungsschaltflächen am Anfang oder Ende dieser Seite, um uns Ihre Meinung mitzuteilen. Wenn wir mit Ihnen Kontakt aufnehmen sollen, können Sie Ihre E-Mail-Adresse im Kommentar hinterlassen.

Lassen Sie uns anfangen.

## <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass Sie über Folgendes verfügen:

* Ein aktives Azure-Konto. Wenn Sie keines besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/)registrieren. 
    * Als Alternative können Sie den [Azure Cosmos DB-Emulator](local-emulator.md) für dieses Tutorial verwenden.
* [Visual Studio 2013/Visual Studio 2015](http://www.visualstudio.com/).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Schritt 1: Erstellen eines Azure Cosmos DB-Kontos
Wir erstellen nun ein Azure Cosmos DB-Konto. Wenn Sie bereits über ein Konto verfügen, das Sie verwenden möchten, können Sie diesen Schritt überspringen und mit [Einrichten der Visual Studio-Projektmappe](#SetupVS)fortfahren. Wenn Sie den Azure Cosmos DB-Emulator verwenden, führen Sie bitte die Schritte unter [Azure Cosmos DB-Emulator](local-emulator.md) zum Einrichten des Emulators aus, und fahren Sie dann mit [Einrichten Ihrer Visual Studio-Projektmappe](#SetupVS) fort.

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Schritt 2: Einrichten Ihrer Visual Studio-Projektmappe
1. Öffnen Sie **Visual Studio 2015** auf Ihrem Computer.
2. Wählen Sie im Menü **Datei** die Option **Neu** und anschließend **Projekt** aus.
3. Wählen Sie im Dialogfeld **Neues Projekt** die Option **Vorlagen** / **Visual C#** / **Konsolenanwendung** aus, geben Sie Ihrem Projekt einen Namen, und klicken Sie dann auf **OK**.
   ![Screenshot des Fensters „Neues Projekt“](./media/documentdb-get-started/nosql-tutorial-new-project-2.png)
4. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die neue Konsolenanwendung, die sich unter Ihrer Visual Studio-Projektmappe befindet, und klicken Sie anschließend auf **NuGet-Pakete verwalten...**.
    
    ![Screenshot des Kontextmenüs für das Projekt](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. Klicken Sie auf der Registerkarte **NuGet** auf **Durchsuchen**, und geben Sie in das Suchfeld **azure documentdb** ein.
6. Suchen Sie in den Ergebnissen nach **Microsoft.Azure.DocumentDB**, und klicken Sie auf **Installieren**.
   Die Paket-ID für die Azure Cosmos DB-Clientbibliothek für die DocumentDB-API lautet [Microsoft Azure DocumentDB Client Library](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).
   ![Screenshot des NuGet-Menüs zum Suchen nach dem Azure Cosmos DB-Client-SDK](./media/documentdb-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)

    Wenn Sie eine Meldung im Zusammenhang mit der Überprüfung von Änderungen an der Projektmappe erhalten, klicken Sie auf **OK**. Wenn Sie eine Meldung im Zusammenhang mit der Akzeptierung der Lizenz erhalten, klicken Sie auf **Ich stimme zu**.

Prima. Damit ist die Einrichtung abgeschlossen und wir können mit dem Schreiben von Code beginnen. Ein vollständiges Codeprojekt dieses Tutorials finden Sie auf [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).

## <a id="Connect"></a>Schritt 3: Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto
Fügen Sie zunächst in der Datei "Program.cs" am Anfang der C#-Anwendung folgende Verweise hinzu:

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> Um das Tutorial abzuschließen, müssen Sie die oben genannten Abhängigkeiten hinzufügen.
> 
> 

Fügen Sie diese beiden Konstanten und Ihre Variable *Client* nun unter der öffentlichen *Program*-Klasse hinzu.

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

Navigieren Sie als Nächstes wieder zum [Azure-Portal](https://portal.azure.com), um Ihre Endpunkt-URL und den primären Schlüssel abzurufen. Die Endpunkt-URL und der Primärschlüssel sind erforderlich, damit Ihre Anwendung weiß, womit die Verbindung hergestellt werden soll, und damit Azure Cosmos DB weiß, dass die Verbindung Ihrer Anwendung vertrauenswürdig ist.

Navigieren Sie im Azure-Portal zu Ihrem Azure Cosmos DB-Konto, und klicken Sie auf **Schlüssel**.

Kopieren Sie den URI vom Portal, und fügen Sie ihn in `<your endpoint URL>` in der Datei „program.cs“ ein. Kopieren Sie anschließend den PRIMÄRSCHLÜSSEL aus dem Portal, und fügen Sie ihn in `<your primary key>`ein.

![Screenshot des Azure-Portals, das vom NoSQL-Tutorial zum Erstellen einer C#-Konsolenanwendung verwendet wird. Zeigt ein Azure Cosmos DB-Konto, bei dem der AKTIVE Hub, die Schaltfläche „SCHLÜSSEL“ auf dem Blatt „Azure Cosmos DB-Konto“ sowie auf dem Blatt „Schlüssel“ die Werte URI, PRIMÄRSCHLÜSSEL und SEKUNDÄRSCHLÜSSEL hervorgehoben sind.][keys]

Als Nächstes starten wir die Anwendung, indem wir eine neue Instanz von **DocumentClient** erstellen.

Fügen Sie unterhalb der **Main**-Methode die neue asynchrone Aufgabe mit der Bezeichnung **GetStartedDemo** hinzu. Sie dient zum Instanziieren des neuen **DocumentClient**-Elements.

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

Fügen Sie den folgenden Code hinzu, um die asynchrone Aufgabe über die **Main** -Methode auszuführen. Mit der **Main** -Methode werden Ausnahmen abgefangen und in die Konsole geschrieben.

    static void Main(string[] args)
    {
            // ADD THIS PART TO YOUR CODE
            try
            {
                    Program p = new Program();
                    p.GetStartedDemo().Wait();
            }
            catch (DocumentClientException de)
            {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
            }
            catch (Exception e)
            {
                    Exception baseException = e.GetBaseException();
                    Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
            }
            finally
            {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
            }

Drücken Sie **F5** um die Anwendung auszuführen. Durch die Meldung `End of demo, press any key to exit.` in der Ausgabe des Konsolenfensters wird bestätigt, dass die Verbindung hergestellt wurde.  Das Konsolenfenster kann nun geschlossen werden. 

Glückwunsch! Sie haben die Verbindung mit einem Azure Cosmos DB-Konto erfolgreich hergestellt. Als Nächstes wird beschrieben, wie Sie die Azure Cosmos DB-Ressourcen verwenden.  

## <a name="step-4-create-a-database"></a>Schritt 4: Erstellen einer Datenbank
Bevor Sie den Code zum Erstellen einer Datenbank hinzufügen, fügen Sie eine Hilfsmethode zum Schreiben in die Konsole hinzu.

Kopieren Sie die **WriteToConsoleAndPromptToContinue**-Methode, und fügen Sie sie nach der **GetStartedDemo**-Methode ein.

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

Ihre Azure Cosmos DB-[Datenbank](documentdb-resources.md#databases) kann mithilfe der [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx)-Methode der **DocumentClient**-Klasse erstellt werden. Eine Datenbank ist ein logischer Container für JSON-Dokumentspeicher, der auf Sammlungen aufgeteilt ist.

Kopieren Sie den folgenden Code, und fügen Sie ihn nach der Clienterstellung in die **GetStartedDemo**-Methode ein. Eine Datenbank mit dem Namen *FamilyDB*wird erstellt.

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

Drücken Sie **F5** um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich eine Azure Cosmos DB-Datenbank erstellt.  

## <a id="CreateColl"></a>Schritt 5: Erstellen einer Sammlung
> [!WARNING]
> **CreateDocumentCollectionIfNotExistsAsync** erstellt eine neue Sammlung mit reserviertem Durchsatz. Dies wirkt sich auf die Kosten aus. Weitere Informationen finden Sie auf unserer [Preisseite](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 
> 

Eine [Sammlung](documentdb-resources.md#collections) kann mithilfe der [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx)-Methode der **DocumentClient**-Klasse erstellt werden. Eine Sammlung ist ein Container für JSON-Dokumente und die zugehörige JavaScript-Anwendungslogik.

Kopieren Sie den folgenden Code, und fügen Sie ihn nach der Datenbankerstellung in die **GetStartedDemo**-Methode ein. Hierdurch wird eine Dokumentsammlung namens *FamilyCollection* erstellt.

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART TO YOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

Drücken Sie **F5** um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich eine Azure Cosmos DB-Dokumentsammlung erstellt.  

## <a id="CreateDoc"></a>Schritt 6: Erstellen von JSON-Dokumenten
Ein [Dokument](documentdb-resources.md#documents) kann mithilfe der [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx)-Methode der **DocumentClient**-Klasse erstellt werden. Dokumente sind benutzerdefinierter (beliebiger) JSON-Inhalt. Wir können jetzt eines oder mehrere Dokumente einfügen. Wenn Sie bereits Daten besitzen, die Sie in Ihrer Datenbank speichern möchten, können Sie die Daten mithilfe des [Datenmigrationstools](import-data.md) Azure Cosmos DB in eine Datenbank importieren.

Zunächst müssen wir eine **Family**-Klasse erstellen, mit der in diesem Beispiel in Azure Cosmos DB gespeicherte Objekte dargestellt werden. Außerdem erstellen wir die Unterklassen **Parent**, **Child**, **Pet** und **Address**, die in **Family** verwendet werden. Beachten Sie, dass Dokumente eine **Id**-Eigenschaft enthalten müssen, die in JSON als **id** serialisiert wird. Erstellen Sie diese Klassen, indem Sie die folgenden internen Unterklassen nach der **GetStartedDemo** -Methode hinzufügen.

Kopieren Sie die Klassen **Family**, **Parent**, **Child**, **Pet** und **Address**, und fügen Sie sie nach der **WriteToConsoleAndPromptToContinue**-Methode ein.

    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }

    // ADD THIS PART TO YOUR CODE
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
            return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }

Kopieren Sie die **CreateFamilyDocumentIfNotExists**-Methode, und fügen Sie sie unterhalb der **Address**-Klasse ein.

    // ADD THIS PART TO YOUR CODE
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
            }
            else
            {
                throw;
            }
        }
    }

Fügen Sie zwei Dokumente ein: eines für die Familie Andersen und eines für die Familie Wakefield.

Kopieren Sie den folgenden Code, und fügen Sie ihn nach der Erstellung der Dokumentsammlung in die **GetStartedDemo**-Methode ein.

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
    
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });


    // ADD THIS PART TO YOUR CODE
    Family andersenFamily = new Family
    {
            Id = "Andersen.1",
            LastName = "Andersen",
            Parents = new Parent[]
            {
                    new Parent { FirstName = "Thomas" },
                    new Parent { FirstName = "Mary Kay" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FirstName = "Henriette Thaulow",
                            Gender = "female",
                            Grade = 5,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Fluffy" }
                            }
                    }
            },
            Address = new Address { State = "WA", County = "King", City = "Seattle" },
            IsRegistered = true
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

    Family wakefieldFamily = new Family
    {
            Id = "Wakefield.7",
            LastName = "Wakefield",
            Parents = new Parent[]
            {
                    new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
                    new Parent { FamilyName = "Miller", FirstName = "Ben" }
            },
            Children = new Child[]
            {
                    new Child
                    {
                            FamilyName = "Merriam",
                            FirstName = "Jesse",
                            Gender = "female",
                            Grade = 8,
                            Pets = new Pet[]
                            {
                                    new Pet { GivenName = "Goofy" },
                                    new Pet { GivenName = "Shadow" }
                            }
                    },
                    new Child
                    {
                            FamilyName = "Miller",
                            FirstName = "Lisa",
                            Gender = "female",
                            Grade = 1
                    }
            },
            Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
            IsRegistered = false
    };

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

Drücken Sie **F5** um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich zwei Azure Cosmos DB-Dokumente erstellt.  

![Diagramm zur hierarchischen Beziehung zwischen dem Konto, der Onlinedatenbank, der Sammlung und den Dokumenten, die vom NoSQL-Tutorial zum Erstellen einer C#-Konsolenanwendung verwendet werden.](./media/documentdb-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Schritt 7: Abfragen von Azure Cosmos DB-Ressourcen
Azure Cosmos DB unterstützt umfassende [Abfragen](documentdb-sql-query.md) der in jeder Sammlung gespeicherten JSON-Dokumente.  Der folgende Beispielcode enthält die verschiedenen Abfragen – sowohl per SQL-Syntax von Azure Cosmos DB als auch per LINQ –, die wir nun für die im vorherigen Schritt eingefügten Dokumente ausführen können.

Kopieren Sie die **ExecuteSimpleQuery**-Methode, und fügen Sie sie nach der **CreateFamilyDocumentIfNotExists**-Methode ein.

    // ADD THIS PART TO YOUR CODE
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

            // Here we find the Andersen family via its LastName
            IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                    .Where(f => f.LastName == "Andersen");

            // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
            Console.WriteLine("Running LINQ query...");
            foreach (Family family in familyQuery)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            // Now execute the same query via direct SQL
            IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                    UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                    "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                    queryOptions);

            Console.WriteLine("Running direct SQL query...");
            foreach (Family family in familyQueryInSql)
            {
                    Console.WriteLine("\tRead {0}", family);
            }

            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

Kopieren Sie den folgenden Code, und fügen Sie ihn nach der zweiten Dokumenterstellung in die **GetStartedDemo**-Methode ein.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Drücken Sie **F5** um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich eine Abfrage für eine Azure Cosmos DB-Sammlung durchgeführt.

Das folgende Diagramm veranschaulicht, wie die Azure Cosmos DB-SQL-Abfragesyntax für die erstellte Sammlung aufgerufen wird. Die gleiche Logik wird auf die LINQ-Abfrage angewendet.

![Diagramm zur Veranschaulichung des Bereichs und der Bedeutung der Abfrage, die vom NoSQL-Tutorial zum Erstellen einer C#-Konsolenanwendung verwendet wird.](./media/documentdb-get-started/nosql-tutorial-collection-documents.png)

Das Schlüsselwort [FROM](documentdb-sql-query.md#FromClause) ist in der Abfrage optional, da Azure Cosmos DB bereits Abfragen auf eine einzelne Sammlung begrenzt. Aus diesem Grund ist "FROM Familien f" austauschbar mit "FROM Stamm r" oder einem anderen variablen Namen, den Sie auswählen. Azure Cosmos DB leitet ab, dass Familien, der Stamm oder der variable Name, den Sie ausgewählt haben, standardmäßig auf die aktuelle Sammlung verweist bzw. verweisen.

## <a id="ReplaceDocument"></a>Schritt 8: Ersetzen eines JSON-Dokuments
Azure Cosmos DB unterstützt das Ersetzen von JSON-Dokumenten.  

Kopieren Sie die **ReplaceFamilyDocument**-Methode, und fügen Sie sie nach der **ExecuteSimpleQuery**-Methode ein.

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

Kopieren Sie den folgenden Code, und fügen Sie ihn nach der Abfrageausführung (am Ende der Methode) in die **GetStartedDemo**-Methode ein. Nach dem Ersetzen des Dokuments wird dieselbe Abfrage erneut ausgeführt, um das geänderte Dokument anzuzeigen.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Drücken Sie **F5** um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Dokument ersetzt.

## <a id="DeleteDocument"></a>Schritt 9: Löschen eines JSON-Dokuments
Azure Cosmos DB unterstützt das Löschen von JSON-Dokumenten.  

Kopieren Sie die **DeleteFamilyDocument**-Methode, und fügen Sie sie nach der **ReplaceFamilyDocument**-Methode ein.

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

Kopieren Sie den folgenden Code, und fügen Sie ihn nach der zweiten Abfrageausführung (am Ende der Methode) in die **GetStartedDemo**-Methode ein.

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

Drücken Sie **F5** um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich ein Azure Cosmos DB-Dokument gelöscht.

## <a id="DeleteDatabase"></a>Schritt 10: Löschen der Datenbank
Das Löschen der erstellten Datenbank entfernt die Datenbank und alle untergeordneten Ressourcen (Sammlungen, Dokumente usw.).

Kopieren Sie den folgenden Code, und fügen Sie ihn nach dem Dokumentlöschvorgang in die **GetStartedDemo**-Methode ein, um die gesamte Datenbank und alle untergeordneten Ressourcen zu löschen.

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

Drücken Sie **F5** um die Anwendung auszuführen.

Glückwunsch! Sie haben erfolgreich eine Azure Cosmos DB-Datenbank gelöscht.

## <a id="Run"></a>Schritt 11: Ausführen der gesamten C#-Konsolenanwendung
Drücken Sie in Visual Studio F5 , um die Anwendung im Debugmodus zu erstellen.

Die Ausgabe der GetStarted-Anwendung sollte angezeigt werden. Die Ausgabe zeigt die Ergebnisse der hinzugefügten Abfragen an. Sie sollte mit unten stehendem Beispieltext übereinstimmen.

    Created FamilyDB
    Press any key to continue ...
    Created FamilyCollection
    Press any key to continue ...
    Created Family Andersen.1
    Press any key to continue ...
    Created Family Wakefield.7
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Replaced Family Andersen.1
    Press any key to continue ...
    Running LINQ query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Running direct SQL query...
        Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
    Deleted Family Andersen.1
    End of demo, press any key to exit.

Glückwunsch! Sie haben dieses Tutorial abgeschlossen und verfügen über eine funktionierende C#-Konsolenanwendung.

## <a id="GetSolution"></a> Herunterladen der vollständigen Projektmappe für das Tutorial
Falls Sie die Schritte in diesem Tutorial aus Zeitmangel nicht durchführen konnten oder nur die Codebeispiele herunterladen möchten, können Sie sie über [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) beziehen. 

Zum Erstellen der GetStarted-Projektmappe benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Wenn Sie keines besitzen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/)registrieren.
* Ein [Azure Cosmos DB-Konto][cosmos-db-create-account].
* [GetStarted-Projektmappe](https://github.com/Azure-Samples/documentdb-dotnet-getting-started) (erhältlich auf GitHub)

Um die Verweise auf das Azure Cosmos DB .NET SDK in Visual Studio wiederherzustellen, klicken Sie mit der rechten Maustaste im Projektmappen-Explorer auf die  **GetStarted**-Lösung, und klicken Sie dann auf die Option **NuGet-Paketwiederherstellung aktivieren**. Aktualisieren Sie dann in der Datei „App.config“ die Werte „EndpointUrl“ und „AuthorizationKey“, wie unter [Herstellen einer Verbindung mit einem Azure Cosmos DB-Konto](#Connect) beschrieben.

Das war's auch schon! Nun müssen Sie nur noch die Erstellung durchführen.


## <a name="next-steps"></a>Nächste Schritte
* Möchten Sie ein komplexeres ASP.NET MVC-Tutorial durcharbeiten? Sie finden eines unter [Erstellen einer Webanwendung mit ASP.NET MVC unter Verwendung von Azure Cosmos DB](documentdb-dotnet-application.md).
* Möchten Sie Azure Cosmos DB nutzen, um Skalierungs- und Leistungstests durchzuführen? Weitere Informationen finden Sie unter [Leistungs- und Skalierungstests mit Azure Cosmos DB](performance-testing.md).
* Informieren Sie sich über das [Überwachen eines Azure Cosmos DB-Kontos](monitor-accounts.md).
* Fragen Sie unser Beispieldataset im [Query Playground](https://www.documentdb.com/sql/demo)ab.
* Weitere Informationen zum Programmiermodell finden Sie auf der [Azure Cosmos DB-Dokumentationsseite](https://azure.microsoft.com/documentation/services/documentdb/) im Abschnitt „Develop“ (Entwickeln).

[keys]: media/documentdb-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-documentdb-dotnet.md#create-account

