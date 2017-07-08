---
title: Verwenden von Azure-Speicher in Windows Store-Apps | Microsoft Docs
description: Erfahren Sie, wie Sie eine Windows Store-App erstellen, die Blob, Queue, Table oder File Storage von Azure verwendet.
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 63c4b29d-b2f2-4d7c-b164-a0d38f4d14f6
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 12/08/2016
ms.author: marsma
ms.translationtype: Human Translation
ms.sourcegitcommit: ff0c9f9c800b0e4764efb88e43087bd96fa820e9
ms.openlocfilehash: 7b02809da6082886b4e2982a698cef09212b6862
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Verwenden von Azure Storage in Windows Store-Apps
## <a name="overview"></a>Übersicht
Dieser Leitfaden zeigt die ersten Schritte der Entwicklung einer Windows Store-App, die Azure Storage nutzt.

## <a name="download-required-tools"></a>Herunterladen erforderlicher Tools
* [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) vereinfacht das Erstellen, Debuggen, Lokalisieren, Packen und Bereitstellen von Windows Store-Apps. Visual Studio 2012 oder höher wird vorausgesetzt.
* Die [Azure Storage-Clientbibliothek](https://www.nuget.org/packages/WindowsAzure.Storage) stellt eine Windows-Runtime-Klassenbibliothek für die Arbeit mit Azure Storage bereit.
* [WCF Data Services-Tools für Windows Store-Apps](http://www.microsoft.com/download/details.aspx?id=30714) erweitern die Nutzung von „Dienstverweis hinzufügen“ durch clientseitige OData-Unterstützung für Windows Store-Apps in Visual Studio.

## <a name="develop-apps"></a>Entwickeln von Apps
### <a name="getting-ready"></a>Vorbereitung
Erstellen Sie ein neues Windows Store-App-Projekt in Visual Studio 2012 oder neuer:

![store-apps-storage-vs-project][store-apps-storage-vs-project]

Fügen Sie als Nächstes einen Verweis auf die Azure Storage-Clientbibliothek hinzu, indem Sie mit der rechten Maustaste auf **Verweise** klicken und dann **Verweis hinzufügen** auswählen. Wechseln Sie anschließend zur Speicherclientbibliothek für Windows-Runtime, die Sie heruntergeladen haben:

![store-apps-storage-choose-library][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Verwenden der Bibliothek mit den Blob- und Warteschlangendiensten
An diesem Punkt ist Ihre App bereit zum Aufruf der Blob- und Warteschlangendienste von Azure. Fügen Sie die folgenden **using** -Anweisungen hinzu, sodass direkt auf Azure Storage-Typen verwiesen werden kann:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
```

Fügen der Seite dann eine Schaltfläche hinzu. Fügen Sie dem **Click** -Ereignis den folgenden Code hinzu, und ändern Sie die Ereignishandlermethode mit dem [async-Schlüsselwort](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var blobClient = account.CreateCloudBlobClient();
var container = blobClient.GetContainerReference("container1");
await container.CreateIfNotExistsAsync();
```

Dieser Code setzt voraus, dass Sie die zwei Zeichenfolgenvariablen *accountName* und *accountKey* besitzen. Sie stehen für den Namen Ihres Speicherkontos und den Kontoschlüssel, der dem Konto zugeordnet ist.

Erstellen Sie die Anwendung, und führen Sie sie aus. Beim Klicken auf die Schaltfläche wird überprüft, ob ein Container namens *container1* in Ihrem Konto vorhanden ist. Andernfalls wird er erstellt.

### <a name="using-the-library-with-the-table-service"></a>Verwenden der Bibliothek mit dem Tabellenspeicherdienst
Die Typen, die für die Kommunikation mit dem Azure-Tabellenspeicherdienst verwendet werden, hängen von WCF Data Services für die Windows Store-Apps-Bibliothek ab. Fügen Sie dann mithilfe der Paket-Manager-Konsole einen Verweis zu den erforderlichen WCF-Bibliotheken hinzu:

![store-apps-storage-package-manager][store-apps-storage-package-manager]

Verweisen Sie Package Manager mithilfe des folgenden Befehls auf den Speicherort auf dem Computer:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Dieser Befehl fügt dem Projekt automatisch alle erforderlichen Verweise hinzu. Falls Sie die Paket-Manager-Konsole nicht verwenden möchten, können Sie auch den NuGet-Ordner von WCF Data Services auf dem lokalen Computer zur Liste der Paketquellen hinzufügen. Fügen Sie dann den Verweis über die im Thema zum [Verwalten von NuGet-Paketen mithilfe des Dialogs](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog) beschriebene Oberfläche hinzu.

Wenn Sie auf das NuGet-Paket von WCF Data Services verwiesen haben, ändern Sie den Code im **Click** -Ereignis der Schaltfläche:

```csharp
var credentials = new StorageCredentials(accountName, accountKey);
var account = new CloudStorageAccount(credentials, true);
var tableClient = account.CreateCloudTableClient();
var table = tableClient.GetTableReference("table1");
await table.CreateIfNotExistsAsync();
```

Dieser Code überprüft, ob eine Tabelle namens *table1* in Ihrem Konto vorhanden ist. Andernfalls wird sie erstellt.

Sie können auch einen Verweis zu „Microsoft.WindowsAzure.Storage.Table.dll“ hinzufügen. Diese Datei ist im gleichen heruntergeladenen Paket verfügbar. Diese Bibliothek enthält zusätzliche Funktionen, zum Beispiel Serialisierung und allgemeine Abfragen basierend auf Reflexion. Beachten Sie, dass diese Bibliothek JavaScript nicht unterstützt.

[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png

