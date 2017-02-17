---
title: Verwalten von Azure-Web-Apps mit Azure Automation | Microsoft-Dokumentation
description: Erfahren Sie, wie der Azure Automation-Dienst zur Verwaltung von Azure-Web-Apps verwendet werden kann.
services: app-service\web, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: c960a44f-f941-401d-afba-a4bc0f0394eb
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: magoedte;csand
translationtype: Human Translation
ms.sourcegitcommit: 4edd2696c9a5709ded6e2a3e352090775335f0d2
ms.openlocfilehash: 4fcfa2e7ec2e8257407026ed4cca0e15fd0b5bb6


---
# <a name="managing-azure-web-app-using-azure-automation"></a>Verwalten von Azure-Web-Apps mit Azure Automation
Dieser Leitfaden bietet eine Einführung in den Azure Automation-Dienst und zeigt, wie Azure Automation zur Vereinfachung der Verwaltung Ihrer Azure-Web-Apps genutzt werden kann.

## <a name="what-is-azure-automation"></a>Was ist Azure Automation?
[Azure Automation](../automation/automation-intro.md) ist ein Azure-Dienst für die Vereinfachung der Cloudverwaltung durch eine  Prozessautomatisierung. Mithilfe von Azure Automation können manuelle, sich wiederholende, lang andauernde und fehleranfällige Aufgaben automatisiert werden, um die Zuverlässigkeit und Effizienz für Ihre Organisation zu erhöhen und die Amortisierungszeit zu verkürzen.

Azure Automation bietet ein äußerst zuverlässiges, hoch verfügbares Workflow-Ausführungsmodul, das sich durch Skalierung an Ihre Anforderungen anpasst. In Azure Automation können Prozesse manuell, von Systemen von Drittanbietern oder in geplanten Intervallen gestartet werden, damit Aufgaben genau im Bedarfsfall ausgeführt werden.

Indem Sie die Aufgaben im Zusammenhang mit der Cloudverwaltung mit Azure Automation automatisieren, verringern Sie den Verwaltungsaufwand und ermöglichen es Ihren IT- und DevOps-Mitarbeitern, sich Aufgaben zu widmen, die einen Mehrwert für Ihr Unternehmen liefern.

## <a name="how-can-azure-automation-help-manage-azure-web-app"></a>Inwiefern unterstützt Azure Automation die Verwaltung von Azure-Web-Apps?
Web-Apps können in Azure Automation durch die Verwendung von PowerShell-Cmdlets verwaltet werden, die in den [Azure PowerShell-Modulen](/powershell/azureps-cmdlets-docs)zur Verfügung stehen. Sie können [diese PowerShell-Cmdlets für Web-Apps in Azure Automation installieren](https://azure.microsoft.com/blog/announcing-azure-resource-manager-support-azure-automation-runbooks/), sodass Sie alle Aufgaben im Zusammenhang mit der Verwaltung Ihrer Web-Apps innerhalb des Diensts ausführen können. Sie können diese Cmdlets in Azure Automation auch an die Cmdlets für andere Azure-Dienste koppeln, um komplexe Aufgaben über Azure-Dienste und Drittanbietersysteme hinweg zu automatisieren.

Hier einige Beispiele für die Verwaltung von App Services mit Automation:

* [Skripts für die Verwaltung von Web-Apps](https://azure.microsoft.com/documentation/scripts/)

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen von Azure Automation vertraut gemacht haben, folgen Sie diesen Links, um weitere Informationen zu Azure Automation zu erhalten.

* Tutorial [Erste Schritte](../automation/automation-first-runbook-graphical.md)




<!--HONumber=Dec16_HO1-->


