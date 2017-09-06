---
title: "Voraussetzungen für die Replikation in Azure mithilfe von Azure Site Recovery | Microsoft Docs"
description: "In diesem Artikel werden die Voraussetzungen für die Replikation von virtuellen und physischen Computern in Azure mithilfe des Azure Site Recovery-Diensts zusammengefasst."
services: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: jwhit
editor: tysonn
ms.assetid: e24eea6c-50a7-4cd5-aab4-2c5c4d72ee2d
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/01/2017
ms.author: rajanaki
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: fb5b8c9ac96ac44d0112919664a177f33ef392da
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---

#  <a name="prerequisites-for-replicating-azure-virtual-machines-to-another-region-by-using-azure-site-recovery"></a>Voraussetzungen für die Replikation von virtuellen Azure-Computern in einer anderen Region mithilfe von Azure Site Recovery

> [!div class="op_single_selector"]
> * [Replikation von Azure nach Azure](site-recovery-azure-to-azure-prereq.md)
> * [Replikation von lokal nach Azure](site-recovery-prereq.md)

Der Azure Site Recovery-Dienst unterstützt Ihre Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR) durch die Orchestrierung von Folgendem:
* Replikation von virtuellen Azure-Computern in einer anderen Azure-Region
* Replikation von lokalen physischen Servern und virtuellen Computern in Azure oder in einem sekundären Rechenzentrum 

Wenn es an Ihrem primären Standort zu Ausfällen kommt, können Sie ein Failover zu einem sekundären Standort durchführen, um die Verfügbarkeit von Apps und Workloads zu erhalten. Wenn wieder Normalbetrieb herrscht, können Sie das Failback zum primären Standort durchführen. Weitere Informationen zu Site Recovery finden Sie unter [Was ist Site Recovery?](site-recovery-overview.md).

Dieser Artikel fasst die Voraussetzungen für die Replikation von lokalen Servern nach Azure mit Site Recovery zusammen.

Nach der Lektüre dieses Artikels können Sie Kommentare am Ende des Artikels oder technische Fragen im [Azure Recovery Services-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) veröffentlichen.


## <a name="azure-requirements"></a>Anforderungen für Azure

**Anforderung** | **Details**
--- | ---
**Azure-Konto** | Ein [Microsoft Azure](http://azure.microsoft.com/) -Konto.<br/><br/> Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung.
**Site Recovery-Dienst** | Weitere Informationen zu den Preisen von Site Recovery finden Sie unter [Site Recovery-Preise](https://azure.microsoft.com/pricing/details/site-recovery/). Es wird empfohlen, einen Recovery Services-Tresor in der Azure-Zielregion zu erstellen, die als Standort für die Notfallwiederherstellung verwendet werden soll. Wenn Ihre Quell-VMs beispielsweise im Osten der USA ausgeführt werden und Sie in der Mitte der USA replizieren möchten, sollten Sie den Tresor „USA, Mitte“ erstellen.|
**Azure-Kapazität** | Für die Azure-Zielregion, die Sie als Standort für die Notfallwiederherstellung verwenden möchten, müssen Sie ein Abonnement mit ausreichender Kapazität für virtuelle Computer, Speicherkonten und Netzwerkkomponenten besitzen. Wenden Sie sich an den Support, wenn Sie die Kapazität erweitern möchten.
**Leitfaden für Speicher** | Halten Sie sich an den [Leitfaden für Speicher](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) für virtuelle Azure-Quellcomputer, um Leistungsprobleme zu vermeiden. Wenn Sie die Standardeinstellungen übernehmen, erstellt Site Recovery die erforderlichen Speicherkonten auf Basis der Quellkonfiguration. Wenn Sie Ihre eigenen Einstellungen anpassen und verwenden möchten, achten Sie darauf, die [Skalierbarkeitsziele für Datenträger virtueller Computer](../storage/common/storage-scalability-targets.md#scalability-targets-for-virtual-machine-disks) einzuhalten.
**Leitfaden für Netzwerke** | Sie müssen für bestimmte URLs oder IP-Bereiche ausgehende Verbindungen von Ihrer Azure-VM zur Whitelist hinzufügen. Weitere Informationen finden Sie im Artikel [Leitfaden für Netzwerke zum Replizieren von virtuellen Azure-Computern](site-recovery-azure-to-azure-networking-guidance.md).
**Azure-VM** | Stellen Sie sicher, dass die neuesten Stammzertifikate auf Windows- oder Linux-VMs vorhanden sind. Wenn die neuesten Stammzertifikate nicht vorhanden sind, kann die VM aufgrund von Sicherheitseinschränkungen nicht bei Site Recovery registriert werden.

>[!NOTE]
>Weitere Informationen zur Unterstützung bestimmter Konfigurationen finden Sie in der [Supportmatrix](site-recovery-support-matrix-azure-to-azure.md).

## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie mehr über [Netzwerkrichtlinien zum Replizieren von virtuellen Azure-Computern](site-recovery-azure-to-azure-networking-guidance.md).
- Schützen Sie Ihre Workloads durch die [Replikation virtueller Azure-Computer](site-recovery-azure-to-azure.md).

