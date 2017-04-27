---
title: "Geplante Wartung für virtuelle Windows-Computer in Azure | Microsoft-Dokumentation"
description: "Grundlagen zur geplanten Wartung in Azure – was sie ist und wie sie sich auf die Verfügbarkeit Ihrer virtuellen in Azure ausgeführten Windows-Computer auswirkt."
services: virtual-machines-windows
documentationcenter: 
author: 
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: eb4b92d8-be0f-44f6-a6c3-f8f7efab09fe
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: 
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 198c1804b342b8faf406a08eff7bc42994b9dd0d
ms.lasthandoff: 03/31/2017


---
# <a name="planned-maintenance-for-windows-virtual-machines"></a>Geplante Wartung für virtuelle Windows-Computer 

Microsoft Azure führt regelmäßig weltweit Updates aus, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur zu verbessern, die virtuellen Computern unterliegt. Diese Updates reichen von Patches für Softwarekomponenten in der Hostumgebung (Betriebssystem, Hypervisor und diverse auf dem Host bereitgestellte Agents), Aktualisierungen von Netzwerkkomponenten bis hin zur Außerbetriebnahme von Hardware.

Die Mehrheit dieser Updates wird ohne Auswirkungen auf gehostete virtuelle Computer oder Cloud Services ausgeführt.

Es gibt jedoch Situationen, in denen Updates Auswirkungen auf gehostete virtuelle Computer haben:

-   In „Beibehalten der Wartung von virtuellen Computern (direkte Migration von virtuellen Computern)“ wird eine Klasse von Aktualisierungen beschrieben, bei denen virtuelle Computer während der Wartung nicht neu gestartet werden.

-   Neustarten von VMs zu Wartungszwecken, wobei ein Neustart oder eine erneute Bereitstellung auf gehosteten virtuellen Computern erforderlich ist.

Beachten Sie, dass auf dieser Seite beschrieben wird, wie Microsoft Azure die geplante Wartung ausführt. Weitere Informationen zu ungeplanten Ereignissen (Ausfällen) finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer](manage-availability.md).
