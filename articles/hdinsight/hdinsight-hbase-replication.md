---
title: Einrichten der HBase-Clusterreplikation in virtuellen Azure-Netzwerken | Microsoft-Dokumentation
description: "Erfahren Sie, wie Sie die HBase-Replikation zwischen HDInsight-Versionen für Lastenausgleich, Hochverfügbarkeit, Migration und Updates ohne Ausfallzeit und Notfallwiederherstellung einrichten."
services: hdinsight,virtual-network
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/06/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 87f4d219a678cab78980af2ff8b98ae55739f04c
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="set-up-hbase-cluster-replication-in-azure-virtual-networks"></a>Einrichten der HBase-Clusterreplikation in virtuellen Azure-Netzwerken

Erfahren Sie, wie Sie die HBase-Replikation innerhalb eines virtuellen Netzwerks oder zwischen zwei virtuellen Netzwerken in Azure einrichten.

Clusterreplikation verwendet eine Source-Push-Methodologie. Ein HBase-Cluster kann Quelle oder Ziel sein oder beide Rollen gleichzeitig erfüllen. Die Replikation ist asynchron. Das Ziel der Replikation ist die endgültige Konsistenz. Die Bearbeitung einer Spaltenfamilie in der Quelle bei aktivierter Replikation wird an alle Zielcluster weitergegeben. Wenn Daten von einem Cluster auf einen anderen repliziert werden, werden der Quellcluster und alle Cluster, die bereits Daten genutzt haben, nachverfolgt, um Replikationsschleifen zu verhindern.

In diesem Tutorial richten Sie eine Quell-/Ziel-Replikation ein. Andere Clustertopologien finden Sie im [Apache HBase-Referenzhandbuch](http://hbase.apache.org/book.html#_cluster_replication).

Anwendungsfälle für die HBase-Replikation für ein einzelnes virtuelles Netzwerk sind:

* Lastenausgleich. Sie können z.B. Scans oder MapReduce-Aufträge im Zielcluster ausführen und Daten im Quellcluster erfassen.
* Hinzufügen hoher Verfügbarkeit.
* Migrieren von Daten zwischen HBase-Clustern.
* Aktualisieren eines Azure HDInsight-Clusters von einer Version auf eine andere.

Anwendungsfälle für die HBase-Replikation für zwei virtuelle Netzwerks sind:

* Einrichten der Notfallwiederherstellung.
* Lastenausgleich und Partitionierung der Anwendung.
* Hinzufügen hoher Verfügbarkeit.

Sie können Cluster mit [Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md)-Skripts aus [GitHub](https://github.com/Azure/hbase-utils/tree/master/replication) replizieren.

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial wird ein Azure-Abonnement benötigt. Siehe [How to get Azure Free trial for testing Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/) (So erhalten Sie eine kostenlose Azure-Testversion zum Testen von Hadoop in HDInsight).

## <a name="set-up-the-environments"></a>Einrichten der Umgebungen

Es gibt drei Konfigurationsoptionen:

- Zwei HBase-Cluster in einem virtuellen Azure-Netzwerk.
- Zwei HBase-Cluster in zwei verschiedenen virtuellen Netzwerken in der gleichen Region.
- Zwei HBase-Cluster in zwei verschiedenen virtuellen Netzwerken in zwei verschiedenen Regionen (Georeplikation).

Um Ihnen die Einrichtung der Umgebungen zu erleichtern, haben wir einige [Azure Resource Manager-Vorlagen](../azure-resource-manager/resource-group-overview.md) erstellt. Wenn Sie die Umgebungen mit anderen Methoden einrichten möchten, finden Sie hier weitere Informationen:

- [Erstellen von Linux-basierten Hadoop-Clustern in HDInsight](hdinsight-hadoop-provision-linux-clusters.md)
- [Erstellen von HBase-Clustern in Azure Virtual Network](hdinsight-hbase-provision-vnet.md)

### <a name="set-up-one-virtual-network"></a>Einrichten eines virtuellen Netzwerks

Wählen Sie das folgende Bild, um zwei HBase-Cluster im gleichen virtuellen Netzwerk zu erstellen. Die Vorlage wird in den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-one-vnet/) gespeichert.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-one-vnet%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

### <a name="set-up-two-virtual-networks-in-the-same-region"></a>Einrichten von zwei virtuellen Netzwerken in der gleichen Region

Wählen Sie das folgende Bild, um zwei virtuelle Netzwerke mit Peering virtueller Netzwerke und zwei HBase-Clustern in der gleichen Region zu erstellen. Die Vorlage wird in den [Azure-Schnellstartvorlagen](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-two-vnets-same-region/) gespeichert.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-hbase-replication-two-vnets-same-region%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>



Dieses Szenario erfordert [virtuelle Netzwerke mit Peering](../virtual-network/virtual-network-peering-overview.md). Die Vorlage ermöglicht virtuelle Netzwerke mit Peering.   

Die HBase-Replikation verwendet die IP-Adressen der virtuellen Zookeeper-Computer. Sie müssen statische IP-Adressen für die HBase-Zookeeper-Zielknoten einrichten.

**So konfigurieren Sie statische IP-Adressen**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü auf der linken Seite die Option **Ressourcengruppen** aus.
3. Wählen Sie die Ressourcengruppe, die den HBase-Zielcluster enthält. Dies ist die Ressourcengruppe, die Sie bei Verwendung der Resource Manager-Vorlage zum Erstellen der Umgebung angegeben haben. Sie können die Liste mithilfe des Filters eingrenzen. Es wird eine Liste der Ressourcen angezeigt, die die beiden virtuellen Netzwerke enthalten.
4. Wählen Sie das virtuelle Netzwerk, das den HBase-Zielcluster enthält. Wählen Sie z.B. **xxxx-vnet2**. Es werden drei Geräte angezeigt, deren Namen mit **nic-zookeepermode-** beginnen. Diese drei Geräte sind die virtuellen Zookeeper-Computer.
5. Wählen Sie einen der virtuellen Zookeeper-Computer aus.
6. Wählen Sie die **IP-Konfigurationen** aus.
7. Wählen Sie in der Liste **ipConfig1**.
8. Wählen Sie **Statisch**, und kopieren Sie bzw. notieren Sie sich die tatsächliche IP-Adresse. Sie benötigen die IP-Adresse, wenn Sie die Skriptaktion zur Aktivierung der Replikation ausführen.

  ![Statische IP-Adresse des Zookeeper-Computers für die HBase-Replikation in HDInsight](./media/hdinsight-hbase-replication/hdinsight-hbase-replication-zookeeper-static-ip.png)

9. Wiederholen Sie Schritt 6, um die statische IP-Adresse für die anderen beiden Zookeeper-Knoten zu ermitteln.

Für das virtuelle Netzwerke übergreifende Szenario müssen Sie den **-ip**-Schalter verwenden, wenn Sie die `hdi_enable_replication.sh`-Skriptaktion aufrufen.

### <a name="set-up-two-virtual-networks-in-two-different-regions"></a>Einrichten von zwei virtuellen Netzwerken in zwei verschiedenen Regionen

Wählen Sie das folgende Bild, um zwei virtuelle Netzwerke in zwei verschiedenen Regionen zu erstellen. Die Vorlage ist in einem globalen Azure-Blobcontainer gespeichert.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fhbaseha%2Fdeploy-hbase-geo-replication.json" target="_blank"><img src="./media/hdinsight-hbase-replication/deploy-to-azure.png" alt="Deploy to Azure"></a>

Erstellen Sie ein VPN-Gateway zwischen den beiden virtuellen Netzwerken. Anweisungen dazu finden Sie unter [Erstellen einer Site-to-Site-Verbindung im Azure-Portal](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

Die HBase-Replikation verwendet die IP-Adressen der virtuellen Zookeeper-Computer. Sie müssen statische IP-Adressen für die HBase-Zookeeper-Zielknoten einrichten. Informationen zum Einrichten einer statischen IP-Adresse finden Sie in diesem Artikel im Abschnitt [Einrichten von zwei virtuellen Netzwerken in der gleichen Region](#set-up-two-virtual-networks-in-the-same-region).

Für das virtuelle Netzwerke übergreifende Szenario müssen Sie den **-ip**-Schalter verwenden, wenn Sie die `hdi_enable_replication.sh`-Skriptaktion aufrufen.

## <a name="load-test-data"></a>Laden von Testdaten

Wenn Sie einen Cluster replizieren, müssen Sie die Tabellen angeben, die Sie replizieren möchten. In diesem Abschnitt laden Sie einige Daten in den Quellcluster. Im nächsten Abschnitt aktivieren Sie die Replikation zwischen den beiden Clustern.

Um eine Tabelle namens [Kontakte](hdinsight-hbase-tutorial-get-started-linux.md) zu erstellen und einige Daten in diese Tabelle einzufügen, befolgen Sie die Anweisungen unter **Erste Schritte mit einem Apache HBase-Beispiel in HDInsight**.

## <a name="enable-replication"></a>Replikation aktivieren

Die folgenden Schritte zeigen, wie Sie das Skript mit Skriptaktionen aus dem Azure-Portal aufrufen. Informationen zum Ausführen einer Skriptaktion mithilfe von Azure PowerShell und dem Azure-Befehlszeilenschnittstellen-Tool (Azure CLI) finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

**So aktivieren Sie die HBase-Replikation über das Azure-Portal**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Öffnen Sie den HBase-Quellcluster.
3. Wählen Sie im Clustermenü **Skriptaktionen**.
4. Wählen Sie am oberen Rand der Seite **Neue übermitteln** aus.
5. Wählen Sie folgende Informationen aus, oder geben Sie sie ein:

  1. **Name**: Geben Sie **Replikation aktivieren** ein.
  2. **Bash-Skript-URL**: Geben Sie **https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_enable_replication.sh** ein.
  3.  **Hauptknoten**: Stellen Sie sicher, dass diese Option aktiviert ist. Deaktivieren Sie die anderen Knotentypen.
  4. **Parameter**: Die folgenden Beispielparameter aktivieren die Replikation für alle vorhandenen Tabellen und kopieren dann alle Daten aus dem Quellcluster in den Zielcluster:

            -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -copydata

6. Klicken Sie auf **Erstellen**. Die Ausführung des Skripts kann einige Zeit in Anspruch nehmen, insbesondere dann, wenn Sie das Argument **-copydata** verwenden.

Erforderliche Argumente:

|Name|Beschreibung|
|----|-----------|
|-s, --src-cluster | Gibt den DNS-Namen des HBase-Quellclusters an. Beispiel: -s hbsrccluster, --src-cluster=hbsrccluster |
|-d, --dst-cluster | Gibt den DNS-Namen des HBase-Zielclusters (Replikats) an. Beispiel: -s dsthbcluster, --src-cluster=dsthbcluster |
|-sp, --src-ambari-password | Gibt das Administratorkennwort für Ambari im HBase-Quellcluster an. |
|-dp, --dst-ambari-password | Gibt das Administratorkennwort für Ambari im HBase-Zielcluster an.|

Optionale Argumente:

|Name|Beschreibung|
|----|-----------|
|-su, --src-ambari-user | Gibt den Administratorbenutzernamen für Ambari im HBase-Quellcluster an. Der Standardwert lautet **admin**. |
|-du, --dst-ambari-user | Gibt den Administratorbenutzernamen für Ambari im HBase-Zielcluster an. Der Standardwert lautet **admin**. |
|-t, --table-list | Gibt die zu replizierenden Tabellen an. Beispiel: --table-list="table1;table2;table3". Wenn Sie keine Tabellen angeben, werden alle vorhandenen HBase-Tabellen repliziert.|
|-m, --machine | Gibt den Hauptknoten an, auf dem die Skriptaktion ausgeführt werden soll. Der Wert lautet entweder **hn1** oder **hn0**. Da der **hn0**-Hauptknoten in der Regel höher ausgelastet ist, sollten Sie **hn1** verwenden. Verwenden Sie diese Option, wenn Sie das $0-Skript als Skriptaktion im HDInsight-Portal oder in Azure PowerShell ausführen.|
|-ip | Ist erforderlich, wenn Sie die Replikation zwischen zwei virtuellen Netzwerken aktivieren. Dieses Argument fungiert als Option, um statt der FQDNs die statischen IP-Adressen der Zookeeper-Knoten in den Replikatclustern zu verwenden. Sie müssen die statischen IP-Adressen vorkonfigurieren, bevor Sie die Replikation aktivieren. |
|-cp, -copydata | Aktiviert die Migration vorhandener Daten in den Tabellen, in denen die Replikation aktiviert ist. |
|-rpm, -replicate-phoenix-meta | Aktiviert die Replikation in Phoenix-Systemtabellen. <br><br>*Verwenden Sie diese Option mit Vorsicht.* Es wird empfohlen, die Phoenix-Tabellen in Replikatclustern neu zu erstellen, bevor Sie dieses Skript verwenden. |
|-h, --help | Zeigt Informationen zur Nutzung an. |

Der Abschnitt `print_usage()` des [Skripts](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_enable_replication.sh) bietet eine detaillierte Erläuterung der Parameter.

Nachdem die Skriptaktion erfolgreich bereitgestellt wurde, können Sie über SSH eine Verbindung mit dem HBase-Zielcluster herstellen und dann überprüfen, ob die Daten repliziert wurden.

### <a name="replication-scenarios"></a>Replikationsszenarien

Die folgende Liste zeigt einige allgemeine Anwendungsfälle und die zugehörigen Parametereinstellungen:

- **Aktivieren der Replikation in allen Tabellen zwischen den beiden Clustern**. Dieses Szenario erfordert kein Kopieren oder Migrieren der in den Tabellen vorhandenen Daten und verwendet keine Phoenix-Tabellen. Verwenden Sie die folgenden Parameter:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password>  

- **Aktivieren der Replikation in bestimmten Tabellen**. Verwenden Sie die folgenden Parameter, um die Replikation in „table1“, „table2“ und „table3“ zu aktivieren:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3"

- **Aktivieren der Replikation in bestimmten Tabellen und Kopieren der vorhandenen Daten**. Verwenden Sie die folgenden Parameter, um die Replikation in „table1“, „table2“ und „table3“ zu aktivieren:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -copydata

- **Aktivieren der Replikation in allen Tabellen und Replikation der Phoenix-Metadaten von der Quelle zum Ziel**. Die Replikation der Phoenix-Metadaten ist nicht perfekt. Verwenden Sie sie mit Vorsicht. Verwenden Sie die folgenden Parameter:

        -m hn1 -s <source cluster DNS name> -d <destination cluster DNS name> -sp <source cluster Ambari password> -dp <destination cluster Ambari password> -t "table1;table2;table3" -replicate-phoenix-meta

## <a name="copy-and-migrate-data"></a>Kopieren und Migrieren von Daten

Es gibt zwei separate Skripte mit Skriptaktionen für das Kopieren oder Migrieren von Daten, nachdem die Replikation aktiviert wurde:

- [Skript für kleine Tabellen](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_copy_table.sh) (Tabellen, die wenige Gigabytes groß sind, sodass der gesamte Kopiervorgang in weniger als einer Stunde abgeschlossen sein sollte)

- [Skript für große Tabellen](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/nohup_hdi_copy_table.sh) (Tabellen, deren Kopiervorgang voraussichtlich länger als eine Stunde dauert)

Sie können das gleiche Verfahren wie unter [Aktivieren der Replikation](#enable-replication) befolgen, um die Skriptaktion aufzurufen. Verwenden Sie die folgenden Parameter:

    -m hn1 -t <table1:start_timestamp:end_timestamp;table2:start_timestamp:end_timestamp;...> -p <replication_peer> [-everythingTillNow]

Der Abschnitt `print_usage()` des [Skripts](https://github.com/Azure/hbase-utils/blob/master/replication/hdi_copy_table.sh) bietet eine detaillierte Erläuterung der Parameter.

### <a name="scenarios"></a>Szenarien

- **Kopieren bestimmter Tabellen („test1“, „test2“ und „test3“) für alle bis jetzt (aktueller Zeitstempel) bearbeiteten Zeilen**:

        -m hn1 -t "test1::;test2::;test3::" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow
  Oder:

        -m hn1 -t "test1::;test2::;test3::" --replication-peer="zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure" -everythingTillNow


- **Kopieren bestimmter Tabellen innerhalb eines angegebenen Zeitraums**:

        -m hn1 -t "table1:0:452256397;table2:14141444:452256397" -p "zk5-hbrpl2;zk1-hbrpl2;zk5-hbrpl2:2181:/hbase-unsecure"


## <a name="disable-replication"></a>Deaktivieren der Replikation

Um die Replikation zu deaktivieren, verwenden Sie ein weiteres Skript mit einer Skriptaction, das Sie auf [GitHub](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) finden. Sie können das gleiche Verfahren wie unter [Aktivieren der Replikation](#enable-replication) befolgen, um die Skriptaktion aufzurufen. Verwenden Sie die folgenden Parameter:

    -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> <-all|-t "table1;table2;...">  

Der Abschnitt `print_usage()` des [Skripts](https://raw.githubusercontent.com/Azure/hbase-utils/master/replication/hdi_disable_replication.sh) bietet eine detaillierte Erläuterung der Parameter.

### <a name="scenarios"></a>Szenarien

- **Deaktivieren der Replikation in allen Tabellen**:

        -m hn1 -s <source cluster DNS name> -sp Mypassword\!789 -all
  oder

        --src-cluster=<source cluster DNS name> --dst-cluster=<destination cluster DNS name> --src-ambari-user=<source cluster Ambari user name> --src-ambari-password=<source cluster Ambari password>

- **Deaktivieren der Replikation in angegebenen Tabellen („table1“, „table2“ und „table3“)**:

        -m hn1 -s <source cluster DNS name> -sp <source cluster Ambari password> -t "table1;table2;table3"

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie erfahren, wie Sie die HBase-Replikation innerhalb eines virtuellen Netzwerks oder zwischen zwei virtuellen Netzwerken einrichten. Weitere Informationen zu HDInsight und HBase finden Sie in diesen Artikeln:

* [Erste Schritte mit Apache HBase in HDInsight][hdinsight-hbase-get-started]
* [Überblick über HDInsight HBase][hdinsight-hbase-overview]
* [Erstellen von HBase-Clustern in Azure Virtual Network][hdinsight-hbase-provision-vnet]
* [Analysieren von Twitter-Stimmungen in Echtzeit mit HBase][hdinsight-hbase-twitter-sentiment]
* [Analysieren von Sensordaten mit Apache Storm, Event Hub und HBase in HDInsight (Hadoop)][hdinsight-sensor-data]

[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[hdinsight-hbase-geo-replication-dns]: ../hdinsight-hbase-geo-replication-configure-VNet.md


[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication/HDInsight.HBase.Replication.Network.diagram.png

[powershell-install]: /powershell/azureps-cmdlets-docs
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started-linux.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
[hdinsight-sensor-data]: hdinsight-storm-sensor-data-analysis.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md

