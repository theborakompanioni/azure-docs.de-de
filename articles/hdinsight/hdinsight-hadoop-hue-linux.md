<properties
	pageTitle="Verwenden von Hue mit Hadoop auf HDInsight Linux-Clustern | Microsoft Azure"
	description="Informationen zum Installieren und Verwenden von Hue mit Hadoop-Clustern für HDInsight Linux."
	services="hdinsight"
	documentationCenter=""
	authors="nitinme"
	manager="jhubbard"
	editor="cgronlun"/>

<tags 
	ms.service="hdinsight" 
	ms.workload="big-data" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/13/2016" 
	ms.author="nitinme"/>

# Installieren und Verwenden von Hue in HDInsight Hadoop-Clustern

Erfahren Sie, wie Sie Hue auf HDInsight Linux-Clustern installieren und die Anfragen mittels Tunneln an Hue weiterleiten.

## Was ist Hue?

Bei Hue handelt es sich um einen Satz von Webanwendungen zur Interaktion mit einem Hadoop-Cluster. Mit Hue können Sie den mit einem Hadoop-Cluster verknüpften Speicher (WASB bei HDInsight-Clustern) durchsuchen, Hive-Aufträge und Pig-Skripts ausführen usw. Die folgenden Komponenten sind mit Hue-Installationen in einem HDInsight Hadoop-Cluster verfügbar.

* Beeswax Hive Editor
* Pig
* Metastore Manager
* Oozie
* FileBrowser (Kommunikation mit dem WASB-Standardcontainer)
* Job Browser

> [AZURE.WARNING] Komponenten, die mit dem HDInsight-Cluster bereitgestellt werden, werden vollständig unterstützt, und Microsoft Support hilft Ihnen, Probleme im Zusammenhang mit diesen Komponenten zu isolieren und zu beheben.
>
> Für benutzerdefinierte Komponenten steht kommerziell angemessener Support für eine weiterführende Behebung des Problems zur Verfügung. Auf diese Weise kann das Problem behoben werden, ODER Sie werden aufgefordert, verfügbare Kanäle für Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. So können z. B. viele Communitywebsites verwendet werden, wie: das [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/de-DE/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Für Apache-Projekte gibt es auch Projektwebsites auf [http://apache.org](http://apache.org), z.B. [Hadoop](http://hadoop.apache.org/).

## Installation von Hue mithilfe von Skriptaktionen

Mit der folgenden Skriptaktion kann Hue auf einem Linux-basierten HDInsight-Cluster installiert werden. https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
    
Dieser Abschnitt enthält Anweisungen zur Verwendung des Skripts während der Bereitstellung des Clusters mithilfe des Azure-Portals.

> [AZURE.NOTE] Zum Anwenden von Skriptaktionen können auch Azure PowerShell, die Azure-Befehlszeilenschnittstelle, das HDInsight .NET SDK oder Azure Resource Manager-Vorlagen verwendet werden. Sie können auch Skriptaktionen auf bereits ausgeführte Cluster anwenden. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

1. Beginnen Sie die Bereitstellung eines Clusters anhand der Schritte in [Bereitstellen von HDInsight-Clustern unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal), schließen Sie sie jedoch nicht ab.

	> [AZURE.NOTE] Um Hue auf HDInsight-Clustern zu installieren, ist die empfohlene Hauptknotengröße mindestens A4 (8 Kerne, 14 GB Arbeitsspeicher).

2. Wählen Sie auf dem Blatt **Optionale Konfiguration** die Option **Skriptaktionen**, und geben Sie die folgenden Informationen an:

	![Bereitstellen von Skriptaktionsparametern für Hue](./media/hdinsight-hadoop-hue-linux/hue_script_action.png "Bereitstellen von Skriptaktionsparametern für Hue")

	* __NAME__: Geben Sie einen Anzeigenamen für die Skriptaktion ein.
	* __SCRIPT URI__: https://hdiconfigactions.blob.core.windows.net/linuxhueconfigactionv02/install-hue-uber-v02.sh
	* __HEAD__: Aktivieren Sie diese Option.
	* __WORKER__: Belassen Sie diese Option deaktiviert.
	* __ZOOKEEPER__: Belassen Sie diese Option deaktiviert.
	* __PARAMETERS__: Lassen Sie dieses Feld leer.

3. Verwenden Sie am unteren Rand der **Skriptaktionen** die Schaltfläche **Auswählen**, um die Konfiguration zu speichern. Verwenden Sie schließlich die Schaltfläche **Auswählen** am unteren Rand des Blatts **Optionale Konfiguration**, um die optionalen Konfigurationsinformationen zu speichern.

4. Setzen Sie die Bereitstellung des Clusters entsprechend der Beschreibung unter [Bereitstellen von HDInsight-Clustern unter Linux](hdinsight-hadoop-provision-linux-clusters.md#portal) fort.

## Verwenden von Hue mit HDInsight-Clustern

SSH-Tunneling ist die einzige Möglichkeit für den Zugriff auf Hue auf dem Cluster, wenn Hue ausgeführt wird. Durch das Tunneln über SSH wird der Datenverkehr direkt an den Hauptknoten des Clusters geleitet, auf dem Hue ausgeführt wird. Führen Sie nach Abschluss der Clusterbereitstellung die folgenden Schritte zum Verwenden von Hue in einem HDInsight Linux-Cluster aus.

1. Nutzen Sie die Informationen unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md), um einen SSH-Tunnel von Ihrem Clientsystem an das HDInsight-Cluster zu erstellen und anschließend Ihren Webbrowser für die Verwendung des SSH-Tunnels als Proxy zu konfigurieren.

2. Sobald Sie einen SSH-Tunnel erstellt und Ihren Browser für den Proxydatenverkehr konfiguriert haben, müssen Sie den Hostnamen des primären Hauptknotens ermitteln. Dazu können Sie an Port 22 eine SSH-Verbindung mit dem Cluster herstellen. Beispiel: `ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`. Dabei steht __USERNAME__ für den SSH-Benutzernamen und __CLUSTERNAME__ für den Namen Ihres Clusters.

    Weitere Informationen zur Verwendung von SSH finden Sie in den folgenden Dokumenten:

    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

3. Verwenden Sie nach erfolgter Verbindungsherstellung den folgenden Befehl, um den vollqualifizierten Domänennamen des primären Hauptknotens abzurufen:

        hostname -f

    Die Ausgabe dieses Befehls sieht in etwa wie folgt aus:

        hn0-myhdi-nfebtpfdv1nubcidphpap2eq2b.ex.internal.cloudapp.net
    
    Dies ist der Hostname des primären Hauptknotens, an dem sich die Hue-Website befindet.

2. Öffnen Sie das Hue-Portal (http://HOSTNAME:8888) im Browser. Ersetzen Sie „HOSTNAME“ durch den Namen, den Sie im vorherigen Schritt abgerufen haben.

    > [AZURE.NOTE] Wenn Sie sich zum ersten Mal anmelden, werden Sie aufgefordert, ein Konto für die Anmeldung beim Hue-Portal zu erstellen. Die hier angegebenen Anmeldeinformationen gelten nur für das Portal und beziehen sich nicht auf die Administrator- oder SSH-Benutzeranmeldeinformationen, die Sie beim Bereitstellen des Clusters angegeben haben.

	![Anmelden beim Hue-Portal](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Login.png "Angeben von Anmeldeinformationen für das Hue-Portal")

### Ausführen einer Hive-Abfrage

1. Klicken Sie im Hue-Portal auf **Query Editors** und dann auf **Hive**, um den Hive-Editor zu öffnen.

	![Verwenden von Hive](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.png "Verwenden von Hive")

2. Auf der Registerkarte **Assist** sollte unter **Database** der Eintrag **hivesampletable** angezeigt werden. Dies ist eine Beispieltabelle, die mit allen Hadoop-Clustern für HDInsight geliefert wird. Geben Sie eine Beispielabfrage im rechten Bereich ein. Die Ausgabe wird auf der Registerkarte **Results** im unteren Bereich angezeigt (siehe Bildschirmaufnahme).

	![Ausführen einer Hive-Abfrage](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Hive.Query.png "Ausführen einer Hive-Abfrage")

	Zudem können Sie über die Registerkarte **Chart** eine visuelle Darstellung des Ergebnisses anzeigen.

### Durchsuchen des Clusterspeichers

1. Klicken Sie im Hue-Portal oben rechts in der Menüleiste auf **File Browser**.

2. Standardmäßig wird der Dateibrowser im Verzeichnis **/user/myuser** geöffnet. Klicken Sie in dem Pfad auf den Schrägstrich direkt vor dem Benutzerverzeichnis, um zum Stammverzeichnis des Azure-Speichercontainers zu wechseln, der dem Cluster zugeordnet ist.

	![Verwenden des Dateibrowsers](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.File.Browser.png "Verwenden des Dateibrowsers")

3. Klicken Sie mit der rechten Maustaste auf eine Datei oder einen Ordner, um die verfügbaren Vorgänge anzuzeigen. Verwenden Sie die Schaltfläche **Upload** in der rechten Ecke, um Dateien in das aktuelle Verzeichnis hochzuladen. Über die Schaltfläche **New** können Sie neue Dateien oder Verzeichnisse erstellen.

> [AZURE.NOTE] Im Hue-Dateibrowser können nur die Inhalte des Standardcontainers angezeigt werden, der dem HDInsight-Cluster zugeordnet ist. Auf alle anderen Speicherkonten und Container, die Sie eventuell mit dem Cluster verknüpft haben, können Sie mit dem Dateibrowser nicht zugreifen. Die zusätzlichen mit dem Cluster verknüpften Container sind jedoch für Hive-Aufträge immer verfügbar. Wenn Sie beispielsweise im Hive-Editor den Befehl `dfs -ls wasbs://newcontainer@mystore.blob.core.windows.net` eingeben, werden auch die Inhalte der zusätzlichen Container angezeigt. In diesem Befehl ist **newcontainer** nicht der mit einem Cluster verknüpfte Standardcontainer.

## Wichtige Hinweise

1. Das für die Hue-Installation verwendete Skript installiert Hue nur auf dem primären Hauptknoten des Clusters.

2. Während der Installation werden mehrere Hadoop-Dienste (HDFS, YARN, MR2, Oozie) zum Aktualisieren der Konfiguration neu gestartet. Nach Abschluss der Installation von Hue mit dem Skript kann es einige Zeit dauern, bis andere Hadoop-Dienste gestartet werden. Dies kann anfänglich die Leistung von Hue beeinträchtigen. Nachdem alle Dienste gestartet wurden, ist Hue voll funktionsfähig.

3.	Hue kann keine Tez-Aufträge verarbeiten, wobei es sich um die aktuelle Standardeinstellung für Hive handelt. Wenn Sie MapReduce als Ausführungsmodul für Hive verwenden möchten, müssen Sie das Skript so aktualisieren, dass der folgende Befehl im Skript verwendet wird:

		set hive.execution.engine=mr;

4.	Bei Linux-Clustern können Sie Ihre Dienste auf dem primären Hauptknoten und Resource Manager auf dem sekundären Knoten ausführen. Dieses Szenario kann zu Fehlern führen (siehe unten), wenn mithilfe von Hue Details zu ausgeführten Aufträgen im Cluster angezeigt werden sollen. Nach Abschluss des Auftrags können Sie die Auftragsdetails jedoch anzeigen.

	![Fehler im Hue-Portal](./media/hdinsight-hadoop-hue-linux/HDI.Hue.Portal.Error.png "Fehler im Hue-Portal")

	Dies ist auf ein bekanntes Problem zurückzuführen. Zur Umgehung dieses Problems können Sie Ambari so konfigurieren, dass die aktive Resource Manager-Instanz auch auf dem primären Hauptknoten ausgeführt wird.

5.	Hue verarbeitet WebHDFS, während HDInsight-Cluster Azure Storage mit `wasbs://` verwenden. Daher installiert das mit der Skriptaktion verwendete benutzerdefinierte Skript WebWasb, einen WebHDFS-kompatiblen Dienst für die Kommunikation mit WASB. Auch wenn im Hue-Portal an bestimmten Stellen HDFS angegeben ist (z. B. beim Bewegen des Mauszeigers über den **Dateibrowser**), sollte dies als WASB interpretiert werden.


## Nächste Schritte

- [Installieren von Giraph in HDInsight-Clustern](hdinsight-hadoop-giraph-install-linux.md). Verwenden Sie die Clusteranpassung, um Giraph in HDInsight Hadoop-Clustern zu installieren. Giraph ermöglicht Ihnen, mithilfe von Hadoop Graphverarbeitungen durchzuführen. Es kann zudem mit Azure HDInsight eingesetzt werden.

- [Installieren von Solr in HDInsight-Clustern](hdinsight-hadoop-solr-install-linux.md). Verwenden Sie die Clusteranpassung, um Solr in HDInsight Hadoop-Clustern zu installieren. Solr ermöglicht Ihnen, leistungsstarke Suchvorgänge auf gespeicherte Daten anzuwenden.

- [Installieren und Verwenden von R in HDInsight-Clustern](hdinsight-hadoop-r-scripts-linux.md). Verwenden Sie die Clusteranpassung, um R in HDInsight Hadoop-Clustern zu installieren. R ist eine Open-Source-Sprache und -Umgebung für statistische Berechnungen. Sie bietet Hunderte integrierter Statistikfunktionen und eine eigene Programmiersprache, die Aspekte der funktionalen und objektorientierten Programmierung kombiniert. Darüber hinaus werden umfangreiche Grafikfunktionen geboten.

[powershell-install-configure]: install-configure-powershell-linux.md
[hdinsight-provision]: hdinsight-provision-clusters-linux.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md

<!---HONumber=AcomDC_0921_2016-->