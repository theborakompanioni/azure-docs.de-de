---
title: Azure Storage-Beispiele mit Java | Microsoft Docs
description: "Sie können Beispielcode und -anwendungen für Azure Storage anzeigen, herunterladen und ausführen. Entdecken Sie Beispiele für erste Schritte mit Blobs, Warteschlangen, Tabellen und Dateien, indem Sie die Speicherclientbibliotheken für Java verwenden."
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: 
ms.service: storage
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 01/12/2017
ms.author: seguler
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: fd27e1ac9a773e7b0f5245aa74acdb0521cd098c
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="azure-storage-samples-using-java"></a>Azure Storage-Beispiele mit Java

## <a name="java-sample-index"></a>Index für Java-Beispiele

Die folgende Tabelle enthält eine Übersicht über unser Beispielrepository und in jedem Beispiel behandelten Szenarien. Klicken Sie auf die Links, um den zugehörigen Beispielcode in GitHub anzuzeigen.

<table style="font-size:90%"><thead><tr><th style="font-size:110%">Endpunkt</th><th style="font-size:110%">Szenario</th><th style="font-size:110%">Beispielcode</th></tr></thead><tbody> 
<tr> 
<td rowspan="16"><b>Blob</b></td>
<td>Blob anfügen</td> 
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td> 
</tr> 
<tr> 
<td>Blockblob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td>Clientseitige Verschlüsselung</td>
<td><a href="https://github.com/Azure-Samples/storage-java-client-side-encryption">Erste Schritte mit clientseitiger Azure-Verschlüsselung in Java</a></td>
</tr> 
<tr> 
<td>Kopieren von Blobs</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td>Create Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td>Delete Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td>Delete Container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td>Blobmetadaten/-eigenschaften/-statistiken</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td>Container-ACL/Metadaten/Eigenschaften</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td>Get Page Ranges</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/CloudPageBlobTests.java">Beispiel für Seitenblobtests</a></td>
</tr> 
<tr> 
<td>Leaseblob/-container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td>Listenblob/-container</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td>Seitenblob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr>
<tr> 
<td>SAS</td>
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-test/src/com/microsoft/azure/storage/blob/SasTests.java">Beispiel für SAS-Tests</a></td>
</tr>   
<tr> 
<td>Diensteigenschaften</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobAdvanced.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr>           
<tr> 
<td>Snapshot Blob</td>
<td><a href="https://github.com/Azure-Samples/storage-blob-java-getting-started/blob/master/src/BlobBasics.java">Erste Schritte mit dem Azure Blob-Dienst in Java</a></td>
</tr> 
<tr> 
<td rowspan="9"><b>Datei</b></td>
<td>Erstellen von Freigaben/Verzeichnissen/Dateien</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Erste Schritte mit dem Azure-Dateidienst in Java</a></td> 
</tr>
<tr> 
<td>Löschen von Freigaben/Verzeichnissen/Dateien</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Erste Schritte mit dem Azure-Dateidienst in Java</a></td> 
</tr> 
<tr> 
<td>Verzeichniseigenschaften/-metadaten</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Erste Schritte mit dem Azure-Dateidienst in Java</a></td> 
</tr> 
<tr> 
<td>Herunterladen von Dateien</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Erste Schritte mit dem Azure-Dateidienst in Java</a></td> 
</tr> 
<tr> 
<td>Dateieigenschaften/-metadaten/-metriken</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Erste Schritte mit dem Azure-Dateidienst in Java</a></td> 
</tr> 
<tr> 
<td>Dateidiensteigenschaften</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Erste Schritte mit dem Azure-Dateidienst in Java</a></td> 
</tr> 
<tr> 
<td>Auflisten von Verzeichnissen und Dateien</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Erste Schritte mit dem Azure-Dateidienst in Java</a></td> 
</tr>
<tr> 
<td>Auflisten von Freigaben</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileBasics.java">Erste Schritte mit dem Azure-Dateidienst in Java</a></td> 
</tr>
<tr> 
<td>Freigabeeigenschaften/-metadaten/-statistiken</td> 
<td><a href="https://github.com/Azure-Samples/storage-file-java-getting-started/blob/master/src/FileAdvanced.java">Erste Schritte mit dem Azure-Dateidienst in Java</a></td> 
</tr>
<tr> 
<td rowspan="8"><b>Warteschlange</b></td>
<td>Hinzufügen von Nachrichten</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/queue/gettingstarted/QueueBasics.java">Storage Java Client Library Samples</a> (Speicherclientbibliothek für Java – Beispiele)</td> 
</tr> 
<tr> 
<td>Clientseitige Verschlüsselung</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/encryption/queue/gettingstarted/QueueGettingStarted.java">Storage Java Client Library Samples</a> (Speicherclientbibliothek für Java – Beispiele)</td> 
</tr> 
<tr> 
<td>Erstellen von Warteschlangen</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Erste Schritte mit dem Azure-Warteschlangendienst in Java</a></td> 
</tr> 
<tr> 
<td>Löschen von Nachrichten/Warteschlangen</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Erste Schritte mit dem Azure-Warteschlangendienst in Java</a></td> 
</tr> 
<tr> 
<td>Nachricht einsehen</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Erste Schritte mit dem Azure-Warteschlangendienst in Java</a></td> 
</tr> 
<tr> 
<td>Warteschlangen-ACL/-metadaten/-statistiken</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Erste Schritte mit dem Azure-Warteschlangendienst in Java</a></td> 
</tr> 
<tr> 
<td>Warteschlangendiensteigenschaften</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueAdvanced.java">Erste Schritte mit dem Azure-Warteschlangendienst in Java</a></td> 
</tr> 
<tr> 
<td>Aktualisieren von Nachrichten</td> 
<td><a href="https://github.com/Azure-Samples/storage-queue-java-getting-started/blob/master/src/QueueBasics.java">Erste Schritte mit dem Azure-Warteschlangendienst in Java</a></td> 
</tr> 
<tr> 
<td rowspan="7"><b>Tabelle</b></td>
<td>Erstellen von Tabellen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Erste Schritte mit dem Azure-Tabellenspeicherdienst in Java</a></td> 
</tr> 
<tr> 
<td>Löschen von Entitäten/Tabellen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Erste Schritte mit dem Azure-Tabellenspeicherdienst in Java</a></td> 
</tr> 
<tr> 
<td>Einfügen/Zusammenführen/Ersetzen von Entitäten</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a> (Speicherclientbibliothek für Java – Beispiele)</td> 
</tr> 
<tr> 
<td>Entitäten abfragen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Erste Schritte mit dem Azure-Tabellenspeicherdienst in Java</a></td> 
</tr> 
<tr> 
<td>Abfragen von Tabellen</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableBasics.java">Erste Schritte mit dem Azure-Tabellenspeicherdienst in Java</a></td> 
</tr> 
<tr> 
<td>Tabellen-ACL/-eigenschaften</td> 
<td><a href="https://github.com/Azure-Samples/storage-table-java-getting-started/blob/master/src/TableAdvanced.java">Erste Schritte mit dem Azure-Tabellenspeicherdienst in Java</a></td> 
</tr> 
<tr> 
<td>Entität aktualisieren</td> 
<td><a href="https://github.com/Azure/azure-storage-java/blob/master/microsoft-azure-storage-samples/src/com/microsoft/azure/storage/table/gettingtstarted/TableBasics.java">Storage Java Client Library Samples</a> (Speicherclientbibliothek für Java – Beispiele)</td> 
</tr> 
</tbody> 
</table>
<br/>

## <a name="azure-code-samples-library"></a>Bibliothek mit Azure-Codebeispielen

Wechseln Sie zum Anzeigen der vollständigen Beispielbibliothek zur [Bibliothek mit Azure-Codebeispielen](https://azure.microsoft.com/resources/samples/?service=storage), die Beispiele für Azure Storage enthält, die Sie herunterladen und lokal ausführen können. Die Bibliothek mit Codebeispielen enthält Beispielcode im ZIP-Format. Alternativ können Sie die einzelnen Beispiele im GitHub-Repository suchen und klonen.

[!INCLUDE [storage-java-samples-include](../../../includes/storage-java-samples-include.md)]

## <a name="getting-started-guides"></a>Leitfäden mit ersten Schritten

Lesen Sie den folgenden Leitfäden, um Anleitungen zum Installieren und zu den ersten Schritten mit den Azure Storage-Clientbibliotheken zu erhalten.

* [Erste Schritte mit dem Azure Blob-Dienst in Java](../blobs/storage-java-how-to-use-blob-storage.md)
* [Erste Schritte mit dem Azure-Warteschlangendienst in Java](../storage-java-how-to-use-queue-storage.md)
* [Erste Schritte mit dem Azure-Tabellenspeicherdienst in Java](../../cosmos-db/table-storage-how-to-use-java.md)
* [Erste Schritte mit dem Azure-Dateidienst in Java](../storage-java-how-to-use-file-storage.md)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Beispielen für andere Sprachen:

* .NET: [Azure Storage-Beispiele mit .NET](../storage-samples-dotnet.md)
* Alle anderen Sprachen: [Azure Storage-Beispiele](../storage-samples.md)

