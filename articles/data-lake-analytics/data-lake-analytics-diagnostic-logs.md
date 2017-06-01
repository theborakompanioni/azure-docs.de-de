---
title: "Anzeigen von Diagnoseprotokollen für Azure Data Lake Analytics | Microsoft Docs"
description: "Enthält eine Beschreibung der Grundlagen zum Einrichten von Diagnoseprotokollen und zum damit verbundenen Zugriff für Azure Data Lake Analytics. "
services: data-lake-analytics
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: cf5633d4-bc43-444e-90fc-f90fbd0b7935
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/03/2017
ms.author: larryfr
ms.translationtype: Human Translation
ms.sourcegitcommit: 67ee6932f417194d6d9ee1e18bb716f02cf7605d
ms.openlocfilehash: 71a839d9c076d33c404a56bf0544666dc86eaa4d
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics

Erfahren Sie, wie Sie die Diagnoseprotokollierung für Ihr Data Lake Analytics-Konto aktivieren und die für Ihr Konto erfassten Protokolle anzeigen.

Organisationen können die Diagnoseprotokollierung für ihr Azure Data Lake Analytics-Konto aktivieren, um Überwachungspfade für den Datenzugriff zu erfassen. Diese Protokolle enthalten beispielsweise folgende Informationen:

* Liste der Benutzer, die auf die Daten zugegriffen haben
* Häufigkeit des Datenzugriffs
* Menge der im Konto gespeicherten Daten

## <a name="enable-logging"></a>Aktivieren der Protokollierung

1. Melden Sie sich am neuen [Azure-Portal](https://portal.azure.com)an.

2. Öffnen Sie Ihr Data Lake Analytics-Konto, und wählen Sie im __Überwachungsabschnitt__ die Option **Diagnoseprotokolle**. Wählen Sie anschließend __Diagnose aktivieren__.

    ![Aktivieren der Diagnose zum Sammeln von Überwachungs- und Anforderungsprotokollen](./media/data-lake-analytics-diagnostic-logs/turn-on-logging.png)

3. Legen Sie in den __Diagnoseeinstellungen__ den Status auf __Ein__ fest, und wählen Sie Protokollierungsoptionen aus.

    ![Aktivieren der Diagnose zum Sammeln von Überwachungs- und Anforderungsprotokollen](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Aktivieren von Diagnoseprotokollen")

   * Legen Sie **Status** auf **Ein** fest, um die Diagnoseprotokollierung zu aktivieren.

   * Sie können die Daten auf zwei verschiedene Arten speichern/verarbeiten.

     * Wählen Sie __In einem Speicherkonto archivieren__, um Protokolle in einem Azure-Speicherkonto zu speichern. Verwenden Sie diese Option, wenn Sie die Daten archivieren möchten. Bei Auswahl dieser Option müssen Sie ein Azure-Speicherkonto zum Speichern der Protokolle angeben.

     * Wählen Sie **An einen Event Hub streamen**, um die Protokolldaten an einen Azure Event Hub zu streamen. Verwenden Sie diese Option, wenn Sie eine Downstream-Verarbeitungspipeline einsetzen, um eingehende Protokolle in Echtzeit zu analysieren. Wenn Sie diese Option auswählen, müssen Sie die Details für den Azure Event Hub angeben, den Sie verwenden möchten.

     * Wählen Sie __An Log Analytics senden__, um die Daten an den Log Analytics-Dienst zu senden. Verwenden Sie diese Option, wenn Sie Log Analytics zum Sammeln und Analysieren von Protokollen verwenden möchten.
   * Geben Sie an, ob Sie Überwachungsprotokolle oder Anforderungsprotokolle oder beides abrufen möchten.

   * Geben Sie die Anzahl der Tage an, für die die Daten beibehalten werden müssen.

   * Klicken Sie auf **Speichern**.

        > [!NOTE]
        > Wählen Sie entweder __In einem Speicherkonto archivieren__, __An einen Event Hub streamen__ oder __An Log Analytics senden__ aus, bevor Sie die Schaltfläche __Speichern__ wählen.

Nachdem Sie die Diagnoseeinstellungen aktiviert haben, können Sie zum Blatt __Diagnoseprotokolle__ zurückkehren, um Protokolle anzuzeigen.

## <a name="view-logs"></a>Anzeigen von Protokollen

Es gibt zwei Möglichkeiten, die Protokolldaten Ihres Data Lake Analytics-Kontos anzuzeigen:

* Über die Einstellungen des Data Lake Analytics-Kontos
* Über das Azure Storage-Konto, in dem die Daten gespeichert sind

### <a name="using-the-data-lake-analytics-settings-view"></a>Verwenden der Ansicht mit den Data Lake Analytics-Einstellungen

1. Wählen Sie im Blatt Ihres Data Lake Analytics-Kontos die Option **Diagnoseprotokolle** aus, und wählen Sie dann den Eintrag aus, für den Sie Protokolle anzeigen möchten.

    ![Anzeigen der Diagnoseprotokollierung](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Anzeigen von Diagnoseprotokollen")

2. Die Protokolle sind in **Überwachungsprotokolle** und **Anforderungsprotokolle** unterteilt.

    ![Protokolleinträge](./media/data-lake-analytics-diagnostic-logs/diagnostic-log-entries.png)

   * Anforderungsprotokolle erfassen jede API-Anforderung im Data Lake Analytics-Konto.
   * Überwachungsprotokolle ähneln Anforderungsprotokollen, bieten aber eine viel detailliertere Aufschlüsselung der Vorgänge, die auf dem Data Lake Analytics-Konto ausgeführt werden. Ein einzelner API-Hochladeaufruf in Anforderungsprotokollen könnte möglicherweise in den Überwachungsprotokollen in mehreren „Anfügen“-Vorgängen resultieren.

3. Klicken Sie für einen Protokolleintrag auf den Link **Herunterladen** , um die Protokolle herunterzuladen.

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>Im Azure Storage-Konto, das die Protokolldaten enthält

1. Öffnen Sie das Azure Storage-Kontoblatt, das Data Lake Analytics zur Protokollierung zugeordnet ist, und klicken Sie dann auf „Blobs“. Auf dem Blatt **Blob-Dienst** werden zwei Container aufgelistet.

    ![Anzeigen der Diagnoseprotokollierung](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Anzeigen von Diagnoseprotokollen")

   * Der Container **insights-logs-audit** enthält die Überwachungsprotokolle.
   * Der Container **insights-logs-requests** enthält die Anforderungsprotokolle.
2. Innerhalb dieser Container werden die Protokolle in der folgenden Struktur gespeichert.

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json

   > [!NOTE]
   > Auf dem Blatt `##` -Einträge im Pfad enthalten Jahr, Monat, Tag und Stunde der Protokollerstellung. Data Lake Analytics erstellt eine Datei pro Stunde, sodass `m=` immer den Wert `00` enthält.

    Der vollständige Pfad zu einem Überwachungsprotokoll kann beispielsweise wie folgt lauten:

        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    Entsprechend kann der vollständige Pfad zu einem Anforderungsprotokoll wie folgt lauten:

        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>Protokollstruktur

Die Überwachungs- und Anforderungsprotokolle liegen im JSON-Format vor. In diesem Abschnitt betrachten wir die Struktur von JSON für Anforderungs- und Überwachungsprotokolle.

### <a name="request-logs"></a>Anforderungsprotokollen

Hier ist ein Beispiel für einen Eintrag im JSON-formatierten Anforderungsprotokoll. Jedes Blob hat ein Stammobjekt namens **records** , das ein Array mit Protokollobjekten enthält.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>Anforderungsprotokollschema

| Name | Typ | Beschreibung |
| --- | --- | --- |
| in |String |Der Zeitstempel (UTC) des Protokolls. |
| Ressourcen-ID |String |Die ID der Ressource, auf der der Vorgang stattfand. |
| category |String |Die Protokollkategorie. Beispiel: **Anforderungen**. |
| operationName |String |Der Name des protokollierten Vorgangs. Beispiel: GetAggregatedJobHistory. |
| resultType |String |Der Status des Vorgangs, beispielsweise 200. |
| callerIpAddress |String |Die IP-Adresse des Clients, der die Anforderung gestellt hat. |
| correlationId |String |Die ID des Protokolls. Dieser Wert kann verwendet werden, um einen Satz von zusammengehörigen Protokolleinträgen zu gruppieren. |
| Identität |Objekt |Die Identität, die das Protokoll erstellt hat. |
| Eigenschaften |JSON |Details hierzu finden Sie im nächsten Abschnitt (Eigenschaftenschema des Anforderungsprotokolls). |

#### <a name="request-log-properties-schema"></a>Eigenschaftenschema des Anforderungsprotokolls

| Name | Typ | Beschreibung |
| --- | --- | --- |
| HttpMethod |String |Die HTTP-Methode, die für den Vorgang verwendet werden. Beispiel: GET. |
| Pfad |String |Der Pfad, in dem der Vorgang durchgeführt wurde. |
| RequestContentLength |int |Die Inhaltslänge der HTTP-Anforderung. |
| ClientRequestId |String |Die ID, die diese Anforderung eindeutig identifiziert. |
| StartTime |String |Der Zeitpunkt, zu dem der Server die Anforderung empfangen hat. |
| EndTime |String |Der Zeitpunkt, zu dem der Server eine Antwort gesendet hat. |

### <a name="audit-logs"></a>Überwachungsprotokolle

Hier ist ein Beispiel für einen Eintrag im JSON-formatierten Überwachungsprotokoll. Jeder Blob hat ein Stammobjekt namens **records** , das ein Array von Protokollobjekten enthält.

    {
    "records":
      [        
        . . . .
        ,
        {
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job",
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>Überwachungsprotokollschema

| Name | Typ | Beschreibung |
| --- | --- | --- |
| in |String |Der Zeitstempel (UTC) des Protokolls. |
| Ressourcen-ID |String |Die ID der Ressource, auf der der Vorgang stattfand. |
| category |String |Die Protokollkategorie. Beispiel: **Überwachung**. |
| operationName |String |Der Name des protokollierten Vorgangs. Beispiel: JobSubmitted. |
| resultType |String |Ein Unterstatus für den Auftragsstatus (operationName). |
| resultSignature |String |Weitere Informationen zum Auftragsstatus (operationName). |
| Identität |String |Der Benutzer, der den Vorgang angefordert hat. Beispiel: susan@contoso.com. |
| Eigenschaften |JSON |Details hierzu finden Sie im nächsten Abschnitt (Eigenschaftenschema des Überwachungsprotokolls). |

> [!NOTE]
> Die Elemente **resultType** und **resultSignature** liefern Informationen zum Ergebnis eines Vorgangs und enthalten nur einen Wert, wenn ein Vorgang abgeschlossen wurde. Beispielsweise ist ein Wert vorhanden, wenn **operationName** den Wert **JobStarted** oder **JobEnded** enthält.
>
>

#### <a name="audit-log-properties-schema"></a>Eigenschaftenschema des Überwachungsprotokolls

| Name | Typ | Beschreibung |
| --- | --- | --- |
| JobId |String |Die ID, die dem Auftrag zugewiesen ist. |
| JobName |String |Der Name, der für den Auftrag angegeben wurde. |
| JobRunTime |String |Die zum Verarbeiten des Auftrags verwendete Laufzeit. |
| SubmitTime |String |Der Zeitpunkt (UTC), zu dem der Auftrag übermittelt wurde. |
| StartTime |String |Der Zeitpunkt (UTC), zu dem der Auftrag nach der Übermittlung gestartet wurde. |
| EndTime |String |Der Zeitpunkt, zu dem der Auftrag beendet wurde. |
| Parallelität |String |Die Anzahl von Data Lake Analytics-Einheiten, die für diesen Auftrag während der Übermittlung angefordert wurden. |

> [!NOTE]
> Die Elemente **SubmitTime**, **StartTime**, **EndTime** und **Parallelism** liefern Informationen zu einem Vorgang und enthalten nur dann einen Wert, wenn ein Vorgang gestartet oder abgeschlossen wurde. Beispiel: **SubmitTime** enthält einen Wert, nachdem **operationName** **JobSubmitted** angegeben hat.

## <a name="process-the-log-data"></a>Verarbeitung der Protokolldaten

Azure Data Lake Analytics stellt ein Muster bereit, nach dem die Protokolldaten verarbeitet und analysiert werden sollen. Sie finden das Beispiel hier: [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).

## <a name="next-steps"></a>Nächste Schritte
* [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md)

