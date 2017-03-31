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
ms.date: 02/17/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: fcca957dc365d8c38b5a08991939860c5af96813
ms.lasthandoff: 03/25/2017


---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a>Verarbeiten von Ereignissen aus Azure Event Hubs mit Storm in HDInsight (Java)

Erfahren Sie, wie Sie Azure Event Hub-Instanzen mit Storm in HDInsight verwenden. In diesem Beispiel werden Java-basierte Komponenten zum Lesen und Schreiben von Daten in Azure Event Hub-Instanzen verwendet.

Azure Event Hubs ermöglichen Ihnen die Verarbeitung sehr großer Datenmengen aus Websites, Apps und Geräten. Der Event Hub-Spout vereinfacht die Verwendung von Apache Storm in HDInsight, um diese Daten in Echtzeit zu analysieren. Sie können auch Daten aus Storm mithilfe des Event Hubs-Bolts in Event Hub schreiben.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Storm auf HDInsight-Clusterversion 3.5. Weitere Informationen finden Sie unter [Erste Schritte mit Apache Storm in HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md).
    
    > [!IMPORTANT]
    > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Einen [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

* [Oracle Java Developer Kit (JDK) Version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) oder Äquivalent, z.B. [OpenJDK](http://openjdk.java.net/).

* [Maven](https://maven.apache.org/download.cgi) – Maven ist ein Projekterstellungssystem für Java-Projekte.

* Ein Text-Editor oder eine integrierte Entwicklungsumgebung (IDE).
  
  > [!NOTE]
  > Der Editor oder die IDE verfügen möglicherweise über bestimmte Funktionen für die Arbeit mit Maven, die in diesem Dokument nicht behandelt werden. Informationen zu den Funktionen der Bearbeitungsumgebung finden Sie in der Dokumentation für das von Ihnen verwendete Produkt.
  
  * Einen SSH-Client. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

* Ein SCP-Client. Der Befehl `scp` wird auf allen Linux-, Unix- und OS X-Systemen bereitgestellt (einschließlich Bash unter Windows 10). Für Windows-Systeme, die den Befehl `scp` nicht enthalten, wird PSCP empfohlen. PSCP steht über die [PuTTY-Downloadseite](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) zur Verfügung.

## <a name="understanding-the-example"></a>Grundlegendes zum Beispiel

Das [hdinsight-java-storm-eventub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) Beispiel enthält zwei Topologien:

**com.microsoft.example.EventHubWriter** schreibt zufällige Daten in ein Azure Event Hub. Die Daten werden von einem Spout generiert und sind zufällige Geräte-IDs und Gerät-Werte. Also wird Hardware simuliert, die eine Zeichenfolgen-ID und einen numerischen Wert ausgibt.

**com.microsoft.example.EventHubReader** liest Daten von Event Hub und speichert sie im Standardspeicher des Clusters im Verzeichnis „/devicedata“.

Die Daten werden vor dem Schreiben in den Event Hub als JSON-Dokument formatiert, und beim Lesen durch den Reader werden sie außerhalb von JSON in Tupel analysiert. Das JSON-Format sieht folgendermaßen aus:

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a>Projektkonfiguration

Die **POM.xml** Datei enthält Konfigurationsinformationen für dieses Maven-Projekt. Die interessantesten davon sind:

#### <a name="the-eventhubs-storm-spout-dependency"></a>Die EventHubs Storm Spout-Abhängigkeit

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>1.0.2</version>
</dependency>
```

Mit dieser XML-Datei wird eine Abhängigkeit für das Paket „eventhubs“ definiert, das eine Spout-Komponente zum Lesen aus Event Hub-Instanzen sowie eine Bolt-Komponente zum Schreiben in Event Hub-Instanzen enthält.

> [!NOTE]
> Dieses Paket wird weiter unten in diesem Dokument installiert.

#### <a name="the-hdfsbolt-and-wasb-components"></a>Die HdfsBolt- und WASB-Komponenten

Der HdfsBolt wird in der Regel zum Speichern von Daten im Hadoop Distributed File System HDFS verwendet. Jedoch nutzen HDInsight-Cluster Azure-Speicher (WASB) als Standard-Informationsspeicher, sodass wir mehrere Komponenten laden müssen, die HdfsBolt ermöglichen, das WASB-Dateisystem zu verstehen.

```xml
<!--HdfsBolt stuff -->
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-hdfs</artifactId>
    <exclusions>
    <exclusion>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-client</artifactId>
    </exclusion>
    <exclusion>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-hdfs</artifactId>
    </exclusion>
    </exclusions>
    <version>${storm.version}</version>
</dependency>
<!--So HdfsBolt knows how to talk to WASB -->
<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-client</artifactId>
    <version>${hadoop.version}</version>
</dependency>
<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-hdfs</artifactId>
    <version>${hadoop.version}</version>
</dependency>
<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-azure</artifactId>
    <version>${hadoop.version}</version>
</dependency>
<dependency>
    <groupId>org.apache.hadoop</groupId>
    <artifactId>hadoop-common</artifactId>
    <version>${hadoop.version}</version>
    <exclusions>
    <exclusion>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
    </exclusion>
    </exclusions>
</dependency>
```

> [!NOTE]
> Wenn Sie mit einer früheren Version von HDInsight wie Version 3.2 arbeiten, müssen Sie diese Komponenten manuell registrieren. Ein Beispiel finden Sie in der Verzweigung [Storm v0.9.3](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub/tree/Storm_v0.9.3) des Beispielrepositorys.

#### <a name="the-maven-compiler-plugin"></a>Das maven-compiler-plugin

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>2.3.2</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

Dadurch wird das Projekt zum Generieren von Ausgabedaten für Java 8 konfiguriert, das in HDInsight 3.5 verwendet wird.

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
    <!-- Keep us from getting errors when trying to use WASB from the storm-hdfs bolt -->
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

Dadurch wird die Lösung zum Packen der Ausgabedaten in ein Uberjar konfiguriert. Das Jar enthält den Projektcode und die erforderlichen Abhängigkeiten. Es wird auch für folgende Zwecke verwendet:

* Umbenennen von Lizenzdateien für die Abhängigkeiten
* Ausschließen von Sicherheit/Signaturen
* Stellen Sie sicher, dass mehrere Implementierungen derselben Schnittstelle in einem Eintrag zusammengeführt werden.

Mit diesen Konfigurationseinstellungen werden Fehler zur Laufzeit verhindert.

#### <a name="the-exec-maven-plugin"></a>Das exec-maven-plugin

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.2.1</version>
    <executions>
    <execution>
    <goals>
        <goal>exec</goal>
    </goals>
    </execution>
    </executions>
    <configuration>
    <executable>java</executable>
    <includeProjectDependencies>true</includeProjectDependencies>
    <includePluginDependencies>false</includePluginDependencies>
    <classpathScope>compile</classpathScope>
    <mainClass>${storm.topology}</mainClass>
    </configuration>
</plugin>
```

Mit dieser Konfiguration lässt sich die Topologie einfacher lokal in Ihrer Entwicklungsumgebung ausführen. Die Topologie kann mit der folgenden Syntax lokal ausgeführt werden:

    mvn compile exec:java -Dstorm.topology=<CLASSNAME>

Beispiel: `mvn compile exec:java -Dstorm.topology=com.microsoft.example.EventHubWriter`.

#### <a name="the-resources-section"></a>Der Abschnitt „Ressourcen“

```xml
<resources>
    <resource>
    <directory>${basedir}/conf</directory>
    <filtering>false</filtering>
    <includes>
        <include>EventHubs.properties</include>
    </includes>
    </resource>
</resources>
```

Mit dieser Konfiguration werden die im Projekt enthaltenen nicht Java-basierten Ressourcen definiert. Die Datei **EventHubs.properties** enthält z.B. Informationen zum Herstellen einer Verbindung mit einer Azure Event Hub-Instanz.

## <a name="configure-environment-variables"></a>Konfigurieren von Umgebungsvariablen

Bei der Installation von Java und dem JDK auf Ihrer Entwicklungsworkstation können die folgenden Umgebungsvariablen festgelegt werden. Sie sollten dennoch prüfen, ob die Variablen vorhanden sind und korrekte Werte für Ihr System enthalten.

* **JAVA_HOME** – sollte auf das Verzeichnis verweisen, in dem die Java-Laufzeitumgebung (Java Runtime Environment, JRE) installiert ist. Für eine Unix- oder Linux-Distribution sollte z. B. ein Wert wie `/usr/lib/jvm/java-7-oracle` verwendet werden. Unter Windows sollte der Wert so ähnlich sein wie `c:\Program Files (x86)\Java\jre1.7`
* **PATH** – sollte die folgenden Pfade enthalten:
  
  * **JAVA_HOME** (oder den entsprechenden Pfad)
  * **JAVA_HOME\bin** (oder den entsprechenden Pfad)
  * Das Verzeichnis, in dem Maven installiert ist.

## <a name="download-and-register-the-eventhub-components"></a>Herunterladen und Registrieren der EventHub-Komponenten

1. Laden Sie die Datei `storm-eventhubs-1.0.2-jar-with-dependencies.jar` unter [https://000aarperiscus.blob.core.windows.net/certs/storm-eventhubs-1.0.2-jar-with-dependencies.jar](https://000aarperiscus.blob.core.windows.net/certs/storm-eventhubs-1.0.2-jar-with-dependencies.jar) herunter. Diese Datei enthält eine Spout- und eine Bolt-Komponente zum Lesen und Schreiben von EventHubs.

2. Registrieren Sie die Komponenten mit den folgenden Befehlen in Ihrem lokalen Maven-Repository:
    
        mvn install:install-file -Dfile=storm-eventhubs-1.0.2-jar-with-dependencies.jar -DgroupId=com.microsoft -DartifactId=eventhubs -Dversion=1.0.2 -Dpackaging=jar
    
    Ändern Sie den Parameter `-Dfile=` so, dass er auf den Speicherort der heruntergeladenen Datei verweist.

    Mit diesem Befehl wird die Datei im lokalen Maven-Repository installiert und dort zum Zeitpunkt der Kompilierung von Maven gefunden.

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

2. Verwenden Sie die folgenden Elemente, um das Projekt zu erstellen und zu packen:
   
        mvn package
   
    Mit diesem Befehl werden die erforderlichen Abhängigkeiten heruntergeladen. Anschließend wird das Projekt erstellt und gepackt. Die Ausgabe wird im Verzeichnis **/target** als **EventHubExample-1.0-SNAPSHOT.jar** gespeichert.

## <a name="deploy-the-topologies"></a>Bereitstellen der Topologien

Die von diesem Projekt erstellte JAR-Datei enthält zwei Topologien: **com.microsoft.example.EventHubWriter** und **com.microsoft.example.EventHubReader**. Die Topologie EventHubWriter sollte zuerst gestartet werden, da sie Ereignisse in ein Event Hub schreibt, die dann vom EventHubReader gelesen werden.

1. Kopieren Sie das JAR-Paket mit SCP in Ihren HDInsight-Cluster. Ersetzen Sie USERNAME durch den SSH-Benutzer Ihres Clusters. Ersetzen Sie CLUSTERNAME durch den Namen Ihres HDInsight-Clusters.
   
        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.
   
    Wenn Sie ein Kennwort für das SSH-Konto verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel mit dem Konto verwendet haben, müssen Sie möglicherweise den `-i` -Parameter verwenden, um den Pfad zur Schlüsseldatei anzugeben. Beispiel: `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`.
   
   > [!NOTE]
   > Wenn es sich bei Ihrem Client um eine Windows-Arbeitsstation handelt, ist möglicherweise kein SCP-Befehl installiert. Wir empfehlen PSCP, das Sie auf der [PuTTY-Downloadseite](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen können.
   
    Mit diesem Befehl wird die Datei in das Basisverzeichnis des SSH-Benutzers im Cluster kopiert.

2. Nachdem der Upload der Datei abgeschlossen wurde, können Sie SSH zum Verbinden mit dem HDInsight-Cluster verwenden. Ersetzen Sie **USERNAME** durch den Namen der SSH-Anmeldung. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters:
   
        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
   
   > [!NOTE]
   > Wenn Sie ein Kennwort für das SSH-Konto verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel mit dem Konto verwendet haben, müssen Sie möglicherweise den `-i` -Parameter verwenden, um den Pfad zur Schlüsseldatei anzugeben. Im folgenden Beispiel wird der private Schlüssel aus `~/.ssh/id_rsa` geladen:
   > 
   > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`
   
    Wenn Sie PuTTY verwenden, geben Sie `CLUSTERNAME-ssh.azurehdinsight.net` im Feld **Hostname (oder IP-Adresse)** ein, und klicken Sie dann auf **Öffnen**, um eine Verbindung herzustellen. Sie werden zum Eingeben Ihres SSH-Kontonamens aufgefordert.
   
   > [!NOTE]
   > Wenn Sie ein Kennwort für das SSH-Konto verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen SSH-Schlüssel mit dem Konto verwendet haben, müssen Sie möglicherweise die folgenden Schritte ausführen, um den Schlüssel auszuwählen:
   > 
   > 1. Erweitern Sie in **Kategorie** erst **Verbindung**, dann **SSH**, und wählen Sie anschließend **Authentifizierung** aus.
   > 2. Klicken Sie auf **Durchsuchen** , und wählen Sie die PPK-Datei aus, die Ihren privaten Schlüssel enthält.
   > 3. Klicken Sie auf **Öffnen** , um eine Verbindung herzustellen.

3. Verwenden Sie den folgenden Befehl, um die Website zu starten.
   
        storm jar EventHubExample-1.0-SNAPSHOT.jar com.microsoft.example.EventHubWriter writer
        storm jar EventHubExample-1.0-SNAPSHOT.jar com.microsoft.example.EventHubReader reader
   
    Mit diesen Befehlen werden die Topologien mit den Anzeigenamen „reader“ und „writer“ gestartet.

4. Warten Sie eine Minute, bis die Topologien Daten generieren. Verwenden Sie den folgenden Befehl, um zu überprüfen, ob Daten in HDInsight Storage geschrieben werden:
   
        hadoop fs -ls /devicedata
   
    Mit diesem Befehl wird eine Liste mit Dateien zurückgegeben, die in etwa wie folgt aussieht:
   
        -rw-r--r--   1 storm supergroup      10283 2015-08-11 19:35 /devicedata/wasbbolt-14-0-1439321744110.txt
        -rw-r--r--   1 storm supergroup      10277 2015-08-11 19:35 /devicedata/wasbbolt-14-1-1439321748237.txt
        -rw-r--r--   1 storm supergroup      10280 2015-08-11 19:36 /devicedata/wasbbolt-14-10-1439321760398.txt
        -rw-r--r--   1 storm supergroup      10267 2015-08-11 19:36 /devicedata/wasbbolt-14-11-1439321761090.txt
        -rw-r--r--   1 storm supergroup      10259 2015-08-11 19:36 /devicedata/wasbbolt-14-12-1439321762679.txt
   
   > [!NOTE]
   > Möglicherweise wird für einige Dateien die Dateigröße 0 angezeigt, da sie vom EventHubReader erstellt wurden, aber keine Daten darin gespeichert wurden.
   
    Sie können den Inhalt dieser Dateien mit dem folgenden Befehl anzeigen:
   
        hadoop fs -text /devicedata/*.txt
   
    Mit diesem Befehl werden Daten zurückgegeben, die in etwa wie folgt aussehen:
   
        3409e622-c85d-4d64-8622-af45e30bf774,848981614
        c3305f7e-6948-4cce-89b0-d9fbc2330c36,-1638780537
        788b9796-e2ab-49c4-91e3-bc5b6af1f07e,-1662107246
        6403df8a-6495-402f-bca0-3244be67f225,275738503
        d7c7f96c-581a-45b1-b66c-e32de6d47fce,543829859
        9a692795-e6aa-4946-98c1-2de381b37593,1857409996
        3c8d199b-0003-4a79-8d03-24e13bde7086,-1271260574
   
    Die erste Spalte enthält den Wert der Geräte-ID und die zweite Spalte den Geräte-Wert.

5. Verwenden Sie die folgenden Befehle, um die Topologien zu stoppen:
   
        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a>Löschen des Clusters

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshooting"></a>Problembehandlung

Wenn im Verzeichnis „/devicedata“ keine Dateien angezeigt werden, suchen Sie in der Storm-Benutzeroberfläche nach eventuellen von den Topologien zurückgegebenen Fehlern.

Weitere Informationen zur Storm-Benutzeroberfläche finden Sie in den folgenden Themen:

* Informationen zur Verwendung eines **Linux-basierten** Clusters finden Sie unter [Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight unter Linux](hdinsight-storm-deploy-monitor-topology-linux.md)

* Informationen zur Verwendung eines **Windows-basierten** Clusters finden Sie unter [Bereitstellen und Verwalten von Apache Storm-Topologien in HDInsight unter Windows](hdinsight-storm-deploy-monitor-topology-linux.md)

## <a name="next-steps"></a>Nächste Schritte

* [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)


