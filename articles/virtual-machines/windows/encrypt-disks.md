---
title: "Verschlüsseln von Datenträgern auf einem virtuellen Windows-Computer in Azure | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie virtuelle Datenträger auf einem virtuellen Windows-Computer mithilfe von Azure PowerShell verschlüsseln, um die Sicherheit zu erhöhen."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 04/24/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: 84cdc5eec5567c9c6905eee285afd2426607ff08
ms.lasthandoff: 04/27/2017


---
# <a name="how-to-encrypt-virtual-disks-on-a-windows-vm"></a>Verschlüsseln virtueller Datenträger auf einem virtuellen Windows-Computer
Zum Verbessern der Sicherheit und Compliance von virtuellen Computern können virtuelle Datenträger in Azure verschlüsselt werden. Die Verschlüsselung der Datenträger basiert auf kryptografischen Schlüsseln, die in Azure Key Vault gesichert werden. Diese kryptografischen Schlüssel werden von Ihnen kontrolliert, und Sie können deren Verwendung überwachen. In diesem Artikel wird erläutert, wie Sie virtuelle Datenträger auf einem virtuellen Windows-Computer mithilfe von Azure PowerShell verschlüsseln. Sie können auch [Datenträger auf einem virtuellen Linux-Computer mithilfe von Azure CLI 2.0 verschlüsseln](../linux/encrypt-disks.md).

## <a name="overview-of-disk-encryption"></a>Übersicht über die Datenträgerverschlüsselung
Virtuelle Datenträger auf virtuellen Windows-Computern werden im Ruhezustand mithilfe von BitLocker verschlüsselt. Für die Verschlüsselung virtueller Datenträger in Azure fallen keine Gebühren an. Kryptografische Schlüssel werden in Azure Key Vault mit Softwareschutz gespeichert. Alternativ können Sie Schlüssel aber auch in Hardwaresicherheitsmodulen (HSMs) mit FIPS 140-2 Level 2-Zertifizierung importieren oder generieren. Diese kryptografischen Schlüssel werden allein von Ihnen kontrolliert, und Sie können deren Verwendung überwachen. Die kryptografischen Schlüssel dienen zum Verschlüsseln und Entschlüsseln virtueller Datenträger, die an Ihren virtuellen Computer angefügt sind. Über einen Azure Active Directory-Dienstprinzipal werden die kryptografischen Schlüssel beim Ein- und Ausschalten virtueller Computer sicher ausgegeben.

Gehen Sie zum Verschlüsseln eines virtuellen Computers wie folgt vor:

1. Erstellen Sie in einer Azure Key Vault-Instanz einen kryptografischen Schlüssel.
2. Konfigurieren Sie den kryptografischen Schlüssel so, dass er zum Verschlüsseln von Datenträgern verwendet werden kann.
3. Erstellen Sie einen Azure Active Directory-Dienstprinzipal mit geeigneten Berechtigungen, um den kryptografischen Schlüssel aus der Azure Key Vault-Instanz zu lesen.
4. Führen Sie den Befehl zum Verschlüsseln Ihrer virtuellen Datenträger aus. Geben Sie dabei den Azure Active Directory-Dienstprinzipal und den geeigneten kryptografischen Schlüssel an.
5. Der Azure Active Directory-Dienstprinzipal fordert den erforderlichen kryptografischen Schlüssel von Azure Key Vault an.
6. Die virtuellen Datenträger werden mithilfe des angegebenen kryptografischen Schlüssels verschlüsselt.

## <a name="encryption-process"></a>Verschlüsselungsvorgang
Für die Datenträgerverschlüsselung werden folgende zusätzliche Komponenten benötigt:

* **Azure Key Vault:** Schützt die für die Datenträgerverschlüsselung/-entschlüsselung verwendeten kryptografischen und geheimen Schlüssel. 
  * Sie können eine ggf. bereits vorhandene Azure Key Vault-Instanz verwenden. Für die Datenträgerverschlüsselung wird keine dedizierte Key Vault-Instanz benötigt.
  * Zur Trennung administrativer Grenzen und der Schlüsselsichtbarkeit können Sie eine dedizierte Key Vault-Instanz erstellen.
* **Azure Active Directory:** Wickelt den sicheren Austausch der erforderlichen kryptografischen Schlüssel und die Authentifizierung für angeforderte Aktionen ab. 
  * In der Regel können Sie Ihre Anwendung in einer bereits vorhandenen Instanz von Azure Active Directory platzieren.
  * Der Dienstprinzipal bietet einen sicheren Mechanismus zum Anfordern und Ausgeben der geeigneten kryptografischen Schlüssel. Sie entwickeln im eigentlichen Sinne keine Anwendung, die in Azure Active Directory integriert wird.

## <a name="requirements-and-limitations"></a>Voraussetzungen und Einschränkungen
Unterstützte Szenarien und Voraussetzungen für die Datenträgerverschlüsselung:

* Aktivieren der Verschlüsselung auf neuen virtuellen Windows-Computern über Azure Marketplace-Images oder benutzerdefinierte VHD-Images
* Aktivieren der Verschlüsselung auf vorhandenen virtuellen Windows-Computern in Azure
* Aktivieren der Verschlüsselung auf virtuellen Windows-Computern, die mithilfe von Speicherplätzen konfiguriert sind
* Deaktivieren der Verschlüsselung auf Betriebssystem- und Datenlaufwerken für virtuelle Windows-Computer
* Alle Ressourcen (wie Key Vault, Speicherkonto und virtueller Computer) müssen sich in der gleichen Azure-Region und im gleichen Abonnement befinden.
* Virtuelle Computer im Standard-Tarif, z.B. virtuelle Computer der Reihen A, D, DS, G und GS

Die Datenträgerverschlüsselung wird derzeit in folgenden Szenarien nicht unterstützt:

* Bei virtuellen Computern des Basic-Tarifs
* Bei virtuellen Computern, die mit dem klassischen Bereitstellungsmodell erstellt wurden
* Bei Aktualisierung der kryptografischen Schlüssel auf einem bereits verschlüsselten virtuellen Computer
* Bei Integration in den lokalen Schlüsselverwaltungsdienst

## <a name="create-azure-key-vault-and-keys"></a>Erstellen der Azure Key Vault-Instanz und der Schlüssel
Stellen Sie vor Beginn sicher, dass die neueste Version des Azure PowerShell-Moduls installiert ist. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azure/overview). Ersetzen Sie bei den Befehlsbeispielen alle Beispielparameter durch Ihre eigenen Namen, Orte und Schlüsselwerte. In den folgenden Beispielen wird die Konvention `myResourceGroup`, `myKeyVault`, `myVM` usw. verwendet.

Erstellen Sie zum Speichern Ihrer kryptografischen Schlüssel zunächst eine Azure Key Vault-Instanz. In Azure Key Vault können Schlüssel, geheime Schlüssel und Kennwörter gespeichert werden, um eine sichere Implementierung in Anwendungen und Diensten zu ermöglichen. Bei der Verschlüsselung virtueller Datenträger erstellen Sie einen Schlüsseltresor zum Speichern eines kryptografischen Schlüssels, der zum Verschlüsseln oder Entschlüsseln der virtuellen Datenträger verwendet wird. 

Aktivieren Sie in Ihrem Azure-Abonnement den Azure Key Vault-Anbieter, und erstellen Sie eine Ressourcengruppe. Im folgenden Beispiel wird eine Ressourcengruppe namens `myResourceGroup` am Standort `West US` erstellt:

```powershell
$rgName = "myResourceGroup"
$location = "West US"

Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.KeyVault"
New-AzureRmResourceGroup -Location $location -Name $rgName
```

Die Azure Key Vault-Instanz mit den kryptografischen Schlüsseln und die dazugehörigen Computeressourcen wie Speicher und der eigentliche virtuelle Computer müssen sich in der gleichen Region befinden. Erstellen Sie eine Azure Key Vault-Instanz, und aktivieren Sie diese zur Verwendung der Datenträgerverschlüsselung. Geben Sie einen eindeutigen Namen Schlüsseltresornamen für `keyVaultName` an, wie nachfolgend gezeigt:

```powershell
$keyVaultName = "myUniqueKeyVaultName"
New-AzureRmKeyVault -Location $location -ResourceGroupName $rgName -VaultName $keyVaultName -EnabledForDiskEncryption
```

Kryptografische Schlüssel können mit Softwareschutz oder mit HSM-Schutz (Hardwaresicherheitsmodul) gespeichert werden. Für die Verwendung eines HSMs wird eine Key Vault-Premiuminstanz benötigt. Die Erstellung einer Key Vault-Premiuminstanz ist im Gegensatz zur Verwendung einer Key Vault-Standardinstanz, bei der Schlüssel mit Softwareschutz gespeichert werden, mit zusätzlichen Kosten verbunden. Wenn Sie eine Key Vault-Premiuminstanz erstellen möchten, fügen Sie im vorherigen Schritt die `-Sku "Premium"`-Parameter hinzu. Im folgenden Beispiel werden softwaregeschützte Schlüssel verwendet, da wir eine Key Vault-Standardinstanz erstellt haben. 

Bei beiden Schutzmodellen muss der Azure-Plattform Zugriff gewährt werden, um beim Start des virtuellen Computers die kryptografischen Schlüssel anfordern und die virtuellen Datenträger entschlüsseln zu können. Erstellen Sie in der Key Vault-Instanz einen kryptografischen Schlüssel. Im folgenden Beispiel wird ein Schlüssel namens `myKey` erstellt:

```powershell
Add-AzureKeyVaultKey -VaultName $keyVaultName -Name "myKey" -Destination "Software"
```


## <a name="create-the-azure-active-directory-service-principal"></a>Erstellen des Azure Active Directory-Dienstprinzipals
Wenn virtuelle Datenträger verschlüsselt oder entschlüsselt werden, geben Sie ein Konto an, mit dem die Authentifizierung und der Austausch kryptografischer Schlüssel aus Key Vault abgewickelt werden. Dieses Konto, ein Azure Active Directory-Dienstprinzipal, ermöglicht es der Azure-Plattform, im Auftrag des virtuellen Computers die geeigneten kryptografischen Schlüssel anzufordern. In Ihrem Abonnement steht zwar eine Azure Active Directory-Standardinstanz zur Verfügung, viele Organisationen verwenden jedoch dedizierte Azure Active Directory-Verzeichnisse.

Erstellen Sie einen Dienstprinzipal in Azure Active Directory. Orientieren Sie sich bei der Angabe eines sicheren Kennworts an den [Kennwortrichtlinien und -einschränkungen in Azure Active Directory](../../active-directory/active-directory-passwords-policy.md):

```powershell
$appName = "My App"
$securePassword = "P@ssword!"
$app = New-AzureRmADApplication -DisplayName $appName -HomePage "https://myapp.contoso.com" `
    -IdentifierUris "https://contoso.com/myapp" -Password $securePassword
New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
```

Zum erfolgreichen Ver- und Entschlüsseln virtueller Datenträger müssen Berechtigungen für den in Key Vault gespeicherten kryptografischen Schlüssel festgelegt werden, damit der Azure Active Directory-Dienstprinzipal die Schlüssel lesen kann. Legen Sie die Berechtigungen für die Key Vault-Instanz fest:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyvaultName -ServicePrincipalName $app.ApplicationId `
    -PermissionsToKeys "all" -PermissionsToSecrets "all"
```


## <a name="create-virtual-machine"></a>Erstellen eines virtuellen Computers
Um den Verschlüsselungsvorgang zu testen, wird nun ein virtueller Computer erstellt. Im folgenden Beispiel wird unter Verwendung eines **Windows Server 2016 Datacenter**-Images der virtuelle Computer `myVM` erstellt:

```powershell
subnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork -ResourceGroupName $rgName -Location $location `
    -Name myVnet -AddressPrefix 192.168.0.0/16 -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress -ResourceGroupName $rgName -Location $location `
    -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name "mypublicdns$(Get-Random)"

$nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
    -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 3389 -Access Allow

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface -Name myNic -ResourceGroupName $rgName -Location $location `
    -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

$cred = Get-Credential

$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize Standard_D1 | `
Set-AzureRmVMOperatingSystem -Windows -ComputerName myVM -Credential $cred | `
Set-AzureRmVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzureRmVMNetworkInterface -Id $nic.Id

New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vmConfig
```


## <a name="encrypt-virtual-machine"></a>Verschlüsseln eines virtuellen Computers
Um die virtuellen Datenträger zu verschlüsseln, müssen alle vorherigen Komponenten miteinander kombiniert werden:

1. Geben Sie den Azure Active Directory-Dienstprinzipal und das Kennwort an.
2. Geben Sie die Key Vault-Instanz zum Speichern der Metadaten Ihrer verschlüsselten Datenträger an.
3. Geben Sie die kryptografischen Schlüssel für die eigentliche Ver- und Entschlüsselung an.
4. Geben Sie an, ob Sie den Betriebssystem-Datenträger, die allgemeinen Datenträger oder alles verschlüsseln möchten.

Verschlüsseln Sie den virtuellen Computer unter Verwendung des Azure Key Vault-Schlüssels und der Anmeldeinformationen für den Azure Active Directory-Dienstprinzipal. Im folgenden Beispiel werden alle Schlüsselinformationen abgerufen und anschließend der virtuelle Computer `myVM` verschlüsselt:

```powershell
$keyVault = Get-AzureRmKeyVault -VaultName $keyVaultName -ResourceGroupName $rgName;
$diskEncryptionKeyVaultUrl = $keyVault.VaultUri;
$keyVaultResourceId = $keyVault.ResourceId;
$keyEncryptionKeyUrl = (Get-AzureKeyVaultKey -VaultName $keyVaultName -Name myKey).Key.kid;

Set-AzureRmVMDiskEncryptionExtension -ResourceGroupName $rgName -VMName $vmName -AadClientID $app.ApplicationId `
    -AadClientSecret $securePassword -DiskEncryptionKeyVaultUrl $diskEncryptionKeyVaultUrl `
    -DiskEncryptionKeyVaultId $keyVaultResourceId -KeyEncryptionKeyUrl $keyEncryptionKeyUrl `
    -KeyEncryptionKeyVaultId $keyVaultResourceId
```

Bestätigen Sie die Aufforderung zum Fortsetzen der Verschlüsselung des virtuellen Computers. Der virtuelle Computer wird während des Vorgangs neu gestartet. Nach Abschluss des Verschlüsselungsvorgangs und dem Neustart des virtuellen Computers können Sie den Verschlüsselungsstatus überprüfen:

```powershell
Get-AzureRmVmDiskEncryptionStatus  -ResourceGroupName $rgName -VMName $vmName
```

Die Ausgabe sieht in etwa wie das folgende Beispiel aus:

```powershell
OsVolumeEncrypted          : Encrypted
DataVolumesEncrypted       : Encrypted
OsVolumeEncryptionSettings : Microsoft.Azure.Management.Compute.Models.DiskEncryptionSettings
ProgressMessage            : OsVolume: Encrypted, DataVolumes: Encrypted
```

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum Verwalten von Azure Key Vault finden Sie unter [Einrichten von Key Vault für virtuelle Computer](key-vault-setup.md).
* Weitere Informationen zur Datenträgerverschlüsselung (etwa zum Vorbereiten des Uploads eines verschlüsselten benutzerdefinierten virtuellen Computers in Azure) finden Sie unter [Azure-Datenträgerverschlüsselung für virtuelle Windows- und Linux-IaaS-Computer](../../security/azure-security-disk-encryption.md).

