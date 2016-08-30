<properties
	pageTitle="Streamen des Azure-Aktivitätsprotokolls an Event Hubs | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie das Azure-Aktivitätsprotokoll an Event Hubs streamen."
	authors="johnkemnetz"
	manager="rboucher"
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/17/2016"
	ms.author="johnkem"/>

# Streamen des Azure-Aktivitätsprotokolls an Event Hubs
Das [**Azure-Aktivitätsprotokoll**](./monitoring-overview-activity-logs.md) kann nahezu in Echtzeit an eine beliebige Anwendung gestreamt werden – entweder mithilfe der integrierten Exportoption des Portals oder durch Aktivieren der Service Bus-Regel-ID in einem Protokollprofil (über die Azure PowerShell-Cmdlets oder über die Azure-Befehlszeilenschnittstelle).

## Verwendungsmöglichkeiten für das Aktivitätsprotokoll und Event Hubs
Im Anschluss finden Sie eine kleine Auswahl von Verwendungsmöglichkeiten für das Streamen des Aktivitätsprotokolls:

- **Streamen an Protokollierungs- und Telemetriesysteme von Drittanbietern:** Event Hubs-Streaming entwickelt sich mehr und mehr zum Mechanismus für die Weiterreichung des Aktivitätsprotokolls an SIEMs und Protokollanalyselösungen von Drittanbietern.
- **Erstellen einer benutzerdefinierter Telemetrie- und Protokollierungsplattform:** Event Hubs ermöglicht dank des hochgradig skalierbaren Veröffentlichen/Abonnieren-Konzepts eine flexible Erfassung des Aktivitätsprotokolls. Dies ist interessant, wenn Sie bereits über eine benutzerdefinierte Telemetrieplattform verfügen oder eine solche Plattform erstellen möchten. Informationen zur Verwendung von Event Hubs für eine globale Telemetrieplattform finden Sie in der [Anleitung von Dan Rosanova](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## Aktivieren des Aktivitätsprotokollstreamings
Das Aktivitätsprotokollstreaming kann entweder programmgesteuert oder über das Portal aktiviert werden. In beiden Fällen wählen Sie einen Service Bus-Namespace aus. (Falls noch kein Namespace vorhanden ist, müssen Sie einen erstellen.) Der ausgewählte Namespace fungiert als Ziel für die Event Hub-Erstellung (falls Sie das Aktivitätsprotokoll zum ersten Mal streamen) oder für das Streaming (falls Sie das Aktivitätsprotokoll bereits an diesen Namespace gestreamt haben), und die Richtlinie definiert die Berechtigungen für den Streamingmechanismus. Für das Streaming an Event Hubs werden aktuell Verwaltungs-, Lese- und Sendeberechtigungen benötigt. Sie können freigegebene Zugriffsrichtlinien für Ihren Service Bus-Namespace im klassischen Portal auf der Registerkarte „Konfigurieren“ erstellen oder ändern. Wenn Sie dem Protokollprofil für das Aktivitätsprotokoll das Streaming-Feature hinzufügen möchten, muss der Client in der Service Bus-Autorisierungsregel über die ListKey-Berechtigung verfügen.

### Verwenden des Azure-Portals 
1. Navigieren Sie über das Menü auf der linken Seite des Portals zum Blatt **Aktivitätsprotokoll**.

    ![Navigation zum Aktivitätsprotokoll im Portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klicken Sie oben auf dem Blatt auf die Schaltfläche **Exportieren**.

    ![Schaltfläche „Exportieren“ im Portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Auf dem angezeigten Blatt können Sie die Regionen, für die Sie Ereignisse streamen möchten, und den Service Bus-Namespace auswählen, in dem eine Event Hub-Instanz zum Streamen dieser Ereignisse erstellt werden soll.

    ![Blatt zum Exportieren des Aktivitätsprotokolls](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klicken Sie auf **Speichern**, um die Einstellungen zu speichern. Die Einstellungen werden sofort auf Ihr Abonnement angewendet.


### Verwenden von PowerShell-Cmdlets
Falls bereits ein Protokollprofil vorhanden ist, müssen Sie dieses zuerst entfernen.

1. Ermitteln Sie mithilfe von `Get-AzureRmLogProfile`, ob ein Protokollprofil vorhanden ist.
2. Falls ja, entfernen Sie es mithilfe von `Remove-AzureRmLogProfile`.
3. Verwenden Sie `Set-AzureRmLogProfile`, um ein Profil zu erstellen:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: {Service Bus-Ressourcen-ID}/authorizationrules/{Schlüsselname}. Beispiel:

### Verwenden der Azure-Befehlszeilenschnittstelle
Falls bereits ein Protokollprofil vorhanden ist, müssen Sie dieses zuerst entfernen.

1. Ermitteln Sie mithilfe von `azure insights logprofile list`, ob ein Protokollprofil vorhanden ist.
2. Falls ja, entfernen Sie es mithilfe von `azure insights logprofile delete`.
3. Verwenden Sie `azure insights logprofile add`, um ein Profil zu erstellen:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`.
 
## Nutzen der Protokolldaten aus Event Hubs
Das Schema für das Aktivitätsprotokoll finden Sie [hier](./monitoring-overview-activity-logs.md). Jedes Ereignis ist ein Array mit JSON-Blobs, die als Datensätze bezeichnet werden.

## Nächste Schritte
- [Lesen Sie die Übersicht über das Azure-Aktivitätsprotokoll.](./monitoring-overview-activity-logs.md)
- [Richten Sie eine Warnung ein, die auf einem Aktivitätsprotokollereignis basiert.](./insights-auditlog-to-webhook-email.md)

<!---HONumber=AcomDC_0817_2016-->