---
title: "Ereignis zum Starten der Größenänderung von Pools: Azure | Microsoft-Dokumentation"
ms.custom: 
ms.date: 2017-02-01
ms.prod: azure
ms.reviewer: 
ms.service: batch
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 8bd33e8b-6390-4a34-95dc-2e63d8bce941
caps.latest.revision: 6
author: tamram
ms.author: tamram
manager: timlt
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 5064d86063251aeed22688d8fac133c4be1a88f8
ms.lasthandoff: 04/13/2017

---
# <a name="pool-resize-start-event"></a>Ereignis zum Starten der Größenänderung von Pools: Azure
Text des Protokolls zum Starten der Größenänderung von Pools

## <a name="remarks"></a>Anmerkungen
 Dieses Ereignis wird ausgegeben, wenn die Größenänderung eines Pools gestartet wurde. Da das Ändern der Größe des Pools ein asynchrones Ereignis ist, können Sie damit rechnen, dass ein Ereignis zum Abschluss der Größenänderung von Pools ausgegeben wird, sobald der Größenänderungsvorgang abgeschlossen ist.

 Das folgende Beispiel zeigt den Text eines Ereignisses zum Starten der Größenänderung von Pools für einen Pool, der manuell von 0 auf 2 Knoten vergrößert wurde.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Element|Typ|Hinweise|
|-------------|----------|-----------|
|poolId|String|Die ID des Pools.|
|nodeDeallocationOption|String|Gibt ab, ob Knoten ggf. aus dem Pool entfernt werden müssen, wenn sich die Poolgröße verringert.<br /><br /> Mögliche Werte:<br /><br /> **requeue**: Beendet ausgeführte Tasks, die erneut in die Warteschlange gestellt werden. Die Tasks werden erneut ausgeführt, sobald der Auftrag aktiviert ist. Entfernen Sie Knoten, sobald Tasks abgeschlossen wurden.<br /><br /> **terminate**: Beendet derzeit ausgeführte Tasks. Die Tasks werden nicht erneut ausgeführt. Entfernen Sie Knoten, sobald Tasks abgeschlossen wurden.<br /><br /> **taskcompletion**: Lässt das Abschließen aktuell ausgeführter Tasks zu. Planen Sie beim Warten keine neuen Tasks. Entfernen Sie Knoten, sobald alle Aufgaben abgeschlossen sind.<br /><br /> **Retaineddata**: Lässt das Abschließen aktuell ausgeführter Tasks zu und wartet dann, bis die Aufbewahrungszeiträume aller Taskdaten abgelaufen sind. Planen Sie beim Warten keine neuen Tasks. Entfernen Sie Knoten, sobald die Aufbewahrungszeiträume aller Tasks abgelaufen sind.<br /><br /> Der Standardwert ist „requeue“.<br /><br /> Wenn die Poolgröße zunimmt, wird der Wert auf **invalid** festgelegt.|
|currentDedicated|Int32|Die Anzahl der Computeknoten, die dem Pool derzeit zugewiesen sind.|
|targetDedicated|Int32|Die Anzahl der Computeknoten, die für den Pool angefordert werden.|
|enableAutoScale|Bool|Gibt an, ob die Poolgröße mit der Zeit automatisch angepasst wird.|
|isAutoPool|Bool|Gibt an, ob der Pool über den AutoPool-Mechanismus eines Auftrags erstellt wurde.|

