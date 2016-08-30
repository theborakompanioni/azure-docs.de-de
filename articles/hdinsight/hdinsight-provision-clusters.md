<properties
   pageTitle="Erstellen Windows-basierter Hadoop-Cluster in HDInsight | Microsoft Azure"
   	description="Erfahren Sie, wie Sie Cluster für Azure HDInsight erstellen."
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/08/2016"
   ms.author="jgao"/>

# Erstellen Windows-basierter Hadoop-Cluster in HDInsight

[AZURE.INCLUDE [Auswahl](../../includes/hdinsight-selector-create-clusters.md)]

Ein Hadoop-Cluster besteht aus mehreren virtuellen Computern (Knoten), die zur verteilten Verarbeitung von Aufgaben im Cluster verwendet werden. Azure abstrahiert die Implementierungsdetails der Installation und Konfiguration einzelner Knoten, sodass Sie lediglich allgemeine Konfigurationsinformationen bereitstellen müssen. In diesem Artikel lernen Sie diese Konfigurationseinstellungen kennen.

>[AZURE.NOTE] Die Informationen in diesem Artikel gelten für Windows-basierte Azure HDInsight-Cluster. Informationen zu Linux-basierten Clustern finden Sie unter [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).

## Clustertypen##

HDInsight bietet derzeit vier verschiedene Typen von Clustern mit einer Reihe von Komponenten, um bestimmte Funktionalitäten bereitzustellen.

| Clustertyp | Funktionalität |
| ------------ | ----------------------------- |
| Hadoop | Abfragen und Analysen (Batchaufträge) |
| HBase | NoSQL-Datenspeicher |
| Storm | Ereignisverarbeitung in Echtzeit |
| Spark (Vorschau) | Arbeitsspeicherinterne Verarbeitung, interaktive Abfragen, Microbatch-Datenstromverarbeitung |

Jeder Clustertyp verfügt über eine eigene Anzahl von Knoten, Terminologie für Knoten im Cluster und eine VM-Standardgröße für jeden Knotentyp. In der folgenden Tabelle ist die Anzahl von Knoten für jeden Knotentyp jeweils in Klammern angegeben.

| Typ| Knoten (Anzahl von Knoten)| Diagramm|
|-----|------|--------|
|Hadoop| Hauptknoten (2), Datenknoten (1+)|![HDInsight-Hadoop-Clusterknoten](./media/hdinsight-provision-clusters/HDInsight.Hadoop.roles.png)|
|HBase|Hauptserver (2), Regionsserver (1+), Master-/Zookeeper-Knoten (3)|![HDInsight-HBase-Clusterknoten](./media/hdinsight-provision-clusters/HDInsight.HBase.roles.png)|
|Storm|Nimbus-Knoten (2), Supervisor-Server (1+), Zookeeper-Knoten (3)|![HDInsight-Storm-Clusterknoten](./media/hdinsight-provision-clusters/HDInsight.Storm.roles.png)|
|Spark|Hauptknoten (2), Workerknoten (1+), Zookeeper-Knoten (3) (kostenlos für Zookeeper-VM-Größe A1)|![HDInsight-Spark-Clusterknoten](./media/hdinsight-provision-clusters/HDInsight.Spark.roles.png)|

> [AZURE.IMPORTANT] Wenn Sie mehr als 32 Workerknoten planen – entweder bei Erstellung des Clusters oder durch Skalierung des Clusters nach der Erstellung – müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB RAM auswählen.

Sie können zu diesen grundlegenden Typen mithilfe von [Skriptaktionen](#customize-clusters-using-script-action) weitere Komponenten wie Hue oder R hinzufügen.

## Grundlegende Konfigurationsoptionen

Nachfolgend werden die grundlegenden Konfigurationsoptionen aufgeführt, die zum Erstellen eines HDInsight-Clusters erforderlich sind.

### Clustername###

Der Clustername wird verwendet, um einen Cluster zu identifizieren. Der Clustername muss global eindeutig sein und diesen Benennungsrichtlinien entsprechen:

- Das Feld muss eine Zeichenfolge mit 3 bis 63 Zeichen enthalten.
- Das Feld darf nur Buchstaben, Zahlen und Bindestriche enthalten.

### Clustertyp ###

Siehe [Clustertypen](#cluster-types).

### Betriebssystem ###

Sie können HDInsight-Cluster unter einem der beiden folgenden Betriebssysteme erstellen:

- HDInsight unter Linux. HDInsight bietet die Möglichkeit, Linux-Cluster unter Azure zu konfigurieren. Konfigurieren Sie einen Linux-Cluster, wenn Sie mit Linux oder Unix vertraut sind, eine Migration von einer vorhandenen Linux-basierten Hadoop-Lösung durchführen oder eine einfache Integration mit Komponenten des Hadoop-Systems wünschen, die für Linux konzipiert sind. Weitere Informationen finden Sie unter [Erste Schritte mit Hadoop unter Linux in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

- HDInsight unter Windows (Windows Server 2012 R2 Datacenter)

### HDInsight-Version###

Die HDInsight-Version wird verwendet, um die für diesen Cluster zu verwendende HDInsight-Version zu bestimmen. Weitere Informationen finden Sie unter [Hadoop-Clusterversionen und -komponenten in HDInsight](https://go.microsoft.com/fwLink/?LinkID=320896&clcid=0x409).

### Abonnementname###

Jeder HDInsight-Cluster ist mit einem Azure-Abonnement verknüpft.

### Ressourcengruppenname###

Mit [Azure Resource Manager](../resource-group-overview.md) können Sie die Ressourcen für Ihre Anwendung bereitstellen, aktualisieren, überwachen oder löschen.

### Anmeldeinformationen

Während der Clustererstellung ermöglichen die HDInsight-Cluster Ihnen das Konfigurieren von drei Benutzerkonten.

- Mit [Azure Resource Manager](../resource-group-overview.md) können Sie mit den Ressourcen in Ihrer Anwendung als Gruppe arbeiten, bezeichnet als „Azure-Ressourcengruppe“. Sie können alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereitstellen, aktualisieren, überwachen oder löschen.

- HTTP-Benutzer: Der Standardbenutzername bei der grundlegenden Konfiguration im Azure-Portal ist *admin*. Dieser wird gelegentlich aus als „Clusterbenutzer“ bezeichnet.
- RDP-Benutzer (Windows-Cluster). Die Verbindungsherstellung mit dem Cluster erfolgt über RDP. Wenn Sie das Konto erstellen, müssen Sie ein Ablaufdatum festlegen, das maximal 90 Tage nach dem aktuellen Datum liegt.
- SSH-Benutzer (Linux-Cluster). Die Verbindungsherstellung mit dem Cluster erfolgt über SSH. Sie können zusätzliche SSH-Benutzerkonten erstellen, nachdem der Cluster erstellt wurde, indem Sie die Schritte unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md) ausführen.

### Datenquelle ###

Im ursprünglichen Hadoop Distributed File System (HDFS) werden viele lokale Datenträger im Cluster verwendet. HDInsight verwendet zur Datenspeicherung Azure Blob-Speicher. Azure-Blobspeicher stellt eine robuste, universelle Speicherlösung dar, die problemlos mit HDInsight integriert werden kann. Über eine HDFS-Schnittstelle können sämtliche Komponenten in HDInsight direkt mit strukturierten oder unstrukturierten Daten im Blobspeicher arbeiten. Wenn Sie Daten im Blobspeicher ablegen, können Sie die für Berechnungen verwendeten HDInsight-Cluster gefahrlos löschen, ohne Benutzerdaten zu verlieren.

Bei der Konfiguration müssen Sie ein Azure-Speicherkonto und einen Azure-Blobspeichercontainer für dieses Konto angeben. Bei einigen Erstellungsprozessen müssen das Azure-Speicherkonto und der Blobspeichercontainer vorher erstellt werden. Der Blobspeichercontainer wird vom Cluster als Standardspeicherort verwendet. Optional können Sie zusätzliche Azure-Speicherkonten (verknüpften Speicher) angeben, auf die der Cluster zugreifen kann. Darüber hinaus kann der Cluster auch auf alle Blobspeichercontainer zugreifen, die mit vollständig öffentlichem Lesezugriff oder mit öffentlichem Lesezugriff nur für Blobs konfiguriert sind. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Azure-Speicherressourcen](../storage/storage-manage-access-to-resources.md).

![HDInsight-Speicher](./media/hdinsight-provision-clusters/HDInsight.storage.png)

>[AZURE.NOTE] Ein Blobspeichercontainer stellt eine Gruppierung von Blobs bereit. Dies ist in der folgenden Abbildung dargestellt:

Bei der Konfiguration müssen Sie ein Azure-Speicherkonto und einen Azure-Blobspeichercontainer für dieses Konto angeben. Bei einigen Erstellungsprozessen müssen das Azure-Speicherkonto und der Blobspeichercontainer vorher erstellt werden. Der Blobspeichercontainer wird vom Cluster als Standardspeicherort verwendet. Optional können Sie zusätzliche Azure-Speicherkonten (verknüpften Speicher) angeben, auf die der Cluster zugreifen kann. Der Cluster kann auch auf alle Blobcontainer zugreifen, die mit vollständig öffentlichem Lesezugriff oder mit öffentlichem Lesezugriff nur für Blobs konfiguriert sind. Weitere Informationen finden Sie unter [Verwalten des Zugriffs auf Azure-Speicherressourcen](../storage/storage-manage-access-to-resources.md).


![Azure Blob Storage](./media/hdinsight-provision-clusters/Azure.blob.storage.jpg)

Es wird nicht empfohlen, den Standard-Blobspeichercontainer zum Speichern von Geschäftsdaten zu verwenden. Stattdessen empfiehlt es sich, den Standard-Blobspeichercontainer nach jeder Verwendung zu löschen, um die Speicherkosten zu verringern. Der Standardcontainer enthält Anwendungs- und Systemprotokolle. Stellen Sie sicher, dass Sie die Protokolle abrufen, bevor Sie den Container löschen.

>[AZURE.WARNING] HDInsight bietet keine Unterstützung für das Freigeben eines Blobspeichercontainers für mehrere Cluster.

Weitere Informationen zu sekundären Blobspeichern finden Sie unter [Verwenden des HDFS-kompatiblen Azure-Blobspeichers mit Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md).

Zusätzlich zu Azure-Blobspeicher können Sie auch [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) als Standardspeicherkonto für einen HBase-Cluster in HDInsight und als verknüpften Speicher für alle vier Typen von HDInsight-Clustern verwenden. Weitere Informationen hierzu finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

### Standort (Region)###

Der HDInsight-Cluster und das zugeordnete Standardspeicherkonto müssen sich an demselben Azure-Standort befinden.

![Azure-Regionen](./media/hdinsight-provision-clusters/Azure.regions.png)

Um eine Liste der unterstützten Regionen zu erhalten, klicken Sie in der Dropdownliste **Region** auf [HDInsight-Preise](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

### Knotentarife###

Kunden wird die Nutzung dieser Knoten für die Laufzeit des Clusters in Rechnung gestellt. Die Abrechnung beginnt, sobald ein Cluster erstellt wurde, und sie endet, wenn der Cluster gelöscht wird. Bei Clustern ist kein Aufheben der Zuweisung oder ein Anhalten möglich.

Unterschiedliche Clustertypen weisen verschiedene Knotentypen, eine unterschiedliche Anzahl von Knoten sowie verschiedene Knotengrößen auf. Ein Hadoop-Cluster beispielsweise besitzt zwei _Hauptknoten_ sowie standardmäßig vier _Datenknoten_. Ein Storm-Cluster dagegen weist zwei _Nimbusknoten_, drei _Zookeeperknoten_ und standardmäßig vier _Supervisorknoten_ auf. Die Kosten von HDInsight-Clustern ergeben sich aus der Anzahl von Knoten und aus der Größe der virtuellen Computer für die Knoten. Wenn Sie z. B. wissen, dass Sie Vorgänge mit hohem Arbeitsspeicherbedarf ausführen werden, sollten Sie eine Computeressource mit großem Arbeitsspeicher auswählen. Zu Lernzwecken wird das Arbeiten mit einem Datenknoten empfohlen. Weitere Informationen zu den Preisen von HDInsight finden Sie unter [HDInsight – Preise](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

>[AZURE.NOTE] Die Begrenzung der Clustergröße variiert je nach Azure-Abonnement. Wenden Sie sich an den Azure-Abrechnungssupport, um diese Begrenzung zu erhöhen.

>Die von Ihrem Cluster verwendeten Knoten zählen nicht als virtuelle Computer, da die VM-Images, die für die Knoten verwendet werden, ein Implementierungsdetail des HDInsight-Diensts sind. Die von den Knoten verwendeten Computekerne zählen jedoch zur Gesamtanzahl von Computekernen, die für Ihr Abonnement verfügbar sind. Sie können die Anzahl verfügbarer Kerne und die vom Cluster verwendeten Kerne beim Erstellen eines HDInsight-Clusters auf dem Blatt „Knotentarife“ im Abschnitt „Zusammenfassung“ anzeigen.

Wenn Sie den Cluster mit dem Azure-Portal konfigurieren, ist die Knotengröße über das Blatt __Knotentarif__ verfügbar. Außerdem werden die Kosten angezeigt, die den unterschiedlichen Knotengrößen zugeordnet sind. Der folgende Screenshot zeigt die Optionen für einen Linux-basierten Hadoop-Cluster.

![VM-Größen für HDInsight-Knoten](./media/hdinsight-provision-clusters/hdinsight.node.sizes.png)

In den folgenden Tabellen sind die von HDInsight-Clustern unterstützten Größen und die von den einzelnen Größen bereitgestellten Kapazitäten aufgeführt.

### Standard-Ebene: A-Serie###

Im klassischen Bereitstellungsmodell unterscheiden sich einige VM-Größen in PowerShell und CLI.

* Standard\_A3 ist „Groß“
* Standard\_A4 ist „Extragroß“

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)|
|---|---|---|---|---|---|---|
|Standard\_A3\\Groß|4|7 GB|2|Temporär = 285 GB |8|8 x 500|
|Standard\_A4\\ExtraGroß|8|14 GB|4|Temporär = 605 GB |16|16 x 500|
|Standard\_A6|4|28 GB|2|Temporär = 285 GB |8|8 x 500|
|Standard\_A7|8|56 GB|4|Temporär = 605 GB |16|16 x 500|


### Standard-Ebene: D-Serie###

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)|
|---|---|---|---|---|---|---|
|Standard\_D3 |4|14 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D4 |8|28 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D12 |4|28 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D13 |8|56 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D14 |16|112 GB|8|Temporär (SSD) = 800 GB |32|32 x 500|


### Standard-Ebene: Dv2-Serie###

|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)|
|---|---|---|---|---|---|---|
|Standard\_D3\_v2 |4|14 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D4\_v2 |8|28 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D12\_v2 |4|28 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D13\_v2 |8|56 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D14\_v2 |16|112 GB|8|Temporär (SSD) = 800 GB |32|32 x 500|     

Überlegungen zur Bereitstellung, die Sie im Hinblick auf die Verwendung dieser Ressourcen berücksichtigen sollten, finden Sie unter [Größen für virtuelle Computer](../virtual-machines/virtual-machines-windows-sizes.md). Informationen zu den Preisen der unterschiedlichen Größen finden Sie unter [HDInsight-Preise](https://azure.microsoft.com/pricing/details/hdinsight).

> [AZURE.IMPORTANT] Wenn Sie mehr als 32 Workerknoten planen – entweder bei Erstellung des Clusters oder durch Skalierung des Clusters nach der Erstellung – müssen Sie eine Hauptknotengröße mit mindestens 8 Kernen und 14 GB RAM auswählen.

Die Abrechnung beginnt, sobald ein Cluster erstellt wurde, und sie endet, wenn der Cluster gelöscht wird. Weitere Informationen zu den Preisen finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).


|Größe |CPU-Kerne|Arbeitsspeicher|Netzwerkkarten (max.)|Maximale Datenträgergröße|Max. Datenträger (jeweils 1023 GB)|Max. IOPS (500 pro Datenträger)|
|---|---|---|---|---|---|---|
|Standard\_D3\_v2 |4|14 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D4\_v2 |8|28 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D12\_v2 |4|28 GB|4|Temporär (SSD) = 200 GB |8|8 x 500|
|Standard\_D13\_v2 |8|56 GB|8|Temporär (SSD) = 400 GB |16|16 x 500|
|Standard\_D14\_v2 |16|112 GB|8|Temporär (SSD) = 800 GB |32|32 x 500|    

Überlegungen zur Bereitstellung, die Sie im Hinblick auf die Verwendung dieser Ressourcen berücksichtigen sollten, finden Sie unter [Größen für virtuelle Computer](../virtual-machines/virtual-machines-windows-sizes.md). Informationen zu den Preisen der unterschiedlichen Größen finden Sie unter [HDInsight-Preise](https://azure.microsoft.com/pricing/details/hdinsight).

> [AZURE.IMPORTANT] Wenn Sie mehr als 32 Workerknoten planen – entweder bei Erstellung des Clusters oder durch Skalierung des Clusters nach der Erstellung – müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB RAM auswählen.

 Die Abrechnung beginnt, sobald ein Cluster erstellt wurde, und sie endet, wenn der Cluster gelöscht wird. Weitere Informationen zu den Preisen finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).


## Hinzufügen von mehr Speicher

In einigen Fällen möchten Sie dem Cluster möglicherweise zusätzlichen Speicher hinzufügen. Dies kann beispielsweise dann der Fall sein, wenn Sie über mehrere Azure-Speicherkonten in verschiedenen geografischen Regionen oder für verschiedene Dienste verfügen und sämtliche dieser Konten mit HDInsight analysieren möchten.

Weitere Informationen zu sekundären Blobspeichern finden Sie unter [Verwenden des HDFS-kompatiblen Azure-Blobspeichers mit Hadoop in HDInsight](hdinsight-hadoop-use-blob-storage.md). Weitere Informationen zur Verwendung von sekundärem Data Lake-Speicher finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).


## Verwenden eines Hive/Oozie-Metastores

Es wird dringend empfohlen, einen benutzerdefinierten Metastore zu verwenden, wenn Sie Ihre Hive-Tabellen nach dem Löschen des HDInsight-Clusters beibehalten möchten. So können Sie diesen Metastore später auch an einen anderen HDInsight-Cluster anfügen.

> [AZURE.IMPORTANT] Der HDInsight-Metastore ist nicht abwärtskompatibel. Sie können beispielsweise nicht einen Metastore eines HDInsight 3.3-Clusters zum Erstellen eines HDInsight 3.2-Clusters verwenden.

Metastore enthält Hive- und Oozie-Metadaten, wie z. B. Hive-Tabellen, Partitionen, Schemata und Spalten. Der Metastore unterstützt Sie dabei, Ihre Hive- und Oozie-Metadaten beizubehalten. So müssen Sie beim Erstellen eines neuen Clusters keine Hive-Tabellen oder Oozie-Aufträge neu erstellen. Standardmäßig speichert Hive diese Informationen in einer eingebetteten Azure SQL-Datenbank. Die eingebettete Datenbank kann die Metadaten nicht beibehalten, wenn der Cluster gelöscht wird. Wenn Sie beispielsweise Hive-Tabellen in einem Cluster erstellen, der mit einem Hive-Metastore erstellt wurde, können Sie diese Tabellen sehen, wenn Sie den Cluster mit demselben Hive-Metastore löschen und neu erstellen.

Für HBase-Clustertypen ist keine Metastore-Konfiguration verfügbar.

> [AZURE.IMPORTANT] Verwenden Sie beim Erstellen eines benutzerdefinierten Metastores keinen Datenbanknamen, der Gedankenstriche oder Bindestriche enthält, da es bei der Clustererstellung ansonsten zu Fehlern kommen kann.

## Verwenden von Azure Virtual Network

[Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) ermöglicht das Erstellen eines sicheren, persistenten Netzwerks mit allen Ressourcen, die Sie für Ihre Lösung benötigen. Mit einem virtuellen Netzwerk haben Sie folgende Möglichkeiten:

* Verbinden von Cloudressourcen in einem privaten Netzwerk (nur in der Cloud)

	![Diagramm der Nur-Cloud-Konfiguration](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-cloud-only.png)

* Verbinden Ihrer Cloudressourcen mit dem Netzwerk in Ihrem lokalen Datencenter (Standort-zu-Standort oder Punkt-zu-Standort) mithilfe eines virtuellen privaten Netzwerks (VPN)

| Standort-zu-Standort-Konfiguration | Point-to-Site-Konfiguration |
| -------------------------- | --------------------------- |
| Mit einer Standort-zu-Standort-Konfiguration können Sie mehrere Ressourcen aus Ihrem Rechenzentrum mit dem virtuellen Azure-Netzwerk über ein Hardware-VPN oder den Routing- und RAS-Dienst verbinden.<br />![Diagramm der Standort-zu-Standort-Konfiguration](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-site-to-site.png) | Bei der Punkt-zu-Standort-Konfiguration können Sie eine bestimmte Ressource über ein Software-VPN mit dem virtuellen Azure-Netzwerk verbinden.<br />![Diagramm der Punkt-zu-Standort-Konfiguration](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-vnet-point-to-site.png) |

Für Windows-basierte Cluster ist ein klassisches virtuelles Netzwerk erforderlich, Linux-basierte Cluster erfordern ein virtuelles Azure Resource Manager-Netzwerk. Wenn nicht der richtige Netzwerktyp vorhanden ist, kann das Netzwerk nicht zum Erstellen des Clusters verwendet werden.

Weitere Informationen zur Funktionsweise von HDInsight mit einem virtuellen Netzwerk, einschließlich spezifischer Konfigurationsanforderungen für das virtuelle Netzwerk, finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

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

Um die Änderungen für die Laufzeit des Clusters beizubehalten, können Sie während des Erstellungsprozesses die HDInsight-Clusteranpassung nutzen. Sie können auch Ambari in Linux-basierten Clustern verwenden. Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).

>[AZURE.NOTE] Die Windows-basierten Cluster können die Änderungen aufgrund des Reimagings nicht beibehalten. Weitere Informationen finden Sie unter [Role Instance Restarts Due to OS Upgrades](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx) (Neustart von Rolleninstanzen aufgrund von Betriebssystemupdates – in englischer Sprache). Um die Änderungen für die Laufzeit des Clusters beizubehalten, müssen Sie während des Erstellungsprozesses die HDInsight-Clusteranpassung nutzen.

## Anpassen von Clustern mithilfe von Skriptaktionen

Sie können zusätzliche Komponenten installieren oder die Clusterkonfiguration mithilfe von Skripts während der Erstellung anpassen. Diese Skripts werden mithilfe der Konfigurationsoption **Skriptaktion** aufgerufen, die vom Azure-Verwaltungsportal, von HDInsight Windows PowerShell-Cmdlets oder dem HDInsight .NET SDK verwendet werden kann. Weitere Informationen finden Sie unter [Anpassen eines HDInsight-Clusters mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster.md).



## Methoden zur Clustererstellung

In diesem Artikel haben Sie grundlegende Informationen zum Erstellen eines Windows-basierten HDInsight-Clusters erhalten. In der folgenden Tabelle finden Sie spezifische Informationen zum Erstellen von Clustern mit der Methode, die Ihren Anforderungen am besten entspricht.

| Verfahren zur Clustererstellung | Webbrowser | Befehlszeile | REST-API | SDK | Linux, Mac OS X oder Unix | Windows |
| ------------------------------- |:----------------------:|:--------------------:|:------------------:|:------------:|:-----------------------------:|:------------:|
| [Azure-Portal](hdinsight-hadoop-create-windows-clusters-portal.md) | ✔ | &nbsp; | &nbsp; | &nbsp; | ✔ | ✔ |
| [Azure-Befehlszeilenschnittstelle](hdinsight-hadoop-create-windows-clusters-cli.md) | &nbsp; | ✔ | &nbsp; | &nbsp; | ✔ | ✔ |
| [Azure PowerShell](hdinsight-hadoop-create-windows-clusters-powershell.md) | &nbsp; | ✔ | &nbsp; | &nbsp; | ✔ | ✔ |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) | &nbsp; | ✔ | ✔ | &nbsp; | ✔ | ✔ |
| [.NET SDK](hdinsight-hadoop-create-windows-clusters-dotnet-sdk.md) | &nbsp; | &nbsp; | &nbsp; | ✔ | ✔ | ✔ |
| [Azure-Ressourcen-Manager-Vorlagen](hdinsight-hadoop-create-windows-clusters-arm-templates.md) | &nbsp; | ✔ | &nbsp; | &nbsp; | ✔ | ✔ |

<!---HONumber=AcomDC_0817_2016-->