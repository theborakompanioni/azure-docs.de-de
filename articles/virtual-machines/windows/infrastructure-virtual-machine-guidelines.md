---
title: "Richtlinien für Azure Virtual Machines | Microsoft Docs"
description: "Erfahren Sie mehr über die wichtigsten Entwurfs- und Implementierungsrichtlinien für die Bereitstellung virtueller Windows-Computer in Azure."
documentationcenter: 
services: virtual-machines-windows
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f932e65a-437b-48b0-8d70-f61ded8ce1c6
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: f5fd03ef7e18706bd99688acb83cda02fb4a027a
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
# <a name="azure-virtual-machines-guidelines-for-windows"></a>Richtlinien für virtuelle Azure-Computer unter Windows
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-intro](../../../includes/virtual-machines-windows-infrastructure-guidelines-intro.md)]

In diesem Artikel liegt das Hauptaugenmerk darauf, die erforderlichen Planungsschritte zum Erstellen und Verwalten virtueller Computer (VMs) in Ihrer Azure-Umgebung zu erläutern.

## <a name="implementation-guidelines-for-vms"></a>Implementierungsrichtlinien für virtuelle Computer
Entscheidungen:

* Wie viele virtuelle Computer benötigen Sie für die verschiedenen Anwendungsebenen und Komponenten Ihrer Infrastruktur?
* Über welche CPU- und Speicherressourcen muss jeder virtuelle Computer verfügen, und welche Speicheranforderungen liegen vor?

Aufgaben:

* Definieren Sie die Workloads für Ihre Anwendung und die Ressourcen, die die VMs benötigen.
* Stimmen Sie den Ressourcenbedarf für jeden virtuellen Computer mit der entsprechenden Größe und dem Speichertyp der virtuellen Computer ab.
* Definieren Sie die Ressourcengruppen für die verschiedenen Ebenen und Komponenten Ihrer Infrastruktur.
* Definieren Sie die Namenskonvention für virtuelle Computer.
* Erstellen Sie die virtuellen Computer mit der Azure PowerShell, dem Webportal oder mit Resource Manager-Vorlagen.

## <a name="virtual-machines"></a>Virtuelle Computer
Virtuelle Computer gehören wahrscheinlich zu den wichtigsten Ressourcen in Ihrer Azure-Umgebung. Auf dieser Ressource führen Sie Ihre Anwendungen, Datenbanken, Authentifizierungsdienste usw. aus.

Sie müssen die [unterschiedlichen Größen virtueller Computer](sizes.md) kennen, um die Größe Ihrer Umgebung im Hinblick auf Leistung und Kosten richtig festzulegen. Wenn Ihre virtuellen Computer nicht über genügend CPU-Kerne oder Arbeitsspeicher verfügen, leidet die Leistung der Anwendung unabhängig davon, wie gut sie entworfen und entwickelt wurde. Überprüfen Sie zunächst die vorgeschlagenen Workloads für jede Serie virtueller Computer, wenn Sie entscheiden, welche Größe Sie für virtuelle Computer für jede Komponente in Ihrer Infrastruktur verwenden möchten. Sie können [die Größe eines virtuellen Computers](resize-vm.md) nach der Bereitstellung ändern.

Speicher spielt eine wichtige Rolle für die Leistung virtueller Computer. Sie können Standardspeicher auf normalen rotierenden Festplatten oder Storage Premium-Speicher auf SSD-Datenträgern für hohe E/A-Workloads und Höchstleistung nutzen. Wie die Größe des virtuellen Computers müssen auch Kostenaspekte berücksichtigt werden, wenn es darum geht, das Speichermedium auszuwählen. Sie können den [Artikel mit Richtlinien für die Speicherinfrastruktur](infrastructure-storage-solutions-guidelines.md) lesen, um zu verstehen, wie der geeignete Speicher für eine optimale Leistung Ihrer virtuellen Computer entworfen wird.

## <a name="resource-groups"></a>Ressourcengruppen
Komponenten wie virtuelle Computer werden zur Vereinfachung der Verwaltung und Wartung mit [Azure-Ressourcengruppen](../../azure-resource-manager/resource-group-overview.md) logisch gruppiert. Mithilfe von Ressourcengruppen können Sie alle Ressourcen, die eine bestimmte Anwendung bilden, erstellen, verwalten und überwachen. Sie können auch [rollenbasierte Zugriffskontrollen](../../active-directory/role-based-access-control-what-is.md) implementieren, um anderen Personen in Ihrem Team Zugriff nur auf die jeweils erforderlichen Ressourcen zu gewähren. Planen Sie die Ressourcengruppen und Rollenzuweisungen sorgfältig. Es gibt verschiedene Ansätze für das eigentliche Entwerfen und Implementieren von Ressourcengruppen. Lesen Sie also unbedingt den [Artikel mit Richtlinien für Ressourcengruppen](infrastructure-resource-groups-guidelines.md), damit Sie verstehen, wie Sie Ihre virtuellen Computer am besten erstellen.

## <a name="templates"></a>Vorlagen
Sie können Vorlagen erstellen, die durch deklarative JSON-Dateien definiert sind, um Ihre virtuellen Computer zu erstellen. Mit Vorlagen werden in der Regel auch die erforderlichen Speicher, Netzwerke, Netzwerkschnittstellen, IP-Adressen usw. zusammen mit den virtuellen Computern selbst erstellt. Sie können Vorlagen zum Erstellen konsistenter und reproduzierbarer Umgebungen für Entwicklungs- und Testzwecke verwenden, um auf einfache Weise Produktionsumgebungen zu replizieren (und umgekehrt). Lesen Sie mehr über das [Erstellen und Verwenden von Vorlagen](../../azure-resource-manager/resource-group-overview.md#template-deployment), um zu verstehen, wie Sie sie zum Erstellen und Bereitstellen der virtuellen Computer verwenden können.

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [virtual-machines-windows-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-windows-infrastructure-guidelines-next-steps.md)]


