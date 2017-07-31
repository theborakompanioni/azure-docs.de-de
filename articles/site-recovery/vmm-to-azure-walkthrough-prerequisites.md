---
title: "Überprüfen der Voraussetzungen für die Replikation von Hyper-V in Azure (mit System Center VMM) mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: Beschreibt die Voraussetzungen zum Einrichten von Replikation, Failover und Wiederherstellung lokaler Hyper-V-Computer in VMM-Clouds in Azure mit Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a1c30fd5-c979-473c-af44-4f725ad3e3ba
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 87a5d721ca785329b407d31126bd0b211b17ccf3
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---



# <a name="step-2-review-the-prerequisites-for-hyper-v-with-vmm-to-azure-replication"></a>Schritt 2: Überprüfen der Voraussetzungen für die Replikation von Hyper-V (mit VMM) in Azure

Machen Sie sich in diesem Artikel nach der Überprüfung der [Szenarioarchitektur](vmm-to-azure-walkthrough-architecture.md) mit den Voraussetzungen für die Bereitstellung vertraut. 

## <a name="prerequisites-and-limitations"></a>Voraussetzungen und Einschränkungen

**Anforderung** | **Details**
--- | ---
**Azure-Konto** | Sie benötigen ein [Microsoft Azure-Konto](http://azure.microsoft.com/).
**Azure-Speicher** | Sie benötigen ein Azure Storage-Konto, um replizierte Daten zu speichern.<br/><br/> Das Speicherkonto muss sich in der gleichen Region befinden wie der Azure Recovery Services-Tresor.<br/><br/>Sie können [georedundanten Speicher](../storage/storage-redundancy.md#geo-redundant-storage) oder lokal redundanten Speicher verwenden. Wir empfehlen georedundanten Speicher. Mit georedundantem Speicher sind Ihre Daten geschützt, wenn es zu einem regionalen Ausfall kommt oder die primäre Region nicht wiederhergestellt werden kann.<br/><br/> Sie können ein standardmäßiges Azure-Speicherkonto oder Azure [Storage Premium](../storage/storage-premium-storage.md) verwenden. Storage Premium eignet sich zum Hosten E/A-intensiver Workloads und wird üblicherweise für virtuelle Computer verwendet, die eine konsistent hohe E/A-Leistung und geringe Wartezeit erfordern. Wenn Sie Storage Premium für replizierte Daten verwenden, benötigen Sie auch ein Standard-Speicherkonto. Ein Standard-Speicherkonto dient zum Speichern von Replikationsprotokollen, in denen laufende Änderungen an lokalen Daten erfasst werden.
**Azure-Netzwerk** | Sie benötigen ein [Azure-Netzwerk](../virtual-network/virtual-network-get-started-vnet-subnet.md), mit dem virtuelle Azure-Computer nach dem Failover eine Verbindung herstellen können. Das Azure-Netzwerk muss sich in der gleichen Region befinden wie der Recovery Services-Tresor.
**Lokale VMM-Server** | Sie benötigen mindestens einen VMM-Server mit System Center 2012 R2 oder einer höheren Version.<br/><br/> Jeder VMM-Server muss über mindestens eine private Cloud verfügen. Jede Cloud benötigt mindestens eine Hostgruppe.<br/><br/> Der VMM-Server muss über Internetzugriff verfügen.
**Lokale Hyper-V-Instanz** | Auf Hyper-V-Hostservern muss mindestens Windows Server 2012 R2 mit der Hyper-V-Rolle oder Microsoft Hyper-V Server 2012 R2 ausgeführt werden. Die neuesten Updates müssen installiert sein.<br/><br/> Der Hyper-V-Host muss sich in einer VMM-Hostgruppe (in einer VMM-Cloud) befinden.<br/><br/> Ein Host muss über mindestens einen virtuellen Computer verfügen, den Sie replizieren möchten.<br/><br/> Hyper-V-Hosts müssen für die Replikation in Azure über eine direkte oder proxybasierte Internetverbindung verfügen. Hyper-V-Server müssen über die im Artikel [2961977](https://support.microsoft.com/kb/2961977) beschriebenen Fehlerbehebungen verfügen.
**Lokale Hyper-V-VMs** | Auf VMs, die Sie replizieren möchten, sollte ein [unterstütztes Betriebssystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) ausgeführt werden, das die [Azure-Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllt. Der Name des virtuellen Computers kann nach der Aktivierung der Replikation geändert werden. 




## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 3: Planen der Kapazität](vmm-to-azure-walkthrough-capacity.md).

