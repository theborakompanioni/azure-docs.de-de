---
title: "Aktivieren von öffentlichem Lesezugriff auf Container und Blobs in Azure Blob Storage | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Container und Blobs für den anonymen Zugriff verfügbar machen und darauf programmgesteuert zugreifen."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a2cffee6-3224-4f2a-8183-66ca23b2d2d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: marsma
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 8d4f4c7c208baf0db6155eb78a53e37c4ec1e023
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>Verwalten des anonymen Lesezugriffs auf Container und Blobs
Sie können anonymen öffentlichen Lesezugriff auf einen Container und dessen Blobs in Azure Blob Storage aktivieren. Auf diese Weise können Sie schreibgeschützten Zugriff auf diese Ressourcen gewähren, ohne Ihren Kontoschlüssel freizugeben und eine Shared Access Signature (SAS) zu erfordern.

Öffentlicher Lesezugriff ist am besten für Szenarien, in denen bestimmte Blobs stets für anonymen Lesezugriff zur Verfügung stehen sollen. Für eine feiner abgestufte Zugriffssteuerung können Sie eine Shared Access Signature erstellen. Shared Access Signatures ermöglichen es Ihnen, eingeschränkten Zugriff mit unterschiedlichen Berechtigungen für einen bestimmten Zeitraum zu gewähren. Weitere Informationen zum Erstellen von Shared Access Signatures finden Sie unter [Verwenden von Shared Access Signatures (SAS) in Azure Storage](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>Erteilen von anonymen Benutzerberechtigungen für Container und Blobs
Standardmäßig kann nur der Besitzer eines Speicherkontos auf einen Container und alle darin enthaltenen Blobs zugreifen. Wenn anonyme Benutzer Leseberechtigungen für einen Container und die enthaltenen Blobs erhalten sollen, können Sie den öffentlichen Zugriff durch Festlegen der Containerberechtigungen gestatten. Anonyme Benutzer können Blobs innerhalb eines öffentlich zugänglichen Containers lesen, ohne dass die Anforderung authentifiziert werden muss.

Sie können einen Container mit den folgenden Berechtigungen konfigurieren:

* **Kein öffentlicher Lesezugriff:** Nur der Speicherkontobesitzer kann auf den Container und dessen Blobs zugreifen. Dies ist die Standardeinstellung für alle neuen Container.
* **Öffentlicher Lesezugriff nur für Blobs:** Blobs im Container können über anonyme Anforderungen gelesen werden, Containerdaten sind jedoch nicht verfügbar. Anonyme Clients können die Blobs im Container nicht aufzählen.
* **Vollständiger öffentlicher Lesezugriff:** Alle Container- und Blobdaten können über anonyme Anforderungen gelesen werden. Clients können Blobs im Container über anonyme Anforderungen aufzählen, können aber keine Container im Speicherkonto aufzählen.

Sie können Folgendes verwenden, um Containerberechtigungen festzulegen:

* [Azure-Portal](https://portal.azure.com)
* [Azure PowerShell](../common/storage-powershell-guide-full.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#how-to-manage-azure-blobs)
* [Azure CLI 2.0](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#create-and-manage-blobs)
* Programmgesteuert, indem Sie eine der Speicherclientbibliotheken oder die REST-API verwenden

### <a name="set-container-permissions-in-the-azure-portal"></a>Festlegen von Containerberechtigungen im Azure-Portal
Um Containerberechtigungen im [Azure-Portal](https://portal.azure.com) festzulegen, führen Sie diese Schritte aus:

1. Öffnen Sie im Portal das Blatt **Speicherkonto**. Sie finden Ihr Speicherkonto, indem Sie auf dem Menüblatt des Hauptportals die Option **Speicherkonten** auswählen.
1. Wählen Sie auf dem Menüblatt unter **BLOB-DIENST** die Option **Container** aus.
1. Klicken Sie mit der rechten Maustaste auf die Containerzeile, oder wählen Sie die Auslassungspunkte aus, um das **Kontextmenü** des Containers zu öffnen.
1. Wählen Sie im Kontextmenü den Befehl **Zugriffsrichtlinie** aus.
1. Wählen Sie im Dropdownmenü einen **Zugriffstyp** aus.

    ![Dialogfeld „Containermetadaten bearbeiten“](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a>Festlegen von Containerberechtigungen mit .NET
Um Berechtigungen für einen Container mit C# und der Speicherclientbibliothek für .NET festzulegen, rufen Sie zunächst die vorhandenen Berechtigungen des Containers ab, indem Sie die **GetPermissions**-Methode aufrufen. Legen Sie dann die **PublicAccess**-Eigenschaft für das **BlobContainerPermissions**-Objekt fest, das von der **GetPermissions**-Methode zurückgegeben wird. Rufen Sie abschließend die **SetPermissions** -Methode mit den aktualisierten Berechtigungen auf.

Im folgenden Beispiel werden die Berechtigungen des Containers auf vollständigen öffentlichen Lesezugriff festgelegt. Legen Sie zum Festlegen von Berechtigungen auf den öffentlichen Lesezugriff nur für Blobs die **PublicAccess**-Eigenschaft auf **BlobContainerPublicAccessType.Blob** fest. Um alle Berechtigungen für anonyme Benutzer zu entfernen, legen Sie die Eigenschaft auf **BlobContainerPublicAccessType.Off**fest.

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a>Anonymer Zugriff auf Container und Blobs
Ein Client, der auf Container und Blobs anonym zugreift, kann Konstruktoren verwenden, für die keine Anmeldeinformationen erforderlich sind. In den folgenden Beispielen werden verschiedene Möglichkeiten zum anonymen Verweisen auf Blob-Dienstressourcen gezeigt.

### <a name="create-an-anonymous-client-object"></a>Erstellen eines anonymen Clientobjekts
Sie können ein neues Dienstclientobjekt für den anonymen Zugriff durch Bereitstellen des Blob-Dienstendpunkts für das Konto erstellen. Allerdings müssen Sie auch den Namen eines Containers in diesem Konto kennen, der für den anonymen Zugriff verfügbar ist.

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create the client object using the Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read the container's properties. Note this is only possible when the container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a>Anonymes Verweisen auf einen Container
Wenn Sie über die URL zu einem Container verfügen, der anonym verfügbar ist, können Sie damit direkt auf den Container verweisen.

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference to a container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in the container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a>Anonymes Verweisen auf ein Blob
Wenn Sie über die URL zu einem Blob verfügen, das für den anonymen Zugriff verfügbar ist, können Sie damit über diese URL direkt auf das Blob verweisen:

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-to-anonymous-users"></a>Für anonyme Benutzer verfügbare Features
Die folgende Tabelle zeigt, welche Vorgänge von anonymen Benutzern aufgerufen werden können, wenn der öffentliche Zugriff in der Zugriffssteuerungsliste (Access Control List, ACL) eines Containers gestattet wurde.

| REST-Vorgang | Berechtigung mit vollständigem öffentlichen Lesezugriff | Berechtigung mit öffentlichem Lesezugriff nur für Blobs |
| --- | --- | --- |
| List Containers |Nur Besitzer |Nur Besitzer |
| Create Container |Nur Besitzer |Nur Besitzer |
| Get Container Properties |Alle |Nur Besitzer |
| Get Container Metadata |Alle |Nur Besitzer |
| Set Container Metadata |Nur Besitzer |Nur Besitzer |
| Get Container ACL |Nur Besitzer |Nur Besitzer |
| Set Container ACL |Nur Besitzer |Nur Besitzer |
| Delete Container |Nur Besitzer |Nur Besitzer |
| List Blobs |Alle |Nur Besitzer |
| Put Blob |Nur Besitzer |Nur Besitzer |
| Get Blob |Alle |Alle |
| Get Blob Properties |Alle |Alle |
| Set Blob Properties |Nur Besitzer |Nur Besitzer |
| Get Blob Metadata |Alle |Alle |
| Set Blob Metadata |Nur Besitzer |Nur Besitzer |
| Put Block |Nur Besitzer |Nur Besitzer |
| Get Block List (nur Blöcke mit ausgeführtem Commit) |Alle |Alle |
| Get Block List (nur Blöcke ohne ausgeführten Commit oder alle Blöcke) |Nur Besitzer |Nur Besitzer |
| Put Block List |Nur Besitzer |Nur Besitzer |
| Delete Blob |Nur Besitzer |Nur Besitzer |
| Kopieren von Blobs |Nur Besitzer |Nur Besitzer |
| Snapshot Blob |Nur Besitzer |Nur Besitzer |
| Lease Blob |Nur Besitzer |Nur Besitzer |
| Put Page |Nur Besitzer |Nur Besitzer |
| Get Page Ranges |Alle |Alle |
| Blob anfügen |Nur Besitzer |Nur Besitzer |

## <a name="next-steps"></a>Nächste Schritte

* [Authentifizierung für Azure Storage-Dienste](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [Verwenden von Shared Access Signatures (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
* [Delegieren des Zugriffs mit einer Shared Access Signature](https://msdn.microsoft.com/library/azure/ee395415.aspx)

