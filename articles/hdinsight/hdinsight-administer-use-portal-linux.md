<properties
	pageTitle="Verwalten von Linux-basierten Hadoop-Clustern in HDInsight mit dem Azure-Portal | Microsoft Azure"
	description="Hier erhalten Sie Informationen zum Erstellen und Verwalten von Linux-basierten HDInsight-Clustern mithilfe des Azure-Portals."
	services="hdinsight"
	documentationCenter=""
	authors="Blackmist"
	manager="paulettm"
	editor="cgronlun"
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/30/2016"
	ms.author="larryfr"/>

#Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal

[AZURE.INCLUDE [Auswahl](../../includes/hdinsight-portal-management-selector.md)]


Mithilfe des [Azure-Portals][preview-portal] können Sie Linux-basierte Hadoop-Cluster in Azure HDInsight bereitstellen und verwalten.

> [AZURE.NOTE] Die Schritte in diesem Dokument gelten für Linux-basierte Hadoop-Cluster. Informationen zur Verwendung von Windows-basierten Clustern finden Sie unter [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-management-portal.md).


[AZURE.INCLUDE [preview-portal](../../includes/hdinsight-azure-preview-portal-nolink.md)]


## Andere Tools zum Verwalten von HDInsight
Neben dem Azure-Portal stehen auch andere Tools zum Verwalten von HDInsight zur Verfügung.

- [Verwalten von HDInsight mithilfe der Azure-Befehlszeilenschnittstelle](hdinsight-administer-use-command-line.md): Die Azure-Befehlszeilenschnittstelle ist ein plattformübergreifendes Befehlszeilentool, mit dem Sie Azure-Dienste verwalten können.

- [Verwalten von HDInsight mit Azure PowerShell](hdinsight-administer-use-powershell.md): Azure PowerShell stellt PowerShell-Cmdlets zum Verwalten von Azure-Diensten bereit.

## Voraussetzungen

Bevor Sie mit diesem Artikel beginnen können, benötigen Sie Folgendes:

- **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## Erstellen von Hadoop-Clustern

Sie können HDInsight-Cluster aus dem Azure-Portal erstellen. Dieser Vorgang wird im Dokument [Erstellen von Linux-basierten Clustern in HDInsight mit dem Azure-Portal](hdinsight-hadoop-create-linux-clusters-portal.md) ausführlich erläutert.

## Verwalten eines Clusters

Wenn Sie einen Cluster im Azure-Portal auswählen, werden grundlegende Informationen zum Cluster angezeigt, z. B. der Name, die Ressourcengruppe, das Betriebssystem und die URL für den Cluster (wird für den Zugriff auf Ambari-Web für Linux-basierte Cluster verwendet.)

![Clusterdetails](./media/hdinsight-administer-use-portal-linux/clusterdetails.png)

Im Folgenden werden die Symbole oben auf diesem Blatt und in den Abschnitten __Zusammenfassung__ und __QuickLinks__ erläutert:

* __Einstellungen__ und __Alle Einstellungen__: Zeigt das Blatt __Einstellungen__ für den Cluster an, über das Sie auf detaillierte Konfigurationsinformationen für den Cluster zugreifen können.

* __Dashboard__, __Cluster-Dashboard__ und __URL__: Über diese Optionen können Sie auf das Cluster-Dashboard zugreifen. Abhängig vom Clustertyp wird möglicherweise eine Liste der Dashboards im Cluster angezeigt. Beispielsweise wird für den Spark-Clustertyp eine Liste mit Dashboards angezeigt, wenn Sie das __Dashboard__-Symbol auswählen, während bei einem Hadoop-Cluster die Ambari Web-Benutzeroberfläche geöffnet wird.

* __Secure Shell__: Informationen, die für den Zugriff auf den Clusterhauptknoten über SSH erforderlich sind.

* __Cluster skalieren__: Dient zum Ändern der Anzahl von Workerknoten für den Cluster.

* __Löschen__: Löscht den HDInsight-Cluster.

* __Schnellstart__ (![Cloud- und Blitzsymbol = Schnellstart](./media/hdinsight-administer-use-portal-linux/quickstart.png)): Zeigt hilfreiche Informationen für die ersten Schritte mit HDInsight an.

* __Benutzer__ (![Benutzersymbol](./media/hdinsight-administer-use-portal-linux/users.png)): Dient zum Festlegen der Berechtigungen für die _Portalverwaltung_ dieses Clusters für andere Benutzer in Ihrem Azure-Abonnement.

	> [AZURE.IMPORTANT] Die hier vorgenommenen Einstellungen betreffen _nur_ den Zugriff und die Berechtigungen für diesen Cluster im Azure-Portal und haben keine Auswirkung darauf, wer eine Verbindung mit dem HDInsight-Cluster herstellen oder Aufträge übermitteln kann.

* __Tags__ (![Tagsymbol](./media/hdinsight-administer-use-portal-linux/tags.png)): Mithilfe von Tags können Sie Schlüssel-Wert-Paare festlegen, um eine benutzerdefinierte Taxonomie für Ihre Cloud Services zu definieren. Sie können z. B. einen Schlüssel mit dem Namen __Projekt__ erstellen und dann einen gemeinsamen Wert für alle mit einem bestimmten Projekt verknüpften Dienste verwenden.

* __Dokumentation__: Links zur Dokumentation für Azure HDInsight.

> [AZURE.IMPORTANT] Um die vom HDInsight-Cluster bereitgestellten Dienste zu verwalten, müssen Sie Ambari Web oder die Ambari-REST-API verwenden. Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md).

### Einstellungen

Durch Auswahl des Symbols __Einstellungen__ oder des Links __Alle Einstellungen__ wird das Blatt „Einstellungen“ angezeigt. Verschiedene der im Bereich __Grundlagen__ verfügbaren Funktionen, die zuvor besprochen wurden, sind auch hier verfügbar, beispielsweise die Skalierung oder Informationen zu Secure Shell. Sie können über die Einstellungen außerdem auf Folgendes zugreifen:

* __Überwachungsprotokolle__: Protokollierte Informationen, die zur Diagnose von Problemen mit der Clusterintegrität nützlich sind.

* __Clusteranmeldung__: Zeigt den __Clusteranmeldenamen__ und die __Remoteadresse__ an, der/die für den Zugriff auf den Cluster über HTTPS verwendet werden kann.

* __Externe Mestores__: Zeigt Informationen zu externen Metastores an, die ggf. von Ihrem Cluster verwendet werden. Wenn Sie während der Clusterkonfiguration keinen benutzerdefinierten Metastore konfiguriert haben, werden keine Informationen angezeigt.

* __Skriptaktionen__: Zeigt Informationen zu Skriptaktionen an, die auf diesem Cluster ausgeführt wurden. Sie können auch neue Skriptaktionen ausführen und zuvor ausgeführte Skriptaktionen speichern oder löschen. Weitere Informationen zu Skriptaktionen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

* __Apps__: Zeigt Informationen zu Apps an, die im Cluster installiert wurden, und ermöglicht es Ihnen, dem Cluster über den Azure Marketplace neue Anwendungen hinzuzufügen.

* __Azure-Speicherschlüssel__: Zeigt Informationen zu den vom Cluster verwendeten Azure-Speicherkonten an. Wenn Sie ein Speicherkonto auswählen, wird das Speicherkontoblatt für das ausgewählte Konto geladen.

* __Azure Active Directory-Identität für den Cluster__: Zeigt Informationen zum Dienstprinzipal für diesen HDInsight-Cluster an. Der Dienstprinzipal wird verwendet, um auf Azure Data Lake-Speicher zuzugreifen. Wenn Sie Ihren Cluster während der Erstellung nicht dem Azure Data Lake-Speicher zugeordnet haben, wird auf diesem Blatt die Information __Nicht konfiguriert__ angezeigt. Weitere Informationen zum Verwenden des Azure Data Lake-Speichers mit HDInsight finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake-Speicher](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### <a name="scaling"></a>Skalieren

Zum Skalieren eines Clusters über das Portal wählen Sie den HDInsight-Cluster und anschließend __Cluster skalieren__ aus. Geben Sie die __Anzahl von Workerknoten__ ein, die Sie für den Cluster festlegen möchten, und klicken Sie auf __Speichern__.

![Dialogfeld für die Skalierung](./media/hdinsight-administer-use-portal-linux/scaling.png)

Weitere Informationen zum Skalieren von Vorgängen finden Sie unter [Informationen zur Verwendung von HDInsight unter Linux](hdinsight-hadoop-linux-information.md#scaling).

## Überwachen eines Clusters

Der Abschnitt __Nutzung__ des Blatts für den HDInsight-Cluster enthält Informationen zur Anzahl von Kernen, die für Ihr Abonnement zur Verwendung mit HDInsight verfügbar sind, und zur Anzahl von Kernen, die diesem Cluster zugeordnet sind. Es wird auch angezeigt, wie die Kerne für die Knoten innerhalb dieses Clusters zugeordnet werden.

![Informationen zur Nutzung](./media/hdinsight-administer-use-portal-linux/usage.png)

> [AZURE.IMPORTANT] Um die vom HDInsight-Cluster bereitgestellten Dienste zu überwachen, müssen Sie Ambari Web oder die Ambari-REST-API verwenden. Weitere Informationen zur Verwendung von Ambari finden Sie unter [Verwalten von HDInsight-Clustern mit Ambari](hdinsight-hadoop-manage-ambari.md).

## Nächste Schritte
In diesem Artikel haben Sie erfahren, wie ein HDInsight-Cluster mit dem Azure-Portal erstellt und wie das Hadoop-Befehlszeilentool geöffnet wird. Weitere Informationen finden Sie in den folgenden Artikeln:

* [Verwalten von HDInsight mit Azure PowerShell](hdinsight-administer-use-powershell.md)
* [Verwalten von HDInsight mit der Azure-CLI](hdinsight-administer-use-command-line.md)
* [Bereitstellen von HDInsight-Clustern](hdinsight-provision-clusters.md)
* [Programmgesteuerte Übermittlung von Hadoop-Aufträgen](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Erste Schritte mit Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Welche Hadoop-Version wird in Azure HDInsight verwendet?](hdinsight-component-versioning.md)

[preview-portal]: https://portal.azure.com

<!---HONumber=AcomDC_0406_2016-->