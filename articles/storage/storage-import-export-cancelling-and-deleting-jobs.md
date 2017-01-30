---
title: "Abbrechen und Löschen von Aufträgen des Azure Import/Export-Diensts | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Aufträge für den Microsoft Azure Import/Export-Dienst abbrechen und löschen."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: fd3d66f0-1dbb-4c75-9223-307d5abaeefc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/01/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 22e37e26fec913a7638c71b2547c38f5efacd10b
ms.openlocfilehash: 640b814e4280f5cce029bea7324881bbfef4b1a4


---

# <a name="cancelling-and-deleting-jobs"></a>Abbrechen und Löschen von Aufträgen
Sie können anfordern, dass ein Auftrag abgebrochen wird, bevor er den Status `Packaging` aufweist. Dazu rufen Sie den [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update)-Vorgang auf und legen das `CancelRequested`-Element auf `true` fest. Der Auftrag wird auf Grundlage der besten Leistung abgebrochen. Wenn Laufwerke gerade Daten übertragen, wird die Datenübertragung möglicherweise auch nach der Abbruchanforderung fortgesetzt.

 Ein abgebrochener Auftrag wechselt in den `Completed`-Status, wird 90 Tage lang aufbewahrt und anschließend gelöscht.

 Zum Löschen eines Auftrags rufen Sie den Vorgang [Auftrag löschen](/rest/api/storageimportexport/jobs#Jobs_Delete) auf, bevor der Auftrag gesendet wird (*d.h.* während der Auftrag den Status `Creating` hat). Sie können einen Auftrag auch löschen, wenn er den Status `Completed` aufweist. Nachdem ein Auftrag gelöscht wurde, sind die zugehörigen Informationen und der Status nicht mehr über die REST-API oder das Azure-Portal zugänglich.

## <a name="see-also"></a>Weitere Informationen
 [Verwenden der REST-API des Import/Export-Diensts](storage-import-export-using-the-rest-api.md)



<!--HONumber=Dec16_HO3-->


