---
title: "Überwachen und Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-REST-API | Microsoft Docs"
description: "Erfahren Sie, wie Sie Ambari zum Überwachen und Verwalten von Linux-basierten HDInsight-Clustern verwenden. In diesem Dokument erfahren Sie, wie Sie die in HDInsight-Clustern enthaltene Ambari-REST-API verwenden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/02/2016
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 11523a95a8eac687d391c23cc1e29fa0684a40ae
ms.openlocfilehash: 2723716eb1ec823809d34ce1698d25a0ed9d012b


---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Verwalten von HDInsight-Clustern mithilfe der Ambari-REST-API

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereinfacht die Verwaltung und Überwachung von Hadoop-Clustern durch die Bereitstellung einer benutzerfreundlichen Webbenutzeroberfläche und REST-API. Ambari ist in Linux-basierten HDInsight-Clustern enthalten und wird verwendet, um den Cluster zu überwachen und Konfigurationsänderungen vorzunehmen. Dieses Dokument enthält grundlegende Informationen zur Verwendung der Ambari-REST-API im Rahmen allgemeiner Aufgaben mit cURL.

> [!IMPORTANT]
> Die Beispiele in diesem Dokument wurden mithilfe von PowerShell unter Windows 10 und Bash getestet. In vielen Fällen können die Befehle für beides verwendet werden. Ist dies nicht der Fall, steht sowohl ein PowerShell- als auch ein Bash-Beispiel zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

* [cURL](http://curl.haxx.se/): cURL ist ein plattformübergreifendes Hilfsprogramm, das zum Arbeiten mit REST-APIs über die Befehlszeile verwendet werden kann. In diesem Dokument wird es für die Kommunikation mit der Ambari-REST-API verwendet.

    > [!WARNING]
    > Bei Verwendung von PowerShell müssen Sie den Standardalias für den Befehl `curl` mithilfe des Befehls `remove-item alias:curl` entfernen. Wenn Sie den Alias nicht entfernen, tritt ein Fehler wie der folgende auf: „Der Parameter kann nicht verarbeitet werden, da der Parametername "u" nicht eindeutig ist.“

* [jq](https://stedolan.github.io/jq/): jq ist ein plattformübergreifendes Befehlszeilenprogramm zum Arbeiten mit JSON-Dokumenten. In diesem Dokument wird es zum Analysieren der von der Ambari REST-API zurückgegebenen JSON-Dokumente verwendet.

* [Azure CLI 2.0 (Vorschau)](https://docs.microsoft.com/cli/azure/install-az-cli2): Ein plattformübergreifendes Befehlszeilenprogramm für die Verwendung von Azure-Diensten.
  
[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)] 

## <a name="a-idwhatisawhat-is-ambari"></a><a id="whatis"></a>Was ist Ambari?

[Apache Ambari](http://ambari.apache.org) vereinfacht die Hadoop-Verwaltung durch die Bereitstellung einer benutzerfreundlichen Webbenutzeroberfläche, die zum Bereitstellen, Verwalten und Überwachen von Hadoop-Clustern verwendet werden kann. Entwickler können diese Funktionen mithilfe der [Ambari-REST-APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)in ihre Anwendungen integrieren.

Ambari wird standardmäßig mit Linux-basierten Clustern bereitgestellt.

## <a name="rest-api"></a>REST-API

Der Basis-URI für die Ambari-REST-API in HDInsight-Clustern lautet https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, wobei **CLUSTERNAME** für den Namen Ihres Clusters steht.

> [!IMPORTANT]
> Beim Clusternamen im vollqualifizierten Domänennamen (FQDN) innerhalb des URI (CLUSTERNAME.azurehdinsight.net) wird die Groß-/Kleinschreibung nicht beachtet, bei anderen Vorkommen im URI dagegen schon. Wenn Ihr Cluster beispielsweise den Namen MyCluster hat, sind folgende URIs gültig:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> Bei den folgenden URIs wird ein Fehler zurückgegeben, da die Groß-/Kleinschreibung beim zweiten Vorkommen des Namens nicht korrekt ist:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Zum Herstellen einer Verbindung mit Ambari in HDInsight ist HTTPS erforderlich. Bei der Verbindungsauthentifizierung müssen Sie den Administratorkontonamen (standardmäßig **admin**) und das Kennwort verwenden, die Sie bei der Erstellung des Clusters angegeben haben.

Im folgenden Beispiel wird mithilfe von cURL eine GET-Anforderung für die REST-API ausgeführt: Ersetzen Sie **PASSWORD** durch das Administratorkennwort für den Cluster. Ersetzen Sie **CLUSTERNAME** durch den Namen des Clusters:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"

Die Antwort ist ein JSON-Dokument, das in etwa wie folgt beginnt:

    {
    "href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
    "Clusters" : {
        "cluster_id" : 2,
        "cluster_name" : "CLUSTERNAME",
        "health_report" : {
        "Host/stale_config" : 0,
        "Host/maintenance_state" : 0,
        "Host/host_state/HEALTHY" : 7,
        "Host/host_state/UNHEALTHY" : 0,
        "Host/host_state/HEARTBEAT_LOST" : 0,
        "Host/host_state/INIT" : 0,
        "Host/host_status/HEALTHY" : 7,
        "Host/host_status/UNHEALTHY" : 0,
        "Host/host_status/UNKNOWN" : 0,
        "Host/host_status/ALERT" : 0

Da es sich um JSON handelt, ist es einfacher, einen JSON-Parser zu verwenden, um mit den Daten zu arbeiten. Im folgenden Beispiel wird beispielsweise jq verwendet, um nur das `health_report`-Element anzuzeigen.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'

## <a name="example-get-the-fqdn-of-cluster-nodes"></a>Beispiel: Abrufen des FQDN von Clusterknoten

Bei der Arbeit mit HDInsight müssen Sie möglicherweise den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) eines Clusterknotens kennen. Sie können den FQDN für die verschiedenen Knoten im Cluster einfach wie folgt abrufen:

* **Hauptknoten**: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`

* **Workerknoten**: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`

* **Zookeeper-Knoten**: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Das Muster ist in jedem dieser Beispiele gleich:

1. Abfragen einer Komponente, von der bekannt ist, dass sie auf diesen Knoten ausgeführt wird

2. Abrufen der `host_name`-Elemente, die den FQDN für diese Knoten enthalten

Das `host_components`-Element des Rückgabedokuments enthält mehrere Elemente. Durch Verwendung von `.host_components[]` und Angabe eines Pfads im Element werden die einzelnen Elemente durchlaufen und der Wert aus dem jeweiligen Pfad abgerufen. Wenn Sie nur einen Wert, z. B. den ersten FQDN-Eintrag, erhalten möchten, können Sie die Elemente als Auflistung zurückgeben und dann einen bestimmten Eintrag auswählen:

    jq '[.host_components[].HostRoles.host_name][0]'

Dadurch wird der erste FQDN aus der Auflistung zurückgegeben.

## <a name="example-get-the-default-storage-account-and-container"></a>Beispiel: Abrufen des Standardspeicherkontos und -containers

Wenn Sie einen HDInsight-Cluster erstellen, müssen Sie ein Azure-Speicherkonto und einen Azure-Blob-Container als Standardspeicher für den Cluster verwenden. Sie können diese Informationen mit Ambari abrufen, nachdem der Cluster erstellt wurde. Dies bietet sich zum Beispiel an, wenn Sie Daten programmgesteuert direkt in den Container schreiben möchten.

Mit der folgenden Anweisung wird der WASB-URI des Clusterstandardspeichers abgerufen:

```bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

Für PowerShell gelten bei der Verwendung einfacher und doppelter Anführungszeichen etwas andere Regeln. Verwenden Sie in PowerShell den folgenden Befehl:

```PowerShell
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["""fs.defaultFS"""] | select(. != null)'
```

> [!NOTE]
> Hierdurch wird die erste auf den Server angewendete Konfiguration (`service_config_version=1`) zurückgegeben, die diese Information enthält. Wenn Sie einen Wert abrufen, der nach der Erstellung des Clusters geändert wurde, müssen Sie möglicherweise die Konfigurationsversionen auflisten und die neueste Version abrufen.

Dadurch wird ein ähnlicher Wert wie im folgenden Beispiel zurückgegeben, wobei **CONTAINER** für den Standardcontainer und **ACCOUNTNAME** für den Namen des Azure Storage-Kontos steht:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Diese Information können Sie dann mithilfe von [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) verwenden, um Daten in den Container hochzuladen oder daraus herunterzuladen.

1. Verwenden Sie den folgenden Befehl, um die eindeutige ID für das Speicherkonto abzurufen. Ersetzen Sie im folgenden Befehl **ACCOUNTNAME** durch den aus Ambari abgerufenen Namen des Speicherkontos:

        az storage account list --query "[?name=='ACCOUNTNAME'].id" --out list

2. Verwenden Sie den folgenden Befehl, um einen Schlüssel für das Speicherkonto abzurufen. Ersetzen Sie **STORAGEID** durch die ID des Speicherkontos:

        az storage account keys list --ids STORAGEID --query "keys[?keyName=='key1'].value" --out list

    Hierdurch wird der Primärschlüssel für das Konto zurückgegeben.

3. Verwenden Sie den upload-Befehl, um eine Datei im Container zu speichern:
   
        az storage blob upload --account-name ACOUNTNAME --account-key ACCOUNTKEY -f FILEPATH -c CONTAINER -n BLOBPATH
   
    Ersetzen Sie **ACCOUNTNAME** durch den Namen des Speicherkontos. Ersetzen Sie **ACCOUNTKEY** durch den zuvor abgerufenen Schlüssel. Ersetzen Sie __CONTAINER__ durch den Containernamen. **FILEPATH** ist der Pfad zu der Datei, die Sie hochladen möchten, und **BLOBPATH** ist der Pfad im Container.
   
    Wenn beispielsweise die Datei in HDInsight in wasbs://example/data/filename.txt angegeben werden soll, hat **BLOBPATH** den Wert `example/data/filename.txt`.

## <a name="example-update-ambari-configuration"></a>Beispiel: Aktualisieren der Ambari-Konfiguration

1. Rufen Sie die aktuelle Konfiguration ab, die von Ambari als „gewünschte Konfiguration“ gespeichert wird:
   
        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
   
    In diesem Beispiel wird ein JSON-Dokument mit der aktuellen Konfiguration (angegeben durch den Wert *tag* ) für die im Cluster installierten Komponenten zurückgegeben. Das folgende Beispiel ist ein Datenauszug aus der Rückgabe eines Spark-Clustertyps:
   
        "spark-metrics-properties" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-fairscheduler" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        },
        "spark-thrift-sparkconf" : {
            "tag" : "INITIAL",
            "user" : "admin",
            "version" : 1
        }
   
    Aus dieser Liste müssen Sie den Namen der Komponente (z.B. **spark\_thrift\_sparkconf**) und den Wert für **tag** kopieren.

2. Rufen Sie die Konfiguration für die Komponente ab, und kennzeichnen Sie sie mit dem unten angegebenen Befehl. Ersetzen Sie **spark-thrift-sparkconf** und **INITIAL** durch die Komponente und das Tag, für die bzw. das Sie die Konfiguration abrufen möchten.
   
    ```bash
    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```PowerShell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```
   
    cURL ruft das JSON-Dokument ab. Anschließend werden die Daten mithilfe von jq angepasst, um eine Vorlage zu erstellen. Diese Vorlage wird dann zum Hinzufügen/Ändern von Konfigurationswerten verwendet. Folgendes wird durchgeführt:
   
    * Es wird ein eindeutiger Wert erstellt, der die Zeichenfolge „version“ und das Datum enthält und in **newtag** gespeichert wird.

    * Ein Stammdokument für die neue gewünschte Konfiguration wird erstellt.

    * Der Inhalt des `.items[]`-Arrays wird abgerufen und unter dem **desired_config**-Element hinzugefügt.

    * Die Elemente **href**, **version** und **Config** werden gelöscht, da sie zum Übermitteln einer neuen Konfiguration nicht benötigt werden.

    * Ein neues **tag**-Element wird hinzugefügt, und der Wert wird auf **version#################** festgelegt. Der numerische Teil basiert auf dem aktuellen Datum. Jede Konfiguration muss über ein eindeutiges Tag verfügen.
     
        Schließlich werden die Daten im Dokument **newconfig.json** gespeichert. Die Dokumentstruktur sollte in etwa wie folgt aussehen:
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. Öffnen Sie das Dokument **newconfig.json**, und ändern Sie die Werte im Objekt **properties**, bzw. fügen Sie Werte hinzu. Im folgenden Beispiel wird der Wert von **spark.yarn.am.memory** von **1g** in **3g** geändert, und für **spark.kryoserializer.buffer.max** wird ein neues Element mit dem Wert **256m** hinzugefügt.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Speichern Sie die Datei, nachdem Sie alle Änderungen vorgenommen haben.
4. Verwenden Sie den folgenden Befehl, um die aktualisierte Konfiguration an Ambari zu übermitteln:
   
        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
   
    Mit diesem Befehl wird der Inhalt der Datei **newconfig.json** an die curl-Anforderung weitergeleitet, die diesen als neue gewünschte Konfiguration an den Cluster übermittelt. Die cURL-Anforderung gibt ein JSON-Dokument zurück. Das **versionTag**-Element in diesem Dokument sollte mit der von Ihnen übermittelten Version übereinstimmen, und das Objekt **configs** enthält die Konfigurationsänderungen, die Sie angefordert haben.

### <a name="example-restart-a-service-component"></a>Beispiel: Neustarten einer Dienstkomponente

Wenn Sie an diesem Punkt die Ambari-Webbenutzeroberfläche betrachten, gibt der Spark-Dienst an, dass er neu gestartet werden muss, bevor die neue Konfiguration wirksam werden kann. Führen Sie die unten angegebenen Schritte aus, um den Dienst neu zu starten.

1. Verwenden Sie Folgendes, um den Wartungsmodus für den Spark-Dienst zu aktivieren:
   
        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
   
    Mit diesem Befehl wird ein JSON-Dokument an den Server gesendet (in der `echo`-Anweisung enthalten) und der Wartungsmodus aktiviert.
    Mit der folgenden Anforderung können Sie überprüfen, ob sich der Dienst nun im Wartungsmodus befindet:
   
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
   
    Der Wert `"ON"`wird zurückgegeben.

2. Verwenden Sie als Nächstes Folgendes, um den Dienst zu deaktivieren:
    
    ```
    echo '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
    ```
    
    Die Antwort dieses Befehls sieht in etwa wie folgt aus:
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    Im Wert `href`, der von diesem URI zurückgegeben wird, wird die interne IP-Adresse des Clusterknotens verwendet. Ersetzen Sie für die Nutzung von außerhalb des Clusters den Teil „10.0.0.18:8080“ durch den FQDN des Clusters. Mit dem folgenden Befehl können Sie beispielsweise den Status der Anforderung abrufen:
    
    ```
    curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    ```
    
    Wird hierbei der Wert `"COMPLETED"` zurückgegeben, ist die Anforderung abgeschlossen.

3. Verwenden Sie Folgendes, um den Dienst zu starten, nachdem die vorherige Anforderung abgeschlossen wurde:
   
        echo '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
   
    Nach dem Neustart des Diensts werden die neuen Konfigurationseinstellungen verwendet.

4. Verwenden Sie abschließend Folgendes, um den Wartungsmodus zu deaktivieren:
   
        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

## <a name="next-steps"></a>Nächste Schritte

Eine vollständige Referenz der REST-API finden Sie unter [Referenz zur Ambari-API V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).




<!--HONumber=Dec16_HO1-->


