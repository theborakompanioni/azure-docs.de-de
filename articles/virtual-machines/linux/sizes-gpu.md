---
title: "Größen von virtuellen Azure Linux-Computern – GPU | Microsoft-Dokumentation"
description: "Auflistung der verschiedenen verfügbaren GPU-optimierten Größen für virtuelle Linux-Computer in Azure."
services: virtual-machines-linux
documentationcenter: 
author: jonbeck7
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/28/2017
ms.author: jonbeck
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: ec289cf53f2cfecd2744b739667ef831dafd59a4
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---

# <a name="gpu-linux-vm-sizes"></a>GPU-optimierte Größen von virtuellen Linux-Computern

[!INCLUDE [virtual-machines-common-sizes-gpu](../../../includes/virtual-machines-common-sizes-gpu.md)]


[!INCLUDE [virtual-machines-common-sizes-table-defs](../../../includes/virtual-machines-common-sizes-table-defs.md)]

[!INCLUDE [virtual-machines-n-series-linux-support](../../../includes/virtual-machines-n-series-linux-support.md)]

Informationen zur Installation von Treibern und zu Überprüfungsschritten finden Sie unter [N-series driver setup for Linux](n-series-driver-setup.md) (Einrichtung von Treibern für die N-Serie unter Linux).

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
