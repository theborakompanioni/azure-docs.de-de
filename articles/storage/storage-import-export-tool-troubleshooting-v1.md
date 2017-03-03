---
title: Behandeln von Problemen mit dem Azure Import/Export-Tool | Microsoft-Dokumentation
description: "Dieser Artikel enthält Informationen zu allgemeinen Problemen von Benutzern bei der Verwendung des Import/Export-Tools und deren Behebung."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: b91ca5eb-c557-460a-9afc-0590b38471f9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 9aca8aad3f268bf21f3bad9fa22821f5d825f99d
ms.openlocfilehash: 53755e0048cfd80d9f930419a105893e5b34dc41
ms.lasthandoff: 02/16/2017


---

# <a name="troubleshooting-the-azure-import-export-tool"></a>Behandeln von Problemen mit dem Azure Import/Export-Tool
Das Microsoft Azure Import/Export-Tool zeigt Fehlermeldungen an, wenn Probleme auftreten. Dieses Thema enthält einige häufig auftretende Probleme, auf die Benutzer stoßen können.  
  
## <a name="a-copy-session-fails-what-i-should-do"></a>Wie gehe ich vor, wenn bei einer Kopiersitzung ein Fehler auftritt?  
 Wenn ein Fehler in einer Kopiersitzung auftritt, gibt es zwei Optionen:  
  
 Wenn der Fehler durch Wiederholen behoben werden kann, beispielsweise wenn die Netzwerkfreigabe kurze Zeit offline war und nun wieder online ist, können Sie die Kopiersitzung fortsetzen. Wenn der Fehler nicht durch erneutes Versuchen behoben werden kann, etwa wenn Sie das falsche Quellverzeichnis der Dateien in den Befehlszeilenparametern angegeben haben, müssen Sie die Kopiersitzung abbrechen. Weitere Informationen zum Fortsetzen und Abbrechen von Kopiersitzungen finden Sie unter [Preparing Hard Drives for an Import Job](storage-import-export-tool-preparing-hard-drives-import-v1.md) (Vorbereiten von Festplatten für Importaufträge).  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a>Ich kann eine Kopiersitzung nicht fortsetzen oder abbrechen.  
 Falls die Kopiersitzung die erste Kopiersitzung für ein Laufwerk ist, muss die Fehlermeldung wie folgt lauten: „The first copy session cannot be resumed or aborted.“ (Die erste Kopiersitzung kann nicht fortgesetzt oder abgebrochen werden.). In diesem Fall können Sie die alte Journaldatei löschen und den Befehl erneut ausführen.  
  
 Wenn eine Kopiersitzung nicht die erste Kopiersitzung für ein Laufwerk ist, kann sie immer fortgesetzt oder abgebrochen werden.  
  
## <a name="i-lost-the-journal-file-can-i-still-create-the-job"></a>Ich habe die Journaldatei verloren, kann ich den Auftrag trotzdem erstellen?  
 Die Journaldatei für ein Laufwerk enthält die vollständigen Informationen zum Kopieren von Daten auf dieses Laufwerk, ist erforderlich, um dem Laufwerk weitere Dateien hinzufügen, und wird verwendet, um einen Importauftrag zu erstellen. Wenn die Journaldatei verloren geht, müssen Sie alle Kopiersitzungen für das Laufwerk wiederholen.  
  
## <a name="see-also"></a>Weitere Informationen  
 [Einrichten des Azure Import/Export-Tools](storage-import-export-tool-setup-v1.md)   
 [Vorbereiten von Festplatten für einen Importauftrag](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
 [Überprüfen des Auftragsstatus mit Kopierprotokolldateien](storage-import-export-tool-reviewing-job-status-v1.md)   
 [Reparieren eines Importauftrags](storage-import-export-tool-repairing-an-import-job-v1.md)   
 [Reparieren eines Exportauftrags](storage-import-export-tool-repairing-an-export-job-v1.md)

