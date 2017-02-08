---
title: Erstellen eines klassischen Linux-Computers mit der Azure-CLI | Microsoft Docs
description: Erfahren Sie, wie Sie einen virtuellen Linux-Computer mithilfe der Azure-Befehlszeilenschnittstelle und dem klassischen Bereitstellungsmodell erstellen.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: f8071a2e-ed91-4f77-87d9-519a37e5364f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/14/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: f6537e4ebac76b9f3328223ee30647885ee15d3e
ms.openlocfilehash: d3817e5e0f2c237375cb36993661968358c28b55


---
# <a name="how-to-create-a-linux-vm-with-the-azure-cli"></a>Erstellen einer Linux-VM mit der Azure-Befehlszeilenschnittstelle
> [!IMPORTANT] 
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager- und klassische Bereitstellung](../azure-resource-manager/resource-manager-deployment-model.md). Dieser Artikel befasst sich mit der Verwendung des klassischen Bereitstellungsmodells. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Die Resource Manager-Version finden Sie [hier](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Dieses Thema enthält Informationen zum Erstellen eines virtuellen Linux-Computers (VM, Virtual Machine) mithilfe der Azure-Befehlszeilenschnittstelle und des klassischen Bereitstellungsmodells. Wir verwenden ein Linux-Image aus den verfügbaren **IMAGES** in Azure. Die Azure-Befehlszeilenschnittstellen-Befehle enthalten u.a. folgende Konfigurationsoptionen:

* Verbinden des virtuellen Computers mit einem virtuellen Netzwerk
* Hinzufügen des virtuellen Computers zu einem vorhandenen Clouddienst
* Hinzufügen des virtuellen Computers zu einem vorhandenen Speicherkonto
* Hinzufügen des virtuellen Computers zu einer Verfügbarkeitsgruppe oder einem Speicherort

> [!IMPORTANT]
> Wenn Ihr virtueller Computer ein virtuelles Netzwerk verwenden soll, damit Sie sich direkt über den Hostnamen oder über eine lokal eingerichtete Verbindung mit ihm verbinden können, stellen Sie sicher, dass Sie das virtuelle Netzwerk schon dann angeben, wenn Sie den virtuellen Computer erstellen. Ein virtueller Computer kann so konfiguriert werden, dass er nur zum Zeitpunkt seiner Erstellung einem virtuellen Netzwerk hinzugefügt werden kann. Ausführliche Informationen über virtuelle Netzwerke erhalten Sie unter [Übersicht über Azure Virtual Network](http://go.microsoft.com/fwlink/p/?LinkID=294063).
> 
> 

## <a name="how-to-create-a-linux-vm-using-the-classic-deployment-model"></a>Erstellen einer Linux-VM mithilfe des klassischen Bereitstellungsmodells
[!INCLUDE [virtual-machines-create-LinuxVM](../../includes/virtual-machines-create-linuxvm.md)]




<!--HONumber=Dec16_HO1-->


