<properties
    pageTitle="Ausführen von MPI-Anwendungen in Azure Batch mit Tasks mit mehreren Instanzen | Microsoft Azure"
    description="Erfahren Sie, wie MPI-Anwendungen (Message Passing Interface) mithilfe des Tasktyps mit mehreren Instanzen in Azure Batch ausgeführt werden."
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
    ms.date="09/29/2016"
    ms.author="marsma" />


# <a name="use-multi-instance-tasks-to-run-message-passing-interface-(mpi)-applications-in-azure-batch"></a>Verwendung von Tasks mit mehreren Instanzen zum Ausführen von MPI-Anwendungen (Message Passing Interface) in Azure Batch

Tasks mit mehreren Instanzen ermöglichen die Ausführung eines Azure Batch-Tasks auf mehreren Computeknoten gleichzeitig. Diese Tasks machen High Performance Computing-Szenarien wie Message Passing Interface-Anwendungen (MPI) in Batch möglich. In diesem Artikel erfahren Sie, wie mithilfe der [Batch .NET][api_net]-Bibliothek Tasks mit mehreren Instanzen ausgeführt werden.

>[AZURE.NOTE] Die Beispiele in diesem Artikel konzentrieren sich auf Batch .NET, MS-MPI und Windows-Computeknoten, doch die vorgestellten Konzepte für mehrere Instanzen sind jedoch auch auf andere Plattformen und Technologien (z. B. Python und Intel MPI auf Linux-Knoten) anwendbar.

## <a name="multi-instance-task-overview"></a>Task mit mehreren Instanzen – Übersicht

In Batch wird normalerweise jeder Task auf einem einzelnen Computeknoten ausgeführt – Sie übermitteln mehrere Tasks an einen Auftrag, und Batch-Dienst plant die Ausführung der einzelnen Tasks auf einem Knoten. Durch Konfigurieren der **Einstellungen für mehrere Instanzen**eines Tasks können Sie Batch jedoch anweisen, diesen Task in Subtasks zur Ausführung auf mehreren Knoten aufzuteilen.

![Task mit mehreren Instanzen – Übersicht][1]

Wenn Sie einen Task mit Einstellungen für mehrere Instanzen an einen Auftrag übermitteln, führt Batch mehrere für Tasks mit mehreren Instanzen eindeutige Schritte aus:

1. Der Batch-Dienst teilt den Task in einen **Primärtask** und mehrere **Subtasks** auf. Die Gesamtzahl der Tasks (primärer Task und alle Subtasks) entspricht der Anzahl der **Instanzen** (Computeknoten), die Sie in den Einstellungen für mehrere Instanzen festlegen.
1. Batch legt einen Computeknoten als **Master** fest und plant die Ausführung der primären Task auf dem Master. Es plant die Ausführung der Subtasks auf den restlichen Computeknoten, die dem Task mit mehreren Instanzen zugeordnet sind (eine Unteraufgabe pro Knoten).
1. Der Primärtask und alle Subtasks laden alle **gemeinsamen Ressourcendateien** herunter, die Sie in den Einstellungen für mehreren Instanzen angeben.
1. Nachdem die gemeinsamen Ressourcendateien heruntergeladen wurden, wird der in den Einstellungen für mehrere Instanzen angegebene **Koordinationsbefehl** vom Primärtask und von den Subtasks ausgeführt. Der Koordinationsbefehl wird in der Regel für die Vorbereitung von Knoten zum Ausführen des Tasks verwendet. Dazu zählen das Starten von Diensten im Hintergrund (z. B. die `smpd.exe` von [Microsoft MPI][msmpi_msdn]) und Sicherstellen, dass die Knoten zum Verarbeiten von Nachrichten zwischen den Knoten bereit sind.
1. Der Primärtask führt den **Anwendungsbefehl** auf dem Masterknoten aus, *nachdem* der Koordinationsbefehl vom Primärtask und von allen Subtasks erfolgreich abgeschlossen wurde. Der Anwendungsbefehl ist die Befehlszeile des Tasks mit mehreren Instanzen und wird nur vom Primärtask ausgeführt. In einer [MS MPI][msmpi_msdn]-basierten Lösung führen Sie hier Ihre MPI-fähige Anwendung mit `mpiexec.exe` aus.

> [AZURE.NOTE] Obwohl er sich funktional unterscheidet, ist der „Task mit mehreren Instanzen“ kein eindeutiger Tasktyp wie [StartTask][net_starttask] oder [JobPreparationTask][net_jobprep]. Der Task mit mehreren Instanzen ist einfach eine standardmäßiger Batch-Task ([CloudTask][net_task] in Batch .NET), dessen Einstellungen für mehrere Instanzen konfiguriert wurden. In diesem Artikel bezeichnen wir sie als **Task mit mehreren Instanzen**.

## <a name="requirements-for-multi-instance-tasks"></a>Anforderungen für Tasks mit mehreren Instanzen

Tasks mit mehreren Instanzen erfordern einen Pool, in dem die **Kommunikation zwischen Knoten** aktiviert und die** gleichzeitige Ausführung von Tasks deaktiviert** ist. Wenn Sie versuchen, einen Task mit mehreren Instanzen in einem Pool auszuführen, in dem die knotenübergreifende Kommunikation deaktiviert ist oder der einen höheren *maxTasksPerNode* -Wert als 1 aufweist, wird der Task nie geplant – er bleibt auf unbestimmte Zeit im aktiven Zustand. Dieser Codeausschnitt zeigt die Erstellung eines solchen Pools mithilfe der Batch-Bibliothek für .NET.

```csharp
CloudPool myCloudPool =
    myBatchClient.PoolOperations.CreatePool(
        poolId: "MultiInstanceSamplePool",
        targetDedicated: 3
        virtualMachineSize: "small",
        cloudServiceConfiguration: new CloudServiceConfiguration(osFamily: "4"));

// Multi-instance tasks require inter-node communication, and those nodes
// must run only one task at a time.
myCloudPool.InterComputeNodeCommunicationEnabled = true;
myCloudPool.MaxTasksPerComputeNode = 1;
```

Darüber hinaus werden Tasks mit mehreren Instanzen *nur* auf Knoten in **Pools ausgeführt, die nach dem 14. Dezember 2015 erstellt wurden**.

> [AZURE.TIP] Wenn Sie für die Computeknoten in Ihrem Batch-Pool eine [für RDMA geeignete Größe](../virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) wie z.B. A9 auswählen, kann die MPI-Anwendung das hochleistungsfähige RDMA-Netzwerk (Remote Direct Memory Access) mit geringer Latenz von Azure nutzen. Die vollständige Liste der für Batch-Pools verfügbaren Computeknotengrößen finden Sie unter [Größen für Clouddienste](./../cloud-services/cloud-services-sizes-specs.md).

### <a name="use-a-starttask-for-mpi-application-installation"></a>Verwenden eines StartTask für die Installation der MPI-Anwendung

Zum Ausführen von MPI-Anwendungen mit einem Task mit mehreren Instanzen müssen Sie zunächst die MPI-Software auf den Computeknoten im Pool verfügbar machen. Dies ist der ideale Zeitpunkt, um einen [StartTask][net_starttask] zu verwenden, der immer ausgeführt wird, wenn ein Knoten mit einem Pool verknüpft oder neu gestartet wird. Dieser Codeausschnitt erstellt einen Starttask, der das MS-MPI-Setuppaket als [Ressourcendatei][net_resourcefile] festlegt, sowie die Befehlszeile, die ausgeführt wird, nachdem die Ressourcendatei auf den Knoten heruntergeladen wurde.

```csharp
// Create a StartTask for the pool which we use for installing MS-MPI on
// the nodes as they join the pool (or when they are restarted).
StartTask startTask = new StartTask
{
    CommandLine = "cmd /c MSMpiSetup.exe -unattend -force",
    ResourceFiles = new List<ResourceFile> { new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MSMpiSetup.exe", "MSMpiSetup.exe") },
    RunElevated = true,
    WaitForSuccess = true
};
myCloudPool.StartTask = startTask;

// Commit the fully configured pool to the Batch service to actually create
// the pool and its compute nodes.
await myCloudPool.CommitAsync();
```

> [AZURE.NOTE] Bei der Implementierung einer MPI-Lösung mit Tasks mit mehreren Instanzen in Batch sind Sie nicht auf die Verwendung von MS-MPI beschränkt. Sie können jede Implementierung des MPI-Standards verwenden, die mit dem Betriebssystem kompatibel ist, das Sie für die Computeknoten in Ihrem Pool angeben.

## <a name="create-a-multi-instance-task-with-batch-.net"></a>Erstellen eines Tasks mit mehreren Instanzen mithilfe von Batch .NET

Nachdem wie die Poolanforderungen und die MPI-Paketinstallation jetzt behandelt haben, erstellen wir den Task mit mehreren Instanzen. In diesem Ausschnitt erstellen wir einen Standard-[CloudTask][net_resourcefile] und konfigurieren dann seine [MultiInstanceSettings][net_multiinstance_prop]-Eigenschaft. Wie bereits erwähnt, ist der Task mit mehreren Instanzen kein eigener Tasktyp, sondern ein standardmäßiger Batch-Task, der mit Einstellungen für mehrere Instanzen konfiguriert ist.

```csharp
// Create the multi-instance task. Its command line is the "application command"
// and will be executed *only* by the primary, and only after the primary and
// subtasks execute the CoordinationCommandLine.
CloudTask myMultiInstanceTask = new CloudTask(id: "mymultiinstancetask",
    commandline: "cmd /c mpiexec.exe -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe");

// Configure the task's MultiInstanceSettings. The CoordinationCommandLine will be executed by
// the primary and all subtasks.
myMultiInstanceTask.MultiInstanceSettings =
    new MultiInstanceSettings(numberOfNodes) {
    CoordinationCommandLine = @"cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d",
    CommonResourceFiles = new List<ResourceFile> {
    new ResourceFile("https://mystorageaccount.blob.core.windows.net/mycontainer/MyMPIApplication.exe",
                     "MyMPIApplication.exe")
    }
};

// Submit the task to the job. Batch will take care of splitting it into subtasks and
// scheduling them for execution on the nodes.
await myBatchClient.JobOperations.AddTaskAsync("mybatchjob", myMultiInstanceTask);
```

## <a name="primary-task-and-subtasks"></a>Primärer Task und Subtasks

Beim Erstellen der Einstellungen für mehrere Instanzen für einen Task geben Sie die Anzahl von Computeknoten an, auf denen der Task ausgeführt werden soll. Wenn Sie den Task an einen Auftrag übermitteln, erstellt der Batch-Dienst einen **Primärtask** und eine ausreichende Anzahl von **Subtasks**, die zusammen der festgelegten Anzahl von Knoten entsprechen.

Diesen Tasks wird eine ganzzahlige ID im Bereich von 0 bis *numberOfInstances* - 1 zugewiesen. Der Task mit der ID 0 ist der primäre Task, und alle anderen IDs sind Subtasks. Wenn Sie z. B. für einen Task die folgenden -Einstellungen für mehrere Instanzen erstellen, erhält der Primärtask die ID 0, und die Subtasks erhalten die IDs 1 bis 9.

```csharp
int numberOfNodes = 10;
myMultiInstanceTask.MultiInstanceSettings = new MultiInstanceSettings(numberOfNodes);
```

### <a name="master-node"></a>Masterknoten
Wenn Sie einen Task mit mehreren Instanzen übermitteln, legt Batch einen Computeknoten als Masterknoten fest und plant die Ausführung des primären Tasks auf dem Masterknoten. Die Subtasks werden auf den restlichen Knoten ausgeführt, die dem Task mit mehreren Instanzen zugeordnet sind.

## <a name="coordination-command"></a>Koordinationsbefehl

Der **Koordinationsbefehl** wird vom Primärtask und den Subtasks ausgeführt.

Der Aufruf des Koordinationsbefehls führt zu einer Blockierung – Batch führt den Anwendungsbefehl erst dann aus, wenn der Koordinationsbefehl für alle Subtasks erfolgreich zurückgegeben wurde. Der Koordinationsbefehl sollte daher alle erforderlichen Hintergrunddienste starten, sicherstellen, dass sie einsatzbereit sind, und dann beendet werden. Dieser Koordinationsbefehl für eine Lösung mit MS-MPI Version 7 startet z. B. den SMPD-Dienst auf dem Knoten und wird dann beendet:

```
cmd /c start cmd /c ""%MSMPI_BIN%\smpd.exe"" -d
```

Beachten Sie die Verwendung von `start` in diesem Koordinationsbefehl. Dies ist erforderlich, da die `smpd.exe` -Anwendung nicht sofort nach der Ausführung zurückgegeben wird. Ohne die Verwendung des [start][cmd_start]-Befehls würde der Koordinationsbefehl nicht zurückgegeben und würde daher die Ausführung des Anwendungsbefehls blockieren.

## <a name="application-command"></a>Anwendungsbefehl

Nachdem der Primärtask und die Subtasks die Ausführung des Koordinationsbefehls abgeschlossen haben, wird die Befehlszeile des Tasks mit mehreren Instanzen *nur*vom Primärtask ausgeführt. Wir bezeichnen dies zur Unterscheidung vom Koordinationsbefehl als **Anwendungsbefehl** .

Verwenden Sie bei MS-MPI-Anwendungen den Anwendungsbefehl, um Ihre MPI-fähige Anwendung mit `mpiexec.exe` auszuführen. Hier sehen Sie ist z. B. einen Anwendungsbefehl für eine Lösung mit MS-MPI Version 7:

```
cmd /c ""%MSMPI_BIN%\mpiexec.exe"" -c 1 -wdir %AZ_BATCH_TASK_SHARED_DIR% MyMPIApplication.exe
```

>[AZURE.NOTE] Da die Datei `mpiexec.exe` von MS-MPI standardmäßig die Variable `CCP_NODES` verwendet (siehe [Umgebungsvariablen](#environment-variables)), wird sie in der obigen Beispielbefehlszeile der Anwendung ausgeschlossen.

## <a name="environment-variables"></a>Umgebungsvariablen

Batch erstellt mehrere [Umgebungsvariablen][msdn_env_var] für Tasks mit mehreren Instanzen auf den einem Task mit mehreren Instanzen zugeordneten Computeknoten. Ihre Anwendungs- und Koordinationsbefehlszeilen sowie die Skripts und Programme, die sie ausführen, können auf diese Umgebungsvariablen verweisen.

Die folgenden Umgebungsvariablen werden vom Batch-Dienst für die Verwendung von Tasks mit mehreren Instanzen erstellt:

* `CCP_NODES`
* `AZ_BATCH_NODE_LIST`
* `AZ_BATCH_HOST_LIST`
* `AZ_BATCH_MASTER_NODE`
* `AZ_BATCH_TASK_SHARED_DIR`
* `AZ_BATCH_IS_CURRENT_NODE_MASTER`

Weitere Informationen zu diesen und anderen Umgebungsvariablen der Batch-Computeknoten, ihren Inhalten und ihrer Sichtbarkeit finden Sie unter [Compute node environment variables][msdn_env_var].

>[AZURE.TIP] Das Batch-Linux-MPI-Codebeispiel enthält ein Beispiel, wie einige dieser Umgebungsvariablen verwendet werden können. Das Bashs-Sript [coordination-cmd][coord_cmd_example] lädt gemeinsame Anwendungs- und Eingabedateien aus Azure Storage herunter, aktiviert eine Network File System (NFS)-Freigabe auf dem Masterknoten und konfiguriert die anderen Knoten, die dem Task mit mehreren Aufgaben als NFS-Clients zugewiesen sind.

## <a name="resource-files"></a>Ressourcendateien

Es gibt zwei Sätze von Ressourcendateien, die bei Tasks mit mehreren Instanzen berücksichtigt werden müssen: **gemeinsame Ressourcendateien**, die von *allen* Tasks (Primärtask und Subtasks) heruntergeladen werden, und die **Ressourcendateien** für den Task mit mehreren Instanzen selbst, die *nur vom Primärtask* heruntergeladen werden.

In den Einstellungen für mehrere Instanzen eines Tasks können Sie eine oder mehrere **gemeinsame Ressourcendateien** angeben. Diese gemeinsamen Ressourcendateien werden vom Primärtask und von den Subtasks aus [Azure Storage](./../storage/storage-introduction.md) in das **freigegebene Verzeichnis** des Tasks aller Knoten heruntergeladen. Sie können mithilfe der `AZ_BATCH_TASK_SHARED_DIR` -Umgebungsvariablen über Anwendungs- und Koordinationsbefehlszeilen auf das freigegebene Verzeichnis eines Tasks zugreifen. Da der Pfad `AZ_BATCH_TASK_SHARED_DIR` ist bei allen dem Task mit mehreren Instanzen zugeordneten Knoten identisch ist, können Sie für den Primärtask und alle Subtasks denselben Koordinationsbefehl verwenden. Batch wird das Verzeichnis nicht im Sinne eines Remotzugriffs „freigeben“, aber Sie können es wie im Tipp zu Umgebungsvariablen erwähnt als Bereitstellungs- oder Freigabepunkt verwenden.

Ressourcendateien, die Sie für den Task mit mehreren Instanzen angeben, werden standardmäßig in das Arbeitsverzeichnis des Tasks (`AZ_BATCH_TASK_WORKING_DIR`) heruntergeladen. Im Gegensatz zu gemeinsamen Ressourcendateien lädt wie erwähnt nur der Primärtask die für den Task mit mehreren Instanzen angegebenen Ressourcendateien herunter.

> [AZURE.IMPORTANT] Verwenden Sie immer die Umgebungsvariablen `AZ_BATCH_TASK_SHARED_DIR` und `AZ_BATCH_TASK_WORKING_DIR`, um in Ihren Befehlszeilen auf diese Verzeichnisse zu verweisen. Versuchen Sie nicht, die Pfade manuell zu erstellen.

## <a name="task-lifetime"></a>Gültigkeitsdauer von Tasks

Die Gültigkeitsdauer des Primärtasks steuert die Gültigkeitsdauer des gesamten Tasks mit mehreren Instanzen. Wenn der Primärtask beendet wird, werden auch alle Subtasks beendet. Der Exitcode des Primärtasks ist der Exitcode des Tasks und wird daher verwendet, um den Erfolg oder Misserfolg des Tasks zu Wiederholungszwecken zu bestimmen.

Wenn beispielsweise einer der Subtasks fehlschlägt und mit einem Rückgabecode ungleich NULL beendet wird, schlägt der gesamte Task mit mehreren Instanzen fehl. Der Task mit mehreren Instanzen wird dann beendet und wiederholt, bis die Grenze für erneute Versuche erreicht ist.

Wenn Sie einen Task mit mehreren Instanzen löschen, werden der Primärtask und alle Subtasks ebenfalls vom Batch-Dienst gelöscht. Alle Subtaskverzeichnisse und die Dateien darin werden genau wie bei einem Standardtask von den Computeknoten gelöscht.

[TaskConstraints][net_taskconstraints] für einen Task mit mehreren Instanzen, z. B. die Eigenschaften [MaxTaskRetryCount][net_taskconstraint_maxretry], [MaxWallClockTime][net_taskconstraint_maxwallclock] und [RetentionTime][net_taskconstraint_retention] werden berücksichtigt, da sie für einen Standardtask angegeben sind und für den Primärtask sowie alle Subtasks gelten. Wenn Sie die Eigenschaft [RetentionTime][net_taskconstraint_retention] jedoch nach dem Hinzufügen des Tasks mit mehreren Instanzen zum Auftrag ändern, wird diese Änderung nur für den Primärtask übernommen. Alle Subtasks verwenden weiterhin die ursprüngliche Eigesnchaft [RetentionTime][net_taskconstraint_retention].

In der Liste der aktuellen Tasks eines Computeknotens wird die ID eines Subtasks angezeigt, wenn der aktuelle Task Teil eines Tasks mit mehreren Instanzen war.

## <a name="obtain-information-about-subtasks"></a>Abrufen von Informationen zu Subtasks

Zum Abrufen von Informationen zu Subtasks mithilfe der Batch-Bibliothek für .NET rufen Sie die [CloudTask.ListSubtasks][net_task_listsubtasks]-Methode auf. Diese Methode gibt Informationen zu allen Subtasks sowie zum Computeknoten zurück, auf dem die Tasks ausgeführt wurden. Anhand dieser Informationen können Sie u. a. das Stammverzeichnis, die Pool-ID, den aktuellen Zustand und den Exitcode der einzelnen Subtasks bestimmen. Diese Informationen können Sie in Kombination mit der [PoolOperations.GetNodeFile][poolops_getnodefile]-Methode zum Abrufen der Dateien des Subtasks verwenden. Beachten Sie, dass diese Methode keine Informationen für den Primärtask (ID 0) zurückgibt.

> [AZURE.NOTE] Sofern nicht anders angegeben, gelten Batch .NET-Methoden, die für den [CloudTask][net_task] mit mehreren Instanzen selbst angewendet werden, *nur* für den Primärtask. Wenn Sie beispielsweise die [CloudTask.ListNodeFiles][net_task_listnodefiles]-Methode für einen Task mit mehreren Instanzen aufrufen, werden nur die Dateien des Primärtasks zurückgegeben.

Der folgende Codeausschnitt zeigt das Abrufen von Informationen zu Subtasks sowie das Anfordern von Dateiinhalten von den Knoten, auf denen sie ausgeführt werden.

```csharp
// Obtain the job and the multi-instance task from the Batch service
CloudJob boundJob = batchClient.JobOperations.GetJob("mybatchjob");
CloudTask myMultiInstanceTask = boundJob.GetTask("mymultiinstancetask");

// Now obtain the list of subtasks for the task
IPagedEnumerable<SubtaskInformation> subtasks = myMultiInstanceTask.ListSubtasks();

// Asynchronously iterate over the subtasks and print their stdout and stderr
// output if the subtask has completed
await subtasks.ForEachAsync(async (subtask) =>
{
    Console.WriteLine("subtask: {0}", subtask.Id);
    Console.WriteLine("exit code: {0}", subtask.ExitCode);

    if (subtask.State == TaskState.Completed)
    {
        ComputeNode node =
            await batchClient.PoolOperations.GetComputeNodeAsync(subtask.ComputeNodeInformation.PoolId,
                                                                 subtask.ComputeNodeInformation.ComputeNodeId);

        NodeFile stdOutFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardOutFileName);
        NodeFile stdErrFile = await node.GetNodeFileAsync(subtask.ComputeNodeInformation.TaskRootDirectory + "\\" + Constants.StandardErrorFileName);
        stdOut = await stdOutFile.ReadAsStringAsync();
        stdErr = await stdErrFile.ReadAsStringAsync();

        Console.WriteLine("node: {0}:", node.Id);
        Console.WriteLine("stdout.txt: {0}", stdOut);
        Console.WriteLine("stderr.txt: {0}", stdErr);
    }
    else
    {
        Console.WriteLine("\tSubtask {0} is in state {1}", subtask.Id, subtask.State);
    }
});
```

## <a name="next-steps"></a>Nächste Schritte

- Im-Blogartikel des Microsoft HPC- und Azure Batch-Teams (in englischer Sprache) erfahren Sie mehr über die [MPI-Unterstützung für Linux auf Azure Batch][blog_mpi_linux] und zur Verwendung von [OpenFOAM][openfoam] mit Batch. Sie finden Python-Codebeispiele für das [OpenFOAM-Beispiel auf GitHub][Github_mpi].

- Es empfiehlt sich, eine einfache MS-MPI-Anwendung zu erstellen, die zum Testen von Tasks mit mehreren Instanzen in Batch verwendet wird. Der Blogartikel [How to compile and run a simple MS-MPI program][msmpi_howto] (Kompilieren und Ausführen eines einfachen MS-MPI-Programms) enthält eine exemplarische Vorgehensweise zum Erstellen einer einfachen MPI-Anwendung mithilfe von MS-MPI.

- Aktuelle Informationen zu MS-MPI finden Sie auf der Seite [Microsoft MPI][msmpi_msdn] auf MSDN.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[batch_explorer]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[blog_mpi_linux]: https://blogs.technet.microsoft.com/windowshpc/2016/07/20/introducing-mpi-support-for-linux-on-azure-batch/
[cmd_start]: https://technet.microsoft.com/library/cc770297.aspx
[coord_cmd_example]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/mpi/data/linux/openfoam/coordination-cmd
[github_mpi]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples/mpi
[github_samples]: https://github.com/Azure/azure-batch-samples
[msdn_env_var]: https://msdn.microsoft.com/library/azure/mt743623.aspx
[msmpi_msdn]: https://msdn.microsoft.com/library/bb524831.aspx
[msmpi_sdk]: http://go.microsoft.com/FWLink/p/?LinkID=389556
[msmpi_howto]: http://blogs.technet.com/b/windowshpc/archive/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program.aspx
[openfoam]: http://www.openfoam.com/

[net_jobprep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_multiinstance_class]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_multiinstance_prop]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.multiinstancesettings.aspx
[net_multiinsance_commonresfiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.commonresourcefiles.aspx
[net_multiinstance_coordcmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.coordinationcommandline.aspx
[net_multiinstance_numinstances]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.numberofinstances.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_pool_create]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx
[net_pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_resourcefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.resourcefile.aspx
[net_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.aspx
[net_starttask_cmdline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.starttask.commandline.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_taskconstraints]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.aspx
[net_taskconstraint_maxretry]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxtaskretrycount.aspx
[net_taskconstraint_maxwallclock]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.maxwallclocktime.aspx
[net_taskconstraint_retention]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.taskconstraints.retentiontime.aspx
[net_task_listsubtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listsubtasks.aspx
[net_task_listnodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[poolops_getnodefile]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getnodefile.aspx

[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx

[1]: ./media/batch-mpi/batch_mpi_01.png "Mehrere Instanzen – Übersicht"



<!--HONumber=Oct16_HO2-->


