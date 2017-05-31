---
title: "Übersicht über Recovery Services-Tresore | Microsoft-Dokumentation"
description: "Übersicht über Recovery Services-und Azure Backup-Tresore sowie Vergleich dieser Tresore"
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.assetid: 38d4078b-ebc8-41ff-9bc8-47acf256dc80
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/15/2017
ms.author: markgal;arunak
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 19e2aafe3de106be32f3d90c63c0ea03c626f272
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="recovery-services-vaults-overview"></a>Übersicht über Recovery Services-Tresore

In diesem Artikel werden die Funktionen eines Recovery Services-Tresors beschrieben. Ein Recovery Services-Tresor ist eine Speicherentität in Azure, die Daten enthält. Bei den Daten handelt es sich in der Regel um Kopien von Daten oder Konfigurationsinformationen für virtuelle Computer (VMs), Workloads, Server oder Arbeitsstationen. Ein Recovery Services-Tresor ist die Resource Manager-Version eines Sicherungstresors. Microsoft empfiehlt Ihnen, Recovery Services-Tresore zu verwenden und Sicherungstresore in Recovery Services-Tresore zu konvertieren.

## <a name="what-is-a-recovery-services-vault"></a>Was ist ein Recovery Services-Tresor?

Ein Recovery Services-Tresor ist eine Onlinespeicherentität in Azure, die zum Speichern von Daten wie Sicherungskopien, Wiederherstellungspunkten und Sicherungsrichtlinien verwendet wird. Mit Recovery Services-Tresoren können Sie Sicherungsdaten für verschiedene Azure-Dienste speichern, z.B. IaaS-VMs (Linux oder Windows) und Azure SQL-Datenbanken. Recovery Services-Tresore unterstützen System Center DPM, Windows Server, Azure Backup Server etc. Recovery Services-Tresore vereinfachen die Organisation Ihrer Sicherungsdaten und minimieren gleichzeitig den Verwaltungsaufwand.

Innerhalb eines Azure-Abonnements können Sie beliebig viele Recovery Services-Tresore erstellen.

## <a name="comparing-recovery-services-vaults-and-backup-vaults"></a>Vergleich zwischen Recovery Services- und Sicherungstresoren

Recovery Services-Tresore basieren auf dem Azure Resource Manager-Modell von Azure, wohingegen Sicherungstresore auf dem Azure Services Manager-Modell beruhen. Beim Upgrade eines Sicherungstresors auf einen Recovery Services-Tresor bleiben die Sicherungsdaten während und nach dem Upgradevorgang intakt. Recovery Services-Tresore bieten Funktionen, die nicht für Sicherungstresore verfügbar sind. Hierzu zählen z.B. Folgende:

- **Erweiterte Funktionen zum Schutz von Sicherungsdaten**: Durch Recovery Services-Tresore bietet Azure Backup Sicherheitsfunktionen zum Schutz von Cloudsicherungen. Mit diesen Sicherheitsfunktionen wird sichergestellt, dass Sie Ihre Sicherungen schützen und Daten sicher aus Cloudsicherungen wiederherstellen können, selbst wenn Produktions- und Sicherungsserver kompromittiert sind. [Weitere Informationen](backup-azure-security-feature.md)

- **Zentrale Überwachung Ihrer Hybrid-IT-Umgebung**: Mit Recovery Services-Tresoren können Sie nicht nur Ihre [Azure-IaaS-VMs](backup-azure-manage-vms.md), sondern auch Ihre [lokalen Ressourcen](backup-azure-manage-windows-server.md#manage-backup-items) über ein zentrales Portal überwachen. [Weitere Informationen](http://azure.microsoft.com/blog/alerting-and-monitoring-for-azure-backup)

- **Rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)**: Die RBAC ermöglicht eine präzise Verwaltung der Zugriffssteuerung in Azure. [Azure bietet verschiedene integrierte Rollen](../active-directory/role-based-access-built-in-roles.md), während Azure Backup über drei [integrierte Rollen zum Verwalten von Wiederherstellungspunkten](backup-rbac-rs-vault.md) verfügt. Recovery Services-Tresore sind mit der RBAC kompatibel, die den Zugriff auf Sicherungen und Wiederherstellungen auf den definierten Satz von Benutzerrollen beschränkt. [Weitere Informationen](backup-rbac-rs-vault.md)

- **Schutz aller Konfigurationen von virtuellen Azure-Computern**: Recovery Services-Tresore schützen Resource Manager-basierte VMs, einschließlich Premium-Datenträger, verwaltete Datenträger und verschlüsselte VMs. Durch die Durchführung eines Upgrades eines Sicherungstresors auf einen Recovery Services-Tresor erhalten Sie die Möglichkeit, ein Upgrade für Ihre Service Manager-basierten VMs auf Resource Manager-basierte VMs durchzuführen. Während des Upgrades des Tresors können Sie die Wiederherstellungspunkte für Ihre Service Manager-basierten VMs beibehalten und den Schutz für die aktualisierten (Resource Manager-fähigen) VMs konfigurieren. [Weitere Informationen](http://azure.microsoft.com/blog/azure-backup-recovery-services-vault-ga)

- **Sofortige Wiederherstellung für IaaS-VMs**: Durch Recovery Services-Tresore können Sie Dateien und Ordner von einer IaaS-VM wiederherstellen, ohne die gesamte VM wiederherstellen zu müssen. So werden die Wiederherstellungszeiten verkürzt. Die sofortige Wiederherstellung für IaaS-VMs ist für Windows- und Linux-VMs verfügbar. [Weitere Informationen](http://azure.microsoft.com/blog/instant-file-recovery-from-azure-linux-vm-backup-using-azure-backup-preview)

## <a name="managing-your-recovery-services-vaults-in-the-portal"></a>Verwalten von Recovery Services-Tresoren im Portal
Die Erstellung und Verwaltung von Recovery Services-Tresoren im Azure-Portal ist einfach, da der Sicherungsdienst auf dem Blatt „Azure-Einstellungen“ integriert ist. Eine derartige Integration bedeutet, dass Sie einen Recovery Services-Tresor *im Kontext des Zieldiensts* erstellen oder verwalten können. Um beispielsweise die Wiederherstellungspunkte für eine VM anzuzeigen, wählen Sie sie aus, und klicken Sie auf dem Blatt „Einstellungen“ auf **Sicherung**. Die Sicherungsinformationen speziell zu dieser VM werden angezeigt. Im folgenden Beispiel ist **ContosoVM** der Name des virtuellen Computers. **ContosoVM-demovault** ist der Name des Recovery Services-Tresors. Sie müssen sich den Namen des Recovery Services-Tresors, in dem die Wiederherstellungspunkte gespeichert werden, nicht merken, sondern können über den virtuellen Computer auf diese Informationen zugreifen.  

![Details zum Recovery Services-Tresor über die VM](./media/backup-azure-recovery-services-vault-overview/rs-vault-in-context.png)

Wenn mehrere Server mit demselben Recovery Services-Tresor geschützt sind, kann es sinnvoller sein, den Recovery Services-Tresor zu überprüfen. Sie können nach allen Recovery Services-Tresoren im Abonnement suchen und einen aus der Liste auswählen.

Die folgenden Abschnitte enthalten Links zu Artikeln, in denen erläutert wird, wie Sie einen Recovery Services-Tresor in jedem Aktivitätstyp verwenden.

### <a name="back-up-data"></a>Sichern von Daten
- [Sichern einer Azure-VM](backup-azure-vms-first-look-arm.md)
- [Sichern von Daten von Windows Server oder einer Windows-Arbeitsstation](backup-try-azure-backup-in-10-mins.md)
- [Sichern von DPM-Workloads in Azure](backup-azure-dpm-introduction.md)
- [Vorbereiten der Sicherung von Workloads per Azure Backup Server](backup-azure-microsoft-azure-backup.md)

### <a name="manage-recovery-points"></a>Verwalten von Wiederherstellungspunkten
- [Verwalten von Azure-VM-Sicherungen](backup-azure-manage-vms.md)
- [Verwalten von Dateien und Ordnern](backup-azure-manage-windows-server.md)

### <a name="restore-data-from-the-vault"></a>Wiederherstellen von Daten aus dem Tresor
- [Wiederherstellen einzelner Dateien aus einer Azure-VM](backup-azure-restore-files-from-vm.md)
- [Wiederherstellen einer Azure-VM](backup-azure-arm-restore-vms.md)

### <a name="secure-the-vault"></a>Sichern des Tresors
- [Sichern von Cloudsicherungsdaten in Recovery Services-Tresoren](backup-azure-security-feature.md)



## <a name="next-steps"></a>Nächste Schritte
In den folgenden Artikeln erhalten Sie weitere Informationen:</br>
[Sichern einer IaaS-VM](backup-azure-arm-vms-prepare.md)</br>
[Sichern von Azure Backup Server](backup-azure-microsoft-azure-backup.md)</br>
[Sichern von Windows Server](backup-configure-vault.md)

