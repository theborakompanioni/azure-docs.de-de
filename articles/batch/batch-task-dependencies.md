<properties
	pageTitle="Abhängigkeiten von Aufgaben in Azure Batch | Microsoft Azure"
	description="Erstellen Sie Aufgaben, die vom erfolgreichen Abschluss von Aufgaben abhängig sind, um Vorgänge im MapReduce-Stil und ähnliche Big Data-Workloads in Azure Batch zu verarbeiten."
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

# Abhängigkeiten von Aufgaben in Azure Batch

Wenn Sie eine Workloadberechnung im MapReduce-Stil in der Cloud verarbeiten möchten, verfügen Sie über einen Datenverarbeitungsauftrag, dessen Aufgaben als gerichteter azyklischer Graph (Directed Acyclic Graph, DAG) ausgedrückt werden können, oder einen beliebigen anderen Auftrag, bei dem nachgelagerte Aufgaben von der Ausgabe vorgelagerter Aufgaben abhängen. Hierfür kann das Feature „Abhängigkeiten von Aufgaben“ von Azure Batch Ihnen als Lösung dienen.

Mit diesem Feature können Sie Aufgaben erstellen, die für die Ausführung auf Computeknoten erst nach dem erfolgreichen Abschluss von mindestens einer anderen Aufgabe geplant sind. Beispielsweise können Sie einen Auftrag erstellen, bei dem jedes Bild eines 3D-Films mit separaten, parallelen Aufgaben gerendert wird und dessen abschließende Aufgabe – die Zusammenführungsaufgabe – die gerenderten Bilder erst dann zu einem vollständigen Film zusammenführt, nachdem alle Bilder erfolgreich gerendert wurden.

Sie können Aufgaben erstellen, die basierend auf einer 1:1- oder 1:n-Beziehung voneinander abhängig sind oder sogar über eine Bereichsabhängigkeit verfügen, bei der eine Aufgabe vom erfolgreichen Abschluss einer Gruppe von Aufgaben in einem bestimmten Bereich von Aufgaben-IDs abhängt. Sie können diese drei grundlegenden Szenarien auch kombinieren, um m:n-Beziehungen zu erstellen.

## Abhängigkeiten von Aufgaben in Verbindung mit Batch .NET

In diesem Artikel wird beschrieben, wie Sie Abhängigkeiten von Aufgaben mit der [Batch .NET][net_msdn]-Bibliothek konfigurieren. Zuerst zeigen wir Ihnen, wie Sie die [Abhängigkeit von Aufgaben für Aufträge aktivieren](#enable-task-dependency), und dann wird kurz beschrieben, wie Sie [eine Aufgabe mit Abhängigkeiten konfigurieren](#create-dependent-tasks). Zuletzt geht es um die von Batch unterstützten [Abhängigkeitsszenarien](#dependency-scenarios).

## Aktivieren der Abhängigkeiten von Aufgaben

Um Abhängigkeiten von Aufgaben in der Batch-Anwendung verwenden zu können, müssen Sie dem Batch-Dienst zuerst mitteilen, dass für den Auftrag Abhängigkeiten von Aufgaben verwendet werden. Führen Sie die Aktivierung in Batch .NET unter [CloudJob][net_cloudjob] durch, indem Sie die dazugehörige [UsesTaskDependencies][net_usestaskdependencies]-Eigenschaft auf `true` festlegen:

```csharp
CloudJob unboundJob = batchClient.JobOperations.CreateJob( "job001",
    new PoolInformation { PoolId = "pool001" });

// IMPORTANT: This is REQUIRED for using task dependencies.
unboundJob.UsesTaskDependencies = true;
```

Im obigen Codeausschnitt ist „batchClient“ eine Instanz der [BatchClient][net_batchclient]-Klasse.

## Erstellen von abhängigen Aufgaben

Zum Erstellen einer Aufgabe, die vom erfolgreichen Abschluss von mindestens einer anderen Aufgabe abhängig ist, teilen Sie Batch mit, dass die Aufgabe von den anderen Aufgaben abhängt („Abhängig von“). Konfigurieren Sie in Batch .NET die [CloudTask][net_cloudtask].[DependsOn][net_dependson]-Eigenschaft mit einer Instanz der [TaskDependencies][net_taskdependencies]-Klasse:

```csharp
// Task 'Flowers' depends on completion of both 'Rain' and 'Sun'
// before it is run.
new CloudTask("Flowers", "cmd.exe /c echo Flowers")
{
    DependsOn = TaskDependencies.OnIds("Rain", "Sun")
},
```

Mit diesem Codeausschnitt wird eine Aufgabe mit der ID „Flowers“ erstellt, deren Ausführung auf einem Computeknoten erst dann geplant ist, nachdem die Aufgaben mit den IDs „Rain“ und „Sun“ erfolgreich abgeschlossen wurden.

 > [AZURE.NOTE] Eine Aufgabe wird als erfolgreich abgeschlossen angesehen, wenn sie den Zustand **abgeschlossen** aufweist und der **Exitcode** den Wert `0` hat. In Batch .NET bedeutet dies, dass der [CloudTask][net_cloudtask].[State][net_taskstate]-Eigenschaftswert `Completed` und der [TaskExecutionInformation][net_taskexecutioninformation].[ExitCode][net_exitcode]-Eigenschaftswert von CloudTask `0` lautet.

## Abhängigkeitsszenarien

Es gibt drei grundlegende Szenarien für Abhängigkeiten von Aufgaben, die Sie in Azure Batch verwenden können: 1:1, 1:n und Aufgaben-ID-Bereich. Diese können Sie kombinieren, um ein viertes Szenario zu schaffen, und zwar „m:n“.

 Szenario&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | Beispiel | |
 :-------------------: | ------------------- | -------------------
 [1:1](#one-to-one) | *AufgabeB* hängt von *AufgabeA* ab<p/> *AufgabeB* wird erst für die Ausführung eingeplant, nachdem *AufgabeA* erfolgreich abgeschlossen wurde | ![Diagramm: 1:1-Abhängigkeit von Aufgaben][1]
 [1:n](#one-to-many) | *AufgabeC* hängt von *AufgabeA* und *AufgabeB* ab<p/> *AufgabeC* wird erst für die Ausführung eingeplant, nachdem *AufgabeA* und *AufgabeB* erfolgreich abgeschlossen wurden | ![Diagramm: 1:n-Abhängigkeit von Aufgaben][2]
 [Aufgaben-ID-Bereich](#task-id-range) | *AufgabeD* hängt von einem Bereich von Aufgaben ab <p/> *AufgabeD* wird erst für die Ausführung eingeplant, nachdem die Aufgaben mit den IDs *1* bis *10* erfolgreich abgeschlossen wurden | ![Diagramm: Aufgaben-ID-Bereich-Abhängigkeit][3]

>[AZURE.TIP] Sie können **m:n**-Beziehungen erstellen, bei denen beispielsweise die Aufgaben C, D, E und F jeweils von den Aufgaben A und B abhängen. Dies ist beispielsweise für Fälle mit parallelisierter Vorverarbeitung nützlich, in denen Ihre nachgelagerten Aufgaben von der Ausgabe mehrerer vorgelagerter Aufgaben abhängig sind.

## 1:1

Um eine Aufgabe mit einer Abhängigkeit von der erfolgreichen Ausführung einer anderen Aufgabe zu erstellen, stellen Sie eine einzelne Aufgaben-ID für die statische [TaskDependencies][net_taskdependencies].[OnId][net_onid]-Methode bereit, wenn Sie die [DependsOn][net_dependson]-Eigenschaft der [CloudTask][net_cloudtask] auffüllen.

```csharp
// Task 'taskA' doesn't depend on any other tasks
new CloudTask("taskA", "cmd.exe /c echo taskA"),

// Task 'taskB' depends on completion of task 'taskA'
new CloudTask("taskB", "cmd.exe /c echo taskB")
{
    DependsOn = TaskDependencies.OnId("taskA")
},
```

## 1:n

Um eine Aufgabe mit einer Abhängigkeit von der erfolgreichen Ausführung mehrerer Aufgaben zu erstellen, stellen Sie eine Sammlung mit Aufgaben-IDs für die statische [TaskDependencies][net_taskdependencies].[OnIds][net_onids]-Methode bereit, wenn Sie die [DependsOn][net_dependson]-Eigenschaft der [CloudTask][net_cloudtask] auffüllen.

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

## Aufgaben-ID-Bereich

Um eine Aufgabe mit einer Abhängigkeit von der erfolgreichen Ausführung einer Gruppe von Aufgaben zu erstellen, deren IDs innerhalb eines bestimmten Bereichs liegen, stellen Sie die ersten und letzten Aufgaben-IDs des Bereichs für die statische [TaskDependencies][net_taskdependencies].[OnIdRange][net_onidrange]-Methode bereit, wenn Sie die [DependsOn][net_dependson]-Eigenschaft der [CloudTask][net_cloudtask] auffüllen.

>[AZURE.IMPORTANT] Wenn Sie Aufgaben-ID-Bereiche für Ihre Abhängigkeiten verwenden, *muss* es sich bei den Aufgaben-IDs im Bereich um **Zeichenfolgendarstellungen** von **Ganzzahlwerten** handeln. Außerdem muss **jede Aufgabe im Bereich** erfolgreich abgeschlossen werden, damit die abhängige Aufgabe für die Ausführung eingeplant wird.

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

Das Beispielprojekt [TaskDependencies][github_taskdependencies] ist eines der [Azure Batch-Codebeispiele][github_samples] auf GitHub. Mit dieser Visual Studio 2015-Projektmappe wird veranschaulicht, wie Sie Abhängigkeiten von Aufgaben für einen Auftrag aktivieren, Aufgaben erstellen, die von anderen Aufgaben abhängen, und diese Aufgaben in einem Pool mit Computeknoten ausführen.

## Nächste Schritte

### Anwendungsbereitstellung

Das Feature [Anwendungspakete](batch-application-packages.md) von Batch ist eine einfache Möglichkeit zum Bereitstellen und Versionieren der Anwendungen, die von Ihren Aufgaben auf Computeknoten ausgeführt werden.

### Installieren von Anwendungen und Bereitstellen von Daten (Staging)

Der Beitrag [Installing applications and staging data on Batch compute nodes][forum_post] (Installieren von Anwendungen und Bereitstellen von Daten auf Batch-Computeknoten) im Azure Batch-Forum bietet einen Überblick über die verschiedenen Methoden, mit denen Knoten auf die Ausführung von Aufgaben vorbereitet werden können. Der Beitrag wurde von einem Mitglied des Azure Batch-Teams verfasst und ist eine gute Möglichkeit, um sich mit den verschiedenen Vorgehensweisen vertraut zu machen, mit denen sich Dateien (einschließlich Anwendungen und Aufgabeneingabedaten) auf Computeknoten bereitstellen lassen. Darüber hinaus enthält der Beitrag einige Besonderheiten, die bei den einzelnen Methoden zu berücksichtigen sind.

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
[3]: ./media/batch-task-dependency/03_task_id_range.png "Diagramm: Aufgaben-ID-Bereich-Abhängigkeit"

<!---HONumber=AcomDC_0629_2016-->