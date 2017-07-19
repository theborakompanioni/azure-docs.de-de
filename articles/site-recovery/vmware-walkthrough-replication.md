---
title: "Aufstellen einer Replikationsrichtlinie für die VM-Replikation von VMware in Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Schritte zusammengefasst, die Sie zum Aufstellen einer Replikationsrichtlinie beim Replizieren von VMware-VMs im Azure-Speicher benötigen."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d7874bd8-6626-4668-9ec9-dbd2d26f8f81
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 3c4b7ad16e6a03fb605447def18f7475d502fdd1
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-9-set-up-a-replication-policy-for-vmware-vm-replication-to-azure"></a>Schritt 9: Aufstellen einer Replikationsrichtlinie für die VM-Replikation von VMware in Azure


In diesem Artikel wird beschrieben, wie Sie bei der Replikation von VMs von VMware in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal eine Replikationsrichtlinie erstellen.


Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="configure-a-policy"></a>Konfigurieren einer Richtlinie

Bevor Sie beginnen, verschaffen Sie sich schnell einen Überblick per Video:
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video2-vCenter-Server-Discovery-and-Replication-Policy/player]

1. Klicken Sie zum Erstellen einer neuen Replikationsrichtlinie auf **Site Recovery-Infrastruktur** > **Replikationsrichtlinien** > **+Replikationsrichtlinie**.
2. Geben Sie unter **Replikationsrichtlinie erstellen** einen Richtliniennamen an.
3. Geben Sie unter **RPO-Schwellenwert** den RPO-Grenzwert an. Mit diesem Wert wird angegeben, wie oft Datenwiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte (in Stunden) an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden. Für nach Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt (72 Stunden für Standardspeicher).
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit**an, wie häufig (in Minuten) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Klicken Sie auf **OK**, um die Richtlinie zu erstellen.

    ![Replikationsrichtlinie](./media/vmware-walkthrough-replication/gs-replication2.png)
8. Wenn Sie eine neue Richtlinie erstellen, wird sie dem Konfigurationsserver automatisch zugeordnet. Standardmäßig wird für das Failback automatisch eine passende Richtlinie erstellt. Bei Verwendung der Replikationsrichtlinie **rep-policy** lautet die Failbackrichtlinie beispielsweise **rep-policy-failback**. Diese Richtlinie wird erst verwendet, wenn Sie ein Failback über Azure initiieren.

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 10: Installieren des Mobilitätsdiensts](vmware-walkthrough-install-mobility.md).

