---
title: "Überwachen des Auftragsstatus durch das Zählen von Tasks nach Status – Azure Batch | Microsoft-Dokumentation"
description: "Überwachen Sie den Status eines Auftrags, indem Sie den Vorgang zum Abrufen der Taskanzahl (Get Task Counts) aufrufen, um die Tasks für einen Auftrag zu zählen. Sie können Zahlen zu den aktiven, ausgeführten und abgeschlossenen Tasks sowie zu den erfolgreichen und fehlgeschlagenen Tasks abrufen."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/02/2017
ms.author: tamram
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: ceff59d7063b60a1344a47489d3d73e0e8ee07df
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="count-tasks-by-state-to-monitor-a-jobs-progress-preview"></a>Zählen von Tasks nach Status zur Überwachung des Auftragsstatus (Vorschauversion)

Azure Batch ermöglicht die effiziente Überwachung des Status eines Auftrags, da die Anwendung für die Ausführung der entsprechenden Tasks zuständig ist. Sie können den Vorgang [Get Task Counts][rest_get_task_counts] (Taskanzahl abrufen) aufrufen, um zu ermitteln, wie viele Tasks sich Status „Aktiv“, „Wird ausgeführt“ oder „Abgeschlossen“ befinden und wie viele erfolgreich bzw. nicht erfolgreich waren. Indem Sie die Anzahl von Tasks jedes Status zählen, können Sie für Benutzer leichter den Fortschrittsstatus des Auftrags anzeigen oder unerwartete Verzögerungen oder Fehler erkennen, die sich unter Umständen auf den Auftrag auswirken.

> [!IMPORTANT]
> Der Vorgang „Get Task Counts“ (Taskanzahl abrufen) befindet sich derzeit in der Vorschauphase und ist für Azure Government, Azure China und Azure Deutschland noch nicht verfügbar. 
>
>

## <a name="how-tasks-are-counted"></a>Zählen von Tasks

Mit dem Vorgang „Get Task Counts“ (Taskanzahl abrufen) werden Tasks wie folgt anhand ihres Status gezählt:

- Ein Task wird als **Aktiv** gezählt, wenn er in die Warteschlange eingereiht wurde und ausgeführt werden kann, aber gerade keinem Computeknoten zugewiesen ist. Ein Task wird auch als **Aktiv** gezählt, wenn er von einem übergeordneten Task abhängig ist, der noch nicht abgeschlossen ist. Weitere Informationen zu Taskabhängigkeiten finden Sie unter [Erstellen von Taskabhängigkeiten zum Ausführen von Tasks, die von anderen Tasks abhängen](batch-task-dependencies.md). 
- Ein Task wird als **Ausgeführt** gezählt, wenn er einem Computeknoten zugewiesen, aber noch nicht abgeschlossen wurde. Ein Task wird als **Ausgeführt** gezählt, wenn sein Status entweder `preparing` oder `running` lautet. Dies wird mit dem Vorgang [Get information about a task][rest_get_task] (Informationen zu einem Task abrufen) angegeben.
- Ein Task wird als **Abgeschlossen** gezählt, wenn er nicht mehr für die Ausführung ansteht. Ein als **Abgeschlossen** gezählter Task wurde in der Regel entweder erfolgreich abgeschlossen oder war nicht erfolgreich und hat das Wiederholungslimit ausgeschöpft. 

Mit dem Vorgang „Get Task Counts“ (Taskanzahl abrufen) wird auch gemeldet, wie viele Tasks erfolgreich waren oder fehlgeschlagen sind. Batch ermittelt, ob ein Task erfolgreich war oder nicht, indem die **result** -Eigenschaft der [executionInfo](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task#executionInfo) -Eigenschaft überprüft wird:

    - Ein Task wird als **Erfolgreich** gezählt, wenn das Ergebnis der Taskausführung `success` lautet.
    - Ein Task wird als **Fehlgeschlagen** gezählt, wenn das Ergebnis der Taskausführung `failure` lautet.

Weitere Informationen zum Status von Tasks finden Sie unter [Get information about a task][rest_get_task] (Informationen zu einem Task abrufen).

Das folgende .NET-Codebeispiel zeigt, wie Sie die Taskanzahl nach dem Status abrufen: 

```csharp
var taskCounts = await batchClient.JobOperations.GetTaskCountsAsync("job-1");

Console.WriteLine("Task count in active state: {0}", taskCounts.Active);
Console.WriteLine("Task count in preparing or running state: {0}", taskCounts.Running);
Console.WriteLine("Task count in completed state: {0}", taskCounts.Completed);
Console.WriteLine("Succeeded task count: {0}", taskCounts.Succeeded);
Console.WriteLine("Failed task count: {0}", taskCounts.Failed);
Console.WriteLine("ValidationStatus: {0}", taskCounts.ValidationStatus);
```

> [!NOTE]
> Sie können ein ähnliches Muster für REST und andere unterstützte Sprachen verwenden, um die Taskanzahl für einen Auftrag zu erhalten. 
> 
> 

## <a name="consistency-checking-for-task-counts"></a>Konsistenzprüfung für Taskanzahl

Der Batch-Dienst aggregiert die Taskanzahlergebnisse, indem Daten aus mehreren Teilen eines asynchronen verteilten Systems gesammelt werden. Zur Sicherstellung der Richtigkeit von Taskanzahlergebnissen umfasst Batch eine zusätzliche Überprüfung der Statusanzahl, bei der Konsistenzprüfungen für mehrere Komponenten des Systems durchgeführt werden. Batch führt diese Konsistenzprüfungen durch, sofern der Auftrag weniger als 200.000 Tasks enthält. Für den relativ unwahrscheinlichen Fall, dass bei der Konsistenzprüfung Fehler gefunden werden, korrigiert Batch das Ergebnis des Vorgangs „Get Tasks Counts“ (Taskanzahl abrufen) basierend auf den Ergebnissen der Konsistenzprüfung. Die Konsistenzprüfung ist eine zusätzliche Maßnahme, mit der sichergestellt wird, dass Kunden, die den Vorgang „Get Tasks Counts“ (Taskanzahl abrufen) nutzen, die richtigen benötigten Informationen für ihre Lösung erhalten.

Mit der **validationStatus**-Eigenschaft in der Antwort wird angegeben, ob die Konsistenzprüfung von Batch durchgeführt wurde. Wenn Batch die Statusanzahl nicht anhand der tatsächlichen Status im System überprüfen konnte, wird die **validationStatus**-Eigenschaft auf `unvalidated` festgelegt. Aus Leistungsgründen führt Batch die Konsistenzprüfung nicht durch, wenn der Auftrag mehr als 200.000 Tasks enthält. Die **validationStatus**-Eigenschaft ist dann also ggf. auf `unvalidated` festgelegt. Die Taskanzahl muss in diesem Fall aber nicht unbedingt falsch sein, da auch die Wahrscheinlichkeit eines sehr geringen Datenverlusts nicht hoch ist. 

Wenn sich der Status einer Aufgabe ändert, wird diese von der Aggregationspipeline innerhalb von wenigen Sekunden verarbeitet. Der Vorgang „Get Task Counts“ (Taskanzahl abrufen) spiegelt die aktualisierte Taskanzahl in diesem Zeitraum wider. Falls von der Aggregationspipeline eine Änderung des Taskstatus übersehen wird, wird diese Änderung aber erst beim nächsten Überprüfungslauf registriert. Während dieses Zeitraums ist die Taskanzahl aufgrund des übersehenen Ereignisses ggf. nicht ganz exakt, aber dies wird beim nächsten Überprüfungslauf korrigiert.

## <a name="best-practices-for-counting-a-jobs-tasks"></a>Bewährte Methoden zum Zählen der Tasks eines Auftrags

Das Aufrufen des Vorgangs „Get Task Counts“ (Taskanzahl abrufen) ist die effizienteste Möglichkeit, um anhand des Status eine grundlegende Anzahl von Tasks eines Auftrags zurückzugeben. Wenn Sie Version 2017-06-01.5.1 des Batch-Diensts verwenden, empfehlen wir Ihnen, Ihren Code so zu schreiben bzw. zu aktualisieren, dass „Get Task Counts“ (Taskanzahl abrufen) verwendet wird.

Der Vorgang „Get Task Counts“ (Taskanzahl abrufen) ist nicht in Versionen des Batch-Diensts verfügbar, die älter als 2017-06-01.5.1 sind. Falls Sie eine ältere Version des Diensts nutzen, können Sie stattdessen eine list-Abfrage zum Zählen von Tasks einsetzen. Weitere Informationen finden Sie unter [Erstellen von Abfragen zum effizienten Auflisten von Batch-Ressourcen](batch-efficient-list-queries.md).

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu den Konzepten und Features des Batch-Diensts finden Sie in der [Funktionsübersicht für Batch](batch-api-basics.md). In diesem Artikel werden die primären Batch-Ressourcen beschrieben, z.B. Pools, Computeknoten, Aufträge und Aufgaben. Außerdem enthält er eine Übersicht über die Features des Diensts.
* Informieren Sie sich über die Grundlagen der Entwicklung einer Batch-fähigen Anwendung mit der [Batch-.NET-Clientbibliothek](batch-dotnet-get-started.md) oder mit [Python](batch-python-tutorial.md). In diesen Einführungsartikeln wird schrittweise eine funktionierende Anwendung beschrieben, in der der Batch-Dienst zum Ausführen einer Workload in mehreren Computeknoten verwendet wird.


[rest_get_task_counts]: https://docs.microsoft.com/rest/api/batchservice/get-the-task-counts-for-a-job
[rest_get_task]: https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task
[rest_list_tasks]: https://docs.microsoft.com/rest/api/batchservice/list-the-tasks-associated-with-a-job

