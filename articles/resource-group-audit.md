---
title: Überwachen von Vorgängen mit dem Ressourcen-Manager | Microsoft Docs
description: Verwenden Sie das Aktivitätsprotokoll in Resource Manager, um Benutzeraktionen und Fehler zu überprüfen. Zeigt Azure-Portal, PowerShell, Azure-Befehlszeilenschnittstelle und REST.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn

ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: tomfitz

---
# Überwachen von Vorgängen mit dem Ressourcen-Manager
Mithilfe von Aktivitätsprotokollen können Sie Folgendes ermitteln:

* Welche Vorgänge in den Ressourcen in Ihrem Abonnement ausgeführt wurden
* Wer den Vorgang initiiert hat (auch wenn die von einem Back-End-Dienst initiierten Vorgänge keinen Benutzer als Aufrufer zurückgeben)
* Wann der Vorgang abgeschlossen wurde
* Den Status des Vorgangs
* Die Werte anderer Eigenschaften, mit denen Sie den Vorgang eventuell untersuchen können

[!INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

In diesem Thema geht es um Überwachungsvorgänge. Informationen zur Verwendung der Aktivitätsprotokolle für die Problembehandlung bei einer Bereitstellung finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen in Azure](resource-manager-troubleshoot-deployments-portal.md).

Sie können Informationen aus den Aktivitätsprotokollen über das Portal, PowerShell, die Azure-Befehlszeilenschnittstelle, die Insights-REST-API oder die [Insights-.NET-Bibliothek](https://www.nuget.org/packages/Microsoft.Azure.Insights/) abrufen.

## Verwenden des Portals zum Anzeigen von Aktivitätsprotokollen
1. Um die Aktivitätsprotokolle im Portal anzuzeigen, wählen Sie **Weitere Dienste** und **Aktivitätsprotokolle** aus.
   
    ![Aktivitätsprotokolle auswählen](./media/resource-group-audit/select-audit-logs.png)
2. Auf dem Blatt **Aktivitätsprotokoll** wird eine Übersicht über die zuletzt ausgeführten Vorgänge für alle Ressourcengruppen in Ihrem Abonnement angezeigt. Die zuletzt ausgeführten Vorgänge werden in Listenform angezeigt.
   
    ![Aktionen anzeigen](./media/resource-group-audit/audit-summary.png)
3. Um die Anzahl der angezeigten Vorgänge zu einzuschränken, wählen Sie verschiedene Bedingungen aus. Die folgende Abbildung zeigt beispielsweise die Felder **Zeitraum** und **Ereignis initiiert von**, die geändert wurden, um die Aktionen anzuzeigen, die von einem bestimmten Benutzer oder einer bestimmten Anwendung im letzten Monat durchgeführt wurden.
   
    ![Filteroptionen festlegen](./media/resource-group-audit/set-filter.png)
4. Wählen Sie **Übernehmen** aus, um die Ergebnisse Ihrer Abfrage anzuzeigen.
5. Wenn Sie die Abfrage später erneut ausführen möchten, wählen Sie **Speichern** aus, und benennen Sie die Abfrage.
   
    ![Abfrage speichern](./media/resource-group-audit/save-query.png)
6. Sie können auch automatisch nach einer bestimmten Ressource oder Ressourcengruppe filtern, indem Sie auf dem Blatt der jeweiligen Ressource bzw. Ressourcengruppe die Option **Aktivitätsprotokoll** auswählen. Beachten Sie, dass das Aktivitätsprotokoll automatisch nach der ausgewählten Ressource gefiltert wird.
   
    ![Nach Ressourcen filtern](./media/resource-group-audit/filtered-by-resource.png)

## Verwenden von PowerShell zum Anzeigen von Aktivitätsprotokollen
1. Um Protokolleinträge abzurufen, führen Sie den Befehl **Get-AzureRmLog** aus. Wenn Sie die Liste der Einträge filtern möchten, können Sie dem Befehl Parameter hinzufügen. Wenn Sie keine Start- und Endzeit angeben, werden die Einträge der letzten Stunde zurückgegeben. Führen Sie beispielsweise folgenden Befehl aus, um die Vorgänge für eine Ressourcengruppe während der letzten Stunde abzurufen:
   
        Get-AzureRmLog -ResourceGroup ExampleGroup
   
    Das folgende Beispiel zeigt, wie Sie mithilfe des Überwachungsprotokolls nach Vorgängen suchen können, die während eines bestimmten Zeitraums durchgeführt wurden. Die Start- und Enddaten werden in einem Datumsformat angegeben.
   
        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
   
    Alternativ können Sie mithilfe von Datumsfunktionen den Datumsbereich angeben, beispielsweise die letzten 14 Tage.
   
        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
2. Je nachdem, welche Startzeit Sie angeben, können die oben stehenden Befehle eine sehr lange Liste mit Vorgängen für diese Ressourcengruppe zurückgeben. Sie können die Ergebnisse filtern, indem Sie Suchkriterien eingeben. Wenn Sie beispielsweise herausfinden möchten, wie eine Web-App angehalten wurde, können Sie folgenden Befehl ausführen.
   
        Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
   
    In diesem Beispiel wird damit gezeigt, dass die Aktion durch someone@contoso.com durchgeführt wurde.
   
        Authorization     :
        Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
        Action    : Microsoft.Web/sites/stop/action
        Role      : Subscription Admin
        Condition :
        Caller            : someone@contoso.com
        CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
        EventSource       : Administrative
        EventTimestamp    : 8/28/2015 4:08:18 PM
        OperationName     : Microsoft.Web/sites/stop/action
        ResourceGroupName : ExampleGroup
        ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
        Status            : Succeeded
        SubscriptionId    : xxxxx
        SubStatus         : OK
3. Sie können die Aktionen anzeigen, die von einem bestimmten Benutzer durchgeführt wurden, selbst für eine Ressourcengruppe, die nicht mehr vorhanden ist.
   
        Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com

## Verwenden der Azure-Befehlszeilenschnittstelle zum Anzeigen von Aktivitätsprotokollen
1. Um Protokolleinträge abzurufen, führen Sie den Befehl **azure group log show** aus.
   
        azure group log show ExampleGroup
2. Sie können die Ergebnisse mit einem JSON-Hilfsprogramm wie z. B. [jq](http://stedolan.github.io/jq/download/) filtern. Das folgende Beispiel zeigt, wie Sie nach Vorgängen zur Aktualisierung einer Webkonfigurationsdatei suchen.
   
        azure group log show ExampleGroup --json | jq ".[] | select(.operationName.localizedValue == "Update web sites config")"
3. Sie können die Aktionen für einen bestimmten Benutzer überprüfen.
   
        azure group log show ExampleGroup --json | jq ".[] | select(.caller=="someone@contoso.com")"

## REST-API zum Anzeigen von Überwachungsprotokollen
Die REST-Vorgänge für die Arbeit mit Aktivitätsprotokollen gehören zur [Insights-REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Informationen zum Abrufen von Aktivitätsprotokollereignissen finden Sie unter [Auflisten der Verwaltungsereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## Nächste Schritte
* Azure-Aktivitätsprotokolle können mit Power BI verwendet werden, um bessere Einblicke in die in Ihrem Abonnement ausgeführten Aktionen zu erhalten. Informationen hierzu finden Sie unter [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/) (Anzeigen und Analysieren von Azure-Aktivitätsprotokollen in Power BI und mehr).
* Informationen zum Festlegen von Sicherheitsrichtlinien finden Sie unter [Rollenbasierte Access Control in Azure](active-directory/role-based-access-control-configure.md).
* Weitere Informationen zu den Befehlen zur Problembehandlung bei Bereitstellungen finden Sie unter [Problembehandlung beim Bereitstellen von Ressourcengruppen in Azure](resource-manager-troubleshoot-deployments-portal.md).
* Informationen zum Verhindern von Löschvorgängen für eine Ressource für alle Benutzer finden Sie unter [Sperren von Ressourcen mit Azure Resource Manager](resource-group-lock-resources.md).

<!---HONumber=AcomDC_0824_2016-->