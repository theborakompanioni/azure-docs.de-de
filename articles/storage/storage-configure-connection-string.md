<properties 
	pageTitle="Konfigurieren einer Verbindungszeichenfolge für Azure Storage | Microsoft Azure"
	description="Konfigurieren Sie eine Verbindungszeichenfolge für ein Azure-Speicherkonto. Eine Verbindungszeichenfolge enthält die erforderlichen Informationen zum Authentifizieren des Zugriffs zur Laufzeit auf ein Speicherkonto aus Ihrer Anwendung."
	services="storage"
	documentationCenter=""
	authors="tamram"
	manager="carmonm"
	editor="tysonn"/>

<tags
	ms.service="storage"
	ms.workload="storage"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/03/2016"
	ms.author="tamram"/>

# Konfigurieren von Azure Storage-Verbindungszeichenfolgen

## Übersicht

Eine Verbindungszeichenfolge enthält die erforderlichen Authentifizierungsinformationen zum Zugriff auf Daten in einem Azure-Speicherkonto aus Ihrer Anwendung zur Laufzeit. Sie können eine Verbindungszeichenfolge konfigurieren, um Folgendes zu tun:

- Verbinden mit dem Azure-Speicheremulator
- Zugreifen auf ein Speicherkonto in Azure
- Zugreifen auf angegebene Ressourcen in Azure über eine Shared Access Signature (SAS)

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## Speichern der Verbindungszeichenfolge

Die Anwendung muss zur Laufzeit auf die Verbindungszeichenfolge zugreifen, um bei Azure Storage eingehende Anforderungen zu authentifizieren. Es gibt mehrere Möglichkeiten, die Verbindungszeichenfolge zu speichern:

- Für eine Anwendung, die auf dem Desktop oder auf einem Gerät ausgeführt wird, können Sie die Verbindungszeichenfolge in einer `app.config `-Datei oder einer `web.config`-Datei speichern. Fügen Sie die Verbindungszeichenfolge dem Abschnitt **AppSettings** hinzu.
- Wenn Ihre Anwendung in einem Clouddienst in Azure ausgeführt wird, speichern Sie die Verbindungszeichenfolge zumeist im [Azure-Dienstkonfigurationsschema (CSCFG-Datei)](https://msdn.microsoft.com/library/ee758710.aspx). Fügen Sie die Verbindungszeichenfolge zum Abschnitt **ConfigurationSettings** der Dienstkonfigurationsdatei hinzu.
- Sie können die Verbindungszeichenfolge auch direkt in Ihrem Code verwenden. In den meisten Fällen wird jedoch empfohlen, dass Sie die Konfigurationszeichenfolge in einer Konfigurationsdatei speichern.

Das Speichern der Verbindungszeichenfolge in der Konfigurationsdatei erleichtert die Aktualisierung der Verbindungszeichenfolge, um zwischen dem Speicheremulator und einem Azure-Speicherkonto in der Cloud zu wechseln. Sie müssen die Verbindungszeichenfolge nur so bearbeiten, dass sie auf Ihre Zielumgebung verweist.

Über die [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)-Klasse können Sie zur Laufzeit auf die Verbindungszeichenfolge zugreifen, unabhängig davon, wo Ihre Anwendung ausgeführt wird.

## Erstellen einer Verbindungszeichenfolge mit dem Speicheremulator

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Unter [Einsatz des Azure-Speicheremulators für Entwicklung und Tests](storage-use-emulator.md) finden Sie weitere Informationen zum Speicheremulator.

## Erstellen einer Verbindungszeichenfolge für ein Azure-Speicherkonto

Um eine Verbindungszeichenfolge für Ihr Azure-Speicherkonto zu erstellen, verwenden Sie das nachstehende Format der Verbindungszeichenfolge. Geben Sie an, ob Sie über HTTPS (empfohlen) oder HTTP eine Verbindung mit dem Speicherkonto herstellen möchten, ersetzen Sie `myAccountName` durch den Namen Ihres Speicherkontos, und ersetzen Sie `myAccountKey` durch Ihren Kontozugriffsschlüssel:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Ihre Verbindungszeichenfolge sollte der folgenden Beispiel-Verbindungszeichenfolge ähneln:

	DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure Storage unterstützt sowohl HTTP als auch HTTPS. Die Verwendung von HTTPS wird jedoch ausdrücklich empfohlen.

## Erstellen einer Verbindungszeichenfolge mit einer Shared Access Signature

Wenn Sie über eine URL für Shared Access Signature (SAS) verfügen, können Sie die SAS in Ihrer Verbindungszeichenfolge verwenden. Da die SAS die zum Authentifizieren der Anforderung erforderlichen Informationen in den URI einbindet, stellt der SAS-URI das Protokoll, den Dienstendpunkt und die erforderlichen Anmeldeinformationen zum Zugriff auf die Ressource bereit.

Um eine Verbindungszeichenfolge zu erstellen, die eine SAS enthält, geben Sie die Zeichenfolge im folgenden Format ein:

    BlobEndpoint=myBlobEndpoint;
	QueueEndpoint=myQueueEndpoint;
	TableEndpoint=myTableEndpoint;
	FileEndpoint=myFileEndpoint;
	SharedAccessSignature=sasToken

Jeder Dienstendpunkt ist optional, obwohl die Verbindungszeichenfolge mindestens einen enthalten muss.

Die Verwendung von HTTPS mit einem SAS wird als bewährte Methode empfohlen. Weitere Informationen zu Shared Access Signatures finden Sie unter [Shared Access Signatures, Teil 1: Grundlagen zum SAS-Modell](storage-dotnet-shared-access-signature-part-1.md).

>[AZURE.NOTE] Wenn Sie eine SAS in einer Verbindungszeichenfolge in einer Konfigurationsdatei angeben, müssen Sie womöglich Sonderzeichen in der URL codieren.

### Beispiel für Dienst-SAS

Hier ist ein Beispiel einer Verbindungszeichenfolge, die eine Dienst-SAS für Blobspeicher enthält:

	BlobEndpoint=https://storagesample.blob.core.windows.net;SharedAccessSignature=sv=2015-04-05&sr=b&si=tutorial-policy-635959936145100803&sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D

Und hier ist ein Beispiel für die gleiche Verbindungszeichenfolge mit codierten Sonderzeichen:

	BlobEndpoint=https://storagesample.blob.core.windows.net;SharedAccessSignature=sv=2015-04-05&amp;sr=b&amp;si=tutorial-policy-635959936145100803&amp;sig=9aCzs76n0E7y5BpEi2GvsSv433BZa22leDOZXX%2BXXIU%3D

### Beispiel für Konto-SAS

Hier ist ein Beispiel einer Verbindungszeichenfolge, die eine Dienst-SAS für Blob-und Dateispeicher enthält: Beachten Sie, dass Endpunkte für beide Dienste angegeben werden:

	BlobEndpoint=https://storagesample.blob.core.windows.net;
	FileEndpoint=https://storagesample.file.core.windows.net;
	SharedAccessSignature=sv=2015-07-08&sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&spr=https&st=2016-04-12T03%3A24%3A31Z&se=2016-04-13T03%3A29%3A31Z&srt=s&ss=bf&sp=rwl

Und hier ist ein Beispiel für die gleiche Verbindungszeichenfolge mit URL-Codierung:

	BlobEndpoint=https://storagesample.blob.core.windows.net;
	FileEndpoint=https://storagesample.file.core.windows.net;
	SharedAccessSignature=sv=2015-07-08&amp;sig=iCvQmdZngZNW%2F4vw43j6%2BVz6fndHF5LI639QJba4r8o%3D&amp;spr=https&amp;st=2016-04-12T03%3A24%3A31Z&amp;se=2016-04-13T03%3A29%3A31Z&amp;srt=s&amp;ss=bf&amp;sp=rwl

## Erstellen einer Verbindungszeichenfolge für einen bestimmten Speicherendpunkt

Sie können die Dienstendpunkte in Ihrer Verbindungszeichenfolge explizit angeben, anstatt die Standardendpunkte zu verwenden: Um eine Verbindungszeichenfolge zu erstellen, die einen expliziten Endpunkt festlegt, geben Sie für jeden Dienst den vollständigen Dienstendpunkt einschließlich der Protokollspezifikation (HTTPS (empfohlen) oder HTTP) im folgenden Format an:

	DefaultEndpointsProtocol=[http|https];
	BlobEndpoint=myBlobEndpoint;
	QueueEndpoint=myQueueEndpoint;
	TableEndpoint=myTableEndpoint;
	FileEndpoint=myFileEndpoint;
	AccountName=myAccountName;
	AccountKey=myAccountKey

Bei einem möglichen Szenario für die Angabe eines expliziten Endpunkts haben Sie den Endpunkt Ihres Blobspeichers einer benutzerdefinierten Domäne zugeordnet. In diesem Fall können Sie Ihren benutzerdefinierten Endpunkt für Blobspeicher in der Verbindungszeichenfolge angeben und optional die Standardendpunkte für den anderen Dienst angeben, wenn die Anwendung sie verwendet.

Es folgen Beispiele für gültige Verbindungszeichenfolgen, die einen expliziten Endpunkt für den Blobdienst angeben:

	# Blob endpoint only
	DefaultEndpointsProtocol=https;
	BlobEndpoint=www.mydomain.com;
	AccountName=storagesample;
	AccountKey=account-key

	# All service endpoints
	DefaultEndpointsProtocol=https;
	BlobEndpoint=www.mydomain.com;
	FileEndpoint=myaccount.file.core.windows.net;
	QueueEndpoint=myaccount.queue.core.windows.net;
	TableEndpoint=myaccount;
	AccountName=storagesample;
	AccountKey=account-key

Der in der Verbindungszeichenfolge aufgeführte Endpunktwert wird verwendet, um die Anforderungs-URIs für den BLOB-Dienst zu erstellen. Ferner gibt er die Form aller URIs vor, die dem Code zurückgegeben werden.

Wenn Sie einen Endpunkt in der Verbindungszeichenfolge auslassen, können Sie nicht anhand dieser Verbindungszeichenfolge von Ihrem Code aus auf die Daten in diesem Dienst zugreifen.

### Erstellen einer Verbindungszeichenfolge mit einem Endpunktsuffix

Zum Erstellen einer Verbindungszeichenfolge für den Speicherdienst in Regionen oder Instanzen mit anderen Endpunktsuffixen, z. B. für Azure China oder Azure Governance, verwenden Sie das folgende Format der Verbindungszeichenfolge. Geben Sie an, ob Sie über HTTP oder HTTPS (empfohlen) eine Verbindung mit dem Speicherkonto herstellen möchten, ersetzen Sie `myAccountName` durch den Namen Ihres Speicherkontos, `myAccountKey` durch Ihren Kontozugriffsschlüssel und `mySuffix` durch das URI-Suffix:


	DefaultEndpointsProtocol=[http|https];
	AccountName=myAccountName;
	AccountKey=myAccountKey;
	EndpointSuffix=mySuffix;


Ihre Verbindungszeichenfolge sollte beispielsweise der folgenden Verbindungszeichenfolge ähneln:

	DefaultEndpointsProtocol=https;
	AccountName=storagesample;
	AccountKey=<account-key>;
	EndpointSuffix=core.chinacloudapi.cn;

## Analysieren einer Verbindungszeichenfolge

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## Nächste Schritte

- [Einsatz des Azure-Speicheremulators für Entwicklung und Tests](storage-use-emulator.md)
- [Windows Azure-Speicher-Explorer](storage-explorers.md)

<!---HONumber=AcomDC_0810_2016-->