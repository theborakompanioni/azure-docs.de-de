<properties 
	pageTitle="Verschieben von Daten – Datenverwaltungsgateway | Microsoft Azure"
	description="Richten Sie ein Datengateway ein, um Daten zwischen dem lokalen Speicher und der Cloud zu verschieben. Verwenden Sie das Datenverwaltungsgateway in Azure Data Factory zum Verschieben Ihrer Daten." 
    keywords="Datengateway, Datenintegration, Daten verschieben, Gatewayanmeldeinformationen"
	services="data-factory" 
	documentationCenter="" 
	authors="spelluru" 
	manager="jhubbard" 
	editor="monicar"/>

<tags 
	ms.service="data-factory" 
	ms.workload="data-services" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="07/08/2016" 
	ms.author="spelluru"/>

# Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway
Dieser Artikel enthält eine Übersicht über die Datenintegration zwischen lokalen Datenspeichern und Clouddatenspeichern mit Data Factory. Er baut auf dem Artikel [Datenverschiebungsaktivitäten](data-factory-data-movement-activities.md) und anderen Artikeln über zentrale Konzepte von Data Factory auf: [Datasets](data-factory-create-datasets.md) und [Pipelines](data-factory-create-pipelines.md).

## Gateway zur Datenverwaltung
Sie müssen das Datenverwaltungsgateway auf dem lokalen Computer installieren, um das Verschieben von Daten in einen bzw. aus einem lokalen Datenspeicher zu ermöglichen. Das Gateway kann auf dem gleichen Computer wie der Datenspeicher oder auf einem anderen Computer installiert werden, solange das Gateway eine Verbindung mit dem Datenspeicher herstellen kann. Im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) finden Sie alle Informationen zum Datenverwaltungsgateway.

Die folgende exemplarische Vorgehensweise zeigt Ihnen, wie Sie eine Data Factory mit einer Pipeline erstellen, die Daten aus einer lokalen SQL Server-Datenbank in ein Azure-Blob verschiebt. Im Rahmen der exemplarischen Vorgehensweise installieren und konfigurieren Sie das Datenverwaltungsgateway auf Ihrem Computer.

## Exemplarische Vorgehensweise: Kopieren lokaler Daten in die Cloud
  
## Erstellen einer Data Factory
In diesem Schritt verwenden Sie das Azure-Portal zum Erstellen einer Azure Data Factory-Instanz mit dem Namen **ADFTutorialOnPremDF**. Sie können auch eine Data Factory mithilfe von Azure Data Factory-Cmdlets erstellen.

1.	Nach der Anmeldung beim [Azure-Portal](https://portal.azure.com) klicken Sie links unten auf **NEU**, wählen **Datenanalyse** auf dem Blatt **Erstellen** aus und klicken auf dem Blatt **Datenanalyse** auf **Data Factory**.

	![Neu -> Data Factory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)
  
6. Gehen Sie auf dem Blatt **Neue Data Factory** wie folgt vor:
	1. Geben Sie **ADFTutorialOnPremDF** als **Namen** ein.
	2. Klicken Sie auf **RESSOURCENGRUPPENNAME**, und wählen Sie **ADFTutorialResourceGroup** aus. Sie können eine vorhandene Ressourcengruppe auswählen oder eine neue erstellen. So erstellen Sie eine neue Ressourcengruppe:
		1. Klicken Sie auf **Neue Ressourcengruppe erstellen**.
		2. Geben Sie auf dem Blatt **Ressourcengruppe erstellen** unter **Name** einen Namen für die Ressourcengruppe ein, und klicken Sie auf **OK**.

7. Auf dem Blatt **Neue Data Factory** ist **Zum Startmenü hinzufügen** aktiviert.

	![Zum Startmenü hinzufügen](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

8. Klicken Sie auf dem Blatt **Neue Data Factory** auf **Erstellen**.

	Der Name der Azure Data Factory muss global eindeutig sein. Bei Anzeige der Fehlermeldung **Data factory name "ADFTutorialOnPremDF" is not available** ändern Sie den Namen der Data Factory (z. B.in "IhrNameADFTutorialOnPremDF") und wiederholen den Vorgang. Verwenden Sie diesen Namen beim Ausführen der restlichen Schritte in diesem Lernprogramm anstelle von "ADFTutorialOnPremDF".

9. Suchen Sie Benachrichtigungen des Erstellungsvorgangs, indem Sie auf der Titelleiste auf die Schaltfläche **Benachrichtigungen** klicken, wie im folgenden Bild gezeigt. Klicken Sie erneut auf die Schaltfläche, um das Benachrichtigungsfenster zu schließen.

	![Hub BENACHRICHTIGUNGEN](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNotificationsHub.png)

11. Nach Abschluss der Erstellung wird das Blatt **DATA FACTORY** wie unten dargestellt angezeigt:

	![Data Factory-Startseite](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## Erstellen des Gateways
5. Klicken Sie auf dem Blatt **DATA FACTORY** auf die Kachel **Erstellen und bereitstellen**, um den Editor für die Data Factory zu starten.

	![Kachel "Erstellen und bereitstellen"](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
6.	Klicken Sie in Data Factory Editor auf der Symbolleiste auf **... (Auslassungspunkte)** und dann auf **Neues Daten-Gateway**.

	!["Neues Daten-Gateway" auf der Symbolleiste](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. Geben Sie auf dem Blatt **Erstellen** den Namen **adftutorialgateway** unter **Name** ein, und klicken Sie auf **OK**.

	![Blatt "Gateway erstellen"](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)

3. Klicken Sie auf dem Blatt **Konfigurieren** auf **Direkt auf diesem Computer installieren**. Daraufhin wird das Installationspaket für das Gateway heruntergeladen, installiert, konfiguriert und das Gateway auf dem Computer registriert.

	> [AZURE.NOTE] 
	Hierfür ist Internet Explorer oder ein mit Microsoft ClickOnce kompatibler Webbrowser erforderlich.
	> 
	> Navigieren Sie bei Verwendung von Chrome zum [Chrome Web Store](https://chrome.google.com/webstore/), und suchen Sie nach „ClickOnce“. Wählen Sie eine der ClickOnce-Erweiterungen aus, und installieren Sie sie.
	>  
	> Gehen Sie bei Firefox genauso vor (Installation des Add-Ins). Klicken Sie auf der Symbolleiste auf die Schaltfläche **Menü öffnen** (**drei waagerechte Striche** oben rechts), klicken Sie auf **Add-Ons**, suchen Sie nach dem Stichwort „ClickOnce“, wählen Sie eine der ClickOnce-Erweiterungen aus, und installieren Sie sie.

	![Blatt "Gateway konfigurieren"](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

	Dies ist die einfachste Möglichkeit (One-Click) zum Herunterladen, Installieren, Konfigurieren und Registrieren des Gateways in einem einzigen Schritt. Sie können sehen, dass die Anwendung **Microsoft Datenverwaltungsgateway – Konfigurations-Manager** auf Ihrem Computer installiert ist. Die ausführbare Datei **ConfigManager.exe** befindet sich im Ordner **C:\\Programme\\Microsoft Data Management Gateway\\1.0\\Shared**.

	Sie können das Gateway auch manuell über die Links in diesem Blatt herunterladen und installieren und mit dem Schlüssel im Textfeld **NEUER SCHLÜSSEL** registrieren.
	
	Im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) finden Sie alle Informationen zum Gateway.

	>[AZURE.NOTE] Sie müssen ein Administrator auf dem lokalen Computer sein, um das Datenverwaltungsgateway erfolgreich installieren und konfigurieren zu können. Sie können der lokalen Windows-Gruppe "Datenverwaltungsgateway-Benutzer" zusätzliche Benutzer hinzufügen. Die Mitglieder dieser Gruppe können den Datenverwaltungsgateway-Konfigurations-Manager verwenden, um das Gateway zu konfigurieren.

5. Warten Sie einige Minuten, und starten Sie dann den **Datenverwaltungsgateway-Konfigurations-Manager** auf Ihrem Computer. Geben Sie im Fenster **Suchen** den Begriff **Datenverwaltungsgateway** ein, um auf dieses Hilfsprogramm zuzugreifen. Die ausführbare Datei **ConfigManager.exe** befindet sich im Ordner **C:\\Programme\\Microsoft Data Management Gateway\\1.0\\Shared**.

	![Gatewaykonfigurations-Manager](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)

6. Warten Sie, bis die Werte wie folgt festgelegt sind:
	2. **Gatewayname** wird auf **adftutorialgateway** festgelegt.
	4. Die Statusleiste im unteren Bereich zeigt **Verbunden mit Clouddienst** sowie ein **grünes Häkchen** an.

	Auf der Registerkarte **Startseite** können Sie auch Folgendes tun: – **Registrieren** eines Gateways mit einem Schlüssel aus dem Azure-Portal mithilfe der Schaltfläche „Registrieren“. – **Beenden** des Datenverwaltungsgateway-Hostdiensts, der auf Ihrem Gatewaycomputer ausgeführt wird. – **Updates planen**, die zu einem bestimmten Zeitpunkt des Tages installiert werden. – Anzeigen, wann das Gateway **zuletzt aktualisiert** wurde.

8. Wechseln Sie zu der Registerkarte **Einstellungen**. Das Zertifikat im Abschnitt „Zertifikat“ dient zum Verschlüsseln/Entschlüsseln von Anmeldeinformationen für den lokalen Datenspeicher, den Sie im Portal angeben. (optional für die Zwecke dieses Tutorials) Klicken Sie auf **Ändern**, um stattdessen Ihr eigenes Zertifikat zu verwenden. Standardmäßig verwendet das Gateway das Zertifikat, das vom Data Factory-Dienst automatisch generiert wird.

	![Konfiguration des Gatewayzertifikats](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

	Sie können auch Folgendes auf der Registerkarte „Einstellungen“ tun: – Anzeigen oder Exportieren des Zertifikats, das vom Gateway verwendet wird. – Ändern des HTTPS-Endpunkts, der vom Gateway verwendet wird. –
9. (Optional) Wechseln Sie zur Registerkarte **Diagnose**, und aktivieren Sie die Option **Ausführliche Protokollierung aktivieren**, wenn Sie die ausführliche Protokollierung aktivieren möchten, um Probleme mit dem Gateway behandeln zu können. Die Protokollinformationen finden Sie in der **Ereignisanzeige** unter **Anwendungs- und Dienstprotokolle** -> Knoten **Datenverwaltungsgateway**.

	![Registerkarte „Diagnose“](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

	Sie können auch wie folgt auf der Registerkarte **Diagnose** vorgehen:
	
		- Use *Test Connection** section to an on-premises data source using the gateway.
		- Click **View Logs** to see the Data Management Gateway log in a Event Viewer window. 
		- Click **Send Logs** to upload a zip file with logs of last 7 days to Microsoft to facilitate troubleshooting of your issues. 
10. Klicken Sie im Azure-Portal auf **OK** auf dem Blatt **Konfigurieren** und dann auf dem Blatt **Neues Daten-Gateway**.
6. In der Strukturansicht links sollte **adftutorialgateway** unter **Daten-Gateways** angezeigt werden. Wenn Sie darauf klicken, sollte die zugehörige JSON angezeigt werden.
	

## Erstellen von verknüpften Diensten 
In diesem Schritt erstellen Sie zwei verknüpfte Dienste: **AzureStorageLinkedService** und **SqlServerLinkedService**. Der **SqlServerLinkedService** verknüpft eine lokale SQL Server-Datenbank und der verknüpfte Dienst **AzureStorageLinkedService** einen Azure-Blobspeicher mit der Data Factory. Sie erstellen eine Pipeline weiter unten in dieser exemplarischen Vorgehensweise, die Daten aus der lokalen SQL Server-Datenbank in den Azure-Blobspeicher kopiert.

#### Hinzufügen eines verknüpften Diensts zu einer lokalen SQL Server-Datenbank
1.	Klicken Sie in **Data Factory Editor** auf der Symbolleiste auf **Neuer Datenspeicher**, und wählen Sie **SQL Server**.

	![Neuer mit SQL Server verknüpfter Dienst](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png)
3.	Gehen Sie im **JSON-Editor** folgendermaßen vor:
	1. Geben Sie unter **gatewayName** den Namen **adftutorialgateway** an.
	2. Bei Verwendung der Windows-Authentifizierung:
		1. In **connectionString**:
			1. Legen Sie **Integrierte Sicherheit** auf **wahr** fest.
			2. Geben Sie für die Datenbank **Servername** und **Datenbankname** an.
			2. Entfernen Sie **Benutzer-ID** und **Kennwort**.
		3. Geben Sie den Benutzernamen und das Kennwort für die Eigenschaften **userName** und **password** an.
		
				"typeProperties": {
            		"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=True;",
            		"gatewayName": "adftutorialgateway",
            		"userName": "<Specify user name if you are using Windows Authentication. Example: <domain>\<user>",
            		"password": "<Specify password for the user account>"
        		}

	4. Bei Verwendung der SQL-Authentifizierung:
		1. Geben Sie für die Datenbank **Servername**, **Datenbankname**, **Benutzer-ID** und **Kennwort** in **connectionString** an.       
		2. Entfernen Sie die letzten beiden JSON-Eigenschaften – **userName** und **password** – aus der JSON.
		3. Entfernen Sie das nachgestellte **, (Komma)** am Ende der Zeile, die den Wert für die **gatewayName**-Eigenschaft angibt. 

				"typeProperties": {
            		"connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
	           		"gatewayName": "<Name of the gateway that the Data Factory service should use to connect to the on-premises SQL Server database>"
    		    }
	
		Die Anmeldeinformationen werden mithilfe eines Zertifikats, das der Data Factory-Dienst besitzt, **verschlüsselt**. Wenn Sie stattdessen das Zertifikat verwenden möchten, das dem Datenverwaltungsgateway zugeordnet ist, gehen Sie zu [Anmeldeinformationen sicher festlegen](#set-credentials-and-security).
    
2.	Klicken Sie auf der Befehlsleiste auf **Bereitstellen**, um den verknüpften SQL Server-Dienst bereitzustellen.

#### Hinzufügen eines verknüpften Diensts für Azure-Speicherkonten
 
1. Klicken Sie im **Data Factory-Editor** auf der Befehlsleiste auf **Neuer Datenspeicher** und dann auf **Azure-Speicher**.
2. Geben Sie den Namen des Azure-Speicherkontos für **Kontoname** ein.
3. Geben Sie den Schlüssel des Azure-Speicherkontos für **Kontoschlüssel** ein.
4. Klicken Sie auf **Bereitstellen**, um den **AzureStorageLinkedService** bereitzustellen.
   
 
## Erstellen von Datasets
In diesem Schritt erstellen Sie die Eingabe- und Ausgabedatasets, die ein- und ausgehende Daten für den Kopiervorgang darstellen (lokale SQL Server-Datenbank = > Azure-Blobspeicher). Vor dem Erstellen von Datasets oder Tabellen (rechteckige Datasets) müssen Sie Folgendes ausführen (detaillierte Schritte in der Liste):

- Erstellen Sie eine Tabelle mit dem Namen **emp** in der SQL Server-Datenbank, die Sie der Data Factory als verknüpften Dienst hinzugefügt haben, und fügen Sie verschiedene Beispieleinträge in die Tabelle ein.
- Erstellen Sie einen Blob-Container mit dem Namen **adftutorial** im Azure-Blob-Speicherkonto, das Sie der Data Factory als verknüpften Dienst hinzugefügt haben.

### Vorbereitung des lokalen SQL Servers für das Lernprogramm

1. Verwenden Sie in der Datenbank, die Sie für den lokalen verknüpften SQL Server-Dienst (**SqlServerLinkedService**) angegeben haben, das folgende SQL-Skript zum Erstellen der Tabelle **emp** in der Datenbank.


        CREATE TABLE dbo.emp
		(
			ID int IDENTITY(1,1) NOT NULL, 
			FirstName varchar(50),
			LastName varchar(50),
    		CONSTRAINT PK_emp PRIMARY KEY (ID)
		)
		GO
 

2. Fügen Sie einige Beispiele in die Tabelle ein:


        INSERT INTO emp VALUES ('John', 'Doe')
		INSERT INTO emp VALUES ('Jane', 'Doe')



### Erstellen der Eingabetabelle

1. Klicken Sie im **Data Factory-Editor** auf der Befehlsleiste auf **Neues Dataset** und dann auf **SQL Server-Tabelle**. 
2.	Ersetzen Sie das JSON-Skript im rechten Bereich durch folgenden Text:    

		{
		  "name": "EmpOnPremSQLTable",
		  "properties": {
		    "type": "SqlServerTable",
		    "linkedServiceName": "SqlServerLinkedService",
		    "typeProperties": {
		      "tableName": "emp"
		    },
		    "external": true,
		    "availability": {
		      "frequency": "Hour",
		      "interval": 1
		    },
		    "policy": {
		      "externalData": {
		        "retryInterval": "00:01:00",
		        "retryTimeout": "00:10:00",
		        "maximumRetry": 3
		      }
		    }
		  }
		}

	Beachten Sie Folgendes:
	
	- **type** ist auf **SqlServerTable** festgelegt.
	- **tableName** ist auf **emp** festgelegt.
	- **linkedServiceName** ist auf **SqlServerLinkedService** festgelegt (diesen verknüpften Dienst haben Sie in Schritt 2 erstellt).
	- Für eine Eingabetabelle, die nicht durch eine andere Pipeline in Azure Data Factory generiert wird, müssen Sie **external** auf **true** festlegen. Dies bedeutet, dass die eingegebenen Daten außerhalb des Azure Data Factory-Diensts erstellt werden. Optional können Sie externe Datenrichtlinien mit dem **externalData**-Element im **policy**-Abschnitt angeben.    

	Details zu den JSON-Eigenschaften finden Sie in der [JSON-Skriptreferenz][json-script-reference].

2. Klicken Sie auf der Befehlsleiste auf **Bereitstellen**, um das Dataset bereitzustellen (eine Tabelle ist ein rechteckiges Dataset). Vergewissern Sie sich, dass auf der Titelleiste **TABELLE ERFOLGREICH BEREITGESTELLT** angezeigt wird.


### Erstellen der Ausgabetabelle

1.	Klicken Sie im **Data Factory-Editor** auf der Symbolleiste auf **Neues Dataset** und dann auf **Azure-Blobspeicher**.
2.	Ersetzen Sie das JSON-Skript im rechten Bereich durch folgenden Text: 

		{
		  "name": "OutputBlobTable",
		  "properties": {
		    "type": "AzureBlob",
		    "linkedServiceName": "AzureStorageLinkedService",
		    "typeProperties": {
		      "folderPath": "adftutorial/outfromonpremdf",
		      "format": {
		        "type": "TextFormat",
		        "columnDelimiter": ","
		      }
		    },
		    "availability": {
		      "frequency": "Hour",
		      "interval": 1
		    }
		  }
		}
  
	Beachten Sie Folgendes:
	
	- **type** ist auf **AzureBlob** festgelegt.
	- **linkedServiceName** ist auf **AzureStorageLinkedService** festgelegt (diesen verknüpften Dienst haben Sie in Schritt 2 erstellt).
	- **folderPath** ist auf **adftutorial/outfromonpremdf** festgelegt, wobei "outfromonpremdf" der Ordner im "adftutorial"-Container ist. Sie müssen lediglich den **adftutorial**-Container erstellen.
	- Die Verfügbarkeit (**availability**) ist auf **hourly**, (**frequency** auf **hour** und **interval** auf **1**) festgelegt. Der Data Factory-Dienst generiert in der Tabelle **emp** in der Azure SQL-Datenbank stündlich einen Ausgabedatenslice.

	Wenn Sie keinen **fileName** für eine **Eingabetabelle** angeben, werden alle Dateien/Blobs aus dem Eingabeordner (**folderPath**) als Eingaben angesehen. Wenn Sie einen Dateinamen in der JSON-Datei angeben, wird nur die angegebene Datei/der angegebene Blob als Eingabe betrachtet. Beachten Sie auch die Beispieldateien im [Tutorial][adf-tutorial].
 
	Wenn Sie keinen **fileName** für eine **Ausgabetabelle** angeben, werden die generierten Dateien in **folderPath** im folgenden Format benannt: Data.<Guid>.txt (Beispiel: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt).

	Um **folderPath** und **fileName** dynamisch basierend auf der **SliceStart**-Zeit festzulegen, verwenden Sie die "partitionedBy"-Eigenschaft. Im folgenden Beispiel verwendet folderPath die Angaben für Jahr, Monat und Tag aus "SliceStart" (Startzeit des zu verarbeitenden Slices) und "fileName" die Angabe für Stunde aus "SliceStart". Wenn beispielsweise ein Slice für den Zeitpunkt "2014-10-20T08:00:00" erzeugt wird, wird "folderName" auf "wikidatagateway/wikisampledataout/2014/10/20" und "filName" auf "08.csv" festgelegt.

	  	"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
        	{ "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 

	Details zu den JSON-Eigenschaften finden Sie in der [JSON-Skriptreferenz][json-script-reference].

2.	Klicken Sie auf der Befehlsleiste auf **Bereitstellen**, um das Dataset bereitzustellen (eine Tabelle ist ein rechteckiges Dataset). Vergewissern Sie sich, dass auf der Titelleiste **TABELLE ERFOLGREICH BEREITGESTELLT** angezeigt wird.
  

## Erstellen der Pipeline
In diesem Schritt erstellen Sie eine **Pipeline** mit einer **Kopieraktivität**, die **EmpOnPremSQLTable** als Eingabe und **OutputBlobTable** als Ausgabe verwendet.

1.	Klicken Sie auf dem Blatt **DATA FACTORY** auf die Kachel **Erstellen und bereitstellen**, um den Editor für die Data Factory zu starten.

	![Kachel "Erstellen und bereitstellen"](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png)
2.	Klicken Sie auf der Befehlsleiste auf **Neue Pipeline**. Wenn die Schaltfläche nicht angezeigt wird, klicken Sie auf **... (Auslassungspunkte)**, um die Befehlsleiste zu erweitern.
2.	Ersetzen Sie das JSON-Skript im rechten Bereich durch folgenden Text:   


		{
		  "name": "ADFTutorialPipelineOnPrem",
		  "properties": {
		    "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
		    "activities": [
		      {
		        "name": "CopyFromSQLtoBlob",
		        "description": "Copy data from on-prem SQL server to blob",
		        "type": "Copy",
		        "inputs": [
		          {
		            "name": "EmpOnPremSQLTable"
		          }
		        ],
		        "outputs": [
		          {
		            "name": "OutputBlobTable"
		          }
		        ],
		        "typeProperties": {
		          "source": {
		            "type": "SqlSource",
		            "sqlReaderQuery": "select * from emp"
		          },
		          "sink": {
		            "type": "BlobSink"
		          }
		        },
		        "Policy": {
		          "concurrency": 1,
		          "executionPriorityOrder": "NewestFirst",
		          "style": "StartOfInterval",
		          "retry": 0,
		          "timeout": "01:00:00"
		        }
		      }
		    ],
		    "start": "2016-07-05T00:00:00Z",
		    "end": "2016-07-06T00:00:00Z",
		    "isPaused": false
		  }
		}

	Beachten Sie Folgendes:
 
	- Der Abschnitt "activities" enthält nur eine Aktivität, deren **type** auf **Copy** festgelegt ist.
	- **Input** für die Aktivität ist auf **EmpOnPremSQLTable** und **output** auf **OutputBlobTable** festgelegt.
	- Im Abschnitt **transformation** ist **SqlSource** als **Quelltyp** und **BlobSink** als **Senkentyp** angegeben.
	- Die SQL-Abfrage **select * from emp** ist für die **sqlReaderQuery**-Eigenschaft von **SqlSource** angegeben.

	Ersetzen Sie den Wert der Eigenschaft **start** durch den aktuellen Tag und den Wert der Eigenschaft **end** durch den nächsten Tag. Die Start- und Endzeit von Datums-/Uhrzeitangaben müssen im [ISO-Format](http://en.wikipedia.org/wiki/ISO_8601) angegeben werden. Beispiel: 2014-10-14T16:32:41Z. Die Angabe für **end** ist optional, wird aber in diesem Lernprogramm verwendet.
	
	Wenn für die **end**-Eigenschaft kein Wert angegeben wird, wird sie als „**start + 48 Stunden**“ berechnet. Um die Pipeline auf unbestimmte Zeit auszuführen, geben Sie als Wert für die **end**-Eigenschaft **9/9/9999** an.
	
	Durch Angabe des aktiven Zeitraums für eine Pipeline definieren Sie die Dauer, für die die Datenslices verarbeitet werden, basierend auf den **Verfügbarkeitseigenschaften**, die für jede Azure Data Factory-Tabelle definiert wurden.
	
	Im obigen Beispiel ergeben sich 24 Datenslices, da jede Stunde ein Datenslice erstellt wird.
	
2. Klicken Sie auf der Befehlsleiste auf **Bereitstellen**, um das Dataset bereitzustellen (eine Tabelle ist ein rechteckiges Dataset). Vergewissern Sie sich, dass auf der Titelleiste **PIPELINE ERFOLGREICH BEREITGESTELLT** angezeigt wird.
5. Schließen Sie jetzt das Blatt **Editor**, indem Sie auf **X** klicken. Klicken Sie erneut auf **X**, um das Blatt "ADFTutorialDataFactory" mit der Symbolleiste und Strukturansicht zu schließen. Wenn die Meldung **Ihre nicht gespeicherten Änderungen werden verworfen** angezeigt wird, klicken Sie auf **OK**.
6. Sie sollten sich nun wieder auf dem Blatt **DATA FACTORY** für **ADFTutorialOnPremDF** befinden.

**Glückwunsch!** Sie haben erfolgreich eine Azure Data Factory, verknüpfte Dienste, Tabellen und eine Pipeline erstellt und die Pipeline geplant.

#### Anzeigen einer Diagrammansicht der Data Factory 
1. Klicken Sie im **Azure-Portal** auf der Startseite für die Data Factory **ADFTutorialOnPremDF** auf die Kachel **Diagramm**.

	![Link "Diagramm"](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Ein Diagramm ähnlich dem folgenden sollte angezeigt werden:

	![Diagrammansicht](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

	Sie können die Ansicht vergrößern, verkleinern, auf 100 % anpassen, an die Fenstergröße anpassen, Pipelines und Tabellen automatisch positionieren und Informationen zur Datenherkunft anzeigen (d. h. vor- und nachgelagerte Elemente ausgewählter Elemente hervorheben). Sie können auf ein Objekt (in der Ein-/Ausgabetabelle oder Pipeline) doppelklicken, um seine Eigenschaften anzuzeigen.

## Überwachen der Pipeline
In diesem Schritt verwenden Sie das Azure-Portal zur Überwachung der Aktivitäten in einer Azure Data Factory. Sie können auch PowerShell-Cmdlets zum Überwachen von Datasets und Pipelines verwenden. Einzelheiten zur Überwachung finden Sie unter [Überwachen und Verwalten von Pipelines](data-factory-monitor-manage-pipelines.md).

1. Navigieren Sie zum **Azure-Portal** (sofern Sie es geschlossen haben).
2. Wenn das Blatt für **ADFTutorialOnPremDF** nicht geöffnet ist, klicken Sie im **Startmenü** zum Öffnen auf **ADFTutorialOnPremDF**.
3. Auf diesem Blatt sollten die **Anzahl** und **Namen** der erstellten Tabellen und der Pipeline angezeigt werden.

	![Data Factory-Startseite](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)
4. Klicken Sie nun auf die Kachel **Datasets**.
5. Klicken Sie auf dem Blatt **Datasets** auf **EmpOnPremSQLTable**.

	![EmpOnPremSQLTable-Slices](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Beachten Sie, dass die Datenslices bis zum aktuellen Zeitpunkt bereits erstellt wurden und den Status **Bereit** aufweisen. Das liegt daran, dass Sie die Daten in die SQL Server-Datenbank eingefügt haben und sie immer vorhanden sind. Überprüfen Sie, ob keine Slices im Abschnitt **Problemslices** am unteren Rand angezeigt werden.


	Die Listen **Letzte aktualisierte Slices** und **Letzte fehlerhafte Slices** werden anhand der **UHRZEIT DER LETZTEN AKTUALISIERUNG** sortiert. Der Zeitpunkt der Aktualisierung eines Slices wird in den folgenden Situationen geändert.
    

	-  Sie haben den Status eines Slices manuell aktualisiert, z. B. mit dem Cmdlet **Set-AzureRmDataFactorySliceStatus**, oder durch Klicken auf **Ausführen** auf dem Blatt **SLICE** des Slices.
	-  Der Status des Slices ändert sich aufgrund einer Ausführung (z. B. Ausführung gestartet, Ausführung mit Fehler beendet, Ausführung erfolgreich beendet usw.).
 
	Klicken Sie auf den Titel der Listen oder auf **...** (Auslassungspunkte), um eine umfangreichere Liste mit Slices anzuzeigen. Klicken Sie auf der Symbolleiste auf **Filter**, um die Slices zu filtern.
	
	Zum Anzeigen der nach Start-/Endzeit sortierten Datenslices klicken Sie auf die Kachel **Datenslices (nach Slicezeit)**.

7. Klicken Sie jetzt auf dem Blatt **Datasets** auf **OutputBlobTable**.

	![OputputBlobTable-Slices][image-data-factory-output-blobtable-slices]
8. Überprüfen Sie, ob die Slices bis zum aktuellen Zeitpunkt bereits erstellt wurden und den Status **Bereit** aufweisen. Warten Sie, bis der Status der Slices bis zum aktuellen Zeitpunkt auf **Bereit** festgelegt ist.
9. Klicken Sie auf einen beliebigen Datenslice in der Liste. Das Blatt **DATENSLICE** sollte angezeigt werden.

	![Blatt "Datenslice"](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

	Wenn der Slice nicht den Status **Bereit** hat, sehen Sie die vorgelagerten Slices, die nicht bereit sind und das Ausführen des aktuellen Slices blockieren, in der Liste **Vorgelagerte Slices, die nicht bereit sind**.

10. Klicken Sie in der Liste unten auf die **Aktivitätsausführung**, um **Aktivitätsausführung – Details** anzuzeigen.

	![Blatt "Aktivitätsausführung – Details"][image-data-factory-activity-run-details]

11. Klicken Sie auf **X**, um alle Blätter zu schließen, bis wieder
12. das Startblatt für **ADFTutorialOnPremDF** angezeigt wird.
14. (Optional) Klicken Sie auf **Pipelines**, dann auf **ADFTutorialOnPremDF**, und zeigen Sie Details zu den Eingabetabellen (**Genutzt**) oder Ausgabetabellen (**Erstellt**) an.
15. Verwenden Sie Tools wie z. B. **Azure Storage-Explorer**, um die Ausgabe zu überprüfen.

	![Azure-Speicher-Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## Nächste Schritte

- Im Artikel [Datenverwaltungsgateway](data-factory-data-management-gateway.md) finden Sie alle Informationen zum Datenverwaltungsgateway.
- Unter [Kopieren von Daten aus Blob Storage in SQL-Datenbank mithilfe von Data Factory](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) erfahren Sie im Allgemeinen, wie Sie die Kopieraktivität verwenden, um Daten aus einem Quelldatenspeicher in einen Senkendatenspeicher zu verschieben.

<!---HONumber=AcomDC_0713_2016-->