---
title: "Übersicht über das Azure-Aktivitätsprotokoll | Microsoft Docs"
description: "Es wird beschrieben, was das Azure-Aktivitätsprotokoll ist und wie Sie es nutzen können, um Informationen zu den Ereignissen in Ihrem Azure-Abonnement zu erhalten."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c274782f-039d-4c28-9ddb-f89ce21052c7
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 8ff9f73fc0732cd2227b7e0cc1091e04d69014eb
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="monitor-subscription-activity-with-the-azure-activity-log"></a>Überwachen der Abonnementaktivität per Azure-Aktivitätsprotokoll
Das **Azure-Aktivitätsprotokoll** ist ein Abonnementprotokoll, das Einblicke in Ereignisse auf Abonnementebene ermöglicht, die in Azure aufgetreten sind. Dies schließt einen Datenbereich von Azure Resource Manager-Betriebsdaten bis hin zu Aktualisierungen für Dienstintegritätsereignisse ein. Das Aktivitätsprotokoll wurde bisher als „Überwachungsprotokolle“ oder „Vorgangsprotokolle“ bezeichnet, da die Verwaltungskategorie Ereignisse der Steuerungsebene für Ihre Abonnements enthält. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für die Ressourcen Ihres Abonnements durchgeführt wurden. Sie können auch den Status des Vorgangs und andere relevante Eigenschaften verstehen. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET) oder Vorgänge für Ressourcen, die das klassische Modell/RDFE-Modell verwenden.

![Aktivitätsprotokoll im Vergleich zu anderen Protokolltypen ](./media/monitoring-overview-activity-logs/Activity_Log_vs_other_logs_v5.png)

Abbildung 1: Aktivitätsprotokoll im Vergleich zu anderen Protokolltypen

Das Aktivitätsprotokoll unterscheidet sich von [Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md). Aktivitätsprotokolle enthalten Daten zu den Vorgängen an einer Ressource von außen („Steuerungsebene“). Diagnoseprotokolle werden von einer Ressource ausgegeben und enthalten Informationen zu den Vorgängen dieser Ressource („Datenebene“).

Sie können Ereignisse per Azure-Portal, Befehlszeilenschnittstelle, PowerShell-Cmdlets und Azure Monitor-REST-API aus dem Aktivitätsprotokoll abrufen.


> [!WARNING]
> Das Azure-Aktivitätsprotokoll ist in erster Linie für Aktivitäten bestimmt, die in Azure Resource Manager stattfinden. Es verfolgt keine Ressourcen nach, die das klassische Modell/RDFE-Modell verwenden. Einige klassische Ressourcentypen weisen einen Proxyressourcenanbieter in Azure Resource Manager auf (z.B. Microsoft.ClassicCompute). Wenn Sie mithilfe dieser Proxyressourcenanbieter über Azure Resource Manager mit einem klassischen Ressourcentyp interagieren, werden die Vorgänge im Aktivitätsprotokoll aufgeführt. Wenn Sie mit einem klassischen Ressourcentyp im klassischen Portal oder an anderer Stelle außerhalb der Azure Resource Manager-Proxys interagieren, werden Ihre Aktionen nur in das Vorgangsprotokoll aufgenommen. Auf das Vorgangsprotokoll kann nur im klassischen Portal zugegriffen werden.
>
>

Sehen Sie sich dieses Video zum Aktivitätsprotokoll an.
> [!VIDEO https://channel9.msdn.com/Blogs/Seth-Juarez/Logs-John-Kemnetz/player]
> 
>

## <a name="categories-in-the-activity-log"></a>Kategorien im Aktivitätsprotokoll
Das Aktivitätsprotokoll enthält verschiedene Kategorien von Daten. Umfassende Informationen zu den Schemas dieser Kategorien finden Sie in [diesem Artikel](monitoring-activity-log-schema.md). Diese umfassen:
* **Administration**: Diese Kategorie enthält die Datensätze aller Erstellungs-, Aktualisierungs-, Lösch- und Aktionsvorgänge, die über Resource Manager ausgeführt wurden. Zu den Ereignissen in dieser Kategorie gehören das Erstellen eines virtuellen Computers und das Löschen einer Netzwerksicherheitsgruppe. Jede Aktion, die von einem Benutzer oder einer Anwendung mithilfe von Resource Manager ausgeführt wird, wird als Vorgang für einen bestimmten Ressourcentyp modelliert. Wenn der Vorgangstyp „Schreiben“, „Löschen“ oder „Aktion“ ist, werden die Datensätze zum Start und zum Erfolg oder Fehler dieses Vorgangs in der Kategorie „Administration“ aufgezeichnet. Die Kategorie „Administration“ enthält außerdem alle Änderungen an der rollenbasierten Zugriffssteuerung in einem Abonnement.
* **Dienstintegrität**: Diese Kategorie enthält Datensätze zu allen Incidents im Zusammenhang mit der Dienstintegrität, die in Azure aufgetreten sind. Ein Beispiel für ein Ereignis in dieser Kategorie ist „Ausfallzeiten bei SQL Azure in der Region ‚USA, Osten‘“. Für Ereignisse zur Dienstintegrität gibt es fünf Varianten: Aktion erforderlich, unterstützte Wiederherstellung, Incident, Wartung, Information oder Sicherheit. Sie werden nur angezeigt, wenn eine Ressource in Ihrem Abonnement von dem Ereignis betroffen wäre.
* **Warnung**: Diese Kategorie enthält die Datensätze zu allen Aktivierungen von Azure-Warnungen. Ein Beispiel für ein Ereignis in dieser Kategorie ist „CPU-Auslastung auf ‚myVM‘ liegt in den letzten 5 Minuten über 80“. Eine Vielzahl von Azure-Systemen weist ein Konzept für Warnungen auf: Sie können eine Regel definieren und erhalten eine Benachrichtigung, wenn die Bedingungen mit der Regel übereinstimmen. Jedes Mal, wenn ein unterstützter Azure-Warnungstyp „aktiviert“ wird oder die Bedingungen erfüllt sind, sodass eine Benachrichtigung generiert wird, wird ein Datensatz der Aktivierung auch in dieser Kategorie des Aktivitätsprotokolls abgelegt.
* **Autoskalierung**: Diese Kategorie enthält Datensätze von Ereignissen im Zusammenhang mit automatischen Skalierungsvorgängen basierend auf den Einstellungen für die automatische Skalierung, die Sie in Ihrem Abonnement definiert haben. Ein Beispiel für Ereignisse in dieser Kategorie ist „Fehler beim automatischen zentralen Hochskalieren“. Mit der automatischen Skalierung können Sie die Anzahl der Instanzen eines unterstützten Ressourcentyps basierend auf der Tageszeit und/oder Lastdaten (Metrik) mithilfe einer Einstellung für die automatische Skalierung automatisch horizontal hoch- oder herunterskalieren. Wenn die Bedingungen zum zentralen Hoch- oder Herunterskalieren erfüllt sind, werden Ereignisse zum Start und zum Erfolg bzw. Fehler in dieser Kategorie aufgezeichnet.
* **Empfehlung**: Diese Kategorie enthält Empfehlungsereignisse von bestimmten Ressourcentypen, beispielsweise von Websites und SQL-Servern. Diese Ereignisse bieten Empfehlungen, wie Sie Ihre Ressourcen besser nutzen können. Sie erhalten Ereignisse dieses Typs nur dann, wenn Sie über Ressourcen verfügen, die Empfehlungen ausgeben.
* **Richtlinie, Sicherheit und Ressourcenintegrität**: Diese Kategorien enthalten keine Ereignisse; sie sind für die künftige Verwendung reserviert.

## <a name="event-schema-per-category"></a>Ereignisschema nach Kategorie
[Lesen Sie diesen Artikel, um sich über die Grundlagen des Aktivitätsprotokoll-Ereignisschemas nach Kategorie zu informieren.](monitoring-activity-log-schema.md)

## <a name="what-you-can-do-with-the-activity-log"></a>Möglichkeiten mit dem Aktivitätsprotokoll
Hier sind einige Verwendungsmöglichkeiten für das Aktivitätsprotokoll aufgeführt:

![Azure-Aktivitätsprotokoll](./media/monitoring-overview-activity-logs/Activity_Log_Overview_v3.png)


* Abfragen und Anzeigen des Protokolls im **Azure-Portal**
* [Erstellen einer Warnung zu einem Aktivitätsprotokollereignis](monitoring-activity-log-alerts.md)
* Streamen zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI [an einen **Event Hub**](monitoring-stream-activity-logs-event-hubs.md).
* Analysieren in Power BI mit dem [**Power BI-Inhaltspaket**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)
* [Speichern unter einem **Speicherkonto** zur Archivierung oder manuellen Untersuchung](monitoring-archive-activity-log.md) Sie können die Aufbewahrungsdauer (in Tagen) mithilfe des **Protokollprofils** angeben.
* Fragen Sie es per PowerShell-Cmdlet, CLI oder REST-API ab.

## <a name="query-the-activity-log-in-the-azure-portal"></a>Abfragen des Aktivitätsprotokolls im Azure-Portal
Im Azure-Portal können Sie Ihr Aktivitätsprotokoll an mehreren Stellen anzeigen:
* Auf dem **Blatt „Aktivitätsprotokoll“**, auf das Sie zugreifen können, indem Sie im Navigationsbereich auf der linken Seite unter „Weitere Dienste“ nach dem Aktivitätsprotokoll suchen.
* Auf dem **Blatt „Überwachen“**, das standardmäßig im Navigationsbereich auf der linken Seite angezeigt wird. Das Aktivitätsprotokoll ist ein Abschnitt dieses Azure Monitor-Blatts.
* Auf jedem **Ressourcenblatt** einer Ressource, z.B. dem Konfigurationsblatt eines virtuellen Computers. Das Aktivitätsprotokoll ist einer der Abschnitte auf den meisten dieser Ressourcenblätter. Wenn Sie darauf klicken, werden die Ereignisse automatisch so gefiltert, dass nur die Ereignisse für die jeweilige Ressource angezeigt werden.

Im Azure-Portal können Sie Ihr Aktivitätsprotokoll nach diesen Feldern filtern:
* Zeitraum: Die Start- und Endzeit für die Ereignisse.
* Kategorie: Die oben beschriebene Ereigniskategorie.
* Abonnement: Name von mindestens einem Azure-Abonnement.
* Ressourcengruppe: Mindestens eine Ressourcengruppe in diesen Abonnements.
* Ressource (Name): Der Name einer bestimmten Ressource.
* Ressourcentyp: Der Typ der Ressource, z.B. „Microsoft.Compute/virtualmachines“.
* Vorgangsname: Der Name eines Azure Resource Manager-Vorgangs, z.B. „Microsoft.SQL/servers/Write“.
* Schweregrad: Der Schweregrad des Ereignisses (Information, Warnung, Fehler, Kritisch).
* Ereignis initiiert von: Der „Aufrufer“ oder Benutzer, der den Vorgang durchgeführt hat.
* Open search (Freie Suche): Dies ist ein Feld für eine offene Textsuche, bei der in allen Feldern aller Ereignisse nach der Zeichenfolge gesucht wird.

Nachdem Sie eine Gruppe mit Filtern definiert haben, können Sie diese als Abfrage speichern. Sie wird für den Fall beibehalten, dass Sie die gleiche Abfrage mit diesen Filtern zu einem späteren Zeitpunkt noch einmal durchführen müssen. Sie haben auch die Möglichkeit, eine Abfrage in Ihrem Azure-Dashboard anzuheften, um immer den Überblick über bestimmte Ereignisse zu haben.

Wenn Sie auf „Übernehmen“ klicken, wird Ihre Abfrage ausgeführt, und es werden alle übereinstimmenden Ereignisse angezeigt. Wenn Sie in der Liste auf ein Ereignis klicken, werden eine Zusammenfassung dieser Ereignisse und der vollständige unformatierte JSON-Code des Ereignisses angezeigt.

Sie haben noch mehr Optionen, wenn Sie auf das Symbol **Protokollsuche** klicken, über das Ihre Aktivitätsprotokolldaten in der [Log Analytics-Lösung für Aktivitätsprotokolle](../log-analytics/log-analytics-activity.md) angezeigt werden. Das Blatt „Aktivitätsprotokoll“ enthält eine einfache Filter-/Suchoberfläche für Protokolle, aber mit Log Analytics haben Sie effektivere Möglichkeiten, Ihre Daten zu pivotieren, abzufragen und zu visualisieren.

## <a name="export-the-activity-log-with-a-log-profile"></a>Exportieren des Aktivitätsprotokolls mit einem Protokollprofil
Mit einem **Protokollprofil** wird gesteuert, wie das Aktivitätsprotokoll exportiert wird. Mit einem Protokollprofil können Sie Folgendes konfigurieren:

* Wohin das Aktivitätsprotokoll gesendet wird (Speicherkonto oder Event Hubs)
* Welche Ereigniskategorien gesendet werden sollen („Write“, „Delete“, „Action“) *„Kategorie“ hat in Protokollprofilen und Aktivitätsprotokollen eine unterschiedliche Bedeutung. Im Protokollprofil ist „Kategorie“ der Vorgangstyp („Write“, „Delete“, „Action“). In einem Aktivitätsprotokoll stellt die Eigenschaft „Kategorie“ die Quelle oder den Typ des Ereignisses dar (z.B. „Administration“, „ServiceHealth“, „Alert“ usw.).*
* Welche Regionen (Standorte) exportiert werden sollen. Stellen Sie sicher, dass Sie „global“ einbeziehen, da viele Ereignisse im Aktivitätsprotokoll globale Ereignisse sind.
* Wie lange das Aktivitätsprotokoll in einem Speicherkonto beibehalten werden soll.
    - Wenn für die Beibehaltungsdauer 0 Tage festgelegt sind, bedeutet dies, dass Protokolle unbegrenzt beibehalten werden. Andernfalls kann als Wert die Anzahl von Tagen (1 bis 2.147.483.647) festgelegt werden.
    - Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (etwa, wenn nur die Event Hubs- oder die OMS-Option aktiviert ist), werden die Aufbewahrungsrichtlinien ignoriert.
    - Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht.

Sie können ein Speicherkonto oder Event Hub-Namespace verwenden, das sich nicht im gleichen Abonnement befindet wie das, das Protokolle angibt. Der Benutzer, der die Einstellung konfiguriert, benötigt den entsprechenden RBAC-Zugriff auf beide Abonnements.

Diese Einstellungen können über die Option „Exportieren“ auf dem Blatt „Aktivitätsprotokoll“ im Portal konfiguriert werden. Sie können auch [mithilfe der Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931927.aspx), über PowerShell-Cmdlets oder über die Befehlszeilenschnittstelle programmgesteuert konfiguriert werden. Ein Abonnement kann nur über ein Protokollprofil verfügen.

### <a name="configure-log-profiles-using-the-azure-portal"></a>Konfigurieren von Protokollprofilen mit dem Azure-Portal
Sie können das Aktivitätsprotokoll an einen Event Hub streamen oder in einem Speicherkonto speichern, indem Sie im Azure-Portal die Option „Exportieren“ verwenden.

1. Navigieren Sie zum Blatt **Aktivitätsprotokoll** , indem Sie das Menü auf der linken Seite des Portals verwenden.

    ![Navigation zum Aktivitätsprotokoll im Portal](./media/monitoring-overview-activity-logs/activity-logs-portal-navigate.png)
2. Klicken Sie oben auf dem Blatt auf die Schaltfläche **Exportieren** .

    ![Schaltfläche „Exportieren“ im Portal](./media/monitoring-overview-activity-logs/activity-logs-portal-export.png)
3. Auf dem daraufhin angezeigten Blatt können Sie Folgendes auswählen:  
  * Regionen, die für die Ereignisse exportiert werden sollen
  * das Speicherkonto, in dem Sie Ereignisse speichern möchten
  * die Anzahl der Tage, die diese Ereignisse im Speicher aufbewahrt werden sollen. Bei einer Einstellung von 0 Tagen werden die Protokolle unbegrenzt aufbewahrt.
  * den Service Bus-Namespace, in dem Sie einen Event Hub für das Streamen dieser Ereignisse erstellen möchten

     ![Blatt zum Exportieren des Aktivitätsprotokolls](./media/monitoring-overview-activity-logs/activity-logs-portal-export-blade.png)
4. Klicken Sie auf **Speichern** , um die Einstellungen zu speichern. Die Einstellungen werden sofort auf Ihr Abonnement angewendet.

### <a name="configure-log-profiles-using-the-azure-powershell-cmdlets"></a>Konfigurieren von Protokollprofilen mit den Azure PowerShell-Cmdlets
#### <a name="get-existing-log-profile"></a>Abrufen eines vorhandenen Protokollprofils
```
Get-AzureRmLogProfile
```

#### <a name="add-a-log-profile"></a>Hinzufügen eines Protokollprofils
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action
```

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Name |Ja |Name des Protokollprofils. |
| StorageAccountId |Nein |Ressourcen-ID des Speicherkontos, in dem das Aktivitätsprotokoll gespeichert werden soll. |
| serviceBusRuleId |Nein |Service Bus-Regel-ID für den Service Bus-Namespace, unter dem Event Hubs erstellt werden sollen. Dies ist eine Zeichenfolge mit dem folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`. |
| Standorte |Ja |Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. |
| RetentionInDays |Ja |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| Categories |Nein |Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

#### <a name="remove-a-log-profile"></a>Entfernen eines Protokollprofils
```
Remove-AzureRmLogProfile -name my_log_profile
```

### <a name="configure-log-profiles-using-the-azure-cross-platform-cli"></a>Konfigurieren von Protokollprofilen mit der plattformübergreifenden Azure-Befehlszeilenschnittstelle
#### <a name="get-existing-log-profile"></a>Abrufen eines vorhandenen Protokollprofils
```
azure insights logprofile list
```
```
azure insights logprofile get --name my_log_profile
```
Die `name` -Eigenschaft sollte den Namen des Protokollprofils enthalten.

#### <a name="add-a-log-profile"></a>Hinzufügen eines Protokollprofils
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Name |Ja |Name des Protokollprofils. |
| storageId |Nein |Ressourcen-ID des Speicherkontos, in dem das Aktivitätsprotokoll gespeichert werden soll. |
| serviceBusRuleId |Nein |Service Bus-Regel-ID für den Service Bus-Namespace, unter dem Event Hubs erstellt werden sollen. Dies ist eine Zeichenfolge mit dem folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`. |
| locations |Ja |Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. |
| RetentionInDays |Ja |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| categories |Nein |Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

#### <a name="remove-a-log-profile"></a>Entfernen eines Protokollprofils
```
azure insights logprofile delete --name my_log_profile
```

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zum Aktivitätsprotokoll (bisher „Überwachungsprotokolle“)](../azure-resource-manager/resource-group-audit.md)
* [Stream the Azure Activity Log to Event Hubs (Streamen des Azure-Aktivitätsprotokolls auf Event Hubs)](monitoring-stream-activity-logs-event-hubs.md)

