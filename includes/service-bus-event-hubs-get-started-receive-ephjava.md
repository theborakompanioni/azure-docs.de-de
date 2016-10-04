## Empfangen von Nachrichten mit EventProcessorHost in Java

EventProcessorHost ist eine Java-Klasse, die das Empfangen von Ereignissen von Event Hubs durch die Verwaltung von permanenten Prüfpunkten und parallelen Empfangsvorgängen von diesen Event Hubs vereinfacht. Mit EventProcessorHost können Sie Ereignisse selbst dann auf mehrere Empfänger aufteilen, wenn diese auf verschiedenen Knoten gehostet werden. Dieses Beispiel zeigt, wie EventProcessorHost für einen einzelnen Empfänger verwendet wird.

###Erstellen Sie ein Speicherkonto.

Um EventProcessorHost verwenden zu können, benötigen Sie ein [Azure-Speicherkonto][]\:

1. Melden Sie sich beim [klassischen Azure-Portal][] an, und klicken Sie im unteren Teil des Bildschirms auf **NEU**.

2. Klicken Sie auf **Data Services**, dann auf **Speicher** und **Schnellerfassung**, und geben Sie dann einen Namen für das Speicherkonto ein. Wählen Sie die gewünschte Region aus, und klicken Sie dann auf **Speicherkonto erstellen**.

    ![][11]

3. Klicken Sie auf das neu erstellte Speicherkonto und anschließend auf **Zugriffsschlüssel verwalten**:

    ![][12]

    Kopieren Sie den primären Zugriffsschlüssel für die spätere Verwendung in diesem Tutorial.

###Erstellen eines Java-Projekts mit EventProcessorHost

Die Java-Clientbibliothek für Event Hubs steht für die Verwendung in Maven-Projekten im [zentralen Maven-Repository][Maven Package] zur Verfügung. Sie können darauf verweisen, indem Sie in der Maven-Projektdatei die folgende Abhängigkeitsdeklaration verwenden:

``` XML
<dependency>
	<groupId>com.microsoft.azure</groupId>
	<artifactId>azure-eventhubs</artifactId>
	<version>{VERSION}</version>
</dependency>
<dependency>
	<groupId>com.microsoft.azure</groupId>
	<artifactId>azure-eventhubs-eph</artifactId>
	<version>{VERSION}</version>
</dependency>
```
 
Für unterschiedliche Arten von Buildumgebungen können Sie explizit die zuletzt veröffentlichten JAR-Dateien aus dem [zentralen Maven-Repository][Maven Package] oder vom [Versionsverteilungspunkt auf GitHub](https://github.com/Azure/azure-event-hubs/releases) abrufen.

1. Erstellen Sie für das folgende Beispiel zuerst ein neues Maven-Projekt für eine Konsolen-/Shellanwendung in Ihrer bevorzugten Java-Entwicklungsumgebung. Die Klasse hat den Namen ```ErrorNotificationHandler```.

	``` Java
	import java.util.function.Consumer;
	import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

	public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
	{
		@Override
		public void accept(ExceptionReceivedEventArgs t)
		{
			System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
		}
	}
	```

2. Verwenden Sie den folgenden Code, um eine neue Klasse mit dem Namen ```EventProcessor``` zu erstellen.

	```Java
	import com.microsoft.azure.eventhubs.EventData;
	import com.microsoft.azure.eventprocessorhost.CloseReason;
	import com.microsoft.azure.eventprocessorhost.IEventProcessor;
	import com.microsoft.azure.eventprocessorhost.PartitionContext;

	public class EventProcessor implements IEventProcessor
	{
		private int checkpointBatchingCount = 0;

		@Override
		public void onOpen(PartitionContext context) throws Exception
		{
			System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
		}

		@Override
		public void onClose(PartitionContext context, CloseReason reason) throws Exception
		{
			System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
		}
		
		@Override
		public void onError(PartitionContext context, Throwable error)
		{
			System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
		}

		@Override
		public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
		{
			System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
			int messageCount = 0;
			for (EventData data : messages)
			{
				System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
						data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
				messageCount++;
				
				this.checkpointBatchingCount++;
				if ((checkpointBatchingCount % 5) == 0)
				{
					System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
						data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
					context.checkpoint(data);
				}
			}
			System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
		}
	}
	```

3. Erstellen Sie abschließend eine Klasse mit dem Namen ```EventProcessorSample```, indem Sie den folgenden Code verwenden.

	```Java
	import com.microsoft.azure.eventprocessorhost.*;
	import com.microsoft.azure.servicebus.ConnectionStringBuilder;
	import com.microsoft.azure.eventhubs.EventData;

	public class EventProcessorSample
	{
		public static void main(String args[])
		{
			final String consumerGroupName = "$Default";
			final String namespaceName = "----ServiceBusNamespaceName-----";
			final String eventHubName = "----EventHubName-----";
			final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
			final String sasKey = "---SharedAccessSignatureKey----";

			final String storageAccountName = "---StorageAccountName----";
			final String storageAccountKey = "---StorageAccountKey----";
			final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
			
			ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
			
			EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
			
			System.out.println("Registering host named " + host.getHostName());
			EventProcessorOptions options = new EventProcessorOptions();
			options.setExceptionNotification(new ErrorNotificationHandler());
			try
			{
				host.registerEventProcessor(EventProcessor.class, options).get();
			}
			catch (Exception e)
			{
				System.out.print("Failure while registering: ");
				if (e instanceof ExecutionException)
				{
					Throwable inner = e.getCause();
					System.out.println(inner.toString());
				}
				else
				{
					System.out.println(e.toString());
				}
			}

			System.out.println("Press enter to stop");
			try
			{
				System.in.read();
				host.unregisterEventProcessor();
				
				System.out.println("Calling forceExecutorShutdown");
				EventProcessorHost.forceExecutorShutdown(120);
			}
			catch(Exception e)
			{
				System.out.println(e.toString());
				e.printStackTrace();
			}
			
			System.out.println("End of sample");
		}
	}
	```

4. Ersetzen Sie die folgenden Felder durch die Werte, die Sie beim Erstellen des Event Hub und des Speicherkontos verwendet haben.

	``` Java
	final String namespaceName = "----ServiceBusNamespaceName-----";
	final String eventHubName = "----EventHubName-----";

	final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
	final String sasKey = "---SharedAccessSignatureKey----";

	final String storageAccountName = "---StorageAccountName----"
	final String storageAccountKey = "---StorageAccountKey----";
	```

> [AZURE.NOTE] Dieses Lernprogramm verwendet eine einzelne Instanz von EventProcessorHost. Zur Erhöhung des Durchsatzes wird empfohlen, mehrere Instanzen von EventProcessorHost auszuführen. In diesen Fällen koordinieren sich die verschiedenen automatisch untereinander, um die Last der eingegangenen Ereignisse ausgeglichen zu verteilen. Wenn mehrere Empfänger für jeden Prozess *alle* Ereignisse verarbeiten sollen, müssen Sie das **ConsumerGroup**-Konzept verwenden. Wenn Ereignisse von anderen Computern empfangen werden, kann es hilfreich sein, die EventProcessorHost-Instanzen nach den Computern (oder Rollen) zu benennen, auf denen sie bereitgestellt worden sind.

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md
[Azure-Speicherkonto]: ../storage/storage-create-storage-account.md
[klassischen Azure-Portal]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png

<!---HONumber=AcomDC_0928_2016-->