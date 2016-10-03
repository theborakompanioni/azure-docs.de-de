<properties
	pageTitle="Verschieben von Daten in den und aus dem Azure-Blobspeicher mithilfe von AzCopy | Microsoft Azure"
	description="Verschieben von Daten in den und aus dem Azure-Blobspeicher mithilfe von AzCopy"
	services="machine-learning,storage"
	documentationCenter=""
	authors="bradsev"
	manager="jhubbard"
	editor="cgronlun" />

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/14/2016"
	ms.author="bradsev" />

# Verschieben von Daten in den und aus dem Azure-Blobspeicher mithilfe von AzCopy

AzCopy ist ein Befehlszeilenprogramm, das zum Hochladen, Herunterladen und Kopieren von Daten in und aus Microsoft Azure-Blob-, -Datei- und -Tabellenspeicher konzipiert wurde.

Anleitungen zur Installation von AzCopy sowie zusätzliche Informationen zu seiner Verwendung mit der Azure-Plattform finden Sie unter [Erste Schritte mit dem Befehlszeilenprogramm AzCopy](../storage/storage-use-azcopy.md).

Nachstehend finden Sie Links zu Anleitungen für Technologien zum Verschieben von Daten in den und aus dem Azure-BLOB-Speicher:

[AZURE.INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]


> [AZURE.NOTE] Wenn Sie einen virtuellen Computer verwenden, der mit den von den [virtuellen Data Science-Computern in Azure](machine-learning-data-science-virtual-machines.md) bereitgestellten Skripts eingerichtet wurde, ist AzCopy bereits auf dem virtuellen Computer installiert.

> [AZURE.NOTE] Eine umfassende Einführung in Azure-Blobspeicher finden Sie unter [Erste Schritte mit Azure Blob Storage](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure-Blobdienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).


## Voraussetzungen

In diesem Dokument wird davon ausgegangen, dass Sie über ein Azure-Abonnement, ein Speicherkonto und den zugehörigen Speicherschlüssel für dieses Konto verfügen. Bevor Sie Daten hoch- und herunterladen können, müssen Sie den Namen Ihres Azure-Speicherkontos und den Kontoschlüssel kennen.

- Informationen zum Einrichten eines Azure-Abonnements finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).

- Anleitungen zum Erstellen eines Speicherkontos und zum Abrufen von Konto- und Schlüsselinformationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md).


## Ausführen von AzCopy-Befehlen

Zum Ausführen von AzCopy-Befehlen öffnen Sie ein Befehlsfenster und navigieren zum Installationsverzeichnis von AzCopy auf Ihrem Computer, in dem sich die ausführbare Datei „AzCopy.exe“ befindet.

Die grundlegende Syntax für AzCopy-Befehle ist:

	AzCopy /Source:<source> /Dest:<destination> [Options]

>[AZURE.NOTE] Sie können den Speicherort der AzCopy-Installation Ihrem Systempfad hinzufügen und die Befehle dann aus einem beliebigen Verzeichnis ausführen. AzCopy wird standardmäßig in *%ProgramFiles(x86)%\\Microsoft SDKs\\Azure\\AzCopy* oder *%ProgramFiles%\\Microsoft SDKs\\Azure\\AzCopy* installiert.

## Hochladen von Dateien in einen Azure-Blob

Verwenden Sie zum Hochladen einer Datei den folgenden Befehl:

	# Upload from local file system
	AzCopy /Source:<your_local_directory> /Dest: https://<your_account_name>.blob.core.windows.net/<your_container_name> /DestKey:<your_account_key> /S


## Herunterladen von Dateien aus einem Azure-Blob

Verwenden Sie zum Herunterladen einer Datei den folgenden Befehl:

	# Downloading blobs to local file system
	AzCopy /Source:https://<your_account_name>.blob.core.windows.net/<your_container_name>/<your_sub_directory_at_blob>  /Dest:<your_local_directory> /SourceKey:<your_account_key> /Pattern:<file_pattern> /S


## Übertragen von Blobs zwischen Azure-Containern

Verwenden Sie zum Übertragen von Blobs zwischen Azure-Containern den folgenden Befehl:

	# Transferring blobs between Azure containers
	AzCopy /Source:https://<your_account_name1>.blob.core.windows.net/<your_container_name1>/<your_sub_directory_at_blob1> /Dest:https://<your_account_name2>.blob.core.windows.net/<your_container_name2>/<your_sub_directory_at_blob2> /SourceKey:<your_account_key1> /DestKey:<your_account_key2> /Pattern:<file_pattern> /S

	<your_account_name>: your storage account name
	<your_account_key>: your storage account key
	<your_container_name>: your container name
	<your_sub_directory_at_blob>: the sub directory in the container
	<your_local_directory>: directory of local file system where files to be uploaded from or the directory of local file system files to be downloaded to
	<file_pattern>: pattern of file names to be transferred. The standard wildcards are supported


## Tipps zur Verwendung von AzCopy

> [AZURE.TIP]   
> 1. Beim **Hochladen** von Dateien wird der Vorgang mit */S* rekursiv durchgeführt. Ohne diesen Parameter werden Dateien in Unterverzeichnissen nicht hochgeladen.
> 2. Beim **Herunterladen** von Datei durchsucht */S* die Container rekursiv, bis alle Dateien im angegebenen Verzeichnis und seinen Unterverzeichnissen oder alle Dateien, die dem angegebenen Muster im angegebenen Verzeichnis und seinen Unterverzeichnissen entsprechen, heruntergeladen wurden.
> 3.  Sie können mit dem */Source*-Parameter keine **bestimmte Blobdatei** für das Herunterladen angeben. Um eine bestimmte Datei herunterzuladen, geben Sie den Blobdateinamen mithilfe des */Pattern*-Parameters an. Der **/S**-Parameter kann verwendet werden, damit AzCopy rekursiv nach einem Dateinamensmuster sucht. Ohne den „/Pattern“-Parameter lädt AzCopy alle Dateien in diesem Verzeichnis herunter.

<!---HONumber=AcomDC_0921_2016-->