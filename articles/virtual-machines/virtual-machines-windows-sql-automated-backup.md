<properties
	pageTitle="Automatisierte Sicherung für virtuelle SQL Server-Computer (Resource Manager) | Microsoft Azure"
	description="Erläutert das Feature „Automatisierte Sicherung“ für SQL Server auf virtuellen Azure-Computern unter Verwendung des Resource Manager. "
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-resource-manager"/>
<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="07/14/2016"
	ms.author="jroth" />

# Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (Resource Manager)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-automated-backup.md)
- [Klassisch](virtual-machines-windows-classic-sql-automated-backup.md)

Die automatisierte Sicherung konfiguriert automatisch [Managed Backup für Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx) für alle vorhandenen und neuen Datenbanken auf einer Azure-VM, auf der SQL Server 2014 Standard oder Enterprise ausgeführt wird. Dies bietet Ihnen die Möglichkeit, reguläre Datenbanksicherungen zu konfigurieren, die permanenten Azure-Blob-Speicher nutzen. Die automatische Sicherung basiert auf der [Erweiterung für den SQL Server-IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] Klassisches Bereitstellungsmodell. Den Artikel zum klassischen Bereitstellungsmodell finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (klassisch)](virtual-machines-windows-classic-sql-automated-backup.md).

## Voraussetzungen

Beachten Sie bei der Verwendung der automatisierten Sicherung die folgenden Voraussetzungen:

**Betriebssystem:**

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-Version/-Edition:**

- SQL Server 2014 Standard
- SQL Server 2014 Enterprise

**Datenbankkonfiguration**:

- Zieldatenbanken müssen das vollständige Wiederherstellungsmodell verwenden.

**Azure PowerShell**:

- [Installieren Sie die aktuellen Azure PowerShell-Befehle](../powershell-install-configure.md), wenn Sie die automatisierte Sicherung mit PowerShell konfigurieren möchten.

>[AZURE.NOTE] Die automatisierte Sicherung basiert auf der Erweiterung für den SQL Server-IaaS-Agent. Aktuelle Katalogimages für virtuelle SQL-Computer fügen diese Erweiterung standardmäßig hinzu. Weitere Informationen finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (klassisch)](virtual-machines-windows-sql-server-agent-extension.md).

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
Mit dem Azure-Portal können Sie die automatisierte Sicherung während der Bereitstellung oder für vorhandene virtuelle Computer konfigurieren.

### Neue virtuelle Computer
Verwenden Sie das Azure-Portal zum Konfigurieren der automatisierten Sicherung, wenn Sie einen neuen virtuellen Computer mit SQL Server 2014 im Resource Manager-Bereitstellungsmodell erstellen.

Wählen Sie auf dem Blatt **SQL Server-Einstellungen** die Option **Automatisierte Sicherung** aus. Auf dem folgenden Screenshot des Azure-Portals sehen Sie das Blatt **Automatisierte SQL-Sicherung**.

![Konfigurieren der automatisierten SQL-Sicherung im Azure-Portal](./media/virtual-machines-windows-sql-automated-backup/azure-sql-arm-autobackup.png)

Wenn Sie weiteren Kontext benötigen, lesen Sie das vollständige Thema zum [Bereitstellen eines virtuellen Computers mit SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md).

### Vorhandene virtuelle Computer
Wählen Sie für vorhandene virtuelle Computer mit SQL Server Ihren virtuellen Computer mit SQL Server aus. Wählen Sie dann auf dem Blatt **Einstellungen** den Abschnitt **SQL Server-Konfiguration** aus.

![Automatisierte SQL-Sicherung für vorhandene virtuelle Computer](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-existing-vms.png)

Klicken Sie auf dem Blatt **SQL Server-Konfiguration** im Abschnitt für die automatisierte Sicherung auf die Schaltfläche **Bearbeiten**.

![Konfigurieren der automatisierten SQL-Sicherung für vorhandene virtuelle Computer](./media/virtual-machines-windows-sql-automated-backup/azure-sql-rm-autobackup-configuration.png)

Klicken Sie abschließend unten auf dem Blatt **SQL Server-Konfiguration** auf die Schaltfläche **OK**, um die Änderungen zu speichern.

Falls Sie die automatisierte Sicherung zum ersten Mal aktivieren, konfiguriert Azure den SQL Server-IaaS-Agent im Hintergrund. Im Azure-Portal wird währenddessen u.U. nicht angezeigt, dass die automatisierte Sicherung konfiguriert wird. Warten Sie einige Minuten, bis der Agent installiert und konfiguriert wurde. Danach werden die neuen Einstellungen im Azure-Portal angezeigt.

>[AZURE.NOTE] Sie können die automatisierte Sicherung auch mithilfe einer Vorlage konfigurieren. Weitere Informationen finden Sie unter [Azure quickstart template for Automated Backup](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autobackup-update) (Azure-Schnellstartvorlage für die automatisierte Sicherung).

## Konfiguration mit PowerShell

Nach der Bereitstellung des virtuellen SQL-Computers können Sie die automatisierte Sicherung mithilfe von PowerShell konfigurieren.

Im folgenden PowerShell-Beispiel wird die automatisierte Sicherung für eine vorhandene SQL Server 2014-VM konfiguriert. Der Befehl **AzureRM.Compute\\New-AzureVMSqlServerAutoBackupConfig** konfiguriert die Einstellungen der automatisierten Sicherung zum Speichern von Sicherungen in dem mit dem virtuellen Computer verknüpften Azure-Speicherkonto. Die Sicherungen werden für 10 Tage beibehalten. Der Befehl **Set-AzureRmVMSqlServerExtension** aktualisiert den angegebenen virtuellen Azure-Computer mit diesen Einstellungen.

	$vmname = "vmname"
	$resourcegroupname = "resourcegroupname"
    $autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriodInDays 10 -ResourceGroupName $resourcegroupname

    Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Die Installation und Konfiguration des SQL Server-IaaS-Agents kann mehrere Minuten in Anspruch nehmen.

Um die Verschlüsselung zu aktivieren, ändern Sie das vorherige Skript, um den **EnableEncryption**-Parameter und ein Kennwort (sichere Zeichenfolge) für den **CertificatePassword**-Parameter zu übergeben. Das folgende Skript aktiviert die Einstellungen der automatisierten Sicherung im vorherigen Beispiel und fügt die Verschlüsselung hinzu.

	$vmname = "vmname"
	$resourcegroupname = "resourcegroupname"
	$password = "P@ssw0rd"
	$encryptionpassword = $password | ConvertTo-SecureString -AsPlainText -Force  
	$autobackupconfig = AzureRM.Compute\New-AzureVMSqlServerAutoBackupConfig -Enable -RetentionPeriod 10 -EnableEncryption -CertificatePassword $encryptionpassword -ResourceGroupName $resourcegroupname

	Set-AzureRmVMSqlServerExtension -AutoBackupSettings $autobackupconfig -VMName $vmname -ResourceGroupName $resourcegroupname

Führen Sie zum Deaktivieren der automatisierten Sicherung das gleiche Skript ohne den **-Enable**-Parameter für den Befehl **AzureRM.Compute\\New-AzureVMSqlServerAutoBackupConfig** aus. Das Fehlen des Parameters **-Enable** signalisiert dem Befehl, die Funktion zu deaktivieren. Ähnlich wie die Installation kann auch das Deaktivieren der automatisierten Sicherung mehrere Minuten dauern.

>[AZURE.NOTE] Durch das Entfernen des SQL Server-IaaS-Agents werden die zuvor konfigurierten Einstellungen für die automatisierte Sicherung nicht entfernt. Die automatisierte Sicherung sollte vor dem Deaktivieren oder Deinstallieren des SQL Server-IaaS-Agents deaktiviert werden.

## Nächste Schritte

Die automatisierte Sicherung konfiguriert Managed Backup für Azure-VMs. Lesen Sie daher unbedingt die [Dokumentation für Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx), um sich mit dem Verhalten und den Auswirkungen vertraut zu machen.

Weitere Informationen zur Sicherung und Wiederherstellung für SQL Server auf Azure-VMs finden Sie im folgenden Thema: [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-backup-recovery.md).

Informationen zu anderen verfügbaren Automatisierungsaufgaben finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (klassisch)](virtual-machines-windows-sql-server-agent-extension.md).

Ausführlichere Informationen zur Ausführung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0720_2016-->