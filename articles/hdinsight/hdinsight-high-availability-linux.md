<properties
	pageTitle="Funktionen für hohe Verfügbarkeit bei Linux-basiertem HDInsight (Hadoop) | Microsoft Azure"
	description="Erfahren Sie, wie Linux-basierte HDInsight-Cluster durch die Verwendung eines zusätzlichen Hauptknotens eine höhere Zuverlässigkeit und Verfügbarkeit bieten. Sie erfahren, wie dies Hadoop-Dienste wie Ambari und Hive beeinflusst und wie eine Verbindung mit jedem Hauptknoten über SSH hergestellt wird."
	services="hdinsight"
	editor="cgronlun"
	manager="jhubbard"
	authors="Blackmist"
	documentationCenter=""
	tags="azure-portal"/>

<tags
	ms.service="hdinsight"
	ms.workload="big-data"
	ms.tgt_pltfrm="na"
	ms.devlang="multiple"
	ms.topic="article"
	ms.date="09/13/2016"
	ms.author="larryfr"/>

#Verfügbarkeit und Zuverlässigkeit von Hadoop-Clustern in HDInsight

Hadoop erzielt hohe Verfügbarkeit und Zuverlässigkeit durch Verteilen von redundanten Kopien von Diensten und Daten auf die Knoten in einem Cluster. Hadoop-Standarddistributionen weisen jedoch typischerweise nur einen einzigen Hauptknoten auf. Jeder Ausfall dieses Hauptknotens kann dazu führen, dass der Cluster nicht mehr funktioniert.

Zur Behebung dieses potenziellen Problems stellen Linux-basierte HDInsight-Cluster in Azure zwei Hauptknoten bereit, um die Verfügbarkeit und Zuverlässigkeit von ausgeführten Hadoop-Diensten und -Aufträgen zu verbessern.

> [AZURE.NOTE] Die Schritte in diesem Dokument gelten für Linux-basierte HDInsight-Cluster. Wenn Sie einen Windows-basierten Cluster verwenden, finden Sie unter [Verfügbarkeit und Zuverlässigkeit von Windows-basierten Hadoop-Clustern in HDInsight](hdinsight-high-availability.md) entsprechende Informationen für Windows.

##Grundlegendes zu den Knoten

Knoten in einem HDInsight-Cluster werden mithilfe von virtuellen Azure-Computern implementiert. Falls ein Knoten ausfällt, wird er offline geschaltet, und es wird ein neuer Knoten erstellt, um den fehlerhaften Knoten zu ersetzen. Während der Knoten offline ist, wird ein anderer Knoten desselben Typs verwendet, bis der neue Knoten online geschaltet wird.

> [AZURE.NOTE] Wenn auf dem Knoten zum Zeitpunkt des Ausfalls Daten analysiert wurden, gehen die Fortschritte dieses Auftrags verloren. Der Auftrag, der auf dem Knoten ausgeführt wurde, wird neu an einen anderen Knoten übermittelt.

In den folgenden Abschnitten werden die einzelnen Arten von Knoten erläutert, die mit HDInsight verwendet werden. Es werden nicht alle Knotentypen für alle Clustertypen verwendet. Ein Hadoop-Cluster z. B. weist keine Nimbusknoten auf. Weitere Informationen zu den von den HDInsight-Clustertypen verwendeten Knoten finden Sie im Artikel [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md#cluster-types) im Abschnitt „Clustertypen“.

###Hauptknoten

In einigen Implementierungen von Hadoop hostet ein einzelner Hauptknoten Dienste und Komponenten, die zur reibungslosen Verwaltung ausgefallener Workerknoten dienen. Bei einem Ausfall eines Masterdiensts auf dem Hauptknoten würde jedoch der komplette Cluster ausfallen.

HDInsight-Cluster verfügen über einen sekundären Hauptknoten, sodass Masterdienste und -komponenten bei einem Ausfall des primären Knotens weiterhin auf dem sekundären Knoten ausgeführt werden können.

> [AZURE.IMPORTANT] Beide Hauptknoten sind aktiv und werden gleichzeitig innerhalb des Clusters ausgeführt. Einige Dienste (z. B. HDFS oder YARN) sind jeweils nur auf einem Hauptknoten „aktiv“ (und auf dem anderen Hauptknoten im „Standby“). Andere Dienste (z. B. HiveServer2 oder Hive-MetaStore) sind gleichzeitig auf beiden Hauptknoten aktiv.

Hauptknoten (und andere Knoten in HDInsight) verfügen über numerische Werte als Teil des Hostnamens des Knotens. Beispiel: `hn0-CLUSTERNAME` oder `hn4-CLUSTERNAME`.

> [AZURE.IMPORTANT] Der numerische Wert gibt nicht an, ob ein Knoten primär oder sekundär ist; der numerische Wert vergibt nur einen eindeutigen Namen für jeden Knoten.

###Nimbusknoten

In Storm-Clustern stellen Nimbusknoten die gleiche Funktionalität für den Hadoop JobTracker bereit, indem sie die Verarbeitung auf Workerknoten verteilen und überwachen. HDInsight stellt zwei Nimbusknoten für den Storm-Clustertyp bereit.

###Zookeeper-Knoten

[ZooKeeper](http://zookeeper.apache.org/)-Knoten (ZKs) wählen eine übergeordnete Instanz von Masterdiensten auf Hauptknoten aus und teilen Diensten, Datenknoten (Workerknoten) und Gateways mit, auf welchem Hauptknoten ein Masterdienst aktiv ist. Standardmäßig stellt HDInsight drei Zookeeper-Knoten bereit.

###Workerknoten

Workerknoten führen die eigentliche Analyse aus, wenn ein Auftrag an den Cluster übermittelt wird. Wenn ein Workerknoten ausfällt, wird die Ausgabe, die auf dem Knoten gerade ausgeführt wurde, an einen anderen Workerknoten übermittelt. Standardmäßig erstellt HDInsight vier Workerknoten. Sie können diese Anzahl jedoch sowohl während als auch nach der Clustererstellung gemäß Ihren Anforderungen ändern.

###Edgeknoten

Ein Edgeknoten ist nicht aktiv an der Datenanalyse im Cluster beteiligt, sondern wird von Entwicklern oder Datenspezialisten bei der Arbeit mit Hadoop verwendet. Der Edgeknoten befindet sich im gleichen virtuellen Azure-Netzwerk wie die anderen Knoten im Cluster und kann direkt auf alle anderen Knoten zugreifen. Da dieser Knoten nicht an der Analyse von Daten für den Cluster beteiligt ist, kann er problemlos eingesetzt werden, ohne dass Sie sich Gedanken darüber machen müssen, ob dadurch Ressourcen von kritischen Hadoop-Diensten oder -Analyseaufträgen abgezogen werden.

Zurzeit ist R Server auf HDInsight der einzige Clustertyp, der standardmäßig einen Edgeknoten bereitstellt. Für R Server auf HDInsight wird der Edgeknoten verwendet, um R-Code lokal auf dem Knoten zu testen, bevor der Code zur verteilten Verarbeitung an den Cluster übermittelt wird.

[Create a Linux-based HDInsight cluster with Hue on an Edge Node](https://azure.microsoft.com/documentation/templates/hdinsight-linux-with-hue-on-edge-node/) (Erstellen eines Linux-basierten HDInsight-Clusters mit Hue auf einem Edgeknoten) ist eine Beispielvorlage, die zum Erstellen eines Hadoop-Clustertyps verwendet werden kann, der über einen Edgeknoten verfügt.


## Zugreifen auf die Knoten

Der Zugriff auf den Cluster über das Internet wird über ein öffentliches Gateway bereitgestellt und ist auf das Herstellen einer Verbindung mit den Hauptknoten und (wenn es sich um einen Cluster mit R Server in HDInsight handelt) dem Edgeknoten beschränkt. Die Bereitstellung mehrerer Hauptknoten wirkt sich nicht auf den Zugriff auf die auf den Hauptknoten ausgeführten Dienste aus, da das öffentliche Gateway Anforderungen an den Hauptknoten weiterleitet, der den angeforderten Dienst hostet. Ein Beispiel: Ambari wird zurzeit auf dem sekundären Hauptknoten gehostet, und das Gateway leitet eingehende Anforderungen für Ambari an diesen Knoten weiter.

Beim Zugriff auf den Cluster über SSH wird bei Verwendung von Port 22 (Standardport für SSH) eine Verbindung mit dem primären Hauptknoten hergestellt; bei Verwendung von Port 23 eine Verbindung mit dem sekundären Hauptknoten. `ssh username@mycluster-ssh.azurehdinsight.net` stellt z.B. eine Verbindung mit dem primären Hauptknoten des Clusters mit der Bezeichnung __mycluster__ her.

> [AZURE.NOTE] Dies gilt auch für SSH-basierte Protokolle, wie etwa SFTP (SSH File Transfer Protocol).

Auf den Edgeknoten, der in Clustern mit R Server in HDInsight bereitgestellt wird, kann auch direkt mithilfe von SSH über Port 22 zugegriffen werden. `ssh username@RServer.mycluster.ssh.azurehdinsight.net` stellt z. B. eine Verbindung mit dem Edgeknoten für einen Cluster mit R Server in HDInsight mit der Bezeichnung __mycluster__ her.

### Interne vollqualifizierte Domänennamen (FQDN)

Knoten in einem HDInsight-Cluster besitzen eine interne IP-Adresse und einen internen FQDN, auf die nur über den Cluster zugegriffen werden kann (z. B. eine SSH-Sitzung mit dem Hauptknoten oder ein im Cluster ausgeführter Auftrag). Wenn Sie mit dem internen FQDN oder der internen IP-Adresse auf Dienste im Cluster zugreifen, sollten Sie die zu verwendende IP-Adresse oder den FQDN mithilfe von Ambari überprüfen.

Der Oozie-Dienst kann z. B. nur auf einem Hauptknoten ausgeführt werden, und zur Verwendung des `oozie`-Befehls in einer SSH-Sitzung ist die URL des Diensts erforderlich. Die URL kann mit dem folgenden Befehl aus Ambari abgerufen werden:

	curl -u admin:PASSWORD "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=oozie-site&tag=TOPOLOGY_RESOLVED" | grep oozie.base.url

Der zurückgegebene Wert enthält die mit dem `oozie`-Befehl zu verwendende interne URL und sieht in etwa wie folgt aus:

	"oozie.base.url": "http://hn0-CLUSTERNAME-randomcharacters.cx.internal.cloudapp.net:11000/oozie"

### Zugreifen auf andere Knotentypen

Mit den folgenden Methoden können Sie eine Verbindung mit Knoten herstellen, auf die nicht direkt über das Internet zugegriffen werden kann.

* __SSH__: Sobald über SSH eine Verbindung mit dem Hauptknoten hergestellt wurde, können Sie SSH auf dem Hauptknoten verwenden, um Verbindungen mit anderen Knoten im Cluster herzustellen.
* __SSH-Tunnel__: Wenn Sie auf einen Webdienst zugreifen müssen, der auf einem der nicht direkt mit dem Internet verbundenen Knoten gehostet wird, müssen Sie [einen SSH-Tunnel verwenden](hdinsight-linux-ambari-ssh-tunnel.md).
* __Virtuelles Azure-Netzwerk__: Wenn Ihr HDInsight-Cluster zu einem virtuellen Azure-Netzwerk gehört, können alle Ressourcen in diesem virtuellen Netzwerk direkt auf alle Knoten im Cluster zugreifen.

## Überprüfen eines Dienststatus

Sie können sowohl die Ambari-Webbenutzeroberfläche als auch die Ambari-REST-API verwenden, um den Status von Diensten zu überprüfen, die auf dem Hauptknoten ausgeführt werden.

###Ambari-Webbenutzeroberfläche

Die Ambari-Webbenutzeroberfläche ist unter https://CLUSTERNAME.azurehdinsight.net verfügbar. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters. Wenn Sie dazu aufgefordert werden, geben Sie die HTTP-Anmeldeinformationen für Ihren Cluster ein. Der standardmäßige HTTP-Benutzername ist **admin**, und das Kennwort ist das Kennwort, das Sie beim Erstellen des Clusters eingegeben haben.

Die installierten Dienste werden im linken Bereich der Ambari-Seite aufgeführt.

![Installierte Dienste](./media/hdinsight-high-availability-linux/services.png)

Neben den Diensten werden möglicherweise Statussymbole angezeigt. Alle Warnungen in Zusammenhang mit einem Dienst können über den Link **Warnungen** oben auf der Seite angezeigt werden. Sie können einen Dienst auswählen, um weitere Informationen anzuzeigen.

Auf der Dienstseite werden zwar Informationen zum Status und zur Konfiguration jedes Diensts angezeigt, aber keine Informationen dazu, auf welchem Hauptknoten der Dienst ausgeführt wird. Diese Informationen können über den Link **Hosts** oben auf der Seite aufgerufen werden. Wenn Sie auf den Link klicken, werden die Hosts innerhalb des Clusters angezeigt, einschließlich der Hauptknoten.

![Liste der Hosts](./media/hdinsight-high-availability-linux/hosts.png)

Wenn Sie auf den Link für einen der Hauptknoten klicken, werden die auf diesem Knoten ausgeführten Dienste und Komponenten angezeigt.

![Komponentenstatus](./media/hdinsight-high-availability-linux/nodeservices.png)

###Ambari-REST-API

Die Ambari-REST-API ist über das Internet verfügbar, und das öffentliche Gateway verarbeitet Routinganforderungen an den Hauptknoten, auf dem die REST-API derzeit gehostet wird.

Sie können den folgenden Befehl verwenden, um den Status eines Diensts über die Ambari-REST-API zu überprüfen:

	curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICENAME?fields=ServiceInfo/state

* Ersetzen Sie **PASSWORD** durch Kontokennwort des HTTP-Benutzers (Administrator).

* Ersetzen Sie **CLUSTERNAME** durch den Namen des Clusters.

* Ersetzen Sie **SERVICENAME** durch den Namen des Diensts, dessen Status Sie überprüfen möchten.

Wenn Sie z. B. den Status des **HDFS**-Diensts in einem Cluster mit dem Namen **mycluster** überprüfen möchten und das Kennwort **password** lautet, würden Sie folgenden Befehl verwenden:

	curl -u admin:password https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state

Die Antwort sieht in etwa wie folgt aus:

	{
	  "href" : "http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8080/api/v1/clusters/mycluster/services/HDFS?fields=ServiceInfo/state",
	  "ServiceInfo" : {
	    "cluster_name" : "mycluster",
	    "service_name" : "HDFS",
	    "state" : "STARTED"
	  }
	}

Der URL können Sie entnehmen, dass der Dienst zurzeit auf einem Hauptknoten namens __hn0-CLUSTERNAME__ ausgeführt wird.

Der Statuswert (state) gibt an, dass der Dienst zurzeit ausgeführt wird bzw. sich im Status **STARTED** befindet.

Falls Sie nicht wissen, welche Dienste auf dem Cluster installiert sind, können Sie mit dem folgenden Befehl eine Liste abrufen:

	curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services

####Dienstkomponenten

Dienste können Komponenten enthalten, deren Status Sie einzeln überprüfen möchten. HDFS enthält z. B. die NameNode-Komponente. Der Befehl zum Anzeigen von Informationen zu einer Komponente lautet wie folgt:

	curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component

Falls Sie nicht wissen, welche Komponenten von einem Dienst bereitgestellt werden, können Sie mit dem folgenden Befehl eine Liste abrufen:

	curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SERVICE/components/component
    
## Zugreifen auf Protokolldateien auf den Hauptknoten

###SSH

Wenn Sie über SSH mit einem Hauptknoten verbunden sind, sind die Protokolldateien unter **/Var/Log** verfügbar. **/var/log/hadoop-yarn/yarn** enthält z. B. Protokolle für YARN.

Da für jeden Hauptknoten spezifische Protokolleinträge vorhanden sein können, sollten Sie die Protokolle auf beiden Hauptknoten überprüfen.

###SFTP

Sie können auch über das SSH File Transfer Protocol oder das Secure File Transfer Protocol (SFTP) eine Verbindung mit dem Hauptknoten herstellen und die Protokolldateien direkt herunterladen.

Ähnlich wie bei der Verwendung eines SSH-Clients müssen Sie beim Herstellen einer Verbindung mit dem Cluster den SSH-Benutzerkontonamen und die SSD-Adresse des Clusters angeben. Beispiel: `sftp username@mycluster-ssh.azurehdinsight.net`. Sie müssen auch das Kennwort für das Konto eingeben, wenn Sie dazu aufgefordert werden, oder mit dem Parameter `-i` einen öffentlichen Schlüssel bereitstellen.

Wenn die Verbindung hergestellt ist, wird Ihnen eine `sftp>`-Eingabeaufforderung angezeigt. An dieser Eingabeaufforderung können Sie Verzeichnisse wechseln sowie Dateien hoch- und herunterladen. Folgende Befehle wechseln z. B. zum Verzeichnis **/var/log/hadoop/hdfs** und laden dann alle Dateien in diesem Verzeichnis herunter.

    cd /var/log/hadoop/hdfs
    get *

Um eine Liste aller verfügbaren Befehle zu erhalten, geben Sie an der `sftp>`-Eingabeaufforderung den Befehlt `help` ein.

> [AZURE.NOTE] Es gibt auch grafische Benutzeroberflächen, mit denen Sie das Dateisystem visuell anzeigen können, wenn eine Verbindung über SFTP hergestellt wurde. [MobaXTerm](http://mobaxterm.mobatek.net/) z. B. ermöglicht das Durchsuchen des Dateisystems in einer Benutzeroberfläche ähnlich Windows Explorer.


###Ambari

> [AZURE.NOTE] Für den Zugriff auf Protokolldateien über Ambari ist ein SSH-Tunnel erforderlich, da die Websites für die einzelnen Dienste nicht öffentlich im Internet verfügbar sind. Informationen zum Verwenden eines SSH-Tunnels finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md).

Wählen Sie in der Ambari-Webbenutzeroberfläche den Dienst aus, für den Sie Protokolle anzeigen möchten (z. B. YARN), und wählen Sie dann mithilfe von **Quicklinks** den gewünschten Hauptknoten aus.

![Verwenden von Quicklinks zum Anzeigen von Protokollen](./media/hdinsight-high-availability-linux/viewlogs.png)

## Konfigurieren der Knotengröße ##

Die Größe eines Knotens kann nur beim Erstellen des Clusters ausgewählt werden. Eine Liste der verschiedenen VM-Größen, die für HDInsight verfügbar sind (einschließlich Kerne, Arbeitsspeicher und lokaler Speicher), finden Sie auf der Seite [HDInsight-Preisdetails](https://azure.microsoft.com/pricing/details/hdinsight/).

Beim Erstellen eines neuen Clusters können Sie die Größe der Knoten angeben. Die Größe kann wie folgt im [Azure-Portal][preview-portal], mit [Azure PowerShell][azure-powershell] und mit der [Azure-Befehlszeilenschnittstelle][azure-cli] festgelegt werden:

* **Azure-Portal**: Beim Erstellen eines neuen Clusters haben Sie die Möglichkeit, die Größe (Tarif) der Haupt- und Workerknoten sowie (sofern vom Clustertyp verwendet) der Zookeeper-Knoten für den Cluster festzulegen:

	![Screenshot des Assistenten zur Clustererstellung mit der Auswahl der Knotengröße](./media/hdinsight-high-availability-linux/headnodesize.png)

* **Azure-Befehlszeilenschnittstelle**: Die Größe der Haupt-, Worker- und Zookeeper-Knoten kann bei Verwendung des Befehls `azure hdinsight cluster create` mit den Parametern `--headNodeSize`, `--workerNodeSize` und `--zookeeperNodeSize` angegeben werden.

* **Azure-PowerShell**: Die Größe der Haupt-, Worker- und Zookeeper-Knoten kann bei Verwendung des Cmdlets `New-AzureRmHDInsightCluster` mit den Parametern `-HeadNodeVMSize`, `-WorkerNodeSize` und `-ZookeeperNodeSize` angegeben werden.

##Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Azure HDInsight hohe Verfügbarkeit für Hadoop bietet. In den folgenden Artikeln finden Sie weiterführende Informationen.

- [Ambari-REST-Referenz](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)

- [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)

- [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md)

- [Verwalten von HDInsight mit Ambari](hdinsight-hadoop-manage-ambari.md)

- [Bereitstellen Linux-basierter HDInsight-Cluster](hdinsight-hadoop-provision-linux-clusters.md)

[preview-portal]: https://portal.azure.com/
[azure-powershell]: ../powershell-install-configure.md
[azure-cli]: ../xplat-cli-install.md

<!---HONumber=AcomDC_0921_2016-->