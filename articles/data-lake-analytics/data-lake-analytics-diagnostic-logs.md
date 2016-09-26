<properties 
   pageTitle="Anzeigen von Diagnoseprotokollen für Azure Data Lake Analytics | Microsoft Azure" 
   description="Enthält eine Beschreibung der Grundlagen zum Einrichten von Diagnoseprotokollen und zum damit verbundenen Zugriff für Azure Data Lake Analytics. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics

Erfahren Sie, wie Sie die Diagnoseprotokollierung für Ihr Data Lake Analytics-Konto aktivieren und die für Ihr Konto erfassten Protokolle anzeigen.

Organisationen können die Diagnoseprotokollierung für ihr Azure Data Lake Analytics-Konto aktivieren, um Überwachungspfade für den Datenzugriff zu erfassen. Diese Protokolle enthalten beispielsweise folgende Informationen:

* Liste der Benutzer, die auf die Daten zugegriffen haben
* Häufigkeit des Datenzugriffs
* Menge der im Konto gespeicherten Daten

## Voraussetzungen

- **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
- **Aktivieren Sie Ihr Azure-Abonnement** für die öffentliche Vorschauversion von Data Lake Analytics. Weitere Informationen finden Sie in den [Anweisungen](data-lake-analytics-get-started-portal.md#signup).
- **Azure Data Lake Analytics-Konto**. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md) aus.

## Aktivieren der Protokollierung

1. Melden Sie sich am neuen [Azure-Portal](https://portal.azure.com) an.

2. Öffnen Sie Ihr Data Lake Analytics-Konto, und klicken Sie auf dem Data Lake Analytics-Kontoblatt auf **Einstellungen** und dann auf **Diagnoseeinstellungen**.

3. Nehmen Sie auf dem Blatt **Diagnose** die folgenden Änderungen vor, um die Diagnoseprotokollierung zu konfigurieren.

	![Aktivieren der Diagnoseprotokollierung](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "Aktivieren von Diagnoseprotokollen")

	* Legen Sie **Status** auf **Ein** fest, um die Diagnoseprotokollierung zu aktivieren.
	* Sie können die Daten auf zwei verschiedene Arten speichern/verarbeiten.
		* Wählen Sie die Option **In Event Hubs exportieren**, um die Protokolldaten an einen Azure Event Hub zu streamen. Verwenden Sie diese Option, wenn Sie eine Downstream-Verarbeitungspipeline einsetzen, um eingehende Protokolle in Echtzeit zu analysieren. Wenn Sie diese Option auswählen, müssen Sie die Details für den Azure Event Hub angeben, den Sie verwenden möchten.
		* Wählen Sie die Option **In Speicherkonto exportieren**, um Protokolle in einem Azure Storage-Konto zu speichern. Verwenden Sie diese Option, wenn Sie die Daten archivieren möchten. Bei Auswahl dieser Option müssen Sie ein Azure Storage-Konto zum Speichern der Protokolle bereitstellen.
	* Geben Sie an, ob Sie Überwachungsprotokolle oder Anforderungsprotokolle oder beides abrufen möchten.
	* Geben Sie die Anzahl der Tage an, für die die Daten beibehalten werden müssen.
	* Klicken Sie auf **Speichern**.

Nachdem Sie die Diagnoseeinstellungen aktiviert haben, können Sie die Protokolle auf der Registerkarte **Diagnoseprotokolle** verfolgen.

## Anzeigen von Protokollen

Es gibt zwei Möglichkeiten, die Protokolldaten Ihres Data Lake Analytics-Kontos anzuzeigen:

* Über die Einstellungen des Data Lake Analytics-Kontos
* Über das Azure Storage-Konto, in dem die Daten gespeichert sind

### Verwenden der Ansicht mit den Data Lake Analytics-Einstellungen

1. Klicken Sie in Ihrem Data Lake Analytics-Konto auf dem Blatt **Einstellungen** auf **Diagnoseprotokolle**.

	![Anzeigen der Diagnoseprotokollierung](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "Anzeigen der Diagnoseprotokolle")

2. Auf dem Blatt **Diagnoseprotokolle** sollten die Protokolle nach **Überwachungsprotokollen** und **Anforderungsprotokollen** kategorisiert sein.
	* Anforderungsprotokolle erfassen jede API-Anforderung im Data Lake Analytics-Konto.
	* Überwachungsprotokolle ähneln Anforderungsprotokollen, bieten aber eine viel detailliertere Aufschlüsselung der Vorgänge, die auf dem Data Lake Analytics-Konto ausgeführt werden. Ein einzelner API-Hochladeaufruf in Anforderungsprotokollen könnte möglicherweise in den Überwachungsprotokollen in mehreren „Anfügen“-Vorgängen resultieren.

3. Klicken Sie für einen Protokolleintrag auf den Link **Herunterladen**, um die Protokolle herunterzuladen.

### Im Azure Storage-Konto, das die Protokolldaten enthält

1. Öffnen Sie das Azure Storage-Kontoblatt, das Data Lake Analytics zur Protokollierung zugeordnet ist, und klicken Sie dann auf „Blobs“. Auf dem Blatt **Blob-Dienst** werden zwei Container aufgelistet.

	![Anzeigen der Diagnoseprotokollierung](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "Anzeigen der Diagnoseprotokolle")

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
    
    > [AZURE.NOTE] Die `##`-Einträge im Pfad enthalten Jahr, Monat, Tag und Stunde der Protokollerstellung. Data Lake Analytics erstellt eine Datei pro Stunde, sodass `m=` immer den Wert `00` enthält.

	Der vollständige Pfad zu einem Überwachungsprotokoll kann beispielsweise wie folgt lauten:
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

	Entsprechend kann der vollständige Pfad zu einem Anforderungsprotokoll wie folgt lauten:
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## Protokollstruktur

Die Überwachungs- und Anforderungsprotokolle liegen im JSON-Format vor. In diesem Abschnitt betrachten wir die Struktur von JSON für Anforderungs- und Überwachungsprotokolle.

### Anforderungsprotokolle

Hier ist ein Beispiel für einen Eintrag im JSON-formatierten Anforderungsprotokoll. Jedes Blob hat ein Stammobjekt namens **records**, das ein Array mit Protokollobjekten enthält.

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

#### Anforderungsprotokollschema

| Name | Typ | Beschreibung |
|-----------------|--------|--------------------------------------------------------------------------------|
| in | String | Der Zeitstempel (UTC) des Protokolls. |
| Ressourcen-ID | String | Die ID der Ressource, auf der der Vorgang stattfand. |
| category | String | Die Protokollkategorie. Beispiel: **Anforderungen**. |
| operationName | String | Der Name des protokollierten Vorgangs. Beispiel: GetAggregatedJobHistory. |
| resultType | String | Der Status des Vorgangs, beispielsweise 200. |
| callerIpAddress | String | Die IP-Adresse des Clients, der die Anforderung gestellt hat. |
| correlationId | String | Die ID des Protokolls. Dieser Wert kann verwendet werden, um einen Satz von zusammengehörigen Protokolleinträgen zu gruppieren. |
| Identität | Objekt | Die Identität, die das Protokoll erstellt hat. |
| Eigenschaften | JSON | Details hierzu finden Sie im nächsten Abschnitt (Eigenschaftenschema des Anforderungsprotokolls). |

#### Eigenschaftenschema des Anforderungsprotokolls

| Name | Typ | Beschreibung |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod | String | Die HTTP-Methode, die für den Vorgang verwendet werden. Beispiel: GET. |
| Pfad | String | Der Pfad, in dem der Vorgang durchgeführt wurde. |
| RequestContentLength | int | Die Inhaltslänge der HTTP-Anforderung. |
| ClientRequestId | String | Die ID, die diese Anforderung eindeutig identifiziert. |
| StartTime | String | Der Zeitpunkt, zu dem der Server die Anforderung empfangen hat. |
| EndTime | String | Der Zeitpunkt, zu dem der Server eine Antwort gesendet hat. |

### Überwachungsprotokolle

Hier ist ein Beispiel für einen Eintrag im JSON-formatierten Überwachungsprotokoll. Jeder Blob hat ein Stammobjekt namens **records**, das ein Array von Protokollobjekten enthält.

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

#### Überwachungsprotokollschema

| Name | Typ | Beschreibung |
|-----------------|--------|--------------------------------------------------------------------------------|
| in | String | Der Zeitstempel (UTC) des Protokolls. |
| Ressourcen-ID | String | Die ID der Ressource, auf der der Vorgang stattfand. |
| category | String | Die Protokollkategorie. Beispiel: **Überwachung**. |
| operationName | String | Der Name des protokollierten Vorgangs. Beispiel: JobSubmitted. |
| resultType | String | Ein Unterstatus für den Auftragsstatus (operationName). |
| resultSignature | String | Weitere Informationen zum Auftragsstatus (operationName). |
| Identität | String | Der Benutzer, der den Vorgang angefordert hat. Beispiel: susan@contoso.com. |
| Eigenschaften | JSON | Details hierzu finden Sie im nächsten Abschnitt (Eigenschaftenschema des Überwachungsprotokolls). |

> [AZURE.NOTE] Die Elemente __resultType__ und __resultSignature__ liefern Informationen zum Ergebnis eines Vorgangs und enthalten nur einen Wert, wenn ein Vorgang abgeschlossen wurde. Beispielsweise ist ein Wert vorhanden, wenn __operationName__ den Wert __JobStarted__ oder __JobEnded__ enthält.

#### Eigenschaftenschema des Überwachungsprotokolls

| Name | Typ | Beschreibung |
|------------|--------|------------------------------------------|
| JobId | String | Die ID, die dem Auftrag zugewiesen ist. |
| JobName | String | Der Name, der für den Auftrag angegeben wurde. |
| JobRunTime | String | Die zum Verarbeiten des Auftrags verwendete Laufzeit. |
| SubmitTime | String | Der Zeitpunkt (UTC), zu dem der Auftrag übermittelt wurde. |
| StartTime | String | Der Zeitpunkt (UTC), zu dem der Auftrag nach der Übermittlung gestartet wurde. |
| EndTime | String | Der Zeitpunkt, zu dem der Auftrag beendet wurde. |
| Parallelität | String | Die Anzahl von Data Lake Analytics-Einheiten, die für diesen Auftrag während der Übermittlung angefordert wurden. |

> [AZURE.NOTE] Die Elemente __SubmitTime__, __StartTime__, __EndTime__ und __Parallelism__ liefern Informationen zu einem Vorgang und enthalten nur dann einen Wert, wenn ein Vorgang gestartet oder abgeschlossen wurde. Beispiel: __SubmitTime__ enthält einen Wert nach __operationName__ mit dem Hinweis __JobSubmitted__.

## Verarbeitung der Protokolldaten

Azure Data Lake Analytics stellt ein Muster bereit, nach dem die Protokolldaten verarbeitet und analysiert werden sollen. Sie finden das Beispiel hier: [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).


## Nächste Schritte

- [Azure Data Lake Analytics – Übersicht](data-lake-analytics-overview.md)

<!---HONumber=AcomDC_0914_2016-->