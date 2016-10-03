<properties
    pageTitle="Verwenden des Azure-Blobspeichers mit iOS | Microsoft Azure"
	description="Speichern Sie nicht strukturierte Daten in der Cloud mit Azure Blob Storage (Objektspeicher)."
    services="storage"
    documentationCenter="ios"
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="objective-c"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="micurd;tamram"/>

# Verwenden des Blobspeichers mit iOS

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)] 
<br/> 
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## Übersicht

In diesem Artikel wird die Durchführung gängiger Szenarien mit Microsoft Azure-Blobspeicher demonstriert. Die Beispiele sind in Objective-C geschrieben und greifen auf die [Azure-Speicherclientbibliothek für iOS](https://github.com/Azure/azure-storage-ios) zurück. Die hier beschriebenen Szenarien umfassen das **Hochladen**, **Auflisten**, **Herunterladen** und **Löschen** von Blobs. Weitere Informationen zu Blobs finden Sie im Abschnitt [Nächste Schritte](#next-steps). Sie können auch die [Beispielapp](https://github.com/Azure/azure-storage-ios/tree/master/BlobSample) herunterladen, um eine schnelle Demonstration der Verwendung von Azure Storage in einer iOS-Anwendung zu sehen.

[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## Importieren der Azure Storage-iOS-Bibliothek in Ihre Anwendung

Sie können die Azure Storage-iOS-Bibliothek entweder mithilfe von [Azure Storage CocoaPod](https://cocoapods.org/pods/AZSClient) oder durch Importieren der **Framework**-Datei in Ihre Anwendung importieren.

## CocoaPod

1. Sofern Sie es noch nicht getan haben, [installieren Sie CocoaPods](https://guides.cocoapods.org/using/getting-started.html#toc_3) auf Ihrem Computer, indem Sie ein Terminalfenster öffnen und den folgenden Befehl ausführen:

        sudo gem install cocoapods

2. Erstellen Sie als Nächstes im Projektverzeichnis (das Verzeichnis mit Ihrer `.xcodeproj`-Datei) eine neue Datei namens `Podfile` (ohne Erweiterung). Fügen Sie `Podfile` das Folgende hinzu, und speichern Sie die Datei.

        pod 'AZSClient'

3. Navigieren Sie im Terminalfenster zu dem Projektverzeichnis, und führen Sie den folgenden Befehl aus:

        pod install

4. Wenn Ihre `.xcodeproj`-Datei in Xcode geöffnet ist, schließen Sie sie. Öffnen Sie im Projektverzeichnis die neu erstellte Projektdatei, die die Erweiterung `.xcworkspace` erhält. Dies ist die Datei, mit der Sie von jetzt an arbeiten.

## Framework
Um die Azure Storage-iOS-Bibliothek zu verwenden, müssen Sie zunächst die Frameworkdatei erstellen.

1. Laden Sie zuerst das [azure-storage-ios-repo](https://github.com/azure/azure-storage-ios) herunter, oder klonen Sie es.

2. Wechseln Sie zu *azure-storage-ios* -> *Lib* -> *Azure Storage-Client-Bibliothek*, und öffnen Sie `AZSClient.xcodeproj` in Xcode.

3. Ändern Sie oben links in Xcode das aktive Schema von "Azure Storage-Client Library" in "Framework".

4. Erstellen Sie das Projekt (⌘+B). Dadurch wird eine `AZSClient.framework`-Datei auf Ihrem Desktop erstellt.

Sie können die Frameworkdatei dann folgendermaßen in Ihre Anwendung importieren:

1. Erstellen Sie ein neues Projekt, oder öffnen Sie das vorhandene Projekt in Xcode.

2. Klicken Sie im linken Navigationsbereich auf Ihr Projekt, und klicken Sie auf die Registerkarte *General* (Allgemein) am oberen Rand des Projekteditors.

3. Klicken Sie unter dem Abschnitt *Linked Frameworks und Libraries* (Verknüpfte Frameworks und Bibliotheken) auf die Schaltfläche zum Hinzufügen (+).

4. Klicken Sie auf *Add Other...* (Weitere hinzufügen). Navigieren Sie zur gerade erstellten `AZSClient.framework`-Datei, und fügen Sie sie hinzu.

5. Klicken Sie unter dem Abschnitt *Linked Frameworks und Libraries* (Verknüpfte Frameworks und Bibliotheken) erneut auf die Schaltfläche zum Hinzufügen (+).

6. Suchen Sie in der Liste der bereits bereitgestellten Bibliotheken nach `libxml2.2.dylib`, und fügen Sie es dem Projekt hinzu.

7. Klicken Sie am oberen Rand des Projekt-Editors auf die Registerkarte *Build Settings* (Buildeinstellungen).

8. Doppelklicken Sie im Abschnitt *Search Paths* (Suchpfade) auf *Framework Search Paths* (Framework Suchpfade), und fügen Sie den Pfad Ihrer `AZSClient.framework`-Datei hinzu.

## Import-Anweisung
Sie müssen die folgende Import-Anweisung in die Datei einschließen, in der Sie die Azure Storage-API aufrufen möchten.

    // Include the following import statement to use blob APIs.
    #import <AZSClient/AZSClient.h>

[AZURE.INCLUDE [storage-mobile-authentication-guidance](../../includes/storage-mobile-authentication-guidance.md)]

## Asynchrone Vorgänge
> [AZURE.NOTE] Alle Methoden, die eine Anforderung für den Dienst ausführen, sind asynchrone Vorgänge. In den Codebeispielen werden Sie feststellen, dass diese Methoden einen Abschlusshandler haben. Der Code im Abschlusshandler wird **nach** Abschluss der Anforderung ausgeführt. Der Code nach dem Abschlusshandler wird **während** der Anforderung ausgeführt.

## Erstellen eines Containers
Jeder Blob in Azure Storage muss sich in einem Container befinden. Das folgende Beispiel zeigt, wie Sie einen Container namens *newcontainer* in Ihrem Storage-Konto erstellen, wenn er noch nicht vorhanden ist. Wenn Sie einen Namen für den Container auswählen, achten Sie auf die oben genannten Benennungsregeln.

    -(void)createContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"newcontainer"];

      // Create container in your Storage account if the container doesn't already exist
      [blobContainer createContainerIfNotExistsWithCompletionHandler:^(NSError *error, BOOL exists) {
         if (error){
             NSLog(@"Error in creating container.");
         }
      }];
    }

Sie können überprüfen, ob dies funktioniert, indem Sie sich vergewissern, dass *newcontainer* sich im [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com) in der Liste der Container für das Speicherkonto befindet.

## Festlegen von Containerberechtigungen
Die Berechtigungen eines Containers werden standardmäßig für den **Privatzugriff** konfiguriert. Container bieten jedoch einige unterschiedliche Optionen für den Containerzugriff:

- **Privat**: Container- und Blobdaten können nur vom Kontobesitzer gelesen werden.

- **Blob**: Blobdaten innerhalb dieses Containers können über anonyme Anforderungen gelesen werden, aber die Containerdaten sind nicht verfügbar. Clients können keine Blobs innerhalb des Containers über anonyme Anforderungen aufzählen.

- **Container**: Container- und Blobdaten können über anonyme Anforderungen gelesen werden. Clients können Blobs innerhalb des Containers über eine anonyme Anforderung aufzählen, können aber keine Container innerhalb des Speicherkontos aufzählen.

Das folgende Beispiel veranschaulicht das Erstellen eines Containers mit **Container**-Zugriffsberechtigungen, die allen Benutzern im Internet den öffentlichen, schreibgeschützten Zugriff gewähren:

    -(void)createContainerWithPublicAccess{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create container in your Storage account if the container doesn't already exist
        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists){
            if (error){
                NSLog(@"Error in creating container.");
            }
        }];
    }

## Hochladen eines Blobs in einen Container
Wie im Abschnitt [Blobdienstkonzepte](#blob-service-concepts) erwähnt, bietet der Blobspeicher drei verschiedene Arten von Blobs: Blockblobs, Anfügeblobs und Seitenblobs. Zu diesem Zeitpunkt unterstützt die Azure Storage-iOS-Bibliothek nur Blockblobs. In den meisten Fällen wird die Verwendung von Blockblobs empfohlen.

Das folgende Beispiel zeigt den Upload eines Blockblobs aus einer NSString. Wenn in diesem Container bereits ein Blob mit demselben Namen vorhanden ist, wird der Inhalt dieses Blobs überschrieben.

    -(void)uploadBlobToContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        [blobContainer createContainerIfNotExistsWithAccessType:AZSContainerPublicAccessTypeContainer requestOptions:nil operationContext:nil completionHandler:^(NSError *error, BOOL exists)
         {
             if (error){
                 NSLog(@"Error in creating container.");
             }
             else{
                 // Create a local blob object
                 AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

                 // Upload blob to Storage
                 [blockBlob uploadFromText:@"This text will be uploaded to Blob Storage." completionHandler:^(NSError *error) {
                     if (error){
                         NSLog(@"Error in creating blob.");
                     }
                 }];
             }
         }];
    }

Sie können überprüfen, ob dies funktioniert, indem Sie sich vergewissern, dass der Container *containerpublic* im [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com) das Blob *sampleblob* enthält. In diesem Beispiel haben wir einen öffentlichen Container verwendet, sodass Sie die Funktion auch durch das Aufrufen der Blob-URI überprüfen können:

    https://nameofyourstorageaccount.blob.core.windows.net/containerpublic/sampleblob

Neben dem Hochladen eines Blockblobs aus einer NSString gibt es ähnliche Methoden für NSData, NSInputStream oder eine lokale Datei.

## Auflisten der Blobs in einem Container
Das folgende Beispiel zeigt, wie alle Blobs in einem Container aufgelistet werden. Achten Sie beim Durchführen dieses Vorgangs auf die folgenden Parameter:

- **ContinuationToken**: Das Fortsetzungstoken gibt an, wo der Auflistungsvorgang beginnen soll. Wenn kein Token angegeben wird, listet es Blobs vom Anfang auf. Es kann eine beliebige Anzahl von Blobs aufgeführt werden, von 0 (null) bis zu einem festgelegten Maximum. Auch wenn diese Methode keine Ergebnisse zurückgibt, wenn `results.continuationToken` nicht Null ist, gibt es möglicherweise mehr Blobs für den Dienst, die nicht aufgelistet wurden.
- **prefix** – Sie können das Präfix angeben, das für die Blobliste verwendet werden soll. Nur Blobs, die mit diesem Präfix beginnen, werden aufgelistet.
- **useFlatBlobListing** – Wie im Abschnitt [Benennen von Containern und Blobs und verweisen auf diese](#naming-and-referencing-containers-and-blobs) erwähnt, können Sie eine virtuelle Hierarchie durch Benennen von Blobs mit Pfadinformationen erstellen, selbst wenn der Blobdienst ein flaches Speicherschema ist. Allerdings wird derzeit eine nicht flache Auflistung nicht unterstützt. Dies ist in Kürze verfügbar. Bis dahin sollte der folgende Wert verwendet werden: `YES`.
- **blobListingDetails** – Sie können angeben, welche Elemente beim Auflisten der Blobs einbezogen werden sollen.
	- `AZSBlobListingDetailsNone`: Listen Sie nur zugesicherte Blobs auf, und geben Sie keine Blobmetadaten zurück.
	- `AZSBlobListingDetailsSnapshots`: Listen Sie zugesicherte Blobs und Blobmomentaufnahmen auf.
	- `AZSBlobListingDetailsMetadata`: Rufen Sie Blobmetadaten für jeden Blob auf, der in der Liste zurückgegeben wird.
	- `AZSBlobListingDetailsUncommittedBlobs`: Listen Sie zugesicherte und nicht zugesicherte Blobs auf.
	- `AZSBlobListingDetailsCopy`: Schließen Sie die Kopiereigenschaften in der Liste ein.
	- `AZSBlobListingDetailsAll`: Listen Sie alle verfügbaren zugesicherten Blobs, nicht zugesicherten Blobs und Momentaufnahmen auf, und geben Sie alle Metadaten und Kopierstatus für diese Blobs zurück.
- **maxResults** – Die maximale Anzahl von Ergebnissen, die für diesen Vorgang zurückgegeben werden. Verwenden Sie -1, um keinen Grenzwert festzulegen.
- **completionHandler** – Der auszuführende Codeblock mit den Ergebnissen des Auflistungsvorgangs.

In diesem Beispiel wird eine Hilfsmethode verwendet, um jedes Mal die Methode zum Auflisten von Blobs wiederholt aufzurufen, wenn ein Fortsetzungstoken zurückgegeben wird.

    -(void)listBlobsInContainer{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        //List all blobs in container
        [self listBlobsInContainerHelper:blobContainer continuationToken:nil prefix:nil blobListingDetails:AZSBlobListingDetailsAll maxResults:-1 completionHandler:^(NSError *error) {
            if (error != nil){
                NSLog(@"Error in creating container.");
            }
        }];
    }

    //List blobs helper method
    -(void)listBlobsInContainerHelper:(AZSCloudBlobContainer *)container continuationToken:(AZSContinuationToken *)continuationToken prefix:(NSString *)prefix blobListingDetails:(AZSBlobListingDetails)blobListingDetails maxResults:(NSUInteger)maxResults completionHandler:(void (^)(NSError *))completionHandler
    {
        [container listBlobsSegmentedWithContinuationToken:continuationToken prefix:prefix useFlatBlobListing:YES blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:^(NSError *error, AZSBlobResultSegment *results) {
            if (error)
            {
                completionHandler(error);
            }
            else
            {
                for (int i = 0; i < results.blobs.count; i++) {
                    NSLog(@"%@",[(AZSCloudBlockBlob *)results.blobs[i] blobName]);
                }
                if (results.continuationToken)
                {
                    [self listBlobsInContainerHelper:container continuationToken:results.continuationToken prefix:prefix blobListingDetails:blobListingDetails maxResults:maxResults completionHandler:completionHandler];
                }
                else
                {
                    completionHandler(nil);
                }
            }
        }];
    }


## Herunterladen eines Blobs

Das folgende Beispiel zeigt den Download eines Blobs in ein NSString-Objekt.

    -(void)downloadBlobToString{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob"];

        // Download blob
        [blockBlob downloadToTextWithCompletionHandler:^(NSError *error, NSString *text) {
            if (error) {
                NSLog(@"Error in downloading blob");
            }
            else{
                NSLog(@"%@",text);
            }
        }];
    }

## Löschen eines BLOBs

Das folgende Beispiel zeigt den Löschvorgang eines Blobs.

    -(void)deleteBlob{
        NSError *accountCreationError;

        // Create a storage account object from a connection string.
        AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

        if(accountCreationError){
            NSLog(@"Error in creating account.");
        }

        // Create a blob service client object.
        AZSCloudBlobClient *blobClient = [account getBlobClient];

        // Create a local container object.
        AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

        // Create a local blob object
        AZSCloudBlockBlob *blockBlob = [blobContainer blockBlobReferenceFromName:@"sampleblob1"];

        // Delete blob
        [blockBlob deleteWithCompletionHandler:^(NSError *error) {
            if (error) {
                NSLog(@"Error in deleting blob.");
            }
        }];
    }

## Löschen eines Blob-Containers

Das folgende Beispiel zeigt den Löschvorgang eines Containers.

    -(void)deleteContainer{
      NSError *accountCreationError;

      // Create a storage account object from a connection string.
      AZSCloudStorageAccount *account = [AZSCloudStorageAccount accountFromConnectionString:@"DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here" error:&accountCreationError];

      if(accountCreationError){
         NSLog(@"Error in creating account.");
      }

      // Create a blob service client object.
      AZSCloudBlobClient *blobClient = [account getBlobClient];

      // Create a local container object.
      AZSCloudBlobContainer *blobContainer = [blobClient containerReferenceFromName:@"containerpublic"];

      // Delete container
      [blobContainer deleteContainerIfExistsWithCompletionHandler:^(NSError *error, BOOL success) {
         if(error){
             NSLog(@"Error in deleting container");
         }
      }];
    }

## Nächste Schritte

Da Sie jetzt die Verwendung von Blobspeicher in iOS kennen, folgen Sie diesen Links, um weitere Informationen zur iOS-Bibliothek und dem Speicherdienst zu erhalten.

- [Azure Storage-Clientbibliothek für iOS](https://github.com/azure/azure-storage-ios)
- [Azure Storage-iOS-Referenzdokumentation](http://azure.github.io/azure-storage-ios/)
- [REST-API für Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/dd179355.aspx)
- [Azure Storage-Teamblog](http://blogs.msdn.com/b/windowsazurestorage)

Stellen Sie Fragen zu dieser Bibliothek in unserem [MSDN Azure-Forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=windowsazuredata) oder unter [Stack Overflow](http://stackoverflow.com/questions/tagged/windows-azure-storage+or+windows-azure-storage+or+azure-storage-blobs+or+azure-storage-tables+or+azure-table-storage+or+windows-azure-queues+or+azure-storage-queues+or+azure-storage-emulator+or+azure-storage-files). Richten Sie Vorschläge für Azure Storage-Features an [Feedback zu Azure Storage](https://feedback.azure.com/forums/217298-storage/).

<!---HONumber=AcomDC_0921_2016-->