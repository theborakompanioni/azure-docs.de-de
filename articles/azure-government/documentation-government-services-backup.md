---
title: Azure Government Backup | Microsoft Docs
description: "Dieser Artikel bietet einen Überblick über die Azure Backup-Features in Azure Government."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
ms.assetid: a7622135-8790-4be4-a02a-7b9ac8a4996f
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/11/2016
ms.author: markgal;
translationtype: Human Translation
ms.sourcegitcommit: 0095a95afc14de42c1160a73139a0f059cd758dd
ms.openlocfilehash: 3b3ecaef33aefc89e5a5c1cd351566d21246d0b8


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

> [!IMPORTANT]
> Derzeit bietet Azure Government Backup Unterstützung für Dienst-Manager-Bereitstellungen, auch bekannt als klassisches Bereitstellungsmodell. Resource Manager-Bereitstellungen werden noch nicht unterstützt. Lesen Sie den folgenden Artikel, um die [Unterschiede zwischen dem Azure Resource Manager- und dem klassischen Bereitstellungsmodell](../resource-manager-deployment-model.md) kennenzulernen.

[!INCLUDE [learn-about-backup-deployment models](../../includes/backup-deployment-models.md)]

## <a name="azure-backup-components-available-in-azure-government-backup"></a>In Azure Government Backup verfügbare Azure Backup-Komponenten

Sie können mit Azure Backup Dateien, Ordner, Volumes, virtuelle Computer, Anwendungen und Workloads schützen. Je nachdem, welche Elemente Sie schützen möchten und wo diese Daten vorliegen, verwenden Sie eine andere Azure Backup-Komponente. Die folgenden Abschnitte enthalten Links zur öffentlichen Azure Backup-Dokumentation für jede Komponente.

Jeder Artikel erläutert, wie Sie die Azure Backup-Komponente in der klassischen Portalversion verwenden.

### <a name="windows-server-and-windows-computers"></a>Windows-Server und Windows-Computer

- [Sichern eines Windows-Servers oder -Clients](../backup/backup-configure-vault-classic.md)
- [Wiederherstellen eines Windows-Servers oder -Clients](../backup/backup-azure-restore-windows-server.md)
- [Verwalten von Sicherungen für Windows-Server oder -Clients](../backup/backup-azure-manage-windows-server.md)
- [Verwenden von PowerShell zum Sichern von Windows Server](../backup/backup-client-automation-classic.md)

### <a name="virtual-machines"></a>Virtual Machines

- [Vorbereiten Ihrer VM-Umgebung](../backup/backup-azure-vms-prepare.md)
- [Sichern virtueller Computer](../backup/backup-azure-vms-first-look.md)
- [Wiederherstellen virtueller Computer](../backup/backup-azure-restore-vms.md)
- [Verwalten virtueller Computer](../backup/backup-azure-manage-vms-classic.md)
- [Verwenden von PowerShell zum Sichern von virtuellen Computern](../backup/backup-azure-vms-classic-automation.md)

### <a name="system-center-data-protection-manager"></a>System Center Data Protection Manager

- [Sicherung von System Center Data Protection Manager](../backup/backup-azure-dpm-introduction-classic.md)

### <a name="azure-backup-server"></a>Azure Backup Server

- [Azure Backup Server](../backup/backup-azure-microsoft-azure-backup-classic.md)

Azure Backup Server ist eine Azure Backup-Komponente, die ähnliche Funktionen wie System Center Data Protection Manager (DPM) umfasst. Mit Azure Backup Server können Sie Anwendungsworkloads wie Hyper-V-VMs, Microsoft SQL Server, SharePoint Server, Microsoft Exchange und Windows-Clients über eine zentrale Konsole in der Cloud sichern.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie sich nicht sicher sind, wo Sie anfangen sollen, beginnen Sie mit dem Artikel [Sichern eines Windows-Servers oder -Clients in Azure mit dem klassischen Bereitstellungsmodell](../backup/backup-configure-vault-classic.md). In diesem Tutorial werden Sie durch die Schritte zum Einrichten eines Sicherungsprojekts auf einem Windows-Server oder -Client geleitet.

Wenn Sie bereits wissen, dass Sie Azure Backup verwenden könnten, sich aber über die Kosten informieren möchten, finden Sie weitere Informationen auf der Seite mit den [Backup-Preisdetails](http://azure.microsoft.com/pricing/details/backup/). Dort finden Sie eine Liste mit häufig gestellten Fragen, die nützliche Informationen liefern. Beachten Sie auch, dass das Dropdownmenü **Region** zwei Azure Government-Regionen umfasst.



<!--HONumber=Nov16_HO3-->


