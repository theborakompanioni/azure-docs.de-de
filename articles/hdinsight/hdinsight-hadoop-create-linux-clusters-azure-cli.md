<properties
   	pageTitle="Bereitstellen von Hadoop-, HBase- oder Storm-Clustern unter Linux in HDInsight mithilfe der plattformübergreifenden Azure-Befehlszeilenschnittstelle | Microsoft Azure"
   	description="Erfahren Sie, wie Sie Linux-basierte HDInsight-Cluster mit der plattformübergreifenden Azure-Befehlszeilenschnittstelle, Azure-Ressourcen-Manager-Vorlagen und der Azure-REST-API erstellen. Sie können den Cluster-Typ (Hadoop, HBase oder Storm) angeben oder Skripts verwenden, um benutzerdefinierte Komponenten zu installieren."
   	services="hdinsight"
   	documentationCenter=""
   	authors="Blackmist"
   	manager="paulettm"
   	editor="cgronlun"
	tags="azure-portal"/>

<tags
   	ms.service="hdinsight"
   	ms.devlang="na"
   	ms.topic="article"
   	ms.tgt_pltfrm="na"
   	ms.workload="big-data"
   	ms.date="06/13/2016"
   	ms.author="larryfr"/>

#Erstellen von Linux-basierten Clustern in HDInsight mithilfe der Azure-Befehlszeilenschnittstelle

[AZURE.INCLUDE [Auswahl](../../includes/hdinsight-selector-create-clusters.md)]

Die Azure-Befehlszeilenschnittstelle ist ein plattformübergreifendes Befehlszeilentool zur Verwaltung von Azure-Diensten. Es kann mit Azure-Ressourcen-Manager-Vorlagen zum Erstellen eines HDInsight-Clusters verwendet werden, zusammen mit zugeordneten Speicherkonten und anderen Diensten.

Bei Azure-Ressourcen-Manager-Vorlagen handelt es sich um JSON-Dokumente, mit denen eine __Ressourcengruppe__ und alle darin enthaltenen Ressourcen (z. B. HDInsight) beschrieben werden. Dieser vorlagenbasierte Ansatz ermöglicht Ihnen das Definieren aller für HDInsight benötigten Ressourcen in einer einzigen Vorlage sowie das Verwalten von Änderungen an der gesamten Gruppe durch __Bereitstellungen__, mit denen Änderungen an der Gruppe vorgenommen werden.

Anhand der Schritte in diesem Dokument werden Sie durch die Erstellung eines neuen HDInsight-Clusters mithilfe der Azure-Befehlszeilenschnittstelle und einer Vorlage geführt.

> [AZURE.IMPORTANT] Bei den Schritten in diesem Dokument wird die Standardanzahl von Workerknoten (4) für einen HDInsight-Cluster verwendet. Wenn Sie mehr als 32 Workerknoten planen, entweder bei Erstellung des Clusters oder durch eine Skalierung des Clusters nach der Erstellung, müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB Arbeitsspeicher (RAM) auswählen.
>
> Weitere Informationen zu Knotengrößen und damit verbundenen Kosten finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).

##Voraussetzungen

[AZURE.INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

- **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
- __Azure-Befehlszeilenschnittstelle__. Die Schritte in diesem Dokument wurden mit der neuesten Version der Azure-CLI (0.10.1) getestet.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##Anmelden bei Ihrem Azure-Abonnement

Führen Sie die Schritte aus, die unter [Herstellen einer Verbindung mit einem Azure-Abonnement von der Azure Befehlszeilenschnittstelle (Azure CLI)](../xplat-cli-connect.md) dokumentiert sind, und stellen Sie eine Verbindung mit Ihrem Abonnement über die __login__-Methode her.

##Erstellen eines Clusters

Die folgenden Schritte müssen in einer Befehlszeilen-, Shell- oder Terminalsitzung nach der Installation und Konfiguration der Azure-Befehlszeilenschnittstelle erfolgen.

1. Führen Sie den folgenden Befehl aus, um sich bei Ihrem Azure-Abonnement zu authentifizieren:

        azure login

    Sie werden aufgefordert, Ihren Namen und das Kennwort anzugeben. Wenn Sie über mehrere Azure-Abonnements verfügen, können Sie `azure account set <subscriptionname>` zum Festlegen des Abonnements verwenden, das von den Befehlen der Azure-Befehlszeilenschnittstelle genutzt werden soll.

3. Wechseln Sie mit dem folgenden Befehl in den Azure-Ressourcen-Manager-Modus:

        azure config mode arm

4. Erstellen Sie eine neue Ressourcengruppe. Die zu erstellende Gruppe umfasst den HDInsight-Cluster und das zugeordnete Speicherkonto.

        azure group create groupname location
        
    * Ersetzen Sie __groupname__ durch einen eindeutigen Gruppenamen. 
    * Ersetzen Sie __location__ durch die geografische Region, in der die Gruppe erstellt werden soll. 
    
        Eine Liste der gültigen Speicherorte können Sie mithilfe des Befehls `azure locations list` erzeugen. Verwenden Sie anschließend einen der Speicherorte aus der Spalte __Name__.

5. Erstellen Sie ein neues Speicherkonto Dies wird als Standardspeicher für den HDInsight-Cluster verwendet werden.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename
        
     * Ersetzen Sie __groupname__ durch den Namen der im vorherigen Schritt erstellten Gruppe:
     * Ersetzen Sie __location__ durch den gleichen Speicherort, den Sie im vorherigen Schritt verwendet haben. 
     * Ersetzen Sie __storagename__ durch einen eindeutigen Namen für das Speicherkonto.
     
     > [AZURE.NOTE] Verwenden Sie `azure storage account create -h`, um sich in der Hilfe zu diesem Befehl weitere Informationen zu den verwendeten Parametern anzeigen zu lassen.

5. Rufen Sie den Schlüssel ab, der für den Zugriff auf das Speicherkonto verwendet wird.

        azure storage account keys list -g groupname storagename
        
    * Ersetzen Sie __groupname__ durch den Namen der Ressourcengruppe.
    * Ersetzen Sie __storagename__ durch den Namen des Speicherkontos.
    
    Speichern Sie von den zurückgegebenen Daten den __key__-Wert für __key1__.

6. Erstellen Sie einen neuen HDInsight-Cluster.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 2 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Ersetzen Sie __groupname__ durch den Namen der Ressourcengruppe.
    * Ersetzen Sie __location__ durch den gleichen Speicherort, den Sie in den vorherigen Schritten verwendet haben.
    * Ersetzen Sie __storagename__ durch den Namen des Speicherkontos.
    * Ersetzen Sie __storagekey__ durch den Schlüssel, den Sie im vorherigen Schritt abgerufen haben. 
    * Verwenden Sie für den `--defaultStorageContainer`-Parameter den gleichen Namen wie für den Cluster.
    * Ersetzen Sie __admin__ und __httppassword__ durch den Namen und das Kennwort, die Sie beim Zugriff auf den Cluster über HTTPS verwenden möchten.
    * Ersetzen Sie __sshuser__ und __sshuserpassword__ durch den Benutzernamen und das Kennwort, die Sie beim Zugriff auf den Cluster über SSH verwenden möchten.

    Die Fertigstellung der Clustererstellung kann möglicherweise einige Minuten dauern. In der Regel dauert es etwa 15 Minuten.

##Nächste Schritte

Nachdem Sie einen HDInsight-Cluster erfolgreich mithilfe der Azure-Befehlszeilenschnittstelle erstellt haben, nutzen Sie die folgenden Informationen, um mehr über die Arbeit mit Ihrem Cluster zu lernen:

###Hadoop-Cluster

* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce mit HDInsight](hdinsight-use-mapreduce.md)

###HBase-Cluster

* [Erste Schritte mit HBase in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Entwickeln von Java-Anwendungen für HBase in HDInsight](hdinsight-hbase-build-java-maven-linux.md)

###Storm-Cluster

* [Entwickeln von Java-Topologien für Storm in HDInsight](hdinsight-storm-develop-java-topology.md)
* [Verwenden von Python-Komponenten in Storm in HDInsight](hdinsight-storm-develop-python-topology.md)
* [Bereitstellen und Überwachen von Topologien mit Storm in HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)

<!---HONumber=AcomDC_0615_2016-->