---
title: Verwalten von Azure Cloud Services mithilfe von Azure Automation | Microsoft-Dokumentation
description: Erfahren Sie, wie der Azure Automation-Dienst zur angemessenen Verwaltung von Azure-Clouddiensten verwendet werden kann.
services: cloud-services, automation
documentationcenter: 
author: jodoglevy
manager: timlt
editor: 
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 47307ed521201b462730a2ea4cb8d0ca665b6d72


---
# <a name="managing-azure-cloud-services-using-azure-automation"></a>Verwalten von Azure Cloud Services mithilfe von Azure Automation
Dieser Leitfaden bietet eine Einführung in den Azure Automation-Dienst und zeigt, wie Azure Automation zur einfacheren Verwaltung Ihrer Azure Cloud Services genutzt werden kann.

## <a name="what-is-azure-automation"></a>Was ist Azure Automation?
[Azure Automation](https://azure.microsoft.com/services/automation/) ist ein Azure-Dienst für die Vereinfachung der Cloudverwaltung durch eine  Prozessautomatisierung. Mit Azure Automation können zeitintensive, manuelle, fehleranfällige und häufig ausgeführte Aufgaben automatisiert werden, um Zuverlässigkeit und Effizienz zu steigern und eine schnellere Amortisierung für Ihre Organisation zu erzielen.

Azure Automation bietet ein äußerst zuverlässiges und hochverfügbares Workflow-Ausführungsmodul, das nach Ihren Anforderungen skaliert werden kann, wenn Ihre Organisation wächst. In Azure Automation können Prozesse manuell, durch Drittanbietersysteme oder in geplanten Intervallen gestartet werden, sodass Aufgaben genau nach Bedarf ausgeführt werden.

Indem Sie die Aufgaben in Zusammenhang mit der Cloudverwaltung mit Azure Automation automatisieren, verringern Sie den Verwaltungsaufwand und ermöglichen es Ihren IT- und DevOps-Mitarbeitern, sich Aufgaben zu widmen, die einen Mehrwert für Ihr Unternehmen liefern.

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Wie kann Azure Automation Sie beim Verwalten von Azure Cloud Services unterstützen?
Azure Cloud Services können in Azure Automation über die PowerShell-Cmdlets verwaltet werden, die in den [Azure PowerShell-Tools](https://msdn.microsoft.com/library/azure/jj156055.aspx)zur Verfügung stehen. Diese PowerShell-Cmdlets für Clouddienste sind in Azure Automation integriert und sofort einsatzfähig, sodass Sie alle Aufgaben in Zusammenhang mit der Verwaltung Ihrer Clouddienste innerhalb des Diensts ausführen können. Sie können diese Cmdlets in Azure Automation auch mit den Cmdlets für andere Azure-Dienste koppeln, um komplexe Aufgaben in Azure-Diensten und Systemen anderer Hersteller zu automatisieren.

Beispiele für die Verwendung von Azure Automation für die Verwaltung von Azure Cloud Services:

* [Kontinuierliche Bereitstellung eines Clouddiensts, wenn die CSCFG- oder CSPKG-Datei im Azure-BLOB-Speicher aktualisiert wird](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [Paralleles Neustarten von Clouddienstinstanzen (Upgradedomäne für Upgradedomäne)](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen von Azure Automation und dessen Verwendung zum Verwalten von Azure Cloud Services vertraut gemacht haben, können Sie diese Links verwenden, um weitere Informationen zu Azure Automation zu erhalten.

* [Übersicht über Azure Automation](../automation/automation-intro.md)
* [Mein erstes Runbook](../automation/automation-first-runbook-graphical.md)
* [Azure Automation Lernkurve](https://azure.microsoft.com/documentation/learning-paths/automation/)



<!--HONumber=Nov16_HO3-->


