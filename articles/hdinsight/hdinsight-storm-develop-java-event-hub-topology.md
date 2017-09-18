---
title: Verarbeiten von Ereignissen aus Event Hubs mit Storm in HDInsight mit Java | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Daten von Event Hubs mit einer mit Maven erstellten Java Storm-Topologie erstellen.
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 453fa7b0-c8a6-413e-8747-3ac3b71bed86
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/13/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: db278f2ecd025257a969e3a9f05f5269a659999d
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a>Verarbeiten von Ereignissen aus Azure Event Hubs mit Storm in HDInsight (Java)

Erfahren Sie, wie Sie Azure Event Hub-Instanzen mit Storm in HDInsight verwenden. In diesem Beispiel werden Java-basierte Komponenten zum Lesen und Schreiben von Daten in Azure Event Hub-Instanzen verwendet.

Azure Event Hubs ermöglichen Ihnen die Verarbeitung sehr großer Datenmengen aus Websites, Apps und Geräten. Der Event Hub-Spout vereinfacht die Verwendung von Apache Storm in HDInsight, um diese Daten in Echtzeit zu analysieren. Sie können auch Daten aus Storm mithilfe des Event Hubs-Bolts in Event Hub schreiben.

## <a name="prerequisites"></a>Voraussetzungen

* Apache Storm auf HDInsight-Clusterversion 3.6. Weitere Informationen finden Sie unter [Erste Schritte mit Apache Storm in HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).

    > [!IMPORTANT]
    > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Einen [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

* [Oracle Java Developer Kit (JDK) Version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) oder Äquivalent, z.B. [OpenJDK](http://openjdk.java.net/).

* [Maven](https://maven.apache.org/download.cgi) – Maven ist ein Projekterstellungssystem für Java-Projekte.

* Ein Text-Editor oder eine integrierte Entwicklungsumgebung (IDE).

    > [!NOTE]
    > Der Editor oder die IDE verfügen möglicherweise über bestimmte Funktionen für die Arbeit mit Maven, die in diesem Dokument nicht behandelt werden. Informationen zu den Funktionen der Bearbeitungsumgebung finden Sie in der Dokumentation für das von Ihnen verwendete Produkt.

    * Einen SSH-Client. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

* Die `ssh`- und `scp`-Befehle. Damit werden Dateien auf den HDInsight-Cluster kopiert. Unter Windows 10 können Sie diese durch Bash abrufen.

## <a name="understanding-the-example"></a>Grundlegendes zum Beispiel

Das [hdinsight-java-storm-eventub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) Beispiel enthält zwei Topologien:

Die `resources/writer.yaml`-Topologie schreibt Zufallsdaten in einen Azure Event Hub. Die Daten – eine zufällige Geräte-ID und ein Gerätewert – werden von der `DeviceSpout`-Komponente generiert. Also wird Hardware simuliert, die eine Zeichenfolgen-ID und einen numerischen Wert ausgibt.

Die `resources/reader.yaml`-Topologie liest (von EventHubWriter geschriebene) Daten aus dem Event Hub, analysiert die JSON-Daten und protokolliert dann die `deviceId`- und `deviceValue`-Daten.

Die Daten werden vor dem Schreiben in den Event Hub als JSON-Dokument formatiert, und beim Lesen durch den Reader werden sie außerhalb von JSON in Tupel analysiert. Das JSON-Format sieht folgendermaßen aus:

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a>Projektkonfiguration

Die Datei `POM.xml` enthält Konfigurationsinformationen für dieses Maven-Projekt. Die interessantesten davon sind:

#### <a name="event-hub-components"></a>Event Hub-Komponenten

Die aus Azure Event Hubs lesende und darin schreibende Komponente befindet sich im [HDInsight-Repository](https://github.com/hdinsight/mvn-repo). Die folgenden Abschnitte in der `POM.xml`-Datei laden die Komponenten aus diesem Repository.

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="the-eventhubs-storm-spout-dependency"></a>Die EventHubs Storm Spout-Abhängigkeit

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

Mit dieser XML-Datei wird eine Abhängigkeit für das Paket „eventhubs“ definiert, das eine Spout-Komponente zum Lesen aus Event Hub-Instanzen sowie eine Bolt-Komponente zum Schreiben in Event Hub-Instanzen enthält.

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

Diese XML-Datei konfiguriert das Projekt zum Generieren von Ausgabedaten für Java 8, das in HDInsight 3.5 oder höher verwendet wird.

#### <a name="the-maven-shade-plugin"></a>Das maven-shade-plugin

```xml
<!-- build an uber jar -->
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-shade-plugin</artifactId>
<version>2.3</version>
<configuration>
    <transformers>
    <!-- Keep us from getting a can't overwrite file error -->
    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer"/>
    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
    </transformers>
    <!-- Keep us from getting a bad signature error -->
    <filters>
    <filter>
        <artifact>*:*</artifact>
        <excludes>
            <exclude>META-INF/*.SF</exclude>
            <exclude>META-INF/*.DSA</exclude>
            <exclude>META-INF/*.RSA</exclude>
        </excludes>
    </filter>
    </filters>
</configuration>
<executions>
    <execution>
    <phase>package</phase>
    <goals>
        <goal>shade</goal>
    </goals>
    </execution>
</executions>
</plugin>
```

Diese XML-Datei konfiguriert die Lösung zum Packen der Ausgabedaten in eine Uber-JAR-Datei. Das Jar enthält den Projektcode und die erforderlichen Abhängigkeiten. Es wird auch für folgende Zwecke verwendet:

* Umbenennen von Lizenzdateien für die Abhängigkeiten
* Ausschließen von Sicherheit/Signaturen
* Stellen Sie sicher, dass mehrere Implementierungen derselben Schnittstelle in einem Eintrag zusammengeführt werden.

Mit diesen Konfigurationseinstellungen werden Fehler zur Laufzeit verhindert.

#### <a name="topology-definitions"></a>Topologiedefinitionen

Dieses Beispiel verwendet das [Flux](https://storm.apache.org/releases/1.1.0/flux.html)-Framework. Dieses Framework verwendet YAML, um die Topologien zu definieren. Der Hauptvorteil besteht, dass Sie die Topologie nicht im Java-Code als vordefinierten Code festlegen. Da die Definition YAML ist, können Sie sie vor der Übermittlung der Topologie ändern, ohne alles neu kompilieren zu müssen.

__writer.yaml__:

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubwriter"

components:
  # Configure the Event Hub spout
  - id: "eventhubbolt-config"
    className: "org.apache.storm.eventhubs.bolt.EventHubBoltConfig"
    constructorArgs:
      # These are populated from the .properties file when the topology is started
      - "${eventhub.write.policy.name}"
      - "${eventhub.write.policy.key}"
      - "${eventhub.namespace}"
      - "servicebus.windows.net"
      - "${eventhub.name}"

spouts:
  - id: "device-emulator-spout"
    className: "com.microsoft.example.DeviceSpout"
    parallelism: ${eventhub.partitions}

bolts:
  - id: "eventhub-bolt"
    className: "org.apache.storm.eventhubs.bolt.EventHubBolt"
    constructorArgs:
      - ref: "eventhubbolt-config" # config declared in components section
    # parallelism hint. This should be the same as the number of partitions for your Event Hub, so we read it from the dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

# How data flows through the components
streams:
  - name: "spout -> eventhub" # just a string used for logging
    from: "device-emulator-spout"
    to: "eventhub-bolt"
    grouping:
        type: SHUFFLE

  - name: "spout -> logger"
    from: "device-emulator-spout"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

__reader.yaml__:

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubreader"

components:
  # Configure the Event Hub spout
  - id: "eventhubspout-config"
    className: "org.apache.storm.eventhubs.spout.EventHubSpoutConfig"
    constructorArgs:
      # These are populated from the .properties file when the topology is started
      - "${eventhub.read.policy.name}"
      - "${eventhub.read.policy.key}"
      - "${eventhub.namespace}"
      - "${eventhub.name}"
      - ${eventhub.partitions}

spouts:
  - id: "eventhub-spout"
    className: "org.apache.storm.eventhubs.spout.EventHubSpout"
    constructorArgs:
      - ref: "eventhubspout-config" # config declared in components section
    # parallelism hint. This should be the same as the number of partitions for your Event Hub, so we read it from the dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

bolts:
  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

  # Parses from JSON into tuples
  - id: "parser-bolt"
    className: "com.microsoft.example.ParserBolt"
    parallelism: ${eventhub.partitions}

# How data flows through the components
streams:
  - name: "spout -> parser" # just a string used for logging
    from: "eventhub-spout"
    to: "parser-bolt"
    grouping:
        type: SHUFFLE

  - name: "parser -> log-bolt"
    from: "parser-bolt"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

#### <a name="tell-the-topology-about-event-hub"></a>Informieren der Topologie über Event Hub

Zur Laufzeit wird die `dev.properties`-Datei verwendet, um die Event Hub-Konfiguration an die Topologie zu übergeben. Der standardmäßige Inhalt der Datei sieht etwa wie im folgenden Beispiel aus:

```yaml
eventhub.write.policy.name: writer
eventhub.write.policy.key: your_key_here
eventhub.read.policy.name: reader
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: storm
eventhub.partitions: 2
```

## <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Bei der Installation von Java und dem JDK auf Ihrer Entwicklungsworkstation können die folgenden Umgebungsvariablen festgelegt werden. Sie sollten dennoch prüfen, ob die Variablen vorhanden sind und korrekte Werte für Ihr System enthalten.

* **JAVA_HOME** – sollte auf das Verzeichnis verweisen, in dem die Java-Laufzeitumgebung (Java Runtime Environment, JRE) installiert ist. Für eine Unix- oder Linux-Distribution sollte z. B. ein Wert wie `/usr/lib/jvm/java-7-oracle` verwendet werden. Unter Windows sollte der Wert so ähnlich sein wie `c:\Program Files (x86)\Java\jre1.7`
* **PATH** – sollte die folgenden Pfade enthalten:

  * **JAVA_HOME** (oder den entsprechenden Pfad)
  * **JAVA_HOME\bin** (oder den entsprechenden Pfad)
  * Das Verzeichnis, in dem Maven installiert ist.

## <a name="configure-event-hub"></a>Konfigurieren von Event Hub

Event Hubs ist die Datenquelle für dieses Beispiel. Führen Sie die folgenden Schritte aus, um eine Event Hub-Instanz zu erstellen.

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) die Optionen **NEU** > **Service Bus** > **Event Hub** > **Benutzerdefiniert erstellen**.

2. Geben Sie auf dem Bildschirm **Neuen Event Hub hinzufügen** den **Namen des Event Hubs** ein. Wählen Sie die **Region** aus, in der der Hub erstellt wird, und erstellen Sie dann einen Namespace, oder wählen Sie einen vorhandenen Namespace aus. Klicken Sie schließlich auf den **Pfeil**, um fortzufahren.

    ![Seite 1 des Assistenten](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > Wählen Sie den gleichen **Speicherort** auf dem HDInsight-Server aus wie für Storm, um Latenz und Kosten zu verringern.

3. Geben Sie im Bildschirm **Event Hub konfigurieren** die Werte für **Partitionsanzahl** und **Nachrichtenaufbewahrung** ein. Verwenden Sie für dieses Beispiel eine Anzahl von 10 Partitionen und eine Nachrichtenaufbewahrung von 1. Notieren Sie sich die Partitionsanzahl, da Sie diesen Wert später benötigen werden.

    ![Seite 2 des Assistenten](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. Nachdem der Event Hub erstellt wurde, wählen Sie den Namespace, wählen Sie **Event Hubs**aus, und wählen Sie dann den zuvor erstellten Event Hub aus.
5. Klicken Sie auf **Konfigurieren**, und erstellen Sie zwei neue Zugriffsrichtlinien mit den folgenden Informationen:

    <table>
    <tr><th>Name</th><th>Berechtigungen</th></tr>
    <tr><td>Writer</td><td>Send</td></tr>
    <tr><td>Leser</td><td>Empfangen</td></tr>
    </table>

    Klicken Sie nach dem Erstellen der Berechtigungen auf das Symbol **Speichern** am unteren Seitenrand. Diese SAS-Richtlinien werden zum Lesen aus und Schreiben in Event Hub-Instanzen verwendet.

    ![Richtlinien](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. Verwenden Sie nach dem Speichern der Richtlinien den **Freigegebener Generator für Zugriffsschlüssel** am unteren Seitenrand, um den Schlüssel für die Richtlinien **writer** und **reader** abzurufen. Speichern Sie diese Schlüssel.

## <a name="download-and-build-the-project"></a>Herunterladen und Erstellen des Projekts

1. Laden Sie das Projekt von GitHub herunter: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub). Sie können das Paket als Zip-Archiv herunterladen oder [git](https://git-scm.com/) verwenden, um das Projekt lokal zu klonen.

2. Ändern Sie die `dev.properties`-Datei mit der Konfiguration für Ihren Event Hub.

3. Verwenden Sie die folgenden Elemente, um das Projekt zu erstellen und zu packen:

        mvn package

    Mit diesem Befehl werden die erforderlichen Abhängigkeiten heruntergeladen. Anschließend wird das Projekt erstellt und gepackt. Die Ausgabe wird im Verzeichnis **/target** als **EventHubExample-1.0-SNAPSHOT.jar** gespeichert.

## <a name="test-locally"></a>Lokales Testen

Da diese Topologien nur aus Event Hubs lesen und darin schreiben, können Sie sie lokal testen, wenn Sie eine [Storm-Entwicklungsumgebung](http://storm.apache.org/releases/current/Setting-up-development-environment.html) besitzen. Verwenden Sie zur lokalen Ausführung in der Entwicklungsumgebung die folgenden Schritte:

1. Writer-Ausführung:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. Reader-Ausführung:

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * `--local`: Führen Sie die Topologie im lokalen (nicht verteilten) Modus aus.
> * `-R /writer.yaml`: Laden Sie die Topologiedefinition aus den in der JAR-Datei verpackten `resources`. Wenn die Topologie eine Datei auf dem lokalen Dateisystem ist, geben Sie den Pfad dahin stattdessen als letzten Parameter an.
> * `--filter dev.properties`: Verwenden Sie den Inhalt von `dev.properties`, um die Topologiedefinitionen mit Werten zu füllen. Beispiel: `${eventhub.read.policy.name}`.

Bei lokaler Ausführung wird die Ausgabe auf der Konsole protokolliert. Sie können die Topologie mit __STRG+C__ beenden.

## <a name="deploy-the-topologies"></a>Bereitstellen der Topologien

1. Kopieren Sie das JAR-Paket mit SCP in Ihren HDInsight-Cluster. Ersetzen Sie USERNAME durch den SSH-Benutzer Ihres Clusters. Ersetzen Sie CLUSTERNAME durch den Namen Ihres HDInsight-Clusters.

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    Wenn Sie ein Kennwort für das SSH-Konto verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel mit dem Konto verwendet haben, müssen Sie möglicherweise den `-i` -Parameter verwenden, um den Pfad zur Schlüsseldatei anzugeben. Beispiel: `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`

    Mit diesem Befehl wird die Datei in das Basisverzeichnis des SSH-Benutzers im Cluster kopiert.

2. Nachdem der Upload der Datei abgeschlossen wurde, können Sie SSH zum Verbinden mit dem HDInsight-Cluster verwenden. Ersetzen Sie **USERNAME** durch den Namen der SSH-Anmeldung. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > Wenn Sie ein Kennwort für das SSH-Konto verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel mit dem Konto verwendet haben, müssen Sie möglicherweise den `-i` -Parameter verwenden, um den Pfad zur Schlüsseldatei anzugeben. Im folgenden Beispiel wird der private Schlüssel aus `~/.ssh/id_rsa` geladen:
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. Verwenden Sie den folgenden Befehl, um die Website zu starten.

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * `--remote`: Sendet die Topologie an den Dienst Nimbus, der ihn auf den Workerknoten im Cluster startet.

4. Um die protokollierten Daten anzuzeigen, gehen Sie zu „https://CLUSTERNAME.azurehdinsight.net/stormui“. Dabei steht __CLUSTERNAME__ für den Namen des HDInsight-Clusters. Wählen Sie die Topologien aus, und führen Sie einen Drilldown zu den Komponenten aus. Wählen Sie den Eintrag __port__ für eine Instanz einer Komponente, um die protokollierten Informationen anzuzeigen.

5. Verwenden Sie die folgenden Befehle, um die Topologien zu stoppen:

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a>Nächste Schritte

* [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)

