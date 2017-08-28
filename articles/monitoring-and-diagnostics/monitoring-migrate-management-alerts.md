---
title: "Migrieren von Azure-Warnungen für Verwaltungsereignisse zu Aktivitätsprotokollwarnungen | Microsoft-Dokumentation"
description: "Warnungen für Verwaltungsereignisse werden am 1. Oktober entfernt. Bereiten Sie sich darauf vor, indem Sie vorhandene Warnungen migrieren."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 08a457029d3721f5c38dbcd2d2aab7d09a241d8f
ms.contentlocale: de-de
ms.lasthandoff: 08/15/2017

---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a>Migrieren von Azure-Warnungen für Verwaltungsereignisse zu Aktivitätsprotokollwarnungen


> [!WARNING]
> Warnungen für Verwaltungsereignisse werden am oder nach dem 1. Oktober deaktiviert. Befolgen Sie die nachstehenden Anweisungen, um herauszufinden, ob Sie über solche Warnungen verfügen, und diese gegebenenfalls zu migrieren.
>
> 

## <a name="what-is-changing"></a>Was ändert sich?

Azure Monitor (ehemals „Azure Insights“) hat eine Funktion zum Erstellen einer Warnung bereitgestellt, die Verwaltungsereignisse ausgelöst und Benachrichtigungen für eine Webhook-URL oder für E-Mail-Adressen generiert hat. Möglicherweise haben Sie eine dieser Warnungen auf eine der folgenden Weisen erstellt:
* Durch Auswählen von „Überwachung“ -> „Warnungen“ -> „Warnung hinzufügen“ im Azure-Portal für bestimmte Ressourcentypen und Festlegen der Option „Warnung bei“ auf „Ereignisse“
* Durch Ausführen des PowerShell-Cmdlets „Add-AzureRmLogAlertRule“
* Durch direktes Verwenden [der REST-API für Warnungen](http://docs.microsoft.com/rest/api/monitor/alertrules) mit den Werten „ManagementEventRuleCondition“ für „odata.type“ und „RuleManagementEventDataSource“ für „dataSource.odata.type“
 
Das folgende PowerShell-Skript gibt eine Liste aller Warnungen für Verwaltungsereignisse zurück, die in Ihrem Abonnement vorliegen, sowie die für die einzelnen Warnungen festgelegten Bedingungen.

```powershell
Login-AzureRmAccount
$alerts = $null
foreach ($rg in Get-AzureRmResourceGroup ) {
  $alerts += Get-AzureRmAlertRule -ResourceGroup $rg.ResourceGroupName
}
foreach ($alert in $alerts) {
  if($alert.Properties.Condition.DataSource.GetType().Name.Equals("RuleManagementEventDataSource")) {
    "Alert Name: " + $alert.Name
    "Alert Resource ID: " + $alert.Id
    "Alert conditions:"
    $alert.Properties.Condition.DataSource
    "---------------------------------"
  }
} 
```

Wenn keine Warnungen für Verwaltungsereignisse vorliegen, gibt das oben aufgeführte PowerShell-Cmdlet eine Reihe von Warnmeldungen wie die Folgende aus:

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

Diese Warnmeldungen können ignoriert werden. Wenn Warnungen für Verwaltungsereignisse vorliegen, sieht die Ausgabe dieses PowerShell-Cmdlets wie folgt aus:

```
Alert Name: webhookEvent1
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/webhookEvent1
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : microsoft.web/sites/start/action
ResourceGroupName    : 
ResourceProviderName : 
Status               : succeeded
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Web/sites/samplealertapp

---------------------------------
Alert Name: someclilogalert
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/someclilogalert
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : Start
ResourceGroupName    : 
ResourceProviderName : 
Status               : 
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Compute/virtualMachines/Seaofclouds

---------------------------------
```

Jede Warnung wird durch eine gestrichelte Linie getrennt und enthält Details wie die Ressourcen-ID der Warnung und die jeweilige zu überwachende Regel.

Diese Funktionalität wurde auf [Azure Monitor-Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts.md) übertragen. Mithilfe dieser neuen Warnungen können Sie eine Bedingung für Aktivitätsprotokollereignisse festlegen und eine Benachrichtigung erhalten, wenn ein neues Ereignis der Bedingung entspricht. Zudem bringen sie auch verschiedene Verbesserungen für Warnungen zu Verwaltungsereignisse mit sich:
* Sie können Ihre Gruppe von Benachrichtigungsempfängern („Aktionen“) mittels [Aktionsgruppen](monitoring-action-groups.md) für eine Vielzahl von Warnungen wiederverwenden, wodurch die Komplexität beim Ändern der Person, die eine Warnung erhalten soll, verringert wird.
* Sie können durch Aktionsgruppen direkt per SMS eine Benachrichtigung auf Ihrem Telefon empfangen.
* Sie können [Aktivitätsprotokollwarnungen mit Resource Manager-Vorlagen erstellen](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
* Sie können Bedingungen mit größerer Flexibilität und Komplexität im Hinblick auf Ihre speziellen Anforderungen erstellen.
* Benachrichtigungen werden schneller übermittelt.
 
## <a name="how-to-migrate"></a>Vorgehensweise zum Migrieren
 
Für die Erstellung einer neuen Aktivitätsprotokollwarnung stehen Ihnen folgende Informationsquellen zur Verfügung:
* Unser Leitfaden zum [Erstellen einer Warnung im Azure-Portal](monitoring-activity-log-alerts.md)
* Der Artikel [Erstellen einer Warnung mithilfe einer Resource Manager-Vorlage](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
 
Zuvor erstellte Warnungen für Verwaltungsereignisse werden nicht automatisch zu Aktivitätsprotokollwarnungen migriert. Sie müssen das vorangehende PowerShell-Skript verwenden, um die gegenwärtig konfigurierten Warnungen für Verwaltungsereignisse aufzulisten und diese manuell als Aktivitätsprotokollwarnungen neu zu erstellen. Dies muss vor dem 1. Oktober geschehen, da Warnungen für Verwaltungsereignisse danach nicht mehr in Ihrem Azure-Abonnement angezeigt werden. Andere Arten von Azure-Warnungen, einschließlich Azure Monitor-Metrikwarnungen, Application Insights-Warnungen und Log Analytics-Warnungen, sind von dieser Änderung nicht betroffen. Bei Fragen fügen Sie in den unten stehenden Kommentaren Ihren Beitrag hinzu.


## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu [Aktivitätsprotokollen](monitoring-overview-activity-logs.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen über das Azure-Portal](monitoring-activity-log-alerts.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen über den Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md)
* Weitere Informationen zu [Dienstbenachrichtigungen](monitoring-service-notifications.md)
* Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md)

