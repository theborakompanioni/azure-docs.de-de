---
title: "Azure Batch: Ereignis zum Starten des Löschvorgangs von Pools | Microsoft-Dokumentation"
description: "Referenz zum Batch-Ereignis zum Starten des Löschvorgangs von Pools."
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
ms.openlocfilehash: f8a5241dce422e5c826ab428da6d7bc93284a1cf
ms.lasthandoff: 04/22/2017

---

# <a name="pool-delete-start-event"></a>Ereignis zum Starten des Löschvorgangs von Pools

 Dieses Ereignis wird ausgegeben, wenn ein Löschvorgang eines Pools gestartet wurde. Da das Löschen des Pools ein asynchrones Ereignis ist, können Sie damit rechnen, dass ein Ereignis zum Abschluss des Löschvorgangs von Pools ausgegeben wird, sobald der Löschvorgang abgeschlossen ist.

 Das folgende Beispiel zeigt den Text eines Ereignisses zum Starten des Löschvorgangs von Pools.

```
{
    "id": "myPool1"
}
```

|Element|Typ|Hinweise|
|-------------|----------|-----------|
|id|String|Die ID des Pools.|
