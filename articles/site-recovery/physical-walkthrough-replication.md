---
title: "Einrichten einer Replikationsrichtlinie für die Replikation physischer Server in Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: Fasst die Schritte zusammen, die zum Einrichten einer Replikationsrichtlinie beim Replizieren lokaler physischer Server in Azure Storage mithilfe des Azure Site Recovery-Diensts erforderlich sind
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
ms.openlocfilehash: 9ce23382001b54e7b9b7a58b8dd9aa61b400826d
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-8-set-up-a-replication-policy-for-physical-server-replication-to-azure"></a>Schritt 8: Erstellen einer Replikationsrichtlinie für die Replikation physischer Server in Azure


In diesem Artikel wird beschrieben, wie Sie bei der Replikation von physischen Windows/Linux-Servern in Azure mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts im Azure-Portal eine Replikationsrichtlinie erstellen.


Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="configure-a-policy"></a>Konfigurieren einer Richtlinie

1. Klicken Sie auf **Site Recovery-Infrastruktur** > **Replikationsrichtlinien** > **+Replikationsrichtlinie**.
2. Geben Sie unter **Replikationsrichtlinie erstellen** einen Richtliniennamen an.
3. Geben Sie unter **RPO-Schwellenwert** den RPO-Grenzwert an. Mit diesem Wert wird angegeben, wie oft Datenwiederherstellungspunkte erstellt werden. Wenn dieser Grenzwert bei der fortlaufenden Replikation überschritten wird, wird eine Warnung generiert.
4. Geben Sie unter **Aufbewahrungszeitraum des Wiederherstellungspunkts** das Aufbewahrungszeitfenster für die einzelnen Wiederherstellungspunkte (in Stunden) an. Replizierte VMs können für jeden Punkt eines Zeitfensters wiederhergestellt werden. Für nach Storage Premium replizierte Computer wird eine Aufbewahrungsdauer von bis zu 24 Stunden unterstützt (72 Stunden für Standardspeicher).
5. Geben Sie unter **App-konsistente Momentaufnahmehäufigkeit**an, wie häufig (in Minuten) Wiederherstellungspunkte erstellt werden sollen, die anwendungskonsistente Momentaufnahmen enthalten. Klicken Sie auf **OK**, um die Richtlinie zu erstellen.

    ![Replikationsrichtlinie](./media/physical-walkthrough-replication/gs-replication2.png)
8. Wenn Sie eine neue Richtlinie erstellen, wird sie dem Konfigurationsserver automatisch zugeordnet. Standardmäßig wird für das Failback automatisch eine passende Richtlinie erstellt. Bei Verwendung der Replikationsrichtlinie **rep-policy** lautet die Failbackrichtlinie beispielsweise **rep-policy-failback**. Diese Richtlinie wird erst verwendet, wenn Sie ein Failback über Azure initiieren.

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 9: Installieren des Mobilitätsdiensts](physical-walkthrough-install-mobility.md).

