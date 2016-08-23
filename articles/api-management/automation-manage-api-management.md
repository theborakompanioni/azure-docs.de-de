<properties
	pageTitle="Verwalten von Azure API Management mit Azure Automation"
	description="Erfahren Sie, wie der Azure Automation-Dienst zum Verwalten von Azure API Management verwendet werden kann."
	services="api-management, automation"
	documentationCenter=""
	authors="csand-msft"
	manager="eamono"
	editor=""/>

<tags
	ms.service="api-management"
	ms.workload="mobile"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/09/2016"
	ms.author="csand"/>



#Verwalten von Azure API Management mit Azure Automation

Dieser Leitfaden bietet eine Einführung in den Azure Automation-Dienst und zeigt, wie er zur Vereinfachung der Verwaltung von Azure API Management genutzt werden kann.

## Was ist Azure Automation?

[Azure Automation](https://azure.microsoft.com/services/automation/) ist ein Azure-Dienst für die Vereinfachung der Cloudverwaltung durch eine Prozessautomatisierung. Mithilfe von Azure Automation können manuelle, sich wiederholende, lang andauernde und fehleranfällige Aufgaben automatisiert werden, um die Zuverlässigkeit und Effizienz für Ihre Organisation zu erhöhen und die Amortisierungszeit zu verkürzen.

Azure Automation bietet ein äußerst zuverlässiges, hoch verfügbares Workflow-Ausführungsmodul, das sich durch Skalierung an Ihre Anforderungen anpasst. In Azure Automation können Prozesse manuell, von Systemen von Drittanbietern oder in geplanten Intervallen gestartet werden, damit Aufgaben genau im Bedarfsfall ausgeführt werden.

Indem Sie die Aufgaben im Zusammenhang mit der Cloudverwaltung mit Azure Automation automatisieren, verringern Sie den Verwaltungsaufwand und ermöglichen es Ihren IT- und DevOps-Mitarbeitern, sich Aufgaben zu widmen, die einen Mehrwert für Ihr Unternehmen liefern.


## Wie kann Azure Automation beim Verwalten von Azure API Management nützlich sein?

API Management kann in Azure Automation mithilfe von [Windows PowerShell-Cmdlets für Azure API Management](https://azure.microsoft.com/updates/full-set-of-windows-powershell-cmdlets-for-azure-api-management-api/) verwaltet werden. Innerhalb von Azure Automation können Sie PowerShell-Workflowskripts schreiben, um viele Ihrer API Management-Aufgaben mithilfe von Cmdlets auszuführen. Sie können diese Cmdlets in Azure Automation auch mit den Cmdlets für andere Azure-Dienste koppeln, um komplexe Aufgaben in Azure-Diensten und Systemen anderer Hersteller zu automatisieren.

Hier sind einige Beispiele für die Verwendung von API Management mit Automation:
* [Azure API Management – Sichern und Wiederherstellen mithilfe von PowerShell](https://blogs.msdn.microsoft.com/katriend/2015/10/02/azure-api-management-using-powershell-for-backup-and-restore/)

## Nächste Schritte

Nachdem Sie sich nun mit den Grundlagen von Azure Automation und dessen Einsatz zur Verwaltung von Azure API Management vertraut gemacht haben, folgen Sie diesen Links, um weitere Informationen zu erhalten.

* Siehe das Tutorial [Erste Schritte](../automation/automation-first-runbook-graphical.md) zu Azure Automation.

<!---HONumber=AcomDC_0810_2016-->