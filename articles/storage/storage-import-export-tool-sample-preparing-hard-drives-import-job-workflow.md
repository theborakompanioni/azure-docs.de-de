---
title: "Beispielworkflow für das Vorbereiten von Festplatten für einen Azure-Import/Export-Auftrag | Microsoft-Dokumentation"
description: "Lernen Sie eine exemplarische Vorgehensweise für den vollständigen Prozess der Laufwerkvorbereitung für einen Importauftrag im Azure-Import/Export-Dienst kennen."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 41bc5585f3d9dea2a08dc8a6bc1e4fdf9f0c8fc4
ms.openlocfilehash: 763ad16242da2d895686934422e345555d5b0e05


---

# <a name="sample-workflow-to-prepare-hard-drives-for-an-import-job"></a>Beispielworkflow für die Vorbereitung von Festplatten für einen Importauftrag

Dieser Artikel führt Sie durch den vollständigen Prozess zur Vorbereitung von Laufwerken für einen Importauftrag.

## <a name="sample-data"></a>Beispieldaten

In diesem Beispiel werden die folgenden Daten in ein Azure-Speicherkonto namens `mystorageaccount` importiert:

|Location|Beschreibung|Datengröße|
|--------------|-----------------|-----|
|H:\Video|Eine Sammlung von Videos|12TB|
|H:\Photo|Eine Sammlung von Fotos|30 GB|
|K:\Temp\FavoriteMovie.ISO|Ein Blu-ray™-Datenträgerimage|25 GB|
|\\\bigshare\john\music|Eine Sammlung von Musikdateien auf einer Netzwerkfreigabe|10 GB|

## <a name="storage-account-destinations"></a>Speicherkontoziele

Der Importauftrag importiert diese Daten in die folgenden Ziele im Speicherkonto:

|Quelle|Virtuelles Zielverzeichnis oder Blob|
|------------|-------------------------------------------|
|H:\Video|https://mystorageaccount.blob.core.windows.net/video|
|H:\Photo|https://mystorageaccount.blob.core.windows.net/photo|
|K:\Temp\FavoriteMovie.ISO|https://mystorageaccount.blob.core.windows.net/favorite/FavoriteMovies.ISO|
|\\\bigshare\john\music|https://mystorageaccount.blob.core.windows.net/music|

Mit dieser Zuordnung wird die Datei `H:\Video\Drama\GreatMovie.mov` in das Blob `https://mystorageaccount.blob.core.windows.net/video/Drama/GreatMovie.mov` importiert.

## <a name="determine-hard-drive-requirements"></a>Ermitteln der Festplattenanforderungen

Berechnen Sie nun die Größe der Daten, um zu bestimmen, wie viele Festplatten erforderlich sind:

`12TB + 30GB + 25GB + 10GB = 12TB + 65GB`

In diesem Beispiel sollten zwei 8-TB-Festplatten ausreichen. Da das Quellverzeichnis `H:\Video` jedoch 12TB Daten aufweist, und die Kapazität der einzelnen Festplatte nur 8TB beträgt, können Sie dies folgendermaßen in der Datei **dataset.csv** angeben:

```
BasePath,DstBlobPathOrPrefix,BlobType,Disposition,MetadataFile,PropertiesFile
H:\Video\,https://mystorageaccount.blob.core.windows.net/video/,BlockBlob,rename,None,H:\mydirectory\properties.xml
H:\Photo\,https://mystorageaccount.blob.core.windows.net/photo/,BlockBlob,rename,None,H:\mydirectory\properties.xml
K:\Temp\FavoriteVideo.ISO,https://mystorageaccount.blob.core.windows.net/favorite/FavoriteVideo.ISO,BlockBlob,rename,None,H:\mydirectory\properties.xml
\\myshare\john\music\,https://mystorageaccount.blob.core.windows.net/music/,BlockBlob,rename,None,H:\mydirectory\properties.xml
```

## <a name="attach-drives-and-configure-the-job"></a>Anfügen von Laufwerken und Konfigurieren des Auftrags

Sie fügen beide Festplatten dem Computer an und erstellen Volumes. Dann erstellen Sie die Datei **driveset.csv**:

```
DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
X,Format,SilentMode,Encrypt,
Y,Format,SilentMode,Encrypt,
```

Das Tool verteilt die Daten optimiert auf zwei Festplatten.

Darüber hinaus können Sie die folgenden Metadaten für alle Dateien festlegen:

* **UploadMethod:** Microsoft Azure-Import/Export-Dienst
* **DataSetName:** SampleData
* **CreationDate:** 1.10.2013

Um Metadaten für die importierten Dateien festzulegen, erstellen Sie die Textdatei `c:\WAImportExport\SampleMetadata.txt` mit folgendem Inhalt:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Metadata>
    <UploadMethod>Windows Azure Import/Export Service</UploadMethod>
    <DataSetName>SampleData</DataSetName>
    <CreationDate>10/1/2013</CreationDate>
</Metadata>
```

Sie können auch bestimmte Eigenschaften für das Blob `FavoriteMovie.ISO` festlegen:

* **Content-Type:** application/octet-stream
* **Content-MD5:** Q2hlY2sgSW50ZWdyaXR5IQ ==
* **Cache-Control:** no-cache

Um diese Eigenschaften festzulegen, erstellen Sie die Textdatei `c:\WAImportExport\SampleProperties.txt`:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Properties>
    <Content-Type>application/octet-stream</Content-Type>
    <Content-MD5>Q2hlY2sgSW50ZWdyaXR5IQ==</Content-MD5>
    <Cache-Control>no-cache</Cache-Control>
</Properties>
```

## <a name="run-the-azure-importexport-tool-waimportexportexe"></a>Ausführen des Azure-Import/Export-Tools (WAImportExport.exe)

Jetzt können Sie das Azure-Import/Export-Tool ausführen, um die beiden Festplatten vorzubereiten.

**Für die erste Sitzung:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

Wenn weiteren Daten hinzugefügt werden müssen, erstellen Sie eine andere Datasetdatei (gleiches Format wie das anfängliche Dataset).

**Für die zweite Sitzung:**

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

Nach Abschluss der Kopiersitzungen können Sie die beiden Laufwerke vom Kopiercomputer trennen und sie an das entsprechende Azure-Rechenzentrum senden. Bei der Erstellung des Importauftrags im Azure-Portal laden Sie die beiden Journaldateien `<FirstDriveSerialNumber>.xml` und `<SecondDriveSerialNumber>.xml` hoch.

## <a name="next-steps"></a>Nächste Schritte

* [Vorbereiten von Festplatten für einen Importauftrag](storage-import-export-tool-preparing-hard-drives-import.md)
* [Kurzübersicht über häufig verwendete Befehle](storage-import-export-tool-quick-reference.md)



<!--HONumber=Dec16_HO3-->


