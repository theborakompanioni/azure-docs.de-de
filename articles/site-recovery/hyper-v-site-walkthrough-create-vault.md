---
title: "Einrichten eines Tresors für die Hyper-V-Replikation (ohne System Center VMM) in Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel ist zusammengefasst, wie Sie einen Tresor für die Hyper-V-Replikation in Azure mit Azure Site Recovery einrichten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a936b3e2-0dbe-47ac-a98e-5285d96dc786
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: 8212ff011633c3a89d3310e828b6d5f1cda6ce3f
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

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
3. Wählen Sie unter **Schutzziel** die Option **To Azure** >  (Zu Azure) und dann **Yes, with Hyper-V** (Ja, mit Hyper-V) aus. Wählen Sie **Nein** aus, um zu bestätigen, dass Sie VMM nicht verwenden. 

    ![Ziele wählen](./media/hyper-v-site-walkthrough-create-vault/choose-goals2.png)



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 8: Einrichten der Quelle und des Ziels](hyper-v-site-walkthrough-source-target.md).

