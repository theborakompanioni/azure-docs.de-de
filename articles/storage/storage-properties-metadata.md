---
title: "Festlegen und Abrufen von Eigenschaften und Metadaten für Objekte in Azure Storage | Microsoft Docs"
description: "Speichern Sie benutzerdefinierten Metadaten für Objekte in Azure Storage, legen Sie Systemeigenschaften fest, und rufen Sie diese ab."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 036f9006-273e-400b-844b-3329045e9e1f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 3868d36948342739eb78b013bb4b466df4381b4f
ms.openlocfilehash: 7c1ca950c3ab1b8ffb754a74597d45b82777838c
ms.lasthandoff: 02/15/2017


---
# <a name="set-and-retrieve-properties-and-metadata"></a>Festlegen und Abrufen von Eigenschaften und Metadaten
## <a name="overview"></a>Übersicht
Objekte in Azure-Speicher unterstützen zusätzlich zu den Daten, die sie enthalten, Systemeigenschaften und benutzerdefinierte Metadaten:

* **Systemeigenschaften.** Systemeigenschaften sind in jeder Speicherressource vorhanden. Einige davon können gelesen oder festgelegt werden, während andere schreibgeschützt sind. Darüber hinaus entsprechen einige Systemeigenschaften bestimmten HTTP-Standardheadern. Die Azure-Speicher-Clientbibliothek verwaltet diese für Sie.
* **Benutzerdefinierte Metadaten.** Benutzerdefinierte Metadaten sind Metadaten, die für eine bestimmte Ressource in Form von Name-Wert-Paaren angegeben werden. Sie können Metadaten verwenden, um zusätzliche Werte für eine Speicherressource zu speichern. Diese Werte dienen nur den von Ihnen festgelegten Zwecken und haben keine Auswirkungen auf das Verhalten der Ressource.

Das Abrufen von Eigenschafts- und Metadatenwerten einer Speicherressource ist ein zweistufiger Prozess. Bevor Sie diese Werte lesen können, müssen Sie sie explizit durch Aufrufen der **FetchAttributes** -Methode abrufen.

> [!IMPORTANT]
> Eigenschafts- und Metadatenwerte werden bei einer Speicherressource nicht aufgefüllt, sofern Sie nicht eine der **FetchAttributes** -Methoden aufrufen.
>
> Sie erhalten die Meldung `400 Bad Request`, wenn Name-Wert-Paare Nicht-ASCII-Zeichen enthalten. Name-Wert-Paare für Metadaten sind gültige HTTP-Header und müssen daher allen Einschränkungen für HTTP-Header entsprechen. Es wird daher empfohlen, für Namen und Werte mit Nicht-ASCII-Zeichen die URL- oder Base64-Codierung zu wählen.
>

## <a name="setting-and-retrieving-properties"></a>Festlegen und Abrufen von Eigenschaften
Zum Abrufen von Eigenschaftswerten rufen Sie die **FetchAttributes** -Methode für das Blob oder den Container auf, um die Eigenschaften aufzufüllen, und lesen anschließend die Werte.

Um Eigenschaften für ein Objekt festzulegen, geben Sie den Eigenschaftswert an und rufen dann die **SetProperties** -Methode auf.

Das folgende Codebeispiel erstellt einen Container und schreibt einige der zugehörigen Eigenschaftswerte in ein Konsolenfenster:

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create the service client object for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
container.FetchAttributes();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a>Festlegen und Abrufen von Metadaten
Sie können Metadaten als ein oder mehrere Name-Wert-Paare für ein Blob oder einen Container angeben. Fügen Sie zum Festlegen von Metadaten Name-Wert-Paare zur **Metadaten**-Auflistung der Ressource hinzu, und rufen Sie dann die **SetMetadata**-Methode auf, um die Werte für den Dienst zu speichern.

> [!NOTE]
> Der Name der Metadaten muss den Benennungskonventionen für C#-Bezeichner entsprechen.
>
>

Das folgende Codebeispiel legt die Metadaten für einen Container fest. Mittels der **Hinzufügen** -Methode der Sammlung wird ein Wert festgelegt. Der andere Wert wird mit der impliziten Schlüssel-Wert-Syntax festgelegt. Beide eignen sich hierzu.

```csharp
public static void AddContainerMetadata(CloudBlobContainer container)
{
    //Add some metadata to the container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    //Set the container's metadata.
    container.SetMetadata();
}
```

Zum Abrufen von Metadaten rufen Sie die **FetchAttributes**-Methode für das Blob oder den Container auf, um die **Metadaten**-Auflistung zu füllen, und lesen anschließend die Werte wie im unten stehenden Beispiel gezeigt.

```csharp
public static void ListContainerMetadata(CloudBlobContainer container)
{
    //Fetch container attributes in order to populate the container's properties and metadata.
    container.FetchAttributes();

    //Enumerate the container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="see-also"></a>Weitere Informationen
* [Azure Storage-Clientbibliothek für .NET-Referenz](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
* [Azure Storage-Clientbibliothek für .NET-Paket](https://www.nuget.org/packages/WindowsAzure.Storage/)


