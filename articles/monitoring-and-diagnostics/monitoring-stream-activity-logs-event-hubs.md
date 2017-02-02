---
title: "Streamen des Azure-Aktivitätsprotokolls an Event Hubs | Microsoft Docs"
description: "Hier erfahren Sie, wie Sie das Azure-Aktivitätsprotokoll an Event Hubs streamen."
author: johnkemnetz
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: johnkem
translationtype: Human Translation
ms.sourcegitcommit: 51a7a274c0cdbec169154bd1100abb4534283cff
ms.openlocfilehash: ac478dc5c6691ef9f6c4829bd47a81912774d032


---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Streamen des Azure-Aktivitätsprotokolls an Event Hubs
Das [**Azure-Aktivitätsprotokoll**](monitoring-overview-activity-logs.md) kann nahezu in Echtzeit an eine beliebige Anwendung gestreamt werden – entweder mithilfe der integrierten Exportoption des Portals oder durch Aktivieren der Service Bus-Regel-ID in einem Protokollprofil (über die Azure PowerShell-Cmdlets oder über die Azure-Befehlszeilenschnittstelle).

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Verwendungsmöglichkeiten für das Aktivitätsprotokoll und Event Hubs
Im Anschluss finden Sie eine kleine Auswahl von Verwendungsmöglichkeiten für das Streamen des Aktivitätsprotokolls:

* **Streamen an Protokollierungs- und Telemetriesysteme von Drittanbietern:** Event Hubs-Streaming entwickelt sich mehr und mehr zum Mechanismus für die Weiterreichung des Aktivitätsprotokolls an SIEMs und Protokollanalyselösungen von Drittanbietern.
* **Erstellen einer benutzerdefinierter Telemetrie- und Protokollierungsplattform:** Event Hubs ermöglicht dank des hochgradig skalierbaren Veröffentlichen/Abonnieren-Konzepts eine flexible Erfassung des Aktivitätsprotokolls. Dies ist interessant, wenn Sie bereits über eine benutzerdefinierte Telemetrieplattform verfügen oder eine solche Plattform erstellen möchten. [Informationen zur Verwendung von Event Hubs für eine globale Telemetrieplattform finden Sie in der Anleitung von Dan Rosanova.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Aktivieren des Aktivitätsprotokollstreamings
Das Aktivitätsprotokollstreaming kann entweder programmgesteuert oder über das Portal aktiviert werden. In beiden Fällen wählen Sie einen Service Bus-Namespace und eine SAS-Richtlinie für diesen Namespace aus. In diesem Namespace wird dann ein Event Hub erstellt, wenn das erste neue Aktivitätsprotokollereignis eintritt. Wenn Sie nicht über einen Service Bus-Namespace verfügen, müssen Sie diesen zunächst erstellen. Wenn Sie bereits Aktivitätsprotokollereignisse zu diesem Service Bus-Namespace übermittelt haben, wird der zuvor erstellte Event Hub wiederverwendet. Die SAS-Richtlinie definiert die Berechtigungen für den Streamingmechanismus. Für das Streaming an Event Hubs werden aktuell Berechtigungen für das **Verwalten**, **Senden** und **Lauschen** benötigt. Sie können freigegebene Zugriffsrichtlinien für Ihren Service Bus-Namespace im klassischen Portal auf der Registerkarte „Konfigurieren“ erstellen oder ändern. Wenn Sie dem Protokollprofil für das Aktivitätsprotokoll das Streamingfeature hinzufügen möchten, muss der Benutzer, der die Änderung vornimmt, in der Service Bus-Autorisierungsregel über die ListKey-Berechtigung verfügen.

Der Service Bus- oder Event Hub-Namespace muss sich nicht unter demselben Abonnement befinden, das Protokolle ausgibt, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements hat.

### <a name="via-azure-portal"></a>Verwenden des Azure-Portals
1. Navigieren Sie über das Menü auf der linken Seite des Portals zum Blatt **Aktivitätsprotokoll** .
   
    ![Navigation zum Aktivitätsprotokoll im Portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klicken Sie oben auf dem Blatt auf die Schaltfläche **Exportieren** .
   
    ![Schaltfläche „Exportieren“ im Portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Auf dem angezeigten Blatt können Sie die Regionen, für die Sie Ereignisse streamen möchten, und den Service Bus-Namespace auswählen, in dem eine Event Hub-Instanz zum Streamen dieser Ereignisse erstellt werden soll.
   
    ![Blatt zum Exportieren des Aktivitätsprotokolls](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern. Die Einstellungen werden sofort auf Ihr Abonnement angewendet.

### <a name="via-powershell-cmdlets"></a>Verwenden von PowerShell-Cmdlets
Falls bereits ein Protokollprofil vorhanden ist, müssen Sie dieses zuerst entfernen.

1. Ermitteln Sie mithilfe von `Get-AzureRmLogProfile` , ob ein Protokollprofil vorhanden ist.
2. Falls ja, entfernen Sie es mithilfe von `Remove-AzureRmLogProfile` .
3. Verwenden Sie `Set-AzureRmLogProfile`, um ein Profil zu erstellen:

```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: {Service Bus-Ressourcen-ID}/authorizationrules/{Schlüsselname}. Beispiel: 

### <a name="via-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Falls bereits ein Protokollprofil vorhanden ist, müssen Sie dieses zuerst entfernen.

1. Ermitteln Sie mithilfe von `azure insights logprofile list` , ob ein Protokollprofil vorhanden ist.
2. Falls ja, entfernen Sie es mithilfe von `azure insights logprofile delete` .
3. Verwenden Sie `azure insights logprofile add` , um ein Profil zu erstellen:

```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Nutzen der Protokolldaten aus Event Hubs
Das Schema für das Aktivitätsprotokoll finden Sie [hier](monitoring-overview-activity-logs.md). Jedes Ereignis ist ein Array mit JSON-Blobs, die als Datensätze bezeichnet werden.

## <a name="next-steps"></a>Nächste Schritte
* [Archivieren Sie das Aktivitätsprotokoll mithilfe eines Speicherkontos.](monitoring-archive-activity-log.md)
* [Lesen Sie die Übersicht über das Azure-Aktivitätsprotokoll.](monitoring-overview-activity-logs.md)
* [Richten Sie eine Warnung ein, die auf einem Aktivitätsprotokollereignis basiert.](insights-auditlog-to-webhook-email.md)




<!--HONumber=Dec16_HO2-->


