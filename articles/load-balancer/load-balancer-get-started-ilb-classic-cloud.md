---
title: "Erstellen eines internen Lastenausgleichs für Azure Cloud Services | Microsoft-Dokumentation"
description: Hier erfahren Sie, wie Sie mithilfe von PowerShell einen internen Load Balancer im klassischen Bereitstellungsmodell erstellen.
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 57966056-0f46-4f95-a295-483ca1ad135d
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: e32ffa81f7465682579eec92087b98aebbe3c4a8

---

# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a>Erste Schritte zum Erstellen eines internen Lastenausgleichs (klassisch) für Clouddienste

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure-Befehlszeilenschnittstelle](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Clouddienste](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md).  Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Erfahren Sie, wie Sie [diese Schritte mit dem Resource Manager-Modell ausführen](load-balancer-get-started-ilb-arm-ps.md).

## <a name="configure-internal-load-balancer-for-cloud-services"></a>Konfigurieren des internen Lastenausgleichs für Clouddienste

Der interne Lastenausgleich wird sowohl für virtuelle Computer als auch für Clouddienste unterstützt. Ein Endpunkt eines internen Lastenausgleichs, der in einem Clouddienst außerhalb eines regionalen virtuellen Netzwerks erstellt wurde, ist nur innerhalb des Clouddiensts verfügbar.

Die Konfiguration des internen Lastenausgleichs muss während der Erstellung der ersten Bereitstellung im Clouddienst festgelegt werden, wie im folgenden Beispiel dargestellt.

> [!IMPORTANT]
> Voraussetzung für die Ausführung der folgenden Schritte ist, dass Sie bereits ein virtuelles Netzwerk für die Cloudbereitstellung erstellt haben. Zum Erstellen des internen Lastenausgleichs benötigen Sie den Namen des virtuellen Netzwerks und den Subnetznamen.

### <a name="step-1"></a>Schritt 1

Öffnen Sie die Dienstkonfigurationsdatei (CSCFG) für Ihre Cloudbereitstellung in Visual Studio, und fügen Sie den folgenden Abschnitt hinzu, um den internen Lastausgleich unter dem letzten "`</Role>`"-Element für die Netzwerkkonfiguration zu erstellen.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of the load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Wir fügen nun die Werte für die Netzwerkkonfigurationsdatei hinzu, um den Vorgang zu veranschaulichen. Im Beispiel wird davon ausgegangen, dass Sie ein Subnetz mit dem Namen „test_vnet“ und einem 10.0.0.0/24-Subnetz erstellt haben, das „test_subnet“ heißt und die statische IP 10.0.0.4 aufweist. Der Load Balancer hat den Namen testLB.

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

Weitere Informationen zum Lastenausgleichsschema finden Sie unter [Hinzufügen eines Lastenausgleichs](https://msdn.microsoft.com/library/azure/dn722411.aspx).

### <a name="step-2"></a>Schritt 2

Ändern Sie die Dienstdefinitionsdatei (CSDEF), um Endpunkte zum internen Lastenausgleich hinzuzufügen. Zum Zeitpunkt der Erstellung einer Rolleninstanz fügt die Dienstdefinitionsdatei die Rolleninstanzen zum internen Lastenausgleich hinzu.

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

Als Nächstes fügen wir der Dienstdefinitionsdatei die Werte hinzu, indem wir die Werte aus dem obigen Beispiel verwenden.

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

Der Lastenausgleich für den Netzwerkdatenverkehr erfolgt für eingehende Anforderungen mit dem Lastenausgleichsmodul "testLB" über Port 80. Die Workerrolleninstanzen werden ebenfalls über Port 80 gesendet.

## <a name="next-steps"></a>Nächste Schritte

[Konfigurieren eines Load Balancer-Verteilungsmodus mithilfe der Quell-IP-Affinität](load-balancer-distribution-mode.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)




<!--HONumber=Jan17_HO4-->


