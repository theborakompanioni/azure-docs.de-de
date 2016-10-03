<properties
   pageTitle="Überwachen und Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-REST-API | Microsoft Azure"
   description="Erfahren Sie, wie Sie Ambari zum Überwachen und Verwalten von Linux-basierten HDInsight-Clustern verwenden. In diesem Dokument erfahren Sie, wie Sie die in HDInsight-Clustern enthaltene Ambari-REST-API verwenden."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
	tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/20/2016"
   ms.author="larryfr"/>

#Verwalten von HDInsight-Clustern mithilfe der Ambari-REST-API

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereinfacht die Verwaltung und Überwachung von Hadoop-Clustern durch die Bereitstellung einer benutzerfreundlichen Webbenutzeroberfläche und REST-API. Ambari ist in Linux-basierten HDInsight-Clustern enthalten und wird verwendet, um den Cluster zu überwachen und Konfigurationsänderungen vorzunehmen. Dieses Dokument enthält grundlegende Informationen zur Verwendung der Ambari-REST-API im Rahmen allgemeiner Aufgaben mit cURL.

##Voraussetzungen

* [cURL](http://curl.haxx.se/): cURL ist ein plattformübergreifendes Hilfsprogramm, das zum Arbeiten mit REST-APIs über die Befehlszeile verwendet werden kann. In diesem Dokument wird es für die Kommunikation mit der Ambari-REST-API verwendet.
* [jq](https://stedolan.github.io/jq/): jq ist ein plattformübergreifendes Befehlszeilenprogramm zum Arbeiten mit JSON-Dokumenten. In diesem Dokument wird es zum Analysieren der von der Ambari REST-API zurückgegebenen JSON-Dokumente verwendet.
* [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md): ein plattformübergreifendes Befehlszeilenprogramm zum Arbeiten mit Azure-Diensten.

    [AZURE.INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

##<a id="whatis"></a>Was ist Ambari?

[Apache Ambari](http://ambari.apache.org) vereinfacht die Hadoop-Verwaltung durch die Bereitstellung einer benutzerfreundlichen Webbenutzeroberfläche, die zum Bereitstellen, Verwalten und Überwachen von Hadoop-Clustern verwendet werden kann. Entwickler können diese Funktionen mithilfe der [Ambari-REST-APIs](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md) in ihre Anwendungen integrieren.

Ambari wird standardmäßig mit Linux-basierten Clustern bereitgestellt.

##REST-API

Der Basis-URI für die Ambari REST-API in HDInsight lautet https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, wobei __CLUSTERNAME__ der Name des Clusters ist.

> [AZURE.IMPORTANT] Beim Clusternamen im vollqualifizierten Domänennamen (FQDN) innerhalb des URI (CLUSTERNAME.azurehdinsight.net) wird die Groß-/Kleinschreibung nicht beachtet, bei anderen Vorkommen im URI dagegen schon. Wenn Ihr Cluster beispielsweise den Namen MyCluster hat, sind folgende URIs gültig:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster` `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Bei den folgenden URIs wird ein Fehler zurückgegeben, da die Groß-/Kleinschreibung beim zweiten Vorkommen des Namens nicht korrekt ist:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster` `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Zum Herstellen einer Verbindung mit Ambari in HDInsight ist HTTPS erforderlich. Bei der Verbindungsauthentifizierung müssen Sie den Administratorkontonamen (standardmäßig __admin__) und das Kennwort verwenden, die Sie bei der Erstellung des Clusters angegeben haben.

Im folgenden Beispiel wird mithilfe von cURL eine GET-Anforderung für die REST-API ausgeführt: Ersetzen Sie __PASSWORD__ durch das Administratorkennwort für den Cluster. Ersetzen Sie __CLUSTERNAME__ durch den Namen des Clusters:

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

##Beispiel: Abrufen des FQDN von Clusterknoten

Bei der Arbeit mit HDInsight müssen Sie möglicherweise den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) eines Clusterknotens kennen. Sie können den FQDN für die verschiedenen Knoten im Cluster einfach wie folgt abrufen:

* __Hauptknoten__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Workerknoten__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper-Knoten__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Das Muster ist in jedem dieser Beispiele gleich:

1. Abfragen einer Komponente, von der bekannt ist, dass sie auf diesen Knoten ausgeführt wird
2. Abrufen der `host_name`-Elemente, die den FQDN für diese Knoten enthalten

Das `host_components`-Element des Rückgabedokuments enthält mehrere Elemente. Durch Verwendung von `.host_components[]` und Angabe eines Pfads im Element werden die einzelnen Elemente durchlaufen und der Wert aus dem jeweiligen Pfad abgerufen. Wenn Sie nur einen Wert, z. B. den ersten FQDN-Eintrag, erhalten möchten, können Sie die Elemente als Auflistung zurückgeben und dann einen bestimmten Eintrag auswählen:

    jq '[.host_components[].HostRoles.host_name][0]'

Dadurch wird der erste FQDN aus der Auflistung zurückgegeben.

##Beispiel: Abrufen des Standardspeicherkontos und -containers

Wenn Sie einen HDInsight-Cluster erstellen, müssen Sie ein Azure-Speicherkonto und einen Azure-Blob-Container als Standardspeicher für den Cluster verwenden. Sie können diese Informationen mit Ambari abrufen, nachdem der Cluster erstellt wurde. Dies bietet sich zum Beispiel an, wenn Sie Daten programmgesteuert direkt in den Container schreiben möchten.

Mit der folgenden Anweisung wird der WASB-URI des Clusterstandardspeichers abgerufen:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Hierdurch wird die erste auf den Server angewendete Konfiguration (`service_config_version=1`) zurückgegeben, die diese Information enthält. Wenn Sie einen Wert abrufen, der nach der Erstellung des Clusters geändert wurde, müssen Sie möglicherweise die Konfigurationsversionen auflisten und die neueste Version abrufen.

Dadurch wird ein ähnlicher Wert wie im folgenden Beispiel zurückgegeben, wobei __CONTAINER__ der Standardcontainer und __ACCOUNTNAME__ der Name des Azure-Speicherkontos ist:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Sie können dann diese Information mit der [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) verwenden, um Daten in den Container hochzuladen oder aus diesem herunterzuladen.

1. Rufen Sie die Ressourcengruppe für das Speicherkonto ab. Ersetzen Sie __ACCOUNTNAME__ durch den aus Ambari abgerufenen Namen des Speicherkontos:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Hierdurch wird der Ressourcengruppenname für das Konto zurückgegeben.
    
    > [AZURE.NOTE] Wenn von diesem Befehl nichts zurückgegeben wird, müssen Sie die Azure-Befehlszeilenschnittstelle eventuell in den Azure-Ressourcen-Manager-Modus ändern und den Befehl erneut ausführen. Wechseln Sie mit dem folgenden Befehl in den Azure Resource Manager-Modus:
    >
    > `azure config mode arm`
    
2. Rufen Sie den Schlüssel für das Speicherkonto ab. Ersetzen Sie __GROUPNAME__ durch den Namen der Ressourcengruppe aus dem vorherigen Schritt. Ersetzen Sie __ACCOUNTNAME__ durch den Namen des Speicherkontos:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    In diesem Beispiel wird der Primärschlüssel für das Konto zurückgegeben.
    
3. Verwenden Sie den upload-Befehl, um eine Datei im Container zu speichern:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Ersetzen Sie __ACCOUNTNAME__ durch den Namen des Speicherkontos. Ersetzen Sie __ACCOUNTKEY__ durch den zuvor abgerufenen Schlüssel. __FILEPATH__ ist der Pfad zu der Datei, die Sie hochladen möchten, und __BLOBPATH__ ist der Pfad im Container.

    Wenn beispielsweise die Datei in HDInsight in wasbs://example/data/filename.txt angegeben werden soll, lautet __BLOBPATH__ `example/data/filename.txt`.

##Beispiel: Aktualisieren der Ambari-Konfiguration

1. Rufen Sie die aktuelle Konfiguration ab, die von Ambari als „gewünschte Konfiguration“ gespeichert wird:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    In diesem Beispiel wird ein JSON-Dokument mit der aktuellen Konfiguration (angegeben durch den Wert _tag_) für die im Cluster installierten Komponenten zurückgegeben. Das folgende Beispiel ist ein Datenauszug aus der Rückgabe eines Spark-Clustertyps:
    
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

    Aus dieser Liste müssen Sie den Namen der Komponente (z.B. __spark\_thrift\_sparkconf__) und den Wert für __tag__ kopieren.
    
2. Rufen Sie die Konfiguration für die Komponente ab, und kennzeichnen Sie sie mit dem unten angegebenen Befehl. Ersetzen Sie __spark-thrift-sparkconf__ und __INITIAL__ durch die Komponente und das Tag, für die bzw. das Sie die Konfiguration abrufen möchten.

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    
    cURL ruft das JSON-Dokument ab. Anschließend werden die Daten mithilfe von jq angepasst, um eine Vorlage zu erstellen. Diese Vorlage wird dann zum Hinzufügen/Ändern von Konfigurationswerten verwendet. Folgendes wird durchgeführt:
    
    * Ein eindeutiger Wert wird erstellt, der die Zeichenfolge „version“ und das Datum enthält und in __newtag__ gespeichert wird.
    * Ein Stammdokument für die neue gewünschte Konfiguration wird erstellt.
    * Der Inhalt des `.items[]`-Arrays wird abgerufen und unter dem __desired\_config__-Element hinzugefügt.
    * Die Elemente __href__, __version__ und __Config__ werden gelöscht, da sie zum Übermitteln einer neuen Konfiguration nicht benötigt werden.
    * Ein neues __tag__-Element wird hinzugefügt, und der Wert wird auf __version#################__ festgelegt. Der numerische Teil basiert auf dem aktuellen Datum. Jede Konfiguration muss über ein eindeutiges Tag verfügen.
    
    Schließlich werden die Daten im Dokument __newconfig.json__ gespeichert. Die Dokumentstruktur sollte in etwa wie folgt aussehen:
    
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

3. Öffnen Sie das Dokument __newconfig.json__, und ändern Sie die Werte im Objekt __properties__, bzw. fügen Sie Werte hinzu. Im folgenden Beispiel wird der Wert von __"spark.yarn.am.memory"__ von __"1g"__ in __"3g"__ geändert, und für __"spark.kryoserializer.buffer.max"__ wird ein neues Element mit dem Wert __"256m"__ hinzugefügt.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Speichern Sie die Datei, nachdem Sie alle Änderungen vorgenommen haben.

4. Verwenden Sie den folgenden Befehl, um die aktualisierte Konfiguration an Ambari zu übermitteln:

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Mit diesem Befehl wird der Inhalt der Datei __newconfig.json__ an die curl-Anforderung weitergeleitet, die diesen als neue gewünschte Konfiguration an den Cluster übermittelt. Die cURL-Anforderung gibt ein JSON-Dokument zurück. Das __versionTag__-Element in diesem Dokument sollte mit der von Ihnen übermittelten Version übereinstimmen, und das Objekt __configs__ enthält die Konfigurationsänderungen, die Sie angefordert haben.

###Beispiel: Neustarten einer Dienstkomponente

Wenn Sie an diesem Punkt die Ambari-Webbenutzeroberfläche betrachten, gibt der Spark-Dienst an, dass er neu gestartet werden muss, bevor die neue Konfiguration wirksam werden kann. Führen Sie die unten angegebenen Schritte aus, um den Dienst neu zu starten.

1. Verwenden Sie Folgendes, um den Wartungsmodus für den Spark-Dienst zu aktivieren:

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Mit diesem Befehl wird ein JSON-Dokument an den Server gesendet (in der `echo`-Anweisung enthalten) und der Wartungsmodus aktiviert. Mit der folgenden Anforderung können Sie überprüfen, ob sich der Dienst nun im Wartungsmodus befindet:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Der Wert `"ON"` wird zurückgegeben.

3. Verwenden Sie als Nächstes Folgendes, um den Dienst zu deaktivieren:

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Die Antwort dieses Befehls sieht in etwa wie folgt aus:
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    Im Wert `href`, der von diesem URI zurückgegeben wird, wird die interne IP-Adresse des Clusterknotens verwendet. Ersetzen Sie für die Nutzung von außerhalb des Clusters den Teil „10.0.0.18:8080“ durch den FQDN des Clusters. Mit dem folgenden Befehl können Sie beispielsweise den Status der Anforderung abrufen:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Wird hierbei der Wert `"COMPLETED"` zurückgegeben, ist die Anforderung abgeschlossen.

4. Verwenden Sie Folgendes, um den Dienst zu starten, nachdem die vorherige Anforderung abgeschlossen wurde:

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Nach dem Neustart des Diensts werden die neuen Konfigurationseinstellungen verwendet.

5. Verwenden Sie abschließend Folgendes, um den Wartungsmodus zu deaktivieren:

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##Nächste Schritte

Eine vollständige Referenz der REST-API finden Sie unter [Referenz zur Ambari-API V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Einige Ambari-Funktionen sind deaktiviert, da sie vom HDInsight-Clouddienst verwaltet werden, z. B. Hinzufügen oder Entfernen von Hosts im Cluster oder Hinzufügen neuer Dienste.

<!---HONumber=AcomDC_0921_2016-->