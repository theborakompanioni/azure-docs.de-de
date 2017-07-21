---
title: Tipps zur Verwendung von Hadoop unter Linux-basiertem HDInsight in Azure | Microsoft-Dokumentation
description: "Hier erhalten Sie Implementierungstipps für die Verwendung von Linux-basierten HDInsight (Hadoop)-Clustern in einer vertrauten Linux-Umgebung, die in der Azure-Cloud ausgeführt wird."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c41c611c-5798-4c14-81cc-bed1e26b5609
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 1477ed13ef0434ed86938c49e6bb815837cb40fb
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="information-about-using-hdinsight-on-linux"></a>Informationen zur Verwendung von HDInsight unter Linux

Azure HDInsight-Cluster stellen Hadoop in einer vertrauten Linux-Umgebung bereit, die in der Azure-Cloud ausgeführt wird. Die Lösung sollte sich größtenteils genauso wie jede andere Installation von Hadoop unter Linux verhalten. In diesem Dokument werden bestimmte Unterschiede aufgeführt, die Sie kennen sollten.

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Welche Hadoop-Komponenten und -Versionen sind in HDInsight verfügbar?](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Voraussetzungen

In vielen Schritten in diesem Dokument werden die folgenden Hilfsprogramme verwendet. Sie müssen diese also möglicherweise auf Ihrem System installieren.

* [cURL](https://curl.haxx.se/) – wird für die Kommunikation mit webbasierten Diensten verwendet
* [jq](https://stedolan.github.io/jq/) – wird verwendet, um JSON-Dokumente zu analysieren
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (Vorschau) – wird zur Remoteverwaltung von Azure-Diensten verwendet

## <a name="users"></a>Benutzer

Sofern es nicht [in eine Domäne eingebunden](hdinsight-domain-joined-introduction.md) ist, sollte HDInsight als **Einzelbenutzersystem** betrachtet werden. Ein SSH-Einzelbenutzerkonto wird mit dem Cluster mit Administratorrechten erstellt. Zusätzliche SSH-Konten können erstellt werden, haben jedoch auch Administratorzugriff auf den Cluster.

In die Domäne eingebundenes HDInsight bietet Unterstützung mehrerer Benutzer sowie präzisere Einstellungen für Berechtigungen und Rollen. Weitere Informationen finden Sie unter [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md) (Verwalten von in die Domäne eingebundenen HDInsight-Clustern).

## <a name="domain-names"></a>Domänennamen

Der vollqualifizierte Domänenname (FQDN) für die Verbindung zum Cluster aus dem Internet lautet **&lt;Clustername>.azurehdinsight.net** oder (nur für SSH) **&lt;Clustername>-ssh.azurehdinsight.net**.

Intern verfügt jeder Knoten im Cluster über einen Namen, der während der Konfiguration des Clusters zugewiesen wird. Die Clusternamen finden Sie auf der Ambari-Webbenutzeroberfläche auf der Seite **Hosts**. Sie können auch Folgendes verwenden, um eine Liste mit Hosts aus der Ambari-REST-API abzurufen:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Ersetzen Sie **PASSWORD** durch das Kennwort des Administratorkontos und **CLUSTERNAME** durch den Namen des Clusters. Dieser Befehl gibt ein JSON-Dokument zurück, das eine Liste der Hosts im Cluster enthält. Jq wird zum Extrahieren des Werts des Elements `host_name` für jeden Host verwendet.

Wenn Sie den Namen des Knotens für einen bestimmten Dienst suchen müssen, können Sie Ambari nach dieser Komponente abfragen. Verwenden Sie beispielsweise den folgenden Befehl, um nach den Hosts für den HDFS-Namensknoten zu suchen:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Mit diesem Befehl wird ein JSON-Dokument mit einer Beschreibung des Diensts zurückgegeben. Anschließend ruft jq nur den `host_name`-Wert für die Hosts ab.

## <a name="remote-access-to-services"></a>Remotezugriff auf Dienste

* **Ambari (Web)** – https://&lt;Clustername>.azurehdinsight.net

    Authentifizieren Sie sich mit dem Benutzernamen und Kennwort des Clusteradministrators, und melden Sie sich anschließend bei Ambari an.

    Die Authentifizierung erfolgt unverschlüsselt. Verwenden Sie immer HTTPS, um eine sichere Verbindung zu gewährleisten.

    > [!IMPORTANT]
    > Einige der über Ambari verfügbaren Webbenutzeroberflächen greifen über einen internen Domänennamen auf Knoten zu. Auf interne Domänennamen besteht kein öffentlicher Zugriff über das Internet. Sie erhalten ggf. die Fehlermeldung, dass der Server nicht gefunden wurde, wenn Sie versuchen, auf einige Features über das Internet zuzugreifen.
    >
    > Damit Sie die Funktionalität der Ambari-Webbenutzeroberfläche vollständig nutzen können, verwenden Sie einen SSH-Tunnel, um den Webdatenverkehr per Proxy an den Clusterhauptknoten weiterzuleiten. Weitere Informationen finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md).

* **Ambari (REST)** – https://&lt;Clustername>.azurehdinsight.net/ambari

    > [!NOTE]
    > Authentifizieren Sie sich mit dem Benutzernamen und Kennwort des Clusteradministrators.
    >
    > Die Authentifizierung erfolgt unverschlüsselt. Verwenden Sie immer HTTPS, um eine sichere Verbindung zu gewährleisten.

* **WebHCat (Templeton)** – https://&lt;Clustername>.azurehdinsight.net/templeton

    > [!NOTE]
    > Authentifizieren Sie sich mit dem Benutzernamen und Kennwort des Clusteradministrators.
    >
    > Die Authentifizierung erfolgt unverschlüsselt. Verwenden Sie immer HTTPS, um eine sichere Verbindung zu gewährleisten.

* **SSH** - &lt;Clustername>-ssh.azurehdinsight.net an Port 22 oder 23. Port 22 dient zum Herstellen einer Verbindung mit dem primären Hauptknoten, während 23 zum Herstellen einer Verbindung mit dem sekundären Knoten verwendet wird. Weitere Informationen zu Hauptknoten finden Sie unter [Verfügbarkeit und Zuverlässigkeit von Hadoop-Clustern in HDInsight](hdinsight-high-availability-linux.md).

    > [!NOTE]
    > Sie können auf einem Clientcomputer nur über SSH auf die Hauptknoten des Clusters zugreifen. Nachdem die Verbindung hergestellt wurde, können Sie von einem Hauptknoten aus über SSH auf die Workerknoten zugreifen.

## <a name="file-locations"></a>Dateispeicherorte

Zu Hadoop zugehörige Dateien befinden sich auf den Clusterknoten in `/usr/hdp`. Dieses Verzeichnis enthält die folgenden Unterverzeichnisse:

* **2.2.4.9-1**: Der Name des Verzeichnisses ist die Version von Hortonworks Data Platform, die von HDInsight verwendet wird. Der Wert Ihres Clusters unterscheidet sich möglicherweise vom hier aufgeführten.
* **current**: Dieses Verzeichnis enthält Links zu den Unterverzeichnissen im Verzeichnis **2.2.4.9-1**. Da dieses Verzeichnis vorhanden ist, müssen Sie sich nicht die Versionsnummer merken.

Beispieldaten und JAR-Dateien finden Sie im Hadoop Distributed File System unter `/example` und `/HdiSamples`.

## <a name="hdfs-azure-storage-and-data-lake-store"></a>HDFS, Azure Storage und Data Lake Store

In den meisten Hadoop-Distributionen wird HDFS auf den Computern im Cluster durch lokalen Speicher gesichert. Die Nutzung von lokalem Speicher kann für eine cloudbasierte Lösung mit zeitabhängiger Abrechnung (pro Stunde oder pro Minute) für Computeressourcen kostenintensiv sein.

HDInsight verwendet als Standardspeicher entweder Blobs in Azure Storage oder Azure Data Lake Store. Diese Dienste bieten die folgenden Vorteile:

* Kostengünstige langfristige Speicherung
* Zugriff über externe Dienste wie Websites, Hilfsprogramme zum Hochladen/Herunterladen von Dateien, SDKs für verschiedene Sprachen und Webbrowser

> [!WARNING]
> HDInsight unterstützt nur __allgemeine__ Azure Storage-Konten. Der Kontotyp __Blobspeicher__ wird derzeit nicht unterstützt.

Ein Azure Storage-Konto kann bis zu 4,75 TB an Daten aufnehmen, obwohl einzelne Blobs (oder Dateien aus HDInsight-Sicht) nur bis zu 195 GB enthalten können. Azure Data Lake Store kann dynamisch wachsen, und Billionen von Dateien aufnehmen, wobei einzelne Dateien größer als ein Petabyte sein können. Weitere Informationen finden Sie unter [Grundlegendes zu Blobs](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs) und [Data Lake Store](https://azure.microsoft.com/services/data-lake-store/).

Wenn Sie entweder Azure Storage oder Data Lake Store nutzen, müssen Sie in HDInsight für den Datenzugriff keine besonderen Schritte ausführen. Mit dem folgenden Befehl werden Dateien im Ordner `/example/data` beispielsweise unabhängig davon aufgelistet, ob dieser in Azure Storage oder Data Lake Store gespeichert ist:

    hdfs dfs -ls /example/data

### <a name="uri-and-scheme"></a>URI und Schema

Einige Befehle erfordern ggf. die Angabe des Schemas als Teil des URI, wenn auf eine Datei zugegriffen wird. Beispielsweise erfordert die Komponente Storm-HDFS, dass Sie das Schema angeben. Bei Verwendung von nicht standardmäßigem Speicher (der dem Cluster als „zusätzlicher“ Speicher hinzugefügt wird), müssen Sie stets das Schema als Teil des URI angeben.

Nutzen Sie bei Verwendung von __Azure Storage__ eines der folgenden URI-Schemas:

* `wasb:///`: Zugriff auf Standardspeicher über unverschlüsselte Verbindungen.

* `wasbs:///`: Zugriff auf Standardspeicher über verschlüsselte Verbindungen.

* `wasbs://<container-name>@<account-name>.blob.core.windows.net/`: Wird bei einer Verbindung mit einem nicht standardmäßigen Speicherkonto verwendet. Beispielsweise wenn Sie ein zusätzliches Speicherkonto haben oder auf Daten in einem öffentlich zugänglichen Speicherkonto zugreifen.

Nutzen Sie bei Verwendung von __Data Lake Store__ eines der folgenden URI-Schemas:

* `adl:///`: Zugriff auf den standardmäßigen Data Lake Store für den Cluster.

* `adl://<storage-name>.azuredatalakestore.net/`: Wird bei einer Verbindung mit einem nicht standardmäßigen Data Lake Store verwendet. Wird auch verwendet, um auf Daten außerhalb des Stammverzeichnisses Ihres HDInsight-Clusters zuzugreifen.

> [!IMPORTANT]
> Wenn Data Lake Store als Standardspeicher für HDInsight verwendet wird, müssen Sie einen Pfad innerhalb des Speichers als Stamm des HDInsight-Speichers angeben. Der Standardpfad lautet `/clusters/<cluster-name>/`.
>
> Bei Verwendung von `/` oder `adl:///` für den Zugriff auf Daten können Sie nur auf Daten zugreifen, die im Stammverzeichnis (z.B. `/clusters/<cluster-name>/`) des Clusters gespeichert sind. Um auf Daten an beliebiger Stelle im Speicher zuzugreifen, verwenden Sie das Format `adl://<storage-name>.azuredatalakestore.net/`.

### <a name="what-storage-is-the-cluster-using"></a>Der vom Cluster verwendete Speicher

Mithilfe von Ambari können Sie die Standardspeicherkonfiguration des Clusters abrufen. Verwenden Sie den folgenden Befehl, um HDFS-Konfigurationsinformationen über „curl“ abzurufen, und filtern Sie sie mit [jq](https://stedolan.github.io/jq/):

```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'```

> [!NOTE]
> Hierdurch wird die erste auf den Server angewendete Konfiguration (`service_config_version=1`) zurückgegeben, die diese Informationen enthält. Möglicherweise müssen Sie alle Konfigurationsversionen auflisten, um die neueste zu finden.

Dieser Befehl gibt einen Wert zurück, der in etwa wie die folgenden URIs aussieht:

* `wasbs://<container-name>@<account-name>.blob.core.windows.net` bei Verwenden eines Azure Storage-Kontos.

    Der Kontoname ist der Name des Azure Storage-Kontos, während der Containername zum Blobcontainer gehört, der das Stammverzeichnis des Clusterspeichers darstellt.

* `adl://home` bei Verwenden von Azure Data Lake Store. Verwenden Sie zum Abrufen des Data Lake Store-Namens den folgenden REST-Aufruf:

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'```

    Mit diesem Befehl wird der folgende Hostname zurückgegeben: `<data-lake-store-account-name>.azuredatalakestore.net`.

    Verwenden Sie zum Abrufen des Verzeichnisses innerhalb des Speichers, der das Stammverzeichnis für HDInsight ist, den folgenden REST-Aufruf:

    ```curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'```

    Mit diesem Befehl wird ein Pfad zurückgegeben, der in etwa wie folgt aussieht: `/clusters/<hdinsight-cluster-name>/`.

Sie können die Speicherinformationen auch über das Azure-Portal finden, indem Sie die folgenden Schritte ausführen:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) Ihren HDInsight-Cluster aus.

2. Wählen Sie im Abschnitt **Eigenschaften** den Eintrag **Speicherkonten** aus. Die Speicherinformationen für den Cluster werden angezeigt.

### <a name="how-do-i-access-files-from-outside-hdinsight"></a>Zugreifen auf Daten außerhalb von HDInsight

Es gibt viele verschiedene Möglichkeiten, wie Sie außerhalb des HDInsight-Clusters auf Daten zugreifen können. Es folgen einige Links zu Hilfsprogrammen und SDKs, die zum Arbeiten mit Ihren Daten verwendet werden können:

Wenn Sie __Azure Storage__ nutzen, finden Sie unter den folgenden Links Methoden für den Zugriff auf Ihre Daten:

* [Azure CLI 2.0:](https://docs.microsoft.com/cli/azure/install-az-cli2) Befehlszeilenschnittstelle für die Arbeit mit Azure. Verwenden Sie nach der Installation den Befehl `az storage`, um Hilfe zur Speicherung abzurufen, oder `az storage blob` für blobspezifische Befehle.
* [blobxfer.py](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): Python-Skript zum Arbeiten mit Blobs im Azure-Speicher.
* Verschiedene SDKs:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Storage-REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

Wenn Sie __Azure Data Lake Store__ nutzen, finden Sie unter den folgenden Links Methoden für den Zugriff auf Ihre Daten:

* [Webbrowser](../data-lake-store/data-lake-store-get-started-portal.md)
* [PowerShell](../data-lake-store/data-lake-store-get-started-powershell.md)
* [Azure CLI 2.0](../data-lake-store/data-lake-store-get-started-cli-2.0.md)
* [WebHDFS-REST-API](../data-lake-store/data-lake-store-get-started-rest-api.md)
* [Data Lake-Tools für Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504)
* [.NET](../data-lake-store/data-lake-store-get-started-net-sdk.md)
* [Java](../data-lake-store/data-lake-store-get-started-java-sdk.md)
* [Python](../data-lake-store/data-lake-store-get-started-python.md)

## <a name="scaling"></a>Skalieren des Clusters

Mithilfe der Clusterskalierung können Sie die Anzahl der von einem Cluster verwendeten Datenknoten ändern. Sie können Skalierungsvorgänge ausführen, während andere Aufträge oder Prozesse auf einem Cluster ausgeführt werden.

Folgende Clustertypen sind von der Skalierung betroffen:

* **Hadoop**: Wenn Sie die Anzahl von Knoten in einem Cluster zentral herunterskalieren, werden einige Dienste im Cluster neu gestartet. Skalierungsvorgänge können bei ihrem Abschluss bei aktiven und ausstehenden Aufträgen zu einem Fehler führen. Sie können die Aufträge nach Abschluss des Vorgangs erneut senden.
* **HBase**: Regionale Server werden innerhalb weniger Minuten nach Abschluss des Skalierungsvorgangs automatisch ausgeglichen. Um regionale Server manuell auszugleichen, führen Sie folgende Schritte aus:

    1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Weitere Informationen finden Sie unter [Verwenden von SSH mit Linux-basiertem Hadoop in HDInsight unter Linux, Unix oder OS X](hdinsight-hadoop-linux-use-ssh-unix.md).

    2. Starten Sie die HBase-Shell wie folgt:

            hbase shell

    3. Sobald die HBase-Shell geladen ist, führen Sie folgende Schritte aus, um die regionalen Server manuell auszugleichen:

            balancer

* **Storm**: Sie sollten alle ausgeführten Storm-Topologien erneut ausgleichen, nachdem ein Skalierungsvorgang durchgeführt wurde. Durch eine Neuverteilung kann die Topologie die Parallelitätseinstellungen an die neue Anzahl von Knoten im Cluster anpassen. Verwenden Sie eine der folgenden Optionen, um ausgeführte Topologien erneut auszugleichen:

    * **SSH**: Stellen Sie eine Verbindung zum Server her, und verwenden Sie folgenden Befehl, um eine Topologie erneut auszugleichen:

            storm rebalance TOPOLOGYNAME

        Sie können auch Parameter angeben, um die ursprünglich von der Topologie bereitgestellten Parallelitätshinweise zu überschreiben. Der Befehl `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` konfiguriert z.B. die Topologie mit 5 Workerprozessen, 3 Executors für die Komponente „blue-spout“ und 10 Executors für die Komponente „yellow-bolt“ neu.

    * **Storm-Benutzeroberfläche**: Führen Sie die folgenden Schritte aus, um eine Topologie mithilfe der Storm-Benutzeroberfläche erneut auszugleichen.

        1. Öffnen Sie **https://CLUSTERNAME.azurehdinsight.net/stormui** im Webbrowser, wobei CLUSTERNAME der Name Ihres Storm-Clusters ist. Geben Sie bei entsprechender Aufforderung den Namen und das Kennwort des HDInsight-Clusteradministrators ein, die Sie beim Erstellen des Clusters festgelegt haben.
        2. Wählen Sie die Topologie aus, die Sie erneut ausgleichen möchten, und klicken Sie dann auf die Schaltfläche **Neu ausgleichen**. Geben Sie die Verzögerung ein, bevor der Neuausgleich ausgeführt wird.

Detaillierte Informationen zum Skalieren von HDInsight-Clustern finden Sie hier:

* [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Verwalten von Hadoop-Clustern in HDInsight mit Azure PowerShell](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Wie installiere ich Hue (oder eine andere Hadoop-Komponente)?

HDInsight ist ein verwalteter Dienst. Wenn Azure ein Problem mit dem Cluster erkennt, kann der betroffene Knoten gelöscht und ein Ersatzknoten erstellt werden. Falls Sie Komponenten manuell im Cluster installieren, werden sie nicht beibehalten, wenn dieser Vorgang erfolgt. Verwenden Sie stattdessen [HDInsight-Skriptaktionen](hdinsight-hadoop-customize-cluster.md). Sie können eine Skriptaktion verwenden, um die folgenden Änderungen vorzunehmen:

* Installieren und Konfigurieren eines Diensts oder einer Website, z.B. Spark oder Hue.
* Installieren und Konfigurieren einer Komponente, für die Konfigurationsänderungen auf mehreren Knoten im Cluster erforderlich sind. Z. B. eine erforderliche Umgebungsvariable, Erstellen eines Protokollierungsverzeichnisses oder Erstellen einer Konfigurationsdatei.

Bei Skriptaktionen handelt es sich um Bash-Skripts. Die Skripts werden während der Clusterbereitstellung ausgeführt und können zur Installation und Konfiguration zusätzlicher Komponenten im Cluster verwendet werden. Zur Installation der folgenden Komponenten werden Beispielskripts bereitgestellt:

* [Hue](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Informationen zum Entwickeln eigener Skriptaktionen finden Sie unter [Entwickeln von Skriptaktionen mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="jar-files"></a>JAR-Dateien

Einige Hadoop-Technologien werden in eigenständigen JAR-Dateien mit Funktionen bereitgestellt, die als Teil eines MapReduce-Auftrags oder aus Pig oder Hive verwendet werden. Diese können mit Skriptaktionen erstellt werden, häufig ist jedoch kein Setup erforderlich und sie können nach der Bereitstellung in den Cluster hochgeladen und direkt verwendet werden. Wenn Sie sicherstellen möchten, dass die Komponente bei einem Re-Imaging des Clusters nicht beschädigt wird, können Sie die JAR-Datei im Standardspeicher Ihres Clusters (WASB oder ADL) speichern.

Wenn Sie beispielsweise die neueste Version von [DataFu](http://datafu.incubator.apache.org/)verwenden möchten, können Sie eine JAR-Datei mit dem Projekt herunterladen und in den HDInsight-Cluster hochladen. Führen Sie dann die Anweisungen in der DataFu-Dokumentation aus, um sie aus Pig oder Hive zu verwenden.

> [!IMPORTANT]
> Einige Komponenten, bei denen es sich um eigenständige JAR-Dateien handelt, werden mit HDInsight bereitgestellt, sind jedoch nicht im Pfad vorhanden. Wenn Sie eine bestimmte Komponente suchen, können Sie sie hiermit im Cluster suchen:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Mit diesem Befehl wird der Pfad aller übereinstimmenden JAR-Dateien zurückgegeben.

Um eine andere Version einer Komponente zu verwenden, laden Sie die benötigte Version hoch, und verwenden Sie sie in Ihren Aufträgen.

> [!WARNING]
> Komponenten, die mit dem HDInsight-Cluster bereitgestellt werden, werden vollständig unterstützt, und Microsoft Support hilft Ihnen, Probleme im Zusammenhang mit diesen Komponenten zu isolieren und zu beheben.
>
> Für benutzerdefinierte Komponenten steht kommerziell angemessener Support für eine weiterführende Behebung des Problems zur Verfügung. Auf diese Weise kann das Problem behoben werden, ODER Sie werden aufgefordert, verfügbare Kanäle für Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. So können z. B. viele Communitywebsites verwendet werden, wie: das [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Für Apache-Projekte gibt es Projektwebsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Nächste Schritte

* [Migrieren von Windows-basiertem HDInsight zum Linux-basierten](hdinsight-migrate-from-windows-to-linux.md)
* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce-Aufträgen mit HDInsight](hdinsight-use-mapreduce.md)

