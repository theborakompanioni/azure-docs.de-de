<properties
   pageTitle="Bereitstellen und Verwalten von Sicherungen für ARM-VMs mit PowerShell | Microsoft Azure"
   description="Verwenden Sie PowerShell zur Bereitstellung und Verwaltung von Sicherungen in Azure für ARM-VMs"
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""/>

<tags
   ms.service="backup"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="storage-backup-recovery"
   ms.date="05/09/2016"
   ms.author="markgal; trinadhk"/>

# Bereitstellen und Verwalten von Sicherungen für ARM-VMs mit PowerShell

> [AZURE.SELECTOR]
- [Resource Manager-PowerShell](backup-azure-vms-automation.md)
- [Klassische Azure PowerShell](backup-azure-vms-classic-automation.md)

In diesem Artikel erfahren Sie, wie Sie eine Azure-VM mit Azure PowerShell in einem Recovery Services-Tresor sichern und wiederherstellen. Ein Recovery Services-Tresor ist eine Ressource von Azure Resource Manager (ARM) und dient zum Schutz von Daten und Assets in den Diensten Azure Backup und Azure Site Recovery. Verwenden Sie einen Recovery Services-Tresor, wenn Sie in einer ARM-Bereitstellung arbeiten. Sie können einen Recovery Services-Tresor sowohl zum Schutz von VMs verwenden, die mit dem Azure Service Manager (ASM) bereitgestellt wurden, als auch zum Schutz von ARM-VMs.

>[AZURE.NOTE] Azure verfügt über zwei Bereitstellungsmodelle zum Erstellen und Verwenden von Ressourcen: [Resource Manager-Modell und klassisches Modell](../resource-manager-deployment-model.md). Dieser Artikel ist für die Verwendung mit virtuellen Computern erstellt, die mit dem Resource Manager-Modell bereitgestellt wurden.

In diesem Artikel erfahren Sie, wie Sie einen virtuellen Computer mit PowerShell schützen und Daten von einem Wiederherstellungspunkt aus wiederherstellen.

## Konzepte

Wenn Sie nicht mit dem Dienst Azure Backup vertraut sind, können Sie sich mit [Was ist Azure Backup?](backup-introduction-to-azure-backup.md) eine Übersicht über den Dienst verschaffen. Bevor Sie beginnen, stellen Sie sicher, dass Sie die Voraussetzungen zur Arbeit mit Azure Backup und die Einschränkungen der aktuellen Lösung zur Sicherung virtueller Computer im Wesentlichen kennen.

Damit Sie PowerShell effektiv nutzen können, ist es notwendig, dass Sie die Objekthierarchie verstehen und wissen, womit Sie beginnen sollten.

![Recovery Services-Objekthierarchie](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

## Einrichtung und Registrierung

Vorbereitung:

1. [Laden Sie die neueste PowerShell-Version herunter](https://github.com/Azure/azure-powershell/releases) (erforderliche Mindestversion: 1.0.0).

2. Ermitteln Sie die Azure-PowerShell-Cmdlets zur Datensicherung mithilfe des folgenden Befehls:

```
PS C:\WINDOWS\system32> Get-Command *azurermrecoveryservices*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmRecoveryServicesBackupItem           1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Disable-AzureRmRecoveryServicesBackupProtection    1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Enable-AzureRmRecoveryServicesBackupProtection     1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupContainer         1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupItem              1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJob               1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJobDetails        1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupManagementServer  1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupProperties        1.0.7      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesBackupProtectionPolicy  1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRMRecoveryServicesBackupRecoveryPoint     1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupRetentionPolic... 1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupSchedulePolicy... 1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesVault                   1.0.7      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesVaultSettingsFile       1.0.7      AzureRM.RecoveryServices
Cmdlet          New-AzureRmRecoveryServicesBackupProtectionPolicy  1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          New-AzureRmRecoveryServicesVault                   1.0.7      AzureRM.RecoveryServices
Cmdlet          Remove-AzureRmRecoveryServicesProtectionPolicy     1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Remove-AzureRmRecoveryServicesVault                1.0.7      AzureRM.RecoveryServices
Cmdlet          Restore-AzureRMRecoveryServicesBackupItem          1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesBackupProperties        1.0.7      AzureRM.RecoveryServices
Cmdlet          Set-AzureRmRecoveryServicesBackupProtectionPolicy  1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesVaultContext            1.0.7      AzureRM.RecoveryServices
Cmdlet          Stop-AzureRmRecoveryServicesBackupJob              1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupContainer  1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupManagem... 1.0.0      AzureRM.RecoveryServices.Backup
Cmdlet          Wait-AzureRmRecoveryServicesBackupJob              1.0.0      AzureRM.RecoveryServices.Backup
```


Die folgenden Aufgaben können mit PowerShell automatisiert werden:

- Erstellen eines Recovery Services-Tresors
- Sichern oder Schützen von Azure-VMs
- Auslösen eines Sicherungsauftrags
- Überwachen eines Sicherungsauftrags
- Wiederherstellen eines virtuellen Azure-Computers

## Erstellen eines Recovery Services-Tresors

> [AZURE.TIP] Kunden, die Azure Backup zum ersten Mal verwenden, müssen den Azure Recovery Service-Anbieter registrieren, der mit ihrem Abonnement verwendet werden soll. Führen Sie hierzu den folgenden Befehl aus: Register-AzureRmResourceProvider -ProviderNamespace „Microsoft.RecoveryServices“.

Sie können einen neuen Recovery Services-Tresor mithilfe des **New-AzureRmRecoveryServicesVault**-Cmdlets erstellen. Der Recovery Services-Tresor ist eine ARM-Ressource. Deshalb müssen Sie ihn in eine Ressourcengruppe einfügen. Führen Sie die folgenden Befehle in einer Azure PowerShell-Konsole mit erhöhten Rechten aus:

```
PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
```

Um eine Liste aller Recovery Services-Tresore in einem Abonnement anzuzeigen, verwenden Sie das **Get-AzureRmRecoveryServicesVault**-Cmdlet.

### Festlegen der Speicherredundanz
Geben Sie beim Erstellen eines Recovery Services-Tresors den Typ der zu verwendenden Speicherredundanz an – lokal redundanter Speicher (LRS) oder geografisch redundanter Speicher (GRS). Das folgende Beispiel zeigt, dass für die Option BackupStorageRedundancy für testVault der Wert GeoRedundant festgelegt werden muss.

```
PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testVault"
PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
```


> [AZURE.TIP] Viele Azure Backup-Cmdlets benötigen das Recovery Services-Tresorobjekt als Eingabe. Aus diesem Grund sollte das zur Sicherung verwendete Recovery Services-Tresorobjekt in einer Variablen gespeichert werden.


## Sichern von virtuellen Azure-Computern

Vor dem Aktivieren des Schutzes auf einem virtuellen Computer müssen Sie den Tresorkontext festlegen. Der Kontext wird auf alle nachfolgenden Cmdlets angewendet.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name testvault | Set-AzureRmRecoveryServicesVaultContext
```

### Erstellen einer Schutzrichtlinie

Wenn Sie einen neuen Tresor erstellen, gilt für ihn eine Standardrichtlinie. Diese Richtlinie löst täglich um 21:30 Uhr einen Sicherungsauftrag aus. Die Sicherungsmomentaufnahme wird 30 Tage lang gespeichert. Mit der Standardrichtlinie können Sie Ihren virtuellen Computer schnell schützen und die Richtlinie später mit anderen Details bearbeiten.

Um die verfügbare Liste der Richtlinien im Tresor anzuzeigen, verwenden Sie das Get-AzureRmRecoveryServicesBackupProtectionPolicy-Cmdlet:

```
PS C:\WINDOWS\system32> get-AzureRMRecoveryServicesBackupProtectionPolicy -WorkloadType AzureVM
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [AZURE.NOTE] Die Zeitzone des Feldes „BackupTime“ in PowerShell ist UTC. Wenn jedoch der Zeitpunkt der Sicherung im Azure-Portal angezeigt wird, wird die Zeit auf Ihre lokale Zeitzone eingestellt.

Eine Sicherungsschutzrichtlinie ist mindestens einer Aufbewahrungsrichtlinie zugeordnet. Die Aufbewahrungsrichtlinie definiert, wie lange ein Wiederherstellungspunkt in Azure Backup gespeichert wird. Verwenden Sie Get-AzureRmRecoveryServicesBackupRetentionPolicyObject, um die standardmäßige Aufbewahrungsrichtlinie anzuzeigen. Ähnlich können Sie die standardmäßige Zeitplanrichtlinie mit Get-AzureRmRecoveryServicesBackupSchedulePolicyObject abrufen. Zeitplan- und Gruppenrichtlinienobjekte werden als Eingaben für das Cmdlet New-AzureRmRecoveryServicesBackupProtectionPolicy verwendet.

Eine Sicherungsschutzrichtlinie definiert, wann und wie oft die Sicherung eines Elements erfolgt. Das Cmdlet New-AzureRmRecoveryServicesBackupProtectionPolicy erstellt ein PowerShell-Objekt, das Informationen zu Sicherungsrichtlinien enthält. Die Sicherungsrichtlinie wird als Eingabe für das Cmdlet Enable-AzureRmRecoveryServicesBackupProtection verwendet.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\>  $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\>  New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType AzureVM -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```

### Schutz aktivieren

Zum Aktivieren des Schutzes sind zwei Objekte erforderlich – das Element und die Richtlinie. Beide Objekte sind erforderlich, um den Schutz für den Tresor zu aktivieren. Sobald die Richtlinie mit dem Element verknüpft ist, wird der Sicherungsworkflow zu dem im Richtlinienzeitplan definierten Zeitpunkt gestartet.

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

Für ASM-basierte virtuelle Computer

```
PS C:\>  $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\>  Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### Ändern einer Schutzrichtlinie

Um die Richtlinie zu ändern, ändern Sie das BackupSchedulePolicyObject oder BackupRetentionPolicy-Objekt, und ändern Sie die Richtlinie mithilfe von Set-AzureRmRecoveryServicesBackupProtectionPolicy.

Im folgenden Beispiel wird der Aufbewahrungswert in 365 geändert.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRMRecoveryServicesBackupProtectionPolicy -Name NewPolicy
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy  $RetPol
```

## Ausführen einer ersten Sicherung

Der Sicherungszeitplan löst bei der ersten Sicherung eine vollständige Sicherung für das Element aus. Nachfolgende Sicherungen sind inkrementelle Kopien. Wenn Sie die erste Sicherung zu einem bestimmten Zeitpunkt oder sogar sofort erzwingen möchten, verwenden Sie das Cmdlet Backup-AzureRmRecoveryServicesBackupItem:

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "V2VM";
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM";
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item;
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM        Backup               InProgress            4/23/2016 5:00:30 PM            cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [AZURE.HINWEIS: Die Zeitzone der Felder StartTime und EndTime in PowerShell ist UTC. Wenn jedoch der Zeitpunkt im Azure-Portal angezeigt wird, wird die Zeit auf Ihre lokale Zeitzone eingestellt.

## Überwachen eines Sicherungsauftrags

Die meisten Vorgänge mit langer Ausführungszeit werden in Azure Backup als Auftrag modelliert. So kann der Fortschritt auf einfache Weise nachverfolgt werden, ohne dass das Azure-Portal immer geöffnet bleiben muss.

Mit dem Cmdlet Get-AzureRMRecoveryservicesBackupJob können Sie den aktuellen Status eines Auftrags abrufen, der sich in Bearbeitung befindet.

```
PS C:\ > $joblist = Get-AzureRMRecoveryservicesBackupJob –Status InProgress
PS C:\ > $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM        Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Anstatt die Fertigstellung dieser Aufträge abzufragen – was unnötigen, zusätzlichen Code bedeutet – ist es einfacher, das Cmdlet Wait-AzureRmRecoveryServicesBackupJob zu verwenden. Bei Verwendung in einem Skript hält das Cmdlet die Ausführung an, bis entweder der Auftrag abgeschlossen oder der angegebene Timeoutwert erreicht ist.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## Wiederherstellen eines virtuellen Azure-Computers

Um die gesicherten Daten wiederherstellen zu können, müssen Sie das gesicherte Element und den Wiederherstellungspunkt angeben, der die Zeitpunktdaten enthält. Diese Informationen werden für das Cmdlet Restore-AzureRMRecoveryServicesBackupItem bereitgestellt, um eine Wiederherstellung von Daten aus dem Tresor im Konto des Kunden zu initiieren.

### Auswählen des virtuellen Computers

Zum Abrufen des PowerShell-Objekts, das das richtige Sicherungselement identifiziert, starten Sie vom Container im Tresor aus, und durchlaufen Sie die Objekthierarchie nach unten. Um den Container auszuwählen, der den virtuellen Computer darstellt, verwenden Sie das Cmdlet Get-AzureRmRecoveryServicesBackupContainer und reichen dieses an das Cmdlet Get-AzureRmRecoveryServicesBackupItem weiter.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType AzureVM –Status Registered -Name 'V2VM'
PS C:\> $backupitem=Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### Auswählen eines Wiederherstellungspunkts

Sie können jetzt alle Wiederherstellungspunkte für das Sicherungselement mithilfe des Cmdlets Get-AzureRMRecoveryServicesBackupRecoveryPoint auflisten und anschließend den Wiederherstellungspunkt auswählen. Üblicherweise wählen Benutzer den neuesten AppConsistent-Punkt in der Liste aus.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRMRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM
                              /recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

Die Variable „$rp“ ist ein Array mit Wiederherstellungspunkten für das ausgewählte Sicherungselement, und zwar in zeitlich umgekehrter Reihenfolge sortiert, d.h. der letzte Wiederherstellungspunkt befindet sich an Index 0. Verwenden Sie die standardmäßige PowerShell-Arrayindizierung zum Auswählen des Wiederherstellungspunkts. Beispiel: „$rp[0]“ wählt den letzten Wiederherstellungspunkt aus.

### Wiederherstellen von Datenträgern

Es besteht ein entscheidender Unterschied zwischen Wiederherstellungsvorgängen, die über das Azure-Portal ausgeführt werden, und Wiederherstellungsvorgängen, die mit Azure PowerShell ausgeführt werden. Mit PowerShell endet der Wiederherstellungsvorgang mit der Wiederherstellung der Datenträger und den Konfigurationsinformationen aus dem Wiederherstellungspunkt. Es wird kein virtueller Computer erstellt.

> [AZURE.WARNING] Restore-AzureRMRecoveryServicesBackupItem erstellt keinen virtuellen Computer, sondern stellt nur die Datenträger mit dem angegebenen Speicherkonto wieder her. Dies unterscheidet sich von dem, was im Azure-Portal geschieht.

```
PS C:\> $restorejob = Restore-AzureRMRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName DestAccount
 -StorageAccountResourceGroupName DestRG
PS C:\> $restorejob
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM        Restore               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Nachdem der Wiederherstellungsauftrag abgeschlossen ist, können Sie die Details des Wiederherstellungsvorgangs mit dem Cmdlet Get-AzureRmRecoveryServicesBackupJobDetails abrufen. Die JobDetails-Eigenschaft enthält die Informationen, die zum erneuten Erstellen des virtuellen Computers benötigt werden.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails
```

## Registrieren von Windows-Server oder DPM bei einem Recovery Services-Tresor

Laden Sie nach dem Erstellen des Recovery Services-Tresors den neuesten Agent sowie die Tresoranmeldeinformationen herunter, und speichern Sie sie an einem zentralen Ort wie C:\\Downloads.

```
PS C:\> $credspath = "C:\downloads"
PS C:\> $credsfilename = Get-AzureRmRecoveryServicesVaultSettingsFile -Backup -Vault $vault1 -Path  $credspath
PS C:\> $credsfilename
C:\downloads\testvault\_Sun Apr 10 2016.VaultCredentials
```

Führen Sie auf dem Windows-Server oder DPM-Server das [Start-OBRegistration](https://technet.microsoft.com/library/hh770398%28v=wps.630%29.aspx)-Cmdlet aus, um den Computer beim Tresor zu registrieren.

```
PS C:\> $cred = $credspath + $credsfilename
PS C:\> Start-OBRegistration-VaultCredentials $cred -Confirm:$false
CertThumbprint      :7a2ef2caa2e74b6ed1222a5e89288ddad438df2
SubscriptionID      : ef4ab577-c2c0-43e4-af80-af49f485f3d1
ServiceResourceName: testvault
Region              :West US
Machine registration succeeded.
```

<!---HONumber=AcomDC_0518_2016-->