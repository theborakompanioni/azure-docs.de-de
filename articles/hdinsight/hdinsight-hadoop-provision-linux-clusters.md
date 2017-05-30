---
title: Erstellen von Hadoop-, HBase-, Kafka-, Storm- oder Spark-Clustern in Azure HDInsight | Microsoft-Dokumentation
description: "Erfahren Sie, wie Hadoop-, HBase-, Storm- oder Spark-Cluster unter Linux für HDInsight mithilfe eines Browsers, der Azure-Befehlszeilenschnittstelle, Azure PowerShell, REST oder durch ein SDK erstellt werden."
keywords: Einrichtung von Hadoop-Clustern, Einrichtung von Kafka-Clustern, Einrichtung von Spark-Clustern, Einrichtung von HBase-Clustern, Einrichtung von Storm-Clustern, was ist ein Hadoop-Cluster?
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 23a01938-3fe5-4e2e-8e8b-3368e1bbe2ca
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/01/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: ed0a5cfc02572d537f4b179ad612ad153a2db1d4
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="create-hadoop-clusters-in-hdinsight"></a>Erstellen von Hadoop-Clustern in HDInsight
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Ein Hadoop-Cluster besteht aus mehreren virtuellen Computern (Knoten), die zur verteilten Verarbeitung von Aufgaben im Cluster verwendet werden. Azure HDInsight abstrahiert die Implementierungsdetails der Installation und Konfiguration einzelner Knoten, sodass Sie nur allgemeine Konfigurationsinformationen bereitstellen müssen. In diesem Artikel lernen Sie diese Konfigurationseinstellungen kennen.

## <a name="basic-configurations"></a>Grundlegende Konfigurationen

Im [Azure-Portal](https://portal.azure.com) können Sie einen HDInsight-Cluster mithilfe der *Schnellerfassung* oder *benutzerdefiniert* erstellen. Dieser Abschnitt behandelt die grundlegenden Konfigurationseinstellungen bei der Option „Schnellerfassung“. Die Option „Benutzerdefiniert“ enthält die folgenden zusätzlichen Konfigurationen:

- [Anwendungen](#hdinsight-applications)
- [Clustergröße](#cluster-size)
- Erweiterte Einstellungen

  - [Skriptaktionen](#customize-clusters-using-script-action)
  - [Virtuelles Netzwerk](#use-virtual-network)

### <a name="subscription"></a>Abonnement 
Jeder HDInsight-Cluster ist mit einem Azure-Abonnement verknüpft.

### <a name="resource-group-name"></a>Ressourcengruppenname
Mit [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) können Sie mit den Ressourcen in Ihrer Anwendung als Gruppe arbeiten. Dies wird als „Azure-Ressourcengruppe“ bezeichnet. Sie können alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereitstellen, aktualisieren, überwachen oder löschen.

### <a name="cluster-name"></a>Clustername
Der Clustername wird verwendet, um einen Cluster zu identifizieren. Der Clustername muss global eindeutig sein und den folgenden Benennungsrichtlinien entsprechen:

* Das Feld muss eine Zeichenfolge mit 3 bis 63 Zeichen enthalten.
* Das Feld darf nur Buchstaben, Zahlen und Bindestriche enthalten.

### <a name="cluster-types"></a> Clustertyp
Azure HDInsight bietet derzeit die folgenden Typen von Clustern mit je einer Reihe von Komponenten, um bestimmte Funktionalitäten bereitzustellen:

| Clustertyp | Funktionalität |
| --- | --- |
| [Hadoop](hdinsight-hadoop-introduction.md) |Abfragen und Analysen (Batchaufträge) |
| [HBase](hdinsight-hbase-overview.md) |NoSQL-Datenspeicher |
| [Storm](hdinsight-storm-overview.md) |Ereignisverarbeitung in Echtzeit |
| [Spark](hdinsight-apache-spark-overview.md) |Arbeitsspeicherinterne Verarbeitung, interaktive Abfragen, Microbatch-Datenstromverarbeitung |
| [Kafka (Vorschau)](hdinsight-apache-kafka-introduction.md) | Dies ist eine verteilte Open-Source-Streamingplattform, die zum Erstellen von Datenpipelines und Anwendungen mit Echtzeitstreaming verwendet werden kann. |
| [R Server](hdinsight-hadoop-r-server-overview.md) |Umfasst eine Vielzahl von Big Data-Statistiken, Vorhersagemodellierung und Machine Learning-Funktionen. |
| [Interactive Hive (Vorschau)](hdinsight-hadoop-use-interactive-hive.md) |Interaktive und schnellere Hive-Abfragen durch speicherinternes Caching |

Jeder Clustertyp verfügt über eine eigene Anzahl von Knoten im Cluster, Terminologie für Knoten im Cluster und eine VM-Standardgröße für jeden Knotentyp. In der folgenden Tabelle ist die Anzahl von Knoten für jeden Knotentyp jeweils in Klammern angegeben.

| Typ | Nodes | Diagramm |
| --- | --- | --- |
| Hadoop |Hauptknoten (2), Datenknoten (1+) |![HDInsight-Hadoop-Clusterknoten](./media/hdinsight-hadoop-provision-linux-clusters/HDInsight-Hadoop-roles.png) |
| HBase |Hauptserver (2), Regionsserver (1+), Master-/Zookeeper-Knoten (3) |![HDInsight-HBase-Clusterknoten](./media/hdinsight-hadoop-provision-linux-clusters/HDInsight-HBase-roles.png) |
| Storm |Nimbus-Knoten (2), Supervisor-Server (1+), Zookeeper-Knoten (3) |![HDInsight-Storm-Clusterknoten](./media/hdinsight-hadoop-provision-linux-clusters/HDInsight-Storm-roles.png) |
| Spark |Hauptknoten (2), Workerknoten (1+), Zookeeper-Knoten (3) (kostenlos für Zookeeper-VM-Größe A1) |![HDInsight-Spark-Clusterknoten](./media/hdinsight-hadoop-provision-linux-clusters/HDInsight-Spark-roles.png) |

Die folgenden Tabellen enthalten die Standard-VM-Größen für HDInsight:

* Alle unterstützten Regionen, mit Ausnahme von „Brasilien, Süden“ und „Japan, Westen“:

  | Clustertyp | Hadoop | HBase | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- |
  | Head – Standard-VM-Größe |D3 v2 |D3 v2 |A3 |D12 v2 |D12 v2 |
  | Head – empfohlene VM-Größen |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |A3, A4, A5 |D12 v2, D13 v2, D14 v2 |D12 v2, D13 v2, D14 v2 |
  | Worker – Standard-VM-Größe |D3 v2 |D3 v2 |D3 v2 |Windows: D12 v2; Linux: D4 v2 |Windows: D12 v2; Linux: D4 v2 |
  | Worker – empfohlene VM-Größen |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |D3 v2, D4 v2, D12 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
  | Zookeeper – Standard-VM-Größen | |A3 |A2 | | |
  | Zookeeper – empfohlene VM-Größen | |A3, A4, A5 |A2, A3, A4 | | |
  | Edge – Standard-VM-Größe | | | | |Windows: D12 v2; Linux: D4 v2 |
  | Edge – empfohlene VM-Größe | | | | |Windows: D12 v2, D13 v2, D14 v2; Linux: D4 v2, D12 v2, D13 v2, D14 v2 |
* Nur „Brasilien, Süden“ und „Japan, Westen“ (hier keine v2-Größen):

  | Clustertyp | Hadoop | HBase | Storm | Spark | R Server |
  | --- | --- | --- | --- | --- | --- |
  | Head – Standard-VM-Größe |D3 |D3 |A3 |D12 |D12 |
  | Head – empfohlene VM-Größen |D3, D4, D12 |D3, D4, D12 |A3, A4, A5 |D12, D13, D14 |D12, D13, D14 |
  | Worker – Standard-VM-Größe |D3 |D3 |D3 |Windows: D12; Linux: D4 |Windows: D12; Linux: D4 |
  | Worker – empfohlene VM-Größen |D3, D4, D12 |D3, D4, D12 |D3, D4, D12 |Windows: D12, D13, D14; Linux: D4, D12, D13, D14 |Windows: D12, D13, D14; Linux: D4, D12, D13, D14 |
  | Zookeeper – Standard-VM-Größen | |A2 |A2 | | |
  | Zookeeper – empfohlene VM-Größen | |A2, A3, A4 |A2, A3, A4 | | |
  | Edge – Standard-VM-Größen | | | | |Windows: D12; Linux: D4 |
  | Edge – empfohlene VM-Größen | | | | |Windows: D12, D13, D14; Linux: D4, D12, D13, D14 |

> [!NOTE]
> Head wird für den Storm-Clustertyp als *Nimbus* bezeichnet. „Worker“ wird für den HBase-Clustertyp als *Region* und für den Storm-Clustertyp als *Supervisor* bezeichnet.

> [!IMPORTANT]
> Wenn Sie mehr als 32 Workerknoten planen – entweder bei Erstellung des Clusters oder durch Skalierung des Clusters nach der Erstellung – müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB RAM auswählen.
>
>

Sie können diesen grundlegenden Typen mithilfe von [Skriptaktionen](#customize-clusters-using-script-action) weitere Komponenten wie Hue hinzufügen.

> [!IMPORTANT]
> HDInsight-Cluster gibt es in vielen verschiedenen Typen, die der Workload oder Technologie entsprechen, für die der Cluster optimiert ist. Es ist keine unterstützte Methode zum Erstellen eines Clusters vorhanden, bei der mehrere Typen kombiniert werden, z.B. Storm und HBase in einem Cluster.
>
>

Wenn für Ihre Lösung Technologien erforderlich sind, die auf mehrere HDInsight-Clustertypen verteilt sind, sollten Sie ein virtuelles Azure-Netzwerk erstellen und dann die erforderlichen Clustertypen darin erstellen. Durch diese Konfiguration können die Cluster und der gesamte Code, den Sie dafür bereitstellen, direkt miteinander kommunizieren.

Weitere Informationen zur Verwendung eines virtuellen Azure-Netzwerks finden Sie unter [Erweitern von HDInsight mit virtuellen Azure-Netzwerken](hdinsight-extend-hadoop-virtual-network.md).

Ein Beispiel für die Verwendung von zwei Clustertypen in einem virtuellen Azure-Netzwerk finden Sie unter [Analysieren von Sensordaten mit Apache Storm und HBase](hdinsight-storm-sensor-data-analysis.md).

### <a name="operating-system"></a>Betriebssystem
Sie können HDInsight-Cluster unter Linux oder Windows erstellen.  Weitere Informationen zu den Betriebssystemversionen finden Sie unter [Unterstützte HDInsight-Versionen](hdinsight-component-versioning.md#supported-hdinsight-versions). HDInsight 3.4 oder höher verwendet die Ubuntu Linux-Distribution als zugrunde liegendes Betriebssystem für den Cluster. 

### <a name="version"></a>Version
Diese Option dient zum Ermitteln der Version von HDInsight, die für diesen Cluster benötigt wird. Weitere Informationen finden Sie unter [Unterstützte HDInsight-Versionen](hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="cluster-tiers"></a>Clustertarif

Azure HDInsight bietet die Cloudlösungen für Big Data in zwei Kategorien an: Standard und Premium.  Weitere Informationen finden Sie unter [HDInsight Standard und HDInsight Premium]](hdinsight-component-versioning.md#hdinsight-standard-and-hdinsight-premium).

Der folgende Screenshot zeigt die Azure-Portal-Informationen zum Auswählen von Clustertypen.

![Konfiguration von HDInsight Premium](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-type-configuration.png)


### <a name="credentials"></a>Anmeldeinformationen
Während der Clustererstellung ermöglichen die HDInsight-Cluster Ihnen das Konfigurieren von zwei Benutzerkonten:

* HTTP-Benutzer: Der Standardbenutzername lautet *admin*. Für ihn gilt im Azure-Portal die Standardkonfiguration. Er wird auch als „Clusterbenutzer“ bezeichnet.
* SSH-Benutzer (Linux-Cluster). Wird verwendet, um die Verbindung mit dem Cluster per SSH herzustellen. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

### <a name="location"></a>Ort
Die Clusterspeicherort muss nicht explizit angegeben werden. Der Cluster verwendet den gleichen Speicherort im Standardspeicher. Um eine Liste der unterstützten Regionen zu erhalten, klicken Sie in der Dropdownliste **Region** auf [HDInsight-Preise](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).


### <a name="storage"></a>Speicher

Im ursprünglichen Hadoop Distributed File System (HDFS) werden viele lokale Datenträger im Cluster verwendet. HDInsight verwendet entweder Blobs in [Azure Storage](hdinsight-hadoop-provision-linux-clusters.md#azure-storage) oder [Azure Data Lake Store](hdinsight-hadoop-provision-linux-clusters.md#azure-data-lake-store). Es gelten einige bestimmte Anforderungen zur Verwendung von Data Lake Stores für HDInsight. Weitere Informationen finden Sie im Einführungsabschnitt unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

![HDInsight-Speicher](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-cluster-creation-storage.png)


#### <a name="azure-storage"></a>Azure Storage
Azure Storage stellt eine robuste, universelle Speicherlösung dar, die problemlos mit HDInsight integriert werden kann. Über eine HDFS-Schnittstelle können sämtliche Komponenten in HDInsight direkt mit strukturierten oder unstrukturierten Daten arbeiten, die in Blobs gespeichert sind. Die Speicherung von Daten in Azure Storage sorgt dafür, dass die HDInsight-Cluster, die für Berechnungen verwendet werden, sicher gelöscht werden können, ohne Benutzerdaten zu verlieren. HDInsight unterstützt nur __allgemeine__ Azure Storage-Konten. Der Kontotyp __Blobspeicher__ wird derzeit nicht unterstützt.

Bei der Konfiguration geben Sie ein Azure Storage-Konto und einen Blobcontainer in diesem Konto an. Der Blobcontainer wird vom Cluster als Standardspeicherort verwendet. Optional können Sie zusätzliche Azure Storage-Konten (verknüpften Speicher) angeben, auf die der Cluster zugreifen kann. Der Cluster kann auch auf alle Blobcontainer zugreifen, die mit vollständig öffentlichem Lesezugriff oder mit öffentlichem Lesezugriff nur für Blobs konfiguriert sind.

Die Verwendung des Standard-Blobcontainers zum Speichern von Geschäftsdaten wird nicht empfohlen. Stattdessen empfiehlt es sich, den Standard-Blobcontainer nach jeder Verwendung zu löschen, um die Speicherkosten zu verringern. Beachten Sie, dass der Standardcontainer Anwendungs- und Systemprotokolle enthält. Stellen Sie sicher, dass Sie die Protokolle abrufen, bevor Sie den Container löschen.

Das Freigeben eines Blobcontainers für mehrere Cluster wird nicht unterstützt.

Weitere Informationen zur Verwendung eines Azure Storage-Kontos finden Sie unter [Verwenden von Azure Storage mit HDInsight](hdinsight-hadoop-use-blob-storage.md).

#### <a name="azure-data-lake-store"></a>Azure Data Lake Store
Neben Azure Storage können Sie auch [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) als Standardspeicherkonto für HBase-Cluster in HDInsight und als verknüpften Speicher für alle vier Typen von HDInsight-Clustern verwenden. Weitere Informationen hierzu finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

#### <a name="use-additional-storage"></a>Zusätzlicher Speicher

In manchen Fällen sollte dem Cluster zusätzlicher Speicher hinzugefügt werden können. Dies kann beispielsweise dann der Fall sein, wenn Sie über mehrere Azure-Speicherkonten in verschiedenen geografischen Regionen oder für verschiedene Dienste verfügen und all diese Konten mit HDInsight analysieren möchten.

Sie können Speicherkonten hinzufügen, wenn Sie einen HDInsight-Cluster erstellen oder nachdem die Erstellung des Clusters abgeschlossen ist.  Weitere Informationen finden Sie unter [Anpassen Linux-basierter HDInsight-Cluster mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

Weitere Informationen zu einem sekundären Azure Storage-Konto finden Sie unter [Verwenden von Azure Storage mit HDInsight](hdinsight-hadoop-use-blob-storage.md). Weitere Informationen zur Verwendung von sekundärem Data Lake-Speicher finden Sie unter [Erstellen eines HDInsight-Clusters mit Data Lake Store mithilfe des Azure-Portals](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).


#### <a name="use-hiveoozie-metastore"></a>Hive-Metastore

Wir empfehlen, einen benutzerdefinierten Metastore zu verwenden, wenn Sie nach dem Löschen des HDInsight-Clusters Ihre Hive-Tabellen beibehalten möchten. Sie können diesen Metastore an einen anderen HDInsight-Cluster anfügen.

> [!IMPORTANT]
> Ein HDInsight-Metastore, der für eine HDInsight-Clusterversion erstellt wurde, kann nicht über verschiedene HDInsight-Clusterversionen freigegeben werden. Eine Liste mit den HDInsight-Versionen finden Sie unter [Unterstützte HDInsight-Versionen](hdinsight-component-versioning.md#supported-hdinsight-versions).

Der Metastore enthält Hive-Metadaten, wie z. B. Hive-Tabellen, Partitionen, Schemata und Spalten. Mit dem Metastore können Sie Ihre Hive-Metadaten beibehalten, sodass Sie beim Erstellen eines neuen Clusters keine Hive-Tabellen neu erstellen müssen. Standardmäßig speichert Hive diese Informationen in einer eingebetteten Azure SQL-Datenbank. Die eingebettete Datenbank kann die Metadaten nicht beibehalten, wenn der Cluster gelöscht wird. Wenn Sie eine Hive-Tabelle in einem HDInsight-Cluster mit einem konfigurierten Hive-Metastore erstellen, wird die Tabelle beibehalten, falls Sie den Cluster mit demselben Hive-Metastore erneut erstellen.

Die Metastore-Konfiguration ist nicht für alle Clustertypen verfügbar. Für HBase- oder Kafka-Cluster ist sie beispielsweise nicht verfügbar.

> [!IMPORTANT]
> Verwenden Sie beim Erstellen eines benutzerdefinierten Metastores keinen Datenbanknamen, der Gedankenstriche, Bindestriche oder Leerzeichen enthält. Dies kann dazu führen, dass der Clustererstellungsprozess fehlschlägt.

> [!WARNING]
> Azure SQL Data Warehouse wird für den Hive-Metastore nicht unterstützt.


#### <a name="oozie-metastore"></a>Oozie-Metastore

Verwenden Sie zur Erhöhung der Leistung bei Verwendung von Oozie einen benutzerdefinierten Metastore. Ein benutzerdefinierter Metastore ist auch nützlich, wenn Sie nach dem Löschen Ihres Clusters auf Oozie-Auftragsdaten zugreifen möchten. Wenn Sie Oozie nicht oder nur gelegentlich verwenden möchten, müssen Sie keinen benutzerdefinierten Metastore erstellen.

> [!IMPORTANT]
> Ein benutzerdefinierter Oozie-Metastore kann nicht wiederverwendet werden. Wenn Sie einen benutzerdefinierten Oozie-Metastore verwenden möchten, müssen Sie beim Erstellen des HDInsight-Clusters eine leere Azure SQL-Datenbank bereitstellen.

Die Metastore-Konfiguration ist nicht für alle Clustertypen verfügbar. Für HBase- oder Kafka-Cluster ist sie beispielsweise nicht verfügbar.

> [!WARNING]
> Azure SQL Data Warehouse wird für den Oozie-Metastore nicht unterstützt.

## <a name="install-hdinsight-applications"></a>Installieren von HDInsight-Anwendungen

Eine HDInsight-Anwendung kann von Benutzern in einem Linux-basierten HDInsight-Cluster installiert werden. Diese Anwendungen können von Microsoft oder von unabhängigen Softwareanbietern (Independent Software Vendors, ISVs) bezogen oder aber selbst entwickelt werden. Weitere Informationen finden Sie unter [Installieren von Hadoop-Anwendungen von Drittanbietern in Azure HDInsight](hdinsight-apps-install-applications.md).

Die meisten HDInsight-Anwendungen werden auf einem leeren Edgeknoten installiert.  Ein leerer Edgeknoten ist ein virtueller Linux-Computer, auf dem die gleichen Clienttools installiert und konfiguriert sind wie im Hauptknoten. Sie können den Edgeknoten zum Zugreifen auf den Cluster, Testen Ihrer Clientanwendungen und Hosten von Clientanwendungen verwenden. Weitere Informationen finden Sie unter [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md)(Verwenden leerer Edgeknoten in HDInsight).

## <a name="configure-cluster-size"></a>Konfigurieren der Clustergröße

Kunden wird die Nutzung dieser Knoten für die Laufzeit des Clusters in Rechnung gestellt. Die Abrechnung beginnt, sobald ein Cluster erstellt wurde, und sie endet, wenn der Cluster gelöscht wird. Bei Clustern ist kein Aufheben der Zuweisung oder ein Anhalten möglich.

Unterschiedliche Clustertypen weisen verschiedene Knotentypen, eine unterschiedliche Anzahl von Knoten sowie verschiedene Knotengrößen auf. Ein Hadoop-Cluster besitzt beispielsweise zwei *Hauptknoten* sowie standardmäßig vier *Datenknoten*. Ein Storm-Cluster dagegen weist zwei *Nimbusknoten*, drei *Zookeeperknoten* und standardmäßig vier *Supervisorknoten* auf. Die Kosten von HDInsight-Clustern ergeben sich aus der Anzahl der Knoten und aus der Größe der virtuellen Computer für die Knoten. Wenn Sie z.B. wissen, dass Sie Vorgänge mit hohem Arbeitsspeicherbedarf ausführen werden, sollten Sie eine Computeressource mit großem Arbeitsspeicher auswählen. Zu Lernzwecken wird die Verwendung eines Datenknotens empfohlen. Weitere Informationen zu den Preisen von HDInsight finden Sie unter [HDInsight – Preise](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> Die Begrenzung der Clustergröße variiert je nach Azure-Abonnement. Wenden Sie sich an den Azure-Abrechnungssupport, um diese Begrenzung zu erhöhen.
>
> Die von Ihrem Cluster verwendeten Knoten zählen nicht als virtuelle Computer, da die VM-Images, die für die Knoten verwendet werden, ein Implementierungsdetail des HDInsight-Diensts sind. Die von den Knoten verwendeten Computekerne zählen jedoch zur Gesamtanzahl von Computekernen, die für Ihr Abonnement verfügbar sind. Beim Erstellen eines HDInsight-Clusters können Sie die Anzahl der verfügbaren Kerne und die Kerne, die vom Cluster verwendet werden sollen, auf dem Blatt **Clustergröße** im Abschnitt „Zusammenfassung“ anzeigen.
>
>

Wenn Sie das Azure-Portal zum Konfigurieren des Clusters verwenden, ist die Knotengröße über das Blatt **Knotentarife** verfügbar. Außerdem werden die Kosten angezeigt, die den unterschiedlichen Knotengrößen zugeordnet sind. Der folgende Screenshot zeigt die Optionen für einen Linux-basierten Hadoop-Cluster.

![VM-Größen für HDInsight-Knoten](./media/hdinsight-hadoop-provision-linux-clusters/hdinsight-node-sizes.png)

In den folgenden Tabellen sind die von HDInsight-Clustern unterstützten Größen und die von den einzelnen Größen bereitgestellten Kapazitäten aufgeführt.

### <a name="standard-tier-a-series"></a>Standard-Ebene: A-Serie
Im klassischen Bereitstellungsmodell unterscheiden sich einige VM-Größen in PowerShell und der Befehlszeilenschnittstelle (CLI).

* Standard_A3 ist „Groß“
* Standard_A4 ist „Extragroß“

| Größe | CPU-Kerne | Arbeitsspeicher | Netzwerkkarten (max.) | Max. Datenträgergröße | Maximal Datenträger (jeweils 1023 GB) | Max. IOPS (500 pro Datenträger) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_A3\Groß |4 |7 GB |2 |Temporär = 285 GB |8 |8 x 500 |
| Standard_A4\ExtraGroß |8 |14 GB |4 |Temporär = 605 GB |16 |16 x 500 |
| Standard_A6 |4 |28 GB |2 |Temporär = 285 GB |8 |8 x 500 |
| Standard_A7 |8 |56 GB |4 |Temporär = 605 GB |16 |16 x 500 |

### <a name="standard-tier-d-series"></a>Standard-Ebene: D-Serie
| Größe | CPU-Kerne | Arbeitsspeicher | Netzwerkkarten (max.) | Max. Datenträgergröße | Maximal Datenträger (jeweils 1023 GB) | Max. IOPS (500 pro Datenträger) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D3 |4 |14 GB |4 |Temporär (SSD) = 200 GB |8 |8 x 500 |
| Standard_D4 |8 |28 GB |8 |Temporär (SSD) = 400 GB |16 |16 x 500 |
| Standard_D12 |4 |28 GB |4 |Temporär (SSD) = 200 GB |8 |8 x 500 |
| Standard_D13 |8 |56 GB |8 |Temporär (SSD) = 400 GB |16 |16 x 500 |
| Standard_D14 |16 |112 GB |8 |Temporär (SSD) = 800 GB |32 |32 x 500 |

### <a name="standard-tier-dv2-series"></a>Standard-Ebene: Dv2-Serie
| Größe | CPU-Kerne | Arbeitsspeicher | Netzwerkkarten (max.) | Max. Datenträgergröße | Maximal Datenträger (jeweils 1023 GB) | Max. IOPS (500 pro Datenträger) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D3_v2 |4 |14 GB |4 |Temporär (SSD) = 200 GB |8 |8 x 500 |
| Standard_D4_v2 |8 |28 GB |8 |Temporär (SSD) = 400 GB |16 |16 x 500 |
| Standard_D12_v2 |4 |28 GB |4 |Temporär (SSD) = 200 GB |8 |8 x 500 |
| Standard_D13_v2 |8 |56 GB |8 |Temporär (SSD) = 400 GB |16 |16 x 500 |
| Standard_D14_v2 |16 |112 GB |8 |Temporär (SSD) = 800 GB |32 |32 x 500 |

Überlegungen zur Bereitstellung, die Sie im Hinblick auf die Verwendung dieser Ressourcen berücksichtigen sollten, finden Sie unter [Größen für virtuelle Computer](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Informationen zu den Preisen der unterschiedlichen Größen finden Sie unter [HDInsight-Preise](https://azure.microsoft.com/pricing/details/hdinsight).   

> [!IMPORTANT]
> Wenn Sie mehr als 32 Workerknoten planen – entweder bei Erstellung des Clusters oder durch Skalierung des Clusters nach der Erstellung – müssen Sie eine Hauptknotengröße von mindestens 8 Kernen und 14 GB RAM auswählen.
>
>

Die Abrechnung beginnt, sobald ein Cluster erstellt wurde, und sie endet, wenn der Cluster gelöscht wird. Weitere Informationen zu den Preisen finden Sie unter [HDInsight – Preise](https://azure.microsoft.com/pricing/details/hdinsight/).


> [!WARNING]
> Die Verwendung eines zusätzlichen Speicherkontos an einem anderen Ort als dem HDInsight-Cluster wird nicht unterstützt.



## <a name="use-virtual-network"></a>Verwenden von Azure Virtual Network
[Azure Virtual Network](https://azure.microsoft.com/documentation/services/virtual-network/) ermöglicht Ihnen das Erstellen eines sicheren, persistenten Netzwerks mit allen Ressourcen, die Sie für Ihre Lösung benötigen. Weitere Informationen zur Verwendung von HDInsight mit einem virtuellen Netzwerk, einschließlich spezifischer Konfigurationsanforderungen für das virtuelle Netzwerk, finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).


## <a name="customize-clusters-using-script-action"></a>Anpassen von Clustern mithilfe von Skriptaktionen

Sie können zusätzliche Komponenten installieren oder die Clusterkonfiguration mithilfe von Skripts während der Erstellung anpassen. Diese Skripts werden mithilfe der Konfigurationsoption **Skriptaktion**aufgerufen, die vom Azure-Verwaltungsportal, von HDInsight Windows PowerShell-Cmdlets oder dem HDInsight .NET SDK verwendet werden kann. Weitere Informationen finden Sie unter [Anpassen eines HDInsight-Clusters mithilfe von Skriptaktionen](hdinsight-hadoop-customize-cluster-linux.md).

Einige systemeigene Java-Komponenten wie Mahout und Cascading können auf dem Cluster als JAR-Dateien (Java Archive) ausgeführt werden. Diese JAR-Dateien können an Azure Storage verteilt und mit den Verfahren zur Übermittlung von Hadoop-Aufträgen an HDInsight-Cluster gesendet werden. Weitere Informationen finden Sie unter [Programmgesteuerte Übermittlung von Hadoop-Aufträgen](hdinsight-submit-hadoop-jobs-programmatically.md).

> [!NOTE]
> Wenn bei der Bereitstellung von JAR-Dateien für HDInsight-Cluster oder beim Aufrufen von JAR-Dateien für HDInsight-Cluster Probleme auftreten, wenden Sie sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/).
>
> Cascading wird von HDInsight nicht unterstützt, und es steht kein Microsoft-Support dafür zur Verfügung. Listen der unterstützten Komponenten finden Sie unter [Neuheiten in den von HDInsight bereitgestellten Clusterversionen](hdinsight-component-versioning.md).
>
>

Es kann vorkommen, dass Sie die folgenden Konfigurationsdateien während des Erstellungsprozesses bearbeiten möchten:

* clusterIdentity.xml
* core-site.xml
* gateway.xml
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Weitere Informationen finden Sie unter [Anpassen von HDInsight-Clustern mithilfe von Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="cluster-creation-methods"></a>Methoden zur Clustererstellung
In diesem Artikel haben Sie grundlegende Informationen zum Erstellen eines Linux-basierten HDInsight-Clusters erhalten. In der folgenden Tabelle finden Sie spezifische Informationen zum Erstellen von Clustern mit der Methode, die Ihren Anforderungen am besten entspricht.

| Verfahren zur Clustererstellung | Webbrowser | Befehlszeile | REST-API | SDK | Linux, Mac OS X oder Unix | Windows |
| --- |:---:|:---:|:---:|:---:|:---:|:---:|
| [Azure-Portal](hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |✔ |✔ |
| [Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |✔ |✔ |
| [Azure-Befehlszeilenschnittstelle](hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |✔ |✔ |
| [Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |✔ |✔ |
| [cURL](hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |✔ |✔ |
| [.NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |&nbsp; |&nbsp; |&nbsp; |✔ |✔ |✔ |
| [Azure-Ressourcen-Manager-Vorlagen](hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |✔ |✔ |

## <a name="troubleshoot"></a>Problembehandlung

Falls beim Erstellen von HDInsight-Clustern Probleme auftreten, sehen Sie sich die [Voraussetzungen für die Zugriffssteuerung](hdinsight-administer-use-portal-linux.md#create-clusters) an.

## <a name="next-steps"></a>Nächste Schritte

- [Was ist HDInsight?](hdinsight-hadoop-introduction.md)
- [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
