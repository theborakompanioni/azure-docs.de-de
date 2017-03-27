---
title: "Installieren von Zeppelin Notebooks für Spark-Cluster unter Azure HDInsight mithilfe einer Skriptaktion | Microsoft-Dokumentation"
description: "Enthält eine Schritt-für-Schritt-Anleitung zur Installation und Verwendung von Zeppelin Notebooks mit Spark-Clustern in HDInsight Linux."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: cb276220-fb02-49e2-ac55-434fcb759629
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a939a0845d7577185ff32edd542bcb2082543a26
ms.openlocfilehash: eadf0611ca46a975c364a1b073828c6c3faf5f77
ms.lasthandoff: 01/24/2017


---
# <a name="install-zeppelin-notebooks-for-apache-spark-cluster-on-hdinsight"></a>Installieren von Zeppelin Notebooks für Apache Spark-Cluster unter HDInsight

Erfahren Sie, wie Zeppelin Notebooks auf Apache Spark-Clustern installiert und zur Ausführung von Spark-Aufträgen verwendet werden.

> [!IMPORTANT]
> Wenn Sie Spark 1.6-Cluster unter HDInsight 3.5 bereitgestellt haben, können Sie standardmäßig wie unter [Verwenden von Zeppelin Notebooks mit Apache Spark-Cluster unter HDInsight (Linux)](hdinsight-apache-spark-zeppelin-notebook.md) beschrieben auf Zeppelin Notebooks zugreifen. Wenn Sie Zeppelin in HDInsight-Clustern der Version 3.3 oder 3.4 (oder Spark 2.0 unter HDInsight 3.5) verwenden möchten, halten Sie sich an die Zeppelin-Installationsanleitung in diesem Artikel.
>
>

**Voraussetzungen:**

* Bevor Sie mit diesem Lernprogramm beginnen, müssen Sie über ein Azure-Abonnement verfügen. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)(in englischer Sprache).
* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).
* Einen SSH-Client. Für Linux- und Unix-Distributionen oder Macintosh OS X steht der Befehl `ssh` über das Betriebssystem zur Verfügung. Bei Windows wird [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)

  > [!NOTE]
  > Wenn Sie einen anderen SSH-Client als `ssh` oder PuTTY verwenden möchten, finden Sie Informationen zum Herstellen eines SSH-Tunnels in der Dokumentation zu Ihrem Client.
  >
  >
* Einen Webbrowser, der für die Verwendung eines SOCKS-Proxys konfiguriert werden kann.
* **(Optional)**: ein Plug-In wie [FoxyProxy](http://getfoxyproxy.org/,) , das Regeln anwenden kann, sodass nur routenspezifische Anfragen durch den Tunnel geleitet werden.

  > [!WARNING]
  > Ohne ein Plug-In wie FoxyProxy können alle über den Browser gesendeten Anfragen über den Tunnel weitergeleitet werden. Dies kann dazu führen, dass Webseiten in Ihrem Browser langsamer geladen werden.
  >
  >

## <a name="install-zeppelin-on-a-spark-cluster"></a>Installieren von Zeppelin auf einem Spark-Cluster
Sie können Zeppelin mithilfe von Skriptaktionen in einem Spark-Cluster installieren. Skriptaktionen verwenden benutzerdefinierte Skripts zum Installieren von Komponenten im Cluster, die nicht standardmäßig verfügbar sind. Sie können das benutzerdefinierte Skript verwenden, um Zeppelin im Azure-Portal zu installieren. Verwenden Sie dazu das HDInsight .NET SDK oder Azure PowerShell. Sie können das Skript verwenden, um Zeppelin entweder als Teil der Clustererstellung zu installieren, oder sobald der Cluster ausgeführt wird. Links in den folgenden Abschnitten enthalten die entsprechenden Anweisungen.

### <a name="using-the-azure-portal"></a>Verwenden des Azure-Portals
Eine Anleitung zur Verwendung des Azure-Portals zum Ausführen der Skriptaktion zum Installieren von Zeppelin finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Sie müssen an den Anweisungen in diesem Artikel einige Änderungen vornehmen.

* Sie müssen das Skript verwenden, um Zeppelin installieren zu können. Das benutzerdefinierte Skript zum Installieren von Zeppelin in einem Spark-Cluster in HDInsight ist unter folgenden Links verfügbar:

  * Für Spark 1.6.0-Cluster – `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark160-v01.sh`
  * Für Spark 1.5.2-Cluster – `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark151-v01.sh`
* Sie müssen die Skriptaktion nur auf dem Hauptknoten ausführen.
* Das Skript benötigt keine Parameter.

### <a name="using-hdinsight-net-sdk"></a>Das HDInsight .NET SDK
Eine Anleitung zur Verwendung des HDInsight .NET SDK zum Ausführen der Skriptaktion zum Installieren von Zeppelin finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md#use-a-script-action-during-cluster-creation). Sie müssen an den Anweisungen in diesem Artikel einige Änderungen vornehmen.

* Sie müssen das Skript verwenden, um Zeppelin installieren zu können. Das benutzerdefinierte Skript zum Installieren von Zeppelin in einem Spark-Cluster in HDInsight ist unter folgenden Links verfügbar:

  * Für Spark 1.6.0-Cluster – `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark160-v01.sh`
  * Für Spark 1.5.2-Cluster – `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark151-v01.sh`
* Das Skript benötigt keine Parameter.
* Legen Sie den zu erstellenden Clustertyp auf „Spark“ fest.

### <a name="using-azure-powershell"></a>Verwenden von Azure PowerShell
Verwenden Sie den folgenden PowerShell-Codeausschnitt, um einen Spark-Cluster unter HDInsight Linux mit Installation von Zeppelin zu erstellen. Abhängig von Ihrer Version des Spark-Clusters müssen Sie den unten angegebenen PowerShell-Codeausschnitt aktualisieren, um den Link zum entsprechenden benutzerdefinierten Skript einzufügen.

* Für Spark 1.6.0-Cluster – `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark160-v01.sh`
* Für Spark 1.5.2-Cluster – `https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark151-v01.sh`

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    Login-AzureRMAccount

    # PROVIDE VALUES FOR THE VARIABLES
    $clusterAdminUsername="admin"
    $clusterAdminPassword="<<password>>"
    $clusterSshUsername="adminssh"
    $clusterSshPassword="<<password>>"
    $clusterName="<<clustername>>"
    $clusterContainerName=$clusterName
    $resourceGroupName="<<resourceGroupName>>"
    $location="<<region>>"
    $storage1Name="<<storagename>>"
    $storage1Key="<<storagekey>>"
    $subscriptionId="<<subscriptionId>>"

    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    $passwordAsSecureString=ConvertTo-SecureString $clusterAdminPassword -AsPlainText -Force
    $clusterCredential=New-Object System.Management.Automation.PSCredential ($clusterAdminUsername, $passwordAsSecureString)
    $passwordAsSecureString=ConvertTo-SecureString $clusterSshPassword -AsPlainText -Force
    $clusterSshCredential=New-Object System.Management.Automation.PSCredential ($clusterSshUsername, $passwordAsSecureString)

    $azureHDInsightConfigs= New-AzureRmHDInsightClusterConfig -ClusterType Spark
    $azureHDInsightConfigs.DefaultStorageAccountKey = $storage1Key
    $azureHDInsightConfigs.DefaultStorageAccountName = "$storage1Name.blob.core.windows.net"

    Add-AzureRMHDInsightScriptAction -Config $azureHDInsightConfigs -Name "Install Zeppelin" -NodeType HeadNode -Parameters "void" -Uri "https://hdiconfigactions.blob.core.windows.net/linuxincubatorzeppelinv01/install-zeppelin-spark151-v01.sh"

    New-AzureRMHDInsightCluster -Config $azureHDInsightConfigs -OSType Linux -HeadNodeSize "Standard_D12" -WorkerNodeSize "Standard_D12" -ClusterSizeInNodes 2 -Location $location -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $clusterCredential -DefaultStorageContainer $clusterContainerName -SshCredential $clusterSshCredential -Version "3.3"

## <a name="access-the-zeppelin-notebook"></a>Zugreifen auf das Zeppelin Notebook

Nachdem Sie Zeppelin mithilfe der Skriptaktion installiert haben, können Sie die folgenden Schritte ausführen, um auf das Zeppelin Notebook im Spark-Cluster zuzugreifen. In diesem Abschnitt sehen Sie, wie die Anweisungen „%sql“ und „%hive“ ausgeführt werden.

1. Öffnen Sie im Webbrowser den folgenden Endpunkt:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin

   
2. Erstellen Sie ein neues Notebook. Klicken Sie im Headerbereich auf **Notebook**, und wählen Sie die Option **Neue Notiz erstellen** aus.

    ![Erstellen eines neuen Zeppelin Notebooks](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.createnewnote.png "Erstellen eines neuen Zeppelin Notebooks")

    Auf derselben Seite sollte unter der Überschrift **Notebook** ein neues Notebook angezeigt werden, dessen Name mit **Notiz XXXXXXXXX** beginnt. Klicken Sie auf das neue Notebook.
3. Klicken Sie auf der Webseite für das neue Notebook auf die Überschrift, und ändern Sie den Namen des Notebooks, wenn Sie dies möchten. Drücken Sie die EINGABETASTE, um die Namensänderung zu speichern. Stellen Sie außerdem sicher, dass im Header des Notebooks in der oberen rechten Ecke der Status **Verbunden** angezeigt wird.

    ![Zeppelin Notebook-Status](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.newnote.connected.png "Zeppelin Notebook-Status")

### <a name="run-sql-statements"></a>Ausführen von SQL-Anweisungen
1. Laden Sie Beispieldaten in eine temporäre Tabelle. Wenn Sie einen Spark-Cluster in HDInsight erstellen, wird die Beispieldatei **hvac.csv** in das zugeordnete Speicherkonto unter **\HdiSamples\SensorSampleData\hvac** kopiert.

    Fügen Sie in den leeren Absatz, der im neuen Notebook standardmäßig erstellt wird, den folgenden Codeausschnitt ein.

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)

        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0),
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()

        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")

    Drücken Sie ****UMSCHALT+EINGABETASTE, oder klicken Sie auf die Schaltfläche **Wiedergeben** für den Absatz, um den Codeausschnitt auszuführen. Der Status in der rechten Ecke des Absatzes sollte sich entsprechend ändern: BEREIT, AUSSTEHEND, WIRD AUSGEFÜHRT bis zu BEENDET. Die Ausgabe wird unten im Absatz angezeigt. Der Screenshot sieht folgendermaßen aus:

    ![Erstellen einer temporären Tabelle auf der Grundlage von Rohdaten](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.note.loaddDataintotable.png "Erstellen einer temporären Tabelle auf der Grundlage von Rohdaten")

    Sie können auch einen Titel für jeden Absatz angeben. Klicken Sie in der rechten Ecke auf das Symbol **Einstellungen** und dann auf **Titel anzeigen**.
2. Sie können jetzt Spark-SQL-Anweisungen für die **hvac** -Tabelle ausführen. Fügen Sie die folgende Abfrage in einen neuen Absatz ein. Mit der Abfrage werden die Gebäude-ID und der Unterschied zwischen den Ziel- und Ist-Temperaturen für jedes Gebäude an einem bestimmten Datum abgerufen. Drücken Sie UMSCHALT+EINGABETASTE ****.

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date
        from hvac
        where date = "6/1/13"

    Mit der **%sql**-Anweisung am Anfang wird das Notebook angewiesen, den Spark-SQL-Interpreter zu verwenden. Sie können die definierten Interpreter im Header des Notebooks auf der Registerkarte **Interpreter** anzeigen.

    Im folgenden Screenshot ist die Ausgabe dargestellt.

    ![Ausführen einer Spark-SQL-Anweisung mit dem Notebook](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.note.sparksqlquery1.png "Ausführen einer Spark SQL-Anweisung mit dem Notebook")

     Klicken Sie auf die Anzeigeoptionen (im Rechteck hervorgehoben), um für eine Ausgabe zwischen unterschiedlichen Darstellungen zu wechseln. Klicken Sie auf **Einstellungen** , um auszuwählen, worum es sich bei den Schlüsseln und Werten in der Ausgabe handelt. Im obigen Screenshot werden **buildingID** als Schlüssel und der Mittelwert von **temp_diff** als Wert verwendet.
3. Sie können auch Spark-SQL-Anweisungen ausführen, indem Sie die Variablen in der Abfrage verwenden. Der nächste Codeausschnitt zeigt, wie Sie eine Variable ( **Temp**) in der Abfrage mit den möglichen Werten definieren, die für die Abfrage verwendet werden sollen. Beim ersten Ausführen der Abfrage wird automatisch eine Dropdownliste mit den Werten aufgefüllt, die Sie für die Variable angegeben haben.

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff
        from hvac
        where targettemp > "${Temp = 65,65|75|85}"

    Fügen Sie diesen Codeausschnitt in einen neuen Absatz ein, und drücken Sie UMSCHALT+EINGABETASTE ****. Im folgenden Screenshot ist die Ausgabe dargestellt.

    ![Ausführen einer Spark-SQL-Anweisung mit dem Notebook](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.note.sparksqlquery2.png "Ausführen einer Spark SQL-Anweisung mit dem Notebook")

    Für nachfolgende Abfragen können Sie einen neuen Wert aus der Dropdownliste auswählen und die Abfrage erneut ausführen. Klicken Sie auf **Einstellungen** , um auszuwählen, worum es sich bei den Schlüsseln und Werten in der Ausgabe handelt. Im obigen Screenshot werden **buildingID** als Schlüssel, der Mittelwert von **temp_diff** als Wert und **targettemp** als Gruppe verwendet.
4. Starten Sie den Spark-SQL-Interpreter neu, um die Anwendung zu beenden. Klicken Sie oben auf die Registerkarte **Interpreter**, und klicken Sie für den Spark-Interpreter auf **Neu starten**.

    ![Neustarten des Zeppelin-Interpreters](./media/hdinsight-apache-spark-use-zeppelin-notebook/hdispark.zeppelin.restart.interpreter.png "Neustarten des Zeppelin-Interpreters")

### <a name="run-hive-statements"></a>Ausführen von Hive-Anweisungen
1. Klicken Sie im Zeppelin Notebook auf die Schaltfläche **Interpreter** .

    ![Aktualisieren des Hive-Interpreters](./media/hdinsight-apache-spark-use-zeppelin-notebook/zeppelin-update-hive-interpreter-1.png "Aktualisieren des Hive-Interpreters")
2. Klicken Sie für den **Hive**-Interpreter auf **Bearbeiten**.

    ![Aktualisieren des Hive-Interpreters](./media/hdinsight-apache-spark-use-zeppelin-notebook/zeppelin-update-hive-interpreter-2.png "Aktualisieren des Hive-Interpreters")

    Aktualisieren Sie die folgenden Eigenschaften.

   * Legen Sie **default.password** auf das Kennwort fest, das Sie beim Erstellen des HDInsight Spark-Clusters für den Benutzer „admin“ angegeben haben.
   * Legen Sie **default.url** auf `jdbc:hive2://<spark_cluster_name>.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=/hive2` fest. Ersetzen Sie **\<Spark-Clustername>** durch den Namen Ihres Spark-Clusters.
   * Legen Sie **default.user** auf den Namen des Benutzers „admin“ fest, den Sie beim Erstellen des Clusters angegeben haben. Beispiel: *admin*.
3. Klicken Sie auf **Speichern**. Wenn Sie aufgefordert werden, den Hive-Interpreter neu zu starten, klicken Sie auf **OK**.
4. Erstellen Sie ein neues Notebook, und führen Sie die folgende Anweisung aus, um alle Hive-Tabellen im Cluster aufzulisten.

        %hive
        SHOW TABLES

    Standardmäßig enthält ein HDInsight-Cluster eine Beispieltabelle namens **hivesampletable** , sodass Sie die folgende Ausgabe erhalten sollten.

    ![Hive-Ausgabe](./media/hdinsight-apache-spark-use-zeppelin-notebook/zeppelin-update-hive-interpreter-3.png "Hive-Ausgabe")
5. Führen Sie die folgende Anweisung aus, um die Einträge in der Tabelle anzuzeigen.

        %hive
        SELECT * FROM hivesampletable LIMIT 5

    Sie sollten eine Ausgabe ähnlich der folgenden erhalten.

    ![Hive-Ausgabe](./media/hdinsight-apache-spark-use-zeppelin-notebook/zeppelin-update-hive-interpreter-4.png "Hive-Ausgabe")

## <a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Verwenden von externen Paketen mit Jupyter Notebooks](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: storage-create-storage-account.md

