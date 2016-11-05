---
title: Azure Key Vault-Lösung in Log Analytics | Microsoft Docs
description: Sie können die Azure Key Vault-Lösung in Log Analytics verwenden, um Azure Key Vault-Protokolle zu überprüfen.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''

ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2016
ms.author: richrund

---
# <a name="azure-key-vault-(preview)-solution-in-log-analytics"></a>Azure Key Vault-Lösung (Vorschau) in Log Analytics
> [!NOTE]
> Dies ist eine [Vorschaulösung](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).
> 
> 

Sie können die Azure Key Vault-Lösung in Log Analytics verwenden, um AuditEvent-Protokolle von Azure Key Vault zu überprüfen.

Sie können die Protokollierung von Überwachungsereignissen für Azure Key Vault aktivieren. Diese Protokolle werden in Azure Blob Storage geschrieben, wo sie dann durch Log Analytics für die Suche und Analyse indiziert werden können.

## <a name="install-and-configure-the-solution"></a>Installieren und Konfigurieren der Lösung
Gehen Sie folgendermaßen vor, um die Azure Key Vault-Lösung zu installieren und zu konfigurieren:

1. Aktivieren Sie die [Diagnoseprotokollierung für Key Vault](../key-vault/key-vault-logging.md)-Ressourcen, die Sie überwachen möchten.
2. Konfigurieren Sie Log Analytics für das Lesen der Protokolle aus Blob Storage. Gehen Sie dabei vor wie unter [JSON-Dateien in Blob Storage](log-analytics-azure-storage-json.md) beschrieben.
3. Aktivieren Sie die Azure Key Vault-Lösung mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses.  

## <a name="review-azure-key-vault-data-collection-details"></a>Überprüfen von Details zur Datensammlung von Azure Key Vault
Die Azure Key Vault-Lösung erfasst Diagnoseprotokolle von Azure Blob Storage für Azure Key Vault.
Es ist kein Agent für die Datensammlung erforderlich.

Die folgende Tabelle enthält die Datensammlungsmethoden und andere Details dazu, wie Daten für Azure Key Vault erfasst werden.

| Plattform | Direkt-Agent | System Center Operations Manager-Agent (SCOM) | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Azure |![Nein](./media/log-analytics-azure-keyvault/oms-bullet-red.png) |![Nein](./media/log-analytics-azure-keyvault/oms-bullet-red.png) |![Ja](./media/log-analytics-azure-keyvault/oms-bullet-green.png) |![Nein](./media/log-analytics-azure-keyvault/oms-bullet-red.png) |![Nein](./media/log-analytics-azure-keyvault/oms-bullet-red.png) |10 Minuten |

## <a name="use-azure-key-vault"></a>Verwenden von Azure Key Vault
Nachdem Sie die Lösung installiert haben, können Sie die Zusammenfassung der Anforderungsstatus anzeigen, die über einen bestimmten Zeitraum für Ihre überwachten Schlüsseltresore erfasst wurden. Verwenden Sie dazu in Log Analytics die Kachel **Azure Key Vault** auf der Seite **Übersicht**.

![Abbildung der Kachel „Azure Key Vault“](./media/log-analytics-azure-keyvault/log-analytics-keyvault-tile.png)

Nachdem Sie auf die Kachel **Übersicht** geklickt haben, können Sie Zusammenfassungen der Protokolle anzeigen und einen Drilldown zu Details für die folgenden Kategorien durchführen:

* Umfang aller Schlüsseltresorvorgänge über einen Zeitraum
* Fehler bei der Ausführung von Volumes über einen Zeitraum
* Durchschnittliche Ausführungslatenz nach Vorgang
* Quality of Service für Vorgänge mit der Anzahl von Vorgängen, die mehr als 1.000 ms dauern, sowie eine Liste der Vorgänge, die mehr als 1.000 ms dauern

![Abbildung des Azure Key Vault-Dashboards](./media/log-analytics-azure-keyvault/log-analytics-keyvault01.png)

![Abbildung des Azure Key Vault-Dashboards](./media/log-analytics-azure-keyvault/log-analytics-keyvault02.png)

### <a name="to-view-details-for-any-operation"></a>So zeigen Sie Details zu einzelnen Vorgängen an
1. Klicken Sie auf der Seite **Übersicht** auf die Kachel **Azure Key Vault**.
2. Prüfen Sie im **Azure Key Vault**-Dashboard die Zusammenfassungsinformationen in einem der Blätter, und klicken Sie dann auf einen Eintrag, um ausführliche Informationen auf der Seite „Protokollsuche“ anzuzeigen.
   
    Sie können auf jeder Seite für die Protokollsuche die Ergebnisse nach Zeit, detaillierte Ergebnisse und Ihren Protokollsuchverlauf anzeigen. Außerdem können Sie nach Facets filtern, um die Ergebnisse einzugrenzen.

## <a name="log-analytics-records"></a>Log Analytics-Datensätze
Die Azure Key Vault-Lösung analysiert Datensätze vom Typ **KeyVaults**, die aus den [AuditEvent-Protokollen](../key-vault/key-vault-logging.md) in Azure-Diagnose gesammelt wurden.  Die Eigenschaften der Datensätze finden Sie in der folgenden Tabelle.  

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Typ |*KeyVaults* |
| SourceSystem |*AzureStorage* |
| CallerIpAddress |IP-Adresse des Clients, der die Anforderung gestellt hat |
| Kategorie |Für Schlüsseltresor-Protokolle ist AuditEvent der einzige verfügbare Wert |
| CorrelationId |Optionale GUID, die vom Client zum Korrelieren von clientseitigen Protokollen mit dienstseitigen Protokollen (Schlüsseltresor) übergeben werden kann |
| DurationMs |Verarbeitungsdauer der REST-API-Anforderung in Millisekunden. Die Netzwerklatenz ist hierin nicht enthalten, sodass die auf der Clientseite gemessene Zeit unter Umständen nicht mit diesem Zeitraum übereinstimmt. |
| HttpStatusCode_d |Der von der Anforderung zurückgegebene HTTP-Statuscode |
| Id_s |Eindeutige ID der Anforderung |
| Identity_o |Identität des Tokens, das beim Erstellen der REST-API-Anforderung angegeben wurde. Dies ist normalerweise ein „Benutzer“, ein „Dienstprinzipal“ oder die Kombination „Benutzer + App-ID“, wie bei einer Anforderung, die auf einem Azure PowerShell-Cmdlet basiert. |
| NameVorgang |Der Name des Vorgangs, wie unter [Azure Key Vault-Protokollierung](../key-vault/key-vault-logging.md) beschrieben |
| OperationVersion |Die vom Client angeforderte REST-API-Version |
| RemoteIPLatitude |Der Breitengrad des Clients, der die Anforderung gestellt hat |
| RemoteIPLongitude |Der Längengrad des Clients, der die Anforderung gestellt hat |
| RemoteIPCountry |Das Land des Clients, der die Anforderung gestellt hat |
| RequestUri_s |Der URI der Anforderung |
| Ressource |Der Name des Schlüsseltresors |
| ResourceGroup |Die Ressourcengruppe des Schlüsseltresors |
| ResourceId |Azure-Ressourcen-Manager-Ressourcen-ID. Für Schlüsseltresor-Protokolle ist dies immer die Schlüsseltresor-Ressourcen-ID. |
| ResourceProvider |*MICROSOFT.KEYVAULT* |
| ResultSignature |HTTP-Status |
| ResultType |Das Ergebnis der REST-API-Anforderung |
| SubscriptionId |Die ID des Azure-Abonnements mit dem Schlüsseltresor |

## <a name="next-steps"></a>Nächste Schritte
* Verwenden Sie [Protokollsuchen in Log Analytics](log-analytics-log-searches.md), um ausführliche Azure Key Vault-Daten anzuzeigen.

<!--HONumber=Oct16_HO2-->


