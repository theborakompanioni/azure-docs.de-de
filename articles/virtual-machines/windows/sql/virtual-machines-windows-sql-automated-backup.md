---
title: "Automatisierte Sicherung für SQL Server 2014-VMs in -Azure | Microsoft-Dokumentation"
description: "Erläutert das Feature „Automatisierte Sicherung“ für SQL Server 2014-VMs, die in Azure ausgeführt werden. Dieser Artikel bezieht sich speziell auf VMs, die das Resource Manager-Modell verwenden."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: bdc63fd1-db49-4e76-87d5-b5c6a890e53c
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/24/2017
ms.author: jroth
translationtype: Human Translation
ms.sourcegitcommit: aaf97d26c982c1592230096588e0b0c3ee516a73
ms.openlocfilehash: dcb7b326cbb4ed18769010d8061f80dc9f941c87
ms.lasthandoff: 04/27/2017

---

# <a name="automated-backup-for-sql-server-2014-virtual-machines-resource-manager"></a>Automatisierte Sicherung für SQL Server 2014-VMs (Resource Manager)

> [!div class="op_single_selector"]
> * [SQL Server 2014](virtual-machines-windows-sql-automated-backup.md)
> * [SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md)

Die automatisierte Sicherung konfiguriert automatisch [Managed Backup für Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) für alle vorhandenen und neuen Datenbanken auf einer Azure-VM, auf der SQL Server 2014 Standard oder Enterprise ausgeführt wird. Dies bietet Ihnen die Möglichkeit, reguläre Datenbanksicherungen zu konfigurieren, die permanenten Azure-Blob-Speicher nutzen. Die automatische Sicherung basiert auf der [Erweiterung für den SQL Server-IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="prerequisites"></a>Voraussetzungen
Beachten Sie bei der Verwendung der automatisierten Sicherung die folgenden Voraussetzungen:

**Betriebssystem:**

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-Version/-Edition:**

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

> [!IMPORTANT]
> Die automatisierte Sicherung funktioniert mit SQL Server 2014. Bei Einsatz von SQL Server 2016 können Sie die automatisierte Sicherung v2 zum Sichern Ihrer Datenbanken verwenden. Weitere Informationen finden Sie unter [Automated Backup v2 for SQL Server 2016 Azure Virtual Machines (Resource Manager)](virtual-machines-windows-sql-automated-backup-v2.md) (Automatisierte Sicherung v2 für SQL Server 2016-VMs in Azure [Resource Manager]).

**Datenbankkonfiguration**:

- Zieldatenbanken müssen das vollständige Wiederherstellungsmodell verwenden. Weitere Informationen zu den Auswirkungen des vollständigen Wiederherstellungsmodells auf Sicherungen finden Sie unter [Sichern beim vollständigen Wiederherstellungsmodell](https://technet.microsoft.com/library/ms190217.aspx).
- Zieldatenbanken müssen die Standardinstanz von SQL Server aufweisen. Die SQL Server-IaaS-Erweiterung unterstützt keine benannten Instanzen.

**Azure-Bereitstellungsmodell:**

- Ressourcen-Manager

**Azure PowerShell**:

- [Installieren Sie die aktuellen Azure PowerShell-Befehle](/powershell/azure/overview) , wenn Sie die automatisierte Sicherung mit PowerShell konfigurieren möchten.

> [!NOTE]
> Die automatisierte Sicherung basiert auf der Erweiterung für den SQL Server-IaaS-Agent. Aktuelle Katalogimages für virtuelle SQL-Computer fügen diese Erweiterung standardmäßig hinzu. Weitere Informationen finden Sie unter [Erweiterung für SQL Server-IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="settings"></a>Einstellungen

In der folgenden Tabelle werden die Optionen beschrieben, die für die automatisierte Sicherung konfiguriert werden können. Die tatsächlichen Konfigurationsschritte variieren abhängig davon, ob Sie das Azure-Portal oder Azure Windows PowerShell-Befehle verwenden.

| Einstellung | Bereich (Standard) | Beschreibung |
| --- | --- | --- |
| **Automatisierte Sicherung** | Aktivieren/Deaktivieren (deaktiviert) | Aktiviert oder deaktiviert die automatisierte Sicherung für eine Azure-VM mit SQL Server 2014 Standard oder Enterprise. |
| **Aufbewahrungszeitraum** | 1 bis 30 Tage (30 Tage) | Die Anzahl von Tagen, für die eine Sicherung aufbewahrt wird. |
| **Speicherkonto** | Azure-Speicherkonto | Ein Azure-Speicherkonto, mit dem Dateien der automatisierten Sicherung im Blob-Speicher gespeichert werden. An diesem Speicherort wird ein Container zum Speichern aller Sicherungsdateien erstellt. Die Namenskonvention für die Sicherungsdatei enthält das Datum, die Uhrzeit und den Computernamen. |
| **Verschlüsselung** | Aktivieren/Deaktivieren (deaktiviert) | Aktiviert oder deaktiviert die Verschlüsselung. Wenn die Verschlüsselung aktiviert ist, befinden sich die Zertifikate zum Wiederherstellen der Sicherung im angegebenen Speicherkonto im gleichen `automaticbackup`-Container (mit der gleichen Namenskonvention). Wenn das Kennwort geändert wird, wird ein neues Zertifikat mit diesem Kennwort generiert, das alte Zertifikat bleibt jedoch zum Wiederherstellen vorheriger Sicherungen erhalten. |
| **Kennwort** | Kennworttext | Ein Kennwort für Verschlüsselungsschlüssel. Ein Kennwort ist nur erforderlich, wenn die Verschlüsselung aktiviert ist. Um eine verschlüsselte Sicherung wiederherzustellen, benötigen Sie das richtige Kennwort und das zugehörige Zertifikat, das beim Erstellen der Sicherung verwendet wurde. |

## <a name="configuration-in-the-portal"></a>Konfiguration im Portal

Mit dem Azure-Portal können Sie die automatisierte Sicherung während der Bereitstellung oder für vorhandene virtuelle SQL Server 2014-Computer konfigurieren.

### <a name="new-vms"></a>Neue virtuelle Computer

Verwenden Sie das Azure-Portal zum Konfigurieren der automatisierten Sicherung, wenn Sie einen neuen virtuellen Computer mit SQL Server 2014 im Resource Manager-Bereitstellungsmodell erstellen.

Wählen Sie auf dem Blatt **SQL Server-Einstellungen** die Option **Automatisierte Sicherung** aus. Auf dem folgenden Screenshot des Azure-Portals sehen Sie das Blatt **Automatisierte SQL-Sicherung** .

![Konfigurieren der automatisierten SQL-Sicherung im Azure-Portal](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Wenn Sie weiteren Kontext benötigen, lesen Sie das vollständige Thema zum [Bereitstellen eines virtuellen Computers mit SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Vorhandene virtuelle Computer

Wählen Sie für vorhandene virtuelle Computer mit SQL Server Ihren virtuellen Computer mit SQL Server aus. Wählen Sie dann auf dem Blatt **Einstellungen** den Abschnitt **SQL Server-Konfiguration** aus.

![Automatisierte SQL-Sicherung für vorhandene virtuelle Computer](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

Klicken Sie auf dem Blatt **SQL Server-Konfiguration** im Abschnitt für die automatisierte Sicherung auf die Schaltfläche **Bearbeiten**.

![Konfigurieren der automatisierten SQL-Sicherung für vorhandene virtuelle Computer](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Klicken Sie abschließend unten auf dem Blatt **SQL Server-Konfiguration** auf die Schaltfläche **OK**, um die Änderungen zu speichern.

Falls Sie die automatisierte Sicherung zum ersten Mal aktivieren, konfiguriert Azure den SQL Server-IaaS-Agent im Hintergrund. Im Azure-Portal wird währenddessen u.U. nicht angezeigt, dass die automatisierte Sicherung konfiguriert wird. Warten Sie einige Minuten, bis der Agent installiert und konfiguriert wurde. Danach werden die neuen Einstellungen im Azure-Portal angezeigt.

> [!NOTE]
> Sie können die automatisierte Sicherung auch mithilfe einer Vorlage konfigurieren. Weitere Informationen finden Sie unter [Azure quickstart template for Automated Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update)(Azure-Schnellstartvorlage für die automatisierte Sicherung).

## <a name="configuration-with-powershell"></a>Konfiguration mit PowerShell

Die automatisierte Sicherung kann mithilfe von PowerShell konfiguriert werden. Bevor Sie beginnen, müssen Sie Folgendes tun:

- [Laden Sie die aktuelle Version von Azure PowerShell herunter, und installieren Sie sie.](http://aka.ms/webpi-azps)
- Öffnen Sie Windows PowerShell, und ordnen Sie es Ihrem Konto zu. Befolgen Sie hierfür die Schritte im Abschnitt [Konfigurieren Ihres Abonnements](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-ps-sql-create#configure-your-subscription) des Bereitstellungsthemas.

### <a name="install-the-sql-iaas-extension"></a>Installieren der SQL-IaaS-Erweiterung
Wenn Sie einen virtuellen SQL Server-Computer über das Azure-Portal bereitgestellt haben, müsste die SQL Server-IaaS-Erweiterung bereits installiert sein. Rufen Sie den Befehl **Get-AzureRmVM** auf, und sehen Sie sich die Eigenschaft **Extensions** an, um zu ermitteln, ob sie für Ihren virtuellen Computer installiert ist.

```powershell
$vmname = "vmname"
$resourcegroupname = "resourcegroupname"

(Get-AzureRmVM -Name $vmname -ResourceGroupName $resourcegroupname).Extensions
```

Wenn die SQL Server-IaaS-Agent-Erweiterung installiert ist, wird „SqlIaaSAgent“ oder „SQLIaaSExtension“ aufgeführt. Außerdem muss für die Erweiterung unter **ProvisioningState** der Zustand „Succeeded“ angezeigt werden.

Falls die Erweiterung nicht installiert oder nicht erfolgreich bereitgestellt wurde, können Sie sie mithilfe des folgenden Befehls installieren. Neben dem Namen des virtuellen Computers und der Ressourcengruppe müssen Sie auch die Region (**$region**) angeben, in der sich Ihr virtueller Computer befindet.

```powershell
$region = “EASTUS2”
Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region
```

### <a id="verifysettings"></a> Überprüfen der aktuellen Einstellungen

Wenn Sie die automatisierte Sicherung während der Bereitstellung aktiviert haben, können Sie mithilfe von PowerShell Ihre aktuelle Konfiguration überprüfen. Führen Sie den Befehl **Get-AzureRmVMSqlServerExtension** aus, und sehen Sie sich die Eigenschaft **AutoBackupSettings** an:

```powershell
(Get-AzureRmVMSqlServerExtension -VMName $vmname -ResourceGroupName $resourcegroupname).AutoBackupSettings
```

Die Ausgabe sollte in etwa wie folgt aussehen:

```
Enable                      : False
EnableEncryption            : False
RetentionPeriod             : -1
StorageUrl                  : NOTSET
StorageAccessKey            : 
Password                    : 
BackupSystemDbs             : False
BackupScheduleType          : 
FullBackupFrequency         : 
FullBackupStartTime         : 
FullBackupWindowHours       : 
LogBackupFrequency          : 
```

Falls Sie in der Ausgabe sehen, dass **Enable** auf **False** festgelegt ist, müssen Sie die automatisierte Sicherung aktivieren. Die gute Nachricht: Die Vorgehensweise zum Aktivieren der automatisierten Sicherung unterscheidet sich nicht von der Vorgehensweise zum Konfigurieren. Entsprechende Informationen finden Sie im nächsten Abschnitt.

> [!NOTE] 
> Wenn Sie eine Änderung vorgenommen haben und die Einstellungen direkt danach überprüfen, werden unter Umständen noch die alten Konfigurationswerte zurückgegeben. Warten Sie einige Minuten, und überprüfen Sie die Einstellungen dann erneut, um sich zu vergewissern, dass die Änderungen angewendet wurden.

### <a name="configure-automated-backup"></a>Konfigurieren der automatisierten Sicherung
Mithilfe von PowerShell können Sie die automatisierte Sicherung nicht nur aktivieren, sondern auch jederzeit ihre Konfiguration und ihr Verhalten ändern.

Wählen Sie zunächst ein Speicherkonto für die Sicherungsdateien aus, oder erstellen Sie eines. Das folgende Skript wählt ein Speicherkonto aus. Ist kein Speicherkonto vorhanden, wird eines erstellt.

```powershell
$storage_accountname = “yourstorageaccount”
$storage_resourcegroupname = $resourcegroupname

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }
```

> [!NOTE]
> Die automatisierte Sicherung unterstützt zwar nicht das Speichern von Sicherungen in Storage Premium, kann aber Sicherungen von VM-Datenträgern erstellen, die Storage Premium verwenden.

Verwenden Sie dann den Befehl **New-AzureRmVMSqlServerAutoBackupConfig**, um die Einstellungen der automatisierten Sicherung so zu konfigurieren, dass Sicherungen in dem Azure-Speicherkonto gespeichert werden. In diesem Beispiel ist für die Sicherungen eine Aufbewahrungsdauer von zehn Tagen festgelegt. Der zweite Befehl (**Set-AzureRmVMSqlServerExtension**) aktualisiert den angegebenen virtuellen Azure-Computer mit diesen Einstellungen.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

Die Installation und Konfiguration des SQL Server-IaaS-Agents kann mehrere Minuten in Anspruch nehmen.

> [!NOTE]
> Es gibt andere Einstellungen für **New-AzureRmVMSqlServerAutoBackupConfig**, die nur für SQL Server 2016 und die automatisierte Sicherung, Version 2, gelten. SQL Server 2014 unterstützt nicht die folgenden Einstellungen: **BackupSystemDbs**, **BackupScheduleType**, **FullBackupFrequency**, **FullBackupStartHour**, **FullBackupWindowInHours** und **LogBackupFrequencyInMinutes**. Wenn Sie versuchen, diese Einstellungen auf einem virtuellen Computer mit SQL Server 2014 zu konfigurieren, wird zwar kein Fehler angezeigt, doch die Einstellungen werden nicht angewendet. Wenn Sie diese Einstellungen auf einem virtuellen Computer mit SQL Server 2016verwenden möchten, siehe [Automatisierte Sicherung v2 für Azure Virtual Machines mit SQL Server 2016](virtual-machines-windows-sql-automated-backup-v2.md).

Um die Verschlüsselung zu aktivieren, ändern Sie das vorherige Skript, um den **EnableEncryption**-Parameter und ein Kennwort (sichere Zeichenfolge) für den **CertificatePassword**-Parameter zu übergeben. Das folgende Skript aktiviert die Einstellungen der automatisierten Sicherung im vorherigen Beispiel und fügt die Verschlüsselung hinzu.

```powershell
$password = "P@ssw0rd"
$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -EnableEncryption -CertificatePassword $encryptionpassword `
    -RetentionPeriodInDays 10 -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

[Überprüfen Sie die Konfiguration der automatisierten Sicherung](#verifysettings), um sich zu vergewissern, dass Ihre Einstellungen angewendet wurden.

### <a name="disable-automated-backup"></a>Deaktivieren der automatisierten Sicherung

Führen Sie zum Deaktivieren der automatisierten Sicherung das gleiche Skript aus, lassen Sie beim Befehl **New-AzureRmVMSqlServerAutoBackupConfig** aber den Parameter **-Enable** weg. Das Fehlen des Parameters **-Enable** signalisiert dem Befehl, die Funktion zu deaktivieren. Ähnlich wie die Installation kann auch das Deaktivieren der automatisierten Sicherung mehrere Minuten dauern.

```powershell
$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -ResourceGroupName $storage_resourcegroupname

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

### <a name="example-script"></a>Beispielskript

Das folgende Skript stellt einen Satz von Variablen bereit, die Sie anpassen können, um die automatisierte Sicherung für Ihren virtuellen Computer zu aktivieren und zu konfigurieren. Unter Umständen müssen Sie das Skript an Ihre individuellen Anforderungen anpassen. So müssen Sie das Skript beispielsweise ändern, wenn Sie die Sicherung von Systemdatenbanken deaktivieren oder die Verschlüsselung aktivieren möchten.

```powershell
$vmname = "yourvmname"
$resourcegroupname = "vmresourcegroupname"
$region = “Azure region name such as EASTUS2”
$storage_accountname = “storageaccountname”
$storage_resourcegroupname = $resourcegroupname
$retentionperiod = 10

# ResourceGroupName is the resource group which is hosting the VM where you are deploying the SQL IaaS Extension

Set-AzureRmVMSqlServerExtension -VMName $vmname `
    -ResourceGroupName $resourcegroupname -Name "SQLIaasExtension" `
    -Version "1.2" -Location $region

# Creates/use a storage account to store the backups

$storage = Get-AzureRmStorageAccount -ResourceGroupName $resourcegroupname `
    -Name $storage_accountname -ErrorAction SilentlyContinue
If (-Not $storage)
    { $storage = New-AzureRmStorageAccount -ResourceGroupName $storage_resourcegroupname `
    -Name $storage_accountname -SkuName Standard_GRS -Location $region }

# Configure Automated Backup settings

$autobackupconfig = New-AzureRmVMSqlServerAutoBackupConfig -Enable `
    -RetentionPeriodInDays $retentionperiod -StorageContext $storage.Context `
    -ResourceGroupName $storage_resourcegroupname

# Apply the Automated Backup settings to the VM

Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig `
    -VMName $vmname -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Nächste Schritte

Die automatisierte Sicherung konfiguriert Managed Backup für Azure-VMs. Lesen Sie daher unbedingt die [Dokumentation für Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) , um sich mit dem Verhalten und den Auswirkungen vertraut zu machen.

Weitere Informationen zur Sicherung und Wiederherstellung für SQL Server auf Azure-VMs finden Sie im folgenden Thema: [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-backup-recovery.md).

Informationen zu anderen verfügbaren Automatisierungsaufgaben finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (klassisch)](virtual-machines-windows-sql-server-agent-extension.md).

Ausführlichere Informationen zur Ausführung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).


