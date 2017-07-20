---
title: "Voraussetzungen für die Replikation physischer Server in Azure mithilfe von Azure Site Recovery | Microsoft-Dokumentation"
description: "Fasst die Voraussetzungen für die Replikation von Workloads, die auf physischen Windows/Linux-Servern ausgeführt werden, mithilfe des Azure Site Recovery-Diensts zu Azure zusammen."
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
ms.openlocfilehash: 5f5cb4b9b6fcee6b56ccdcb6223afddd9de90fec
ms.contentlocale: de-de
ms.lasthandoff: 06/29/2017

---

# <a name="step-2-review-the-prerequisites-for-physical-server-to-azure-replication"></a>Schritt 2: Überprüfen der Voraussetzungen für die Replikation physischer Server in Azure

Überprüfen Sie, ob die in der Tabelle zusammengefassten Voraussetzungen erfüllt sind.


**Voraussetzung** | **Details**
--- | ---
**Azure** | Informieren Sie sich über die [Azure-Anforderungen](site-recovery-prereq.md#azure-requirements).
**Lokaler Konfigurationsserver** | Sie benötigen einen physischen Server (oder virtuellen VMware-Computer), auf dem Windows Server 2012 R2 oder höher ausgeführt wird. Sie richten diesen Server während der Site Recovery-Bereitstellung ein.<br/><br/> Standardmäßig werden auch der Prozessserver und der Masterzielserver auf diesem Computer installiert. Beim zentralen Hochskalieren benötigen Sie unter Umständen einen separaten Prozessserver. Hierfür gelten dann die gleichen Anforderungen wie für den Konfigurationsserver.<br/><br/> [Weitere Informationen](site-recovery-set-up-vmware-to-azure.md#configuration-server-minimum-requirements)
**Lokale VMs** | Computer, die Sie replizieren möchten, müssen ein [unterstütztes Betriebssystem](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) verwenden und die [Azure-Voraussetzungen](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) erfüllen.
**URLs** | Der Konfigurationsserver benötigt Zugriff auf diese URLs:<br/><br/> [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]<br/><br/> Stellen Sie bei Verwendung von auf IP-Adressen basierenden Firewallregeln sicher, dass hierfür die Kommunikation mit Azure möglich ist.<br/></br> Lassen Sie die [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-Bereiche für Azure-Rechenzentren) und den HTTPS-Port (443) zu.<br/></br> Lassen Sie IP-Adressbereiche für die Azure-Region Ihres Abonnements und für die Region „USA, Westen“ (wird für Zugriffsteuerung und Identitätsverwaltung verwendet) zu.<br/><br/> Lassen Sie zum Herunterladen von MySQL den Zugriff auf die URL „http://cdn.mysql.com/archives/mysql-5.5/mysql-5.5.37-win32.msi“ zu.
**Mobilitätsdienst** | Ist auf jedem replizierten Server installiert.




## <a name="limitations"></a>Einschränkungen

Beachten Sie vor der Bereitstellung die Einschränkungen, die in der Tabelle zusammengefasst wurden.

**Einschränkung** | **Details**
--- | ---
**Azure** | Speicher- und Netzwerkkonten müssen sich in derselben Region wie der Tresor befinden.<br/><br/> Bei Verwendung eines Storage Premium-Kontos benötigen Sie auch ein Standardspeicherkonto zum Speichern von Replikationsprotokollen.<br/><br/> Die Replikation in Premium-Konten in den Regionen „Indien, Mitte“ und „Indien, Süden“ ist nicht möglich.
**Lokaler Konfigurationsserver** | Wenn Sie einen virtuellen VMware-Computer als Konfigurationsservercomputer verwenden, muss der virtuelle VMware-Computer über den Adaptertyp VMXNET3 verfügen. [Installieren Sie dieses Update](https://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2110245&sliceId=1&docTypeID=DT_KB_1_1&dialogID=26228401&stateId=1), falls dies nicht der Fall ist.<br/><br/> Für einen virtuellen VMware-Computer sollte vSphere PowerCLI 6.0 installiert sein.<br/><br> Der Computer sollte kein Domänencontroller sein.<br/><br/> Der Computer sollte über eine statische IP-Adresse verfügen.<br/><br/> Der Hostname sollte maximal 15 Zeichen lang sein, und es sollte ein Betriebssystem in englischer Sprache verwendet werden.
**Zu replizierende Computer** | Überprüfen Sie die [Einschränkungen für Azure-VMs](site-recovery-prereq.md#azure-requirements).<br/><br/> Es ist nicht möglich, VMs mit verschlüsselten Datenträgern oder VMs mit UEFI/EFI-Startvorgang zu replizieren.<br/><br> Cluster mit freigegebenen Datenträgern werden nicht unterstützt. Wenn auf dem virtuellen Quellcomputer NIC-Teamvorgänge ausgeführt wurden, wird er nach dem Failover in eine einzelne NIC konvertiert.<br/><br/> Falls VMs über einen iSCSI-Datenträger verfügen, wird er von Site Recovery nach dem Failover in eine VHD-Datei konvertiert. Wenn das iSCSI-Ziel für die Azure-VM erreichbar ist, wird eine Verbindung damit hergestellt, und sowohl das Ziel als auch die VHD sind sichtbar. Trennen Sie die Verbindung mit dem iSCSI-Ziel in diesem Fall.<br/><br/> Öffnen Sie Port 20004 auf der VM, falls Sie die Multi-VM-Konsistenz aktivieren möchten. Hierbei können VMs, auf denen die gleiche Workload ausgeführt wird, gemeinsam an einem konsistenten Datenpunkt wiederhergestellt werden.<br/><br/> Windows muss auf dem Laufwerk C installiert sein. Der Betriebssystemdatenträger sollte ein einfacher Datenträger sein (kein dynamischer). Der Datenträger für Daten kann dynamisch sein.<br/><br/> Linux-Dateien vom Typ „/etc/hosts“ auf VMs müssen Einträge enthalten, mit denen der Name des lokalen Hosts den IP-Adressen aller Netzwerkkarten zugeordnet wird. Der Hostname, Bereitstellungspunkte, Gerätename, Systempfade und Dateinamen (z.B. „/etc/“, „/usr“) sollten nur englische Zeichen umfassen.<br/><br/> Bestimmte Arten von [Linux-Speicher](site-recovery-support-matrix-to-azure.md#support-for-storage) werden unterstützt.<br/><br/>Erstellen Sie **disk.enableUUID=true** in den VM-Einstellungen, bzw. legen Sie dies fest. So wird für das VMDK eine konsistente UUID bereitgestellt, damit die Bereitstellung richtig durchgeführt wird. Außerdem wird sichergestellt, dass während des Failbacks nur Deltaänderungen zurück an den lokalen Standort übertragen werden und keine vollständige Replikation durchgeführt wird.


## <a name="next-steps"></a>Nächste Schritte

- Wenn Sie eine vollständige Bereitstellung ausführen, fahren Sie mit [Schritt 3: Planen der Kapazität](physical-walkthrough-capacity.md) fort.
- Wenn Sie eine einfache Testbereitstellung durchführen, fahren Sie mit [Schritt 4: Planen von Netzwerken](physical-walkthrough-network.md) fort.

