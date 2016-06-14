<properties 
	pageTitle="Herstellen einer Verbindung mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB | Microsoft Azure" 
	description="Erfahren Sie, wie Sie eine Verbindung mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB herstellen (jetzt als Vorschau verfügbar). Stellen Sie mithilfe der MongoDB-Verbindungszeichenfolge eine Verbindung her." 
	keywords="MongoDB-Verbindungszeichenfolge"
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

# Herstellen einer Verbindung mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB

Erfahren Sie, wie Sie eine Verbindung mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB mit dem standardmäßigen URI-Format für MongoDB-Verbindungszeichenfolgen herstellen.

## Abrufen der Informationen zur Verbindungszeichenfolge für das Konto

1. Melden Sie sich in einem neuen Fenster beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf dem Blatt **Alle Einstellungen** auf **Verbindungszeichenfolge**. Klicken Sie zum Navigieren zu **Alle Einstellungen** auf der Navigationsleiste auf **Durchsuchen**, klicken Sie auf **DocumentDB-Konten**, und wählen Sie dann das DocumentDB-Konto mit Protokollunterstützung für MongoDB aus.

	![Screenshot des Blatts „Alle Einstellungen“](./media/documentdb-connect-mongodb-account/SettingsBlade.png)

3. Das Blatt **Informationen zur Verbindungszeichenfolge** wird geöffnet. Es enthält alle erforderlichen Informationen, um mithilfe eines Treibers für MongoDB eine Verbindung mit dem Konto herzustellen, einschließlich einer vorab erstellten Verbindungszeichenfolge.

	![Screenshot des Blatts „Verbindungszeichenfolge“](./media/documentdb-connect-mongodb-account/ConnectionStringBlade.png)

## Anforderungen an die Verbindungszeichenfolge

Beachten Sie, dass DocumentDB das standardmäßige URI-Format für MongoDB-Verbindungszeichenfolgen unterstützt. Es gelten jedoch einige besondere Anforderungen: DocumentDB-Konten erfordern eine Authentifizierung und eine sichere Kommunikation über SSL. Folglich sieht das Format der Verbindungszeichenfolge wie folgt aus:

	mongodb://username:password@host:port/[database]?ssl=true

Die Werte dieser Zeichenfolge sind auf dem oben gezeigten Blatt „Verbindungszeichenfolge“ verfügbar.

- Benutzername (erforderlich)
	- Name des DocumentDB-Kontos
- Kennwort (erforderlich)
	- Kennwort des DocumentDB-Kontos
- Host (erforderlich)
	- FQDN des DocumentDB-Kontos
- Port (erforderlich)
	- 10250
- Datenbank (optional)
	- Die Standarddatenbank, die von der Verbindung verwendet wird
- ssl=true (erforderlich)

Betrachten Sie beispielsweise das Konto in den Informationen zur Verbindungszeichenfolge weiter oben. Eine gültige Verbindungszeichenfolge ist:
	
	mongodb://contoso123:<password@contoso123.documents.azure.com:10250/mydatabase?ssl=true

## Herstellen einer Verbindung mit dem C#-Treiber für MongoDB
Wie bereits erwähnt, erfordern alle DocumentDB-Konten eine Authentifizierung und eine sichere Kommunikation über SSL. Während das URI-Format der MongoDB-Verbindungszeichenfolge einen Abfragezeichenfolgen-Parameter „ssl=true“ unterstützt, erfordert die Verwendung eines MongoDB C#-Treibers das MongoClientSettings-Objekt beim Erstellen eines MongoClient. Der folgende Codeausschnitt veranschaulicht anhand der obigen Kontoinformationen, wie die Verbindung mit dem Konto hergestellt und die Datenbank „Tasks“ verwendet wird.

	        MongoClientSettings settings = new MongoClientSettings();
            settings.Server = new MongoServerAddress("contoso123.documents.azure.com", 10250);
            settings.UseSsl = true;
            settings.SslSettings = new SslSettings();
            settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

            MongoIdentity identity = new MongoInternalIdentity("Tasks", "contoso123");
            MongoIdentityEvidence evidence = new PasswordEvidence("<password>");

            settings.Credentials = new List<MongoCredential>()
            {
                new MongoCredential("SCRAM-SHA-1", identity, evidence)
            };
            MongoClient client = new MongoClient(settings);
            var database = client.GetDatabase("Tasks",);
	

## Nächste Schritte


- Erfahren Sie, wie Sie [MongoChef](documentdb-mongodb-mongochef.md) mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB verwenden.
- Untersuchen Sie DocumentDB mit Protokollunterstützung für MongoDB anhand von [Beispielen](documentdb-mongodb-samples.md).

 

<!---HONumber=AcomDC_0601_2016-->