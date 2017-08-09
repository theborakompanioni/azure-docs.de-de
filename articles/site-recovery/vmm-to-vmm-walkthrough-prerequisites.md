---
title: "Überprüfen der Voraussetzungen für die Hyper-V-Replikation an einem sekundären VMM-Standort mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Beschreibt die Voraussetzungen zum Replizieren virtueller Hyper-V-Computer an einem sekundären VMM-Standort mit Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 21ff0545-8be5-4495-9804-78ab6e24add6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 7897a30bf1774609ca8e6037dabcd5fbf4151271
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="step-2-review-the-prerequisites-and-limitations-for-hyper-v-vm-replication-to-a-secondary-vmm-site"></a>Schritt 2: Überprüfen der Voraussetzungen und Einschränkungen für die Replikation virtueller Hyper-V-Computer an einem sekundären VMM-Standort


Nachdem Sie die [Szenarioarchitektur](vmm-to-vmm-walkthrough-architecture.md) überprüft haben, lesen Sie diesen Artikel, und machen Sie sich mit den Voraussetzungen für die Bereitstellung vertraut, die beim Replizieren von lokalen, in System Center Virtual Machine Manager-Clouds (VMM) verwalteten virtuellen Hyper-V-Computern an einem sekundären Standort mithilfe von [Azure Site Recovery](site-recovery-overview.md) im Azure-Portal gelten.

Kommentare können Sie am Ende dieses Artikels oder im [Forum zu Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) hinterlassen.


## <a name="prerequisites-and-limitations"></a>Voraussetzungen und Einschränkungen

**Anforderung** | **Details**
--- | ---
**Azure** | Ein [Microsoft Azure](http://azure.microsoft.com/)-Abonnement<br/><br/> Für den Einstieg steht eine [kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) zur Verfügung.<br/><br/> [hier](https://azure.microsoft.com/pricing/details/site-recovery/) .<br/><br/> Konsultieren Sie die unterstützten Regionen für Site Recovery unter „Geografische Verfügbarkeit“ auf der Seite [Site Recovery – Preise](https://azure.microsoft.com/pricing/details/site-recovery/).
**VMM-Server** | Wir empfehlen die Verwendung von zwei VMM-Servern: einer am primären und einer am sekundären Standort.<br/><br/> Die cloudübergreifende Replikation auf einem einzelnen VMM-Server wird unterstützt.<br/><br/> Auf den VMM-Servern muss mindestens System Center 2012 SP1 mit den neuesten Updates ausgeführt werden.<br/><br/> VMM-Server benötigen Zugriff auf das Internet.
**VMM-Clouds** | Jeder VMM-Server muss über mindestens eine Cloud verfügen, und für alle Clouds muss das Hyper-V-Kapazitätsprofil festgelegt sein. <br/><br/>Die Clouds müssen mindestens eine VMM-Hostgruppe enthalten.<br/><br/> Wenn Sie nur über einen einzelnen VMM-Server verfügen, benötigt dieser mindestens zwei Clouds (primär und sekundär).
**Hyper-V** | Auf den Hyper-V-Servern muss mindestens Windows Server 2012 mit der Hyper-V-Rolle ausgeführt werden, und die neuesten Updates müssen installiert sein.<br/><br/> Ein Hyper-V-Server muss mindestens einen virtuellen Computer enthalten.<br/><br/>  Hyper-V-Hostserver müssen sich in Hostgruppen in der primären und in der sekundären VMM-Cloud befinden.<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 R2 ausführen, installieren Sie das [Update 2961977](https://support.microsoft.com/kb/2961977).<br/><br/> Wenn Sie Hyper-V in einem Cluster unter Windows Server 2012 ausführen und über einen auf statischen IP-Adressen basierenden Cluster verfügen, wird der Clusterbroker nicht automatisch erstellt. Konfigurieren Sie den Clusterbroker manuell. [Weitere Informationen](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx)<br/><br/> Hyper-V-Server benötigen Zugriff auf das Internet.




## <a name="next-steps"></a>Nächste Schritte

Wechseln Sie zu [Schritt 3: Planen von Netzwerken](vmm-to-vmm-walkthrough-network.md).

