---
title: "Wartung für Windows-Computer in Azure mit Beibehaltung der virtuellen Computer | Microsoft-Dokumentation"
description: "Direkte Migration von virtuellen Computern für Aktualisierungen mit Speicherbeibehaltung."
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
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 08da5407cc5ddceeba21a558dc0de1008a566bab
ms.lasthandoff: 04/03/2017


---



# <a name="vm-preserving-maintenance-in-place-vm-migration"></a>Wartung mit Beibehaltung der virtuellen Computer (direkte Migration von virtuellen Computern)

Die Mehrzahl der Aktualisierungen haben zwar keine Auswirkung auf gehostete virtuelle Computer, es gibt jedoch Situationen, in denen Aktualisierungen von Komponenten und Diensten minimale Beeinträchtigungen bei der Ausführung von virtuellen Computern bewirken (ohne kompletten Neustart des jeweiligen virtuellen Computers).

Diese Updates werden mit Techniken durchgeführt, die eine direkte Livemigration ermöglichen (auch als „speichererhaltende“ Aktualisierung bezeichnet). Bei der Aktualisierung des Hosts wird der virtuelle Computer in einen pausierten Zustand versetzt. Dadurch bleibt der Speicher im Arbeitsspeicher erhalten, während die Hostumgebung (d.h. das zugrunde liegende Betriebssystem) die erforderlichen Updates und Patches anwendet.
Der virtuelle Computer wird innerhalb von 30 Sekunden Pause wieder fortgesetzt.
Die Uhr des fortgesetzten virtuellen Computers wird automatisch synchronisiert.

Nicht alle Updates können mit diesem Mechanismus bereitgestellt werden. Dank der Kürze der Pausierung verringert diese Art der Updatebereitstellung jedoch deutlich die Auswirkungen auf virtuelle Computer.

Updates für mehrere Instanzen (virtuelle Computer in einer Verfügbarkeitsgruppe) werden nacheinander auf die einzelnen Updatedomänen angewendet.

Auf einem virtuellen Computer ausgeführte Anwendungen werden über anstehende Aktualisierungen benachrichtigt, indem die geplanten Ereignisse des Metadatendienstes aufgerufen werden. Weitere Informationen zu geplanten Ereignissen finden Sie unter [Azure-Metadatendienst: Geplante Ereignisse](../virtual-machines-scheduled-events.md).
