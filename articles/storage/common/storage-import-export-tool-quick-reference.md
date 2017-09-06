---
title: "Kurzübersicht über Befehle des Azure Import/Export-Tools für Importaufträge | Microsoft-Dokumentation"
description: "Befehlsreferenz für Befehle des Azure Import/Export-Tools, die häufig für Importaufträge verwendet werden."
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
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: d51ae35ead0e7d8289de663e5b7b48d28271e810
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Kurzübersicht über häufig verwendete Befehle für Importaufträge

Dieser Artikel enthält eine Kurzübersicht über einige häufig verwendete Befehle. Ausführliche Informationen zur Verwendung finden Sie unter [Vorbereiten von Festplatten für einen Importauftrag](../storage-import-export-tool-preparing-hard-drives-import.md).

## <a name="first-session"></a>Erste Sitzung

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a>Zweite Sitzung

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a>Abbruch der aktuellen Sitzung

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a>Fortsetzen der aktuellen unterbrochenen Sitzung

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-to-latest-session"></a>Hinzufügen von Laufwerken zur aktuellen Sitzung

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a>Nächste Schritte

* [Beispielworkflow für die Vorbereitung von Festplatten für einen Importauftrag](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)

