---
title: Lesen von NSG-Datenflussprotokollen | Microsoft-Dokumentation
description: "In diesem Artikel wird erläutert, wie NSG-Datenflussprotokolle analysiert werden"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 9bb48157b2b8e483e063058f761c3a8f531927f9
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="read-nsg-flow-logs"></a>Lesen von NSG-Datenflussprotokollen

Hier erfahren Sie, wie NSG-Datenflussprotokolle mit PowerShell gelesen werden.

NSG-Datenflussprotokolle werden in einem Speicherkonto in [Blockblobs](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs.md#about-block-blobs) gespeichert. Blockblobs setzen sich aus kleineren Blöcken zusammen. Jedes Protokoll stellt einen separaten Blockblob dar, der einmal pro Stunde generiert wird. Neue Protokolle werden stündlich generiert. Die Protokolle werden anhand der neuesten Daten alle paar Minuten mit neuen Einträgen aktualisiert. In diesem Artikel erfahren Sie, wie Sie Teile der Datenflussprotokolle lesen können.

## <a name="scenario"></a>Szenario

Im folgenden Beispiel liegt ein Beispiel-Datenflussprotokoll vor, das in einem Speicherkonto gespeichert ist. Es wird schrittweise erläutert, wie Sie selektiv die aktuellen Ereignisse in NSG-Datenflussprotokollen lesen. In diesem Artikel wird PowerShell verwendet. Die erörterten Konzepte sind jedoch nicht auf diese Programmiersprache beschränkt und gelten für alle Sprachen, die von den Azure Storage-APIs unterstützt werden.

## <a name="setup"></a>Einrichtung

Bevor Sie beginnen, müssen Sie die NSG-Datenflussprotokollierung für mindestens eine Netzwerksicherheitsgruppe Ihres Kontos aktiviert haben. Eine Anleitung zum Aktivieren von NSG-Datenflussprotokollen finden Sie im Artikel [Einführung in die Datenflussprotokollierung für Netzwerksicherheitsgruppen](network-watcher-nsg-flow-logging-overview.md).

## <a name="retrieve-the-block-list"></a>Abrufen der Liste der Blöcke

Mit dem folgenden PowerShell-Skript werden die erforderlichen Variablen zum Abfragen des Blobs mit dem NSG-Datenflussprotokoll eingerichtet, und die Blöcke im Blockblob [CloudBlockBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azurestorage-8.1.3) werden aufgelistet. Aktualisieren Sie das Skript so, dass es gültige Werte für Ihre Umgebung enthält.

```powershell
# The SubscriptionID to use
$subscriptionId = "00000000-0000-0000-0000-000000000000"

# Resource group that contains the Network Security Group
$resourceGroupName = "<resourceGroupName>"

# The name of the Network Security Group
$nsgName = "NSGName"

# The storage account name that contains the NSG logs
$storageAccountName = "<storageAccountName>" 

# The date and time for the log to be queried, logs are stored in hour intervals.
[datetime]$logtime = "06/16/2017 20:00"

# Retrieve the primary storage account key to access the NSG logs
$StorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName).Value[0]

# Setup a new storage context to be used to query the logs
$ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

# Container name used by NSG flow logs
$ContainerName = "insights-logs-networksecuritygroupflowevent"

# Name of the blob that contains the NSG flow log
$BlobName = "resourceId=/SUBSCRIPTIONS/${subscriptionId}/RESOURCEGROUPS/${resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/${nsgName}/y=$($logtime.Year)/m=$(($logtime).ToString("MM"))/d=$(($logtime).ToString("dd"))/h=$(($logtime).ToString("HH"))/m=00/PT1H.json"

# Gets the storage blog
$Blob = Get-AzureStorageBlob -Context $ctx -Container $ContainerName -Blob $BlobName

# Gets the block blog of type 'Microsoft.WindowsAzure.Storage.Blob.CloudBlob' from the storage blob
$CloudBlockBlob = [Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob] $Blob.ICloudBlob

# Stores the block list in a variable from the block blob.
$blockList = $CloudBlockBlob.DownloadBlockList()
```

Die `$blockList`-Variable gibt eine Liste der Blöcke im Blob zurück. Jeder Blockblob enthält mindestens zwei Blöcke.  Der erste Block weist eine Länge von `21` Byte auf; dieser Block enthält die öffnenden Klammern des json-Protokolls. Der andere Block enthält die schließenden Klammern und hat eine Länge von `9` Byte.  Wie Sie sehen, enthält das folgende Beispielprotokoll sieben Einträge, von denen jeder einen Einzeleintrag darstellt. Alle neuen Einträge im Protokoll werden am Ende unmittelbar vor dem letzten Block hinzugefügt.

```
Name                                         Length Committed
----                                         ------ ---------
ZDk5MTk5N2FkNGE0MmY5MTk5ZWViYjA0YmZhODRhYzY=     21      True
NzQxNDA5MTRhNDUzMGI2M2Y1MDMyOWZlN2QwNDZiYzQ=   2685      True
ODdjM2UyMWY3NzFhZTU3MmVlMmU5MDNlOWEwNWE3YWY=   2586      True
ZDU2MjA3OGQ2ZDU3MjczMWQ4MTRmYWNhYjAzOGJkMTg=   2688      True
ZmM3ZWJjMGQ0ZDA1ODJlOWMyODhlOWE3MDI1MGJhMTc=   2775      True
ZGVkYTc4MzQzNjEyMzlmZWE5MmRiNjc1OWE5OTc0OTQ=   2676      True
ZmY2MjUzYTIwYWIyOGU1OTA2ZDY1OWYzNmY2NmU4ZTY=   2777      True
Mzk1YzQwM2U0ZWY1ZDRhOWFlMTNhYjQ3OGVhYmUzNjk=   2675      True
ZjAyZTliYWE3OTI1YWZmYjFmMWI0MjJhNzMxZTI4MDM=      9      True
```

## <a name="read-the-block-blob"></a>Lesen des Blockblobs

Nun muss die `$blocklist`-Variable gelesen werden, um die Daten abzurufen. In diesem Beispiel wird die Liste der Blöcke durchlaufen, die Bytes werden aus den einzelnen Blöcken gelesen und in einem Array gespeichert. Die Daten werden mit der [DownloadRangeToByteArray](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob.downloadrangetobytearray?view=azurestorage-8.1.3#Microsoft_WindowsAzure_Storage_Blob_CloudBlob_DownloadRangeToByteArray_System_Byte___System_Int32_System_Nullable_System_Int64__System_Nullable_System_Int64__Microsoft_WindowsAzure_Storage_AccessCondition_Microsoft_WindowsAzure_Storage_Blob_BlobRequestOptions_Microsoft_WindowsAzure_Storage_OperationContext_)-Methode abgerufen.

```powershell
# Set the size of the byte array to the largest block
$maxvalue = ($blocklist | measure Length -Maximum).Maximum

# Create an array to store values in
$valuearray = @()

# Define the starting index to track the current block being read
$index = 0

# Loop through each block in the block list
for($i=0; $i -lt $blocklist.count; $i++)
{

# Create a byte array object to story the bytes from the block
$downloadArray = New-Object -TypeName byte[] -ArgumentList $maxvalue

# Download the data into the ByteArray, starting with the current index, for the number of bytes in the current block. Index is increased by 3 when reading to remove preceding comma.
$CloudBlockBlob.DownloadRangeToByteArray($downloadArray,0,$index+3,$($blockList[$i].Length-1)) | Out-Null

# Increment the index by adding the current block length to the previous index
$index = $index + $blockList[$i].Length

# Retrieve the string from the byte array

$value = [System.Text.Encoding]::ASCII.GetString($downloadArray)

# Add the log entry to the value array
$valuearray += $value
}
```

Nun enthält das `$valuearray`-Array den Zeichenfolgenwert jedes Blocks. Um den Eintrag zu überprüfen, rufen Sie den zweitletzten Wert aus dem Array ab. Führen Sie dazu `$valuearray[$valuearray.Length-2]` aus. Der letzte Wert ist lediglich die schließende Klammer und wird daher nicht gewünscht.

Die Ergebnisse dieses Werts werden im folgenden Beispiel gezeigt:

```json
        {
             "time": "2017-06-16T20:59:43.7340000Z",
             "systemId": "5f4d02d3-a7d0-4ed4-9ce8-c0ae9377951c",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/CONTOSORG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/CONTOSONSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_AllowInternetOutBound","flows":[{"mac":"000D3A18077E","flowTuples":["1497646722,10.0.0.4,168.62.32.14,44904,443,T,O,A","1497646722,10.0.0.4,52.240.48.24,45218,443,T,O,A","1497646725,10.
0.0.4,168.62.32.14,44910,443,T,O,A","1497646725,10.0.0.4,52.240.48.24,45224,443,T,O,A","1497646728,10.0.0.4,168.62.32.14,44916,443,T,O,A","1497646728,10.0.0.4,52.240.48.24,45230,443,T,O,A","1497646732,10.0.0.4,168.62.32.14,44922,443,T,O,A","14976
46732,10.0.0.4,52.240.48.24,45236,443,T,O,A","1497646735,10.0.0.4,168.62.32.14,44928,443,T,O,A","1497646735,10.0.0.4,52.240.48.24,45242,443,T,O,A","1497646738,10.0.0.4,168.62.32.14,44934,443,T,O,A","1497646738,10.0.0.4,52.240.48.24,45248,443,T,O,
A","1497646742,10.0.0.4,168.62.32.14,44942,443,T,O,A","1497646742,10.0.0.4,52.240.48.24,45256,443,T,O,A","1497646745,10.0.0.4,168.62.32.14,44948,443,T,O,A","1497646745,10.0.0.4,52.240.48.24,45262,443,T,O,A","1497646749,10.0.0.4,168.62.32.14,44954
,443,T,O,A","1497646749,10.0.0.4,52.240.48.24,45268,443,T,O,A","1497646753,10.0.0.4,168.62.32.14,44960,443,T,O,A","1497646753,10.0.0.4,52.240.48.24,45274,443,T,O,A","1497646756,10.0.0.4,168.62.32.14,44966,443,T,O,A","1497646756,10.0.0.4,52.240.48
.24,45280,443,T,O,A","1497646759,10.0.0.4,168.62.32.14,44972,443,T,O,A","1497646759,10.0.0.4,52.240.48.24,45286,443,T,O,A","1497646763,10.0.0.4,168.62.32.14,44978,443,T,O,A","1497646763,10.0.0.4,52.240.48.24,45292,443,T,O,A","1497646766,10.0.0.4,
168.62.32.14,44984,443,T,O,A","1497646766,10.0.0.4,52.240.48.24,45298,443,T,O,A","1497646769,10.0.0.4,168.62.32.14,44990,443,T,O,A","1497646769,10.0.0.4,52.240.48.24,45304,443,T,O,A","1497646773,10.0.0.4,168.62.32.14,44996,443,T,O,A","1497646773,
10.0.0.4,52.240.48.24,45310,443,T,O,A","1497646776,10.0.0.4,168.62.32.14,45002,443,T,O,A","1497646776,10.0.0.4,52.240.48.24,45316,443,T,O,A","1497646779,10.0.0.4,168.62.32.14,45008,443,T,O,A","1497646779,10.0.0.4,52.240.48.24,45322,443,T,O,A"]}]}
,{"rule":"DefaultRule_DenyAllInBound","flows":[]},{"rule":"UserRule_ssh-rule","flows":[]},{"rule":"UserRule_web-rule","flows":[{"mac":"000D3A18077E","flowTuples":["1497646738,13.82.225.93,10.0.0.4,1180,80,T,I,A","1497646750,13.82.225.93,10.0.0.4,
1184,80,T,I,A","1497646768,13.82.225.93,10.0.0.4,1181,80,T,I,A","1497646780,13.82.225.93,10.0.0.4,1336,80,T,I,A"]}]}]}
        }
```

Anhand dieses Szenarios wird beispielhaft veranschaulicht, wie Einträge in NSG-Datenflussprotokollen gelesen werden, ohne dass das gesamte Protokoll analysiert werden muss. Sie können neue Einträge im Protokoll lesen, da diese unter Verwendung der Block-ID geschrieben wurden. Sie können dazu auch die Länge der im Blockblob gespeicherten Blöcke verfolgen. Dadurch können Sie ausschließlich die neuen Einträge lesen.


## <a name="next-steps"></a>Nächste Schritte

Lesen Sie [Visualisieren von Azure Network Watcher-NSG-Datenflussprotokollen mit Open-Source-Tools](network-watcher-visualize-nsg-flow-logs-open-source-tools.md) durch, um mehr über andere Möglichkeiten zum Einsehen von NSG-Datenflussprotokollen zu erfahren.

Weitere Informationen zu Speicherblobs erhalten Sie unter [Azure Functions – Blob Storage-Bindungen](../azure-functions/functions-bindings-storage-blob.md)
