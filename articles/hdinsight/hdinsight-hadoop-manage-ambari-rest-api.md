---
title: "Überwachen und Verwalten von Azure HDInsight-Clustern mithilfe der Ambari-REST-API | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ambari zum Überwachen und Verwalten von Linux-basierten HDInsight-Clustern verwenden. In diesem Dokument erfahren Sie, wie Sie die in HDInsight-Clustern enthaltene Ambari-REST-API verwenden."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/23/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 2e26bd81c59fd53a0e8fc693dde30cb403995896
ms.openlocfilehash: 38d37e45c34c8c0a3bd2ed94f72944208292f466
ms.lasthandoff: 02/24/2017


---
# <a name="manage-hdinsight-clusters-by-using-the-ambari-rest-api"></a>Verwalten von HDInsight-Clustern mithilfe der Ambari-REST-API

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereinfacht die Verwaltung und Überwachung von Hadoop-Clustern durch die Bereitstellung einer benutzerfreundlichen Webbenutzeroberfläche und REST-API. Ambari ist in HDInsight-Clustern mit dem Linux-Betriebssystem enthalten und wird verwendet, um den Cluster zu überwachen und Konfigurationsänderungen vorzunehmen. Dieses Dokument enthält grundlegende Informationen zur Verwendung der Ambari-REST-API.

## <a id="whatis"></a>Was ist Ambari?

[Apache Ambari](http://ambari.apache.org) vereinfacht die Hadoop-Verwaltung durch die Bereitstellung einer benutzerfreundlichen Webbenutzeroberfläche, die zum Bereitstellen, Verwalten und Überwachen von Hadoop-Clustern verwendet werden kann. Entwickler können diese Funktionen mithilfe der [Ambari-REST-APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md)in ihre Anwendungen integrieren.

Ambari wird standardmäßig mit Linux-basierten Clustern bereitgestellt.

## <a name="how-to-use-the-ambari-rest-api"></a>Verwenden der Ambari-REST-API

> [!IMPORTANT]
> Die Informationen und Beispiele in diesem Dokument erfordern einen HDInsight-Cluster, der Linux als Betriebssystem nutzt. Weitere Informationen finden Sie unter [Hadoop-Tutorial: Erste Schritte bei der Verwendung von Hadoop in HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).

In diesem Dokument finden Sie sowohl Beispiele für die Bourne-Shell (Bash) als auch PowerShell. Die Bash-Beispiele wurden mit GNU-Bash 4.3.11 getestet, sollten jedoch mit anderen Unix-Shells funktionieren. Die PowerShell-Beispiele wurden mit PowerShell 5.0 getestet, sollten jedoch mit PowerShell 3.0 oder höher funktionieren.

Zur Verwendung der __Bourne-Shell__ (Bash) müssen Sie Folgendes installiert haben:

* [cURL](http://curl.haxx.se/): cURL ist ein Hilfsprogramm, das zum Arbeiten mit REST-APIs über die Befehlszeile verwendet werden kann. In diesem Dokument wird es für die Kommunikation mit der Ambari-REST-API verwendet.

Unabhängig davon, ob Sie Bash oder PowerShell verwenden, müssen Sie [jq](https://stedolan.github.io/jq/) installiert haben. Jq ist ein Hilfsprogramm für die Arbeit mit JSON-Dokumenten. Es wird in **allen** Bash-Beispielen und **einem** der PowerShell-Beispiele verwendet.

### <a name="base-uri-for-ambari-rest-api"></a>Basis-URI für Ambari-Rest-API

Der Basis-URI für die Ambari-REST-API in HDInsight-Clustern lautet https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, wobei **CLUSTERNAME** für den Namen Ihres Clusters steht.

> [!IMPORTANT]
> Beim Clusternamen im vollqualifizierten Domänennamen (FQDN) innerhalb des URI (CLUSTERNAME.azurehdinsight.net) wird die Groß-/Kleinschreibung nicht beachtet, bei anderen Vorkommen im URI dagegen schon. Wenn Ihr Cluster beispielsweise den Namen `MyCluster` hat, sind folgende URIs gültig:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> Bei den folgenden URIs wird ein Fehler zurückgegeben, da die Groß-/Kleinschreibung beim zweiten Vorkommen des Namens nicht korrekt ist:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a>Authentifizierung

Zum Herstellen einer Verbindung mit Ambari in HDInsight ist HTTPS erforderlich. Bei der Verbindungsauthentifizierung müssen Sie den Administratorkontonamen (standardmäßig **admin**) und das Kennwort verwenden, die Sie bei der Erstellung des Clusters angegeben haben.

## <a name="examples-authentication-and-parsing-json"></a>Beispiele: Authentifizierung und JSON-Analyse

In den folgenden Beispielen wird veranschaulicht, wie Sie eine GET-Anforderung mit der Basis-Ambari-REST-API durchführen:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> Für die Bash-Beispiele in diesem Dokument gelten die folgenden Annahmen:
>
> * Der Anmeldename für den Cluster ist der Standardwert von `admin`.
> * `$PASSWORD` enthält das Kennwort für den HDInsight-Anmeldebefehl. Sie können diesen Wert mit `PASSWORD='mypassword'` festlegen.
> * `$CLUSTERNAME` enthält den Namen des Clusters. Sie können diesen Wert mit `set CLUSTERNAME='clustername'` festlegen.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> Für die PowerShell-Beispiele in diesem Dokument gelten die folgenden Annahmen:
>
> * `$creds` ist ein Objekt mit Anmeldeinformationen, das die Administratoranmeldung und das Kennwort für den Cluster enthält. Sie können diesen Wert mit `$creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"` festlegen und die Anmeldeinformationen bei Aufforderung angeben.
> * `$clusterName` ist eine Zeichenfolge, die den Namen des Clusters enthält. Sie können diesen Wert mit `$clusterName="clustername"` festlegen.

Beide Beispiele geben ein JSON-Dokument zurück, das in etwa wie folgt beginnt:

```json
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
    ...
```

### <a name="parsing-json-data"></a>Analyse von JSON-Daten

Im folgenden Beispiel wird `jq` verwendet, um das JSON-Antwortdokument zu analysieren und nur die `health_report`-Informationen aus den Ergebnissen anzuzeigen.

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

PowerShell 3.0 und höher enthält das `ConvertFrom-Json`-Cmdlet, das das JSON-Dokument in ein Objekt konvertiert, mit dem Sie in PowerShell einfacher arbeiten können. Im folgenden Beispiel wird `ConvertFrom-Json` verwendet, um nur die `health_report`-Informationen aus den Ergebnissen anzuzeigen.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> Während die meisten Beispiele in diesem Dokument `ConvertFrom-Json` verwenden, um Elemente aus dem Antwortdokument anzuzeigen, verwendet das Beispiel zum [Aktualisieren der Ambari-Konfiguration](#example-update-ambari-configuration) jq. Jq wird in diesem Beispiel verwendet, um eine neue Vorlage aus dem JSON-Antwortdokument zu erstellen.

Eine vollständige Referenz der REST-API finden Sie unter [Referenz zur Ambari-API V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="example-get-the-fqdn-of-cluster-nodes"></a>Beispiel: Abrufen des FQDN von Clusterknoten

Bei der Arbeit mit HDInsight müssen Sie möglicherweise den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) eines Clusterknotens kennen. Sie können den FQDN für die verschiedenen Knoten im Cluster einfach wie folgt abrufen:

* **Alle Knoten**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* **Hauptknoten**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Workerknoten**

    ```bash
    curl -u admin:PASSWORD -sS -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Zookeeper-Knoten**

    ```bash
    curl -u admin:PASSWORD -sS -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-the-internal-ip-address-of-cluster-nodes"></a>Beispiel: Abrufen der internen IP-Adresse der Clusterknoten

> [!IMPORTANT]
> Auf die von den Beispielen in diesem Abschnitt zurückgegebenen IP-Adressen kann nicht direkt über das Internet zugegriffen werden. Auf sie kann nur innerhalb des virtuellen Azure-Netzwerks, das den HDInsight-Cluster enthält, zugegriffen werden.
>
> Informationen zum Arbeiten mit HDInsight und virtuellen Netzwerken finden Sie unter [Erweitern der HDInsight-Funktionen mit Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md).

Sie müssen den FQDN für den Host kennen, bevor Sie die IP-Adresse abrufen können. Sobald Sie den FQDN kennen, können Sie die IP-Adresse des Hosts abrufen. Die folgenden Beispiele fragen Ambari zuerst nach dem FQDN aller Hostknoten und dann nach der IP-Adresse jedes Hosts ab.

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts"
$resp = Invoke-WebRequest -Uri $uri -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

## <a name="example-get-the-default-storage"></a>Beispiel: Abrufen des Standardspeichers

Wenn Sie einen HDInsight-Cluster erstellen, müssen Sie ein Azure-Speicherkonto oder Data Lake Store als Standardspeicher für den Cluster verwenden. Sie können diese Informationen mit Ambari abrufen, nachdem der Cluster erstellt wurde. Sie möchten z.B. Daten außerhalb HDInsight aus dem Container lesen bzw. in ihn hineinschreiben.

Die folgenden Beispiele rufen die Standardspeicherkonfiguration aus dem Cluster ab:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]
> Diese Beispiele geben die erste auf den Server angewendete Konfiguration (`service_config_version=1`) zurück, die diese Informationen enthält. Wenn Sie einen Wert abrufen, der nach der Erstellung des Clusters geändert wurde, müssen Sie möglicherweise die Konfigurationsversionen auflisten und die neueste Version abrufen.

Der Rückgabewert ähnelt einem der folgenden Beispiele:

* `wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net` – dieser Wert gibt an, dass der Cluster ein Azure Storage-Konto als Standardspeicher verwendet. Der Wert `ACCOUNTNAME` ist der Name des Speicherkontos. Der Teil `CONTAINER` ist der Name des Blobcontainers im Speicherkonto. Der Container ist der Stamm des HDFS-kompatiblen Speichers für den Cluster.

* `adl://home` – dieser Wert gibt an, dass der Cluster einen Azure Data Lake Store als Standardspeicher verwendet.

    Um den Kontonamen des Data Lake Store zu ermitteln, verwenden Sie die folgenden Beispiele:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    Der Rückgabewert ähnelt `ACCOUNTNAME.azuredatalakestore.net`, wobei `ACCOUNTNAME` der Name des Data Lake Store-Kontos ist.

    Um das Verzeichnis in Data Lake Store zu suchen, das den Speicher für den Cluster enthält, verwenden Sie die folgenden Beispiele:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    Der Rückgabewert ähnelt `/clusters/CLUSTERNAME/`. Dieser Wert entspricht dem Pfad im Data Lake Store-Konto. Dieser Pfad ist der Stamm des HDFS-kompatiblen Dateisystems für den Cluster. 

> [!NOTE]
> Das `Get-AzureRmHDInsightCluster`-Cmdlet von [Azure PowerShell](https://docs.microsoft.com/powershell/) gibt auch die Speicherinformationen für den Cluster zurück.


## <a name="example-get-configuration"></a>Beispiel: Konfigurationsinformationen abrufen

1. Rufen Sie die verfügbaren Konfigurationen für Ihren Cluster ab.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    In diesem Beispiel wird ein JSON-Dokument mit der aktuellen Konfiguration (angegeben durch den Wert *tag* ) für die im Cluster installierten Komponenten zurückgegeben. Das folgende Beispiel ist ein Datenauszug aus der Rückgabe eines Spark-Clustertyps:
   
   ```json
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
   ```

2. Rufen Sie die Konfiguration für die Komponente ab, die Sie interessiert. Ersetzen Sie im folgenden Beispiel `INITIAL` mit dem Tagwert, der von der vorherigen Anforderung zurückgegeben wurde.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    In diesem Beispiel wird ein JSON-Dokument zurückgegeben, das die aktuelle Konfiguration für die `core-site`-Komponente enthält.

## <a name="example-update-configuration"></a>Beispiel: Konfiguration aktualisieren

1. Rufen Sie die aktuelle Konfiguration ab, die von Ambari als „gewünschte Konfiguration“ gespeichert wird:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    In diesem Beispiel wird ein JSON-Dokument mit der aktuellen Konfiguration (angegeben durch den Wert *tag* ) für die im Cluster installierten Komponenten zurückgegeben. Das folgende Beispiel ist ein Datenauszug aus der Rückgabe eines Spark-Clustertyps:
   
    ```json
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
    ```
   
    Aus dieser Liste müssen Sie den Namen der Komponente (z.B. **spark\_thrift\_sparkconf**) und den Wert für **tag** kopieren.

2. Rufen Sie die Konfiguration für die Komponente ab, und kennzeichnen Sie sie mit den unten angegebenen Befehlen. Ersetzen Sie **spark-thrift-sparkconf** und **INITIAL** durch die Komponente und das Tag, für die bzw. das Sie die Konfiguration abrufen möchten.
   
    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
    | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```powershell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=spark-thrift-sparkconf&tag=INITIAL" `
        -Credential $creds
    $resp.Content | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```
   
    Jq wird verwendet, um die aus HDInsight abgerufenen Daten in eine neue Konfigurationsvorlage umzuwandeln. Im Einzelnen führen diese Beispiele folgende Aktionen aus:
   
    * Es wird ein eindeutiger Wert erstellt, der die Zeichenfolge „version“ sowie das Datum enthält und in `newtag` gespeichert wird.

    * Ein Stammdokument für die neue gewünschte Konfiguration wird erstellt.

    * Der Inhalt des `.items[]`-Arrays wird abgerufen und unter dem **desired_config**-Element hinzugefügt.

    * Die Elemente `href`, `version` und `Config` werden gelöscht, da sie zum Übermitteln einer neuen Konfiguration nicht benötigt werden.

    * Ein `tag`-Element mit dem Wert `version#################` wird hinzugefügt. Der numerische Teil basiert auf dem aktuellen Datum. Jede Konfiguration muss über ein eindeutiges Tag verfügen.
     
    Schließlich werden die Daten im Dokument `newconfig.json` gespeichert. Die Dokumentstruktur sollte in etwa wie folgt aussehen:
     
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

3. Öffnen Sie das Dokument `newconfig.json` und ändern Sie Werte im `properties`-Objekt, bzw. fügen Sie sie hinzu. Im folgenden Beispiel wird der Wert von `"spark.yarn.am.memory"` von `"1g"` in `"3g"` geändert. Außerdem wird `"spark.kryoserializer.buffer.max"` mit dem Wert `"256m"` hinzugefügt.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Speichern Sie die Datei, nachdem Sie alle Änderungen vorgenommen haben.

4. Verwenden Sie die folgenden Befehle, um die aktualisierte Konfiguration an Ambari zu übermitteln.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```
   
    Diese Befehle übermitteln den Inhalt der Datei **newconfig.json** als neue gewünschte Konfiguration an den Cluster. Die Anforderung gibt ein JSON-Dokument zurück. Das **versionTag**-Element in diesem Dokument sollte mit der von Ihnen übermittelten Version übereinstimmen, und das Objekt **configs** enthält die Konfigurationsänderungen, die Sie angefordert haben.

### <a name="example-restart-a-service-component"></a>Beispiel: Neustarten einer Dienstkomponente

Wenn Sie an diesem Punkt die Ambari-Webbenutzeroberfläche betrachten, gibt der Spark-Dienst an, dass er neu gestartet werden muss, bevor die neue Konfiguration wirksam werden kann. Führen Sie die unten angegebenen Schritte aus, um den Dienst neu zu starten.

1. Verwenden Sie Folgendes, um den Wartungsmodus für den Spark-Dienst zu aktivieren:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```
   
    Diese Befehle senden ein JSON-Dokument an den Server, der den Wartungsmodus aktiviert. Mit der folgenden Anforderung können Sie überprüfen, ob sich der Dienst nun im Wartungsmodus befindet:
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    Der Rückgabewert ist `ON`.

2. Deaktivieren Sie als Nächstes folgendermaßen den Dienst:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```
    
    Die Antwort ähnelt dem folgenden Beispiel:
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    > [!IMPORTANT]
    > Im Wert `href`, der von diesem URI zurückgegeben wird, wird die interne IP-Adresse des Clusterknotens verwendet. Ersetzen Sie für die Nutzung von außerhalb des Clusters den Teil „10.0.0.18:8080“ durch den FQDN des Clusters. 
    
    Die folgenden Befehle rufen den Status der Anforderung ab:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    Die Antwort `COMPLETED` gibt an, dass die Anforderung abgeschlossen ist.

3. Verwenden Sie Folgendes, um den Dienst zu starten, nachdem die vorherige Anforderung abgeschlossen wurde:
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```
   
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```
    Der Dienst verwendet jetzt die neue Konfiguration.

4. Verwenden Sie abschließend Folgendes, um den Wartungsmodus zu deaktivieren:
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a>Nächste Schritte

Eine vollständige Referenz der REST-API finden Sie unter [Referenz zur Ambari-API V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).


