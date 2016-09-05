<properties 
	pageTitle="Erstellen eines DocumentDB-Kontos mit Protokollunterstützung für MongoDB | Microsoft Azure" 
	description="Erfahren Sie, wie Sie ein DocumentDB-Konto mit Protokollunterstützung für MongoDB erstellen (jetzt als Vorschau verfügbar)." 
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
	ms.date="08/23/2016" 
	ms.author="stbaro"/>

# Erstellen eines DocumentDB-Kontos mit Protokollunterstützung für MongoDB über das Azure-Portal

Zum Erstellen eines Azure DocumentDB-Kontos mit Protokollunterstützung für MongoDB benötigen Sie Folgendes:

- Ein Azure-Konto. Sie können ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) einrichten, falls Sie noch kein Konto besitzen.

## Erstellen des Kontos  

Führen Sie die folgenden Schritte aus, um ein DocumentDB-Konto mit Protokollunterstützung für MongoDB zu erstellen.

1. Melden Sie sich in einem neuen Fenster beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie auf **NEU**, auf **Daten und Speicher** und dann auf **Alle anzeigen**. Suchen Sie anschließend in der Kategorie **Daten und Speicher** nach „DocumentDB-Protokoll“. Klicken Sie auf **DocumentDB-Protokollunterstützung für MongoDB**.

	![Screenshot der Blätter „Marketplace“ und „Daten und Speicher“ für die Suche mit hervorgehobener Option „DocumentDB – Protokollunterstützung für MongoDB“ (Mongo-Datenbank)](./media/documentdb-create-mongodb-account/marketplacegallery2.png)

3. Alternativ können Sie in der Kategorie **Daten und Speicher** unter **Speicher** auf **Weitere** und dann einmal oder mehrmals auf **Weitere laden** klicken, bis **DocumentDB-Protokollunterstützung für MongoDB** angezeigt wird. Klicken Sie auf **DocumentDB-Protokollunterstützung für MongoDB**.

	![Screenshot der Blätter „Marketplace“ und „Daten und Speicher“ mit hervorgehobener Option „DocumentDB – Protokollunterstützung für MongoDB“ (Mongo-Datenbank)](./media/documentdb-create-mongodb-account/marketplacegallery1.png)

4. Klicken Sie auf dem Blatt **DocumentDB-Protokollunterstützung für MongoDB (Vorschau)** auf **Erstellen**, um die Registrierung für die Vorschau zu starten.

	![Blatt „DocumentDB – Protokollunterstützung für MongoDB“ im Azure-Portal](./media/documentdb-create-mongodb-account/marketplacegallery3.png)

5. Klicken Sie auf dem Blatt **DocumentDB-Konto** auf **Für Vorschau registrieren**. Prüfen Sie die Informationen, und klicken Sie auf **OK**.

	![Blatt „Noch heute für die Vorschau anmelden“ für „DocumentDB – Protokollunterstützung für MongoDB“ im Azure-Portal](./media/documentdb-create-mongodb-account/registerforpreview.png)

6.  Nachdem Sie die Bedingungen der Vorschau akzeptiert haben, wird wieder das Blatt „Erstellen“ angezeigt. Geben Sie auf dem Blatt **DocumentDB-Konto** die gewünschte Konfiguration für das Konto an.

	![Screenshot des Blatts für ein neues DocumentDB-Konto mit Protokollunterstützung für MongoDB](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-account.png)


	- Geben Sie im Feld **ID** einen Namen zur Identifizierung des Kontos ein. Wenn die **ID** überprüft wurde, wird im Feld **ID** ein grünes Häkchen angezeigt. Der Wert für **ID** wird zum Hostnamen innerhalb des URI. Die **ID** darf nur Kleinbuchstaben, Ziffern und den Bindestrich "-" enthalten und muss zwischen 3 und 50 Zeichen lang sein. Beachten Sie, dass *documents.azure.com* an den gewählten Namen des Endpunkts angehängt wird, um den Endpunkt des Kontos zu erstellen.

	- Wählen Sie unter **Abonnement** das Azure-Abonnement aus, das Sie für das Konto verwenden möchten. Wenn Ihr Konto nur über ein Abonnement verfügt, wird dieses Konto standardmäßig ausgewählt.

	- Unter **Ressourcengruppe** können Sie eine Ressourcengruppe für das Konto auswählen oder erstellen. Standardmäßig wird eine vorhandene Ressourcengruppe im Azure-Abonnement ausgewählt. Sie können jedoch eine neue Ressourcengruppe erstellen, zu der Sie Ihr Konto hinzufügen möchten. Weitere Informationen finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](resource-group-portal.md).

	- Unter **Standort** können Sie einen geografischen Standort angeben, an dem das Konto gehostet wird.
   
	- Aktivieren Sie **An das Dashboard anheften**.

7.	Klicken Sie nach der Konfiguration der Optionen für das neue Konto auf **Erstellen**. Die Erstellung des Kontos kann einige Minuten dauern. Sie können den Fortschritt im Startmenü überwachen, um den Status zu überprüfen.![Screenshot der Kachel "Erstellen" im Startmenü – Online-Datenbankersteller](./media/documentdb-create-mongodb-account/create-nosql-db-databases-json-tutorial-3.png)

	Sie können den Status auch über den Hub "Benachrichtigungen" überwachen.

	![Datenbanken schnell erstellen - Screenshot des Benachrichtigungs-Hubs, der zeigt, dass das DocumentDB-Konto erstellt wird](./media/documentdb-create-mongodb-account/create-nosql-db-databases-json-tutorial-4.png)

	![Screenshot des Benachrichtigungs-Hubs, der zeigt, dass das DocumentDB-Konto erfolgreich erstellt und in einer Ressourcengruppe bereitgestellt wurde – Benachrichtigung für Online-Datenbankersteller](./media/documentdb-create-mongodb-account/create-nosql-db-databases-json-tutorial-5.png)

8.	Nachdem das Konto erstellt wurde, kann es mit den Standardeinstellungen verwendet werden.

	![Screenshot des Blatts für Standardkonten](./media/documentdb-create-mongodb-account/defaultaccountblades.png)
	

## Nächste Schritte


- Erfahren Sie, wie Sie eine [Verbindung](documentdb-connect-mongodb-account.md) mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB herstellen.

 

<!---HONumber=AcomDC_0824_2016-->