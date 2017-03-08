---
title: "Erstellen einer schreibgeschützten Momentaufnahme eines Blobs | Microsoft Docs"
description: "Erfahren Sie, wie Sie eine Momentaufnahme eines Blobs erstellen, um Blobdaten zu einem angegebenen Zeitpunkt zu sichern. Sie erhalten grundlegende Informationen dazu, wie Momentaufnahmen in Rechnung gestellt und wie Sie mithilfe von Momentaufnahmen Ihre Kapazitätskosten minimieren können."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 3710705d-e127-4b01-8d0f-29853fb06d0d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: a8b570cfbab594e1a21417e081eaf6d34a4659d0
ms.openlocfilehash: 40f10b1cb860ca0c018cc3589417c54588269b0c
ms.lasthandoff: 02/28/2017


---
# <a name="create-a-blob-snapshot"></a>Erstellen einer Momentaufnahme eines Blobs
## <a name="overview"></a>Übersicht
Eine Momentaufnahme ist eine schreibgeschützte Version eines Blobs, die zu einem bestimmten Zeitpunkt erstellt wird. Momentaufnahmen sind nützlich, um Blobs zu sichern. Nach dem Erstellen einer Momentaufnahme kann sie gelesen, kopiert oder gelöscht, aber nicht mehr geändert werden.

Eine Momentaufnahme eines Blobs ist mit dem dazugehörigen Basisblob bis auf die Ausnahme identisch, dass an den Blob-URI ein **DateTime**-Wert angefügt ist. Hiermit wird der Zeitpunkt angegeben, zu dem die Momentaufnahme erstellt wurde. Wenn der Seitenblob-URI `http://storagesample.core.blob.windows.net/mydrives/myvhd` ist, lautet der Momentaufnahmen-URI z.B. in etwa `http://storagesample.core.blob.windows.net/mydrives/myvhd?snapshot=2011-03-09T01:42:34.9360000Z`.

> [!NOTE]
> Für alle Momentaufnahmen wird der URI des Basisblobs verwendet. Der einzige Unterschied zwischen dem Basisblob und der Momentaufnahme ist der angefügte **DateTime** -Wert.
>
>

Für ein Blob kann eine beliebige Anzahl von Momentaufnahmen vorhanden sein. Momentaufnahmen bleiben bestehen, bis sie explizit gelöscht werden, Eine Momentaufnahme kann das dazugehörige Basisblob nicht überleben. Sie können alle einem Basisblob zugeordneten Momentaufnahmen auflisten, um die aktuell vorhandenen Momentaufnahmen nachzuverfolgen.

Wenn Sie eine Momentaufnahme eines Blobs erstellen, werden seine Systemeigenschaften mit denselben Werten in die Momentaufnahme kopiert. Die Metadaten des Basisblobs werden auch in die Momentaufnahme kopiert, sofern Sie beim Erstellen keine separaten Metadaten für die Momentaufnahme angeben.

Dem Basis-Blob zugeordnete Leases wirken sich nicht auf die Momentaufnahme aus. Sie können für eine Momentaufnahme keine Lease abrufen.

Für das Speichern der aktuellen Informationen und des Status eines VM-Datenträgers wird eine VHD-Datei verwendet. Sie können einen Datenträger vom virtuellen Computer trennen oder die VM herunterfahren und dann eine Momentaufnahme der VHD-Datei erstellen. Sie können diese Momentaufnahmedatei später verwenden, um die VHD-Datei zu diesem Zeitpunkt abzurufen und den virtuellen Computer neu zu erstellen.

Wenn Storage Service Encryption (SSE) für das Speicherkonto aktiviert ist, in dem sich das Blob befindet, werden Momentaufnahmen des Blobs im Ruhezustand verschlüsselt.

## <a name="create-a-snapshot"></a>Erstellen einer Momentaufnahme
Das folgende Codebeispiel zeigt, wie Sie eine Momentaufnahme in .NET erstellen. In diesem Beispiel werden separate Metadaten für die Momentaufnahme angegeben, wenn sie erstellt wird.

```csharp
private static async Task CreateBlockBlobSnapshot(CloudBlobContainer container)
{
    // Create a new block blob in the container.
    CloudBlockBlob baseBlob = container.GetBlockBlobReference("sample-base-blob.txt");

    // Add blob metadata.
    baseBlob.Metadata.Add("ApproxBlobCreatedDate", DateTime.UtcNow.ToString());

    try
    {
        // Upload the blob to create it, with its metadata.
        await baseBlob.UploadTextAsync(string.Format("Base blob: {0}", baseBlob.Uri.ToString()));

        // Sleep 5 seconds.
        System.Threading.Thread.Sleep(5000);

        // Create a snapshot of the base blob.
        // Specify metadata at the time that the snapshot is created to specify unique metadata for the snapshot.
        // If no metadata is specified when the snapshot is created, the base blob's metadata is copied to the snapshot.
        Dictionary<string, string> metadata = new Dictionary<string, string>();
        metadata.Add("ApproxSnapshotCreatedDate", DateTime.UtcNow.ToString());
        await baseBlob.CreateSnapshotAsync(metadata, null, null, null);
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

## <a name="copy-snapshots"></a>Kopieren von Momentaufnahmen
Für Kopiervorgänge, die Blobs und Momentaufnahmen betreffen, gelten folgende Regeln:

* Sie können eine Momentaufnahme über das zugehörige Basis-Blob kopieren. Indem Sie eine Momentaufnahme zu einem Basis-Blob heraufstufen, können Sie eine frühere Version eines Blobs wiederherstellen. Die Momentaufnahme bleibt erhalten, ihre Quelle wird jedoch durch eine Kopie überschrieben, die gelesen und geschrieben werden kann.
* Sie können eine Momentaufnahme in ein Ziel-Blob mit einem anderen Namen kopieren. Das resultierende Ziel-Blob ist ein beschreibbares Blob und keine Momentaufnahme.
* Wenn ein Quell-Blob kopiert wird, werden ggf. vorhandene Momentaufnahmen des Quell-Blobs nicht mit in das Ziel kopiert. Wenn ein Zielblob durch eine Kopie überschrieben wird, bleiben alle dem ursprünglichen Zielblob zugeordneten Momentaufnahmen erhalten.
* Wenn Sie eine Momentaufnahme eines Block-Blobs erstellen, wird die Liste der Blöcke mit ausgeführtem Commit für das Blob ebenfalls in die Momentaufnahme kopiert. Blöcke ohne ausgeführten Commit werden nicht kopiert.

## <a name="specify-an-access-condition"></a>Angeben einer Zugriffsbedingung
Sie können eine Zugriffsbedingung angeben, sodass die Momentaufnahme nur erstellt wird, wenn eine bestimmte Bedingung erfüllt ist. Zum Angeben einer Zugriffsbedingung verwenden Sie die **AccessCondition** -Eigenschaft. Wenn die angegebene Bedingung nicht erfüllt ist, wird die Momentaufnahme nicht erstellt, und der Blob-Dienst gibt den Statuscode "HTTPStatusCode.PreconditionFailed" zurück.

## <a name="delete-snapshots"></a>Löschen von Momentaufnahmen
Das Löschen eines Blobs mit Momentaufnahmen ist nur möglich, wenn auch die Momentaufnahmen gelöscht werden. Sie können eine Momentaufnahme einzeln löschen oder angeben, dass alle Momentaufnahmen gelöscht werden sollen, wenn das Quellblob gelöscht wird. Wenn Sie versuchen, ein Blob zu löschen, für das noch Momentaufnahmen vorhanden sind, tritt ein Fehler auf.

Im folgenden Codebeispiel wird veranschaulicht, wie Sie ein Blob und die dazugehörigen Momentaufnahmen in .NET löschen, wenn `blockBlob` eine Variable vom Typ **CloudBlockBlob**ist:

```csharp
await blockBlob.DeleteIfExistsAsync(DeleteSnapshotsOption.IncludeSnapshots, null, null, null);
```

## <a name="snapshots-with-azure-premium-storage"></a>Momentaufnahmen mit Azure Premium-Speicher
Die Verwendung von Momentaufnahmen mit Premium-Speicher unterliegt den folgenden Regeln:

* Die maximale Anzahl von Momentaufnahmen pro Seitenblob in einem Premium-Speicherkonto beträgt 100. Wenn diese Grenze überschritten wird, gibt der Momentaufnahmen-Blobvorgang Fehlercode 409 (**SnapshotCountExceeded**) zurück.
* Sie können unter einem Premium-Speicherkonto ca. alle zehn Minuten eine Momentaufnahme eines Seitenblobs erstellen. Wenn diese Grenze überschritten wird, gibt der Momentaufnahmen-Blobvorgang Fehlercode 409 (**SnaphotOperationRateExceeded**) zurück.
* Zum Lesen einer Momentaufnahme können Sie eine Momentaufnahme mithilfe des Copy Blob-Vorgangs in ein anderes Seitenblob im Konto kopieren. Das Ziel-Blob für den Kopiervorgang darf keine vorhandenen Momentaufnahmen besitzen. Wenn das Zielblob Momentaufnahmen aufweist, gibt der Copy Blob-Vorgang den Fehlercode 409 (**SnapshotsPresent**) zurück.

## <a name="return-the-absolute-uri-to-a-snapshot"></a>Zurückgeben des absoluten URI zu einer Momentaufnahme
Dieses C#-Codebeispiel erstellt eine Momentaufnahme und schreibt den absoluten URI für den primären Standort.

```csharp
//Create the blob service client object.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";

CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

//Get a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
container.CreateIfNotExists();

//Get a reference to a blob.
CloudBlockBlob blob = container.GetBlockBlobReference("sampleblob.txt");
blob.UploadText("This is a blob.");

//Create a snapshot of the blob and write out its primary URI.
CloudBlockBlob blobSnapshot = blob.CreateSnapshot();
Console.WriteLine(blobSnapshot.SnapshotQualifiedStorageUri.PrimaryUri);
```

## <a name="understand-how-snapshots-accrue-charges"></a>Grundlegendes zur Ermittlung der Gebühren für Momentaufnahmen
Durch das Erstellen einer Momentaufnahme, die eine schreibgeschützte Kopie eines BLOBs darstellt, können auf Ihrem Konto zusätzliche Gebühren für die Datenspeicherung anfallen. Wenn Sie beim Entwurf Ihrer Anwendung die Art und Weise der Ermittlung dieser Gebühren berücksichtigen, können Sie unnötige Kosten minimieren.

### <a name="important-billing-considerations"></a>Wichtige Überlegungen zur Abrechnung
Die folgende Liste enthält wichtige Punkte, die beim Erstellen einer Momentaufnahme zu berücksichtigen sind.

* Für Ihr Speicherkonto fallen Gebühren für eindeutige Blöcke oder Seiten an, die im Blob oder in der Momentaufnahme enthalten sind. Ihrem Konto werden erst dann zusätzliche Gebühren für Momentaufnahmen angerechnet, die einem BLOB zugeordnet sind, wenn Sie das BLOB aktualisieren, auf dem sie basieren. Sobald Sie das Basisblob aktualisieren, entstehen Abweichungen von den zugeordneten Momentaufnahmen. In diesem Fall werden für alle eindeutigen Blöcke oder Seiten in jedem Blob bzw. einer Momentaufnahme Gebühren berechnet.
* Wenn Sie einen Block innerhalb eines Block-BLOBs ersetzen, wird dieser Block anschließend als eindeutiger Block berechnet. Dies gilt auch, wenn der Block dieselbe Block-ID und dieselben Daten enthält wie in der Momentaufnahme. Nachdem ein erneuter Commit für den Block ausgeführt wurde, weicht er von seinem Pendant in den Momentaufnahmen ab, und Ihnen werden die Daten des Blocks berechnet. Das gleiche gilt für eine Seite in einem Seiten-BLOB, die mit identischen Daten aktualisiert wird.
* Wenn Sie ein Blockblob durch einen Aufruf der Methode **UploadFile**, **UploadText**, **UploadStream** oder **UploadByteArray** ersetzen, werden alle Blöcke im Blob ersetzt. Wenn dem Blob eine Momentaufnahme zugeordnet ist, weisen anschließend alle Blöcke im Basisblob und in der Momentaufnahme Abweichungen auf, und Ihnen werden Gebühren für alle Blöcke in beiden Blobs berechnet. Dies gilt auch, wenn die Daten im Basis-BLOB und in der Momentaufnahme identisch sind.
* Der Azure-Blob-Dienst kann nicht feststellen, ob zwei Blöcke identische Daten enthalten. Jeder hochgeladene Block, für den ein Commit ausgeführt wird, wird als eindeutig behandelt, selbst wenn die enthaltenen Daten und die Block-ID identisch sind. Da Gebühren jeweils für eindeutige Blöcke berechnet werden, ist zu berücksichtigen, dass beim Aktualisieren eines Blobs mit einer zugeordneten Momentaufnahme zusätzliche eindeutige Blöcke generiert werden, für die zusätzliche Gebühren entstehen.

> [!NOTE]
> Planen und verwalten Sie Momentaufnahmen sorgfältig, um unnötige Gebühren zu vermeiden. Es wird empfohlen, Momentaufnahmen folgendermaßen zu verwalten:
>
> * Löschen und erstellen Sie zugehörige Momentaufnahmen für ein BLOB neu, wenn Sie das BLOB aktualisieren, selbst wenn Sie mit identischen Daten aktualisieren, es sei denn, der Anwendungsentwurf erfordert, dass die Momentaufnahmen beibehalten werden. Durch das Löschen und Neuerstellen der Momentaufnahmen für ein BLOB können Sie sicherstellen, dass das BLOB und die Momentaufnahmen nicht voneinander abweichen.
> * Wenn Sie Momentaufnahmen für ein Blob beibehalten, sollten Sie Aufrufe von **UploadFile**, **UploadText**, **UploadStream** und **UploadByteArray** zum Aktualisieren des Blobs vermeiden. Bei diesen Methoden werden alle Blöcke im Blob ersetzt, sodass Ihr Basisblob und Momentaufnahmen erheblich voneinander abweichen. Aktualisieren Sie stattdessen so wenig Blöcke wie möglich, indem Sie die **PutBlock**-Methode oder die **PutBlockList**-Methode aufrufen.
>
>

### <a name="snapshot-billing-scenarios"></a>Abrechnungsszenarien für Momentaufnahmen
Die folgenden Szenarien veranschaulichen, wie Gebühren für ein Block-BLOB und zugehörige Momentaufnahmen berechnet werden.

In Szenario 1 wurde das Basis-Blob nicht aktualisiert, nachdem die Momentaufnahme erstellt wurde, sodass Gebühren nur für die eindeutigen Blöcke 1, 2 und 3 berechnet werden:

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-1.png)

In Szenario 2 wurde das Basisblob aktualisiert, die Momentaufnahme jedoch nicht. Block 3 wurde aktualisiert. Obwohl er die gleichen Daten und dieselbe ID enthält, ist er nicht identisch mit dem Block 3 der Momentaufnahme. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-2.png)

In Szenario 3 wurde das Basisblob aktualisiert, die Momentaufnahme jedoch nicht. Block 3 wurde im Basis-BLOB durch Block 4 ersetzt, die Momentaufnahme enthält aber immer noch den Block 3. Daher wird das Konto mit Gebühren für vier Blöcke belastet:

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-3.png)

In Szenario 4 wurde das Basis-Blob vollständig aktualisiert und enthält keinen der ursprünglichen Blöcke. Daher wird das Konto für alle acht eindeutigen Blöcke belastet. Dieses Szenario kann auftreten, wenn Sie eine der Updatemethoden **UploadFile**, **UploadText**, **UploadFromStream** oder **UploadByteArray** verwenden, da diese Methoden alle Inhalte eines Blobs ersetzen.

![Azure Storage-Ressourcen](./media/storage-blob-snapshots/storage-blob-snapshots-billing-scenario-4.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Beispiele zur Verwendung von Blobspeicher finden Sie unter [Azure-Codebeispiele](https://azure.microsoft.com/documentation/samples/?service=storage&term=blob). Sie können eine Beispielanwendung herunterladen und ausführen oder den Code auf GitHub durchsuchen.


