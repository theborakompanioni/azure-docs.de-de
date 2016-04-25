<properties
	pageTitle="Erweiterung für SQL Server-IaaS-Agent (klassisch) | Microsoft Azure"
	description="Dieses Thema beschreibt die SQL Server-Agent-Erweiterung, die einer VM mit SQL Server in Azure die Verwendung von Automatisierungsfunktionen ermöglicht. Hierfür wird das klassische Bereitstellungsmodell verwendet."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="rothja"
	manager="jhubbard"
   editor=""    
   tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="04/08/2016"
	ms.author="jroth"/>

# Erweiterung für SQL Server-IaaS-Agent (klassisch)

Mit dieser Erweiterung kann SQL Server auf virtuellen Azure-Computern bestimmte Dienste nutzen. Diese Dienste sind in diesem Artikel aufgeführt und können nur verwendet werden, wenn die Erweiterung installiert ist. Diese Erweiterung wird automatisch für SQL Server-Galerie-Images im Azure-Portal installiert. Sie kann auf allen virtuellen SQL Server-Computern in Azure installiert werden, auf denen auch der Gast-Agent für virtuelle Azure-Computer installiert ist.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Modell.


## Voraussetzungen
Anforderungen für die Verwendung von Powershell-Cmdlets:

- Neueste Azure PowerShell-Version [hier verfügbar](../powershell-install-configure.md)

Anforderungen für die Verwendung der Erweiterung auf Ihrem virtuellen Computer:

- Gast-Agent für virtuelle Azure-Computer
- Windows Server 2012, Windows Server 2012 R2 oder höher
- SQL Server 2012, SQL Server 2014 oder höher

## Mit der Erweiterung verfügbare Dienste

- **Automatisierte SQL-Sicherung**: Dieser Dienst automatisiert die Planung von Sicherungen für alle Datenbanken für die Standardinstanz von SQL Server auf der VM. Weitere Informationen finden Sie unter [Automated backup for SQL Server in Azure Virtual Machines (Classic)](virtual-machines-windows-classic-sql-automated-backup.md) (Automatisierte Sicherung für SQL Server in Azure Virtual Machines (klassisch)).
- **Automatisiertes SQL-Patchen**: Mit diesem Dienst können Sie ein Wartungsfenster konfigurieren, in dem Updates für die VM ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden. Weitere Informationen finden Sie unter [Automated Patching for SQL Server in Azure Virtual Machines (Classic)](virtual-machines-windows-classic-sql-automated-patching.md) (Automatisiertes Patchen für SQL Server in Azure Virtual Machines (klassisch)).
- **Azure Key Vault-Integration**: Mit diesem Dienst können Sie Azure Key Vault auf Ihrer SQL Server-VM automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure-Schlüsseltresor-Integration für SQL Server auf virtuellen Azure-Computern (klassische Bereitstellung)](virtual-machines-windows-classic-ps-sql-keyvault.md).

## Hinzufügen der Erweiterung mit Powershell
Wenn Sie den virtuellen SQL Server-Computer mit dem [Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md) bereitstellen, wird die Erweiterung automatisch installiert. Für SQL Server-VMs, die mit dem klassischen Azure-Portal bereitgestellt werden, oder für VMs, auf denen eigene SQL-Lizenzen bereitgestellt werden, können Sie diese Erweiterung mit dem Azure PowerShell-Cmdlet **Set-AzureVMSqlServerExtension** hinzufügen.

### Syntax

Set-AzureVMSqlServerExtension [[-ReferenceName] [String]] [-VM] IPersistentVM [[-Version] [String]] [[-AutoPatchingSettings] [AutoPatchingSettings]] [-AutoBackupSettings[AutoBackupSettings]] [-Profile [AzureProfile]] [CommonParameters]

> [AZURE.NOTE] Es wird empfohlen, den Parameter „–Version“ auszulassen. Ohne diesen Parameter wird standardmäßig die neueste Version der Erweiterung festgelegt.

### Beispiel
Im folgenden Beispiel werden die Einstellungen für die automatische Sicherung mithilfe einer in $abs definierten Konfiguration (hier nicht dargestellt) konfiguriert. Der serviceName bezeichnet den Namen des Clouddiensts, der den virtuellen Computer hostet. Ein komplettes Beispiel finden Sie unter [Automated backup for SQL Server in Azure Virtual Machines (Classic)](virtual-machines-windows-classic-sql-automated-backup.md) (Automatisierte Sicherung für SQL Server in Azure Virtual Machines (klassisch)).

	Get-AzureVM –ServiceName "serviceName" –Name "vmName" | Set-AzureVMSqlServerExtension –AutoBackupSettings $abs | Update-AzureVM**

## Überprüfen des Erweiterungsstatus
Zum Überprüfen des Status dieser Erweiterung und der zugeordneten Dienste können Sie beide Portale verwenden. Rufen Sie in den Details des vorhandenen virtuellen Computers unter **Einstellungen** die Option **Erweiterungen** auf.

Sie können auch das Azure PowerShell-Cmdlet **Get-AzureVMSqlServerExtension** verwenden.

### Syntax

Get-AzureVMSqlServerExtension [[-VM] [IPersistentVM]] [-Profile [AzureProfile]] [CommonParameters]

### Beispiel
	Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## Entfernen der Erweiterung mit Powershell   
Wenn Sie diese Erweiterung von Ihrer VM entfernen möchten, können Sie das Azure PowerShell-Cmdlet **Remove-AzureVMSqlServerExtension** verwenden.

### Syntax

Remove-AzureVMSqlServerExtension [-Profile [AzureProfile]] -VM IPersistentVM [CommonParameters]

<!---HONumber=AcomDC_0413_2016-->