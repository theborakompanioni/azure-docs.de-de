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
ms.date: 04/26/2016
ms.author: drewm
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: f81d4a407e738aa8133e91a24f06feba5af75e63


---
# <a name="planned-maintenance-for-virtual-machines-in-azure"></a>Geplante Wartung für virtuelle Computer in Azure
Grundlagen zur geplanten Wartung in Azure – was sie ist und wie sie sich auf die Verfügbarkeit Ihrer virtuellen Windows-Computer auswirken kann. Dieser Artikel ist auch für [virtuelle Linux-Computer](virtual-machines-linux-planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)verfügbar. 

Dieser Artikel bietet Hintergrundinformationen zur geplanten Wartung in Azure. Informationen zur Problembehandlung bei einem VM-Neustart finden Sie in [diesem Blogbeitrag, in dem die Anzeige von VM-Neustartprotokollen erläutert wird](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## <a name="why-azure-performs-planned-maintenance"></a>Gründe für das Ausführen einer geplanten Wartung durch Azure
Microsoft Azure führt regelmäßig weltweit Updates aus, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur zu verbessern, die virtuellen Computern unterliegt. Viele dieser Updates werden ohne Beeinträchtigung der virtuellen Computer oder Clouddienste ausgeführt. Dazu zählen auch speichererhaltende Updates.

Bei bestimmten Updates ist allerdings ein Neustart der virtuellen Computer erforderlich, damit die erforderlichen Aktualisierungen auf die Infrastruktur angewendet werden können. Die virtuellen Computer werden heruntergefahren, während der Patch für die Infrastruktur vorgenommen wird. Danach werden die virtuellen Computer neu gestartet.

Beachten Sie, dass es zwei Wartungsarten gibt, die die Verfügbarkeit Ihrer virtuellen Computer beeinträchtigen können: geplante und ungeplante. Diese Seite beschreibt, wie Microsoft Azure die geplante Wartung ausführt. Weitere Informationen über ungeplante Wartungen finden Sie unter [Unterschied zwischen geplanter und ungeplanter Wartung](virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]




<!--HONumber=Nov16_HO3-->


