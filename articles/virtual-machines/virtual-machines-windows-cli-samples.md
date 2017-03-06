---
title: "Azure CLI-Beispiele für Windows | Microsoft-Dokumentation"
description: "Azure CLI-Beispiele für Windows"
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
ms.date: 02/27/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: c1cfaec3517dc490d48cb989e28d2c8d3a2bad84
ms.openlocfilehash: 7003225447425d239cfa75852b5c83bdab97b93b
ms.lasthandoff: 03/01/2017


---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Azure CLI-Beispiele für Windows-VMs

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe der Azure CLI erstellt wurden und mit denen Windows-VMs bereitgestellt werden.

| | |
|---|---|
|**Erstellen von virtuellen Computern**||
| [Erstellen eines virtuellen Computers](./scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Erstellt eine Windows-VM mit Minimalkonfiguration. |
| [Erstellen einer vollständig konfigurierten VM](./scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Erstellt eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen.|
| [Erstellen hoch verfügbarer VMs](./scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Erstellt mehrere virtuelle Computer in einer Konfiguration mit hoher Verfügbarkeit und Lastenausgleich. |
| [Erstellen einer VM und Ausführen eines Konfigurationsskripts](./scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Erstellt einen virtuellen Computer und verwendet die benutzerdefinierte Azure-Skripterweiterung zum Installieren von IIS. |
| [Erstellen einer VM und Ausführen einer DSC-Konfiguration](./scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Erstellt einen virtuellen Computer und verwendet die Azure-DSC-Erweiterung (Desired State Configuration) zum Installieren von IIS. |
|**Netzwerk-VMs**||
| [Sichern des Netzwerkdatenverkehrs zwischen virtuellen Computern](./scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Erstellt zwei virtuelle Computer, alle zugehörigen Ressourcen sowie eine interne und eine externe Netzwerksicherheitsgruppe (NSG). |
|**Überwachen virtueller Computer**||
| [Überwachen einer VM mit der Operations Management Suite](./scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) | Erstellt einen virtuellen Computer, installiert den OMS-Agent (Operations Management Suite) und registriert die VM in einem OMS-Arbeitsbereich.  |
| | |

