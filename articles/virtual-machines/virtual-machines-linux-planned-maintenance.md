---
title: Geplante Wartung für virtuelle Linux-Computer | Microsoft Docs
description: Grundlagen zur geplanten Wartung in Azure – was sie ist und wie sie sich auf die Verfügbarkeit Ihrer über Azure ausgeführten virtuellen Linux-Computer auswirkt.
services: virtual-machines-linux
documentationcenter: ''
author: drewm
manager: timlt
editor: ''
tags: azure-service-management,azure-resource-manager

ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/26/2016
ms.author: drewm

---
# Geplante Wartung für virtuelle Linux-Computer in Azure
Grundlagen zur geplanten Wartung in Azure – was sie ist und wie sie sich auf die Verfügbarkeit Ihrer virtuellen Linux-Computer auswirken kann. Dieser Artikel ist auch für [virtuelle Windows-Computer](virtual-machines-windows-planned-maintenance.md) verfügbar.

Dieser Artikel bietet Hintergrundinformationen zur geplanten Wartung in Azure. Informationen zur Problembehandlung bei einem VM-Neustart finden Sie in [diesem Blogbeitrag, in dem die Anzeige von VM-Neustartprotokollen erläutert wird](https://azure.microsoft.com/blog/viewing-vm-reboot-logs/).

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## Gründe für das Ausführen einer geplanten Wartung durch Azure
Microsoft Azure führt regelmäßig weltweit Updates aus, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur zu verbessern, die virtuellen Computern unterliegt. Viele dieser Updates werden ohne Beeinträchtigung der virtuellen Computer oder Clouddienste ausgeführt. Dazu zählen auch speichererhaltende Updates.

Bei bestimmten Updates ist allerdings ein Neustart der virtuellen Computer erforderlich, damit die erforderlichen Aktualisierungen auf die Infrastruktur angewendet werden können. Die virtuellen Computer werden heruntergefahren, während der Patch für die Infrastruktur vorgenommen wird. Danach werden die virtuellen Computer neu gestartet.

Beachten Sie, dass es zwei Wartungsarten gibt, die die Verfügbarkeit Ihrer virtuellen Computer beeinträchtigen können: geplante und ungeplante. Diese Seite beschreibt, wie Microsoft Azure die geplante Wartung ausführt. Weitere Informationen über ungeplante Wartung finden Sie unter [Unterschied zwischen geplanter und ungeplanter Wartung](virtual-machines-linux-manage-availability.md).

[!INCLUDE [virtual-machines-common-planned-maintenance](../../includes/virtual-machines-common-planned-maintenance.md)]

<!---HONumber=AcomDC_0525_2016-->