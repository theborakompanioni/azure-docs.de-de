---
title: Verwalten des Ablaufs von Azure Storage-Blobinhalten in Azure CDN | Microsoft Docs
description: "Informationen zu den Optionen für das Festlegen der Gültigkeitsdauer von Blobs beim Azure CDN-Zwischenspeichern."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: ad4801e9-d09a-49bf-b35c-efdc4e6034e8
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 09/15/2016
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 109ca4a4672d21969096af26a094390673de25d9
ms.openlocfilehash: 751db56ee29ebc635d4d2a27dd18f99f7efb5a34


---
# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>Verwalten des Ablaufs von Azure Storage-Blobinhalten in Azure CDN
> [!div class="op_single_selector"]
> * [Azure-Web-Apps/Cloud Services, ASP.NET oder IIS](cdn-manage-expiration-of-cloud-service-content.md)
> * [Azure Storage Blob-Dienst](cdn-manage-expiration-of-blob-content.md)
> 
> 

Der [Blobdienst](../storage/storage-introduction.md#blob-storage) in [Azure Storage](../storage/storage-introduction.md) ist einer von verschiedenen Azure-basierten Ursprüngen, die in Azure CDN integriert wurden.  Jeder öffentlich zugängliche Blobinhalt kann in Azure CDN zwischengespeichert werden, bis seine Gültigkeitsdauer abläuft.  Die Gültigkeitsdauer wird durch den [*Cache-Control* -Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) in der HTTP-Antwort von Azure Storage bestimmt.

> [!TIP]
> Sie haben auch die Möglichkeit, für einen Blob keine Gültigkeitsdauer festzulegen.  In diesem Fall wendet Azure CDN automatisch eine Standardgültigkeitsdauer von sieben Tagen an.
> 
> Weitere Informationen dazu, wie Azure CDN den Zugriff auf Blobs und andere Dateien beschleunigen kann, finden Sie in der [Übersicht über das Azure Content Delivery Network (CDN)](cdn-overview.md).
> 
> Weitere Informationen zum Azure Storage-Blobdienst finden Sie unter [Konzepte des Blobdiensts](https://msdn.microsoft.com/library/dd179376.aspx). 
> 
> 

Dieses Tutorial zeigt verschiedene Methoden, mit denen Sie die Gültigkeitsdauer für ein Blob in Azure Storage festlegen können.  

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azureps-cmdlets-docs) ist eine der schnellsten und leistungsstärksten Möglichkeiten zum Verwalten Ihrer Azure-Dienste.  Verwenden Sie das `Get-AzureStorageBlob`-Cmdlet zum Abrufen eines Verweises auf das Blob, und legen Sie dann die Eigenschaft `.ICloudBlob.Properties.CacheControl` fest. 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

> [!TIP]
> Sie können auch PowerShell zum [Verwalten Ihrer CDN-Profile und Endpunkte](cdn-manage-powershell.md) verwenden.
> 
> 

## <a name="azure-storage-client-library-for-net"></a>Azure Storage-Clientbibliothek für .NET
Zum Bestimmten der Gültigkeitsdauer eines Blobs mithilfe von .NET verwenden Sie die [Azure Storage-Clientbibliothek für .NET](../storage/storage-dotnet-how-to-use-blobs.md), um die Eigenschaft [CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx) festzulegen.

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);

        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

> [!TIP]
> Unter [Azure Blob Storage Samples for .NET](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)(Azure-Blobspeicher-Beispiele für .NET) finden Sie weitere .NET-Codebeispiele.
> 
> 

## <a name="other-methods"></a>Andere Methoden
* [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)
  
    Legen Sie beim Hochladen von Blobs die Eigenschaft *cacheControl* mit dem Switch `-p` fest.  In diesem Beispiel wird die Gültigkeitsdauer auf eine Stunde (3.600 Sekunden) festgesetzt.
  
    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```
* [REST-API für Azure-Speicherdienste](https://msdn.microsoft.com/library/azure/dd179355.aspx)
  
    Legen Sie explizit die Eigenschaft *x-ms-blob-cache-control* für eine Anforderung des Typs [Put Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx), [Put Block List](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx) oder [Set Blob Properties](https://msdn.microsoft.com/library/azure/ee691966.aspx) fest.
* Speicherverwaltungstools von Drittanbietern
  
    Einige Azure Storage-Verwaltungstools von Drittanbietern erlauben es Ihnen, die Eigenschaft *CacheControl* für Blobs festzulegen. 

## <a name="testing-the-cache-control-header"></a>Testen des *Cache-Control* -Headers
Sie können die Gültigkeitsdauer Ihrer Blobs einfach überprüfen.  Mithilfe der [Entwicklertools](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)Ihres Browsers können Sie prüfen, ob Ihr Blob den *Cache-Control* -Antwortheader enthält.  Sie können auch ein Tool wie **wget**, [Postman](https://www.getpostman.com/) oder [Fiddler](http://www.telerik.com/fiddler) verwenden, um die Antwortheader zu untersuchen.

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum *Cache-Control*-Header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [Erfahren Sie, wie Sie den Ablauf von Clouddienstinhalten in Azure CDN verwalten können.](cdn-manage-expiration-of-cloud-service-content.md)




<!--HONumber=Dec16_HO2-->


