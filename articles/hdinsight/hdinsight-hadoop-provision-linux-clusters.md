<properties
   	pageTitle="Erstellen von Hadoop-, HBase-, Storm oder Spark-Clustern unter Linux in HDInsight | Microsoft Azure"
   	description="Erfahren Sie, wie Hadoop-, HBase-, Storm- oder Spark-Cluster unter Linux für HDInsight mithilfe eines Browsers, der Azure-Befehlszeilenschnittstelle, Azure PowerShell, REST oder durch ein SDK erstellt werden."
   	services="hdinsight"
   	documentationCenter=""
   	authors="mumian"
   	manager="jhubbard"
   	editor="cgronlun"
	tags="azure-portal"/>

<tags
   	ms.service="hdinsight"
   	ms.devlang="na"
   	ms.topic="article"
   	ms.tgt_pltfrm="na"
   	ms.workload="big-data"
   	ms.date="09/06/2016"
   	ms.author="jgao"/>


# Erstellen von Linux-basierten Hadoop-Clustern in HDInsight

[AZURE.INCLUDE [Auswahl](../../includes/hdinsight-selector-create-clusters.md)]

Ein Hadoop-Cluster besteht aus mehreren virtuellen Computern (Knoten), die zur verteilten Verarbeitung von Aufgaben im Cluster verwendet werden. Azure abstrahiert die Implementierungsdetails der Installation und Konfiguration einzelner Knoten, sodass Sie nur allgemeine Konfigurationsinformationen bereitstellen müssen. In diesem Artikel lernen Sie diese Konfigurationseinstellungen kennen.

## Clustertypen

Azure HDInsight bietet derzeit fünf verschiedene Typen von Clustern mit einer Reihe von Komponenten, um bestimmte Funktionalitäten bereitzustellen.

| Clustertyp | Funktionalität |
| ------------ | ----------------------------- |
| Hadoop | Abfragen und Analysen (Batchaufträge) |
| HBase | NoSQL-Datenspeicher |
| Storm | Ereignisverarbeitung in Echtzeit |
| Spark (Vorschau) | Arbeitsspeicherinterne Verarbeitung, interaktive Abfragen, Microbatch-Datenstromverarbeitung |
| R Server auf Spark | Umfasst eine Vielzahl von Big Data-Statistiken, Vorhersagemodellierung und Machine Learning-Funktionen. |

Jeder Clustertyp verfügt über eine eigene Anzahl von Knoten im Cluster, Terminologie für Knoten im Cluster und eine VM-Standardgröße für jeden Knotentyp. In der folgenden Tabelle ist die Anzahl von Knoten für jeden Knotentyp jeweils in Klammern angegeben.

| Typ| Nodes | Diagramm|
|-----|------|--------|
|Hadoop| Hauptknoten (2), Datenknoten (1+)|![HDInsight-Hadoop-Clusterknoten](./media/hdinsight-provision-clusters/HDInsight.Hadoop.roles.png)|
|HBase|Hauptserver (2), Regionsserver (1+), Master-/Zookeeper-Knoten (3)|![HDInsight-HBase-Clusterknoten](./media/hdinsight-provision-clusters/HDInsight.HBase.roles.png)|
|Storm|Nimbus-Knoten (2), Supervisor-Server (1+), Zookeeper-Knoten (3)|![HDInsight-Storm-Clusterknoten](./media/hdinsight-provision-clusters/HDInsight.Storm.roles.png)|
|Spark|Hauptknoten (2), Workerknoten (1+), Zookeeper-Knoten (3) (kostenlos für Zookeeper-VM-Größe A1)|![HDInsight-Spark-Clusterknoten](./media/hdinsight-provision-clusters/HDInsight.Spark.roles.png)|


Die folgende Tabelle enthält die VM-Standardgrößen für HDInsight.

|Clustertyp|	Hadoop|	HBase|	Storm|	Spark|
|------------|--------|------|-------|-------|
|Head – Standard-VM-Größe|	D3 |A3|	A3|	D12|
|Head – empfohlene VM-Größen|	D3, D4, D12 |A3, A4, A5 |A3, A4, A5|	D12, D13, D14|
|Worker – Standard-VM-Größe|	D3|	D3|	D3|	D12|
|Worker – empfohlene VM-Größen |	D3, D4, D12|	D3, D4, D12 |D3, D4, D12|	D12, D13, D14|
|Zookeeper – Standard-VM-Größe| |	A2|	A2 | |
|Zookeeper – empfohlene VM-Größen | |A2, A3, A4 |A2, A3, A4 | |

Beachten Sie, dass Head für den Storm-Clustertyp als *Nimbus* bezeichnet wird. „Worker“ wird für den HBase-Clustertyp als *Region* und für den Storm-Clustertyp als *Supervisor* bezeichnet.



> [AZURE.IMPORTANT] Wenn Sie mehr als 32 Workerknoten planen – entweder bei Erstellung des Clusters oder durch Skalierung des Clusters nach der Erstellung – müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB RAM auswählen.

Sie können zu diesen grundlegenden Typen mithilfe von [Skriptaktionen](#customize-clusters-using-script-action) weitere Komponenten wie Hue oder R hinzufügen.

## Cluster-Ebenen

Azure HDInsight bietet die Cloudlösungen für Big Data in zwei Kategorien an: Standard und [Premium](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium). HDInsight Premium enthält R und andere zusätzliche Komponenten. HDInsight Premium wird nur von HDInsight-Version 3.4 unterstützt.

Die folgende Tabelle listet den HDInsight-Clustertyp und die Supportmatrix für HDInsight Premium auf.

| Clustertyp | Standard | Premium |
|--------------|---------------|--------------|
| Hadoop | Ja | Ja |
| Spark | Ja | Ja |
| HBase | Ja | Nein |
| Storm | Ja | Nein |
| R Server auf Spark | Nein | Ja |

Diese Tabelle wird aktualisiert, sobald weitere Clustertypen in HDInsight Premium hinzugefügt werden. Der folgende Screenshot zeigt die Azure-Portal-Informationen zum Auswählen von Clustertypen.

![Konfiguration von HDInsight Premium](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)


## Grundlegende Konfigurationsoptionen

Unten sind die grundlegenden Konfigurationsoptionen zum Erstellen eines HDInsight-Clusters angegeben.

### Clustername ###

Der Clustername wird verwendet, um einen Cluster zu identifizieren. Der Clustername muss global eindeutig sein und den folgenden Benennungsrichtlinien entsprechen:

- Das Feld muss eine Zeichenfolge mit 3 bis 63 Zeichen enthalten.
- Das Feld darf nur Buchstaben, Zahlen und Bindestriche enthalten.

### Clustertyp###

Informationen hierzu finden Sie unter [Clustertypen](#cluster-types) und [Cluster-Ebenen](#cluster-tiers).

### Betriebssystem ###

Sie können HDInsight-Cluster unter einem der beiden folgenden Betriebssysteme erstellen:

- HDInsight unter Linux. HDInsight bietet die Möglichkeit, Linux-Cluster unter Azure zu konfigurieren. Konfigurieren Sie einen Linux-Cluster, wenn Sie mit Linux oder Unix und der Migration von einer vorhandenen Linux-basierten Hadoop-Lösung vertraut sind, oder Sie eine einfache Integration mit Komponenten des Hadoop-Systems wünschen, die für Linux konzipiert sind. Weitere Informationen finden Sie unter [Erste Schritte mit Hadoop unter Linux in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
- HDInsight unter Windows (Windows Server 2012 R2 Datacenter)

### HDInsight-Version###

Dient zum Ermitteln der Version von HDInsight, die für diesen Cluster benötigt wird. Weitere Informationen finden Sie unter [Hadoop-Clusterversionen und -komponenten in HDInsight](https://go.microsoft.com/fwLink/?LinkID=320896&clcid=0x409).

### Abonnementname###

Jeder HDInsight-Cluster ist mit einem Azure-Abonnement verknüpft.

### Ressourcengruppenname ###

Mit [Azure Resource Manager](../resource-group-overview.md) können Sie mit den Ressourcen in Ihrer Anwendung als Gruppe arbeiten, was als Azure-Ressourcengruppe bezeichnet wird. Sie können alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereitstellen, aktualisieren, überwachen oder löschen.

### Anmeldeinformationen###

Während der Clustererstellung ermöglichen die HDInsight-Cluster Ihnen das Konfigurieren von zwei Benutzerkonten:

- HTTP-Benutzer: Der Standardbenutzername bei der grundlegenden Konfiguration im Azure-Portal ist *admin*. Er wird auch als „Clusterbenutzer“ bezeichnet.
- SSH-Benutzer (Linux-Cluster): Wird verwendet, um die Verbindung mit dem Cluster per SSH herzustellen. Sie können zusätzliche SSH-Benutzerkonten erstellen, nachdem der Cluster erstellt wurde, indem Sie die Schritte unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md) oder [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-unix.md) ausführen.

    >[AZURE.NOTE] Für Windows-basierte Cluster können Sie einen RDP-Benutzer erstellen, um eine Verbindung mit dem Cluster per RDP herzustellen.

### Datenquelle###

Im ursprünglichen Hadoop Distributed File System (HDFS) werden viele lokale Datenträger im Cluster verwendet. HDInsight verwendet zur Datenspeicherung Azure Blob-Speicher. Azure-Blobspeicher stellt eine robuste, universelle Speicherlösung dar, die problemlos mit HDInsight integriert werden kann. Über eine HDFS-Schnittstelle können sämtliche Komponenten in HDInsight direkt mit strukturierten oder unstrukturierten Daten im Blobspeicher arbeiten. Die Speicherung von Daten im Blobspeicher sorgt dafür, dass die HDInsight-Cluster, die für Berechnungen verwendet werden, sicher gelöscht werden können, ohne Benutzerdaten zu verlieren.

Bei der Konfiguration müssen Sie ein Azure-Speicherkonto und einen Azure-Blobspeichercontainer für dieses Konto angeben. Bei einigen Erstellungsprozessen müssen das Azure-Speicherkonto und der Blobspeichercontainer vorher erstellt werden. Der Blobspeichercontainer wird vom Cluster als Standardspeicherort verwendet. Optional können Sie zusätzliche Azure-Speicherkonten (verknüpften Speicher) angeben, auf die der Cluster zugreifen kann. Darüber hinaus kann der Cluster auch auf alle Blobspeichercontainer zugreifen, die mit vollständig öffentlichem Lesezugriff oder mit öffentlichem Lesezugriff nur für Blobs konfiguriert sind. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Azure-Speicherressourcen](../storage/storage-manage-access-to-resources.md).

![HDInsight-Speicher](./media/hdinsight-provision-clusters/HDInsight.storage.png)

>[AZURE.NOTE] Ein Blobspeichercontainer stellt eine Gruppierung von Blobs bereit. Dies ist in der folgenden Abbildung dargestellt:

![Azure Blob Storage](./media/hdinsight-provision-clusters/Azure.blob.storage.jpg)

Es wird nicht empfohlen, den Standard-Blobspeichercontainer zum Speichern von Geschäftsdaten zu verwenden. Stattdessen empfiehlt es sich, den Standard-Blobspeichercontainer nach jeder Verwendung zu löschen, um die Speicherkosten zu verringern. Beachten Sie, dass der Standardcontainer Anwendungs- und Systemprotokolle enthält. Stellen Sie sicher, dass Sie die Protokolle abrufen, bevor Sie den Container löschen.

>[AZURE.WARNING] Das Freigeben eines Blobspeichercontainers für mehrere Cluster wird nicht unterstützt.

Weitere Informationen zu sekundärem Blobspeicher finden Sie unter [Verwenden von Azure-Blobspeicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md).

Zusätzlich zu Azure-Blobspeicher können Sie auch [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) als Standardspeicherkonto für HBase-Cluster in HDInsight und als verknüpften Speicher für alle vier Typen von HDInsight-Clustern verwenden. Weitere Informationen hierzu finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### Standort (Region) ###

Der HDInsight-Cluster und das zugeordnete Standardspeicherkonto müssen sich an demselben Azure-Standort befinden.

![Azure-Regionen](./media/hdinsight-provision-clusters/Azure.regions.png)

Um eine Liste der unterstützten Regionen zu erhalten, klicken Sie in der Dropdownliste **Region** auf [HDInsight-Preise](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

### Knotentarife###

Kunden wird die Nutzung dieser Knoten für die Laufzeit des Clusters in Rechnung gestellt. Die Abrechnung beginnt, sobald ein Cluster erstellt wurde, und sie endet, wenn der Cluster gelöscht wird. Bei Clustern ist kein Aufheben der Zuweisung oder ein Anhalten möglich.

Unterschiedliche Clustertypen weisen verschiedene Knotentypen, eine unterschiedliche Anzahl von Knoten sowie verschiedene Knotengrößen auf. Ein Hadoop-Cluster beispielsweise besitzt zwei _Hauptknoten_ sowie standardmäßig vier _Datenknoten_. Ein Storm-Cluster dagegen weist zwei _Nimbusknoten_, drei _Zookeeperknoten_ und standardmäßig vier _Supervisorknoten_ auf. Die Kosten von HDInsight-Clustern ergeben sich aus der Anzahl der Knoten und aus der Größe der virtuellen Computer für die Knoten. Wenn Sie z. B. wissen, dass Sie Vorgänge mit hohem Arbeitsspeicherbedarf ausführen werden, sollten Sie eine Computeressource mit großem Arbeitsspeicher auswählen. Zu Lernzwecken wird empfohlen, einen Datenknoten zu verwenden. Weitere Informationen zu den Preisen von HDInsight finden Sie unter [HDInsight – Preise](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

>[AZURE.NOTE] Die Begrenzung der Clustergröße variiert je nach Azure-Abonnement. Wenden Sie sich an den Azure-Abrechnungssupport, um diese Begrenzung zu erhöhen.

>Die von Ihrem Cluster verwendeten Knoten zählen nicht als virtuelle Computer, da die VM-Images, die für die Knoten verwendet werden, ein Implementierungsdetail des HDInsight-Diensts sind. Die von den Knoten verwendeten Computekerne zählen jedoch zur Gesamtanzahl von Computekernen, die für Ihr Abonnement verfügbar sind. Sie können die Anzahl der verfügbaren Kerne und die Kerne, die vom Cluster verwendet werden sollen, beim Erstellen eines HDInsight-Clusters auf dem Blatt „Knotentarife“ im Abschnitt „Zusammenfassung“ anzeigen.

Wenn Sie das Azure-Portal zum Konfigurieren des Clusters verwenden, ist die Knotengröße über das Blatt __Knotentarif__ verfügbar. Außerdem werden die Kosten angezeigt, die den unterschiedlichen Knotengrößen zugeordnet sind. Der folgende Screenshot zeigt die Optionen für einen Linux-basierten Hadoop-Cluster:

![VM-Größen für HDInsight-Knoten](./media/hdinsight-provision-clusters/hdinsight.node.sizes.png)

In den folgenden Tabellen sind die von HDInsight-Clustern unterstützten Größen und die von den einzelnen Größen bereitgestellten Kapazitäten aufgeführt.

#### Standard-Ebene: A-Serie####

Im klassischen Bereitstellungsmodell unterscheiden sich einige VM-Größen in PowerShell und CLI.
* Standard\_A3 ist „Groß“
* Standard\_A4 ist „Extragroß“

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)|
|---|---|---|---|---|---|---|
|Standard\_A3\\Groß|4|7 GB|2|Temporär = 285 GB |8|8 x 500|
|Standard\_A4\\ExtraGroß|8|14 GB|4|Temporär = 605 GB |16|16 x 500|
|Standard\_A6|4|28 GB|2|Temporär = 285 GB |8|8 x 500|
|Standard\_A7|8|56 GB|4|Temporär = 605 GB |16|16 x 500|


#### Standard-Ebene: D-Serie####

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)|
|---|---|---|---|---|---|---|
|Standard\_D3 |4|14 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D4 |8|28 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D12 |4|28 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D13 |8|56 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D14 |16|112 GB|8|Temporär (SSD) = 800 GB |32|32 x 500|

#### Standard-Ebene: Dv2-Serie####

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)|
|---|---|---|---|---|---|---|
|Standard\_D3\_v2 |4|14 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D4\_v2 |8|28 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D12\_v2 |4|28 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D13\_v2 |8|56 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D14\_v2 |16|112 GB|8|Temporär (SSD) = 800 GB |32|32 x 500|    

Überlegungen zur Bereitstellung, die Sie im Hinblick auf die Verwendung dieser Ressourcen berücksichtigen sollten, finden Sie unter [Größen für virtuelle Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md). Informationen zu den Preisen der unterschiedlichen Größen finden Sie unter [HDInsight-Preise](https://azure.microsoft.com/pricing/details/hdinsight).

> [AZURE.IMPORTANT] Wenn Sie mehr als 32 Workerknoten planen – entweder bei Erstellung des Clusters oder durch Skalierung des Clusters nach der Erstellung – müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB RAM auswählen.

Die Abrechnung beginnt, sobald ein Cluster erstellt wurde, und sie endet, wenn der Cluster gelöscht wird. Weitere Informationen zu den Preisen finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).


## Verwenden von zusätzlichem Speicher

In manchen Fällen ist es wünschenswert, dem Cluster zusätzlichen Speicher hinzufügen zu können. Dies kann beispielsweise dann der Fall sein, wenn Sie über mehrere Azure-Speicherkonten in verschiedenen geografischen Regionen oder für verschiedene Dienste verfügen und all diese Konten mit HDInsight analysieren möchten.

Sie können Speicherkonten hinzufügen, wenn Sie einen HDInsight-Cluster erstellen oder nachdem die Erstellung des Clusters abgeschlossen ist. Weitere Informationen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

Weitere Informationen zu sekundären Blobspeichern finden Sie unter [Verwenden von Azure-Blobspeicher mit HDInsight](hdinsight-hadoop-use-blob-storage.md). Weitere Informationen zur Verwendung von sekundärem Data Lake-Speicher finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).


## Verwenden des Hive/Oozie-Metastores

Es wird dringend empfohlen, einen benutzerdefinierten Metastore zu verwenden, wenn Sie nach dem Löschen des HDInsight-Clusters Ihre Hive-Tabellen beibehalten möchten. Sie können diesen Metastore an einen anderen HDInsight-Cluster anfügen.

> [AZURE.IMPORTANT] Der HDInsight-Metastore ist nicht abwärtskompatibel. Sie können beispielsweise nicht einen Metastore eines HDInsight 3.4-Clusters zum Erstellen eines HDInsight 3.3-Clusters verwenden.

Metastore enthält Hive- und Oozie-Metadaten, wie z. B. Hive-Tabellen, Partitionen, Schemata und Spalten. Mit dem Metastore können Sie Ihre Hive- und Oozie-Metadaten beibehalten, damit Sie Hive-Tabellen oder Oozie-Aufträge nicht neu erstellen müssen, wenn Sie einen neuen Cluster erstellen. Standardmäßig speichert Hive diese Informationen in einer eingebetteten Azure SQL-Datenbank. Die eingebettete Datenbank kann die Metadaten nicht beibehalten, wenn der Cluster gelöscht wird. Wenn Sie eine Hive-Tabelle in einem HDInsight-Cluster mit einem konfigurierten Hive-Metastore erstellen, wird die Tabelle beibehalten, falls Sie den Cluster mit demselben Hive-Metastore erneut erstellen.

Für HBase-Clustertypen ist keine Metastore-Konfiguration verfügbar.

> [AZURE.IMPORTANT] Verwenden Sie beim Erstellen eines benutzerdefinierten Metastores keinen Datenbanknamen, der Gedankenstriche oder Bindestriche enthält. Dies kann dazu führen, dass der Clustererstellungsprozess fehlschlägt.

## Verwenden virtueller Azure-Netzwerke

Mit einem [virtuellen Azure-Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) können Sie ein sicheres, persistentes Netzwerk mit allen Ressourcen erstellen, die Sie für Ihre Lösung benötigen. Mit einem virtuellen Netzwerk haben Sie folgende Möglichkeiten:

* Verbinden von Cloudressourcen in einem privaten Netzwerk (nur in der Cloud)

	![Diagramm der Nur-Cloud-Konfiguration](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-cloud-only.png)

* Verbinden Ihrer Cloudressourcen mit dem Netzwerk in Ihrem lokalen Datencenter (Standort-zu-Standort oder Punkt-zu-Standort) mithilfe eines virtuellen privaten Netzwerks (VPN)

| Standort-zu-Standort-Konfiguration | Point-to-Site-Konfiguration |
| -------------------------- | --------------------------- |
| Mit einer Standort-zu-Standort-Konfiguration können Sie mehrere Ressourcen aus Ihrem Datencenter mit dem virtuellen Azure-Netzwerk über ein Hardware-VPN oder den Routing- und RAS-Dienst verbinden.<br />![Diagramm der Standort-zu-Standort-Konfiguration](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-site-to-site.png) | Bei der Point-to-Site-Konfiguration können Sie eine bestimmte Ressource über ein Software-VPN mit dem virtuellen Azure-Netzwerk verbinden.<br />![Diagramm der Punkt-zu-Standort-Konfiguration](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-point-to-site.png) |

Für Windows-basierte Cluster ist ein virtuelles Netzwerk der Version 1 (klassisch) erforderlich, für Linux-basierte Cluster dagegen ein virtuelles Netzwerk der Version 2 (Azure Resource Manager). Wenn nicht der richtige Netzwerktyp vorhanden ist, kann das Netzwerk nicht zum Erstellen des Clusters verwendet werden.

Weitere Informationen zur Verwendung von HDInsight mit einem virtuellen Netzwerk, einschließlich spezifischer Konfigurationsanforderungen für das virtuelle Netzwerk, finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

## Anpassen von Clustern mithilfe der HDInsight-Clusteranpassung (Bootstrap)

Es kann vorkommen, dass Sie die folgenden Konfigurationsdateien bearbeiten möchten:

- clusterIdentity.xml
- core-site.xml
- gateway.xml
- hbase-env.xml
- hbase-site.xml
- hdfs-site.xml
- hive-env.xml
- hive-site.xml
- mapred-site
- oozie-site.xml
- oozie-env.xml
- storm-site.xml
- tez-site.xml
- webhcat-site.xml
- yarn-site.xml

Um die Änderungen für die Laufzeit des Clusters beizubehalten, können Sie während des Erstellungsprozesses die HDInsight-Clusteranpassung nutzen. Alternativ dazu können Sie Ambari in Linux-basierten Clustern verwenden. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).

>[AZURE.NOTE] Die Windows-basierten Cluster können die Änderungen aufgrund des Re-Imagings nicht beibehalten. Weitere Informationen finden Sie unter [Role Instance Restarts Due to OS Upgrades](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx) (Neustart von Rolleninstanzen aufgrund von Betriebssystemupdates – in englischer Sprache). Um die Änderungen für die Laufzeit des Clusters beizubehalten, müssen Sie während des Erstellungsprozesses die HDInsight-Clusteranpassung nutzen.

## Anpassen von Clustern mithilfe von Skriptaktionen

Sie können zusätzliche Komponenten installieren oder die Clusterkonfiguration mithilfe von Skripts während der Erstellung anpassen. Diese Skripts werden mithilfe der Konfigurationsoption **Skriptaktion** aufgerufen, die vom Azure-Verwaltungsportal, von HDInsight Windows PowerShell-Cmdlets oder dem HDInsight .NET SDK verwendet werden kann. Weitere Informationen finden Sie unter [Anpassen eines HDInsight-Clusters mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

Einige systemeigene Java-Komponenten wie Mahout und Cascading können auf dem Cluster als JAR-Dateien (Java Archive) ausgeführt werden. Diese JAR-Dateien können an Azure-Blobspeicher verteilt und mit den Verfahren zur Übermittlung von Hadoop-Aufträgen an HDInsight-Cluster gesendet werden. Weitere Informationen finden Sie unter [Programmgesteuerte Übermittlung von Hadoop-Aufträgen](hdinsight-submit-hadoop-jobs-programmatically.md).

>[AZURE.NOTE] Wenn bei der Bereitstellung von JAR-Dateien für HDInsight-Cluster oder beim Aufrufen von JAR-Dateien für HDInsight-Cluster Probleme auftreten, wenden Sie sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/).

> Cascading wird von HDInsight nicht unterstützt, und es steht kein Microsoft-Support dafür zur Verfügung. Listen der unterstützten Komponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Clusterversionen](hdinsight-component-versioning.md).

## Verwenden des Edgeknotens

 Ein leerer Edgeknoten ist ein virtueller Linux-Computer, auf dem die gleichen Clienttools installiert und konfiguriert sind wie im Hauptknoten. Sie können den Edgeknoten zum Zugreifen auf den Cluster, Testen Ihrer Clientanwendungen und Hosten von Clientanwendungen verwenden. Weitere Informationen finden Sie unter [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) (Verwenden leerer Edgeknoten in HDInsight).
 
## Methoden zur Clustererstellung

In diesem Artikel haben Sie grundlegende Informationen zum Erstellen eines Linux-basierten HDInsight-Clusters erhalten. In der folgenden Tabelle finden Sie spezifische Informationen zum Erstellen von Clustern mit der Methode, die Ihren Anforderungen am besten entspricht:

| Verfahren zur Clustererstellung | Webbrowser | Befehlszeile | REST-API | SDK | Linux, Mac OS X oder Unix | Windows |
| ------------------------------- |:----------------------:|:--------------------:|:------------------:|:------------:|:-----------------------------:|:------------:|
| [Azure-Portal](hdinsight-hadoop-create-linux-clusters-portal.md) | ✔ | &nbsp; | &nbsp; | &nbsp; | ✔ | ✔ |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) | ✔ | ✔ | ✔ |✔ | ✔ | ✔ |
| [Azure-Befehlszeilenschnittstelle](hdinsight-hadoop-create-linux-clusters-azure-cli.md) | &nbsp; | ✔ | &nbsp; | &nbsp; | ✔ | ✔ |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) | &nbsp; | ✔ | &nbsp; | &nbsp; | ✔ | ✔ |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) | &nbsp; | ✔ | ✔ | &nbsp; | ✔ | ✔ |
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) | &nbsp; | &nbsp; | &nbsp; | ✔ | ✔ | ✔ |
| [Azure Resource Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md) | &nbsp; | ✔ | &nbsp; | &nbsp; | ✔ | ✔ |

<!---HONumber=AcomDC_0914_2016-->