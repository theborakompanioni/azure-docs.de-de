<properties 
   pageTitle="Erste Schritte mit Data Lake-Speicher mit REST-API | Microsoft Azure" 
   description="Verwenden von WebHDFS-REST-APIs, um Vorgänge in Data Lake-Speicher auszuführen" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="04/08/2016"
   ms.author="nitinme"/>

# Erste Schritte mit Azure Data Lake-Speicher mithilfe von REST-APIs

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST-API](data-lake-store-get-started-rest-api.md)
- [Azure-Befehlszeilenschnittstelle](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

In diesem Artikel erfahren Sie, wie Sie mit WebHDFS-REST-APIs und Data Lake Speicher-REST-APIs sowohl Kontenverwaltung als auch Dateisystemvorgänge in Azure Data Lake-Speicher ausführen. Azure Data Lake-Speicher stellt eigene REST-APIs für Kontenverwaltungsvorgänge bereit. Data Lake-Speicher ist jedoch mit HDFS und Hadoop-Ökosystem kompatibel und unterstützt die Verwendung von WebHDFS-REST-APIs für Dateisystemvorgänge.

>[AZURE.NOTE] Detaillierte Informationen über die REST-API-Unterstützung für Data Lake-Speicher finden Sie unter [Azure Data Lake Store REST API Reference](https://msdn.microsoft.com/library/mt693424.aspx) (REST-API-Referenz für Azure Data Lake-Speicher).

## Voraussetzungen

- **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
- **Aktiviertes Azure-Abonnement** für die öffentliche Vorschauversion des Data Lake-Speichers. Weitere Informationen finden Sie in den [Anweisungen](data-lake-store-get-started-portal.md#signup).
- **Erstellen einer Azure Active Directory-Anwendung.** Siehe [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mithilfe des Portals](../resource-group-create-service-principal-portal.md). Nach dem Erstellen der Anwendung rufen Sie die folgenden Werte ab, die mit der Anwendung in Zusammenhang stehen.
	- Rufen Sie die Client-ID und Mandanten-ID für die Anwendung ab.
	- Erstellen eines Authentifizierungsschlüssels
	- Festlegen der delegierten Berechtigungen

	Informationen zum Abrufen dieser Werte sind im oben aufgeführten Link verfügbar.
- **Weisen Sie die Azure Active Directory-Anwendung einer Rolle zu**. Die Rolle kann sich auf der Ebene des Bereichs befinden, auf der Sie die Berechtigung für die Azure Active Directory-Anwendung gewähren möchten. Beispielsweise können Sie die Anwendung auf Abonnementebene oder auf der Ebene einer Ressourcengruppe zuweisen. Eine Anleitung hierzu finden Sie unter [Zuweisen einer Anwendung zur Rolle](../resource-group-create-service-principal-portal.md#assign-application-to-role).
- [cURL](http://curl.haxx.se/). Dieser Artikel zeigt mit cURL, wie Sie REST-API-Aufrufe eines Data Lake-Speicherkontos ausführen.

## Wie authentifiziere ich mich mithilfe von Azure Active Directory?

Zur Authentifizierung mit Azure Active Directory können Sie zwischen zwei Ansätzen wählen.

* **Interaktiv**, wobei die Anwendung den Benutzer zum Anmelden auffordert. Weitere Informationen finden Sie unter [Authorization code grant flow](https://msdn.microsoft.com/library/azure/dn645542.aspx) (Autorisierungscodegewährungs-Fluss).

* **Nicht interaktiv**, wobei die Anwendung eigene Anmeldeinformationen bereitstellt. Weitere Informationen finden Sie unter [Aufrufe zwischen Diensten mithilfe von Clientanmeldeinformationen](https://msdn.microsoft.com/library/azure/dn645543.aspx).

In diesem Artikel wird der **nicht interaktive** Ansatz verfolgt. Hierzu müssen Sie eine POST-Anforderung wie unten abgebildet ausgeben.

	curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Die Ausgabe dieser Anforderung enthält ein Autorisierungstoken (gekennzeichnet durch `access-token` in der folgenden Ausgabe), das Sie anschließend mit Ihren REST-API-Aufrufen übergeben werden. Speichern Sie dieses Authentifizierungstoken in einer Textdatei, die Sie später in diesem Artikel benötigen werden.

	{"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

## Erstellen eines Data Lake-Speicherkontos

Dieser Vorgang basiert auf dem [hier](https://msdn.microsoft.com/library/mt694078.aspx) definierten REST-API-Aufruf.

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie **<yourstorename>** durch Ihren Data Lake-Speichernamen.

	curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@C:\temp\input.json

Ersetzen Sie im obigen Befehl <`REDACTED`> durch das Autorisierungstoken, das Sie zuvor abgerufen haben. Die Anforderungsnutzlast für diesen Befehl ist in der Datei **input.json** enthalten, die für den oben genannten `-d`-Parameter bereitgestellt wird. Der Inhalt der Datei „input.json“ sieht folgendermaßen aus:

	{
	"location": "eastus2",
	"tags": {
		"department": "finance"
		},
	"properties": {}
	}	

## Erstellen von Ordnern in einem Data Lake-Speicherkonto

Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory) definierten WebHDFS-REST-API-Aufruf.

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie **<yourstorename>** durch Ihren Data Lake-Speichernamen.

	curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

Ersetzen Sie im obigen Befehl <`REDACTED`> durch das Autorisierungstoken, das Sie zuvor abgerufen haben. Dieser Befehl erstellt ein Verzeichnis namens **mytempdir** im Stammordner Ihres Data Lake-Speicherkontos.

Wenn der Vorgang erfolgreich abgeschlossen wurde, sollte eine Antwort wie folgt angezeigt werden:

	{"boolean":true}

## Auflisten von Ordnern in einem Data Lake-Speicherkonto

Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory) definierten WebHDFS-REST-API-Aufruf.

Verwenden Sie den folgenden cURL-Befehl. Ersetzen Sie **<yourstorename>** durch Ihren Data Lake-Speichernamen.

	curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

Ersetzen Sie im obigen Befehl <`REDACTED`> durch das Autorisierungstoken, das Sie zuvor abgerufen haben.

Wenn der Vorgang erfolgreich abgeschlossen wurde, sollte eine Antwort wie folgt angezeigt werden:

	{
	"FileStatuses": {
		"FileStatus": [{
			"length": 0,
			"pathSuffix": "mytempdir",
			"type": "DIRECTORY",
			"blockSize": 268435456,
			"accessTime": 1458324719512,
			"modificationTime": 1458324719512,
			"replication": 0,
			"permission": "777",
			"owner": "NotSupportYet",
			"group": "NotSupportYet"
		}]
	}
	}

## Hochladen von Daten in das Data Lake-Speicherkonto

Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File) definierten WebHDFS-REST-API-Aufruf.

Hochladen von Daten mithilfe der REST-API für WebHDFS ist ein zweistufiger Prozess, wie nachstehend beschrieben.

1. Senden Sie eine HTTP-PUT-Anforderung, ohne die hochzuladenden Dateidaten zu senden. Ersetzen Sie im folgenden Befehl **<yourstorename>** durch Ihren Data Lake-Speichernamen.

		curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

	Die Ausgabe für diesen Befehl enthält eine temporäre Umleitungs-URL wie die unten gezeigte.

		HTTP/1.1 100 Continue

		HTTP/1.1 307 Temporary Redirect
		...
		...
		Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
		...
		...

2. Nun müssen Sie eine andere HTTP-PUT-Anforderung an die URL richten, die in der Antwort für die Eigenschaft **Location** aufgeführt wird. Ersetzen Sie **<yourstorename>** durch Ihren Data Lake-Speichernamen.

		curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

	Die Ausgabe sieht etwa wie folgt aus:

		HTTP/1.1 100 Continue

		HTTP/1.1 201 Created
		...
		...

## Lesen von Daten aus einem Data Lake-Speicherkonto

Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File) definierten WebHDFS-REST-API-Aufruf.

Lesen von Daten aus einem Data Lake-Speicherkonto ist ein zweistufiger Prozess.

* Sie müssen zuerst eine GET-Anforderung an den Endpunkt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN` senden. Er gibt einen Speicherort zurück, an den Sie die nächste GET-Anforderung senden sollen.
* Dann senden Sie die GET-Anforderung an den Endpunkt `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Damit wird der Inhalt der Datei angezeigt.

Da es in den Eingabeparametern keinen Unterschied zwischen dem ersten und zweiten Schritt gibt, können Sie mit dem `-L`-Parameter die erste Anforderung senden. Die Option `-L` kombiniert im Wesentlichen zwei Anforderungen in einer und lässt cURL die Anforderung am neuen Speicherort nochmals ausführen. Schließlich wird die Ausgabe aller Anforderungsaufrufe wie unten dargestellt angezeigt. Ersetzen Sie **<yourstorename>** durch Ihren Data Lake-Speichernamen.

	curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Es sollte eine Ausgabe angezeigt werden, die Folgendem ähnelt:

	HTTP/1.1 307 Temporary Redirect
	...
	Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
	...
	
	HTTP/1.1 200 OK
	...
	
	Hello, Data Lake Store user!

## Umbenennen einer Datei in einem Data Lake-Speicherkonto

Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory) definierten WebHDFS-REST-API-Aufruf.

Verwenden Sie zum Umbenennen einer Datei den folgenden cURL-Befehl. Ersetzen Sie **<yourstorename>** durch Ihren Data Lake-Speichernamen.

	curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Es sollte eine Ausgabe angezeigt werden, die Folgendem ähnelt:

	HTTP/1.1 200 OK
	...
	
	{"boolean":true}

## Löschen einer Datei aus einem Data Lake-Speicherkonto

Dieser Vorgang basiert auf dem [hier](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory) definierten WebHDFS-REST-API-Aufruf.

Verwenden Sie zum Löschen einer Datei den folgenden cURL-Befehl. Ersetzen Sie **<yourstorename>** durch Ihren Data Lake-Speichernamen.

	curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Folgendes sollte angezeigt werden:

	HTTP/1.1 200 OK
	...
	
	{"boolean":true}

## Löschen eines Data Lake-Speicherkontos

Dieser Vorgang basiert auf dem [hier](https://msdn.microsoft.com/library/mt694075.aspx) definierten REST-API-Aufruf.

Verwenden Sie zum Löschen eines Data Lake-Speicherkontos den folgenden cURL-Befehl. Ersetzen Sie **<yourstorename>** durch Ihren Data Lake-Speichernamen.

	curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Folgendes sollte angezeigt werden:

	HTTP/1.1 200 OK
	...
	...

## Weitere Informationen

- [Open Source-Big Data-Anwendungen, die mit Azure Data Lake-Speicher funktionieren](data-lake-store-compatible-oss-other-applications.md)
 

<!---HONumber=AcomDC_0413_2016-->