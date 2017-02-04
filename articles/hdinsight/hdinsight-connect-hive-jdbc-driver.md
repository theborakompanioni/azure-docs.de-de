---
title: "Verwenden von JDBC zum Abfragen von Hive für Azure HDInsight"
description: "Sie erfahren, wie Sie JDBC zum Herstellen einer Verbindung mit Hive für Azure HDInsight verwenden und Abfragen für in der Cloud gespeicherte Daten per Remoteverbindung ausführen."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 928f8d2a-684d-48cb-894c-11c59a5599ae
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/08/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 8c07f0da21eab0c90ad9608dfaeb29dd4a01a6b7
ms.openlocfilehash: 1fbf8cd4347c5a49fe7769fd8d02c2dbfddfc74e


---
# <a name="connect-to-hive-on-azure-hdinsight-using-the-hive-jdbc-driver"></a>Herstellen einer Verbindung mit Hive unter Azure HDInsight per Hive-JDBC-Treiber

[!INCLUDE [ODBC-JDBC-selector](../../includes/hdinsight-selector-odbc-jdbc.md)]

In diesem Dokument erfahren Sie, wie Sie JDBC über eine Java-Anwendung nutzen, um Hive-Abfragen per Remoteverbindung an einen HDInsight-Cluster zu senden. Es wird erläutert, wie Sie eine Verbindung von SQuirreL SQL Client aus herstellen und wie Sie eine programmgesteuerte Verbindung von Java aus herstellen.

Weitere Informationen zur Hive-JDBC-Schnittstelle finden Sie unter [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie die in diesem Artikel aufgeführten Schritte ausführen können, benötigen Sie Folgendes:

* Hadoop für einen HDInsight-Cluster. Linux-basierte oder Windows-basierte Cluster funktionieren.

  > [!IMPORTANT]
  > Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL ist eine JDBC-Clientanwendung.

Zum Erstellen und Ausführen der Beispiel-Java-Anwendung, die Sie über einen Link in diesem Artikel aufrufen können, benötigen Sie Folgendes:

* [Java Developer Kit (JDK) Version 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) oder höher.
* [Apache Maven](https://maven.apache.org). Maven ist ein Projekterstellungssystem für Java-Projekte, das für das diesem Artikel zugeordnete Projekt verwendet wird.

## <a name="connection-string"></a>Verbindungszeichenfolge

JDBC-Verbindungen mit einem HDInsight-Cluster unter Azure werden über 443 hergestellt, und der Datenverkehr wird per SSL geschützt. Das öffentliche Gateway, hinter dem sich die Cluster befinden, leitet den Datenverkehr an den Port um, auf dem von HiveServer2 gelauscht wird. Eine typische Verbindungszeichenfolge sieht wie folgt aus:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=/hive2

Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters.

## <a name="authentication"></a>Authentifizierung

Beim Herstellen der Verbindung müssen Sie den Administratornamen und das Kennwort für den HDInsight-Cluster verwenden, um sich beim Clustergateway zu authentifizieren. Beim Herstellen der Verbindung in JDBC-Clients wie SQuirreL SQL müssen Sie den Administratornamen und das Kennwort in den Clienteinstellungen eingeben.

In einer Java-Anwendung müssen Sie den Namen und das Kennwort beim Herstellen einer Verbindung eingeben. Beispielsweise wird mit dem folgenden Java-Code eine neue Verbindung geöffnet, indem die Verbindungszeichenfolge, der Administratorname und das Kennwort verwendet werden:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>Herstellen einer Verbindung mit dem SQuirreL SQL-Client

SQuirreL SQL ist ein JDBC-Client, der für die Remoteausführung von Hive-Abfragen mit Ihrem HDInsight-Cluster verwendet werden kann. Die folgenden Schritte führen Sie durch das Herunterladen und Konfigurieren der Treiber für Hive. Dabei wird davon ausgegangen, dass Sie SQuirreL SQL bereits installiert haben.

1. Kopieren Sie die Hive-JDBC-Treiber aus Ihrem HDInsight-Cluster.
   
    * Bei **Linux-basiertem HDInsight** gehen Sie folgendermaßen vor, um die erforderlichen JAR-Dateien herunterzuladen.
     
        1. Erstellen Sie ein neues Verzeichnis, das die Dateien enthalten soll. Beispiel: `mkdir hivedriver`.
        2. Ändern Sie die Verzeichnisse an einer Eingabeaufforderung, Bash, PowerShell oder einer anderen Befehlszeile in das neue Verzeichnis, und verwenden Sie die folgenden Befehle, um die Dateien aus dem HDInsight-Cluster zu kopieren.
        
                scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-jdbc*standalone.jar .
                scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
                scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
        
            Ersetzen Sie **USERNAME** durch den Namen des SSH-Benutzerkontos für den Cluster. Ersetzen Sie **CLUSTERNAME** durch den Namen des HDInsight-Clusters.
        
        > [!NOTE]
        > In Windows-Umgebungen müssen Sie anstelle von scp das Dienstprogramm PSCP verwenden. Sie können es unter [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)herunterladen.

    * Bei **Windows-basiertem HDInsight** gehen Sie folgendermaßen vor, um die JAR-Dateien herunterzuladen.
     
        1. Wählen Sie im Azure-Portal den HDInsight-Cluster aus, und wählen Sie dann das Symbol **Remotedesktop** aus.
        
            ![Symbol „Remotedesktop“](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. Stellen Sie auf dem Blatt „Remotedesktop“ mithilfe der Schaltfläche **Verbinden** eine Verbindung mit dem Cluster her. Wenn Remotedesktop nicht aktiviert ist, geben Sie einen Benutzernamen und ein Kennwort in das Formular ein, und wählen Sie dann **Aktivieren** aus, um Remotedesktop für den Cluster zu aktivieren.
        
            ![Blatt „Remotedesktop“](./media/hdinsight-connect-hive-jdbc-driver/remotedesktopblade.png)
        
            Nachdem Sie **Verbinden** ausgewählt haben, wird eine RDP-Datei heruntergeladen. Verwenden Sie diese Datei, um den Remotedesktopclient zu starten. Wenn Sie dazu aufgefordert werden, verwenden Sie den Benutzernamen und das Kennwort, die Sie für den Remotedesktopzugriff eingegeben haben.

        3. Nachdem die Verbindung hergestellt wurde, kopieren Sie die folgenden Dateien aus der Remotedesktopsitzung auf Ihren lokalen Computer. Speichern Sie sie in einem lokalen Verzeichnis mit dem Namen `hivedriver`.
        
            * C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-jdbc-0.14.0.2.2.9.1-7-standalone.jar
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-common-2.6.0.2.2.9.1-7.jar
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.jar
          
            > [!NOTE]
            > Die in den Pfaden und Dateinamen enthaltenen Versionsnummern können für Ihren Cluster anders lauten.

        4. Trennen Sie die Remotedesktopsitzung, sobald Sie mit dem Kopieren der Dateien fertig sind.

2. Starten Sie die SQuirreL SQL-Anwendung. Wählen Sie auf der linken Seite des Fensters **Drivers** aus.
   
    ![Registerkarte „Drivers“ auf der linken Seite des Fensters](./media/hdinsight-connect-hive-jdbc-driver/squirreldrivers.png)

3. Wählen Sie aus den Symbolen oben im Dialogfeld **Drivers** das Symbol **+** aus, um einen neuen Treiber zu erstellen.
   
    ![Treibersymbole](./media/hdinsight-connect-hive-jdbc-driver/driversicons.png)

4. Fügen Sie im Dialogfeld „Add Driver“ die folgenden Informationen hinzu:
   
    * **Name**: Hive
    * **Beispiel-URL**: `jdbc:hive2://localhost:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=/hive2`
    * **Extra Class Path**: Fügen Sie mit der Schaltfläche „Add“ die JAR-Dateien hinzu, die Sie zuvor heruntergeladen haben.
    * **Class Name**: org.apache.hive.jdbc.HiveDriver
    
   ![Dialogfeld „Add Driver“](./media/hdinsight-connect-hive-jdbc-driver/adddriver.png)
    
   Klicken Sie auf **OK**, um die Einstellungen zu speichern.

5. Wählen Sie auf der linken Seite des Fensters „SQuirreL SQL“ die Option **Aliases** aus. Klicken Sie dann auf das Symbol **+**, um einen neuen Verbindungsalias zu erstellen.
   
    ![Hinzufügen eines neuen Alias](./media/hdinsight-connect-hive-jdbc-driver/aliases.png)

6. Verwenden Sie für das Dialogfeld **Add Alias** die folgenden Werte.
   
    * **Name**: Hive on HDInsight

    * **Driver**: Wählen Sie in der Dropdownliste den Treiber **Hive** aus.

    * **URL**: jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;ssl=true?hive.server2.transport.mode=http;hive.server2.thrift.http.path=/hive2
     
        Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres HDInsight-Clusters.

    * **User Name**: Der Name des Clusteranmeldekontos für den HDInsight-Cluster. Der Standardwert lautet `admin`.

    * **Password**: Das Kennwort für das Clusteranmeldekonto. Dies ist ein Kennwort, das Sie beim Erstellen des HDInsight-Clusters angegeben haben.
     
    ![Dialogfeld „Add Alias“](./media/hdinsight-connect-hive-jdbc-driver/addalias.png)
     
    Überprüfen Sie mithilfe der Schaltfläche **Test**, ob die Verbindung funktioniert. Wenn das Dialogfeld **Connect to: Hive on HDInsight** angezeigt wird, wählen Sie **Connect** aus, um den Test durchzuführen. Wenn der Test erfolgreich ist, wird das Dialogfeld **Connection successful** angezeigt.
     
    Verwenden Sie die Schaltfläche **OK** unten im Dialogfeld **Add Alias**, um den Verbindungsalias zu speichern.

7. Wählen Sie oben in SQuirreL SQL in der Dropdownliste **Connect to** die Option **Hive on HDInsight** aus. Wenn Sie dazu aufgefordert werden, wählen Sie **Connect** aus.
   
    ![Verbindungsdialogfeld](./media/hdinsight-connect-hive-jdbc-driver/connect.png)

8. Nachdem die Verbindung hergestellt wurde, geben Sie im Dialogfeld für die SQL-Abfrage die folgende Abfrage ein, und wählen Sie das Symbol **Run** aus. Die Ergebnisse der Abfrage sollten im Ergebnisbereich angezeigt werden.
   
        select * from hivesampletable limit 10;
   
    ![Dialogfeld für die SQL-Abfrage einschließlich der Ergebnisse](./media/hdinsight-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Herstellen einer Verbindung von einer Beispiel-Java-Anwendung aus

Ein Beispiel für die Nutzung eines Java-Clients zum Abfragen von Hive in HDInsight ist unter [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc)verfügbar. Befolgen Sie die Anleitung im Repository, um das Beispiel zu erstellen und auszuführen.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a>Unerwarteter Fehler beim Versuch, eine SQL-Verbindung zu öffnen

**Symptome**: Beim Herstellen einer Verbindung mit einem HDInsight-Cluster der Version 3.3 oder 3.4 erhalten Sie möglicherweise die Meldung, dass ein unerwarteter Fehler aufgetreten ist. Die Stapelüberwachung für diesen Fehler beginnt mit folgenden Zeilen:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Ursache**: Dieser Fehler wird durch einen Konflikt zwischen der Version der von SQuirreL verwendeten Datei „commons-codec.jar“ und der Version verursacht, die die aus dem HDInsight-Cluster heruntergeladenen Hive JDBC-Komponenten benötigen.

**Lösung**: Führen Sie folgende Schritte aus, um diesen Fehler zu beheben.

1. Laden Sie die commons-codec.jar-Datei aus Ihrem HDInsight-Cluster herunter.
   
        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. Beenden Sie SQuirreL, und wechseln Sie zu dem Verzeichnis, in dem SQuirreL in Ihrem System installiert ist. Ersetzen Sie im SquirreL-Verzeichnis, unterhalb des `lib` -Verzeichnisses, die vorhandene commons-codec.jar-Datei durch die Datei, die Sie aus dem HDInsight-Cluster heruntergeladen haben.

3. Starten Sie SQuirreL neu. Dieser Fehler sollte jetzt nicht mehr auftreten, wenn Sie eine Verbindung mit Hive in HDInsight herstellen.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erfahren haben, wie Sie JDBC mit Hive verwenden, können Sie mithilfe der nachfolgenden Links andere Möglichkeiten für die Arbeit mit Azure HDInsight untersuchen.

* [Hochladen von Daten in HDInsight](hdinsight-upload-data.md)
* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce-Aufträgen mit HDInsight](hdinsight-use-mapreduce.md)




<!--HONumber=Jan17_HO3-->


