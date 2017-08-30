---
title: Analysieren von Sensordaten mit Apache Storm und HBase | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mit einem virtuellen Netzwerk eine Verbindung mit Apache Storm herstellen. Verwenden Sie Storm mit HBase, um Sensordaten von einem Event Hub zu verarbeiten und mit D3.js darzustellen.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/09/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 760543dc3880cb0dbe14070055b528b94cffd36b
ms.openlocfilehash: 0d1cc959c87bd64ed728f8b56c9b9156fa492a8b
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a>Analysieren von Sensordaten mit Apache Storm, Event Hub und HBase in HDInsight (Hadoop)

Erfahren Sie, wie Sie Apache Storm in HDInsight zur Verarbeitung von Sensordaten von Azure Event Hub verwenden. Die Daten werden dann in Apache HBase in HDInsight gespeichert und mithilfe von D3.js visualisiert.

Die in diesem Dokument verwendete Azure Resource Manager-Vorlage veranschaulicht das Erstellen mehrerer Azure-Ressourcen in einer Ressourcengruppe. Mit der Vorlage werden ein virtuelles Azure-Netzwerk, zwei HDInsight-Cluster (Storm und HBase) und eine Azure-Web-App erstellt. Eine node.js-Implementierung eines Echtzeit-Webdashboards wird automatisch für die Web-App bereitgestellt.

> [!NOTE]
> Die Informationen und das Beispiel in diesem Dokument erfordern HDInsight-Version 3.6.
>
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement.
* [Node.js](http://nodejs.org/): Wird für eine lokale Vorschau des Webdashboards in Ihrer Entwicklungsumgebung verwendet.
* [Java und JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Wird zum Entwickeln der Storm-Topologie verwendet.
* [Maven](http://maven.apache.org/what-is-maven.html): Dient zum Erstellen und Kompilieren des Projekts.
* [Git](http://git-scm.com/): Dient zum Herunterladen des Projekts von GitHub.
* Ein **SSH** -Client: Dient zum Herstellen der Verbindung mit den Linux-basierten HDInsight-Clustern. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).


> [!IMPORTANT]
> Sie benötigen keinen vorhandenen HDInsight-Cluster. Durch die Schritte in diesem Dokument werden die folgenden Ressourcen erstellt:
> 
> * Ein virtuelles Azure-Netzwerk
> * Ein Storm-Cluster in HDInsight (Linux-basiert, zwei Workerknoten)
> * Ein HBase-Cluster in HDInsight (Linux-basiert, zwei Workerknoten)
> * Eine Azure-Web-App, die das Webdashboard hostet

## <a name="architecture"></a>Architektur

![Architekturdiagramm](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

Dieses Beispiel umfasst die folgenden Komponenten:

* **Azure Event Hubs**: Enthalten von Sensoren gesammelte Daten.
* **Storm in HDInsight**: Bietet Echtzeitverarbeitung von Event Hub-Daten.
* **HBase in HDInsight**: Stellt einen persistenten NoSQL-Datenspeicher für Daten bereit, nachdem sie von Storm verarbeitet wurden.
* **Azure Virtual Network-Dienst**: Ermöglicht die sichere Kommunikation zwischen Storm in HDInsight-Clustern und HBase in HDInsight-Clustern.
  
  > [!NOTE]
  > Ein virtuelles Netzwerk ist erforderlich, wenn die Java-HBase-Client-API verwendet wird. Sie ist nicht über das öffentliche Gateway für HBase-Cluster verfügbar gemacht. Die Installation von HBase- und Storm-Clustern in dem gleichen virtuellen Netzwerk ermöglicht dem Storm-Cluster (oder beliebigen anderen Systemen im virtuellen Netzwerk) direkten HBase-Zugriff mithilfe der Client-API.

* **Dashboard-Website**: Ein Beispiel-Dashboard, das Daten in Echtzeit aufzeichnet.
  
  * Die Website ist in Node.js implementiert.
  * [Socket.io](http://socket.io/) wird für die Echtzeitkommunikation zwischen der Storm-Topologie und der Website verwendet.
    
    > [!NOTE]
    > Die Verwendung von Socket.io für die Kommunikation ist ein Implementierungsdetail. Sie können alle Kommunikationsframeworks, wie z. B. reine WebSockets oder SignalR, verwenden.

  * [D3.js](http://d3js.org/) wird verwendet, um die Daten grafisch darzustellen, die an die Website gesendet werden.

> [!IMPORTANT]
> Zwei Cluster sind erforderlich, da keine unterstützte Methode zum Erstellen eines HDInsight-Clusters für Storm und HBase verfügbar ist.

Die Topologie liest mithilfe der [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html)-Klasse Daten aus Event Hub und schreibt mithilfe der [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html)-Klasse Daten in HBase. Kommunikation mit der Website erfolgt mithilfe von [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Im Folgenden ist die Topologieanordnung dargestellt:

![Topologiediagramm](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> Das Diagramm ist eine vereinfachte Ansicht der Topologie. Eine Instanz jeder Komponente wird für jede Partition in Ihrem Event Hub erstellt. Diese Instanzen werden über die Knoten im Cluster verteilt, und Daten werden wie folgt zwischen ihnen weitergeleitet:
> 
> * Für Daten vom Spout an den Parser wird ein Lastenausgleich ausgeführt.
> * Vom Parser zum Dashboard und zu HBase übertragene Daten werden nach Geräte-ID gruppiert, sodass Nachrichten vom gleichen Gerät immer an die gleiche Komponente weitergeleitet werden.

### <a name="topology-components"></a>Topologiekomponenten

* **Event Hub-Spout**: Der Spout wird als Teil der Apache Storm-Version 0.10.0 und höher bereitgestellt.
  
  > [!NOTE]
  > Für den in diesem Beispiel verwendeten Event Hub-Spout ist ein Storm-Cluster der Version 3.5 oder 3.6 in HDInsight erforderlich.

* **ParserBolt.java**: Der Spout gibt reine JSON-Daten aus, und gelegentlich werden mehrere Ereignisse gleichzeitig ausgegeben. Dieser Bolt liest die vom Spout ausgegebenen Daten und analysiert die JSON-Nachricht. Der Bolt gibt dann die Daten als Tupel aus, das mehrere Felder enthält.
* **DashboardBolt.java**: Diese Komponente veranschaulicht, wie die Socket.io-Clientbibliothek für Java zum Senden von Daten an das Webdashboard in Echtzeit verwendet wird.
* **no-hbase.yaml**: Die bei der Ausführung im lokalen Modus verwendete Topologiedefinition. Dabei werden keine HBase-Komponenten verwendet.
* **with-hbase.yaml**: Die bei der Ausführung der Topologie im Cluster verwendete Topologiedefinition. Dabei werden HBase-Komponenten verwendet.
* **dev.properties**: Die Konfigurationsinformationen für den Event Hub-Spout, den HBase-Bolt und die Dashboard-Komponenten.

## <a name="prepare-your-environment"></a>Vorbereiten der Umgebung

Bevor Sie dieses Beispiel verwenden können, müssen Sie einen Azure Event Hub erstellen, der von der Storm-Topologie gelesen wird.

### <a name="configure-event-hub"></a>Konfigurieren von Event Hub

Event Hub ist die Datenquelle für dieses Beispiel. Führen Sie die folgenden Schritte aus, um einen Event Hub zu erstellen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **+ Neu** -> **Internet der Dinge (IoT)** -> **Event Hubs** aus.
2. Führen Sie im Abschnitt **Namespace erstellen** die folgenden Aufgaben aus:
   
   1. Geben Sie einen **Namen** für den Namespace ein.
   2. Wählen Sie einen Tarif. **Basic** ist für dieses Beispiel ausreichend.
   3. Wählen Sie aus, welches Azure- **Abonnement** verwendet werden soll.
   4. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe.
   5. Wählen Sie den **Speicherort** für den Event Hub aus.
   6. Wählen Sie **An Dashboard anheften** aus, und klicken Sie dann auf **Erstellen**.

3. Nachdem der Erstellungsvorgang abgeschlossen ist, werden die Informationen der Event Hubs für Ihren Namespace angezeigt. Wählen Sie hier die Option **+ Event Hub hinzufügen**. Geben Sie im Abschnitt **Event Hub erstellen** den Namen **sensordata** ein, und wählen Sie dann **Erstellen** aus. Behalten Sie für die anderen Felder den Standardwert bei.
4. Wählen Sie in der Ansicht „Event Hubs“ für Ihren Namespace die Option **Event Hubs** aus. Wählen Sie den Eintrag **sensordata** aus.
5. Wählen Sie im Event Hub „sensordata“ die Option **SAS-Richtlinien** aus. Verwenden Sie den Link **+ Hinzufügen** , um die folgenden Richtlinien hinzuzufügen:

    | Richtlinienname | Ansprüche |
    | ----- | ----- |
    | devices | Senden |
    | storm | Empfangen |

1. Wählen Sie beide Richtlinien aus, und notieren Sie sich den Wert von **PRIMÄRER SCHLÜSSEL** . Sie benötigen den Wert für beide Richtlinien für die weiteren Schritte.

## <a name="download-and-configure-the-project"></a>Herunterladen und Konfigurieren des Projekts

Führen Sie den folgenden Befehl aus, um das Projekt von GitHub herunterzuladen.

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

Nach Abschluss des Befehls verfügen Sie über die folgende Verzeichnisstruktur:

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO to the web dashboard.
        dashboard/nodejs/ - this is the node.js web dashboard.
        SendEvents/ - utilities to send fake sensor data.

> [!NOTE]
> Dieses Dokument geht nicht ausführlich auf alle Details des in diesem Beispiel enthaltenen Codes ein. Der Code ist jedoch vollständig kommentiert.

Um das Projekt zum Lesen von Event Hub zu konfigurieren, öffnen Sie die Datei `hdinsight-eventhub-example/TemperatureMonitor/dev.properties`, und fügen Sie die Event Hub-Informationen den folgenden Zeilen hinzu:

```bash
eventhub.read.policy.name: your_read_policy_name
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a>Kompilieren und lokales Testen

> [!IMPORTANT]
> Für die lokale Verwendung der Topologie ist eine funktionsfähige Storm-Entwicklungsumgebung erforderlich. Weitere Informationen finden Sie unter [Setting up a Storm development environment](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) (Einrichten einer Storm-Entwicklungsumgebung) unter Apache.org.

> [!WARNING]
> Wenn Sie eine Windows-Entwicklungsumgebung verwenden, erhalten Sie möglicherweise beim lokalen Ausführen der Topologie eine `java.io.IOException`. Wenn dies der Fall ist, führen Sie die Topologie in HDInsight aus.

Vor dem Testen müssen Sie das Dashboard zur Anzeige der Ausgabe der Topologie und zum Generieren der Daten starten, die im Event Hub gespeichert werden sollen.

> [!IMPORTANT]
> Die HBase-Komponente dieser Topologie ist beim lokalen Testen nicht aktiv. Von außerhalb des virtuellen Azure-Netzwerks, das die Cluster enthält, ist kein Zugriff auf die Java-API für den HBase-Cluster möglich.

### <a name="start-the-web-application"></a>Starten der Webanwendung

1. Öffnen Sie eine Eingabeaufforderung, und ändern Sie das Verzeichnis in `hdinsight-eventhub-example/dashboard`. Verwenden Sie den folgenden Befehl, um die von der Webanwendung benötigten Anwendungen zu installieren:
   
    ```bash
    npm install
    ```

2. Verwenden Sie den folgenden Befehl, um die Webanwendung zu starten.
   
    ```bash
    node server.js
    ```
   
    Es wird eine Meldung ähnlich der folgenden angezeigt:
   
        Server listening at port 3000

3. Öffnen Sie einen Webbrowser, und geben Sie `http://localhost:3000/` als Adresse ein. Eine Seite ähnlich der folgenden Abbildung wird angezeigt:
   
    ![Webdashboard](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    Lassen Sie die Eingabeaufforderung geöffnet. Verwenden Sie nach dem Testen STRG+C, um den Webserver zu beenden.

### <a name="generate-data"></a>Generieren von Daten

> [!NOTE]
> Die Schritte in diesem Abschnitt verwenden „Node.js“, sodass sie auf einer beliebigen Plattform ausgeführt werden können. Andere Sprachbeispiele finden Sie im Verzeichnis `SendEvents`.

1. Öffnen Sie eine neue Eingabeaufforderung, eine neue Shell oder ein neues Terminal, und wechseln Sie in das Verzeichnis `hdinsight-eventhub-example/SendEvents/nodejs`. Verwenden Sie den folgenden Befehl, um die für die Anwendung erforderlichen Abhängigkeiten zu installieren:

    ```bash
    npm install
    ```

2. Öffnen Sie die Datei `app.js` in einem Text-Editor, und fügen Sie die Event Hub-Informationen hinzu, die Sie zuvor abgerufen haben:
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'YourNamespace';
    // Event Hub Name
    var hubname ='sensordata';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'devices';
    var my_key = 'YourKey';
    ```
   
   > [!NOTE]
   > In diesem Beispiel wird vorausgesetzt, dass Sie `sensordata` als Namen für Ihren Event Hub und `devices` als Namen der Richtlinie, die einen `Send`-Anspruch enthält, verwendet haben.

3. Verwenden Sie den folgenden Befehl zum Einfügen neuer Einträge in Event Hub:
   
    ```bash
    node app.js
    ```
   
    Es werden mehrere Ausgabezeilen angezeigt, die die an Event Hub gesendeten Daten enthalten:
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-the-topology"></a>Erstellen und Starten der Topologie

1. Öffnen Sie eine neue Eingabeaufforderung, und wechseln Sie in das Verzeichnis `hdinsight-eventhub-example/TemperatureMonitor`. Erstellen und packen Sie die Topologie mit folgendem Befehl: 

    ```bash
    mvn clean package
    ```

2. Verwenden Sie den folgenden Befehl, um die Topologie im lokalen Modus zu starten:

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * Mit `--local` wird die Topologie im lokalen Modus gestartet.
    * Mit `--filter` wird die Datei `dev.properties` zum Auffüllen der Parameter in der Topologiedefinition verwendet.
    * Mit `resources/no-hbase.yaml` wird die Topologiedefinition `no-hbase.yaml` verwendet.
 
   Nach dem Starten liest die Topologie Einträge aus dem Event Hub und sendet sie an das Dashboard, das auf Ihrem lokalen Computer ausgeführt wird. Im Webdashboard sollten nun Linien angezeigt werden, die der folgenden Abbildung ähneln:
   
    ![Dashboard mit Daten](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. Verwenden Sie während der Ausführung des Dashboards den Befehl `node app.js` aus den vorherigen Schritten, um neue Daten an Event Hubs zu senden. Da die Temperaturwerte zufällig generiert werden, sollte das Diagramm aktualisiert werden, um große Temperaturänderungen anzuzeigen.
   
   > [!NOTE]
   > Sie müssen sich im Verzeichnis **hdinsight-eventhub-example/SendEvents/Nodejs** befinden, wenn Sie den Befehl `node app.js` verwenden.

3. Beenden Sie die Topologie durch Eingabe von STRG+C, nachdem Sie überprüft haben, dass die Updates der Dashboards funktionieren. Sie können STRG+C auch verwenden, um den lokalen Webserver zu beenden.

## <a name="create-a-storm-and-hbase-cluster"></a>Erstellen eines Storm- und HBase-Clusters

In den Schritten in diesem Abschnitt wird mit einer [Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-template-deploy.md) ein virtuelles Azure-Netzwerk sowie ein Storm- und HBase-Cluster im virtuellen Netzwerk erstellt. Die Vorlage erstellt auch eine Azure-Web-App und stellt eine Kopie des Dashboards darin bereit.

> [!NOTE]
> Ein virtuelles Netzwerk wird verwendet, sodass die Topologie im Storm-Cluster direkt mithilfe der HBase-Java-API mit dem HBase-Cluster kommunizieren kann.

Die in diesem Dokument verwendete Resource Manager-Vorlage befindet sich in einem öffentlichen Blobcontainer unter **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.

1. Klicken Sie auf die folgende Schaltfläche, um sich bei Azure anzumelden und die Resource Manager-Vorlage im Azure-Portal zu öffnen.
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Geben Sie im Abschnitt **Benutzerdefinierte Bereitstellung** die folgenden Werte ein:
   
    ![HDInsight-Parameter](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * **Base Cluster Name** (Cluster-Basisname): Dieser Wert wird als Basisname für Storm- und HBase-Cluster verwendet. Bei Eingabe von **abc** wird z.B. ein Storm-Cluster mit dem Namen **storm-abc** und ein HBase-Cluster mit dem Namen **hbase-abc** erstellt.
   * **Benutzername für Clusteranmeldung**: Der Administratorbenutzername für die Storm- und HBase-Cluster.
   * **Kennwort für Clusteranmeldung**: Das Administratorbenutzerkennwort für die Storm- und HBase-Cluster.
   * **SSH-Benutzername**: Der für den Storm- und HBase-Cluster zu erstellende SSH-Benutzer.
   * **SSH-Kennwort**: Das Kennwort für den SSH-Benutzer für Storm- und HBase-Cluster.
   * **Standort**: Die Region, in der die Cluster erstellt werden.
     
     Klicken Sie auf **OK** , um die Parameter zu speichern.

3. Erstellen Sie im Abschnitt **Grundlagen** eine Ressourcengruppe, oder wählen Sie eine vorhandene aus.
4. Wählen Sie im Dropdownmenü **Ressourcengruppenstandort** den gleichen Speicherort aus, den Sie für den Parameter **Speicherort** im Abschnitt **Einstellungen** ausgewählt haben.
5. Lesen Sie die Geschäftsbedingungen, und wählen Sie anschließend die Option **Ich stimme den oben genannten Geschäftsbedingungen zu** aus.
6. Aktivieren Sie zum Schluss **An Dashboard anheften**, und wählen Sie dann **Kaufen** aus. Das Erstellen der Cluster dauert ca. 20 Minuten.

Nachdem die Ressourcen erstellt wurden, werden Informationen zu der Ressourcengruppe angezeigt.

![Ressourcengruppe für VNET und Cluster](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> Beachten Sie, dass die Namen der HDInsight-Cluster **storm-BASENAME** und **hbase-BASENAME** lauten, wobei BASENAME der Name ist, den Sie für die Vorlage angegeben haben. Sie verwenden diese Namen in einem späteren Schritt, wenn Sie eine Verbindung mit den Clustern herstellen. Beachten Sie auch, dass der Name der Dashboardwebsite **basename-Dashboard**lautet. Dieser Wert wird weiter unten in diesem Dokument verwendet.

## <a name="configure-the-dashboard-bolt"></a>Konfigurieren des Dashboard-Bolts

Um Daten an das Dashboard zu senden, das als Web-App bereitgestellt ist, müssen Sie die folgende Zeile in der Datei `dev.properties` ändern:

```yaml
dashboard.uri: http://localhost:3000
```

Ändern Sie `http://localhost:3000` in `http://BASENAME-dashboard.azurewebsites.net`, und speichern Sie die Datei. Ersetzen Sie **BASENAME** durch den Basisnamen, den Sie im vorherigen Schritt angegeben haben. Sie können auch die zuvor erstellte Ressourcengruppe verwenden, um das Dashboard auszuwählen und die URL anzuzeigen.

## <a name="create-the-hbase-table"></a>Erstellen der HBase-Tabelle

Um Daten in HBase zu speichern, müssen wir zuerst eine Tabelle erstellen. Erstellen Sie Ressourcen, die Storm zum Schreiben benötigt, vorab, denn der Versuch, Ressourcen innerhalb einer Storm-Topologie zu erstellen, kann dazu führen, dass mehrere Instanzen versuchen, die gleiche Ressource zu erstellen. Erstellen Sie die Ressourcen außerhalb der Topologie, und verwenden Sie Storm für Lesen/Schreiben und Analysen.

1. Verwenden Sie SSH, um mit dem SSH-Benutzernamen und dem Kennwort, die Sie während der Erstellung des Clusters der Vorlage angegeben haben, eine Verbindung mit dem HBase-Cluster herzustellen. Wenn Sie beispielsweise eine Verbindung mit dem `ssh` -Befehl herstellen, verwenden Sie die folgende Syntax:
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    Ersetzen Sie `sshuser` durch den SSH-Benutzernamen, den Sie beim Erstellen des Clusters angegeben haben. Ersetzen Sie `clustername` durch den Namen des HBase-Clusters.

2. Starten Sie die HBase-Shell in der SSH-Sitzung.
   
    ```bash
    hbase shell
    ```
   
    Nachdem die Shell geladen wurde, sehen Sie eine `hbase(main):001:0>`-Eingabeaufforderung.

3. Geben Sie in der HBase-Befehlszeile den folgenden Befehl ein, um eine Tabelle zu erstellen, in der die Sensordaten gespeichert werden:
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. Stellen Sie mit folgendem Befehl sicher, dass die Tabelle erstellt wurde:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Hiermit sollten Informationen wie im folgenden Beispiel zurückgegeben werden, in dem angezeigt wird, dass die Tabelle 0 Zeilen enthält.
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Geben Sie `exit` ein, um HBase-Shell zu beenden:

## <a name="configure-the-hbase-bolt"></a>Konfigurieren des HBase-Bolts

Um vom Storm-Cluster aus in HBase zu schreiben, müssen Sie den HBase-Bolt mit den Konfigurationsdetails Ihres HBase-Clusters bereitstellen.

1. Verwenden Sie eines der folgenden Beispiele, um das Zookeeper-Quorum für den HBase-Cluster abzurufen:

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > Ersetzen Sie `your_HDInsight_cluster_name` durch den Namen Ihres HDInsight-Clusters. Weitere Informationen zum Installieren des Hilfsprogramms `jq` finden Sie unter [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).
    >
    > Geben Sie bei entsprechender Aufforderung das Kennwort für die SQL-HDInsight-Administratoranmeldung ein.

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name`
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > Ersetzen Sie „your_HDInsight_cluster_name“ durch den Namen Ihres HDInsight-Clusters. Geben Sie bei entsprechender Aufforderung das Kennwort für die SQL-HDInsight-Administratoranmeldung ein.
    >
    > Für dieses Beispiel ist Azure PowerShell erforderlich. Weitere Informationen zur Verwendung von Azure PowerShell finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6).

    Die in diesen Beispielen zurückgegebenen Informationen ähneln dem folgenden Text:

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    Diese Informationen werden in Storm für die Kommunikation mit dem HBase-Cluster verwendet.

2. Ändern Sie die Datei `dev.properties`, und fügen Sie die Informationen des Zookeeper-Quorums der folgenden Zeile hinzu:

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a>Erstellen, Verpacken und Bereitstellen der Lösung in HDInsight

Führen Sie die folgenden Schritte in Ihrer Entwicklungsumgebung aus, um die Storm-Topologie im Storm-Cluster bereitzustellen.

1. Führen Sie im Verzeichnis `TemperatureMonitor` den folgenden Befehl aus, um einen neuen Buildvorgang auszuführen und ein JAR-Paket aus Ihrem Projekt zu erstellen:
   
        mvn clean package
   
    Mit diesem Befehl wird die Datei `TemperatureMonitor-1.0-SNAPSHOT.jar in the ` Zielverzeichnis Ihres Projekts erstellt.

2. Verwenden Sie „scp“ zum Hochladen der Dateien `TemperatureMonitor-1.0-SNAPSHOT.jar` und `dev.properties` in Ihren Storm-Cluster. Ersetzen Sie im folgenden Beispiel `sshuser` durch den SSH-Benutzer, den Sie beim Erstellen des Clusters angegeben haben, und `clustername` durch den Namen Ihres Storm-Clusters. Geben Sie nach Aufforderung das Kennwort für den SSH-Benutzer ein.
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > Das Hochladen der Dateien kann einige Minuten dauern.

    Weitere Informationen zum Verwenden der Befehle `scp` und `ssh` mit HDInsight finden Sie unter [Verwenden von SSH mit HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).

3. Nachdem die Datei hochgeladen wurde, stellen Sie über SSH eine Verbindung mit dem Storm-Cluster her.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Ersetzen Sie `sshuser` durch den SSH-Benutzernamen. Ersetzen Sie `clustername` durch den Namen des Storm-Clusters.

4. Verwenden Sie zum Starten der Topologie den folgenden Befehl in der SSH-Sitzung:
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * Mit `--remote` wird die Topologie an den Nimbus-Dienst gesendet, der sie an die Supervisorknoten im Cluster verteilt.
    * Mit `--filter` wird die Datei `dev.properties` zum Auffüllen der Parameter in der Topologiedefinition verwendet.
    * Mit `-R /with-hbase.yaml` wird die im Paket enthaltene Topologie `with-hbase.yaml` verwendet.

5. Nachdem die Topologie gestartet wurde, öffnen Sie die Website, die Sie in Azure veröffentlicht haben, in einem Browser, und verwenden Sie dann den Befehl `node app.js` zum Senden von Daten an Event Hub. Das Webdashboard sollte mit den Informationen aktualisiert werden.
   
    ![Dashboard](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a>Anzeigen von HBase-Daten

Befolgen Sie die folgenden Schritte, um eine Verbindung mit HBase herzustellen und zu überprüfen, ob die Daten in die Datei geschrieben wurden:

1. Verwenden Sie SSH zum Herstellen einer Verbindung mit dem HBase-Cluster.
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    Ersetzen Sie `sshuser` durch den SSH-Benutzernamen. Ersetzen Sie `clustername` durch den Namen des HBase-Clusters.

2. Starten Sie die HBase-Shell in der SSH-Sitzung.
   
    ```bash
    hbase shell
    ```
   
    Nachdem die Shell geladen wurde, sehen Sie eine `hbase(main):001:0>`-Eingabeaufforderung.

3. Anzeigen von Zeilen aus der Tabelle:
   
    ```hbase
    scan 'SensorData'
    ```
   
    Durch diesen Befehl sollten Informationen wie im folgenden Text zurückgegeben werden, in dem angezeigt wird, dass die Tabelle Daten enthält.
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > Dieser Scanvorgang gibt maximal 10 Zeilen aus der Tabelle zurück.

## <a name="delete-your-clusters"></a>Löschen der Cluster

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Um die Cluster, den Speicher und die Web-App auf einmal zu löschen, löschen Sie die Ressourcengruppe, die sie enthält.

## <a name="next-steps"></a>Nächste Schritte

Weitere Beispiel-Storm-Topologien finden Sie unter [Beispiel-Storm-Topologien für Storm auf HDInsight](hdinsight-storm-example-topology.md).

Weitere Informationen zu Apache Storm finden Sie auf der [Apache Storm](https://storm.incubator.apache.org/) -Website.

Weitere Informationen zu HBase in HDInsight finden Sie unter [HBase mit HDInsight: Übersicht](hdinsight-hbase-overview.md).

Weitere Informationen über Socket.io finden Sie auf der [socket.io](http://socket.io/) -Website (in englischer Sprache).

Weitere Informationen zu D3.js finden Sie unter [D3.js – Data Driven Documents](http://d3js.org/)(in englischer Sprache).

Informationen zum Erstellen von Topologien in Java finden Sie unter [Entwickeln von Java-Topologien für Apache Storm in HDInsight](hdinsight-storm-develop-java-topology.md).

Informationen zum Erstellen von Topologien in .NET finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com

