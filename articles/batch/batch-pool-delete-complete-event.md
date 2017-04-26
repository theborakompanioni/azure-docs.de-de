---
title: "Ereignis zum Abschluss des Löschvorgangs von Pools: Azure | Microsoft-Dokumentation"
ms.custom: 
ms.date: 2017-02-01
ms.prod: azure
ms.reviewer: 
ms.service: batch
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 580a1278-5740-4143-826c-7d875ef127ff
caps.latest.revision: 5
author: tamram
ms.author: tamram
manager: timlt
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 991ee552ccc564be149844b58a0a3fea7452f2a3
ms.lasthandoff: 04/13/2017

---
# <a name="pool-delete-complete-event"></a>Ereignis zum Abschluss des Löschvorgangs von Pools
Text des Protokolls zum Ereignis zum Abschluss des Löschvorgangs von Pools

## <a name="remarks"></a>Anmerkungen
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
