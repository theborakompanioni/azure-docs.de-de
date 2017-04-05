---
title: "Übersicht über den Agent für virtuelle Azure-Computer | Microsoft-Dokumentation"
description: "Übersicht über den Agent für virtuelle Azure-Computer"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 0a1f212e-053e-4a39-9910-8d622959f594
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/28/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: b4802009a8512cb4dcb49602545c7a31969e0a25
ms.openlocfilehash: accfd5f0fec69175e584528ff9f6db66402cb89e
ms.lasthandoff: 03/29/2017


---
# <a name="azure-virtual-machine-agent-overview"></a>Übersicht über den Agent für virtuelle Azure-Computer

Der Agent für virtuelle Microsoft Azure-Computer (VM-Agent) ist ein geschützter, einfacher Prozess zur Verwaltung der VM-Interaktion mit dem Azure Fabric Controller. Der VM-Agent spielt eine primäre Rolle bei der Aktivierung und Ausführung von Azure-VM-Erweiterungen. VM-Erweiterungen ermöglichen es, virtuelle Computer nach der Bereitstellung zu konfigurieren (beispielsweise, um Software zu installieren und zu konfigurieren). Außerdem ermöglichen VM-Erweiterungen den Einsatz von Wiederherstellungsfeatures wie das Zurücksetzen des Administratorkennworts eines virtuellen Computers. Ohne Azure-VM-Agent können keine VM-Erweiterungen ausgeführt werden.

In diesem Dokument werden Installation, Erkennung und Entfernung des Azure VM-Agents erläutert.

## <a name="install-the-vm-agent"></a>Installieren des VM-Agents

### <a name="azure-gallery-image"></a>Azure-Katalogimage

Der Azure-VM-Agent wird standardmäßig auf jedem virtuellen Windows-Computer installiert, der auf der Grundlage eines Azure-Katalogimages bereitgestellt wird. Wenn Sie ein Azure-Katalogimage über das Verwaltungsportal, mithilfe von PowerShell, über die Befehlszeilenschnittstelle oder unter Verwendung einer Azure Resource Manager-Vorlage bereitstellen, wird der Azure-VM-Agent ebenfalls installiert. 

### <a name="manual-installation"></a>Manuelle Installation

Der Agent für virtuelle Windows-Computer kann manuell mithilfe eines Windows-Installationspakets installiert werden. Eine manuelle Installation kann erforderlich sein, wenn Sie ein benutzerdefiniertes VM-Image erstellen, das in Azure bereitgestellt wird. Wenn Sie den Agent für virtuelle Windows-Computer manuell installieren möchten, laden Sie [hier](http://go.microsoft.com/fwlink/?LinkID=394789) das VM-Agent-Installationsprogramm herunter. 

Der VM-Agent kann mittels Doppelklick auf die Windows-Installationsdatei installiert werden. Wenn Sie eine automatische oder unbeaufsichtigte Installation des VM-Agents durchführen möchten, verwenden Sie den folgenden Befehl.

```cmd
msiexec.exe /i WindowsAzureVmAgent.2.7.1198.778.rd_art_stable.160617-1120.fre /quiet
```

## <a name="detect-the-vm-agent"></a>Erkennen des VM-Agents

### <a name="powershell"></a>PowerShell

Mithilfe des Azure Resource Manager-PowerShell-Moduls können Sie Informationen zu Azure Virtual Machines abrufen. Durch Ausführen von `Get-AzureRmVM` erhalten Sie eine ganze Reihe von Informationen – unter anderem auch den Bereitstellungsstatus des Azure-VM-Agents.

```PowerShell
Get-AzureRmVM
```

Die folgenden Angaben stellen lediglich einen Teil der Ausgabe von `Get-AzureRmVM` dar. Beachten Sie die in `OSProfile` geschachtelte `ProvisionVMAgent`-Eigenschaft: Sie gibt Aufschluss darüber, ob der VM-Agent für den virtuellen Computer bereitgestellt wurde.

```PowerShell
OSProfile                  :
  ComputerName             : myVM
  AdminUsername            : muUserName
  WindowsConfiguration     :
    ProvisionVMAgent       : True
    EnableAutomaticUpdates : True
```

Mit dem folgenden Skript können Sie eine präzise Liste mit den Namen von virtuellen Computern und dem Status des VM-Agents zurückgeben.

```PowerShell
$vms = Get-AzureRmVM

foreach ($vm in $vms) {
    $agent = $vm | Select -ExpandProperty OSProfile | Select -ExpandProperty Windowsconfiguration | Select ProvisionVMAgent
    Write-Host $vm.Name $agent.ProvisionVMAgent
}
```

### <a name="manual-detection"></a>Manuelle Erkennung

Wenn Sie bei einem virtuellen Azure-Computer unter Windows angemeldet sind, können Sie mithilfe des Task-Managers die ausgeführten Prozesse untersuchen. Öffnen Sie zum Überprüfen des Azure-VM-Agents den Task-Manager, klicken Sie auf die Registerkarte „Details“, und suchen sie nach einem Prozess namens `WindowsAzureGuestAgent.exe`. Ist dieser Prozess vorhanden, ist der VM-Agent installiert.

## <a name="upgrade-the-vm-agent"></a>Upgraden des VM-Agents

Der Azure VM-Agent für Windows wird automatisch aktualisiert. Wenn neue virtuelle Computer für Azure bereitgestellt werden, erhalten diese jeweils den neuesten VM-Agent. Benutzerdefinierte VM-Images sollten manuell mit dem neuen VM-Agent aktualisiert werden.
