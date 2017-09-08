---
title: Reaktion auf Azure Blob Storage-Ereignisse (Vorschau) | Microsoft-Dokumentation
description: Abonnieren Sie Blob Storage-Ereignisse mit Azure Event Grid.
services: storage,event-grid
keywords: 
author: cbrooksmsft
ms.author: cbrooks
ms.date: 08/25/2017
ms.topic: article
ms.service: storage
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: b9b117bdeb62f5ebb2e4e3fbfe71572068927082
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---

# <a name="reacting-to-blob-storage-events-preview"></a>Reaktion auf Blob Storage-Ereignisse (Vorschau)

Azure Blob Storage-Ereignisse ermöglichen Anwendungen, auf das Erstellen und Löschen von Blobs mit modernen serverlosen Architekturen und ohne komplizierten Code oder teure und ineffiziente Abrufdienste zu reagieren.  Stattdessen werden die Ereignisse per Push über [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) an Abonnenten wie [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/) oder sogar Ihren eigenen benutzerdefinierten HTTP-Listener übertragen, und Sie zahlen nur für das, was Sie tatsächliche verwenden.

Gängige Blob Storage-Ereignisszenarien enthalten Bild- oder Videobearbeitung, Suchindizierung oder dateiorientierte Workflows.  Asynchrone Dateiuploads eignen sich hervorragend für Ereignisse.  Wenn Änderungen selten sind, aber Ihr Szenario die sofortige Reaktion erfordert, kann die ereignisbasierte Architektur besonders effizient sein.

![Event Grid-Modell](./media/storage-blob-event-overview/event-grid-functional-model.png)

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="join-the-preview"></a>Nehmen Sie an der Vorschau teil
Blob Storage-Ereignisse sind für die Vorschau verfügbar.  Benutzer können die Teilnahme an der Vorschau anfordern, indem sie die folgenden Befehle für ihr Abonnement eingeben:
```azurecli-interactive
az provider register --namespace  Microsoft.EventGrid
az feature register --name storageEventSubscriptions --namespace Microsoft.EventGrid
```
Abonnements werden dem Vorschauprogramm im Rahmen der verfügbaren Kapazität hinzugefügt.  Den Anforderungsstatus können Sie mit folgendem Befehl überwachen:
```azurecli-interactive
az feature show --name storageEventSubscriptions --namespace Microsoft.EventGrid
```
Sobald Ihr Registrierungsstatus sich in „Registriert“ ändert, sind Sie zur Teilnahme am Vorschauprogramm zugelassen und können Blob Storage-Ereignisse für Konten in der Region *USA, Westen-Mitte* abonnieren.  Unter [Weiterleiten von Blob Storage-Ereignissen an einen benutzerdefinierten Webendpunkt](storage-blob-event-quickstart.md) finden Sie ein kurzes Beispiel.

## <a name="blob-storage-accounts"></a>Blob Storage-Konten
Blob Storage-Ereignisse stehen in [BLOB-Speicherkonten](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) (und nicht in allgemeinen Speicherkonten) zur Verfügung.  BLOB-Speicherkonten sind spezielle Speicherkonten und dienen dazu, unstrukturierte Daten als Blobs (Objekte) in Azure Storage zu speichern. BLOB-Speicherkonten sind wie allgemeine Speicherkonten und besitzen die gleichen Haltbarkeits-, Verfügbarkeits-, Skalierbarkeits- und Leistungseigenschaften, die Sie schon heute verwenden – einschließlich vollständiger API-Konsistenz für Blockblobs und Anfügeblobs. Bei Anwendungen, die nur Block- oder Anfügeblobspeicher benötigen, empfiehlt sich die Verwendung von BLOB-Speicherkonten.

## <a name="available-blob-storage-events"></a>Verfügbare Blob Storage-Ereignisse
Event Grid verwendet [Ereignisabonnements](../../event-grid/concepts.md#event-subscriptions) zum Weiterleiten von Ereignisnachrichten an Abonnenten.  Blob Storage-Ereignisabonnements können zwei Arten von Ereignissen enthalten:  

> |Veranstaltungsname|Beschreibung|
> |----------|-----------|
> |`Microsoft.Storage.BlobCreated`|Wird ausgelöst, wenn ein Blob erstellt oder durch die Vorgänge `PutBlob`, `PutBlockList` oder `CopyBlob` ersetzt wird.|
> |`Microsoft.Storage.BlobDeleted`|Wird ausgelöst, wenn ein Blob, durch den Vorgang `DeleteBlob` gelöscht wird.|

## <a name="event-schema"></a>Ereignisschema
Blob Storage-Ereignisse enthalten alle Informationen, die Sie für die Reaktion auf Änderungen in Ihren Daten benötigen.  Sie können ein Blob Storage-Ereignis identifizieren, da die eventType-Eigenschaft mit „Microsoft.Storage“ beginnt.  
Weitere Informationen zur Verwendung von Event Grid-Ereigniseigenschaften finden Sie unter [Event Grid-Ereignisschema](../../event-grid/event-schema.md).  

> |Eigenschaft|Typ|Beschreibung|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |Thema|string|Vollständige Azure Resource Manager-ID des Speicherkontos, das das Ereignis ausgibt.|
> |subject|string|Der relative Ressourcenpfad zu dem Objekt, das der Betreff des Ereignisses ist, in dem gleichen erweiterten Azure Resource Manager-Format, mit dem Speicherkonten, Dienste und Container für RBAC von Azure beschrieben werden.  Dieses Format schließt einen Blobnamen ein, bei dem die Groß-/Kleinschreibung beibehalten wird.|
> |eventTime|string|Datum und Uhrzeit der Ereignisgenerierung im ISO 8601-Format.|
> |eventType|string|„Microsoft.Storage.BlobCreated“ oder „Microsoft.Storage.BlobDeleted“|
> |ID|string|Eindeutiger Bezeichner dieses Ereignisses|
> |data|objekt|Sammlung Blob Storage-spezifischer Ereignisdaten|
> |data.contentType|string|Der Inhaltstyp des Blobs, wie er im Content-Type-Header aus dem Blob zurückgegeben werden würde.|
> |data.contentLength|number|Die Größe des Blobs im Ganzzahlformat, eine Anzahl von Bytes darstellend, wie sie im Content-Length-Header aus dem Blob zurückgegeben werden würde.  Gesendet mit BlobCreated-Ereignis, jedoch nicht mit BlobDeleted.|
> |data.url|string|Die URL des Objekts, das der Betreff des Ereignisses ist.|
> |data.eTag|string|Das ETag des Objekts, wenn dieses Ereignis ausgelöst wird.  Für das Ereignis BlobDeleted nicht verfügbar.|
> |data.api|string|Der Name des API-Vorgangs, der dieses Ereignis ausgelöst hat.  Für BlobCreated-Ereignisse ist dieser Wert „PutBlob“, „PutBlockList“ oder „Copy“.  Für BlobDeleted-Ereignisse ist dieser Wert „DeleteBlob“.  Diese Werte sind die gleichen API-Namen, die in den Azure Storage-Diagnoseprotokollen vorhanden sind.  Informationen finden Sie unter [Protokollierte Speicheranalysevorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).|
> |data.sequencer|string|Ein nicht transparenter Zeichenfolgenwert, der die logische Reihenfolge von Ereignissen für einen bestimmten Blobnamen darstellt.  Benutzer können anhand des standardmäßigen Zeichenfolgenvergleichs die relative Reihenfolge von zwei Ereignissen unter dem gleichen Blobnamen verstehen.|
> |data.requestId|string|Dienstgenerierte Anforderungs-ID für den Speicher-API-Vorgang.  Kann verwendet werden, um Azure Storage-Diagnoseprotokolle mithilfe des Felds „request-id-header“ in den Protokollen zu korrelieren, und wird vom einleitenden API-Aufruf im „x-ms-request-id“-Header zurückgegeben. Informationen finden Sie unter [Storage Analytics Log Format](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format) (Storage Analytics-Protokollformat).|
> |data.clientRequestId|string|Vom Client bereitgestellte Anforderungs-ID für den Speicher-API-Vorgang.  Kann zum Korrelieren mit Azure Storage-Diagnoseprotokollen mithilfe des Felds „client-request-id“ in den Protokollen verwendet und in Clientanforderungen mit dem „x-ms-client-request-id“-Header bereitgestellt werden. Informationen finden Sie unter [Storage Analytics Log Format](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format) (Storage Analytics-Protokollformat).|
> |data.storageDiagnostics|objekt|Diagnosedaten, die gelegentlich vom Azure Storage-Dienst einbezogen werden.  Falls vorhanden, sollten sie vom Ereignisconsumer ignoriert werden.|

Hier sehen Sie ein Beispiel für ein BlobCreated-Ereignis:
```json
[{
  "topic": "/subscriptions/319a9601-1ec0-0000-aebc-8fe82724c81e/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file1.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T01:57:26.005121Z",
  "id": "602a88ef-0001-00e6-1233-1646070610ea",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "799304a4-bbc5-45b6-9849-ec2c66be800a",
    "requestId": "602a88ef-0001-00e6-1233-164607000000",
    "eTag": "0x8D4E44A24ABE7F1",
    "contentType": "text/plain",
    "contentLength": 447,
    "blobType": "BlockBlob",
    "url": "https://myaccount.blob.core.windows.net/testcontainer/file1.txt",
    "sequencer": "00000000000000EB000000000000C65A",
  }
}]

```

Weitere Informationen finden Sie unter [Azure Blob Storage](../../event-grid/event-schema.md#azure-blob-storage).

## <a name="filtering-events"></a>Filtern von Ereignissen
Blobereignisabonnements können basierend auf dem Ereignistyp sowie auf dem Container- und Blobnamen des Objekts, das erstellt oder gelöscht wurde, gefiltert werden.  Betrefffilter funktionieren in Event Grid auf Grundlage von „beginnt mit“- und „endet mit“-Übereinstimmungen, sodass Ereignisse mit einem übereinstimmenden Betreff an den Abonnenten übermittelt werden.
Der Betreff von Blob Storage-Ereignissen verwendet das Format:
```json
/blobServices/default/containers/<containername>/blobs/<blobname>
```
Zur Übereinstimmung mit allen Ereignissen für ein Speicherkonto können Sie den Betrefffilter leer lassen.

Verwenden Sie zur Übereinstimmung mit Ereignissen aus Blobs, die in einem Satz von Containern erstellt wurden und ein gemeinsames Präfix verwenden, einen `subjectBeginsWith`-Filter wie:
```json
/blobServices/default/containers/containerprefix
```
Verwenden Sie zur Übereinstimmung mit Ereignissen aus Blobs, die in einem bestimmten Container erstellt wurden, einen `subjectBeginsWith`-Filter wie:
```json
/blobServices/default/containers/containername/
```
Verwenden Sie zur Übereinstimmung mit Ereignissen aus Blobs, die in einem bestimmten Container erstellt wurden und ein gemeinsames Blobnamenspräfix verwenden, einen `subjectBeginsWith`-Filter wie:
```json
/blobServices/default/containers/containername/blobs/blobprefix
```

Verwenden Sie zur Übereinstimmung mit Ereignissen aus Blobs, die in einem bestimmten Container erstellt wurden und ein gemeinsames Blobsuffix verwenden, einen `subjectEndsWith`-Filter wie „.log“ oder „.jpg“.

Weitere Informationen finden Sie unter [Begriffe in Azure Event Grid](../../event-grid/concepts.md#filters).

## <a name="practices-for-consuming-events"></a>Methoden zum Nutzen von Ereignissen
Anwendungen, die Blob Storage-Ereignisse behandeln, sollten einige bewährte Methoden nutzen:
> [!div class="checklist"]
> * Da mehrere Abonnements zum Weiterleiten von Ereignissen an den gleichen Ereignishandler konfiguriert werden können, ist es wichtig, nicht davon auszugehen, dass Ereignisse aus einer bestimmten Quelle stammen, sondern das Thema der Nachricht zu überprüfen, um sicherzustellen, dass es aus dem Speicherkonto stammt, das Sie erwarten.
> * Überprüfen Sie auf ähnliche Weise, ob Sie auf die Verarbeitung des eventType vorbereitet sind, und gehen Sie nicht davon aus, dass alle Ereignisse, die Sie empfangen, den von Ihnen erwarteten Typen entsprechen.
> * Da Nachrichten in falscher Reihenfolge und mit Verzögerung eintreffen können, verwenden Sie die etag-Felder, um zu verstehen, ob Ihre Informationen zu Objekten weiterhin auf dem neuesten Stand ist.  Verwenden Sie auch die sequencer-Felder, um die Reihenfolge der Ereignisse für ein bestimmtes Objekt zu verstehen.
> * Verwenden Sie das blobType-Feld, um zu verstehen, welche Arten von Vorgängen für das Blob zulässig sind, und welche Typen von Clientbibliotheken Sie für den Zugriff auf das Blob verwenden sollten.
> * Verwenden Sie das url-Feld mit `CloudBlockBlob`- und `CloudAppendBlob`-Konstruktor für den Zugriff auf das Blob.
> * Ignorieren Sie Felder, die Sie nicht verstehen.  So müssen Sie sich nicht mit neuen Features auseinandersetzen, die in der Zukunft hinzugefügt werden könnten.


## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über Event Grid , und probieren Sie Blob Storage-Ereignisse aus:

- [An introduction to Azure Event Grid](../../event-grid/overview.md) (Einführung in Azure Event Grid)
- [Weiterleiten von Blob Storage-Ereignissen an einen benutzerdefinierten Webendpunkt](storage-blob-event-quickstart.md)
