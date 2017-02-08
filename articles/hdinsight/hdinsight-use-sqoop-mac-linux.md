---
title: Verwenden von Hadoop Sqoop in Linux-basiertem HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie einen Sqoop-Import und -Export zwischen einem Linux-basierten HDInsight-Cluster und einer Azure-SQL-Datenbank durchführen können."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
author: Blackmist
tags: azure-portal
ms.assetid: 303649a5-4be5-4933-bf1d-4b232083c354
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: afc41529325d7edb4a827a2cfe45bb3d06d19fac


---
# <a name="use-sqoop-with-hadoop-in-hdinsight-ssh"></a>Verwenden von Sqoop mit Hadoop in HDInsight (SSH)
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Erfahren Sie, wie Sqoop zum Importieren und Exportieren zwischen einem Linux-basierten HDInsight-Cluster und einer Azure SQL-Datenbank oder einer SQL-Server-Datenbank verwendet werden kann.

> [!NOTE]
> In den Schritten in diesem Artikel wird mit SSH eine Verbindung zu einem Linux-basierten HDInsight-Cluster hergestellt. Windows-Clients können auch Azure PowerShell und HDInsight .NET SDK verwenden, um auf Linux-basierten Clustern mit Sqoop zu arbeiten. Öffnen Sie diese Artikel mithilfe der Registerkartenauswahl.
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, benötigen Sie Folgendes:

* Ein **Hadoop-Cluster in HDInsight** und eine **Azure SQL-Datenbank**: Die Schritte in diesem Dokument basieren auf dem Cluster und der Datenbank, die mithilfe des Dokuments [Erstellen von Cluster und SQL-Datenbank](hdinsight-use-sqoop.md#create-cluster-and-sql-database) erstellt wurden. Wenn Sie bereits über einen HDInsight-Cluster und eine SQL-Datenbank verfügen, können Sie sie für die in diesem Dokument verwendeten Werte einsetzen.
* **Arbeitsstation**: ein Computer mit einem SSH-Client.

## <a name="install-freetds"></a>Installieren von FreeTDS
1. Stellen Sie über SSH eine Verbindung mit dem Linux-basierten HDInsight-Cluster her. Verwenden Sie für die Verbindung die Adresse `CLUSTERNAME-ssh.azurehdinsight.net` und den Port `22`.
   
    Weitere Informationen zur Verwendung von SSH zum Verbindungsaufbau mit HDInsight finden Sie in den folgenden Dokumenten:
   
   * **Linux-, Unix- und OS X-Clients**: [Verbinden mit einem Linux-basierten HDInsight-Cluster unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-linux-based-hdinsight-cluster)
   * **Windows-Clients**: [Verbinden mit einem Linux-basierten HDInsight-Cluster unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md#connect-to-a-linux-based-hdinsight-cluster)
2. Geben Sie den folgenden Befehl für die Installation von FreeTDS ein:
   
        sudo apt-get --assume-yes install freetds-dev freetds-bin
   
    FreeTDS wird in mehreren Schritten verwendet, um eine Verbindung zur SQL-Datenbank herzustellen.

## <a name="create-the-table-in-sql-database"></a>Erstellen der Tabelle in einer SQL-Datenbank
> [!IMPORTANT]
> Wenn Sie einen HDInsight-Cluster und eine SQL-Datenbank verwenden, die Sie anhand der Schritte unter [Erstellen von Cluster und SQL-Datenbank](hdinsight-use-sqoop.md)erstellt haben, ignorieren Sie die Schritte in diesem Abschnitt, da die Datenbank und die Tabelle als Teil der Schritte im anderen Dokument bereits erstellt wurden.
> 
> 

1. Verwenden Sie über die SSH-Verbindung zu HDInsight den folgenden Befehl, um eine Verbindung zu dem SQL-Datenbankserver herzustellen und die Tabelle zu erstellen, die in den weiteren Schritten benötigt wird.
   
        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest
   
    Eine Ausgabe ähnlich der folgenden wird angezeigt:
   
        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to sqooptest
        1>
2. Geben Sie bei der Eingabeaufforderung `1>` folgende Zeilen ein:
   
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
        GO
        CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)
        GO
   
    Nach Eingabe der Anweisung `GO` werden die vorherigen Anweisungen ausgewertet. Zunächst wird die Tabelle **mobiledata** erstellt und dieser dann ein gruppierter Index hinzugefügt (für SQL-Datenbank erforderlich).
   
    Stellen Sie wie folgt sicher, dass die Tabelle erstellt wurde:
   
        SELECT * FROM information_schema.tables
        GO
   
    Eine Ausgabe ähnlich der folgenden sollte angezeigt werden:
   
        TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
        sqooptest       dbo     mobiledata      BASE TABLE
3. Geben Sie zum Beenden des Dienstprogramms tsql an der Eingabeaufforderung `exit` at the `1>` ein.

## <a name="sqoop-export"></a>Sqoop-Export
1. Verwenden Sie den folgenden Befehl über die SSH-Verbindung zu HDInsight, um sicherzustellen, dass Sqoop Ihre SQL-Datenbank sehen kann:
   
        sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
   
    Es sollte eine Liste mit Datenbanken zurückgeliefert werden, in der auch die zuvor erstellte Datenbank **sqooptest** enthalten ist.
2. Verwenden Sie den folgenden Befehl, um Daten aus **hivesampletable** in die Tabelle **mobiledata** zu exportieren:
   
        sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --export-dir 'wasbs:///hive/warehouse/hivesampletable' --fields-terminated-by '\t' -m 1
   
    Dadurch wird Sqoop angewiesen, eine Verbindung mit SQL-Datenbank bzw. der Datenbank **sqooptest** herzustellen und Daten aus **wasbs:///hive/warehouse/hivesampletable** (physische Dateien von *hivesampletable*) in die Tabelle **mobiledata** zu exportieren.
3. Nach Abschluss des Befehls stellen Sie wie folgt über TSQL eine Verbindung mit der Datenbank her:
   
        TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D sqooptest
   
    Nachdem die Verbindung hergestellt wurde, überprüfen Sie mithilfe der folgenden Anweisungen, ob die Daten in die Tabelle **mobiledata** exportiert wurden:
   
        SELECT * FROM mobiledata
        GO
   
    Es sollte eine Liste der Tabellendaten angezeigt werden. Geben Sie `exit` ein, um das Dienstprogramm tsql zu beenden.

## <a name="sqoop-import"></a>Sqoop-Import
1. Importieren Sie wie folgt Daten aus der Tabelle **mobiledata** in der SQL-Datenbank in das Verzeichnis **wasbs:///tutorials/usesqoop/importeddata** in HDInsight:
   
        sqoop import --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1
   
    Die importierten Daten enthalten Felder, die durch ein Tabstoppzeichen getrennt sind, und die Zeilen werden durch ein Zeilenumbruchzeichen abgeschlossen.
2. Sobald der Import abgeschlossen ist, verwenden Sie den folgenden Befehl zum Auflisten der Daten in dem neuen Verzeichnis:
   
        hadoop fs -text wasbs:///tutorials/usesqoop/importeddata/part-m-00000

## <a name="using-sql-server"></a>Verwenden von SQL Server
Sie können auch Sqoop zum Importieren und Exportieren von Daten aus SQL-Server verwenden, entweder in Ihrem Rechenzentrum oder auf einem in Azure gehosteten virtuellen Computer. Die Unterschiede zwischen der Verwendung von SQL-Datenbank und SQL Server sind:

* Sowohl HDInsight als auch SQL Server müssen sich im selben virtuellen Azure-Netzwerk befinden.
  
  > [!NOTE]
  > HDInsight unterstützt nur standortbasierte virtuelle Netzwerke und kann momentan nicht mit affinitätsgruppenbasierten virtuellen Netzwerken verwendet werden.
  > 
  > 
  
    Wenn Sie SQL Server in Ihrem Datencenter verwenden, müssen Sie das virtuelle Netzwerk entweder als *Standort-zu-Standort* oder als *Punkt-zu-Standort* konfigurieren.
  
  > [!NOTE]
  > Für virtuelle Netzwerke im **Punkt-zu-Standort**-Modus muss die Anwendung zum Konfigurieren des VPN-Clients auf dem SQL Server ausgeführt werden. Diese Anwendung ist im **Dashboard** der Konfiguration Ihres virtuellen Azure-Netzwerks verfügbar.
  > 
  > 
  
    Weitere Informationen zu Azure Virtual Network erhalten Sie in der [Übersicht über virtuelle Netzwerke](../virtual-network/virtual-networks-overview.md).
* SQL Server muss so konfiguriert sein, dass SQL-Authentifizierung erlaubt ist. Weitere Informationen finden Sie unter [Auswählen eines Authentifizierungsmodus](https://msdn.microsoft.com/ms144284.aspx)
* Sie müssen SQL Server möglicherweise für Remoteverbindungen konfigurieren. Weitere Informationen finden Sie unter [So beheben Sie Fehler bei der Verbindung zum SQL Server-Datenbankmodul](http://social.technet.microsoft.com/wiki/contents/articles/2102.how-to-troubleshoot-connecting-to-the-sql-server-database-engine.aspx) .
* Die Datenbank **sqooptest** wird in SQL Server mit einem Dienstprogramm wie etwa **SQL Server Management Studio** oder **tsql** erstellt – die Schritte zur Verwendung der Azure-Befehlszeilenschnittstelle gelten nur für Azure SQL-Datenbank.
  
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
* Bei der Verbindung mit dem SQL-Server aus HDInsight müssen Sie möglicherweise die IP-Adresse des SQL-Servers verwenden, sofern kein DNS-System zur Namensauflösung in dem virtuellen Azure-Netzwerk konfiguriert wurde. Beispiel:
  
        sqoop import --connect 'jdbc:sqlserver://10.0.1.1:1433;database=sqooptest' --username <adminLogin> --password <adminPassword> --table 'mobiledata' --target-dir 'wasbs:///tutorials/usesqoop/importeddata' --fields-terminated-by '\t' --lines-terminated-by '\n' -m 1

## <a name="limitations"></a>Einschränkungen
* Massenexport: Bei Linux-basiertem HDInsight unterstützt der zum Exportieren von Daten nach Microsoft SQL Server oder Azure SQL-Datenbank verwendete Sqoop-Connector derzeit keine Masseneinfügungen.
* Batchverarbeitung: Wenn Sie in Linux-basiertem HDInsight zum Ausführen von Einfügevorgängen den Schalter `-batch` verwenden, führt Sqoop mehrere Einfügevorgänge aus, anstatt diese zu einem Batch zusammenzufassen.

## <a name="next-steps"></a>Nächste Schritte
Nun wissen Sie, wie Sqoop verwendet haben. Weitere Informationen finden Sie unter:

* [Verwenden von Oozie mit HDInsight][hdinsight-use-oozie]: Verwenden der Sqoop-Aktion in einem Oozie-Workflow.
* [Analysieren von Daten zu Flugverspätungen mit HDInsight][hdinsight-analyze-flight-data]: Verwenden von Hive zur Analyse von Daten zu Flugverspätungen und Verwenden von Sqoop zum Exportieren von Daten in die SQL-Datenbank.
* [Hochladen von Daten in HDInsight][hdinsight-upload-data]: Andere Methoden zum Hochladen von Daten in HDInsight/Azure-Blobspeicher.

[hdinsight-versions]:  hdinsight-component-versioning.md
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



<!--HONumber=Nov16_HO3-->


