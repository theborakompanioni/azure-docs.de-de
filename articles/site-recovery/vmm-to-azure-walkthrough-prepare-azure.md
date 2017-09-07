---
title: "Vorbereiten von Azure-Ressourcen für die Replikation von virtuellen Hyper-V-Computern (mit System Center VMM) in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt Voraussetzungen, die in Azure erfüllt sein müssen, bevor Sie beginnen, virtuelle Hyper-V-Computer (mit VMM) mithilfe von Azure Site Recovery in Azure zu replizieren."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 1568bdc3-e767-477b-b040-f13699ab5644
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 63b005f37ab5e15e8a1b4645446d65f1529f1bbd
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-with-vmm-to-azure"></a>Schritt 5: Vorbereiten der Azure-Ressourcen für die Hyper-V-Replikation (mit VMM) in Azure

Nach dem Überprüfen der [Netzwerkanforderungen](vmm-to-azure-walkthrough-network.md) bereiten Sie anhand der Anweisungen in diesem Artikel die Azure-Ressourcen vor, damit Sie mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts lokale virtuelle Hyper-V-Computer in System Center Virtual Machine Manager-Clouds (VMM) in Azure replizieren können.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="set-up-an-azure-account"></a>Einrichten eines Azure-Kontos

- Erstellen Sie ein [Microsoft Azure-Konto](http://azure.microsoft.com/).
- Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung.
- Konsultieren Sie die unterstützten Regionen für Site Recovery unter „Geografische Verfügbarkeit“ auf der Seite [Site Recovery – Preise](https://azure.microsoft.com/pricing/details/site-recovery/).
- Erfahren Sie mehr über [Site Recovery-Preise](site-recovery-faq.md#pricing), und erhalten Sie [Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/).
- Stellen Sie sicher, dass Ihr Azure-Konto über die entsprechenden [Berechtigungen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) zum Erstellen virtueller Azure-Computer verfügt. [Weitere Informationen](../active-directory/role-based-access-built-in-roles.md) zur rollenbasierten Zugriffssteuerung in Azure.


## <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

- Richten Sie ein [Azure-Netzwerk](../virtual-network/virtual-network-get-started-vnet-subnet.md) ein. Azure-VMs werden in dieses Netzwerk platziert, wenn sie nach dem Failover erstellt werden.
- Das Netzwerk muss sich in der gleichen Region befinden wie der Recovery Services-Tresor.
- Site Recovery im Azure-Portal kann Netzwerke verwenden, die in [Resource Manager](../resource-manager-deployment-model.md) oder im klassischen Modus eingerichtet sind.
- Wir empfehlen Ihnen, ein Netzwerk einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen.
- Informationen zu [Preisen für virtuelle Netzwerke](https://azure.microsoft.com/pricing/details/virtual-network/).


## <a name="set-up-an-azure-storage-account"></a>Richten Sie ein Azure-Speicherkonto ein

- Site Recovery repliziert lokale Computer in den Azure-Speicher. Virtuelle Azure-Computer werden nach dem Failover aus dem Speicher erstellt.
- Richten Sie ein Standard- oder Premium-[Azure-Speicherkonto](../storage/common/storage-create-storage-account.md#create-a-storage-account) zum Speichern von Daten, die nach Azure repliziert werden.
- [Premiumspeicher](../storage/common/storage-premium-storage.md) wird normalerweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Latenz zum Hosten von E/A-intensiven Workloads erfordern.
- Wenn Sie ein Premiumkonto zum Speichern replizierter Daten verwenden möchten, benötigen Sie außerdem ein Standard-Speicherkonto zum Speichern von Replikationsprotokollen, in denen laufende Änderungen lokaler Daten erfasst werden.
- Richten Sie ein Konto im [Resource Manager-Modus](../storage/common/storage-create-storage-account.md) oder im [klassischen Modus](../storage/common/storage-create-storage-account.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
- Es wird empfohlen, ein Speicherkonto einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen. Die Konten müssen sich in der gleichen Region wie der Recovery Services-Tresor befinden.
- Speicherkonten, die von Site Recovery verwendet werden, können nicht über Ressourcengruppen hinweg im gleichen Abonnement oder über verschiedene Abonnements hinweg verschoben werden.


## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 6: Vorbereiten von VMM](vmm-to-azure-walkthrough-vmm-hyper-v.md).

