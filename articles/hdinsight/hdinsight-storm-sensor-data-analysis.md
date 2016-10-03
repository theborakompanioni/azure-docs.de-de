<properties
   pageTitle="Analysieren von Sensordaten mit Apache Storm und HBase | Microsoft Azure"
   description="Erfahren Sie, wie Sie mit einem virtuellen Netzwerk eine Verbindung mit Apache Storm herstellen. Verwenden Sie Storm mit HBase, um Sensordaten von einem Event Hub zu verarbeiten und mit D3.js darzustellen."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/20/2016"
   ms.author="larryfr"/>

# Analysieren von Sensordaten mit Apache Storm, Event Hub und HBase in HDInsight (Hadoop) 

Erfahren Sie, wie Sie Apache Storm in HDInsight verwenden, um Sensordaten von Azure Event Hubs zu verarbeiten, in Apache HBase auf HDInsight zu speichern und mit D3.js – ausgeführt als Azure-Web-App – zu visualisieren.

Die in diesem Dokument verwendete Azure Resource Manager-Vorlage veranschaulicht das Erstellen mehrerer Azure-Ressourcen in einer Ressourcengruppe. Insbesondere werden ein virtuelles Netzwerk in Azure, zwei HDInsight-Cluster (Storm und HBase) und eine Azure-Web-App erstellt. Eine node.js-Implementierung eines Echtzeit-Webdashboards wird automatisch für die Web-App bereitgestellt.

> [AZURE.NOTE] Die Informationen in diesem Dokument und das präsentierte Beispiel wurden mit Linux-basierten HDInsight 3.3- und 3.4-Clusterversionen getestet.

## Voraussetzungen

* Ein Azure-Abonnement. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (in englischer Sprache).

    > [AZURE.IMPORTANT] Sie benötigen keinen vorhandenen HDInsight-Cluster; die Schritte in diesem Dokument erstellen die folgenden Ressourcen:
    >
    > * Ein virtuelles Azure-Netzwerk
    > * Einen Storm-Cluster in HDInsight (Linux-basiert, 2 Workerknoten)
    > * Einen HBase-Cluster in HDInsight (Linux-basiert, 2 Workerknoten)
    > * Eine Azure-Web-App, die das Webdashboard hostet

* [Node.js](http://nodejs.org/): Wird für eine lokale Vorschau des Webdashboards in Ihrer Entwicklungsumgebung verwendet.

* [Java und JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Wird zum Entwickeln der Storm-Topologie verwendet.

* [Maven](http://maven.apache.org/what-is-maven.html): Dient zum Erstellen und Kompilieren des Projekts.

* [Git](http://git-scm.com/): Dient zum Herunterladen des Projekts von GitHub.

* Ein __SSH__-Client: Dient zum Herstellen der Verbindung mit den Linux-basierten HDInsight-Clustern. Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten.

    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    > [AZURE.NOTE] Außerdem benötigen Sie Zugriff auf den `scp`-Befehl, der zum Kopieren von Dateien zwischen Ihrer lokalen Entwicklungsumgebung und dem HDInsight-Cluster über SSH verwendet wird.

## Architektur

![Architekturdiagramm](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

Dieses Beispiel umfasst die folgenden Komponenten:

* **Azure Event Hubs**: Enthalten von Sensoren gesammelte Daten. In diesem Beispiel wird eine Anwendung bereitgestellt, die die Daten generiert.

* **Storm in HDInsight**: Bietet Echtzeitverarbeitung von Event Hub-Daten.

* **HBase in HDInsight**: Stellt einen persistenten NoSQL-Datenspeicher für Daten bereit, nachdem sie von Storm verarbeitet wurden.

* **Azure Virtual Network-Dienst**: Ermöglicht die sichere Kommunikation zwischen Storm in HDInsight-Clustern und HBase in HDInsight-Clustern.

    > [AZURE.NOTE] Ein virtuelles Netzwerk ist erforderlich, um die Java-HBase-Client-API zu verwenden, da sie nicht über das öffentliche Gateway für HBase-Cluster verfügbar gemacht wird. Die Installation von HBase- und Storm-Clustern in dem gleichen virtuellen Netzwerk ermöglicht dem Storm-Cluster (oder beliebigen anderen Systemen im virtuellen Netzwerk) direkten HBase-Zugriff mithilfe der Client-API.

* **Dashboard-Website**: Ein Beispiel-Dashboard, das Daten in Echtzeit aufzeichnet.

	* Die Website ist in "Node.js" implementiert, sodass sie auf jedem Clientbetriebssystem für Tests ausgeführt oder auf Azure-Websites bereitgestellt werden kann.

	* [Socket.io](http://socket.io/) wird für die Echtzeitkommunikation zwischen der Storm-Topologie und der Website verwendet.

		> [AZURE.NOTE] Dies ist ein Implementierungsdetail. Sie können alle Kommunikationsframeworks, wie z. B. reine WebSockets oder SignalR, verwenden.

	* [D3.js](http://d3js.org/) wird verwendet, um die Daten grafisch darzustellen, die an die Website gesendet werden.

Die Topologie liest mithilfe der [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html)-Klasse Daten aus Event Hub und schreibt mithilfe der [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/javadoc/apidocs/org/apache/storm/hbase/bolt/class-use/HBaseBolt.html)-Klasse Daten in HBase. Kommunikation mit der Website erfolgt mithilfe von [socket.io client.java](https://github.com/nkzawa/socket.io-client.java).

Hier sehen Sie eine Darstellung der Topologie:

![Topologiediagramm](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [AZURE.NOTE] Dies ist eine äußerst vereinfachte Ansicht der Topologie. Zur Laufzeit wird eine Instanz jeder Komponente für jede Partition des Event Hubs erstellt, der gelesen wird. Diese Instanzen werden über die Knoten im Cluster verteilt, und Daten werden wie folgt zwischen ihnen weitergeleitet:
>
> * Für Daten vom Spout an den Parser wird ein Lastenausgleich ausgeführt.
> * Vom Parser zum Dashboard und zu HBase übertragene Daten werden nach Geräte-ID gruppiert, sodass Nachrichten vom gleichen Gerät immer an die gleiche Komponente weitergeleitet werden.

### Topologiekomponenten

* **EventHub-Spout**: Der Spout wird als Teil der Apache-Storm-Version 0.10.0 und höher bereitgestellt.

    > [AZURE.NOTE] Für den in diesem Beispiel verwendeten Event Hubs-Spout ist ein Storm-Cluster der Version 3.3 oder 3.4 in HDInsight erforderlich. Informationen zur Verwendung von Event Hubs mit einer älteren Version von Storm in HDInsight finden Sie unter [Verarbeitung von Ereignissen von Azure Event Hubs mit Storm auf HDInsight](hdinsight-storm-develop-java-event-hub-topology.md).

* **ParserBolt.java**: Der Spout gibt reine JSON-Daten aus, und gelegentlich werden mehrere Ereignisse gleichzeitig ausgegeben. Dieser Bolt veranschaulicht, wie vom Spout ausgegebene Daten gelesen und als ein Tupel, das mehrere Felder enthält, an einen neuen Datenstrom ausgegeben werden.

* **DashboardBolt.java**: Dies veranschaulicht, wie die Socket.io-Clientbibliothek für Java zum Senden von Daten an das Webdashboard in Echtzeit verwendet wird.

Da in diesem Beispiel das [Flux](https://storm.apache.org/releases/0.10.0/flux.html)-Framework verwendet wird, ist die Topologiedefinition in YAML-Dateien enthalten. Es gibt zwei:

* __no-hbase.yaml__: Verwenden Sie diese Datei, wenn Sie die Topologie in Ihrer Entwicklungsumgebung testen. Es werden keine HBase-Komponenten verwendet, da Sie von außerhalb des virtuellen Netzwerks, in dem sich der Cluster befindet, nicht auf die HBase-Java-API zugreifen können.

* __with-hbase.yaml__: Verwenden Sie diese Datei, wenn Sie die Topologie im Storm-Cluster bereitstellen. Es werden HBase-Komponenten verwendet, da die Ausführung in demselben virtuellen Netzwerk wie für den HBase-Cluster erfolgt.

## Vorbereiten der Umgebung

Bevor Sie dieses Beispiel verwenden können, müssen Sie einen Azure Event Hub erstellen, der von der Storm-Topologie gelesen wird.

### Konfigurieren von Event Hub

Event Hub ist die Datenquelle für dieses Beispiel. Führen Sie die folgenden Schritte aus, um einen neuen Event Hub zu erstellen.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) die Optionen **+ Neu** > __Internet der Dinge (IoT)__ > __Event Hubs__.

2. Führen Sie auf dem Blatt __Namespace erstellen__ die folgenden Aufgaben durch:

    1. Geben Sie einen __Namen__ für den Namespace ein.
    2. Wählen Sie einen Tarif. __Basic__ ist für dieses Beispiel ausreichend.
    3. Wählen Sie aus, welches Azure-__Abonnement__ verwendet werden soll.
    4. Wählen Sie eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue Ressourcengruppe.
    5. Wählen Sie den __Speicherort__ für den Event Hub aus.
    6. Wählen Sie __An Dashboard anheften__, und klicken Sie dann auf __Erstellen__.

3. Nachdem der Erstellungsvorgang abgeschlossen ist, wird das Blatt „Event Hubs“ für Ihren Namespace angezeigt. Wählen Sie hier die Option __+ Event Hub hinzufügen__. Geben Sie auf dem Blatt __Event Hub erstellen__ den Namen __sensordata__ ein, und wählen Sie __Erstellen__. Behalten Sie für die anderen Felder den Standardwert bei.

4. Wählen Sie auf dem Blatt „Event Hubs“ für Ihren Namespace die Option __Event Hubs__. Wählen Sie den Eintrag __sensordata__ aus.

5. Wählen Sie auf dem Blatt für den Event Hub „sensordata“ die Option __Richtlinien für gemeinsamen Zugriff__. Verwenden Sie den Link __+ Hinzufügen__, um die folgenden Richtlinien hinzuzufügen:


	| Richtlinienname | Ansprüche |
    | ----- | ----- |
	| devices | Senden |
	| storm | Empfangen |

5. Wählen Sie beide Richtlinien aus, und notieren Sie sich den Wert von __PRIMÄRER SCHLÜSSEL__. Sie benötigen den Wert für beide Richtlinien für die weiteren Schritte.

## Herunterladen und Konfigurieren des Projekts

Führen Sie den folgenden Befehl aus, um das Projekt von GitHub herunterzuladen.

	git clone https://github.com/Blackmist/hdinsight-eventhub-example

Nach Abschluss des Befehls verfügen Sie über die folgende Verzeichnisstruktur:

	hdinsight-eventhub-example/
		TemperatureMonitor/ - this contains the topology
			resources/
                log4j2.xml - set logging to minimal
                no-hbase.yaml - topology definition for local testing
                with-hbase.yaml - topology definition that uses HBase in a virutal network
			src/ - the Java bolts
            dev.properties - contains configuration values for your environment
		dashboard/nodejs/ - this is the node.js web dashboard
		SendEvents/ - utilities to send fake sensor data

> [AZURE.NOTE] Dieses Dokument geht nicht ausführlich auf alle Details des in diesem Beispiel enthaltenen Codes ein. Der Code ist jedoch vollständig kommentiert.

Öffnen Sie die Datei **hdinsight-eventhub-example/TemperatureMonitor/dev.properties**, und fügen Sie Ihre Event Hub-Informationen den folgenden Zeilen hinzu:

    eventhub.read.policy.name: storm
    eventhub.read.policy.key: KeyForTheStormPolicy
    eventhub.namespace: YourNamespace
    eventhub.name: sensordata

> [AZURE.NOTE] In diesem Beispiel wird vorausgesetzt, dass Sie __storm__ als Name der Richtlinie mit dem Anspruch __Listen__ (Lauschen) verwendet haben und dass Ihr Event Hub den Namen __sensordata__ hat.

 Speichern Sie die Datei, nachdem Sie diese Informationen hinzugefügt haben.

## Kompilieren und lokales Testen

Vor dem Testen müssen Sie das Dashboard zur Anzeige der Ausgabe der Topologie und zum Generieren der Daten starten, die im Event Hub gespeichert werden sollen.

> [AZURE.IMPORTANT] Die HBase-Komponente dieser Topologie ist beim lokalen Testen nicht aktiv, da von außerhalb des virtuellen Azure-Netzwerks, das die Cluster enthält, kein Zugriff auf die Java-API für den HBase-Cluster möglich ist.

### Starten der Webanwendung

1. Öffnen Sie eine neue Eingabeaufforderung oder ein Terminal, und wechseln Sie ins Verzeichnis **hdinsight-eventhub-example/dashboard**. Verwenden Sie dann folgenden Befehl, um die von der Webanwendung benötigten Abhängigkeiten zu installieren:

		npm install

2. Verwenden Sie den folgenden Befehl, um die Webanwendung zu starten.

		node server.js

	Es wird eine Meldung ähnlich der folgenden angezeigt:

		Server listening at port 3000

2. Öffnen Sie einen Webbrowser, und geben Sie **http://localhost:3000/** als Adresse ein. Eine Seite ähnlich der folgenden wird angezeigt:

	![Webdashboard](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)

	Lassen Sie die Eingabeaufforderung oder das Terminal geöffnet. Verwenden Sie nach dem Testen STRG+C, um den Webserver zu beenden.

### Starten der Datengenerierung

> [AZURE.NOTE] Die Schritte in diesem Abschnitt verwenden „Node.js“, sodass sie auf einer beliebigen Plattform ausgeführt werden können. Andere Sprachbeispiele finden Sie im **SendEvents**-Verzeichnis.

1. Öffnen Sie eine neue Eingabeaufforderung, eine Shell oder ein neues Terminal, und wechseln Sie ins Verzeichnis **hdinsight-eventhub-example/SendEvents/nodejs**. Verwenden Sie dann den folgenden Befehl, um die von der Anwendung benötigten Abhängigkeiten zu installieren:

		npm install

2. Öffnen Sie die Datei **app.js** in einem Text-Editor, und fügen Sie die Event Hub-Informationen hinzu, die Sie zuvor abgerufen haben:

		// ServiceBus Namespace
		var namespace = 'YourNamespace';
		// Event Hub Name
		var hubname ='sensordata';
		// Shared access Policy name and key (from Event Hub configuration)
		var my_key_name = 'devices';
		var my_key = 'YourKey';
    
    > [AZURE.NOTE] In diesem Beispiel wird vorausgesetzt, dass Sie __sensordata__ als Name für den Event Hub und __devices__ als Name für die Richtlinie mit dem Anspruch __Send__ (Senden) verwendet haben.

2. Verwenden Sie den folgenden Befehl zum Einfügen neuer Einträge in Event Hub:

		node app.js

	Es sollten mehrere Ausgabezeilen angezeigt werden, die die an Event Hub gesendeten Daten enthalten. Dies sieht in etwa wie folgt aus:

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

### Starten der Topologie

2. Öffnen Sie ein neues Eingabeaufforderungsfenster, eine Shell oder ein Terminal, und wechseln Sie zum Verzeichnis __hdinsight-eventhub-example/TemperatureMonitor__. Verwenden Sie dann den folgenden Befehl, um die Topologie zu starten:

        mvn compile exec:java -Dexec.args="--local -R /no-hbase.yaml --filter dev.properties"
    
    Gehen Sie wie folgt vor, wenn Sie stattdessen PowerShell verwenden:

        mvn compile exec:java "-Dexec.args=--local -R /no-hbase.yaml --filter dev.properties"

    > [AZURE.NOTE] Wenn Sie sich auf einem System mit Linux/Unix/OS X befinden und [Storm in der Entwicklungsumgebung installiert haben](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), können Sie stattdessen die folgenden Befehle verwenden:
    >
    > `mvn compile package` `storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /no-hbase.yaml`

	Hiermit wird die Topologie, die in der Datei __no-hbase.yaml__ definiert ist, im lokalen Modus gestartet. Die Werte in der Datei __dev.properties__ enthalten die Verbindungsinformationen für Event Hubs. Nach dem Starten liest die Topologie Einträge aus dem Event Hub und sendet sie an das Dashboard, das auf Ihrem lokalen Computer ausgeführt wird. Im Webdashboard sollten nun Linien angezeigt werden, die den folgenden ähneln:

	![Dashboard mit Daten](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

3. Verwenden Sie während der Ausführung des Dashboards den Befehl `node app.js` aus den vorherigen Schritten, um neue Daten an Event Hubs zu senden. Da die Temperaturwerte zufällig generiert werden, sollte das Diagramm aktualisiert werden, um große Temperaturänderungen anzuzeigen.

    > [AZURE.NOTE] Sie müssen sich im Verzeichnis __hdinsight-eventhub-example/SendEvents/Nodejs__ befinden, wenn Sie den Befehl `node app.js` verwenden.

3. Beenden Sie die Topologie durch Eingabe von STRG+C, nachdem Sie überprüft haben, dass dies funktioniert. Sie können STRG+C auch verwenden, um den lokalen Webserver zu beenden.

## Erstellen eines Storm- und HBase-Clusters

Um die Topologie in HDInsight auszuführen und den HBase-Bolt zu aktivieren, müssen Sie einen neuen Storm-Cluster und HBase-Cluster erstellen. In den Schritten in diesem Abschnitt werden mit einer [Azure Resource Manager-Vorlage](../resource-group-template-deploy.md) ein neues virtuelles Azure-Netzwerk sowie ein Storm- und HBase-Cluster im virtuellen Netzwerk erstellt. Die Vorlage erstellt auch eine Azure-Web-App und stellt eine Kopie des Dashboards darin bereit.

> [AZURE.NOTE] Ein virtuelles Netzwerk wird verwendet, sodass die Topologie im Storm-Cluster direkt mithilfe der HBase-Java-API mit dem HBase-Cluster kommunizieren kann.

Die in diesem Dokument verwendete Resource Manager-Vorlage befindet sich in einem öffentlichen Blobcontainer unter\_\_https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet.json__.

1. Klicken Sie auf die folgende Schaltfläche, um sich bei Azure anzumelden und die Resource Manager-Vorlage im Azure-Portal zu öffnen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Geben Sie auf dem Blatt **Parameter** Folgendes ein:

    ![HDInsight-Parameter](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
    
    * **BASECLUSTERNAME**: Dieser Wert wird als Basisname für den Storm- und HBase-Cluster verwendet. Bei Eingabe von __hdi__ wird z.B. ein Storm-Cluster mit dem Namen __storm-hdi__ und ein HBase-Cluster mit dem Namen __hbase-hdi__ erstellt.
    * __CLUSTERLOGINUSERNAME__: Der Administratorbenutzername für den Storm- und HBase-Cluster.
    * __CLUSTERLOGINPASSWORD__: Das Administratorbenutzerkennwort für den Storm- und HBase-Cluster.
    * __SSHUSERNAME__: Der für den Storm- und HBase-Cluster zu erstellende SSH-Benutzer.
    * __SSHPASSWORD__: Das Kennwort für den SSH-Benutzer für Storm- und HBase-Cluster.
    * __LOCATION__: Die Region, in der die Cluster erstellt werden.
    
    Klicken Sie auf __OK__, um die Parameter zu speichern.
    
3. Erstellen Sie im Abschnitt __Ressourcengruppe__ eine neue Ressourcengruppe, oder wählen Sie eine vorhandene aus.

4. Wählen Sie im Dropdownmenü __Ressourcengruppenstandort__ den gleichen Speicherort aus, den Sie für den Parameter __LOCATION__ ausgewählt haben.

5. Wählen Sie __Rechtliche Bedingungen__ und dann __Erstellen__.

6. Aktivieren Sie zum Schluss __An Dashboard anheften__, und wählen Sie dann __Erstellen__. Das Erstellen der Cluster dauert etwa 20 Minuten.

Sobald die Ressourcen erstellt sind, werden Sie zu einem Blatt für die Ressourcengruppe weitergeleitet, das die Cluster und das Webdashboard enthält.

![Ressourcengruppenblatt für vnet und Cluster](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [AZURE.IMPORTANT] Beachten Sie, dass die Namen der HDInsight-Cluster __storm-BASENAME__ und __hbase-BASENAME__ lauten, wobei BASENAME der Name ist, den Sie für die Vorlage bereitgestellt haben. Sie werden diese Namen in späteren Schritten verwenden, wenn Sie eine Verbindung mit den Clustern herstellen. Beachten Sie auch, dass der Name der Dashboardwebsite __basename-Dashboard__ lautet. Sie verwenden ihn später, wenn Sie das Dashboard anzeigen.

## Konfigurieren des Dashboard-Bolts

Um Daten an das Dashboard zu senden, das als Web-App bereitgestellt ist, müssen Sie die folgende Zeile in der Datei __dev.properties__ ändern:

    dashboard.uri: http://localhost:3000

Ändern Sie `http://localhost:3000` in `http://BASENAME-dashboard.azurewebsites.net`, und speichern Sie die Datei. Ersetzen Sie __BASENAME__ durch den Basisnamen, den Sie im vorherigen Schritt angegeben haben. Sie können auch die zuvor erstellte Ressourcengruppe verwenden, um das Dashboard auszuwählen und die URL anzuzeigen.

## Erstellen der HBase-Tabelle

Um Daten in HBase zu speichern, müssen wir zuerst eine Tabelle erstellen. Im Allgemeinen werden Sie Ressourcen, die Storm zum Schreiben benötigt, vorab erstellen, denn der Versuch, Ressourcen innerhalb einer Storm-Topologie zu erstellen, kann dazu führen, dass mehrere verteilte Kopien des Codes versuchen, die gleiche Ressource zu erstellen. Erstellen Sie die Ressourcen außerhalb der Topologie, und verwenden Sie nur Storm für Lesen/Schreiben und Analysen.

1. Verwenden Sie SSH, um mit dem SSH-Benutzernamen und dem Kennwort, die Sie während der Erstellung des Clusters der Vorlage angegeben haben, eine Verbindung mit dem HBase-Cluster herzustellen. Wenn Sie beispielsweise eine Verbindung mit dem `ssh`-Befehl herstellen, verwenden Sie die folgende Syntax:

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net
    
    Ersetzen Sie in diesem Befehl __USERNAME__ mit dem SSH-Benutzernamen, den Sie beim Erstellen des Clusters bereitgestellt, und __BASENAME__ mit dem Basisnamen, den Sie angegeben haben. Geben Sie nach Aufforderung das Kennwort für den SSH-Benutzer ein.

2. Starten Sie die HBase-Shell in der SSH-Sitzung.

    	hbase shell
	
	Nachdem die Shell geladen wurde, sehen Sie eine `hbase(main):001:0>`-Eingabeaufforderung.

3. Geben Sie in der HBase-Befehlszeile den folgenden Befehl ein, um eine Tabelle zu erstellen, in der die Sensordaten gespeichert werden:

    	create 'SensorData', 'cf'

4. Stellen Sie mit folgendem Befehl sicher, dass die Tabelle erstellt wurde:

    	scan 'SensorData'
		
	Hiermit sollten Informationen wie im folgenden Beispiel zurückgegeben werden, in dem angezeigt wird, dass die Tabelle 0 Zeilen enthält.
	
		ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. Beenden Sie die HBase-Shell durch folgende Eingabe:

		exit

## Konfigurieren des HBase-Bolts

Um vom Storm-Cluster aus in HBase zu schreiben, müssen Sie den HBase-Bolt mit den Konfigurationsdetails Ihres HBase-Clusters bereitstellen. Die einfachste Möglichkeit hierzu ist, __hbase-site.xml__ aus dem Cluster herunterzuladen und in Ihr Projekt aufzunehmen. Sie müssen auch mehrere Abhängigkeiten in der __pom.xml__-Datei auskommentieren, die die storm-hbase-Komponente und dafür erforderliche Abhängigkeiten laden.

> [AZURE.IMPORTANT] Außerdem müssen Sie die Datei „storm-hbase.jar“ herunterladen, die auf Ihren HDInsight 3.3- und 3.4-Storm-Clustern bereitgestellt wird. Diese Version ist zur Funktion mit HBase 1.1.x kompiliert, das für HBase auf HDInsight 3.3- und 3.4-Clustern verwendet wird. Wenn Sie eine storm-hbase-Komponente aus anderer Quelle verwenden, könnte sie für eine ältere Version von HBase kompiliert worden sein.

### Herunterladen von „hbase-site.xml“

Verwenden Sie über eine Befehlszeile SCP zum Herunterladen der Datei __hbase-site.xml__ aus dem Cluster. Ersetzen Sie im folgenden Beispiel __USERNAME__ mit dem SSH-Benutzernamen, den Sie beim Erstellen des Clusters bereitgestellt, und __BASENAME__ mit dem Basisnamen, den Sie früher angegeben haben. Geben Sie nach Aufforderung das Kennwort für den SSH-Benutzer ein. Ersetzen Sie `/path/to/TemperatureMonitor/resources/hbase-site.xml` durch den Pfad zu dieser Datei im TemperatureMonitor-Projekt.

    scp USERNAME@hbase-BASENAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml /path/to/TemperatureMonitor/resources/hbase-site.xml

Damit wird __hbase-site.xml__ in den angegebenen Pfad heruntergeladen.

### Herunterladen und Installieren der storm-hbase-Komponente

1. Verwenden Sie über eine Befehlszeile SCP zum Herunterladen der Datei __storm-hbase.jar__ aus dem Storm-Cluster. Ersetzen Sie im folgenden Beispiel __USERNAME__ mit dem SSH-Benutzernamen, den Sie beim Erstellen des Clusters bereitgestellt, und __BASENAME__ mit dem Basisnamen, den Sie früher angegeben haben. Geben Sie nach Aufforderung das Kennwort für den SSH-Benutzer ein.

        scp USERNAME@storm-BASENAME-ssh.azurehdinsight.net:/usr/hdp/current/storm-client/contrib/storm-hbase/storm-hbase*.jar .

    Damit wird eine Datei namens `storm-hbase-####.jar` heruntergeladen, wobei ### die Versionsnummer von Storm für diesen Cluster ist. Notieren Sie sich diese Nummer, da sie später verwendet wird.

2. Verwenden Sie den folgenden Befehl, um diese Komponente in Ihrer Entwicklungsumgebung im lokalen Maven-Repository zu installieren. So kann Maven das Paket beim Kompilieren des Projekts finden. Ersetzen Sie __####__ mit der Versionsnummer, die im Dateinamen enthalten ist.

        mvn install:install-file -Dfile=storm-hbase-####.jar -DgroupId=org.apache.storm -DartifactId=storm-hbase -Dversion=#### -Dpackaging=jar
    
    Verwenden Sie den folgenden Befehl, wenn Sie PowerShell nutzen:

        mvn install:install-file "-Dfile=storm-hbase-####.jar" "-DgroupId=org.apache.storm" "-DartifactId=storm-hbase" "-Dversion=####" "-Dpackaging=jar"

### Aktivieren der storm-hbase-Komponente im Projekt

1. Öffnen Sie die Datei __TemperatureMonitor/pom.xml__, und löschen Sie die folgenden Zeilen:

        <!-- uncomment this section to enable the hbase-bolt
        end comment for hbase-bolt section -->
    
    > [AZURE.IMPORTANT] Löschen Sie nur diese zwei Zeilen; löschen Sie keine dazwischen liegenden Zeilen.
    
    Dies aktiviert mehrere Komponenten, die zur Kommunikation mit HBase mithilfe des hbase-Bolts erforderlich sind.

2. Suchen Sie die folgenden Zeilen, und ersetzen Sie __####__ mit der Versionsnummer der storm-hbase-Datei, die Sie zuvor heruntergeladen haben.

        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>storm-hbase</artifactId>
            <version>####</version>
        </dependency>

    > [AZURE.IMPORTANT] Die Versionsnummer muss mit der Version übereinstimmen, die Sie bei der Installation der Komponente im lokalen Maven-Repository verwendet haben, denn Maven verwendet diese Informationen, um die Komponente beim Erstellen des Projekts zu laden.

2. Speichern Sie die Datei __pom.xml__.

## Erstellen, Verpacken und Bereitstellen der Lösung in HDInsight

Führen Sie die folgenden Schritte in Ihrer Entwicklungsumgebung aus, um die Storm-Topologie im Storm-Cluster bereitzustellen.

1. Führen Sie im Verzeichnis __TemperatureMonitor__ den folgenden Befehl aus, um einen neuen Buildvorgang auszuführen und ein JAR-Paket aus Ihrem Projekt zu erstellen:

		mvn clean compile package

	Daraufhin wird eine Datei mit dem Namen **TemperatureMonitor-1.0-SNAPSHOT.jar** im Verzeichnis **target** für Ihr Projekt erstellt.

2. Verwenden Sie scp, um die Datei __TemperatureMonitor-1.0-SNAPSHOT.jar__ auf Ihren Storm-Cluster hochzuladen. Ersetzen Sie im folgenden Beispiel __USERNAME__ mit dem SSH-Benutzernamen, den Sie beim Erstellen des Clusters bereitgestellt, und __BASENAME__ mit dem Basisnamen, den Sie früher angegeben haben. Geben Sie nach Aufforderung das Kennwort für den SSH-Benutzer ein.

        scp target\TemperatureMonitor-1.0-SNAPSHOT.jar USERNAME@storm-BASENAME-ssh.azurehdinsight.net:TemperatureMonitor-1.0-SNAPSHOT.jar
    
    > [AZURE.NOTE] Das Hochladen der Datei kann mehrere Minuten dauern, da sie mehrere Megabyte groß sein wird.

    Verwenden Sie „scp“ zum Hochladen der Datei __dev.properties__, da sie die Informationen enthält, die zum Herstellen der Verbindung mit Event Hubs und dem Dashboard verwendet werden.

        scp dev.properties USERNAME@storm-BASENAME-ssh.azurehdinsight.net:dev.properties

3. Stellen Sie nach dem Hochladen der Dateien über SSH eine Verbindung mit dem Cluster her.

        ssh USERNAME@storm-BASENAME-ssh.azurehdinsight.net

4. Verwenden Sie in der SSH-Sitzung den folgenden Befehl, um die Topologie zu starten.

        storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /with-hbase.yaml --filter dev.properties
    
    Die Topologie wird mit der Topologiedefinition in der Datei __with-hbase.yaml__ und den Konfigurationswerten in der Datei __dev.properties__ gestartet.

3. Nachdem die Topologie gestartet wurde, öffnen Sie die Website, die Sie in Azure veröffentlicht haben, in einem Browser, und verwenden Sie dann den Befehl `node app.js` zum Senden von Daten an Event Hub. Das Webdashboard sollte mit den Informationen aktualisiert werden.

	![Dashboard](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## Anzeigen von HBase-Daten

Nachdem Sie die Daten mit `node app.js` zur Topologie gesendet haben, stellen Sie mit den folgenden Schritten die Verbindung mit HBase her, und stellen Sie sicher, dass die Daten in die Tabelle geschrieben wurden, die Sie zuvor erstellt haben.

1. Verwenden Sie SSH zum Herstellen einer Verbindung mit dem HBase-Cluster.

        ssh USERNAME@hbase-BASENAME-ssh.azurehdinsight.net

2. Starten Sie die HBase-Shell in der SSH-Sitzung.

    	hbase shell
	
	Nachdem die Shell geladen wurde, sehen Sie eine `hbase(main):001:0>`-Eingabeaufforderung.

2. Anzeigen von Zeilen aus der Tabelle:

    	scan 'SensorData'
		
	Dies sollte Informationen ähnlich den folgenden zurückgeben, die darauf hinweisen, dass die Tabelle 0 Zeilen enthält.
	
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

    > [AZURE.NOTE] Dieser Scanvorgang wird nur bis zu 10 Zeilen aus der Tabelle zurückgeben.

## Löschen der Cluster

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Um die Cluster, den Speicher und die Web-App auf einmal zu löschen, löschen Sie die Ressourcengruppe, die sie enthält.

## Nächste Schritte

Sie haben gelernt, wie Sie Daten mithilfe von Storm aus Event Hubs lesen, sie in HBase speichern und die Informationen mit „Socket.io“ und „D3.js“ auf einem externen Dashboard visualisieren.

* Weitere Beispiele für Storm-Topologien mit HDInsight finden Sie unter:

    * [Beispiele für Storm-Topologien für Storm in HDInsight](hdinsight-storm-example-topology.md)

* Weitere Informationen zu Apache Storm finden Sie auf der [Apache Storm](https://storm.incubator.apache.org/)-Website.

* Weitere Informationen zu HBase in HDInsight finden Sie unter [HBase mit HDInsight: Übersicht](hdinsight-hbase-overview.md).

* Weitere Informationen über Socket.io finden Sie auf der [socket.io](http://socket.io/)-Website (in englischer Sprache).

* Weitere Informationen zu D3.js finden Sie unter [D3.js – Data Driven Documents](http://d3js.org/) (in englischer Sprache).

* Informationen zum Erstellen von Topologien in Java finden Sie unter [Entwickeln von Java-Topologien für Apache Storm in HDInsight](hdinsight-storm-develop-java-topology.md).

* Informationen zum Erstellen von Topologien in .NET finden Sie unter [Entwickeln von C#-Topologien für Apache Storm in HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

[azure-portal]: https://portal.azure.com

<!---HONumber=AcomDC_0921_2016-->