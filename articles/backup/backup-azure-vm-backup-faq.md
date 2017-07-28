
---
title: "Häufig gestellte Fragen zu Azure VM Backup | Microsoft-Dokumentation"
description: "Hier finden Sie Antworten auf häufig gestellte Fragen zur Funktionsweise von Azure VM Backup, zu Einschränkungen sowie zu den Folgen von Richtlinienänderungen."
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: Azure VM Backup, Azure VM Restore, Sicherungsrichtlinie
ms.assetid: c4cd7ff6-8206-45a3-adf5-787f64dbd7e1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 8c06a90f58cf56ebb4e75e7567e237de7414a300
ms.contentlocale: de-de
ms.lasthandoff: 07/20/2017

---
# <a name="questions-about-the-azure-vm-backup-service"></a>Fragen zum Azure VM Backup-Dienst
Dieser Artikel enthält Antworten auf häufig gestellte Fragen, damit Sie sich schnell mit den Komponenten von Azure VM Backup vertraut machen können. Einige Antworten enthalten Links zu Artikeln mit umfassenderen Informationen. Außerdem können Sie Fragen zum Azure Backup-Dienst im [Diskussionsforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)stellen.

## <a name="configure-backup"></a>Konfigurieren der Sicherung
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Unterstützen Recovery Services-Tresore klassische virtuelle Computer oder Resource Manager-basierte virtuelle Computer? <br/>
Recovery Services-Tresore unterstützen beide Modelle.  Sie können eine klassische VM (die im klassischen Portal erstellt wurde) oder eine Resource Manager-VM (die im Azure-Portal erstellt wurde) in einem Recovery Services-Tresor schützen.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup-"></a>Welche Konfigurationen werden von Azure VM Backup nicht unterstützt?
Informationen zu unterstützten Betriebssystemen finden Sie [hier](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup). Informationen zu Einschränkungen beim Sichern eines virtuellen Computers finden Sie [hier](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm).

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Warum wird mein virtueller Computer im Sicherungskonfigurations-Assistenten nicht angezeigt?
Im Sicherungskonfigurations-Assistenten werden nur virtuelle Computer aufgeführt, die folgende Voraussetzungen erfüllen:
* Sie sind noch nicht geschützt: Navigieren Sie zum Ermitteln des Sicherungsstatus eines virtuellen Computers zum VM-Blatt, und überprüfen Sie über das Einstellungsmenü des Blatts den Sicherungsstatus. Weitere Informationen zum Überprüfen des Sicherungsstatus eines virtuellen Computers finden Sie [hier](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade).
* Er gehört zur gleichen Region wie der virtuelle Computer.

## <a name="backup"></a>Sicherung
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Wird bei bedarfsbasierten Sicherungsaufträgen der gleiche Aufbewahrungszeitplan verwendet wie bei geplanten Sicherungen?
Nein. Bei einem bedarfsbasierten Sicherungsauftrag muss die gewünschte Aufbewahrungsdauer angegeben werden. Bei Aufträgen, die über das Portal ausgelöst werden, beträgt die Aufbewahrungsdauer standardmäßig 30 Tage. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Ich habe kürzlich für einige virtuelle Computer Azure Disk Encryption aktiviert. Funktionieren meine Sicherungen auch weiterhin?
Sie müssen dem Azure Backup-Dienst Zugriff auf Key Vault gewähren. Die erforderlichen Berechtigungen können Sie über PowerShell erteilen. Führen Sie dazu die Schritte des Abschnitts *Aktivieren der Sicherung* in der [PowerShell-Dokumentation](backup-azure-vms-automation.md) aus.

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Ich habe Datenträger eines virtuellen Computers zu verwalteten Datenträgern gemacht. Funktionieren meine Sicherungen auch weiterhin?
Ja. Sicherungen funktionieren reibungslos, und die Sicherung muss nicht neu konfiguriert werden. 

## <a name="restore"></a>Wiederherstellen
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Wann sollte ich Datenträger wiederherstellen und wann den gesamten virtuellen Computer?
Bei der vollständigen Wiederherstellung eines virtuellen Azure-Computers handelt es sich gewissermaßen um eine Schnellerstellungsoption für wiederhergestellte virtuelle Computer. Die Option „Virtuellen Computer wiederherstellen“ ändert die Namen der Datenträger, die von Datenträgern verwendeten Container, die öffentlichen IP-Adressen sowie die Namen der Netzwerkschnittstellen, um im Rahmen der Erstellung des virtuellen Computers eindeutige Ressourcen zu erhalten. Außerdem wird der virtuelle Computer keiner Verfügbarkeitsgruppe hinzugefügt. 

Die Datenträgerwiederherstellung sollte in folgenden Fällen verwendet werden:
* Zum Anpassen des virtuellen Computers, der auf der Grundlage einer Zeitpunktkonfiguration erstellt wird (beispielsweise, um die Größe aus der Sicherungskonfiguration anzupassen)
* Zum Hinzufügen von Konfigurationen, die zum Zeitpunkt der Sicherung nicht vorhanden waren 
* Zum Steuern der Namenskonvention für erstellte Ressourcen
* Zum Hinzufügen des virtuellen Computers zu einer Verfügbarkeitsgruppe
* Sie verfügen über eine beliebige Konfiguration, die nur über PowerShell/eine deklarative Vorlagendefinition erreicht werden kann

## <a name="manage-vm-backups"></a>Verwalten von VM-Sicherungen
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Was passiert, wenn ich eine Sicherungsrichtlinie für VMs ändere?
Wenn eine neue Richtlinie für VMs angewendet wird, werden der Zeitplan und die Aufbewahrung der neuen Richtlinie beachtet. Bei einer Ausweitung der Aufbewahrung werden bereits vorhandene Wiederherstellungspunkte markiert, um sie gemäß der neuen Richtlinie aufzubewahren. Bei einer Verkürzung der Aufbewahrung werden sie im Rahmen der nächsten Bereinigung gelöscht. 

