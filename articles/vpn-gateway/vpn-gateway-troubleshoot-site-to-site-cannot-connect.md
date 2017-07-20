---
title: 'Problembehandlung bei dem Problem: Azure-Site-to-Site-VPN-Verbindung kann nicht hergestellt werden | Microsoft-Dokumentation'
description: "Erfahren Sie, wie Sie das Problem beheben, bei dem die Site-to-Site-VPN-Verbindung plötzlich getrennt und nicht mehr hergestellt werden kann."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/21/2017
ms.author: genli
ms.translationtype: Human Translation
ms.sourcegitcommit: 61fd58063063d69e891d294e627ae40cb878d65b
ms.openlocfilehash: a72a46e4c2c93fdb3ab1a26c8854adb8cdc488a5
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="troubleshooting-azure-site-to-site-vpn-connection-cannot-connect-and-stops-working"></a>Problembehandlung: Azure-Site-to-Site-VPN-Verbindung kann keine Verbindung herstellen und reagiert nicht mehr

Sie konfigurieren die Site-to-Site-VPN-Verbindung zwischen dem lokalen Netzwerk und einem Microsoft Azure Virtual Network. Die VPN-Verbindung funktioniert plötzlich nicht mehr, und kann nicht wiederhergestellt werden. Dieser Artikel enthält Schritte, mit denen Sie das Problem beheben können. 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung

Um das Problem zu beheben, versuchen Sie zunächst [das virtuelle Netzwerkgateway von Azure zurückzusetzen](vpn-gateway-resetgw-classic.md) und den Tunnel des lokalen VPN-Geräts zurückzusetzen. Sollte das Problem weiterhin bestehen, gehen Sie folgendermaßen vor, um die Ursache des Problems zu identifizieren.

### <a name="prerequisite-step"></a>Voraussetzung

Überprüfen Sie, um welchen Typ des virtuellen Netzwerkgateways von Azure es sich handelt:

1. Navigieren Sie zum [Azure-Portal](https://portal.azure.com).
2. Überprüfen Sie auf der **Übersichtsseite** des virtuellen Netzwerkgateways die Typinformationen.
    
    ![Übersicht über das Gateway](media\vpn-gateway-troubleshoot-site-to-site-cannot-connect\gatewayoverview.png)

### <a name="step-1-check-whether-the-on-premises-vpn-device-is-validated"></a>Schritt 1: Überprüfen, ob das lokale VPN-Gerät validiert wurde

1. Überprüfen Sie, ob [das VPN-Gerät und die Version des Betriebssystems validiert wurden](vpn-gateway-about-vpn-devices.md#a-namedevicetableavalidated-vpn-devices-and-device-configuration-guides). Wenn es sich bei dem Gerät nicht um ein überprüftes VPN-Gerät handelt, müssen Sie sich möglicherweise beim Gerätehersteller erkundigen, ob ein Kompatibilitätsproblem vorliegt.
2. Stellen Sie sicher, dass das VPN-Gerät ordnungsgemäß konfiguriert ist. Weitere Informationen finden Sie unter [Bearbeiten der Gerätekonfigurationsvorlagen](/vpn-gateway-about-vpn-devices.md#editing).

### <a name="step-2-verify-the-shared-keypsk"></a>Schritt 2 Überprüfen des gemeinsam verwendeten Schlüssels (PSK)

Vergleichen Sie den **Gemeinsam verwendeten Schlüssel** des lokalen VPN-Geräts und des VPNs des virtuellen Netzwerks, um sicherzustellen, dass die Schlüssel übereinstimmen. 

Verwenden Sie zum Anzeigen der PSK für die Azure-VPN-Verbindung eine der folgenden Methoden:

**Azure-Portal**

1. Gehen Sie zum Gateway des virtuellen Netzwerks > Standort-zu-Standort-Verbindung, die Sie erstellt haben.
2. Klicken Sie im Abschnitt **Einstellungen** auf **Gemeinsam verwendeter Schlüssel**.
    
    ![Gemeinsam verwendeter Schlüssel](media/vpn-gateway-troubleshoot-site-to-site-cannot-connect/sharedkey.png)

**Azure PowerShell**

Für den Resource Manager-Modus

    Get-AzureRmVirtualNetworkGatewayConnectionSharedKey -Name <Connection name> -ResourceGroupName <Resource group name>

Für klassische virtuelle Computer

    Get-AzureVNetGatewayKey -VNetName -LocalNetworkSiteName

### <a name="step-3-verify-the-vpn-peer-ips"></a>Schritt 3 Überprüfen Sie die VPN-Peer-IP-Adressen

-   Die IP-Definition im Objekt **Lokales Netzwerkgateway** in Azure muss mit der IP-Adresse des lokalen Geräts übereinstimmen.
-   Die Azure-Gateway-IP-Definition, die auf dem lokalen Gerät festgelegt ist, muss der Azure-Gateway-IP-Adresse entsprechen.

### <a name="step-4-nsg-and-udr-on-gateway-subnet"></a>Schritt 4 NSG und UDR auf Gatewaysubnetz

Suchen und Entfernen Sie Benutzerdefiniertes Routing (UDR) oder Netzwerksicherheitsgruppen (NSG) auf dem Gatewaysubnetz, und testen Sie dann das Ergebnis. Wenn das Problem behoben wurde, überprüfen Sie die Einstellungen der angewendeten NSG oder UDR.

### <a name="step-5-check-on-premises-vpn-device-external-interface-address"></a>Schritt 5: Überprüfen der Adresse der externen Schnittstelle des lokalen VPN-Geräts

- Wenn die IP-Adresse des VPN-Geräts mit Internetzugriff in der Definition **Lokales Netzwerk** in Azure enthalten ist, kann es vereinzelt zur Trennung der Verbindung kommen.
- Die externe Schnittstelle des Geräts muss sich direkt im Internet befinden. Es darf sich keine Netzwerkadressübersetzung oder Firewall zwischen dem Internet und dem Gerät befinden.
-  Wenn Sie Firewall Clustering konfigurieren, um über eine virtuelle IP-Adresse zu verfügen, müssen Sie den Cluster unterbrechen und das VPN-Gerät direkt an eine öffentliche Schnittstelle anschließen, mit der das Gateway kompatibel ist.

### <a name="step-6-verify--subnets-match-exactly-azure-policy-based-gateways"></a>Schritt 6: Überprüfen, ob Subnetze genau übereinstimmen (Auf Azure-Richtlinien basierende Gateways)

-   Stellen Sie sicher, dass die Subnetze zwischen dem virtuellen Netzwerk in Azure und den lokalen Definitionen für das virtuelle Netzwerk von Azure genau übereinstimmen.
-   Stellen Sie sicher, dass die Subnetze zwischen dem **lokalen Netzwerkgateway** und den lokalen Definitionen für das lokale Netzwerk genau übereinstimmen.

### <a name="step-5-verify-azure-gateway-health-probe"></a>Schritt 5: Überprüfen des Azure-Gateway-Integritätstests

1. Navigieren Sie zu „https://&lt;YourVirtualNetworkGatewayIP&gt;:8081/healthprobe“.
2. Klicken Sie sich durch die Zertifikatswarnung.
3. Wenn Sie eine Antwort erhalten haben, wird das virtuelle Netzwerkgateway als fehlerfrei beurteilt. Wenn Sie keine Antwort erhalten, ist das Gateway möglicherweise nicht fehlerfrei, oder auf dem Gatewaysubnetz ist eine NSG vorhanden, die das Problem verursacht. Der folgende Text ist eine Beispielantwort:

    &lt;?xml version="1.0"?>  <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Primäre Instanz: GatewayTenantWorker_IN_1 GatewayTenantVersion: 14.7.24.6</Zeichenfolge&gt;

### <a name="step-7-check-whether-the-on-premises-vpn-device-has-perfect-forward-secrecy-pfs-feature-enabled"></a>Schritt 7: Überprüfen, ob beim lokalen VPN-Gerät die Funktion Perfect Forward Secrecy (PFS) aktiviert ist

Die Funktion Perfect Forward Secrecy kann die Probleme mit der Trennung der Verbindung verursachen. Wenn beim VPN-Gerät Perfect Forward Secrecy aktiviert ist, sollten Sie diese Funktion deaktivieren. Aktualisieren Sie dann die IPsec-Richtlinie des virtuellen Netzwerkgateways.

## <a name="next-steps"></a>Nächste Schritte

-   [Erstellen einer Site-to-Site-Verbindung im Azure-Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
-   [Konfigurieren einer IPsec/IKE-Richtlinie für Standort-zu-Standort-VPN-Verbindungen](vpn-gateway-ipsecikepolicy-rm-powershell.md)

