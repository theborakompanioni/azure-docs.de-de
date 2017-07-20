---
title: " Verwalten eines VMware vCenter Servers in Azure Site Recovery | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie VMware vCenter in Azure Site Recovery hinzufügen und verwalten."
services: site-recovery
documentationcenter: 
author: AnoopVasudavan
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: backup-recovery
ms.date: 06/29/2017
ms.author: anoopkv
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 091f0884417535427c52beee7bcdc5ed1dd83315
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---

# <a name="manage-vmware-vcenter-server-in-azure-site-recovery"></a>Verwalten eines VMware vCenter Servers in Azure Site Recovery
Dieser Artikel beschreibt die verschiedenen Site Recovery-Vorgänge, die in einem VMware vCenter ausgeführt werden können.

## <a name="prerequisites"></a>Voraussetzungen

**Unterstützung von VMware vCenter und des VMware vSphere-ESX-Hosts** | **Details** |
|--- | --- |
|**Lokale VMware-Server** | Mindestens einen VMware vSphere-Server mit 6.0, 5.5 oder 5.1 mit den neuesten Updates. Die Server sollten sich in demselben Netzwerk wie der Konfigurationsserver (oder der separate Prozessserver) befinden.<br/><br/> Wir empfehlen Ihnen die Verwendung eines vCenter-Servers zum Verwalten von Hosts, auf dem Version 6.0 oder 5.5 mit den neuesten Updates ausgeführt wird. Bei der Bereitstellung von Version 6.0 werden nur Features unterstützt, die in 5.5 verfügbar sind.|

## <a name="prepare-an-account-for-automatic-discovery"></a>Vorbereiten eines Kontos für die automatische Ermittlung
Für Site Recovery ist der Zugriff auf VMware erforderlich, damit der Prozessserver VMs automatisch ermitteln und das Failover und Failback von VMs durchgeführt werden kann.

* **Migrieren**: Wenn Sie nur VMware-VMs zu Azure migrieren möchten, ohne jemals ein Failback durchzuführen, können Sie ein VMware-Konto mit einer schreibgeschützten Rolle verwenden. Mit einer Rolle dieser Art kann ein Failover durchgeführt werden, aber es ist nicht möglich, geschützte Quellcomputer herunterzufahren. Dies ist für die Migration nicht erforderlich.
* **Replizieren/Wiederherstellen**: Wenn Sie die vollständige Replikation (Replizieren, Failover, Failback) bereitstellen möchten, müssen mit dem Konto bestimmte Vorgänge möglich sein, z.B. das Erstellen und Entfernen von Datenträgern, das Aktivieren von VMs usw.
* **Automatische Ermittlung**: Mindestens ein schreibgeschütztes Konto ist erforderlich.


|**Aufgaben** | **Erforderliches Konto bzw. Rolle** | **Berechtigungen** | **Details**|
|--- | --- | --- | ---|
|**Prozessserver ermittelt VMware-VMs automatisch** | Sie benötigen mindestens einen Benutzer mit Lesezugriff. | Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat Zugriff auf alle Objekte im Rechenzentrum.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.|
|**Failover** | Sie benötigen mindestens einen Benutzer mit Lesezugriff. | Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat Zugriff auf alle Objekte im Rechenzentrum.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.<br/><br/> Ist für Migrationszwecke geeignet, aber nicht für die vollständige Replikation, Failover oder Failback.|
|**Failover und Failback** | Wir empfehlen, dass Sie eine Rolle (AzureSiteRecoveryRole) mit den erforderlichen Berechtigungen erstellen und sie dann einem VMware-Benutzer oder einer VMware-Gruppe zuweisen | Rechenzentrumsobjekt -> An untergeordnetes Objekt weitergeben, Rolle=AzureSiteRecoveryRole<br/><br/> Datenspeicher -> Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren<br/><br/> Netzwerk -> Netzwerk zuweisen<br/><br/> Ressource -> Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren<br/><br/> Tasks (Aufgaben) -> Create task (Aufgabe erstellen), update task (Aufgabe aktualisieren)<br/><br/> Virtueller Computer -> Konfiguration<br/><br/> Virtueller Computer -> Interagieren -> Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren<br/><br/> Virtueller Computer -> Inventar -> Erstellen, Registrieren, Registrierung aufheben<br/><br/> Virtueller Computer -> Bereitstellung -> Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen<br/><br/> Virtual machine -> Snapshots -> Remove snapshots | Der Benutzer wird auf Rechenzentrumsebene zugewiesen und hat Zugriff auf alle Objekte im Rechenzentrum.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.|

## <a name="create-an-account-to-connect-to-vmware-vcenter-server-vmware-vsphere-exsi-host"></a>Erstellen eines Kontos für das Herstellen der Verbindung mit VMware vCenter-Server / VMware vSphere-EXSi-Host
1. Melden Sie sich beim Konfigurationsserver an, und starten Sie „cspsconfigtool.exe“ über die Verknüpfung auf dem Desktop.
2. Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.

  ![add-account](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Geben Sie die Kontodetails ein, und klicken Sie auf OK, um das Konto hinzuzufügen. Das Konto muss über die Berechtigungen verfügen, die im Abschnitt [Vorbereiten eines Kontos für die automatische Ermittlung](#prepare-an-account-for-automatic-discovery) aufgelistet sind.

  >[!NOTE]
  Es dauert ungefähr 15 Minuten, bis die Kontoinformationen mit dem Site Recovery-Dienst synchronisiert sind.


## <a name="associate-a-vmware-vcenter-vmware-vsphere-esx-host-add-vcenter"></a>Zuordnen eines VMware vCenters / VMware vSphere-ESX-Hosts (vCenter hinzufügen)
* Navigieren Sie im Azure-Portal zu *Ihr_Recovery_Services-Tresor* > **Site Recovery-Infrastruktur** > **Konfigurationsserver** > *Konfigurationsserver*.
* Klicken Sie auf der Detailsseite des Konfigurationsservers auf die Schaltfläche „+vCenter“.

[!INCLUDE [site-recovery-add-vcenter](../../includes/site-recovery-add-vcenter.md)]

## <a name="modify-credentials-used-to-connect-to-the-vcenter-server-vsphere-esxi-host"></a>Ändern von Anmeldeinformationen zum Herstellen der Verbindung mit dem vCenter-Server / vSphere ESXi-Host

1. Melden Sie sich beim Konfigurationsserver an, und rufen Sie die Datei „cspsconfigtool.exe“ auf.
2. Klicken Sie auf der Registerkarte **Konten verwalten** auf **Konto hinzufügen**.

  ![add-account](./media/site-recovery-vmware-to-azure-manage-vcenter/addaccount.png)
3. Geben Sie die neuen Kontodetails ein, und klicken Sie auf OK, um das Konto hinzuzufügen. Das Konto muss über die Berechtigungen verfügen, die im Abschnitt [Vorbereiten eines Kontos für die automatische Ermittlung](#prepare-an-account-for-automatic-discovery) aufgelistet sind.
4. Navigieren Sie im Azure-Portal zu *Ihr_Recovery_Services-Tresor* > **Site Recovery-Infrastruktur** > **Konfigurationsserver** > *Konfigurationsserver*.
5. Klicken Sie auf der Detailsseite des Konfigurationsservers auf die Schaltfläche **Server aktualisieren**.
6. Wählen Sie nach Abschluss des Serveraktualisierungsauftrags den vCenter-Server, um die vCenter-Zusammenfassungsseite zu öffnen.
7. Wählen Sie das neu hinzugefügte Konto im Feld **vCenter-Server/vSphere-Hostkonto**, und klicken Sie auf die Schaltfläche **Speichern**.

  ![modify-account](./media/site-recovery-vmware-to-azure-manage-vcenter/modify-vcente-creds.png)

## <a name="delete-a-vcenter-in-azure-site-recovery"></a>Löschen eines vCenters in Azure Site Recovery
1. Navigieren Sie im Azure-Portal zu *Ihr_Recovery_Services-Tresor* > **Site Recovery-Infrastruktur** > **Konfigurationsserver** > *Konfigurationsserver*.
2. Wählen Sie auf der Detailsseite des Konfigurationsservers den vCenter-Server, um die vCenter-Zusammenfassungsseite zu öffnen.
3. Klicken Sie auf die Schaltfläche **Löschen**, um das vCenter zu löschen.

  ![delete-account](./media/site-recovery-vmware-to-azure-manage-vcenter/delete-vcenter.png)

> [!NOTE]
Wenn Sie Änderungen an IP-Adresse/FQDN bzw. Port des vCenters vornehmen müssen, dann müssen Sie den vCenter-Server löschen und wieder hinzufügen.

