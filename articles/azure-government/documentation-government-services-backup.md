---
title: Azure Government Backup | Microsoft Docs
description: "Dieser Artikel bietet einen Überblick über die Azure Backup-Features in Azure Government."
services: azure-government
documentationcenter: 
author: markgalioto
manager: carmonm
ms.assetid: a7622135-8790-4be4-a02a-7b9ac8a4996f
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 1/5/2017
ms.author: markgal;
translationtype: Human Translation
ms.sourcegitcommit: fa00142a9e89c5ad2630f688ea9771a1a542c052
ms.openlocfilehash: e5f89f845302ecb890caa50dd8f86503b29f1154


---
# <a name="azure-government-backup"></a>Azure Government Backup

Dieser Artikel bietet einen Überblick über den Azure Backup-Dienst und listet die in Azure Government verfügbaren Backup-Features auf. Azure Backup ist der Azure-basierte Dienst, den Sie zum Sichern (bzw. Schützen) Ihrer Daten in der Microsoft Cloud verwenden können. Der Schutz Ihrer Daten in Azure schließt dabei nicht nur das Sichern von Daten in der Cloud, sondern auch das Wiederherstellen der Daten in einer lokalen Installation ein. Azure Backup bietet die folgenden Hauptvorteile:

- Automatische Speicherverwaltung
- Unbegrenzte Skalierung
- Mehrere Speicheroptionen
- Unbegrenzte Dateiübertragungen
- Datenverschlüsselung
- Anwendungskonsistente Sicherungen
- Langfristige Aufbewahrung

Wenn Sie noch nicht mit Azure Backup vertraut sind und eine Übersicht der verfügbaren Features anzeigen möchten, lesen Sie den Artikel [Was ist Azure Backup?](../backup/backup-introduction-to-azure-backup.md).

[!INCLUDE [learn-about-backup-deployment models](../../includes/backup-deployment-models.md)]

## <a name="azure-backup-components-available-in-azure-government-backup"></a>In Azure Government Backup verfügbare Azure Backup-Komponenten

Sie können mit Azure Backup Dateien, Ordner, Volumes, virtuelle Computer, Anwendungen und Workloads schützen. Je nachdem, welche Elemente Sie schützen möchten und wo diese Daten vorliegen, verwenden Sie eine andere Azure Backup-Komponente. Die folgenden Abschnitte enthalten Links zur öffentlichen Azure Backup-Dokumentation für jede Komponente. Die Abschnitten werden in das klassischen Portal und das Azure-Portal unterteilt. Verwenden Sie die Version mit dem Azure-Portal, wenn Sie Resource Manager-Bereitstellungen planen.

### <a name="using-windows-server-and-windows-computers-in-azure-portal"></a>Verwenden von Windows Server- und Windows-Computer im Azure-Portal

- [Sichern eines Windows-Servers oder -Clients](../backup/backup-configure-vault.md)
- [Wiederherstellen eines Windows-Servers oder -Clients](../backup/backup-azure-restore-windows-server.md)
- [Verwalten von Sicherungen für Windows-Server oder -Clients](../backup/backup-azure-manage-windows-server.md)
- [Verwenden von PowerShell zum Sichern von Windows Server](../backup/backup-client-automation.md)

### <a name="using-windows-server-and-windows-computers-in-classic-portal"></a>Verwenden von Windows Server- und Windows-Computer im klassischen Portal

- [Sichern eines Windows-Servers oder -Clients](../backup/backup-configure-vault-classic.md)
- [Wiederherstellen eines Windows-Servers oder -Clients](../backup/backup-azure-restore-windows-server-classic.md)
- [Verwalten von Sicherungen für Windows-Server oder -Clients](../backup/backup-azure-manage-windows-server-classic.md)
- [Verwenden von PowerShell zum Sichern von Windows Server](../backup/backup-client-automation-classic.md)

### <a name="using-virtual-machines-in-azure-portal"></a>Verwenden von Virtual Machines im Azure-Portal

- [Vorbereiten Ihrer VM-Umgebung](../backup/backup-azure-arm-vms-prepare.md)
- [Sichern virtueller Computer](../backup/backup-azure-vms-first-look-arm.md)
- [Wiederherstellen virtueller Computer](../backup/backup-azure-arm-restore-vms.md)
- [Verwalten virtueller Computer](../backup/backup-azure-manage-vms.md)
- [Verwenden von PowerShell zum Sichern von virtuellen Computern](../backup/backup-azure-vms-automation.md)

### <a name="using-virtual-machines-in-classic-portal"></a>Verwenden von Virtual Machines im klassischen Portal

- [Vorbereiten Ihrer VM-Umgebung](../backup/backup-azure-vms-prepare.md)
- [Sichern virtueller Computer](../backup/backup-azure-vms-first-look.md)
- [Wiederherstellen virtueller Computer](../backup/backup-azure-restore-vms.md)
- [Verwalten virtueller Computer](../backup/backup-azure-manage-vms-classic.md)
- [Verwenden von PowerShell zum Sichern von virtuellen Computern](../backup/backup-azure-vms-classic-automation.md)

### <a name="using-system-center-data-protection-manager-in-azure-portal"></a>Verwenden von System Center Data Protection Manager im Azure-Portal

- [Sicherung von System Center Data Protection Manager](../backup/backup-azure-dpm-introduction.md)

### <a name="using-system-center-data-protection-manager-in-classic-portal"></a>Verwenden von System Center Data Protection Manager im klassischen Portal

- [Sicherung von System Center Data Protection Manager](../backup/backup-azure-dpm-introduction-classic.md)

### <a name="using-azure-backup-server-in-azure-portal"></a>Verwenden von Azure Backup Server im Azure-Portal

Azure Backup Server ist eine Azure Backup-Komponente, die ähnliche Funktionen wie System Center Data Protection Manager (DPM) umfasst. Sie können mit Azure Backup Server allerdings keine Sicherungen auf Band durchführen. Mit Azure Backup Server können Sie Anwendungsworkloads wie Hyper-V-VMs, Microsoft SQL Server, SharePoint Server, Microsoft Exchange und Windows-Clients über eine zentrale Konsole in der Cloud sichern. Azure Backup Server erfordert keine System Center-Lizenz.

- [Azure Backup Server](../backup/backup-azure-microsoft-azure-backup.md)

### <a name="using-azure-backup-server-in-classic-portal"></a>Verwenden von Azure Backup Server im klassischen Portal

- [Azure Backup Server](../backup/backup-azure-microsoft-azure-backup-classic.md)


## <a name="next-steps"></a>Nächste Schritte

Wenn Sie sich nicht sicher sind, wo Sie anfangen sollen, beginnen Sie mit dem Artikel [Sichern eines Windows-Servers oder -Clients in Azure mit dem klassischen Bereitstellungsmodell](../backup/backup-configure-vault-classic.md). In diesem Tutorial werden Sie durch die Schritte zum Einrichten eines Sicherungsprojekts auf einem Windows-Server oder -Client geleitet.

Wenn Sie bereits wissen, dass Sie Azure Backup verwenden könnten, sich aber über die Kosten informieren möchten, finden Sie weitere Informationen auf der Seite mit den [Backup-Preisdetails](http://azure.microsoft.com/pricing/details/backup/). Dort finden Sie eine Liste mit häufig gestellten Fragen, die nützliche Informationen liefern. Beachten Sie auch, dass das Dropdownmenü **Region** zwei Azure Government-Regionen umfasst.



<!--HONumber=Jan17_HO1-->


