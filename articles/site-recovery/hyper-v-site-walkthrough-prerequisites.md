---
title: "Überprüfen der Voraussetzungen für die Replikation von Hyper-V in Azure (ohne System Center VMM) mit Azure Site Recovery | Microsoft-Dokumentation"
description: Beschreibt die Voraussetzungen zum Einrichten von Replikation, Failover und Wiederherstellung von lokalen Hyper-V-Computern in Azure mit Azure Site Recovery
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 7ef3fb46-52f5-4c8a-b1a1-658c2305762a
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 31ecec607c78da2253fcf16b3638cc716ba3ab89
ms.openlocfilehash: 63ad1dcc5a069a9958e56c8260f9aa208fafc645
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---

# <a name="step-2-review-the-prerequisites-for-hyper-v-without-vmm-to-azure-replication"></a>Schritt 2: Überprüfen der Voraussetzungen für die Replikation von Hyper-V (ohne VMM) in Azure

Die Voraussetzungen sind in der Tabelle zusammengefasst.


**Voraussetzung** | **Details** 
--- | --- 
**Azure** | Informieren Sie sich über die [Azure-Anforderungen](site-recovery-prereq.md#azure-requirements).
**Lokale Server** | [Erfahren Sie mehr](site-recovery-prereq.md#disaster-recovery-of-hyper-v-virtual-machines-to-azure-no-virtual-machine-manager) zu den Anforderungen für den lokalen VMM-Server und die Hyper-V-Hosts.
**Lokale Hyper-V-VMs** | Auf VMs, die Sie replizieren möchten, sollte ein [unterstütztes Betriebssystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) ausgeführt werden, das die [Azure-Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllt.
**Azure-URLs** | Hyper-V-Hosts benötigen Zugriff auf die folgenden URLs:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.<br/></br> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.<br/></br> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.



## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie eine vollständige Bereitstellung ausführen, fahren Sie mit [Schritt 3: Planen der Kapazität](hyper-v-site-walkthrough-capacity.md) fort.
- Wenn Sie eine einfache Testbereitstellung durchführen, fahren Sie mit [Schritt 4: Planen von Netzwerken](hyper-v-site-walkthrough-network.md) fort.

