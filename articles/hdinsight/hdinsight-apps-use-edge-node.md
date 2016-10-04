<properties
	pageTitle="Verwenden leerer Edgeknoten in HDInsight | Microsoft Azure"
	description="Sie erfahren, wie Sie HDInsight-Cluster einen leeren Edgeknoten hinzufügen, der als Client verwendet werden kann, und wie Sie Ihre HDInsight-Anwendungen testen/hosten."
	services="hdinsight"
	editor="cgronlun"
	manager="jhubbard"
	authors="mumian"
	tags="azure-portal"
	documentationCenter=""/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/14/2016"
	ms.author="jgao"/>

# Verwenden leerer Edgeknoten in HDInsight

Erfahren Sie, wie Sie einem Linux-basierten HDInsight-Cluster einen leeren Edgeknoten hinzufügen. Ein leerer Edgeknoten ist ein virtueller Linux-Computer, auf dem die gleichen Clienttools installiert und konfiguriert sind wie im Hauptknoten, aber keine Hadoop-Dienste ausgeführt werden. Sie können den Edgeknoten zum Zugreifen auf den Cluster sowie zum Testen und Hosten Ihrer Clientanwendungen verwenden.

Sie können einen leeren Edgeknoten zu einem vorhandenen HDInsight-Cluster oder zu einem neuen Cluster hinzufügen, wenn Sie den Cluster erstellen. Einen leeren Edgeknoten fügen Sie mit der Azure Resource Manager-Vorlage hinzu. Im folgenden Beispiel sehen Sie, wie dies mithilfe einer Vorlage erfolgt:

    "resources": [
		{
			"name": "[concat(parameters('clusterName'),'/', variables('applicationName'))]",
			"type": "Microsoft.HDInsight/clusters/applications",
			"apiVersion": "[variables('clusterApiVersion')]",
			"properties": {
				"marketPlaceIdentifier": "EmptyNode",
				"computeProfile": {
					"roles": [{
						"name": "edgenode",
						"targetInstanceCount": 1,
						"hardwareProfile": {
							"vmSize": "[parameters('edgeNodeSize')]"
						}
					}]
				},
				"installScriptActions": [{
					"name": "[concat('emptynode','-' ,uniquestring(variables('applicationName')))]",
					"uri": "https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh",
					"roles": ["edgenode"]
				}],
				"uninstallScriptActions": [],
				"httpsEndpoints": [],
				"applicationType": "CustomApplication"
			}
		}
	],

Wie im Beispiel gezeigt, können Sie optional eine [Skriptaktion](hdinsight-hadoop-customize-cluster-linux.md) aufrufen, um zusätzliche Konfigurationsschritte wie die Installation von [Apache Farbton](hdinsight-hadoop-hue-linux.md) in den Edgeknoten auszuführen.

Nach der Erstellung eines Edgeknotens können Sie über SSH eine Verbindung zum Edgeknoten herstellen und Client-Tools ausführen, um auf den Hadoop-Cluster in HDInsight zuzugreifen.

## Hinzufügen eines Edgeknotens zu einem vorhandenen Cluster

In diesem Abschnitt verwenden Sie eine Resource Manager-Vorlage, um einen Edgeknoten zu einem vorhandenen HDInsight-Cluster hinzuzufügen. Die Resource Manager-Vorlage finden Sie in [GitHub](https://github.com/hdinsight/Iaas-Applications/tree/master/EmptyNode). Die Resource Manager-Vorlage ruft ein Skript für Skriptaktionen unter https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh auf. Das Skript führt keine Aktionen aus. Es demonstriert das Aufrufen der Skriptaktion aus einer Resource Manager-Vorlage.

**Hinzufügen eines Edgeknotens zu einem vorhandenen Cluster**

1. Erstellen Sie einen HDInsight-Cluster, wenn Sie noch keinen besitzen. Informationen dazu erhalten Sie im [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Linux-basiertem Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klicken Sie auf die folgende Abbildung, um sich bei Azure anzumelden und die Azure Resource Manager-Vorlage im Azure-Portal zu öffnen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fhdinsight%2FIaas-Applications%2Fmaster%2FEmptyNode%2Fazuredeploy.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurieren Sie die folgenden Eigenschaften:

	- CLUSTERNAME: Geben Sie den Namen eines vorhandenen HDInsight-Clusters ein.
	- EDGENODESIZE: Wählen Sie eine der VM-Größen aus.
	- EDGENODEPREFIX: Der Standardwert ist **new** (Neu). Wenn Sie den Standardwert verwenden, heißt der Edgeknoten **new-edgenode**. Sie können das Präfix im Portal anpassen. Sie können auch den vollständigen Namen in der Vorlage anpassen.


4. Klicken Sie zum Speichern der Änderungen auf **OK**.
5. Wahlen Sie unter **Ressourcengruppe** eine Ressourcengruppe aus.
6. Klicken Sie auf **Prüfen Sie die rechtlichen Bedingungen**, und klicken Sie dann auf **Kaufen**.
7. Wählen Sie **An Dashboard anheften**, und klicken Sie dann auf **Erstellen**.

## Hinzufügen eines Edgeknotens beim Erstellen eines Clusters

In diesem Abschnitt verwenden Sie eine Resource Manager-Vorlage, um HDInsight-Cluster mit einem Edgeknoten zu erstellen. Die Resource Manager-Vorlage finden Sie in einem öffentlichen [Azure Blobspeichercontainer](http://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json). Die Resource Manager-Vorlage ruft ein Skript für Skriptaktionen unter https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/EmptyNode/scripts/EmptyNodeSetup.sh auf. Das Skript führt keine Aktionen aus. Es demonstriert das Aufrufen der Skriptaktion aus einer Resource Manager-Vorlage.

**Hinzufügen eines Edgeknotens zu einem vorhandenen Cluster**

1. Erstellen Sie einen HDInsight-Cluster, wenn Sie noch keinen besitzen. Informationen dazu erhalten Sie im [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Linux-basiertem Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
2. Klicken Sie auf die folgende Abbildung, um sich bei Azure anzumelden und die Azure Resource Manager-Vorlage im Azure-Portal zu öffnen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hadoop-cluster-in-hdinsight-with-edge-node.json" target="_blank"><img src="https://acom.azurecomcdn.net/80C57D/cdn/mediahandler/docarticles/dpsmedia-prod/azure.microsoft.com/en-us/documentation/articles/hdinsight-hbase-tutorial-get-started-linux/20160201111850/deploy-to-azure.png" alt="Deploy to Azure"></a>

3. Konfigurieren Sie die folgenden Eigenschaften:
		
	- CLUSTERNAME: Geben Sie einen Namen für den neu zu erstellenden Cluster ein.
	- CLUSTERLOGINUSERNAME: Geben Sie den Hadoop-HTTP-Benutzernamen ein. Der Standardname lautet **admin**.
	- CLUSTERLOGINPASSWORD: Geben Sie das Hadoop-HTTP-Benutzerpasswort ein.
	- SSHUSERNAME: Geben Sie den SSH-Benutzernamen ein. Der Standardname ist **sshuser**.
	- SSHPASSWORD: Geben Sie das SSH-Benutzerpasswort ein.
	- LOCATION: Geben Sie den Speicherort des Ressourcengruppennamens, des Clusters und des Standardspeicherkontos ein.
	- CLUSTERTYPE: Der Standardwert ist **hadoop**.
	- CLUSTERWORKERNODECOUNT: Der Standardwert ist 2.
	- EDGENODESIZE: Wählen Sie eine der VM-Größen aus.
	- EDGENODEPREFIX: Der Standardwert ist **new** (Neu). Wenn Sie den Standardwert verwenden, heißt der Edgeknoten **new-edgenode**. Sie können das Präfix im Portal anpassen. Sie können auch den vollständigen Namen in der Vorlage anpassen.

4. Klicken Sie zum Speichern der Änderungen auf **OK**.
5. Geben Sie unter **Ressourcengruppe** einen Namen für die Ressourcengruppe ein.
6. Klicken Sie auf **Prüfen Sie die rechtlichen Bedingungen**, und klicken Sie dann auf **Kaufen**.
7. Wählen Sie **An Dashboard anheften**, und klicken Sie dann auf **Erstellen**.


## Zugreifen auf einen Edgeknoten

Der ssh-Endpunkt des Edgeknotens ist <EdgeNodeName>.<ClusterName>-ssh.azurehdinsight.net:22. Beispiel: new-edgenode.myedgenode0914-ssh.azurehdinsight.net:22.

Der Edgeknoten wird als Anwendung im Azure-Portal angezeigt. Im Portal erfahren Sie, wie Sie über SSH auf den Edgeknoten zuzugreifen.

**Überprüfen des SSH-Endpunkts der Edgeknoten**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Öffnen Sie den HDInsight-Cluster mit einem Edgeknoten.
3. Klicken Sie im Blatt „Cluster“ auf **Anwendungen**. Der Edgeknoten sollte angezeigt werden. Der Standardname lautet **new-edgenode**.
4. Klicken Sie auf den Edgeknoten. Der SSH-Endpunkt sollte angezeigt werden.

**Verwenden von Hive auf dem Edgeknoten**

5. Stellen Sie über SSH eine Verbindung mit dem Edgeknoten her. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix und OS X](hdinsight-hadoop-linux-use-ssh-unix.md) oder [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md).
6. Verwenden Sie nach dem Herstellen einer Verbindung zum Edgeknoten über SSH den folgenden Befehl, um die Hive-Konsole zu öffnen:

		hive
7. Führen Sie den folgenden Befehl aus, um Hive-Tabellen im Cluster anzuzeigen:

		show tables;

## Löschen eines Edgeknotens

Sie können einen Edgeknoten aus dem Azure-Portal löschen.

**Zugreifen auf einen Edgeknoten**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an.
2. Öffnen Sie den HDInsight-Cluster mit einem Edgeknoten.
3. Klicken Sie im Blatt „Cluster“ auf **Anwendungen**. Eine Liste mit Edgeknoten sollte angezeigt werden.
4. Klicken Sie mit der rechten Maustaste auf den Edgeknoten, den Sie löschen möchten, und klicken Sie dann auf **Löschen**.
5. Klicken Sie auf **Ja**, um zu bestätigen.

## Nächste Schritte

In diesem Artikel haben Sie erfahren, wie Sie einen Edgeknoten hinzufügen und auf den Edgeknoten zuzugreifen. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Installieren von HDInsight-Anwendungen](hdinsight-apps-install-applications.md): Hier erfahren Sie, wie Sie eine HDInsight-Anwendung in Ihren Clustern installieren.
- [Installieren benutzerdefinierter HDInsight-Anwendungen](hdinsight-apps-install-custom-applications.md): Hier erfahren Sie, wie Sie eine nicht veröffentlichte HDInsight-Anwendung in HDInsight bereitstellen.
- [Veröffentlichen von HDInsight-Anwendungen](hdinsight-apps-publish-applications.md): Hier erfahren Sie, wie Sie benutzerdefinierte HDInsight-Anwendungen im Azure Marketplace veröffentlichen.
- [MSDN: Install an HDInsight application](https://msdn.microsoft.com/library/mt706515.aspx) (MSDN: Installieren einer HDInsight-Anwendung): Hier erfahren Sie, wie Sie HDInsight-Anwendungen definieren.
- [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md): Hier erfahren Sie, wie Sie mithilfe der Skriptaktion zusätzliche Anwendungen installieren.
- [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight mit Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md): Hier erfahren Sie, wie Sie Resource Manager-Vorlagen für die Erstellung von HDInsight-Clustern aufrufen.

<!---HONumber=AcomDC_0914_2016-->