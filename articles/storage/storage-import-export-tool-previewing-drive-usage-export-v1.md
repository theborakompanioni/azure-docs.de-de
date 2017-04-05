---
title: "Anzeigen einer Vorschau der Laufwerknutzung für einen Exportauftrag in Azure Import/Export (V1) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Vorschau der Liste der Blobs anzeigen, die Sie für einen Exportauftrag im Azure Import/Export-Dienst ausgewählt haben."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 7707d744-7ec7-4de8-ac9b-93a18608dc9a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 7bf74247090f91e17f81a9bc98ebfa78334c8c10
ms.lasthandoff: 03/30/2017


---

# <a name="previewing-drive-usage-for-an-export-job"></a>Anzeigen einer Vorschau der Laufwerknutzung für einen Exportauftrag
Bevor Sie einen Exportauftrag erstellen, müssen Sie eine Gruppe von Blobs auswählen, die Sie exportieren möchten. Der Microsoft Azure Import/Export-Dienst bietet Ihnen die Möglichkeit, die ausgewählten Blobs mithilfe einer Liste von Blobpfaden oder Blobpräfixen darzustellen.  
  
Als Nächstes müssen Sie bestimmen, wie viele Laufwerke Sie senden müssen. Im Import/Export-Tool können Sie mit dem Befehl `PreviewExport` basierend auf der Größe der zu verwendenden Laufwerke eine Vorschau der Laufwerknutzung für die ausgewählten Blobs anzeigen.

## <a name="command-line-parameters"></a>Befehlszeilenparameter

Bei der Verwendung des Befehls `PreviewExport` des Import/Export-Tools können Sie die folgenden Parameter verwenden.

|Befehlszeilenparameter|Beschreibung|  
|--------------------------|-----------------|  
|**/logdir:**<Protokollverzeichnis\>|Optional. Das Protokollverzeichnis. In dieses Verzeichnis werden ausführliche Protokolldateien geschrieben. Wird kein Protokollverzeichnis angegeben, wird das aktuelle Verzeichnis als Protokollverzeichnis verwendet.|  
|**/sn:**<Speicherkontoname\>|Erforderlich. Der Name des Speicherkontos für den Exportauftrag.|  
|**/sk:**<Speicherkontoschlüssel\>|Nur erforderlich, wenn keine Container-SAS (Shared Access Signature) angegeben wurde. Der Kontoschlüssel des Speicherkontos für den Exportauftrag.|  
|**/csas:**<Container-SAS\>|Nur erforderlich, wenn kein Speicherkontoschlüssel angegeben wurde. Die Container-SAS zum Auflisten der Blobs, die im Exportauftrag exportiert werden sollen.|  
|**/ExportBlobListFile:**<Datei mit der Blobliste für den Export\>|Erforderlich. Pfad zur XML-Datei mit der Liste der Blobpfade oder Blobpfadpräfixe für die zu exportierenden Blobs. Das Dateiformat entspricht dem Format, das im Element `BlobListBlobPath` im [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate)-Vorgang der REST-API des Import/Export-Diensts verwendet wird.|  
|**/DriveSize:**<Laufwerkgröße\>|Erforderlich. Die Größe der Laufwerke, die für einen Exportauftrag verwendet werden sollen, *beispielsweise* 500 GB, 1,5 TB.|  

## <a name="command-line-example"></a>Befehlszeilenbeispiel

Das folgende Beispiel veranschaulicht die Verwendung des Befehls `PreviewExport`:  
  
```  
WAImportExport.exe PreviewExport /sn:bobmediaaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\WAImportExport\mybloblist.xml /DriveSize:500GB    
```  
  
Die Datei mit der Blobliste für den Export kann wie hier gezeigt Blobnamen und Blobpräfixe enthalten:  
  
```xml 
<?xml version="1.0" encoding="utf-8"?>  
<BlobList>  
<BlobPath>pictures/animals/koala.jpg</BlobPath>  
<BlobPathPrefix>/vhds/</BlobPathPrefix>  
<BlobPathPrefix>/movies/</BlobPathPrefix>  
</BlobList>  
```

Das Azure Import/Export-Tool listet alle zu exportierenden Blobs auf, berechnet unter Berücksichtigung des notwendigen Mehraufwands, wie diese in Laufwerke der angegebenen Größe gepackt werden, und schätzt dann die erforderliche Anzahl von Laufwerken für die Blobs sowie Informationen zur Laufwerknutzung.  
  
Hier sehen Sie ein Beispiel der Ausgabe ohne Informationsprotokolle:  
  
```  
Number of unique blob paths/prefixes:   3  
Number of duplicate blob paths/prefixes:        0  
Number of nonexistent blob paths/prefixes:      1  
  
Drive size:     500.00 GB  
Number of blobs that can be exported:   6  
Number of blobs that cannot be exported:        2  
Number of drives needed:        3  
        Drive #1:       blobs = 1, occupied space = 454.74 GB  
        Drive #2:       blobs = 3, occupied space = 441.37 GB  
        Drive #3:       blobs = 2, occupied space = 131.28 GB    
```  
  
## <a name="next-steps"></a>Nächste Schritte

* [Azure Import/Export-Tool – Referenz](storage-import-export-tool-how-to-v1.md)

