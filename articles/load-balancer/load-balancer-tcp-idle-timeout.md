---
title: Konfigurieren des TCP-Leerlauftimeouts für den Load Balancer | Microsoft Docs
description: Konfigurieren des TCP-Leerlauftimeouts für den Load Balancer
services: load-balancer
documentationcenter: na
author: sdwheeler
manager: carmonm
editor: ''

ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/03/2016
ms.author: sewhee

---
# Ändern von TCP-Leerlauftimeout-Einstellungen für den Load Balancer
In der Standardkonfiguration ist der Azure Load Balancer auf ein Leerlauftimeout von 4 Minuten eingestellt.

Dies bedeutet: Wenn die Dauer einer Inaktivitätsperiode den Timeoutwert überschreitet, gibt es keine Garantie dafür, dass die TCP- oder HTTP-Sitzung zwischen dem Client und Ihrem Clouddienst noch besteht.

Sobald die Verbindung geschlossen wird, wird in der Clientanwendung eine Fehlermeldung ähnlich dieser angezeigt: „Die zugrunde liegende Verbindung wurde geschlossen: Eine Verbindung, deren Aufrechterhaltung erwartet wurde, ist vom Server geschlossen worden.“

Eine gängige Praxis, die Verbindung über einen längeren Zeitraum aktiv zu halten, ist die Verwendung von TCP-Keep-alive. (Sie finden [.NET-Beispiele ](https://msdn.microsoft.com/library/system.net.servicepoint.settcpkeepalive.aspx).) Pakete werden gesendet, wenn keine Aktivität der Verbindung erkannt wird. Diese Netzwerkaktivität garantiert, dass der Wert für den Leerlauftimeout niemals erreicht und die Verbindung über einen langen Zeitraum aufrechterhalten wird.

Um den Verlust der Verbindung zu vermeiden, müssen Sie TCP-Keep-alive mit einem Intervall von weniger als der Leerlauftimeout-Einstellung konfigurieren bzw. den Leerlauftimeout-Wert erhöhen.

Während TCP-Keep-alive für Szenarien gut funktioniert, in denen Akkuleistung keine Rolle spielt, empfehlen wir dies im Allgemeinen nicht für mobile Anwendungen. TCP-Keep-alive verkürzt in einer mobilen Anwendung wahrscheinlich die Akkulebensdauer.

Für solche Szenarien haben wir Unterstützung für konfigurierbare Leerlauftimeouts hinzugefügt. Sie können nun eine Dauer von 4 bis 30 Minuten festlegen. Diese Einstellung funktioniert nur für eingehende Verbindungen.

![TCP-Timeout](./media/load-balancer-tcp-idle-timeout/image1.png)

In den folgenden Abschnitten wird das Ändern von Leerlauftimeout-Einstellungen von virtuellen Computern und Clouddiensten beschrieben.

> [!NOTE]
> Um die Konfiguration dieser Einstellungen zu unterstützen, stellen Sie sicher, dass Sie das neueste Azure PowerShell-Paket installiert haben.
> 
> 

## Festlegen des TCP-Timeouts für Ihre öffentliche IP auf Instanzebene auf 15 Minuten
    Set-AzurePublicIP -PublicIPName webip -VM MyVM -IdleTimeoutInMinutes 15

`IdleTimeoutInMinutes` ist optional. Wenn dieser Wert nicht festgelegt ist, beträgt das Standardtimeout 4 Minuten.

> [!NOTE]
> Der zulässige Timeoutbereich beträgt 4 bis 30 Minuten.
> 
> 

## Festlegen des Leerlauftimeouts beim Erstellen eines Azure-Endpunkts auf einem virtuellen Computer
Ändern der Timeouteinstellung für einen Endpunkt:

    Get-AzureVM -ServiceName "mySvc" -Name "MyVM1" | Add-AzureEndpoint -Name "HttpIn" -Protocol "tcp" -PublicPort 80 -LocalPort 8080 -IdleTimeoutInMinutes 15| Update-AzureVM

Abrufen Ihrer Leerlauftimeout-Konfiguration:

    PS C:\> Get-AzureVM -ServiceName "MyService" -Name "MyVM" | Get-AzureEndpoint
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

## Festlegen des TCP-Timeouts für einen Endpunktsatz mit Lastenausgleich
Wenn Endpunkte Bestandteil eines Endpunktsatzes mit Lastenausgleich sind, muss das TCP-Timeout für den Endpunktsatz mit Lastenausgleich festgelegt werden:

    Set-AzureLoadBalancedEndpoint -ServiceName "MyService" -LBSetName "LBSet1" -Protocol tcp -LocalPort 80 -ProbeProtocolTCP -ProbePort 8080 -IdleTimeoutInMinutes 15

## Ändern von Timeouteinstellungen für Clouddienste
Sie können das Azure SDK für .NET 2.4 verwenden, um Ihren Clouddienst zu aktualisieren.

Endpunkteinstellungen für Clouddienste nehmen Sie in der CSDEF-Datei vor. Das Aktualisieren des TCP-Timeouts für die Bereitstellung eines Clouddiensts erfordert ein Upgrade der Bereitstellung. Ausnahme: Das TCP-Timeout wird nur für eine öffentliche IP festgelegt. Einstellungen für die öffentliche IP sind in der CSCFG-Datei gespeichert, und Sie können sie während des Updates und Upgrades der Bereitstellung aktualisieren.

In der CSDEF-Datei müssen die Einstellungen für einen Endpunkt folgendermaßen geändert werden:

    <WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
      <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" idleTimeoutInMinutes="tcp-timeout" />
      </Endpoints>
    </WorkerRole>

Die Änderungen in der CSCFG-Datei für die Timeouteinstellung für öffentliche IP-Adressen lauten:

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

## REST-API-Beispiel
Sie können das TCP-Leerlauftimeout mithilfe der Dienstverwaltungs-API konfigurieren. Stellen Sie sicher, dass der x-ms-version-Header auf Version 2014-06-01 oder höher festgelegt ist.

Aktualisieren Sie die Konfiguration der angegebenen Eingabeendpunkte mit Lastenausgleich auf allen virtuellen Computern in einer Bereitstellung.

    Request:

    POST https://management.core.windows.net/<subscription-id>/services/hostedservices/<cloudservice-name>/deployments/<deployment-name>
<BR>

    Response:

    <LoadBalancedEndpointList xmlns="http://schemas.microsoft.com/windowsazure" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <InputEndpoint>
    <LoadBalancedEndpointSetName>endpoint-set-name</LoadBalancedEndpointSetName>
    <LocalPort>local-port-number</LocalPort>
    <Port>external-port-number</Port>
    <LoadBalancerProbe>
    <Path>path-of-probe</Path>
    <Port>port-assigned-to-probe</Port>
    <Protocol>probe-protocol</Protocol>
    <IntervalInSeconds>interval-of-probe</IntervalInSeconds>
    <TimeoutInSeconds>timeout-for-probe</TimeoutInSeconds>
    </LoadBalancerProbe>
    <LoadBalancerName>name-of-internal-loadbalancer</LoadBalancerName>
    <Protocol>endpoint-protocol</Protocol>
    <IdleTimeoutInMinutes>15</IdleTimeoutInMinutes>
    <EnableDirectServerReturn>enable-direct-server-return</EnableDirectServerReturn>
    <EndpointACL>
    <Rules>
    <Rule>
    <Order>priority-of-the-rule</Order>
    <Action>permit-rule</Action>
    <RemoteSubnet>subnet-of-the-rule</RemoteSubnet>
    <Description>description-of-the-rule</Description>
    </Rule>
    </Rules>
    </EndpointACL>
    </InputEndpoint>
    </LoadBalancedEndpointList>

## Nächste Schritte
[Interner Lastenausgleich (Übersicht)](load-balancer-internal-overview.md)

[Erste Schritte zum Konfigurieren eines Lastenausgleichs für Internetverbindungen](load-balancer-get-started-internet-arm-ps.md)

[Konfigurieren eines Lastenausgleichs-Verteilungsmodus](load-balancer-distribution-mode.md)

<!---HONumber=AcomDC_0914_2016-->