---
title: "Erstellen von Warnungen für Azure-Dienste: PowerShell | Microsoft-Dokumentation"
description: "E-Mails, Benachrichtigungen oder Automatisierung werden ausgelöst und URLs (Webhooks) von Websites aufgerufen, wenn die von Ihnen angegebenen Bedingungen erfüllt sind."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d26ab15b-7b7e-42a9-81c8-3ce9ead5d252
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: robb
translationtype: Human Translation
ms.sourcegitcommit: 2c9877f84873c825f96b62b492f49d1733e6c64e
ms.openlocfilehash: 62faba3827e9fc33e9788cd2d487adf04d760791
ms.lasthandoff: 03/15/2017


---
# <a name="create-alerts-in-azure-monitor-for-azure-services---powershell"></a>Erstellen von Warnungen in Azure Monitor für Azure-Dienste – PowerShell 
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Übersicht
In diesem Artikel erfahren Sie, wie Sie Warnungen mit PowerShell einrichten können.  

Sie können auf der Grundlage von Überwachungsmetriken für Ihre Azure-Services oder von Ereignissen, die bei diesen auftreten, eine Warnung empfangen.

* **Metrikwerte** : Die Warnung wird ausgelöst, wenn der Wert einer angegebenen Metrik einen von Ihnen festgelegten Schwellenwert in beliebiger Richtung überschreitet. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch danach, wenn diese Bedingung nicht mehr erfüllt wird.    
* **Aktivitätsprotokollereignisse** : Eine Warnung kann für *jedes* Ereignis oder nur dann ausgelöst werden, wenn eine bestimmte Anzahl von Ereignissen erfolgt ist.

Sie können konfigurieren, dass bei einer Warnung Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator und Co-Administratoren
* Senden von E-Mal an weitere von Ihnen angegebene Adressen
* Aufrufen eines Webhooks
* Starten der Ausführung eines Azure-Runbooks (nur über das Azure-Portal)

Sie haben folgende Möglichkeiten zum Konfigurieren von Warnregeln und Abrufen zugehöriger Informationen:

* [Azure-Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Befehlszeilenschnittstelle (CLI)](insights-alerts-command-line-interface.md)
* [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Um weitere Informationen zu erhalten, geben Sie ```Get-Help``` und dann den PowerShell-Befehl ein, zu dem Sie Hilfe benötigen.

## <a name="create-alert-rules-in-powershell"></a>Erstellen von Warnregeln in PowerShell
1. Melden Sie sich bei Azure an.   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. Rufen Sie eine Liste der Abonnements ab, die Ihnen zur Verfügung stehen. Stellen Sie sicher, dass Sie das richtige Abonnement verwenden. Falls nicht, legen Sie mithilfe der Ausgabe von `Get-AzureRmSubscription`das richtige fest.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. Um vorhandene Regeln für eine Ressourcengruppe aufzulisten, verwenden Sie den folgenden Befehl:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. Um eine Regel zu erstellen, benötigen Sie zunächst verschiedene wichtige Informationen.

   * Die **Ressourcen-ID** der Ressource, für die Sie eine Warnung festlegen möchten
   * Die für diese Ressource verfügbaren **Metrikdefinitionen**

     Eine Möglichkeit zum Abrufen der Ressourcen-ID ist das Azure-Portal. Falls die Ressource bereits erstellt wurde, wählen Sie sie im Portal aus. Wählen Sie dann auf dem nächsten Blatt im Abschnitt *Einstellungen* die Option *Eigenschaften* aus. **RESSOURCEN-ID** ist ein Feld auf dem nächsten Blatt. Eine andere Möglichkeit ist der [Azure-Ressourcen-Explorer](https://resources.azure.com/).

     Es folgt ein Beispiel einer Ressourcen-ID für eine Web-App:

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Sie können `Get-AzureRmMetricDefinition` zum Anzeigen der Liste aller Definitionen von Metriken für eine bestimmte Ressource verwenden.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     Im folgenden Beispiel wird eine Tabelle mit der Metrik „Name“ und der Einheit für diese Metrik generiert.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     Eine vollständige Liste der verfügbaren Optionen für „Get-AzureRmMetricDefinition“ wird durch Ausführen von „Get-MetricDefinitions“ verfügbar.
5. Im folgenden Beispiel wird eine Warnung für eine Websiteressource eingerichtet. Die Warnung wird ausgelöst, wenn fünf Minuten durchgängig Datenverkehr empfangen wird. Sie wird erneut ausgelöst, wenn fünf Minuten lang kein Datenverkehr empfangen wird.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. Um einen Webhook zu erstellen oder E-Mail zu senden, wenn eine Warnung ausgelöst wird, müssen Sie zunächst die E-Mail und/oder Webhooks erstellen. Erstellen Sie anschließend sofort die Regel mit dem „-Actions“-Tag (wie im folgenden Beispiel gezeigt). Über PowerShell können Sie bereits erstellten Regeln keine Webhooks oder E-Mails zuordnen.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```


1. Zum Erstellen einer Warnung, die bei einer bestimmten Bedingung im Aktivitätsprotokoll ausgelöst wird, wählen Sie Befehle im folgenden Format:

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmLogAlertRule -Name myLogAlertRule -Location "East US" -ResourceGroup myresourcegroup -OperationName microsoft.web/sites/start/action -Status Succeeded -TargetResourceGroup resourcegroupbeingmonitored -Actions $actionEmail, $actionWebhook
    ```

    „-OperationName“ entspricht einem Ereignistyp eines Eintrags im Aktivitätsprotokoll. Beispiele sind *Microsoft.Compute/virtualMachines/delete* und *microsoft.insights/diagnosticSettings/write*.

    Sie können den PowerShell-Befehl [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) zum Abrufen einer Liste möglicher Vorgangsnamen verwenden. Alternativ können Sie im Azure-Portal das Aktivitätsprotokoll abfragen und bestimmte erfolgte Vorgänge suchen, für die Sie eine Warnung erstellen möchten. Die Vorgänge werden in der grafischen Protokollansicht mit dem Anzeigenamen angezeigt. Suchen Sie im JSON-Code nach dem Eintrag, und entnehmen Sie den Wert des Vorgangsnamen.   
2. Überprüfen Sie, ob Warnungen ordnungsgemäß erstellt wurden, indem Sie sich die einzelnen Regeln ansehen.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
3. Löschen Sie Ihre Warnungen. Mit diesen Befehlen werden die zuvor in diesem Artikel erstellten Regeln gelöscht.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über die Azure-Überwachung](monitoring-overview.md) , einschließlich der Typen von Informationen, die Sie sammeln und überwachen können.
* Erfahren Sie mehr über das [Konfigurieren von Webhooks in Warnungen](insights-webhooks-alerts.md).
* Erfahren Sie mehr zu [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Verschaffen Sie sich einen [Überblick über das Sammeln von Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md) , um detaillierte Hochfrequenzmetriken für Ihren Dienst zu erfassen.
* Verschaffen Sie sich einen Überblick über das [Sammeln von Dienstmetriken](insights-how-to-customize-monitoring.md) , um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.

