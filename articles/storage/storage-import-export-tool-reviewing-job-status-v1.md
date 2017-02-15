---
title: "Überprüfen des Auftragsstatus mit Kopierprotokolldateien | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe der Protokolldateien, die beim Ausführen des Import- oder Exportauftrags erstellt werden, den Status des Import/Export-Auftrags ermitteln."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: c69d1d69-6403-4eee-9949-0185faeecfa1
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 25cd0308115994463bd50562594d4e2bb88e8286
ms.openlocfilehash: 66d7690249cb653dfb8d2e591d1ce66162d98a7e


---

# <a name="reviewing-job-status-with-copy-log-files"></a>Überprüfen des Auftragsstatus mit Kopierprotokolldateien
Wenn der Microsoft Azure Import/Export-Dienst Laufwerke im Rahmen eines Import- oder Exportauftrags verarbeitet, schreibt er Kopierprotokolldateien in das Speicherkonto, in das bzw. aus dem Sie Blobs importieren oder exportieren. Die Protokolldatei enthält ausführliche Statusinformationen zu den einzelnen importierten oder exportierten Dateien. Die URL zu den einzelnen Kopierprotokolldateien wird zurückgegeben, wenn Sie den Status eines abgeschlossenen Auftrags abfragen. (Weitere Informationen finden Sie unter [Get Job](/rest/api/storageservices`Get-Job3).)  
  
 Im Anschluss finden Sie Beispiel-URLs für Kopierprotokolldateien für einen Importauftrag mit zwei Laufwerken:  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath/waies/myjob_9WM35C2V_20130921-034307-902_error.xml`  
  
 `http://myaccount.blob.core.windows.net/ImportExportStatesPath /waies/myjob_9WM45A6Q_20130921-042122-021_error.xml`  
  
 Informationen zum Format von Kopierprotokollen sowie eine vollständige Liste mit Statuscodes finden Sie unter [Format der Protokolldateien des Import/Export-Diensts](storage-import-export-file-format-log.md).  
  
## <a name="see-also"></a>Weitere Informationen  
 [Einrichten des Azure Import/Export-Tools](storage-import-export-tool-setup-v1.md)   
 [Vorbereiten von Festplatten für einen Importauftrag](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 [Reparieren eines Importauftrags](storage-import-export-tool-repairing-an-import-job-v1.md)   
 [Reparieren eines Exportauftrags](storage-import-export-tool-repairing-an-export-job-v1.md)   
 [Behandeln von Problemen mit dem Azure Import/Export-Tool](storage-import-export-tool-troubleshooting-v1.md)



<!--HONumber=Dec16_HO2-->


