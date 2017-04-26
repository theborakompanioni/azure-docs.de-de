---
title: 'Taskfehlerereignis: Azure | Microsoft-Dokumentation'
ms.custom: 
ms.date: 2017-02-01
ms.prod: azure
ms.reviewer: 
ms.service: batch
ms.suite: 
ms.tgt_pltfrm: 
ms.topic: reference
ms.assetid: 8c16a533-1ac7-4b65-a84e-8eafb937b3d7
caps.latest.revision: 3
author: tamram
ms.author: tamram
manager: timlt
translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 880fe1cfb0496311d7c386f25762c5d02a6c91f4
ms.lasthandoff: 04/13/2017

---
# <a name="task-fail-event"></a>Taskfehlerereignis
Text des Protokolls zum Taskfehlerereignis

## <a name="remarks"></a>Anmerkungen
 Dieses Ereignis wird ausgegeben, wenn ein Task mit einem Fehler abgeschlossen wird. Derzeit gelten alle Exitcodes ungleich null als Fehler. Dieses Ereignis wird *zusätzlich* zum Ereignis zum Abschluss eines Tasks ausgegeben und dient zum Ermitteln, wann ein Task fehlgeschlagen ist.


 Das folgende Beispiel zeigt den Text eines Taskfehlerereignisses.

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
        "startTime": "2016-09-08T16:32:23.799Z",
        "endTime": "2016-09-08T16:34:00.666Z",
        "exitCode": 1,
        "retryCount": 2,
        "requeueCount": 0
    }
}
```

|Elementname|Typ|Hinweise|
|------------------|----------|-----------|
|jobId|String|Die ID des Auftrags, der den Task enthält.|
|id|String|Die ID des Tasks.|
|taskType|String|Der Typ des Tasks. Entweder „JobManager“, was bedeutet, dass dies ein Auftrags-Manager-Task ist, oder „User“, was bedeutet, dass dies nicht der Fall ist. Dieses Ereignis wird nicht für Auftragsvorbereitungstasks, Auftragsfreigabetasks oder Starttasks ausgegeben.|
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
|numberOfInstances|Int32|Die Anzahl der Computeknoten, die vom Task benötigt werden.|

###  <a name="constraints"></a> Einschränkungen

|Elementname|Typ|Hinweise|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Gibt an, wie oft der Task maximal wiederholt werden kann. Der Batch-Dienst wiederholt einen Task, wenn sein Exitcode ungleich null ist.<br /><br /> Beachten Sie, dass dieser Wert die Anzahl der Wiederholungen ausdrücklich steuert. Der Batch-Dienst wiederholt den Task einmal und kann ihn anschließend bis zu diesem Grenzwert wiederholen. Wenn beispielsweise die maximale Anzahl von Wiederholungsversuchen 3 ist, versucht der Batch-Dienst einen Task bis zu viermal (ein erster Versuch und drei Wiederholungsversuche).<br /><br /> Wenn die maximale Anzahl von Wiederholungsversuchen 0 ist, wiederholt der Batch-Dienst Tasks nicht.<br /><br /> Wenn die maximale Anzahl von Wiederholungsversuchen -1 ist, wiederholt der Batch-Dienst Tasks unbegrenzt.<br /><br /> Der Standardwert ist 0 (keine Wiederholungsversuche).|


###  <a name="executionInfo"></a> executionInfo

|Elementname|Typ|Hinweise|
|------------------|----------|-----------|
|startTime|DateTime|Der Zeitpunkt, an dem die Ausführung des Tasks gestartet wurde. „Running“ entspricht dem Status **Wird ausgeführt**. Wenn also der Task Ressourcendateien oder Anwendungspakete angibt, reflektiert die Startzeit den Zeitpunkt, an dem der Task mit dem Herunterladen oder Bereitstellen dieser Elemente begonnen hat.  Wenn der Task neu gestartet oder wiederholt wurde, ist dies der letzte Zeitpunkt, an dem die Ausführung des Tasks gestartet wurde.|
|endTime|DateTime|Der Zeitpunkt, an dem die Ausführung des Tasks beendet wurde.|
|exitCode|Int32|Der Exitcode des Tasks.|
|retryCount|Int32|Die Häufigkeit, mit der der Task vom Batch-Dienst wiederholt wurde. Der Vorgang wird wiederholt, wenn der Exitcode ungleich null ist, und zwar bis zum angegebenen Wert von „MaxTaskRetryCount“.|
|requeueCount|Int32|Die Häufigkeit, mit der der Tasks vom Batch-Dienst als Ergebnis einer Benutzeranforderung erneut in die Warteschlange gestellt wurde.<br /><br /> Wenn der Benutzer Knoten aus einem Pool entfernt (durch Vergrößern oder Verkleinern des Pools) oder der Auftrag deaktiviert wird, kann der Benutzer angeben, dass auf den Knoten ausgeführte Tasks zur Ausführung erneut in die Warteschlange gestellt werden. Dieser Zähler überwacht, wie oft der Task aus diesen Gründen in die Warteschlange gestellt wurde.|

