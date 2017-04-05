
---
title: "Größen von virtuellen Windows-Computern in Azure | Microsoft-Dokumentation"
description: "Listet die verschiedenen verfügbaren Größen für virtuelle Windows-Computer in Azure auf."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 365d6ad9ec0e0a7ad8d9742d863540646257e298
ms.lasthandoff: 03/27/2017

---

# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Größen für virtuelle Windows-Computer in Azure

Dieser Artikel beschreibt die verfügbaren Größen und Optionen für die virtuellen Azure-Computer, die Sie zum Ausführen Ihrer Windows-Apps und Workloads verwenden können. Darüber hinaus werden Überlegungen zur Bereitstellung angestellt, die Sie berücksichtigen sollten, wenn Sie eine Verwendung dieser Ressourcen planen.  Dieser Artikel ist auch für [virtuelle Linux-Computer](virtual-machines-linux-sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)verfügbar.

> [!IMPORTANT]
>* Informationen zu den Preisen der unterschiedlichen Größen finden Sie unter [Virtual Machines – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows). 
>* Informationen zu den allgemeinen Einschränkungen von virtuellen Azure-Computern finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md).
>* Speicherkosten werden separat basierend auf bereits verwendete Seiten im Speicherkonto berechnet. Weitere Informationen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).
> * Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](virtual-machines-windows-acu.md) die Computeleistung von Azure-SKUs vergleichen können.
>
>
<br>    




| Typ                     | Größen           |    Beschreibung       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Allgemeiner Zweck](virtual-machines-windows-sizes-general.md)          | DSv2, Dv2, DS, D, Av2, A0-7 | Ausgewogenes Verhältnis von CPU zu Arbeitsspeicher. Ideal für Tests und Entwicklung, kleine bis mittlere Datenbanken sowie Webserver mit geringer bis mittlerer Auslastung. |
| [Computeoptimiert](virtual-machines-windows-sizes-compute.md)        | Fs, F             | Hohes Verhältnis von CPU zu Arbeitsspeicher. Ideal für Webserver, Network Appliances, Stapelverarbeitungsvorgänge und Anwendungsserver mit mittlerer Auslastung.        |
| [Arbeitsspeicheroptimiert](virtual-machines-windows-sizes-memory.md)         | GS, G, DSv2, DS   | Hohes Verhältnis von Speicher zu Kern. Hervorragend geeignet für relationale Datenbankserver, mittlere bis große Caches und In-Memory-Analysen.                 |
| [Speicheroptimiert](virtual-machines-windows-sizes-storage.md)        | Ls                | Datenträgerdurchsatz und -E/A auf hohem Niveau. Ideal für Big Data sowie SQL- und NoSQL-Datenbanken.                                                         |
| [GPU](virtual-machines-windows-sizes-gpu.md)            | NV, NC            | Spezialisierte virtuelle Computer für intensives Grafikrendering und intensive Videobearbeitung. Mit einem oder mehreren GPUs verfügbar.       |
| [High Performance Computing](virtual-machines-windows-sizes-hpc.md) | H, A8-11          | Unsere virtuellen Computer mit den schnellsten und leistungsfähigsten CPUs, die optional über Netzwerkschnittstellen mit hohem Durchsatz (RDMA) verfügen. 

<br>

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](virtual-machines-windows-acu.md) die Computeleistung von Azure-SKUs vergleichen können.

Weitere Informationen zu den verschiedenen verfügbaren Größen von virtuellen Computern:
- [Allgemeiner Zweck](virtual-machines-windows-sizes-general.md)
- [Computeoptimiert](virtual-machines-windows-sizes-compute.md)
- [Arbeitsspeicheroptimiert](virtual-machines-windows-sizes-memory.md)
- [Speicheroptimiert](virtual-machines-windows-sizes-storage.md)
- [GPU-optimiert](virtual-machines-windows-sizes-gpu.md)
- [High Performance Computing](virtual-machines-windows-sizes-hpc.md)




