---
title: Azure CLI-Beispiele | Microsoft-Dokumentation
description: Azure CLI-Beispiele
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/26/2017
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: 5a73094c90b2a7400a664a827e07d5ada2184952
ms.openlocfilehash: 372f6413903fbf7344871a136ffcddcadfc81782
ms.lasthandoff: 02/27/2017


---
# <a name="azure-virtual-machine-cli-samples"></a>Azure CLI-Beispiele für virtuelle Computer

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe der Azure CLI erstellt wurden.

| | |
|---|---|
|**Erstellen von virtuellen Computern**||
| [Erstellen eines virtuellen Computers](./scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Erstellt eine Linux-VM mit Minimalkonfiguration. |
| [Erstellen einer vollständig konfigurierten VM](./scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Erstellt eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen.|
| [Erstellen hoch verfügbarer VMs](./scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Erstellt mehrere virtuelle Computer in einer Konfiguration mit hoher Verfügbarkeit und Lastenausgleich. |
| [Erstellen eines virtuellen Computers mit Docker](./scripts/virtual-machines-linux-cli-sample-create-docker-host.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Erstellt einen virtuellen Computer, konfiguriert diesen virtuellen Computer als Docker-Host und führt einen NGINX-Container aus. |
| [Erstellen einer VM und Ausführen eines Konfigurationsskripts](./scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Erstellt einen virtuellen Computer und verwendet die benutzerdefinierte Azure-Skripterweiterung zum Installieren von NGINX. |
| [Erstellen eines virtuellen Computers mit WordPress](./scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Erstellt einen virtuellen Computer und verwendet die benutzerdefinierte Azure-Skripterweiterung zum Installieren von WordPress. |
|**Netzwerk-VMs**||
| [Sicherer Netzwerkdatenverkehr zwischen virtuellen Computern](./scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Erstellt zwei virtuelle Computer, alle zugehörigen Ressourcen sowie eine interne und eine externe Netzwerksicherheitsgruppe (NSG). |
|**Überwachen virtueller Computer**||
| [Überwachen einer VM mit der Operations Management Suite](./scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Erstellt einen virtuellen Computer, installiert den OMS-Agent (Operations Management Suite) und registriert die VM in einem OMS-Arbeitsbereich.  |
|**Problembehandlung bei virtuellen Computern**||
| [Problembehandlung bei einem Betriebssystem-Datenträger eines virtuellen Computers](./scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) | Bindet einen Betriebssystem-Datenträger eines virtuellen Computers als Datenträger für Daten auf einem zweiten virtuellen Computer ein. |
| | |

