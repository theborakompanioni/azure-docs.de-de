---
title: "Virtueller Azure-Computer – PowerShell-Beispiele | Microsoft-Dokumentation"
description: "Virtueller Azure-Computer – PowerShell-Beispiele"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/16/2017
ms.author: nepeters
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: c57bf0efc1eb97d34d76483b8889d884ccfd0988
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="azure-virtual-machine-powershell-samples"></a>Virtueller Azure-Computer – PowerShell-Beispiele

Die folgende Tabelle enthält Links zu Beispielen von PowerShell-Skripts, die virtuelle Windows-Computer erstellen und verwalten.

| | |
|---|---|
|**Erstellen von virtuellen Computern**||
| [Erstellen einer vollständig konfigurierten VM](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen.|
| [Erstellen einer VM und Ausführen eines Konfigurationsskripts](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt einen virtuellen Computer und verwendet die benutzerdefinierte Azure-Skripterweiterung zum Installieren von IIS. |
| [Erstellen einer VM und Ausführen einer DSC-Konfiguration](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt einen virtuellen Computer und verwendet die Azure-DSC-Erweiterung (Desired State Configuration) zum Installieren von IIS. |
| [Erstellen eines virtuellen Computers von einem verwalteten Betriebssystemdatenträger](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt einen virtuellen Computer durch Anfügen eines vorhandenen verwalteten Datenträgers als Betriebssystemdatenträger. |
| [Erstellen eines virtuellen Computers aus einer Momentaufnahme](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt einen virtuellen Computer aus einer Momentaufnahme, indem aus der Momentaufnahme zuerst ein verwalteter Datenträger erstellt und dieser neue verwaltete Datenträger anschließend als Betriebssystemdatenträger angefügt wird. |
|**Überwachen virtueller Computer**||
| [Überwachen einer VM mit der Operations Management Suite](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Erstellt einen virtuellen Computer, installiert den OMS-Agent (Operations Management Suite) und registriert die VM in einem OMS-Arbeitsbereich.  |
| | |

