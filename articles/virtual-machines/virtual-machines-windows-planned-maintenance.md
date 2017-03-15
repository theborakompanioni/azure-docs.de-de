---
title: "Geplante Wartung für virtuelle Windows-Computer | Microsoft Docs"
description: "Grundlagen zur geplanten Wartung in Azure – was sie ist und wie sie sich auf die Verfügbarkeit Ihrer virtuellen in Azure ausgeführten Windows-Computer auswirkt."
services: virtual-machines-windows
documentationcenter: 
author: drewm
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: eb4b92d8-be0f-44f6-a6c3-f8f7efab09fe
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/01/2017
ms.author: drewm
translationtype: Human Translation
ms.sourcegitcommit: cea53acc33347b9e6178645f225770936788f807
ms.openlocfilehash: fecc7666f80f7a9a5b2f8daaa65fd1766616fc85
ms.lasthandoff: 03/03/2017


---
# <a name="planned-maintenance-for-virtual-machines-in-azure"></a>Geplante Wartung für virtuelle Computer in Azure
Grundlagen zur geplanten Wartung in Azure – was sie ist und wie sie sich auf die Verfügbarkeit Ihrer virtuellen Windows-Computer auswirken kann. Dieser Artikel ist auch für [virtuelle Linux-Computer](virtual-machines-linux-planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)verfügbar.

Dieser Artikel bietet Hintergrundinformationen zur geplanten Wartung in Azure. Informationen zur Problembehandlung bei einem VM-Neustart finden Sie in [diesem Blogbeitrag, in dem die Anzeige von VM-Neustartprotokollen erläutert wird](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="why-azure-performs-planned-maintenance"></a>Gründe für das Ausführen einer geplanten Wartung durch Azure
Microsoft Azure führt regelmäßig weltweit Updates aus, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur zu verbessern, die virtuellen Computern unterliegt. Viele dieser Updates werden ohne Beeinträchtigung der virtuellen Computer oder Clouddienste ausgeführt. Dazu zählen auch speichererhaltende Updates.

Bei bestimmten Updates ist allerdings ein Neustart der virtuellen Computer erforderlich, damit die erforderlichen Aktualisierungen auf die Infrastruktur angewendet werden können. Die virtuellen Computer werden heruntergefahren, während der Patch für die Infrastruktur vorgenommen wird. Danach werden die virtuellen Computer neu gestartet.

Beachten Sie, dass zwei Wartungsarten die Verfügbarkeit Ihrer virtuellen Computer beeinträchtigen können: geplante und ungeplante. Diese Seite beschreibt, wie Microsoft Azure die geplante Wartung ausführt. Weitere Informationen über ungeplante Wartungen finden Sie unter [Unterschied zwischen geplanter und ungeplanter Wartung](virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]

