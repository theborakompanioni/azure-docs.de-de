<properties
	pageTitle="Verwalten von Hadoop-Clustern in HDInsight mit Azure PowerShell | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie administrative Aufgaben für Hadoop-Cluster in HDInsight mit Azure PowerShell ausführen."
	services="hdinsight"
	editor="cgronlun"
	manager="paulettm"
	tags="azure-portal"
	authors="mumian"
	documentationCenter=""/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/10/2016"
	ms.author="jgao"/>

# Verwalten von Hadoop-Clustern in HDInsight mit Azure PowerShell

[AZURE.INCLUDE [Auswahl](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell ist eine leistungsstarke Skriptumgebung, mit der Sie die Bereitstellung und Verwaltung Ihrer Arbeitsauslastungen in Azure steuern und automatisieren können. In diesem Artikel erfahren Sie, wie Sie Hadoop-Cluster in Azure HDInsight mit einer lokalen Azure PowerShell-Konsole über Windows PowerShell verwalten. Eine Liste der HDInsight PowerShell-Cmdlets finden Sie unter [HDInsight-Cmdlet-Referenz][hdinsight-powershell-reference].



**Voraussetzungen**

Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

##Installieren von Azure PowerShell

[AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Wenn Sie Azure PowerShell Version 0.9x installiert haben, müssen Sie sie deaktivieren, bevor Sie eine neue Version installieren können.

So überprüfen Sie die Version der installierten PowerShell:

	Get-Module *azure*
	
Um die ältere Version zu deinstallieren, rufen Sie „Programme und Features“ in der Systemsteuerung auf.


##Erstellen von Clustern

Weitere Informationen finden Sie unter [Erstellen von Linux-basierten Clustern in HDInsight mit Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md).

##Auflisten von Clustern
Verwenden Sie den folgenden Befehl, um alle Cluster des aktuellen Abonnements aufzulisten:

	Get-AzureRmHDInsightCluster

##Anzeigen von Clustern

Verwenden Sie den folgenden Befehl, um Details zu einem bestimmten Cluster des aktuellen Abonnements anzuzeigen:

	Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

##Löschen von Clustern

Mit dem folgenden Befehl können Sie ein Cluster löschen:

	Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

Sie können einen Cluster auch löschen, indem Sie die Ressourcengruppe entfernen, die den Cluster enthält. Beachten Sie, dass damit alle Ressourcen in der Gruppe, einschließlich des Standardspeicherkontos, gelöscht werden.

	Remove-AzureRmResourceGroup -Name <Resource Group Name>
			
##Skalieren von Clustern
Mithilfe der Clusterskalierung können Sie die Anzahl der von einem in Azure HDInsight ausgeführten Cluster verwendeten Workerknoten ändern, ohne den Cluster neu erstellen zu müssen.

>[AZURE.NOTE] Es werden nur Cluster mit HDInsight-Versionen ab 3.1.3 unterstützt. Überprüfen Sie ggf. auf der Seite „Eigenschaften“ die Version Ihres Clusters. Informationen hierzu finden Sie unter [Auflisten und Anzeigen von Clustern](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).

Auswirkungen der Änderung der Anzahl von Datenknoten für die von HDInsight unterstützten Clustertypen:

- Hadoop

	Sie können die Anzahl der Workerknoten in einem aktiven Hadoop-Cluster problemlos ohne Auswirkungen auf ausstehende oder aktive Aufträge erhöhen. Neue Aufträge können auch während des Vorgangs gesendet werden. Fehler bei einer Skalierung werden ordnungsgemäß behandelt, sodass der Cluster immer in einem funktionsfähigen Zustand verbleibt.

	Wenn ein Hadoop-Cluster durch Verringern der Anzahl der Datenknoten zentral herunterskaliert wird, werden einige der Dienste im Cluster neu gestartet. Dies führt beim Abschluss des Skalierungsvorgangs bei allen aktiven und ausstehenden Aufträgen zu einem Fehler. Sie können die Aufträge jedoch nach Abschluss des Vorgangs erneut senden.

- HBase

	Sie können Knoten reibungslos Ihrem HBase-Cluster hinzufügen oder aus diesem entfernen, während er aktiv ist. Regionale Server werden innerhalb weniger Minuten nach Abschluss des Skalierungsvorgangs automatisch ausgeglichen. Allerdings können Sie die regionalen Server auch manuell ausgleichen, indem Sie sich am Hauptknoten des Clusters anmelden und in einem Eingabeaufforderungsfenster die folgenden Befehle ausführen:

		>pushd %HBASE_HOME%\bin
		>hbase shell
		>balancer

- Storm

	Sie können Datenknoten übergangslos zum Storm-Cluster hinzufügen oder aus diesem entfernen, während er aktiv ist. Nach erfolgreichen Abschluss des Skalierungsvorgangs müssen Sie die Topologie neu ausgleichen.

	Es stehen zwei Methoden für den erneuten Ausgleich zur Verfügung:

	* Storm-Webbenutzeroberfläche
	* Befehlszeilenschnittstelle (CLI)

	Weitere Informationen finden Sie in der Dokumentation zu [Apache Storm](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).

	Die Storm-Webbenutzeroberfläche ist für den HDInsight-Cluster verfügbar:

	![HDInsight Storm-Skalierung ausgleichen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

	Es folgt ein Beispiel, wie die Storm-Topologie mithilfe des CLI-Befehls neu ausgeglichen werden kann:

		## Reconfigure the topology "mytopology" to use 5 worker processes,
		## the spout "blue-spout" to use 3 executors, and
		## the bolt "yellow-bolt" to use 10 executors

		$ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Führen Sie den folgenden Befehl auf einem Clientcomputer aus, um die Hadoop-Clustergröße mithilfe von Azure PowerShell zu ändern:

	Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
	

##Gewähren/Entziehen des Zugriffs

In HDInsight-Clustern stehen die folgenden HTTP-Webdienste zur Verfügung (alle mit RESTful-Endpunkten):

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton


Der Zugriff auf diese Dienste wird standardmäßig gewährt. Sie können den Zugriff widerrufen/gewähren. Zum Widerrufen:

	Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

Zum Gewähren:

	$clusterName = "<HDInsight Cluster Name>"

	# Credential option 1
	$hadoopUserName = "admin"
	$hadoopUserPassword = "<Enter the Password>"
	$hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
	$credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

	# Credential option 2
	#$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"
	
	Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

>[AZURE.NOTE] Durch Gewähren/Widerrufen des Zugriffs werden der Benutzername und das Kennwort des Clusterbenutzers zurückgesetzt.

Dies kann auch über das Portal durchgeführt werden. Siehe [Verwalten von HDInsight mit dem Azure-Portal][hdinsight-admin-portal]

##Aktualisieren von HTTP-Anmeldeinformationen

Das Verfahren ist mit dem [Gewähren/Widerrufen von HTTP-Zugriff](#grant/revoke-access) identisch. Wenn dem Cluster der HTTP-Zugriff gewährt wurde, müssen Sie dies zunächst widerrufen. Gewähren Sie anschließend den Zugriff mit den neuen HTTP-Anmeldeinformationen.


##Suchen des Standardspeicherkontos

Das folgende PowerShell-Skript veranschaulicht, wie der Name und Schlüssel des Standardspeicherkontos für einen Cluster abgerufen werden.

	$clusterName = "<HDInsight Cluster Name>"
	
	$cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
	$resourceGroupName = $cluster.ResourceGroup
	$defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
	$defaultBlobContainerName = $cluster.DefaultStorageContainer
	$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
	$defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey 

##Suchen der Ressourcengruppe

Im Resource Manager-Modus gehört jeder HDInsight-Cluster einer Azure-Ressourcengruppe an. So finden Sie die Ressourcengruppe:

	$clusterName = "<HDInsight Cluster Name>"
	
	$cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
	$resourceGroupName = $cluster.ResourceGroup


##Übermitteln von Aufträgen

**So übermitteln Sie MapReduce-Aufträge**

Weitere Informationen finden Sie unter [Ausführen von Hadoop MapReduce-Beispielen in Windows-basiertem HDInsight](hdinsight-run-samples.md).

**So übermitteln Sie Hive-Aufträge**

Weitere Informationen finden Sie unter [Ausführen von Hive-Abfragen mit PowerShell](hdinsight-hadoop-use-hive-powershell.md).

**So übermitteln Sie Pig-Aufträge**

Weitere Informationen finden Sie unter [Ausführen von Pig-Aufträgen mit PowerShell](hdinsight-hadoop-use-pig-powershell.md).

**So übermitteln Sie Sqoop-Aufträge**

Weitere Informationen finden Sie unter [Verwenden von Sqoop mit HDInsight](hdinsight-use-sqoop.md).

**So übermitteln Sie Oozie-Aufträge**

Weitere Informationen finden Sie unter [Verwenden von Oozie mit Hadoop zum Definieren und Ausführen eines Workflows in HDInsight](hdinsight-use-oozie.md).

##Hochladen von Daten in Azure-Blobspeicher
Siehe [Hochladen von Daten in HDInsight][hdinsight-upload-data].


## Weitere Informationen
* [Referenzdokumentation zum HDInsight-Cmdlet][hdinsight-powershell-reference]
* [Verwalten von HDInsight mit dem Azure-Portal][hdinsight-admin-portal]
* [Verwalten von HDInsight über eine Befehlszeilenschnittstelle][hdinsight-admin-cli]
* [Erstellen von Hadoop-Clustern in HDInsight][hdinsight-provision]
* [Hochladen von Daten in HDInsight][hdinsight-upload-data]
* [Programmgesteuerte Übermittlung von Hadoop-Aufträgen][hdinsight-submit-jobs]
* [Erste Schritte mit Azure HDInsight][hdinsight-get-started]


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-provision-custom-options]: hdinsight-provision-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: powershell-install-configure.md

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png

<!---HONumber=AcomDC_0817_2016-->