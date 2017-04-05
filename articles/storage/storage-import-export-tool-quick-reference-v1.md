---
title: "Kurzübersicht über Befehle des Azure Import/Export-Tools für Importaufträge (V1) | Microsoft-Dokumentation"
description: "Befehlsreferenz für Befehle des Azure Import/Export-Tools, die häufig für Importaufträge verwendet werden. Dies bezieht sich auf V1 des Import/Export-Tools."
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
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: 632100f324c47d69f64cff020aafbaa300ae8c2b
ms.lasthandoff: 03/30/2017


---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Kurzübersicht über häufig verwendete Befehle für Importaufträge
Dieser Abschnitt enthält eine Kurzübersicht über einige häufig verwendete Befehle. Ausführliche Informationen zur Verwendung finden Sie unter [Vorbereiten von Festplatten für einen Importauftrag](storage-import-export-tool-preparing-hard-drives-import-v1.md).  

## <a name="prepare-the-disks-when-data-already-copied-to-the-disks"></a>Vorbereiten der Datenträger, wenn bereits Daten kopiert wurden
 Hier ist ein Beispielbefehl zum Vorbereiten eines Datenträgers angegeben, wenn bereits Daten auf die Festplatte kopiert wurden, die noch nicht mit BitLocker verschlüsselt sind:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a>Kopieren eines einzelnen Verzeichnisses auf eine Festplatte  
 Hier ist ein Beispielbefehl zum Kopieren eines einzelnen Quellverzeichnisses auf eine Festplatte angegeben, die noch nicht mit BitLocker verschlüsselt wurde:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-wwo-directories-to-a-hard-drive"></a>Kopieren von zwei Verzeichnissen auf eine Festplatte  
 Sie benötigen zwei Befehle, um zwei Quellverzeichnisse auf ein Laufwerk zu kopieren.  
  
 Mit dem ersten Befehl werden das Protokollverzeichnis, der Speicherkontoschlüssel, der Buchstabe des Ziellaufwerks und die `format/encrypt`-Anforderungen sowie die allgemeinen Parameter angegeben:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 Mit dem zweiten Befehl werden die Journaldatei, eine neue Sitzungs-ID und die Quell- und Zielspeicherorte angegeben:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a>Kopieren einer großen Datei auf eine Festplatte in einer zweiten Kopiersitzung  
 Hier ist ein Beispielbefehl angegeben, mit dem eine einzelne große Datei auf ein Laufwerk kopiert wird, das in der vorherigen Kopiersitzung vorbereitet wurde:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Nächste Schritte

* [Beispielworkflow für die Vorbereitung von Festplatten für einen Importauftrag](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)

