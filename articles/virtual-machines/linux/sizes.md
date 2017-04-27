---
title: "Größen von virtuellen Linux-Computern in Azure | Microsoft-Dokumentation"
description: "Listet die verschiedenen verfügbaren Größen für virtuelle Linux-Computer in Azure auf."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: d068cf9b6b2fd9084ffc60cac4837aa378a09c79
ms.lasthandoff: 04/12/2017

---

# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Größen für virtuelle Linux-Computer in Azure
Dieser Artikel beschreibt die verfügbaren Größen und Optionen für die virtuellen Azure-Computer, die Sie zum Ausführen Ihrer Linux-Apps und Workloads verwenden können. Darüber hinaus werden Überlegungen zur Bereitstellung angestellt, die Sie berücksichtigen sollten, wenn Sie eine Verwendung dieser Ressourcen planen. Dieser Artikel ist auch für [virtuelle Windows-Computer](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)verfügbar.

> [!IMPORTANT]
> * Informationen zu den Preisen der unterschiedlichen Größen finden Sie unter [Virtual Machines – Preise](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
> * Informationen zur Verfügbarkeit von VM-Größen in Azure-Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/).
> * Informationen zu den allgemeinen Einschränkungen von virtuellen Azure-Computern finden Sie unter [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../../azure-subscription-service-limits.md).
> * Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](../windows/acu.md) die Computeleistung von Azure-SKUs vergleichen können.
> 
> 

<br>   


| Typ                     | Größen           |    Beschreibung       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Allgemeiner Zweck](sizes-general.md)          | DSv2, Dv2, DS, D, Av2, A0-7  | Ausgewogenes Verhältnis von CPU zu Arbeitsspeicher. Ideal für Tests und Entwicklung, kleine bis mittlere Datenbanken sowie Webserver mit geringer bis mittlerer Auslastung. |
| [Computeoptimiert](sizes-compute.md)        | Fs, F             | Hohes Verhältnis von CPU zu Arbeitsspeicher. Ideal für Webserver, Network Appliances, Stapelverarbeitungsvorgänge und Anwendungsserver mit mittlerer Auslastung.        |
| [Arbeitsspeicheroptimiert](sizes-memory.md)         | GS, G, DSv2, DS, Dv2, D   | Hohes Verhältnis von Speicher zu Kern. Hervorragend geeignet für relationale Datenbankserver, mittlere bis große Caches und In-Memory-Analysen.                 |
| [Speicheroptimiert](sizes-storage.md)        | Ls                | Datenträgerdurchsatz und -E/A auf hohem Niveau. Ideal für Big Data sowie SQL- und NoSQL-Datenbanken.                                                         |
| [GPU](sizes-gpu.md)            | NV, NC            | Spezialisierte virtuelle Computer für intensives Grafikrendering und intensive Videobearbeitung. Mit einem oder mehreren GPUs verfügbar.       |
| [High Performance Computing](sizes-hpc.md) | H, A8-11          | Unsere virtuellen Computer mit den schnellsten und leistungsfähigsten CPUs, die optional über Netzwerkschnittstellen mit hohem Durchsatz (RDMA) verfügen. 

<br>

Weitere Informationen dazu, wie Sie mit [Azure-Computeeinheiten (ACU)](../windows/acu.md) die Computeleistung von Azure-SKUs vergleichen können.

Weitere Informationen zu den verschiedenen verfügbaren Größen von virtuellen Computern:
- [Allgemeiner Zweck](sizes-general.md)
- [Computeoptimiert](sizes-compute.md)
- [Arbeitsspeicheroptimiert](sizes-memory.md)
- [Speicheroptimiert](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [High Performance Computing](sizes-hpc.md)




