---
title: "Planen des Netzwerks für die Replikation von Azure-VMs in Azure mit Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel wird die Planung des Netzwerks beim Replizieren von Azure-VMs mit Azure Site Recovery erläutert."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/01/2017
ms.author: sujayt
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: 7b37e853f6b97ba313111f9201f877846a28fae9
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---
# <a name="step-3-plan-networking-for-azure-vm-replication"></a>Schritt 3: Planen des Netzwerks für die Replikation von Azure-VMs

Nach dem Überprüfen der [Bereitstellungsvoraussetzungen](azure-to-azure-walkthrough-prerequisites.md) lesen Sie diesen Artikel, um die Netzwerkaspekte kennen zu lernen, die beim Replizieren und Wiederherstellen von virtuellen Azure-Computern (VMs) zwischen Azure-Regionen mit Azure Site Recovery eine Rolle spielen. 

- Wenn Sie den Artikel gelesen haben, wissen Sie, wie Sie den ausgehenden Zugriff für virtuelle Azure-Computer einrichten, die Sie replizieren möchten, und wie Sie von Ihrem lokalen Standort aus eine Verbindung mit diesen virtuellen Computern herstellen.
- Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.

>[!NOTE]
> Die Replikation von Azure-VMs mit Site Recovery befindet sich derzeit in der Vorschau.



## <a name="network-overview"></a>Netzwerküberblick

Üblicherweise befinden sich all Ihre Azure-VMs in einem virtuellen Azure-Netzwerk bzw. Subnetz, und es besteht eine Azure ExpressRoute- oder VPN-Verbindung zwischen Ihrem lokalen Standort und Azure.

Netzwerke werden in der Regel durch Firewalls und/oder Netzwerksicherheitsgruppen (NSGs) geschützt. Firewalls können zum Steuern der Netzwerkkonnektivität URL- oder IP-basierte Listen verwenden. Netzwerksicherheitsgruppen verwenden auf IP-Adressbereichen basierende Regeln. 


Damit Site Recovery erwartungsgemäß funktioniert, müssen Sie für die VMs, die Sie replizieren möchten, einige Änderungen an der ausgehenden Netzwerkkonnektivität vornehmen. Site Recovery unterstützt die Verwendung eines Authentifizierungsproxys zur Steuerung der Netzwerkkonnektivität nicht. Wenn Sie einen Authentifizierungsproxy verwenden, kann die Replikation nicht aktiviert werden. 


Die folgende Abbildung zeigt eine typische Umgebung für eine Anwendung, die auf virtuellen Azure-Computern ausgeführt wird.

![Kundenumgebung](./media/azure-to-azure-walkthrough-network/source-environment.png)

**Azure-VM-Umgebung**

Sie haben möglicherweise auch eine Azure ExpressRoute- oder VPN-Verbindung zwischen Ihrem lokalen Standort und Azure eingerichtet. 

![Kundenumgebung](./media/azure-to-azure-walkthrough-network/source-environment-expressroute.png)

**Lokale Verbindung mit Azure**



## <a name="outbound-connectivity-for-urls"></a>Ausgehende Konnektivität für URLs

Wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Konnektivität verwenden, stellen Sie sicher, dass Sie alle URLs zulassen, die von Site Recovery verwendet werden.

**URL** | **Details**  
--- | ---
*.blob.core.windows.net | Ermöglicht das Schreiben von Daten aus der VM in das Cachespeicherkonto in der Quellregion.
login.microsoftonline.com | Stellt die Autorisierung und Authentifizierung für Site Recovery-Dienst-URLs bereit.
*.hypervrecoverymanager.windowsazure.com | Ermöglicht die Kommunikation aus der VM mit dem Site Recovery-Dienst.
*.servicebus.windows.net | Erforderlich, damit die Site Recovery-Überwachungs- und -Diagnosedaten über die VM geschrieben werden können.

## <a name="outbound-connectivity-for-ip-address-ranges"></a>Ausgehende Konnektivität für IP-Adressbereiche

- Sie können die erforderlichen Regeln in der Netzwerksicherheitsgruppe automatisch erstellen, indem Sie [dieses Skript](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702) herunterladen und ausführen.
- Es wird empfohlen, die erforderlichen Regeln für die Netzwerksicherheitsgruppe in einer Test-Netzwerksicherheitsgruppe zu erstellen und sicherzustellen, dass keine Probleme vorliegen, bevor Sie die Regeln in einer produktiven Netzwerksicherheitsgruppe erstellen.
- Um die erforderliche Anzahl von NSG-Regeln zu erstellen, stellen Sie sicher, dass Ihr Abonnement in der Whitelist enthalten ist. Wenden Sie sich an den Support, um den Grenzwert der NSG-Regeln in Ihrem Abonnement zu erhöhen.
Wenn Sie IP-basierte Firewallproxys oder NSG-Regeln zur Steuerung ausgehender Verbindungen verwenden, müssen abhängig von den Quell- und Zielstandorten der virtuellen Computer folgende IP-Adressbereiche zur Whitelist hinzugefügt werden:

    - Alle IP-Adressbereiche, die dem Quellstandort entsprechen. Laden Sie die [Bereiche](https://www.microsoft.com/download/confirmation.aspx?id=41653) herunter. Whitelists sind erforderlich, damit Daten aus der VM in das Cachespeicherkonto geschrieben werden können.
    - Alle IP-Adressbereiche, die den [IPv4-Endpunkten zur Authentifizierung und Identifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) bei Office 365 entsprechen. Wenn neue IP-Adressen zu den Office 365-IP-Adressbereichen hinzugefügt werden, müssen Sie neue Regeln für die Netzwerksicherheitsgruppe erstellen.
-     IP-Adressen von Site Recovery-Dienstendpunkten ([in einer XML-Datei verfügbar](https://aka.ms/site-recovery-public-ips)), die von Ihrem Zielstandort abhängig sind: 

   **Zielstandort** | **IP-Adressen des Site Recovery-Diensts** |  **IP-Adressen zur Site Recovery-Überwachung**
   --- | --- | ---
   Ostasien | 52.175.17.132</br>40.83.121.61 | 13.94.47.61
   Südostasien | 52.187.58.193</br>52.187.169.104 | 13.76.179.223
   Indien (Mitte) | 52.172.187.37</br>52.172.157.193 | 104.211.98.185
   Indien, Süden | 52.172.46.220</br>52.172.13.124 | 104.211.224.190
   USA (Mitte/Norden) | 23.96.195.247</br>23.96.217.22 | 168.62.249.226
   Nordeuropa | 40.69.212.238</br>13.74.36.46 | 52.169.18.8
   Westeuropa | 52.166.13.64</br>52.166.6.245 | 40.68.93.145
   USA (Ost) | 13.82.88.226</br>40.71.38.173 | 104.45.147.24
   USA (West) | 40.83.179.48</br>13.91.45.163 | 104.40.26.199
   USA (Mitte/Süden) | 13.84.148.14</br>13.84.172.239 | 104.210.146.250
   USA (Mitte) | 40.69.144.231</br>40.69.167.116 | 52.165.34.144
   USA (Ost) 2 | 52.184.158.163</br>52.225.216.31 | 40.79.44.59
   Japan Ost | 52.185.150.140</br>13.78.87.185 | 138.91.1.105
   Japan (Westen) | 52.175.146.69</br>52.175.145.200 | 138.91.17.38
   Brasilien Süd | 191.234.185.172</br>104.41.62.15 | 23.97.97.36
   Australien (Osten) | 104.210.113.114</br>40.126.226.199 | 191.239.64.144
   Australien (Südosten) | 13.70.159.158</br>13.73.114.68 | 191.239.160.45
   Kanada, Mitte | 52.228.36.192</br>52.228.39.52 | 40.85.226.62
   Kanada, Osten | 52.229.125.98</br>52.229.126.170 | 40.86.225.142
   USA, Westen-Mitte | 52.161.20.168</br>13.78.230.131 | 13.78.149.209
   USA, Westen 2 | 52.183.45.166</br>52.175.207.234 | 13.66.228.204
   UK, Westen | 51.141.3.203</br>51.140.226.176 | 51.141.14.113
   UK, Süden | 51.140.43.158</br>51.140.29.146 | 51.140.189.52

## <a name="example-nsg-configuration"></a>Beispielkonfiguration für eine Netzwerksicherheitsgruppe

Dieser Abschnitt zeigt, wie Sie Regeln für Netzwerksicherheitsgruppen konfigurieren, sodass Replikationen für eine VM funktionieren. Wenn Sie Regeln für Netzwerksicherheitsgruppen verwenden, um die ausgehende Konnektivität zu steuern, verwenden Sie für alle erforderlichen IP-Adressbereiche die Regeln „Ausgehende HTTPS-Verbindungen zulassen“.

In diesem Beispiel lautet der Quellstandort des virtuellen Computers „USA, Osten“. Der Zielstandort für die Replikation ist „USA, Mitte“.


### <a name="example"></a>Beispiel

#### <a name="east-us"></a>USA (Ost)

1. Erstellen Sie Regeln, die [IP-Bereichen für die Region „USA, Osten“](https://www.microsoft.com/download/confirmation.aspx?id=41653) entsprechen. Dies ist erforderlich, damit über die VM Daten in das Cachespeicherkonto geschrieben werden können.
2. Erstellen Sie Regeln für alle IP-Adressbereiche, die den [IPv4-Endpunkten zur Authentifizierung und Identifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) bei Office 365 entsprechen.
3. Erstellen Sie Regeln, die dem Zielstandort entsprechen:

   **Standort** | **IP-Adressen des Site Recovery-Diensts** |  **IP-Adressen zur Site Recovery-Überwachung**
    --- | --- | ---
   USA (Mitte) | 40.69.144.231</br>40.69.167.116 | 52.165.34.144

#### <a name="central-us"></a>USA (Mitte)

Diese Regeln sind erforderlich, damit nach dem Failover die Replikation von der Zielregion in die Quellregion aktiviert werden kann.

1. Erstellen Sie Regeln, die [IP-Adressbereichen für die Region „USA, Mitte“](https://www.microsoft.com/download/confirmation.aspx?id=41653) entsprechen.
2. Erstellen Sie Regeln für alle IP-Adressbereiche, die den [IPv4-Endpunkten zur Authentifizierung und Identifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) bei Office 365 entsprechen.
3. Erstellen Sie Regeln, die dem Quellstandort entsprechen:

   **Standort** | **IP-Adressen des Site Recovery-Diensts** |  **IP-Adressen zur Site Recovery-Überwachung**
    --- | --- | ---
   USA (Ost) | 13.82.88.226</br>40.71.38.173 | 104.45.147.24


## <a name="existing-on-premises-connection"></a>Vorhandene lokale Verbindung

Wenn eine ExpressRoute- oder VPN-Verbindung zwischen Ihrem lokalen Standort und dem Quellstandort in Azure besteht, befolgen Sie diese Richtlinien:

**Konfiguration** | **Details**
--- | ---
**Tunnelerzwingung** | Üblicherweise erzwingt eine Standardroute (0.0.0.0/0), dass ausgehender Internetdatenverkehr durch den lokalen Standort geleitet wird. Dies ist nicht zu empfehlen. Der Replikationsdatenverkehr und die Site Recovery-Kommunikation sollten innerhalb von Azure bleiben.<br/><br/> Um dieses Problem zu lösen, fügen Sie benutzerdefinierte Routen (User-Defined Routes, UDRs) für [diese IP-Adressbereiche](#outbound-connectivity-for-azure-site-recovery-ip-ranges) hinzu, damit der Datenverkehr nicht an den lokalen Standort gesendet wird.
**Konnektivität** | Wenn Apps eine Verbindung mit lokalen Computern herstellen müssen oder lokale Clients per VPN/ExpressRoute über den lokalen Standort eine Verbindung mit der App herstellen, stellen Sie sicher, dass mindestens eine [Standort-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) zwischen Ihrer Azure-Zielregion und dem lokalen Standort besteht.<br/><br/> Wenn zwischen der Azure-Zielregion und dem lokalen Standort ein hohes Datenverkehrsaufkommen besteht, erstellen Sie eine weitere [ExpressRoute-Verbindung](../expressroute/expressroute-introduction.md) zwischen der Zielregion und dem lokalen Standort.<br/><br/> Wenn Sie nach einem Failover IP-Adressen für virtuelle Computer beibehalten möchten, lassen Sie die Standort-zu-Standort-/ExpressRoute-Verbindung der Zielregion getrennt. Dadurch wird sichergestellt, dass es keine Konflikte zwischen den Quell- und Ziel-IP-Adressbereichen gibt.
**ExpressRoute** | Erstellen Sie eine ExpressRoute-Leitung in den Quell- und Zielregionen.<br/><br/> Erstellen Sie eine Verbindung zwischen dem Quellnetzwerk und der ExpressRoute-Leitung und zwischen dem Zielnetzwerk und der Leitung.<br/><br/> Es wird empfohlen, verschiedene IP-Adressbereiche in Quell- und Zielregionen zu verwenden. Die Leitung kann nicht gleichzeitig eine Verbindung mit mehr als einem Azure-Netzwerk mit denselben IP-Adressbereichen herstellen.<br/><br/> Sie können virtuelle Netzwerke mit denselben IP-Adressbereichen in beiden Regionen erstellen und dann ExpressRoute-Leitungen in beiden Regionen erstellen. Um ein Failover durchzuführen, trennen Sie die Leitung vom virtuellen Quellnetzwerk, und verbinden Sie die Leitung mit dem virtuellen Zielnetzwerk.<br/><br/> Wenn die primäre Region vollständig ausgefallen ist, kann beim Trennvorgang ein Fehler auftreten. In diesem Fall kann das virtuelle Zielnetzwerk keine Verbindung über ExpressRoute herstellen.
**ExpressRoute Premium** | Sie können Leitungen in derselben geopolitischen Region erstellen.<br/><br/> Um Leitungen in verschiedenen geopolitischen Regionen zu erstellen, benötigen Sie Azure ExpressRoute Premium.<br/><br/> Bei Premium sind die Kosten inkrementell. Wenn Sie diesen Dienst bereits verwenden, fallen keine zusätzlichen Kosten an.<br/><br/> Erfahren Sie mehr über [Standorte](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) und [Preise](https://azure.microsoft.com/pricing/details/expressroute/).



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 4: Erstellen eines Tresors](azure-to-azure-walkthrough-vault.md).


