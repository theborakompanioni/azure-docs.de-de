---
title: "Protokollierung von Metriken und Diagnosen für Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Hier erhalten Sie Informationen zum Konfigurieren der Azure SQL-Datenbankressourcen zum Speichern von Statistiken für Ressourcenverbrauch, Konnektivität und Abfrageausführung."
services: sql-database
documentationcenter: 
author: vvasic
manager: jhubbard
editor: 
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: vvasic
ms.translationtype: Human Translation
ms.sourcegitcommit: 3bbc9e9a22d962a6ee20ead05f728a2b706aee19
ms.openlocfilehash: 71c286061e214898a7f10fa8dfd6c6d86af8db39
ms.contentlocale: de-de
ms.lasthandoff: 06/10/2017


---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Protokollierung von Metriken und Diagnosen für Azure SQL-Datenbank 
Azure SQL-Datenbank kann Metrik- und Diagnoseprotokolle ausgeben, um die Überwachung zu erleichtern. Sie können Azure SQL-Datenbank zum Speichern von Ressourcenverbrauch, Workern und Sitzungen sowie Verbindungen in einer der folgenden Azure-Ressourcen konfigurieren:
- **Azure Storage:** für die Archivierung großer Mengen von Telemetriedaten zu einem kleinen Preis
- **Azure Event Hub**: Für die Integration von Telemetriedaten von Azure SQL-Datenbank in Ihrer benutzerdefinierten Überwachungslösung oder Hotpipelines
- **Azure Log Analytics**: Für eine einsatzbereite Überwachungslösung mit Funktionen für Berichterstellung, Warnungen und Problemlösung 

    ![Architektur](./media/sql-database-metrics-diag-logging/architecture.png)

## <a name="enable-logging"></a>Aktivieren der Protokollierung

Die Protokollierung von Metriken und Diagnosen ist standardmäßig nicht aktiviert. Sie können die Protokollierung von Metriken und Diagnosen mit einer der folgenden Methoden aktivieren und verwalten:
- Azure-Portal
- PowerShell
- Azure-Befehlszeilenschnittstelle
- REST-API 
- Resource Manager-Vorlage

Wenn Sie die Protokollierung von Metriken und Diagnosen aktivieren, müssen Sie die Azure-Ressource angeben, in der die ausgewählten Daten erfasst werden. Verfügbare Optionen:
- Log Analytics
- Event Hub
- Azure Storage 

Sie können eine neue Azure-Ressource bereitstellen oder eine vorhandene Ressource auswählen. Nachdem Sie die Speicherressource ausgewählt haben, müssen Sie angeben, welche Daten erfasst werden sollen. Verfügbare Optionen:

- **[1-minute metrics](sql-database-metrics-diag-logging.md#1-minute-metrics)** (Minutenmetriken) – Enthält DTU-Prozentsatz, DTU-Limit, CPU-Prozentsatz, Prozentsatz der gelesenen physischen Daten, Prozentsatz für Protokollschreibvorgang, Verbindungen mit Status Erfolgreich/Fehlgeschlagen/Durch Firewall blockiert, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent, XTP-Speicher in Prozent

Wenn Sie Event Hub oder ein AzureStorage-Konto angeben, können Sie eine Aufbewahrungsrichtlinie einrichten, um anzugeben, dass Daten ab einem bestimmten ausgewählten Alter gelöscht werden sollen. Wenn Sie Log Analytics angeben, hängt die Aufbewahrungsrichtlinie vom ausgewählten Tarif ab. Weitere Informationen erhalten Sie unter [Log Analytics – Preise](https://azure.microsoft.com/pricing/details/log-analytics/). 

Es wird empfohlen, sowohl den Artikel [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) als auch den Artikel [Übersicht über Azure-Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) durchzulesen, um sich mit dem Aktivieren der Protokollierung vertraut zu machen und ein Verständnis der von den verschiedenen Azure-Diensten unterstützten Metriken und Protokollkategorien zu erlangen.

### <a name="azure-portal"></a>Azure-Portal

Um die Sammlung von Metrik- und Diagnoseprotokollen im Azure-Portal zu aktivieren, navigieren Sie zur Seite der Azure SQL-Datenbank oder des Pools für elastische Datenbanken, und klicken Sie auf **Diagnoseeinstellungen**.

   ![Aktivieren im Azure-Portal](./media/sql-database-metrics-diag-logging/enable-portal.png)

### <a name="powershell"></a>PowerShell

Verwenden Sie die folgenden Befehle, um die Metrik- und Diagnoseprotokollierung mit PowerShell zu aktivieren:

- Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   Die Speicherkonto-ID (StorageAccountId) ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

- Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format:

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- Sie können die Ressourcen-ID mit folgendem Befehl aus Ihrem Log Analytics-Arbeitsbereich abrufen:

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="cli"></a>Befehlszeilenschnittstelle (CLI)

Verwenden Sie die folgenden Befehle, um die Metrik- und Diagnoseprotokollierung mit der Azure-CLI zu aktivieren:

- Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   Die Speicherkonto-ID (StorageAccountId) ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

- Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format:

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="rest-api"></a>REST-API

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen mithilfe der Azure Monitor-REST-API ändern](https://msdn.microsoft.com/library/azure/dn931931.aspx). 

### <a name="resource-manager-template"></a>Resource Manager-Vorlage

Informieren Sie sich darüber, wie Sie [Diagnoseeinstellungen beim Erstellen von Ressourcen mithilfe der Resource Manager-Vorlage aktivieren](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md). 

## <a name="stream-into-log-analytics"></a>Streamen nach Log Analytics 
Metrik- und Diagnoseprotokolle für Azure SQL-Datenbank können mithilfe der integrierten Option „An Log Analytics senden“ im Portal in Log Analytics gestreamt werden, oder durch Aktivieren von Log Analytics in einer Diagnoseeinstellung über Azure PowerShell-Cmdlets, die Azure-CLI oder die Azure Monitor-REST-API.

### <a name="installation-overview"></a>Übersicht über die Installation

Das Überwachen der Azure SQL-Datenbankflotte mit Log Analytics ist einfach. Es sind drei Schritte erforderlich:

1.  Erstellen von Log Analytics-Ressourcen
2.  Konfigurieren von Datenbanken zum Aufzeichnen von Metrik- und Diagnoseprotokollen in der erstellten Log Analytics-Instanz
3.  Installieren Sie die **Azure SQL-Analyse**-Lösung aus dem Katalog in Log Analytics

### <a name="create-log-analytics-resource"></a>Erstellen von Log Analytics-Ressourcen

1. Klicken Sie im linken Menü auf **Neu**.
2. Klicken Sie auf **Überwachung und Verwaltung**.
3. Klicken Sie auf **Log Analytics**.
4. Füllen Sie das Log Analytics-Formular mit den erforderlichen zusätzlichen Informationen: Name des Arbeitsbereichs, Abonnement, Ressourcengruppe, Ort und Tarif.

   ![Log Analytics](./media/sql-database-metrics-diag-logging/log-analytics.png)

### <a name="configure-databases-to-record-metrics-and-diagnostic-logs"></a>Konfigurieren von Datenbanken zum Aufzeichnen von Metrik- und Diagnoseprotokollen

Das Azure-Portal stellt die einfachste Möglichkeit dar, das Verzeichnis zu konfigurieren, in dem Datenbanken ihre Metriken aufzeichnen. Navigieren Sie im Azure-Portal zu Ihrer Azure SQL-Datenbankressource, und klicken Sie auf **Diagnoseeinstellungen**. 

### <a name="install-the-azure-sql-analytics-solution-from-gallery"></a>Installieren der Azure SQL-Analyselösung aus dem Katalog  

1. Installieren Sie die Azure SQL-Analyse-Lösung, sobald die Log Analytics-Ressource erstellt wurde und die Daten in diese fließen. Dieser Vorgang kann über den **Lösungskatalog** ausgeführt werden. Dieser findet sich auf der OMS-Startseite und im seitlichen Menü. Suchen Sie im Katalog nach dem Eintrag **Azure SQL-Analyse**, klicken Sie darauf, und klicken Sie dann auf **Hinzufügen**.

   ![Überwachungslösung](./media/sql-database-metrics-diag-logging/monitoring-solution.png)

2. Auf der OMS-Startseite wird eine neue Kachel mit dem Namen **Azure SQL-Analyse** angezeigt. Wenn Sie diese Kachel auswählen, wird das Azure SQL-Analyse-Dashboard geöffnet.

### <a name="using-azure-sql-analytics-solution"></a>Verwenden der Azure SQL-Analyselösung

Azure SQL-Analyse stellt ein hierarchisches Dashboard dar, in dem Sie die Hierarchie der Azure SQL-Datenbankressourcen durchsuchen können. Durch diese Fähigkeit wird eine Überwachung auf hoher Ebene ermöglicht, gleichzeitig können Sie die Überwachung aber auch auf die wirklich relevanten Ressourcen beschränken.
Im Dashboard werden die Listen der verschiedenen Ressourcen unter der ausgewählten Ressource angezeigt. Für ein ausgewähltes Abonnement werden beispielsweise alle Server, Pools für elastische Datenbanken und Datenbanken angezeigt, die zu dem betreffenden Abonnement gehören. Für Pools für elastische Datenbanken und Datenbanken werden zudem die Metriken zum Ressourcenverbrauch für die betreffende Ressource angezeigt. Hierzu zählen Diagramme für DTU, CPU, E/A, LOG, Sitzungen, Worker, Verbindungen und Speicher in GB.

## <a name="stream-into-azure-event-hub"></a>Streamen nach Azure Event Hub

Metrik- und Diagnoseprotokolle für Azure SQL-Datenbank können mithilfe der integrierten Option „An einen Event Hub streamen“ im Portal in Event Hub gestreamt werden, oder durch Aktivieren der Service Bus-Regel-ID in einer Diagnoseeinstellung über Azure PowerShell-Cmdlets, die Azure-CLI oder die Azure Monitor-REST-API. 

### <a name="what-to-do-with-metrics-and-diagnostic-logs-in-event-hub"></a>Welche Vorgänge können mit Metrik- und Diagnoseprotokollen in Event Hub ausgeführt werden?
Wenn die ausgewählten Daten nach Event Hub gestreamt werden, sind Sie der Einrichtung fortgeschrittener Überwachungsszenarien ein ganzes Stück näher gekommen. Event Hubs fungiert als „Eingangstür“ für eine Ereignispipeline. Nach der Erfassung in Event Hubs können Sie Daten mit einem beliebigen Echtzeit-Analyseanbieter oder mit Batchverarbeitungs-/Speicheradaptern umwandeln und speichern. Event Hubs entkoppelt die Erzeugung eines Datenstroms von Ereignissen von der Nutzung dieser Ereignisse, sodass  Ereignisconsumer nach einem eigenen Zeitplan auf Ereignisse zugreifen können. Weitere Informationen zu Event Hub finden Sie unter:

- [Was sind Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
- [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


Im Anschluss finden Sie eine kleine Auswahl von Verwendungsmöglichkeiten für das Streamen:

-   Anzeigen der Dienstintegrität durch Streamen von Daten zum langsamsten Pfad an PowerBI: Mithilfe von Event Hubs, Stream Analytics und PowerBI können Sie sich anhand Ihrer Metrik- und Diagnosedaten problemlos und nahezu in Echtzeit einen Einblick in Ihre Azure-Dienste verschaffen. [Stream Analytics und Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md) bietet eine gute Übersicht über die Einrichtung von Event Hubs, die Verarbeitung von Daten mit Stream Analytics und die Verwendung von PowerBI als Ausgabe.
-   Streamen von Protokollen in Protokollierungs- und Telemetrie-Streams von Drittanbietern: Mithilfe des Event Hubs-Streamings können Sie Ihre Metrik- und Diagnoseprotokolle in verschiedene Überwachungs- und Protokollanalyselösungen von Drittanbietern übertragen. 
-   Erstellen einer benutzerdefinierten Telemetrie- und Protokollierungsplattform: Event Hubs ermöglicht dank des hochgradig skalierbaren Veröffentlichen/Abonnieren-Konzepts eine flexible Erfassung von Diagnoseprotokollen. Dies ist interessant, wenn Sie bereits über eine benutzerdefinierte Telemetrieplattform verfügen oder eine solche Plattform erstellen möchten. Informationen zur Verwendung von Event Hubs für eine globale Telemetrieplattform finden Sie in der [Anleitung von Dan Rosanova](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="stream-into-azure-storage"></a>Streamen nach Azure Storage

Azure SQL-Datenbank-Metrik- und Diagnoseprotokolle können mithilfe der integrierten Option „In ein Speicherkonto archivieren“ im Azure-Portal in Azure Storage gespeichert werden, oder durch Aktivieren von Azure Storage in einer Diagnoseeinstellung über Azure PowerShell-Cmdlets, die Azure-CLI oder die Azure Monitor-REST-API.

### <a name="schema-of-metrics-and-diagnostic-logs-in-the-storage-account"></a>Schema der Metrik- und Diagnoseprotokolle im Speicherkonto

Wenn Sie die Sammlung von Metrik- und Diagnoseprotokollen eingerichtet haben, wird ein Speichercontainer im ausgewählten Speicherkonto erstellt, wenn die ersten Datenzeilen verfügbar sind. Die Struktur dieser Blobs sieht wie folgt aus:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
Einfacher ausgedrückt:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

Ein Blob-Name für Minutenmetriken kann beispielsweise wie folgt lauten:

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

Wenn Sie die Daten aus dem Pool für elastische Datenbanken aufzeichnen möchten, weicht der Blob-Name etwas ab:

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-azure-storage"></a>Herunterladen von Metriken und Protokollen aus Azure Storage

Siehe [Herunterladen von Metrik- und Diagnoseprotokollen aus Azure Storage](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)

## <a name="1-minute-metrics"></a>Minutenmetriken

| |  |
|---|---|
|**Ressource**|**Metriken**|
|Datenbank|DTU-Prozentsatz, DTU-Verwendung, DTU-Limit, CPU-Prozentsatz, Prozentsatz der gelesen physischen Daten, Prozentsatz für Protokollschreibvorgang, Verbindungen mit Status Erfolgreich/Fehlgeschlagen/Durch Firewall blockiert, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent, XTP-Speicher in Prozent, Deadlocks |
|Elastischer Pool|eDTU-Prozentsatz, eDTU-Verwendung, eDTU-Limit, CPU-Prozentsatz, Prozentsatz der gelesen physischen Daten, Prozentsatz für Protokollschreibvorgang, Sitzungen in Prozent, Worker in Prozent, Speicher, Speicher in Prozent, Speicherbegrenzung, XTP-Speicher in Prozent |
|||

## <a name="next-steps"></a>Nächste Schritte

- Lesen Sie sowohl den Artikel [Überblick über Metriken in Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md) als auch den Artikel [Übersicht über Azure-Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) durch, um sich mit dem Aktivieren der Protokollierung vertraut zu machen und ein Verständnis der von den verschiedenen Azure-Diensten unterstützten Metriken und Protokollkategorien zu erlangen.
- Lesen Sie diese Artikel durch, um sich über Event Hubs zu informieren:
   - [Was sind Azure Event Hubs?](../event-hubs/event-hubs-what-is-event-hubs.md)
   - [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)
- Siehe [Herunterladen von Metrik- und Diagnoseprotokollen aus Azure Storage](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)

