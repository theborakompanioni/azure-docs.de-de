---
title: Azure Service Fabric-Reverseproxy | Microsoft-Dokumentation
description: "Verwenden Sie den Reverseproxy von Service Fabric für die Kommunikation mit Microservices innerhalb und außerhalb des Clusters."
services: service-fabric
documentationcenter: .net
author: BharatNarasimman
manager: timlt
editor: vturecek
ms.assetid: 47f5c1c1-8fc8-4b80-a081-bc308f3655d3
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/08/2017
ms.author: bharatn
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: 7897458e9e4a0bbe185bd3f7b4c133c1b26769f9
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="reverse-proxy-in-azure-service-fabric"></a>Reverseproxy in Azure Service Fabric
Der in Azure Service Fabric integrierte Reverseproxy adressiert Microservices in dem Service Fabric-Cluster, der HTTP-Endpunkte verfügbar macht.

## <a name="microservices-communication-model"></a>Microservices-Kommunikationsmodell
Microservices werden in Service Fabric in der Regel in einer Teilmenge der VMs im Cluster ausgeführt und können aus verschiedenen Gründen aus einer VM in eine andere verschoben werden. Deshalb können sich die Endpunkte für Microservices dynamisch ändern. Das Standardmuster für die Kommunikation mit dem Microservice ist die nachstehende Auflösungsschleife:

1. Lösen Sie den Speicherort des Diensts zunächst über den Naming Service auf.
2. Stellen Sie eine Verbindung mit dem Dienst her.
3. Ermitteln Sie die Ursache von Verbindungsfehlern, und lösen Sie den Speicherort des Diensts bei Bedarf erneut auf.

Dieser Prozess umfasst im Allgemeinen das Umschließen der clientseitigen Kommunikationsbibliotheken in einer Wiederholungsschleife, die die Dienstauflösung und Wiederholungsrichtlinien implementiert.
Weitere Informationen finden Sie unter [Herstellung einer Verbindung mit Diensten in Service Fabric und die Kommunikation mit diesen Diensten](service-fabric-connect-and-communicate-with-services.md).

### <a name="communicating-by-using-the-reverse-proxy"></a>Kommunizieren mithilfe des Reverseproxys
Der Reverseproxy in Service Fabric wird auf allen Knoten im Cluster ausgeführt. Er übernimmt den gesamten Dienstauflösungsprozess im Auftrag des Clients und leitet anschließend die Clientanforderung weiter. Deshalb können im Cluster ausgeführte Clients alle clientseitigen HTTP-Kommunikationsbibliotheken nutzen, um mit dem Zieldienst über den Reverseproxy zu kommunizieren, der lokal auf dem gleichen Knoten ausgeführt wird.

![Interne Kommunikation][1]

## <a name="reaching-microservices-from-outside-the-cluster"></a>Zugreifen auf Microservices von außerhalb des Clusters
Das Standardmodell für die externe Kommunikation für Microservices ist ein Aktivierungsmodell, was bedeutet, dass externe Clients nicht direkt auf die einzelnen Dienste zugreifen können. [Azure Load Balancer](../load-balancer/load-balancer-overview.md) ist eine Netzwerkgrenze zwischen Microservices und externen Clients, und er übernimmt die Netzwerkadressübersetzung und leitet externe Anforderungen an interne IP:Port-Endpunkte weiter. Damit externe Clients direkt auf den Endpunkt eines Microservice zugreifen können, müssen Sie den Load Balancer zunächst zum Weiterleiten von Datenverkehr an die einzelnen Ports konfigurieren, die der Dienst im Cluster verwendet. Darüber hinaus sind die meisten Microservices, insbesondere zustandsbehaftete Microservices, nicht auf allen Knoten des Clusters aktiv. Die Microservices können bei einem Failover zwischen Knoten verschoben werden. In solchen Fällen kann Load Balancer die Position des Zielknotens der Replikate, an die der Datenverkehr weitergeleitet werden soll, nicht effektiv bestimmen.

### <a name="reaching-microservices-via-the-reverse-proxy-from-outside-the-cluster"></a>Zugreifen auf Microservices über den Reverseproxy von außerhalb des Clusters
Statt den Port eines einzelnen Diensts in Load Balancer zu konfigurieren, reicht es, wenn Sie nur den Port des Reverseproxys in Load Balancer konfigurieren. Mit dieser Konfiguration können Clients außerhalb des Clusters ohne zusätzliche Konfiguration über den Reverseproxy auf Dienste innerhalb des Clusters zugreifen.

![Externe Kommunikation][0]

> [!WARNING]
> Wenn Sie den Port des Reverseproxys in Load Balancer konfigurieren, können alle Microservices im Cluster, die einen HTTP-Endpunkt verfügbar machen, von außerhalb des Clusters adressiert werden.
>
>


## <a name="uri-format-for-addressing-services-by-using-the-reverse-proxy"></a>URI-Format für die Adressierung von Diensten über den Reverseproxy
Der Reverseproxy verwendet ein bestimmtes URI-Format (Uniform Resource Identifier), um die Dienstpartition zu bestimmen, an die die eingehende Anforderung weitergeleitet werden soll:

```
http(s)://<Cluster FQDN | internal IP>:Port/<ServiceInstanceName>/<Suffix path>?PartitionKey=<key>&PartitionKind=<partitionkind>&ListenerName=<listenerName>&TargetReplicaSelector=<targetReplicaSelector>&Timeout=<timeout_in_seconds>
```

* **HTTP(S):** Der Reverseproxy kann zum Akzeptieren von HTTP- oder HTTPS-Datenverkehr konfiguriert werden. Für die HTTPS-Weiterleitung lesen Sie die Informationen unter [Herstellen einer Verbindung mit einem sicheren Dienst mit dem Reverseproxy](service-fabric-reverseproxy-configure-secure-communication.md), nachdem Sie den Reverseproxy für das Lauschen von HTTPS eingerichtet haben.
* **Voll qualifizierter Domänenname (Fully Qualified Domain Name, FQDN) des Clusters | interne IP**: Für externe Clients können Sie den Reverseproxy so konfigurieren, dass er über die Clusterdomäne erreichbar ist, z.B. „mycluster.eastus.cloudapp.azure.com“. Standardmäßig wird der Reverseproxy auf jedem Knoten ausgeführt. Für internen Datenverkehr ist der Reverseproxy unter „localhost“ oder einer beliebigen internen Knoten-IP (z.B. 10.0.0.1) erreichbar.
* **Port:** Der Port, z.B. 19081, der für den Reverseproxy angegeben wurde.
* **ServiceInstanceName:** Der vollqualifizierte Name der bereitgestellten Dienstinstanz, den Sie erreichen möchten, ohne das Schema „fabric:/“. Um beispielsweise den Dienst *fabric:/myapp/myservice/* zu erreichen, müssen Sie *myapp/myservice* verwenden.

    Beim Dienstinstanznamen wird Groß-/Kleinschreibung beachtet. Eine andere Groß-/Kleinschreibung für den Dienstinstanznamen in der URL bewirkt, dass die Anforderungen mit 404 (Nicht gefunden) fehlschlagen.
* **Suffixpfad:** Der tatsächliche URL-Pfad des Diensts, mit dem Sie eine Verbindung herstellen möchten, z.B. *myapi/values/add/3*.
* **PartitionKey:** Für einen partitionierten Dienst ist dies der berechnete Partitionsschlüssel der Partition, die Sie erreichen möchten. Beachten Sie, dass dies *nicht* die GUID der Partitions-ID ist. Dieser Parameter ist für Dienste, die mit einem einzelnen Partitionsschema arbeiten, nicht erforderlich.
* **PartitionKind:** Das Partitionsschema des Diensts. Dies kann „Int64Range“ oder „Named“ sein. Dieser Parameter ist für Dienste, die mit einem einzelnen Partitionsschema arbeiten, nicht erforderlich.
* **ListenerName** Die Endpunkte des Diensts weisen folgendes Format auf: {"Endpoints":{"Listener1":"Endpoint1","Listener2":"Endpoint2" ...}} Wenn der Dienst mehrere Endpunkte verfügbar macht, identifiziert dieser Parameter den Endpunkt, an den die Clientanforderung weitergeleitet werden soll. Dieser Parameter kann ausgelassen werden, wenn der Dienst nur über einen Listener verfügt.
* **TargetReplicaSelector** Gibt an, wie das Zielreplikat oder die Zielinstanz ausgewählt werden soll.
  * Wenn der Zieldienst zustandsbehaftet ist, kann der TargetReplicaSelector entweder „PrimaryReplica“, „RandomSecondaryReplica“ oder „RandomReplica“ sein. Ist dieser Parameter nicht angegeben, lautet der Standardwert „PrimaryReplica“.
  * Wenn der Zieldienst zustandslos ist, wählt der Reverseproxy eine zufällige Instanz der Dienstpartition aus, an die die Anforderung weitergeleitet wird.
* **Timeout:** Gibt das Timeout für die HTTP-Anforderung an, die im Auftrag der Clientanforderung vom Reverseproxy für den Dienst erstellt wird. Der Standardwert beträgt 60 Sekunden. Dieser Parameter ist optional.

### <a name="example-usage"></a>Beispielverwendung
Nehmen wir als Beispiel den Dienst *fabric:/MyApp/MyService*, der einen HTTP-Listener für die folgende URL öffnet:

```
http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/
```

Dies sind die Ressourcen für den Dienst:

* `/index.html`
* `/api/users/<userId>`

Wenn der Dienst ein einzelnes Partitionierungsschema verwendet, sind die Abfragezeichenfolgen-Parameter *PartitionKey* und *PartitionKind* nicht erforderlich. Der Dienst kann wie folgt über das Gateway erreicht werden:

* Extern: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService`
* Intern: `http://localhost:19081/MyApp/MyService`

Wenn der Dienst das Partitionierungsschema „Uniform Int64“ verwendet, müssen die Abfragezeichenfolgen-Parameter *PartitionKey* und *PartitionKind* verwendet werden, um eine Partition des Diensts zu erreichen:

* Extern: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`
* Intern: `http://localhost:19081/MyApp/MyService?PartitionKey=3&PartitionKind=Int64Range`

Um die vom Dienst verfügbar gemachten Ressourcen zu erreichen, fügen Sie einfach hinter dem Dienstnamen den Ressourcenpfad in die URL ein:

* Extern: `http://mycluster.eastus.cloudapp.azure.com:19081/MyApp/MyService/index.html?PartitionKey=3&PartitionKind=Int64Range`
* Intern: `http://localhost:19081/MyApp/MyService/api/users/6?PartitionKey=3&PartitionKind=Int64Range`

Das Gateway leitet dann diese Anfragen an die Dienst-URL weiter:

* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/index.html`
* `http://10.0.0.5:10592/3f0d39ad-924b-4233-b4a7-02617c6308a6-130834621071472715/api/users/6`

## <a name="special-handling-for-port-sharing-services"></a>Spezielle Einrichtung für Dienste mit Portfreigabe
Das Azure Application Gateway versucht eine erneute Auflösung einer Dienstadresse und Wiederholung der Anforderung, wenn ein Dienst nicht erreicht werden kann. Dies ist ein Hauptvorteil des Application Gateways, da der Clientcode keine eigene Dienstauflösung und Auflösungsschleife implementieren muss.

Wenn ein Dienst nicht erreicht werden kann, bedeutet dies normalerweise, dass die Dienstinstanz oder das Replikat im Rahmen des normalen Lebenszyklus auf einen anderen Knoten verschoben wurde. In diesem Fall kann das Application Gateway eine Netzwerkverbindungs-Fehlermeldung erhalten, was bedeutet, dass ein Endpunkt in der ursprünglich aufgelösten Adresse nicht mehr geöffnet ist.

Allerdings können Replikate oder Dienstinstanzen einen Hostprozess und auch einen Port freigeben, wenn ein http.sys-basierter Webserver als Host fungiert, wie z.B.:

* [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener%28v=vs.110%29.aspx)
* [ASP.NET Core WebListener](https://docs.asp.net/latest/fundamentals/servers.html#weblistener)
* [Katana](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.OwinSelfHost/)

In dieser Situation ist es wahrscheinlich, dass der Webserver im Hostprozess verfügbar ist und auf Anforderungen antwortet, aber die aufgelöste Dienstinstanz oder das Replikat nicht mehr auf dem Host verfügbar ist. In diesem Fall empfängt das Gateway vom Webserver eine Antwort des Typs „HTTP 404“. Die HTTP 404-Antwort hat daher zwei unterschiedliche Bedeutungen:

- Fall 1: Die Dienstadresse ist richtig, aber die vom Benutzer angeforderte Ressource ist nicht vorhanden.
- Fall 2: Die Dienstadresse ist falsch, und die vom Benutzer angeforderte Ressource ist vielleicht auf einem anderen Knoten vorhanden.

Der erste Fall ist eine normale HTTP 404-Antwort, die als Benutzerfehler betrachtet wird. Allerdings hat der Benutzer im zweiten Fall eine Ressource angefordert, die nicht vorhanden ist. Application Gateway konnte sie nicht finden, da der Dienst selbst verschoben wurde. Application Gateway muss die Adresse erneut auflösen und die Anforderung wiederholen.

Application Gateway benötigt daher eine Möglichkeit, zwischen diesen beiden Fällen zu unterscheiden. Um diese Unterscheidung vornehmen zu können, ist ein Hinweis des Servers erforderlich.

* Standardmäßig geht Application Gateway vom 2. Fall aus und versucht, die Anforderung erneut aufzulösen und zu stellen.
* Um Application Gateway den 1. Fall anzugeben, muss der Dienst den folgenden HTTP-Antwortheader zurückgeben:

  `X-ServiceFabric : ResourceNotFound`

Dieser HTTP-Antwortheader gibt eine normale HTTP 404-Situation an, in der die angeforderte Ressource nicht vorhanden ist und Application Gateway nicht versucht, die Dienstadresse erneut aufzulösen.

## <a name="setup-and-configuration"></a>Setup und Konfiguration

### <a name="enable-reverse-proxy-via-azure-portal"></a>Aktivieren des Reverseproxys über das Azure-Portal

Das Azure-Portal bietet eine Option, mit der Sie beim Erstellen eines neuen Service Fabric-Clusters den Reverseproxy aktivieren können.
Aktivieren Sie unter **Service Fabric-Cluster erstellen** in Schritt 2 (Clusterkonfiguration) bei der Knotentypkonfiguration das Kontrollkästchen „Reverseproxy aktivieren“.
Zum Konfigurieren eines sicheren Reverseproxys können Sie in Schritt 3 (Sicherheit) beim Konfigurieren der Clustersicherheitseinstellungen ein SSL-Zertifikat angeben, indem Sie das Kontrollkästchen „SSL-Zertifikat für Reverseproxy einschließen“ aktivieren und die Zertifikatdetails eingeben.

### <a name="enable-reverse-proxy-via-azure-resource-manager-templates"></a>Aktivieren des Reverseproxys über Azure Resource Manager-Vorlagen

Sie können den Service Fabric-Reverseproxy über die [Azure Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md) für den Cluster aktivieren.

Unter [Configure HTTPS Reverse Proxy in a secure cluster](https://github.com/ChackDan/Service-Fabric/tree/master/ARM Templates/ReverseProxySecureSample#configure-https-reverse-proxy-in-a-secure-cluster) (Konfigurieren des HTTPS-Reverseproxys in einem sicheren Cluster) finden Sie Azure Resource Manager-Vorlagenbeispiele zum Konfigurieren eines sicheren Reverseproxys mit einem Zertifikat und Umgang mit einem Zertifikatrollover.

Zunächst rufen Sie die Vorlage für den Cluster ab, den Sie bereitstellen möchten. Sie können entweder die Beispielvorlagen verwenden oder eine benutzerdefinierte Resource Manager-Vorlage erstellen. Anschließend können Sie den Reverseproxy mit folgenden Schritten aktivieren:

1. Definieren Sie im Abschnitt [parameters](../azure-resource-manager/resource-group-authoring-templates.md) der Vorlage einen Port für den Reverseproxy.

    ```json
    "SFReverseProxyPort": {
        "type": "int",
        "defaultValue": 19081,
        "metadata": {
            "description": "Endpoint for Service Fabric Reverse proxy"
        }
    },
    ```
2. Geben Sie den Port für jedes der nodetype-Objekte im **Cluster**-[Ressourcentypenabschnitt](../azure-resource-manager/resource-group-authoring-templates.md) an.

    Der Port wird anhand des Parameternamens reverseProxyEndpointPort identifiziert.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        ...
       "nodeTypes": [
          {
           ...
           "reverseProxyEndpointPort": "[parameters('SFReverseProxyPort')]",
           ...
          },
        ...
        ],
        ...
    }
    ```
3. Um den Reverseproxy von außerhalb des Azure-Clusters zu adressieren, richten Sie für den in Schritt 1 angegebenen Port die Azure Load Balancer-Regeln ein.

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
4. Um SSL-Zertifikate für den Port des Reverseproxys zu konfigurieren, fügen Sie das Zertifikat der ***reverseProxyCertificate***-Eigenschaft im **Cluster**-[Ressourcentypenabschnitt](../resource-group-authoring-templates.md) hinzu.

    ```json
    {
        "apiVersion": "2016-09-01",
        "type": "Microsoft.ServiceFabric/clusters",
        "name": "[parameters('clusterName')]",
        "location": "[parameters('clusterLocation')]",
        "dependsOn": [
            "[concat('Microsoft.Storage/storageAccounts/', parameters('supportLogStorageAccountName'))]"
        ],
        "properties": {
            ...
            "reverseProxyCertificate": {
                "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                "x509StoreName": "[parameters('sfReverseProxyCertificateStoreName')]"
            },
            ...
            "clusterState": "Default",
        }
    }
    ```

### <a name="supporting-a-reverse-proxy-certificate-thats-different-from-the-cluster-certificate"></a>Unterstützung für Reverseproxyzertifikate, die sich vom Clusterzertifkat unterscheiden
 Wenn das Reverseproxyzertifikat sich von dem Zertifikat unterscheidet, das den Cluster schützt, sollte das zuvor angegebene Zertifikat auf dem virtuellen Computer installiert und der Zugriffssteuerungsliste (Access Control List, ACL) hinzugefügt worden sein, damit Service Fabric darauf zugreifen kann. Dies kann im [Ressourcentypenabschnitt](../resource-group-authoring-templates.md) **virtualMachineScaleSets** erfolgen. Fügen Sie für die Installation dieses Zertifikat dem osProfile hinzu. Der Erweiterungsabschnitt der Vorlage kann das Zertifikat in der ACL aktualisieren.

  ```json
  {
    "apiVersion": "[variables('vmssApiVersion')]",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    ....
      "osProfile": {
          "adminPassword": "[parameters('adminPassword')]",
          "adminUsername": "[parameters('adminUsername')]",
          "computernamePrefix": "[parameters('vmNodeType0Name')]",
          "secrets": [
            {
              "sourceVault": {
                "id": "[parameters('sfReverseProxySourceVaultValue')]"
              },
              "vaultCertificates": [
                {
                  "certificateStore": "[parameters('sfReverseProxyCertificateStoreValue')]",
                  "certificateUrl": "[parameters('sfReverseProxyCertificateUrlValue')]"
                }
              ]
            }
          ]
        }
   ....
   "extensions": [
          {
              "name": "[concat(parameters('vmNodeType0Name'),'_ServiceFabricNode')]",
              "properties": {
                      "type": "ServiceFabricNode",
                      "autoUpgradeMinorVersion": false,
                      ...
                      "publisher": "Microsoft.Azure.ServiceFabric",
                      "settings": {
                        "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
                        "nodeTypeRef": "[parameters('vmNodeType0Name')]",
                        "dataPath": "D:\\\\SvcFab",
                        "durabilityLevel": "Bronze",
                        "testExtension": true,
                        "reverseProxyCertificate": {
                          "thumbprint": "[parameters('sfReverseProxyCertificateThumbprint')]",
                          "x509StoreName": "[parameters('sfReverseProxyCertificateStoreValue')]"
                        },
                  },
                  "typeHandlerVersion": "1.0"
              }
          },
      ]
    }
  ```
> [!NOTE]
> Wenn Sie zum Aktivieren des Reverseproxys auf einem vorhandenen Cluster Zertifikate verwenden, die sich vom Clusterzertifikat unterscheiden, installieren Sie das Reverseproxyzertifikat, und aktualisieren Sie die ACL auf dem Cluster, bevor Sie den Reverseproxy aktivieren. Schließen Sie die Bereitstellung der [Azure Resource Manager-Vorlage](service-fabric-cluster-creation-via-arm.md) mit den oben angegebenen Einstellungen ab, bevor Sie eine Bereitstellung starten, um den Reverseproxy mit den Schritten 1–4 zu aktivieren.

## <a name="next-steps"></a>Nächste Schritte
* Ein Beispiel für die HTTP-Kommunikation zwischen Diensten finden Sie im [Beispielprojekt auf GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Weiterleitung an sicheren HTTP-Dienst mit dem Reverseproxy](service-fabric-reverseproxy-configure-secure-communication.md)
* [Remoteprozeduraufrufe mit Reliable Services-Remoting](service-fabric-reliable-services-communication-remoting.md)
* [Web-API, die OWIN in Reliable Services verwendet](service-fabric-reliable-services-communication-webapi.md)
* [WCF-Kommunikation mit Reliable Services](service-fabric-reliable-services-communication-wcf.md)
* Weitere Konfigurationsoptionen für den Reverseproxy finden Sie im Abschnitt zu ApplicationGateway/Http unter [Anpassen der Service Fabric-Clustereinstellungen](service-fabric-cluster-fabric-settings.md).

[0]: ./media/service-fabric-reverseproxy/external-communication.png
[1]: ./media/service-fabric-reverseproxy/internal-communication.png

