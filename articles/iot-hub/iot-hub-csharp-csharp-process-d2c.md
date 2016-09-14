<properties
	pageTitle="Verarbeiten von D2C-Nachrichten mit IoT Hub | Microsoft Azure"
	description="In diesem Tutorial werden nützliche Verfahren zum Verarbeiten von Gerät-zu-Cloud-Nachrichten (Device-to-Cloud, D2C) mit IoT Hub beschrieben."
	services="iot-hub"
	documentationCenter=".net"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="07/19/2016"
     ms.author="dobett"/>

# Tutorial: Verarbeiten von D2C-Nachrichten mit IoT Hub

## Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Anwendungs-Back-End ermöglicht. Weitere Tutorials ([Erste Schritte mit IoT Hub] und [Senden von C2D-Nachrichten mit IoT Hub][lnk-c2d]) veranschaulichen, wie Sie die grundlegenden Device-to-Cloud- und Cloud-to-Device-Messagingfunktionen von IoT Hub verwenden.

Dieses Tutotrial baut auf dem Code des Tutotrials [Erste Schritte mit IoT Hub] auf und veranschaulicht zwei skalierbare Muster, die Sie zum Verarbeiten von D2C-Nachrichten verwenden können:

- Die zuverlässige Speicherung von Nachrichten von einem Gerät an die Cloud in [Azur Blob Storage]. Ein sehr häufiges Szenario ist die *Cold Path*-Analyse, in der Sie Telemetriedaten in Blobs speichern, um sie als Eingabe für Analyseprozesse zu verwenden. Diese Prozesse können von Tools wie z.B. [Azure Data Factory] oder dem [HDInsight (Hadoop)]-Stapel gesteuert werden.

- Zuverlässige Verarbeitung von *interaktiven* D2C-Nachrichten: D2C-Nachrichten sind interaktiv, wenn es sich um unmittelbare Auslöser für eine Reihe von Aktionen im Anwendungs-Back-End handelt. Beispielsweise könnte ein Gerät eine Alarmnachricht senden, die das Einfügen eines Tickets in ein CRM-System auslöst. Im Gegensatz dazu werden *Datenpunktnachrichten* einfach in ein Analysemodul eingegeben. Temperaturtelemetriedaten von einem Gerät, die zur späteren Analyse gespeichert werden, sind beispielsweise Datenpunktnachrichten.

Da in IoT Hub ein [Event Hubs][lnk-event-hubs]-kompatibler Endpunkt zum Empfangen von D2C-Nachrichten verfügbar gemacht wird, wird in diesem Tutorial eine [EventProcessorHost]-Instanz verwendet. Diese Instanz:

* Zuverlässiges Speichern von *Datenpunkt*-Nachrichten in Azure-Blobspeicher.
* Leitet *interaktive* D2C-Nachrichten zur sofortigen Verarbeitung an eine Azure [Service Bus-Warteschlange] weiter.

Service Bus stellt die zuverlässige Verarbeitung von interaktiven Nachrichten sicher, da Checkpoints pro Nachricht und die Deduplizierung auf Grundlage von Zeitfenstern bereitgestellt werden.

> [AZURE.NOTE] Eine **EventProcessorHost**-Instanz ist nur eine Möglichkeit, interaktive Nachrichten zu verarbeiten. Weitere Optionen sind [Azure Service Fabric][lnk-service-fabric] und [Azure Stream Analytics][lnk-stream-analytics].

Am Ende dieses Tutorials führen Sie drei Windows-Konsolenapps aus:

* **SimulatedDevice**, eine geänderte Version der im Tutorial [Erste Schritte mit IoT Hub] erstellten App, die jede Sekunde Datenpunkt-D2C-Nachrichten und alle 10 Sekunden interaktive D2C-Nachrichten sendet. Für diese App wird das AMQPS-Protokoll für die Kommunikation mit IoT Hub verwendet.
* **ProcessDeviceToCloudMessages** verwendet die [EventProcessorHost]-Klasse zum Abrufen von Nachrichten aus dem Event Hubs-kompatiblen Endpunkt. Dann speichert sie zuverlässig Datenpunktnachrichten in Azure-Blobspeicher und leitet interaktive Nachrichten an eine Service Bus-Warteschlange weiter.
* **ProcessD2CInteractiveMessages** entfernt die interaktiven Nachrichten aus der Service Bus-Warteschlange.

> [AZURE.NOTE] IoT Hub verfügt über SDK-Unterstützung für zahlreiche Geräteplattformen und Sprachen, z.B. C, Java und JavaScript. Schritt-für-Schritt-Anleitungen zum Ersetzen des simulierten Geräts in diesem Tutorial durch ein physisches Gerät und allgemeine Informationen zum Verbinden von Geräten mit einem IoT Hub finden Sie im [Azure IoT Developer Center].

Dieses Tutorial kann direkt auf andere Möglichkeiten zum Verarbeiten von Event Hubs-kompatiblen Nachrichten übertragen werden, z.B. Projekte vom Typ [HDInsight (Hadoop)]. Weitere Informationen finden Sie unter [Entwicklungsleitfaden für Azure IoT Hub – Gerät an Cloud].

Für dieses Tutorial benötigen Sie Folgendes:

+ Microsoft Visual Studio 2015.

+ Ein aktives Azure-Konto. <br/>Falls Sie über kein Azure-Abonnement verfügen, können Sie in wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

Sie sollten über grundlegende Kenntnisse in Bezug auf [Azure Storage] und [Azure Service Bus] verfügen.


## Senden interaktiver Nachrichten von einem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierte Geräteanwendung, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, sodass sie interaktive D2C-Nachrichten an IoT Hub sendet.

1. Fügen Sie in Visual Studio im **SimulatedDevice**-Projekt der **Program**-Klasse folgende Methode hinzu.

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    Diese Methode ähnelt der **SendDeviceToCloudMessagesAsync**-Methode im Projekt **SimulatedDevice**. Der einzige Unterschied besteht darin, dass Sie jetzt die Systemeigenschaft **MessageId** und eine Benutzereigenschaft **messageType** festlegen. Der Code weist der Eigenschaft **MessageId** einen global eindeutigen Bezeichner (GUID) zu. Der Service Bus kann damit Nachrichten, die er empfängt, deduplizieren. Im Beispiel wird die Eigenschaft **messageType** verwendet, um interaktive Nachrichten von Datenpunktnachrichten zu unterscheiden. Die Anwendung übergibt diese Informationen in Nachrichteneigenschaften anstatt im Nachrichtentext. Auf diese Weise muss der Ereignisprozessor zum Nachrichtenrouting nicht die gesamte Nachricht deserialisieren.

    > [AZURE.NOTE] Es ist wichtig, die **MessageId** zur Deduplizierung interaktiver Nachrichten im Gerätecode zu erstellen. Zeitweise Netzwerkkommunikation oder andere Fehler könnten dazu führen, dass mehrere Neuübertragungen der gleichen Nachricht von diesem Gerät erfolgen. Sie können anstelle einer GUID auch eine semantische Nachrichten-ID verwenden, z.B. einen Hash der relevanten Nachrichtendatenfelder.

2. Fügen Sie in der **Main**-Methode unmittelbar vor der Zeile `Console.ReadLine()` die folgende Methode hinzu:

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie eine Wiederholungsrichtlinie implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler] beschrieben.

## Verarbeiten von Gerät-zu-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Windows-Konsolen-App, die D2C-Nachrichten von IoT Hub verarbeitet. IoT Hub macht einen mit [Event Hubs] kompatiblen Endpunkt verfügbar, der einer Anwendung das Lesen von D2C-Nachrichten ermöglicht. In diesem Tutorial wird die [EventProcessorHost]-Klasse verwendet, um die Nachrichten in einer Konsolen-App zu verarbeiten. Weitere Informationen zum Verarbeiten von Nachrichten von Event Hubs finden Sie im Tutorial [Erste Schritte mit Event Hubs].

Die größte Herausforderung beim Implementieren der zuverlässigen Speicherung von Datenpunktnachrichten oder der Weiterleitung von interaktiven Nachrichten besteht darin, dass der Nachrichtenconsumer für die Event Hubs-Ereignisverarbeitung Prüfpunkte für den Status bereitstellen muss. Darüber hinaus sollten Sie beim Lesen von Event Hubs Prüfpunkte für große Batches festlegen, um einen hohen Durchsatz zu erzielen. Hieraus ergibt sich die Möglichkeit einer doppelten Verarbeitung für eine große Zahl von Nachrichten, wenn ein Fehler vorliegt und Sie zum vorherigen Prüfpunkt zurückkehren. In diesem Tutorial wird veranschaulicht, wie Azure-Speicherschreibvorgänge und Service Bus-Deduplizierungsfenster mit **EventProcessorHost**-Prüfpunkten synchronisiert werden.

Um Nachrichten zuverlässig in den Azure-Speicher zu schreiben, wird im Beispiel das Feature für den individuellen Block-Commit von [Blockblobs][Azure Block Blobs] verwendet. Der Ereignisprozessor akkumuliert Nachrichten im Arbeitsspeicher, bis es an der Zeit ist, einen Prüfpunktvorgang bereitzustellen (z.B. nachdem der akkumulierte Nachrichtenpuffer die maximale Blockgröße von 4 MB erreicht hat oder nachdem das Service Bus-Deduplizierungszeitfenster abgelaufen ist). Vor dem Prüfpunkt führt der Code für den Blob dann einen Commit für einen neuen Block durch.

Der Ereignisprozessor nutzt Event Hubs-Nachrichten-Offsets als Block-IDs. Dies ermöglicht eine Überprüfung der Deduplizierung vor dem Ausführen eines Commits für den neuen Block an den Speicher, und ein etwaiger Konflikt zwischen dem Commit eines Blocks und dem Prüfpunkt wird verhindert.

> [AZURE.NOTE] In diesem Tutorial wird ein einzelnes Speicherkonto zum Schreiben aller Nachrichten verwendet, die von IoT Hub abgerufen werden. Informationen dazu, ob Sie in Ihrer Lösung mehrere Azure Storage-Konten benötigen, finden Sie in den [Richtlinien zur Azure Storage-Skalierbarkeit].

Die Anwendung nutzt die Service Bus-Deduplizierungsfunktion, um beim Verarbeiten interaktiver Nachrichten Duplikate zu vermeiden. Das simulierte Gerät versieht jede interaktive Nachricht mit einer eindeutigen **MessageId**. Dadurch kann Service Bus sicherstellen, dass im angegebenen Zeitfenster für die Deduplizierung nicht zwei Nachrichten mit der gleichen **MessageId** an die Empfänger übertragen werden. Diese Deduplizierung sorgt zusammen mit der Abschlusssemantik pro Nachricht von Service Bus-Warteschlangen dafür, dass die zuverlässige Verarbeitung von interaktiven Nachrichten vereinfacht wird.

Um sicherzustellen, dass Nachrichten nicht außerhalb des Deduplizierungsfensters erneut übermittelt werden, wird der **EventProcessorHost**-Prüfpunktmechanismus im Code mit dem Deduplizierungsfenster der Service Bus-Warteschlange synchronisiert. Hierzu wird mindestens einmal bei jedem Ablauf des Deduplizierungsfensters (in diesem Tutorial nach einer Stunde) ein Prüfpunkt erzwungen.

> [AZURE.NOTE] In diesem Tutorial wird eine einzelne partitionierte Service Bus-Warteschlange verwendet, um alle interaktiven Nachrichten zu verarbeiten, die von IoT Hub abgerufen werden. Weitere Informationen zur Erfüllung der Skalierbarkeitsanforderungen Ihrer Lösung mithilfe von Service Bus-Warteschlangen finden Sie in der [Dokumentation zu Azure Service Bus].

### Bereitstellen eines Azure-Speicherkontos und einer Service Bus-Warteschlange
Zum Verwenden der [EventProcessorHost]-Klasse müssen Sie über ein Azure-Speicherkonto verfügen, damit von **EventProcessorHost** Prüfpunktinformationen erfasst werden können. Sie können ein vorhandenes Speicherkonto verwenden oder mithilfe der Anweisungen unter [Informationen zu Azure Storage] ein neues Konto erstellen. Notieren Sie sich die Verbindungszeichenfolge für das Speicherkonto.

> [AZURE.NOTE] Stellen Sie beim Kopieren und Einfügen der Speicherkonto-Verbindungszeichenfolge sicher, dass keine Leerzeichen aufgenommen werden.

Sie benötigen außerdem eine Service Bus-Warteschlange, um die zuverlässige Verarbeitung von interaktiven Nachrichten zu aktivieren. Sie können eine Warteschlange programmgesteuert mit einem Deduplizierungsfenster von einer Stunde erstellen. Dies ist unter [Erste Schritte mit Service Bus-Warteschlangen][Service Bus queue] beschrieben. Alternativ können Sie im [klassischen Azure-Portal][lnk-classic-portal] die folgenden Schritte ausführen:

1. Klicken Sie in der linken unteren Ecke auf **Neu**. Klicken Sie anschließend auf **App Services** > **Service Bus** > **Warteschlange** > **Benutzerdefiniert erstellen**. Geben Sie den Namen **d2ctutorial** ein, wählen Sie eine Region aus, und verwenden Sie einen vorhandenen Namespace, oder erstellen Sie einen neuen Namespace. Wählen Sie auf der nächsten Seite **Duplikaterkennung aktivieren** aus, und legen Sie für **Fenster für Duplikaterkennungsverlauf-Zeitpunkt** eine Stunde fest. Klicken Sie dann auf das Kontrollkästchen in der rechten unteren Ecke, um die Warteschlangenkonfiguration zu speichern.

    ![Erstellen einer Warteschlange im Azure-Portal][30]

2. Klicken Sie in der Liste mit den Service Bus-Warteschlangen auf **d2ctutorial** und dann auf **Konfigurieren**. Erstellen Sie zwei SAS-Richtlinien: eine mit dem Namen **send** und Berechtigungen zum **Senden** und eine mit dem Namen **listen** und Berechtigungen zum **Lauschen**. Klicken Sie anschließend am unteren Rand auf **Speichern**.

    ![Konfigurieren einer Warteschlange im Azure-Portal][31]

3. Klicken Sie oben auf **Dashboard** und am unteren Rand auf **Verbindungsinformationen**. Notieren Sie sich die beiden Verbindungszeichenfolgen.

    ![Warteschlangen-Dashboards im Azure-Portal][32]

### Erstellen des Ereignisprozessors

1. Klicken Sie in der aktuellen Visual Studio-Projektmappe auf **Datei** > **Hinzufügen** > **Neues Projekt**, um mit der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Windows-Projekt zu erstellen. Stellen Sie sicher, dass .NET-Framework-Version 4.5.1 oder höher verwendet wird. Geben Sie dem Projekt den Namen **ProcessDeviceToCloudMessages**, und klicken Sie auf **OK**.

    ![Neues Projekt in Visual Studio][10]

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ProcessDeviceToCloudMessages**, und klicken Sie dann auf **NuGet-Pakete verwalten**. Das Dialogfeld **NuGet-Paket-Manager** wird angezeigt.

3. Suchen Sie nach **WindowsAzure.ServiceBus**, klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Daraufhin wird das [Azure Service Bus-NuGet-Paket](https://www.nuget.org/packages/WindowsAzure.ServiceBus) mit allen Abhängigkeiten heruntergeladen und installiert, und dem Projekt wird ein Verweis auf das Paket hinzugefügt.

4. Suchen Sie nach **Microsoft Azure Service Bus Event Hub – EventProcessorHost**, klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Daraufhin wird das NuGet-Paket [Azure Service Bus Event Hub - EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) mit allen Abhängigkeiten heruntergeladen und installiert, und dem Projekt wird ein Verweis auf das Paket hinzugefügt.

5. Klicken Sie mit der rechten Maustaste auf das Projekt **ProcessDeviceToCloudMessages**, klicken Sie auf **Hinzufügen**, und klicken Sie dann auf **Klasse**. Geben Sie der neuen Klasse den Namen **StoreEventProcessor**, und klicken Sie auf **OK**, um die Klasse zu erstellen.

6. Fügen Sie die folgenden Anweisungen am Anfang der Datei "StoreEventProcessor.cs" hinzu:

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. Ersetzen Sie den Text der Klasse durch folgenden Code:

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    Die **EventProcessorHost**-Klasse ruft diese Klasse auf, um von IoT Hub empfangene D2C-Nachrichten zu verarbeiten. Mit dem Code in dieser Klasse wird die Logik zum zuverlässigen Speichern von Nachrichten in einem Blobcontainer implementiert, und interaktive Nachrichten werden an die Service Bus-Warteschlange weitergeleitet.

    Die **OpenAsync**-Methode initialisiert die **currentBlockInitOffset**-Variable. Diese dient zum Nachverfolgen des aktuellen Offsets der ersten Nachricht, die von diesem Ereignisprozessor gelesen wird. Denken Sie daran, dass jeder Prozessor für eine bestimmte Partition verantwortlich ist.

    Mit der **ProcessEventsAsync**-Methode wird ein Nachrichtenbatch von IoT Hub empfangen und wie folgt verarbeitet: Interaktive Nachrichten werden an die Service Bus-Warteschlange gesendet, und Datenpunktnachrichten werden an den Arbeitsspeicherpuffer mit dem Namen **toAppend** angefügt. Falls der Arbeitsspeicherpuffer das Blocklimit von 4 MB erreicht oder die Service Bus-Deduplizierungszeitfenster seit dem letzten Prüfpunkt abgelaufen sind (in diesem Tutorial eine Stunde), wird ein Prüfpunkt ausgelöst.

    Mit der **AppendAndCheckpoint**-Methode wird zuerst eine Block-ID für den anzufügenden Block generiert. Für Azure Storage müssen alle Block-IDs die gleiche Länge besitzen. Die Methode füllt den Offset daher mit führenden Nullen auf: `currentBlockInitOffset.ToString("0000000000000000000000000")`. Wenn ein Block mit dieser ID im Blob bereits vorhanden ist, überschreibt die Methode ihn mit dem aktuellen Inhalt des Puffers.

    > [AZURE.NOTE] Um den Code zu vereinfachen, wird in diesem Tutorial eine einzelne Blob-Datei pro Partition zum Speichern von Nachrichten verwendet. Für eine echte Lösung würde das „File Rolling“ implementiert werden, indem zusätzliche Dateien erstellt werden, wenn ein bestimmter Zeitraum verstrichen ist oder wenn sie eine bestimmte Größe erreichen (ein Azure-Blockblob kann maximal 195 GB groß sein).

8. Fügen Sie am Anfang der **Program**-Klasse die folgenden **using**-Anweisung hinzu:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. Ändern Sie die **Main**-Methode in der **Program**-Klasse wie unten gezeigt. Ersetzen Sie die IoT Hub-Verbindungszeichenfolge **iothubowner** (aus dem Tutorial [Erste Schritte mit IoT Hub]), die Speicherverbindungszeichenfolge und die Service Bus-Verbindungszeichenfolge durch Berechtigungen zum **Senden** für die Warteschlange mit dem Namen **d2ctutorial**:

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] Der Einfachheit halber wird in diesem Tutorial eine einzelne Instanz der [EventProcessorHost]-Klasse verwendet. Weitere Informationen finden Sie im [Programmierleitfaden für Event Hubs].

## Empfangen von interaktiven Nachrichten
In diesem Abschnitt schreiben Sie eine Windows-Konsolen-App, die interaktive Nachrichten aus der Service Bus-Warteschlange empfängt. Weitere Informationen zum Erstellen einer Lösung mit Service Bus finden Sie unter [.NET-Anwendungen mit mehreren Ebenen unter Verwendung von Azure Service Bus-Warteschlangen][].

1. Erstellen Sie in der aktuellen Visual Studio-Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Windows-Projekt. Geben Sie dem Projekt den Namen **ProcessD2CInteractiveMessages**.

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ProcessD2CInteractiveMessages**, und klicken Sie dann auf **NuGet-Pakete verwalten**. Das Fenster **NuGet-Paket-Manager** wird angezeigt.

3. Suchen Sie nach **WindowsAzure.ServiceBus**, klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Daraufhin wird [Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus) mit allen Abhängigkeiten heruntergeladen, installiert und mit einem Verweis versehen.

4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden **using**-Anweisungen hinzu:

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu. Ersetzen Sie die Verbindungszeichenfolge durch **Listen**-Berechtigungen für die Warteschlange namens **d2ctutorial**:

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

1.	Klicken Sie in Visual Studio im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen** aus. Wählen Sie **Mehrere Startprojekte** aus, und wählen Sie dann für die Projekte **ProcessDeviceToCloudMessages**, **SimulatedDevice** und **ProcessD2CInteractiveMessages** die Aktion **Starten** aus.

2.	Drücken Sie F5, um die drei Konsolenanwendungen zu starten. Die Anwendung **ProcessD2CInteractiveMessages** sollte jede interaktive Nachricht verarbeiten, die von der Anwendung **SimulatedDevice** gesendet wird.

  ![Drei Konsolenanwendungen][50]

> [AZURE.NOTE] Um Aktualisierungen in Ihrer Blobdatei sehen zu können, müssen Sie unter Umständen für die Konstante **MAX\_BLOCK\_SIZE** in der **StoreEventProcessor**-Klasse einen niedrigeren Wert festlegen (beispielsweise **1024**). Dies liegt daran, dass es bei den mit dem simulierten Gerät gesendeten Daten einige Zeit dauert, bis die Blockgröße erreicht wird. Bei einer kleineren Blockgröße müssen Sie nicht so lange warten, bis das Blob erstellt und aktualisiert wird. Allerdings sorgt die Verwendung einer höheren Blockgröße für eine bessere Skalierbarkeit der Anwendung.

## Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie mit der [EventProcessorHost]-Klasse zuverlässig Datenpunktnachrichten und interaktive D2C-Nachrichten verarbeiten.

Im Tutorial [Senden von Cloud-zu-Gerät-Nachrichten mithilfe von IoT Hub][lnk-c2d] erfahren Sie, wie Sie Nachrichten aus Ihrem Back-End an Ihre Geräte senden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT Suite][lnk-suite].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [IoT Hub-Entwicklerhandbuch].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Azur Blob Storage]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Service Bus queue]: ../service-bus/service-bus-dotnet-get-started-with-queues.md
[Service Bus-Warteschlange]: ../service-bus/service-bus-dotnet-get-started-with-queues.md

[Entwicklungsleitfaden für Azure IoT Hub – Gerät an Cloud]: iot-hub-devguide.md#d2c

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/
[Dokumentation zu Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT Hub-Entwicklerhandbuch]: iot-hub-devguide.md
[Erste Schritte mit IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Informationen zu Azure Storage]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Erste Schritte mit Event Hubs]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Richtlinien zur Azure Storage-Skalierbarkeit]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Programmierleitfaden für Event Hubs]: ../event-hubs/event-hubs-programming-guide.md
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[.NET-Anwendungen mit mehreren Ebenen unter Verwendung von Azure Service Bus-Warteschlangen]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

<!---HONumber=AcomDC_0831_2016-->