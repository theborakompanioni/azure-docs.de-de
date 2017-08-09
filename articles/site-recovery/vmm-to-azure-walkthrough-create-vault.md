---
title: "Einrichten eines Tresors für die Hyper-V-Replikation (mit System Center VMM) in Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel ist zusammengefasst, wie Sie einen Tresor für die Hyper-V-Replikation (mit VMM) in Azure mit Azure Site Recovery einrichten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: b3cd6f03-c33c-406d-91d4-5cba69f79abd
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: af453ec27ba15ad8c59cf9f544584ad18dc0f74a
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="step-7-set-up-a-vault-for-hyper-v-replication"></a>Schritt 7: Einrichten eines Tresors für die Hyper-V-Replikation

In diesem Artikel wird beschrieben, wie Sie einen Tresor erstellen und angeben, was Sie von Ihrem lokalen Standort mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst im Azure-Portal in Azure replizieren möchten.


Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]



## <a name="select-a-protection-goal"></a>Wählen eines Schutzziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Klicken Sie auf **Recovery Services-Tresore** > „Tresor“.
2. Klicken Sie im Ressourcenmenü auf **Site Recovery** > **Infrastruktur vorbereiten** > **Schutzziel**.
3. Wählen Sie unter **Schutzziel** die Option **To Azure** >  (Zu Azure) und dann **Yes, with Hyper-V** (Ja, mit Hyper-V) aus. Wählen Sie **Ja** aus, um zu bestätigen, dass Sie VMM verwenden. 

     ![Ziele wählen](./media/vmm-to-azure-walkthrough-create-vault/choose-goals.png)



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 8: Einrichten der Quelle und des Ziels](vmm-to-azure-walkthrough-source-target.md).

