---
title: Festlegen von Eigenschaften und Metadaten im Rahmen des Importprozesses | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie beim Ausführen des Import/Export-Tools zur Vorbereitung der Laufwerke Eigenschaften und Metadaten angeben, die für die Zielblobs festgelegt werden sollen."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: e8541695-bcfb-4bf0-84d9-72c9de32a0a8
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 5d75bdb5ff39934e02d9af721b51e0aca24026bb
ms.openlocfilehash: f49604cf6738da3576ed2a65ca96d083d5e8219b


---

# <a name="setting-properties-and-metadata-during-the-import-process"></a>Festlegen von Eigenschaften und Metadaten im Rahmen des Importprozesses
Wenn Sie das Microsoft Azure Import/Export-Tool zum Vorbereiten der Laufwerke ausführen, können Sie Eigenschaften und Metadaten angeben, die für die Zielblobs festgelegt werden sollen. Folgen Sie diesen Schritten:  
  
1.  Erstellen Sie zum Festlegen von Blobeigenschaften auf dem lokalen Computer eine Textdatei mit Eigenschaftsnamen und -werten.  
  
2.  Erstellen Sie zum Festlegen von Blobmetadaten auf dem lokalen Computer eine Textdatei mit Metadatennamen und -werten.  
  
3.  Übergeben Sie im Rahmen des Vorgangs `PrepImport` den vollständigen Pfad zu einer oder beiden Dateien an das Azure Import/Export-Tool.  
  
> [!NOTE]
>  Wenn Sie bei einer Kopiersitzung eine Eigenschaften- oder Metadatendatei angeben, werden diese Eigenschaften bzw. Metadaten für jedes Blob festgelegt, das im Rahmen dieser Kopiersitzung importiert wird. Wenn Sie für einige der importierten Blobs andere Eigenschaften oder Metadaten festlegen möchten, müssen Sie eine separate Kopiersitzung mit anderen Eigenschaften- oder Metadatendateien erstellen.  
  
## <a name="specify-blob-properties-in-a-text-file"></a>Festlegen von Blobeigenschaften in einer Textdatei  
Um Blobeigenschaften festzulegen, erstellen Sie eine lokale Textdatei, und fügen Sie XML hinzu, das die Eigenschaftsnamen als Elemente und die Eigenschaftswerte als Werte angibt. Hier sehen Sie ein Beispiel, in dem einige Eigenschaftswerte angegeben werden:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Properties>  
    <Content-Type>application/octet-stream</Content-Type>  
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>  
    <Cache-Control>no-cache</Cache-Control>  
</Properties>  
```
  
Speichern Sie die Datei an einem lokalen Speicherort wie `C:\WAImportExport\ImportProperties.txt`.  
  
## <a name="specify-blob-metadata-in-a-text-file"></a>Festlegen von Blobmetadaten in einer Textdatei  
Um Blobmetadaten festzulegen, erstellen Sie in ähnlicher Weise eine lokale Textdatei, in der die Metadatennamen als Elemente und Metadatenwerte als Werte angegeben werden. Hier sehen Sie ein Beispiel, in dem einige Metadatenwerte angegeben werden:  
  
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<Metadata>  
    <UploadMethod>Windows Azure Import/Export Service</UploadMethod>  
    <DataSetName>SampleData</DataSetName>  
    <CreationDate>10/1/2013</CreationDate>  
</Metadata>  
```
  
Speichern Sie die Datei an einem lokalen Speicherort wie `C:\WAImportExport\ImportMetadata.txt`.  
  
## <a name="create-a-copy-session-including-the-properties-or-metadata-files"></a>Erstellen einer Kopiersitzung mit Eigenschaften- oder Metadatendateien  
Wenn Sie das Azure Import/Export-Tool zum Vorbereiten des Importauftrags ausführen, geben Sie die Eigenschaftendatei in der Befehlszeile mit dem Parameter `PropertyFile` an. Geben Sie die Metadatendatei in der Befehlszeile mit dem Parameter `/MetadataFile` an. Im folgenden Beispiel werden beide Dateien angegeben:  
  
```
WAImportExport.exe PrepImport /j:SecondDrive.jrn /id:BlueRayIso /srcfile:K:\Temp\BlueRay.ISO /dstblob:favorite/BlueRay.ISO /MetadataFile:c:\WAImportExport\SampleMetadata.txt /PropertyFile:c:\WAImportExport\SampleProperties.txt  
```
  
## <a name="see-also"></a>Weitere Informationen  
[Format der Metadaten- und Eigenschaftendatei des Import/Export-Diensts](storage-import-export-file-format-metadata-and-properties.md)



<!--HONumber=Dec16_HO2-->


