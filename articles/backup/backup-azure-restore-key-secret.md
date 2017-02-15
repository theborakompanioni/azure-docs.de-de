---
title: "Wiederherstellen von Key Vault-Schlüssel und -Geheimnis für verschlüsselte virtuelle Computer mithilfe von Azure Backup | Microsoft Docs"
description: "Informationen zum Wiederherstellen von Key Vault-Schlüssel und -Geheimschlüssel in Azure Backup mithilfe von PowerShell"
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 45214083-d5fc-4eb3-a367-0239dc59e0f6
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2016
ms.author: pajosh
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 0c5b2969ddc943b2b15826003f5a9e686e84e1c4


---
# <a name="restore-key-vault-key-and-secret-for-encrypted-vms-using-azure-backup"></a>Wiederherstellen von Key Vault-Schlüssel und -Geheimschlüssel für verschlüsselte virtuelle Computer mithilfe von Azure Backup
Dieser Artikel befasst sich mit Azure VM Backup für die Wiederherstellung von verschlüsselten Azure-VMs, wenn Schlüssel und Geheimschlüssel nicht im Schlüsseltresor vorhanden sind. Diese Schritte können auch verwendet werden, wenn Sie eine separate Kopie von Schlüssel (Key Encryption Key) und Geheimschlüssel (BitLocker-Verschlüsselungsschlüssel) für den wiederhergestellten virtuellen Computer verwalten möchten.

## <a name="pre-requisites"></a>Voraussetzungen
1. **Sicherung verschlüsselter VMs** – Verschlüsselte Azure-VMs wurden mithilfe von Azure Backup gesichert. Finden Sie im Artikel [Sichern und Wiederherstellen von Azure-VMs mithilfe von PowerShell](backup-azure-vms-automation.md) ausführliche Informationen zur Sicherung verschlüsselter Azure-VMs.
2. **Konfigurieren von Azure Key Vault** – Stellen Sie sicher, dass der Schlüsseltresor, für den Schlüssel und Geheimschlüssel wiederhergestellt werden müssen, bereits vorhanden ist. Finden Sie im Artikel [Erste Schritte mit Azure Key Vault](../key-vault/key-vault-get-started.md) ausführliche Informationen zum Verwalten des Schlüsseltresors.

## <a name="setup-recovery-services-vault"></a>Recovery Services-Tresor einrichten
Gehen Sie folgendermaßen vor, um sich bei PowerShell anzumelden und den Recovery Services Tresorkontext festzulegen.

### <a name="log-in-to-azure-powershell"></a>Anmelden an Azure PowerShell
Melden Sie sich beim Azure-Konto mit dem folgenden Cmdlet an.

```
PS C:\> Login-AzureRmAccount
```

### <a name="set-recovery-services-vault-context"></a>Festlegen des Kontexts des Recovery Services-Tresors
Sobald Sie angemeldet sind, verwenden Sie das folgende Cmdlet zum Abrufen der Liste der verfügbaren Abonnements.

```
PS C:\> Get-AzureRmSubscription
```

Wählen Sie das Abonnement, in dem Ressourcen verfügbar sind.

```
PS C:\> Set-AzureRmContext -SubscriptionId "<subscription-id>"
```

Legen Sie den Tresorkontext mithilfe von Recovery Services-Tresor fest, bei dem eine Sicherung für verschlüsselte virtuelle Computer aktiviert wurde.

```
PS C:\> Get-AzureRmRecoveryServicesVault -ResourceGroupName "<rg-name>" -Name "<rs-vault-name>" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="get-recovery-point"></a>Abrufen eines Wiederherstellungspunkts
Wählen Sie den Container im Tresor, der den verschlüsselten virtuellen Azure-Computer darstellt.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -Name "<vm-name>"
```

Mithilfe dieses Containers rufen Sie das Sicherungselement für den entsprechenden virtuellen Computer ab.

```
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
```

Rufen Sie ein Array von Wiederherstellungspunkten für das ausgewählte Sicherungselement in der Variablen rp ab.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
```

## <a name="restore-encrypted-virtual-machine"></a>Verschlüsselten virtuellen Computer wiederherstellen
Gehen Sie folgendermaßen vor, um den verschlüsselten virtuellen Computer, seinen Schlüssel und geheimen Schlüssel wiederherzustellen.

### <a name="restore-key"></a>Schlüssel wiederherstellen
Das Array „$rp“ oben wird in umgekehrter Reihenfolge sortiert. Der letzte Wiederherstellungspunkt liegt bei Index 0. Beispiel: „$rp[0]“ wählt den letzten Wiederherstellungspunkt aus.

```
PS C:\> $rp1 = Get-AzureRmRecoveryServicesBackupRecoveryPoint -RecoveryPointId $rp[0].RecoveryPointId -Item $backupItem -KeyFileDownloadLocation "C:\Users\downloads"
```

> [!NOTE]
> Nachdem dieses Cmdlet erfolgreich ausgeführt wurde, wird eine Blob-Datei im angegebenen Ordner auf dem gleichen Computer generiert. Diese Blobdatei stellt den Schlüssel in verschlüsselter Form dar.
> 
> 

Stellen Sie den Schlüssel zum Schlüsseltresor mit dem folgenden Cmdlet wieder her. 

```
PS C:\> Restore-AzureKeyVaultKey -VaultName "contosokeyvault" -InputFile "C:\Users\downloads\key.blob"
```

### <a name="restore-secret"></a>Geheimen Schlüssel wiederherstellen
Stellen Sie die Daten des geheimen Schlüssels vom oben abgerufenen Wiederherstellungspunkt wieder her.

```
PS C:\> $rp1.KeyAndSecretDetails.SecretUrl

https://contosokeyvault.vault.azure.net/secrets/B3284AAA-DAAA-4AAA-B393-60CAA848AAAA/20aaae9eaa99996d89d99a29990d999a
```

> [!NOTE]
> Der Text vor „vault.azure.net“ steht für den ursprünglichen Schlüsseltresornamen. Der Text nach „secrets/“ steht für den Geheimschlüsselnamen. 
> 
> 

Erhalten Sie den Namen und Wert des geheimen Schlüssels aus der Ausgabe des Cmdlets, das oben ausgeführt wurde, für den Fall, dass Sie den gleichen Namen des geheimen Schlüssels verwenden möchten. In anderen Fällen sollte „$secretname“ unten aktualisiert werden, um den neuen Namen des geheimen Schlüssels zu verwenden. 

```
PS C:\> $secretname = "B3284AAA-DAAA-4AAA-B393-60CAA848AAAA"
PS C:\> $secretdata = $rp1.KeyAndSecretDetails.SecretData
PS C:\> $Secret = ConvertTo-SecureString -String $secretdata -AsPlainText -Force
```

Setzen Sie Tags für den geheimen Schlüssel, falls der virtuelle Computer auch wiederhergestellt werden muss. Beim Tag „DiskEncryptionKeyFileName“ sollte der Wert den Namen des geheimen Schlüssels erhalten, den Sie verwenden möchten. 

```
PS C:\> $Tags = @{'DiskEncryptionKeyEncryptionAlgorithm' = 'RSA-OAEP';'DiskEncryptionKeyFileName' = 'B3284AAA-DAAA-4AAA-B393-60CAA848AAAA.BEK';'DiskEncryptionKeyEncryptionKeyURL' = 'https://contosokeyvault.vault.azure.net:443/keys/KeyName/84daaac999949999030bf99aaa5a9f9';'MachineName' = 'vm-name'}
```

> [!NOTE]
> Der Wert für „DiskEncryptionKeyFileName“ ist identisch mit dem Namen des geheimen Schlüssels, den Sie oben abgerufen haben. Der Wert für „DiskEncryptionKeyEncryptionKeyURL“ kann mithilfe des Cmdlet [Get-AzureKeyVaultKey](https://msdn.microsoft.com/library/dn868053.aspx) aus dem Schlüsseltresor abgerufen werden, nachdem die Schlüssel wiederhergestellt wurden.    
> 
> 

Setzen Sie den geheimen Schlüssel wieder auf den Schlüsseltresor zurück.

```
PS C:\> Set-AzureKeyVaultSecret -VaultName "contosokeyvault" -Name $secretname -SecretValue $secret -Tags $Tags -SecretValue $Secret -ContentType  "Wrapped BEK"
```

### <a name="restore-virtual-machine"></a>Wiederherstellen des virtuellen Computers
Die oben aufgeführten PowerShell-Cmdlets unterstützen Sie, Schlüssel und geheime Schlüssel auf den Schlüsseltresor wiederherzustellen, wenn Sie einen verschlüsselten virtuellen Computer mit Azure VM Backup gesichert haben. Im Artikel [Sichern und Wiederherstellen von Azure-VMs mithilfe von PowerShell](backup-azure-vms-automation.md) finden Sie ausführliche Informationen zum Wiederherstellen verschlüsselter virtueller Computer.




<!--HONumber=Nov16_HO3-->


