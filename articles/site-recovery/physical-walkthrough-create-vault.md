---
title: "Einrichten eines Tresors für die Replikation physischer Server in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: Fasst die Schritte zum Einrichten eines Tresors zur Replikation physischer Server in Azure mithilfe von Azure Site Recovery zusammen
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d99f2605-f417-4995-be77-5323136b814f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: deb5ad0495edc969b374795eeb2698326dd4ff4d
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-6-set-up-a-vault-for-physical-server-replication-to-azure"></a>Schritt 6: Einrichten eines Tresors für die Replikation physischer Server in Azure


In diesem Artikel wird beschrieben, wie Sie einen Tresor einrichten. Sie erstellen den Tresor und geben an, was Sie von Ihrem lokalen Standort mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst im Azure-Portal in Azure replizieren möchten.


Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.




## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Wählen eines Schutzziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Klicken Sie auf **Recovery Services-Tresore** > „Tresor“.
2. Klicken Sie im Ressourcenmenü auf **Site Recovery** > **Infrastruktur vorbereiten** > **Schutzziel**.
3. Wählen Sie unter **Schutzziel** die Option **In Azure** > **Nicht virtualisiert/Andere** aus.


## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 7: Quelle und Ziel austauschen](physical-walkthrough-source-target.md).

