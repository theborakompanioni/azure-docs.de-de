---
title: Verwenden der REST-API des Azure Import/Export-Diensts | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie die REST-API des Azure Import/Export-Diensts verwenden.
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 233f80e9-2e7f-48e0-9639-5c7785e7d743
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 22e37e26fec913a7638c71b2547c38f5efacd10b
ms.openlocfilehash: 675aef8ec0f9344d2b3073021e83856c2fb34b7c


---
# <a name="using-the-azure-importexport-service-rest-api"></a>Verwenden der REST-API des Azure Import/Export-Diensts

Der Microsoft Azure Import/Export-Dienst macht eine REST-API verfügbar, um die programmgesteuerte Kontrolle von Import-/Exportaufträgen zu ermöglichen. Mithilfe der REST-API können Sie alle Import-/Exportvorgänge durchführen, die mit dem [Azure-Portal](https://portal.azure.com/) möglich sind. Außerdem können Sie mit der REST-API bestimmte präzise Vorgänge durchführen – z.B. Abfragen des prozentualen Fortschritts eines Auftrags – die im klassischen Portal derzeit nicht verfügbar sind.

Eine Übersicht über den Import/Export-Dienst und ein Tutorial, in dem die Verwendung des klassischen Portals zum Erstellen und Verwalten von Import- und Exportaufträgen beschrieben wird, finden Sie unter [Verwenden des Microsoft Azure Import/Export-Diensts zum Übertragen von Daten in den Blobspeicher](storage-import-export-service.md).

## <a name="service-endpoints"></a>Dienstendpunkte

Der Azure Import/Export-Dienst ist ein Ressourcenanbieter für Azure Resource Manager und bietet am folgenden HTTPS-Endpunkt zum Verwalten von Import-/Exportaufträgen einen Satz von REST-APIs:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Versionsverwaltung

Anforderungen an den Import/Export-Dienst müssen den `api-version`-Parameter angeben und dessen Wert auf `2016-11-01` festlegen.

## <a name="in-this-section"></a>Inhalt dieses Abschnitts

[Erstellen eines Importauftrags](storage-import-export-creating-an-import-job.md)

[Erstellen eines Exportauftrags](storage-import-export-creating-an-export-job.md)

[Abrufen von Statusinformationen für einen Auftrag](storage-import-export-retrieving-state-info-for-a-job.md)

[Aufzählen von Aufträgen](storage-import-export-enumerating-jobs.md)

[Abbrechen und Löschen von Aufträgen](storage-import-export-cancelling-and-deleting-jobs.md)

[Sichern von Laufwerkmanifesten](storage-import-export-backing-up-drive-manifests.md)

[Diagnose und Fehlerbehebung für Import/Export-Aufträge](storage-import-export-diagnostics-and-error-recovery.md)

## <a name="see-also"></a>Weitere Informationen
 [Speicherimport/-export – REST](/rest/api/storageimportexport)



<!--HONumber=Dec16_HO3-->


