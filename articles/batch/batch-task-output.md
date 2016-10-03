<properties
	pageTitle="Beibehalten der Ausgabe von Aufträgen und Tasks in Azure Batch | Microsoft Azure"
	description="Erfahren Sie, wie Sie Azure Storage als dauerhaften Speicher für die Ausgabe von Batch-Tasks und -Aufträgen verwenden und die Anzeige dieser dauerhaft gespeicherten Ausgabe im Azure-Portal aktivieren."
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
	ms.date="09/07/2016"
	ms.author="marsma" />

# Beibehalten der Ausgabe von Azure Batch-Aufträgen und -Tasks

Die Tasks, die Sie in Batch ausführen, generieren in der Regel Ausgaben, die gespeichert und später von anderen Tasks im Auftrag und/oder von der Clientanwendung, die den Auftrag ausführt, abgerufen werden müssen. Diese Ausgaben können Dateien sein, die durch die Verarbeitung von Eingabedaten oder Protokolldateien, die mit der Taskausführung verknüpft sind, generiert werden. In diesem Artikel wird eine .NET-Klassenbibliothek beschrieben, die ein auf Konventionen basierendes Verfahren zum Beibehalten solcher Taskausgaben in Azure Blob Storage verwendet. So sind die Ausgaben auch dann noch verfügbar, wenn Sie Ihre Pools, Aufträge und Computeknoten gelöscht haben.

Mithilfe des in diesem Artikel beschriebenen Verfahrens können Sie die Taskausgabe auch im [Azure-Portal][portal] unter **Gespeicherte Ausgabedateien** und **Gespeicherte Protokolle** anzeigen.

![Auswahl von „Gespeicherte Ausgabedateien“ und „Gespeicherte Protokolle“ im Portal][1]

>[AZURE.NOTE] Die in diesem Artikel behandelte .NET-Klassenbibliothek zu [Azure Batch-Dateikonventionen][nuget_package] befindet sich derzeit in der Vorschauphase. Einige der hier beschriebenen Features werden bis zur allgemeinen Verfügbarkeit unter Umständen noch geändert.

## Überlegungen zur Taskausgabe

Beim Entwerfen Ihrer Batch-Lösung sind mehrere Faktoren zu berücksichtigen, die die Ausgabe von Aufträgen und Tasks betreffen.

* **Lebensdauer der Computeknoten**: Computeknoten sind häufig vorübergehender Natur, insbesondere in Pools mit aktivierter automatischer Skalierung. Die Ausgabe von Tasks, die auf einem Knoten ausgeführt werden, ist nur verfügbar, solange der Knoten vorhanden ist, und nur für die Dateiaufbewahrungszeit, die Sie für den Task festgelegt haben. Um sicherzustellen, dass die Taskausgabe erhalten bleibt, müssen Ihre Tasks daher die Ausgabedateien in einen permanenten Speicher hochladen, z.B. Azure Storage.

* **Ausgabespeicher**: Um die Taskausgabedaten in einem permanenten Speicher beizubehalten, können Sie das [Azure Storage-SDK](../storage/storage-dotnet-how-to-use-blobs.md) im Taskcode verwenden, um die Taskausgabe in einen Blob Storage-Container hochzuladen. Wenn Sie einen Container mit der entsprechenden Dateinamenskonvention implementieren, können die Clientanwendung oder andere Tasks im Auftrag diese Ausgabe auf Grundlage der Konvention suchen und herunterladen.

* **Abrufen der Ausgabe**: Sie können die Taskausgabe direkt aus den Computeknoten im Pool oder, wenn Ihre Tasks die Ausgabe beibehalten, aus Azure Storage abrufen. Um die Ausgabe eines Tasks direkt von einem Computeknoten abzurufen, benötigen Sie den Dateinamen und den Ausgabespeicherort auf dem Knoten. Wenn Sie die Ausgabe in Azure Storage beibehalten, müssen nachgelagerte Tasks oder die Clientanwendung über den vollständigen Dateipfad in Azure Storage verfügen, um die Datei mithilfe des Azure Storage-SDKs herunterzuladen.

* **Anzeigen der Ausgabe**: Wenn Sie im Azure-Portal zu einem Batch-Task navigieren und **Dateien auf Knoten** auswählen, werden Ihnen alle Dateien angezeigt, die mit dem Task verknüpft sind (nicht nur die Ausgabedateien, die Sie interessieren). Wie bereits erwähnt, sind Dateien auf Computeknoten nur verfügbar, solange der Knoten vorhanden ist, und nur für die Dateiaufbewahrungszeit, die Sie für den Task festgelegt haben. Um die in Azure Storage beibehaltene Taskausgabe im Portal oder einer Anwendung wie dem [Azure-Speicher-Explorer][storage_explorer] anzuzeigen, müssen Sie ihren Speicherort kennen und direkt zur gewünschten Datei navigieren.

## Hilfe für permanente Ausgabe

Um die Beibehaltung der Ausgabe von Aufträgen und Tasks zu vereinfachen, hat das Batch-Team eine Reihe von Namenskonventionen sowie eine .NET-Klassenbibliothek definiert und implementiert (die [Azure Batch-Dateikonventionenbibliothek][nuget_package]), die Sie in Ihren Batch-Anwendungen nutzen können. Darüber hinaus sind diese Namenskonventionen auch im Azure-Portal definiert. So können Sie die gespeicherten Dateien problemlos mithilfe der Bibliothek finden.

## Verwenden der Dateikonventionenbibliothek

[Azure Batch-Dateikonventionen][nuget_package] sind eine .NET-Klassenbibliothek, mit der Batch-.NET-Anwendungen Taskausgaben problemlos in Azure Storage speichern bzw. aus Azure Storage abrufen können. Diese Bibliothek ist für die Verwendung in Task- und Clientcode gedacht – im Taskcode für das Beibehalten von Dateien, im Clientcode zum Auflisten und Abrufen dieser Dateien. Ihre Tasks können die Bibliothek auch zum Abrufen der Ausgaben von vorgelagerten Tasks verwenden, z.B. in einem Szenario mit [Taskabhängigkeiten](batch-task-dependencies.md).

Die Konventionenbibliothek sorgt dafür, dass Speichercontainer und Taskausgabedateien gemäß der Konvention benannt und bei Beibehaltung in Azure Storage in das richtige Verzeichnis hochgeladen werden. Beim Abrufen von Ausgaben können Sie problemlos nach der Ausgabe für einen bestimmten Auftrag oder Task suchen, indem Sie die Ausgaben nach ID und Zweck auflisten oder abrufen, anstatt die Dateinamen oder Speicherorte in Storage kennen zu müssen.

Beispielsweise können Sie die Bibliothek für die Vorgänge „alle Zwischendateien für Task 7 auflisten“ oder „Miniaturansicht für den Auftrag *Mymovie* aufrufen“ verwenden, ohne den Dateinamen oder den Speicherort in Ihrem Storage-Konto kennen zu müssen.

### Abrufen der Bibliothek

Sie können die Bibliothek, die neue Klassen enthält und die Klassen [CloudJob][net_cloudjob] und [CloudTask][net_cloudtask] um neue Methoden erweitert, über [NuGet][nuget_package] abrufen. Um die Bibliothek zu Ihrem Visual Studio-Projekt hinzuzufügen, können Sie den [NuGet Library Package Manager][nuget_manager] verwenden.

>[AZURE.TIP] Sie finden den [Quellcode][github_file_conventions] für die Azure Batch-Dateikonventionenbibliothek auf GitHub im „Microsoft Azure-SDK für .NET“-Repository.

## Voraussetzung: verknüpftes Speicherkonto

Um Ausgaben mithilfe der Dateikonventionenbibliothek in dauerhaftem Speicher beizubehalten und im Azure-Portal anzuzeigen, müssen Sie [ein Azure Storage-Konto](batch-application-packages.md#link-a-storage-account) mit Ihrem Batch-Konto verknüpfen. Wenn Sie dies nicht bereits getan haben, verknüpfen Sie ein Storage-Konto über das Azure-Portal mit Ihrem Batch-Konto:

Blatt **Batch-Konto** > **Einstellungen** > **Storage-Konto** > **Storage-Konto** (Keine) > Wählen Sie ein Storage-Konto in Ihrem Abonnement

Eine ausführliche Vorgehensweise zum Verknüpfen eines Storage-Kontos finden Sie unter [Anwendungsbereitstellung mit Azure Batch-Anwendungspaketen](batch-application-packages.md).

## Beibehalten der Ausgabe

Sie müssen zwei Hauptaufgaben ausführen, wenn Sie Auftrags- und Taskausgaben mit der Dateikonventionenbibliothek speichern: Erstellen des Speichercontainers und Speichern der Ausgabe im Container.

>[AZURE.WARNING] Da alle Auftrags- und Taskausgaben im gleichen Container gespeichert werden, können die [Speicherbeschränkungsgrenzwerte](../storage/storage-performance-checklist.md#blobs) erzwungen werden, wenn eine große Anzahl von Tasks gleichzeitig versucht, Dateien dauerhaft in den Speicher zu schreiben.

### Erstellen eines Speichercontainers

Bevor die Tasks damit beginnen können, Ausgaben dauerhaft in den Speicher zu schreiben, müssen Sie einen Blob Storage-Container erstellen, in den die Ausgaben hochgeladen werden. Rufen Sie dazu [CloudJob][net_cloudjob].[PrepareOutputStorageAsync][net_prepareoutputasync] auf. Diese Erweiterungsmethode verwendet ein [CloudStorageAccount][net_cloudstorageaccount]-Objekt als Parameter und erstellt einen Container, der so benannt ist, dass sein Inhalt vom Azure-Portal und von den später in diesem Artikel erläuterten Abrufmethoden gefunden werden kann.

In der Regel wird dieser Code in der Clientanwendung verwendet (also in der Anwendung, die Pools, Aufträge und Tasks erstellt).

```csharp
CloudJob job = batchClient.JobOperations.CreateJob(
	"myJob",
	new PoolInformation { PoolId = "myPool" });

// Create reference to the linked Azure Storage account
CloudStorageAccount linkedStorageAccount =
	new CloudStorageAccount(myCredentials, true);

// Create the blob storage container for the outputs
await job.PrepareOutputStorageAsync(linkedStorageAccount);
```

### Speichern von Taskausgaben

Nachdem Sie einen Blob Storage-Container vorbereitet haben, können Tasks Ausgaben in diesem Container speichern, indem sie die [TaskOutputStorage][net_taskoutputstorage]-Klasse aus der Dateikonventionenbibliothek verwenden.

Erstellen Sie in Ihrem Taskcode zunächst ein [TaskOutputStorage][net_taskoutputstorage]-Objekt. Wenn der Task abgeschlossen ist, rufen Sie die [TaskOutputStorage][net_taskoutputstorage].[SaveAsync][net_saveasync]-Methode auf, um die Ausgabe in Azure Storage zu speichern.

```csharp
CloudStorageAccount linkedStorageAccount = new CloudStorageAccount(myCredentials);
string jobId = Environment.GetEnvironmentVariable("AZ_BATCH_JOB_ID");
string taskId = Environment.GetEnvironmentVariable("AZ_BATCH_TASK_ID");

TaskOutputStorage taskOutputStorage = new TaskOutputStorage(
	linkedStorageAccount, jobId, taskId);

/* Code to process data and produce output file(s) */

await taskOutputStorage.SaveAsync(TaskOutputKind.TaskOutput, "frame_full_res.jpg");
await taskOutputStorage.SaveAsync(TaskOutputKind.TaskPreview, "frame_low_res.jpg");
```

Der Parameter „output kind“ kategorisiert persistent gespeicherte Dateien. Es gibt vier vordefinierte [TaskOutputKind][net_taskoutputkind]-Typen: „TaskOutput“, „TaskPreview“, „TaskLog“ und "TaskIntermediate". Sie können auch benutzerdefinierte Typen definieren, wenn diese für Ihren Workflow sinnvoll sind.

Mit diesen Ausgabetypen können Sie angeben, welche Typen von Ausgaben aufgelistet werden, wenn Sie die beibehaltenen Ausgaben eines bestimmten Tasks später in Batch abfragen. Beim Auflisten der Ausgaben für einen Task können Sie die Liste also nach einem der Ausgabetypen filtern. Beispiel: „*preview*-Ausgabe für Task *109* abrufen.“ Weitere Informationen zum Auflisten und Abrufen von Ausgaben finden Sie unter [Abrufen der Ausgabe](#retrieve-output) weiter unten in diesem Artikel.

>[AZURE.TIP] Die Art der Ausgabe bestimmt auch, wo eine bestimmte Datei im Azure-Portal angezeigt wird: Als *TaskOutput* kategorisierte Dateien werden unter „Taskausgabedateien“ angezeigt, als *TaskLog* kategorisierte Dateien unter „Taskprotokolle“.

### Speichern von Auftragsausgaben

Zusätzlich zum Speichern von Taskausgaben können Sie die Ausgaben speichern, die einem vollständigen Auftrag zugeordnet sind. Beispielsweise können Sie im Zusammenführungstask eines Filmrenderauftrags den vollständig gerenderten Film als Ausgabe des Auftrags beibehalten. Wenn der Auftrag abgeschlossen ist, kann Ihre Clientanwendung einfach die Ausgaben für den Auftrag auflisten und abrufen, sodass nicht die einzelnen Tasks abgefragt werden müssen.

Speichern Sie die Ausgabe des Auftrags durch Aufrufen der [JobOutputStorage][net_joboutputstorage].[SaveAsync][net_joboutputstorage_saveasync]-Methode, und geben Sie [JobOutputKind][net_joboutputkind] und Dateiname an:

```
CloudJob job = await batchClient.JobOperations.GetJobAsync(jobId);
JobOutputStorage jobOutputStorage = job.OutputStorage(linkedStorageAccount);

await jobOutputStorage.SaveAsync(JobOutputKind.JobOutput, "mymovie.mp4");
await jobOutputStorage.SaveAsync(JobOutputKind.JobPreview, "mymovie_preview.mp4");
```

Ähnlich wie bei TaskOutputKind für Taskausgaben verwenden Sie den Parameter [JobOutputKind][net_joboutputkind], um die beibehaltenen Dateien eines Auftrags zu kategorisieren. Mit diesem Parameter können Sie später einen bestimmten Ausgabetyp abfragen (oder auflisten). JobOutputKind umfasst sowohl Ausgabe- als auch Vorschautypen und unterstützt das Erstellen von benutzerdefinierten Typen.

### Speichern von Taskprotokollen

Zusätzlich zum Beibehalten einer Datei in dauerhaftem Speicher, nachdem ein Task oder ein Auftrag abgeschlossen ist, kann es notwendig sein, Dateien beizubehalten, die während der Ausführung eines Tasks aktualisiert werden – z.B. Protokolldateien oder `stdout.txt` und `stderr.txt`. Zu diesem Zweck stellt die Azure Batch-Dateikonventionenbibliothek die [TaskOutputStorage][net_taskoutputstorage].[SaveTrackedAsync][net_savetrackedasync]-Methode zur Verfügung. Mit [SaveTrackedAsync][net_savetrackedasync] können Sie Aktualisierungen einer Datei auf dem Knoten (in einem von Ihnen angegebenen Intervall) nachverfolgen und die Aktualisierungen in Azure Storage beibehalten.

Im folgenden Codeausschnitt wird [SaveTrackedAsync][net_savetrackedasync] verwendet, um `stdout.txt` während der Ausführung des Tasks alle 15 Sekunden in Azure Storage zu aktualisieren:

```csharp
TimeSpan stdoutFlushDelay = TimeSpan.FromSeconds(3);
string logFilePath = Path.Combine(
	Environment.GetEnvironmentVariable("AZ_BATCH_TASK_DIR"), "stdout.txt");

// The primary task logic is wrapped in a using statement that sends updates to
// the stdout.txt blob in Storage every 15 seconds while the task code runs.
using (ITrackedSaveOperation stdout =
		await taskStorage.SaveTrackedAsync(
		TaskOutputKind.TaskLog,
		logFilePath,
		"stdout.txt",
		TimeSpan.FromSeconds(15)))
{
	/* Code to process data and produce output file(s) */

	// We are tracking the disk file to save our standard output, but the
	// node agent may take up to 3 seconds to flush the stdout stream to
	// disk. So give the file a moment to catch up.
 	await Task.Delay(stdoutFlushDelay);
}
```

`Code to process data and produce output file(s)` ist ein Platzhalter für den Code, den der Task normalerweise ausführen würde. Unter Umständen verfügen Sie beispielsweise über Code, der Daten aus Azure Storage herunterlädt und eine Transformation oder Berechnung mit diesen Daten durchführt. Dieses Beispiel soll veranschaulichen, wie solcher Code in einem `using`-Block umschlossen werden kann, um eine Datei in regelmäßigen Abständen mit [SaveTrackedAsync][net_savetrackedasync] zu aktualisieren.

`Task.Delay` wird am Ende des `using`-Blocks benötigt, um sicherzustellen, dass der Knoten-Agent Zeit hat, um den Inhalt der Standardausgabe in die Datei „stdout.txt“ auf dem Knoten zu leeren. (Der Knoten-Agent ist ein Programm, das auf jedem Knoten im Pool ausgeführt wird, und stellt die Befehls- und Steuerungsschnittstelle zwischen dem Knoten und dem Batch-Dienst dar.) Ohne diese Verzögerung gehen unter Umständen die letzten Sekunden der Ausgabe verloren. Diese Verzögerung ist möglicherweise nicht für alle Dateien erforderlich.

>[AZURE.NOTE] Wenn Sie die Dateinachverfolgung mit SaveTrackedAsync aktivieren, werden nur *Anfügungen* der nachverfolgten Datei in Azure Storage beibehalten. Verwenden Sie diese Methode zum Nachverfolgen von nicht rotierenden Protokolldateien oder anderen Dateien, bei denen Daten angefügt (also beim Aktualisieren nur am Dateiende hinzugefügt) werden.

## Abrufen der Ausgabe

Wenn Sie die dauerhaft gespeicherte Ausgabe mithilfe der Dateikonventionenbibliothek von Azure Batch abrufen, wird dieser Vorgang task- oder auftragsbasiert ausgeführt. Sie können die Ausgabe für einen bestimmten Task oder Auftrag anfordern, ohne den Pfad in Blob Storage oder den Dateinamen kennen zu müssen. Sie können z.B. folgende Abfrage formulieren: „Ausgabedateien für Task*109* abrufen.“

Der folgende Codeausschnitt durchläuft alle Tasks eines Auftrags, gibt einige Informationen zu den Ausgabedateien für den Task aus und lädt dann die Dateien aus Storage herunter.

```csharp
foreach (CloudTask task in myJob.ListTasks())
{
    foreach (TaskOutputStorage output in
		task.OutputStorage(storageAccount).ListOutputs(
			TaskOutputKind.TaskOutput))
    {
        Console.WriteLine($"output file: {output.FilePath}");

		output.DownloadToFileAsync(
			$"{jobId}-{output.FilePath}",
			System.IO.FileMode.Create).Wait();
    }
}
```

## Taskausgaben und das Azure-Portal

Im Azure-Portal werden Taskausgaben und Protokolle angezeigt, die unter Verwendung der Namenskonventionen in der [Infodatei zu Azure Batch-Dateikonventionen][github_file_conventions_readme] dauerhaft in einem verknüpften Azure Storage-Konto gespeichert werden. Sie können diese Konventionen in Ihrer bevorzugten Sprache implementieren oder die Dateikonventionenbibliothek in Ihren .NET-Anwendungen verwenden.

### Aktivieren der Portal-Anzeige

Um die Anzeige Ihrer Ausgaben im Portal zu aktivieren, müssen die folgenden Voraussetzungen erfüllt sein:

 1. [Verknüpfen Sie ein Azure Storage-Konto](#requirement-linked-storage-account) mit Ihrem Batch-Konto.
 2. Halten Sie sich an die vordefinierten Namenskonventionen für Storage-Container und Dateien, wenn Sie Ausgaben dauerhaft speichern. Eine Definition dieser Konventionen finden Sie in der [Infodatei][github_file_conventions_readme] zur Dateikonventionenbibliothek. Bei Verwendung der [Azure Batch-Dateikonventionenbibliothek][nuget_package], um Ihre Ausgabe dauerhaft zu speichern, wird diese Anforderung erfüllt.

### Anzeigen von Ausgaben im Portal

Wenn Sie Taskausgaben und Protokolle im Azure-Portal anzeigen möchten, navigieren Sie zum Task, dessen Ausgabe angezeigt werden soll, und klicken Sie auf **Gespeicherte Ausgabedateien** oder auf **Gespeicherte Protokolle**. Diese Abbildung zeigt die **gespeicherten Ausgabedateien** für den Task mit der ID 007:

![Taskausgabeblatt im Azure-Portal][2]

## Codebeispiel

Das Beispielprojekt [PersistOutputs][github_persistoutputs] ist eines der [Azure Batch-Codebeispiele][github_samples] auf GitHub. Diese Visual Studio 2015-Lösung veranschaulicht, wie die Dateikonventionenbibliothek von Azure Batch verwendet werden kann, um die Taskausgabe in dauerhaftem Speicher beizubehalten. Gehen Sie folgendermaßen vor, um das Beispiel auszuführen:

1. Öffnen Sie das Projekt in **Visual Studio 2015**.
2. Fügen Sie die **Anmeldeinformationen** für Ihr Batch- und Storage-Konto zu **AccountSettings.settings** im Microsoft.Azure.Batch.Samples.Common-Projekt hinzu.
3. **Erstellen** Sie die Lösung (aber führen Sie sie nicht aus). Stellen Sie NuGet-Pakete wieder her, wenn Sie dazu aufgefordert werden.
4. Laden Sie im Azure-Portal ein [Anwendungspaket](batch-application-packages.md) für **PersistOutputsTask** hoch. Fügen Sie `PersistOutputsTask.exe` und die abhängigen Assemblys dem ZIP-Paket hinzu, und legen Sie die Anwendungs-ID auf „PersistOutputsTask“ und die Version des Anwendungspakets auf „1.0“ fest.
5. **Starten** Sie das **PersistOutputs**-Projekt (d.h. führen Sie es aus).

## Nächste Schritte

### Anwendungsbereitstellung

Das Batch-Feature [Anwendungspakete](batch-application-packages.md) bietet eine einfache Möglichkeit zum Bereitstellen und Versionieren der Anwendungen, die von Ihren Tasks auf Computeknoten ausgeführt werden.

### Installieren von Anwendungen und Bereitstellen von Daten (Staging)

Der Beitrag [Installing applications and staging data on Batch compute nodes][forum_post] \(Installieren von Anwendungen und Bereitstellen von Daten auf Batch-Computeknoten) im Azure Batch-Forum bietet einen Überblick über die verschiedenen Methoden, mit denen Knoten für die Ausführung von Tasks vorbereitet werden können. Der Beitrag wurde von einem Mitglied des Azure Batch-Teams verfasst und ist eine gute Möglichkeit, um sich mit den verschiedenen Vorgehensweisen vertraut zu machen, mit denen sich Dateien (einschließlich Anwendungen und Aufgabeneingabedaten) auf Computeknoten bereitstellen lassen. Darüber hinaus enthält der Beitrag einige Besonderheiten, die bei den einzelnen Methoden zu berücksichtigen sind.

[forum_post]: https://social.msdn.microsoft.com/Forums/de-DE/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[github_file_conventions]: https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Batch/FileConventions
[github_file_conventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[github_persistoutputs]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/PersistOutputs
[github_samples]: https://github.com/Azure/azure-batch-samples
[net_batchclient]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_cloudstorageaccount]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.cloudstorageaccount.aspx
[net_cloudtask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx
[net_fileconventions_readme]: https://github.com/Azure/azure-sdk-for-net/blob/AutoRest/src/Batch/FileConventions/README.md
[net_joboutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputkind.aspx
[net_joboutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.aspx
[net_joboutputstorage_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.joboutputstorage.saveasync.aspx
[net_msdn]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_prepareoutputasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.cloudjobextensions.prepareoutputstorageasync.aspx
[net_saveasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.saveasync.aspx
[net_savetrackedasync]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.savetrackedasync.aspx
[net_taskoutputkind]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputkind.aspx
[net_taskoutputstorage]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.conventions.files.taskoutputstorage.aspx
[nuget_manager]: https://docs.nuget.org/consume/installing-nuget
[nuget_package]: https://www.nuget.org/packages/Microsoft.Azure.Batch.Conventions.Files
[portal]: https://portal.azure.com
[storage_explorer]: http://storageexplorer.com/

[1]: ./media/batch-task-output/task-output-01.png "Auswahl von „Gespeicherte Ausgabedateien“ und „Gespeicherte Protokolle“ im Portal"
[2]: ./media/batch-task-output/task-output-02.png "Taskausgabeblatt im Azure-Portal"

<!---HONumber=AcomDC_0907_2016-->