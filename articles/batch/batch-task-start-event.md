---
title: 'Taskstartereignis: Azure | Microsoft-Dokumentation'
ms.custom: 
ms.date: 2017-02-01
ms.prod: azure
ms.reviewer: 
ms.service: batch
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 95f7762a-a715-4c83-907b-8aed004e69b1
caps.latest.revision: 3
author: tamram
ms.author: tamram
manager: timlt
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 81e27c1db2687c819aee15646c2c72e8765293c8
ms.lasthandoff: 04/13/2017

---
# <a name="task-start-event"></a>Taskstartereignis
Text des Protokolls zum Taskstartereignis

## <a name="remarks"></a>Anmerkungen
 Dieses Ereignis wird einmal ausgegeben, nachdem ein Task vom Scheduler für den Start auf einem Computeknoten geplant wurde. Wenn dieser Task wiederholt oder erneut in die Warteschlange gestellt wird, wird dieses Ereignis erneut für denselben Task ausgegeben, doch der Wiederholungszähler und die Systemtaskversion werden entsprechend aktualisiert.


 Das folgende Beispiel zeigt den Text eines Taskstartereignisses.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "retryCount": 0
    }
}
```

|Elementname|Typ|Hinweise|
|------------------|----------|-----------|
|jobId|String|Die ID des Auftrags, der den Task enthält.|
|id|String|Die ID des Tasks.|
|taskType|String|Der Typ des Tasks. Entweder „JobManager“, was bedeutet, dass dies ein Auftrags-Manager-Task ist, oder „User“, was bedeutet, dass dies nicht der Fall ist.|
|systemTaskVersion|Int32|Dies ist der interne Wiederholungszähler für einen Task. Der Batch-Dienst kann intern einen Task wiederholen, um vorübergehende Probleme zu berücksichtigen. Bei diesen Problemen kann es sich um interne Planungsfehler oder Versuche handeln, Computeknoten mit einem fehlerhaften Status wiederherzustellen.|
|[nodeInfo](#nodeInfo)|Komplexer Typ|Enthält Informationen zu den Computeknoten, auf dem der Task ausgeführt wurde.|
|[multiInstanceSettings](#multiInstanceSettings)|Komplexer Typ|Gibt an, dass der Task ein Task mit mehreren Instanzen ist, für den mehrere Computeknoten erforderlich sind.  Details finden Sie unter [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task).|
|[constraints](#constraints)|Komplexer Typ|Die Ausführungseinschränkungen, die für diesen Task gelten.|
|[executionInfo](#executionInfo)|Komplexer Typ|Enthält Informationen zur Ausführung des Tasks.|

###  <a name="nodeInfo"></a> nodeInfo

|Elementname|Typ|Hinweise|
|------------------|----------|-----------|
|poolId|String|Die ID des Pools, auf den der Task angewendet wurde.|
|nodeId|String|Die ID des Knotens, auf dem der Task ausgeführt wurde.|

###  <a name="multiInstanceSettings"></a> multiInstanceSettings

|Elementname|Typ|Hinweise|
|------------------|----------|-----------|
|numberOfInstances|int|Die Anzahl der Computeknoten, die vom Task benötigt werden.|

###  <a name="constraints"></a> Einschränkungen

|Elementname|Typ|Hinweise|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Gibt an, wie oft der Task maximal wiederholt werden kann. Der Batch-Dienst wiederholt einen Task, wenn sein Exitcode ungleich null ist.<br /><br /> Beachten Sie, dass dieser Wert die Anzahl der Wiederholungen ausdrücklich steuert. Der Batch-Dienst wiederholt den Task einmal und kann ihn anschließend bis zu diesem Grenzwert wiederholen. Wenn beispielsweise die maximale Anzahl von Wiederholungsversuchen 3 ist, versucht der Batch-Dienst einen Task bis zu viermal (ein erster Versuch und drei Wiederholungsversuche).<br /><br /> Wenn die maximale Anzahl von Wiederholungsversuchen 0 ist, wiederholt der Batch-Dienst Tasks nicht.<br /><br /> Wenn die maximale Anzahl von Wiederholungsversuchen -1 ist, wiederholt der Batch-Dienst Tasks unbegrenzt.<br /><br /> Der Standardwert ist 0 (keine Wiederholungsversuche).|

###  <a name="executionInfo"></a> executionInfo

|Elementname|Typ|Hinweise|
|------------------|----------|-----------|
|retryCount|Int32|Die Häufigkeit, mit der der Task vom Batch-Dienst wiederholt wurde. Der Vorgang wird wiederholt, wenn der Exitcode ungleich null ist, und zwar bis zum angegebenen Wert von „MaxTaskRetryCount“.|

