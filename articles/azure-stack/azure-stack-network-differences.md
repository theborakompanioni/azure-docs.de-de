---
title: "Azure Stack-Netzwerke: Unterschiede und Überlegungen"
description: "Dieser Artikel beschreibt die Unterschiede und zu berücksichtigende Aspekte bei der Verwendung von Netzwerken in Azure Stack."
services: azure-stack
keywords: 
author: ScottNapolitan
ms.author: victorh
ms.date: 08/02/2017
ms.topic: article
ms.service: azure-stack
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: 0830b394c71cc27d96c7a46867b6c151260f6325
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="considerations-for-azure-stack-networking"></a>Überlegungen zu Azure Stack-Netzwerken

Netzwerke in Azure Stack bieten viele der Funktionen, die Sie auch in Azure finden. Allerdings gibt es einige Unterschiede, die Sie vor dem Bereitstellen kennen sollten.


Dieser Artikel enthält eine Übersicht über die eindeutigen Aspekte für Netzwerke und die entsprechenden Features in Azure Stack. Informationen zu allgemeinen Unterschieden zwischen Azure Stack und Azure finden Sie im Thema [Key considerations](azure-stack-considerations.md) (Wichtige Aspekte).


## <a name="cheat-sheet-networking-differences"></a>Spickzettel: Unterschiede bei Netzwerken

|Dienst | Feature | Azure (global) | Azure Stack |
| --- | --- | --- | --- |
| DNS | Mehrinstanzenfähiges DNS | Unterstützt| Noch nicht unterstützt|
| |DNS-AAAA-Datensätze|Unterstützt|Nicht unterstützt|
| |DNS-Zonen pro Abonnement|100 (Standard)<br>Kann auf Anforderung erhöht werden|100|
| |DNS-Datensätze pro Zone|5000 (Standard)<br>Kann auf Anforderung erhöht werden|5.000|
||Namenserver für die Zonendelegierung|Azure bietet vier Namenserver für jede erstellte Benutzerzone (Mandantenzone).|Azure Stack bietet zwei Namenserver für jede erstellte Benutzerzone (Mandantenzone).|
| Virtuelles Netzwerk|Peering in virtuellen Netzwerken|Verbinden von zwei virtuellen Netzwerken in derselben Region über das Azure-Backbonenetzwerk|Noch nicht unterstützt|
| |IPv6-Adressen|Sie können eine IPv6-Adresse als Teil der [Netzwerkschnittstellenkonfiguration](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-network-interface-addresses#ip-address-versions) zuweisen.|Es wird nur IPv4 unterstützt.|
|VPN-Gateways|Point-to-Site-VPN Gateway|Unterstützt|Noch nicht unterstützt|
| |VNet-zu-VNet-Gateway|Unterstützt|Noch nicht unterstützt|
| |VPN Gateway-SKUs|Unterstützung für Basic, GW1, GW2, GW3, Standard High Performance, Ultra-High Performance |Unterstützung für Basic-, Standard- und High Performance-SKUs|
|Load Balancer|Öffentliche IPv6-Adresse|Unterstützung zum Zuweisen einer öffentlichen IPv6-Adresse zu einem Lastenausgleichsmodul|Es wird nur IPv4 unterstützt.|
|Network Watcher|Überwachungsfunktionen für Network Watcher-Mandantennetzwerk|Unterstützt|Noch nicht unterstützt|
|CDN|Content Delivery Network-Profile|Unterstützt|Noch nicht unterstützt|
|Anwendungsgateway|Lastenausgleich auf Schicht 7|Unterstützt|Noch nicht unterstützt|
|Traffic Manager|Weiterleiten von eingehendem Datenverkehr zur Optimierung der Anwendungsleistung und -zuverlässigkeit|Unterstützt|Noch nicht unterstützt|
|ExpressRoute|Einrichten einer schnellen privaten Verbindung mit Microsoft Cloud Services in Ihrer lokalen Infrastruktur oder Kollokationsumgebung|Unterstützt|Unterstützung zum Verbinden von Azure Stack mit einer Express Route-Verbindung|

## <a name="next-steps"></a>Nächste Schritte

[DNS in Azure Stack](azure-stack-dns.md)

