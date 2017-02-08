---
title: Konfigurieren des Lastenausgleichs-Verteilungsmodus | Microsoft Docs
description: "Konfigurieren des Verteilungsmodus für den Azure Load Balancer zur Unterstützung von Quell-IP-Affinität"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: 7df27a4d-67a8-47d6-b73e-32c0c6206e6e
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 664e668d6660fae956d4b6bdf0179abf67d743b1
ms.openlocfilehash: 1bc28b6f2979f47284a27a5a25becb8599b2ffab

---

# <a name="configure-the-distribution-mode-for-load-balancer"></a>Konfigurieren des Verteilungsmodus für den Lastenausgleich

## <a name="hash-based-distribution-mode"></a>Modus der hashbasierten Verteilung

Standardmäßig wird ein Fünf-Tupel-Hash-Verteilungsalgorithmus (Quell-IP, Quellport, IP-Zieladresse, Zielport, Protokolltyp) verwendet, um den Datenverkehr verfügbaren Servern zuzuordnen. Dabei wird Bindung nur in einer Transportsitzung angeboten. Pakete in der gleichen Sitzung werden an die gleiche Instanz der Rechenzentrums-IP (DIP) hinter dem Lastenausgleichs-Endpunkt geleitet. Wenn der Client eine neue Sitzung über die gleiche Quell-IP startet, wird der Quellport geändert, wodurch der Datenverkehr an einen anderen DIP-Endpunkt geleitet wird.

![Lastenausgleich auf Hashbasis](./media/load-balancer-distribution-mode/load-balancer-distribution.png)

Abbildung 1: Fünf-Tupel-Verteilung

## <a name="source-ip-affinity-mode"></a>Modus „Quell-IP-Affinität“

Uns steht ein weiterer Verteilungsmodus namens Quell-IP-Affinität (auch Sitzungsaffinität oder Client-IP-Affinität genannt) zur Verfügung. Azure Load Balancer kann für die Verwendung von zwei Tupeln (Quell-IP, Ziel-IP) oder drei Tupeln (Quell-IP, Ziel-IP, Protokoll) konfiguriert werden, um Datenverkehr den verfügbaren Servern zuzuordnen. Mithilfe der Quell-IP-Affinität werden Verbindungen, die vom gleichen Clientcomputer initiiert werden, an den gleichen DIP-Endpunkt geleitet.

Das folgende Diagramm veranschaulicht eine Konfiguration mit zwei Tupeln. Beachten Sie, dass die beiden Tupel durch den Lastenausgleich zum ersten virtuellen Computer (VM1) führen, der dann durch VM2 und VM3 abgesichert wird.

![Sitzungsaffinität](./media/load-balancer-distribution-mode/load-balancer-session-affinity.png)

Abbildung 2: Zwei-Tupel-Verteilung

Mit der Quell-IP-Affinität wird das Problem der Inkompatibilität zwischen Azure Load Balancer und RD-Gateway (Remotedesktop) gelöst. Jetzt können Sie eine RD-Gatewayfarm in einem einzelnen Clouddienst erstellen.

Ein anderer Anwendungsfall ist das Hochladen von Medien, wobei der Datenupload über UDP erfolgt, während die Steuerungsebene über TCP realisiert wird:

* Ein Client initiiert zunächst eine TCP-Sitzung mit der öffentlichen Adresse mit Lastenausgleich, wird dann an eine bestimmte DIP-Adresse geleitet, und dieser Kanal bleibt zur Überwachung des Zustands der Verbindung aktiv.
* Eine neue UDP-Sitzung des gleichen Clientcomputers wird auf dem gleichen öffentlichen Endpunkt mit Lastenausgleich initiiert. Es wird hier davon ausgegangen, dass diese Verbindung auch an den gleichen DIP-Endpunkt geleitet wird wie die vorherige TCP-Verbindung, sodass der Medienupload mit hohem Durchsatz ausgeführt werden kann, während ein Steuerungskanal über TCP erhalten bleibt.

> [!NOTE]
> Bei einer Änderung der Lastenausgleichsgruppe (Entfernen oder Hinzufügen eines virtuellen Computers) wird die Verteilung der Clientanforderungen neu berechnet. Sie können sich nicht darauf verlassen, dass neue Verbindungen von vorhandenen Clients beim gleichen Server landen. Darüber hinaus kann die Verwendung des Quell-IP-Affinitäts-Verteilungsmodus zu einer ungleichen Verteilung des Datenverkehrs führen. Clients, die hinter Proxys ausgeführt werden, können als eine einzige Clientanwendung betrachtet werden.

## <a name="configuring-source-ip-affinity-settings-for-load-balancer"></a>Konfigurieren von Quell-IP-Affinitätseinstellungen für Lastenausgleich

Für virtuelle Computer können Sie per PowerShell die Timeouteinstellungen ändern:

Fügen Sie einem virtuellen Computer einen Azure-Endpunkt hinzu, und legen Sie den Lastenausgleichs-Verteilungsmodus fest.

```powershell
Get-AzureVM -ServiceName mySvc -Name MyVM1 | Add-AzureEndpoint -Name HttpIn -Protocol TCP -PublicPort 80 -LocalPort 8080 –LoadBalancerDistribution sourceIP | Update-AzureVM
```

„LoadBalancerDistribution“ kann für eine Zwei-Tupel-Konfiguration (Quell-IP, Ziel-IP) auf „sourceIP“, für eine Drei-Tupel-Konfiguration (Quell-IP, Ziel-IP, Protokoll) auf „sourceIPProtocol“ oder gar nicht festgelegt werden, um das Standardverhalten (Fünf-Tupel-Lastenausgleich) zu verwenden.

Verwenden Sie Folgendes, um eine Lastenausgleichs-Verteilungsmoduskonfiguration für einen Endpunkt abzurufen:

    PS C:\> Get-AzureVM –ServiceName MyService –Name MyVM | Get-AzureEndpoint

    VERBOSE: 6:43:50 PM - Completed Operation: Get Deployment
    LBSetName : MyLoadBalancedSet
    LocalPort : 80
    Name : HTTP
    Port : 80
    Protocol : tcp
    Vip : 65.52.xxx.xxx
    ProbePath :
    ProbePort : 80
    ProbeProtocol : tcp
    ProbeIntervalInSeconds : 15
    ProbeTimeoutInSeconds : 31
    EnableDirectServerReturn : False
    Acl : {}
    InternalLoadBalancerName :
    IdleTimeoutInMinutes : 15
    LoadBalancerDistribution : sourceIP

Wenn das Element "LoadBalancerDistribution" nicht vorhanden ist, verwendet der Azure-Lastenausgleich den standardmäßigen 5-Tupel-Algorithmus.

### <a name="set-the-distribution-mode-on-a-load-balanced-endpoint-set"></a>Festlegen des Verteilungsmodus für einen Endpunktsatz mit Lastenausgleich

Wenn Endpunkte Bestandteil eines Endpunktsatzes mit Lastenausgleich sind, muss der Verteilungsmodus für den Endpunktsatz mit Lastenausgleich festgelegt werden:

```powershell
Set-AzureLoadBalancedEndpoint -ServiceName MyService -LBSetName LBSet1 -Protocol TCP -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 –LoadBalancerDistribution sourceIP
```

### <a name="cloud-service-configuration-to-change-distribution-mode"></a>Clouddienstkonfiguration zum Ändern des Verteilungsmodus

Sie können das (voraussichtlich im November erscheinende) Azure SDK für .NET 2.5 verwenden, um Ihren Clouddienst zu aktualisieren. Endpunkteinstellungen für Cloud-Dienste werden in der CSDEF-Datei vorgenommen. Um den Lastenausgleichs-Verteilungsmodus für eine Bereitstellung mit Cloud-Diensten zu aktualisieren, ist ein Upgrade der Bereitstellung erforderlich.
Nachfolgend sehen Sie ein Beispiel für in der CSDEF-Datei vorgenommene Änderungen für Endpunkteinstellungen:

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancerDistribution="sourceIP" />
    </Endpoints>
</WorkerRole>
<NetworkConfiguration>
    <VirtualNetworkSite name="VNet"/>
    <AddressAssignments>
<InstanceAddress roleName="VMRolePersisted">
    <PublicIPs>
    <PublicIP name="public-ip-name" idleTimeoutInMinutes="timeout-in-minutes"/>
    </PublicIPs>
</InstanceAddress>
    </AddressAssignments>
</NetworkConfiguration>
```

## <a name="api-example"></a>API-Beispiel

Sie können die Lastenausgleichsverteilung mithilfe der Dienstverwaltungs-API konfigurieren. Stellen Sie sicher, dass der `x-ms-version`-Header eingefügt und auf Version `2014-09-01` oder höher festgelegt wird.

### <a name="update-the-configuration-of-the-specified-load-balanced-set-in-a-deployment"></a>Aktualisieren der Konfiguration für den angegebenen Endpunktsatz mit Lastenausgleich in einer Bereitstellung

#### <a name="request-example"></a>Anforderungsbeispiel

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>?comp=UpdateLbSet   x-ms-version: 2014-09-01
    Content-Type: application/xml

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
      <InputEndpoint>
        <LoadBalancedEndpointSetName> endpoint-set-name </LoadBalancedEndpointSetName>
        <LocalPort> local-port-number </LocalPort>
        <Port> external-port-number </Port>
        <LoadBalancerProbe>
          <Port> port-assigned-to-probe </Port>
          <Protocol> probe-protocol </Protocol>
          <IntervalInSeconds> interval-of-probe </IntervalInSeconds>
          <TimeoutInSeconds> timeout-for-probe </TimeoutInSeconds>
        </LoadBalancerProbe>
        <Protocol> endpoint-protocol </Protocol>
        <EnableDirectServerReturn> enable-direct-server-return </EnableDirectServerReturn>
        <IdleTimeoutInMinutes>idle-time-out</IdleTimeoutInMinutes>
        <LoadBalancerDistribution>sourceIP</LoadBalancerDistribution>
      </InputEndpoint>
    </LoadBalancedEndpointList>

Der Wert von „LoadBalancerDistribution“ kann „sourceIP“ (Zwei-Tupel-Affinität) oder „sourceIPProtocol“ (Drei-Tupel-Affinität) lauten oder nicht festgelegt werden (keine Affinität, also Fünf Tupel).

#### <a name="response"></a>Antwort

    HTTP/1.1 202 Accepted
    Cache-Control: no-cache
    Content-Length: 0
    Server: 1.0.6198.146 (rd_rdfe_stable.141015-1306) Microsoft-HTTPAPI/2.0
    x-ms-servedbyregion: ussouth2
    x-ms-request-id: 9c7bda3e67c621a6b57096323069f7af
    Date: Thu, 16 Oct 2014 22:49:21 GMT

## <a name="next-steps"></a>Nächste Schritte

[Interner Lastenausgleich (Übersicht)](load-balancer-internal-overview.md)

[Erste Schritte zum Konfigurieren des Lastenausgleichs für Internetverbindungen](load-balancer-get-started-internet-arm-ps.md)

[Konfigurieren von TCP-Leerlauftimeout-Einstellungen für den Lastenausgleich](load-balancer-tcp-idle-timeout.md)



<!--HONumber=Nov16_HO3-->


