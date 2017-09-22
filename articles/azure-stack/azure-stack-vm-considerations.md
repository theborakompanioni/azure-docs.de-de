---
title: "Unterschiede und Aspekte für virtuelle Computer in Azure Stack | Microsoft-Dokumentation"
description: Es werden die Unterschiede und Aspekte beim Verwenden von virtuellen Computern in Azure Stack beschrieben.
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/15/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: e05028ad46ef6ec2584cd2d3f4843cf38bb54f9e
ms.openlocfilehash: b09c035a15453bdaf4eaf590e3bad9db3da15173
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="considerations-for-virtual-machines-in-azure-stack"></a>Aspekte von virtuellen Computern in Azure Stack

Bei virtuellen Computern handelt es sich hier um bedarfsgesteuerte, skalierbare Computingressourcen, die von Azure Stack angeboten werden. Bei der Nutzung von virtuellen Computern sollte Ihnen klar sein, dass es Unterschiede zwischen den verfügbaren Features in Azure und Azure Stack gibt. Dieser Artikel enthält eine Übersicht über die eindeutigen Aspekte für virtuelle Computer und die entsprechenden Features in Azure Stack. Informationen zu allgemeinen Unterschieden zwischen Azure Stack und Azure finden Sie im Thema [Wichtige Aspekte: Verwenden von Diensten oder Erstellen von Apps für Azure Stack](azure-stack-considerations.md).

## <a name="cheat-sheet-virtual-machine-differences"></a>Cheat Sheet: Unterschiede bei virtuellen Computern

| Feature | Azure (global) | Azure Stack |
| --- | --- | --- |
| VM-Images | Der Azure Marketplace enthält Images, die Sie zum Erstellen eines virtuellen Computers verwenden können. Auf der Seite [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) können Sie die Liste mit den Images anzeigen, die im Azure Marketplace verfügbar sind. | Im Azure Stack-Marketplace sind standardmäßig keine Images verfügbar. Der Azure Stack-Cloudadministrator sollte Images [veröffentlichen](azure-stack-add-default-image.md) oder [auf den Azure Stack-Marketplace herunterladen](azure-stack-download-azure-marketplace-item.md), bevor sie von Benutzern verwendet werden können. |
| Größen virtueller Computer | Azure unterstützt eine Vielzahl von Größen für virtuelle Computer. Informationen zu den verfügbaren Größen und Optionen finden Sie in den Themen [Größen für virtuelle Windows-Computer in Azure (Windows)](../virtual-machines/virtual-machines-windows-sizes.md) und [Größen für virtuelle Linux-Computer in Azure (Linux)](../virtual-machines/linux/sizes.md). | Azure Stack unterstützt eine Teilmenge der VM-Größen, die in Azure verfügbar sind. Die Liste mit den unterstützten Größen finden Sie in diesem Artikel im Abschnitt [Größen virtueller Computer](#virtual-machine-sizes). |
| Kontingente für virtuelle Computer | [Kontingentgrenzen](../azure-subscription-service-limits.md#service-specific-limits) werden von Microsoft festgelegt. | Der Azure Stack-Cloudadministrator muss [Kontingente zuweisen](azure-stack-setting-quotas.md), bevor den Benutzern die virtuellen Computer angeboten werden. |
| VM-Erweiterungen |Azure unterstützt eine Vielzahl von Erweiterungen für virtuelle Computer. Weitere Informationen zu den verfügbaren Erweiterungen finden Sie im Thema [Erweiterungen und Features für virtuelle Computer für Windows](../virtual-machines/windows/extensions-features.md).| Azure Stack unterstützt eine Teilmenge der Erweiterungen, die in Azure verfügbar sind, und jede Erweiterung hat eine bestimmte Version. Der Azure Stack-Cloudadministrator kann jeweils wählen, welche Erweiterungen für seine Benutzer verfügbar gemacht werden sollen. Die Liste mit den unterstützten Erweiterungen finden Sie in diesem Artikel im Abschnitt [VM-Erweiterungen](#virtual-machine-extensions). |
| Netzwerk für virtuelle Computer | Öffentliche IP-Adressen, die dem virtuellen Mandantencomputer zugewiesen sind, sind über das Internet zugänglich.<br><br><br>Azure Virtual Machines verfügen über einen feststehenden DNS-Namen. | Öffentliche IP-Adressen, die einem virtuellen Mandantencomputer zugewiesen sind, sind nur in der Azure Stack Development Kit-Umgebung zugänglich. Ein Benutzer muss per [RDP](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) Zugriff auf das Azure Stack Development Kit haben, um eine Verbindung mit einem virtuellen Computer herstellen zu können, der in Azure Stack erstellt wird.<br><br>Virtuelle Computer, die in einer bestimmten Azure Stack-Instanz erstellt werden, verfügen über einen DNS-Namen basierend auf dem Wert, der vom Cloudadministrator konfiguriert wird. |
| VM-Speicher | Unterstützt [verwaltete Datenträger](../virtual-machines/windows/managed-disks-overview.md). | Verwaltete Datenträger werden in Azure Stack noch nicht unterstützt. |
| API-Versionen | Azure verfügt für alle VM-Features immer über die aktuellen API-Versionen. | Azure Stack unterstützt bestimmte Azure-Dienste und bestimmte API-Versionen für diese Dienste. Die Liste mit den unterstützten API-Versionen finden Sie in diesem Artikel im Abschnitt [API-Versionen](#api-versions). |
|VM-Verfügbarkeitsgruppen|Mehrere Fehlerdomänen (zwei oder drei pro Region)<br>Mehrere Updatedomänen<br>Unterstützung von verwalteten Datenträgern|Einzelne Fehlerdomäne<br>Einzelne Updatedomäne<br>Keine Unterstützung von verwalteten Datenträgern|
|VM-Skalierungsgruppen|Automatische Skalierung wird unterstützt|Automatische Skalierung wird nicht unterstützt<br>Fügen Sie einer Skalierungsgruppe mit dem Portal, Resource Manager-Vorlagen oder PowerShell weitere Instanzen hinzu.

## <a name="virtual-machine-sizes"></a>Größen virtueller Computer 

Das Azure Stack Development Kit unterstützt die folgenden Größen: 

| Typ | Größe | Bereich der unterstützten Größen |
| --- | --- | --- |
|Allgemeiner Zweck |Basic A|A0 - A4|
|Allgemeiner Zweck |Standard A|A0 - A7|
|Allgemeiner Zweck |Standard D|D1 - D4|
|Allgemeiner Zweck |Standard Dv2|D1v2 - D5v2|
|Arbeitsspeicheroptimiert|D-Serie|D11 - D14|
|Arbeitsspeicheroptimiert |Dv2-Serie|D11v2 - D14v2|

Die VM-Größen und ihre zugeordneten Ressourcenmengen sind für Azure Stack und Azure konsistent. Dies umfasst beispielsweise die Arbeitsspeichermenge, die Anzahl von Kernen und die Anzahl bzw. Größe von Datenträgern für Daten, die erstellt werden können. Die Leistung der gleichen VM-Größe in Azure Stack richtet sich aber nach den zugrunde liegenden Merkmalen der jeweiligen Azure Stack-Umgebung.

## <a name="virtual-machine-extensions"></a>VM-Erweiterungen 

 Das Azure Stack Development Kit unterstützt die folgenden Versionen von VM-Erweiterungen:

![VM-Erweiterungen](media/azure-stack-vm-considerations/vm-extensions.png)

Verwenden Sie das folgende PowerShell-Skript, um die Liste mit den VM-Erweiterungen abzurufen, die in Ihrer Azure Stack-Umgebung verfügbar sind:

```powershell 
Get-AzureRmVmImagePublisher -Location local | `
  Get-AzureRmVMExtensionImageType | `
  Get-AzureRmVMExtensionImage | `
  Select Type, Version | `
  Format-Table -Property * -AutoSize 
```

## <a name="api-versions"></a>API-Versionen 

VM-Features im Azure Stack Development Kit unterstützen die folgenden API-Versionen:

![VM-Ressourcentypen](media/azure-stack-vm-considerations/vm-resoource-types.png)

Sie können das folgende PowerShell-Skript verwenden, um die API-Versionen für die VM-Features abzurufen, die in Ihrer Azure Stack-Umgebung verfügbar sind:

```powershell 
Get-AzureRmResourceProvider | `
  Select ProviderNamespace -Expand ResourceTypes | `
  Select * -Expand ApiVersions | `
  Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} | `
  where-Object {$_.ProviderNamespace -like “Microsoft.compute”}
```
Die Liste mit den unterstützten Ressourcentypen und API-Versionen kann variieren, wenn der Cloudbetreiber Ihre Azure Stack-Umgebung auf eine neuere Version aktualisiert.

## <a name="next-steps"></a>Nächste Schritte

[Erstellen eines virtuellen Windows-Computers mithilfe von PowerShell in Azure Stack](azure-stack-quick-create-vm-powershell.md)

