---
title: "Planen der Netzwerkzuordnung für die Hyper-V-VM-Replikation mit Site Recovery | Microsoft-Dokumentation"
description: "Informationen zum Einrichten der Netzwerkzuordnung für die Replikation virtueller Hyper-V-Computer aus einem lokalen Rechenzentrum in Azure oder an einen sekundären Standort."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: tysonn
ms.assetid: fcaa2f52-489d-4c1c-865f-9e78e000b351
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/23/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: b1b8b1ebc013a5dfb69528f9353369e18f84e61f
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017

---


# <a name="plan-network-mapping-for-hyper-v-vm-replication-with-site-recovery"></a>Planen der Netzwerkzuordnung für die Hyper-V-VM-Replikation mit Site Recovery



Dieser Artikel hilft Ihnen, die Netzwerkzuordnung bei der Replikation von Hyper-V-VMs in Azure oder einem sekundären Standort mit dem [Azure Site Recovery-Dienst](site-recovery-overview.md) zu verstehen und zu planen.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende dieses Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="network-mapping-for-replication-to-azure"></a>Netzwerkzuordnung für die Replikation nach Azure

Die Netzwerkzuordnung wird bei der Replikation von Hyper-V-VMs (die in VMM verwaltet werden) nach Azure verwendet. Die Netzwerkzuordnung erfolgt zwischen VM-Netzwerken auf einem VMM-Quellserver und Azure-Zielnetzwerken. Die Zuordnung ermöglicht Folgendes:

- **Netzwerkverbindung** – Stellt sicher, dass replizierte Azure-VMs mit dem zugeordneten Netzwerk verbunden sind. Alle Computer, auf denen ein Failover im selben Netzwerk ausgeführt wird, können untereinander eine Verbindung herstellen, selbst wenn das Failover in verschiedenen Wiederherstellungsplänen stattgefunden hat.
- **Netzwerkgateway** – Wenn im Azure-Zielnetzwerk ein Netzwerkgateway eingerichtet ist, können die VMs eine Verbindung mit anderen lokalen virtuellen Computern herstellen.

Beachten Sie Folgendes:

- Ein VMM-VM-Quellnetzwerk wird einem virtuellen Azure-Netzwerk zugeordnet.
- Nach einem Failover werden Azure-VMs im Quellnetzwerk mit dem zugeordneten virtuellen Zielnetzwerk verbunden.
- Neue VMs, die dem VM-Quellnetzwerk hinzugefügt werden, werden bei der Replikation mit dem zugeordneten Azure-Netzwerk verbunden.
- Fall das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, stellt der virtuelle Replikatcomputer nach dem Failover eine Verbindung mit diesem Zielsubnetz her.
- Ist kein Zielsubnetz mit einem übereinstimmenden Namen vorhanden, stellt der virtuelle Computer eine Verbindung mit dem ersten Subnetz im Netzwerk her.


## <a name="network-mapping-for-replication-to-a-secondary-datacenter"></a>Netzwerkzuordnung für die Replikation in einem sekundären Rechenzentrum

Die Netzwerkzuordnung wird bei der Replikation von Hyper-V-VMs (die in System Center Virtual Machine Manager, kurz VMM, verwaltet werden) in einem sekundären Rechenzentrum verwendet. Die Netzwerkzuordnung erfolgt zwischen VM-Netzwerken auf einem VMM-Quellserver und VM-Netzwerken auf einem VMM-Zielserver. Die Zuordnung ermöglicht Folgendes:

- **Netzwerkverbindung** – Verbindet VMs nach einem Failover mit den entsprechenden Netzwerken. Die Replikat-VM wird mit dem Zielnetzwerk verbunden, das dem Quellnetzwerk zugeordnet ist.
- **Optimale Platzierung** – Platziert die Replikat-VMs optimal auf Hyper-V-Hostservern. Replikat-VMs werden auf Hosts platziert, die auf die zugeordneten VM-Netzwerke zugreifen können.
- **Keine Netzwerkzuordnung** – Wenn Sie keine Netzwerkzuordnung konfigurieren, werden Replikat-VMs nach dem Failover nicht mit VM-Netzwerken verbunden.

Beachten Sie Folgendes:

- Die Netzwerkzuordnung kann zwischen VM-Netzwerken auf zwei VMM-Servern konfiguriert werden (oder auf einem einzelnen VMM-Server, wenn zwei Standorte vom gleichen Server verwaltet werden).
- Bei korrekt konfigurierter Zuordnung und aktivierter Replikation ist ein virtueller Computer am primären Standort mit einem Netzwerk verbunden, und sein Replikat am Zielort wird mit dem zugeordneten Netzwerk verbunden.
-
- Wenn die Netzwerke in VMM ordnungsgemäß eingerichtet wurden, gilt Folgendes: Wenn Sie bei der Netzwerkzuordnung ein VM-Zielnetzwerk auswählen, werden die VMM-Quellclouds, die das VM-Quellnetzwerk verwenden, sowie die verfügbaren VM-Zielnetzwerke in den Zielclouds angezeigt, die für den Schutz verwendet werden.
- Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, wird der virtuelle Replikatcomputer nach dem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.



### <a name="example"></a>Beispiel

Im Anschluss folgt ein Beispiel zur Veranschaulichung dieses Mechanismus. Für das Beispiel verwenden wir eine Organisation mit zwei Standorten in New York und Chicago.

**Standort** | **VMM-Server** | **VM-Netzwerke** | **Zuordnung**
---|---|---|---
New York | VMM-NewYork| VMNetwork1-NewYork | Zugeordnet zu „VMNetwork1-Chicago“
 |  | VMNetwork2-NewYork | Nicht zugeordnet
Chicago | VMM-Chicago| VMNetwork1-Chicago | Zugeordnet zu „VMNetwork1-NewYork“
 | | VMNetwork1-Chicago | Nicht zugeordnet

In diesem Beispiel:

- Wenn für einen virtuellen, mit „VMNetwork1-NewYork“ verbundenen Computer ein virtueller Replikatcomputer erstellt wird, wird dieser mit „VMNetwork1-Chicago“ verbunden.
- Ein für „VMNetwork2-NewYork“ oder „VMNetwork2-Chicago“ erstellter virtueller Replikatcomputer wird mit keinem Netzwerk verbunden.

Im Anschluss finden Sie Informationen zur Einrichtung der VMM-Clouds für unser Beispielunternehmen sowie zu den logischen Netzwerken, die den Clouds zugeordnet sind.

#### <a name="cloud-protection-settings"></a>Cloudschutzeinstellungen

**Geschützte Cloud** | **Schützende Cloud** | **Logisches Netzwerk (New York)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>NA</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

#### <a name="logical-and-vm-network-settings"></a>Einstellungen für das logische und das VM-basierte Netzwerk

**Standort** | **Logisches Netzwerk** | **Zugeordnetes VM-Netzwerk**
---|---|---
New York | LogicalNetwork1-NewYork | VMNetwork1-NewYork
Chicago | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2-Chicago | VMNetwork2-Chicago

#### <a name="target-network-settings"></a>Zielnetzwerkeinstellungen

Die folgende Tabelle zeigt die Optionen, die zur Verfügung stehen, wenn Sie bei Verwendung dieser Einstellungen das VM-Zielnetzwerk auswählen:

**Auswahl** | **Geschützte Cloud** | **Schützende Cloud** | **Verfügbarkeit des Zielnetzwerks**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | Verfügbar
 | GoldCloud1 | GoldCloud2 | Verfügbar
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | Nicht verfügbar
 | GoldCloud1 | GoldCloud2 | Verfügbar


Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, wird der virtuelle Replikatcomputer nach dem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.


#### <a name="failback-behavior"></a>Failbackverhalten

Zur Veranschaulichung der Vorgänge bei einem Failback (umgekehrte Replikation) nehmen wir an, dass zwischen „VMNetwork1-NewYork“ und „VMNetwork1-Chicago“ eine Zuordnung mit den folgenden Einstellungen besteht:


**Virtueller Computer** | **Verbundenes VM-Netzwerk**
---|---
VM1 | VMNetwork1-NewYork
VM2 (Replikat von VM1) | VMNetwork1-Chicago

Einige mögliche Szenarien mit diesen Einstellungen:

**Szenario** | **Ergebnis**
---|---
Keine Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover | VM-1 bleibt mit dem Quellnetzwerk verbunden.
Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover und Trennung der Verbindung | VM-1 wird getrennt.
Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover und Verbindung mit "VMNetwork2-Chicago" | Wenn "VMNetwork2-Chicago" nicht zugeordnet ist, wird VM-1 getrennt.
Änderung der Netzwerkzuordnung von "VMNetwork1-Chicago" | VM-1 wird mit dem Netzwerk verbunden, das jetzt "VMNetwork1-Chicago" zugeordnet ist.



## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über das [Planen der Netzwerkinfrastruktur](site-recovery-network-design.md).

