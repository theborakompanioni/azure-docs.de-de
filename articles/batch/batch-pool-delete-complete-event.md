---
title: "Azure Batch: Ereignis zum Abschluss des Löschvorgangs von Pools | Microsoft-Dokumentation"
description: "Referenz zum Batch-Ereignis zum Abschluss des Löschvorgangs von Pools."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 890f2ba7fda37060c56177868d6214d517d91831
ms.lasthandoff: 04/22/2017

---

# <a name="pool-delete-complete-event"></a>Ereignis zum Abschluss des Löschvorgangs von Pools

 Dieses Ereignis wird ausgegeben, wenn ein Löschvorgang eines Pools abgeschlossen wurde.

 Das folgende Beispiel zeigt den Text eines Ereignisses zum Abschluss des Löschvorgangs von Pools.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Element|Typ|Hinweise|
|-------------|----------|-----------|
|id|String|Die ID des Pools.|
|startTime|DateTime|Die Startzeit des Löschvorgangs des Pools.|
|endTime|DateTime|Die Endzeit des Löschvorgangs des Pools.|

## <a name="remarks"></a>Anmerkungen
Weitere Informationen zu Status und Fehlercodes für den Löschvorgang des Pools finden Sie unter [Löschen eines Pools aus einem Konto](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).
