---
title: "Erweiterungen und Features für virtuelle Computer | Microsoft Docs"
description: "Sie erhalten einen Überblick über die Erweiterungen für virtuelle Azure-Computer, gruppiert nach den bereitgestellten oder verbesserten Funktionen."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/30/2016
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: 7bd48e2ef9177369190494c38bfdcf1bb99e61ea


---
# <a name="about-virtual-machine-extensions-and-features"></a>Informationen zu Erweiterungen und Features für virtuelle Computer
## <a name="azure-vm-extensions"></a>Azure-VM-Erweiterungen
Erweiterungen für virtuelle Azure-Computer sind kleine Anwendungen, die Konfigurations- und Automatisierungsaufgaben auf virtuellen Azure-Computern nach der Bereitstellung ermöglichen. Wenn z.B. Software auf einem virtuellen Computer installiert werden muss oder ein Virenschutz oder eine Docker-Konfiguration erforderlich ist, kann eine VM-Erweiterung zum Ausführen dieser Aufgaben verwendet werden. Azure VM-Erweiterungen können mithilfe der Azure-CLI, PowerShell, Resource Manager-Vorlagen und dem Azure-Portal ausgeführt werden. Erweiterungen können mit einer neuen Bereitstellung für virtuelle Computer gebündelt oder in Bezug auf ein bestehendes System ausgeführt werden.

In diesem Dokument finden Sie Informationen zu den Voraussetzungen für Erweiterungen für virtuelle Azure-Computer sowie dazu, wie Sie die verfügbaren VM-Erweiterungen ermitteln. 

## <a name="azure-vm-agent"></a>Azure-VM-Agent
Der Azure-VM-Agent verwaltet die Interaktion eines virtuellen Azure-Computers mit dem Azure Fabric Controller. Der VM-Agent ist für viele funktionale Aspekte in Bezug auf die Bereitstellung und Verwaltung virtueller Azure-Computer verantwortlich. Dies umfasst auch das Ausführen von VM-Erweiterungen. Der Azure-VM-Agent ist in Images aus dem Azure-Katalog vorinstalliert und kann auf unterstützten Betriebssystemen installiert werden. 

Informationen zu unterstützten Betriebssystemen und Installationshinweise finden Sie unter [Informationen zum Agent und zu Erweiterungen für virtuelle Computer](virtual-machines-windows-classic-agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="discover-vm-extensions"></a>Ermitteln von VM-Erweiterungen
Für die Verwendung mit virtuellen Azure-Computern stehen viele verschiedene VM-Erweiterungen zur Verfügung. Wenn Sie eine vollständige Liste erhalten möchten, führen Sie mit der Azure-CLI den folgenden Befehl aus. Ersetzen Sie den Speicherort dabei durch den gewünschten Speicherort.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Häufige VM-Erweiterungen
| Name der Erweiterung | Beschreibung | Weitere Informationen |
| --- | --- | --- |
| CustomScript-Erweiterung für Windows |Ausführen von Skripts für virtuelle Azure-Computer |[Benutzerdefinierte Skripterweiterung für Windows](virtual-machines-windows-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| DSC-Erweiterung für Windows |PowerShell-DSC-Erweiterung (Desired State Configuration, Konfigurieren des gewünschten Zustands). |[Docker-VM-Erweiterung](virtual-machines-windows-extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure-Diagnoseerweiterung |Verwalten der Azure-Diagnose |[Azure-Diagnoseerweiterung](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |




<!--HONumber=Nov16_HO3-->


