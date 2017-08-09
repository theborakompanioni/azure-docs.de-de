---
title: "Zuordnen von Netzwerken für die Replikation virtueller Hyper-V-Computer an einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt, wie Netzwerke beim Replizieren virtueller Hyper-V-Computer an einem sekundären VMM-Standort mit Azure Site Recovery zugeordnet werden."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 461b7c1c-ef61-4005-aeec-2324e727a3d0
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 606e2d3d0e31b9d80200105e812f9d7bbbcf939c
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-7-map-networks-for-hyper-v-vm-replication-to-a-secondary-site"></a>Schritt 7: Zuordnen von Netzwerken für die Replikation virtueller Hyper-V-Computer an einem sekundären Standort


Nach dem Einrichten der [Quell-und Zieleinstellungen](vmm-to-vmm-walkthrough-source-target.md) für die Replikation virtueller Hyper-V-Computer an einem sekundären System Center Virtual Machine Manager-Standort (VMM) konfigurieren Sie anhand der Informationen in diesem Artikel die Netzwerkzuordnung für die Replikation virtueller Hyper-V-Computer an einem sekundären Standort mithilfe von [Azure Site Recovery](site-recovery-overview.md).

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.


## <a name="before-you-start"></a>Vorbereitung

- Machen Sie sich zunächst mit der [Netzwerkzuordnung](vmm-to-vmm-walkthrough-network.md#network-mapping-overview) vertraut.
- Stellen Sie sicher, dass die virtuellen Computer auf den VMM-Servern mit einem VM-Netzwerk verbunden sind.

## <a name="configure-network-mapping"></a>Konfigurieren der Netzwerkzuordnung

1. Klicken Sie unter **Netzwerkzuordnung** > **Netzwerkzuordnungen** auf **+Netzwerkzuordnung**.
2. Wählen Sie auf der Registerkarte **Netzwerkzuordnung hinzufügen** die Quell- und Ziel-VMM-Server aus. Die VM-Netzwerke, die den VMM-Servern zugeordnet sind, werden abgerufen.
3. Wählen Sie unter **Quellnetzwerk**in der Liste mit den VM-Netzwerken, die dem primären VMM-Server zugeordnet sind, das gewünschte Netzwerk aus.
4. Wählen Sie unter **Zielnetzwerk** das Netzwerk aus, das Sie auf dem sekundären VMM-Server verwenden möchten. Klicken Sie dann auf **OK**.

    ![Netzwerkzuordnung](./media/vmm-to-vmm-walkthrough-network-mapping/network-mapping2.png)

Wenn die Netzwerkzuordnung beginnt, passiert Folgendes:

* Vorhandene virtuelle Replikatcomputer, die dem Quell-VM-Netzwerk entsprechen, werden mit dem Ziel-VM-Netzwerk verbunden.
* Neue virtuelle Computer, die mit dem Quell-VM-Netzwerk verbunden sind, werden nach der Replikation mit dem zugeordneten Zielnetzwerk verbunden.
* Wenn Sie eine vorhandene Zuordnung mit einem neuen Netzwerk ändern, werden virtuelle Replikatcomputer anhand der neuen Einstellungen verbunden.
* Wenn das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, wird der virtuelle Replikatcomputer nach dem Failover mit diesem Zielsubnetz verbunden. Gibt es kein Zielsubnetz mit einem übereinstimmenden Namen, wird der virtuelle Computer mit dem ersten Subnetz im Netzwerk verbunden.



## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 8: Konfigurieren einer Replikationsrichtlinie](vmm-to-vmm-walkthrough-replication.md).
