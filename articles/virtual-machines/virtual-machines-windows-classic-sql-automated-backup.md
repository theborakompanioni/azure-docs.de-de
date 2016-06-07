<properties
	pageTitle="Automatisierte Sicherung für virtuelle SQL Server-Computer (klassisch) | Microsoft Azure"
	description="Erläutert das Feature „Automatisierte Sicherung“ für SQL Server auf virtuellen Azure-Computern unter Verwendung des Resource Manager."
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-service-management" />
<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="05/18/2016"
	ms.author="jroth" />

# Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (klassisch)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-automated-backup.md)
- [Klassisch](virtual-machines-windows-classic-sql-automated-backup.md)

Die automatisierte Sicherung konfiguriert automatisch [Managed Backup für Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) für alle vorhandenen und neuen Datenbanken auf einer Azure-VM, auf der SQL Server 2014 Standard oder Enterprise ausgeführt wird. Dies bietet Ihnen die Möglichkeit, reguläre Datenbanksicherungen zu konfigurieren, die permanenten Azure-Blob-Speicher nutzen. Die automatische Sicherung basiert auf der [Erweiterung für den SQL Server-IaaS-Agent](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Ressourcen-Manager-Modell. Die Resource Manager-Version dieses Artikels finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-backup.md).

## Voraussetzungen

Beachten Sie bei der Verwendung der automatisierten Sicherung die folgenden Voraussetzungen:

**Betriebssystem:**

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-Version/-Edition:**

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise
- SQL Server 2016 Standard
- SQL Server 2016 Enterprise

**Datenbankkonfiguration:**

- Zieldatenbanken müssen das vollständige Wiederherstellungsmodell verwenden.

**Azure PowerShell:**

- [Installieren Sie die aktuellen Azure PowerShell-Befehle](../powershell-install-configure.md), wenn Sie die automatisierte Sicherung mit PowerShell konfigurieren möchten.

>[AZURE.NOTE] Die automatisierte Sicherung basiert auf der Erweiterung für den SQL Server-IaaS-Agent. Aktuelle Katalogimages für virtuelle SQL-Computer fügen diese Erweiterung standardmäßig hinzu. Weitere Informationen finden Sie unter [SQL Server IaaS Agent Extension](virtual-machines-windows-classic-sql-server-agent-extension.md) (Erweiterung für den SQL Server-IaaS-Agent).

## Einstellungen

In der folgenden Tabelle werden die Optionen beschrieben, die für die automatisierte Sicherung konfiguriert werden können. Die tatsächlichen Konfigurationsschritte variieren abhängig davon, ob Sie das Azure-Portal oder Azure Windows PowerShell-Befehle verwenden.

|Einstellung|Bereich (Standard)|Beschreibung|
|---|---|---|
|**Automatisierte Sicherung**|Aktivieren/Deaktivieren (deaktiviert)|Aktiviert oder deaktiviert die automatisierte Sicherung für eine Azure-VM mit SQL Server 2014 Standard oder Enterprise.|
|**Aufbewahrungszeitraum**|1 bis 30 Tage (30 Tage)|Die Anzahl von Tagen, für die eine Sicherung aufbewahrt wird.|
|**Speicherkonto**|Azure-Speicher-Konto (das für die angegebene VM erstellte Speicherkonto)|Ein Azure-Speicherkonto, mit dem Dateien der automatisierten Sicherung im Blob-Speicher gespeichert werden. An diesem Speicherort wird ein Container zum Speichern aller Sicherungsdateien erstellt. Die Namenskonvention für die Sicherungsdatei enthält das Datum, die Uhrzeit und den Computernamen.|
|**Verschlüsselung**|Aktivieren/Deaktivieren (deaktiviert)|Aktiviert oder deaktiviert die Verschlüsselung. Wenn die Verschlüsselung aktiviert ist, befinden sich die Zertifikate zum Wiederherstellen der Sicherung im angegebenen Speicherkonto im gleichen automaticbackup-Container (mit derselben Namenskonvention). Wenn das Kennwort geändert wird, wird ein neues Zertifikat mit diesem Kennwort generiert, das alte Zertifikat bleibt jedoch zum Wiederherstellen vorheriger Sicherungen erhalten.|
|**Kennwort**|Kennworttext (keiner)|Ein Kennwort für Verschlüsselungsschlüssel. Ein Kennwort ist nur erforderlich, wenn die Verschlüsselung aktiviert ist. Um eine verschlüsselte Sicherung wiederherzustellen, benötigen Sie das richtige Kennwort und das zugehörige Zertifikat, das beim Erstellen der Sicherung verwendet wurde.|

## Konfiguration im Portal

Sie können das Azure-Portal zum Konfigurieren der automatisierten Sicherung verwenden, wenn Sie einen neuen virtuellen Computer mit SQL Server 2014 im klassischen Bereitstellungsmodell erstellen.

Der folgende Azure-Portal-Screenshot zeigt die oben beschriebenen Optionen unter **OPTIONALE KONFIGURATION** | **AUTOMATISIERTE SQL-SICHERUNG**.

![Konfigurieren der automatischen SQL-Sicherung im Azure-Portal](./media/virtual-machines-windows-classic-sql-automated-backup/IC778483.jpg)

Wählen Sie für vorhandene virtuelle Computer mit SQL Server 2014 die Einstellungen für die **automatische Sicherung** im Abschnitt **Konfiguration** der Eigenschaften des virtuellen Computers aus. Im Fenster **Automatisierte Sicherung** können Sie die Funktion aktivieren, den Aufbewahrungszeitraum festlegen, das Speicherkonto auswählen und die Verschlüsselung festlegen. Diese Einstellungen zeigt der folgende Screenshot.

![Konfigurieren der automatisierten Sicherung im Azure-Portal](./media/virtual-machines-windows-classic-sql-automated-backup/IC792133.jpg)

>[AZURE.NOTE] Wenn Sie die automatisierte Sicherung zum ersten Mal aktivieren, konfiguriert Azure den SQL Server-IaaS-Agent im Hintergrund. Im Azure-Portal wird währenddessen u.U. nicht angezeigt, dass die automatisierte Sicherung konfiguriert wird. Warten Sie einige Minuten, bis der Agent installiert und konfiguriert wurde. Danach werden die neuen Einstellungen im Azure-Portal angezeigt.

## Konfiguration mit PowerShell

Im folgenden PowerShell-Beispiel wird die automatisierte Sicherung für eine vorhandene SQL Server 2014-VM konfiguriert. Der Befehl **New-AzureVMSqlServerAutoBackupConfig** konfiguriert die Einstellungen der automatisierten Sicherung zum Speichern von Sicherungen in dem mit der $storageaccount-Variable angegebenen Azure-Speicherkonto. Die Sicherungen werden für 10 Tage beibehalten. Der Befehl **Set AzureVMSqlServerExtension** aktualisiert die angegebene Azure-VM mit diesen Einstellungen.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Die Installation und Konfiguration des SQL Server-IaaS-Agents kann mehrere Minuten in Anspruch nehmen.

Um die Verschlüsselung zu aktivieren, ändern Sie das vorherige Skript, um den EnableEncryption-Parameter und ein Kennwort (sichere Zeichenfolge) für den CertificatePassword-Parameter zu übergeben. Das folgende Skript aktiviert die Einstellungen der automatisierten Sicherung im vorherigen Beispiel und fügt die Verschlüsselung hinzu.

    $storageaccount = "<storageaccountname>"
    $storageaccountkey = (Get-AzureStorageKey -StorageAccountName $storageaccount).Primary
    $storagecontext = New-AzureStorageContext -StorageAccountName $storageaccount -StorageAccountKey $storageaccountkey
    $password = "P@ssw0rd"
    $encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
    $autobackupconfig = New-AzureVMSqlServerAutoBackupConfig -StorageContext $storagecontext -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoBackupSettings $autobackupconfig | Update-AzureVM

Führen Sie zum Deaktivieren der automatisierten Sicherung das gleiche Skript ohne den **-Enable**-Parameter für **New-AzureVMSqlServerAutoBackupConfig** aus. Ähnlich wie die Installation kann auch das Deaktivieren der automatisierten Sicherung mehrere Minuten dauern.

>[AZURE.NOTE] Durch das Deaktivieren und Deinstallieren des SQL Server-IaaS-Agents werden die zuvor konfigurierten Managed Backup-Einstellungen nicht entfernt. Die automatisierte Sicherung sollte vor dem Deaktivieren oder Deinstallieren des SQL Server-IaaS-Agents deaktiviert werden.

## Nächste Schritte

Die automatisierte Sicherung konfiguriert Managed Backup für Azure-VMs. Lesen Sie daher unbedingt die [Dokumentation für Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx), um sich mit dem Verhalten und den Auswirkungen vertraut zu machen.

Weitere Informationen zur Sicherung und Wiederherstellung für SQL Server auf Azure-VMs finden Sie im folgenden Thema: [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-backup-recovery.md).

Informationen zu anderen verfügbaren Automatisierungsaufgaben finden Sie unter [SQL Server IaaS Agent Extension](virtual-machines-windows-classic-sql-server-agent-extension.md) (Erweiterung für den SQL Server-IaaS-Agent).

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

<!----HONumber=AcomDC_0525_2016-->