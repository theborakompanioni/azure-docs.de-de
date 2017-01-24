---
title: "Automatisiertes Patchen für virtuelle SQL Server-Computer (Resource Manager) | Microsoft Docs"
description: "Erläutert die Funktion „Automatisiertes Patchen“ für virtuelle SQL Server-Computer in Azure unter Verwendung des Resource Manager."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: 58232e92-318f-456b-8f0a-2201a541e08d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 08/19/2016
ms.author: jroth
translationtype: Human Translation
ms.sourcegitcommit: 0c23ee550d8ac88994e8c7c54a33d348ffc24372
ms.openlocfilehash: b092b8a746f4f94add9727dc17c6b8fc198b6404


---
# <a name="automated-patching-for-sql-server-in-azure-virtual-machines-resource-manager"></a>Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (Resource Manager)
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-sql-automated-patching.md)
> * [Klassisch](../sqlclassic/virtual-machines-windows-classic-sql-automated-patching.md)
> 
> 

Beim automatisierten Patchen wird ein Wartungsfenster für einen virtuellen Azure-Computer mit SQL Server eingerichtet. Automatische Updates können nur während dieses Wartungsfensters installiert werden. Bei SQL Server stellt diese Einschränkung sicher, dass Systemupdates und alle erforderlichen Neustarts zum bestmöglichen Zeitpunkt für die Datenbank stattfinden. Das automatisierte Patchen basiert auf der [Erweiterung für den SQL Server-IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Den Artikel zum klassischen Bereitstellungsmodell finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (klassisch)](../sqlclassic/virtual-machines-windows-classic-sql-automated-patching.md).

## <a name="prerequisites"></a>Voraussetzungen
Beachten Sie bei der Verwendung des automatisierten Patchens die folgenden Voraussetzungen:

**Betriebssystem:**

* Windows Server 2012
* Windows Server 2012 R2

**SQL Server-Version:**

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell:**

* [Installieren Sie die aktuellen Azure PowerShell-Befehle](/powershell/azureps-cmdlets-docs) , wenn Sie das automatisierte Patchen mit PowerShell konfigurieren möchten.

> [!NOTE]
> Das automatisierte Patchen basiert auf der Erweiterung für den SQL Server-IaaS-Agent. Aktuelle Katalogimages für virtuelle SQL-Computer fügen diese Erweiterung standardmäßig hinzu. Weitere Informationen finden Sie unter [Erweiterung für SQL Server-IaaS-Agent](virtual-machines-windows-sql-server-agent-extension.md).
> 
> 

## <a name="settings"></a>Einstellungen
In der folgenden Tabelle werden die Optionen beschrieben, die für das automatisierte Patchen konfiguriert werden können. Die tatsächlichen Konfigurationsschritte variieren abhängig davon, ob Sie das Azure-Portal oder Azure Windows PowerShell-Befehle verwenden.

| Einstellung | Mögliche Werte | Beschreibung |
| --- | --- | --- |
| **Automatisiertes Patchen** |Aktivieren/Deaktivieren (deaktiviert) |Aktiviert oder deaktiviert das automatisierte Patchen für einen virtuellen Azure-Computer. |
| **Wartungszeitplan** |Täglich, Montag, Dienstag, Mittwoch, Donnerstag, Freitag, Samstag, Sonntag |Der Zeitplan für das Herunterladen und Installieren von Windows-, SQL Server- und Microsoft-Updates für den virtuellen Computer |
| **Wartung beginnt um** |0-24 |Die lokale Startzeit zum Aktualisieren des virtuellen Computers |
| **Dauer des Wartungsfensters** |30-180 |Der Zeitraum in Minuten, in dem das Herunterladen und Installieren von Updates abgeschlossen werden darf |
| **Patch Category** |Wichtig |Die Kategorie der Updates, die heruntergeladen und installiert werden. |

## <a name="configuration-in-the-portal"></a>Konfiguration im Portal
Mit dem Azure-Portal können Sie das automatisierte Patchen während der Bereitstellung oder für vorhandene virtuelle Computer konfigurieren.

### <a name="new-vms"></a>Neue virtuelle Computer
Verwenden Sie das Azure-Portal zum Konfigurieren des automatisierten Patchens, wenn Sie einen neuen virtuellen Computer mit SQL Server im Resource Manager-Bereitstellungsmodell erstellen.

Wählen Sie auf dem Blatt **SQL Server-Einstellungen** die Option **Automatisiertes Patchen** aus. Auf dem folgenden Screenshot des Azure-Portals sehen Sie das Blatt **Automatisiertes SQL-Patchen** .

![Automatisiertes Patchen für SQL im Azure-Portal](./media/virtual-machines-windows-sql-automated-patching/azure-sql-arm-patching.png)

Wenn Sie weiteren Kontext benötigen, lesen Sie das vollständige Thema zum [Bereitstellen eines virtuellen Computers mit SQL Server in Azure](virtual-machines-windows-portal-sql-server-provision.md).

### <a name="existing-vms"></a>Vorhandene virtuelle Computer
Wählen Sie für vorhandene virtuelle Computer mit SQL Server Ihren virtuellen Computer mit SQL Server aus. Wählen Sie dann auf dem Blatt **Einstellungen** den Abschnitt **SQL Server-Konfiguration** aus.

![Automatisiertes Patchen für SQL für vorhandene virtuelle Computer](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-existing-vms.png)

Klicken Sie auf dem Blatt **SQL Server-Konfiguration** im Abschnitt für das automatisierte Patchen auf die Schaltfläche **Bearbeiten**.

![Konfigurieren des automatisierten Patchens für SQL für vorhandene virtuelle Computer](./media/virtual-machines-windows-sql-automated-patching/azure-sql-rm-patching-configuration.png)

Klicken Sie abschließend unten auf dem Blatt **SQL Server-Konfiguration** auf die Schaltfläche **OK**, um die Änderungen zu speichern.

Falls Sie das automatisierte Patchen zum ersten Mal aktivieren, konfiguriert Azure den SQL Server-IaaS-Agent im Hintergrund. Im Azure-Portal wird währenddessen u.U. nicht angezeigt, dass das automatisierte Patchen konfiguriert wird. Warten Sie einige Minuten, bis der Agent installiert und konfiguriert wurde. Danach werden die neuen Einstellungen im Azure-Portal angezeigt.

> [!NOTE]
> Sie können das automatisierte Patchen auch mithilfe einer Vorlage konfigurieren. Weitere Informationen finden Sie unter [Azure quickstart template for Automated Patching](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-autopatching-update)(Azure-Schnellstartvorlage für das automatisierte Patchen).
> 
> 

## <a name="configuration-with-powershell"></a>Konfiguration mit PowerShell
Nach der Bereitstellung des virtuellen SQL-Computers können Sie das automatisierte Patchen mithilfe von PowerShell konfigurieren.

Im folgenden Beispiel wird PowerShell zum Konfigurieren des automatisierten Patchens auf einem vorhandenen virtuellen SQL Server-Computer verwendet. Der Befehl **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** konfiguriert ein neues Wartungsfenster für automatische Updates.

    $vmname = "vmname"
    $resourcegroupname = "resourcegroupname"
    $aps = AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig -Enable -DayOfWeek "Thursday" -MaintenanceWindowStartingHour 11 -MaintenanceWindowDuration 120  -PatchCategory "Important"

    Set-AzureRmVMSqlServerExtension -AutoPatchingSettings $aps -VMName $vmname -ResourceGroupName $resourcegroupname

In der folgenden Tabelle wird basierend auf diesem Beispiel die tatsächliche Auswirkung auf den virtuellen Azure-Zielcomputer beschrieben:

| Parameter | Effekt |
| --- | --- |
| **DayOfWeek** |Patches werden jeden Donnerstag installiert. |
| **MaintenanceWindowStartingHour** |Updates werden um 11:00 Uhr gestartet. |
| **MaintenanceWindowsDuration** |Patches müssen innerhalb von 120 Minuten installiert werden. Auf der Grundlage der Startzeit müssen sie um 13:00 Uhr abgeschlossen sein. |
| **PatchCategory** |Die einzig mögliche Einstellung für diesen Parameter lautet **Wichtig**. |

Die Installation und Konfiguration des SQL Server-IaaS-Agents kann mehrere Minuten in Anspruch nehmen.

Führen Sie zum Deaktivieren des automatisierten Patchens das gleiche Skript ohne den **-Enable**-Parameter für **AzureRM.Compute\New-AzureVMSqlServerAutoPatchingConfig** aus. Das Fehlen des Parameters **-Enable** signalisiert dem Befehl, die Funktion zu deaktivieren.

## <a name="next-steps"></a>Nächste Schritte
Informationen zu anderen verfügbaren Automatisierungsaufgaben finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (klassisch)](virtual-machines-windows-sql-server-agent-extension.md).

Ausführlichere Informationen zur Ausführung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).




<!--HONumber=Jan17_HO2-->


