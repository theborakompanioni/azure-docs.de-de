---
title: "Größen von virtuellen Azure Linux-Computern – GPU | Microsoft-Dokumentation"
description: "Auflistung der verschiedenen verfügbaren GPU-optimierten Größen für virtuelle Linux-Computer in Azure."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: cynthn
ms.translationtype: Human Translation
ms.sourcegitcommit: e155891ff8dc736e2f7de1b95f07ff7b2d5d4e1b
ms.openlocfilehash: e0cd2a14b4102797024925a72783ecaf56919aec
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---

# <a name="gpu-linux-vm-sizes"></a>GPU-optimierte Größen von virtuellen Linux-Computern

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]

## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme

Informationen zu unterstützten Betriebssystemen und Treiberanforderungen finden Sie unter [Einrichten von GPU-Treibern für virtuelle Computer der N-Serie unter Linux](n-series-driver-setup.md).

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-considerations](../../../includes/virtual-machines-n-series-considerations.md)]

* Sie sollten nicht X-Server oder andere Systeme installieren, die den Nouveau-Treiber auf Ubuntu-NC-VMs verwenden. Vor der Installation von NVIDIA-GPU-Treibern müssen Sie den Nouveau-Treiber deaktivieren.  

## <a name="other-sizes"></a>Andere Größen
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [High Performance Computing](sizes-hpc.md)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](acu.md) die Computeleistung von Azure-SKUs vergleichen können.
