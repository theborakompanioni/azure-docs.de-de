<properties
   pageTitle="Service Fabric-Reverseproxy | Microsoft Azure"
   description="Verwenden Sie den Reverseproxy von Service Fabric für die Kommunikation mit Microservices innerhalb und außerhalb des Clusters."
   services="service-fabric"
   documentationCenter=".net"
   authors="BharatNarasimman,vturecek"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="required"
   ms.date="07/26/2016"
   ms.author="vturecek"/>

# Service Fabric-Reverseproxy

Der Service Fabric-Reverseproxy ist ein in Service Fabric integrierter Reverseproxy, der die Adressierung von Microservices im Service Fabric-Cluster ermöglicht, die HTTP-Endpunkte verfügbar machen.

## Microservices-Kommunikationsmodell

Microservices werden in Service Fabric in der Regel in einer Teilmenge der VMs im Cluster ausgeführt und können aus verschiedenen Gründen aus einer VM in eine andere verschoben werden. Deshalb können sich die Endpunkte für Microservices dynamisch ändern. Das Standardmuster für die Kommunikation mit dem Microservice ist die nachstehende Auflösungsschleife.

1. Lösen Sie den Speicherort des Diensts zunächst über den Naming Service auf.
2. Stellen Sie eine Verbindung mit dem Dienst her.
3. Ermitteln Sie die Ursache von Verbindungsfehlern, und lösen Sie den Speicherort des Diensts bei Bedarf erneut auf.

Dieser Prozess umfasst im Allgemeinen das Umschließen der clientseitigen Kommunikationsbibliotheken in einer Wiederholungsschleife, die die Dienstauflösung und Wiederholungsrichtlinien implementiert. Weitere Informationen zu diesem Thema finden Sie unter [Kommunizieren mit Diensten](service-fabric-connect-and-communicate-with-services.md).

### Kommunizieren über den Service Fabric-Reverseproxy
Der Service Fabric-Reverseproxy wird auf allen Knoten im Cluster ausgeführt. Er übernimmt den gesamten Dienstauflösungsprozess im Auftrag des Clients und leitet anschließend die Clientanforderung weiter. Deshalb können im Cluster ausgeführte Clients alle clientseitigen HTTP-Kommunikationsbibliotheken nutzen, um mit dem Zieldienst über den Service Fabric-Reverseproxy zu kommunizieren, der lokal auf demselben Knoten ausgeführt wird.

![Interne Kommunikation][1]

## Zugreifen auf Microservices von außerhalb des Clusters
Das Standardmodell für die externe Kommunikation für Microservices ist **Aktivieren**, was heißt, dass externe Clients nicht direkt auf die einzelnen Dienste zugreifen können. Der [Azure Load Balancer](../load-balancer/load-balancer-overview.md) ist eine Netzwerkgrenze zwischen Microservices und externen Clients. Er übernimmt die Netzwerkadressübersetzung und leitet externe Anforderungen an interne **IP-Adresse:Port**-Endpunkte weiter. Damit externe Clients direkt auf den Endpunkt eines Microservice zugreifen können, muss der Azure Load Balancer zunächst zum Weiterleiten von Datenverkehr an die einzelnen Ports konfiguriert werden, die vom Dienst im Cluster verwendet werden. Darüber hinaus befinden sich die meisten Microservices (insbesondere zustandsbehaftete Microservices) nicht auf allen Knoten des Clusters und können bei einem Failover zwischen Knoten verschoben werden. Deshalb kann der Azure Load Balancer in solchen Fällen nicht effektiv den Zielknoten der Replikate bestimmen, zu denen der Datenverkehr geleitet werden soll.

### Zugreifen auf Microservices über den Service Fabric-Reverseproxy von außerhalb des Clusters

Anstatt Ports für einzelne Dienste im Azure Load Balancer zu konfigurieren, reicht es aus, den Port des Service Fabric-Reverseproxys im Azure Load Balancer zu konfigurieren. Dadurch können Clients außerhalb des Clusters ohne zusätzliche Konfiguration über den Reverseproxy auf Dienste innerhalb des Clusters zugreifen.

![Externe Kommunikation][0]

>[AZURE.WARNING] Durch das Konfigurieren des Ports des Reverseproxys im Load Balancer können alle Microservices im Cluster, die einen HTTP-Endpunkt verfügbar machen, von außerhalb des Clusters adressiert werden.


## URI-Format für die Adressierung von Diensten über den Reverseproxy

Der Reverseproxy verwendet ein bestimmtes URI-Format, um die Dienstpartition zu bestimmen, an die die eingehende Anforderung weitergeleitet werden soll:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&Timeout=<timeout_in_seconds>
```

 - **HTTP(S):** Der Reverseproxy kann zum Akzeptieren von HTTP- oder HTTPS-Datenverkehr konfiguriert werden. Bei HTTPS-Datenverkehr erfolgt die SSL-Beendigung im Reverseproxy. Anforderungen, die vom Reverseproxy an Dienste im Cluster weitergeleitet werden, erfolgen über HTTP.
 - **Cluster-FQDN| internal IP:** For external clients, the reverse proxy can be configured so that it is reachable through the cluster domain (e.g., mycluster.eastus.cloudapp.azure.com). By default the reverse proxy runs on every node, so for internal traffic it can be reached on localhost or on any internal node IP (e.g., 10.0.0.1).
 - **Port:** Der Port, der für den Reverseproxy angegeben wurde. Beispiel: 19008.
 - **ServiceInstanceName:** Der vollqualifizierte Name der bereitgestellten Dienstinstanz des Diensts, den Sie erreichen möchten (ohne das Schema „fabric:/“). Um beispielsweise den Dienst *fabric:/myapp/myservice/* zu erreichen, müssen Sie *myapp/myservice* verwenden.
 - **Suffixpfad:** Der tatsächliche URL-Pfad des Diensts, mit dem Sie eine Verbindung herstellen möchten. Beispiel: *myapi/values/add/3*
 - **PartitionKey:** Für einen partitionierten Dienst ist dies der berechnete Partitionsschlüssel der Partition, die Sie erreichen möchten. Beachten Sie, dass dies *nicht* die GUID der Partitions-ID ist. Dieser Parameter ist für Dienste, die mit einem einzelnen Partitionsschema arbeiten, nicht erforderlich.
 - **PartitionKind:** Das Partitionsschema des Diensts. Dies kann „Int64Range“ oder „Named“ sein. Dieser Parameter ist für Dienste, die mit einem einzelnen Partitionsschema arbeiten, nicht erforderlich.
 - **Timeout:** Gibt das Timeout für die HTTP-Anforderung an, die im Auftrag der Clientanforderung vom Reverseproxy für den Dienst erstellt wird. Der Standardwert hierfür ist 60 Sekunden. Dieser Parameter ist optional.

### Beispielverwendung

Nehmen wir als Beispiel den Dienst **fabric:/MyApp/MyService**, der einen HTTP-Listener für die folgende URL öffnet:

```
http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Mit den folgenden Ressourcen:

 - `/index.html`
 - `/api/users/<userId>`

Wenn der Dienst ein einzelnes Partitionierungsschema verwendet, sind die Abfragezeichenfolgen-Parameter *PartitionKey* und *PartitionKind* nicht erforderlich. Der Dienst kann wie folgt über das Gateway erreicht werden:

 - Extern: `http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService`
 - Intern: `http://localhost:19008/MyApp/MyService`

Wenn der Dienst das Partitionierungsschema „Uniform Int64“ verwendet, müssen die Abfragezeichenfolgen-Parameter *PartitionKey* und *PartitionKind* verwendet werden, um eine Partition des Diensts zu erreichen:

 - Extern: `http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
 - Intern: `http://localhost:19008/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Um die vom Dienst verfügbar gemachten Ressourcen zu erreichen, fügen Sie einfach hinter dem Dienstnamen den Ressourcenpfad in die URL ein:

 - Extern: `http://mycluster.eastus.cloudapp.azure.com:19008/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
 - Intern: `http://localhost:19008/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Das Gateway leitet dann diese Anfragen an die Dienst-URL weiter:

 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
 - `http://10.0.05:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## Spezielle Einrichtung für Dienste mit Portfreigabe

Das Application Gateway versucht eine erneute Auflösung einer Dienstadresse und Wiederholung der Anforderung, wenn ein Dienst nicht erreicht werden kann. Dies ist einer der Hauptvorteile des Gateways, da der Clientcode keine eigene Dienstauflösung und Auflösungsschleife implementieren muss.

Wenn ein Dienst nicht erreicht werden kann, bedeutet dies normalerweise, dass die Dienstinstanz oder das Replikat im Rahmen des normalen Lebenszyklus auf einen anderen Knoten verschoben wurde. In diesem Fall kann das Gateway einen Netzwerkverbindungsfehler erhalten, was bedeutet, dass ein Endpunkt in der ursprünglich aufgelösten Adresse nicht mehr geöffnet ist.

Allerdings können Replikate oder Dienstinstanzen einen Hostprozess und auch einen Port freigeben, wenn ein auf HTTP.SYS basierender Webserver als Host fungiert, wie z.B.:

 - [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
 - [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
 - [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

In dieser Situation ist es wahrscheinlich, dass der Webserver im Hostprozess verfügbar ist und auf Anforderungen antwortet, aber die aufgelöste Dienstinstanz oder das Replikat nicht mehr auf dem Host verfügbar ist. In diesem Fall empfängt das Gateway vom Webserver eine Antwort des Typs „HTTP 404“. Die HTTP 404-Antwort hat daher zwei unterschiedliche Bedeutungen:

 1. Die Dienstadresse ist richtig, aber die vom Benutzer angeforderte Ressource ist nicht vorhanden.
 2. Die Dienstadresse ist falsch, und die vom Benutzer angeforderte Ressource ist möglicherweise auf einem anderen Knoten tatsächlich vorhanden.

Im ersten Fall ist dies eine normale HTTP 404-Antwort, die als Benutzerfehler betrachtet wird. Doch im zweiten Fall hat der Benutzer eine Ressource angefordert, die vorhanden ist. Das Gateway war allerdings nicht in der Lage, sie zu finden, da der Dienst selbst verschoben wurde. In diesem Fall muss das Gateway die Adresse erneut auflösen und den Vorgang wiederholen.

Das Gateway benötigt daher eine Möglichkeit zur Unterscheidung zwischen diesen beiden Fällen. Um diese Unterscheidung vornehmen zu können, ist ein Hinweis des Servers erforderlich.

 - Standardmäßig geht das Application Gateway vom 2. Fall aus und versucht, die Anforderung erneut aufzulösen und zu stellen.
 - Um dem Application Gateway den 1. Fall anzugeben, muss der Dienst den folgenden HTTP-Antwortheader zurückgeben:

`X-ServiceFabric : ResourceNotFound`

Dieser HTTP-Antwortheader gibt eine normale HTTP 404-Situation an, in der die angeforderte Ressource nicht vorhanden ist und das Gateway nicht versucht, die Dienstadresse erneut aufzulösen.

## Setup und Konfiguration
Der Service Fabric-Reverseproxy kann über die [Azure Resource Manager-Vorlage](./service-fabric-cluster-creation-via-arm.md) für den Cluster aktiviert werden.

Sobald Sie über die Vorlage für den Cluster verfügen, die Sie bereitstellen möchten (entweder aus den Beispielvorlagen oder durch Erstellen einer benutzerdefinierten Resource Manager-Vorlage), kann der Reverseproxy über die folgenden Schritte in der Vorlage aktiviert werden.

1. Definieren Sie im Abschnitt [parameters](../resource-group-authoring-templates.md) der Vorlage einen Port für den Reverseproxy.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19008,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Geben Sie den Port für jedes der nodetype-Objekte im [Abschnitt der Ressourcentypen](../resource-group-authoring-templates.md) des **Clusters** an.

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "httpApplicationGatewayEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. Um den Reverseproxy von außerhalb des Azure-Clusters zu adressieren, richten Sie für den in Schritt 1 angegebenen Port die **Azure Load Balancer-Regeln** ein.

    ```json
    {
        "apiVersion": "[variables('lbApiVersion')]",
        "type": "Microsoft.Network/loadBalancers",
        ...
        ...
        "loadBalancingRules": [
            ...
            {
                "name": "LBSFReverseProxyRule",
                "properties": {
                    "backendAddressPool": {
                        "id": "[variables('lbPoolID0')]"
                    },
                    "backendPort": "[parameters('SFReverseProxyPort')]",
                    "enableFloatingIP": "false",
                    "frontendIPConfiguration": {
                        "id": "[variables('lbIPConfig0')]"
                    },
                    "frontendPort": "[parameters('SFReverseProxyPort')]",
                    "idleTimeoutInMinutes": "5",
                    "probe": {
                        "id": "[concat(variables('lbID0'),'/probes/SFReverseProxyProbe')]"
                    },
                    "protocol": "tcp"
                }
            }
        ],
        "probes": [
            ...
            {
                "name": "SFReverseProxyProbe",
                "properties": {
                    "intervalInSeconds": 5,
                    "numberOfProbes": 2,
                    "port":     "[parameters('SFReverseProxyPort')]",
                    "protocol": "tcp"
                }
            }  
        ]
    }
    ```
4. Um SSL-Zertifikate für den Port des Reverseproxys zu konfigurieren, fügen Sie das Zertifikat der httpApplicationGatewayCertificate-Eigenschaft im Abschnitt der [Ressourcentypen](../resource-group-authoring-templates.md) des **Clusters** hinzu.

    ```json
    {
        "apiVersion": "2016-03-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "httpApplicationGatewayCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

## Nächste Schritte
 - Ein Beispiel für die HTTP-Kommunikation zwischen Diensten finden Sie im [Beispielprojekt auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/master/Services/WordCount).

 - [Remoteprozeduraufrufe mit Reliable Services-Remoting](service-fabric-reliable-services-communication-remoting.md)

 - [Web-API, die OWIN in Reliable Services verwendet](service-fabric-reliable-services-communication-webapi.md)

 - [WCF-Kommunikation mit Reliable Services](service-fabric-reliable-services-communication-wcf.md)


[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png

<!---HONumber=AcomDC_0921_2016-->