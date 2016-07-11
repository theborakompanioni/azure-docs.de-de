<properties
	pageTitle="Automatisiertes Patchen für virtuelle SQL Server-Computer (klassisch) | Microsoft Azure"
	description="Erläutert das Feature „Automatisiertes Patchen“ für virtuelle SQL Server-Computer in Azure mithilfe des klassischen Bereitstellungsmodus."
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

# Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (klassisch)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-automated-patching.md)
- [Klassisch](virtual-machines-windows-classic-sql-automated-patching.md)

Beim automatisierten Patchen wird ein Wartungsfenster für einen virtuellen Azure-Computer mit SQL Server eingerichtet. Automatische Updates können nur während dieses Wartungsfensters installiert werden. Dadurch wird bei SQL Server sichergestellt, dass Systemupdates und alle erforderlichen Neustarts zum bestmöglichen Zeitpunkt für die Datenbank stattfinden. Das automatisierte Patchen basiert auf der [Erweiterung für den SQL Server-IaaS-Agent](virtual-machines-windows-classic-sql-server-agent-extension.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] Die Resource Manager-Version dieses Artikels finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-patching.md).

## Voraussetzungen

Beachten Sie bei der Verwendung des automatisierten Patchens die folgenden Voraussetzungen:

**Betriebssystem:**

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-Version:**

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell:**

- [Installieren Sie die aktuellen Azure PowerShell-Befehle](../powershell-install-configure.md), wenn Sie das automatisierte Patchen mit PowerShell konfigurieren möchten.

>[AZURE.NOTE] Das automatisierte Patchen basiert auf der Erweiterung für den SQL Server-IaaS-Agent. Aktuelle Katalogimages für virtuelle SQL-Computer fügen diese Erweiterung standardmäßig hinzu. Weitere Informationen finden Sie unter [SQL Server IaaS Agent Extension](virtual-machines-windows-classic-sql-server-agent-extension.md) (Erweiterung für den SQL Server-IaaS-Agent).

## Einstellungen

In der folgenden Tabelle werden die Optionen beschrieben, die für das automatisierte Patchen konfiguriert werden können. Die tatsächlichen Konfigurationsschritte variieren abhängig davon, ob Sie das Azure-Portal oder Azure Windows PowerShell-Befehle verwenden.

|Einstellung|Mögliche Werte|Beschreibung|
|---|---|---|
|**Automatisiertes Patchen**|Aktivieren/Deaktivieren (deaktiviert)|Aktiviert oder deaktiviert das automatisierte Patchen für einen virtuellen Azure-Computer.|
|**Wartungszeitplan**|Täglich, Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag, Sonntag|Der Zeitplan für das Herunterladen und Installieren von Windows-, SQL Server- und Microsoft-Updates für den virtuellen Computer|
|**Wartung beginnt um**|0-24|Die lokale Startzeit zum Aktualisieren des virtuellen Computers|
|**Dauer des Wartungsfensters**|30-180|Der Zeitraum in Minuten, in dem das Herunterladen und Installieren von Updates abgeschlossen werden darf|
|**Patch Category** (Patchkategorie)|Wichtig|Die Kategorie der Updates, die heruntergeladen und installiert werden.|

## Konfiguration im Portal

Sie können das [Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=525040&clcid=0x409) zum Konfigurieren des automatisierten Patchens verwenden, wenn Sie einen neuen virtuellen Computer mit SQL Server im klassischen Bereitstellungsmodell erstellen.

Der folgende Azure-Portal-Screenshot zeigt die oben beschriebenen Optionen unter **OPTIONALE KONFIGURATION** | **AUTOMATISIERTES SQL-PATCHEN**.

![Automatisches Patchen für SQL im Azure-Portal](./media/virtual-machines-windows-classic-sql-automated-patching/IC778484.jpg)

Wählen Sie für vorhandene virtuelle Computer mit SQL Server 2012 oder 2014 die Einstellungen für das **automatische Patchen** im Abschnitt **Konfiguration** der Eigenschaften des virtuellen Computers aus. Im Fenster zum **automatisierten Patchen** können Sie das Feature aktivieren, den Wartungszeitplan und den Beginn festlegen und die Dauer des Wartungsfensters auswählen. Diese Einstellungen sind auf dem folgenden Bildschirmfoto zu sehen:

![Konfigurieren des automatischen Patchens im Azure-Portal](./media/virtual-machines-windows-classic-sql-automated-patching/IC792132.jpg)

>[AZURE.NOTE] Wenn Sie das automatische Patchen zum ersten Mal aktivieren, konfiguriert Azure den SQL Server-IaaS-Agent im Hintergrund. Im Azure-Portal wird währenddessen u.U. nicht angezeigt, dass das automatisierte Patchen konfiguriert wird. Warten Sie einige Minuten, bis der Agent installiert und konfiguriert wurde. Danach werden die neuen Einstellungen im Azure-Portal angezeigt.

## Konfiguration mit PowerShell

Sie können das automatisierte Patchen auch mithilfe von PowerShell konfigurieren.

Im folgenden Beispiel wird PowerShell zum Konfigurieren des automatisierten Patchens auf einem vorhandenen virtuellen SQL Server-Computer verwendet. Der Befehl **New-AzureVMSqlServerAutoPatchingConfig** konfiguriert ein neues Wartungsfenster für automatische Updates.

    $aps = New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -AutoPatchingSettings $aps | Update-AzureVM

In der folgenden Tabelle wird basierend auf diesem Beispiel die tatsächliche Auswirkung auf den virtuellen Azure-Zielcomputer beschrieben:

|Parameter|Effekt|
|---|---|
|**DayOfWeek**|Patches werden jeden Donnerstag installiert.|
|**MaintenanceWindowStartingHour**|Updates werden um 11:00 Uhr gestartet.|
|**MaintenanceWindowsDuration**|Patches müssen innerhalb von 120 Minuten installiert werden. Auf der Grundlage der Startzeit müssen sie um 13:00 Uhr abgeschlossen sein.|
|**PatchCategory**|Die einzig mögliche Einstellung für diesen Parameter lautet „Wichtig“.|

Die Installation und Konfiguration des SQL Server-IaaS-Agents kann mehrere Minuten in Anspruch nehmen.

Führen Sie zum Deaktivieren des automatisierten Patchens das gleiche Skript ohne den Parameter „-Enable“ für „New-AzureVMSqlServerAutoPatchingConfig“ aus. Wie bei der Installation kann auch das Deaktivieren des automatisierten Patchens mehrere Minuten dauern.

## Nächste Schritte

Informationen zu anderen verfügbaren Automatisierungsaufgaben finden Sie unter [SQL Server IaaS Agent Extension](virtual-machines-windows-classic-sql-server-agent-extension.md) (Erweiterung für den SQL Server-IaaS-Agent).

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0629_2016-->