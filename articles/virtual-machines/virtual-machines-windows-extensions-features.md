<properties
 pageTitle="Erweiterungen und Features für virtuelle Computer | Microsoft Azure"
 description="Sie erhalten einen Überblick über die Erweiterungen für virtuelle Azure-Computer, gruppiert nach den bereitgestellten oder verbesserten Funktionen."
 services="virtual-machines-windows"
 documentationCenter=""
 authors="neilpeterson"
 manager="timlt"
 editor=""
 tags="azure-service-management,azure-resource-manager"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="infrastructure-services"
 ms.date="09/30/2016"
 ms.author="nepeters"/>


# <a name="about-virtual-machine-extensions-and-features"></a>Informationen zu Erweiterungen und Features für virtuelle Computer

## <a name="azure-vm-extensions"></a>Azure-VM-Erweiterungen

Erweiterungen für virtuelle Azure-Computer sind kleine Anwendungen, die Konfigurations- und Automatisierungsaufgaben auf virtuellen Azure-Computern nach der Bereitstellung ermöglichen. Wenn z.B. Software auf einem virtuellen Computer installiert werden muss oder ein Virenschutz oder eine Docker-Konfiguration erforderlich ist, kann eine VM-Erweiterung zum Ausführen dieser Aufgaben verwendet werden. Azure VM-Erweiterungen können mithilfe der Azure-CLI, PowerShell, Resource Manager-Vorlagen und dem Azure-Portal ausgeführt werden. Erweiterungen können mit einer neuen Bereitstellung für virtuelle Computer gebündelt oder in Bezug auf ein bestehendes System ausgeführt werden.

In diesem Dokument finden Sie Informationen zu den Voraussetzungen für Erweiterungen für virtuelle Azure-Computer sowie dazu, wie Sie die verfügbaren VM-Erweiterungen ermitteln. 

## <a name="azure-vm-agent"></a>Azure-VM-Agent

Der Azure-VM-Agent verwaltet die Interaktion eines virtuellen Azure-Computers mit dem Azure Fabric Controller. Der VM-Agent ist für viele funktionale Aspekte in Bezug auf die Bereitstellung und Verwaltung virtueller Azure-Computer verantwortlich. Dies umfasst auch das Ausführen von VM-Erweiterungen. Der Azure-VM-Agent ist in Images aus dem Azure-Katalog vorinstalliert und kann auf unterstützten Betriebssystemen installiert werden. 

Informationen zu unterstützten Betriebssystemen und Installationshinweise finden Sie unter [Informationen zum Agent und zu Erweiterungen für virtuelle Computer](./virtual-machines-windows-classic-agents-and-extensions.md).

## <a name="discover-vm-extensions"></a>Ermitteln von VM-Erweiterungen

Für die Verwendung mit virtuellen Azure-Computern stehen viele verschiedene VM-Erweiterungen zur Verfügung. Wenn Sie eine vollständige Liste erhalten möchten, führen Sie mit der Azure-CLI den folgenden Befehl aus. Ersetzen Sie den Speicherort dabei durch den gewünschten Speicherort.

```none
Get-AzureVMAvailableExtension | Select ExtensionName, Version
```

<br />

## <a name="common-vm-extensions"></a>Häufige VM-Erweiterungen

|Name der Erweiterung   |Beschreibung   |Weitere Informationen   |
|---|---|---|
|CustomScript-Erweiterung für Windows  | Ausführen von Skripts für virtuelle Azure-Computer  |[Benutzerdefinierte Skripterweiterung für Windows](./virtual-machines-windows-extensions-customscript.md)   |
|DSC-Erweiterung für Windows | PowerShell-DSC-Erweiterung (Desired State Configuration, Konfigurieren des gewünschten Zustands).  | [Docker-VM-Erweiterung](./virtual-machines-windows-extensions-dsc-overview.md)  |
|Azure-Diagnoseerweiterung | Verwalten der Azure-Diagnose |[Azure-Diagnoseerweiterung](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |



<!--HONumber=Oct16_HO2-->


