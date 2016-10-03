<properties
	pageTitle="Taskabhängigkeiten in Azure Batch | Microsoft Azure"
	description="Erstellen Sie Tasks, die vom erfolgreichen Abschluss anderer Tasks abhängig sind, um Vorgänge vom MapReduce-Typ und ähnliche Big Data-Workloads in Azure Batch zu verarbeiten."
	services="batch"
	documentationCenter=".net"
	authors="mmacy"
	manager="timlt"
	editor="" />

<tags
	ms.service="batch"
	ms.devlang="multiple"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows"
	ms.workload="big-compute"
	ms.date="06/29/2016"
	ms.author="marsma" />

# Taskabhängigkeiten in Azure Batch

Das Feature für Taskabhängigkeiten in Azure Batch eignet sich hervorragend für die Verarbeitung der folgenden Workloads und Aufträge:

- Workloads vom Typ MapReduce in der Cloud.
- Aufträge, deren Datenverarbeitungstasks als gerichteter azyklischer Graph (DAG) ausgedrückt werden können.
- Alle anderen Aufträge, in denen nachgelagerte Tasks von der Ausgabe vorgelagerter Tasks abhängen.

Mit diesem Feature können Sie Tasks erstellen, die erst nach dem erfolgreichen Abschluss von mindestens einem anderen Task auf Computeknoten ausgeführt werden. Beispielsweise können Sie einen Auftrag erstellen, der jedes Einzelbild eines 3D-Films mit separaten, parallelen Tasks rendert. Der letzte Task – der „Zusammenführungstask“ – fügt die gerenderten Einzelbilder erst als vollständigen Film zusammen, nachdem alle Einzelbilder erfolgreich gerendert worden sind.

Sie können Tasks erstellen, die in einer 1:1- oder 1:n-Beziehung von anderen Tasks abhängen. Sie können sogar eine Bereichsabhängigkeit erstellen, bei der ein Task vom erfolgreichen Abschluss einer Gruppe von Tasks innerhalb eines bestimmten Task-ID-Bereichs abhängig ist. Sie können diese drei grundlegenden Szenarien auch kombinieren, um m:n-Beziehungen zu erstellen.

## Taskabhängigkeiten bei Batch .NET

In diesem Artikel wird beschrieben, wie Sie Taskabhängigkeiten mit der [.NET-Bibliothek für Batch][net_msdn] konfigurieren. Zuerst zeigen wir Ihnen, wie Sie die [Abhängigkeit von Tasks für Aufträge aktivieren](#enable-task-dependencies), und dann wird kurz beschrieben, wie Sie [einen Task mit Abhängigkeiten konfigurieren](#create-dependent-tasks). Zuletzt geht es um die von Batch unterstützten [Abhängigkeitsszenarien](#dependency-scenarios).

## Aktivieren von Taskabhängigkeiten

Um Taskabhängigkeiten in der Batch-Anwendung verwenden zu können, müssen Sie dem Batch-Dienst zuerst mitteilen, dass für den Auftrag Taskabhängigkeiten verwendet werden. Führen Sie die Aktivierung in Batch .NET für Ihren [CloudJob][net_cloudjob] durch, indem Sie die dazugehörige [UsesTaskDependencies][net_usestaskdependencies]-Eigenschaft auf `true` festlegen:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Im obigen Codeausschnitt ist „batchClient“ eine Instanz der [BatchClient][net_batchclient]-Klasse.

## Erstellen von abhängigen Tasks

Zum Erstellen eines Tasks, der vom erfolgreichen Abschluss von mindestens eines anderen Tasks abhängig ist, legen Sie in Batch fest, dass der Task von den anderen Tasks abhängt („Abhängig von“). Konfigurieren Sie in Batch .NET die [CloudTask][net_cloudtask].[DependsOn][net_dependson]-Eigenschaft mit einer Instanz der [TaskDependencies][net_taskdependencies]-Klasse:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Mit diesem Codeausschnitt wird ein Task mit der ID „Flowers“ erstellt, dessen Ausführung auf einem Computeknoten erst dann geplant ist, nachdem die Tasks mit den IDs „Rain“ und „Sun“ erfolgreich abgeschlossen wurden.

 > [AZURE.NOTE] Ein Task wird als abgeschlossen angesehen, wenn er den Zustand **abgeschlossen** aufweist und der **Exitcode** den Wert `0` hat. In Batch .NET bedeutet dies, dass der [CloudTask][net_cloudtask].[State][net_taskstate]-Eigenschaftswert `Completed` und der [TaskExecutionInformation][net_taskexecutioninformation].[ExitCode][net_exitcode]-Eigenschaftswert von CloudTask `0` lautet.

## Abhängigkeitsszenarien

Es gibt drei grundlegende Szenarien für Abhängigkeiten von Tasks, die Sie in Azure Batch verwenden können: 1:1, 1:n und Task-ID-Bereich. Diese können Sie kombinieren, um ein viertes Szenario zu schaffen, und zwar „m:n“.

 Szenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Beispiel | |
 :-------------------: | ------------------- | -------------------
 [1:1](#one-to-one) | *TaskB* hängt von *TaskA* ab<p/> *TaskB* wird erst ausgeführt, nachdem *TaskA* erfolgreich abgeschlossen wurde | ![Diagramm: 1:1-Abhängigkeit von Tasks][1]
 [1:n](#one-to-many) | *TaskC* hängt von *TaskA* und *TaskB* ab<p/> *TaskC* wird erst ausgeführt, nachdem *TaskA* und *TaskB* erfolgreich abgeschlossen wurden | ![Diagramm: 1:n-Abhängigkeit von Tasks][2]
 [Task-ID-Bereich](#task-id-range) | *TaskD* hängt von einem Taskbereich ab <p/> *TaskD* wird erst ausgeführt, nachdem die Tasks mit den IDs *1* bis *10* erfolgreich abgeschlossen wurden | ![Diagramm: Task-ID-Bereich-Abhängigkeit][3]

>[AZURE.TIP] Sie können **m:n**-Beziehungen erstellen, bei denen beispielsweise die Tasks C, D, E und F jeweils von den Tasks A und B abhängen. Dies ist beispielsweise für Fälle mit parallelisierter Vorverarbeitung nützlich, in denen Ihre nachgelagerten Tasks von der Ausgabe mehrerer vorgelagerter Tasks abhängig sind.

### 1:1

Um einen Task mit einer Abhängigkeit von der erfolgreichen Ausführung eines anderen Tasks zu erstellen, stellen Sie eine einzelne Task-ID für die statische [TaskDependencies][net_taskdependencies].[OnId][net_onid]-Methode bereit, wenn Sie die [DependsOn][net_dependson]-Eigenschaft der [CloudTask][net_cloudtask] auffüllen.

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

### 1:n

Um einen Task mit einer Abhängigkeit von der erfolgreichen Ausführung mehrerer Tasks zu erstellen, stellen Sie eine Sammlung von Task-IDs für die statische [TaskDependencies][net_taskdependencies].[OnIds][net_onids]-Methode bereit, wenn Sie die [DependsOn][net_dependson]-Eigenschaft der [CloudTask][net_cloudtask] auffüllen.

```csharp
// 'Rain' and 'Sun' don't depend on any other tasks
new CloudTask("Rain", "cmd.exe /c echo Rain"),
new CloudTask("Sun", "cmd.exe /c echo Sun"),

// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

### Task-ID-Bereich

Um einen Task mit einer Abhängigkeit von der erfolgreichen Ausführung einer Gruppe von Tasks zu erstellen, deren IDs innerhalb eines bestimmten Bereichs liegen, stellen Sie die erste und die letzte Task-ID des Bereichs für die statische [TaskDependencies][net_taskdependencies].[OnIdRange][net_onidrange]-Methode bereit, wenn Sie die [DependsOn][net_dependson]-Eigenschaft der [CloudTask][net_cloudtask] auffüllen.

>[AZURE.IMPORTANT] Wenn Sie Task-ID-Bereiche für Ihre Abhängigkeiten verwenden, *muss* es sich bei den Task-IDs im Bereich um Zeichenfolgendarstellungen von Ganzzahlwerten handeln. Außerdem muss jeder Task im Bereich erfolgreich abgeschlossen werden, damit die Ausführung des abhängigen Tasks geplant wird.

```csharp
// Tasks 1, 2, and 3 don't depend on any other tasks. Because
// we will be using them for a task range dependency, we must
// specify string representations of integers as their ids.
new CloudTask("1", "cmd.exe /c echo 1"),
new CloudTask("2", "cmd.exe /c echo 2"),
new CloudTask("3", "cmd.exe /c echo 3"),

// Task 4 depends on a range of tasks, 1 through 3
new CloudTask("4", "cmd.exe /c echo 4")
{
    // To use a range of tasks, their ids must be integer values.
    // Note that we pass integers as parameters to TaskIdRange,
    // but their ids (above) are string representations of the ids.
    DependsOn = TaskDependencies.OnIdRange(1, 3)
},
```

## Codebeispiel

Das Beispielprojekt [TaskDependencies][github_taskdependencies] ist eines der [Azure Batch-Codebeispiele][github_samples] auf GitHub. Mit dieser Visual Studio 2015-Projektmappe wird veranschaulicht, wie Sie Abhängigkeiten von Tasks für einen Auftrag aktivieren, Tasks erstellen, die von anderen Tasks abhängen, und diese Tasks in einem Pool mit Computeknoten ausführen.

## Nächste Schritte

### Anwendungsbereitstellung

Das Batch-Feature [Anwendungspakete](batch-application-packages.md) bietet eine einfache Möglichkeit zum Bereitstellen und Versionieren der Anwendungen, die von Ihren Tasks auf Computeknoten ausgeführt werden.

### Installieren von Anwendungen und Bereitstellen von Daten (Staging)

Der Beitrag [Installing applications and staging data on Batch compute nodes][forum_post] \(Installieren von Anwendungen und Bereitstellen von Daten auf Batch-Computeknoten) im Azure Batch-Forum bietet einen Überblick über die verschiedenen Methoden, mit denen Knoten für die Ausführung von Tasks vorbereitet werden können. Der Beitrag wurde von einem Mitglied des Azure Batch-Teams verfasst und enthält alle relevanten Informationen, um sich mit den verschiedenen Vorgehensweisen vertraut zu machen, mit denen sich Dateien (einschließlich Anwendungen und Taskeingabedaten) auf Computeknoten bereitstellen lassen. Darüber hinaus zeigt der Beitrag wichtige Aspekte auf, die bei den einzelnen Methoden berücksichtigt werden sollten.

[forum_post]: https://social.msdn.microsoft.com/Forums/de-DE/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_taskdependencies]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_dependson]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.dependson.aspx
[net_exitcode]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.exitcode.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_onid]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onid.aspx
[net_onids]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onids.aspx
[net_onidrange]: https://msdn.microsoft.com/library/microsoft.azure.batch.taskdependencies.onidrange.aspx
[net_taskexecutioninformation]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskexecutioninformation.aspx
[net_taskstate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.taskstate.aspx
[net_usestaskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.usestaskdependencies.aspx
[net_taskdependencies]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskdependencies.aspx

[1]: ./media/batch-task-dependency/01_one_to_one.png "Diagramm: 1:1-Abhängigkeit"
[2]: ./media/batch-task-dependency/02_one_to_many.png "Diagramm: 1:n-Abhängigkeit"
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagramm: Task-ID-Bereich-Abhängigkeit"

<!---HONumber=AcomDC_0810_2016-->