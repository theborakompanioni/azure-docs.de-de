---
title: "Vorbereiten von Hyper-V-Hosts (ohne System Center VMM) für die Replikation in Azure | Microsoft-Dokumentation"
description: "Beschreibt das Vorbereiten von Hyper-V-Hosts für die Replikation in Azure mithilfe von Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0f204e24-8d78-4076-95c5-8137d1be9c01
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: e61708f4af0715eaa7147e9cd17113d5979dcdf7
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="step-6-prepare-hyper-v-hosts-for-replication-to-azure"></a>Schritt 6: Vorbereiten von Hyper-V-Hosts für die Replikation in Azure

Verwenden Sie die Anweisungen in diesem Artikel, um lokale Hyper-V-Hosts für die Interaktion mit Azure Site Recovery vorzubereiten.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="prepare-hosts"></a>Vorbereiten von Hosts

- Stellen Sie sicher, dass die Hyper-V-Hosts die [Voraussetzungen](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager) erfüllen.
- Stellen Sie sicher, dass die Hosts auf die erforderlichen URLs zugreifen können.

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.
- Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.
- Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.

Während der Site Recovery-Bereitstellung fügen Sie Hyper-V-Hosts hinzu, die virtuelle Computer enthalten, die an einem Hyper-V-Standort repliziert werden sollen. Der Site Recovery-Anbieter und der Recovery Services-Agent werden auf jedem Host installiert. Der Hyper-V-Standort wird im Recovery Services-Tresor registriert.

## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 7: Erstellen eines Tresors](hyper-v-site-walkthrough-create-vault.md).


