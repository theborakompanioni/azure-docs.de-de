---
title: Informationen zum Hochladen von Dateien in Azure IoT Hub | Microsoft Docs
description: "Entwicklerhandbuch: Verwenden des Dateiupload-Features von IoT Hub zum Verwalten der Uploads von Dateien von einem Gerät in einen Azure Storage-Blobcontainer."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a0427925-3e40-4fcd-96c1-2a31d1ddc14b
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 9ded95283b52f0fc21ca5b99df8e72e1e152fe1c
ms.openlocfilehash: c56a568fa003ec45e92279e070e6309763071827


---
# <a name="file-uploads-with-iot-hub"></a>Dateiuploads mit IoT Hub
## <a name="overview"></a>Übersicht
Wie im Artikel [IoT Hub-Endpunkte][lnk-endpoints] ausführlich beschrieben, können Geräte Dateiuploads auslösen, indem sie eine Benachrichtigung über einen geräteseitigen Endpunkt (**/devices/{Geräte-ID}/files**) senden.  Wenn ein Gerät IoT Hub über einen abgeschlossenen Upload benachrichtigt, generiert IoT Hub Dateiuploadbenachrichtigungen, die Sie über einen dienstseitigen Endpunkt als Nachrichten empfangen können (**/messages/servicebound/filenotifications**).

Statt Nachrichten über IoT Hub selbst zu übertragen, fungiert IoT Hub als Verteiler für ein zugeordnetes Azure Storage-Konto. Ein Gerät fordert ein Speichertoken von IoT Hub an, das spezifisch für die vom Gerät hochgeladene Datei gilt. Das Gerät verwendet den SAS-URI zum Hochladen der Datei in den Speicher. Wenn der Upload abgeschlossen ist, sendet das Gerät eine Benachrichtigung über den Abschluss an IoT Hub. IoT Hub überprüft, ob die Datei hochgeladen wurde, und fügt dann dem neuen dienstseitigen Messaging-Endpunkt für Dateibenachrichtigungen eine Dateiuploadbenachrichtigung hinzu.

Bevor Sie eine Datei von einem Gerät in IoT Hub hochladen, müssen Sie Ihren Hub konfigurieren, indem Sie ihm ein [Azure Storage-Konto zuordnen][lnk-associate-storage].

Ihr Gerät kann dann [einen Upload initialisieren][lnk-initialize] und anschließend [IoT Hub benachrichtigen][lnk-notify], sobald der Upload abgeschlossen ist. Optional: Wenn ein Gerät IoT Hub benachrichtigt, dass der Upload abgeschlossen ist, kann der Dienst eine [Benachrichtigung][lnk-service-notification] generieren.

### <a name="when-to-use"></a>Einsatzgebiete
Verwenden Sie Dateiuploads zum Senden von Mediendateien und umfangreichen Telemetriebatches, die von zeitweise verbundenen Geräten hochgeladen oder komprimiert werden, um Bandbreite zu sparen.

Weitere Informationen zur Verwendung von gemeldeten Eigenschaften, D2C-Nachrichten oder Dateiuploads finden Sie im Thema [Leitfaden zur D2C-Kommunikation][lnk-d2c-guidance].

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Zuordnen eines Azure Storage-Kontos zu IoT Hub
Damit Sie die Funktion zum Hochladen von Dateien verwenden können, müssen Sie IoT Hub zunächst ein Azure Storage-Konto zuweisen. Sie können diese Aufgabe über das [Azure-Portal][lnk-management-portal] oder programmgesteuert über [REST-APIs für den IoT Hub-Ressourcenanbieter][lnk-resource-provider-apis] ausführen. Nachdem Sie Ihrem IoT Hub ein Azure Storage-Konto zugeordnet haben, gibt der Dienst einen SAS-URI an ein Gerät zurück, wenn das Gerät eine Anforderung zum Dateiupload auslöst.

> [!NOTE]
> Die [Azure IoT SDKs][lnk-sdks] behandeln automatisch das Abrufen des SAS-URIs, das Hochladen der Datei und das Benachrichtigen von IoT Hub über einen abgeschlossenen Upload.
> 
> 

## <a name="initialize-a-file-upload"></a>Initialisieren eines Dateiuploads
IoT Hub verfügt über einen Endpunkt speziell für Geräte zum Anfordern eines SAS-URI für die Speicherung zum Hochladen einer Datei. Das Gerät leitet den Dateiupload ein, indem es eine POST-Anforderung mit dem folgenden JSON-Text an den IoT Hub unter `{iot hub}.azure-devices.net/devices/{deviceId}/files`sendet.

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

IoT Hub gibt die folgenden Daten zurück, die vom Gerät zum Hochladen der Datei genutzt werden:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Veraltet: Initialisieren des Uploads einer Datei mit einer GET-Anweisung
> [!NOTE]
> In diesem Abschnitt wird veraltete Funktionalität zum Empfangen eines SAS-URI von IoT Hub beschrieben. Sie sollten die zuvor beschriebene POST-Methode verwenden.
> 
> 

IoT Hub verfügt über zwei REST-Endpunkte, um den Dateiupload zu unterstützen: einen zum Abrufen des SAS-URIs für den Speicher und einen zum Benachrichtigen von IoT Hub über einen abgeschlossenen Upload. Das Gerät initiiert den Dateiupload, indem es eine GET-Anforderung an IoT Hub unter `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`sendet. Der IoT-Hub gibt einen SAS-URI, der speziell für die hochzuladende Datei gilt, sowie eine Korrelations-ID zurück, die nach Abschluss des Uploads verwendet werden soll.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Benachrichtigen von IoT Hub über einen abgeschlossenen Dateiupload
Das Gerät ist verantwortlich für das Hochladen der Datei in den Speicher mithilfe der Azure Storage-SDKs. Nach Abschluss des Uploads sendet das Gerät eine POST-Anforderung mit dem folgenden JSON-Text an den IoT Hub unter `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications`:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Der Wert `isSuccess` ist eine boolesche Darstellung, ob die Datei erfolgreich hochgeladen wurde. Der Statuscode `statusCode` ist der Status des Uploads der Datei in Speicher, und `statusDescription` entspricht `statusCode`.

## <a name="reference-topics"></a>Referenzthemen:
Die folgenden Referenzthemen enthalten weitere Informationen zum Hochladen von Dateien von einem Gerät.

## <a name="file-upload-notifications"></a>Benachrichtigungen zum Dateiupload
Wenn ein Gerät eine Datei hochlädt und IoT Hub über den Abschluss des Uploads informiert, generiert der Dienst optional eine Benachrichtigung, die den Namen und den Speicherort der Datei enthält.

Wie im Abschnitt [Endpunkte][lnk-endpoints] erläutert, übermittelt IoT Hub Dateiuploadbenachrichtigungen als Nachrichten über einen dienstseitigen Endpunkt (**/messages/servicebound/fileuploadnotifications**). Die Empfangssemantik für Dateiuploadbenachrichtigungen entspricht der Empfangssemantik für C2D-Nachrichten und besitzt den gleichen [Nachrichtenlebenszyklus][lnk-lifecycle]. Jede Nachricht, die vom Endpunkt für Dateiuploadbenachrichtigungen abgerufen wird, ist ein JSON-Datensatz mit den folgenden Eigenschaften:

| Eigenschaft | Beschreibung |
| --- | --- |
| EnqueuedTimeUtc |Zeitstempel, der die Erstellung der Benachrichtigung angibt. |
| deviceId |**DeviceId** des Geräts, das die Datei hochgeladen hat. |
| BlobUri |Der URI der hochgeladenen Datei. |
| BlobName |Der Name der hochgeladenen Datei. |
| LastUpdatedTime |Zeitstempel, der die letzte Aktualisierung der Datei angibt. |
| BlobSizeInBytes |Die Größe der hochgeladenen Datei. |

**Beispiel**. Dieses Beispiel zeigt den Text einer Dateiuploadbenachrichtigung.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Konfigurationsoptionen für Dateiuploadbenachrichtigungen
Jeder IoT Hub legt die folgenden Konfigurationsoptionen für Dateiuploadbenachrichtigungen offen:

| Eigenschaft | Beschreibung | Bereich und Standardwert |
| --- | --- | --- |
| **enableFileUploadNotifications** |Steuert, ob Dateiuploadbenachrichtigungen in den Endpunkt für Dateibenachrichtigungen geschrieben werden. |Bool. Standardwert: TRUE. |
| **fileNotifications.ttlAsIso8601** |Standardgültigkeitsdauer für Dateiuploadbenachrichtigungen. |ISO_8601-Intervall bis zu 48 Stunden (mindestens 1 Minute). Standardwert: 1 Stunde. |
| **fileNotifications.lockDuration** |Sperrdauer für die Warteschlange der Dateiuploadbenachrichtigungen. |5 bis 300 Sekunden (mindestens 5 Sekunden). Standardwert: 60 Sekunden. |
| **fileNotifications.maxDeliveryCount** |Maximale Übermittlungsanzahl für die Warteschlange der Dateiuploadbenachrichtigungen. |1 bis 100. Standardwert: 100. |

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im IoT Hub-Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Azure IoT-SDKs für Geräte und Dienste][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit IoT Hub verwenden können.
* Unter [Referenz – Abfragesprache für Zwillinge und Aufträge][lnk-query] wird die IoT Hub-Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen zu Gerätezwillingen und Aufträgen abrufen können.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie gelernt haben, wie Sie Dateien mithilfe von IoT Hub von Geräten hochladen, sind möglicherweise die folgenden Themen im IoT Hub-Entwicklerhandbuch für Sie interessant:

* [Verwalten von Geräteidentitäten in IoT Hub][lnk-devguide-identities]
* [Verwalten des Zugriffs auf IoT Hub][lnk-devguide-security]
* [Grundlegendes zu Gerätezwillingen – Vorschau][lnk-devguide-device-twins]
* [Aufrufen einer direkten Methode auf einem Gerät][lnk-devguide-directmethods]
* [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, ist möglicherweise das folgende IoT Hub-Tutorial für Sie interessant:

* [Hochladen von Dateien von Geräten in die Cloud mit IoT Hub][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md



<!--HONumber=Feb17_HO3-->


