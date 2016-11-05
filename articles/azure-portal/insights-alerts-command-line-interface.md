---
title: Verwenden der plattformübergreifenden Befehlszeilenschnittstelle (CLI) zum Erstellen von Warnungen für Azure-Dienste | Microsoft Docs
description: Verwenden Sie die Befehlszeilenschnittstelle, um Azure-Warnungen zu erstellen, die Benachrichtigungen oder eine Automatisierung auslösen, wenn die angegebenen Bedingungen erfüllt sind.
author: rboucher
manager: ''
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics

ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2016
ms.author: robb

---
# Verwenden der plattformübergreifenden Befehlszeilenschnittstelle (CLI) zum Erstellen von Warnungen für Azure-Dienste
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](insights-alerts-command-line-interface.md)
> 
> 

## Übersicht
In diesem Artikel erfahren Sie, wie Sie Warnungen mit der Azure-Befehlszeilenschnittstelle (CLI) einrichten können.

Sie können auf der Grundlage von Überwachungsmetriken für Ihre Azure-Services oder von Ereignissen, die bei diesen auftreten, eine Warnung empfangen.

* **Metrikwerte**: Die Warnung wird ausgelöst, wenn der Wert einer angegebenen Metrik einen von Ihnen festgelegten Schwellenwert in beliebiger Richtung überschreitet. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch danach, wenn diese Bedingung nicht mehr erfüllt wird.
* **Aktivitätsprotokollereignisse**: Eine Warnung kann für *jedes* Ereignis oder nur dann ausgelöst werden, wenn eine bestimmte Anzahl von Ereignissen erfolgt ist.

Sie können konfigurieren, dass bei einer Warnung Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator und Co-Administratoren
* Senden von E-Mal an weitere von Ihnen angegebene Adressen
* Aufrufen eines Webhooks
* Starten der Ausführung eines Azure-Runbooks (derzeit nur über das Azure-Portal)

Sie haben folgende Möglichkeiten zum Konfigurieren von Warnregeln und Abrufen zugehöriger Informationen:

* [Azure-Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Befehlszeilenschnittstelle (CLI)](insights-alerts-command-line-interface.md)
* [Azure Insights-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Sie können stets Hilfe zu Befehlen erhalten, indem Sie einen Befehl eingeben und am Ende „-help“ hinzufügen. Beispiel:

    azure insights alerts -help
    azure insights alerts actions email create -help


## Erstellen von Warnregeln mithilfe der CLI
1. Erfüllen Sie die Voraussetzungen, und melden Sie sich bei Azure an. Siehe [Azure Insights: CLI-Beispiele](insights-cli-samples.md). Installieren Sie also die CLI, und führen Sie diese Befehle aus. Über diese Befehle werden Sie angemeldet und über das verwendete Abonnement informiert. Außerdem wird die Ausführung von Insights-Befehlen vorbereitet.

    ```console
    azure login
    azure account show
    azure config mode arm 

    ```

1. Um vorhandene Regeln für eine Ressourcengruppe aufzulisten, verwenden Sie das folgende Format: **azure insights alerts rule list** *[Optionen] &lt;Ressourcengruppe&gt;*
   
   ```console
   azure insights alerts rule list myresourcegroupname
   
   ```
2. Um eine Regel zu erstellen, benötigen Sie zunächst verschiedene wichtige Informationen.
   
   * Die **Ressourcen-ID** der Ressource, für die Sie eine Warnung festlegen möchten
   * Die für diese Ressource verfügbaren **Metrikdefinitionen**
     
     Eine Möglichkeit zum Abrufen der Ressourcen-ID ist das Azure-Portal. Falls die Ressource bereits erstellt wurde, wählen Sie sie im Portal aus. Wählen Sie auf dem nächsten Blatt *Eigenschaften* im Abschnitt *Einstellungen* aus. Die *Ressourcen-ID* ist ein Feld auf dem nächsten Blatt. Eine andere Möglichkeit ist der [Azure-Ressourcen-Explorer](https://resources.azure.com/).
     
     Es folgt ein Beispiel einer Ressourcen-ID für eine Web-App:
     
     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```
     
     Um eine Liste der verfügbaren Metriken und Einheiten für diese Metriken für das vorherige Ressourcenbeispiel zu erhalten, geben Sie den folgenden CLI-Befehl ein:
     
     ```console
     azure insights metrics list /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename PT1M 
     ```
     
     *PT1M* ist die Granularität der verfügbaren Messung (1-Minuten-Intervalle). Durch Verwenden verschiedener Granularitäten erhalten Sie verschiedene Metrikoptionen.
3. Geben Sie zum Erstellen einer metrikbasierten Warnregel einen Befehl im folgenden Format ein:
   
    **azure insights alerts rule metric set** *[Optionen] &lt;Regelname&gt; &lt;Speicherort&gt; &lt;Ressourcengruppe&gt; &lt;Fenstergröße&gt; &lt;Operator&gt; &lt;Schwellenwert&gt; &lt;Zielressourcen-ID&gt; &lt;Metrikname&gt; &lt;Zeitaggregationsoperator&gt;*
   
    Im folgenden Beispiel wird eine Warnung für eine Websiteressource eingerichtet. Die Warnung wird ausgelöst, wenn fünf Minuten durchgängig Datenverkehr empfangen wird. Sie wird erneut ausgelöst, wenn fünf Minuten lang kein Datenverkehr empfangen wird.
   
    ```console
    azure insights alerts rule metric set myrule eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
   
    ```
4. Um einen Webhook zu erstellen oder E-Mail zu senden, wenn eine Warnung ausgelöst wird, müssen Sie zunächst die E-Mail und/oder Webhooks erstellen. Erstellen Sie dann die Regel unmittelbar danach. Über die CLI können Sie bereits erstellten Regeln keine Webhooks oder E-Mails zuordnen.
   
    ```console
    azure insights alerts actions email create --customEmails myemail@contoso.com
   
    azure insights alerts actions webhook create https://www.contoso.com
   
    azure insights alerts rule metric set myrulewithwebhookandemail eastus myreasourcegroup PT5M GreaterThan 2 /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename BytesReceived Total
    ```
5. Zum Erstellen einer Warnung, die bei einer bestimmten Bedingung im Aktivitätsprotokoll ausgelöst wird, wählen Sie dieses Format:
   
    **insights alerts rule log set** *[Optionen] &lt;Regelname&gt; &lt;Speicherort&gt; &lt;Ressourcengruppe&gt; &lt;Vorgangsname&gt;*
   
    Beispiel:
   
    ```console
    azure insights alerts rule log set myActivityLogRule eastus myresourceGroupName Microsoft.Storage/storageAccounts/listKeys/action
    ```
   
    Der Vorgangsname entspricht einem Ereignistyp eines Eintrags im Aktivitätsprotokoll. Beispiele sind *Microsoft.Compute/virtualMachines/delete* und *microsoft.insights/diagnosticSettings/write*.
   
    Sie können den PowerShell-Befehl [Get-AzureRmProviderOperation](https://msdn.microsoft.com/library/mt603720.aspx) zum Abrufen einer Liste möglicher Vorgangsnamen verwenden. Alternativ können Sie im Azure-Portal das Aktivitätsprotokoll abfragen und bestimmte erfolgte Vorgänge suchen, für die Sie eine Warnung erstellen möchten. Die Vorgänge werden in der grafischen Protokollansicht mit dem Anzeigenamen angezeigt. Suchen Sie im JSON-Code nach dem Eintrag, und entnehmen Sie den Wert des Vorgangsnamen.
6. Sie können überprüfen, ob Warnungen ordnungsgemäß erstellt wurden, indem Sie sich eine einzelne Regel ansehen.
   
    ```console
    azure insights alerts rule list myresourcegroup --ruleName myrule
    ```
7. Verwenden Sie zum Löschen von Regeln einen Befehl mit dem folgenden Format:
   
    **insights alerts rule delete** [Optionen] &lt;Ressourcengruppe&gt; &lt;Regelname&gt;
   
    Mit diesen Befehlen werden die zuvor in diesem Artikel erstellten Regeln gelöscht.
   
    ```console
    azure insights alerts rule delete myresourcegroup myrule
    azure insights alerts rule delete myresourcegroup myrulewithwebhookandemail
    azure insights alerts rule delete myresourcegroup myActivityLogRule
    ```

## Nächste Schritte
* Verschaffen Sie sich eine [Übersicht über die Azure-Überwachung](monitoring-overview.md), einschließlich der Typen von Informationen, die Sie sammeln und überwachen können.
* Erfahren Sie mehr über das [Konfigurieren von Webhooks in Warnungen](insights-webhooks-alerts.md).
* Erfahren Sie mehr zu [Azure Automation-Runbooks](../automation/automation-starting-a-runbook.md).
* Verschaffen Sie sich einen [Überblick über das Sammeln von Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md), um detaillierte Hochfrequenzmetriken für Ihren Dienst zu erfassen.
* Verschaffen Sie sich einen Überblick über das [Sammeln von Dienstmetriken](insights-how-to-customize-monitoring.md), um sicherzustellen, dass Ihr Dienst verfügbar und reaktionsfähig ist.

<!---HONumber=AcomDC_0928_2016-->