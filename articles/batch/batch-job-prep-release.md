<properties
	pageTitle="Auftragsvorbereitung und -bereinigung in Batch | Microsoft Azure"
	description="Verwenden Sie Vorbereitungs- und Freigabeaufgaben auf Auftragsebene, um Datenübertragungen auf Azure Batch-Computeknoten zu minimieren und Knoten nach Abschluss des Auftrags zu bereinigen."
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
	ms.date="06/22/2016"
	ms.author="marsma" />

# Ausführen von Auftragsvorbereitungs- und Auftragsabschlussaufgaben auf Azure Batch-Computeknoten

Azure Batch-Aufträge erfordern sowohl vor ihrer Ausführung häufig eine Einrichtung als auch nach Abschluss der Aufgaben des Auftrags eine Art Wartung. Batch stellt die Mechanismen für diese Vorbereitung und Wartung in Form von optionalen Aufgaben zur Auftragsvorbereitung und Auftragsfreigabe bereit.

Bevor eine Aufgabe eines Auftrags ausgeführt wird, wird die **Auftragsvorbereitungsaufgabe** auf allen Serverknoten ausgeführt, die für die Ausführung von Aufgaben eingeplant sind. Nach Abschluss des Auftrags wird die **Auftragsfreigabeaufgabe** auf jedem Knoten im Pool ausgeführt, der mindestens eine Aufgabe ausgeführt hat. Wie bei normalen Batch-Aufgaben können Sie eine Befehlszeile für Aufgaben zur Auftragsvorbereitung oder -freigabe angeben, die zur Ausführung dieser Aufgaben aufgerufen wird. Diese besonderen Aufgaben bieten andere vertraute Aufgabenfeatures wie das Herunterladen von Dateien, Ausführen mit höheren Rechten, benutzerdefinierte Umgebungsvariablen, maximale Ausführungsdauer, Anzahl der Wiederholungsversuche und Dateiaufbewahrungsdauer.

In den folgenden Abschnitten wird erläutert, wie Sie diese zwei speziellen Aufgabentypen mithilfe der Klassen „[JobPreparationTask][net_job_prep]” und „[JobReleaseTask][net_job_release]” in der [Batch .NET][api_net]-API verwenden.

> [AZURE.TIP] Aufgaben zur Auftragsvorbereitung und -freigabe sind insbesondere in Umgebungen mit einem gemeinsam genutzten Pool hilfreich – also in Umgebungen, in denen ein Pool von Computeknoten zwischen Auftragsausführungen beibehalten und von vielen verschiedenen Aufträgen gemeinsam genutzt wird.

## Verwendungsmöglichkeiten für Auftragsvorbereitungs- und Auftragsfreigabeaufgaben

Die Verwendung von Aufgaben zur Auftragsvorbereitung und -freigabe empfiehlt sich immer dann, wenn Sie Knoten mit auftragsspezifischer Konfiguration oder auftragsspezifischen Daten vorbereiten (und Auftragsergebnisdaten bereinigen oder beibehalten) müssen. Dies kann beispielsweise in folgenden Situationen der Fall sein:

**Übertragung von allgemeinen Aufgabendaten**

Batchaufträge erfordern oft einen gemeinsamen Satz von Daten als Eingabe für die Aufgaben des Auftrags. Bei täglich durchgeführten Berechnungen zur Risikoanalyse sind Marktdaten z. B. auftragsspezifisch, gelten aber trotzdem für alle Aufgaben im Auftrag. Diese Marktdaten, die oft mehrere GB groß sind, sollten auf jeden Computeknoten nur einmal heruntergeladen werden, sodass sie von jeder auf einem Knoten ausgeführten Aufgabe verwendet werden können. Verwenden Sie eine **Auftragsvorbereitungsaufgabe**, um die Daten vor der Ausführung anderer Aufgaben im Auftrag auf jeden Knoten herunterzuladen.

**Löschen von Auftragsdaten**

In einer Umgebung mit einem gemeinsam genutzten Pool, in der die Computeknoten eines Pools zwischen Aufträgen nicht außer Betrieb gesetzt werden, müssen zwischen Ausführungen unter Umständen Auftragsdaten gelöscht werden, um Speicherplatz auf den Knoten zu sparen oder ggf. Sicherheitsrichtlinien Ihrer Organisation zu erfüllen. Verwenden Sie eine **Auftragsfreigabeaufgabe**, um Daten zu löschen, die von einer Auftragsvorbereitungsaufgabe heruntergeladen oder während der Aufgabenausführung generiert wurden.

**Protokollaufbewahrung**

Eventuell wollen Sie eine Kopie der von den Aufgaben generierten Protokolle oder die von fehlerhaften Anwendungen generierten Absturzabbilddateien aufbewahren. Verwenden Sie in diesen Fällen eine **Auftragsfreigabeaufgabe**, um die Daten zu komprimieren und in ein [Azure-Speicherkonto][azure_storage] hochzuladen.

## Auftragsvorbereitungsaufgabe

Vor der Ausführung der Aufgaben eines Auftrags wird die Auftragsvorbereitungsaufgabe auf jedem Computeknoten, der zum Ausführen einer Aufgabe eingeplant ist, ausgeführt. Standardmäßig wartet der Batch-Dienst bis die Auftragsvorbereitungsaufgabe fertig ist, bevor er die geplanten Aufgaben auf dem Knoten ausführt. Sie können den Dienst allerdings auch dahingehend konfigurieren, dass er nicht wartet. Die Auftragsvorbereitungsaufgabe wird erneut auf einem Computeknoten ausgeführt, wenn dieser neu gestartet wird, Sie können jedoch auch dieses Verhalten deaktivieren.

Die Auftragsvorbereitungsaufgabe wird nur auf Knoten ausgeführt, die zum Ausführen einer Aufgabe eingeplant sind. Das verhindert die unnötige Ausführung von Auftragsvorbereitungsaufgaben an Knoten, denen keine Aufgaben zugewiesen sind. Dies kann eintreten, wenn die Anzahl der Aufgaben für einen Auftrag geringer ist, als die Anzahl der Knoten in einem Pool. Dies gilt auch, wenn die [gleichzeitige Aufgabenausführung](batch-parallel-node-tasks.md) aktiviert ist. Einige Knoten bleiben unbeschäftigt, wenn die Gesamtanzahl an Aufgaben geringer ist, als die Gesamtzahl der möglichen zeitgleich ausgeführten Aufgaben. Indem Sie die Auftragsvorbereitungsaufgabe nicht auf Knoten im Leerlauf ausführen, können Sie bei den Gebühren für Datenübertragungen sparen.

> [AZURE.NOTE] [JobPreparationTask][net\_job\_prep\_cloudjob] unterscheidet sich von der [CloudPool.StartTask][pool_starttask] insofern, dass „JobPreparationTask“ beim Start jedes Auftrags ausgeführt wird, während „StartTask“ nur ausgeführt wird, wenn ein Computeknoten einem Pool erstmals hinzugefügt oder neu gestartet wird.

## Auftragsfreigabeaufgabe

Nachdem ein Auftrag als abgeschlossen markiert wurde, wird die Auftragsfreigabeaufgabe auf jedem Knoten im Pool ausgeführt, der mindestens eine Aufgabe ausgeführt hat. Um einen Auftrag als abgeschlossen zu markieren, geben Sie eine Terminate-Anforderung aus. Der Batch-Dienst legt anschließend den Status des Auftrags auf *Wird beendet* fest, beendet alle aktiven bzw. ausgeführten Aufgaben im Zusammenhang mit dem Auftrag und führt die Auftragsfreigabeaufgabe aus. Danach wird der Status des Auftrags in *Abgeschlossen* geändert.

> [AZURE.NOTE] Beim Löschen eines Auftrags wird die Auftragsfreigabeaufgabe ebenfalls ausgeführt. Wurde ein Auftrag jedoch bereits zuvor beendet, wird die Freigabeaufgabe kein zweites Mal ausgeführt, wenn dieser Auftrag anschließend gelöscht wird.

## Aufgaben zur Auftragsvorbereitung und -freigabe mit Batch .NET

Sie erstellen und konfigurieren ein [JobPreparationTask][net_job_prep]-Objekt, um eine Auftragsvorbereitungsaufgabe zu verwenden, und weisen es der [CloudJob.JobPreparationTask][net_job_prep_cloudjob]-Eigenschaft Ihres Auftrags zu. Auf ähnliche Weise initialisieren Sie die [JobReleaseTask][net_job_release], die Sie der [CloudJob.JobReleaseTask][net_job_prep_cloudjob]-Eigenschaft zuweisen, um die Freigabeaufgabe des Auftrags festzulegen.

In diesem Codeausschnitt ist `myBatchClient` eine vollständig initialisierte Instanz von [BatchClient][net_batch_client], und `myPool` ist ein vorhandener Pool im Batch-Konto.

		// Create the CloudJob for CloudPool "myPool"
		CloudJob myJob = myBatchClient.JobOperations.CreateJob("JobPrepReleaseSampleJob",
															   new PoolInformation() { PoolId = "myPool" });

		// Specify the command lines for the job preparation and release tasks
		string jobPrepCmdLine = "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
		string jobReleaseCmdLine = "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

		// Assign the job preparation task to the job
		myJob.JobPreparationTask = new JobPreparationTask { CommandLine = jobPrepCmdLine };

		// Assign the job release task to the job
		myJob.JobReleaseTask = new JobPreparationTask { CommandLine = jobReleaseCmdLine };

		await myJob.CommitAsync();

Wie oben erwähnt, wird die Freigabeaufgabe ausgeführt, wenn ein Auftrag beendet oder gelöscht wird. Ein Auftrag mit der Batch .NET-API wird beendet, indem [JobOperations.TerminateJobAsync][net_job_terminate] aufgerufen wird. Ein Auftrag wird mit [JobOperations.DeleteJobAsync][net_job_delete] gelöscht. Diese beiden Aktionen werden typischerweise durchgeführt, wenn die Aufgaben eines Auftrag abgeschlossen sind oder ein von Ihnen definiertes Zeitlimit erreicht wurde.

		// Terminate the job to mark it as Completed; this will initiate the Job Release Task on any node
		// that executed job tasks. Note that the Job Release Task is also executed when a job is deleted,
		// thus you need not call Terminate if you typically delete your jobs upon task completion.
		await myBatchClient.JobOperations.TerminateJobAsync("JobPrepReleaseSampleJob");

## Codebeispiel auf GitHub

Sehen Sie sich das Beispielprojekt [JobPrepRelease][job_prep_release_sample] auf GitHub an, um Auftragsvorbereitungs- und Auftragsfreigabeaufgaben in Aktion zu erleben. Diese Konsolenanwendung führt folgende Schritte aus:

1. Sie erstellt einen Pool mit zwei „kleinen“ Knoten.
2. Sie erstellt einen Auftrag mit Auftragsvorbereitungs-, Auftragsfreigabe- und Standardaufgaben.
3. Sie führt die Auftragsvorbereitungsaufgabe aus, die zunächst die Knoten-ID in eine Textdatei im „freigegebenen“ Verzeichnis des Knotens schreibt.
4. Sie führt auf jedem Knoten eine Aufgabe aus, welche die Aufgaben-ID in dieselbe Textdatei schreibt.
5. Nach dem Abschluss aller Aufgaben (oder Erreichen des Timeouts) gibt sie den Inhalt der Textdatei jedes Knotens an die Konsole aus.
6. Sie führt die Auftragsfreigabeaufgabe aus, um die Datei aus dem Knoten zu löschen, wenn der Auftrag abgeschlossen ist.
6. Sie gibt die Exitcodes der Auftragsvorbereitungs- und Auftragsfreigabeaufgaben für jeden Knoten aus, auf dem diese ausgeführt wurden.
7. Sie hält die Ausführung an, um auf die Löschbestätigung für den Auftrag und/oder den Pool zu warten.

Die Ausgabe der Beispielanwendung sieht in etwa wie folgt aus:

```
Attempting to create pool: JobPrepReleaseSamplePool
Created pool JobPrepReleaseSamplePool with 2 small nodes
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-2434664350_1-20160623t173951z:
-------------------------------------------
tvm-2434664350_1-20160623t173951z tasks:
  task001
  task004
  task005
  task006

Contents of shared\job_prep_and_release.txt on tvm-2434664350_2-20160623t173951z:
-------------------------------------------
tvm-2434664350_2-20160623t173951z tasks:
  task008
  task002
  task003
  task007

Waiting for job JobPrepReleaseSampleJob to reach state Completed
...

tvm-2434664350_1-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-2434664350_2-20160623t173951z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
yes

Sample complete, hit ENTER to exit...
```

>[AZURE.NOTE] Die tatsächliche Ausgabe kann sich aufgrund der variablen Erstellungs- und Startzeit von Knoten in einem neuen Pool unterscheiden. (Einige Knoten sind schneller für Aufgaben bereit als andere.) Aufgrund der schnellen Ausführung der Aufgaben werden unter Umständen alle Aufgaben des Auftrags von einem einzelnen Knoten des Pools ausgeführt. In diesem Fall werden Sie feststellen, dass die Aufgaben zur Auftragsvorbereitung und -freigabe für den Knoten, die keine Aufgaben ausgeführt hat, nicht vorhanden sind.

### Überprüfen von Aufgaben zur Auftragsvorbereitung und -freigabe im Azure-Portal

Wenn Sie die obige Beispielanwendung ausführen, können Sie die Eigenschaften des Auftrags und dessen Aufgaben im [Azure-Portal][portal] anzeigen und sogar die durch die Aufgaben des Auftrags geänderte, freigegebene Textdatei herunterladen.

Der folgende Screenshot zeigt das Blatt **Preparation tasks** (Vorbereitungsaufgaben) im Azure-Portal nach der Ausführung der Beispielanwendung. Navigieren Sie nach Abschluss der Aufgaben (aber noch vor dem Löschen des Auftrags und Pools) zu den Eigenschaften für *JobPrepReleaseSampleJob*, und klicken Sie auf **Preparation tasks** (Vorbereitungsaufgaben) oder **Release tasks** (Freigabeaufgaben), um deren Eigenschaften anzeigen.

![Aufgabenvorbereitungseigenschaften im Azure-Portal][1]

## Nächste Schritte

### Anwendungspakete

Neben der Aufgabe zur Auftragsvorbereitung können Sie auch das Batch-Feature [Anwendungspakete](batch-application-packages.md) verwenden, um Compute-Knoten für die Aufgabenausführung vorzubereiten. Dieses Feature ist besonders bei der Bereitstellung von Anwendungen hilfreich, für die kein Installationsprogramm ausgeführt werden muss, sowie bei Anwendungen, die mehr als 100 Dateien umfassen oder eine strenge Versionskontrolle erfordern.

### Installieren von Anwendungen und Bereitstellen von Daten

Der Beitrag [Installing applications and staging data on Batch compute nodes][forum_post] (Installieren von Anwendungen und Bereitstellen von Daten auf Batch-Computeknoten) im Azure Batch-Forum bietet eine Übersicht über die verschiedenen Methoden, mit denen Knoten auf die Ausführung von Aufgaben vorbereitet werden können. Der Beitrag wurde von einem Mitglied des Azure Batch-Teams verfasst und ist eine gute Möglichkeit, um sich mit den verschiedenen Vorgehensweisen vertraut zu machen, mit denen sich Dateien (einschließlich Anwendungen und Aufgabeneingabedaten) auf Computeknoten bereitstellen lassen. Darüber hinaus enthält der Beitrag einige Besonderheiten, die bei den einzelnen Methoden zu berücksichtigen sind.

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[portal]: https://portal.azure.com
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[forum_post]: https://social.msdn.microsoft.com/Forums/de-DE/87b19671-1bdf-427a-972c-2af7e5ba82d9/installing-applications-and-staging-data-on-batch-compute-nodes?forum=azurebatch
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net\_job\_prep\_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[1]: ./media/batch-job-prep-release/portal-jobprep-01.png

<!---HONumber=AcomDC_0629_2016-->