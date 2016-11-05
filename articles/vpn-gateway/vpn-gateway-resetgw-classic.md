---
title: Zurücksetzen einer Azure VPN Gateway-Instanz | Microsoft Docs
description: In diesem Artikel sind die Schritte zum Zurücksetzen einer Azure VPN Gateway-Instanz beschrieben. Der Artikel bezieht sich auf VPN-Gateways im klassischen Bereitstellungsmodell und im Resource Manager-Bereitstellungsmodell.
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: carmonm
editor: ''
tags: azure-resource-manager,azure-service-management

ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: cherylmc

---
# <a name="reset-an-azure-vpn-gateway-using-powershell"></a>Zurücksetzen einer Azure VPN Gateway-Instanz mit PowerShell
In diesem Artikel werden die Schritte zum Zurücksetzen einer Azure VPN Gateway-Instanz mithilfe von PowerShell-Cmdlets beschrieben. Diese Anweisungen erstrecken sich sowohl auf das klassische Bereitstellungsmodell als auch auf das Resource Manager-Bereitstellungsmodell.

Das Zurücksetzen von Azure VPN Gateway-Instanzen ist nützlich, wenn die standortübergreifende VPN-Verbindung bei mindestens einem S2S-VPN-Tunnel unterbrochen ist. In diesem Fall funktionieren Ihre lokalen VPN-Geräte ordnungsgemäß, können jedoch keine IPsec-Tunnelverbindungen mit Azure VPN Gateway-Instanzen herstellen. 

Jede Azure VPN Gateway-Instanz umfasst zwei VM-Instanzen, die in einer Konfiguration mit aktivem Standbymodus ausgeführt werden. Bei Verwendung des PowerShell-Cmdlets zum Zurücksetzen des Gateways wird das Gateway neu gestartet, und die standortübergreifenden Konfigurationen werden erneut auf das Gateway angewendet. Die öffentliche IP-Adresse des Gateways bleibt unverändert. Die VPN-Routerkonfiguration muss also nicht mit einer neuen öffentlichen IP-Adresse für Azure VPN Gateway aktualisiert werden.  

Wenn der Befehl ausgeführt wird, wird die gegenwärtig aktive Instanz der Azure VPN Gateway-Instanz umgehend neu gestartet. Während des Failovers von der aktiven Instanz (die Instanz, die neu gestartet wird) auf die Standbyinstanz kommt es zu einer kurzen Unterbrechung. Diese Unterbrechung sollte weniger als 1 Minute dauern.

Wenn die Verbindung nach dem ersten Neustart nicht wiederhergestellt wird, führen Sie denselben Befehl erneut aus, um die zweite VM-Instanz (das neue aktive Gateway) neu zu starten. Wenn die beiden Neustarts nacheinander angefordert werden, dauert der Neustart der beiden VM-Instanzen (aktive Instanz und Standbyinstanz) etwas länger. Der Neustart der beiden VMs kann zwei bis vier Minuten dauern, sodass die VPN-Konnektivität etwas länger unterbrochen ist.

Wenn nach den beiden Neustarts weiterhin standortübergreifende Konnektivitätsprobleme auftreten, erstellen Sie über das Azure-Portal ein Supportticket.

## <a name="before-you-begin"></a>Voraussetzungen
Überprüfen Sie vor dem Zurücksetzen des Gateways die unten aufgeführten Elemente für die einzelnen IPsec-Site-to-Site-VPN-Tunnel (S2S). Treten bei diesen Elementen Konflikte auf, werden S2S-VPN-Tunnelverbindungen unterbrochen. Indem Sie die Konfigurationen für Ihre lokalen VPN-Gateways und Azure VPN Gateway-Instanzen überprüfen und korrigieren, werden nicht erforderliche Neustarts und Unterbrechungen anderer funktionierender Verbindungen der Gateways verhindert.

Überprüfen Sie die folgenden Elemente, bevor Sie Ihr Gateway zurücksetzen:

* Die Internet-IP-Adressen (VIPs) der Azure VPN Gateway-Instanz und des lokalen VPN-Gateways sind sowohl in Azure als auch in den lokalen VPN-Richtlinien ordnungsgemäß konfiguriert.
* Der vorinstallierte Schlüssel muss sowohl bei der Azure VPN Gateway-Instanz als auch auf dem lokalen VPN-Gateway identisch sein.
* Wenn Sie eine bestimmte IPsec/IKE-Konfiguration – z.B. Verschlüsselung, Hashalgorithmus oder PFS (Perfect Forward Secrecy) – anwenden, müssen Sie sicherstellen, dass die Azure VPN Gateway-Instanz und das lokale VPN-Gateway dieselbe Konfiguration aufweisen.

## <a name="reset-a-vpn-gateway-using-the-resource-management-deployment-model"></a>Zurücksetzen einer VPN Gateway-Instanz über das Resource Manager-Bereitstellungsmodell
Zum Zurücksetzen des Gateways wird das Resource Manager-PowerShell-Cmdlet `Reset-AzureRmVirtualNetworkGateway`verwendet. Im folgenden Beispiel wird das Azure VPN-Gateway „VNet1GW“ in der Ressourcengruppe „TestRG1“ zurückgesetzt.

    $gw = Get-AzureRmVirtualNetworkGateway -Name VNet1GW -ResourceGroup TestRG1
    Reset-AzureRmVirtualNetworkGateway -VirtualNetworkGateway $gw

## <a name="reset-a-vpn-gateway-using-the-classic-deployment-model"></a>Zurücksetzen einer VPN Gateway-Instanz über das klassische Bereitstellungsmodell
Zum Zurücksetzen von Azure VPN Gateway-Instanzen wird das PowerShell-Cmdlet `Reset-AzureVNetGateway`verwendet. Im folgenden Beispiel wird die Azure VPN Gateway-Instanz für das virtuelle Netzwerk „ContosoVNet“ zurückgesetzt.

    Reset-AzureVNetGateway –VnetName “ContosoVNet” 

Ergebnis:

    Error          :
    HttpStatusCode : OK
    Id             : f1600632-c819-4b2f-ac0e-f4126bec1ff8
    Status         : Successful
    RequestId      : 9ca273de2c4d01e986480ce1ffa4d6d9
    StatusCode     : OK


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in der [Referenz zu PowerShell-Dienstverwaltungs-Cmdlets](https://msdn.microsoft.com/library/azure/mt617104.aspx) und in der [Referenz zu Resource Manager-PowerShell-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=828732).

<!--HONumber=Oct16_HO2-->


