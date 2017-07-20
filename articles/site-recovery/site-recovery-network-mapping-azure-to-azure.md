---
title: Netzwerkzuordnung zwischen zwei Azure-Regionen in Azure Site Recovery | Microsoft-Dokumentation
description: "Azure Site Recovery koordiniert Replikation, Failover und Wiederherstellung virtueller Computer und physischer Server. Informieren Sie sich über das Failover zu Azure oder zu einem sekundären Rechenzentrum."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 5/25/2017
ms.author: pratshar
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 34f5caa13aa94dc982ed497d7c8ddeceed61a6d8
ms.contentlocale: de-de
ms.lasthandoff: 06/15/2017


---
# <a name="network-mapping-between-two-azure-regions"></a>Netzwerkzuordnung zwischen zwei Azure-Regionen


In diesem Artikel wird beschrieben, wie Sie virtuelle Azure-Netzwerke aus zwei Azure-Regionen einander zuordnen. Durch die Netzwerkzuordnung wird sichergestellt, dass ein replizierter virtueller Computer in der Azure-Zielregion in dem virtuellen Netzwerk erstellt wird, das dem virtuellen Netzwerk des virtuellen Quellcomputers zugeordnet ist.  

## <a name="prerequisites"></a>Voraussetzungen
Vergewissern Sie sich vor dem Zuordnen von Netzwerken, dass Sie [virtuelle Azure-Netzwerke](../virtual-network/virtual-networks-overview.md) sowohl in der Azure-Quellregion als auch in der Azure-Zielregion erstellt haben.

## <a name="map-networks"></a>Zuordnen von Netzwerken

Wechseln Sie zum Zuordnen eines virtuellen Azure-Netzwerks zu einem virtuellen Netzwerk in einer anderen Azure-Region zu „Site Recovery-Infrastruktur > Netzwerkzuordnung“ (für virtuelle Azure-Computer), und erstellen Sie eine Netzwerkzuordnung.

![Netzwerkzuordnung](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


Im folgenden Beispiel wird der virtuelle Computer in der Region „Asien, Osten“ ausgeführt und in „Asien, Südosten“ repliziert.

Wählen Sie das Quell- und das Zielnetzwerk aus, und klicken Sie dann auf „OK“, um eine Netzwerkzuordnung von „Asien, Osten“ mit „Asien, Südosten“ zu erstellen.

![Netzwerkzuordnung](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


Führen Sie dieselben Schritte aus, um eine Netzwerkzuordnung von „Asien, Südosten“ mit „Asien, Osten“ zu erstellen.  
![Netzwerkzuordnung](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a>Netzwerkzuordnung beim Aktivieren der Replikation

Wenn beim erstmaligen Replizieren eines virtuellen Computers von einer Azure-Region in einer anderen keine Netzwerkzuordnung vorgenommen wurde, können Sie das Zielnetzwerk im Rahmen dieses Vorgangs auswählen. Site Recovery erstellt die Netzwerkzuordnungen von Quellregion und Zielregion und von Zielregion und Quellregion basierend auf dieser Auswahl.   

![Netzwerkzuordnung](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

Site Recovery erstellt standardmäßig ein Netzwerk in der Zielregion, das identisch mit dem Quellnetzwerk ist, und fügt das Suffix „-asr“ an den Namen des Quellnetzwerks an. Sie können ein bereits erstelltes Netzwerk auswählen, indem Sie auf „Anpassen“ klicken.

![Netzwerkzuordnung](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


Wenn die Netzwerkzuordnung bereits erfolgt ist, können Sie das virtuelle Zielnetzwerk beim Aktivieren der Replikation nicht ändern. Um dies zu ändern, müssen Sie die vorhandene Netzwerkzuordnung ändern.  

![Netzwerkzuordnung](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Netzwerkzuordnung](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> Wenn Sie eine Netzwerkzuordnung von Region-1 zu Region-2 ändern, stellen Sie sicher, dass Sie die Netzwerkzuordnung von Region-2 zu Region-1 ebenfalls ändern.
>
>


## <a name="subnet-selection"></a>Subnetzauswahl
Das Subnetz des virtuellen Zielcomputers wird basierend auf dem Namen des Subnetzes des virtuellen Quellcomputers ausgewählt. Wenn im Zielnetzwerk ein Subnetz mit demselben Namen wie der virtuelle Quellcomputer verfügbar ist, wird dieses für den virtuellen Zielcomputer ausgewählt. Wenn im Zielnetzwerk kein Subnetz mit demselben Namen vorhanden ist, wird das Subnetz, das in der alphabetischen Reihenfolge an erster Stelle steht, als Zielsubnetz ausgewählt. Sie können dieses Subnetz ändern, indem Sie zu den Einstellungen für Compute und Netzwerk des virtuellen Computers navigieren.

![Ändern des Subnetzes](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a>IP-Adresse

Die IP-Adresse für jede Netzwerkschnittstelle des virtuellen Zielcomputers wird wie folgt ausgewählt:

### <a name="dhcp"></a>DHCP
Wenn die Netzwerkschnittstelle des virtuellen Quellcomputers DHCP verwendet, wird die Netzwerkschnittstelle des virtuellen Zielcomputers ebenfalls als DHCP festgelegt.

### <a name="static-ip"></a>Statische IP
Wenn die Netzwerkschnittstelle des virtuellen Quellcomputers eine statische IP verwendet, wird die Netzwerkschnittstelle des virtuellen Zielcomputers ebenfalls für die Verwendung einer statischen IP festgelegt. Die statische IP-Adresse wird wie folgt ausgewählt:

#### <a name="same-address-space"></a>Gleicher Adressraum

Wenn das Quellsubnetz und das Zielsubnetz über denselben Adressraum verfügen, wird die Ziel-IP auf die IP-Adresse der Netzwerkschnittstelle des virtuellen Quellcomputers festgelegt. Wenn dieselbe IP nicht verfügbar ist, wird eine andere verfügbare IP als die Ziel-IP festgelegt.

#### <a name="different-address-space"></a>Unterschiedlicher Adressraum

Wenn das Quellsubnetz und das Zielsubnetz über unterschiedliche Adressräume verfügen, wird die Ziel-IP auf eine der verfügbaren IPs im Zielsubnetz festgelegt.

Sie können die Ziel-IP für jede Netzwerkschnittstelle ändern, indem Sie zu den Einstellungen für Compute und Netzwerk des virtuellen Computers navigieren.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu [Netzwerkrichtlinien zum Replizieren von virtuellen Azure-Computern](site-recovery-azure-to-azure-networking-guidance.md)

