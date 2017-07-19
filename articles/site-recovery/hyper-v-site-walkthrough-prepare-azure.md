---
title: "Vorbereiten von Azure-Ressourcen für die Replikation von virtuellen Hyper-V-Computern (ohne System Center VMM) in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt Voraussetzungen, die in Azure erfüllt sein müssen, bevor Sie beginnen, virtuelle Hyper-V-Computer (ohne VMM) mithilfe von Azure Site Recovery in Azure zu replizieren"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 28fa722c-675e-4637-98eb-7ccbf3806d69
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: fe67a94a2b56fbc72035582f7ee1625b12b21ead
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="step-5-prepare-azure-resources-for-hyper-v-replication-to-azure"></a>Schritt 5: Vorbereiten der Azure-Ressourcen für die Hyper-V-Replikation in Azure

Verwenden Sie die Anweisungen in diesem Artikel, um Azure-Ressourcen vorzubereiten, damit Sie mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts lokale virtuelle Hyper-V-Computer (ohne System Center VMM) zu Azure replizieren können.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="before-you-start"></a>Vorbereitung

Stellen Sie sicher, dass Sie die [Voraussetzungen](hyper-v-site-walkthrough-prerequisites.md) gelesen haben.

## <a name="set-up-an-azure-account"></a>Einrichten eines Azure-Kontos

- Erstellen Sie ein [Microsoft Azure-Konto](http://azure.microsoft.com/).
- Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung.
- Überprüfen Sie die unterstützten Regionen für Site Recovery unter „Geografische Verfügbarkeit“ auf der Seite [Site Recovery – Preise](https://azure.microsoft.com/pricing/details/site-recovery/).
- Erfahren Sie mehr über [Site Recovery-Preise](site-recovery-faq.md#pricing), und erhalten die [Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/).


## <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

- Richten Sie ein Azure-Netzwerk ein. Azure-VMs werden in dieses Netzwerk platziert, wenn sie nach dem Failover erstellt werden.
- Das Netzwerk muss sich in der gleichen Region befinden wie der Recovery Services-Tresor.
- Site Recovery im Azure-Portal kann Netzwerke verwenden, die in [Resource Manager](../resource-manager-deployment-model.md) oder im klassischen Modus eingerichtet sind.
- Wir empfehlen Ihnen, ein Netzwerk einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen.
- Informationen zu [Preisen für virtuelle Netzwerke](https://azure.microsoft.com/pricing/details/virtual-network/).


## <a name="set-up-an-azure-storage-account"></a>Richten Sie ein Azure-Speicherkonto ein

- Site Recovery repliziert lokale Computer in den Azure-Speicher. Virtuelle Azure-Computer werden nach dem Failover aus dem Speicher erstellt.
- Richten Sie ein Standard- oder Premium-[Azure-Speicherkonto](../storage/storage-create-storage-account.md#create-a-storage-account) zum Speichern von Daten, die nach Azure repliziert werden.
- [Premiumspeicher](../storage/storage-premium-storage.md) wird normalerweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Latenz zum Hosten von E/A-intensiven Workloads erfordern.
- Wenn Sie ein Premiumkonto zum Speichern replizierter Daten verwenden möchten, benötigen Sie außerdem ein Standard-Speicherkonto zum Speichern von Replikationsprotokollen, in denen laufende Änderungen lokaler Daten erfasst werden.
- Richten Sie ein Konto im [Resource Manager-Modus](../storage/storage-create-storage-account.md) oder im [klassischen Modus](../storage/storage-create-storage-account-classic-portal.md) ein (je nachdem, welches Ressourcenmodell Sie für virtuelle Azure-Computer nach dem Failover verwenden möchten).
- Es wird empfohlen, ein Speicherkonto einzurichten, bevor Sie beginnen. Falls Sie es nicht tun, müssen Sie diesen Schritt während der Site Recovery-Bereitstellung ausführen. Die Konten müssen sich in der gleichen Region wie der Recovery Services-Tresor befinden.
- Speicherkonten, die von Site Recovery verwendet werden, können nicht über Ressourcengruppen hinweg im gleichen Abonnement oder über verschiedene Abonnements hinweg verschoben werden.


## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 6: Vorbereiten von Hyper-V-Ressourcen](hyper-v-site-walkthrough-prepare-hyper-v.md).

