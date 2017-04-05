---
title: "Abbrechen und Löschen eines Azure Import/Export-Auftrags | Microsoft-Dokumentation"
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
ms.date: 01/23/2017
ms.author: muralikk
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: e0a7ff391e5a03ed563912dea54c7cfe73111bcf
ms.lasthandoff: 03/30/2017


---

# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Abbrechen und Löschen von Azure Import/Export-Aufträgen

Sie können anfordern, dass ein Auftrag abgebrochen wird, bevor er den Status `Packaging` aufweist. Dazu rufen Sie den [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update)-Vorgang auf und legen das `CancelRequested`-Element auf `true` fest. Der Auftrag wird auf Grundlage der besten Leistung abgebrochen. Wenn Laufwerke gerade Daten übertragen, wird die Datenübertragung möglicherweise auch nach der Abbruchanforderung fortgesetzt.

 Ein abgebrochener Auftrag wechselt in den `Completed`-Status, wird 90 Tage lang aufbewahrt und anschließend gelöscht.

 Zum Löschen eines Auftrags rufen Sie den Vorgang [Auftrag löschen](/rest/api/storageimportexport/jobs#Jobs_Delete) auf, bevor der Auftrag gesendet wird (*d.h.* während der Auftrag den Status `Creating` hat). Sie können einen Auftrag auch löschen, wenn er den Status `Completed` aufweist. Nachdem ein Auftrag gelöscht wurde, sind die zugehörigen Informationen und der Status nicht mehr über die REST-API oder das Azure-Portal zugänglich.

## <a name="next-steps"></a>Nächste Schritte

* [Verwenden der REST-API des Import/Export-Diensts](storage-import-export-using-the-rest-api.md)

