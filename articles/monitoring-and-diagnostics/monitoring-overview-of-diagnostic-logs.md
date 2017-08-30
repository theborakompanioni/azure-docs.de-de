---
title: "Übersicht über Azure-Diagnoseprotokolle | Microsoft Docs"
description: Hier erfahren Sie, worum es sich bei Azure-Diagnoseprotokollen handelt, und wie Sie mithilfe von Diagnoseprotokollen Ereignisse innerhalb einer Azure-Ressource nachvollziehen.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem; magoedte
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: d59abde29fc7b73a799e5bf3659b02f824b693de
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen

## <a name="what-are-azure-resource-diagnostic-logs"></a>Was sind Diagnoseprotokolle auf Azure-Ressourcenebene?
**Diagnoseprotokolle auf Azure-Ressourcenebene** sind von einer Ressource ausgegebene Protokolle mit umfangreichen, in kurzen Abständen erfassten Betriebsdaten der Ressource. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp. Beispielweise sind Netzwerksicherheitsgruppen-Regelzähler und Key Vault-Überwachungen zwei Kategorien von Ressourcenprotokollen.

Diagnoseprotokolle auf Ressourcenebene unterscheiden sich vom [Aktivitätsprotokoll](monitoring-overview-activity-logs.md). Das Aktivitätsprotokoll ermöglicht Einblicke in die Vorgänge, die für Ressourcen Ihres Abonnements mit dem Resource Manager durchgeführt wurden, z.B. das Erstellen eines virtuellen Computers oder das Löschen einer Logik-App. Das Aktivitätsprotokoll ist ein Protokoll auf Abonnementebene. Diagnoseprotokolle auf Ressourcenebene ermöglichen Einblicke in Vorgänge, die für die Ressource selbst durchgeführt wurden, z.B. das Abrufen eines Geheimnisses aus einem Key Vault.

Diagnoseprotokolle auf Ressourcenebene unterscheiden sich auch von Diagnoseprotokollen auf Gastbetriebssystemebene. Diagnoseprotokolle auf Gastbetriebssystemebene werden von einem Agent erfasst, der auf einem virtuellen Computer oder einem anderen Ressourcentyp ausgeführt wird. Für Diagnoseprotokolle auf Ressourcenebene ist kein Agent erforderlich, und ressourcenspezifische Daten werden von der Azure-Plattform selbst erfasst. Für Diagnoseprotokolle auf Gastbetriebssystemebene werden Daten dagegen aus dem Betriebssystem und den Anwendungen erfasst, die auf einem virtuellen Computer ausgeführt werden.

Die hier beschriebene neue Art von Diagnoseprotokollen für Ressourcen wird nicht von allen Ressourcen unterstützt. Dieser Artikel enthält einen Abschnitt, in dem aufgeführt ist, welche Ressourcentypen die neuen Diagnoseprotokolle der Ressourcenebene unterstützen.

![Ressourcendiagnoseprotokolle im Vergleich zu anderen Protokolltypen ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a>Möglichkeiten mit Diagnoseprotokollen auf Ressourcenebene
Hier sind einige Verwendungsmöglichkeiten für Diagnoseprotokolle auf Ressourcenebene aufgeführt:

![Logische Anordnung von Diagnoseprotokollen für Ressourcen](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem [**Speicherkonto**](monitoring-archive-diagnostic-logs.md) gespeichert werden. Mithilfe der **Diagnoseeinstellungen für Ressourcen** können Sie eine Aufbewahrungsdauer (in Tagen) angeben.
* [Sie können Diagnoseprotokolle zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md) streamen.
* Diagnoseprotokolle können mit [OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md)

Sie können ein Speicherkonto oder Event Hubs-Namespace verwenden, das sich nicht im gleichen Abonnement befindet wie das, das Protokolle angibt. Der Benutzer, der die Einstellung konfiguriert, benötigt den entsprechenden RBAC-Zugriff auf beide Abonnements.

## <a name="resource-diagnostic-settings"></a>Diagnoseeinstellungen für Ressourcen
Ressourcendiagnoseprotokolle für computefremde Ressourcen werden mithilfe von Diagnoseeinstellungen für Ressourcen konfiguriert. Mit **Diagnoseeinstellungen für Ressourcen** für ein Ressourcensteuerelement können Sie Folgendes festlegen:

* Wohin Ressourcendiagnoseprotokolle und Metriken gesendet werden sollen (Speicherkonto, Event Hubs und/oder OMS Log Analytics).
* Welche Protokollkategorien gesendet werden, und ob auch Metrikdaten gesendet werden.
* Wie lange die einzelnen Protokollkategorien in einem Speicherkonto beibehalten werden sollen
    - Wenn für die Beibehaltungsdauer 0 Tage festgelegt sind, bedeutet dies, dass Protokolle unbegrenzt beibehalten werden. Andernfalls kann als Wert die Anzahl von Tagen (1 bis 2.147.483.647) festgelegt werden.
    - Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (etwa, wenn nur die Event Hubs- oder die OMS-Option aktiviert ist), werden die Aufbewahrungsrichtlinien ignoriert.
    - Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht.

Diese Einstellungen können für eine Ressource ganz einfach über die Diagnoseeinstellungen im Azure-Portal, mithilfe von Azure PowerShell oder CLI-Befehlen oder über die [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx) konfiguriert werden.

> [!WARNING]
> Bei Diagnoseprotokollen und Metriken für Computeressourcen aus der Gastbetriebssystem-Schicht (etwa virtuelle Computer oder Service Fabric) wird [ein separater Mechanismus für die Konfiguration und Auswahl von Ausgaben](../azure-diagnostics.md) verwendet.
>
>

## <a name="how-to-enable-collection-of-resource-diagnostic-logs"></a>Gewusst wie: Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen
Die Erfassung von Diagnoseprotokollen für Ressourcen kann [im Zuge der Ressourcenerstellung in einer Resource Manager-Vorlage](./monitoring-enable-diagnostic-logs-using-template.md) oder später im Portal über die Seite der Ressource aktiviert werden. Darüber hinaus können Sie die Erfassung jederzeit mithilfe von Azure PowerShell oder CLI-Befehlen oder mithilfe der Azure Monitor-REST-API aktivieren.

> [!TIP]
> Diese Anweisungen lassen sich unter Umständen nicht immer direkt auf jede Ressource anwenden. Informationen zu Sonderschritten, die ggf. für bestimmte Ressourcentypen erforderlich sind, finden Sie unter den Schemalinks am Ende dieser Seite.
>
>

### <a name="enable-collection-of-resource-diagnostic-logs-in-the-portal"></a>Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen im Portal
Sie können die Erfassung von Diagnoseprotokollen für Ressourcen nach dem Erstellen einer Ressource im Azure-Portal aktivieren, indem Sie entweder zu einer bestimmten Ressource oder zu Azure Monitor navigieren. So aktivieren Sie dies über Azure Monitor:

1. Navigieren Sie im [Azure-Portal](http://portal.azure.com) zu Azure Monitor, und klicken Sie auf **Diagnoseeinstellungen**.

    ![Abschnitt „Überwachung“ von Azure Monitor](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

2. Filtern Sie optional die Liste nach Ressourcengruppe oder Ressourcentyp, und klicken Sie auf die Ressource, für die Sie eine Diagnoseeinstellung festlegen möchten.

3. Wenn keine Einstellungen für die Ressource vorhanden sind, die Sie ausgewählt haben, werden Sie aufgefordert, eine Einstellung zu erstellen. Klicken Sie auf „Diagnose aktivieren“.

   ![Diagnoseeinstellung hinzufügen – keine Einstellungen vorhanden](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-none.png)

   Wenn Einstellungen für die Ressource vorhanden sind, sehen Sie eine Liste der Einstellungen, die bereits für diese Ressource konfiguriert sind. Klicken Sie auf „Diagnoseeinstellung hinzufügen“.

   ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-multiple.png)

3. Geben Sie Ihrer Einstellung einen Namen, aktivieren Sie die Kontrollkästchen für die einzelnen Ziele, an die Sie Daten senden möchten, und konfigurieren Sie, welche Ressourcen für die einzelnen Ziele verwendet werden. Legen Sie optional eine Anzahl von Tagen für die Aufbewahrung dieser Protokolle fest. Verwenden Sie dazu die Schieberegler unter **Aufbewahrung (Tage)** (gilt nur für das Speicherkontoziel). Bei einer Aufbewahrung von 0 Tagen werden die Protokolle dauerhaft gespeichert.
   
   ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-configure.png)
    
4. Klicken Sie auf **Speichern**.

Nach einigen Augenblicken wird die neue Einstellung in der Liste der Einstellungen für diese Ressource angezeigt, und Diagnoseprotokolle werden an die angegebenen Ziele gesendet, sobald neue Ereignisdaten generiert werden.

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen mit PowerShell
Verwenden Sie die folgenden Befehle, um die Erfassung von Diagnoseprotokollen für Ressourcen mit Azure PowerShell zu aktivieren:

Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

Die Speicherkonto-ID ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

Die Service Bus-Regel-ID ist eine Zeichenfolge mit dem folgenden Format: `{Service Bus resource ID}/authorizationrules/{key name}`.

Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

Sie können die Ressourcen-ID mit dem folgenden Befehl aus Ihrem Log Analytics-Arbeitsbereich abrufen:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="enable-collection-of-resource-diagnostic-logs-via-cli"></a>Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen per CLI
Mit den folgenden Befehlen können Sie Diagnoseprotokolle für Ressourcen über die Azure-Befehlszeilenschnittstelle aktivieren:

Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

Die Speicherkonto-ID ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Die Service Bus-Regel-ID ist eine Zeichenfolge mit dem folgenden Format: `{Service Bus resource ID}/authorizationrules/{key name}`.

Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen per REST-API
Informationen zum Ändern der Diagnoseeinstellungen mithilfe der Azure Monitor-REST-API finden Sie in [diesem Dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>Verwalten von Diagnoseeinstellungen für Ressourcen im Portal
Stellen Sie sicher, dass für alle Ihre Ressourcen Diagnoseeinstellungen festgelegt sind. Navigieren Sie im Portal zu **Überwachen**, und öffnen Sie **Diagnoseeinstellungen**.

![Das Blatt „Diagnoseprotokolle“ im Portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-nav.png)

Möglicherweise müssen Sie auf „Weitere Dienste“ klicken, um den Abschnitt „Überwachung“ zu finden.

Hier können Sie alle Ressourcen anzeigen und filtern, die Diagnoseeinstellungen unterstützen, um zu ermitteln, ob die Diagnose dafür aktiviert ist. Sie können auch einen Drilldown ausführen, um zu sehen, ob mehrere Einstellungen für eine Ressource festgelegt sind, und überprüfen, zu welchem Speicherkonto, Event Hubs-Namespace und/oder Log Analytics-Arbeitsbereich die Daten fließen.

![Ergebnisse von Diagnoseprotokollen im Portal](./media/monitoring-overview-of-diagnostic-logs/diagnostic-settings-blade.png)

Wenn Sie eine Diagnoseeinstellung hinzufügen, wird das Blatt „Diagnoseeinstellungen“ angezeigt, auf dem Sie die Diagnoseeinstellungen für die ausgewählte Ressource aktivieren, deaktivieren oder ändern können.

## <a name="supported-services-categories-and-schemas-for-resource-diagnostic-logs"></a>Unterstützte Dienste, Kategorien und Schemas für Ressourcendiagnoseprotokolle
[In diesem Artikel](monitoring-diagnostic-logs-schema.md) finden Sie eine vollständige Liste der unterstützten Dienste, Protokollkategorien und Schemas, die von diesen Diensten verwendet werden.

## <a name="next-steps"></a>Nächste Schritte

* [Streamen von Diagnoseprotokollen für Ressourcen an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* Ändern der Diagnoseeinstellungen für Ressourcen mithilfe der Azure Monitor-REST-API ([Service Diagnostic Settings](https://msdn.microsoft.com/library/azure/dn931931.aspx) (Diagnoseeinstellungen für Dienste))
* [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)

