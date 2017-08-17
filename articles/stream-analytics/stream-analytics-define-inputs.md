---
title: 'Datenverbindung: Datenstromeingaben aus einem Ereignisstrom | Microsoft Docs'
description: "Erfahren Sie, wie eine „Eingaben“ genannte Datenverbindung mit Stream Analytics eingerichtet wird. Eingaben umfassen einen Datenstrom von Ereignissen und Verweisdaten."
keywords: Datenstrom, Datenverbindung, Ereignisstrom
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 8155823c-9dd8-4a6b-8393-34452d299b68
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/05/2017
ms.author: samacha
ms.translationtype: Human Translation
ms.sourcegitcommit: bb794ba3b78881c967f0bb8687b1f70e5dd69c71
ms.openlocfilehash: 09066927641054acb8c53a3585e111df87893e50
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="data-connection-learn-about-data-stream-inputs-from-events-to-stream-analytics"></a>Datenverbindung: Erfahren Sie mehr über Datenstromeingaben aus Ereignissen in Stream Analytics
Bei der Datenverbindung mit einem Stream Analytics-Auftrag handelt es sich um einen Datenstrom von Ereignissen aus einer Datenquelle, die als *Eingabe* des Auftrags bezeichnet wird. Stream Analytics bietet eine hervorragende Integration in Datenstromquellen von Azure wie [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/), [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) und [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/). Diese Eingabequellen können aus demselben Azure-Abonnement wie dem Ihres Analyseauftrags oder aus einem anderen Abonnement stammen.

## <a name="data-input-types-data-stream-and-reference-data"></a>Dateneingabetypen: Datenstrom und Verweisdaten
Werden Daten mithilfe von Push an eine Datenquelle übertragen, werden sie vom Stream Analytics-Auftrag genutzt und in Echtzeit verarbeitet. Eingaben werden in zwei Typen unterteilt: Datenstromeingaben und Verweisdateneingaben.

### <a name="data-stream-inputs"></a>Datenstromeingaben
Ein Datenstrom ist eine ungebundene Abfolge von Ereignissen im Verlauf der Zeit. Stream Analytics-Aufträge müssen mindestens eine Datenstromeingabe enthalten. Event Hubs, IoT Hubs und Blob Storage werden als Datenstrom-Eingabequellen unterstützt. Event Hubs werden verwendet, um Ereignisdatenströme von verschiedenen Geräten und Diensten zu erfassen. Bei diesen Datenströmen kann es sich beispielsweise um Aktivitätsfeeds sozialer Medien, Börseninformationen oder Daten von Sensoren handeln. IoT Hubs sind für die Sammlung von Daten von verbundenen Geräten in IoT-Szenarien (Internet der Dinge) optimiert.  Blob Storage kann als Eingabequelle für die Erfassung von Massendaten als Datenstrom (z.B. Protokolldateien) verwendet werden.  

### <a name="reference-data"></a>Verweisdaten
Stream Analytics unterstützt auch Eingaben wie *Verweisdaten*. Dies sind Hilfsdaten, die entweder statisch sind oder sich langsam ändern. Sie werden normalerweise für die Durchführung von Korrelationen und Suchvorgängen verwendet. Beispielsweise können Sie Daten in der Datenstromeingabe mit den Daten in den Verweisdaten verknüpfen – ähnlich wie bei einer SQL-Verknüpfung zum Suchen statischer Werte. Azure-BLOB-Speicher ist derzeit die einzige unterstützte Eingabequelle für Verweisdaten. Blobs für Verweisdatenquellen sind auf eine Größe von 100 MB beschränkt.

Informationen zum Erstellen von Verweisdateneingaben finden Sie unter [Verwenden von Verweisdaten](stream-analytics-use-reference-data.md).  

## <a name="create-data-stream-input-from-event-hubs"></a>Erstellen einer Datenstromeingabe aus Event Hubs

Azure Event Hubs sind hoch skalierbare Ereigniserfasser zum Veröffentlichen/Abonnieren. Ein Event Hub kann Millionen von Ereignissen pro Sekunde erfassen. Auf diese Weise können Sie riesige Datenmengen verarbeiten und analysieren, die von vernetzten Geräten und Anwendungen erzeugt werden. Event Hubs und Stream Analytics bieten Ihnen in Kombination eine Komplettlösung für Echtzeitanalysen. So können Sie mit Event Hubs Ereignisse in Azure in Echtzeit übertragen, die von Stream Analytics-Aufträgen in Echtzeit verarbeitet werden können. Beispielsweise können Sie Webklicks, Sensormesswerte oder Onlineprotokollereignisse an Event Hubs senden. Anschließend können Sie Stream Analytics-Aufträge erstellen, um Event Hubs als Eingabedatenströme zum Filtern, Aggregieren und Korrelieren in Echtzeit zu verwenden.

Der Standardzeitstempel von Ereignissen, die von Event Hubs in Stream Analytics stammen, ist der Zeitstempel, an dem das Ereignis im Event Hub eingeht, also `EventEnqueuedUtcTime`. Zum Verarbeiten der Daten als Datenstrom mit einem Zeitstempel in der Ereignisnutzlast müssen Sie das Schlüsselwort [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwenden.

### <a name="consumer-groups"></a>Verbrauchergruppen
Sie sollten für jede Event Hub-Eingabe in Stream Analytics eine eigene Consumergruppe konfigurieren. Wenn ein Auftrag eine Selbstverknüpfung oder mehrere Eingaben enthält, können einige Eingaben möglicherweise von mehreren nachgeschalteten Lesern gelesen werden. Dies wirkt sich auf die Anzahl der Leser in einer einzelnen Consumergruppe aus. Zur Vermeidung der Überschreitung des Event Hub-Limits von fünf Lesern pro Consumergruppe pro Partition empfiehlt es sich, eine Consumergruppe für jeden Stream Analytics-Auftrag anzugeben. Darüber hinaus gilt ein Grenzwert von 20 Consumergruppen pro Event Hub. Weitere Informationen finden Sie im [Programmierleitfaden zu Event Hubs](../event-hubs/event-hubs-programming-guide.md).

### <a name="configure-an-event-hub-as-a-data-stream-input"></a>Konfigurieren von Event Hubs als Datenstromeingabe
In der folgenden Tabelle wird jede Eigenschaft im Azure-Portal auf dem Blatt **Neue Eingabe** erläutert, wenn Sie einen Event Hub als Eingabe konfigurieren.

| Eigenschaft | Beschreibung |
| --- | --- |
| **Eingabealias** |Ein Anzeigename, der in der Auftragsabfrage verwendet wird, um auf diese Eingabe zu verweisen. |
| **Service Bus-Namespace** |Ein Azure Service Bus-Namespace, der ein Container für einen Satz von Messagingentitäten ist. Sie haben bei der Erstellung einer neuen Event Hub-Instanz auch einen Service Bus-Namespace erstellt. |
| **Event Hub-Name** |Der Name des Event Hubs, der als Eingabe verwendet wird. |
| **Event Hub-Richtlinienname** |Die SAS-Richtlinie, die Zugriff auf den Event Hub ermöglicht. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| **Event Hub-Consumergruppe** (optional) |Die Consumergruppe, die zum Erfassen von Daten aus dem Event Hub verwendet werden soll. Wenn keine Consumergruppe angegeben wird, verwendet der Stream Analytics-Auftrag die Standardconsumergruppe an. Es wird empfohlen, für jeden Stream Analytics-Auftrag eine eigene Consumergruppe zu verwenden. |
| **Ereignisserialisierungsformat** |Das Serialisierungsformat (JSON, CSV oder Avro) des eingehenden Datenstroms. |
| **Codieren** | UTF-8 ist derzeit das einzige unterstützte Codierungsformat. |

Wenn Ihre Daten aus einem Event Hub stammen, haben Sie Zugriff auf folgende Metadatenfelder in Ihrer Stream Analytics-Abfrage:

| Eigenschaft | Beschreibung |
| --- | --- |
| **EventProcessedUtcTime** |Das Datum und die Uhrzeit der Verarbeitung des Ereignisses durch Stream Analytics. |
| **EventEnqueuedUtcTime** |Das Datum und die Uhrzeit des Ereignisempfangs durch die Event Hubs. |
| **PartitionId** |Die nullbasierte Partitions-ID für den Eingabeadapter. |

Beispielsweise können Sie anhand dieser Felder eine wie im folgenden Beispiel gezeigte Abfrage schreiben:

````
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
````

## <a name="create-data-stream-input-from-iot-hub"></a>Erstellen einer Datenstromeingabe aus IoT Hub
Azure IoT Hub ist ein hochgradig skalierbares Erfassungsmodul für das Veröffentlichen und Abonnieren von Ereignissen, das für IoT-Szenarien optimiert ist.

Der Standardzeitstempel von Ereignissen, die von IoT Hub in Stream Analytics stammen, ist der Zeitstempel, an dem das Ereignis in IoT Hub eingeht, also `EventEnqueuedUtcTime`. Zum Verarbeiten der Daten als Datenstrom mit einem Zeitstempel in der Ereignisnutzlast müssen Sie das Schlüsselwort [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwenden.

> [!NOTE]
> Nur Nachrichten, die mit der `DeviceClient`-Eigenschaft gesendet wurden, können verarbeitet werden.
> 
> 

### <a name="consumer-groups"></a>Verbrauchergruppen
Sie sollten für jede IoT Hub-Eingabe in Stream Analytics eine eigene Consumergruppe konfigurieren. Wenn ein Auftrag eine Selbstverknüpfung oder mehrere Eingaben enthält, können einige Eingaben möglicherweise von mehreren nachgeschalteten Lesern gelesen werden. Dies wirkt sich auf die Anzahl der Leser in einer einzelnen Consumergruppe aus. Zur Vermeidung der Überschreitung des Azure IoT Hub-Limits von fünf Lesern pro Consumergruppe pro Partition empfiehlt es sich, eine Consumergruppe für jeden Stream Analytics-Auftrag anzugeben.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Konfigurieren von IoT Hub als Datenstromeingabe
In der folgenden Tabelle wird jede Eigenschaft im Azure-Portal auf dem Blatt **Neue Eingabe** erläutert, wenn Sie IoT Hub als Eingabe konfigurieren.

| Eigenschaft | Beschreibung |
| --- | --- |
| **Eingabealias** |Ein Anzeigename, der in der Auftragsabfrage verwendet wird, um auf diese Eingabe zu verweisen.|
| **IoT Hub** |Der Name von IoT Hub, der als Eingabe verwendet wird. |
| **Endpunkt** |Der Endpunkt für IoT Hub.|
| **Name der SAS-Richtlinie** |Die SAS-Richtlinie, die Zugriff auf IoT Hub ermöglicht. Jede SAS-Richtlinie umfasst einen Namen, die von Ihnen festgelegten Berechtigungen und Zugriffsschlüssel. |
| **Schlüssel für SAS-Richtlinie** |Der Schlüssel für den gemeinsamen Zugriff, der für die Autorisierung des Zugriffs auf IoT Hub verwendet wird. |
| **Consumergruppe** (optional) |Die Consumergruppe, die zum Erfassen von Daten aus IoT Hub verwendet werden soll. Wenn keine Consumergruppe angegeben wird, verwendet der Stream Analytics-Auftrag die Standardconsumergruppe an. Es wird empfohlen, für jeden Stream Analytics-Auftrag eine andere Consumergruppe zu verwenden. |
| **Ereignisserialisierungsformat** |Das Serialisierungsformat (JSON, CSV oder Avro) des eingehenden Datenstroms. |
| **Codieren** |UTF-8 ist derzeit das einzige unterstützte Codierungsformat. |

Wenn Ihre Daten aus IoT Hub stammen, haben Sie Zugriff auf folgende Metadatenfelder in Ihrer Stream Analytics-Abfrage:

| Eigenschaft | Beschreibung |
| --- | --- |
| **EventProcessedUtcTime** |Das Datum und die Uhrzeit der Verarbeitung des Ereignisses. |
| **EventEnqueuedUtcTime** |Das Datum und die Uhrzeit, an dem das Ereignis durch IoT Hub empfangen wurde. |
| **PartitionId** |Die nullbasierte Partitions-ID für den Eingabeadapter. |
| **IoTHub.MessageId** | Eine ID, die zum Korrelieren einer bidirektionalen Kommunikation in IoT Hub verwendet wird. |
| **IoTHub.CorrelationId** |Eine ID, die in Nachrichtenantworten und im Feedback in IoT Hub verwendet wird. |
| **IoTHub.ConnectionDeviceId** |Die Authentifizierung-ID, die zum Senden dieser Nachricht verwendet wird. Dieser Wert wird auf servicebound-Nachrichten von IoT Hub gestempelt. |
| **IoTHub.ConnectionDeviceGenerationId** |Die Generierungs-ID des authentifizierten Geräts, das zum Senden dieser Nachricht verwendet wurde. Dieser Wert wird auf servicebound-Nachrichten von IoT Hub gestempelt. |
| **IoTHub.EnqueuedTime** |Der Zeitpunkt, an dem die Nachricht durch IoT Hub empfangen wurde. |
| **IoTHub.StreamId** |Eine benutzerdefinierte Ereigniseigenschaft, die vom Absendergerät hinzugefügt wird. |


## <a name="create-data-stream-input-from-blob-storage"></a>Erstellen einer Datenstromeingabe aus Blob Storage
Für Szenarien mit großen Mengen unstrukturierter Daten, die in der Cloud gespeichert werden sollen, bietet der Azure Blob Storage eine kostengünstige und skalierbare Lösung. Daten in Blob Storage werden in der Regel als ruhende Daten angesehen. Allerdings können diese als Datenstrom von Stream Analytics verarbeitet werden. Ein typisches Szenario für Blob Storage-Eingaben bei Stream Analytics ist die Protokollverarbeitung. In diesem Szenario wurden Telemetriedaten von einem System erfasst, die zur Extraktion aussagekräftiger Daten analysiert und verarbeitet werden müssen.

Der Standardzeitstempel von Blob Storage-Ereignissen in Stream Analytics ist der Zeitstempel, an dem das Blob zuletzt geändert wurde, also `BlobLastModifiedUtcTime`. Zum Verarbeiten der Daten als Datenstrom mit einem Zeitstempel in der Ereignisnutzlast müssen Sie das Schlüsselwort [TIMESTAMP BY](https://msdn.microsoft.com/library/azure/dn834998.aspx) verwenden.

Eingaben im CSV-Format *müssen* über eine Kopfzeile verfügen, um Felder für das Dataset zu definieren. Zudem müssen alle Kopfzeilenfelder eindeutig sein.

> [!NOTE]
> Stream Analytics unterstützt das Hinzufügen von Inhalten zu einem vorhandenen Blob nicht. Stream Analytics zeigt ein Blob nur einmal an. Des Weiteren werden alle Änderungen, die im Blob vorgenommen wurden, nachdem der Auftrag die Daten gelesen hat, nicht verarbeitet. Eine bewährte Methode besteht darin, alle Daten einmal hochzuladen und dem Blob-Speicher keine weiteren Ereignisse mehr hinzuzufügen.
> 

### <a name="configure-blob-storage-as-a-data-stream-input"></a>Konfigurieren von Blob Storage als Datenstromeingabe

In der folgenden Tabelle wird jede Eigenschaft im Azure-Portal auf dem Blatt **Neue Eingabe** erläutert, wenn Sie Blob Storage als Eingabe konfigurieren.

| Eigenschaft | Beschreibung |
| --- | --- |
| **Eingabealias** | Ein Anzeigename, der in der Auftragsabfrage verwendet wird, um auf diese Eingabe zu verweisen. |
| **Speicherkonto** | Der Name des Speicherkontos an, in dem sich die Blobdateien befinden. |
| **Speicherkontoschlüssel** | Der geheime Schlüssel, der dem Speicherkonto zugeordnet ist. |
| **Container** | Der Container für die Blobeingabe. Container stellen eine logische Gruppierung für Blobs bereit, die im Microsoft Azure-Blobdienst gespeichert sind. Wenn Sie ein Blob in den Azure Blob Storage-Dienst hochladen, müssen Sie einen Container für dieses Blob angeben. |
| **Pfadmuster** (optional) | Der Dateipfad, der verwendet wird, um die Blobs im angegebenen Container zu suchen. In dem Pfad können Sie mindestens eine Instanz der folgenden drei Variablen angeben: `{date}`, `{time}` oder `{partition}`.<br/><br/>Beispiel 1: `cluster1/logs/{date}/{time}/{partition}`<br/><br/>Beispiel 2: `cluster1/logs/{date}`<br/><br/>Das Zeichen `*` ist kein zulässiger Wert für das Pfadpräfix. Es sind nur gültige <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">Azure Blob-Zeichen</a> zulässig. |
| **Datumsformat** (optional) | Wenn Sie die Datumsvariable im Pfad verwenden, wird das Datumsformat, in dem die Dateien organisiert sind, verwendet. Beispiel: `YYYY/MM/DD` |
| **Zeitformat** (optional) |  Wenn Sie die Zeitvariable im Pfad verwenden, wird das Zeitformat, in dem die Dateien organisiert sind, verwendet. Der einzige derzeit unterstützte Wert ist `HH`. |
| **Ereignisserialisierungsformat** | Das Serialisierungsformat (JSON, CSV oder Avro) für eingehende Datenströme. |
| **Codieren** | Bei CSV und JSON ist UTF-8 gegenwärtig das einzige unterstützte Codierungsformat. |

Wenn Ihre Daten aus einer Blob Storage-Quelle stammen, haben Sie Zugriff auf folgende Metadatenfelder in Ihrer Stream Analytics-Abfrage:

| Eigenschaft | Beschreibung |
| --- | --- |
| **BlobName** |Der Name des Eingabe-Blobs, aus dem das Ereignis stammt. |
| **EventProcessedUtcTime** |Das Datum und die Uhrzeit der Verarbeitung des Ereignisses durch Stream Analytics. |
| **BlobLastModifiedUtcTime** |Das Datum und die Uhrzeit der letzten Änderung des Blobs. |
| **PartitionId** |Die nullbasierte Partitions-ID für den Eingabeadapter. |

Beispielsweise können Sie anhand dieser Felder eine wie im folgenden Beispiel gezeigte Abfrage schreiben:

````
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
````

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
Sie haben sich mit Datenverbindungsoptionen in Azure für Ihre Stream Analytics-Aufträge vertraut gemacht. Weitere Informationen zu Stream Analytics finden Sie unter:

* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

