---
title: "Verfügbarkeitsgruppen für Windows-VMs in Azure | Microsoft-Dokumentation"
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
ms.date: 03/17/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: d8a586834034460932516e352560b570141d9cda
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="azure-availability-sets-guidelines-for-windows-vms"></a>Richtlinien für Azure-Verfügbarkeitsgruppen für Windows-VMs

[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

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

Anwendungen sollten sich nicht auf einem einzelnen virtuellen Computer befinden. Eine Verfügbarkeitsgruppe, die einen einzelnen virtuellen Computer enthält, bietet keinen Schutz vor geplanten und ungeplanten Ereignissen innerhalb der Azure Platform. Die [Azure-SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines) erfordert mindestens zwei virtuelle Computer in einer Verfügbarkeitsgruppe, um die Verteilung der virtuellen Computer in der zugrunde liegenden Infrastruktur zu ermöglichen. Bei Verwendung von [Azure Storage Premium](../../storage/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) gilt die Azure-SLA für einen einzelnen virtuellen Computer.

Die zugrunde liegende Infrastruktur in Azure ist in mehrere Hardwarecluster unterteilt. Jeder Hardwarecluster kann einen Bereich von VM-Größen unterstützen. Eine Verfügbarkeitsgruppe kann zu einem Zeitpunkt nur auf einem einzelnen Hardwarecluster gehostet werden. Der Bereich der VM-Größen, die in einer einzelnen Verfügbarkeitsgruppe vorhanden sein können, ist deshalb auf den Bereich der VM-Größen beschränkt, die vom Hardwarecluster unterstützt werden. Der Hardwarecluster für die Verfügbarkeitsgruppe wird ausgewählt, wenn der erste virtuelle Computer in der Verfügbarkeitsgruppe bereitgestellt wird, oder wenn der erste virtuelle Computer in einer Verfügbarkeitsgruppe gestartet wird, bei der sich derzeit alle virtuellen Computer im Zustand „Beendet (Zuordnung aufgehoben)“ befinden. Der folgende PowerShell-Befehl kann verwendet werden, um den Bereich der VM-Größen zu bestimmen, der für eine Verfügbarkeitsgruppe verfügbar ist: „Get-AzureRmVMSize -ResourceGroupName \<string\> -AvailabilitySetName \<string\>“

Jeder Hardwarecluster ist in mehrere Updatedomänen und Fehlerdomänen unterteilt. Diese Domänen werden durch die Hosts definiert, die einen gemeinsamen Aktualisierungszyklus oder eine ähnliche physische Infrastruktur wie Stromversorgung und Netzwerk aufweisen. Azure verteilt Ihre virtuellen Computer automatisch in einer Verfügbarkeitsgruppe über Domänen hinweg, um Verfügbarkeit und Fehlertoleranz zu gewährleisten. Abhängig von der Größe der Anwendung und der Anzahl der virtuellen Computer in einer Verfügbarkeitsgruppe können Sie die Anzahl der Domänen anpassen, die Sie verwenden möchten. Informieren Sie sich über das [Verwalten von Verfügbarkeit und die Verwendung von Update- und Fehlerdomänen](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Beim Entwerfen der Anwendungsinfrastruktur sollten Sie auch die Anwendungsebenen planen, die Sie verwenden möchten. Gruppieren Sie virtuelle Computer, die demselben Zweck dienen, in einer Verfügbarkeitsgruppe, z.B. in einer Verfügbarkeitsgruppe für virtuelle Front-End-Computer mit IIS. Erstellen Sie eine separate Verfügbarkeitsgruppe für Ihre virtuellen Back-End-Computer mit SQL Server. Dadurch soll gewährleistet werden, dass jede Komponente der Anwendung durch eine Verfügbarkeitsgruppe geschützt ist und mindestens eine Instanz immer ausgeführt wird.

Lastenausgleichsmodule können vor jeder Anwendungsebene zusammen mit einer Verfügbarkeitsgruppe genutzt werden und sicherstellen, dass der Datenverkehr immer an eine aktive Instanz weitergeleitet werden kann. Ohne einen Load Balancer werden Ihre virtuellen Computer möglicherweise während einer geplanten und ungeplanten Wartung weiter ausgeführt, Ihre Endbenutzer können aber möglicherweise die Probleme nicht beheben, wenn der primäre virtuelle Computer nicht verfügbar ist.

Entwerfen Sie Ihre Anwendung für hohe Verfügbarkeit auf Speicherebene. Die bewährte Methode besteht darin, [Managed Disks für VMs in einer Verfügbarkeitsgruppe zu verwenden](manage-availability.md#use-managed-disks-for-vms-in-an-availability-set). Falls Sie derzeit nicht verwaltete Datenträger verwenden, empfehlen wir Ihnen dringend, [VMs in der Verfügbarkeitsgruppe für die Verwendung von Managed Disks zu konvertieren](convert-unmanaged-to-managed-disks.md#convert-vms-in-an-availability-set-to-managed-disks-in-a-managed-availability-set).

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


