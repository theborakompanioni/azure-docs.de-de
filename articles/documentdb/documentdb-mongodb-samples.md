<properties 
	pageTitle="Beispiele für DocumentDB für MongoDB | Microsoft Azure" 
	description="Sehen Sie sich Beispiele für die DocumentDB-Protokollunterstützung für MongoDB an." 
	keywords="MongoDB-Beispiele"
	services="documentdb" 
	authors="stephbaron" 
	manager="jhubbard" 
	editor="" 
	documentationCenter=""/>

<tags 
	ms.service="documentdb" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/31/2016" 
	ms.author="stbaro"/>

# Beispiele für die DocumentDB-Protokollunterstützung für MongoDB
Um diese Beispiele verwenden zu können, müssen Sie folgende Aktionen ausführen:

- [Erstellen](documentdb-create-mongodb-account.md) eines Azure DocumentDB-Kontos mit Protokollunterstützung für MongoDB.
- Abrufen der Informationen zur [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) für das DocumentDB-Konto mit Protokollunterstützung für MongoDB.

## Erste Schritte mit einer Beispielanwendung für eine ASP.NET-MVC-Aufgabenliste

Sie können das Tutorial [Erstellen einer Web-App in Azure, die eine Verbindung mit einer auf einem virtuellen Computer ausgeführten MongoDB herstellt](../app-service-web/web-sites-dotnet-store-data-mongodb-vm.md) mit minimalen Änderungen verwenden, um schnell eine MongoDB-Anwendung einzurichten (lokal oder veröffentlicht in einer Azure-Web-App), die mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB verbunden wird.

1. Führen Sie das Tutorial mit einer Änderung durch. Ersetzen Sie den Code „Dal.cs“ durch Folgendes:
	
		using System;
		using System.Collections.Generic;
		using System.Linq;
		using System.Web;
		using MyTaskListApp.Models;
		using MongoDB.Driver;
		using MongoDB.Bson;
		using System.Configuration;
		using System.Security.Authentication;

		namespace MyTaskListApp
		{
		    public class Dal : IDisposable
	    	{
        		//private MongoServer mongoServer = null;
        		private bool disposed = false;

		        // To do: update the connection string with the DNS name
        		// or IP address of your server. 
        		//For example, "mongodb://testlinux.cloudapp.net
        		private string connectionString = "mongodb://localhost:27017";
        		private string userName = "<your user name>";
        		private string host = "<your host>";
        		private string password = "<your password>";

        		// This sample uses a database named "Tasks" and a 
        		//collection named "TasksList".  The database and collection 
        		//will be automatically created if they don't already exist.
        		private string dbName = "Tasks";
        		private string collectionName = "TasksList";

        		// Default constructor.        
        		public Dal()
		        {
		        }

        		// Gets all Task items from the MongoDB server.        
		        public List<MyTask> GetAllTasks()
		        {
            		try
            		{
                		var collection = GetTasksCollection();
                		return collection.Find(new BsonDocument()).ToList();
            		}
            		catch (MongoConnectionException)
            		{
                		return new List<MyTask>();
            		}
        		}

        		// Creates a Task and inserts it into the collection in MongoDB.
        		public void CreateTask(MyTask task)
        		{
            		var collection = GetTasksCollectionForEdit();
            		try
            		{
                		collection.InsertOne(task);
            		}
            		catch (MongoCommandException ex)
            		{
                		string msg = ex.Message;
            		}
        		}
		
		        private IMongoCollection<MyTask> GetTasksCollection()
		        {
            		MongoClientSettings settings = new MongoClientSettings();
            		settings.Server = new MongoServerAddress(host, 10250);
            		settings.UseSsl = true;
            		settings.SslSettings = new SslSettings();
            		settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
		
            		MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
            		MongoIdentityEvidence evidence = new PasswordEvidence(password);
		
            		settings.Credentials = new List<MongoCredential>()
            		{
                		new MongoCredential("SCRAM-SHA-1", identity, evidence)
            		};

            		MongoClient client = new MongoClient(settings);
            		var database = client.GetDatabase(dbName);
            		var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
            		return todoTaskCollection;
        		}
		
		        private IMongoCollection<MyTask> GetTasksCollectionForEdit()
		        {
            		MongoClientSettings settings = new MongoClientSettings();
            		settings.Server = new MongoServerAddress(host, 10250);
            		settings.UseSsl = true;
            		settings.SslSettings = new SslSettings();
            		settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;
		
            		MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
            		MongoIdentityEvidence evidence = new PasswordEvidence(password);
		
            		settings.Credentials = new List<MongoCredential>()
            		{
                		new MongoCredential("SCRAM-SHA-1", identity, evidence)
            		};
            		MongoClient client = new MongoClient(settings);
            		var database = client.GetDatabase(dbName);
            		var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
            		return todoTaskCollection;
        		}

        		# region IDisposable
		
		        public void Dispose()
		        {
            		this.Dispose(true);
            		GC.SuppressFinalize(this);
        		}

        		protected virtual void Dispose(bool disposing)
        		{
            		if (!this.disposed)
            		{
                		if (disposing)
                		{
                		}
            		}

            		this.disposed = true;
        		}

        		# endregion
    		}
		}

2.	Ändern Sie die folgenden Variablen in der Datei „Dal.cs“ entsprechend den Einstellungen Ihres Kontos:

        private string userName = "<your user name>";
    	private string host = "<your host>";
        private string password = "<your password>";

3. Verwenden Sie die App.

## Nächste Schritte

- Informieren Sie sich über [Entwicklungsrichtlinien (Vorschau)](documentdb-mongodb-guidelines.md) für DocumentDB-Konten mit Protokollunterstützung für MongoDB.
- Erfahren Sie, wie Sie [MongoChef](documentdb-mongodb-mongochef.md) mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB verwenden.

 

<!---HONumber=AcomDC_0720_2016-->