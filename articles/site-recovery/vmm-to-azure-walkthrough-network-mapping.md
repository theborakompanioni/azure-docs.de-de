---
title: "Konfigurieren der Netzwerkzuordnung für die Replikation virtueller Hyper-V-Computer in VMM-Clouds in Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: Beschreibt, wie die Netzwerkzuordnung beim Replizieren von virtuellen Hyper-V-Computern in VMM-Clouds in Azure mithilfe von Azure Site Recovery konfiguriert wird.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: ed6f73d8baea5af0d2aa5f0ae885f305911ccc82
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="step-9-configure-network-mapping-for-hyper-v-replication-with-vmm-to-azure"></a>Schritt 9: Konfigurieren der Netzwerkzuordnung für die Hyper-V-Replikation (mit VMM) in Azure

Nach dem Einrichten der [Replikationseinstellungen für Quelle und Ziel](vmm-to-azure-walkthrough-source-target.md) konfigurieren Sie anhand der Informationen in diesem Artikel die Netzwerkzuordnung zwischen lokalen VMM-VM-Netzwerken und Azure-Netzwerken.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="before-you-start"></a>Vorbereitung

- Machen Sie sich mit der [Netzwerkzuordnung](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure) vertraut.
- [Bereiten Sie VMM für die Netzwerkzuordnung vor](vmm-to-azure-walkthrough-network.md#prepare-vmm-for-network-mapping). 
- Vergewissern Sie sich, dass virtuelle Computer auf dem VMM-Server über eine Verbindung mit einem VM-Netzwerk verfügen und dass Sie mindestens ein virtuelles Azure-Netzwerk erstellt haben. Einem einzelnen Azure-Netzwerk können mehrere VM-Netzwerke zugeordnet werden.

## <a name="configure-mapping"></a>Konfigurieren der Zuordnung

Konfigurieren Sie die Zuordnung wie folgt:

1. Klicken Sie unter **Site Recovery-Infrastruktur** > **Netzwerkzuordnungen** > **Netzwerkzuordnung** auf das Symbol **+Netzwerkzuordnung**.

    ![Netzwerkzuordnung](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping1.png)
2. Wählen Sie unter **Netzwerkzuordnung hinzufügen** den VMM-Quellserver und **Azure** als Ziel aus.
3. Überprüfen Sie das Abonnement und das Bereitstellungsmodell nach einem Failover.
4. Wählen Sie unter **Quellnetzwerk**das lokale VM-Quellnetzwerk aus, das Sie über die Liste des VMM-Servers zuordnen möchten.
5. Wählen Sie unter **Zielnetzwerk**das Azure-Netzwerk aus, in dem Replikate von Azure-VMs nach der Erstellung angeordnet werden. Klicken Sie dann auf **OK**.

    ![Netzwerkzuordnung](./media/vmm-to-azure-walkthrough-network-mapping/network-mapping2.png)

Wenn die Netzwerkzuordnung beginnt, passiert Folgendes:

* Vorhandene VMs im VM-Quellnetzwerk werden mit dem Zielnetzwerk verbunden, wenn die Zuordnung beginnt. Neue VMs, die mit dem VM-Quellnetzwerk verbunden sind, werden mit dem zugeordneten Azure-Netzwerk verbunden, wenn die Replikation durchgeführt wird.
* Wenn Sie eine vorhandene Netzwerkzuordnung ändern, verwenden virtuelle Replikatcomputer bei der Verbindungsherstellung die neuen Einstellungen.
* Fall das Zielnetzwerk mehrere Subnetze enthält und eines dieser Subnetze den gleichen Namen besitzt wie das Subnetz des virtuellen Quellcomputers, stellt der virtuelle Replikatcomputer nach dem Failover eine Verbindung mit diesem Zielsubnetz her.
* Ist kein Zielsubnetz mit einem übereinstimmenden Namen vorhanden, stellt der virtuelle Computer eine Verbindung mit dem ersten Subnetz im Netzwerk her.



## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 10: Erstellen einer Replikationsrichtlinie](vmm-to-azure-walkthrough-replication.md).

