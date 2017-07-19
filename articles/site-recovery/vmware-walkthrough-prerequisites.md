---
title: "Voraussetzungen für die Replikation von VMware in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel werden die Voraussetzungen für die Replikation von Workloads, die auf VMs von VMware ausgeführt werden, in Azure mit dem Azure Site Recovery-Dienst beschrieben."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 318156ba-793b-48d0-98d4-cc5436bf28a3
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: 8dfadb9cc5010e946ac80a15642c7c28d5dd4a4c
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---

# <a name="step-2-review-the-prerequisites-for-vmware-to-azure-replication"></a>Schritt 2: Überprüfen der Voraussetzungen für die Replikation von VMware in Azure

Schauen Sie sich die im folgenden Artikel aufgelisteten Voraussetzungen an.

**Voraussetzung** | **Details**
--- | ---
**Azure** | Informieren Sie sich über die [Azure-Anforderungen](site-recovery-prereq.md#azure-requirements).
**Lokaler Konfigurationsserver** | Sie benötigen eine VMware-VM mit Windows Server 2012 R2 oder höher. Sie richten diesen Server während der Site Recovery-Bereitstellung ein.<br/><br/> Standardmäßig werden auch der Prozessserver und der Masterzielserver auf dieser VM installiert. Wenn Sie zentral hochskalieren, benötigen Sie möglicherweise einen separaten Prozessserver. Es gelten die gleichen Voraussetzungen wie für den Konfigurationsserver.<br/><br/> Weitere Informationen zu diesen Komponenten finden Sie [hier](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements).
**Lokale VMware-Server** | Mindestens einen VMware vSphere-Server mit 6.0, 5.5 oder 5.1 mit den neuesten Updates. Die Server sollten sich in demselben Netzwerk wie der Konfigurationsserver (oder der separate Prozessserver) befinden.<br/><br/> Wir empfehlen Ihnen die Verwendung eines vCenter-Servers zum Verwalten von Hosts, auf dem Version 6.0 oder 5.5 mit den neuesten Updates ausgeführt wird. Bei der Bereitstellung von Version 6.0 werden nur Features unterstützt, die in 5.5 verfügbar sind.
**Lokale VMs** | Auf VMs, die Sie replizieren möchten, sollte ein [unterstütztes Betriebssystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) ausgeführt werden, das die [Azure-Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllt. Auf der VM sollten die VMware-Tools ausgeführt werden.
**URLs** | Der Konfigurationsserver benötigt Zugriff auf diese URLs:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.<br/></br> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.<br/></br> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.<br/><br/> Lassen Sie zum Herunterladen von MySQL den Zugriff auf die URL „http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi“ zu.
**Mobilitätsdienst** | Ist auf jeder replizierten VM installiert.




## <a name="limitations"></a>Einschränkungen

Stellen Sie sicher, dass Sie vor der Bereitstellung die Einschränkungen verstehen, die in der Tabelle dargelegt werden.

**Einschränkung** | **Details**
--- | ---
**Azure** | Speicher- und Netzwerkkonten müssen sich in derselben Region wie der Tresor befinden.<br/><br/> Bei Verwendung eines Storage Premium-Kontos benötigen Sie auch ein Standardspeicherkonto zum Speichern von Replikationsprotokollen.<br/><br/> Die Replikation in Premium-Konten in den Regionen „Indien, Mitte“ und „Indien, Süden“ ist nicht möglich.
**Lokaler Konfigurationsserver** | Der VMware-VM-Adaptertyp sollte „VMXNET3“ lauten. [Installieren Sie dieses Update](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1), falls dies nicht der Fall ist.<br/><br/> vSphere PowerCLI 6.0 sollte installiert sein.<br/><br> Der Computer sollte kein Domänencontroller sein. Der Computer sollte über eine statische IP-Adresse verfügen.<br/><br/> Der Hostname sollte maximal 15 Zeichen lang sein, und es sollte ein Betriebssystem in englischer Sprache verwendet werden.
**VMware** | Nur Features der Version 5.5 werden in vCenter 6.0 unterstützt. Site Recovery unterstützt neue Features von vCenter und vSphere 6.0 nicht. Hierzu zählen etwa Cross vCenter vMotion, Virtual Volumes und Storage DRS.
**VMs** | Überprüfen Sie die [Einschränkungen für Azure-VMs](site-recovery-prereq.md#azure-requirements).<br/><br/> Es ist nicht möglich, VMs mit verschlüsselten Datenträgern oder VMs mit UEFI/EFI-Startvorgang zu replizieren.<br/><br> Cluster mit freigegebenen Datenträgern werden nicht unterstützt. Wenn auf dem virtuellen Quellcomputer NIC-Teamvorgänge ausgeführt wurden, wird er nach dem Failover in eine einzelne NIC konvertiert.<br/><br/> Falls VMs über einen iSCSI-Datenträger verfügen, wird er von Site Recovery nach dem Failover in eine VHD-Datei konvertiert. Wenn das iSCSI-Ziel für die Azure-VM erreichbar ist, wird eine Verbindung damit hergestellt, und sowohl das Ziel als auch die VHD sind sichtbar. Trennen Sie die Verbindung mit dem iSCSI-Ziel in diesem Fall.<br/><br/> Öffnen Sie Port 20004 auf der VM, falls Sie die Multi-VM-Konsistenz aktivieren möchten. Hierbei können VMs, auf denen die gleiche Workload ausgeführt wird, gemeinsam an einem konsistenten Datenpunkt wiederhergestellt werden.<br/><br/> Windows muss auf dem Laufwerk C installiert sein. Der Betriebssystemdatenträger sollte ein einfacher Datenträger sein (kein dynamischer). Der Datenträger für Daten kann dynamisch sein.<br/><br/> Linux-Dateien vom Typ „/etc/hosts“ auf VMs müssen Einträge enthalten, mit denen der Name des lokalen Hosts den IP-Adressen aller Netzwerkkarten zugeordnet wird. Der Hostname, Bereitstellungspunkte, Gerätename, Systempfade und Dateinamen (z.B. „/etc/“, „/usr“) sollten nur englische Zeichen umfassen.<br/><br/> Bestimmte Arten von [Linux-Speicher](site-recovery-support-matrix-to-azure.md#support-for-storage) werden unterstützt.<br/><br/>Erstellen Sie **disk.enableUUID=true** in den VM-Einstellungen, bzw. legen Sie dies fest. So wird für das VMDK eine konsistente UUID bereitgestellt, damit die Bereitstellung richtig durchgeführt wird. Außerdem wird sichergestellt, dass während des Failbacks nur Deltaänderungen zurück an den lokalen Standort übertragen werden und keine vollständige Replikation durchgeführt wird.


## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie eine vollständige Bereitstellung ausführen, fahren Sie mit [Schritt 3: Planen der Kapazität](vmware-walkthrough-capacity.md) fort.
- Wenn Sie eine einfache Testbereitstellung durchführen, fahren Sie mit [Schritt 4: Planen von Netzwerken](vmware-walkthrough-network.md) fort.

