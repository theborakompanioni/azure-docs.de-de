<properties
   pageTitle="Überwachen und Verwalten von HDInsight-Clustern mithilfe der Apache Ambari-REST-API | Microsoft Azure"
   description="Erfahren Sie, wie Sie Ambari zum Überwachen und Verwalten von Linux-basierten HDInsight-Clustern verwenden. In diesem Dokument erfahren Sie, wie Sie die in HDInsight-Clustern enthaltene Ambari-REST-API verwenden."
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
	tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/05/2016"
   ms.author="larryfr"/>

#Verwalten von HDInsight-Clustern mithilfe der Ambari-REST-API

[AZURE.INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari vereinfacht die Verwaltung und Überwachung von Hadoop-Clustern durch die Bereitstellung einer benutzerfreundlichen Webbenutzeroberfläche und REST-API. Ambari ist in Linux-basierten HDInsight-Clustern enthalten und wird verwendet, um den Cluster zu überwachen und Konfigurationsänderungen vorzunehmen. Dieses Dokument vermittelt Ihnen die Grundlagen für das Arbeiten mit der Ambari-REST-API. Hierzu führen Sie allgemeine Aufgaben aus, z. B. das Suchen des vollqualifizierten Domänennamens der Clusterknoten oder das Suchen des vom Cluster verwendeten Standardspeicherkontos.

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

> [AZURE.IMPORTANT] Während die Groß-/Kleinschreibung beim Clusternamen im vollqualifizierten Domänennamen (FQDN), der Teil des URI (CLUSTERNAME.azurehdinsight.net) ist, nicht berücksichtigt wird, ist dies bei anderen Vorkommen im URI der Fall. Wenn Ihr Cluster beispielsweise den Namen MyCluster hat, sind folgende URIs gültig:
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster` `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> Bei den folgenden URIs wird ein Fehler zurückgegeben, da das zweite Vorkommen des Namens nicht die richtige Groß-/Kleinschreibung aufweist.
>
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster` `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

Zum Herstellen einer Verbindung mit Ambari in HDInsight ist HTTPS erforderlich. Sie müssen sich bei Ambari auch mit dem Administratorkontonamen (standardmäßig __Admin__) und dem Kennwort authentifizieren, die Sie bei der Erstellung des Clusters angegeben haben.

Im folgenden Beispiel wird cURL zum Ausführen einer GET-Anforderung für die REST-API verwendet:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
    
Wenn Sie diesen Befehl ausführen und __PASSWORD__ durch das Administratorkennwort sowie __CLUSTERNAME__ durch den Clusternamen ersetzen, erhalten Sie ein JSON-Dokument, das mit Informationen wie etwa den folgenden beginnt:

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

Da es sich um JSON handelt, ist es in der Regel einfacher, zum Abrufen der Daten einen JSON-Parser zu verwenden. Wenn Sie z.B. Informationen über den Zustand für den Cluster abrufen möchten, können Sie Folgendes verwenden.

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME" | jq '.Clusters.health_report'
    
Hiermit wird das JSON-Dokument abgerufen und dann die Ausgabe an jq übergeben. `.Clusters.health_report` gibt das Element im JSON-Dokument an, das Sie abrufen möchten.

##Beispiel: Abrufen des FQDN von Clusterknoten

Bei der Arbeit mit HDInsight müssen Sie möglicherweise den vollqualifizierten Domänennamen (Fully Qualified Domain Name, FQDN) eines Clusterknotens kennen. Sie können den FQDN für die verschiedenen Knoten im Cluster einfach wie folgt abrufen:

* __Hauptknoten__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/NAMENODE" | jq '.host_components[].HostRoles.host_name'`
* __Workerknoten__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" | jq '.host_components[].HostRoles.host_name'`
* __Zookeeper-Knoten__: `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | jq '.host_components[].HostRoles.host_name'`

Beachten Sie, dass jede dieser Anweisungen nach dem gleichen Muster erfolgt: Abrufen einer Komponente, von der bekannt ist, dass sie auf diesen Knoten ausgeführt wird, und dann Abrufen der `host_name`-Elemente, die den FQDN für diese Knoten enthalten.

Das `host_components`-Element des Rückgabedokuments enthält mehrere Elemente. Durch Verwendung von `.host_components[]` und Angabe eines Pfads im Element werden die einzelnen Elemente durchlaufen und der Wert aus dem jeweiligen Pfad abgerufen. Wenn Sie nur einen Wert, z. B. den ersten FQDN-Eintrag, erhalten möchten, können Sie die Elemente als Auflistung zurückgeben und dann einen bestimmten Eintrag auswählen:

    jq '[.host_components[].HostRoles.host_name][0]'

Mit dieser Anweisung wird der erste FQDN aus der Auflistung zurückgegeben.

##Beispiel: Abrufen des Standardspeicherkontos und -containers

Wenn Sie einen HDInsight-Cluster erstellen, müssen Sie ein Azure-Speicherkonto und einen Azure-Blob-Container als Standardspeicher für den Cluster verwenden. Sie können diese Informationen mit Ambari abrufen, nachdem der Cluster erstellt wurde. Dies bietet sich zum Beispiel an, wenn Sie Daten programmgesteuert direkt in den Container schreiben möchten.

Mit der folgenden Anweisung wird der WASB-URI des Clusterstandardspeichers abgerufen:

    curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
    
> [AZURE.NOTE] Hierdurch wird die erste auf den Server angewendete Konfiguration (`service_config_version=1`) zurückgegeben, die diese Information enthält. Wenn Sie einen Wert abrufen, der nach der Erstellung des Clusters geändert wurde, müssen Sie möglicherweise die Konfigurationsversionen auflisten und die letzte Version abrufen.

Dadurch wird ein ähnlicher Wert wie der folgende zurückgegeben, wobei __CONTAINER__ der Standardcontainer und __ACCOUNTNAME__ der Name des Azure-Speicherkontos ist:

    wasbs://CONTAINER@ACCOUNTNAME.blob.core.windows.net

Sie können dann diese Information mit der [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) verwenden, um Daten in den Container hochzuladen oder aus diesem herunterzuladen.

1. Rufen Sie die Ressourcengruppe für das Speicherkonto ab. Ersetzen Sie __ACCOUNTNAME__ durch den aus Ambari abgerufenen Namen des Speicherkontos:

        azure storage account list --json | jq '.[] | select(.name=="ACCOUNTNAME").resourceGroup'
    
    Hierdurch wird der Ressourcengruppenname für das Konto zurückgegeben.
    
    > [AZURE.NOTE] Wenn von diesem Befehl nichts zurückgegeben wird, müssen Sie die Azure-Befehlszeilenschnittstelle eventuell in den Azure-Ressourcen-Manager-Modus ändern und den Befehl erneut ausführen. Wechseln Sie mit dem folgenden Befehl in den Azure-Ressourcen-Manager-Modus:
    >
    > `azure config mode arm`
    
2. Rufen Sie den Schlüssel für das Speicherkonto ab. Ersetzen Sie __GROUPNAME__ durch den Namen der Ressourcengruppe aus dem vorherigen Schritt. Ersetzen Sie __ACCOUNTNAME__ durch den Namen des Speicherkontos:

        azure storage account keys list -g GROUPNAME ACCOUNTNAME --json | jq '.storageAccountKeys.key1'

    Hierdurch wird der Primärschlüssel für das Konto zurückgegeben.
    
3. Verwenden Sie den upload-Befehl, um eine Datei im Container zu speichern:

        azure storage blob upload -a ACCOUNTNAME -k ACCOUNTKEY -f FILEPATH --container __CONTAINER__ -b BLOBPATH
        
    Ersetzen Sie __ACCOUNTNAME__ durch den Namen des Speicherkontos. Ersetzen Sie __ACCOUNTKEY__ durch den zuvor abgerufenen Schlüssel. __FILEPATH__ ist der Pfad zu der Datei, die Sie hochladen möchten, und __BLOBPATH__ ist der Pfad im Container.

    Wenn beispielsweise die Datei in HDInsight in wasbs://example/data/filename.txt angegeben werden soll, lautet __BLOBPATH__ `example/data/filename.txt`.

##Beispiel: Aktualisieren der Ambari-Konfiguration

1. Rufen Sie die aktuelle Konfiguration ab, die von Ambari als „gewünschte Konfiguration“ gespeichert wird:

        curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME?fields=Clusters/desired_configs"
        
    Es wird ein JSON-Dokument mit der aktuellen Konfiguration (durch den Wert _tag_ gekennzeichnet) für die Komponenten zurückgegeben, die im Cluster installiert sind. Folgendes ist beispielsweise ein Auszug aus den Daten, die aus einem Spark-Clustertyp zurückgegeben werden.
    
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
    
    Curl ruft das JSON-Dokument ab, und dann wird jq verwendet, um einige Änderungen vorzunehmen. Es wird eine Vorlage erstellt, die wir zum Hinzufügen/Ändern von Konfigurationswerten verwenden können. Folgendes wird durchgeführt:
    
    * Erstellt einen eindeutigen Wert, der die Zeichenfolge „version“ und das Datum enthält und in __newtag__ gespeichert wird.
    * Erstellt ein Stammdokument für die neue gewünschte Konfiguration.
    * Ruft den Inhalt des .items-Arrays ab und fügt ihn unter dem __desired\_config__-Element hinzu.
    * Löscht die Elemente __href__, __version__ und __Config__, da diese zum Übermitteln einer neuen Konfiguration nicht benötigt werden.
    * Fügt ein neues __tag__-Element hinzu und legt seinen Wert auf __version#################__ fest, wobei der numerische Teil auf dem aktuellen Datum basiert. Jede Konfiguration muss über ein eindeutiges Tag verfügen.
    
    Schließlich werden die Daten im Dokument __newconfig.json__ gespeichert. Die Dokumentstruktur ähnelt der folgenden Struktur:
    
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

3. Öffnen Sie das Dokument __newconfig.json__, und ändern Sie die Werte im Objekt __properties__, bzw. fügen Sie Werte hinzu. Ändern Sie beispielsweise den Wert von __„spark.yarn.am.memory“__ von __„1g“__ in __„3g“__, und fügen Sie ein neues Element für __„spark.kryoserializer.buffer.max“__ mit dem Wert __„256m“__ hinzu.

        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",

    Speichern Sie die Datei, nachdem Sie alle Änderungen vorgenommen haben.

4. Gehen Sie wie folgt vor, um die aktualisierte Konfiguration an Ambari zu übermitteln.

        cat newconfig.json | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME"
        
    Mit diesem Befehl wird der Inhalt der Datei __newconfig.json__ an die curl-Anforderung weitergeleitet, die diesen als neue gewünschte Konfiguration an den Cluster übermittelt. Hierbei wird ein JSON-Dokument zurückgegeben. Das __versionTag__-Element in diesem Dokument sollte mit der von Ihnen übermittelten Version übereinstimmen, und das Objekt __configs__ enthält die Konfigurationsänderungen, die Sie angefordert haben.

###Beispiel: Neustarten einer Dienstkomponente

Wenn Sie an diesem Punkt die Ambari-Webbenutzeroberfläche betrachten, gibt der Spark-Dienst an, dass er neu gestartet werden muss, bevor die neue Konfiguration wirksam werden kann. Führen Sie die unten angegebenen Schritte aus, um den Dienst neu zu starten.

1. Verwenden Sie Folgendes, um den Wartungsmodus für den Spark-Dienst zu aktivieren.

        echo '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Hiermit wird ein JSON-Dokument an den Server gesendet (in der `echo`-Anweisung enthalten), mit dem der Wartungsmodus aktiviert wird. Sie können mit der folgenden Anforderung überprüfen, ob sich der Dienst jetzt im Wartungsmodus befindet.
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK" | jq .ServiceInfo.maintenance_state
        
    Der Wert `"ON"` wird zurückgegeben.

3. Verwenden Sie als Nächstes Folgendes, um den Dienst zu deaktivieren.

        echo '{"RequestInfo": {"context" :"Stopping the Spark service"}, "Body": {"ServiceInfo": {"state": "INSTALLED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"
        
    Sie erhalten eine Antwort, die in etwa wie folgt aussieht:
    
        {
            "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
            "Requests" : {
                "id" : 29,
                "status" : "Accepted"
            }
        }
    
    Im Wert `href`, der von diesem URI zurückgegeben wird, wird die interne IP-Adresse des Clusterknotens verwendet. Ersetzen Sie für die Nutzung von außerhalb des Clusters den Teil „10.0.0.18:8080“ durch den FQDN des Clusters. Der Status der Anforderung wird beispielsweise wie folgt aufgerufen:
    
        curl -u admin:PASSWORD -H "X-Requested-By: ambari" "https://CLUSTERNAME/api/v1/clusters/CLUSTERNAME/requests/29" | jq .Requests.request_status
    
    Wenn dieser Wert `"COMPLETED"` zurückgibt, ist die Anforderung abgeschlossen.

4. Verwenden Sie Folgendes, um den Dienst zu starten, nachdem die vorherige Anforderung abgeschlossen wurde:

        echo '{"RequestInfo": {"context" :"Restarting the Spark service"}, "Body": {"ServiceInfo": {"state": "STARTED"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

    Nach dem Neustart des Diensts werden die neuen Konfigurationseinstellungen verwendet.

5. Verwenden Sie abschließend Folgendes, um den Wartungsmodus zu deaktivieren:

        echo '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' | curl -u admin:PASSWORD -H "X-Requested-By: ambari" -X PUT -d "@-" "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/SPARK"

##Nächste Schritte

Eine vollständige Referenz der REST-API finden Sie unter [Referenz zur Ambari-API V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

> [AZURE.NOTE] Einige Ambari-Funktionen sind deaktiviert, da sie vom HDInsight-Clouddienst verwaltet werden, z. B. Hinzufügen oder Entfernen von Hosts im Cluster oder Hinzufügen neuer Dienste.

<!---HONumber=AcomDC_0817_2016-->