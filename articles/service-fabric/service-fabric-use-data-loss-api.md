<properties
   pageTitle="Verursachen von Datenverlust in Service Fabric-Diensten | Microsoft Azure"
   description="Beschreibt die Verwendung der API zum Verursachen von Datenverlust."
   services="service-fabric"
   documentationCenter=".net"
   authors="LMWF"
   manager="rsinha"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/19/2016"
   ms.author="lemai"/>
   
# Verursachen von Datenverlust in Diensten

>[AZURE.WARNING] In diesem Dokument wird beschrieben, wie Sie in Ihren Diensten Datenverluste verursachen können, weshalb es mit Umsicht genutzt werden muss.

## Einführung
Sie führen einen Datenverlust in einer Partition Ihres Service Fabric-Diensts herbei, indem Sie „StartPartitionDataLossAsync()“ aufrufen. Diese API verwendet den Fault Injection und Analysis Service zum Herbeiführen von Bedingungen für einen Datenverlust.

## Verwenden des Fault Injection and Analysis Service

Der Fault Injection and Analysis Service unterstützt derzeit die folgenden APIs im nachstehenden Diagramm. Rechts im Diagramm wird das entsprechende PowerShell-Cmdlet gezeigt. In der MSDN-Dokumentation finden Sie weitere Informationen zu den einzelnen APIs.

| C#-API | PowerShell-Cmdlet |
|-------------------------------------|-----------------------------------------------:|
|[StartPartitionDataLossAsync][dl] |[Start-ServiceFabricPartitionDataLoss][psdl] |
|[StartPartitionQuorumLossAsync][ql] |[Start-ServiceFabricPartitionQuorumLoss][psql] |
|[StartPartitionRestartAsync][rp] |[Start-ServiceFabricPartitionRestart][psrp] |

## Konzeptionelle Übersicht über das Ausführen eines Befehls

Der Fault Injection and Analysis Service nutzt ein asynchrones Modell, bei dem Sie den Befehl mit einer API starten, die in diesem Dokument als „Start“-API bezeichnet wird. Anschließend prüft dieser Dienst den Fortschritt dieses Befehls mithilfe einer „GetProgress“-API, bis er einen Endzustand erreicht oder Sie ihn abbrechen. Um einen Befehl zu starten, rufen Sie die „Start“-API der entsprechenden API auf. Diese API liefert eine Rückgabe, wenn der Fault Injection und Analysis Service die Anforderung akzeptiert hat. Sie gibt jedoch nicht an, wie weit der Befehl ausgeführt oder ob er überhaupt gestartet wurde. Um den Status eines Befehls zu überprüfen, rufen Sie die „GetProgress“-API auf, die der zuvor aufgerufenen „Start“-API entspricht. Die „GetProgress“-API gibt ein Objekt zurück, das den aktuellen Status des Befehls in seiner „State“-Eigenschaft angibt. Ein Befehl wird solange ausgeführt, bis:

1.	er erfolgreich abgeschlossen wurde. Wenn Sie in diesem Fall „GetProgress“ dafür aufrufen, ist die „State“-Eigenschaft des Fortschrittsobjekts „Completed“.
2.	ein schwerwiegender Fehler auftritt. Wenn Sie in diesem Fall „GetProgress“ dafür aufrufen, ist die „State“-Eigenschaft des Fortschrittsobjekts „Faulted“.
3.	Sie können den Befehl mit der [CancelTestCommandAsync][cancel]-API oder dem PowerShell-Cmdlet [Stop-ServiceFabricTestCommand][cancelps] abbrechen. Wenn Sie in diesem Fall „GetProgress“ dafür aufrufen, ist die „State“-Eigenschaft des Fortschrittsobjekts entweder „Cancelled“ oder „ForceCancelled“, was von einem Argument für diese API abhängt. Weitere Details finden Sie in der Dokumentation zu [CancelTestCommandAsync][cancel].


## Details der Ausführung eines Befehls

Um einen Befehl zu starten, rufen Sie die Start-API mit den erwarteten Argumenten auf. Alle Start-APIs haben ein „Guid“-Argument mit dem Namen „operationId“. Sie sollten das „operationId“-Argument nachverfolgen, da es zum Verfolgen des Fortschritts dieses Befehls verwendet wird. Es muss an die „GetProgress“-API übergeben werden, damit der Fortschritt des Befehls nachverfolgt werden kann. „operationId“ muss eindeutig sein.

Nach einem erfolgreichen Aufruf der Start-API muss die „GetProgress“-API in einer Schleife aufgerufen werden, bis die zurückgegebene „State“-Eigenschaft des Fortschrittsobjekts „Completed“ lautet. Alle [FabricTransientExceptions][fte] und OperationCanceledExceptions müssen wiederholt werden. Wenn der Befehl einen Endzustand (Completed, Faulted oder Cancelled) erreicht hat, weist die zurückgegebene „Result“-Eigenschaft des Fortschrittsobjekt weitere Informationen auf. Wenn der Status „Completed“ ist, enthält „Result.SelectedPartition.PartitionId“ die ausgewählte Partitions-ID. „Result.Exception“ ist NULL. Wenn der Status „Faulted“ ist, enthält „Result.Exception“ den Grund, warum der Fault Injection and Analysis Service den Befehl mit diesem Status versehen hat. „Result.SelectedPartition.PartitionId“ enthält die ausgewählte Partitions-ID. In einigen Fällen wurde der Befehl ggf. nicht weit genug ausgeführt, um eine Partition auszuwählen. In diesem Fall ist „PartitionId“ gleich 0. Wenn der Status „Cancelled“ ist, ist „Result.Exception“ NULL. Wie im Fall von „Faulted“ enthält „Result.SelectedPartition.PartitionId“ die gewählte Partitions-ID. Doch wenn der Befehl ggf. nicht weit genug ausgeführt wurde, ist diese 0. Sehen Sie sich auch das nachstehende Beispiel an.

Der folgende Beispielcode zeigt, wie ein Befehl gestartet und anschließend überprüft wird, um den Datenverlust auf einer bestimmten Partition zu verursachen.

```csharp
    static async Task PerformDataLossSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/MyService");

        // The id of the target partition inside the target service
        Guid targetPartitionId = new Guid("00000000-0000-0000-0000-000002233445");

        PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(targetServiceName, targetPartitionId);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.        

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                // In a terminal state .Result.SelectedPartition.PartitionId will have the chosen partition
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);
                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}'", operationId, progress.Result.Exception);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(5.0d)).ConfigureAwait(false);
        }
    }
```

Das folgende Beispiel zeigt, wie mithilfe von „PartitionSelector“ nach dem Zufallsprinzip eine Partition eines angegebenen Diensts gewählt wird:

```csharp
    static async Task PerformDataLossUseSelectorSample()
    {
        // Create a unique operation id for the command below
        Guid operationId = Guid.NewGuid();

        // Note: Use the appropriate overload for your configuration
        FabricClient fabricClient = new FabricClient();

        // The name of the target service
        Uri targetServiceName = new Uri("fabric:/SampleService ");

        // Use a PartitionSelector that will have the Fault Injection and Analysis Service choose a random partition of “targetServiceName”
        PartitionSelector partitionSelector = PartitionSelector.RandomOf(targetServiceName);

        // Start the command.  Retry OperationCanceledException and all FabricTransientException's.  Note when StartPartitionDataLossAsync completes
        // successfully it only means the Fault Injection and Analysis Service has saved the intent to perform this work.  It does not say anything about the progress
        // of the command.
        while (true)
        {
            try
            {
                await fabricClient.TestManager.StartPartitionDataLossAsync(operationId, partitionSelector, DataLossMode.FullDataLoss).ConfigureAwait(false);
                break;
            }
            catch (OperationCanceledException)
            {
            }
            catch (FabricTransientException)
            {
            }
            catch (Exception e)
            {
                Console.WriteLine("Unexpected exception '{0}'", e);
                throw;
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }

        PartitionDataLossProgress progress = null;

        // Poll the progress using GetPartitionDataLossProgressAsync until it is either Completed or Faulted.  In this example, we're assuming
        // the command won't be cancelled.

        while (true)
        {
            try
            {
                progress = await fabricClient.TestManager.GetPartitionDataLossProgressAsync(operationId).ConfigureAwait(false);
            }
            catch (OperationCanceledException)
            {
                continue;
            }
            catch (FabricTransientException)
            {
                continue;
            }

            if (progress.State == TestCommandProgressState.Completed)
            {
                Console.WriteLine("Command '{0}' completed successfully", operationId);

                Console.WriteLine("Printing progress.Result:");
                Console.WriteLine("  Printing selected partition='{0}'", progress.Result.SelectedPartition.PartitionId);

                break;
            }
            else if (progress.State == TestCommandProgressState.Faulted)
            {
                // If State is Faulted, the progress object's Result property's Exception property will have the reason why.
                Console.WriteLine("Command '{0}' failed with '{1}', SelectedPartition {2}", operationId, progress.Result.Exception, progress.Result.SelectedPartition);
                break;
            }
            else
            {
                Console.WriteLine("Command '{0}' is currently Running", operationId);
            }

            await Task.Delay(TimeSpan.FromSeconds(1.0d)).ConfigureAwait(false);
        }
    }
```

## Verlauf und Abschneiden

Nachdem ein Befehl einen Endzustand erreicht hat, verbleiben seine Metadaten für eine gewisse Zeit im Fault Injection and Analysis Service, ehe sie zum Freigeben von Speicherplatz entfernt werden. Wenn „GetProgress“ mit der „operationId“ eines Befehls aufgerufen wird, nachdem dieser entfernt wurde, wird eine FabricException mit dem Fehlercode „KeyNotFound“ zurückgegeben.

[dl]: https://msdn.microsoft.com/library/azure/mt693569.aspx
[ql]: https://msdn.microsoft.com/library/azure/mt693558.aspx
[rp]: https://msdn.microsoft.com/library/azure/mt645056.aspx
[psdl]: https://msdn.microsoft.com/library/mt697573.aspx
[psql]: https://msdn.microsoft.com/library/mt697557.aspx
[psrp]: https://msdn.microsoft.com/library/mt697560.aspx
[cancel]: https://msdn.microsoft.com/library/azure/mt668910.aspx
[cancelps]: https://msdn.microsoft.com/library/mt697566.aspx
[fte]: https://msdn.microsoft.com/library/azure/system.fabric.fabrictransientexception.aspx

<!---HONumber=AcomDC_0921_2016-->