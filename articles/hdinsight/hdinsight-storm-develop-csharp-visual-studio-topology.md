---
title: Apache Storm-Topologien mit Visual Studio und C# | Microsoft Docs
description: "Erfahren Sie, wie Sie Storm-Topologien in C# erstellen, indem Sie mit den HDInsight Tools für Visual Studio eine einfache Wortanzahl-Topologie in Visual Studio erstellen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 380d804f-a8c5-4b20-9762-593ec4da5a0d
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/13/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 1336f95809712779ca5c4c68237a3a8c6d630af4
ms.openlocfilehash: 337e1c185ef8989345ec3abb60f6b255dbc9ef4f


---
# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>Entwickeln von C#-Topologien für Apache Storm in HDInsight mithilfe von Hadoop-Tools für Visual Studio

Erfahren Sie, wie Sie eine C#-Storm-Topologie mithilfe der HDInsight-Tools für Visual Studio erstellen. In diesem Lernprogramm werden Sie durch die Erstellung eines neuen Storm-Projekts in Visual Studio, durch den zugehörigen lokalen Test und die Bereitstellung eines Apache Storm-Clusters in HDInsight geleitet.

Außerdem erfahren Sie, wie hybride Topologien erstellt werden, die C#- und Java-Komponenten verwenden.

> [!IMPORTANT]
> Während Sie für die Schritte in diesem Dokument eine Windows-Entwicklungsumgebung mit Visual Studio benötigen, kann das kompilierte Projekt an einen Linux- oder Windows-basierten HDInsight-Cluster übermittelt werden. __Nur Linux-basierte Cluster, die nach dem 28.10.2016 erstellt wurden, unterstützen SCP.NET-Topologien__.
> 
> Um eine C#-Topologie mit einem Linux-basierten Cluster zu verwenden, müssen Sie das NuGet-Paket „Microsoft.SCP.Net.SDK“, das vom Projekt verwendet wird, auf Version 0.10.0.6 oder höher aktualisieren. Die Version des Pakets muss zudem mit der Hauptversion der Storm-Installation auf HDInsight übereinstimmen. Beispielsweise verwenden die Storm in HDInsight-Versionen 3.3 und 3.4 die Storm-Version 0.10.x, während HDInsight 3.5 Storm 1.0.x verwendet.
> 
> C#-Topologien in Linux-basierten Clustern müssen .NET 4.5 verwenden. Zudem muss Mono im HDInsight-Cluster ausgeführt werden. Das meiste ist funktionsfähig, Sie sollten jedoch das Dokument zur [Mono-Kompatibilität](http://www.mono-project.com/docs/about-mono/compatibility/) auf mögliche Inkompatibilitäten überprüfen.


## <a name="prerequisites"></a>Voraussetzungen

* [Java](https://java.com) 1.7 oder höher in Ihrer Entwicklungsumgebung. Java wird verwendet, um die Topologie zu verpacken, wenn sie an den HDInsight-Cluster übermittelt wird.

  * Die **JAVA_HOME**-Umgebungsvariable muss auf das Verzeichnis mit Java zeigen.
  * Das **%JAVA_HOME%/bin**-Verzeichnis muss im Pfad liegen.

* Eine der folgenden Versionen von Visual Studio:
  
  * Visual Studio 2012 mit [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)
  * Visual Studio 2013 mit [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) oder [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)
  * Visual Studio 2015 oder [Visual Studio 2015 Community](https://go.microsoft.com/fwlink/?LinkId=532606)

* Azure SDK 2.9.5 oder höher

* HDInsight-Tools für Visual Studio: Informationen zum Installieren und Konfigurieren der HDInsight-Tools für Visual Studio finden Sie unter [Erste Schritte mit den HDInsight-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) .
  
  > [!NOTE]
  > HDInsight-Tools für Visual Studio werden von Visual Studio Express nicht unterstützt

* Apache Storm in HDInsight-Cluster: Informationen zum Erstellen eines Clusters finden Sie unter [Erste Schritte mit Apache Storm in HDInsight](hdinsight-apache-storm-tutorial-get-started.md) .

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

## <a name="templates"></a>Vorlagen

Die HDInsight-Tools für Visual Studio umfassen die folgenden Vorlagen:

| Projekttyp | Zeigt |
| --- | --- |
| Storm-Anwendung |Leeres Projekt der Storm-Topologie |
| Storm Azure SQL Writer-Beispiel |Gewusst wie: Schreiben in eine Azure SQL-Datenbank |
| Storm DocumentDB Reader-Beispiel |Gewusst wie: Lesen aus der Azure DocumentDB |
| Storm DocumentDB Writer-Beispiel |Gewusst wie: Schreiben in Azure DocumentDB |
| Storm EventHub Reader-Beispiel |Gewusst wie: Lesen aus Azure Event Hubs |
| Storm EventHub Writer-Beispiel |Gewusst wie: Schreiben in Azure Event Hubs |
| Storm HBase Reader-Beispiel |Gewusst wie: Lesen aus HBase auf HDInsight-Clustern |
| Storm HBase Writer-Beispiel |Gewusst wie: Schreiben in HBase auf HDInsight-Clustern |
| Storm-Hybrid-Beispiel |Gewusst wie: Verwenden einer Java-Komponente |
| Storm-Beispiel |Einfache Wortzählungstopologie |

In den Schritten dieses Dokuments verwenden Sie das grundlegende Storm-Anwendungsprojekt, um eine neue Topologie zu erstellen.

### <a name="hbase-templates-notes"></a>Hinweise zu HBase-Vorlagen

In den HBase Reader- und Writer-Vorlagen wird die HBase-REST-API verwendet, um mit HBase auf einem HDInsight-Cluster zu kommunizieren, nicht die HBase-Java-API.

### <a name="eventhub-templates-notes"></a>Hinweise zu EventHub-Vorlagen

> [!IMPORTANT]
> Die Java-basierte EventHub-Spout-Komponente, die in der EventHub-Reader-Vorlage enthalten ist, funktioniert nicht mit Storm auf HDInsight Version 3.5. Verwenden Sie stattdessen die EventHub-Spout-Komponente von [https://000aarperiscus.blob.core.windows.net/certs/storm-eventhubs-1.0.2-jar-with-dependencies.jar](https://000aarperiscus.blob.core.windows.net/certs/storm-eventhubs-1.0.2-jar-with-dependencies.jar).

Eine Beispieltopologie, die diese Komponente verwendet und mit Storm auf HDInsight 3.5 funktioniert, finden Sie unter [https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).

## <a name="create-a-c-topology"></a>Erstellen einer C#-Topologie

1. Wenn Sie die neueste Version der HDInsight-Tools für Visual Studio noch nicht installiert haben, finden Sie Informationen dazu unter [Erste Schritte mit den HDInsight-Tools für Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).

2. Öffnen Sie Visual Studio, wählen Sie **Datei** > **Neu** aus, und klicken Sie dann auf **Projekt**.

3. Erweitern Sie auf dem Bildschirm **Neues Projekt** die Option **Installiert** > **Vorlagen**, und wählen Sie dann **HDInsight** aus. Wählen Sie in der Liste der Vorlagen die Option **Storm-Anwendung**aus. Geben Sie unten im Bildschirm **WordCount** als Namen der Anwendung ein.
   
    ![image](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. Nachdem das Projekt erstellt wurde, sollten die folgenden Dateien vorliegen:
   
   * **Program.cs:**definiert die Topologie für Ihr Projekt. Beachten Sie, dass standardmäßig eine Topologie erstellt wird, die aus einem Spout und einem Bolt besteht.

   * **Spout.cs:**ein Beispiel-Spout, der Zufallszahlen ausgibt.

   * **Bolt.cs:**ein Beispiel-Bolt, der eine Anzahl der vom Spout ausgegebenen Zahlen aufnimmt.
     
     Im Rahmen der Projekterstellung werden die neuesten [SCP.NET-Pakete](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/) von NuGet heruntergeladen.
     
     [!INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

In den nächsten Abschnitten verwandeln Sie dieses Projekt in eine einfache Anwendung zur Wortzählung.

### <a name="implement-the-spout"></a>Implementieren des Spouts

1. Öffnen Sie **Spout.cs**. Mithilfe der Spouts werden Daten aus einer externen Quelle in eine Topologie eingelesen. Wichtigste Komponenten für einen Spout:
   
   * **NextTuple:**wird von Storm aufgerufen, wenn der Spout neue Tupel ausgeben darf.

   * **Ack** (nur transaktionale Topologie): verarbeitet Bestätigungen, die von anderen Komponenten in der Topologie initiiert wurden, für Tupel, die von diesem Spout gesendet wurden. Durch die Tupelbestätigung erfährt der Spout, dass die Verarbeitung durch nachgelagerte Komponenten erfolgreich ausgeführt wurde.

   * **Fail** (nur transaktionale Topologie): verarbeitet Tupel, bei denen bei der Verarbeitung anderer Komponenten in der Topologie ein Fehler aufgetreten ist. Dies bietet die Möglichkeit, das Tupel neu auszugeben, damit es erneut verarbeitet werden kann.

2. Ersetzen Sie den Inhalt der **Spout** -Klasse durch Folgendes. Dadurch wird ein Spout erstellt, der einen Satz willkürlich in die Topologie ausgibt.
    
    ```csharp
    private Context ctx;
    private Random r = new Random();
    string[] sentences = new string[] {
        "the cow jumped over the moon",
        "an apple a day keeps the doctor away",
        "four score and seven years ago",
        "snow white and the seven dwarfs",
        "i am at two with nature"
    };

    public Spout(Context ctx)
    {
        // Set the instance context
        this.ctx = ctx;

        Context.Logger.Info("Generator constructor called");

        // Declare Output schema
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // The schema for the default output stream is
        // a tuple that contains a string field
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
    }

    // Get an instance of the spout
    public static Spout Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Spout(ctx);
    }

    public void NextTuple(Dictionary<string, Object> parms)
    {
        Context.Logger.Info("NextTuple enter");
        // The sentence to be emitted
        string sentence;

        // Get a random sentence
        sentence = sentences[r.Next(0, sentences.Length - 1)];
        Context.Logger.Info("Emit: {0}", sentence);
        // Emit it
        this.ctx.Emit(new Values(sentence));

        Context.Logger.Info("NextTuple exit");
    }

    public void Ack(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }

    public void Fail(long seqId, Dictionary<string, Object> parms)
    {
        // Only used for transactional topologies
    }
    ```
   
    Lesen Sie die Kommentare in Ruhe durch, um die Funktionsweise dieses Codes zu verstehen.

### <a name="implement-the-bolts"></a>Implementieren der Bolts

1. Löschen Sie die vorhandene Datei **Bolt.cs** aus dem Projekt.

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Hinzufügen** > **Neues Element** aus. Wählen Sie in der Liste die Option **Storm Bolt** aus, und geben Sie **Splitter.cs** als Namen ein. Wiederholen Sie diesen Vorgang, um einen zweiten Bolt namens **Counter.cs**zu erstellen.
   
   * **Splitter.cs:**implementiert einen Bolt, der Sätze in einzelne Wörter unterteilt und einen neuen Datenstrom von Wörtern ausgibt.

   * **Counter.cs:**implementiert einen Bolt, der jedes Wort zählt und einen neuen Datenstrom von Wörtern sowie die Anzahl der einzelnen Wörter ausgibt.
     
     > [!NOTE]
     > Obwohl diese Bolts einfach in Datenströme schreiben und aus diesen lesen, können Sie mithilfe eines Bolts auch mit einer Datenbank oder einem Dienst kommunizieren.

3. Öffnen Sie **Splitter.cs**. Beachten Sie, dass die Datei standardmäßig nur eine Methode enthält: **Execute**. Diese wird aufgerufen, wenn das Bolt ein zu verarbeitendes Tupel empfängt. Hier können Sie eingehende Tupel lesen und verarbeiten sowie ausgehende Tupel ausgeben.

4. Ersetzen Sie den Inhalt der **Splitter** -Klasse durch folgenden Code:
    
    ```csharp
    private Context ctx;

    // Constructor
    public Splitter(Context ctx)
    {
        Context.Logger.Info("Splitter constructor called");
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // Input contains a tuple with a string field (the sentence)
        inputSchema.Add("default", new List<Type>() { typeof(string) });
        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // Outbound contains a tuple with a string field (the word)
        outputSchema.Add("default", new List<Type>() { typeof(string) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance of the bolt
    public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Splitter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the sentence from the tuple
        string sentence = tuple.GetString(0);
        // Split at space characters
        foreach (string word in sentence.Split(' '))
        {
            Context.Logger.Info("Emit: {0}", word);
            //Emit each word
            this.ctx.Emit(new Values(word));
        }

        Context.Logger.Info("Execute exit");
    }
    ```
   
    Lesen Sie die Kommentare in Ruhe durch, um die Funktionsweise dieses Codes zu verstehen.

5. Öffnen Sie **Counter.cs** , und ersetzen Sie die Klasseninhalte durch Folgendes:
    
    ```csharp
    private Context ctx;

    // Dictionary for holding words and counts
    private Dictionary<string, int> counts = new Dictionary<string, int>();

    // Constructor
    public Counter(Context ctx)
    {
        Context.Logger.Info("Counter constructor called");
        // Set instance context
        this.ctx = ctx;

        // Declare Input and Output schemas
        Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string field - the word
        inputSchema.Add("default", new List<Type>() { typeof(string) });

        Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
        // A tuple containing a string and integer field - the word and the word count
        outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
        this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
    }

    // Get a new instance
    public static Counter Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new Counter(ctx);
    }

    // Called when a new tuple is available
    public void Execute(SCPTuple tuple)
    {
        Context.Logger.Info("Execute enter");

        // Get the word from the tuple
        string word = tuple.GetString(0);
        // Do we already have an entry for the word in the dictionary?
        // If no, create one with a count of 0
        int count = counts.ContainsKey(word) ? counts[word] : 0;
        // Increment the count
        count++;
        // Update the count in the dictionary
        counts[word] = count;

        Context.Logger.Info("Emit: {0}, count: {1}", word, count);
        // Emit the word and count information
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
        Context.Logger.Info("Execute exit");
    }
    ```
   
    Lesen Sie die Kommentare in Ruhe durch, um die Funktionsweise dieses Codes zu verstehen.

### <a name="define-the-topology"></a>Definieren der Topologie

Spouts und Bolts werden in einem Diagramm angeordnet, das definiert, wie die Daten zwischen den Komponenten verlaufen. Bei dieser Topologie sieht das Diagramm folgendermaßen aus:

![Bild der Anordnung von Komponenten](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

Sätze werden vom Spout ausgegeben, die an Instanzen des Splitter-Bolts verteilt werden. Das Splitter-Bolt unterteilt Sätze in Wörter, die an das Counter-Bolt verteilt werden.

Da die Wortanzahl lokal in der Counter-Instanz gespeichert wird, möchten wir sicherstellen, dass bestimmte Wörter an dieselbe Instanz des Counter-Bolts geleitet werden, damit ein bestimmtes Wort nur von einer Instanz verfolgt wird. Für das Splitter-Bolt spielt es keine Rolle, welcher Bolt welchen Satz empfängt, daher werden die Sätze einfach auf diese Instanzen verteilt, um einen Lastenausgleich zu erreichen.

Öffnen Sie die Datei **Program.cs**. Die entscheidende Methode ist **GetTopologyBuilder**, mit der die an Storm gesendete Topologie definiert wird. Ersetzen Sie den Inhalt von **GetTopologyBuilder** durch Folgendes, um die zuvor beschriebene Topologie zu implementieren:

```csharp
// Create a new topology named 'WordCount'
TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

// Add the spout to the topology.
// Name the component 'sentences'
// Name the field that is emitted as 'sentence'
topologyBuilder.SetSpout(
    "sentences",
    Spout.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
    },
    1);
// Add the splitter bolt to the topology.
// Name the component 'splitter'
// Name the field that is emitted 'word'
// Use suffleGrouping to distribute incoming tuples
//   from the 'sentences' spout across instances
//   of the splitter
topologyBuilder.SetBolt(
    "splitter",
    Splitter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
    },
    1).shuffleGrouping("sentences");

// Add the counter bolt to the topology.
// Name the component 'counter'
// Name the fields that are emitted 'word' and 'count'
// Use fieldsGrouping to ensure that tuples are routed
//   to counter instances based on the contents of field
//   position 0 (the word). This could also have been
//   List<string>(){"word"}.
//   This ensures that the word 'jumped', for example, will always
//   go to the same instance
topologyBuilder.SetBolt(
    "counter",
    Counter.Get,
    new Dictionary<string, List<string>>()
    {
        {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
    },
    1).fieldsGrouping("splitter", new List<int>() { 0 });

// Add topology config
topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
{
    {"topology.kryo.register","[\"[B\"]"}
});

return topologyBuilder;
```

Lesen Sie die Kommentare in Ruhe durch, um die Funktionsweise dieses Codes zu verstehen.

## <a name="submit-the-topology"></a>Übermitteln der Topologie

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **An Storm in HDInsight übermitteln** aus.
   
   > [!NOTE]
   > Wenn Sie dazu aufgefordert werden, geben Sie die Anmeldeinformationen für Ihr Azure-Abonnement ein. Wenn Sie über mehrere Abonnements verfügen, melden Sie sich bei dem Abonnement an, das Ihren Storm-Cluster in HDInsight enthält.

2. Wählen Sie in der Dropdownliste **Storm Cluster** Ihren Storm-Cluster in HDInsight und dann die Option **Übermitteln** aus. Sie können über das Fenster **Ausgabe** überwachen, ob die Übermittlung erfolgreich ausgeführt wurde.

3. Sobald die Topologie erfolgreich übermittelt wurde, sollten die **Storm-Topologien** für den Cluster angezeigt werden. Wählen Sie die **WordCount** -Topologie in der Liste aus, um Informationen zur aktiven Topologie anzuzeigen.
   
   > [!NOTE]
   > Sie können die **Storm-Topologien** auch im **Server-Explorer** anzeigen, indem Sie **Azure** > **HDInsight** erweitern und dann mit der rechten Maustaste auf einen Storm in HDInsight-Cluster klicken und **Storm-Topologien anzeigen** auswählen.

    Verwenden Sie die Links für die Spouts oder Bolts, um Informationen über diese Komponenten anzuzeigen. Für jedes ausgewählte Element wird ein neues Fenster geöffnet.

4. Wählen Sie in der Ansicht **Topologiezusammenfassung** die Option **Beenden** aus, um die Topologie zu beenden.
   
   > [!NOTE]
   > Storm-Topologien werden weiterhin ausgeführt, bis sie deaktiviert werden oder der Cluster gelöscht wird.

## <a name="transactional-topology"></a>Transaktionale Topologie

Die vorherige Topologie ist nicht transaktional. Die Komponenten in der Topologie implementieren keine Funktionen für die Wiedergabe von Nachrichten, wenn bei der Verarbeitung durch eine Komponente in der Topologie ein Fehler auftritt. Erstellen Sie für eine transaktionale Beispieltopologie ein neues Projekt, und wählen Sie **Storm-Beispiel** als Projekttyp aus.

Transaktionale Topologien implementieren Folgendes, um die Wiedergabe von Daten zu unterstützen:

* **Metadaten-Zwischenspeicherung:**Der Spout muss Metadaten zu den ausgegebenen Daten speichern, damit die Daten bei einem auftretenden Fehler erneut abgerufen und ausgegeben werden können. Da die in diesem Beispiel ausgegebene Datenmenge klein ist, werden die Rohdaten der einzelnen Tupel zur Wiedergabe in einem Wörterbuch gespeichert.

* **Ack**: Jeder Bolt in der Topologie kann `this.ctx.Ack(tuple)` aufrufen, um die erfolgreiche Verarbeitung eines Tupels zu bestätigen. Nachdem das Tupel von allen Bolts bestätigt wurde, wird die `Ack` -Methode des Spouts aufgerufen. Dadurch kann der Spout zur Wiedergabe zwischengespeicherte Daten entfernen, da die Daten vollständig verarbeitet wurden.

* **Fail**: Jeder Bolt kann `this.ctx.Fail(tuple)` aufrufen, um anzuzeigen, dass bei der Verarbeitung eines Tupels ein Fehler aufgetreten ist. Der Fehler wird an die `Fail` -Methode des Spouts weitergegeben, wo das Tupel mithilfe der zwischengespeicherten Daten wiedergegeben werden kann.

* **Sequence ID**: Beim Ausgeben eines Tupels kann eine Sequenz-ID angegeben werden. Hierbei sollte es sich um einen Wert handeln, der das Tupel für die Wiedergabeverarbeitung ("Ack" und "Fail") kennzeichnet. Der Spout im Projekt **Storm-Beispiel** verwendet z.B. Folgendes beim Ausgeben von Daten:
  
        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);
  
    Hierbei wird ein neues Tupel, das einen Satz enthält, im Standarddatenstrom ausgegeben, wobei der Sequenz-ID-Wert in **lastSeqId**enthalten ist. In diesem Beispiel wird **lastSeqId** für jedes ausgegebene Tupel einfach erhöht.

Wie im Projekt **Storm-Beispiel** veranschaulicht, kann zur Laufzeit auf Basis der Konfiguration angegeben werden, ob es sich um eine transaktionale Komponente handelt.

## <a name="hybrid-topology"></a>Hybride Topologie

HDInsight-Tools für Visual Studio können auch zum Erstellen von hybriden Topologien verwendet werden, bei denen einige Komponenten aus C#- und andere aus Java-Code bestehen.

Erstellen Sie für eine hybride Beispieltopologie ein neues Projekt, und wählen Sie **Storm-Hybridbeispiel** aus. Dadurch wird eine vollständig kommentierte Beispieldatei mit mehreren Topologien erstellt, die Folgendes veranschaulichen:

* **Java-Spout** und **C#-Bolt**: Definition in **HybridTopology_javaSpout_csharpBolt**
  
    * Eine transaktionale Version ist in **HybridTopologyTx_javaSpout_csharpBolt** definiert.

* **C#-Spout** und **Java-Bolt**: Definition in **HybridTopology_csharpSpout_javaBolt**
  
    * Eine transaktionale Version ist in **HybridTopologyTx_csharpSpout_javaBolt** definiert.
  
  > [!NOTE]
  > Diese Version veranschaulicht zudem, wie Sie Clojure-Code aus einer Textdatei als Java-Komponente verwenden.


Um zwischen den Topologien zu wechseln, die beim Übermitteln des Projekts verwendet werden, verschieben Sie einfach vor der Übermittlung an den Cluster die `[Active(true)]` -Anweisung in die Topologie, die Sie verwenden möchten.

> [!NOTE]
> Alle erforderliche Java-Dateien werden im Rahmen dieses Projekts im Ordner **JavaDependency** bereitgestellt.


Bedenken Sie beim Erstellen und Übermitteln einer hybriden Topologie Folgendes:

* **JavaComponentConstructor** muss verwendet werden, um eine neue Instanz der Java-Klasse für einen Spout oder Bolt zu erstellen.

* **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer** muss zum Serialisieren von Daten in bzw. aus Java-Komponenten von Java-Objekten zu JSON verwendet werden.

* Beim Übermitteln der Topologie an den Server müssen Sie die Option **Zusätzliche Konfigurationen** verwenden, um die **Java-Dateipfade** anzugeben. Der angegebene Pfad muss sich in dem Verzeichnis befinden, das die JAR-Dateien mit den Java-Klassen enthält.

### <a name="azure-event-hubs"></a>Azure Event Hubs

SCP.Net Version 0.9.4.203 bietet eine neue Klasse und Methode speziell für die Arbeit mit dem Event Hub-Spout (ein Java-Spout, der von Event Hub liest). Verwenden Sie die folgenden Methoden beim Erstellen einer Topologie, die diesen Spout verwendet:

* **EventHubSpoutConfig**-Klasse: Erstellt ein Objekt, das die Konfiguration für die Spout-Komponente enthält.

* **TopologyBuilder.SetEventHubSpout**-Methode: Fügt die Event Hub-Spout-Komponente der Topologie hinzu.

> [!NOTE]
> Obwohl hierdurch die Arbeit mit dem Event Hub-Spout gegenüber anderen Java-Komponenten erleichtert wird, müssen Sie dennoch "CustomizedInteropJSONSerializer" zum Serialisieren der von dem Spout erzeugten Daten verwenden.

## <a name="a-idconfigurationmanagerausing-configurationmanager"></a><a id="configurationmanager"></a>Verwenden von ConfigurationManager

Verwenden Sie ConfigurationManager nicht zum Abrufen von Konfigurationswerten aus Bolt- und Spout-Komponenten. Dies kann zu einer NULL-Zeigerausnahme führen. Stattdessen wird die Konfiguration für das Projekt in der Storm-Topologie als Schlüssel-Wert-Paar im Topologiekontext übergeben. Jede Komponente, die von Konfigurationswerten abhängig ist, muss sie während der Initialisierung aus dem Kontext abrufen.

Der folgende Code veranschaulicht, wie diese Werte abgerufen werden:

```csharp
public class MyComponent : ISCPBolt
{
    // To hold configuration information loaded from context
    Configuration configuration;
    ...
    public MyComponent(Context ctx, Dictionary<string, Object> parms)
    {
        // Save a copy of the context for this component instance
        this.ctx = ctx;
        // If it exists, load the configuration for the component
        if(parms.ContainsKey(Constants.USER_CONFIG))
        {
            this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
        }
        // Retrieve the value of "Foo" from configuration
        var foo = this.configuration.AppSettings.Settings["Foo"].Value;
    }
    ...
}
```

Bei Verwendung einer `Get`-Methode zum Zurückgeben einer Instanz Ihrer Komponente müssen Sie sicherstellen, dass die beiden Parameter `Context` und `Dictionary<string, Object>` an den Konstruktor übergeben werden. Das folgende Beispiel ist eine einfache `Get`-Methode, die diese Werte ordnungsgemäß übergibt:

```csharp
public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
{
    return new MyComponent(ctx, parms);
}
```

## <a name="how-to-update-scpnet"></a>Gewusst wie: Aktualisieren von SCP.NET

Neuere Versionen von SCP.NET unterstützen die Paketaktualisierung über NuGet. Wenn ein neues Update verfügbar ist, erhalten Sie eine Upgradebenachrichtigung. Gehen Sie wie folgt vor, um eine Überprüfung auf ein Upgrade manuell durchzuführen:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

2. Wählen Sie im Paket-Manager die Option **Updates**. Wenn ein Update verfügbar ist, wird es aufgeführt. Klicken Sie für das Paket auf die Schaltfläche **Aktualisieren** , um es zu installieren.

> [!IMPORTANT]
> Wenn Ihr Projekt mit einer der früheren Versionen von SCP.NET erstellt wurde, bei denen NuGet nicht für Paketupdates verwendet wurde, müssen Sie zum Aktualisieren auf die neue Version die folgenden Schritte ausführen:
> 
> 1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
> 2. Suchen Sie über das Feld **Suche** nach **Microsoft.SCP.Net.SDK**, und fügen Sie es dem Projekt hinzu.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="null-pointer-exceptions"></a>NULL-Zeigerausnahmen

Wenn Sie eine C#-Topologie mit einem Linux-basierten HDInsight-Cluster verwenden, können Bolt- und Spout-Komponenten, die zum Lesen von Konfigurationseinstellungen zur Laufzeit ConfigurationManager nutzen, NULL-Zeigerausnahmen zurückgeben. Dies liegt daran, dass die Konfiguration für die geladene Domäne nicht aus der Assembly stammt, die das Projekt enthält.

Die Konfiguration für das Projekt wird als Schlüssel-Wert-Paar im Topologiekontext an die Storm-Topologie übergeben und kann vom Wörterbuchobjekt abgerufen werden, das an die Komponenten übergeben wird, wenn sie initialisiert werden.

Das folgende Beispiel zeigt das Laden der Konfigurationswerte aus dem Topologiekontext. Weitere Informationen finden Sie im Abschnitt [ConfigurationManager](#configurationmanager) dieses Dokuments.

### <a name="systemtypeloadexception"></a>System.TypeLoadException

Wenn Sie eine C#-Topologie mit einem Linux-basierten HDInsight-Cluster verwenden, kann der folgende Fehler auftreten:

    System.TypeLoadException: Failure has occurred while loading a type.

Dies geschieht normalerweise, wenn Sie eine Binärdatei verwenden, die nicht mit der Version von .NET kompatibel ist, die Mono unterstützt.

Bei Linux-basierten HDInsight-Clustern sollten Sie sicherstellen, dass das Projekt für .NET 4.5 kompilierte Binärdateien verwendet.

### <a name="test-a-topology-locally"></a>Lokales Testen einer Topologie

Obwohl es relativ einfach ist, eine Topologie in einem Cluster bereitzustellen, müssen Sie eine Topologie möglicherweise auch lokal testen. Gehen Sie wie folgt vor, um die Beispieltopologie in diesem Lernprogramm lokal in Ihrer Entwicklungsumgebung auszuführen und zu testen.

> [!WARNING]
> Lokale Tests funktionieren nur für einfache, ausschließlich in C# umgesetzte Topologien. Sie sollten lokale Tests nicht für hybride Topologien verwenden oder für Topologien, in denen mehrere Streams verwendet werden.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus. Wählen Sie in den Projekteigenschaften für **Ausgabetyp** die Option **Konsolenanwendung** aus.
   
    ![Ausgabetyp](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)
   
   > [!NOTE]
   > Denken Sie daran, für den **Ausgabetyp** später wieder **Klassenbibliothek** festzulegen, bevor die Topologie in einem Cluster bereitgestellt wird.

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Hinzufügen** > **Neues Element** aus. Wählen Sie **Klasse** aus, und geben Sie **LocalTest.cs** als Klassennamen ein. Klicken Sie abschließend auf **Hinzufügen**.

3. Öffnen Sie die Datei **LocalTest.cs**, und fügen Sie die folgende **using**-Anweisung am Anfang hinzu:
    
    ```csharp
    using Microsoft.SCP;
    ```

4. Fügen Sie den folgenden Inhalt in die **LocalTest** -Klasse ein.
    
    ```csharp
    // Drives the topology components
    public void RunTestCase()
    {
        // An empty dictionary for use when creating components
        Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

        #region Test the spout
        {
            Console.WriteLine("Starting spout");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext spoutCtx = LocalContext.Get();
            // Get a new instance of the spout, using the local context
            Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

            // Emit 10 tuples
            for (int i = 0; i < 10; i++)
            {
                sentences.NextTuple(emptyDictionary);
            }
            // Use LocalContext to persist the data stream to file
            spoutCtx.WriteMsgQueueToFile("sentences.txt");
            Console.WriteLine("Spout finished");
        }
        #endregion

        #region Test the splitter bolt
        {
            Console.WriteLine("Starting splitter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext splitterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);

            // Set the data stream to the data created by the spout
            splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                splitter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            splitterCtx.WriteMsgQueueToFile("splitter.txt");
            Console.WriteLine("Splitter bolt finished");
        }
        #endregion

        #region Test the counter bolt
        {
            Console.WriteLine("Starting counter bolt");
            // LocalContext is a local-mode context that can be used to initialize
            // components in the development environment.
            LocalContext counterCtx = LocalContext.Get();
            // Get a new instance of the bolt
            Counter counter = Counter.Get(counterCtx, emptyDictionary);

            // Set the data stream to the data created by splitter bolt
            counterCtx.ReadFromFileToMsgQueue("splitter.txt");
            // Get a batch of tuples from the stream
            List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
            // Process each tuple in the batch
            foreach (SCPTuple tuple in batch)
            {
                counter.Execute(tuple);
            }
            // Use LocalContext to persist the data stream to file
            counterCtx.WriteMsgQueueToFile("counter.txt");
            Console.WriteLine("Counter bolt finished");
        }
        #endregion
    }
    ```

    Nehmen Sie sich einen Moment Zeit, um die Codekommentare zu lesen. Dieser Code verwendet **LocalContext** , um die Komponenten in der Entwicklungsumgebung auszuführen, wobei der Datenstrom zwischen den Komponenten in Textdateien auf dem lokalen Laufwerk bestehen bleibt.

1. Öffnen Sie die Datei **Program.cs**, und fügen Sie der Methode **Main** Folgendes hinzu:
    
    ```csharp
    Console.WriteLine("Starting tests");
    System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
    // Initialize the runtime
    SCPRuntime.Initialize();

    //If we are not running under the local context, throw an error
    if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
    {
        throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
    }
    // Create test instance
    LocalTest tests = new LocalTest();
    // Run tests
    tests.RunTestCase();
    Console.WriteLine("Tests finished");
    Console.ReadKey();
    ```

2. Speichern Sie die Änderungen, und verwenden Sie dann **F5** oder **Debuggen** > **Debugging starten**, um das Projekt zu starten. Daraufhin sollte ein Konsolenfenster angezeigt und mit fortschreitendem Test der Status protokolliert werden. Nachdem **Tests abgeschlossen** angezeigt wurde, drücken Sie eine beliebige Taste, um das Fenster zu schließen.

3. Suchen Sie mithilfe von **Windows-Explorer** das Verzeichnis, in dem Ihr Projekt gespeichert ist, z.B. **C:\Users\\<Benutzername>\Documents\Visual Studio 2013\Projects\WordCount\WordCount**. Öffnen Sie in diesem Verzeichnis das Unterverzeichnis **Bin**, und klicken Sie dann auf **Debug**. Es sollten die beim Ausführen der Tests erstellten Textdateien vorhanden sein: "sentences.txt", "counter.txt" und "splitter.txt". Öffnen Sie die einzelnen Textdateien, und überprüfen Sie die Daten.
   
   > [!NOTE]
   > Zeichenfolgendaten werden in diesen Dateien als Array von Dezimalwerten beibehalten. Beispiel: \[[97,103,111]] in der Datei **splitter.txt** entspricht dem Wort „and“.


Während das lokale Testen einer einfachen Anwendung zur Wortzählung relativ einfach erscheint, zeigt sich der wahre Wert erst bei komplexen Topologien, die mit externen Datenquellen kommunizieren oder komplexe Datenanalysen durchführen. Wenn Sie an einem solchen Projekt arbeiten, müssen Sie möglicherweise Haltepunkte festlegen und den Code in Ihren Komponenten schrittweise durchlaufen, um Problembereiche zu isolieren.

> [!NOTE]
> Vergewissern Sie sich, dass Sie für den **Projekttyp** vor der Bereitstellung in einem Storm in HDInsight-Cluster wieder **Klassenbibliothek** festlegen.

### <a name="log-information"></a>Protokollieren von Informationen

Sie können Informationen aus den Topologiekomponenten problemlos mithilfe von `Context.Logger` protokollieren. Das folgende Beispiel erzeugt z.B. einen informativen Protokolleintrag:

```csharp
Context.Logger.Info("Component started");
```

Protokollierte Informationen können über das **Hadoop-Dienstprotokoll** angezeigt werden, das sich im **Server-Explorer** befindet. Erweitern Sie den Eintrag für Ihren Storm-Cluster in HDInsight, und erweitern Sie dann das **Hadoop-Dienstprotokoll**. Wählen Sie abschließend die anzuzeigende Protokolldatei aus.

> [!NOTE]
> Die Protokolle werden in dem Azure-Speicherkonto gespeichert, das von Ihrem Cluster verwendet wird. Wenn es sich hierbei um ein anderes Abonnement als das handelt, mit dem Sie sich bei Visual Studio angemeldet haben, müssen Sie sich bei dem Abonnement anmelden, das dieses Speicherkonto enthält, um die entsprechenden Informationen anzeigen zu können.

### <a name="view-error-information"></a>Anzeigen von Fehlerinformationen

Gehen Sie wie folgt vor, um die Fehler anzuzeigen, die in einer aktiven Topologie aufgetreten sind:

1. Klicken Sie im **Server-Explorer** mit der rechten Maustaste auf den Storm in HDInsight-Cluster, und wählen Sie **Storm-Topologien anzeigen** aus.

2. Für die **Spouts** und **Bolts** enthält die Spalte **Letzter Fehler** die Informationen zum zuletzt aufgetretenen Fehler.

3. Wählen Sie die **Spout-ID** oder **Bolt-ID** für die Komponente aus, für die ein Fehler aufgeführt ist. Auf der angezeigten Detailseite werden am unteren Rand zusätzliche Fehlerinformationen im Abschnitt **Fehler** aufgeführt.

4. Um weitere Informationen zu erhalten, wählen Sie einen **Port** im Abschnitt **Ausführer** aus, um das Storm-Arbeitsprotokoll für die letzten Minuten anzuzeigen.

### <a name="errors-submitting-topologies"></a>Fehler beim Übermitteln von Topologien

Wenn beim Übermitteln einer Topologie an HDInsight Fehler auftreten, stehen Ihnen Protokolle für die serverseitigen Komponenten zur Verfügung, die die Übermittlung von Topologien auf Ihren HDInsight-Cluster behandeln. Verwenden Sie zum Abrufen dieser Protokolle über die Befehlszeile den folgenden Befehl:

    scp sshuser@clustername-ssh.azurehdinsight.net:/var/log/hdinsight-scpwebapi/hdinsight-scpwebapi.out .

Ersetzen Sie __sshuser__ durch das SSH-Benutzerkonto für den Cluster. Ersetzen Sie __clustername__ durch den Namen des HDInsight-Clusters. Wenn Sie ein Kennwort für das SSH-Konto verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Der Befehl lädt die Datei in das Verzeichnis herunter, aus dem heraus der Befehl ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie Storm-Topologien mithilfe der HDInsight-Tools für Visual Studio entwickeln und bereitstellen, lesen Sie jetzt Informationen zum [Verarbeiten von Ereignissen aus Azure Event Hub mit Storm in HDInsight](hdinsight-storm-develop-csharp-event-hub-topology.md).

Ein Beispiel einer C#-Topologie, die Streamdaten in mehrere Datenströme aufteilt, finden Sie unter [C# Storm example](https://github.com/Blackmist/csharp-storm-example)(in englischer Sprache).

Weitere Informationen zum Erstellen von C#-Topologien finden Sie unter [SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md).

Weitere Möglichkeiten zum Arbeiten mit HDInsight sowie weitere Beispiele für Storm in HDInsight finden Sie in folgenden Themen:

**Microsoft SCP.NET**

* [SCP-Programmierleitfaden](hdinsight-storm-scp-programming-guide.md)

**Apache Storm in HDInsight**

* [Bereitstellen und Überwachen von Topologien mit Apache Storm in HDInsight](hdinsight-storm-deploy-monitor-topology.md)
* [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)

**Apache Hadoop in HDInsight**

* [Verwenden von Hive mit Hadoop in HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit Hadoop in HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit Hadoop in HDInsight](hdinsight-use-mapreduce.md)

**Apache HBase in HDInsight**

* [Erste Schritte mit HBase in HDInsight](hdinsight-hbase-tutorial-get-started.md)




<!--HONumber=Feb17_HO2-->


