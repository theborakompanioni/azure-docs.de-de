---
title: "Azure Batch: Ereignis zum Starten der Größenänderung von Pools | Microsoft-Dokumentation"
description: "Referenz zum Batch-Ereignis zum Starten der Größenänderung von Pools."
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
ms.openlocfilehash: 826cd984d26b923ba38562e05a2e75c399be9121
ms.lasthandoff: 04/22/2017

---

# <a name="pool-resize-start-event"></a>Ereignis zum Starten der Größenänderung von Pools: Azure

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

