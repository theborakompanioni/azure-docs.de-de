---
title: "Richtlinien für Azure-Verfügbarkeitsgruppen | Microsoft Docs"
description: "Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für die Bereitstellung von Verfügbarkeitsgruppen in Azure-Infrastrukturdiensten."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f9449f58-664b-4d5d-82f6-84c5d083047f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 2e981b0bebbc88fdad23ce8279762fe717e48a4c
ms.openlocfilehash: 59ef4be4854c4625bed5f9f1f32e7771ccb6979d


---
# <a name="azure-availability-sets-guidelines"></a>Richtlinien für Azure-Verfügbarkeitsgruppen
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

In diesem Artikel werden die erforderlichen Planungsschritte für Verfügbarkeitsgruppen erläutert, sodass sichergestellt ist, dass auf Ihre Anwendung bei geplanten oder ungeplanten Ereignissen zugegriffen werden kann.

## <a name="implementation-guidelines-for-availability-sets"></a>Implementierungsrichtlinien für Verfügbarkeitsgruppen
Entscheidungen:

* Wie viele Verfügbarkeitsgruppen benötigen Sie für die verschiedenen Rollen und Ebenen in Ihrer Anwendungsinfrastruktur?

Aufgaben:

* Definieren Sie die Anzahl der virtuellen Computer in jeder Anwendungsebene, die Sie benötigen.
* Bestimmen Sie, ob Sie die Anzahl der Fehler- oder Updatedomäne für Ihre Anwendung anpassen müssen.
* Definieren Sie die erforderlichen Verfügbarkeitsgruppen mithilfe Ihrer Namenskonvention und die virtuellen Computer in den Verfügbarkeitsgruppen. Ein virtueller Computer kann sich nur in einer Verfügbarkeitsgruppe befinden. 

## <a name="availability-sets"></a>Verfügbarkeitsgruppen
In Azure können virtuelle Computer (VMs) in einer logischen Gruppierung, die als Verfügbarkeitsgruppe bezeichnet wird, platziert werden. Wenn Sie virtuelle Computer innerhalb einer Verfügbarkeitsgruppe erstellen, verteilt die Azure-Plattform die Platzierung der virtuellen Computer über die zugrunde liegende Infrastruktur. Bei einer geplanten Wartung der Azure-Plattform oder einem Fehler der zugrunde liegenden Hardware/Infrastruktur wird mithilfe von Verfügbarkeitsgruppen sichergestellt, dass mindestens ein virtueller Computer weiterhin ausgeführt wird.

Anwendungen sollten sich nicht auf einem einzelnen virtuellen Computer befinden. Eine Verfügbarkeitsgruppe, die einen einzelnen virtuellen Computer enthält, bietet keinen Schutz vor geplanten und ungeplanten Ereignissen innerhalb der Azure Platform. Die [Azure-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) erfordert mindestens zwei virtuelle Computer in einer Verfügbarkeitsgruppe, um die Verteilung der virtuellen Computer in der zugrunde liegenden Infrastruktur zu ermöglichen.

Die zugrunde liegende Infrastruktur in Azure ist in Updatedomänen und Fehlerdomänen unterteilt. Diese Domänen werden durch die Hosts definiert, die einen gemeinsamen Aktualisierungszyklus oder eine ähnliche physische Infrastruktur wie Stromversorgung und Netzwerk aufweisen. Azure verteilt Ihre virtuellen Computer automatisch in einer Verfügbarkeitsgruppe über Domänen hinweg, um Verfügbarkeit und Fehlertoleranz zu gewährleisten. Abhängig von der Größe der Anwendung und der Anzahl der virtuellen Computer in einer Verfügbarkeitsgruppe können Sie die Anzahl der Domänen anpassen, die Sie verwenden möchten. Informieren Sie sich über das [Verwalten von Verfügbarkeit und die Verwendung von Update- und Fehlerdomänen](virtual-machines-windows-manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Beim Entwerfen der Anwendungsinfrastruktur sollten Sie auch die Anwendungsebenen planen, die Sie verwenden möchten. Gruppieren Sie virtuelle Computer, die demselben Zweck dienen, in einer Verfügbarkeitsgruppe, z.B. in einer Verfügbarkeitsgruppe für virtuelle Front-End-Computer mit IIS. Erstellen Sie eine separate Verfügbarkeitsgruppe für Ihre virtuellen Back-End-Computer mit SQL Server. Dadurch soll gewährleistet werden, dass jede Komponente der Anwendung durch eine Verfügbarkeitsgruppe geschützt ist und mindestens eine Instanz immer ausgeführt wird.

Lastenausgleichsmodule können vor jeder Anwendungsebene zusammen mit einer Verfügbarkeitsgruppe genutzt werden und sicherstellen, dass der Datenverkehr immer an eine aktive Instanz weitergeleitet werden kann. Ohne ein Lastenausgleichsmodul werden Ihre virtuellen Computer möglicherweise während einer geplanten und ungeplanten Wartung weiter ausgeführt, Ihre Endbenutzer können aber möglicherweise die Probleme nicht beheben, wenn der primäre virtuelle Computer nicht verfügbar ist.

Entwerfen Sie Ihre Anwendung für hohe Verfügbarkeit auf Speicherebene. Verwenden Sie als bewährte Methode mehrere Speicherkonten für jeden virtuellen Computer in einer Verfügbarkeitsgruppe. Alle Datenträger (Betriebssystem und Daten) sollten einem virtuellen Computer im selben Speicherkonto zugeordnet sein. Beachten Sie die [Grenzwerte](../storage/storage-scalability-targets.md) des Speicherkontos, wenn Sie einem Speicherkonto weitere VHDs hinzufügen.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]




<!--HONumber=Dec16_HO3-->


