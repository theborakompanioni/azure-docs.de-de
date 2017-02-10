---
title: Verwalten virtueller Computer mit Azure Automation | Microsoft Docs
description: Erfahren Sie, wie der Azure Automation-Dienst zur angemessenen Verwaltung von Azure Virtual Machines verwendet werden kann.
services: virtual-machines-windows, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: ce49f83a-f409-42ee-af74-a8ea2caa9ae8
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2016
ms.author: timlt
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 60d9c161b58419bb85c7ec473d853db4cbc40dd1


---
# <a name="managing-azure-virtual-machines-using-azure-automation"></a>Verwalten von Azure Virtual Machines mithilfe von Azure Automation
Dieser Leitfaden bietet eine Einführung in den Azure Automation-Dienst und zeigt, wie Azure Automation zur Vereinfachung der Verwaltung Ihrer virtuellen Azure-Computer genutzt werden kann.

## <a name="what-is-azure-automation"></a>Was ist Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) ist ein Azure-Dienst für die Vereinfachung der Cloudverwaltung durch eine Prozessautomatisierung. Mit Azure Automation können zeitintensive, manuelle, fehleranfällige und häufig ausgeführte Aufgaben automatisiert werden, um Zuverlässigkeit und Effizienz zu steigern und eine schnellere Amortisierung für Ihre Organisation zu erzielen.

Azure Automation bietet ein äußerst zuverlässiges und hochverfügbares Workflow-Ausführungsmodul, das nach Ihren Anforderungen skaliert werden kann, wenn Ihre Organisation wächst. In Azure Automation können Prozesse manuell, durch Drittanbietersysteme oder in geplanten Intervallen gestartet werden, sodass Aufgaben genau nach Bedarf ausgeführt werden.

Indem Sie die Aufgaben im Zusammenhang mit der Cloudverwaltung mit Azure Automation automatisieren, verringern Sie den Verwaltungsaufwand und ermöglichen es Ihren IT- und DevOps-Mitarbeitern, sich Aufgaben zu widmen, die einen Mehrwert für Ihr Unternehmen liefern.

## <a name="how-can-azure-automation-help-manage-azure-virtual-machines"></a>Wie kann Azure Automation Sie beim Verwalten virtueller Azure-Computer unterstützen?
Virtuelle Computer können in Azure Automation über [Azure PowerShell](https://msdn.microsoft.com/library/azure/jj156055.aspx)verwaltet werden. Azure Automation umfasst PowerShell-Cmdlets, sodass Sie alle Verwaltungsaufgaben Ihrer virtuellen Computer innerhalb des Diensts ausführen können. Sie können die Cmdlets in Azure Automation auch an die Cmdlets für andere Azure-Dienste koppeln, um komplexe Aufgaben über Azure-Dienste und Drittanbietersysteme hinweg zu automatisieren.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen von Azure Automation und dessen Einsatz zur Verwaltung von Azure virtuellen Computern vertraut gemacht haben, erfahren Sie mehr:

* [Übersicht über Azure Automation](../automation/automation-intro.md)
* [Mein erstes Runbook](../automation/automation-first-runbook-graphical.md)
* [Azure Automation Lernkurve](https://azure.microsoft.com/documentation/learning-paths/automation/)




<!--HONumber=Nov16_HO3-->


