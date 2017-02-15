---
title: "Erweiterungen und Features für virtuelle Computer | Microsoft Docs"
description: "Sie erhalten einen Überblick über die Erweiterungen für virtuelle Azure-Computer, gruppiert nach den bereitgestellten oder verbesserten Funktionen."
services: virtual-machines-linux
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 52f5d0ec-8f75-49e7-9e15-88d46b420e63
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/22/2016
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 63cf1a5476a205da2f804fb2f408f4d35860835f
ms.openlocfilehash: f377293e39699cd2be2493df020576389393ee0e


---
# <a name="about-virtual-machine-extensions-and-features"></a>Informationen zu Erweiterungen und Features für virtuelle Computer
## <a name="azure-vm-extensions"></a>Azure-VM-Erweiterungen
Erweiterungen für virtuelle Azure-Computer sind kleine Anwendungen, die Konfigurations- und Automatisierungsaufgaben auf virtuellen Azure-Computern nach der Bereitstellung ermöglichen. Wenn z.B. Software auf einem virtuellen Computer installiert werden muss oder ein Virenschutz oder eine Docker-Konfiguration erforderlich ist, kann eine VM-Erweiterung zum Ausführen dieser Aufgaben verwendet werden. Azure VM-Erweiterungen können mithilfe der Azure-CLI, PowerShell, Resource Manager-Vorlagen und dem Azure-Portal ausgeführt werden. Erweiterungen können mit einer neuen Bereitstellung für virtuelle Computer gebündelt oder in Bezug auf ein bestehendes System ausgeführt werden.

In diesem Dokument finden Sie Informationen zu den Voraussetzungen für Erweiterungen für virtuelle Azure-Computer sowie dazu, wie Sie die verfügbaren VM-Erweiterungen ermitteln. 

## <a name="azure-vm-agent"></a>Azure-VM-Agent
Der Azure-VM-Agent verwaltet die Interaktion eines virtuellen Azure-Computers mit dem Azure Fabric Controller. Der VM-Agent ist für viele funktionale Aspekte in Bezug auf die Bereitstellung und Verwaltung virtueller Azure-Computer verantwortlich. Dies umfasst auch das Ausführen von VM-Erweiterungen. Der Azure-VM-Agent ist in Images aus dem Azure-Katalog vorinstalliert und kann auf unterstützten Betriebssystemen installiert werden. 

Informationen zu unterstützten Betriebssystemen und Installationshinweise finden Sie im [Benutzerhandbuch für Azure Linux-Agent](virtual-machines-linux-agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="discover-vm-extensions"></a>Ermitteln von VM-Erweiterungen
Für die Verwendung mit virtuellen Azure-Computern stehen viele verschiedene VM-Erweiterungen zur Verfügung. Wenn Sie eine vollständige Liste erhalten möchten, führen Sie mit der Azure-CLI den folgenden Befehl aus. Ersetzen Sie den Speicherort dabei durch den gewünschten Speicherort.

```none
azure vm extension-image list westus
```

<br />

## <a name="common-vm-extensions"></a>Häufige VM-Erweiterungen
| Name der Erweiterung | Beschreibung | Weitere Informationen |
| --- | --- | --- |
| Benutzerdefinierte Script-Erweiterung für Linux |Ausführen von Skripts für virtuelle Azure-Computer |[Benutzerdefinierte Script-Erweiterung für Linux](virtual-machines-linux-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Docker-Erweiterung |Installiert den Docker-Daemon zur Unterstützung von Docker-Remotebefehlen. |[Docker-VM-Erweiterung](virtual-machines-linux-dockerextension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) |
| Erweiterungen für den Zugriff auf virtuelle Computer |Erneut Zugriff auf einen virtuellen Azure-Computer erhalten |[Erweiterungen für den Zugriff auf virtuelle Computer](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) |
| Azure-Diagnoseerweiterung |Verwalten der Azure-Diagnose |[Azure-Diagnoseerweiterung](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |




<!--HONumber=Nov16_HO3-->


