---
title: "Ereignis zum Starten des Löschvorgangs von Pools: Azure | Microsoft-Dokumentation"
ms.custom: 
ms.date: 2017-02-01
ms.prod: azure
ms.reviewer: 
ms.service: batch
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: feddca1e-c69c-4257-be44-a36172e77157
caps.latest.revision: 4
author: tamram
ms.author: tamram
manager: timlt
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: ef61238c6dc342173215ae628565a0b87a86bcad
ms.lasthandoff: 04/13/2017

---
# <a name="pool-delete-start-event"></a>Ereignis zum Starten des Löschvorgangs von Pools
Text des Protokolls zum Starten des Löschvorgangs von Pools

## <a name="remarks"></a>Anmerkungen
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
