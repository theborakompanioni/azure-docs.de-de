---
title: "Einrichten eines Tresors für die Azure-VM-Replikation zwischen Regionen mit Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel werden die erforderlichen Schritte zusammengefasst, um einen Tresor für die Azure-Replikation zwischen Azure-Regionen mit Azure Site Recovery einzurichten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: 40472189-3d80-4963-b175-8bddcbc2f61f
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: c30998a77071242d985737e55a7dc2c0bf70b947
ms.openlocfilehash: e03d17992ee0b12049636e40188950bcc4a6f31e
ms.contentlocale: de-de
ms.lasthandoff: 08/02/2017

---

# <a name="step-4-set-up-a-vault-for-azure-to-azure-replication"></a>Schritt 4: Einrichten eines Tresors für die Replikation zwischen Azure-Regionen

Verwenden Sie nach dem [Planen von Netzwerken](azure-to-azure-walkthrough-network.md) diesen Artikel zum Einrichten eines Tresors, um virtuelle Azure-Computer (VMs) mit dem [Azure Site Recovery](site-recovery-overview.md)-Dienst im Azure-Portal in einer anderen Azure-Region zu replizieren.

- Wenn Sie den Artikel durchgearbeitet haben, sollten Sie einen Recovery Services-Tresor eingerichtet haben.
- Kommentare können Sie am Ende dieses Artikels senden oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) Fragen stellen.



>[!NOTE]
>
> Die Azure-VM-Replikation ist derzeit als Vorschau verfügbar.




## <a name="create-a-vault"></a>Erstellen eines Tresors

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Es wird empfohlen, dass Sie den Recovery Services-Tresor an dem Standort erstellen, an dem Ihre VMs repliziert werden sollen. Wenn Ihr Zielstandort beispielsweise „USA, Mitte“ ist, erstellen Sie den Tresor **USA, Mitte**.


## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 5: Aktivieren der Replikation](azure-to-azure-walkthrough-enable-replication.md).

