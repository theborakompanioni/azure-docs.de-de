---
title: "Erste Schritte mit R Server in HDInsight – Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie ein Apache Spark-System in einem HDInsight-Cluster mit R Server erstellen und ein R-Skript im Cluster übermitteln."
services: HDInsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b5e111f3-c029-436c-ba22-c54a4a3016e3
ms.service: HDInsight
ms.custom: hdinsightactive
ms.devlang: R
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/13/2017
ms.author: bradsev
ms.translationtype: HT
ms.sourcegitcommit: 818f7756189ed4ceefdac9114a0b89ef9ee8fb7a
ms.openlocfilehash: ee4298b91f4e2b215b5faabaad96323f4ef234b8
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="get-started-using-r-server-on-hdinsight"></a>Erste Schritte mit R Server in HDInsight

HDInsight umfasst eine R Server-Option für die Integration in Ihren HDInsight-Cluster. Mit dieser Option können R-Skripts Spark und MapReduce verwenden, um verteilte Berechnungen auszuführen. In diesem Dokument erfahren Sie, wie Sie eine R Server-Instanz in einem HDInsight-Cluster erstellen und dann ein R-Skript ausführen, das die Verwendung von Spark für verteilte R-Berechnungen veranschaulicht.


## <a name="prerequisites"></a>Voraussetzungen

* **Ein Azure-Abonnement**: Bevor Sie mit diesem Tutorial beginnen, müssen Sie über ein Azure-Abonnement verfügen. Weitere Informationen finden Sie im Artikel [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (Anfordern einer kostenlosen Azure-Version zum Testen von Hadoop in HDInsight).
* **Ein Secure Shell-Client (SSH)**: Ein SSH-Client wird verwendet, um Remoteverbindungen mit dem HDInsight-Cluster herzustellen und Befehle direkt im Cluster auszuführen. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).
* **SSH-Schlüssel (optional)**: Sie können das SSH-Konto zur Verbindungsherstellung mit dem Cluster mit einem Kennwort oder einem öffentlichen Schlüssel sichern. Die Verwendung eines Kennworts ist einfacher und ermöglicht Ihnen den Einstieg, ohne ein öffentliches/privates Schlüsselpaar zu erstellen. Die Verwendung eines Schlüssels ist jedoch sicherer.

> [!NOTE]
> Für die Schritte in diesem Dokument wird davon ausgegangen, dass Sie ein Kennwort verwenden.


## <a name="automated-cluster-creation"></a>Automatisierte Clustererstellung

Sie können die HDInsight R Server-Erstellung automatisieren, indem Sie Azure Resource Manager-Vorlagen, das SDK und PowerShell verwenden.

* Informationen zur R Server-Erstellung mit einer Azure Resource Manager-Vorlage finden Sie unter [Deploy an R Server HDInsight cluster](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/) (Bereitstellen eines R Server-HDInsight-Clusters).
* Informationen zur R Server-Erstellung mit dem .NET SDK finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mit dem .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md).
* Informationen zur R Server-Bereitstellung mit PowerShell finden Sie im Artikel [Creating an R Server on HDInsight with PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) (Erstellen einer R Server-Instanz unter HDInsight mit PowerShell).


<a name="create-hdi-custer-with-aure-portal"></a>
## <a name="create-the-cluster-using-the-azure-portal"></a>Erstellen des Clusters mit dem Azure-Portal

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie auf **NEU** -> **Intelligence und Analyse** -> **HDInsight**.

    ![Bild der Erstellung eines neuen Clusters](./media/hdinsight-hadoop-r-server-get-started/newcluster.png)

3. Geben Sie in der Oberfläche **Schnellerfassung** im Feld **Clustername** einen Namen für den Cluster ein. Wenn Sie über mehrere Azure-Abonnements verfügen, wählen Sie über den Eintrag **Abonnement** das Abonnement aus, das Sie verwenden möchten.

    ![Auswahl von Clustername und Abonnement](./media/hdinsight-hadoop-r-server-get-started/clustername.png)

4. Wählen Sie **Clustertyp**, um das Blatt **Clusterkonfiguration** zu öffnen. Wählen Sie auf dem Blatt **Clusterkonfiguration** die folgenden Optionen aus:

    * **Clustertyp**: R Server
    * **Version**: Wählen Sie die Version von R Server, die Sie im Cluster installieren möchten. Die derzeit verfügbare Version ist ***R Server 9.1 (HDI 3.6)***. Versionshinweise zu den verfügbaren Versionen von R Server finden Sie [hier](https://msdn.microsoft.com/microsoft-r/notes/r-server-notes).
    * **RStudio Community Edition für R Server**: Diese browserbasierte IDE wird standardmäßig auf dem Edgeknoten installiert. Wenn Sie keine Installation wünschen, deaktivieren Sie das Kontrollkästchen. Wenn Sie eine Installation durchführen, finden Sie die URL zum Zugriff auf die RStudio Server-Anmeldung auf einem Blatt der Portalanwendung für Ihren Cluster, nachdem dieser erstellt wurde.
    * Behalten Sie für die anderen Optionen die Standardwerte bei, und verwenden Sie die Schaltfläche **Auswählen**, um den Clustertyp zu speichern.

        ![Screenshot des Blatts „Clustertyp“](./media/hdinsight-hadoop-r-server-get-started/clustertypeconfig.png)

5. Geben Sie einen **Benutzernamen für Clusteranmeldung** und ein **Kennwort für Clusteranmeldung** ein.

    Geben Sie einen **SSH-Benutzernamen** ein. SSH wird verwendet, um mithilfe eines **Secure Shell**-Clients (SSH) eine Remoteverbindung mit dem Cluster herzustellen. Sie können den SSH-Benutzer in diesem Dialogfeld oder nach der Erstellung des Clusters (auf der Registerkarte „Konfiguration“ des Clusters) angeben. R Server ist so konfiguriert, dass „remoteuser“ als **SSH-Benutzername** erwartet wird.  **Wenn Sie einen anderen Benutzernamen auswählen, müssen Sie nach dem Erstellen des Clusters einen zusätzlichen Schritt ausführen.**

    Lassen Sie das Kontrollkästchen **Dasselbe Kennwort wie für die Clusteranmeldung verwenden** aktiviert, um **KENNWORT** als Authentifizierungstyp zu verwenden, solange Sie nicht die Verwendung eines öffentlichen Schlüssels bevorzugen.  Sie benötigen ein Paar aus einem öffentlichen und privaten Schlüssel, um über einen Remoteclient wie RTVS, RStudio oder eine andere Desktop-IDE auf R Server im Cluster zuzugreifen. Wenn Sie die Community Edition von RStudio Server installieren, müssen Sie ein SSH-Kennwort auswählen.     

    Um ein Paar aus einem öffentlichen und privaten Schlüssel zu erstellen und zu verwenden, deaktivieren Sie **Dasselbe Kennwort wie für die Clusteranmeldung verwenden**, wählen Sie **ÖFFENTLICHER SCHLÜSSEL** aus, und gehen Sie wie folgt vor. Diese Anweisungen setzen voraus, dass Cygwin mit ssh-keygen o.ä. installiert ist.

    * Generieren Sie auf Ihrem Laptop über die Befehlszeile ein Paar mit einem öffentlichen und privaten Schlüssel:

        ssh-keygen -t rsa -b 2048

    * Befolgen Sie die Aufforderung zum Benennen einer Schlüsseldatei, und geben Sie dann eine Passphrase zur Erhöhung der Sicherheit ein. Ihr Bildschirm sollte etwa folgendermaßen aussehen:

        ![SSH-Befehlszeile in Windows](./media/hdinsight-hadoop-r-server-get-started/sshcmdline.png)

    * Mit diesem Befehl wird eine Datei mit einem privaten Schlüssel und eine Datei mit einem öffentlichen Schlüssel mit dem Namen „<Dateiname_privater_Schlüssel>.pub“ erstellt, z.B. „furiosa“ und „furiosa.pub“.

        ![SSH-Verzeichnis](./media/hdinsight-hadoop-r-server-get-started/dir.png)

    * Geben Sie dann die Datei mit dem öffentlichen Schlüssel (&#42;.pub) beim Zuweisen von HDI-Clusteranmeldeinformationen ein, bestätigen Sie abschließend Ihre Ressourcengruppe und Region, und wählen Sie **Weiter** aus.

        ![Blatt „Anmeldeinformationen“](./media/hdinsight-hadoop-r-server-get-started/publickeyfile.png)  

   * Ändern von Berechtigungen für die Datei mit dem privaten Schlüssel auf Ihrem Laptop:

        chmod 600 <Dateiname_privater_Schlüssel>

   * Verwenden der Datei mit dem privaten Schlüssel mit SSH für die Remoteanmeldung:

        ssh –i <Dateiname_privater_Schlüssel> remoteuser@<hostname public ip>

      Oder als Teil die Definition Ihres Rechenkontextes in Hadoop Spark für R Server auf dem Client. Weitere Informationen finden Sie im Unterabschnitt **Using Microsoft R Server as a Hadoop Client** (Verwenden von Microsoft R Server als Hadoop-Client) in [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started#creating-a-compute-context-for-spark) (Erstellen eines Rechenkontextes für Spark).

6. Über die Schnellerfassung gelangen Sie zum Blatt **Speicher**, wo Sie die Speicherkontoeinstellungen für den primären Speicherort des vom Cluster genutzten HDFS-Dateisystems auswählen können. Wählen Sie entweder ein neues oder ein vorhandenes Azure Storage-Konto bzw. ein vorhandenes Data Lake Storage-Konto aus.

    - Ein vorhandenes Azure Storage-Konto wählen Sie aus, indem Sie **Speicherkonto auswählen** und dann das entsprechende Konto auswählen. Erstellen Sie ein neues Konto über den Link **Neu erstellen** im Abschnitt **Speicherkonto auswählen**.

      > [!NOTE]
      > Wenn Sie **Neu** auswählen, müssen Sie einen Namen für das neue Speicherkonto eingeben. Wenn der Name akzeptiert wurde, wird ein grünes Häkchen angezeigt.

      Als **Standardcontainer** wird standardmäßig der Name des Clusters verwendet. Ändern Sie diesen Standardwert nicht.

      Wenn eine neue Speicherkontooption ausgewählt wurde, wird eine Aufforderung zum Auswählen von **Standort** angezeigt, um die Region für die Erstellung des Speicherkontos auszuwählen.  

         ![Blatt „Datenquelle“](./media/hdinsight-getting-started-with-r/datastore.png)  

      > [!IMPORTANT]
      > Durch die Auswahl des Standorts für die Standarddatenquelle wird auch der Standort des HDInsight-Clusters festgelegt. Der Cluster und die Standarddatenquelle müssen sich in derselben Region befinden.

    - Wenn Sie eine vorhandene Data Lake Store-Instanz verwenden möchten, geben Sie das gewünschte ADLS-Speicherkonto an und fügen dem Cluster die *ADD*-Identität des Clusters hinzu, um den Zugriff auf den Speicher zu ermöglichen. Weitere Informationen zu diesem Vorgang finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-hdinsight-hadoop-use-portal).

    Verwenden Sie die Schaltfläche **Auswählen** , um die Datenquellenkonfiguration zu speichern.


7. Anschließend wird das Blatt **Zusammenfassung** angezeigt, auf dem Sie alle Einstellungen überprüfen können. Hier können Sie die **Clustergröße** ändern, um eine andere Anzahl von Servern im Cluster anzugeben, und auch **Skriptaktionen** angeben, die Sie ausführen möchten. Wenn Sie wissen, dass Sie keinen größeren Cluster benötigen, behalten Sie für die Anzahl der Workerknoten den Standardwert `4` bei. Die geschätzten Kosten für den Cluster werden auf dem Blatt angezeigt.

    ![Clusterzusammenfassung](./media/hdinsight-hadoop-r-server-get-started/clustersummary.png)

   > [!NOTE]
   > Bei Bedarf können Sie im Portal die Größe Ihres Clusters später ändern (**Cluster** -> **Einstellungen** -> **Cluster skalieren**), um die Anzahl von Workerknoten zu erhöhen oder zu verringern.  Diese Größenänderung kann zum Herunterfahren des Clusters nützlich sein, wenn er nicht verwendet wird, oder zum Hinzufügen von Kapazität zum Erfüllen der Anforderungen größerer Aufgaben.
   >
   >

   Berücksichtigen Sie beim Ändern der Größe Ihres Clusters, der Datenknoten und der Edgeknoten die folgenden Faktoren:  

   * Die Leistung von verteilten R Server-Analysen in Spark ist proportional zur Anzahl der Workerknoten, wenn die Datenmenge groß ist.  

   * Die Leistung von R Server-Analysen ist linear zur Größe der analysierten Daten. Beispiel:  

     * Bei kleinen bis mittelgroßen Datenmengen ist die Leistung am besten, wenn die Analyse in einem lokalen Rechenkontext auf dem Edgeknoten erfolgt.  Weitere Informationen zu den Szenarien, in denen der lokale und der Spark-Rechenkontext am besten funktionieren, finden Sie unter „Rechenkontextoptionen für R Server in HDInsight“.<br>
     * Wenn Sie sich beim Edgeknoten anmelden und Ihr R-Skript ausführen, werden alle Funktionen bis auf die ScaleR-rx-Funktionen <strong>lokal</strong> im Edgeknoten ausgeführt. Die Größe des Speichers und die Anzahl von Kernen im Edgeknoten sollten daher mit einer entsprechenden Größe festgelegt werden. Dasselbe gilt, wenn Sie R Server in HDI als Remoterechenkontext auf Ihrem Laptop verwenden.

     ![Blatt „Knotenpreistarife“](./media/hdinsight-hadoop-r-server-get-started/pricingtier.png)

     Klicken Sie auf die Schaltfläche **Auswählen** , um die Konfiguration der Knotenpreise zu speichern.

   Es ist auch ein Link zum **Herunterladen von Vorlage und Parametern** vorhanden. Klicken Sie auf diesen Link, um Skripts anzuzeigen, mit denen Sie die Erstellung eines Clusters mit der ausgewählten Konfiguration automatisieren können. Diese Skripts sind auch über den Azure-Portal-Eintrag Ihres Clusters verfügbar, nachdem dieser erstellt wurde.

   > [!NOTE]
   > Die Erstellung des Clusters dauert in der Regel ca. 20 Minuten. Sie können den Status des Erstellungsprozesses auf der Kachel im Startmenü oder im linken Bereich der Seite unter **Benachrichtigungen überprüfen**.
   >
   >

<a name="connect-to-rstudio-server"></a>
## <a name="connect-to-rstudio-server"></a>Herstellen einer Verbindung mit RStudio Server

Wenn Sie sich für den Einschluss der RStudio Server Community Edition in Ihre Installation entschieden haben, können Sie über zwei verschiedene Methoden auf die RStudio-Anmeldung zugreifen.

1. Verwenden Sie die folgende URL (bei der **CLUSTERNAME** für den Namen des erstellten Clusters steht):

    https://**CLUSTERNAME**.azurehdinsight.net/rstudio/

2. Öffnen Sie den Eintrag für Ihren Cluster im Azure-Portal, rufen Sie den Quicklink für die **R Server-Dashboards** auf, und wählen Sie dann das **R Studio-Dashboard** aus:

     ![Zugriff auf das RStudio-Dashboard](./media/hdinsight-getting-started-with-r/rstudiodashboard1.png)

     ![Zugriff auf das RStudio-Dashboard](./media/hdinsight-getting-started-with-r/rstudiodashboard2.png)

   > [!IMPORTANT]
   > Unabhängig von der verwendeten Zugriffsmethode müssen Sie sich bei der ersten Anmeldung zweimal authentifizieren.  Bei der ersten Authentifizierung geben Sie die *Benutzer-ID* und das *Kennwort für den Clusteradministrator* an. Bei der zweiten Aufforderung geben Sie die *Benutzer-ID* und das *Kennwort für SSH* an. Bei nachfolgenden Anmeldungen sind nur die *Benutzer-ID* und das *Kennwort für SSH* erforderlich.

<a name="connect-to-edge-node"></a>
## <a name="connect-to-the-r-server-edge-node"></a>Herstellen einer Verbindung mit dem R Server-Edgeknoten

Stellen Sie mit dem folgenden Befehl über SSH eine Verbindung mit dem R Server-Edgeknoten des HDInsight-Clusters her:

   `ssh USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net`

> [!NOTE]
> Sie finden die Adresse `USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net` auch im Azure-Portal, indem Sie den Cluster und dann **Alle Einstellungen** -> **Apps** -> **R Server** auswählen. Dadurch werden die SSH-Endpunktinformationen für den Edgeknoten angezeigt.
>
> ![Bild des SSH-Endpunkts für den Edgeknoten](./media/hdinsight-hadoop-r-server-get-started/sshendpoint.png)
>
>

Wenn Sie zum Schutz Ihres SSH-Benutzerkontos ein Kennwort verwendet haben, werden Sie zur Eingabe dieses Kennworts aufgefordert. Wenn Sie einen öffentlichen Schlüssel verwendet haben, müssen Sie möglicherweise den Parameter `-i` verwenden, um den passenden privaten Schlüssel anzugeben. Beispiel:

    ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Weitere Informationen finden Sie unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

Nachdem die Verbindung hergestellt wurde, wird eine Eingabeaufforderung wie die folgende angezeigt:

    sername@ed00-myrser:~$

<a name="enable-concurrent-users"></a>
## <a name="enable-multiple-concurrent-users"></a>Aktivieren mehrerer gleichzeitiger Benutzer

Sie können mehrere gleichzeitige Benutzern aktivieren, indem Sie mehr Benutzer für den Edgeknoten hinzufügen, auf dem die RStudio Community-Version ausgeführt wird.

Wenn Sie einen HDInsight-Cluster erstellen, müssen Sie zwei Benutzer (einen HTTP-Benutzer und einen SSH-Benutzer) angeben:

![Gleichzeitiger Benutzer 1](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-1.png)

- **Benutzername für Clusteranmeldung**: ein HTTP-Benutzer für die Authentifizierung über das HDInsight-Gateway, das zum Schutz der von Ihnen erstellten HDInsight-Cluster verwendet wird. Mit diesem HTTP-Benutzer wird auf die Ambari- und die YARN-Benutzeroberfläche sowie auf andere UI-Komponenten zugegriffen.
- **SSH-Benutzername (Secure Shell)**: ein SSH-Benutzer für den Zugriff auf den Cluster über Secure Shell. Dieser Benutzer ist ein Benutzer im Linux-System für alle Hauptknoten, Workerknoten und Edgeknoten. Somit können Sie Secure Shell zum Zugreifen auf einen beliebigen Knoten in einem Remotecluster verwenden.

Die R Studio Server Community-Version, die im Cluster vom Typ „Microsoft R Server in HDInsight“ verwendet wird, akzeptiert für die Anmeldung nur Linux-Benutzernamen und -Kennwörter. Die Übergabe von Token wird nicht unterstützt. Wenn Sie einen neuen Cluster erstellt haben und R Studio für den Zugriff darauf verwenden möchten, müssen Sie sich zweimal anmelden.

- Melden Sie sich zunächst mit den HTTP-Benutzeranmeldeinformationen über das HDInsight-Gateway an: 

    ![Gleichzeitiger Benutzer 2a](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2a.png)

- Verwenden Sie dann die SSH-Benutzeranmeldeinformationen für die Anmeldung bei RStudio:
  
    ![Gleichzeitiger Benutzer 2b](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-2b.png)

Derzeit kann bei der Bereitstellung eines HDInsight-Clusters nur ein SSH-Benutzerkonto erstellt werden. Um mehreren Benutzern den Zugriff auf Microsoft R Server in HDInsight-Clustern zu ermöglichen, müssen wir zusätzliche Benutzer im Linux-System erstellen.

Da RStudio Server Community auf dem Edgeknoten des Clusters ausgeführt wird, müssen mehrere Schritte ausgeführt werden:

1. Anmelden beim Edgeknoten mit dem erstellten SSH-Benutzer
2. Hinzufügen weiterer Linux-Benutzer im Edgeknoten
3. Verwenden der RStudio Community-Version mit dem erstellten Benutzer

### <a name="step-1-use-the-created-ssh-user-to-log-in-to-the-edge-node"></a>Schritt 1: Anmelden beim Edgeknoten mit dem erstellten SSH-Benutzer

Laden Sie ein SSH-Tool (z.B. PuTTY) herunter, und verwenden Sie den vorhandenen SSH-Benutzer für die Anmeldung. Befolgen Sie dann die Anweisungen unter [Herstellen einer Verbindung mit HDInsight (Hadoop) per SSH](hdinsight-hadoop-linux-use-ssh-unix.md), um auf den Edgeknoten zuzugreifen. Die Edgeknotenadresse für den Cluster „R Server in HDInsight“ lautet *clustername-ed-ssh.azurehdinsight.net*


### <a name="step-2-add-more-linux-users-in-edge-node"></a>Schritt 2: Hinzufügen weiterer Linux-Benutzer im Edgeknoten

Fügen Sie zum Hinzufügen eines Benutzers zum Edgeknoten die folgenden Befehle aus:

    sudo useradd yournewusername -m
    sudo passwd yourusername

Daraufhin sollte Folgendes zurückgegeben werden: 

![Gleichzeitiger Benutzer 3](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-3.png)

Wenn Sie aufgefordert werden, das aktuelle Kerberos-Kennwort einzugeben, drücken Sie einfach die EINGABETASTE, um die Aufforderung zu ignorieren****. Die `-m`-Option im Befehl `useradd` gibt an, dass das System für den Benutzer einen Basisordner erstellt, der für die RStudio Community-Version erforderlich ist.


### <a name="step-3-use-rstudio-community-version-with-the-user-created"></a>Schritt 3: Verwenden der RStudio Community-Version mit dem erstellten Benutzer

Melden Sie sich mit dem erstellten Benutzer bei RStudio an:

![Gleichzeitiger Benutzer 4](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-4.png)

Beachten Sie, dass RStudio angibt, dass Sie den neuen Benutzer (in diesem Fall etwa *sshuser6*) für die Anmeldung beim Cluster verwenden: 

![Gleichzeitiger Benutzer 5](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-5.png)

Sie können sich auch gleichzeitig in einem anderen Browserfenster mit den ursprünglichen Anmeldeinformationen (standardmäßig *sshuser*) anmelden.

Sie können einen Auftrag mithilfe von scaleR-Funktionen senden. Hier sehen Sie ein Beispiel für die zum Ausführen eines Auftrags verwendeten Befehle:

    # Set the HDFS (WASB) location of example data.
    bigDataDirRoot <- "/example/data"

    # Create a local folder for storaging data temporarily.
    source <- "/tmp/AirOnTimeCSV2012"
    dir.create(source)

    # Download data to the tmp folder.
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

    # Set directory in bigDataDirRoot to load the data.
    inputDir <- file.path(bigDataDirRoot,"AirOnTimeCSV2012")

    # Create the directory.
    rxHadoopMakeDir(inputDir)

    # Copy the data from source to input.
    rxHadoopCopyFromLocal(source, bigDataDirRoot)

    # Define the HDFS (WASB) file system.
    hdfsFS <- RxHdfsFileSystem()

    # Create info list for the airline data.
    airlineColInfo <- list(
    DAY_OF_WEEK = list(type = "factor"),
    ORIGIN = list(type = "factor"),
    DEST = list(type = "factor"),
    DEP_TIME = list(type = "integer"),
    ARR_DEL15 = list(type = "logical"))

    # Get all the column names.
    varNames <- names(airlineColInfo)

    # Define the text data source in HDFS.
    airOnTimeData <- RxTextData(inputDir, colInfo = airlineColInfo, varsToKeep = varNames, fileSystem = hdfsFS)

    # Define the text data source in local system.
    airOnTimeDataLocal <- RxTextData(source, colInfo = airlineColInfo, varsToKeep = varNames)

    # Specify the formula to use.
    formula = "ARR_DEL15 ~ ORIGIN + DAY_OF_WEEK + DEP_TIME + DEST"

    # Define the Spark compute context.
    mySparkCluster <- RxSpark()

    # Set the compute context.
    rxSetComputeContext(mySparkCluster)

    # Run a logistic regression.
    system.time(
        modelSpark <- rxLogit(formula, data = airOnTimeData)
    )

    # Display a summary.
    summary(modelSpark)


Beachten Sie, dass die übermittelten Aufträge auf der YARN-Benutzeroberfläche unter anderen Benutzernamen angezeigt werden:

![Gleichzeitiger Benutzer 6](./media/hdinsight-hadoop-r-server-get-started/concurrent-users-6.png)

Beachten Sie außerdem, dass die neu hinzugefügten Benutzer nicht über Stammberechtigungen im Linux-System verfügen, aber den gleichen Zugriff auf alle Dateien im HDFS- und WASB-Remotespeicher haben.


<a name="use-r-console"></a>
## <a name="use-the-r-console"></a>Verwenden der R-Konsole

1. Verwenden Sie in der SSH-Sitzung den folgenden Befehl, um die R-Konsole zu starten:  

        R

2. Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:
    
    R, Version 3.2.2 (2015-08-14) – „Fire Safety“, Copyright (C) 2015 The R Foundation for Statistical Computing, Plattform: x86_64-pc-linux-gnu (64 Bit)

    R ist eine kostenlose Software, für die KEINERLEI GARANTIEN übernommen werden.
    Sie dürfen Sie unter bestimmten Vorgaben gerne weiterverteilen.
    Geben Sie „license()“ oder „licence()“ ein, um Einzelheiten zur Verteilung zu erhalten.

    Unterstützung für natürliche Sprache, jedoch Ausführung in einem englischsprachigen Gebietsschema

    R ist ein gemeinschaftliches Projekt mit vielen Mitwirkenden.
    Geben Sie „contributors()“ ein, um weitere Informationen zu erhalten, und „citation()“, um zu erfahren, wie R oder R-Pakete in Veröffentlichungen genannt werden müssen.

    Geben Sie „demo()“ ein, um einige Demos zu erhalten, „help()“ für die Onlinehilfe und „help.start()“ für eine HTML-Browserschnittstelle zur Hilfe.
    Geben Sie „q()“ ein, um R zu beenden.

    Microsoft R Server, Version 8.0: eine erweiterte Verteilung von R-Microsoft-Paketen Copyright (C) 2016 Microsoft Corporation

    Geben Sie „readme()“ ein, um Versionshinweise zu erhalten.
    >

3. An der `>` -Eingabeaufforderung können Sie R-Code eingeben. Zu R Server gehören Pakete, mit denen Sie auf einfache Weise mit Hadoop interagieren und verteilten Berechnungen ausführen können. Verwenden Sie beispielsweise den folgenden Befehl, um das Stammverzeichnis des Standarddateisystems für den HDInsight-Cluster anzuzeigen:

    rxHadoopListFiles("/")

4. Sie können auch eine Adressierung mit der WASB-Syntax verwenden.

    rxHadoopListFiles("wasbs:///")


## <a name="using-r-server-on-hdi-from-a-remote-instance-of-microsoft-r-server-or-microsoft-r-client"></a>Verwenden von R Server in HDI in einer Remote-Instanz von Microsoft R Server oder Microsoft R Client

Es ist möglich, den Zugriff auf den HDI Hadoop Spark-Computekontext von einer auf einem Desktop- oder Laptopcomputer ausgeführten Remoteinstanz von Microsoft R Server oder Microsoft R Client einzurichten. Weitere Informationen finden Sie im Unterabschnitt **Using Microsoft R Server as a Hadoop Client** (Verwenden von Microsoft R Server als Hadoop-Client) in [Create a Compute Context for Spark](https://msdn.microsoft.com/microsoft-r/scaler-spark-getting-started.md) (Erstellen eines Rechenkontexts für Spark). Hierfür müssen Sie die folgenden Optionen beim Definieren des RxSpark-Rechenkontexts auf Ihrem Laptop angeben: hdfsShareDir, shareDir, sshUsername, sshHostname, sshSwitches und sshProfileScript. Beispiel:


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

Mit einem Rechenkontext können Sie steuern, ob die Berechnung lokal auf dem Edgeknoten ausgeführt oder auf die Knoten im HDInsight-Cluster verteilt wird.

1. Verwenden Sie über RStudio Server oder die R-Konsole (in einer SSH-Sitzung) den folgenden Code, um Beispieldaten in den Standardspeicher für HDInsight zu laden:

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

    Es sollte eine Ausgabe, die mit Zeilen wie den folgenden endet, angezeigt werden:

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


## <a name="distribute-r-code-to-multiple-nodes"></a>Verteilen von R-Code auf mehrere Knoten

Mit R Server können Sie mithilfe von `rxExec` vorhandenen R-Code einfach nutzen und auf mehreren Knoten im Cluster ausführen. Diese Funktion ist nützlich für Parameterbereinigungen oder für Simulationen. Im folgenden Code wird die Verwendung von `rxExec` veranschaulicht:

    rxExec( function() {Sys.info()["nodename"]}, timesToRun = 4 )

Wenn Sie noch den Spark- oder MapReduce-Kontext verwenden, gibt dieser Befehl den nodename-Wert für die Workerknoten zurück, auf denen der `(Sys.info()["nodename"])`-Code ausgeführt wird. In einem Cluster mit vier Knoten erwarten Sie beispielsweise eine Ausgabe wie die folgende:

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

Ein Feature in R Server 9.1 bietet direkten Zugriff auf Daten in Hive und Parquet, um diese für ScaleR-Funktionen im Spark-Rechenkontext zu verwenden. Diese Funktionalität steht über die neuen ScaleR-Datenquellenfunktionen namens „RxHiveData“ und „RxParquetData“ zur Verfügung, die Daten unter Verwendung von Spark SQL direkt in einen Spark-Dataframe laden, um sie mit ScaleR zu analysieren.  

Der nachfolgende Code zeigt die Verwendung der neuen Funktionen:

    #Create a Spark compute context:
    myHadoopCluster <- rxSparkConnect(reset = TRUE)

    #Retrieve some sample data from Hive and run a model:
    hiveData <- RxHiveData("select * from hivesampletable",
                     colInfo = list(devicemake = list(type = "factor")))
    rxGetInfo(hiveData, getVarInfo = TRUE)

    rxLinMod(querydwelltime ~ devicemake, data=hiveData)

    #Retrieve some sample data from Parquet and run a model:
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

    #Check on Spark data objects, cleanup, and close the Spark session:
    lsObj <- rxSparkListData() # two data objs are cached
    lsObj
    rxSparkRemoveData(lsObj)
    rxSparkListData() # it should show empty list
    rxSparkDisconnect(myHadoopCluster)


Zusätzliche Informationen zur Verwendung dieser neuen Funktionen finden Sie in der Onlinehilfe zu R Server über die Befehle `?RxHivedata` und `?RxParquetData`.  


## <a name="install-additional-r-packages-on-the-edge-node"></a>Installieren zusätzlicher R-Pakete auf dem Edgeknoten

Falls Sie zusätzliche R-Pakete auf dem Edgeknoten installieren möchten, können Sie `install.packages()` direkt in der R-Konsole verwenden, wenn Sie über SSH eine Verbindung mit dem Edgeknoten hergestellt haben. Wenn Sie jedoch R-Pakete auf den Workerknoten des Clusters installieren möchten, müssen Sie eine Skriptaktion verwenden.

Skriptaktionen sind Bash-Skripts, mit denen Konfigurationsänderungen am HDInsight-Cluster vorgenommen werden oder zusätzliche Software (etwa zusätzliche R-Pakete) installiert wird. Gehen Sie folgendermaßen vor, um zusätzliche Pakete mit einer Skriptaktion zu installieren:

> [!IMPORTANT]
> Skriptaktionen können nur zum Installieren zusätzlicher R-Pakete verwendet werden, nachdem der Cluster erstellt wurde. Verwenden Sie dieses Verfahren nicht bei der Clustererstellung, da für das Skript R Server vollständig installiert und konfiguriert sein muss.
>
>

1. Wählen Sie im [Azure-Portal](https://portal.azure.com)Ihren R Server im HDInsight-Cluster aus.

2. Wählen Sie auf dem Blatt **Einstellungen** die Option **Skriptaktionen** und dann **Neue übermitteln** aus, um eine neue Skriptaktion zu übermitteln.

   ![Bild des Blatts „Skriptaktionen“](./media/hdinsight-hadoop-r-server-get-started/scriptaction.png)

3. Geben Sie auf dem Blatt **Skriptaktion übermitteln** die folgenden Informationen an:

   * **Name**: Ein Anzeigename zum Identifizieren dieses Skripts

   * **Bash-Skript-URI**: `http://mrsactionscripts.blob.core.windows.net/rpackages-v01/InstallRPackages.sh`

   * **Hauptknoten**: Diese Option muss **deaktiviert** sein.

   * **Worker**: Diese Option muss **aktiviert** sein.

   * **Edgeknoten**: Diese Option muss **deaktiviert** sein.

   * **Zookeeper**: Diese Option muss **deaktiviert** sein.

   * **Parameter**: Die zu installierenden R-Pakete. Beispiel: `bitops stringr arules`

   * **Speichern Sie diese Skriptaktion...**: Diese Option muss **aktiviert** sein.  

   > [!NOTE]
   > 1. Standardmäßig werden alle R-Pakete über eine Momentaufnahme des Microsoft MRAN-Repositorys erstellt, die der installierten Version von R Server entspricht. Wenn Sie neuere Versionen der Pakete installieren möchten, besteht das Risiko von Inkompatibilitäten. Diese Art von Installation ist jedoch möglich, wenn Sie `useCRAN` als erstes Element der Paketliste angeben, z.B. `useCRAN bitops, stringr, arules`.  
   > 2. Für einige R-Pakete werden zusätzliche Linux-Systembibliotheken benötigt. Um Ihnen die Arbeit zu erleichtern, haben wird die abhängigen Komponenten für die 100 populärsten R-Pakete für Sie vorinstalliert. Wenn für die R-Pakete, die Sie installieren, weitere Bibliotheken hinzugefügt werden müssen, müssen Sie das hier verwendete Basisskript herunterladen und Schritte zum Installieren der Systembibliotheken hinzufügen. Dann müssen Sie das geänderte Skript in einen öffentlichen Blobcontainer im Azure-Speicher hochladen und das geänderte Skript zum Installieren der Pakete verwenden.
   >    Weitere Informationen zum Entwickeln von Skriptaktionen finden Sie unter [Entwickeln von Skriptaktionen](hdinsight-hadoop-script-actions-linux.md).  
   >
   >

   ![Hinzufügen einer Skriptaktion](./media/hdinsight-getting-started-with-r/submitscriptaction.png)

4. Wählen Sie **Erstellen** aus, um das Skript auszuführen. Nach Abschluss der Skriptausführung sind die R-Pakete auf allen Workerknoten verfügbar.


## <a name="using-microsoft-r-server-operationalization"></a>Verwenden der Microsoft R Server-Operationalisierung

Nach Abschluss der Datenmodellierung können Sie das Modell operationalisieren, um Vorhersagen zu treffen. Führen Sie zum Konfigurieren der Microsoft R Server-Operationalisierung die folgenden Schritte aus:

Erstellen Sie zuerst über SSH eine Verbindung mit dem Edgeknoten. Beispiel: 

    ssh -L USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Ändern Sie nach der Verwendung von SSH das Verzeichnis für die relevante Version, und rufen Sie die Dotnet-DLL per „sudo“ auf: 

- Für Microsoft R Server 9.1:

    cd /usr/lib64/microsoft-r/rserver/o16n/9.1.0   sudo dotnet Microsoft.RServer.Utils.AdminUtil/Microsoft.RServer.Utils.AdminUtil.dll

- Für Microsoft R Server 9.0:

    cd /usr/lib64/microsoft-deployr/9.0.1   sudo dotnet Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll

Gehen Sie zum Konfigurieren der Microsoft R Server-Operationalisierung mit einem einzelnen Computer folgendermaßen vor:

1. Wählen Sie „Configure R Server for Operationalization“ (R Server für Operationalisierung konfigurieren) aus.
2. Wählen Sie „A. One-box (web + compute nodes)”
3. Geben Sie ein Kennwort für den Benutzer **admin** ein

![Operationalisierung mit einem einzelnen Computer](./media/hdinsight-hadoop-r-server-get-started/admin-util-one-box-.png)

Als optionalen Schritt können Sie die Diagnoseprüfungen ausführen, indem Sie wie folgt einen Diagnosetest ausführen:

1. Wählen Sie „6. Run diagnostic tests“
2. Wählen Sie „A. Test configuration“
3. Geben Sie den Benutzernamen „admin“ und das Kennwort aus dem vorherigen Konfigurationsschritt ein.
4. Bestätigen Sie „Overall Health = pass“
5. Beenden Sie das Verwaltungshilfsprogramm
6. Beenden Sie SSH

![Diagnose für Operationalisierung](./media/hdinsight-hadoop-r-server-get-started/admin-util-diagnostics.png)

In dieser Phase ist die Konfiguration der Operationalisierung abgeschlossen. Nun können Sie das Paket „mrsdeploy“ auf Ihrem RClient zum Herstellen einer Verbindung mit der Operationalisierung auf dem Edgeknoten verwenden und mit der Verwendung seiner Features wie [Remoteausführung](https://msdn.microsoft.com/microsoft-r/operationalize/remote-execution) und [Webdienste](https://msdn.microsoft.com/microsoft-r/mrsdeploy/mrsdeploy-websrv-vignette) beginnen. Je nachdem, ob Ihr Cluster in einem virtuellen Netzwerk eingerichtet ist, müssen Sie über die SSH-Anmeldung ein Tunneling für die Portweiterleitung einrichten. Die Einrichtung dieses Tunnels wird in den folgenden Abschnitten erläutert.

### <a name="rserver-cluster-on-virtual-network"></a>RServer-Cluster in virtuellem Netzwerk

Stellen Sie sicher, dass Sie Datenverkehr über Port 12800 zum Edgeknoten zulassen. Auf diese Weise können Sie den Edgeknoten zum Herstellen der Verbindung mit dem Feature „Operationalisierung“ verwenden.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://[your-cluster-name]-ed-ssh.azurehdinsight.net:12800",
        username = "admin",
        password = "xxxxxxx"
    )


Wenn über `remoteLogin()` keine Verbindung mit dem Edgeknoten hergestellt werden kann, Sie aber über SSH eine Verbindung mit dem Edgeknoten aufbauen können, müssen Sie überprüfen, ob die Regel zum Zulassen von Datenverkehr über Port 12800 ordnungsgemäß festgelegt wurde. Wenn das Problem weiterhin besteht, können Sie es umgehen, indem Sie über SSH das Tunneling der Portweiterleitung einrichten. Anweisungen finden Sie im folgenden Abschnitt.

### <a name="rserver-cluster-not-set-up-on-virtual-network"></a>Nicht im virtuellen Netzwerk eingerichteter RServer-Cluster

Wenn Ihr Cluster nicht im VNET eingerichtet wurde oder Sie Probleme mit der Konnektivität über das VNET haben, können Sie das Tunneling der SSH-Portweiterleitung nutzen:

    ssh -L localhost:12800:localhost:12800 USERNAME@CLUSTERNAME-ed-ssh.azurehdinsight.net

Dieses kann auch in Putty eingerichtet werden.

![SSH-Verbindung in Putty](./media/hdinsight-hadoop-r-server-get-started/putty.png)

Sobald Ihre SSH-Sitzung aktiv ist, wird der Datenverkehr in der SSH-Sitzung vom Port 12800 des Computers zum Port 12800 des Edgeknotens weitergeleitet. Stellen Sie sicher, dass Sie `127.0.0.1:12800` in der `remoteLogin()`-Methode verwenden. Hierdurch erfolgt eine Anmeldung bei der Operationalisierung des Edgeknotens über Portweiterleitung.


    library(mrsdeploy)

    remoteLogin(
        deployr_endpoint = "http://127.0.0.1:12800",
        username = "admin",
        password = "xxxxxxx"
    )


## <a name="how-to-scale-microsoft-r-server-operationalization-compute-nodes-on-hdinsight-worker-nodes"></a>Skalieren von Computeknoten für die Microsoft R Server-Operationalisierung auf HDInsight-Workerknoten

### <a name="decommission-the-worker-nodes"></a>Außerbetriebsetzen der Workerknoten

Microsoft R Server wird derzeit nicht über Yarn verwaltet. Wenn die Workerknoten nicht außer Betrieb gesetzt werden, funktioniert der Yarn-Ressourcen-Manager nicht wie erwartet, da er die vom Server belegten Ressourcen nicht ermitteln kann. Um diese Situation zu vermeiden, empfehlen wir die Außerbetriebnahme der Workerknoten, bevor Sie die Serverknoten horizontal hochskalieren.

Schritte zur Außerbetriebnahme von Workerknoten:

* Melden Sie sich bei der Ambari-Konsole des HDI-Clusters an, und klicken Sie auf die Registerkarte „Hosts“.
* Wählen Sie die (außer Betrieb zu setzenden) Workerknoten aus. Klicken Sie auf „Aktionen > Ausgewählte Hosts > Hosts > Wartungsmodus aktivieren“. In der folgenden Abbildung haben wir z.B. „wn3“ und „wn4“ für die Außerbetriebnahme ausgewählt.  

   ![Außerbetriebnahme von Workerknoten](./media/hdinsight-hadoop-r-server-get-started/get-started-operationalization.png)  

* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **DataNodes** aus, und klicken Sie auf **Außer Betrieb setzen**.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **NodeManagers** aus, und klicken Sie auf **Außer Betrieb setzen**.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **DataNodes** aus, und klicken Sie auf **Beenden**.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **NodeManagers** aus, und klicken Sie auf **Beenden**.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **Hosts** aus, und klicken Sie auf **Alle Komponenten beenden**.
* Heben Sie die Auswahl der Workerknoten auf, und wählen Sie die Hauptknoten aus.
* Wählen Sie **Aktionen** > **Ausgewählte Hosts** > **Hosts** > **Alle Komponenten beenden** aus.

### <a name="configure-compute-nodes-on-each-decommissioned-worker-nodes"></a>Konfigurieren von Computeknoten auf allen außer Betrieb gesetzten Workerknoten

1. Stellen Sie per SSH eine Verbindung mit jedem außer Betrieb gesetzten Workerknoten her.
2. Führen Sie das Verwaltungshilfsprogramm mit `dotnet /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Utils.AdminUtil/Microsoft.DeployR.Utils.AdminUtil.dll` aus.
3. Geben Sie „1“ ein, um die Option „Configure R Server for Operationalization“ (R Server für Operationalisierung konfigurieren) auszuwählen.
4. Geben Sie „c“ zum Auswählen dieser Option ein: „C. Compute node“. Hiermit wird der Serverknoten für den Workerknoten konfiguriert.
5. Beenden Sie das Verwaltungshilfsprogramm.

### <a name="add-compute-nodes-details-on-web-node"></a>Hinzufügen von Details zu Computeknoten auf Webknoten

Nachdem alle außer Betrieb gesetzten Workerknoten für die Ausführung als Computeknoten konfiguriert wurden, kehren Sie zum Edgeknoten zurück und fügen die IP-Adressen der außer Betrieb gesetzten Workerknoten der Konfiguration des Webknotens von Microsoft R Server hinzu:

* Stellen Sie per SSH eine Verbindung mit dem Edgeknoten her.
* Führen Sie `vi /usr/lib64/microsoft-deployr/9.0.1/Microsoft.DeployR.Server.WebAPI/appsettings.json`aus.
* Suchen Sie den Abschnitt „URIs“, und fügen Sie die IP-Adress- und Portdetails des Workerknotens hinzu.

    ![Außerbetriebnahme von Workerknoten über die Befehlszeile](./media/hdinsight-hadoop-r-server-get-started/get-started-op-cmd.png)


## <a name="troubleshoot"></a>Problembehandlung

Falls beim Erstellen von HDInsight-Clustern Probleme auftreten, sehen Sie sich die [Voraussetzungen für die Zugriffssteuerung](hdinsight-administer-use-portal-linux.md#create-clusters) an.


## <a name="next-steps"></a>Nächste Schritte

Jetzt sollten Sie verstehen, wie Sie einen neuen HDInsight-Cluster erstellen, der R Server enthält, und Sie sollten die Grundlagen der Verwendung der R-Konsole aus einer SSH-Sitzung kennen. In den folgenden Themen werden andere Methoden zum Verwalten von und Arbeiten mit R Server unter HDInsight erläutert:

* [Hinzufügen von RStudio Server zu HDInsight (falls die Installation nicht bereits während der Clustererstellung durchgeführt wurde)](hdinsight-hadoop-r-server-install-r-studio.md)
* [Rechenkontextoptionen für R Server in HDInsight](hdinsight-hadoop-r-server-compute-contexts.md)
* [Azure Storage-Optionen für R Server in HDInsight](hdinsight-hadoop-r-server-storage.md)

