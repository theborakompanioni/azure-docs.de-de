---
title: "Geplante Wartung für virtuelle Linux-Computer in Azure | Microsoft-Dokumentation"
description: "Grundlagen zur geplanten Wartung in Azure – was sie ist und wie sie sich auf die Verfügbarkeit Ihrer virtuellen in Azure ausgeführten Windows-Computer auswirkt."
services: virtual-machines-linux
documentationcenter: 
author: 
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/27/2017
ms.author: 
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 28c2fb5a67eca0c5ab2f0299bba7c11375e10558
ms.lasthandoff: 04/03/2017


---

# <a name="planned-maintenance-for-linux-virtual-machines"></a>Geplante Wartung für virtuelle Linux-Computer 

Microsoft Azure führt regelmäßig weltweit Updates aus, um die Zuverlässigkeit, Leistung und Sicherheit der Hostinfrastruktur zu verbessern, die virtuellen Computern unterliegt. Diese Updates reichen von Patches für Softwarekomponenten in der Hostumgebung (Betriebssystem, Hypervisor und diverse auf dem Host bereitgestellte Agents), Aktualisierungen von Netzwerkkomponenten bis hin zur Außerbetriebnahme von Hardware.

Die Mehrheit dieser Updates wird ohne Auswirkungen auf gehostete virtuelle Computer oder Clouddienste ausgeführt.

Es gibt jedoch Situationen, in denen Updates Auswirkungen auf gehostete virtuelle Computer haben:

-   In „Beibehalten der Wartung von virtuellen Computern (direkte Migration von virtuellen Computern)“ wird eine Klasse von Updates beschrieben, bei denen virtuelle Computer während der Wartung nicht neu gestartet werden.

-   Neustarten von virtuellen Computern zu Wartungszwecken, wobei ein Neustart oder eine erneute Bereitstellung auf gehosteten virtuellen Computern erforderlich ist.

Beachten Sie, dass auf dieser Seite beschrieben wird, wie Microsoft Azure die geplante Wartung ausführt. Weitere Informationen zu ungeplanten Ereignissen (Ausfällen) finden Sie unter [Verwalten der Verfügbarkeit virtueller Computer](../windows/manage-availability.md).
