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
ms.date: 03/08/2017
ms.author: nepeters
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: 89de5f64faa143fc329b9aa93afb588ed815e3cc
ms.contentlocale: de-de
ms.lasthandoff: 06/05/2017


---
# <a name="azure-cli-samples-for-linux-virtual-machines"></a>Azure CLI-Beispiele für Linux-VMs

Die folgende Tabelle enthält Links zu Bash-Skripts, die mithilfe der Azure CLI erstellt wurden.

| | |
|---|---|
|**Erstellen von virtuellen Computern**||
| [Erstellen eines virtuellen Computers](./../scripts/virtual-machines-linux-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine Linux-VM mit Minimalkonfiguration. |
| [Erstellen einer vollständig konfigurierten VM](./../scripts/virtual-machines-linux-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt eine Ressourcengruppe, einen virtuellen Computer und alle zugehörigen Ressourcen.|
| [Erstellen hoch verfügbarer VMs](./../scripts/virtual-machines-linux-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt mehrere virtuelle Computer in einer Konfiguration mit hoher Verfügbarkeit und Lastenausgleich. |
| [Erstellen eines virtuellen Computers mit Docker](./../scripts/virtual-machines-linux-cli-sample-create-docker-host.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt einen virtuellen Computer, konfiguriert diesen virtuellen Computer als Docker-Host und führt einen NGINX-Container aus. |
| [Erstellen einer VM und Ausführen eines Konfigurationsskripts](./../scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt einen virtuellen Computer und verwendet die benutzerdefinierte Azure-Skripterweiterung zum Installieren von NGINX. |
| [Erstellen eines virtuellen Computers mit WordPress](./../scripts/virtual-machines-linux-cli-sample-create-vm-wordpress.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt einen virtuellen Computer und verwendet die benutzerdefinierte Azure-Skripterweiterung zum Installieren von WordPress. |
| [Erstellen eines virtuellen Computers von einem verwalteten Betriebssystemdatenträger](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json) | Erstellt einen virtuellen Computer durch Anfügen eines vorhandenen verwalteten Datenträgers als Betriebssystemdatenträger. |
| [Erstellen eines virtuellen Computers aus einer Momentaufnahme](./../scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Erstellt einen virtuellen Computer aus einer Momentaufnahme, indem aus der Momentaufnahme zuerst ein verwalteter Datenträger erstellt und dieser neue verwaltete Datenträger anschließend als Betriebssystemdatenträger angefügt wird. |
|**Verwalten von Speicher**||
| [Erstellen verwalteter Datenträger aus einer VHD](./../../storage/scripts/storage-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json) | Erstellt einen verwalteten Datenträger anhand einer bestimmten VHD als Betriebssystemdatenträger oder einer Daten-VHD als Datenträger für Daten.  |
| [Erstellen eines verwalteten Datenträgers aus einer Momentaufnahme](./../../storage/scripts/storage-linux-cli-sample-create-managed-disk-from-snapshot.md?toc=%2fcli%2fmodule%2ftoc.json) | Erstellt einen verwalteten Datenträger aus einer Momentaufnahme. |
| [Kopieren eines verwalteten Datenträgers in dasselbe oder ein anderes Abonnement](./../../storage/scripts/storage-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopiert einen verwalteten Datenträger in dasselbe oder ein anderes Abonnement in derselben Region wie der übergeordnete Datenträger. 
| [Exportieren einer Momentaufnahme als VHD-Datei in ein Speicherkonto](./../../storage/scripts/storage-linux-cli-sample-copy-snapshot-to-storage-account.md?toc=%2fcli%2fmodule%2ftoc.json) | Exportiert eine verwaltete Momentaufnahme als VHD-Datei in ein Speicherkonto in einer anderen Region. |
| [Kopieren einer Momentaufnahme in dasselbe oder ein anderes Abonnement](./../../storage/scripts/storage-linux-cli-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | Kopiert eine Momentaufnahme in dasselbe oder ein anderes Abonnement in derselben Region wie die übergeordnete Momentaufnahme. |
|**Netzwerk-VMs**||
| [Sichern des Netzwerkdatenverkehrs zwischen virtuellen Computern](./../scripts/virtual-machines-linux-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt zwei virtuelle Computer, alle zugehörigen Ressourcen sowie eine interne und eine externe Netzwerksicherheitsgruppe (NSG). |
|**Sichern von virtuellen Computern**||
| [Verschlüsseln eines virtuellen Computers und der Datenträger](./../scripts/virtual-machines-linux-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt einen Azure Key Vault, Verschlüsselungsschlüssel und Dienstprinzipal und verschlüsselt dann einen virtuellen Computer. |
|**Überwachen virtueller Computer**||
| [Überwachen einer VM mit der Operations Management Suite](./../scripts/virtual-machines-linux-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Erstellt einen virtuellen Computer, installiert den OMS-Agent (Operations Management Suite) und registriert die VM in einem OMS-Arbeitsbereich.  |
|**Problembehandlung bei virtuellen Computern**||
| [Problembehandlung bei einem Betriebssystem-Datenträger eines virtuellen Computers](./../scripts/virtual-machines-linux-cli-sample-mount-os-disk.md?toc=%2fcli%2fazure%2ftoc.json) | Bindet einen Betriebssystem-Datenträger eines virtuellen Computers als Datenträger für Daten auf einem zweiten virtuellen Computer ein. |
| | |

