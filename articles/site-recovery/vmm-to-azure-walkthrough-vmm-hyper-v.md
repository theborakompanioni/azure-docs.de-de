---
title: "Vorbereiten von System Center VMM für die Hyper-V-Replikation nach Azure | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt das Vorbereiten des System Center VMM-Servers für die Hyper-V-Replikation nach Azure mithilfe von Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: afcd81ae-d192-4013-a0af-3dac45b3c7e9
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: ec118ed837dbf140083b3ae1e4ecd41c81562018
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="step-6-prepare-vmm-servers-and-hyper-v-hosts-for-hyper-v-replication-to-azure"></a>Schritt 6: Vorbereiten von VMM-Servern und Hyper-V-Hosts für die Hyper-V-Replikation nach Azure

Nachdem Sie die [Azure-Komponenten](vmm-to-azure-walkthrough-prepare-azure.md) für die Bereitstellung eingerichtet haben, verwenden Sie die Anweisungen in diesem Artikel, um lokale VMM-Server und Hyper-V-Hosts auf die Interaktion mit Azure Site Recovery vorzubereiten.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="prepare-vmm-servers"></a>Vorbereiten der VMM-Server

- Sie benötigen mindestens einen VMM-Server, der die Unterstützungsanforderungen für die Site Recovery-Replikation erfüllt (site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- Stellen Sie sicher, dass der VMM-Server auf die [Netzwerkzuordnung](vmm-to-azure-walkthrough-network.md#network-mapping-for-replication-to-azure) vorbereitet ist.
- Stellen Sie sicher, dass der VMM-Server auf diese URLs zugreifen kann.

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.
- Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.
- Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.

Während der Site Recovery-Bereitstellung laden Sie den Site Recovery-Anbieter herunter und installieren ihn auf allen VMM-Servern. Der VMM-Server wird im Recovery Services-Tresor registriert.




## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 7: Erstellen eines Tresors](vmm-to-azure-walkthrough-create-vault.md).


