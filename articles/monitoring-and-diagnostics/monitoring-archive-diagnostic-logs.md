---
title: Archivieren von Azure-Diagnoseprotokollen | Microsoft Docs
description: Hier erfahren Sie, wie Sie Ihre Azure-Diagnoseprotokolle zur langfristigen Aufbewahrung in einem Speicherkonto archivieren.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 3a55c73f-2ef3-45f3-8956-bcf9c0cb7e05
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: dbc5f89001dcb6cd1ab061cb0a9632e4e5d2c1c7
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="archive-azure-diagnostic-logs"></a>Archivieren von Azure-Diagnoseprotokollen
In diesem Artikel erfahren Sie, wie Sie Ihre [Azure-Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md) über das Azure-Portal, mit PowerShell-Cmdlets, der CLI oder der REST-API in einem Speicherkonto archivieren. Diese Option ist hilfreich, wenn Sie Ihre Diagnoseprotokolle mit einer optionalen Aufbewahrungsrichtlinie zur Überwachung, statischen Analyse oder als Sicherungskopie aufbewahren möchten. Das Speicherkonto muss sich nicht unter demselben Abonnement befinden wie die Ressource, die Protokolle ausgibt, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements hat.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie beginnen, müssen Sie [ein Speicherkonto erstellen](../storage/storage-create-storage-account.md) , in dem Sie Ihre Diagnoseprotokolle archivieren können. Um den Zugriff auf Überwachungsdaten besser steuern zu können, wird dringend davon abgeraten, ein bereits vorhandenes Speicherkonto mit anderen, nicht überwachungsbezogenen Daten zu verwenden. Wenn Sie jedoch auch Ihr Aktivitätsprotokoll und Ihre Diagnosemetriken in einem Speicherkonto archivieren, ist es unter Umständen sinnvoll, dieses Speicherkonto auch für Ihre Diagnoseprotokolle zu verwenden, damit sich alle Überwachungsdaten an einem zentralen Ort befinden. Bei dem Speicherkonto muss es sich um ein allgemeines Speicherkonto (nicht um ein Blobspeicherkonto) handeln.

## <a name="diagnostic-settings"></a>Diagnoseeinstellungen
Legen Sie eine **Diagnoseeinstellung** für eine bestimmte Ressource fest, um die Diagnoseprotokolle mit einer der weiter unten angegebenen Methoden zu archivieren. Eine Diagnoseeinstellung für eine Ressource definiert die Kategorien der Protokolle und Metrikdaten, die an ein Ziel (Speicherkonto, Event Hubs-Namespace oder Log Analytics) gesendet werden. Außerdem definiert sie die Aufbewahrungsrichtlinie (Anzahl von Tagen für die Aufbewahrung) für Ereignisse jeder Protokollkategorie und Metrikdaten, die in einem Speicherkonto gespeichert werden. Wird die Aufbewahrungsrichtlinie auf Null festgelegt, werden Ereignisse für diese Protokollkategorie dauerhaft (also für immer) gespeichert. Für eine Aufbewahrungsrichtlinie kann andernfalls eine beliebige Anzahl von Tagen zwischen 1 und 2.147.483.647 festgelegt werden. [Hier](monitoring-overview-of-diagnostic-logs.md#resource-diagnostic-settings)erfahren Sie mehr über Diagnoseeinstellungen. Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht.

## <a name="archive-diagnostic-logs-using-the-portal"></a>Archivieren von Diagnoseprotokollen mithilfe des Portals
1. Navigieren Sie im Portal zu Azure Monitor, und klicken Sie auf **Diagnoseeinstellungen**.

    ![Abschnitt „Überwachung“ von Azure Monitor](media/monitoring-archive-diagnostic-logs/diagnostic-settings-blade.png)

2. Filtern Sie optional die Liste nach Ressourcengruppe oder Ressourcentyp, und klicken Sie auf die Ressource, für die Sie eine Diagnoseeinstellung festlegen möchten.

3. Wenn keine Einstellungen für die Ressource vorhanden sind, die Sie ausgewählt haben, werden Sie aufgefordert, eine Einstellung zu erstellen. Klicken Sie auf „Diagnose aktivieren“.

   ![Diagnoseeinstellung hinzufügen – keine Einstellungen vorhanden](media/monitoring-archive-diagnostic-logs/diagnostic-settings-none.png)

   Wenn Einstellungen für die Ressource vorhanden sind, sehen Sie eine Liste der Einstellungen, die bereits für diese Ressource konfiguriert sind. Klicken Sie auf „Diagnoseeinstellung hinzufügen“.

   ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/monitoring-archive-diagnostic-logs/diagnostic-settings-multiple.png)

3. Geben Sie Ihrer Einstellung einen Namen, aktivieren Sie das Kontrollkästchen **In Speicherkonto exportieren**, und wählen Sie dann ein Speicherkonto aus. Legen Sie optional eine Anzahl von Tagen für die Aufbewahrung dieser Protokolle fest. Verwenden Sie dazu die Schieberegler unter **Aufbewahrung (Tage)**. Bei einer Aufbewahrung von 0 Tagen werden die Protokolle dauerhaft gespeichert.
   
   ![Diagnoseeinstellung hinzufügen – Einstellungen vorhanden](media/monitoring-archive-diagnostic-logs/diagnostic-settings-configure.png)
    
4. Klicken Sie auf **Speichern**.

Nach einigen Augenblicken wird die neue Einstellung in der Liste der Einstellungen für diese Ressource angezeigt, und Diagnoseprotokolle werden in diesem Speicherkonto archiviert, sobald neue Ereignisdaten generiert werden.

## <a name="archive-diagnostic-logs-via-azure-powershell"></a>Archivieren von Diagnoseprotokollen mit Azure PowerShell
```
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg -StorageAccountId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Categories networksecuritygroupevent,networksecuritygrouprulecounter -Enabled $true -RetentionEnabled $true -RetentionInDays 90
```

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| ResourceId |Ja |Ressourcen-ID der Ressource, für die Sie eine Diagnoseeinstellung festlegen möchten |
| StorageAccountId |Nein |Ressourcen-ID des Speicherkontos, in dem Diagnoseprotokolle gespeichert werden sollen |
| Categories |Nein |Durch Trennzeichen getrennte Liste der zu aktivierenden Protokollkategorien |
| Aktiviert |Ja |Boolescher Wert, der angibt, ob die Diagnose für diese Ressource aktiviert oder deaktiviert werden soll |
| RetentionEnabled |Nein |Boolescher Wert, der angibt, ob eine Aufbewahrungsrichtlinie für diese Ressource aktiviert ist |
| RetentionInDays |Nein |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von 0 werden die Protokolle dauerhaft gespeichert. |

## <a name="archive-diagnostic-logs-via-the-cross-platform-cli"></a>Archivieren von Diagnoseprotokollen mit der plattformübergreifenden Befehlszeilenschnittstelle
```
azure insights diagnostic set --resourceId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg --storageId /subscriptions/s1id1234-5679-0123-4567-890123456789/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage –categories networksecuritygroupevent,networksecuritygrouprulecounter --enabled true
```

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| ResourceId |Ja |Ressourcen-ID der Ressource, für die Sie eine Diagnoseeinstellung festlegen möchten |
| storageId |Nein |Ressourcen-ID des Speicherkontos, in dem Diagnoseprotokolle gespeichert werden sollen. |
| categories |Nein |Durch Trennzeichen getrennte Liste der zu aktivierenden Protokollkategorien |
| Aktiviert |Ja |Boolescher Wert, der angibt, ob die Diagnose für diese Ressource aktiviert oder deaktiviert werden soll |

## <a name="archive-diagnostic-logs-via-the-rest-api"></a>Archivieren von Diagnoseprotokollen mit der REST-API
[Dieses Dokument](https://docs.microsoft.com/rest/api/monitor/servicediagnosticsettings) enthält Informationen zum Einrichten einer Diagnoseeinstellung mit der Azure Monitor-REST-API.

## <a name="schema-of-diagnostic-logs-in-the-storage-account"></a>Schema der Diagnoseprotokolle im Speicherkonto
Nach dem Einrichten der Archivierung wird in dem Speicherkonto ein Speichercontainer erstellt, sobald ein Ereignis in einer der aktivierten Protokollkategorien auftritt. Bei den Blobs innerhalb des Containers wird sowohl für die Diagnoseprotokolle als auch das Aktivitätsprotokoll ein einheitliches Format verwendet. Die Struktur dieser Blobs sieht wie folgt aus:

> insights-logs-{Name der Protokollkategorie}/resourceId=/SUBSCRIPTIONS/{Abonnement-ID}/RESOURCEGROUPS/{Ressourcengruppenname}/PROVIDERS/{Name des Ressourcenanbieters}/{Ressourcentyp}/{Ressourcenname}/y={Jahr (vierstellige Zahl)}/m={Monat (zweistellige Zahl)}/d={Tag (zweistellige Zahl)}/h={Stunde im 24-Stunden-Format (zweistellige Zahl)}/m=00/PT1H.json
> 
> 

Einfacher ausgedrückt:

> insights-logs-{Name der Protokollkategorie}/resourceId=/{Ressourcen-ID}/y={Jahr (vierstellige Zahl)}/m={Monat (zweistellige Zahl)}/d={Tag (zweistellige Zahl)}/h={Stunde im 24-Stunden-Format (zweistellige Zahl)}/m=00/PT1H.json
> 
> 

Beispiel für einen Blobnamen:

> insights-logs-networksecuritygrouprulecounter/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUP/TESTNSG/y=2016/m=08/d=22/h=18/m=00/PT1H.json
> 
> 

Jedes Blob vom Typ „PT1H.json“ enthält ein JSON-Blob mit Ereignissen, die innerhalb der in der Blob-URL angegebenen Stunde (Beispiel: h=12) aufgetreten sind. Während der aktuellen Stunde werden Ereignisse an die Datei „PT1H.json“ angefügt, sobald sie auftreten. Der Minutenwert (m=00) ist immer „00“, da Diagnoseprotokollereignisse stundenweise in einzelne Blobs unterteilt werden.

Die einzelnen Ereignisse werden innerhalb der Datei „PT1H.json“ im folgenden Format im Array „records“ gespeichert:

```
{
    "records": [
        {
            "time": "2016-07-01T00:00:37.2040000Z",
            "systemId": "46cdbb41-cb9c-4f3d-a5b4-1d458d827ff1",
            "category": "NetworkSecurityGroupRuleCounter",
            "resourceId": "/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/TESTNSG",
            "operationName": "NetworkSecurityGroupCounters",
            "properties": {
                "vnetResourceGuid": "{12345678-9012-3456-7890-123456789012}",
                "subnetPrefix": "10.3.0.0/24",
                "macAddress": "000123456789",
                "ruleName": "/subscriptions/ s1id1234-5679-0123-4567-890123456789/resourceGroups/testresourcegroup/providers/Microsoft.Network/networkSecurityGroups/testnsg/securityRules/default-allow-rdp",
                "direction": "In",
                "type": "allow",
                "matchedConnections": 1988
            }
        }
    ]
}
```

| Elementname | Beschreibung |
| --- | --- |
| time |Zeitstempel der Ereignisgenerierung durch den Azure-Dienst, der die zum Ereignis gehörende Anforderung verarbeitet hat. |
| ResourceId |Ressourcen-ID der betroffenen Ressource. |
| operationName |Name des Vorgangs. |
| category |Protokollkategorie des Ereignisses |
| Eigenschaften |Satz mit `<Key, Value>` -Paaren (Wörterbuch), die die Details des Ereignisses beschreiben. |

> [!NOTE]
> Die Eigenschaften und deren Verwendung können je nach Ressource variieren.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Herunterladen von Blobs für die Analyse](../storage/storage-dotnet-how-to-use-blobs.md)
* [Streamen von Diagnoseprotokollen an einen Event Hubs-Namespace](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Weitere Informationen zu Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md)

