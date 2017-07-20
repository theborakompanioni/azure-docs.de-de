---
title: "Wartung für Windows-Computer in Azure mit Beibehaltung der virtuellen Computer | Microsoft-Dokumentation"
description: "Direkte Migration von virtuellen Computern für Updates mit Speicherbeibehaltung."
services: virtual-machines-windows
documentationcenter: 
author: zivr
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
ms.author: zivr
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 8888bafbc3aba24168312b611a9b4fbde25f376d
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---



# <a name="vm-preserving-maintenance-with-in-place-vm-migration"></a>Wartung mit direkter Migration und Beibehaltung der virtuellen Computer

Die Mehrzahl der Updates haben zwar keine Auswirkung auf gehostete virtuelle Computer, es gibt jedoch Situationen, in denen Updates von Komponenten und Diensten minimale Beeinträchtigungen bei der Ausführung von virtuellen Computern bewirken (ohne vollständigen Neustart des jeweiligen virtuellen Computers).

Diese Updates werden mit Techniken durchgeführt, die eine direkte Livemigration ermöglichen (auch als „speichererhaltende“ Aktualisierung bezeichnet). Bei der Aktualisierung des Hosts wird der virtuelle Computer in einen „angehaltenen“ Zustand versetzt. Dadurch bleibt der Speicher im Arbeitsspeicher erhalten, während die Hostumgebung (d.h. das zugrunde liegende Betriebssystem) die erforderlichen Updates und Patches anwendet.
Der virtuelle Computer wird innerhalb von 30 Sekunden Pause wieder fortgesetzt.
Die Uhr des fortgesetzten virtuellen Computers wird automatisch synchronisiert.

Nicht alle Updates können mit diesem Mechanismus bereitgestellt werden. Dank der Kürze der Pausierung verringert diese Art der Updatebereitstellung jedoch deutlich die Auswirkungen auf virtuelle Computer.

Updates für mehrere Instanzen (virtuelle Computer in einer Verfügbarkeitsgruppe) werden nacheinander auf die einzelnen Updatedomänen angewendet.

Einige Anwendungen werden möglicherweise mehr als andere durch diese Updates beeinträchtigt. Anwendungen, die Ereignisverarbeitung, Medienstreaming oder Transcodierung in Echtzeit durchführen, sowie Szenarien mit hohem Netzwerkdurchsatz können z.B. nicht für die Tolerierung einer 30-Sekunden-Pause konfiguriert werden. Auf einem virtuellen Computer ausgeführte Anwendungen werden über anstehende Updates benachrichtigt, indem die API für [geplante Ereignisse](../virtual-machines-scheduled-events.md) des [Azure-Metadatendiensts](../virtual-machines-instancemetadataservice-overview.md) aufgerufen wird.

