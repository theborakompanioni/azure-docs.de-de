---
title: "Beispielkonfiguration – Cisco ASA-Gerät, das sich mit Azure-VPN-Gateways verbinden lässt | Microsoft-Dokumentation"
description: "Dieser Artikel enthält eine Beispielkonfiguration für ein Cisco ASA-Gerät, das sich mit Azure-VPN-Gateways verbinden lässt."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: a8bfc955-de49-4172-95ac-5257e262d7ea
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: yushwang
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: 10466b8928e2cd687f7961a2956b6d60823b82be
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="sample-configuration-cisco-asa-device-ikev2no-bgp"></a>Beispielkonfiguration: Cisco ASA-Gerät (IKEv2/no BGP)
Dieser Artikel enthält eine Beispielkonfigurationen für Cisco ASA-Geräte, die sich mit Azure VPN-Gateways verbinden lassen.

## <a name="device-at-a-glance"></a>Gerät auf einen Blick

|                        |                                   |
| ---                    | ---                               |
| Gerätehersteller          | Cisco                             |
| Gerätemodell           | ASA (Adaptive Security Appliance) |
| Zielversion         | 8.4+                              |
| Getestetes Modell           | ASA 5505                          |
| Getestete Version         | 9.2                               |
| IKE-Version            | **IKEv2**                         |
| BGP                    | **Nein**                            |
| Typ des Azure VPN-Gateways | **Routenbasiertes** VPN-Gateway       |
|                        |                                   |

> [!NOTE]
> 1. Mit der folgenden Konfiguration wird eine Verbindung zwischen einem Cisco ASA-Gerät und einem **routenbasierten** Azure-VPN-Gateway hergestellt. Dabei wird die benutzerdefinierte IPsex/IKE-Richtlinie mit der Option „UserPolicyBasedTrafficSelectors“ verwendet, wie in [diesem Artikel](vpn-gateway-connect-multiple-policybased-rm-ps.md) beschrieben.
> 2. Die ASA-Geräte müssen hierfür **IKEv2** mit zugriffslistenbasierten und nicht mit VTI-basierten Konfigurationen verwenden.
> 3. Sehen Sie in den Spezifikationen Ihres VPN-Geräteanbieters nach, um sicherzustellen, dass die Richtlinie auf Ihren lokalen VPN-Geräten unterstützt wird.

## <a name="vpn-device-requirements"></a>Anforderungen an das VPN-Gerät
Azure-VPN-Gateways verwenden für den Aufbau von Standort-zu-Standort-VPN-Tunneln Standard-IPsec/IKE-Protokollsammlungen. Die ausführlichen PSsec-/IKE-Protokollparameter und die standardmäßigen Kryptoalgorithemen für Azure-VPN-Gateways finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). Sie können die genaue Kombination der Kryptoalgorithmen und Schlüsselstärken für eine spezifische Verbindung angeben. Dies ist optional und wird unter [Informationen zu kryptografischen Anforderungen](vpn-gateway-about-compliance-crypto.md) beschrieben. Wenn Sie eine bestimmte Kombination aus Kryptoalgorithmen und Schlüsselstärken wählen, stellen Sie bitte sicher, dass Sie die entsprechenden Spezifikationen auf Ihren VPN-Geräten verwenden.

## <a name="single-vpn-tunnel"></a>Einmaliger VPN-Tunnel
Diese Topologie besteht aus einem einmaligen Standort-zu-Standort-VPN-Tunnel zwischen dem Azure-VPN-Gateway und Ihrem lokalen VPN-Gerät. Als Option können Sie im VPN-Tunnel BGP konfigurieren.

![Einmaliger Tunnel](./media/vpn-gateway-3rdparty-device-config-cisco-asa/singletunnel.png)

Weitere Informationen und eine ausführliche Anleitung für den Aufbau der Azure-Konfigurationen finden Sie in [Einrichtung eines einmaligen Tunnels](vpn-gateway-3rdparty-device-config-overview.md#singletunnel).

### <a name="network-and-vpn-gateway-information"></a>Netzwerk- und VPN-Gatewayinformationen
In diesem Abschnitt sind die Parameter für dieses Beispiel aufgeführt.

| **Parameter**                | **Wert**                    |
| ---                          | ---                          |
| VNET-Adresspräfixe        | 10.11.0.0/16<br>10.12.0.0/16 |
| IP-Adresse des Azure-VPN-Gateways         | Azure_Gateway_Public_IP      |
| Lokale Adresspräfixe | 10.51.0.0/16<br>10.52.0.0/16 |
| IP-Adresse des lokalen VPN-Geräts    | OnPrem_Device_Public_IP     |
| *VNET-BGP-ASN                | 65010                        |
| *IP-Adresse des Azure-BGP-Peers           | 10.12.255.30                 |
| *Lokale BGP-ASN         | 65050                        |
| *IP-Adresse des lokalen BGP-Peers     | 10.52.255.254                |
|                              |                              |

* (*) Optionale Parameter nur für BGP.

### <a name="ipsecike-policy--parameters"></a>IPsec/IKE-Richtlinie und -Parameter

In der folgenden Tabelle werden die im Beispiel verwendeten IPSec/IKE-Algorithmen und -Parameter aufgeführt. Bitte stellen Sie anhand Ihrer VPN-Gerätespezifikationen sicher, dass alle oben aufgelisteten Algorithmen von Ihren VPN-Gerätemodellen und Firmwareversionen unterstützt werden.

| **IPsec/IKEv2**  | **Wert**                            |
| ---              | ---                                  |
| IKEv2-Verschlüsselung | AES256                               |
| IKEv2-Integrität  | SHA384                               |
| DH-Gruppe         | DHGroup24                            |
| IPsec-Verschlüsselung | AES256                               |
| IPsec-Integrität  | SHA1                                 |
| PFS-Gruppe        | PFS24                                |
| QM-SA-Gültigkeitsdauer   | 7.200 Sekunden                         |
| Datenverkehrsselektor | UsePolicyBasedTrafficSelectors $True |
| Vorab ausgetauschter Schlüssel   | PreSharedKey                         |
|                  |                                      |

- (*) Auf manchen Geräten muss die IPsec-Integrität „NULL“ sein, wenn GCM-AES als IPsec-Verschlüsselungsalgorithmus verwendet wird.

### <a name="device-notes"></a>Gerätehinweise

>[!NOTE]
>
> 1. IKEv2-Unterstützung setzt mindestens die ASA-Version 8.4 voraus.
> 2. Eine höhere DH- und PFS-Gruppenunterstützung (höher als Gruppe 5) setzt mindestens ASA-Version 9.x voraus.
> 3. IPSec-Verschlüsselung mit AES-GCM und IPsec-Integrität mit SHA-256-, SHA-384- und SHA-512-Unterstützung erfordert ASA-Version 9.x auf neuere ASA-Hardware; ASA 5505, 5510, 5520, 5540, 5550, 5580 werden **nicht** unterstützt. (Bitte überprüfen Sie zur Bestätigung die Händlerangaben.)
>


### <a name="sample-device-configurations"></a>Beispielgerätekonfigurationen
Das folgende Skript enthält eine Beispielkonfiguration auf Basis der oben genannten Topologie und Parameter. Die Standort-zu-Standort-VPN-Tunnelkonfiguration besteht aus folgenden Teilen:

1. Schnittstellen & Routen
2. Zugriffslisten
3. IKE-Richtlinie und -Parameter (Phase 1 oder Hauptmodus)
4. IPsec-Richtlinien und -Parameter (Phase 2 oder Quickmodus)
5. Sonstige Parameter (TCP-MSS-Clamping etc.)

>[!IMPORTANT] 
>Bitte stellen Sie sicher, dass sie die nachfolgend genannte Zusatzkonfiguration abschließen, und ersetzen Sie die Platzhalter durch die tatsächlichen Werte:
> 
> - Schnittstellenkonfiguration für innerhalb und außerhalb von Schnittstellen
> - Routen für Ihre internen/privaten und externen/öffentlichen Netzwerke
> - Stellen Sie sicher, dass alle Namen und Richtliniennummern auf dem Gerät eindeutig sind
> - Stellen Sie sicher, dass die Kryptoalgorithmen auf Ihrem Gerät unterstützt werden
> - Ersetzen Sie die folgenden Platzhalter durch die tatsächlichen Werte
>   - Name außerhalb der Schnittstelle: „outside“
>   - Azure_Gateway_Public_IP
>   - OnPrem_Device_Public_IP
>   - IKE Pre_Shared_Key
>   - VNET- und lokale Netzwerkgatewaynamen (VNetName, LNGName)
>   - VNET- und lokale Netzwerkadresspräfixe
>   - Ordnungsgemäße Netzmasken

#### <a name="sample-configuration"></a>Beispielkonfiguration

```
! Sample ASA configuration for connecting to Azure VPN gateway
!
! Tested hardware: ASA 5505
! Tested version:  ASA version 9.2(4)
!
! Replace the following place holders with your actual values:
!   - Interface names - default are "outside" and "inside"
!   - <Azure_Gateway_Public_IP>
!   - <OnPrem_Device_Public_IP>
!   - <Pre_Shared_Key>
!   - <VNetName>*
!   - <LNGName>* ==> LocalNetworkGateway - the Azure resource that represents the
!     on-premises network, specifies network prefixes, device public IP, BGP info, etc.
!   - <PrivateIPAddress> ==> Replace it with a private IP address if applicable
!   - <Netmask> ==> Replace it with appropriate netmasks
!   - <Nexthop> ==> Replace it with the actual nexthop IP address
!
! (*) Must be unique names in the device configuration
!
! ==> Interface & route configurations
!
!     > <OnPrem_Device_Public_IP> address on the outside interface or vlan
!     > <PrivateIPAddress> on the inside interface or vlan; e.g., 10.51.0.1/24
!     > Route to connect to <Azure_Gateway_Public_IP> address
!
!     > Example:
!
!       interface Ethernet0/0
!        switchport access vlan 2
!       exit
!
!       interface vlan 1
!        nameif inside
!        security-level 100
!        ip address <PrivateIPAddress> <Netmask>
!       exit
!
!       interface vlan 2
!        nameif outside
!        security-level 0
!        ip address <OnPrem_Device_Public_IP> <Netmask>
!       exit
!
!       route outside 0.0.0.0 0.0.0.0 <NextHop IP> 1
!
! ==> Access lists
!
!     > Most firewall devices deny all traffic by default. Create access lists to
!       (1) Allow S2S VPN tunnels between the ASA and the Azure gateway public IP address
!       (2) Construct traffic selectors as part of IPsec policy or proposal
!
access-list outside_access_in extended permit ip host <Azure_Gateway_Public_IP> host <OnPrem_Device_Public_IP>
!
!     > Object group that consists of all VNet prefixes (e.g., 10.11.0.0/16 &
!       10.12.0.0/16)
!
object-group network Azure-<VNetName>
 description Azure virtual network <VNetName> prefixes
 network-object 10.11.0.0 255.255.0.0
 network-object 10.12.0.0 255.255.0.0
exit
!
!     > Object group that corresponding to the <LNGName> prefixes.
!       E.g., 10.51.0.0/16 and 10.52.0.0/16. Note that LNG = "local network gateway".
!       In Azure network resource, a local network gateway defines the on-premises
!       network properties (address prefixes, VPN device IP, BGP ASN, etc.)
!
object-group network <LNGName>
 description On-Premises network <LNGName> prefixes
 network-object 10.51.0.0 255.255.0.0
 network-object 10.52.0.0 255.255.0.0
exit
!
!     > Specify the access-list between the Azure VNet and your on-premises network.
!       This access list defines the IPsec SA traffic selectors.
!
access-list Azure-<VNetName>-acl extended permit ip object-group <LNGName> object-group Azure-<VNetName>
!
!     > No NAT required between the on-premises network and Azure VNet
!
nat (inside,outside) source static <LNGName> <LNGName> destination static Azure-<VNetName> Azure-<VNetName>
!
! ==> IKEv2 configuration
!
!     > General IKEv2 configuration - enable IKEv2 for VPN
!
group-policy DfltGrpPolicy attributes
 vpn-tunnel-protocol ikev1 ikev2
exit
!
crypto isakmp identity address
crypto ikev2 enable outside
!
!     > Define IKEv2 Phase 1/Main Mode policy
!       - Make sure the policy number is not used
!       - integrity and prf must be the same
!       - DH group 14 and above require ASA version 9.x.
!
crypto ikev2 policy 1
 encryption       aes-256
 integrity        sha384
 prf              sha384
 group            24
 lifetime seconds 86400
exit
!
!     > Set connection type and pre-shared key
!
tunnel-group <Azure_Gateway_Public_IP> type ipsec-l2l
tunnel-group <Azure_Gateway_Public_IP> ipsec-attributes
 ikev2 remote-authentication pre-shared-key <Pre_Shared_Key> 
 ikev2 local-authentication  pre-shared-key <Pre_Shared_Key> 
exit
!
! ==> IPsec configuration
!
!     > IKEv2 Phase 2/Quick Mode proposal
!       - AES-GCM and SHA-2 requires ASA version 9.x on newer ASA models. ASA
!         5505, 5510, 5520, 5540, 5550, 5580 are not supported.
!       - ESP integrity must be null if AES-GCM is configured as ESP encryption
!
crypto ipsec ikev2 ipsec-proposal AES-256
 protocol esp encryption aes-256
 protocol esp integrity  sha-1
exit
!
!     > Set access list & traffic selectors, PFS, IPsec protposal, SA lifetime
!       - This sample uses "Azure-<VNetName>-map" as the crypto map name
!       - ASA supports only one crypto map per interface, if you already have
!         an existing crypto map assigned to your outside interface, you must use
!         the same crypto map name, but with a different sequence number for
!         this policy
!       - "match address" policy uses the access-list "Azure-<VNetName>-acl" defined 
!         previously
!       - "ipsec-proposal" uses the proposal "AES-256" defined previously 
!       - PFS groups 14 and beyond requires ASA version 9.x.
!
crypto map Azure-<VNetName>-map 1 match address Azure-<VNetName>-acl
crypto map Azure-<VNetName>-map 1 set pfs group24
crypto map Azure-<VNetName>-map 1 set peer <Azure_Gateway_Public_IP>
crypto map Azure-<VNetName>-map 1 set ikev2 ipsec-proposal AES-256
crypto map Azure-<VNetName>-map 1 set security-association lifetime seconds 7200
crypto map Azure-<VNetName>-map interface outside
!
! ==> Set TCP MSS to 1350
!
sysopt connection tcpmss 1350
!
```

## <a name="simple-debugging-commands"></a>Einfache Befehle zum Debuggen

Hier einige ASA-Befehle zum Debuggen:

1. Anzeigen der IPSec- und IKE-SAs
    - „show crypto ipsec sa“
    - „show crypto ikev2 sa“
2. Starten des Debugmodus - dies kann starkes Rauschen auf der Konsole erzeugen
    - „debug crypto ikev2 platform <level>“
    - „debug crypto ikev2 protocol <level>“
3. Anzeigen der aktuellen Konfigurationen
    - Mit „show run“ werden die aktuellen Konfigurationen auf dem Gerät angezeigt; Sie können die verschiedenen Unterbefehle dazu verwenden, spezifische Konfigurationsbereiche aufzulisten. Z.B. „show run crypto“, „show run access-list“, „show run tunnel-group“ etc.


## <a name="next-steps"></a>Nächste Schritte
Die Schritte zum Konfigurieren von standortübergreifenden Verbindungen und VNet-zu-VNet-Verbindungen vom Typ „Aktiv/Aktiv“ finden Sie unter [Konfigurieren von Aktiv/Aktiv-VPN-Gateways für standortübergreifende und VNet-zu-VNet-Verbindungen](vpn-gateway-activeactive-rm-powershell.md).


