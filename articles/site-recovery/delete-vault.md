---
title: "Löschen eines Recovery Services-Tresors"
service: site-recovery
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/04/2017
ms.author: rajani-janaki-ram
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 32fcab0c9e4665d07691dc3792bdee90fb01fe66
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="delete-recovery-services-vault"></a>Löschen eines Recovery Services-Tresors
Abhängigkeiten verhindern das Löschen des Recovery Services-Tresors, und die Aktionen, die Sie durchführen müssen, variieren basierend auf dem Typ des Azure Site Recovery-Szenarios – VMWare nach Azure, Hyper-V (mit und ohne VMM) auf Azure und Azure Backup. Klicken auf [diesen](../backup/backup-azure-delete-vault.md) Link, um zu erfahren, wie Sie einen Tresor in Azure Backup löschen.

>[!Important]
>Wenn Sie das Produkts testen und den Tresor schnell löschen möchten und keine Bedenken bezüglich eines Datenverlusts haben, können Sie mit der Löschen erzwingen-Methode den Tresor und alle Abhängigkeiten entfernen.

> Beachten Sie, dass der PowerShell-Befehl den gesamten Inhalt des Tresors löschen wird und nicht umkehrbar ist

## <a name="force-delete-vault-using-powershell"></a>Löschen des Tresors mithilfe von PowerShell erzwingen

Befolgen Sie die unten aufgeführten Schritte, um den Site Recovery-Tresor zu löschen, selbst wenn geschützte Elemente vorhanden sind

    Login-AzureRmAccount

    Select-AzureRmSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzureRmSiteRecoveryVault -Name "vaultname"

    Remove-AzureRmSiteRecoveryVault -Vault $vault



Befolgen Sie die empfohlenen Schritte (in der angegebenen Reihenfolge) für Ihr Szenario, um den Tresor zu löschen

## <a name="delete-vault-used-in-site-recovery-for-protecting-vmware-vms-to-azure"></a>Tresor löschen, in Site Recovery für den Schutz von VMWare-VMs in Azure verwendet:
1. Stellen Sie sicher, dass alle geschützten virtuellen Computer gelöscht werden. Gehen Sie [folgendermaßen vor](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).
2.  Stellen Sie sicher, dass alle Replikationsrichtlinien gelöscht werden. Gehen Sie [folgendermaßen vor](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).
3.  Stellen Sie sicher, dass Verweise auf vCenter gelöscht werden. Gehen Sie [folgendermaßen vor](site-recovery-vmware-to-azure-manage-vCenter.md##delete-a-vcenter-in-azure-site-recovery).
4. Stellen Sie sicher, dass der Konfigurationsserver gelöscht wird. Gehen Sie [folgendermaßen vor](site-recovery-vmware-to-azure-manage-configuration-server.md##decommissioning-a-configuration-server).
5. Versuchen Sie nun den Tresor zu löschen.


## <a name="delete-vault-used-in-site-recovery-for-protecting-hyper-v-vms-with-vmm-to-azure"></a>Tresor löschen, in Site Recovery für den Schutz von Hyper-V-VMs (mit VMM) in Azure verwendet:
1.  Stellen Sie sicher, dass alle geschützten virtuellen Computer gelöscht werden. Gehen Sie [folgendermaßen vor](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).
- Stellen Sie sicher, dass alle Replikationsrichtlinien gelöscht werden. Gehen Sie [folgendermaßen vor](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).
-   Löschen Sie Verweise auf VMM-Server. Gehen Sie [folgendermaßen vor](site-recovery-manage-registration-and-protection.md##unregister-a-connected-vmm-server)
-   Versuchen Sie nun den Tresor zu löschen.

## <a name="delete-vault-used-in-site-recovery--for-protecting-hyper-v-vms-without-vmm-to-azure"></a>Tresor löschen, in Site Recovery für den Schutz von Hyper-V-VMs (ohne VMM) in Azure verwendet:
1. Stellen Sie sicher, dass alle geschützten virtuellen Computer gelöscht werden. Gehen Sie [folgendermaßen vor](site-recovery-manage-registration-and-protection.md##disable-protection-for-a-vmware-vm-or-physical-server).
- Stellen Sie sicher, dass alle Replikationsrichtlinien gelöscht werden. Gehen Sie [folgendermaßen vor](site-recovery-setup-replication-settings-vmware.md##delete-a-replication-policy).
-   Löschen Sie Verweise auf Hyper-V-Servern. Gehen Sie [folgendermaßen vor](/site-recovery-manage-registration-and-protection.md##unregister-a-hyper-v-host-in-a-hyper-v-site).
-   Löschen Sie die Hyper-V-Site.
-   Versuchen Sie nun den Tresor zu löschen.

