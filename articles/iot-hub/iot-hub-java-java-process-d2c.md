<properties
	pageTitle="Verarbeiten von D2C-Nachrichten mit IoT Hub (Java) | Microsoft Azure"
	description="In diesem Java-Tutorial werden nützliche Verfahren zum Verarbeiten von Gerät-zu-Cloud-Nachrichten (Device-to-Cloud, D2C) mit IoT Hub beschrieben."
	services="iot-hub"
	documentationCenter="java"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/01/2016"
     ms.author="dobett"/>

# Tutorial: Verarbeiten von D2C-Nachrichten mit IoT Hub mithilfe von Java

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Anwendungs-Back-End ermöglicht. Weitere Tutorials ([Erste Schritte mit IoT Hub] und [Senden von C2D-Nachrichten mit IoT Hub][lnk-c2d]) veranschaulichen, wie Sie die grundlegenden Device-to-Cloud- und Cloud-to-Device-Messagingfunktionen von IoT Hub verwenden.

Dieses Tutotrial baut auf dem Code des Tutotrials [Erste Schritte mit IoT Hub] auf und veranschaulicht zwei skalierbare Muster, die Sie zum Verarbeiten von D2C-Nachrichten verwenden können:

- Die zuverlässige Speicherung von Nachrichten von einem Gerät an die Cloud in [Azur Blob Storage]. Ein häufiges Szenario ist die *Cold Path*-Analyse, in der Sie Telemetriedaten in Blobs speichern, um sie als Eingabe für Analyseprozesse zu verwenden. Diese Prozesse können von Tools wie z.B. [Azure Data Factory] oder dem [HDInsight (Hadoop)]-Stapel gesteuert werden.

- Zuverlässige Verarbeitung von *interaktiven* D2C-Nachrichten: D2C-Nachrichten sind interaktiv, wenn es sich um unmittelbare Auslöser für eine Reihe von Aktionen im Anwendungs-Back-End handelt. Beispielsweise könnte ein Gerät eine Alarmnachricht senden, die das Einfügen eines Tickets in ein CRM-System auslöst. Im Gegensatz dazu werden *Datenpunktnachrichten* einfach in ein Analysemodul eingegeben. Temperaturtelemetriedaten von einem Gerät, die zur späteren Analyse gespeichert werden, sind beispielsweise Datenpunktnachrichten.

Da in IoT Hub ein [Event Hubs][lnk-event-hubs]-kompatibler Endpunkt zum Empfangen von D2C-Nachrichten verfügbar gemacht wird, wird in diesem Tutorial eine [EventProcessorHost]-Instanz verwendet. Diese Instanz:

* Speichert zuverlässig *Datenpunkt*-Nachrichten in Azure-Blobspeicher.
* Leitet *interaktive* D2C-Nachrichten zur sofortigen Verarbeitung an eine Azure [Service Bus-Warteschlange] weiter.

Service Bus stellt die zuverlässige Verarbeitung von interaktiven Nachrichten sicher, da Checkpoints pro Nachricht und die Deduplizierung auf Grundlage von Zeitfenstern bereitgestellt werden.

> [AZURE.NOTE] Eine **EventProcessorHost**-Instanz ist nur eine Möglichkeit, interaktive Nachrichten zu verarbeiten. Weitere Optionen sind [Azure Service Fabric][lnk-service-fabric] und [Azure Stream Analytics][lnk-stream-analytics].

Am Ende dieses Tutorials führen Sie drei Java-Konsolen-Apps aus:

* **simulated-device**, eine geänderte Version der im Tutorial [Erste Schritte mit IoT Hub] erstellten App, die jede Sekunde Datenpunkt-D2C-Nachrichten und alle 10 Sekunden interaktive D2C-Nachrichten sendet. Für diese App wird das AMQPS-Protokoll für die Kommunikation mit IoT Hub verwendet.
* **process-d2c-messages** verwendet die [EventProcessorHost]-Klasse zum Abrufen von Nachrichten aus dem Event Hubs-kompatiblen Endpunkt. Dann speichert sie zuverlässig Datenpunktnachrichten in Azure-Blobspeicher und leitet interaktive Nachrichten an eine Service Bus-Warteschlange weiter.
* **process-interactive-messages** entfernt die interaktiven Nachrichten aus der Service Bus-Warteschlange.

> [AZURE.NOTE] IoT Hub verfügt über SDK-Unterstützung für zahlreiche Geräteplattformen und Sprachen, z.B. C, Java und JavaScript. Anleitungen zum Ersetzen des simulierten Geräts in diesem Tutorial durch ein physisches Gerät und Informationen zum Verbinden von Geräten mit einem IoT Hub finden Sie im [Azure IoT Developer Center].

Dieses Tutorial kann direkt auf andere Möglichkeiten zum Verarbeiten von Event Hubs-kompatiblen Nachrichten übertragen werden, z.B. Projekte vom Typ [HDInsight (Hadoop)]. Weitere Informationen finden Sie unter [Entwicklungsleitfaden für Azure IoT Hub – Gerät an Cloud].

Für dieses Tutorial benötigen Sie Folgendes:

+ Eine vollständig funktionierende Version des Tutorials [Erste Schritte mit IoT Hub].

+ Java SE 8. <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial Java unter Windows oder Linux installieren.

+ Maven 3. <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial Maven unter Windows oder Linux installieren.

+ Ein aktives Azure-Konto. <br/>Falls Sie über kein Azure-Abonnement verfügen, können Sie in wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

Sie sollten über grundlegende Kenntnisse in Bezug auf [Azure Storage] und [Azure Service Bus] verfügen.


## Senden interaktiver Nachrichten von einem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierte Geräteanwendung, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, sodass sie interaktive D2C-Nachrichten an IoT Hub sendet.

1. Öffnen Sie die Datei „simulated-device\\src\\main\\java\\com\\mycompany\\app\\App.java“ mit einem Text-Editor. Diese Datei enthält den Code für die **simulated-device**-App, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben.

2. Fügen Sie der **App**-Klasse die folgende geschachtelte Klasse hinzu:

    ```
    private static class InteractiveMessageSender implements Runnable {
      public void run() {
        try {
          while (true) {
            String msgStr = "Alert message!";
            Message msg = new Message(msgStr);
            msg.setMessageId(java.util.UUID.randomUUID().toString());
            msg.setProperty("messageType", "interactive");
            System.out.println("Sending interactive message: " + msgStr);

            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);

            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(10000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished sending interactive messages.");
        }
      }
    }
    ```

    Diese Klasse ist vergleichbar mit der **MessageSender**-Klasse im **simulated-device**-Projekt. Der einzige Unterschied besteht darin, dass Sie jetzt die Systemeigenschaft **MessageId** und eine benutzerdefinierte Eigenschaft **messageType** festlegen. Der Code weist der Eigenschaft **MessageId** einen Universally Unique Identifier (UUID) zu. Der Service Bus kann mit diesem Bezeichner Nachrichten, die er empfängt, deduplizieren. Im Beispiel wird die Eigenschaft **messageType** verwendet, um interaktive Nachrichten von Datenpunktnachrichten zu unterscheiden. Die Anwendung übergibt diese Informationen in Nachrichteneigenschaften anstatt im Nachrichtentext. Auf diese Weise muss der Ereignisprozessor zum Nachrichtenrouting nicht die gesamte Nachricht deserialisieren.

    > [AZURE.NOTE] Es ist wichtig, die **MessageId** zur Deduplizierung interaktiver Nachrichten im Gerätecode zu erstellen. Zeitweise Netzwerkkommunikation oder andere Fehler könnten dazu führen, dass mehrere Neuübertragungen der gleichen Nachricht von diesem Gerät erfolgen. Sie können anstelle einer UUID auch eine semantische Nachrichten-ID verwenden, z.B. einen Hash der relevanten Nachrichtendatenfelder.

3. Ändern Sie die Methode **main** wie im folgenden Codeausschnitt gezeigt, um sowohl interaktive Nachrichten als auch Datenpunktnachrichten zu senden:

    ````
    MessageSender sender = new MessageSender();
    InteractiveMessageSender interactiveSender = new InteractiveMessageSender();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(sender);
    executor.execute(interactiveSender);
    ````

4. Speichern und schließen Sie die Datei „simulated-device\\src\\main\\java\\com\\mycompany\\app\\App.java“.

    > [AZURE.NOTE] Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie eine Wiederholungsrichtlinie implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler] beschrieben.

5. Führen Sie zum Erstellen der Anwendung **simulated-device** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „simulated-device“ aus:

    ```
    mvn clean package -DskipTests
    ```

## Verarbeiten von Gerät-zu-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die D2C-Nachrichten (Device-to-Cloud) aus dem IoT Hub verarbeitet. IoT Hub macht einen mit [Event Hubs] kompatiblen Endpunkt verfügbar, der einer Anwendung das Lesen von D2C-Nachrichten ermöglicht. In diesem Tutorial wird die [EventProcessorHost]-Klasse verwendet, um die Nachrichten in einer Konsolen-App zu verarbeiten. Weitere Informationen zum Verarbeiten von Nachrichten von Event Hubs finden Sie im Tutorial [Erste Schritte mit Event Hubs].

Die wesentliche Herausforderung beim Implementieren der zuverlässigen Speicherung von Datenpunktnachrichten oder der Weiterleitung von interaktiven Nachrichten besteht darin, dass die Ereignisverarbeitung davon abhängt, dass der Nachrichtenconsumer Prüfpunkte für den Status bereitstellt. Darüber hinaus sollten Sie beim Lesen von Event Hubs Prüfpunkte für große Batches festlegen, um einen hohen Durchsatz zu erzielen. Hieraus ergibt sich die Möglichkeit einer doppelten Verarbeitung für eine große Zahl von Nachrichten, wenn ein Fehler vorliegt und Sie zum vorherigen Prüfpunkt zurückkehren. In diesem Tutorial lernen Sie, wie Azure-Speicherschreibvorgänge und Service Bus-Deduplizierungsfenster mit **EventProcessorHost**-Prüfpunkten synchronisiert werden.

Um Nachrichten zuverlässig in den Azure-Speicher zu schreiben, wird im Beispiel das Feature für den individuellen Block-Commit von [Blockblobs][Azure Block Blobs] verwendet. Der Ereignisprozessor sammelt Nachrichten im Arbeitsspeicher, bis der Zeitpunkt gekommen ist, einen Prüfpunkt bereitzustellen. Dies ist z.B. der Fall, wenn der Puffer, der die gesammelten Nachrichten enthält, die maximale Blockgröße von 4MB erreicht, oder nach dem Verstreichen des Zeitfensters zur Service Bus-Deduplizierung. Vor dem Prüfpunkt führt der Code für den Blob dann einen Commit für einen neuen Block durch.

Der Ereignisprozessor nutzt Event Hubs-Nachrichten-Offsets als Block-IDs. Dieser Mechanismus ermöglicht dem Ereignisprozessor eine Überprüfung der Deduplizierung vor dem Ausführen eines Commits für den neuen Block an den Speicher, und ein etwaiger Konflikt zwischen dem Commit eines Blocks und dem Prüfpunkt wird verhindert.

> [AZURE.NOTE] In diesem Tutorial wird ein einzelnes Speicherkonto zum Schreiben aller Nachrichten verwendet, die von IoT Hub abgerufen werden. Informationen dazu, ob Sie in Ihrer Lösung mehrere Azure Storage-Konten benötigen, finden Sie in den [Richtlinien zur Azure Storage-Skalierbarkeit].

Die Anwendung nutzt die Service Bus-Deduplizierungsfunktion, um beim Verarbeiten interaktiver Nachrichten Duplikate zu vermeiden. Das simulierte Gerät versieht jede interaktive Nachricht mit einer eindeutigen **MessageId**. Mit dieser ID kann Service Bus sicherstellen, dass im angegebenen Zeitfenster für die Deduplizierung nicht zwei Nachrichten mit der gleichen **MessageId** an die Empfänger übertragen werden. Diese Deduplizierung sorgt zusammen mit der Abschlusssemantik pro Nachricht von Service Bus-Warteschlangen dafür, dass die zuverlässige Verarbeitung von interaktiven Nachrichten vereinfacht wird.

Um sicherzustellen, dass Nachrichten nicht außerhalb des Deduplizierungsfensters erneut übermittelt werden, wird der **EventProcessorHost**-Prüfpunktmechanismus im Code mit dem Deduplizierungsfenster der Service Bus-Warteschlange synchronisiert. Für diese Synchronisierung wird mindestens einmal bei jedem Ablauf des Deduplizierungsfensters (in diesem Tutorial nach einer Stunde) ein Prüfpunkt erzwungen.

> [AZURE.NOTE] In diesem Tutorial wird eine einzelne partitionierte Service Bus-Warteschlange verwendet, um alle interaktiven Nachrichten zu verarbeiten, die von IoT Hub abgerufen werden. Weitere Informationen zur Erfüllung der Skalierbarkeitsanforderungen Ihrer Lösung mithilfe von Service Bus-Warteschlangen finden Sie in der [Dokumentation zu Azure Service Bus].

### Bereitstellen eines Azure-Speicherkontos und einer Service Bus-Warteschlange

Zum Verwenden der [EventProcessorHost]-Klasse müssen Sie über ein Azure Storage-Konto verfügen, damit **EventProcessorHost** Prüfpunktinformationen erfassen kann. Sie können ein vorhandenes Speicherkonto verwenden oder mithilfe der Anweisungen unter [Informationen zu Azure Storage] ein neues Konto erstellen. Notieren Sie sich die Verbindungszeichenfolge für das Speicherkonto.

> [AZURE.NOTE] Stellen Sie beim Kopieren und Einfügen der Speicherkonto-Verbindungszeichenfolge sicher, dass keine Leerzeichen aufgenommen werden.

Sie benötigen außerdem eine Service Bus-Warteschlange, um die zuverlässige Verarbeitung von interaktiven Nachrichten zu aktivieren. Sie können eine Warteschlange programmgesteuert mit einem Deduplizierungsfenster von einer Stunde erstellen. Dies ist unter [Erste Schritte mit Service Bus-Warteschlangen][Service Bus queue] beschrieben. Alternativ können Sie im [klassischen Azure-Portal][lnk-classic-portal] die folgenden Schritte ausführen:

1. Klicken Sie in der linken unteren Ecke auf **Neu**. Klicken Sie anschließend auf **App Services** > **Service Bus** > **Warteschlange** > **Benutzerdefiniert erstellen**. Geben Sie den Namen **d2ctutorial** ein, wählen Sie eine Region aus, und verwenden Sie einen vorhandenen Namespace, oder erstellen Sie einen neuen Namespace. Notieren Sie sich den Namen des Namespaces, denn Sie werden ihn später in diesem Tutorial benötigen. Wählen Sie auf der nächsten Seite **Duplikaterkennung aktivieren** aus, und legen Sie für **Fenster für Duplikaterkennungsverlauf-Zeitpunkt** eine Stunde fest. Klicken Sie dann auf das Kontrollkästchen in der rechten unteren Ecke, um die Warteschlangenkonfiguration zu speichern.

    ![Erstellen einer Warteschlange im Azure-Portal][30]

2. Klicken Sie in der Liste mit den Service Bus-Warteschlangen auf **d2ctutorial** und dann auf **Konfigurieren**. Erstellen Sie zwei SAS-Richtlinien: eine mit dem Namen **send** und Berechtigungen zum **Senden** und eine mit dem Namen **listen** und Berechtigungen zum **Lauschen**. Notieren Sie sich die **Primärschlüsselwerte** für beide Richtlinien, denn Sie werden sie später in diesem Tutorial benötigen. Klicken Sie anschließend am unteren Rand auf **Speichern**.

    ![Konfigurieren einer Warteschlange im Azure-Portal][31]

### Erstellen des Ereignisprozessors

In diesem Abschnitt erstellen Sie eine Java-Anwendung zum Verarbeiten von Nachrichten aus dem Event Hubs-kompatiblen Endpunkt.

Die erste Aufgabe ist, ein Maven-Projekt namens **process-d2c-messages** hinzuzufügen, das D2C-Nachrichten aus dem mit IoT Hub-Events Hubs kompatiblen Endpunkt empfängt und diese Nachrichten an andere Back-End-Dienste weiterleitet.

1. Erstellen Sie im Ordner „iot-java-get-started“, den Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, ein neues Maven-Projekt namens **process-d2c-messages**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Eingabeaufforderung zum neuen Ordner „process-d2c-messages“.

3. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „process-d2c-messages“, und fügen Sie dem Knoten **dependencies** die folgenden Abhängigkeiten hinzu. Mit diesen Abhängigkeiten können Sie die Azure Event Hubs-, Azure Event Hubs-EPH- und Azure Service Bus-Pakete in Ihrer Anwendung zur Interaktion mit Ihrer IoT Hub- und Service Bus-Warteschlange verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-eventhubs-eph</artifactId>
      <version>0.8.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Speichern und schließen Sie die Datei „pom.xml“.

Die nächste Aufgabe besteht darin, dem Projekt eine **ErrorNotificationHandler**-Klasse hinzuzufügen.

1. Verwenden Sie einen Text-Editor, um eine process-d2c-messages\\src\\main\\java\\com\\mycompany\\app\\ErrorNotificationHandler.java-Datei zu erstellen. Fügen Sie der Datei folgenden Code zur Anzeige von Fehlermeldungen der **EventProcesssorHost**-Instanz hinzu:

    ```
    package com.mycompany.app;

    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;

    public class ErrorNotificationHandler implements
        Consumer<ExceptionReceivedEventArgs> {
      @Override
      public void accept(ExceptionReceivedEventArgs t) {
        System.out.println("EventProcessorHost: Host " + t.getHostname()
            + " received general error notification during " + t.getAction() + ": "
            + t.getException().toString());
      }
    }
    ```

2. Speichern und schließen Sie die Datei ErrorNotificationHandler.java.

Jetzt können Sie eine Klasse hinzufügen, die die **IEventProcessor**-Schnittstelle implementiert. Die **EventProcessorHost**-Klasse ruft diese Klasse auf, um von IoT Hub empfangene D2C-Nachrichten zu verarbeiten. Mit dem Code in dieser Klasse wird die Logik zum zuverlässigen Speichern von Nachrichten in einem Blobcontainer implementiert, und interaktive Nachrichten werden an die Service Bus-Warteschlange weitergeleitet.

Die **onEvents**-Methode legt die **latestEventData**-Variable fest, die die Offset- und Sequenznummer der letzten vom Ereignisprozessor gelesenen Nachricht verfolgt. Denken Sie daran, dass jeder Prozessor für eine bestimmte Partition verantwortlich ist. Mit der **onEvents**-Methode wird dann ein Nachrichtenbatch von IoT Hub empfangen und wie folgt verarbeitet: Interaktive Nachrichten werden an die Service Bus-Warteschlange gesendet, und Datenpunktnachrichten werden dem **toAppend**-Arbeitsspeicherpuffer angefügt. Falls der Arbeitsspeicherpuffer das Blocklimit von 4MB erreicht, oder die Deduplizierungszeitfenster abgelaufen sind (in diesem Tutorial eine Stunde nach dem letzten Prüfpunkt), löst die Methode einen Prüfpunkt aus.

Mit der **AppendAndCheckPoint**-Methode wird zuerst eine **blockId** für den anzufügenden Block generiert. Für Azure Storage müssen alle Block-IDs die gleiche Länge besitzen. Die Methode füllt den Offset daher mit führenden Nullen auf. Wenn ein Block mit dieser ID bereits im Blob vorhanden ist, überschreibt die Methode ihn mit dem aktuellen Pufferinhalt.

> [AZURE.NOTE] Um den Code zu vereinfachen, wird in diesem Tutorial eine einzelne Blob-Datei pro Partition zum Speichern von Nachrichten verwendet. Für eine echte Lösung würde das „File Rolling“ implementiert werden, indem zusätzliche Dateien erstellt werden, wenn ein bestimmter Zeitraum verstrichen ist oder wenn sie eine bestimmte Größe erreichen. Denken Sie daran, dass ein Azure-Blockblob höchstens 195GB Daten enthalten kann.

Die nächste Aufgabe besteht darin, die Schnittstelle **IEventProcessor** zu implementieren:

1. Verwenden Sie einen Text-Editor, um eine process-d2c-messages\\src\\main\\java\\com\\mycompany\\app\\EventProcessor.java-Datei zu erstellen.

2. Fügen Sie die folgenden Importe und die Klassendefinition der Datei „EventProcessor.java“ hinzu. Die **EventProcessor**-Klasse implementiert die **IEventProcessor**-Schnittstelle, die das Verhalten des Event Hubs-Clients definiert:

    ```
    package com.mycompany.app;

    import java.io.ByteArrayInputStream;
    import java.io.ByteArrayOutputStream;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.nio.charset.StandardCharsets;
    import java.time.Duration;
    import java.time.Instant;
    import java.util.ArrayList;
    import java.util.Base64;
    import java.util.concurrent.ExecutionException;

    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.storage.*;
    import com.microsoft.azure.storage.blob.*;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.BrokeredMessage;

    public class EventProcessor implements IEventProcessor {

    }
    ```

3. Fügen Sie die folgenden Methoden der **EventProcessor**-Klasse hinzu, um die **IEventProcessor**-Schnittstelle zu implementieren:

    ```
    @Override
    public void onOpen(PartitionContext context) throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is opening");
    }

    @Override
    public void onClose(PartitionContext context, CloseReason reason)
        throws Exception {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " is closing for reason "
          + reason.toString());
    }

    @Override
    public void onError(PartitionContext context, Throwable error) {
      System.out.println("EventProcessorHost: Partition "
          + context.getPartitionId() + " onError: " + error.toString());
    }

    @Override
    public void onEvents(PartitionContext context, Iterable<EventData> messages)
        throws Exception {
    }
    ```

4. Fügen Sie die folgenden Variablen auf Klassenebene der **EventProcessor**-Klasse hinzu:

    ```
    public static CloudBlobContainer blobContainer;
    public static ServiceBusContract serviceBusContract;

    // Use a smaller MAX_BLOCK_SIZE value to test.
    final private int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
    final private Duration MAX_CHECKPOINT_TIME = Duration.ofHours(1);

    private ByteArrayOutputStream toAppend = new ByteArrayOutputStream(
        MAX_BLOCK_SIZE);
    private Instant start = Instant.now();
    private EventData latestEventData;
    ```

5. Fügen Sie eine **AppendAndCheckPoint**-Methode mit der folgenden Signatur der **EventProcessor**-Klasse hinzu:

    ```
    private void AppendAndCheckPoint(PartitionContext context)
      throws URISyntaxException, StorageException, IOException,
      IllegalArgumentException, InterruptedException, ExecutionException {
    }
    ```

6. Fügen Sie den folgenden Code der **AppendAndCheckPoint**-Methode hinzu, um die aktuelle Offset- und Sequenznummer der Nachricht in der Partition abzurufen:

    ```
    String currentOffset = latestEventData.getSystemProperties().getOffset();
    Long currentSequence = latestEventData.getSystemProperties().getSequenceNumber();
    System.out
        .printf(
            "\nAppendAndCheckPoint using partition: %s, offset: %s, sequence: %s\n",
            context.getPartitionId(), currentOffset, currentSequence);
    ```

7. Verwenden Sie in der **AppendAndCheckPoint**-Methode den aktuellen Offsetwert zum Erstellen einer **BlockEntry**-Instanz für den nächsten Block, der im Blob gespeichert werden soll:

    ```
    Long blockId = Long.parseLong(currentOffset);
    String blockIdString = String.format("startSeq:%1$025d", blockId);
    String encodedBlockId = Base64.getEncoder().encodeToString(
        blockIdString.getBytes(StandardCharsets.US_ASCII));
    BlockEntry block = new BlockEntry(encodedBlockId);
    ```

8. Laden Sie in der **AppendAndCheckPoint**-Methode die aktuelle Gruppe von Nachrichten in das Blockblob hoch, und rufen Sie die aktuelle Liste der Blöcke ab:

    ```
    String blobName = String.format("iothubd2c_%s", context.getPartitionId());
    CloudBlockBlob currentBlob = blobContainer.getBlockBlobReference(blobName);

    currentBlob.uploadBlock(block.getId(),
        new ByteArrayInputStream(toAppend.toByteArray()), toAppend.size());
    ArrayList<BlockEntry> blockList = currentBlob.downloadBlockList();
    ```

9. Erstellen Sie in der **AppendAndCheckPoint**-Methode entweder den ersten Block in einem neuen Blob, oder fügen Sie den Block dem vorhandenen Blob hinzu:

    ```
    if (currentBlob.exists()) {
      // Check if we should append new block or overwrite existing block
      BlockEntry last = blockList.get(blockList.size() - 1);
      if (blockList.size() > 0 && !last.getId().equals(block.getId())) {
        System.out.printf("Appending block %s to blob %s\n", blockId, blobName);
        blockList.add(block);
      } else {
        System.out.printf("Overwriting block %s in blob %s\n", blockId,
            blobName);
      }
    } else {
      System.out.printf("Creating initial block %s in new blob: %s\n", blockId,
          blobName);
      blockList.add(block);
    }
    currentBlob.commitBlockList(blockList);
    ```

10. Erstellen Sie schließlich in der **AppendAndCheckPoint**-Methode einen Prüfpunkt in der Partition, und bereiten Sie das Speichern des nächsten Nachrichtenblocks vor:

    ```
    context.checkpoint(latestEventData);

    // Reset everything after the checkpoint.
    toAppend.reset();
    start = Instant.now();
    System.out.printf("Checkpointed on partition id: %s\n",
        context.getPartitionId());
    ```

11. Fügen Sie in der **onEvents**-Methode den folgenden Code zum Empfangen von Nachrichten vom IoT Hub-Endpunkt und Weiterleiten von interaktiven Nachrichten an die Service Bus-Warteschlange hinzu. Rufen Sie dann die **AppendAndCheckPoint**-Methode auf, wenn der Block voll ist, oder das Timeout abläuft:

    ```
    if (messages != null) {
      for (EventData eventData : messages) {
        latestEventData = eventData;
        byte[] data = eventData.getBody();
        if (eventData.getProperties().containsKey("messageType")
            && eventData.getProperties().get("messageType")
                .equals("interactive")) {
          String messageId = (String) eventData.getSystemProperties().get(
              "message-id");
          BrokeredMessage message = new BrokeredMessage(data);
          message.setMessageId(messageId);
          serviceBusContract.sendQueueMessage("d2ctutorial", message);
          continue;
        }
        if (toAppend.size() + data.length > MAX_BLOCK_SIZE
            || Duration.between(start, Instant.now()).compareTo(
                MAX_CHECKPOINT_TIME) > 0) {
          AppendAndCheckPoint(context);
        }
        toAppend.write(data);
      }
    }
    ```

12. Fügen Sie schließlich in die **onEvents**-Methode eine „else if“-Klausel zum Aufrufen von **AppendAndCheckPoint** ein, wenn das Timeout abläuft, während keine Nachrichten von IoT Hub eingehen:

    ```
    else if ((toAppend.size() > 0)
        && Duration.between(start, Instant.now())
            .compareTo(MAX_CHECKPOINT_TIME) > 0) {
      AppendAndCheckPoint(context);
    }
    ```

13. Speichern Sie die Änderungen in der Datei EventProcessor.java.

Die letzte Aufgabe im **process-d2c-messages**-Projekt ist das Hinzufügen von Code zur **main**-Methode, die eine **EventProcessorHost**-Instanz instanziiert.

1. Öffnen Sie die Datei „process-d2c-messages\\src\\main\\java\\com\\mycompany\\app\\App.java“ mit einem Text-Editor.

2. Fügen Sie der Datei die folgenden **import**-Anweisungen hinzu:

    ```
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.storage.CloudStorageAccount;
    import com.microsoft.azure.storage.StorageException;
    import com.microsoft.azure.storage.blob.CloudBlobClient;
    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusConfiguration;
    import com.microsoft.windowsazure.services.servicebus.ServiceBusService;

    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.concurrent.*;
    ```

3. Fügen Sie der **App**-Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie **{yourstorageaccountconnectionstring}** mit der Azure-Speicherkonto-Verbindungszeichenfolge, die Sie bereits im Abschnitt [Bereitstellen eines Azure-Speicherkontos und einer Service Bus-Warteschlange](#provision-an-azure-storage-account-and-a-service-bus-queue) notiert haben:

    ```
    private final static String storageConnectionString = "{yourstorageaccountconnectionstring}";
    ```

4. Fügen Sie die folgenden Variablen auf Klassenebene der **App**-Klasse hinzu, und ersetzen Sie **{yourservicebusnamespace}** mit Ihrem Service Bus-Namespace und **{yourservicebussendkey}** mit dem **send**-Schlüssel Ihrer Warteschlange. Sie haben bereits im Abschnitt [Bereitstellen eines Azure-Speicherkontos und einer Service Bus-Warteschlange](#provision-an-azure-storage-account-and-a-service-bus-queue) Namespace und **listen**-Schlüsselwerte notiert:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "send";
    private final static String serviceBusSASKey = "{yourservicebussendkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    ```

5. Fügen Sie der **App**-Klasse die folgenden Variablen auf Klassenebene hinzu: Ersetzen Sie **{youreventhubcompatibleendpoint}** mit dem Event Hub-kompatiblen Endpunktnamen. Der Endpunktname sieht wie **ihs....namespace** aus, entfernen Sie daher das **sb://**-Präfix und das **.servicebus.windows.net/**-Suffix. Ersetzen Sie **{youreventhubcompatiblename}** mit dem Event Hub-kompatiblen Namen. Ersetzen Sie **{youriothubkey}** mit dem **iothubowner**-Schlüssel. Sie haben sich diese Werte im Abschnitt [Erstellen eines IoT Hubs][lnk-create-an-iot-hub] des Tutorials *Erste Schritte mit Azure IoT Hub für Java* notiert:

    ```
    private final static String consumerGroupName = "$Default";
    private final static String namespaceName = "{youreventhubcompatibleendpoint}";
    private final static String eventHubName = "{youreventhubcompatiblename}";
    private final static String sasKeyName = "iothubowner";
    private final static String sasKey = "{youriothubkey}";
    ```

6. Ändern Sie die Signatur der **main**-Methode wie folgt:

    ```
    public static void main(String args[]) throws InvalidKeyException,
      URISyntaxException, StorageException {
    }
    ```

7. Fügen Sie der **main**-Methode den folgenden Code hinzu, um einen Verweis auf den Blobcontainer abzurufen, in dem die Nachrichten gespeichert sind:

    ```
    System.out.println("Process D2C messages using EventProcessorHost");
    CloudStorageAccount account = CloudStorageAccount
        .parse(storageConnectionString);
    CloudBlobClient client = account.createCloudBlobClient();
    EventProcessor.blobContainer = client
        .getContainerReference("d2cjavatutorial");
    EventProcessor.blobContainer.createIfNotExists();
    ```

8. Fügen Sie der **main**-Methode den folgenden Code hinzu, um einen Verweis auf den Service Bus-Dienst abzurufen:

    ```
    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    EventProcessor.serviceBusContract = ServiceBusService.create(config);
    ```

9. Erstellen Sie in der **main**-Methode eine **EventProcessorHost**-Instanz, und konfigurieren Sie sie. Die **setInvokeProcessorAfterReceiveTimeout**-Option stellt sicher, dass die **EventProcessorHost**-Instanz die **onEvents**-Methode in der **IEventProcessor**-Schnittstelle auch dann aufruft, wenn keine Nachrichten zur Verarbeitung vorhanden sind. Die **onEvents** Methode ruft immer dann die **AppendAndCheckPoint**-Methode auf, wenn das Timeout abläuft.

    ```
    ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(
        namespaceName, eventHubName, sasKeyName, sasKey);
    EventProcessorHost host = new EventProcessorHost(eventHubName,
        consumerGroupName, eventHubConnectionString.toString(),
        storageConnectionString);
    EventProcessorOptions options = new EventProcessorOptions();
    options.setExceptionNotification(new ErrorNotificationHandler());
    options.setInvokeProcessorAfterReceiveTimeout(true);
    ```

10. Registrieren Sie in der **main**-Methode die **IEventProcessor**-Implementierung mit der **EventProcessorHost**-Instanz:

    ```
    try {
      System.out.println("Registering host named " + host.getHostName());
      host.registerEventProcessor(EventProcessor.class, options).get();
    } catch (Exception e) {
      System.out.print("Failure while registering: ");
      if (e instanceof ExecutionException) {
        Throwable inner = e.getCause();
        System.out.println(inner.toString());
      } else {
        System.out.println(e.toString());
      }
      System.out.println(e.toString());
    }
    ```

11. Fügen Sie schließlich der **main**-Methode Code zum Beenden der **EventProcessorHost**-Instanz hinzu:

    ```
    System.out.println("Press enter to stop");
    try {
      System.in.read();
      host.unregisterEventProcessor();

      System.out.println("Calling forceExecutorShutdown");
      EventProcessorHost.forceExecutorShutdown(120);
    } catch (Exception e) {
      System.out.println(e.toString());
      e.printStackTrace();
    }

    System.out.println("End of sample");
    ```

12. Speichern und schließen Sie die Datei „process-d2c-messages\\src\\main\\java\\com\\mycompany\\app\\App.java“.

13. Führen Sie zum Erstellen der Anwendung **process-d2c-messages** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „process-d2c-messages“ aus:

    ```
    mvn clean package -DskipTests
    ```

## Empfangen von interaktiven Nachrichten

In diesem Abschnitt schreiben Sie eine Java-Konsolen-App, die interaktive Nachrichten aus der Service Bus-Warteschlange empfängt.

Die erste Aufgabe ist, ein Maven-Projekt namens **process-interactive-messages** hinzuzufügen, das Nachrichten empfängt, die von den **EventProcessor**-Instanzen über die Service Bus-Warteschlange gesendet werden.

1. Erstellen Sie im Ordner „iot-java-get-started“, den Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, ein neues Maven-Projekt namens **process-interactive-messages**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=process-interactive-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Eingabeaufforderung zum neuen Ordner „process-interactive-messages“.

3. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „process-interactive-messages“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Mit dieser Abhängigkeit können Sie das Azure Service Bus-Paket in Ihrer Anwendung zur Interaktion mit Ihrer Service Bus-Warteschlange verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-servicebus</artifactId>
      <version>0.9.4</version>
    </dependency>
    ```

4. Speichern und schließen Sie die Datei „pom.xml“.

Die nächste Aufgabe ist, Code zum Abrufen von Nachrichten aus der Service Bus-Warteschlange hinzuzufügen.

1. Öffnen Sie die Datei „process-interactive-messages\\src\\main\\java\\com\\mycompany\\app\\App.java“ mit einem Text-Editor.

2. Fügen Sie der Datei die folgenden `import`-Anweisungen hinzu:

    ```
    import java.io.IOException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;

    import com.microsoft.windowsazure.Configuration;
    import com.microsoft.windowsazure.exception.ServiceException;
    import com.microsoft.windowsazure.services.servicebus.*;
    import com.microsoft.windowsazure.services.servicebus.models.*;
    ```

3. Fügen Sie der **App**-Klasse die folgenden Variablen auf Klassenebene hinzu, und ersetzen Sie **{yourservicebusnamespace}** mit Ihrem Service Bus-Namespace, und **{yourservicebuslistenkey}** mit dem **listen**-Schlüssel Ihrer Warteschlange. Sie haben bereits im Abschnitt [Bereitstellen eines Azure-Speicherkontos und einer Service Bus-Warteschlange](#provision-an-azure-storage-account-and-a-service-bus-queue) Namespace und **listen**-Schlüsselwerte notiert:

    ```
    private final static String serviceBusNamespace = "{yourservicebusnamespace}";
    private final static String serviceBusSasKeyName = "listen";
    private final static String serviceBusSASKey = "{yourservicebuslistenkey}";
    private final static String serviceBusRootUri = ".servicebus.windows.net";
    private final static String queueName = "d2ctutorial";
    private static ServiceBusContract service = null;
    ```

4. Fügen Sie die folgende geschachtelte Klasse der **App** Klasse hinzu, um Nachrichten aus der Warteschlange zu empfangen:

    ```
    private static class MessageReceiver implements Runnable {
      public void run() {
        ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
        try {
          while (true) {
            ReceiveQueueMessageResult resultQM = service.receiveQueueMessage(
                queueName, opts);
            BrokeredMessage message = resultQM.getValue();
            if (message != null && message.getMessageId() != null) {
              System.out.println("MessageID: " + message.getMessageId());
              System.out.print("From queue: ");
              byte[] b = new byte[200];
              String s = null;
              int numRead = message.getBody().read(b);
              while (-1 != numRead) {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
              }
              System.out.println();
            } else {
              Thread.sleep(1000);
            }
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        } catch (ServiceException e) {
          System.out.println("ServiceException: " + e.getMessage());
        } catch (IOException e) {
          System.out.println("IOException: " + e.getMessage());
        }
      }
    }
    ```

5. Ändern Sie die Signatur der **main**-Methode wie folgt:

    ```
    public static void main(String args[]) throws ServiceException, IOException {
    }
    ```

6. Fügen Sie der **main**-Methode den folgenden Code zum Starten des Lauschens auf neue Nachrichten hinzu:

    ```
    System.out.println("Process interactive messages");

    Configuration config = ServiceBusConfiguration
        .configureWithSASAuthentication(serviceBusNamespace,
            serviceBusSasKeyName, serviceBusSASKey, serviceBusRootUri);
    service = ServiceBusService.create(config);

    MessageReceiver receiver = new MessageReceiver();

    ExecutorService executor = Executors.newFixedThreadPool(2);
    executor.execute(receiver);

    System.out.println("Press ENTER to exit.");
    System.in.read();
    executor.shutdownNow();
    ```

7. Speichern und schließen Sie die Datei „process-interactive-messages\\src\\main\\java\\com\\mycompany\\app\\App.java“.

8. Führen Sie zum Erstellen der Anwendung **process-interactive-messages** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „process-interactive-messages“ aus:

    ```
    mvn clean package -DskipTests
    ```

## Ausführen der Anwendungen

Sie können jetzt die drei Anwendungen ausführen.

1.	Um die **process-interactive-messages**-Anwendung auszuführen, navigieren Sie in einer Befehlszeile oder Shell zum Ordner „process-interactive-messages“, und geben Sie folgenden Befehl ein:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Ausführen von „process-interactive-messages“][processinteractive]

2.	Um die **process-d2c-messages**-Anwendung auszuführen, navigieren Sie in einer Befehlszeile oder Shell zum Ordner „process-d2c-messages“, und geben Sie folgenden Befehl ein:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Ausführen von „process-d2c-messages“][processd2c]

3.	Um die **simulated-device**-Anwendung auszuführen, navigieren Sie in einer Befehlszeile oder Shell zum Ordner „simulated-device“, und geben Sie folgenden Befehl ein:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Ausführen von „simulated-device“][simulateddevice]

> [AZURE.NOTE] Um Aktualisierungen in Ihrer Blobdatei sehen zu können, müssen Sie unter Umständen für die Konstante **MAX\_BLOCK\_SIZE** in der **StoreEventProcessor**-Klasse einen niedrigeren Wert festlegen (beispielsweise **1.024**). Diese Änderung ist nützlich, weil es bei den mit dem simulierten Gerät gesendeten Daten einige Zeit dauert, bis die Blockgröße erreicht wird. Bei einer kleineren Blockgröße müssen Sie nicht so lange warten, bis das Blob erstellt und aktualisiert wird. Allerdings sorgt die Verwendung einer höheren Blockgröße für eine bessere Skalierbarkeit der Anwendung.

## Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie mit der [EventProcessorHost]-Klasse zuverlässig Datenpunktnachrichten und interaktive D2C-Nachrichten verarbeiten.

Im Tutorial [Senden von Cloud-zu-Gerät-Nachrichten mithilfe von IoT Hub][lnk-c2d] erfahren Sie, wie Sie Nachrichten aus Ihrem Back-End an Ihre Geräte senden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT Suite][lnk-suite].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [IoT Hub-Entwicklerhandbuch].

<!-- Images. -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[processinteractive]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[processd2c]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/createqueue2.png
[31]: ./media/iot-hub-java-java-process-d2c/createqueue3.png

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
[Erste Schritte mit IoT Hub]: iot-hub-java-java-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Informationen zu Azure Storage]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Erste Schritte mit Event Hubs]: ../event-hubs/event-hubs-java-ephjava-getstarted.md
[Richtlinien zur Azure Storage-Skalierbarkeit]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: https://github.com/Azure/azure-event-hubs/tree/master/java/azure-eventhubs-eph
[Event Hubs Programming Guide]: https://github.com/Azure/azure-event-hubs/blob/master/java/readme.md
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-java-java-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[lnk-create-an-iot-hub]: iot-hub-java-java-getstarted.md#create-an-iot-hub

<!---HONumber=AcomDC_0907_2016-->