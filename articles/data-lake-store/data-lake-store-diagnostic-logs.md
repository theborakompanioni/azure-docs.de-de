<properties 
   pageTitle="Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store | Microsoft Azure" 
   description="Grundlegendes zum Einrichten von Diagnoseprotokollen und Zugriff darauf für Azure Data Lake Store " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="07/19/2016"
   ms.author="nitinme"/>

# Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store

Erfahren Sie, wie Sie die Diagnoseprotokollierung für Ihr Data Lake Store-Konto aktivieren, und wie Sie die für Ihr Konto erfassten Protokolle anzeigen.

Organisationen können die Diagnoseprotokollierung für ihre Azure Data Lake Store-Konten zum Erfassen von Datenzugriffs-Überwachungspfaden aktivieren, die Informationen wie die Liste der Benutzer, die auf die Daten zugreifen, die Häufigkeit des Zugriffs auf die Daten, die Größe der im Konto gespeicherten Datenmenge usw. liefern.

## Voraussetzungen

- **Ein Azure-Abonnement**. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).
- **Aktivieren Sie Ihr Azure-Abonnement** für die öffentliche Vorschauversion von Data Lake Store. Weitere Informationen finden Sie in den [Anweisungen](data-lake-store-get-started-portal.md#signup).
- **Azure Data Lake-Speicherkonto**. Führen Sie die Schritte der Anleitung unter [Erste Schritte mit dem Azure Data Lake-Speicher mithilfe des Azure-Portals](data-lake-store-get-started-portal.md) aus.

## Aktivieren der Diagnoseprotokollierung für Ihr Data Lake Store-Konto

1. Melden Sie sich beim neuen [Azure-Portal](https://portal.azure.com) an.

2. Öffnen Sie Ihr Data Lake Store-Konto, und klicken Sie auf Ihrem Data Lake Store-Kontoblatt auf **Einstellungen** und dann auf **Diagnoseeinstellungen**.

3. Nehmen Sie auf dem Blatt **Diagnose**die folgenden Änderungen vor, um die Diagnoseprotokollierung zu konfigurieren.

	![Aktivieren der Diagnoseprotokollierung](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "Aktivieren von Diagnoseprotokollen")

	* Legen Sie **Status** auf **Ein** fest, um die Diagnoseprotokollierung zu aktivieren.
	* Sie können die Daten auf zwei verschiedene Arten speichern/verarbeiten.
		* Wählen Sie die Option **Nach Event Hub Exportieren**, um die Protokolldaten an einen Azure Event Hub zu streamen. Wahrscheinlich werden Sie diese Option verwenden, wenn Sie eine Downstreamverarbeitungs-Pipeline einsetzen, um eingehende Protokolle in Echtzeit zu analysieren. Wenn Sie diese Option auswählen, müssen Sie die Details für den Azure Event Hub angeben, den Sie verwenden möchten.
		* Wählen Sie die Option **Nach Speicherkonto exportieren**, um Protokolle in einem Azure Storage-Konto zu speichern. Verwenden Sie diese Option, wenn Sie die Daten archivieren möchten, die zu einem späteren Zeitpunkt der Batchverarbeitung unterzogen werden sollen. Bei Auswahl dieser Option müssen Sie ein Azure Storage-Konto zum Speichern der Protokolle bereitstellen.
	* Geben Sie an, ob Sie Überwachungsprotokolle oder Anforderungsprotokolle oder beides abrufen möchten.
	* Geben Sie die Anzahl der Tage an, für die die Daten beibehalten werden müssen.
	* Klicken Sie auf **Speichern**.

Nachdem Sie die Diagnoseeinstellungen aktiviert haben, können Sie die Protokolle auf der Registerkarte **Diagnoseprotokolle** verfolgen.

## Anzeigen der Diagnoseprotokolle für Ihr Data Lake Store-Konto

Es gibt zwei Möglichkeiten, die Protokolldaten Ihres Data Lake Store-Kontos anzuzeigen:

* Über die Ansicht „Einstellungen“ des Data Lake Store-Kontos
* Über das Azure Storage-Konto, in dem die Daten gespeichert sind

### Verwenden der Data Lake Store-Ansicht „Einstellungen“

1. Klicken Sie in Ihrem Data Lake Store-Konto auf dem Blatt **Einstellungen** auf **Diagnoseprotokolle**.

	![Anzeigen der Diagnoseprotokollierung](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "Anzeigen der Diagnoseprotokolle")

2. Auf dem Blatt **Diagnoseprotokolle** sollten die Protokolle nach **Überwachungsprotokollen** und **Anforderungsprotokollen** kategorisiert sein.
	* Anforderungsprotokolle erfassen jede API-Anforderung im Data Lake Store-Konto.
	* Überwachungsprotokolle ähneln Anforderungsprotokollen, bieten aber eine viel detailliertere Aufschlüsselung der Vorgänge, die auf dem Data Lake Store-Konto ausgeführt werden. Ein einzelner API-Hochladeaufruf in Anforderungsprotokollen könnte möglicherweise in den Überwachungsprotokollen in mehreren „Anfügen“-Vorgängen resultieren.

3. Klicken Sie für jeden Protokolleintrag auf den Link **Herunterladen**, um die Protokolle herunterzuladen.

### Im Azure Storage-Konto, das die Protokolldaten enthält

1. Öffnen Sie das Azure Storage-Kontoblatt, das zur Protokollierung dem Data Lake Store zugeordnet ist, und klicken Sie dann auf „Blobs“. Auf dem Blatt **Blobdienst** werden zwei Container aufgelistet.

	![Anzeigen der Diagnoseprotokollierung](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "Anzeigen der Diagnoseprotokolle")

	* Der Container **insights-logs-audit** enthält die Überwachungsprotokolle.
	* Der Container **insights-logs-requests** enthält die Anforderungsprotokolle.

2. Innerhalb dieser Container werden die Protokolle in der folgenden Struktur gespeichert.

	![Anzeigen der Diagnoseprotokollierung](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "Anzeigen der Diagnoseprotokolle")

	Der vollständige Pfad zu einem Überwachungsprotokoll könnte z.B. folgendermaßen lauten: `https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

	Entsprechend könnte der vollständige Pfad zu einem Anforderungsprotokoll folgendermaßen lauten: `https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## Grundlegendes zur Struktur der Protokolldaten

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
			 "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
			 "category": "Requests",
			 "operationName": "GETCustomerIngressEgress",
			 "resultType": "200",
			 "callerIpAddress": "::ffff:1.1.1.1",
			 "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
			 "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
			 "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
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
| operationName | String | Der Name des protokollierten Vorgangs. Beispielsweise „getfilestatus“. |
| resultType | String | Der Status des Vorgangs, beispielsweise 200. |
| callerIpAddress | String | Die IP-Adresse des Clients, der die Anforderung gestellt hat. |
| correlationId | String | Die ID des Protokolls, die verwendet werden kann, um einen Satz verbundener Protokolleinträge zu gruppieren. |
| Identität | Objekt | Die Identität, die das Protokoll erstellt hat. |
| Eigenschaften | JSON | Weitere Informationen siehe unten. |

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
			 "time": "2016-07-08T19:08:59.359Z",
			 "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
			 "category": "Audit",
			 "operationName": "SeOpenStream",
			 "resultType": "0",
			 "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
			 "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
			 "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
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
| operationName | String | Der Name des protokollierten Vorgangs. Beispielsweise „getfilestatus“. |
| resultType | String | Der Status des Vorgangs, beispielsweise 200. |
| correlationId | String | Die ID des Protokolls, die verwendet werden kann, um einen Satz verbundener Protokolleinträge zu gruppieren. |
| Identität | Objekt | Die Identität, die das Protokoll erstellt hat. |
| Eigenschaften | JSON | Weitere Informationen siehe unten. |

#### Eigenschaftenschema des Überwachungsprotokolls

| Name | Typ | Beschreibung |
|------------|--------|------------------------------------------|
| StreamName | String | Der Pfad, in dem der Vorgang durchgeführt wurde. |


## Beispiele für die Verarbeitung der Protokolldaten

Azure Data Lake Store stellt ein Muster bereit, nach dem die Protokolldaten verarbeitet und analysiert werden sollen. Sie finden das Beispiel hier: [https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample).


## Siehe auch

- [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)
- [Sichern von Daten in Data Lake-Speicher](data-lake-store-secure-data.md)

<!---HONumber=AcomDC_0914_2016-->