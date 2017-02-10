---
title: "Vorbereiten der Netzwerkzuordnung für den Schutz virtueller Hyper-V-Computer mit VMM in Azure Site Recovery | Microsoft Docs"
description: "Informationen zum Einrichten der Netzwerkzuordnung für die Replikation virtueller Hyper-V-Computer aus einem lokalen Rechenzentrum in Azure oder an einen sekundären Standort."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: a798580b-b166-426b-bdca-573167b0e451
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/05/2016
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8023643345812a9bc18dcca43cfc0ecff31a5007


---
# <a name="prepare-network-mapping-for-hyper-v-virtual-machine-protection-with-vmm-in-azure-site-recovery"></a>Vorbereiten der Netzwerkzuordnung für den Schutz virtueller Hyper-V-Computer mit VMM in Azure Site Recovery
Azure Site Recovery unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung, indem Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server aufeinander abgestimmt werden.

In diesem Artikel wird die Netzwerkzuordnung beschrieben, die zum optimalen Konfigurieren von Netzwerkeinstellungen dient, und zwar bei Verwenden von Site Recovery zum Replizieren virtueller Hyper-V-Computer in VMM-Clouds zwischen zwei lokalen Rechenzentren oder zwischen einem lokalen Rechenzentrum und Azure. Dieser Artikel ist nicht relevant, wenn Sie virtuelle Hyper-V-Computer ohne VMM-Cloud bzw. VMware-VMs oder physische Server replizieren.

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="overview"></a>Übersicht
Die Netzwerkzuordnung wird verwendet, wenn Azure Site Recovery bereitgestellt ist, um virtuelle Hyper-V-Computer mithilfe der Hyper-V-Replikat- oder SAN-Replikation in Azure oder ein sekundäres Rechenzentrum zu replizieren.

* **Replizieren virtueller Hyper-V-Computer in VMM-Clouds zwischen zwei lokalen Rechenzentren**: Die Netzwerkzuordnung wird zwischen VM-Netzwerken auf einem VMM-Quellserver und VM-Netzwerken auf einem VMM-Zielserver für Folgendes eingesetzt:
  
  * **Verbinden virtueller Computer nach einem Failover**: Stellt sicher, dass die virtuellen Computer nach dem Failover mit geeigneten Netzwerken verbunden sind. Der virtuelle Replikatcomputer wird mit dem Zielnetzwerk verbunden, das dem Quellnetzwerk zugeordnet ist.
  * **Platzieren virtueller Replikatcomputer auf Hostservern**: Virtuelle Replikatcomputer werden optimal auf Hyper-V-Hostservern platziert. Virtuelle Replikatcomputer werden auf Hosts platziert, die auf die zugeordneten VM-Netzwerke zugreifen können.
  * **Keine Netzwerkzuordnung**: Wenn Sie keine Netzwerkzuordnung konfigurieren, werden virtuelle Replikatcomputer nach dem Failover nicht mit VM-Netzwerken verbunden.
* **Replizieren virtueller Hyper-V-Computer in einer lokalen VMM-Cloud in Azure**: Die Netzwerkzuordnung wird zwischen VM-Netzwerken auf dem VMM-Quellserver und Azure-Zielnetzwerken für Folgendes genutzt:
  
  * **Verbinden virtueller Computer nach einem Failover**: Alle Computer, für die ein Failover im gleichen Netzwerk durchgeführt wird, können unabhängig vom jeweiligen Wiederherstellungsplan eine Verbindung untereinander herstellen.
  * **Netzwerkgateway**: Wenn im Azure-Zielnetzwerk ein Netzwerkgateway eingerichtet ist, können die virtuellen Computer eine Verbindung mit anderen lokalen virtuellen Computern herstellen.
  * **Keine Netzwerkzuordnung**: Wenn Sie keine Netzwerkzuordnung konfigurieren, können nach dem Failover zu Azure nur virtuelle Computer aus dem gleichen Wiederherstellungsplan eine Verbindung untereinander herstellen.

## <a name="network-mapping-example"></a>Beispiel einer Netzwerkzuordnung
Die Netzwerkzuordnung kann zwischen VM-Netzwerken auf zwei VMM-Servern konfiguriert werden (oder auf einem einzelnen VMM-Server, wenn zwei Standorte vom gleichen Server verwaltet werden). Bei korrekt konfigurierter Zuordnung und aktivierter Replikation ist ein virtueller Computer am primären Ort mit einem Netzwerk verbunden, und sein Replikat am Zielort wird mit dem zugeordneten Netzwerk verbunden.

Wenn die Netzwerke in VMM ordnungsgemäß eingerichtet wurden, gilt Folgendes: Wenn Sie bei der Netzwerkzuordnung ein VM-Zielnetzwerk auswählen, werden die VMM-Quellclouds, die das VM-Quellnetzwerk verwenden, sowie die verfügbaren VM-Zielnetzwerke in den Zielclouds angezeigt, die für den Schutz verwendet werden.

Im Anschluss folgt ein Beispiel zur Veranschaulichung dieses Mechanismus. Für das Beispiel verwenden wir eine Organisation mit zwei Standorten in New York und Chicago.

| **Standort** | **VMM-Server** | **VM-Netzwerke** | **Zuordnung** |
| --- | --- | --- | --- |
| New York |VMM-NewYork |VMNetwork1-NewYork |Zugeordnet zu „VMNetwork1-Chicago“ |
| VMNetwork2-NewYork |Nicht zugeordnet | | |
| Chicago |VMM-Chicago |VMNetwork1-Chicago |Zugeordnet zu „VMNetwork1-NewYork“ |
| VMNetwork2-Chicago |Nicht zugeordnet | | |

In diesem Beispiel gilt Folgendes:

* Wenn für einen virtuellen, mit „VMNetwork1-NewYork“ verbundenen Computer ein virtueller Replikatcomputer erstellt wird, wird dieser mit „VMNetwork1-Chicago“ verbunden.
* Ein für „VMNetwork2-NewYork“ oder „VMNetwork2-Chicago“ erstellter virtueller Replikatcomputer wird mit keinem Netzwerk verbunden.

Im Anschluss finden Sie Informationen zur Einrichtung der VMM-Clouds für unser Beispielunternehmen sowie zu den logischen Netzwerken, die den Clouds zugeordnet sind.

### <a name="cloud-protection-settings"></a>Cloudschutzeinstellungen
| **Geschützte Cloud** | **Schützende Cloud** | **Logisches Netzwerk (New York)** |
| --- | --- | --- |
| GoldCloud1 |GoldCloud2 | |
| SilverCloud1 |SilverCloud2 | |
| GoldCloud2 |<p>NA</p><p></p> |<p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p> |
| SilverCloud2 |<p>NA</p><p></p> |<p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p> |

### <a name="logical-and-vm-network-settings"></a>Einstellungen für das logische und das VM-basierte Netzwerk
| **Standort** | **Logisches Netzwerk** | **Zugeordnetes VM-Netzwerk** |
| --- | --- | --- |
| New York |LogicalNetwork1-NewYork |VMNetwork1-NewYork |
| Chicago |LogicalNetwork1-Chicago |VMNetwork1-Chicago |
| LogicalNetwork2-Chicago |VMNetwork2-Chicago | |

### <a name="target-networks"></a>Zielnetzwerke
Die folgende Tabelle zeigt die Optionen, die zur Verfügung stehen, wenn Sie bei Verwendung dieser Einstellungen das VM-Zielnetzwerk auswählen:

| **Auswahl** | **Geschützte Cloud** | **Schützende Cloud** | **Verfügbarkeit des Zielnetzwerks** |
| --- | --- | --- | --- |
| VMNetwork1-Chicago |SilverCloud1 |SilverCloud2 |Verfügbar |
| GoldCloud1 |GoldCloud2 |Verfügbar | |
| VMNetwork2-Chicago |SilverCloud1 |SilverCloud2 |Nicht verfügbar |
| GoldCloud1 |GoldCloud2 |Verfügbar | |

## <a name="multiple-subnets"></a>Mehrere Subnetze
Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, wird der virtuelle Replikatcomputer nach dem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.

### <a name="failback"></a>Failback
Zur Veranschaulichung der Vorgänge bei einem Failback (umgekehrte Replikation) nehmen wir an, dass zwischen „VMNetwork1-NewYork“ und „VMNetwork1-Chicago“ eine Zuordnung mit den folgenden Einstellungen besteht:

| **Virtueller Computer** | **Verbundenes VM-Netzwerk** |
| --- | --- |
| VM1 |VMNetwork1-NewYork |
| VM2 (Replikat von VM1) |VMNetwork1-Chicago |

Einige mögliche Szenarien mit diesen Einstellungen:

| **Szenario** | **Ergebnis** |
| --- | --- |
| Keine Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover |VM-1 bleibt mit dem Quellnetzwerk verbunden. |
| Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover und Trennung der Verbindung |VM-1 wird getrennt. |
| Änderung der Netzwerkeigenschaften von VM-2 nach dem Failover und Verbindung mit "VMNetwork2-Chicago" |Wenn "VMNetwork2-Chicago" nicht zugeordnet ist, wird VM-1 getrennt. |
| Änderung der Netzwerkzuordnung von "VMNetwork1-Chicago" |VM-1 wird mit dem Netzwerk verbunden, das jetzt "VMNetwork1-Chicago" zugeordnet ist. |

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit Netzwerkzuordnung vertraut gemacht haben, können Sie sich mit der [Site Recovery-Bereitstellung beginnen](site-recovery-best-practices.md).




<!--HONumber=Nov16_HO3-->


