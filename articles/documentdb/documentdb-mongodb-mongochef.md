<properties 
	pageTitle="Verwenden von MongoChef mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB | Microsoft Azure" 
	description="Erfahren Sie, wie Sie MongoChef mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB verwenden (jetzt als Vorschau verfügbar)." 
	keywords="MongoChef"
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

# Verwenden von MongoChef mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB

Voraussetzungen für das Herstellen einer Verbindung mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB mithilfe von MongoChef:

- Herunterladen und Installieren von [MongoChef](http://3t.io/mongochef)
- Informationen zur [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) für das DocumentDB-Konto mit Protokollunterstützung für MongoDB

## Erstellen der Verbindung in MongoChef  

Führen Sie die folgenden Schritte aus, um dem Verbindungs-Manager von MongoChef Ihr DocumentDB-Konto mit Protokollunterstützung für MongoDB hinzuzufügen.

1. Klicken Sie auf **Connect**, um den Verbindungs-Manager zu öffnen, klicken Sie dann auf **New Connection**.

	![Screenshot des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectionManager.png)
	
2. Geben Sie im Fenster **New Connection** auf der Registerkarte **Server** den FQDN des DocumentDB-Kontos mit Protokollunterstützung für MongoDB und den Port ein.
	
	![Screenshot der Registerkarte „Server“ des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectionManagerServerTab.png)

3. Wählen Sie im Fenster **New Connection** auf der Registerkarte **Authentication** den Authentifizierungsmodus **Standard (MONGODB-CR oder SCARM-SHA-1)** aus, und geben Sie die Informationen zu Benutzername und Kennwort ein. Übernehmen Sie den Standardwert für Authentifizierungs-DB („admin“), oder geben Sie einen eigenen Wert ein.

	![Screenshot der Registerkarte „Authentication“ des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectionManagerAuthenticationTab.png)

4. Aktivieren Sie im Fenster **New Connection** auf der Registerkarte **SSL** das Kontrollkästchen **Use SSL protocol to connect** und das Optionsfeld **Accept self-signed SSL certificates**.

	![Screenshot der Registerkarte „SSL“ des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectionManagerSSLTab.png)

5. Klicken Sie auf die Schaltfläche **Test Connection**, um die Verbindungsinformationen zu überprüfen, klicken Sie auf **OK**, um zum Fenster für neue Verbindungen zurückzukehren, und klicken Sie auf **Save**.

	![Screenshot des MongoChef-Fensters „Test Connection“](./media/documentdb-mongodb-mongochef/TestConnectionResults.png)

## Verwenden von MongoChef zum Erstellen von Datenbanken, Sammlungen und Dokumenten  

Führen Sie die folgenden Schritte aus, um Datenbanken, Sammlungen und Dokumente mithilfe von MongoChef zu erstellen.

1. Markieren Sie die Verbindung in **Connection Manager**, und klicken Sie auf **Connect**.

	![Screenshot des Verbindungs-Managers von MongoChef](./media/documentdb-mongodb-mongochef/ConnectToAccount.png)

2. Klicken Sie mit der rechten Maustaste auf den Host, und wählen Sie **Add Database** aus. Geben Sie einen Datenbanknamen an, und klicken Sie auf **OK**.
	
	![Screenshot der MongoChef-Option „Add Database“](./media/documentdb-mongodb-mongochef/AddDatabase1.png)

3. Klicken Sie mit der rechten Maustaste auf die Datenbank, und wählen Sie **Add Collection** aus. Geben Sie einen Sammlungsnamen an, und klicken Sie auf **Create**.

	![Screenshot der MongoChef-Option „Add Collection“](./media/documentdb-mongodb-mongochef/AddCollection.png)

4. Klicken Sie auf das Menüelement **Collection**, klicken Sie dann auf **Add Document**.

	![Screenshot des MongoChef-Menüelements „Add Document“](./media/documentdb-mongodb-mongochef/AddDocument1.png)

5. Fügen Sie im Dialogfeld zum Hinzufügen von Dokumenten Folgendes ein, und klicken Sie dann auf **Add Document**.

		{
    	"_id": "AndersenFamily",
    	"lastName": "Andersen",
    	"parents": [
       		{ "firstName": "Thomas" },
       		{ "firstName": "Mary Kay"}
    	],
    	"children": [
       	{
           	"firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
           	"pets": [{ "givenName": "Fluffy" }]
       	}
    	],
    	"address": { "state": "WA", "county": "King", "city": "seattle" },
    	"isRegistered": true
		}

	
6. Fügen Sie ein weiteres Dokument mit dem folgenden Inhalt hinzu.

		{
	    "_id": "WakefieldFamily",
	    "parents": [
    	    { "familyName": "Wakefield", "givenName": "Robin" },
        	{ "familyName": "Miller", "givenName": "Ben" }
    	],
    	"children": [
	        {
            	"familyName": "Merriam", 
             	"givenName": "Jesse", 
            	"gender": "female", "grade": 1,
            	"pets": [
	                { "givenName": "Goofy" },
                	{ "givenName": "Shadow" }
            	]
        	},
        	{ 
	            "familyName": "Miller", 
             	"givenName": "Lisa", 
             	"gender": "female", 
             	"grade": 8 }
    	],
    	"address": { "state": "NY", "county": "Manhattan", "city": "NY" },
    	"isRegistered": false
		}

7. Führen Sie eine Beispielabfrage aus. Suchen Sie z.B. nach Familien mit dem Nachnamen „Andersen“, und geben Sie übergeordnete Felder und Felder mit Angaben zum Staat zurück.

	![Screenshot der MongoChef-Abfrageergebnisse](./media/documentdb-mongodb-mongochef/QueryDocument1.png)
	

## Nächste Schritte

- Untersuchen Sie DocumentDB mit Protokollunterstützung für MongoDB anhand von [Beispielen](documentdb-mongodb-samples.md).
- Informieren Sie sich über [Entwicklungsrichtlinien (Vorschau)](documentdb-mongodb-guidelines.md) für DocumentDB-Konten mit Protokollunterstützung für MongoDB.

 

<!---HONumber=AcomDC_0601_2016-->