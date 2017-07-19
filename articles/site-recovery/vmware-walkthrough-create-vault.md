---
title: "Voraussetzungen für die VMware-Replikation in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel ist zusammengefasst, wie Sie einen Tresor für die VMware-Replikation in Azure mit Azure Site Recovery einrichten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 8bce940e-f19f-4418-8360-aee7b073519a
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: dca95ad46b8de587140c3573ba6ed5702a122032
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-7-set-up-a-vault-for-vmware-replication-to-azure"></a>Schritt 7: Einrichten eines Tresors für die VMware-Replikation in Azure


In diesem Artikel wird beschrieben, wie Sie einen Tresor erstellen und angeben, was Sie von Ihrem lokalen Standort mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst im Azure-Portal in Azure replizieren möchten.


Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.




## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Wählen eines Schutzziels

Wählen Sie aus, was Sie replizieren möchten und wohin die Daten repliziert werden sollen.

1. Klicken Sie auf **Recovery Services-Tresore** > „Tresor“.
2. Klicken Sie im Ressourcenmenü auf **Site Recovery** > **Infrastruktur vorbereiten** > **Schutzziel**.
3. Wählen Sie unter **Schutzziel** die Option **To Azure** (Zu Azure) > **Yes, with VMware vSphere Hypervisor** (Ja, mit VMware vSphere-Hypervisor) aus.



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 8: Einrichten der Quelle und des Ziels](vmware-walkthrough-source-target.md).

