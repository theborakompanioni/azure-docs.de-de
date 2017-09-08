---
title: Bereitstellen und Verwalten von Sicherungen mit PowerShell auf virtuellen Computern, auf denen Resource Manager bereitgestellt ist | Microsoft Docs
description: "Verwenden Sie PowerShell zum Bereitstellen und Verwalten von Sicherungen in Azure für mit Resource Manager bereitgestellte virtuelle Computer."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 68606e4f-536d-4eac-9f80-8a198ea94d52
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/30/2017
ms.author: markgal;trinadhk
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 414fb9adaa1535d82b1bc385ff0864394efb837c
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-to-back-up-virtual-machines"></a>Verwenden von AzureRM.RecoveryServices.Backup-Cmdlets zum Sichern virtueller Computer
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](backup-azure-vms-automation.md)
> * [Klassisch](backup-azure-vms-classic-automation.md)
>
>

In diesem Artikel erfahren Sie, wie Sie eine Azure-VM mithilfe von Azure PowerShell-Cmdlets in einem Recovery Services-Tresor sichern und daraus wiederherstellen. Ein Recovery Services-Tresor ist eine Ressource von Azure Resource Manager und dient zum Schutz von Daten und Assets in den Diensten Azure Backup und Azure Site Recovery. Sie können einen Recovery Services-Tresor zum Schutz von virtuellen Computern verwenden, die mit Azure Service Manager oder Azure Resource Manager bereitgestellt wurden.

> [!NOTE]
> Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel ist für die Verwendung mit virtuellen Computern erstellt, die mit dem Resource Manager-Modell bereitgestellt wurden.
>
>

In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer mit PowerShell schützen und Daten von einem Wiederherstellungspunkt aus wiederherstellen.

## <a name="concepts"></a>Konzepte
Wenn Sie nicht mit dem Dienst „Azure Backup“ vertraut sind, können Sie sich mit [Was ist Azure Backup?](backup-introduction-to-azure-backup.md) eine Übersicht über den Dienst verschaffen. Bevor Sie beginnen, stellen Sie sicher, dass Sie die Voraussetzungen zur Arbeit mit Azure Backup und die Einschränkungen der aktuellen Lösung zur Sicherung virtueller Computer im Wesentlichen kennen.

Damit Sie PowerShell effektiv nutzen können, ist es notwendig, dass Sie die Objekthierarchie verstehen und wissen, womit Sie beginnen sollten.

![Recovery Services-Objekthierarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

Sehen Sie sich die [Azure Backup - Recovery Services-Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in der Azure-Bibliothek an, um die Cmdlet-Referenz „AzureRm.RecoveryServices.Backup PowerShell“ anzuzeigen.

## <a name="setup-and-registration"></a>Einrichtung und Registrierung
Vorbereitung:

1. [Laden Sie die neueste PowerShell-Version herunter](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (erforderliche Mindestversion: 1.4.0).
2. Ermitteln Sie die Azure-PowerShell-Cmdlets zur Datensicherung mithilfe des folgenden Befehls:

```
PS C:\> Get-Command *azurermrecoveryservices*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmRecoveryServicesBackupItem           1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Disable-AzureRmRecoveryServicesBackupProtection    1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Enable-AzureRmRecoveryServicesBackupProtection     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupContainer         1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupItem              1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJob               1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJobDetails        1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupManagementServer  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRMRecoveryServicesBackupRecoveryPoint     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupRetentionPolic... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupSchedulePolicy... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesVaultSettingsFile       1.4.0      AzureRM.RecoveryServices
Cmdlet          New-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          New-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
Cmdlet          Remove-AzureRmRecoveryServicesProtectionPolicy     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Remove-AzureRmRecoveryServicesVault                1.4.0      AzureRM.RecoveryServices
Cmdlet          Restore-AzureRMRecoveryServicesBackupItem          1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
Cmdlet          Set-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesVaultContext            1.4.0      AzureRM.RecoveryServices
Cmdlet          Stop-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupContainer  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupManagem... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Wait-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
```


Die folgenden Aufgaben können mit PowerShell automatisiert werden:

* Erstellen eines Recovery Services-Tresors
* Sichern von Azure-VMs
* Auslösen eines Sicherungsauftrags
* Überwachen eines Sicherungsauftrags
* Wiederherstellen eines virtuellen Azure-Computers

## <a name="create-a-recovery-services-vault"></a>Erstellen eines Recovery Services-Tresors
Mit den folgenden Schritten können Sie einen Recovery Services-Tresor erstellen. Ein Recovery Services-Tresor unterscheidet sich von einem Sicherungstresor.

1. Falls Sie Azure Backup zum ersten Mal verwenden, müssen Sie das Cmdlet **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** verwenden, um den Azure Recovery Service-Anbieter für Ihr Abonnement zu registrieren.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. Der Recovery Services-Tresor ist eine Resource Manager-Ressource. Deshalb müssen Sie ihn in eine Ressourcengruppe einfügen. Sie können eine vorhandene Ressourcengruppe verwenden oder eine Ressourcengruppe mit dem Cmdlet **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)** erstellen. Wenn Sie eine Ressourcengruppe erstellen, geben Sie den Namen und den Speicherort für die Ressourcengruppe an.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. Erstellen Sie mithilfe des Cmdlets **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** den Recovery Services-Tresor. Stellen Sie sicher, dass Sie den gleichen Speicherort für den Tresor angeben, der für die Ressourcengruppe verwendet wurde.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. Geben Sie den Typ der zu verwendenden Speicherredundanz an – entweder [lokal redundanter Speicher (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) oder [geografisch redundanter Speicher (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). Das folgende Beispiel zeigt, dass für die Option „BackupStorageRedundancy“ für „testVault“ der Wert auf „GeoRedundant“ festgelegt ist.

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Viele Azure Backup-Cmdlets benötigen das Recovery Services-Tresorobjekt als Eingabe. Aus diesem Grund sollte das zur Sicherung verwendete Recovery Services-Tresorobjekt in einer Variablen gespeichert werden.
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>Anzeigen von Tresoren in einem Abonnement
Verwenden Sie **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**, um die Liste mit allen Tresoren im aktuellen Abonnement anzuzeigen. Mithilfe dieses Befehls können Sie überprüfen, ob ein neuer Tresor erstellt wurde, oder die verfügbaren Tresore im Abonnement anzeigen.

Führen Sie den Befehl „Get-AzureRmRecoveryServicesVault“ aus, um alle Tresore im Abonnement anzuzeigen. Im folgenden Beispiele werden die angezeigten Informationen für jeden Tresor dargestellt.

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="back-up-azure-vms"></a>Sichern von Azure-VMs
Verwenden Sie einen Recovery Services-Tresor zum Schutz Ihrer virtuellen Computer. Bevor Sie den Schutz anwenden, legen Sie den Tresorkontext (den Typ der Daten, die im Tresor geschützt werden) fest, und überprüfen Sie die Schutzrichtlinie. Die Schutzrichtlinie stellt den Zeitplan für die Ausführung des Sicherungsauftrags dar, und für die Aufbewahrungsdauer jeder Sicherungsmomentaufnahme.

### <a name="set-vault-context"></a>Festlegen des Tresorskontexts
Verwenden Sie vor dem Aktivieren des Schutzes auf einem virtuellen Computer **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**, um den Tresorkontext festzulegen. Sobald der Tresorkontext festgelegt ist, gilt er für alle nachfolgenden-Cmdlets. Im folgenden Beispiel wird der Tresorkontext für den Tresor *testvault* festgelegt.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Erstellen einer Schutzrichtlinie
Wenn Sie einen Recovery Services-Tresor erstellen, enthält er automatisch standardmäßige Schutz- und Aufbewahrungsrichtlinien. Die standardmäßige Schutzrichtlinie löst täglich zu einem angegebenen Zeitpunkt einen Sicherungsauftrag aus. Die standardmäßige Aufbewahrungsrichtlinie bewahrt den täglichen Wiederherstellungspunkt für 30 Tage auf. Mit der Standardrichtlinie können Sie Ihren virtuellen Computer schnell schützen und die Richtlinie später mit anderen Details bearbeiten.

Verwenden Sie **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**, um die Schutzrichtlinien im Tresor anzuzeigen. Sie können dieses Cmdlet zum Abrufen einer bestimmten Richtlinie verwenden oder um Richtlinien anzuzeigen, die einem bestimmten Workloadtyp zugeordnet sind. Im folgenden Beispiel werden Richtlinien für den Workloadtyp „AzureVM“ abgerufen.

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> Die Zeitzone des Feldes „BackupTime“ in PowerShell ist UTC. Wenn jedoch der Zeitpunkt der Sicherung im Azure-Portal angezeigt wird, wird die Zeit auf Ihre lokale Zeitzone eingestellt.
>
>

Eine Sicherungsschutzrichtlinie ist mindestens einer Aufbewahrungsrichtlinie zugeordnet. Die Aufbewahrungsrichtlinie definiert, wie lange ein Wiederherstellungspunkt in gespeichert wird, bevor er gelöscht wird. Verwenden Sie **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**, um die standardmäßige Aufbewahrungsrichtlinie anzuzeigen.  Auf ganz ähnliche Weise können Sie **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** verwenden, um die standardmäßige Zeitplanrichtlinie abzurufen. Das Cmdlet **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** erstellt ein PowerShell-Objekt, das Informationen zu Sicherungsrichtlinien enthält. Die Zeitplan- und Aufbewahrungsrichtlinienobjekte werden als Eingaben für das Cmdlet **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** verwendet. Im folgenden Beispiel werden die Zeitplanrichtlinie und die Aufbewahrungsrichtlinie in Variablen gespeichert. Im Beispiel werden diese Variablen verwendet, um die Parameter beim Erstellen einer Schutzrichtlinie,*NewPolicy*, zu definieren.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a>Schutz aktivieren
Nachdem Sie die Sicherungsschutzrichtlinie definiert haben, müssen Sie noch die Richtlinie für ein Element aktivieren. Verwenden Sie **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**, um den Schutz zu aktivieren. Zum Aktivieren des Schutzes sind zwei Objekte erforderlich – das Element und die Richtlinie. Sobald die Richtlinie mit dem Tresor verknüpft ist, wird der Sicherungsworkflow zu dem im Richtlinienzeitplan definierten Zeitpunkt gestartet.

Im folgenden Beispiel wird der Schutz für das Element „V2VM“ mithilfe der Richtlinie „NewPolicy“ aktiviert. So aktivieren Sie den Schutz für nicht verschlüsselte Resource Manager-VMs

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Um den Schutz auf verschlüsselten VMs (mit BEK und KEK verschlüsselt) zu aktivieren, müssen Sie dem Azure Backup-Dienst die Berechtigung zum Lesen von Schlüsseln und geheimen Schlüsseln aus dem Schlüsseltresor zuweisen.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Um den Schutz auf verschlüsselten VMs (nur mit BEK verschlüsselt) zu aktivieren, müssen Sie dem Azure Backup-Dienst die Berechtigung zum Lesen von Geheimnissen aus dem Schlüsseltresor zuweisen.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Wenn Sie die Azure Government-Cloud verwenden, verwenden Sie im Cmdlet [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) den Wert ff281ffe-705c-4f53-9f37-a40e6f2c68f3 für den Parameter **ServicePrincipalName**.
>
>

Für klassische virtuelle Computer

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>Ändern einer Schutzrichtlinie
Verwenden Sie [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) zum Bearbeiten der Schutzrichtlinie, um die Objekte „SchedulePolicy“ oder „RetentionPolicy“ zu ändern.

Im folgenden Beispiel wird der Aufbewahrungswert von Wiederherstellungspunkten in 365 Tage geändert.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>Auslösen einer Sicherung
Sie können **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** verwenden, um einen Sicherungsauftrag auszulösen. Wenn es die erste Sicherung ist, handelt es sich um eine vollständige Sicherung. Nachfolgende Sicherungen erstellen inkrementelle Kopien. Verwenden Sie vor dem Auslösen eines Sicherungsauftrags **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**, um den Tresorkontext festzulegen. Im folgenden Beispiel wird davon ausgegangen, dass der Tresorkontext festgelegt wurde.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> Die Zeitzone der Felder „StartTime“ und „EndTime“ in PowerShell ist UTC. Wenn jedoch der Zeitpunkt im Azure-Portal angezeigt wird, wird die Zeit auf Ihre lokale Zeitzone eingestellt.
>
>

## <a name="monitoring-a-backup-job"></a>Überwachen eines Sicherungsauftrags
Sie können Vorgänge mit langer Ausführungsdauer, z.B. Sicherungsaufträge, ohne das Azure-Portal überwachen. Mit dem Cmdlet **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)** können Sie den Status eines Auftrags abrufen, der sich in Bearbeitung befindet. Dieses Cmdlet ruft die Sicherungsaufträge für einen bestimmten Tresor ab. Dieser Tresor ist im Tresorkontext angegeben. Im folgenden Beispiel wird der Status eines laufenden Auftrags als Array abgerufen und in der Variable „$joblist“ gespeichert.

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Anstatt die Fertigstellung dieser Aufträge abzufragen – was unnötigen, zusätzlichen Code bedeutet –, verwenden Sie das Cmdlet **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**. Dieses Cmdlet hält die Ausführung an, bis entweder der Auftrag abgeschlossen oder der angegebene Timeoutwert erreicht ist.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Wiederherstellen eines virtuellen Azure-Computers
Es gibt einen wichtigen Unterschied zwischen dem Wiederherstellen eines virtuellen Computers mithilfe des Azure-Portals und dem Wiederherstellen eines virtuellen Computers mithilfe von PowerShell. Mit PowerShell ist der Wiederherstellungsvorgang mit der Erstellung der Datenträger und den Konfigurationsinformationen aus dem Wiederherstellungspunkt abgeschlossen.

> [!NOTE]
> Der Wiederherstellungsvorgang erstellt keinen virtuellen Computer.
>
>

Informationen zum Erstellen eines virtuellen Computers aus einem Datenträger finden Sie im Abschnitt [Erstellen des virtuellen Computers aus gespeicherten Datenträgern](backup-azure-vms-automation.md#create-a-vm-from-stored-disks). Zum Wiederherstellen eines virtuellen Azure-Computers müssen Sie folgende grundlegende Schritte ausführen:

* Auswählen des virtuellen Computers
* Auswählen eines Wiederherstellungspunkts
* Wiederherstellung der Datenträger
* Erstellen des virtuellen Computers aus gespeicherten Datenträgern

Die folgende Grafik zeigt die Objekthierarchie von „RecoveryServicesVault“ bis „BackupRecoveryPoint“.

![Die Recovery Services-Objekthierarchie zeigt die BackupContainer an](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

Identifizieren Sie das gesicherte Element und den Wiederherstellungspunkt, der die Zeitpunktdaten enthält, um die gesicherten Daten wiederherstellen zu können. Verwenden Sie das Cmdlet **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**, um Daten aus dem Tresor im Kundenkonto wiederherzustellen.

### <a name="select-the-vm"></a>Auswählen des virtuellen Computers
Zum Abrufen des PowerShell-Objekts, das das richtige Sicherungselement identifiziert, starten Sie vom Container im Tresor aus, und durchlaufen Sie die Objekthierarchie nach unten. Verwenden Sie das Cmdlet **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**, und reichen Sie dieses an das Cmdlet **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** weiter, um den Container auszuwählen, der den virtuellen Computer darstellt.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" –Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>Auswählen eines Wiederherstellungspunkts
Verwenden Sie das Cmdlet **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**, um alle Wiederherstellungspunkte für das Sicherungselement aufzulisten. Wählen Sie anschließend den zu wiederherstellenden Wiederherstellungspunkt. Wenn Sie nicht sicher sind, welchen Wiederherstellungspunkt Sie verwenden sollen, empfiehlt es sich, den letzten Punkt in der Liste auszuwählen, an dem sich RecoveryPointType = AppConsistent befand.

Im folgenden Skript ist die Variable **$rp** ein Array von Wiederherstellungspunkten für das ausgewählte Sicherungselement der letzten sieben Tage. Das Array wird in umgekehrter Reihenfolge sortiert. Der letzte Wiederherstellungspunkt liegt bei Index 0. Verwenden Sie die standardmäßige PowerShell-Arrayindizierung zum Auswählen des Wiederherstellungspunkts. Im Beispiel wählt „$rp[0]“ den letzten Wiederherstellungspunkt aus.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```



### <a name="restore-the-disks"></a>Wiederherstellung der Datenträger
Verwenden Sie das Cmdlet **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**, um Daten und die Konfiguration eines Sicherungselements auf einem Wiederherstellungspunkt wiederherzustellen. Nachdem Sie einen Wiederherstellungspunkt ermittelt haben, verwenden Sie diesen als Wert für den **-RecoveryPoint**-Parameter. Im vorherigen Beispielcode wurde **$rp[0]** als Wiederherstellungspunkt ausgewählt, der verwendet werden soll. Im folgenden Beispielcode ist **$rp[0]** der Wiederherstellungspunkt, der zum Wiederherstellen des Datenträgers verwendet wird.

So stellen Sie den Datenträger und die Konfigurationsinformationen wieder her:

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Verwenden Sie das Cmdlet **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**, und warten Sie auf den Abschluss des Wiederherstellungsauftrags.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Nachdem der Wiederherstellungsauftrag abgeschlossen ist, verwenden Sie das Cmdlet **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**, um die Details des Wiederherstellungsvorgangs abzurufen. Die JobDetails-Eigenschaft enthält die Informationen, die zum erneuten Erstellen des virtuellen Computers benötigt werden.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Nachdem Sie die Datenträger wiederhergestellt haben, fahren Sie mit dem nächsten Abschnitt fort, um den virtuellen Computer zu erstellen.

## <a name="create-a-vm-from-restored-disks"></a>Erstellen eines virtuellen Computers aus wiederhergestellten Datenträgern
Nachdem Sie die Datenträger wiederhergestellt haben, verwenden Sie diese Schritte zum Erstellen und Konfigurieren des virtuellen Computers vom Datenträger.

> [!NOTE]
> Zum Erstellen von verschlüsselten virtuellen Computern aus wiederhergestellten Datenträgern muss Ihre Azure-Rolle über die Berechtigung zum Ausführen der Aktion **Microsoft.KeyVault/vaults/deploy/action** verfügen. Wenn Ihre Rolle nicht über diese Berechtigung verfügt, erstellen Sie mit dieser Aktion eine benutzerdefinierte Rolle. Weitere Informationen finden Sie unter [Benutzerdefinierte Rollen in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).
>
>

1. Fragen Sie die Eigenschaften des wiederhergestellten Datenträgers für die Auftragsdetails ab.

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. Legen Sie den Azure-Speicherkontext fest, und stellen Sie die JSON-Konfigurationsdatei wieder her.

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. Verwenden Sie die JSON-Konfigurationsdatei, um die Konfiguration des virtuellen Computers zu erstellen.

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. Fügen Sie die Betriebssystemdatenträger und Datenträger hinzu. Abhängig von der Konfiguration Ihrer virtuellen Computer finden Sie in den entsprechenden Abschnitten Informationen zu den jeweiligen Cmdlets:

    #### <a name="non-managed-non-encrypted-vms"></a>Nicht verwaltete unverschlüsselte VMs

    Verwenden Sie das folgende Beispiel für nicht verwaltete unverschlüsselte VMs.

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a>Nicht verwaltete, verschlüsselte VMs (nur BEK)

    Für nicht verwaltete, verschlüsselte VMs (nur mit BEK verschlüsselt) müssen Sie das Geheimnis für den Schlüsseltresor wiederherstellen, bevor Sie Datenträger anfügen können. Weitere Informationen finden Sie im Artikel [Wiederherstellen eines virtuellen Azure-Computers mithilfe eines Azure Backup-Wiederherstellungspunkts](backup-azure-restore-key-secret.md). Im folgenden Beispiel wird gezeigt, wie man das Betriebssystem und die Datenträger für verschlüsselte VMs anfügt.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a>Nicht verwaltete, verschlüsselte VMs (BEK und KEK)

    Für nicht verwaltete, verschlüsselte VMs (mit BEK und KEK verschlüsselt) müssen Sie den Schlüssel und das Geheimnis für den Schlüsseltresor wiederherstellen, bevor Sie Datenträger anfügen können. Weitere Informationen finden Sie im Artikel [Wiederherstellen eines virtuellen Azure-Computers mithilfe eines Azure Backup-Wiederherstellungspunkts](backup-azure-restore-key-secret.md). Im folgenden Beispiel wird gezeigt, wie man das Betriebssystem und die Datenträger für verschlüsselte VMs anfügt.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="managed-non-encrypted-vms"></a>Verwaltete unverschlüsselte VMs

    Für verwaltete unverschlüsselte VMs müssen Sie verwaltete Datenträger aus Blobspeicher erstellen und die Datenträger anschließend anfügen. Ausführliche Informationen finden Sie im Artikel [Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell](../virtual-machines/windows/attach-disk-ps.md). Der folgende Beispielcode zeigt, wie die Datenträger für verwaltete unverschlüsselte VMs anfügt werden.

    ```
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="managed-encrypted-vms-bek-and-kek"></a>Verwaltete, verschlüsselte VMs (BEK und KEK)

    Für verwaltete, verschlüsselte VMs (mit BEK und KEK verschlüsselt) müssen Sie verwaltete Datenträger aus dem Blobspeicher erstellen und anschließend die Datenträger anfügen. Ausführliche Informationen finden Sie im Artikel [Anfügen eines Datenträgers an einen virtuellen Windows-Computer mithilfe von PowerShell](../virtual-machines/windows/attach-disk-ps.md). Der folgende Beispielcode zeigt, wie man die Datenträger für verwaltete verschlüsselte VMs anfügt.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

5. Legen Sie die Netzwerkeinstellungen fest.

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Erstellen Sie den virtuellen Computer.

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="next-steps"></a>Nächste Schritte
Falls Sie PowerShell für die Arbeit mit Azure-Ressourcen vorziehen, lesen Sie den PowerShell-Artikel [Sicherungen für Windows Server bereitstellen und verwalten](backup-client-automation.md). Wenn Sie DPM-Sicherungen verwalten, finden Sie im Artikel [Bereitstellen und Verwalten der Sicherung für DPM](backup-dpm-automation.md) weitere Informationen. Diese beiden Artikel weisen eine Version für Resource Manager-Bereitstellungen und eine für klassische Bereitstellungen auf.  

