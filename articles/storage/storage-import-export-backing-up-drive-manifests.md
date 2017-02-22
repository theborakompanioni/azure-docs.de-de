---
title: Sichern von Azure-Import/Export-Laufwerkmanifesten | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie die automatische Sicherung Ihrer Laufwerkmanifeste für den Microsoft Azure Import/Export-Dienst einrichten."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 594abd80-b834-4077-a474-d8a0f4b7928a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 22e37e26fec913a7638c71b2547c38f5efacd10b
ms.openlocfilehash: 3d3abfb93ba09aa9f09258743ef6d4c1fc932ae5


---

# <a name="backing-up-drive-manifests"></a>Sichern von Laufwerkmanifesten
Laufwerkmanifeste können automatisch in Blobs gesichert werden, indem die `BackupDriveManifest`-Eigenschaft in den Vorgängen [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) oder [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) auf `true` festlegen. Die Laufwerkmanifeste werden standardmäßig nicht gesichert. Die Sicherungen der Laufwerkmanifeste werden als Blockblobs in einem Container innerhalb des Speicherkontos gespeichert, das dem Auftrag zugeordnet ist. Der Standardname des Containers ist `waimportexport`, Sie können jedoch beim Aufrufen der Vorgänge `Put Job` oder `Update Job Properties` in der `DiagnosticsPath`-Eigenschaft einen anderen Namen angeben. Die Sicherung des Manifestblobs wird im folgenden Format benannt: `waies/jobname_driveid_timestamp_manifest.xml`.

 Sie können den URI der gesicherten Laufwerkmanifeste für einen Auftrag abrufen, indem Sie den Vorgang [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) aufrufen. Der Blob-URI wird in der `ManifestUri`-Eigenschaft für die einzelnen Laufwerke zurückgegeben.

## <a name="see-also"></a>Weitere Informationen
 [Verwenden der REST-API des Import/Export-Diensts](storage-import-export-using-the-rest-api.md)



<!--HONumber=Dec16_HO3-->


