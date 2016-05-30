<properties
   pageTitle="Erste Schritte mit R Server in HDInsight (Vorschau) | Azure"
   description="Erfahren Sie, wie Sie ein Apache Spark-System in einem HDInsight-Cluster (Hadoop) erstellen, zu dem R Server (Vorschau) gehört, und dann ein R-Skript im Cluster übermitteln."
   services="HDInsight"
   documentationCenter=""
   authors="jeffstokes72"
   manager="paulettem"
   editor="cgronlun"
/>

<tags
   ms.service="HDInsight"
   ms.devlang="R"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-services"
   ms.date="05/16/2016"
   ms.author="jeffstok"
/>

# Erste Schritte mit R Server in HDInsight (Vorschau)

Beim Premium-Tarifangebot für HDInsight ist R Server Teil des HDInsight-Clusters (Vorschau). Dadurch können R-Skripts MapReduce und Spark verwenden, um verteilte Berechnungen auszuführen. In diesem Dokument erfahren Sie, wie Sie eine R Server-Instanz in HDInsight erstellen und dann ein R-Skript ausführen, das die Verwendung von Spark für verteilte R-Berechnungen veranschaulicht.

![Diagramm des Workflows für dieses Dokument](./media/hdinsight-getting-started-with-r/rgettingstarted.png)

## Voraussetzungen

* __Ein Azure-Abonnement__: Bevor Sie mit diesem Tutorial beginnen, müssen Sie über ein Azure-Abonnement verfügen. Weitere Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* __Ein Secure Shell-Client (SSH)__: Ein SSH-Client wird verwendet, um Remoteverbindungen mit dem HDInsight-Cluster herzustellen und Befehle direkt im Cluster auszuführen. Linux-, Unix- und OS X-Systeme stellen einen SSH-Client über den Befehl `ssh` bereit. Für Windows-Systeme wird [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) empfohlen.

    * __SSH-Schlüssel (optional)__: Sie können das SSH-Konto, mit dem die Verbindung mit dem Cluster hergestellt wird, mit einem Kennwort oder einem öffentlichen Schlüssel sichern. Die Verwendung eines Kennworts ist einfacher und ermöglicht Ihnen den Einstieg, ohne ein öffentliches/privates Schlüsselpaar zu erstellen. Die Verwendung eines Schlüssels ist jedoch sicherer.
    
        Für die Schritte in diesem Dokument wird davon ausgegangen, dass Sie ein Kennwort verwenden. Informationen zum Erstellen und Verwenden von SSH-Schlüsseln mit HDInsight finden Sie in den folgenden Dokumenten:
        
        * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
        
        * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

## Cluster erstellen

> [AZURE.NOTE] Mit den Schritten in diesem Dokument wird R Server in HDInsight mit grundlegenden Konfigurationsinformationen eingerichtet. Informationen zu anderen Clusterkonfigurationseinstellungen (z. B. zum Hinzufügen zusätzlicher Speicherkonten, zum Verwenden eines virtuellen Azure-Netzwerks oder zum Erstellen eines Metastore für Hive) finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.

2. Wählen Sie __NEU__, __Daten und Analysen__ und anschließend __HDInsight__ aus.

    ![Bild der Erstellung eines neuen Clusters](./media/hdinsight-getting-started-with-r/newcluster.png)

3. Geben Sie im Feld __Clustername__ einen Namen für den Cluster ein. Wenn Sie über mehrere Azure-Abonnements verfügen, wählen Sie über den Eintrag __Abonnement__ das Abonnement aus, das Sie verwenden möchten.

    ![Auswahl von Clustername und Abonnement](./media/hdinsight-getting-started-with-r/clustername.png)

4. Wählen Sie __Clustertyp auswählen__ aus. Wählen Sie auf dem Blatt __Clustertyp__ die folgenden Optionen aus:

    * __Clustertyp__: R Server auf Spark
    
    * __Clustertarif__: Premium

    Behalten Sie für die anderen Optionen die Standardwerte bei, und verwenden Sie die Schaltfläche __Auswählen__, um den Clustertyp zu speichern.
    
    ![Screenshot des Blatts „Clustertyp“](./media/hdinsight-getting-started-with-r/clustertypeconfig.png)
    
    > [AZURE.NOTE] Sie können R Server auch anderen HDInsight-Clustertypen (wie Hadoop oder HBase) hinzufügen, indem Sie erst den Clustertyp und dann __Premium__ auswählen.

5. Wählen Sie **Ressourcengruppe** aus, um eine Liste der vorhandenen Ressourcengruppen anzuzeigen, und wählen Sie dann die Gruppe aus, in der der Cluster erstellt werden soll. Alternativ können Sie eine neue Ressourcengruppe erstellen, indem Sie auf **Neu erstellen** klicken und dann den Namen der neuen Ressourcengruppe eingeben. Wenn der neue Gruppenname verfügbar ist, wird neben dem Namen ein grünes Häkchen angezeigt.

    > [AZURE.NOTE] Dieser Eintrag ist standardmäßig auf eine Ihrer vorhandenen Ressourcengruppen festgelegt (sofern verfügbar).
    
    Verwenden Sie die Schaltfläche __Auswählen__, um die Ressourcengruppe zu speichern.

6. Klicken Sie auf **Anmeldeinformationen**, und geben Sie dann ein **Clusteranmeldekennwort** und einen **Clusterbenutzernamen** ein.

    Geben Sie einen __SSH-Benutzernamen__ ein, und wählen Sie __Kennwort__ aus. Geben Sie dann das __SSH-Kennwort__ zum Konfigurieren des SSH-Kontos ein. SSH wird verwendet, um eine Remoteverbindung mit dem Cluster über einen Secure Shell-Client (SSH) herzustellen.
    
    Verwenden Sie die Schaltfläche __Auswählen__, um die Anmeldeinformationen zu speichern.
    
    ![Blatt "Anmeldeinformationen"](./media/hdinsight-getting-started-with-r/clustercredentials.png)

7. Wählen Sie **Datenquelle** aus, um eine Datenquelle für den Cluster auszuwählen. Wählen Sie ein vorhandenes Speicherkonto aus, indem Sie __Speicherkonto auswählen__ und dann das Konto auswählen, oder erstellen Sie mit dem Link __Neu__ im Abschnitt __Speicherkonto auswählen__ ein neues Konto.

    Wenn Sie __Neu__ verwenden, müssen Sie einen Namen für das neue Speicherkonto eingeben. Wenn der Name akzeptiert wird, wird ein grünes Häkchen angezeigt.

    Als __Standardcontainer__ wird standardmäßig der Name des Clusters verwendet. Ändern Sie diesen Wert nicht.
    
    Wählen Sie __Standort__ aus, um die Region auszuwählen, in der das Speicherkonto erstellt werden soll.
    
    > [AZURE.IMPORTANT] Durch die Auswahl des Standorts für die Standarddatenquelle wird auch der Standort des HDInsight-Clusters festgelegt. Der Cluster und die Standarddatenquelle müssen sich in der gleichen Region befinden.

    Verwenden Sie die Schaltfläche **Auswählen**, um die Datenquellenkonfiguration zu speichern.
    
    ![Blatt „Datenquelle“](./media/hdinsight-getting-started-with-r/datastore.png)

8. Wählen Sie **Knotenpreistarife** aus, um Informationen zu den Knoten anzuzeigen, die für diesen Cluster erstellt werden. Wenn Sie wissen, dass Sie keinen größeren Cluster benötigen, behalten Sie für die Anzahl der Workerknoten den Standardwert `4` bei. Die vorkalkulierten Kosten für den Cluster werden auf dem Blatt angezeigt.

    ![Blatt „Knotenpreistarife“](./media/hdinsight-getting-started-with-r/pricingtier.png)

    Klicken Sie auf die Schaltfläche **Auswählen**, um die Konfiguration der Knotenpreise zu speichern.
    
9. Vergewissern Sie sich auf dem Blatt **Neuer HDInsight-Cluster**, dass **An Startmenü anheften** ausgewählt ist, und klicken Sie dann auf **Erstellen**. Der Cluster wird erstellt, und dem Startmenü Ihres Azure-Portals wird eine Kachel für den Cluster hinzugefügt. Das Symbol zeigt an, dass der Cluster erstellt wird. Sobald die Erstellung abgeschlossen ist, ändert es sich in das HDInsight-Symbol.

    | Während der Erstellung | Erstellung abgeschlossen |
    | ------------------ | --------------------- |
    | ![Erstellungsanzeige im Startmenü](./media/hdinsight-getting-started-with-r/provisioning.png) | ![Kachel für einen erstellten Cluster](./media/hdinsight-getting-started-with-r/provisioned.png) |

    > [AZURE.NOTE] Die Erstellung des Clusters dauert in der Regel ca. 15 Minuten. Sie können den Status des Erstellungsprozesses auf der Kachel im Startmenü oder im linken Bereich der Seite unter **Benachrichtigungen** überprüfen.

## Herstellen einer Verbindung mit dem R Server-Edgeknoten

Stellen Sie über SSH eine Verbindung mit dem R Server-Edgeknoten des HDInsight-Clusters her:

    ssh USERNAME@rserver.CLUSTERNAME.ssh.azurehdinsight.net
    
> [AZURE.NOTE] Sie finden die Adresse `RServer.CLUSTERNAME.ssh.azurehdinsight.net` auch im Azure-Portal, indem Sie den Cluster und dann __Alle Einstellungen__, __Apps__ und __RServer__ auswählen. Dadurch werden die SSH-Endpunktinformationen für den Edgeknoten angezeigt.
>
> ![Bild des SSH-Endpunkts für den Edgeknoten](./media/hdinsight-getting-started-with-r/sshendpoint.png)
    
Wenn Sie zum Schutz Ihres SSH-Benutzerkontos ein Kennwort verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen öffentlichen Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` verwenden, um den passenden privaten Schlüssel anzugeben. Beispiel: `ssh -i ~/.ssh/id_rsa USERNAME@RServer.CLUSTERNAME.ssh.azurehdinsight.net`.
    
Weitere Informationen zum Verwenden von SSH mit Linux-basierten HDInsight-Clustern finden Sie in den folgenden Artikeln:

* [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

* [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Nachdem die Verbindung hergestellt wurde, wird eine Eingabeaufforderung wie die folgende angezeigt.

    username@ed00-myrser:~$

## Verwenden der R-Konsole

1. Verwenden Sie in der SSH-Sitzung den folgenden Befehl, um die R-Konsole zu starten:

        R
    
    Eine Ausgabe ähnlich der folgenden wird angezeigt.
    
        R version 3.2.2 (2015-08-14) -- "Fire Safety"
        Copyright (C) 2015 The R Foundation for Statistical Computing
        Platform: x86_64-pc-linux-gnu (64-bit)

        R is free software and comes with ABSOLUTELY NO WARRANTY.
        You are welcome to redistribute it under certain conditions.
        Type 'license()' or 'licence()' for distribution details.

        Natural language support but running in an English locale

        R is a collaborative project with many contributors.
        Type 'contributors()' for more information and
        'citation()' on how to cite R or R packages in publications.

        Type 'demo()' for some demos, 'help()' for on-line help, or
        'help.start()' for an HTML browser interface to help.
        Type 'q()' to quit R.

        Microsoft R Server version 8.0: an enhanced distribution of R
        Microsoft packages Copyright (C) 2016 Microsoft Corporation

        Type 'readme()' for release notes.

        >

2. An der `>`-Eingabeaufforderung können Sie R-Code eingeben. Zu R Server gehören Pakete, mit denen Sie auf einfache Weise mit Hadoop interagieren und verteilten Berechnungen ausführen können. Verwenden Sie beispielsweise den folgenden Befehl, um das Stammverzeichnis des Standarddateisystems für den HDInsight-Cluster anzuzeigen.

        rxHadoopListFiles("/")
    
    Sie können auch eine Adressierung mit der WASB-Syntax verwenden.
    
        rxHadoopListFiles("wasb:///")

##Verwenden eines Rechenkontexts

Mit einem Rechenkontext können Sie steuern, ob die Berechnung lokal auf dem Edgeknoten ausgeführt wird oder ob sie auf die Knoten im HDInsight-Cluster verteilt wird.
        
1. Verwenden Sie in der R-Konsole Folgendes, um Beispieldaten in den Standardspeicher für HDInsight zu laden.

        # Set the HDFS (WASB) location of example data
        bigDataDirRoot <- "/example/data"
        # create a local folder for storaging data temporarily
        source <- "/tmp/AirOnTimeCSV2012"
        dir.create(source)
        # Download data to the tmp folder
        remoteDir <- "http://packages.revolutionanalytics.com/datasets/AirOnTimeCSV2012"
        download.file(file.path(remoteDir, "airOT201201.csv"), file.path(source, "airOT201201.csv"))
        download.file(file.path(remoteDir, "airOT201202.csv"), file.path(source, "airOT201202.csv"))
        download.file(file.path(remoteDir, "airOT201203.csv"), file.path(source, "airOT201203.csv"))
        download.file(file.path(remoteDir, "airOT201204.csv"), file.path(source, "airOT201204.csv"))
        download.file(file.path(remoteDir, "airOT201205.csv"), file.path(source, "airOT201205.csv"))
        download.file(file.path(remoteDir, "airOT201206.csv"), file.path(source, "airOT201206.csv"))
        download.file(file.path(remoteDir, "airOT201207.csv"), file.path(source, "airOT201207.csv"))
        download.file(file.path(remoteDir, "airOT201208.csv"), file.path(source, "airOT201208.csv"))
        download.file(file.path(remoteDir, "airOT201209.csv"), file.path(source, "airOT201209.csv"))
        download.file(file.path(remoteDir, "airOT201210.csv"), file.path(source, "airOT201210.csv"))
        download.file(file.path(remoteDir, "airOT201211.csv"), file.path(source, "airOT201211.csv"))
        download.file(file.path(remoteDir, "airOT201212.csv"), file.path(source, "airOT201212.csv"))
        # Set directory in bigDataDirRoot to load the data into
        inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012") 
        # Make the directory
        rxHadoopMakeDir(inputDir)
        # Copy the data from source to input
        rxHadoopCopyFromLocal(source, bigDataDirRoot)

2. Als Nächstes erstellen wir einige Dateninformationen und definieren zwei Datenquellen, damit wir mit den Daten arbeiten können.

        # Define the HDFS (WASB) file system
        hdfsFS <- RxHdfsFileSystem()
        # Create info list for the airline data
        airlineColInfo <- list(
            DAY_OF_WEEK = list(type = "factor"),
            ORIGIN = list(type = "factor"),
            DEST = list(type = "factor"),
            DEP_TIME = list(type = "integer"),
            ARR_DEL15 = list(type = "logical"))

        # get all the column names
        varNames <- names(airlineColInfo)

        # Define the text data source in hdfs
        airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)
        # Define the text data source in local system
        airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

        # formula to use
        formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

3. Jetzt führen wir mit dem lokalen Computekontext eine logistische Regression für die Daten aus.

        # Set a local compute context
        rxSetComputeContext("local")
        # Run a logistic regression
        system.time(
            modelLocal <- rxLogit(formula, data = airOnTimeDataLocal)
        )
        # Display a summary 
        summary(modelLocal)

    Eine Ausgabe, die mit Zeilen wie den folgenden endet, sollte angezeigt werden.

        Data: airOnTimeDataLocal (RxTextData Data Source)
        File name: /tmp/AirOnTimeCSV2012
        Dependent variable(s): ARR_DEL15
        Total independent variables: 634 (Including number dropped: 3)
        Number of valid observations: 6005381
        Number of missing observations: 91381
        -2*LogLikelihood: 5143814.1504 (Residual deviance on 6004750 degrees of freedom)

        Coefficients:
                        Estimate Std. Error z value Pr(>|z|)
        (Intercept)   -3.370e+00  1.051e+00  -3.208  0.00134 **
        ORIGIN=JFK     4.549e-01  7.915e-01   0.575  0.56548
        ORIGIN=LAX     5.265e-01  7.915e-01   0.665  0.50590
        ......
        DEST=SHD       5.975e-01  9.371e-01   0.638  0.52377
        DEST=TTN       4.563e-01  9.520e-01   0.479  0.63172
        DEST=LAR      -1.270e+00  7.575e-01  -1.676  0.09364 .
        DEST=BPT         Dropped    Dropped Dropped  Dropped
        ---
        Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

        Condition number of final variance-covariance matrix: 11904202
        Number of iterations: 7

4. Als Nächstes führen wir die gleiche logistische Regression mit dem Spark-Kontext aus. Im Spark-Kontext wird die Verarbeitung auf alle Workerknoten im HDInsight-Cluster verteilt.

        # Define the Spark compute context 
        mySparkCluster <- RxSpark()
        # Set the compute context 
        rxSetComputeContext(mySparkCluster)
        # Run a logistic regression 
        system.time(  
            modelSpark <- rxLogit(formula, data = airOnTimeData)
        )
        # Display a summary
        summary(modelSpark)

    > [AZURE.NOTE] Sie können auch MapReduce verwenden, um die Berechnung auf Clusterknoten zu verteilen. Weitere Informationen zum Computekontext finden Sie unter [Rechenkontextoptionen für R Server in HDInsight Premium](hdinsight-hadoop-r-server-compute-contexts.md).

##Verteilen von R-Code auf mehrere Knoten

Mit R Server können Sie mithilfe von `rxExec` einfach vorhandenen R-Code nutzen und auf mehreren Knoten im Cluster ausführen. Dies ist nützlich für Parameterbereinigungen oder für Simulationen. Im folgenden Beispiel wird die Verwendung von `rxExec` veranschaulicht.

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )
    
Wenn Sie noch den Spark- oder MapReduce-Kontext verwenden, wird der nodename-Wert für die Workerknoten zurückgegeben, auf denen der Code (`Sys.info()["nodename"]`) ausgeführt wird. In einem Cluster mit vier Knoten könnten Sie z. B. eine Ausgabe wie die folgende erhalten.

    $rxElem1
        nodename
    "wn3-myrser"

    $rxElem2
        nodename
    "wn0-myrser"

    $rxElem3
        nodename
    "wn3-myrser"

    $rxElem4
        nodename
    "wn3-myrser"

##Installieren von R-Paketen

Falls Sie zusätzliche R-Pakete auf dem Edgeknoten installieren möchten, können Sie `install.packages()` direkt in der R-Konsole verwenden, wenn Sie über SSH eine Verbindung mit dem Edgeknoten hergestellt haben. Wenn Sie jedoch R-Pakete auf den Workerknoten des Clusters installieren möchten, müssen Sie eine Skriptaktion verwenden.

Skriptaktionen sind Bash-Skripts, mit denen Konfigurationsänderungen am HDInsight-Cluster vorgenommen werden oder zusätzliche Software installiert wird. In diesem Fall werden sie zum Installieren zusätzlicher R-Pakete genutzt. Gehen Sie folgendermaßen vor, um zusätzliche Pakete mit einer Skriptaktion zu installieren.

> [AZURE.IMPORTANT] Skriptaktionen können nur zum Installieren zusätzlicher R-Pakete verwendet werden, nachdem der Cluster erstellt wurde. Sie sollten nicht während der Clustererstellung verwendet werden, da für das Skript R Server vollständig installiert und konfiguriert sein muss.

1. Wählen Sie im [Azure-Portal](https://portal.azure.com) R Server im HDInsight-Cluster aus.

2. Wählen Sie auf dem Blatt für den Cluster __Alle Einstellungen__ und dann __Skriptaktionen__ aus. Wählen Sie auf dem Blatt __Skriptaktionen__ die Option __Neue übermitteln__ aus, um eine neue Skriptaktion zu übermitteln.

    ![Bild des Blatts „Skriptaktionen“](./media/hdinsight-getting-started-with-r/newscriptaction.png)

3. Geben Sie auf dem Blatt __Skriptaktion übermitteln__ die folgenden Informationen an:

    * __Name__: Ein Anzeigename zum Identifizieren dieses Skripts
    * __Bash-Skript-URI__: http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh
    * __Hauptknoten__: Diese Option sollte __deaktiviert__ sein.
    * __Worker__: Diese Option sollte __aktiviert__ sein.
    * __Zookeeper__: Diese Option sollte __deaktiviert__ sein.
    * __Parameter__: Die zu installierenden R-Pakete. Beispiel: `bitops stringr arules`
    * __Diese Skriptaktion ...__: Diese Option sollte __aktiviert__ sein.
    
    > [AZURE.IMPORTANT] Wenn für die R-Pakete, die Sie installieren, Systembibliotheken hinzugefügt werden müssen, müssen Sie das hier verwendete Basisskript herunterladen und Schritte zum Installieren der Systembibliotheken hinzufügen. Dann müssen Sie das geänderte Skript in einen öffentlichen Blobcontainer im Azure-Speicher hochladen und das geänderte Skript zum Installieren der Pakete verwenden.
    >
    >Weitere Informationen zum Entwickeln von Skriptaktionen finden Sie unter [Entwickeln von Skriptaktionen](hdinsight-hadoop-script-actions-linux.md).
    
    ![Hinzufügen einer Skriptaktion](./media/hdinsight-getting-started-with-r/scriptaction.png)

4. Wählen Sie __Erstellen__ aus, um das Skript auszuführen. Nach Abschluss der Skriptausführung sind die R-Pakete auf allen Workerknoten verfügbar.
    
## Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie einen neuen HDInsight-Cluster mit Verwendung von R Server erstellen, und Sie haben die Grundlagen der Verwendung der R-Konsole aus einer SSH-Sitzung kennengelernt. Unter den folgenden Links erhalten Sie Informationen zu weiteren Möglichkeiten für die Arbeit mit R Server in HDInsight.

- [Add RStudio Server to HDInsight premium (Hinzufügen von RStudio Server zu HDInsight Premium)](hdinsight-hadoop-r-server-install-r-studio.md)

- [Rechenkontextoptionen für R Server in HDInsight Premium](hdinsight-hadoop-r-server-compute-contexts.md)

- [Azure Storage options for R Server on HDInsight Premium (Azure Storage-Optionen für R Server in HDInsight Premium)](hdinsight-hadoop-r-server-storage.md)

### Azure-Ressourcen-Manager-Vorlagen

Wenn Sie die Automatisierung der Erstellung einer R Server-Instanz in HDInsight mithilfe von Azure Resource Manager-Vorlagen interessiert, nutzen Sie die folgenden Beispielvorlagen.

* [Create an R Server on HDInsight cluster using an SSH public key (Erstellen einer R Server-Instanz in einem HDInsight-Cluster mit einem öffentlichen SSH-Schlüssel)](http://go.microsoft.com/fwlink/p/?LinkID=780809)
* [Create an R Server on HDInsight cluster using an SSH password (Erstellen einer R Server-Instanz in einem HDInsight-Cluster mit einem SSH-Kennwort)](http://go.microsoft.com/fwlink/p/?LinkID=780810)

Mit beide Vorlagen werden ein neuer HDInsight-Cluster und das zugehörige Speicherkonto erstellt. Sie können über die Azure-Befehlszeilenschnittstelle, Azure PowerShell oder das Azure-Portal verwendet werden.

Allgemeine Informationen zur Verwendung von ARM-Vorlagen finden Sie unter [Erstellen Linux-basierter Hadoop-Cluster in HDInsight mithilfe von ARM-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

<!---HONumber=AcomDC_0518_2016-->