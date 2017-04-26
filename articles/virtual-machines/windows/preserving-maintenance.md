---
title: "Wartung für Windows-Computer in Azure mit Beibehaltung der virtuellen Computer | Microsoft-Dokumentation"
description: "Direkte Migration von virtuellen Computern für Updates mit Speicherbeibehaltung."
services: virtual-machines-windows
documentationcenter: 
author: 
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: 
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: bc903f7523295da704ea8f0128dd553e3fdd96a9
ms.lasthandoff: 03/31/2017


---



# <a name="vm-preserving-maintenance-with-in-place-vm-migration"></a>Wartung mit direkter Migration und Beibehaltung der virtuellen Computer

Die Mehrzahl der Updates haben zwar keine Auswirkung auf gehostete virtuelle Computer, es gibt jedoch Situationen, in denen Updates von Komponenten und Diensten minimale Beeinträchtigungen bei der Ausführung von virtuellen Computern bewirken (ohne vollständigen Neustart des jeweiligen virtuellen Computers).

Diese Updates werden mit Technologien durchgeführt, die eine direkte Live-Migration ermöglichen (auch als „speichererhaltendes“ Update bezeichnet). Bei der Aktualisierung des Hosts wird der virtuelle Computer in einen „angehaltenen“ Zustand versetzt. Dadurch bleibt der Speicher im Arbeitsspeicher erhalten, während die Hostumgebung (d.h. das zugrunde liegende Betriebssystem) die erforderlichen Updates und Patches anwendet.
Der virtuelle Computer wird innerhalb von 30 Sekunden Pause wieder aktiviert.
Die Uhr des fortgesetzten virtuellen Computers wird automatisch synchronisiert.

Nicht alle Updates können mit diesem Mechanismus bereitgestellt werden. Dank der Kürze der Pausierung verringert diese Art der Updatebereitstellung jedoch deutlich die Auswirkungen auf virtuelle Computer.

Updates für mehrere Instanzen (virtuelle Computer in einer Verfügbarkeitsgruppe) werden nacheinander auf die einzelnen Updatedomänen angewendet.

Auf einem virtuellen Computer ausgeführte Anwendungen werden über anstehende Updates benachrichtigt, indem die geplanten Ereignisse des Metadatendiensts aufgerufen werden. Weitere Informationen zu geplanten Ereignissen finden Sie unter [Azure-Metadatadienst: Geplante Ereignisse](../virtual-machines-scheduled-events.md).
