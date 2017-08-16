---
title: "Vorbereiten lokaler VMware-Ressourcen für die Replikation in Azure mit Azure Site Recovery | Microsoft-Dokumentation"
description: "Enthält eine Zusammenfassung der Schritte zum Replizieren von Workloads, die auf VMware-VMs ausgeführt werden, im Azure-Speicher."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 6aba0e89-ad7c-467e-9db2-cfb3bfe4c7d6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: raynew
ms.translationtype: HT
ms.sourcegitcommit: 0aae2acfbf30a77f57ddfbaabdb17f51b6938fd6
ms.openlocfilehash: 3e1c589030210c2eae1ad9c02811775d9d6365d4
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="step-6-prepare-on-premises-vmware-replication-to-azure"></a>Schritt 6: Vorbereiten einer lokalen VMware-Replikation in Azure

Folgen Sie den Anweisungen in diesem Artikel, um lokale VMware-Server für die Interaktion mit Azure Site Recovery vorzubereiten und um VMware-VMs für die Installation des Mobilitätsdiensts vorzubereiten. Der Mobilitätsdiensts-Agent muss auf allen lokalen virtuellen Computern installiert werden, die Sie in Azure replizieren möchten.

## <a name="prepare-for-automatic-discovery"></a>Vorbereiten für die automatische Ermittlung

Site Recovery ermittelt automatisch VMs, die auf vSphere-ESXi-Hosts (mit oder ohne vCenter-Server) ausgeführt werden. Für die automatische Ermittlung benötigt Site Recovery ein Konto zum Zugreifen auf Hosts und Server:

1. Um ein dediziertes Konto zu verwenden, erstellen Sie eine Rolle (auf vCenter-Ebene) mit den Berechtigungen, die in der folgenden Tabelle beschrieben werden. Geben Sie ihr einen Namen wie **Azure_Site_Recovery**.
2. Erstellen Sie anschließend einen Benutzer auf dem vSphere-Host bzw. dem vCenter-Server, und weisen Sie die Rolle dem Benutzer zu. Sie geben dieses Benutzerkonto während der Site Recovery-Bereitstellung an.


### <a name="vmware-account-permissions"></a>VMware-Kontoberechtigungen

Für Site Recovery ist der Zugriff auf VMware erforderlich, damit der Prozessserver VMs automatisch ermitteln und das Failover und Failback von VMs durchgeführt werden kann.

- **Migrieren**: Wenn Sie nur VMware-VMs zu Azure migrieren möchten, ohne jemals ein Failback durchzuführen, können Sie ein VMware-Konto mit einer schreibgeschützten Rolle verwenden. Mit einer Rolle dieser Art kann ein Failover durchgeführt werden, aber es ist nicht möglich, geschützte Quellcomputer herunterzufahren. Dies ist für die Migration nicht erforderlich.
- **Replizieren/Wiederherstellen**: Wenn Sie die vollständige Replikation (Replizieren, Failover, Failback) bereitstellen möchten, müssen mit dem Konto bestimmte Vorgänge möglich sein, z.B. das Erstellen und Entfernen von Datenträgern, das Aktivieren von VMs usw.
- **Automatische Ermittlung**: Mindestens ein schreibgeschütztes Konto ist erforderlich.


**Aufgabe** | **Erforderliches Konto bzw. Rolle** | **Berechtigungen** | **Details**
--- | --- | --- | ---
**Prozessserver ermittelt VMware-VMs automatisch** | Sie benötigen mindestens einen Benutzer mit Lesezugriff. | Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.
**Failover** | Sie benötigen mindestens einen Benutzer mit Lesezugriff. | Data Center object (Rechenzentrenobjekt) –> Propagate to Child Object (An untergeordnetes Objekt weitergeben), role=Read-only (Rolle=schreibgeschützt) | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.<br/><br/> Ist für Migrationszwecke geeignet, aber nicht für die vollständige Replikation, Failover oder Failback.
**Failover und Failback** | Wir empfehlen, dass Sie eine Rolle (Azure_Site_Recovery) mit den erforderlichen Berechtigungen erstellen und sie dann einem VMware-Benutzer oder einer VMware-Gruppe zuweisen | Rechenzentrumsobjekt -> An untergeordnetes Objekt weitergeben, Rolle=Azure_Site_Recovery<br/><br/> Datenspeicher -> Speicherplatz zuordnen, Datenspeicher durchsuchen, Low-Level-Dateivorgänge, Datei entfernen, Dateien virtueller Computer aktualisieren<br/><br/> Netzwerk -> Netzwerk zuweisen<br/><br/> Ressource -> Zuweisen der VM zu einem Ressourcenpool, ausgeschaltete VM migrieren, eingeschaltete VM migrieren<br/><br/> Tasks (Aufgaben) -> Create task (Aufgabe erstellen), update task (Aufgabe aktualisieren)<br/><br/> Virtueller Computer -> Konfiguration<br/><br/> Virtueller Computer -> Interagieren -> Frage beantworten, Geräteverbindung, CD-Medien konfigurieren, Diskettenmedien konfigurieren, Ausschalten, Einschalten, VMware-Tools installieren<br/><br/> Virtueller Computer -> Inventar -> Erstellen, Registrieren, Registrierung aufheben<br/><br/> Virtueller Computer -> Bereitstellung -> Download virtueller Computer zulassen, Upload von Dateien virtueller Computer zulassen<br/><br/> Virtual machine -> Snapshots -> Remove snapshots | Der Benutzer wird auf Datencenterebene zugewiesen und hat Zugriff auf alle Objekte im Datencenter.<br/><br/> Um den Zugriff einzuschränken, weisen Sie den untergeordneten Objekten (vSphere-Hosts, Datenspeicher, VMs und Netzwerke) die Rolle **No access** (Kein Zugriff) mit **Propagate to child object** (Auf untergeordnetes Objekt übertragen) zu.


## <a name="prepare-for-push-installation-of-the-mobility-service"></a>Vorbereiten der Pushinstallation des Mobilitätsdiensts

Der Mobilitätsdienst muss auf jeder VM installiert sein, die Sie replizieren möchten. Es gibt zahlreiche Möglichkeiten, den Dienst zu installieren, z.B. die manuelle Installation, die Pushinstallation vom Site Recovery-Prozessserver, und die Installation mit Methoden wie z.B. System Center Configuration Manager.

Wenn Sie die Pushinstallation verwenden möchten, müssen Sie ein Konto vorbereiten, das Site Recovery verwenden kann, um auf die VM zuzugreifen.

- Sie können ein Domänenkonto oder ein lokales Konto verwenden
- Wenn Sie kein Domänenkonto verwenden, müssen Sie für Windows die Remote-Benutzerzugriffssteuerung auf dem lokalen Computer deaktivieren. Zu diesem Zweck fügen Sie in der Registrierung unter **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** den DWORD-Eintrag **LocalAccountTokenFilterPolicy** mit dem Wert 1 hinzu.
- Geben Sie Folgendes ein, wenn Sie den Registrierungseintrag für Windows über eine CLI hinzufügen möchten: ``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Bei Linux muss auf dem Linux-Quellserver das root-Konto verwendet werden.



## <a name="next-steps"></a>Nächste Schritte

Gehen Sie zu [Schritt 7: Erstellen eines Tresors](vmware-walkthrough-create-vault.md).

