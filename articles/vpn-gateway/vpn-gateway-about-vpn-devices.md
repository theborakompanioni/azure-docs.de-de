---
title: "Informationen zu VPN-Geräten für Site-to-Site-VPN-Gatewayverbindungen in virtuellen Azure-Netzwerken | Microsoft Docs"
description: "In diesem Artikel werden VPN-Geräte und IPsec-Parameter für S2S-VPN-Gateway-Verbindungen beschrieben und Links zu Konfigurationsanleitungen und -beispielen angegeben."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: ba449333-2716-4b7f-9889-ecc521e4d616
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2016
ms.author: yushwang;cherylmc
translationtype: Human Translation
ms.sourcegitcommit: bbaf89bca07fd2d4c4a12403d2daa8323f4d7be5
ms.openlocfilehash: 12e7768665b8d360fe7241c9879bd1f7bfc63664


---
# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Informationen zu VPN-Geräten für VPN-Gatewayverbindungen zwischen Standorten
Ein VPN-Gerät ist erforderlich, um eine VPN-Verbindung zwischen Standorten (Site-to-Site, S2S) zu konfigurieren. Standort-zu-Standort-Verbindungen können zum Erstellen einer Hybridlösung verwendet werden. Sie können sie auch zum Herstellen einer sicheren Verbindung zwischen Ihrem lokalen Netzwerk und dem virtuellen Netzwerk verwenden. Dieser Artikel beschreibt kompatible VPN-Geräte und Konfigurationsparameter.

> [!NOTE]
> Beim Konfigurieren einer Standort-zu-Standort-Verbindung ist eine öffentliche IPv4-IP-Adresse für das VPN-Gerät erforderlich.                                                                                                                                                                               
>
>

Falls Ihr Gerät nicht in der Tabelle mit den [überprüften VPN-Geräten](#devicetable) enthalten ist, helfen Ihnen die Informationen im Abschnitt [Nicht überprüfte VPN-Geräte](#additionaldevices) dieses Artikels weiter. Es ist möglich, dass das Gerät dennoch mit Azure verwendet werden kann. Wenden Sie sich für Unterstützung im Hinblick auf das VPN-Gerät an den Gerätehersteller.

**Was Sie beim Anzeigen der Tabellen beachten sollten:**

* Die Terminologie für statisches und dynamisches Routing wurde geändert. Wahrscheinlich finden Sie beide Begriffe. Die Funktionalität hat sich nicht geändert, nur die Namen.
  * Statisches Routing = Richtlinienbasiert (PolicyBased)
  * Dynamisches Routing = Routenbasiert (RouteBased)
* Die Spezifikationen für Hochleistungs-VPN-Gateways und routenbasierte VPN-Gateways bleiben dieselben, sofern nicht anders angegeben. Beispielsweise sind die überprüften VPN-Geräte, die mit den routenbasierten VPN-Gateways kompatibel sind, auch mit dem Azure-Hochleistungs-VPN-Gateway kompatibel.

## <a name="a-namedevicetableavalidated-vpn-devices"></a><a name="devicetable"></a>Überprüfte VPN-Geräte
Wir haben in Zusammenarbeit mit Geräteherstellern eine Reihe von VPN-Standardgeräten getestet. Alle Geräte der in der folgenden Liste aufgeführten Gerätefamilien sollten mit Azure-VPN-Gateways kompatibel sein. Unter [Informationen zu VPN Gateway](vpn-gateway-about-vpngateways.md) können Sie den Gatewaytyp ermitteln, der zum Konfigurieren der gewünschten Lösung erstellt werden muss.

Hilfreiche Informationen zur Konfiguration des VPN-Geräts finden Sie unter den Links für die entsprechende Gerätefamilie. Wenden Sie sich für Unterstützung im Hinblick auf das VPN-Gerät an den Gerätehersteller.

| **Hersteller** | **Gerätefamilie** | **Betriebssystemversion (Min.)** | **PolicyBased** | **RouteBased** |
| --- | --- | --- | --- | --- |
| Allied Telesis |VPN-Router der AR-Serie |2.9.2 |In Kürze verfügbar |Nicht kompatibel |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall F-Serie |PolicyBased: 5.4.3<br>RouteBased: 6.2.0 |[Konfigurationsanweisungen](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Konfigurationsanweisungen](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall X-Serie |Barracuda Firewall 6.5 |[Barracuda Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Nicht kompatibel |
| Brocade |5400 Vyatta vRouter |Virtual Router 6.6R3 GA |[Konfigurationsanweisungen](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |Nicht kompatibel |
| Check Point |Sicherheitsgateway |R75.40<br>R75.40VS |[Konfigurationsanweisungen](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Konfigurationsanweisungen](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco |ASA |8.3 |[Cisco-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |Nicht kompatibel |
| Cisco |ASR |PolicyBased: IOS 15.1<br>RouteBased: IOS 15.2 |[Cisco-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[Cisco-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco |ISR |PolicyBased: IOS 15.0<br>RouteBased*: IOS 15.1 |[Cisco-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[Cisco-Beispiele*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix |NetScaler MPX, SDX, VPX |ab&10;.1 |[Integrationsanweisungen](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Nicht kompatibel |
| Dell SonicWALL |TZ-Serie, NSA-Serie<br>SuperMassive-Serie<br>E-Class-NSA-Serie |SonicOS 5.8.x<br>[SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850)<br>[SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646) |[Konfigurationshandbuch für SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Konfigurationshandbuch für SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |[Konfigurationshandbuch für SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Konfigurationshandbuch für SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| F5 |BIG-IP-Serie |12.0 |[Konfigurationsanweisungen](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Konfigurationsanweisungen](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.4.x |[Konfigurationsanweisungen](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |[Konfigurationsanweisungen](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |
| Internet Initiative Japan (IIJ) |SEIL-Serie |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Konfigurationsanweisungen](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Nicht kompatibel |
| Juniper |SRX |PolicyBased: JunOS 10.2<br>Routebased: JunOS 11.4 |[Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |J-Serie |PolicyBased: JunOS 10.4r9<br>RouteBased: JunOS 11.4 |[Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 |[Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 |[Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[Juniper-Beispiele](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |Routing- und RAS-Dienst |Windows Server 2012 |Nicht kompatibel |[Microsoft-Beispiele](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| Open Systems AG |Mission Control Security Gateway |N/V |[Installationshandbuch](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |[Installationshandbuch](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan |Openswan |2.6.32 |(In Kürze verfügbar) |Nicht kompatibel |
| Palo Alto Networks |Alle Geräte mit PAN-OS&5;.0 |PAN-OS<br>PolicyBased: 6.1.5 oder höher<br>RouteBased: 7.0.5 oder höher |[Konfigurationsanweisungen](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Konfigurationsanweisungen](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| WatchGuard |Alle |Fireware XTM<br> PolicyBased: v11.11.x<br>RouteBased: v11.12.x |[Konfigurationsanweisungen](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Konfigurationsanweisungen](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|

(*) Router der Serie ISR 7200 unterstützen nur richtlinienbasierte VPNs.

## <a name="a-nameadditionaldevicesanon-validated-vpn-devices"></a><a name="additionaldevices"></a>Nicht überprüfte VPN-Geräte
Auch wenn Ihr Gerät in der Tabelle mit den überprüften VPN-Geräten nicht aufgeführt wird, kann es unter Umständen trotzdem für eine Standort-zu-Standort-Verbindung eingesetzt werden. Stellen Sie sicher, dass Ihr VPN-Gerät die Mindestanforderungen erfüllt, die im Abschnitt mit den Gatewayanforderungen im Artikel [Informationen zu VPN-Gateways](vpn-gateway-about-vpngateways.md) aufgeführt sind. Geräte, die die Mindestanforderungen erfüllen, sollten problemlos mit VPN-Gateways verwendet werden können. Zusätzliche Unterstützung und Konfigurationsanweisungen erhalten Sie vom Gerätehersteller.

## <a name="editing-device-configuration-samples"></a>Bearbeiten der Gerätekonfigurationsvorlagen
Nachdem Sie die bereitgestellte Konfigurationsvorlage für das VPN-Gerät heruntergeladen haben, müssen Sie einige der Werte entsprechend den Einstellungen Ihrer Umgebung ersetzen.

**So bearbeiten Sie eine Vorlage:**

1. Öffnen Sie die Vorlage im Editor.
2. Suchen und ersetzen Sie alle <*text*>-Zeichenfolgen mit den Werten, die für Ihre Umgebung gelten. Schließen Sie dabei unbedingt „<“ und „>“ mit ein. Wenn ein Name angegeben ist, sollte der ausgewählte Name eindeutig sein. Sehen Sie zuerst in der Dokumentation des Geräteherstellers nach, falls ein Befehl nicht funktioniert.

| **Vorlagentext** | **Ändern in** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |Der von Ihnen gewählte Name für dieses Objekt. Beispiel: meinLokalesNetzwerk |
| &lt;RP_AzureNetwork&gt; |Der von Ihnen gewählte Name für dieses Objekt. Beispiel: meinAzureNetzwerk |
| &lt;RP_AccessList&gt; |Der von Ihnen gewählte Name für dieses Objekt. Beispiel: meineAzureZugriffsliste |
| &lt;RP_IPSecTransformSet&gt; |Der von Ihnen gewählte Name für dieses Objekt. Beispiel: meinIPSecTransformSet |
| &lt;RP_IPSecCryptoMap&gt; |Der von Ihnen gewählte Name für dieses Objekt. Beispiel: meineIPSecKryptoMap |
| &lt;SP_AzureNetworkIpRange&gt; |Geben Sie den Bereich an. Beispiel: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Geben Sie die Subnetzmaske an. Beispiel: 255.255.0.0 |
| &lt;SP_OnPremisesNetworkIpRange&gt; |Geben Sie den lokalen Bereich an. Beispiel: 10.2.1.0 |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; |Geben Sie die lokale Subnetzmaske an. Beispiel: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |Diese Informationen gelten nur für Ihr virtuelles Netzwerk und befinden sich im Verwaltungsportal unter **Gateway-IP-Adresse**. |
| &lt;SP_PresharedKey&gt; |Diese Informationen gelten nur für Ihr virtuelles Netzwerk und befinden sich im Verwaltungsportal unter "Schlüssel verwalten". |

## <a name="ipsec-parameters"></a>IPsec-Parameter
> [!NOTE]
> Die in der folgenden Tabelle aufgeführten Werte werden zwar vom Azure-VPN-Gateway unterstützt, derzeit ist es jedoch nicht möglich, über das Azure-VPN-Gateway eine bestimmte Kombination festzulegen oder auszuwählen. Alle Einschränkungen müssen Sie vom lokalen VPN-Gerät aus angeben. Darüber hinaus müssen Sie MSS mit 1350 verknüpfen.
>
>

### <a name="ike-phase-1-setup"></a>IKE Phase 1-Einrichtung
| **Eigenschaft** | **PolicyBased** | **Routenbasiertes und Standard- oder Hochleistungs-VPN-Gateway** |
| --- | --- | --- |
| IKE-Version |IKEv1 |IKEv2 |
| Diffie-Hellman-Gruppe |Gruppe 2 (1024 Bit) |Gruppe 2 (1024 Bit) |
| Authentifizierungsmethode |Vorab ausgetauschter Schlüssel |Vorab ausgetauschter Schlüssel |
| Verschlüsselungsalgorithmen |AES256 AES128 3DES |AES256 3DES |
| Hashalgorithmus |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Phase 1 Sicherheitszuordnung (SA) Lebensdauer (Zeit) |28.800 Sekunden |10.800 Sekunden |

### <a name="ike-phase-2-setup"></a>IKE Phase 2-Einrichtung
| **Eigenschaft** | **PolicyBased** | **Routenbasiertes und Standard- oder Hochleistungs-VPN-Gateway** |
| --- | --- | --- |
| IKE-Version |IKEv1 |IKEv2 |
| Hashalgorithmus |SHA1(SHA128) |SHA1(SHA128) |
| Phase 2 Sicherheitszuordnung (SA) Lebensdauer (Zeit) |3.600 Sekunden |3.600 Sekunden |
| Phase 2 Sicherheitszuordnung (SA) Lebensdauer (Durchsatz) |102.400.000 KB |- |
| IPsec-SA-Verschlüsselungs- und Authentifizierungsangebote (sortiert nach Präferenz) |1. ESP-AES256 2. ESP-AES128 3. ESP-3DES 4. N/V |Siehe unten: *IPsec-Sicherheitszuordnungsangebote (SA) für routenbasierte Gateways* |
| Perfect Forward Secrecy (PFS) |Nein |Nein (*) |
| Dead Peer Detection |Nicht unterstützt |Unterstützt |

(*) Azure Gateway als IKE-Antwortender kann PFS DH-Gruppe 1, 2, 5, 14, 24 akzeptieren.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>IPsec-Sicherheitszuordnungsangebote (SA) für routenbasierte Gateways
Die folgende Tabelle enthält die IPsec-SA-Verschlüsselungs- und -Authentifizierungsangebote. Angebote werden in der Reihenfolge ihrer Priorität aufgeführt, in der das Angebot dargeboten oder akzeptiert wurde.

| **IPsec-SA-Verschlüsselungs- und -Authentifizierungsangebote** | **Azure-Gateway als Initiator** | **Azure-Gateway als Antwortender** |
| --- | --- | --- |
| 1 |ESP AES_256 SHA |ESP AES_128 SHA |
| 2 |ESP AES_128 SHA |ESP 3_DES MD5 |
| 3 |ESP 3_DES MD5 |ESP 3_DES SHA |
| 4 |ESP 3_DES SHA |AH SHA1 mit ESP AES_128 mit Null HMAC |
| 5 |AH SHA1 mit ESP AES_256 mit Null HMAC |AH SHA1 mit ESP 3_DES mit Null HMAC |
| 6 |AH SHA1 mit ESP AES_128 mit Null HMAC |AH MD5 mit ESP 3_DES mit Null HMAC, keine vorgesehene Lebensdauer |
| 7 |AH SHA1 mit ESP 3_DES mit Null HMAC |AH SHA1 mit ESP 3_DES SHA1, keine Lebensdauer |
| 8 |AH MD5 mit ESP 3_DES mit Null HMAC, keine vorgesehene Lebensdauer |AH MD5 mit ESP 3_DES MD5, keine Lebensdauer |
| 9 |AH SHA1 mit ESP 3_DES SHA1, keine Lebensdauer |ESP DES MD5 |
| 10 |AH MD5 mit ESP 3_DES MD5, keine Lebensdauer |ESP DES SHA1, keine Lebensdauer |
| 11 |ESP DES MD5 |AH SHA1 mit ESP DES Null HMAC, keine vorgesehene Lebensdauer |
| 12 |ESP DES SHA1, keine Lebensdauer |AH MD5 mit ESP DES Null HMAC, keine vorgesehene Lebensdauer |
| 13 |AH SHA1 mit ESP DES Null HMAC, keine vorgesehene Lebensdauer |AH SHA1 mit ESP DES SHA1, keine Lebensdauer |
| 14 |AH MD5 mit ESP DES Null HMAC, keine vorgesehene Lebensdauer |AH MD5 mit ESP DES MD5, keine Lebensdauer |
| 15 |AH SHA1 mit ESP DES SHA1, keine Lebensdauer |ESP SHA, keine Lebensdauer |
| 16 |AH MD5 mit ESP DES MD5, keine Lebensdauer |ESP MD5, keine Lebensdauer |
| 17 |- |AH SHA, keine Lebensdauer |
| 18 |- |AH MD5, keine Lebensdauer |

* Sie können IPsec-ESP-NULL-Verschlüsselung mit routenbasierten und High-Performance-VPN-Gateways angeben. Verschlüsselung auf Basis von NULL bietet keinen Schutz der Daten während der Übertragung und sollte nur verwendet werden, wenn maximaler Durchsatz und minimale Latenz erforderlich sind.  Clients können diese in Szenarien mit VNet-zu-VNet-Kommunikation oder bei Anwendung der Verschlüsselung an anderer Stelle in der Lösung verwenden.
* Verwenden Sie für standortübergreifende Konnektivität über das Internet die Standardeinstellungen für Azure-VPN-Gateways mit Verschlüsselung und Hashalgorithmen, die in der Tabelle oben aufgelistet werden, um die Sicherheit Ihrer kritischen Kommunikation zu gewährleisten.



<!--HONumber=Dec16_HO2-->


