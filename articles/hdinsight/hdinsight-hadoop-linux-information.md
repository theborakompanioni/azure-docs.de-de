---
title: Tipps zur Verwendung von Hadoop unter Linux-basiertem HDInsight | Microsoft Docs
description: "Hier erhalten Sie Implementierungstipps für die Verwendung von Linux-basierten HDInsight (Hadoop)-Clustern in einer vertrauten Linux-Umgebung, die in der Azure-Cloud ausgeführt wird."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c41c611c-5798-4c14-81cc-bed1e26b5609
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 854df55045d4d9a062ade3905b9be1daad0a7d8c
ms.openlocfilehash: 3ce0444427ba10e7247aec500017ea4bae3af161


---
# <a name="information-about-using-hdinsight-on-linux"></a>Informationen zur Verwendung von HDInsight unter Linux

Auf Linux basierende Azure HDInsight-Cluster stellen Hadoop in einer vertrauten Linux-Umgebung bereit, die in der Azure-Cloud ausgeführt wird. Die Lösung sollte sich größtenteils genauso wie jede andere Installation von Hadoop unter Linux verhalten. In diesem Dokument werden bestimmte Unterschiede aufgeführt, die Sie kennen sollten.

## <a name="prerequisites"></a>Voraussetzungen

In vielen Schritten in diesem Dokument werden die folgenden Hilfsprogramme verwendet. Sie müssen diese also möglicherweise auf Ihrem System installieren.

* [cURL](https://curl.haxx.se/) – wird für die Kommunikation mit webbasierten Diensten verwendet
* [jq](https://stedolan.github.io/jq/) – wird verwendet, um JSON-Dokumente zu analysieren
* [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) (Vorschau) – wird zur Remoteverwaltung von Azure-Diensten verwendet

## <a name="users"></a>Benutzer

Sofern es nicht [in eine Domäne eingebunden](hdinsight-domain-joined-introduction.md) ist, sollte HDInsight als **Einzelbenutzersystem** betrachtet werden. Ein SSH-Einzelbenutzerkonto wird mit dem Cluster mit Administratorrechten erstellt. Zusätzliche SSH-Konten können erstellt werden, haben jedoch auch Administratorzugriff auf den Cluster.

In die Domäne eingebundenes HDInsight bietet Unterstützung für mehrere Benutzer sowie präzisere Einstellungen für Berechtigungen und Rollen. Weitere Informationen finden Sie unter [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md) (Verwalten von in die Domäne eingebundenen HDInsight-Clustern).

## <a name="domain-names"></a>Domänennamen

Der vollqualifizierte Domänenname (FQDN) für die Verbindung zum Cluster aus dem Internet lautet **&lt;Clustername>.azurehdinsight.net** oder (nur für SSH) **&lt;Clustername>-ssh.azurehdinsight.net**.

Intern verfügt jeder Knoten im Cluster über einen Namen, der während der Konfiguration des Clusters zugewiesen wird. Zum Suchen der Clusternamen können Sie die Seite **Hosts** auf der Ambari-Webbenutzeroberfläche besuchen oder wie folgt eine Liste der Hosts über die Ambari REST-API zurückgeben:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts" | jq '.items[].Hosts.host_name'

Ersetzen Sie **PASSWORD** durch das Kennwort des Administratorkontos und **CLUSTERNAME** durch den Namen des Clusters. Hiermit wird ein JSON-Dokument mit einer Liste der Hosts im Cluster zurückgegeben, dann ruft jq den Wert des `host_name` -Elements für die einzelnen Hosts im Cluster ab.

Wenn Sie den Namen des Knotens für einen bestimmten Dienst suchen müssen, können Sie Ambari nach dieser Komponente abfragen. Beispiel: Um die Hosts für den HDFS-Namensknoten zu suchen, verwenden Sie Folgendes.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'

Hiermit wird ein JSON-Dokument mit einer Beschreibung des Diensts zurückgegeben, dann ruft jq nur den `host_name` -Wert für die Hosts ab.

## <a name="remote-access-to-services"></a>Remotezugriff auf Dienste

* **Ambari (Web)** – https://&lt;Clustername>.azurehdinsight.net

    Authentifizieren Sie sich mit dem Benutzernamen und Kennwort des Clusteradministrators, und melden Sie sich anschließend bei Ambari an. Hierbei werden ebenfalls Benutzername und Kennwort des Clusteradministrators verwendet.

    Die Authentifizierung erfolgt unverschlüsselt. Verwenden Sie immer HTTPS, um eine sichere Verbindung zu gewährleisten.

    > [!IMPORTANT]
    > Während Ambari für Ihren Cluster direkt über das Internet zugänglich ist, benötigen einige Funktionen den Knotenzugriff über den internen Domänennamen, der vom Cluster verwendet wird. Da dies ein interner und somit nicht öffentlicher Domänenname ist, erhalten Sie die Fehlermeldung, dass der Server nicht gefunden wurde, wenn Sie versuchen, auf einige Features über das Internet zuzugreifen.
    >
    > Damit Sie die Funktionalität der Ambari-Webbenutzeroberfläche vollständig nutzen können, verwenden Sie einen SSH-Tunnel, um den Webdatenverkehr per Proxy an den Clusterhauptknoten weiterzuleiten. Weitere Informationen finden Sie unter [Verwenden von SSH-Tunneling zum Zugriff auf die Ambari-Webbenutzeroberfläche, ResourceManager, JobHistory, NameNode, Oozie und andere Webbenutzeroberflächen](hdinsight-linux-ambari-ssh-tunnel.md)

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

* **2.2.4.9-1**: Dieses Verzeichnis wird gemäß der Version von Hortonworks Data Platform benannt, die von HDInsight verwendet wird. Die Zahl für Ihren Cluster kann sich daher von der hier angegebenen Zahl unterscheiden.
* **current**: Dieses Verzeichnis enthält Links zu Verzeichnissen im Verzeichnis **2.2.4.9-1**. Dadurch müssen Sie nicht bei jedem Dateizugriff eine Versionsnummer eingeben (die sich außerdem ändern kann).

Beispieldaten und JAR-Dateien für Hadoop Distributed File System (HDFS) oder Azure-Blobspeicher finden Sie unter '/example' oder 'wasbs:///example'.

## <a name="hdfs-azure-blob-storage-and-storage-best-practices"></a>Bewährte Methoden für HDFS, Azure-Blobspeicher und Speicher

In den meisten Hadoop-Distributionen wird HDFS auf den Computern im Cluster durch lokalen Speicher gesichert. Obwohl dies effizient ist, kann es für eine cloudbasierte Lösung mit zeitabhängiger Abrechnung (pro Stunde oder pro Minute) für Computeressourcen kostenintensiv sein.

HDInsight verwendet Azure-Blobspeicher als Standardspeicher, der folgende Vorteile bietet:

* Kostengünstige langfristige Speicherung
* Zugriff über externe Dienste wie Websites, Hilfsprogramme zum Hochladen/Herunterladen von Dateien, SDKs für verschiedene Sprachen und Webbrowser

Da dies der Standardspeicher für HDInsight ist, sind für seine Verwendung in der Regel keine weiteren Maßnahmen erforderlich. Der folgende Befehl listet z.B. Dateien im Ordner **/example/data** auf, der in Azure Blob Storage gespeichert wird:

    hdfs dfs -ls /example/data

Bei einigen Befehlen müssen Sie möglicherweise angeben, dass Sie Blobspeicher verwenden. Für diese Befehle können Sie die Zeichenfolge **wasb://** oder **wasbs://** voranstellen.

Mit HDInsight können Sie auch mehrere Blobspeicherkonten einem Cluster zuordnen. Für den Zugriff auf Daten in einem nicht standardmäßigen Blobspeicherkonto können Sie das Format **wasbs://&lt;container-name>@&lt;Kontoname>.blob.core.windows.net/** verwenden. Der folgende Befehl listet z.B. den Inhalt des Verzeichnisses **/example/data** für den angegebenen Container und das Blobspeicherkonto auf:

    hdfs dfs -ls wasbs://mycontainer@mystorage.blob.core.windows.net/example/data

### <a name="what-blob-storage-is-the-cluster-using"></a>Welchen Blobspeicher verwendet der Cluster?

Während der Clustererstellung haben Sie entweder die Verwendung eines vorhandenen Azure-Speicherkontos und -containers ausgewählt, oder Sie erstellen diese neu. Dann haben Sie diese möglicherweise wieder vergessen. Sie können das Speicherkonto und den Container mithilfe der Ambari-REST-API finden.

1. Verwenden Sie den folgenden Befehl, um HDFS-Konfigurationsinformationen über „curl“ abzurufen, und filtern Sie sie mit [jq](https://stedolan.github.io/jq/):

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'

    > [!NOTE]
    > Hierdurch wird die erste auf den Server angewendete Konfiguration (`service_config_version=1`) zurückgegeben, die diese Information enthält. Wenn Sie einen Wert abrufen, der nach der Erstellung des Clusters geändert wurde, müssen Sie möglicherweise die Konfigurationsversionen auflisten und die letzte Version abrufen.

    Dadurch wird ein ähnlicher Wert wie der folgende zurückgegeben, wobei **CONTAINER** der Standardcontainer und **ACCOUNTNAME** der Name des Azure Storage-Kontos ist:

        wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

2. Verwenden Sie den folgenden Befehl, um die eindeutige ID für das Speicherkonto abzurufen. Ersetzen Sie im folgenden Befehl **ACCOUNTNAME** durch den aus Ambari abgerufenen Namen des Speicherkontos:

        az storage account list --query "[?name=='ACCOUNTNAME'].id --out list

3. Verwenden Sie den folgenden Befehl, um einen Schlüssel für das Speicherkonto abzurufen. Ersetzen Sie **STORAGEID** durch die ID des Speicherkontos:

        az storage account keys list --ids STORAGEID --out list

    Hierdurch wird der Primärschlüssel für das Konto zurückgegeben.

Sie können die Speicherinformationen auch über das Azure-Portal finden:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/)Ihren HDInsight-Cluster aus.
2. Klicken Sie im Abschnitt **Zusammenfassung** auf **Alle Einstellungen**.
3. Wählen Sie unter **Einstellungen** die Option **Azure-Speicherschlüssel** aus.
4. Wählen Sie unter **Azure-Speicherschlüssel** eines der aufgeführten Speicherkonten aus. Damit werden Informationen zum Speicherkonto angezeigt.
5. Wählen Sie das Schlüsselsymbol aus. Damit werden die Schlüssel für dieses Speicherkonto angezeigt.

### <a name="how-do-i-access-blob-storage"></a>Wie kann ich auf Blobspeicher zugreifen?

Neben dem vom Cluster aus gestarteten Hadoop-Befehl steht eine Vielzahl von Möglichkeiten für den Zugriff auf Blobs zur Verfügung:

* [Azure CLI 2.0:](https://docs.microsoft.com/cli/azure/install-az-cli2) Befehlszeilenschnittstelle für die Arbeit mit Azure. Verwenden Sie nach der Installation den Befehl `az storage`, um Hilfe zur Speicherung abzurufen, oder `az storage blob` für blobspezifische Befehle.
* [blobxfer.py](https://github.com/Azure/azure-batch-samples/tree/master/Python/Storage): Python-Skript zum Arbeiten mit Blobs im Azure-Speicher.
* Eine Vielzahl von SDKs:

    * [Java](https://github.com/Azure/azure-sdk-for-java)
    * [Node.js](https://github.com/Azure/azure-sdk-for-node)
    * [PHP](https://github.com/Azure/azure-sdk-for-php)
    * [Python](https://github.com/Azure/azure-sdk-for-python)
    * [Ruby](https://github.com/Azure/azure-sdk-for-ruby)
    * [.NET](https://github.com/Azure/azure-sdk-for-net)
    * [Storage-REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx)

## <a name="a-namescalingascaling-your-cluster"></a><a name="scaling"></a>Skalieren des Clusters

Mithilfe der Clusterskalierung können Sie die Anzahl der von einem in Azure HDInsight ausgeführten Cluster verwendeten Datenknoten ändern, ohne den Cluster löschen und neu erstellen zu müssen.

Sie können Skalierungsvorgänge ausführen, während andere Aufträge oder Prozesse auf einem Cluster ausgeführt werden.

Folgende Clustertypen sind von der Skalierung betroffen:

* **Hadoop**: Wenn Sie die Anzahl von Knoten in einem Cluster zentral herunterskalieren, werden einige Dienste im Cluster neu gestartet. Dies kann beim Abschluss des Skalierungsvorgangs bei aktiven und ausstehenden Aufträgen zu einem Fehler führen. Sie können die Aufträge nach Abschluss des Vorgangs erneut senden.
* **HBase**: Regionale Server werden innerhalb weniger Minuten nach Abschluss des Skalierungsvorgangs automatisch ausgeglichen. Um regionale Server manuell auszugleichen, führen Sie folgende Schritte aus:

    1. Stellen Sie mithilfe von SSH eine Verbindung mit dem HDInsight-Cluster her. Weitere Informationen zur Verwendung von SSH mit HDInsight finden Sie in den folgenden Dokumenten:

        * [Verwenden von SSH mit HDInsight unter Linux, Unix und Mac OS X](hdinsight-hadoop-linux-use-ssh-unix.md)
        * [Verwenden von SSH (PuTTY) mit HDInsight unter Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

    2. Starten Sie die HBase-Shell wie folgt:

            hbase shell
    
    3. Sobald die HBase-Shell geladen ist, führen Sie folgende Schritte aus, um die regionalen Server manuell auszugleichen:

            balancer

* **Storm**: Sie sollten alle ausgeführten Storm-Topologien erneut ausgleichen, nachdem ein Skalierungsvorgang durchgeführt wurde. So kann die Topologie die Parallelitätseinstellungen an die neue Anzahl von Knoten im Cluster anpassen. Verwenden Sie eine der folgenden Optionen, um ausgeführte Topologien erneut auszugleichen:

    * **SSH**: Stellen Sie eine Verbindung zum Server her, und verwenden Sie folgenden Befehl, um eine Topologie erneut auszugleichen:

            storm rebalance TOPOLOGYNAME

        Sie können auch Parameter angeben, um die ursprünglich von der Topologie bereitgestellten Parallelitätshinweise zu überschreiben. Der Befehl `storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10` z. B. konfiguriert die Topologie neu mit 5 Workerprozessen, 3 Executors für die Komponente "blue-spout" und 10 Executors für die Komponente "yellow-bolt".

    * **Storm-Benutzeroberfläche**: Führen Sie die folgenden Schritte aus, um eine Topologie mithilfe der Storm-Benutzeroberfläche erneut auszugleichen.

        1. Öffnen Sie **https://CLUSTERNAME.azurehdinsight.net/stormui** im Webbrowser, wobei CLUSTERNAME der Name Ihres Storm-Clusters ist. Geben Sie bei entsprechender Aufforderung den Namen und das Kennwort des HDInsight-Clusteradministrators ein, die Sie beim Erstellen des Clusters festgelegt haben.
        2. Wählen Sie die Topologie aus, die Sie erneut ausgleichen möchten, und klicken Sie dann auf die Schaltfläche **Neu ausgleichen**. Geben Sie die Verzögerung ein, bevor der Neuausgleich ausgeführt wird.

Detaillierte Informationen zum Skalieren von HDInsight-Clustern finden Sie hier:

* [Verwalten von Hadoop-Clustern in HDInsight mit dem Azure-Portal](hdinsight-administer-use-portal-linux.md#scale-clusters)
* [Verwalten von Hadoop-Clustern in HDInsight mit Azure PowerShell](hdinsight-administer-use-command-line.md#scale-clusters)

## <a name="how-do-i-install-hue-or-other-hadoop-component"></a>Wie installiere ich Hue (oder eine andere Hadoop-Komponente)?

HDInsight ist ein verwalteter Dienst – dies bedeutet, dass Knoten in einem Cluster von Azure automatisch gelöscht und erneut bereitgestellt werden können, wenn ein Problem erkannt wird. Aus diesem Grund empfiehlt es sich nicht, Komponenten manuell auf den Clusterknoten zu installieren. Verwenden Sie stattdessen [HDInsight-Skriptaktionen](hdinsight-hadoop-customize-cluster.md) , wenn Sie Folgendes installieren müssen:

* Einen Dienst oder eine Website, z. B. Spark oder Hue.
* Eine Komponente, für die Konfigurationsänderungen auf mehreren Knoten im Cluster erforderlich sind. Z. B. eine erforderliche Umgebungsvariable, Erstellen eines Protokollierungsverzeichnisses oder Erstellen einer Konfigurationsdatei.

Bei Skriptaktionen handelt es sich um Bash-Skripts, die während der Clusterbereitstellung ausgeführt werden und zur Installation und Konfiguration zusätzlicher Komponenten auf dem Cluster verwendet werden können. Zur Installation der folgenden Komponenten werden Beispielskripts bereitgestellt:

* [Hue](hdinsight-hadoop-hue-linux.md)
* [Giraph](hdinsight-hadoop-giraph-install-linux.md)
* [Solr](hdinsight-hadoop-solr-install-linux.md)

Informationen zum Entwickeln eigener Skriptaktionen finden Sie unter [Entwickeln von Skriptaktionen mit HDInsight](hdinsight-hadoop-script-actions-linux.md).

### <a name="jar-files"></a>JAR-Dateien

Einige Hadoop-Technologien werden in eigenständigen JAR-Dateien mit Funktionen bereitgestellt, die als Teil eines MapReduce-Auftrags oder aus Pig oder Hive verwendet werden. Diese können mit Skriptaktionen erstellt werden, häufig ist jedoch kein Setup erforderlich und sie können nach der Bereitstellung einfach in den Cluster hochgeladen und direkt verwendet werden. Wenn Sie sicherstellen möchten, dass die Komponente bei einem Reimaging des Clusters nicht beschädigt wird, können Sie die JAR-Datei im WASB speichern.

Wenn Sie beispielsweise die neueste Version von [DataFu](http://datafu.incubator.apache.org/)verwenden möchten, können Sie eine JAR-Datei mit dem Projekt herunterladen und in den HDInsight-Cluster hochladen. Führen Sie dann die Anweisungen in der DataFu-Dokumentation aus, um sie aus Pig oder Hive zu verwenden.

> [!IMPORTANT]
> Einige Komponenten, bei denen es sich um eigenständige JAR-Dateien handelt, werden mit HDInsight bereitgestellt, sind jedoch nicht im Pfad vorhanden. Wenn Sie eine bestimmte Komponente suchen, können Sie sie hiermit im Cluster suchen:
>
> ```find / -name *componentname*.jar 2>/dev/null```
>
> Dadurch wird den Pfad aller entsprechenden JAR-Dateien zurückgegeben.

Wenn der Cluster bereits eine Version einer Komponente als eigenständige JAR-Datei enthält, Sie jedoch eine andere Version verwenden möchten, können Sie eine neue Version der Komponente in den Cluster hochladen und versuchen, sie in Ihren Aufträgen zu verwenden.

> [!WARNING]
> Komponenten, die mit dem HDInsight-Cluster bereitgestellt werden, werden vollständig unterstützt, und Microsoft Support hilft Ihnen, Probleme im Zusammenhang mit diesen Komponenten zu isolieren und zu beheben.
>
> Für benutzerdefinierte Komponenten steht kommerziell angemessener Support für eine weiterführende Behebung des Problems zur Verfügung. Auf diese Weise kann das Problem behoben werden, ODER Sie werden aufgefordert, verfügbare Kanäle für Open-Source-Technologien in Anspruch zu nehmen, die über umfassende Kenntnisse für diese Technologien verfügen. So können z. B. viele Communitywebsites verwendet werden, wie: das [MSDN-Forum für HDInsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Für Apache-Projekte gibt es Projektwebsites auf [http://apache.org](http://apache.org), zum Beispiel: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).

## <a name="next-steps"></a>Nächste Schritte

* [Migrieren von Windows-basiertem HDInsight zum Linux-basierten](hdinsight-migrate-from-windows-to-linux.md)
* [Verwenden von Hive mit HDInsight](hdinsight-use-hive.md)
* [Verwenden von Pig mit HDInsight](hdinsight-use-pig.md)
* [Verwenden von MapReduce-Aufträgen mit HDInsight](hdinsight-use-mapreduce.md)



<!--HONumber=Dec16_HO1-->


