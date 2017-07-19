---
title: Vorbereiten der Azure-Ressourcen zum Replizieren von lokalen physischen Servern in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation
description: "Beschreibt, was Sie direkt in Azure tun müssen, bevor Sie beginnen, lokale Server mithilfe von des Azure Site Recovery-Dienstes in Azure zu replizieren"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 4e320d9b-8bb8-46bb-ba21-77c5d16748ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 2b277f1fb96f347cc60d1395fafb7e2707883a77
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017


---
# <a name="step-5-prepare-azure-resources-for-physical-server-replication-to-azure"></a>Schritt 5: Vorbereiten der Azure-Ressourcen für die Replikation physischer Server in Azure


Verwenden Sie die Anweisungen in diesem Artikel, um Azure-Ressourcen vorzubereiten, damit Sie mithilfe des [Azure Site Recovery](site-recovery-overview.md)-Diensts lokale Server zu Azure replizieren können.

Kommentare und Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.

## <a name="before-you-start"></a>Vorbereitung

Stellen Sie sicher, dass Sie die [Voraussetzungen](physical-walkthrough-prerequisites.md) gelesen haben.

## <a name="set-up-an-azure-account"></a>Einrichten eines Azure-Kontos

- Erstellen Sie ein [Microsoft Azure-Konto](http://azure.microsoft.com/).
- Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/)zur Verfügung.
- Überprüfen Sie die unterstützten Regionen für Site Recovery unter **Geografische Verfügbarkeit** auf der Seite [Site Recovery – Preise](https://azure.microsoft.com/pricing/details/site-recovery/).
- Erfahren Sie mehr über [Site Recovery-Preise](site-recovery-faq.md#pricing), und erhalten die [Preisdetails](https://azure.microsoft.com/pricing/details/site-recovery/).



## <a name="set-up-an-azure-network"></a>Richten Sie ein Azure-Netzwerk ein

- Richten Sie ein Azure-Netzwerk ein. Azure-VMs werden in dieses Netzwerk platziert, wenn sie nach dem Failover erstellt werden.
- Site Recovery im Azure-Portal kann Netzwerke verwenden, die in [Resource Manager](../resource-manager-deployment-model.md) oder im klassischen Modus eingerichtet sind.
- Das Netzwerk muss sich in der gleichen Region befinden wie der Recovery Services-Tresor.
- Informationen zu [Preisen für virtuelle Netzwerke](https://azure.microsoft.com/pricing/details/virtual-network/).
- Erfahren Sie mehr über die [Azure-VM-Konnektivität](physical-walkthrough-network.md) nach einem Failover.


## <a name="set-up-an-azure-storage-account"></a>Richten Sie ein Azure-Speicherkonto ein

- Site Recovery repliziert lokale Server in den Azure-Speicher. Virtuelle Azure-Computer werden nach dem Failover aus dem Speicher erstellt.
- [Richten Sie ein Azure-Speicherkonto ein](../storage/storage-create-storage-account.md#create-a-storage-account), das für replizierte Daten verwendet werden kann.
- Site Recovery kann im Azure-Portal Speicherkonten verwenden, die in Resource Manager oder im klassischen Modus eingerichtet sind.
- Das Speicherkonto kann ein Standard- oder [Premium](../storage/storage-premium-storage.md)-Konto sein.
- Wenn Sie ein Premium-Konto eingerichtet haben, benötigen Sie auch ein zusätzliches Standard-Konto für die Protokolldaten.


## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 6: Einrichten eines Tresors](physical-walkthrough-create-vault.md).

