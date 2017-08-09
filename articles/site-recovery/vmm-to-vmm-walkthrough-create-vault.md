---
title: "Erstellen eines Tresors für die Hyper-V-Replikation an einem sekundären Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie einen Tresor erstellen, wenn Sie virtuelle Hyper-V-Computer mit Azure Site Recovery an einem sekundären System Center VMM-Standort replizieren."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ff65dbfb-cb26-410e-ab48-76971625db08
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 28cfcf12b2e369f96664c163c0b6f2aa8a6ddcb9
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-5-create-a-vault-for-hyper-v-replication-to-a-secondary-site"></a>Schritt 5: Erstellen eines Tresors für die Hyper-V-Replikation an einem sekundären Standort

Nachdem Sie lokale [System Center Virtual Machine Manager-Server (VMM) und Hyper-V-Hosts/Cluster](vmm-to-vmm-walkthrough-vmm-hyper-v.md) auf die Hyper-V-Replikation an einem sekundären Standort mit [Azure Site Recovery](site-recovery-overview.md) vorbereitet haben, können Sie einen Recovery Services-Tresor erstellen und das Replikationsszenario auswählen.

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.


## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="choose-a-protection-goal"></a>Wählen eines Schutzziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Klicken Sie auf **Site Recovery** > **Schritt 1: Bereiten Sie die Infrastruktur vor** > **Schutzziel**.
2. Wählen Sie **Auf Wiederherstellungsstandort** und anschließend **Yes, with Hyper-V** (Ja, mit Hyper-V) aus.
3. Wählen Sie **Ja** aus, um anzugeben, dass Sie die Hyper-V-Hosts mithilfe von VMM verwalten.
4. Wählen Sie **Ja** aus, falls Sie über einen sekundären VMM-Server verfügen. Wenn Sie die Replikation zwischen Clouds auf einem einzelnen VMM-Server bereitstellen, klicken Sie auf **Nein**. Klicken Sie dann auf **OK**.

    ![Ziele wählen](./media/vmm-to-vmm-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 6: Einrichten von Replikationsquelle und -ziel](vmm-to-vmm-walkthrough-source-target.md).
