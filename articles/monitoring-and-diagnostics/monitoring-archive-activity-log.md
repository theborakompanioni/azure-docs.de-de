---
title: "Archivieren des Azure-Aktivitätsprotokolls | Microsoft Docs"
description: "Hier erfahren Sie, wie Sie Ihr Azure-Aktivitätsprotokoll zur langfristigen Aufbewahrung in einem Speicherkonto archivieren."
author: johnkemnetz
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
translationtype: Human Translation
ms.sourcegitcommit: aaa162df8a6cd60cb174242e6a353439f2da58b4
ms.openlocfilehash: eb3a0ad811a4286df1bac963904bd9154c0ccfa3


---
# <a name="archive-the-azure-activity-log"></a>Archivieren des Azure-Aktivitätsprotokolls
In diesem Artikel erfahren Sie, wie Sie Ihr [**Azure-Aktivitätsprotokoll**](monitoring-overview-activity-logs.md) über das Azure-Portal, mithilfe von PowerShell-Cmdlets oder mithilfe der plattformübergreifenden Befehlszeilenschnittstelle in einem Speicherkonto archivieren. Dies ist hilfreich, wenn Sie Ihr Aktivitätsprotokoll (bei vollständiger Kontrolle über die Aufbewahrungsrichtlinie) zur Überwachung, statischen Analyse oder als Sicherungskopie länger als 90 Tage aufbewahren möchten. Falls Sie Ihre Ereignisse nur maximal 90 Tage lang aufbewahren möchten, müssen Sie keine Archivierung in einem Speicherkonto einrichten, da Aktivitätsprotokollereignisse in der Azure-Plattform auch ohne aktivierte Archivierung 90 Tage lang aufbewahrt werden.

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie beginnen, müssen Sie [ein Speicherkonto erstellen](../storage/storage-create-storage-account.md#create-a-storage-account) , in dem Sie Ihr Aktivitätsprotokoll archivieren können. Um den Zugriff auf Überwachungsdaten besser steuern zu können, wird dringend davon abgeraten, ein bereits vorhandenes Speicherkonto mit anderen, nicht überwachungsbezogenen Daten zu verwenden. Wenn Sie jedoch auch Diagnoseprotokolle und Metriken in einem Speicherkonto archivieren, ist es unter Umständen sinnvoll, dieses Speicherkonto auch für Ihr Aktivitätsprotokoll zu verwenden, damit sich alle Überwachungsdaten an einem zentralen Ort befinden. Bei dem Speicherkonto muss es sich um ein allgemeines Speicherkonto (nicht um ein Blobspeicherkonto) handeln. Das Speicherkonto muss sich nicht unter demselben Abonnement befinden, das Protokolle ausgibt, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements hat.

## <a name="log-profile"></a>Protokollprofil
Legen Sie das **Protokollprofil** für ein Abonnement fest, um das Aktivitätsprotokoll mit einer der weiter unten angegebenen Methoden zu archivieren. Das Protokollprofil definiert die Art der Ereignisse, die gespeichert oder gestreamt werden, sowie die Ausgaben (Speicherkonto und/oder Event Hub). Außerdem definiert es die Aufbewahrungsrichtlinie (Anzahl von Tagen für die Aufbewahrung) für Ereignisse, die in einem Speicherkonto gespeichert werden. Wird die Aufbewahrungsrichtlinie auf Null festgelegt, werden Ereignisse dauerhaft gespeichert. Andernfalls kann sie auf einen beliebigen Wert zwischen 1 und 2.147.483.647 festgelegt werden. Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht. Weitere Informationen zu Protokollprofilen finden Sie [hier](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles). 

## <a name="archive-the-activity-log-using-the-portal"></a>Archivieren des Aktivitätsprotokolls über das Portal
1. Klicken Sie im linken Navigationsbereich des Portals auf den Link **Aktivitätsprotokoll** . Sollte kein Link für das Aktivitätsprotokoll angezeigt werden, klicken Sie zuerst auf den Link **Weitere Dienste** .
   
    ![Zum Blatt „Aktivitätsprotokoll“ navigieren](media/monitoring-archive-activity-log/act-log-portal-navigate.png)
2. Klicken Sie im oberen Bereich des Blatts auf **Exportieren**.
   
    ![Auf die Schaltfläche „Exportieren“ klicken](media/monitoring-archive-activity-log/act-log-portal-export-button.png)
3. Aktivieren Sie auf dem daraufhin angezeigten Blatt das Kontrollkästchen **Export to a storage account** (In ein Speicherkonto exportieren), und wählen Sie ein Speicherkonto aus.
   
    ![Speicherkonto festlegen](media/monitoring-archive-activity-log/act-log-portal-export-blade.png)
4. Geben Sie mithilfe des Schiebereglers oder des Textfelds an, für wie viele Tage Aktivitätsprotokollereignisse in Ihrem Speicherkonto aufbewahrt werden sollen. Falls die Daten dauerhaft im Speicherkonto gespeichert werden sollen, legen Sie den Wert auf „0“ fest.
5. Klicken Sie auf **Speichern**.

## <a name="archive-the-activity-log-via-powershell"></a>Archivieren des Aktivitätsprotokolls mithilfe von PowerShell
```
Add-AzureRmLogProfile -Name my_log_profile -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus -RetentionInDays 180 -Categories Write,Delete,Action
```

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| StorageAccountId |Nein |Ressourcen-ID des Speicherkontos, in dem Aktivitätsprotokolle gespeichert werden sollen. |
| Locations |Ja |Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. Eine Liste mit allen Bereichen können Sie sich auf [dieser Seite](https://azure.microsoft.com/en-us/regions) oder mithilfe der [Azure-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/gg441293.aspx) ansehen. |
| RetentionInDays |Ja |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von&0; werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| Categories |Ja |Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

## <a name="archive-the-activity-log-via-cli"></a>Archivieren des Aktivitätsprotokolls mithilfe der Befehlszeilenschnittstelle
```
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --locations global,westus,eastus,northeurope --retentionInDays 180 –categories Write,Delete,Action
```

| Eigenschaft | Erforderlich | Beschreibung |
| --- | --- | --- |
| Name |Ja |Name des Protokollprofils. |
| storageId |Nein |Ressourcen-ID des Speicherkontos, in dem Aktivitätsprotokolle gespeichert werden sollen. |
| Locations |Ja |Kommagetrennte Liste mit den Regionen, für die Sie Aktivitätsprotokollereignisse erfassen möchten. Eine Liste mit allen Bereichen können Sie sich auf [dieser Seite](https://azure.microsoft.com/en-us/regions) oder mithilfe der [Azure-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/gg441293.aspx) ansehen. |
| RetentionInDays |Ja |Anzahl von Tagen für die Aufbewahrung von Ereignissen (1 bis 2.147.483.647). Bei einem Wert von&0; werden die Protokolle dauerhaft (d.h. für immer) gespeichert. |
| categories |Ja |Kommagetrennte Liste mit den Ereigniskategorien, die erfasst werden sollen. Mögliche Werte sind „Write“, „Delete“ und „Action“. |

## <a name="storage-schema-of-the-activity-log"></a>Speicherschema des Aktivitätsprotokolls
Nach dem Einrichten der Archivierung wird in dem Speicherkonto ein Speichercontainer erstellt, sobald ein Aktivitätsprotokollereignis auftritt. Bei den Blobs innerhalb des Containers wird sowohl für das Aktivitätsprotokoll als auch für die Diagnoseprotokolle ein einheitliches Format verwendet. Die Struktur dieser Blobs sieht wie folgt aus:

> insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/{Abonnement-ID}/y={Jahr (vierstellige Zahl)}/m={Monat (zweistellige Zahl)}/d={Tag (zweistellige Zahl)}/h={Stunde im 24-Stunden-Format (zweistellige Zahl)}/m=00/PT1H.json
> 
> 

Beispiel für einen Blobnamen:

> insights-operational-logs/name=default/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/y=2016/m=08/d=22/h=18/m=00/PT1H.json
> 
> 

Jedes Blob vom Typ „PT1H.json“ enthält ein JSON-Blob mit Ereignissen, die innerhalb der in der Blob-URL angegebenen Stunde (Beispiel: h=12) aufgetreten sind. Während der aktuellen Stunde werden Ereignisse an die Datei „PT1H.json“ angefügt, sobald sie auftreten. Der Minutenwert (m=00) ist immer „00“, da Aktivitätsprotokollereignisse stundenweise in einzelne Blobs unterteilt werden.

Die einzelnen Ereignisse werden innerhalb der Datei „PT1H.json“ im folgenden Format im Array „records“ gespeichert:

```
{
    "records": [
        {
            "time": "2015-01-21T22:14:26.9792776Z",
            "resourceId": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
            "operationName": "microsoft.support/supporttickets/write",
            "category": "Write",
            "resultType": "Success",
            "resultSignature": "Succeeded.Created",
            "durationMs": 2826,
            "callerIpAddress": "111.111.111.11",
            "correlationId": "c776f9f4-36e5-4e0e-809b-c9b3c3fb62a8",
            "identity": {
                "authorization": {
                    "scope": "/subscriptions/s1/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841",
                    "action": "microsoft.support/supporttickets/write",
                    "evidence": {
                        "role": "Subscription Admin"
                    }
                },
                "claims": {
                    "aud": "https://management.core.windows.net/",
                    "iss": "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",
                    "iat": "1421876371",
                    "nbf": "1421876371",
                    "exp": "1421880271",
                    "ver": "1.0",
                    "http://schemas.microsoft.com/identity/claims/tenantid": "1e8d8218-c5e7-4578-9acc-9abbd5d23315 ",
                    "http://schemas.microsoft.com/claims/authnmethodsreferences": "pwd",
                    "http://schemas.microsoft.com/identity/claims/objectidentifier": "2468adf0-8211-44e3-95xq-85137af64708",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "admin@contoso.com",
                    "puid": "20030000801A118C",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "9vckmEGF7zDKk1YzIY8k0t1_EAPaXoeHyPRn6f413zM",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "John",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "Smith",
                    "name": "John Smith",
                    "groups": "cacfe77c-e058-4712-83qw-f9b08849fd60,7f71d11d-4c41-4b23-99d2-d32ce7aa621c,31522864-0578-4ea0-9gdc-e66cc564d18c",
                    "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": " admin@contoso.com",
                    "appid": "c44b4083-3bq0-49c1-b47d-974e53cbdf3c",
                    "appidacr": "2",
                    "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
                    "http://schemas.microsoft.com/claims/authnclassreference": "1"
                }
            },
            "level": "Information",
            "location": "global",
            "properties": {
                "statusCode": "Created",
                "serviceRequestId": "50d5cddb-8ca0-47ad-9b80-6cde2207f97c"
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
| category |Kategorie der Aktion, z.B. Schreiben, Lesen, Aktion. |
| resultType |Der Typ des Ergebnisses, z.B. Erfolg, Fehler, Start |
| resultSignature |Abhängig vom Ressourcentyp. |
| durationMs |Dauer des Vorgangs in Millisekunden. |
| callerIpAddress |IP-Adresse des Benutzers, der den Vorgang, UPN-Anspruch oder SPN-Anspruch ausgeführt hat (sofern verfügbar). |
| correlationId |Normalerweise eine GUID im Zeichenfolgenformat. Ereignisse, die über die gleiche correlationId verfügen, gehören zu derselben übergeordneten Aktion. |
| identity |JSON-Blob zur Beschreibung der Autorisierung und Ansprüche. |
| authorization |Blob mit RBAC-Eigenschaften des Ereignisses. Enthält normalerweise die Eigenschaften „action“, „role“ und „scope“. |
| level |Ebene des Ereignisses. Einer der folgenden Werte: Critical, Error, Warning, Informational, Verbose |
| location |Region des Speicherorts (oder „global“). |
| properties |Satz mit `<Key, Value>` -Paaren (Wörterbuch), die die Details des Ereignisses beschreiben. |

> [!NOTE]
> Die Eigenschaften und deren Verwendung können je nach Ressource variieren.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* [Herunterladen von Blobs für die Analyse](../storage/storage-dotnet-how-to-use-blobs.md#download-blobs)
* [Streamen des Aktivitätsprotokolls an Event Hubs](monitoring-stream-activity-logs-event-hubs.md)
* [Weitere Informationen zum Aktivitätsprotokoll](monitoring-overview-activity-logs.md)




<!--HONumber=Dec16_HO2-->


