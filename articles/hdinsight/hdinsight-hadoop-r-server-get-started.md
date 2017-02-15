---
title: Erste Schritte mit R Server in HDInsight | Microsoft Docs
description: "Erfahren Sie, wie Sie ein Apache Spark-System in einem HDInsight-Cluster mit R Server erstellen, und dann ein R-Skript im Cluster übermitteln."
services: HDInsight
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.devlang: R
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/09/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 888a818bc0ea1804366dcb3932ac0e3f7b5802c6
ms.openlocfilehash: 51341d933525ef77d1f9ab604b212854b447ebc5


---
# <a name="get-started-using-r-server-on-hdinsight"></a>Erste Schritte mit R Server in HDInsight
HDInsight umfasst eine R Server-Option für die Integration in Ihren HDInsight-Cluster. Dadurch können R-Skripts Spark und MapReduce verwenden, um verteilte Berechnungen auszuführen. In diesem Dokument erfahren Sie, wie Sie eine R Server-Instanz in einem HDInsight-Cluster erstellen und dann ein R-Skript ausführen, das die Verwendung von Spark für verteilte R-Berechnungen veranschaulicht.

## <a name="prerequisites"></a>Voraussetzungen
* **Ein Azure-Abonnement**: Bevor Sie mit diesem Tutorial beginnen, müssen Sie über ein Azure-Abonnement verfügen. Weitere Informationen finden Sie unter [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Ein Secure Shell-Client (SSH)**: Ein SSH-Client wird verwendet, um Remoteverbindungen mit dem HDInsight-Cluster herzustellen und Befehle direkt im Cluster auszuführen. Linux-, Unix- und OS X-Systeme stellen einen SSH-Client über den Befehl `ssh` bereit. Für Windows-Systeme wird [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)empfohlen.

  * **SSH-Schlüssel (optional)**: Sie können das SSH-Konto zur Verbindungsherstellung mit dem Cluster mit einem Kennwort oder einem öffentlichen Schlüssel sichern. Die Verwendung eines Kennworts ist einfacher und ermöglicht Ihnen den Einstieg, ohne ein öffentliches/privates Schlüsselpaar zu erstellen. Die Verwendung eines Schlüssels ist jedoch sicherer.

      Für die Schritte in diesem Dokument wird davon ausgegangen, dass Sie ein Kennwort verwenden. Informationen zum Erstellen und Verwenden von SSH-Schlüsseln mit HDInsight finden Sie in den folgenden Dokumenten:

    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

### <a name="access-control-requirements"></a>Voraussetzungen für die Zugriffssteuerung
[!INCLUDE [access-control](../../includes/hdinsight-access-control-requirements.md)]

## <a name="create-the-cluster"></a>Cluster erstellen
> [!NOTE]
> In den Schritten dieses Dokuments wird beschrieben, wie Sie einen R Server in einem HDInsight-Cluster mit grundlegenden Konfigurationsinformationen erstellen. Informationen zu anderen Clusterkonfigurationseinstellungen (z.B. zum Hinzufügen zusätzlicher Speicherkonten, zum Verwenden eines virtuellen Azure-Netzwerks oder zum Erstellen eines Metastore für Hive) finden Sie unter [Erstellen von Linux-basierten HDInsight-Clustern](hdinsight-hadoop-provision-linux-clusters.md).
>
> 

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Wählen Sie **NEU**, **Intelligence + Analyse** und dann **HDInsight**.

    ![Bild der Erstellung eines neuen Clusters](./media/hdinsight-getting-started-with-r/newcluster.png)

3. Geben Sie im Feld **Clustername** einen Namen für den Cluster ein. Wenn Sie über mehrere Azure-Abonnements verfügen, wählen Sie über den Eintrag **Abonnement** das Abonnement aus, das Sie verwenden möchten.

    ![Auswahl von Clustername und Abonnement](./media/hdinsight-getting-started-with-r/clustername.png)

4. Wählen Sie die Option **Clusterkonfiguration**. Wählen Sie auf dem Blatt **Clusterkonfiguration** die folgenden Optionen aus:

   * **Clustertyp**: R Server
   * **Version**: Wählen Sie die Version von R Server, die Sie im Cluster installieren möchten. Wählen Sie die aktuelle Version, um von den neuesten Funktionen zu profitieren. Andere Versionen stehen zur Verfügung, falls diese aus Kompatibilitätsgründen benötigt werden. Versionshinweise zu den verfügbaren Versionen stehen [hier](https://msdn.microsoft.com/en-us/microsoft-r/notes/r-server-notes) zur Verfügung.
   * **RStudio Community Edition für R Server**: Diese browserbasierte IDE wird standardmäßig auf dem Edgeknoten installiert.  Wenn Sie keine Installation wünschen, deaktivieren Sie das Kontrollkästchen. Wenn Sie eine Installation durchführen, finden Sie die URL zum Zugriff auf die RStudio Server-Anmeldung auf einem Blatt der Portalanwendung für Ihren Cluster, nachdem dieser erstellt wurde.
     Behalten Sie für die anderen Optionen die Standardwerte bei, und verwenden Sie die Schaltfläche **Auswählen**, um den Clustertyp zu speichern.

     ![Screenshot des Blatts „Clustertyp“](./media/hdinsight-getting-started-with-r/clustertypeconfig.png)

   

5. Klicken Sie auf **Anmeldeinformationen**, und geben Sie dann ein **Clusteranmeldekennwort** und einen **Clusterbenutzernamen** ein.

    Geben Sie einen **SSH-Benutzernamen** ein.  SSH wird verwendet, um mithilfe eines **Secure Shell**-Clients (SSH) eine Remoteverbindung mit dem Cluster herzustellen. Sie können den SSH-Benutzer entweder in diesem Dialogfeld oder nach der Erstellung des Clusters (auf der Registerkarte „Konfiguration“ des Clusters) angeben. R Server ist so konfiguriert, dass „remoteuser“ als **SSH-Benutzername** erwartet wird.  Wenn Sie einen anderen Benutzernamen wählen, müssen Sie nach Erstellung des Clusters einen zusätzlichen Schritt ausführen.

    ![Blatt "Anmeldeinformationen"](./media/hdinsight-getting-started-with-r/clustercredentials.png)

    **SSH-Authentifizierungstyp**: Wählen Sie **KENNWORT** als Authentifizierungstyp, es sei denn, Sie bevorzugen einen öffentlichen Schlüssel.  Sie benötigen ein Paar aus einem öffentlichen und privaten Schlüssel, wenn Sie über einen Remoteclient wie RTVS, RStudio oder eine andere Desktop-IDE auf R Server im Cluster zugreifen möchten. Beachten Sie, dass Sie das SSH-Kennwort angeben müssen, wenn Sie die RStudio Server Community Edition installieren.     

    Wählen Sie zum Erstellen und Nutzen eines Paares aus einem öffentlichen und privaten Schlüssel PUBLIC KEY aus, und fahren Sie wie folgt fort.  Diese Anweisungen setzen voraus, dass Cygwin mit ssh-keygen o.ä. installiert ist.

   * Generieren Sie auf Ihrem Laptop über die Befehlszeile ein Paar mit einem öffentlichen und privaten Schlüssel:

        ssh-keygen -t rsa -b 2048 –f <Dateiname_privater_Schlüssel>

   * Dadurch wird eine Datei mit einem privaten Schlüssel und eine Datei mit einem öffentlichen Schlüssel mit dem Namen <Dateiname_privater_Schlüssel>.pub erstellt, z.B. „davec“ und „davec.pub“.  Geben Sie dann die Datei mit dem öffentlichen Schlüssel (*.pub) beim Zuweisen von Anmeldeinformationen für den HDI-Cluster an:

     ![Blatt „Anmeldeinformationen“](./media/hdinsight-getting-started-with-r/publickeyfile.png)  

   * Ändern von Berechtigungen für die Datei mit dem privaten Schlüssel auf Ihrem Laptop

        chmod 600 <Dateiname_privater_Schlüssel>

   * Verwenden Sie die Datei mit dem privaten Schlüssel mit SSH für die Remoteanmeldung. Beispiel:

        ssh –i <Dateiname_privater_Schlüssel> remoteuser@<hostname public ip>

     Sie können die Datei auch als Teil der Definition Ihres Hadoop Spark-Rechenkontexts für R Server auf dem Client verwenden. Weitere Informationen hierzu finden Sie unter „Using Microsoft R Server as a Hadoop Client“ im Abschnitt [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) („Verwenden von Microsoft R Server als Hadoop-Client“ im Abschnitt „Erstellen eines Rechenkontexts für Spark“) in der Onlineanleitung [Get started with ScaleR on Apache Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started) (Erste Schritte mit ScaleR unter Apache Spark).

6. Wählen Sie die Option **Datenquelle**, um eine Datenquelle als primären Speicherort des HDFS-Dateisystems auszuwählen, das vom Cluster verwendet wird. Wählen Sie entweder ein neues oder ein vorhandenes Azure Storage-Konto bzw. ein vorhandenes Data Lake Storage-Konto aus.

   1. Wählen Sie bei Auswahl eines Azure Storage-Kontos ein vorhandenes Speicherkonto aus, indem Sie auf **Speicherkonto auswählen** klicken und dann das Konto auswählen, oder erstellen Sie mit dem Link **Neu erstellen** im Abschnitt **Speicherkonto auswählen** ein neues Konto.

      > [!NOTE]
      > Wenn Sie **Neu** auswählen, müssen Sie einen Namen für das neue Speicherkonto eingeben. Wenn der Name akzeptiert wird, wird ein grünes Häkchen angezeigt.

      Als **Standardcontainer** wird standardmäßig der Name des Clusters verwendet. Ändern Sie diesen Wert nicht.

      Wenn eine neue Speicherkontooption ausgewählt wurde, können Sie die Option **Standort** verwenden, um die Region für die Erstellung des Speicherkontos auszuwählen.

      > [!IMPORTANT]
      > Durch die Auswahl des Standorts für die Standarddatenquelle wird auch der Standort des HDInsight-Clusters festgelegt. Der Cluster und die Standarddatenquelle müssen sich in derselben Region befinden.

   2. Wenn Sie die Verwendung einer vorhandenen Data Lake Store-Instanz wählen, geben Sie das gewünschte ADLS-Speicherkonto an und fügen die ADD-Identität des Clusters dem Cluster hinzu, um den Zugriff auf den Speicher zu ermöglichen.  Weitere Informationen zu diesem Prozess finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).

      Verwenden Sie die Schaltfläche **Auswählen** , um die Datenquellenkonfiguration zu speichern.

       ![Blatt „Datenquelle“](./media/hdinsight-getting-started-with-r/datastore.png)

7. Wählen Sie **Knotenpreistarife** aus, um Informationen zu den Knoten anzuzeigen, die für diesen Cluster erstellt werden. Wenn Sie wissen, dass Sie keinen größeren Cluster benötigen, behalten Sie für die Anzahl der Workerknoten den Standardwert `4`bei. Die vorkalkulierten Kosten für den Cluster werden auf dem Blatt angezeigt.

   > [!NOTE]
   > Bei Bedarf können Sie im Portal die Größe Ihres Clusters später ändern (Cluster -> Einstellungen -> Cluster skalieren), um die Anzahl der Workerknoten zu erhöhen oder zu verringern.  Dies kann nützlich sein, um den Cluster herunterzufahren, wenn er nicht verwendet wird, oder zum Hinzufügen von Kapazität zum Erfüllen der Anforderungen größerer Aufgaben.
   >
   > 

    Berücksichtigen Sie beim Ändern der Größe Ihres Clusters, der Datenknoten und der Edgeknoten die folgenden Faktoren:  

   * Die Leistung von verteilten R Server-Analysen in Spark ist proportional zur Anzahl der Workerknoten, wenn die Datenmenge groß ist.  

   * Die Leistung von R Server-Analysen ist linear zur Größe der analysierten Daten. Beispiel:  

     * Bei kleinen bis mittelgroßen Datenmengen ist die Leistung am besten, wenn die Analyse in einem lokalen Rechenkontext auf dem Edgeknoten erfolgt.  Weitere Informationen zu den Szenarien, in denen der lokale und der Spark-Rechenkontext am besten funktionieren, finden Sie unter „Rechenkontextoptionen für R Server in HDInsight“.<br>
     * Wenn Sie sich beim Edgeknoten anmelden und Ihr R-Skript ausführen, werden alle Funktionen mit Ausnahme der rx-Funktionen von ScaleR <strong>lokal</strong> auf dem Edgeknoten ausgeführt, weshalb der Arbeitsspeicher und die Anzahl der Kerne auf dem Edgeknoten entsprechend dimensioniert sein müssen. Dasselbe gilt, wenn Sie R Server in HDI als Remoterechenkontext auf Ihrem Laptop verwenden.

     ![Blatt „Knotenpreistarife“](./media/hdinsight-getting-started-with-r/pricingtier.png)

     Klicken Sie auf die Schaltfläche **Auswählen** , um die Konfiguration der Knotenpreise zu speichern.

8. Wählen Sie die gewünschte **Ressourcengruppe** aus, indem Sie entweder eine vorhandene Ressourcengruppe auswählen oder eine neue erstellen. Geben Sie bei Auswahl der Option **Neu erstellen** den Namen der neuen Ressourcengruppe ein. Wenn der neue Gruppenname verfügbar ist, wird neben dem Namen ein grünes Häkchen angezeigt.

   ![Blatt „Knotenpreistarife“](./media/hdinsight-getting-started-with-r/useexistingrg.png)

   

9. Nachdem Sie die Auswahl überprüft haben, können Sie den Cluster erstellen. Wählen Sie hierzu die Option **An Startmenü anheften** und dann **Erstellen**. Der Cluster wird erstellt, und dem Startmenü Ihres Azure-Portals wird eine Kachel für den Cluster hinzugefügt.

   Sie sehen, dass auch ein Link für **Automatisierungsoptionen** vorhanden ist. Wenn Sie auf diesen Link klicken, werden Skripts angezeigt. Sie können diese Skripts verwenden, um die Erstellung eines Clusters mit der ausgewählten Konfiguration zu automatisieren. Diese Skripts sind auch über den Azure-Portal-Eintrag Ihres Clusters verfügbar, nachdem dieser erstellt wurde.

   | Während der Erstellung                           | Erstellung abgeschlossen                        |
   | ---------------------------------------- | ---------------------------------------- |
   | ![Erstellungsanzeige im Startmenü](./media/hdinsight-getting-started-with-r/provisioning.png) | ![Kachel für einen erstellten Cluster](./media/hdinsight-getting-started-with-r/provisioned.png) |

   > [!NOTE]
   > Die Erstellung des Clusters dauert in der Regel ca. 20 Minuten. Sie können den Status des Erstellungsprozesses auf der Kachel im Startmenü oder im linken Bereich der Seite unter **Benachrichtigungen überprüfen**.
   >
   > 

## <a name="connect-to-rstudio-server"></a>Herstellen einer Verbindung mit RStudio Server

Wenn Sie sich für den Einschluss der RStudio Server Community Edition in Ihre Installation entschieden haben, können Sie über zwei verschiedene Methoden auf die RStudio-Anmeldung zugreifen.

1. Entweder verwenden Sie die folgende URL (bei der **CLUSTERNAME** für den Namen des erstellten Clusters steht): 

    https://**CLUSTERNAME**.azurehdinsight.net/rstudio/

2. Oder Sie öffnen den Eintrag für Ihren Cluster im Azure-Portal, indem Sie den Quicklink für die R Server-Dashboards aufrufen und dann das RStudio-Dashboard auswählen:

     ![Zugriff auf das RStudio-Dashboard](./media/hdinsight-getting-started-with-r/rstudiodashboard.png)

   > [!IMPORTANT]
   > Unabhängig von der gewählten Zugriffsmethode werden Sie bei der ersten Anmeldung zweimal zur Authentifizierung aufgefordert.  Bei der ersten Authentifizierung geben Sie die Benutzer-ID und das Kennwort für den Clusteradministrator an. Bei der zweiten Aufforderung geben Sie die Benutzer-ID und das Kennwort für SSH an. Bei nachfolgenden Anmeldungen sind nur die Benutzer-ID und das Kennwort für SSH erforderlich. 

## <a name="connect-to-the-r-server-edge-node"></a>Herstellen einer Verbindung mit dem R Server-Edgeknoten
Stellen Sie über SSH eine Verbindung mit dem R Server-Edgeknoten des HDInsight-Clusters her:

    ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

> [!NOTE]
> Sie finden die Adresse `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` auch im Azure-Portal, indem Sie den Cluster und dann **Alle Einstellungen**, **Apps** und **R Server** auswählen. Dadurch werden die SSH-Endpunktinformationen für den Edgeknoten angezeigt.
>
> ![Bild des SSH-Endpunkts für den Edgeknoten](./media/hdinsight-getting-started-with-r/sshendpoint.png)
>
> 

Wenn Sie zum Schutz Ihres SSH-Benutzerkontos ein Kennwort verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen öffentlichen Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` verwenden, um den passenden privaten Schlüssel anzugeben. Beispiel: `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`.

Weitere Informationen zum Verwenden von SSH mit Linux-basierten HDInsight-Clustern finden Sie in den folgenden Artikeln:

* [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
* [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

Nachdem die Verbindung hergestellt wurde, wird eine Eingabeaufforderung wie die folgende angezeigt.

    username@ed00-myrser:~$

## <a name="use-the-r-console"></a>Verwenden der R-Konsole
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
2. An der `>` -Eingabeaufforderung können Sie R-Code eingeben. Zu R Server gehören Pakete, mit denen Sie auf einfache Weise mit Hadoop interagieren und verteilten Berechnungen ausführen können. Verwenden Sie beispielsweise den folgenden Befehl, um das Stammverzeichnis des Standarddateisystems für den HDInsight-Cluster anzuzeigen.

        rxHadoopListFiles("/")

    Sie können auch eine Adressierung mit der WASB-Syntax verwenden.

        rxHadoopListFiles("wasbs:///")

## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Verwenden von R Server in HDI in einer Remote-Instanz von Microsoft R Server oder Microsoft R Client
Gemäß dem oben stehenden Abschnitt zum Verwenden von öffentlichen/privaten Schlüsselpaaren für den Zugriff auf den Cluster ist es möglich, den Zugriff auf den HDI Hadoop Spark-Rechenkontext von einer Remote-Instanz von Microsoft R Server oder Microsoft R Client einzurichten, die auf einem Desktop- oder Laptopcomputer ausgeführt wird. Näheres dazu finden Sie unter „Using Microsoft R Server as a Hadoop Client“ im Abschnitt [Creating a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) („Verwenden von Microsoft R Server als Hadoop-Client“ im Abschnitt „Erstellen eines Berechnungskontexts für Spark“) in der Onlineanleitung [RevoScaleR Hadoop Spark Getting Started](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started) (Erste Schritte mit RevoScaleR Hadoop Spark).  Hierfür müssen Sie die folgenden Optionen beim Definieren des RxSpark-Rechenkontexts auf Ihrem Laptop angeben: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches und sshProfileScript. Beispiel:

    myNameNode <- "default"
    myPort <- 0 
    
    mySshHostname  <- 'rkrrehdi1-ed-ssh.azurehdinsight.net'  # HDI secure shell hostname
    mySshUsername  <- 'remoteuser'# HDI SSH username
    mySshSwitches  <- '-i /cygdrive/c/Data/R/davec'   # HDI SSH private key
    
    myhdfsShareDir <- paste("/user/RevoShare", mySshUsername, sep="/")
    myShareDir <- paste("/var/RevoShare" , mySshUsername, sep="/")
    
    mySparkCluster <- RxSpark(
      hdfsShareDir = myhdfsShareDir,
      shareDir     = myShareDir,
      sshUsername  = mySshUsername,
      sshHostname  = mySshHostname,
      sshSwitches  = mySshSwitches,
      sshProfileScript = '/etc/profile',
      nameNode     = myNameNode,
      port         = myPort,
      consoleOutput= TRUE
    )



## <a name="use-a-compute-context"></a>Verwenden eines Rechenkontexts
Mit einem Rechenkontext können Sie steuern, ob die Berechnung lokal auf dem Edgeknoten ausgeführt wird oder ob sie auf die Knoten im HDInsight-Cluster verteilt wird.

1. Verwenden Sie über RStudio Server oder die R-Konsole (in einer SSH-Sitzung) Folgendes, um Beispieldaten in den Standardspeicher für HDInsight zu laden.

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

3. Jetzt führen wir mit dem lokalen Rechenkontext eine logistische Regression für die Daten aus.

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

   > [!NOTE]
   > Sie können auch MapReduce verwenden, um die Berechnung auf Clusterknoten zu verteilen. Weitere Informationen zum Rechenkontext finden Sie unter [Rechenkontextoptionen für R Server in HDInsight](hdinsight-hadoop-r-server-compute-contexts.md).
   >
   > 

## <a name="distribute-r-code-to-multiple-nodes"></a>Verteilen von R-Code auf mehrere Knoten
Mit R Server können Sie mithilfe von `rxExec`vorhandenen R-Code einfach nutzen und auf mehreren Knoten im Cluster ausführen. Dies ist nützlich für Parameterbereinigungen oder für Simulationen. Im folgenden Beispiel wird die Verwendung von `rxExec`veranschaulicht.

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Wenn Sie noch den Spark- oder MapReduce-Kontext verwenden, wird der nodename-Wert für die Workerknoten zurückgegeben, auf denen der Code `(Sys.info()["nodename"])` ausgeführt wurde. In einem Cluster mit vier Knoten könnten Sie z. B. eine Ausgabe wie die folgende erhalten.

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

## <a name="accessing-data-in-hive-and-parquet"></a>Zugriff auf Daten in Hive und Parquet
Ein neues Feature in R Server 9.0 und höheren Versionen bietet direkten Zugriff auf Daten in Hive und Parquet, um diese für ScaleR-Funktionen im Spark-Rechenkontext zu verwenden. Diese Funktionalität steht über die neuen ScaleR-Datenquellenfunktionen namens „RxHiveData“ und „RxParquetData“ zur Verfügung, die Daten unter Verwendung von Spark SQL direkt in einen Spark-Dataframe laden, um sie mit ScaleR zu analysieren.  

Der nachfolgende Beispielcode zeigt die Verwendung der neuen Funktionen: 



```
#..create a Spark compute context

myHadoopCluster <- rxSparkConnect(reset = TRUE)
```


```
#..retrieve some sample data from Hive and run a model 

hiveData <- RxHiveData("select * from hivesampletable", 
                 colInfo = list(devicemake = list(type = "factor")))
rxGetInfo(hiveData, getVarInfo = TRUE)

rxLinMod(querydwelltime ~ devicemake, data=hiveData)
```


```
#..retrieve some sample data from Parquet and run a model 

rxHadoopMakeDir('/share')
rxHadoopCopyFromLocal(file.path(rxGetOption('sampleDataDir'), 'claimsParquet/'), '/share/')
pqData <- RxParquetData('/share/claimsParquet',
                 colInfo = list(
            age    = list(type = "factor"),
           car.age = list(type = "factor"),
              type = list(type = "factor")
         ) )
rxGetInfo(pqData, getVarInfo = TRUE)

rxNaiveBayes(type ~ age + cost, data = pqData)
```


``` 
#..check on Spark data objects, cleanup, and close the Spark session 

lsObj <- rxSparkListData() # two data objs are cached
lsObj
rxSparkRemoveData(lsObj)
rxSparkListData() # it should show empty list
rxSparkDisconnect(myHadoopCluster)
```

Zusätzliche Informationen zur Verwendung dieser neuen Funktionen finden Sie in der Onlinehilfe zu R Server über die Befehle „?RxHivedata“ und „?RxParquetData“.  


## <a name="install-r-packages"></a>Installieren von R-Paketen
Falls Sie zusätzliche R-Pakete auf dem Edgeknoten installieren möchten, können Sie `install.packages()` direkt in der R-Konsole verwenden, wenn Sie über SSH eine Verbindung mit dem Edgeknoten hergestellt haben. Wenn Sie jedoch R-Pakete auf den Workerknoten des Clusters installieren möchten, müssen Sie eine Skriptaktion verwenden.

Skriptaktionen sind Bash-Skripts, mit denen Konfigurationsänderungen am HDInsight-Cluster vorgenommen werden oder zusätzliche Software installiert wird. In diesem Fall werden sie zum Installieren zusätzlicher R-Pakete genutzt. Gehen Sie folgendermaßen vor, um zusätzliche Pakete mit einer Skriptaktion zu installieren.

> [!IMPORTANT]
> Skriptaktionen können nur zum Installieren zusätzlicher R-Pakete verwendet werden, nachdem der Cluster erstellt wurde. Sie sollten nicht während der Clustererstellung verwendet werden, da für das Skript R Server vollständig installiert und konfiguriert sein muss.
>
> 

1. Wählen Sie im [Azure-Portal](https://portal.azure.com)Ihren R Server im HDInsight-Cluster aus.
2. Wählen Sie auf dem Blatt **Einstellungen** die Option **Skriptaktionen** und dann **Neue übermitteln**, um eine neue Skriptaktion zu übermitteln.

    ![Bild des Blatts „Skriptaktionen“](./media/hdinsight-getting-started-with-r/newscriptaction.png)
3. Geben Sie auf dem Blatt **Skriptaktion übermitteln** die folgenden Informationen an.

   * **Name**: Ein Anzeigename zum Identifizieren dieses Skripts

   * **Bash-Skript-URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Hauptknoten**: Diese Option muss **deaktiviert** sein.

   * **Worker**: Diese Option muss **aktiviert** sein.

   * **Edgeknoten**: Diese Option muss **deaktiviert** sein.

   * **Zookeeper**: Diese Option muss **deaktiviert** sein.

   * **Parameter**: Die zu installierenden R-Pakete. Beispiel: `bitops stringr arules`

   * **Speichern Sie diese Skript**: Diese Option muss **aktiviert** sein.  

     > [!NOTE]
     > 1. Standardmäßig werden alle R-Pakete über eine Momentaufnahme des Microsoft MRAN-Repositorys erstellt, die der installierten Version von R Server entspricht.  Wenn Sie neuere Versionen von Paketen installieren möchten, besteht das Risiko von Inkompatibilitäten. Eine Installation ist jedoch möglich, indem `useCRAN` als erstes Element der Paketliste angegeben wird, z.B. `useCRAN bitops, stringr, arules`.  
     > 2. Für einige R-Paket werden zusätzliche Linux-Systembibliotheken benötigt. Um Ihnen die Arbeit zu erleichtern, haben wird die abhängigen Komponenten für die 100 populärsten R-Pakete für Sie vorinstalliert. Wenn für die R-Pakete, die Sie installieren, weitere Bibliotheken hinzugefügt werden müssen, müssen Sie das hier verwendete Basisskript herunterladen und Schritte zum Installieren der Systembibliotheken hinzufügen. Dann müssen Sie das geänderte Skript in einen öffentlichen Blobcontainer im Azure-Speicher hochladen und das geänderte Skript zum Installieren der Pakete verwenden.
     >    Weitere Informationen zum Entwickeln von Skriptaktionen finden Sie unter [Entwickeln von Skriptaktionen](hdinsight-hadoop-script-actions-linux.md).  
     >
     >

     ![Hinzufügen einer Skriptaktion](./media/hdinsight-getting-started-with-r/scriptaction.png)
     
4. Wählen Sie **Erstellen** aus, um das Skript auszuführen. Nach Abschluss der Skriptausführung sind die R-Pakete auf allen Workerknoten verfügbar.

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie erfahren, wie Sie einen neuen HDInsight-Cluster mit Verwendung von R Server erstellen, und Sie haben die Grundlagen der Verwendung der R-Konsole aus einer SSH-Sitzung kennengelernt. Unter den folgenden Links erhalten Sie Informationen zu weiteren Möglichkeiten für die Arbeit mit R Server in HDInsight.

* [Hinzufügen von RStudio Server zu HDInsight (falls die Installation nicht bereits während der Clustererstellung durchgeführt wurde)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Rechenkontextoptionen für R Server in HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Azure Storage-Optionen für R Server in HDInsight](hdinsight-hadoop-r-server-storage.md)




<!--HONumber=Dec16_HO2-->


