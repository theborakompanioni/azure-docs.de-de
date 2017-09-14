---
title: "Leitfaden für Netzwerke zur Replikation von virtuellen Computern von Azure nach Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Leitfaden für Netzwerke zum Replizieren von virtuellen Azure-Computern"
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
ms.date: 08/31/2017
ms.author: sujayt
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: 99c08a1efbc610959fb4ba824dcb0601efac5877
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="networking-guidance-for-replicating-azure-virtual-machines"></a>Leitfaden für Netzwerke zum Replizieren von virtuellen Azure-Computern

>[!NOTE]
> Die Site Recovery-Replikation für virtuelle Azure-Computer ist derzeit in der Vorschau verfügbar.

In diesem Artikel wird der Leitfaden für Netzwerke für Azure Site Recovery bei der Replikation und Wiederherstellung von virtuellen Azure-Computern zwischen verschiedenen Regionen beschrieben. Weitere Informationen zu den Anforderungen für Azure Site Recovery finden Sie im Artikel [Voraussetzungen](site-recovery-prereq.md).

## <a name="site-recovery-architecture"></a>Site Recovery-Architektur

Site Recovery bietet eine einfache Möglichkeit, um Anwendungen, die auf virtuellen Azure-Computern ausgeführt werden, in einer anderen Azure-Region zu replizieren, sodass sie im Falle einer Störung in der primären Region wiederhergestellt werden können. Weitere Informationen zu diesem Szenario und zur Site Recovery-Architektur finden Sie [hier](site-recovery-azure-to-azure-architecture.md).

## <a name="your-network-infrastructure"></a>Ihre Netzwerkinfrastruktur

In der folgenden Abbildung wird eine typische Azure-Umgebung für eine Anwendung dargestellt, die auf virtuellen Azure-Computern ausgeführt wird:

![Kundenumgebung](./media/site-recovery-azure-to-azure-architecture/source-environment.png)

Wenn Sie eine Azure ExpressRoute- bzw. VPN-Verbindung über ein lokales Netzwerk in Azure verwenden, sieht die Umgebung wie folgt aus:

![Kundenumgebung](./media/site-recovery-azure-to-azure-architecture/source-environment-expressroute.png)

In der Regel schützen Kunden ihre Netzwerke durch Firewalls und/oder Netzwerksicherheitsgruppen (NSGs). Die Firewalls können URL-basierte oder IP-basierte Whitelists zur Steuerung der Netzwerkkonnektivität verwenden. NSGs können Regeln für die Verwendung von IP-Adressbereichen zur Steuerung der Netzwerkkonnektivität zulassen.

>[!IMPORTANT]
> Authentifizierte Proxy zur Steuerung der Netzwerkkonnektivität werden nicht unterstützt. Zudem kann in diesem Fall die Site Recovery-Replikation nicht aktiviert werden.

In den folgenden Abschnitten werden die Änderungen an ausgehenden Netzwerkverbindungen erläutert, die bei virtuellen Azure-Computern zur Durchführung der Site Recovery-Replikation vorgenommen werden müssen.

## <a name="outbound-connectivity-for-azure-site-recovery-urls"></a>Ausgehende Verbindungen für Azure Site Recovery-URLs

Wenn Sie URL-basierte Firewallproxys zur Steuerung ausgehender Verbindungen verwenden, stellen Sie sicher, dass die folgenden erforderlichen Dienst-URLs von Azure Site Recovery zur Whitelist hinzugefügt werden:


**URL** | **Zweck**  
--- | ---
*.blob.core.windows.net | Erforderlich, damit Daten in das Cachespeicherkonto in der Quellregion über die VM geschrieben werden können.
login.microsoftonline.com | Erforderlich für die Autorisierung und Authentifizierung bei den Site Recovery-Dienst-URLs.
*.hypervrecoverymanager.windowsazure.com | Erforderlich, um die Kommunikation mit dem Site Recovery-Dienst über die VM zu ermöglichen.
*.servicebus.windows.net | Erforderlich, damit die Site Recovery-Überwachungs- und -Diagnosedaten über die VM geschrieben werden können.

## <a name="outbound-connectivity-for-azure-site-recovery-ip-ranges"></a>Ausgehende Verbindungen für Azure Site Recovery-IP-Adressbereiche

>[!NOTE]
> Um automatisch die erforderlichen NSG-Regeln in der Netzwerksicherheitsgruppe zu erstellen, können Sie [dieses Skript herunterladen und verwenden](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

>[!IMPORTANT]
> * Es wird empfohlen, vor der Erstellung der Regeln in einer Netzwerksicherheitsgruppe für die Produktion die erforderlichen NSG-Regeln in einer Test-Netzwerksicherheitsgruppe zu erstellen und sicherzustellen, dass keine Probleme vorliegen.
> * Um die erforderliche Anzahl von NSG-Regeln zu erstellen, stellen Sie sicher, dass Ihr Abonnement in der Whitelist enthalten ist. Wenden Sie sich an den Support, um den Grenzwert der NSG-Regeln in Ihrem Abonnement zu erhöhen.

Wenn Sie IP-basierte Firewallproxys oder NSG-Regeln zur Steuerung ausgehender Verbindungen verwenden, müssen abhängig von den Quell- und Zielstandorten der virtuellen Computer folgende IP-Adressbereiche zur Whitelist hinzugefügt werden:

- Alle IP-Adressbereiche, die dem Quellstandort entsprechen. (Sie können die IP-Adressbereiche [hier](https://www.microsoft.com/download/confirmation.aspx?id=41653) herunterladen.) Whitelists sind erforderlich, damit Daten in das Cachespeicherkonto über die VM geschrieben werden können.

- Alle IP-Adressbereiche, die den [IPv4-Endpunkten zur Authentifizierung und Identifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) bei Office 365 entsprechen.

    >[!NOTE]
    > Wenn in Zukunft neue IP-Adressen zu den Office 365-IP-Adressbereichen hinzugefügt werden, müssen Sie neue NSG-Regeln erstellen.

- IP-Adressen von Site Recovery-Dienstendpunkten ([in einer XML-Datei verfügbar](https://aka.ms/site-recovery-public-ips)), die von Ihrem Zielstandort abhängen:

   **Zielstandort** | **IP-Adressen des Site Recovery-Diensts** |  **IP-Adressen zur Site Recovery-Überwachung**
   --- | --- | ---
   Ostasien | 52.175.17.132 | 13.94.47.61
   Südostasien | 52.187.58.193 | 13.76.179.223
   Indien (Mitte) | 52.172.187.37 | 104.211.98.185
   Indien, Süden | 52.172.46.220 | 104.211.224.190
   USA (Mitte/Norden) | 23.96.195.247 | 168.62.249.226
   Nordeuropa | 40.69.212.238 | 52.169.18.8
   Westeuropa | 52.166.13.64 | 40.68.93.145
   USA (Ost) | 13.82.88.226 | 104.45.147.24
   USA (West) | 40.83.179.48 | 104.40.26.199
   USA (Mitte/Süden) | 13.84.148.14 | 104.210.146.250
   USA (Mitte) | 40.69.144.231 | 52.165.34.144
   USA (Ost) 2 | 52.184.158.163 | 40.79.44.59
   Japan Ost | 52.185.150.140 | 138.91.1.105
   Japan (Westen) | 52.175.146.69 | 138.91.17.38
   Brasilien Süd | 191.234.185.172 | 23.97.97.36
   Australien (Osten) | 104.210.113.114 | 191.239.64.144
   Australien (Südosten) | 13.70.159.158 | 191.239.160.45
   Kanada, Mitte | 52.228.36.192 | 40.85.226.62
   Kanada, Osten | 52.229.125.98 | 40.86.225.142
   USA, Westen-Mitte | 52.161.20.168 | 13.78.149.209
   USA, Westen 2 | 52.183.45.166 | 13.66.228.204
   UK, Westen | 51.141.3.203 | 51.141.14.113
   UK, Süden | 51.140.43.158 | 51.140.189.52
   Großbritannien, Süden 2 | 13.87.37.4| 13.87.34.139
   Großbritannien, Norden | 51.142.209.167 | 13.87.102.68
   Korea, Mitte | 52.231.28.253 | 52.231.32.85
   Korea, Süden | 52.231.298.185 | 52.231.200.144

## <a name="sample-nsg-configuration"></a>Beispielkonfiguration für NSG-Regeln
In diesem Abschnitt werden die Schritte zur Konfiguration von NSG-Regeln erläutert, um die Site Recovery-Replikation auf einem virtuellen Computer durchführen zu können. Wenn Sie ausgehende Verbindungen anhand von NSG-Regeln steuern, verwenden Sie für alle erforderlichen IP-Adressbereiche die Regeln „Ausgehende HTTPS-Verbindungen zulassen“.

>[!Note]
> Um automatisch die erforderlichen NSG-Regeln in der Netzwerksicherheitsgruppe zu erstellen, können Sie [dieses Skript herunterladen und verwenden](https://gallery.technet.microsoft.com/Azure-Recovery-script-to-0c950702).

Wenn der Quellstandort Ihrer VM beispielsweise „USA, Osten“ und Ihr Replikationszielstandort „USA, Mitte“ lautet, führen Sie die Schritte in den folgenden beiden Abschnitten durch.

>[!IMPORTANT]
> * Es wird empfohlen, vor der Erstellung der Regeln in einer Netzwerksicherheitsgruppe für die Produktion die erforderlichen NSG-Regeln in einer Test-Netzwerksicherheitsgruppe zu erstellen und sicherzustellen, dass keine Probleme vorliegen.
> * Um die erforderliche Anzahl von NSG-Regeln zu erstellen, stellen Sie sicher, dass Ihr Abonnement in der Whitelist enthalten ist. Wenden Sie sich an den Support, um den Grenzwert der NSG-Regeln in Ihrem Abonnement zu erhöhen.

### <a name="nsg-rules-on-the-east-us-network-security-group"></a>NSG-Regeln zur Netzwerksicherheitsgruppe für die Region „USA, Osten“

* Erstellen Sie Regeln, die [IP-Bereichen für die Region „USA, Osten“](https://www.microsoft.com/download/confirmation.aspx?id=41653) entsprechen. Dies ist erforderlich, damit über die VM Daten in das Cachespeicherkonto geschrieben werden können.

* Erstellen Sie Regeln für alle IP-Adressbereiche, die den [IPv4-Endpunkten zur Authentifizierung und Identifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) bei Office 365 entsprechen.

* Erstellen Sie Regeln, die dem Zielstandort entsprechen:

   **Standort** | **IP-Adressen des Site Recovery-Diensts** |  **IP-Adressen zur Site Recovery-Überwachung**
    --- | --- | ---
   USA (Mitte) | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules-on-the-central-us-network-security-group"></a>NSG-Regeln zur Netzwerksicherheitsgruppe für die Region „USA, Mitte“

Diese Regeln sind erforderlich, damit nach dem Failover die Replikation von der Zielregion zur Quellregion aktiviert werden kann:

* Regeln, die [IP-Adressbereichen für die Region „USA, Mitte“](https://www.microsoft.com/download/confirmation.aspx?id=41653) entsprechen. Diese sind erforderlich, damit über die VM Daten in das Cachespeicherkonto geschrieben werden können.

* Regeln für alle IP-Adressbereiche, die den [IPv4-Endpunkten zur Authentifizierung und Identifizierung](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2#bkmk_identity) bei Office 365 entsprechen.

* Regeln, die dem Quellstandort entsprechen:

   **Standort** | **IP-Adressen des Site Recovery-Diensts** |  **IP-Adressen zur Site Recovery-Überwachung**
    --- | --- | ---
   USA (Ost) | 13.82.88.226 | 104.45.147.24


## <a name="guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration"></a>Richtlinien für die Konfiguration vorhandener ExpressRoute-Verbindungen zwischen Azure und lokalen Standorten bzw. VPN-Verbindungen

Wenn eine ExpressRoute- oder VPN-Verbindung zwischen dem lokalen und dem Quellstandort in Azure besteht, befolgen Sie die Richtlinien in diesem Abschnitt.

### <a name="forced-tunneling-configuration"></a>Konfiguration der Tunnelerzwingung

Eine allgemeine Kundenkonfiguration dient zum Definieren einer Standardroute (0.0.0.0/0), die die Weiterleitung des ausgehenden Internetdatenverkehrs über den lokalen Standort erzwingt. Hiervon wird abgeraten. Der Replikationsdatenverkehr und die Kommunikation mit dem Site Recovery-Dienst sollten nicht die Grenzen von Azure verlassen. Die Lösung besteht darin, benutzerdefinierte Routen (User-Defined Routes, UDRs) für [diese IP-Adressbereiche](#outbound-connectivity-for-azure-site-recovery-ip-ranges) hinzuzufügen, damit der Replikationsdatenverkehr nicht über den lokalen Standort weitergeleitet wird.

### <a name="connectivity-between-the-target-and-on-premises-location"></a>Konnektivität zwischen dem Zielstandort und dem lokalen Standort

Befolgen Sie die folgenden Richtlinien für Verbindungen zwischen dem Zielstandort und dem lokalen Standort:
- Wenn Ihre Anwendung eine Verbindung mit lokalen Computern herstellen muss oder Clients von lokalen Standorten aus über VPN/ExpressRoute eine Verbindung mit der Anwendung herstellen, stellen Sie sicher, dass mindestens eine [Standort-zu-Standort-Verbindung](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) zwischen Ihrer Azure-Zielregion und dem lokalen Rechenzentrum besteht.

- Wenn Sie mit einem hohen Datenverkehrsaufkommen zwischen Ihrer Azure-Zielregion und dem lokalen Rechenzentrum rechnen, sollten Sie eine weitere [ExpressRoute-Verbindung](../expressroute/expressroute-introduction.md) zwischen der Azure-Zielregion und dem lokalen Rechenzentrum erstellen.

- Wenn Sie nach einem Failover IP-Adressen für virtuelle Computer beibehalten möchten, lassen Sie die Standort-zu-Standort-/ExpressRoute-Verbindung der Zielregion getrennt. Dadurch wird sichergestellt, dass keine Bereichskonflikte zwischen den IP-Adressbereichen der Quellregion und denen der Zielregion entstehen.

### <a name="best-practices-for-expressroute-configuration"></a>Best Practices für die ExpressRoute-Konfiguration
Befolgen Sie die folgenden Best Practices für die ExpressRoute-Konfiguration:

- Sie müssen eine ExpressRoute-Verbindung in den Quell- und Zielregionen herstellen. Dann müssen Sie eine Verbindung zwischen Folgendem herstellen:
  - Dem virtuellen Quellnetzwerk und der ExpressRoute-Verbindung.
  - Dem virtuellen Zielnetzwerk und der ExpressRoute-Verbindung.

- Im Rahmen des ExpressRoute-Standards können Sie Verbindungen in derselben geopolitischen Region erstellen. Für die Erstellung von ExpressRoute-Verbindungen in verschiedenen geopolitischen Regionen ist Azure ExpressRoute Premium erforderlich. Hierfür werden zusätzliche Kosten erhoben. (Wenn Sie bereits ExpressRoute Premium verwenden, fallen keine zusätzlichen Kosten an.) Weitere Informationen finden Sie im [Dokument zu ExpressRoute-Standorten](../expressroute/expressroute-locations.md#azure-regions-to-expressroute-locations-within-a-geopolitical-region) und [ExpressRoute-Preisen](https://azure.microsoft.com/pricing/details/expressroute/).

- Es wird empfohlen, verschiedene IP-Adressbereiche in Quell- und Zielregionen zu verwenden. Die ExpressRoute-Verbindung kann nicht gleichzeitig eine Verbindung mit zwei virtuellen Azure-Netzwerke mit denselben IP-Adressbereichen herstellen.

- Sie können virtuelle Netzwerke mit denselben IP-Adressbereichen in beiden Regionen erstellen und dann ExpressRoute-Verbindungen in beiden Regionen erstellen. Trennen Sie im Falle eines Failovers die Verbindung vom virtuellen Quellnetzwerk und stellen Sie eine Verbindung mit dem virtuellen Zielnetzwerk her.

 >[!IMPORTANT]
 > Wenn die primäre Region nicht vollständig deaktiviert ist, kann ein Fehler beim Trennvorgang auftreten. Hierdurch kann das virtuelle Zielnetzwerk keine ExpressRoute-Verbindung herstellen.

## <a name="next-steps"></a>Nächste Schritte
Schützen Sie Ihre Workloads durch die [Replikation virtueller Azure-Computer](site-recovery-azure-to-azure.md).

