<properties
	pageTitle="Verwenden von Hadoop Sqoop in Linux-basiertem HDInsight | Microsoft Azure"
	description="Erfahren Sie, wie Sie einen Sqoop-Import und -Export zwischen einem Linux-basierten HDInsight-Cluster und einer Azure-SQL-Datenbank durchführen können."
	editor="cgronlun"
	manager="paulettm"
	services="hdinsight"
	documentationCenter=""
	authors="Blackmist"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/09/2016"
	ms.author="larryfr"/>

#Verwenden von Sqoop mit Hadoop in HDInsight (SSH)

[AZURE.INCLUDE [Sqoop-Auswahl](../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie Sqoop zum Importieren und Exportieren zwischen einem Linux-basierten HDInsight-Cluster und einer Azure SQL-Datenbank oder einer SQL-Server-Datenbank verwendet werden kann.

> [AZURE.NOTE] In den Schritten in diesem Artikel wird mit SSH eine Verbindung zu einem Linux-basierten HDInsight-Cluster hergestellt. Windows-Clients können auch Azure PowerShell und HDInsight .NET SDK verwenden, um auf Linux-basierten Clustern mit Sqoop zu arbeiten. Öffnen Sie diese Artikel mithilfe der Registerkartenauswahl.

##Voraussetzungen

Bevor Sie mit diesem Lernprogramm beginnen können, benötigen Sie Folgendes:


- **Hadoop-Cluster in HDInsight**. Informationen finden Sie unter [Erstellen von Cluster und SQL-Datenbank](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Arbeitsstation**: ein Computer mit einem SSH-Client.
- **Azure CLI**: Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../xplat-cli-install.md)

##Sqoop-Export

2. Verwenden Sie den folgenden Befehl, um einen Link zum SQL Server JDBC-Treiber aus dem Sqoop-Verzeichnis "lib" zu erstellen. Dies ermöglicht Sqoop die Nutzung dieses Treibers zur Kommunikation mit der SQL-Datenbank.

        sudo ln /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /usr/hdp/current/sqoop-client/lib/sqljdbc41.jar

3. Verwenden Sie den folgenden Befehl, um zu überprüfen, ob Sqoop Ihre SQL-Datenbank erreichen kann:

        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>

    Es sollte eine Liste mit Datenbanken zurückgeliefert werden, in der auch die zuvor erstellte Datenbank **sqooptest** enthalten ist.

4. Verwenden Sie den folgenden Befehl, um Daten von **hivesampletable** in die Tabelle **mobiledata** zu exportieren:

        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasb:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1

    Dies weist Sqoop an, sich mit SQL-Datenbank bzw. der Datenbank **sqooptest** zu verbinden und Daten aus ****wasb:///hive/warehouse/hivesampletable** (physische Dateien der *hivesampletable*) in die Tabelle **mobiledata** zu exportieren.

5. Nach Abschluss des Befehls stellen Sie wie folgt über TSQL eine Verbindung mit der Datenbank her:

        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest

    Nachdem die Verbindung hergestellt wurde, überprüfen Sie mithilfe der folgenden Anweisungen, ob die Daten in die Tabelle **mobiledata** exportiert wurden:

        SELECT * FROM mobiledata
        GO

    Es sollte eine Liste der Tabellendaten angezeigt werden. Geben Sie `exit` ein, um das Dienstprogramm tsql zu beenden.

##Sqoop-Import

1. Importieren Sie wie folgt Daten aus der Tabelle **mobiledata** in der SQL-Datenbank in das Verzeichnis ****wasb:///tutorials/usesqoop/importeddata** auf HDInsight:

        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

    Die importierten Daten enthalten Felder, die durch ein Tabstoppzeichen getrennt sind, und die Zeilen werden durch ein Zeilenumbruchzeichen abgeschlossen.

2. Sobald der Import abgeschlossen ist, verwenden Sie den folgenden Befehl zum Auflisten der Daten in dem neuen Verzeichnis:

        hadoop fs -text wasb:///tutorials/usesqoop/importeddata/part-m-00000

##Verwenden von SQL Server

Sie können auch Sqoop zum Importieren und Exportieren von Daten aus SQL-Server verwenden, entweder in Ihrem Rechenzentrum oder auf einem in Azure gehosteten virtuellen Computer. Die Unterschiede zwischen der Verwendung von SQL-Datenbank und SQL Server sind:

* Sowohl HDInsight als auch SQL Server müssen sich im selben virtuellen Azure-Netzwerk befinden.

    > [AZURE.NOTE] HDInsight unterstützt nur standortbasierte virtuelle Netzwerke und kann momentan nicht mit affinitätsgruppenbasierten virtuellen Netzwerken verwendet werden.

    Wenn Sie SQL Server in Ihrem Rechenzentrum verwenden, müssen Sie das virtuelle Netzwerk entweder als *Standort-zu-Standort* oder als *Punkt-zu-Standort* konfigurieren.

    > [AZURE.NOTE] Für virtuelle Netzwerke im **Punkt-zu-Standort-Modus** muss die Anwendung zum Konfigurieren des VPN-Clients auf dem SQL Server ausgeführt werden. Diese Anwendung ist im **Dashboard** der Konfiguration Ihres virtuellen Azure-Netzwerks verfügbar.

    Informationen zur Erstellung und Konfiguration von virtuellen Netzwerken finden Sie unter [Aufgaben bei der Konfiguration virtueller Netzwerke](../services/virtual-machines/).

* SQL Server muss so konfiguriert sein, dass SQL-Authentifizierung erlaubt ist. Weitere Informationen finden Sie unter [Auswählen eines Authentifizierungsmodus](https://msdn.microsoft.com/ms144284.aspx).

* Sie müssen SQL Server möglicherweise für Remoteverbindungen konfigurieren. Weitere Informationen finden Sie unter [So beheben Sie Fehler bei der Verbindung zum SQL Server-Datenbankmodul](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx).

* Die Datenbank **sqooptest** wird in SQL Server mit einem Dienstprogramm wie z. B. **SQL Server Management Studio** oder **tsql** erstellt – die Schritte zur Verwendung der Azure-Befehlszeilenschnittstelle gelten nur für Azure SQL-Datenbank.

    Die tsql-Anweisungen zum Erstellen der Tabelle **mobiledata** ähneln denen für SQL-Datenbank, mit Ausnahme der Erstellung eines gruppierten Index – Dies ist für SQL Server nicht erforderlich:

        CREATE TABLE [dbo].[mobiledata](
        [clientid] [nvarchar](50),
        [querytime] [nvarchar](50),
        [market] [nvarchar](50),
        [deviceplatform] [nvarchar](50),
        [devicemake] [nvarchar](50),
        [devicemodel] [nvarchar](50),
        [state] [nvarchar](50),
        [country] [nvarchar](50),
        [querydwelltime] [float],
        [sessionid] [bigint],
        [sessionpagevieworder] [bigint])

* Bei der Verbindung mit dem SQL-Server aus HDInsight müssen Sie möglicherweise die IP-Adresse des SQL-Servers verwenden, sofern kein DNS-System zur Namensauflösung in dem virtuellen Azure-Netzwerk konfiguriert wurde. Zum Beispiel:

        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasb:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

##Nächste Schritte

Nun wissen Sie, wie Sqoop verwendet haben. Weitere Informationen finden Sie unter:

- [Verwenden von Oozie mit HDInsight][hdinsight-use-oozie]\: Verwenden der Sqoop-Aktion in einem Oozie-Workflow.
- [Analysieren von Daten zu Flugverspätungen mit HDInsight][hdinsight-analyze-flight-data]\: Verwenden von Hive zur Analyse von Daten zu Flugverspätungen und Verwenden von Sqoop zum Exportieren von Daten in die SQL-Datenbank.
- [Hochladen von Daten in HDInsight][hdinsight-upload-data]\: Andere Methoden zum Hochladen von Daten in HDInsight/Azure Blob-Speicher.



[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database-create-configure.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html

<!---HONumber=AcomDC_0413_2016-->