---
title: Migrieren von Azure IaaS-VMs zwischen Azure-Regionen | Microsoft-Dokumentation
description: Verwenden Sie Azure Site Recovery, um virtuelle Azure-IaaS-Computer aus einer Azure-Region in eine andere zu migrieren.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: tysonn
ms.assetid: 8a29e0d9-0010-4739-972f-02b8bdf360f6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: ef2972c077a2b1dd2b2fd6ce53cc6560520ea870
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="migrate-azure-iaas-virtual-machines-between-azure-regions-with-azure-site-recovery"></a>Migrieren von virtuellen Azure-IaaS-Computern zwischen Azure-Regionen mit Azure Site Recovery
## <a name="overview"></a>Übersicht
Willkommen bei Azure Site Recovery! Verwenden Sie diesen Artikel, wenn Sie Azure-VMs zwischen Azure-Regionen migrieren möchten. Bevor Sie beginnen, beachten Sie bitte Folgendes:

* Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: Azure Resource Manager- und klassische Bereitstellung. Azure verfügt auch über zwei Portale: das klassische Azure-Portal mit Unterstützung des klassischen Bereitstellungsmodells und das Azure-Portal mit Unterstützung für beide Bereitstellungsmodelle. Die grundlegenden Schritte für die Migration sind bei der Konfiguration von Site Recovery in Resource Manager oder im klassischen Modell identisch. Die UI-Anweisungen und Screenshots in diesem Artikel sind jedoch für das Azure-Portal relevant.
* **Derzeit können Sie nur von einer Region zu einer anderen migrieren. Sie können für virtuelle Computer ein Failover von einer Azure-Region in eine andere durchführen, aber nicht wieder zurück.**

Kommentare oder Fragen können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)veröffentlichen.

## <a name="prerequisites"></a>Voraussetzungen
Für diese Bereitstellung benötigen Sie Folgendes:

* **Virtuelle IaaS-Computer**: die virtuellen Computer, die Sie migrieren möchten. Migrieren Sie diese virtuellen Computer, indem Sie sie wie physische Computer behandeln.

## <a name="deployment-steps"></a>Bereitstellungsschritte
Dieser Abschnitt beschreibt die Bereitstellungsschritte im neuen Azure-Portal.

1. [Erstellen Sie einen Tresor](site-recovery-vmware-to-azure.md).
2. [Aktivieren Sie die Replikation](site-recovery-vmware-to-azure.md). Aktivieren Sie die Replikation für die virtuellen Computer, die Sie migrieren möchten, und wählen Sie Azure als Quelle. 
3. [ Führen Sie ein nicht geplantes Failover durch](site-recovery-failover.md). Nach Abschluss der ersten Replikation können Sie ein nicht geplantes Failover von einer Azure-Region auf eine andere durchführen. Optional können Sie einen Wiederherstellungsplan erstellen und ein nicht geplantes Failover durchführen, um mehrere virtuelle Computer zwischen Regionen zu migrieren. [hier](site-recovery-create-recovery-plans.md) .

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu anderen Replikationsszenarien finden Sie in [Was ist Azure Site Recovery?](site-recovery-overview.md)

