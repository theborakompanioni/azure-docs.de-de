---
title: Einrichten des Azure Import/Export-Tools | Microsoft-Dokumentation
description: "Es wird beschrieben, wie Sie das Tool für die Laufwerkvorbereitung und Reparatur für den Import/Export-Dienst von Azure einrichten."
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
ms.openlocfilehash: cc7107ce89c063ae69184039893d9c28c912db08


---

# <a name="setting-up-the-azure-importexport-tool"></a>Einrichten des Azure Import/Export-Tools

Das Import/Export-Tool von Microsoft Azure ist das Tool für die Laufwerkvorbereitung und Reparatur, das Sie für den Microsoft Azure Import/Export-Dienst verwenden können. Sie können das Tool für die folgenden Funktionen verwenden:

* Vor dem Erstellen eines Importauftrags können Sie dieses Tool zum Kopieren von Daten auf die Laufwerke verwenden, die Sie an ein Azure-Rechenzentrum senden.
* Nach Abschluss eines Importauftrags können Sie dieses Tool zum Reparieren von Blobs verwenden, die beschädigt sind, fehlen oder in Konflikt mit anderen Blobs stehen.
* Nachdem Sie die Laufwerke für einen abgeschlossenen Exportauftrag erhalten haben, können Sie dieses Tool nutzen, um Dateien zu reparieren, die beschädigt waren oder auf den Laufwerken fehlten.

## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie die **Laufwerkvorbereitung** für einen Importauftrag durchführen, müssen Sie die folgenden Voraussetzungen erfüllen:

* Sie benötigen ein aktives Azure-Abonnement.
* Ihr Abonnement muss ein Speicherkonto mit genügend verfügbarem Speicherplatz zum Speichern der zu importierenden Dateien enthalten.
* Sie benötigen mindestens einen der Kontoschlüssel für das Speicherkonto.
* Sie benötigen einen Computer (der „Kopiercomputer“), auf dem Windows 7, Windows Server 2008 R2 oder ein neueres Windows-Betriebssystem installiert ist.
* Auf dem Kopiercomputer muss .NET Framework 4 installiert sein.
* Auf dem Kopiercomputer muss BitLocker aktiviert sein.
* Mindestens eine leere 3,5-Zoll-SATA-Festplatte muss an den Kopiercomputer angeschlossen sein.
* Die zu importierenden Dateien müssen vom Kopiercomputer aus zugänglich sein. Dabei spielt es keine Rolle, ob sie sich auf einer Netzwerkfreigabe oder einer lokalen Festplatte befinden.

Wenn Sie versuchen, einen **Import zu reparieren**, der teilweise nicht erfolgreich war, benötigen Sie Folgendes:

* Kopierprotokolldateien
* Speicherkontoschlüssel

Wenn Sie versuchen, einen **Export zu reparieren**, der teilweise nicht erfolgreich war, benötigen Sie Folgendes:

* Kopierprotokolldateien
* Manifestdateien (optional)
* Speicherkontoschlüssel

## <a name="installing-the-azure-importexport-tool"></a>Installieren des Azure Import/Export-Tools

[Laden Sie zuerst das Azure Import/Export-Tool herunter](http://download.microsoft.com/download/3/6/B/36BFF22A-91C3-4DFC-8717-7567D37D64C5/WAImportExport.zip), und extrahieren Sie es in ein Verzeichnis auf Ihrem Computer, z.B. `c:\WAImportExport`.

Das Azure Import/Export-Tool besteht aus den folgenden Dateien:

* dataset.csv
* driveset.csv
* hddid.dll
* Microsoft.Data.Services.Client.dll
* Microsoft.WindowsAzure.Storage.dll
* WAImportExport.exe
* WAImportExport.exe.config
* WAImportExportCore.dll
* WAImportExportRepair.dll

Öffnen Sie als Nächstes ein Eingabeaufforderungsfenster im **Administratormodus**, und wechseln Sie zum Verzeichnis mit den extrahierten Dateien.

Führen Sie das Tool ohne Parameter aus, um die Hilfe für den Befehl anzuzeigen:

```
WAImportExport, a client tool for Windows Azure Import/Export Service. Microsoft (c) 2013


Copy directories and/or files with a new copy session:
    WAImportExport.exe PrepImport
        /j:<JournalFile>
        /id:<SessionId> [/logdir:<LogDirectory>]
        [/sk:<StorageAccountKey>]
        [/silentmode]
        [/InitialDriveSet:<driveset.csv>]
        DataSet:<dataset.csv>

Add more drives:
    WAImportExport.exe PrepImport
        /j:<JournalFile>
        /id:<SessionId>
        /AdditionalDriveSet:<driveset.csv>

Abort an interrupted copy session:
    WAImportExport.exe PrepImport
        /j:<JournalFile>
        /id:<SessionId>
        /AbortSession

Resume an interrupted copy session:
    WAImportExport.exe PrepImport
        /j:<JournalFile>
        /id:<SessionId>
        /ResumeSession

List drives:
    WAImportExport.exe PrepImport /j:<JournalFile> /ListDrives

List copy sessions:
    WAImportExport.exe PrepImport /j:<JournalFile> /ListCopySessions

Repair a Drive:
    WAImportExport.exe RepairImport | RepairExport
        /r:<RepairFile> [/logdir:<LogDirectory>]
        [/d:<TargetDirectories>] [/bk:<BitLockerKey>]
        /sn:<StorageAccountName> /sk:<StorageAccountKey>
        [/CopyLogFile:<DriveCopyLogFile>] [/ManifestFile:<DriveManifestFile>]
        [/PathMapFile:<DrivePathMapFile>]

Preview an Export Job:
    WAImportExport.exe PreviewExport
        [/logdir:<LogDirectory>]
        /sn:<StorageAccountName> /sk:<StorageAccountKey>
        /ExportBlobListFile:<ExportBlobListFile> /DriveSize:<DriveSize>

Parameters:

    /j:<JournalFile>
        - Required. Path to the journal file. A journal file tracks a set of drives and
          records the progress in preparing these drives. The journal file must always
          be specified.
    /logdir:<LogDirectory>
        - Optional. The log directory. Verbose log files as well as some temporary
          files will be written to this directory. If not specified, current directory
          will be used as the log directory. The log directory can be specified only
          once for the same journal file.
    /id:<SessionId>
        - Optional. The session Id is used to identify a copy session. It is used to
          ensure accurate recovery of an interrupted copy session.
    /ResumeSession
        - Optional. If the last copy session was terminated abnormally, this parameter
          can be specified to resume the session.
    /AbortSession
        - Optional. If the last copy session was terminated abnormally, this parameter
          can be specified to abort the session.
    /sn:<StorageAccountName>
        - Required. Only applicable for RepairImport and RepairExport. The name of
          the storage account.
    /sk:<StorageAccountKey>
        - Required. The key of the storage account.
    /InitialDriveSet:<driveset.csv>
        - Required. A .csv file that contains a list of drives to prepare.
    /AdditionalDriveSet:<driveset.csv>
        - Required. A .csv file that contains a list of additional drives to be added.
    /r:<RepairFile>
        - Required. Only applicable for RepairImport and RepairExport.
          Path to the file for tracking repair progress. Each drive must have one
          and only one repair file.
    /d:<TargetDirectories>
        - Required. Only applicable for RepairImport and RepairExport.
          For RepairImport, one or more semicolon-separated directories to repair;
          For RepairExport, one directory to repair, e.g. root directory of the drive.
    /CopyLogFile:<DriveCopyLogFile>
        - Required. Only applicable for RepairImport and RepairExport. Path to the
          drive copy log file (verbose or error).
    /ManifestFile:<DriveManifestFile>
        - Required. Only applicable for RepairExport. Path to the drive manifest file.
    /PathMapFile:<DrivePathMapFile>
        - Optional. Only applicable for RepairImport. Path to the file containing
          mappings of file paths relative to the drive root to locations of actual files
          (tab-delimited). When first specified, it will be populated with file paths
          with empty targets, which means either they are not found in TargetDirectories,
          access denied, with invalid name, or they exist in multiple directories. The
          path map file can be manually edited to include the correct target paths and
          specified again for the tool to resolve the file paths correctly.
    /ExportBlobListFile:<ExportBlobListFile>
        - Required. Path to the XML file containing list of blob paths or blob path
          prefixes for the blobs to be exported. The file format is the same as the
          blob list blob format in the Put Job operation of the Import/Export Service
          REST API.
    /DriveSize:<DriveSize>
        - Required. Size of drives to be used for export. For example, 500GB, 1.5TB.
          Note: 1 GB = 1,000,000,000 bytes
                1 TB = 1,000,000,000,000 bytes
    /DataSet:<dataset.csv>
        - Required. A .csv file that contains a list of directories and/or a list files
          to be copied to target drives.

    /silentmode
        - Optional. If not specified, it will remind you the requirement of drives and
          need your confirmation to continue.

Examples:

    Copy a data set to a drive:
    WAImportExport.exe PrepImport
        /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GEL
        xmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /InitialDriveSet:driveset1.csv
        /DataSet:data.csv

    Copy another dataset to the same drive following the above command:
    WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#2 /DataSet:dataset2.csv

    Preview how many 1.5 TB drives are needed for an export job:
    WAImportExport.exe PreviewExport
        /sn:mytestaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7K
        ysbbeKLDksg7VoN1W/a5UuM2zNgQ== /ExportBlobListFile:C:\temp\myexportbloblist.xml
        /DriveSize:1.5TB

    Repair an finished import job:
    WAImportExport.exe RepairImport
        /r:9WM35C2V.rep /d:X:\ /bk:442926-020713-108086-436744-137335-435358-242242-2795
        98 /sn:mytestaccount /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94
        f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /CopyLogFile:C:\temp\9WM35C2V_error.log
```

## <a name="next-steps"></a>Nächste Schritte

* [Vorbereiten von Festplatten für einen Importauftrag](storage-import-export-tool-preparing-hard-drives-import.md)
* [Anzeigen einer Vorschau der Laufwerknutzung für einen Exportauftrag](storage-import-export-tool-previewing-drive-usage-export-v1.md)
* [Überprüfen des Auftragsstatus mit Protokollkopiedateien](storage-import-export-tool-reviewing-job-status-v1.md)
* [Reparieren eines Importauftrags](storage-import-export-tool-repairing-an-import-job-v1.md)
* [Reparieren eines Exportauftrags](storage-import-export-tool-repairing-an-export-job-v1.md)
* [Behandeln von Problemen mit dem Azure Import/Export-Tool](storage-import-export-tool-troubleshooting-v1.md)



<!--HONumber=Dec16_HO3-->


