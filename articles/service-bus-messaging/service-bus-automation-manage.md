---
title: Verwalten von Azure Service Bus mithilfe von Azure Automation | Microsoft Docs
description: Erfahren Sie, wie Sie den Azure Automation-Dienst zum Verwalten von Azure Service Bus verwenden.
services: service-bus-messaging, automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 2a06e94b-92ef-4b5d-a2b7-fe827063df82
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: magoedte;csand
translationtype: Human Translation
ms.sourcegitcommit: 994a379129bffd7457912bc349f240a970aed253
ms.openlocfilehash: 4817cda757b9f85f6225237f79293860c990ca88


---
# <a name="managing-azure-service-bus-using-azure-automation"></a>Verwalten von Azure Service Bus mithilfe von Azure Automation
Dieser Leitfaden bietet eine Einführung in den Azure Automation-Dienst und zeigt, wie Azure Automation zur Vereinfachung der Verwaltung von Azure Service Bus genutzt werden kann.

## <a name="what-is-azure-automation"></a>Was ist Azure Automation?
[Azure Automation](../automation/automation-intro.md) ist ein Azure-Dienst zur Vereinfachung der Cloudverwaltung durch Prozessautomatisierung und Konfiguration des gewünschten Zustands. Mithilfe von Azure Automation können manuelle, sich wiederholende, lang andauernde und fehleranfällige Aufgaben automatisiert werden, um die Zuverlässigkeit und Effizienz für Ihre Organisation zu erhöhen und die Amortisierungszeit zu verkürzen.

Azure Automation bietet ein äußerst zuverlässiges, hoch verfügbares Workflow-Ausführungsmodul, das sich durch Skalierung an Ihre Anforderungen anpasst. In Azure Automation können Prozesse manuell, von Systemen von Drittanbietern oder in geplanten Intervallen gestartet werden, damit Aufgaben genau im Bedarfsfall ausgeführt werden.

Indem Sie die Aufgaben im Zusammenhang mit der Cloudverwaltung mit Azure Automation automatisieren, verringern Sie den Verwaltungsaufwand und ermöglichen es Ihren IT- und DevOps-Mitarbeitern, sich Aufgaben zu widmen, die einen Mehrwert für Ihr Unternehmen liefern.

## <a name="how-can-azure-automation-help-manage-azure-service-bus"></a>Wie kann Azure Automation Sie beim Verwalten von Azure Service Bus unterstützen?
Sie können Service Bus mit Azure Automation verwalten, indem Sie zu diesem Zweck die [Service Bus-REST-API](https://docs.microsoft.com/rest/api/servicebus/)einsetzen. Sie können PowerShell-Skripts in Azure Automation ausführen, um viele Ihrer Service Bus-Aufgaben mithilfe von REST-API auszuführen. Sie können diese REST-API-Aufrufe in Azure Automation auch mit den Cmdlets für andere Azure-Dienste koppeln, um komplexe Aufgaben in Azure-Diensten und Systemen anderer Hersteller zu automatisieren.

Hier sind einige Beispiele für die Verwendung von PowerShell zum Verwalten von Azure Service Bus aufgeführt:

* [Custom PowerShell cmdlets to manage Azure Service Bus queues (Benutzerdefinierte PowerShell-Cmdlets zum Verwalten von Azure Service Bus-Warteschlangen)](https://blogs.technet.microsoft.com/uktechnet/2014/12/04/sample-of-custom-powershell-cmdlets-to-manage-azure-servicebus-queues)
* [How to create Service Bus queues, topics and subscriptions using a PowerShell script (Erstellen von Service Bus-Warteschlangen, -Themen und -Abonnements mithilfe eines PowerShell-Skripts, in englischer Sprache)](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Create Azure Service Bus Namespaces using PowerShell](http://buildazure.com/2015/09/24/create-azure-service-bus-namespaces-using-powershell-and-x-plat-cli/)

Das PowerShell-Modul für die Arbeit mit Azure Service Bus in Automation-Runbooks kann aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureServiceBusCreation/1.0)heruntergeladen werden.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich nun mit den Grundlagen von Azure Automation und dessen Verwendung zum Verwalten von Azure Service Bus vertraut gemacht haben, folgen Sie diesen Links, um weitere Informationen zu Azure Automation zu erhalten.

* Tutorial [Erste Schritte](../automation/automation-first-runbook-graphical.md)
* Informationen zum [Verwalten von Service Bus mit PowerShell](service-bus-powershell-how-to-provision.md)




<!--HONumber=Jan17_HO2-->


