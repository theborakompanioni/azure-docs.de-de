---
title: "Importieren und Exportieren von Azure IoT Hub-Geräteidentitäten | Microsoft Docs"
description: "Erfahren Sie, wie Sie mithilfe des Azure IoT-Dienst-SDK Massenvorgänge zum Importieren und Exportieren von Geräteidentitäten auf die Identitätsregistrierung anwenden. Mit Importvorgängen können Sie Geräteidentitäten per Massenvorgang erstellen, aktualisieren und löschen."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2ade1494-45ea-46a7-ade7-cf6e11ce62da
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 79004e91c9e22b085b04e446999d4efe05426436
ms.openlocfilehash: 512c4dc5f77d5f730720909628364c5c9d8b3174
ms.lasthandoff: 02/23/2017


---
# <a name="manage-your-iot-hub-device-identities-in-bulk"></a>Massenverwaltung von IoT Hub-Geräteidentitäten
Jeder IoT Hub verfügt über eine Identitätsregistrierung, die Sie zum Erstellen von gerätebasierten Ressourcen im Dienst verwenden können. Hierzu zählt beispielsweise eine Warteschlange mit gesendeten C2D-Nachrichten. Außerdem wird mit der Identitätsregistrierung der Zugriff auf geräteseitige Endpunkte ermöglicht. In diesem Artikel wird das massenhafte Importieren und Exportieren von Geräteidentitäten in und aus einer Identitätsregistrierung beschrieben.

Import- und Exportvorgänge erfolgen im Kontext von *Aufträgen*, die Ihnen das Anwenden von Massendienstvorgängen auf einen IoT Hub ermöglichen.

Die **RegistryManager**-Klasse enthält die Methoden **ExportDevicesAsync** und **ImportDevicesAsync**, die das **Job**-Framework verwenden. Diese Methoden ermöglichen das Exportieren, Importieren und Synchronisieren der gesamten IoT Hub-Identitätsregistrierung.

## <a name="what-are-jobs"></a>Was sind Aufträge?
Identitätsregistrierungsvorgänge verwenden das **Job**-System, wenn der Vorgang:

* eine potenziell lange Ausführungsdauer im Vergleich mit standardmäßigen Laufzeitvorgängen hat oder
* eine große Menge von Daten an den Benutzer zurückgibt.

In diesen Fällen erstellt der Vorgang asynchron einen **Auftrag** für diesen IoT Hub, anstatt eines einzelnen API-Aufrufs, mit dem auf das Ergebnis des Vorgangs gewartet bzw. mit dem der Vorgang blockiert wird. Für den Vorgang wird dann sofort ein **JobProperties**-Objekt zurückgegeben.

Der folgende C#-Codeausschnitt zeigt, wie ein Exportauftrag erstellt wird:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

> [!NOTE]
> Um die **RegistryManager**-Klasse in Ihrem C#-Code zu verwenden, fügen Sie das **Microsoft.Azure.Devices**-NuGet-Paket Ihrem Projekt hinzu. Die **RegistryManager**-Klasse befindet sich im **Microsoft.Azure.Devices**-Namespace.


Anschließend können Sie die **RegistryManager**-Klasse verwenden, um den Status des **Auftrags** unter Verwendung der zurückgegebenen **JobProperties**-Metadaten abzufragen.

Der folgende C#-Codeausschnitt zeigt, wie alle fünf Sekunden eine Abfrage erfolgt, um festzustellen, ob die Auftragsausführung beendet wurde:

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>Exportieren von Geräten
Verwenden Sie die **ExportDevicesAsync**-Methode, um die gesamte IoT Hub-Identitätsregistrierung mithilfe einer [Shared Access Signature](https://msdn.microsoft.com/library/ee395415.aspx) in einen [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)-Blobcontainer zu exportieren.

Mit dieser Methode können Sie zuverlässige Sicherungen Ihrer Geräte-Informationen in einem Blobcontainer erstellen, den Sie steuern.

Die **ExportDevicesAsync** -Methode erfordert zwei Parameter:

* Eine *Zeichenfolge*, die einen URI eines Blobcontainers enthält. Dieser URI muss ein SAS-Token enthalten, das Schreibzugriff auf den Container gewährt. Der Auftrag erstellt zum Speichern der serialisierten Exportgerätedaten ein Blockblob in diesem Container. Das SAS-Token muss diese Berechtigungen enthalten:
  
   ```
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```
* Einen *booleschen* Wert, der angibt, ob Sie Authentifizierungsschlüssel aus Ihren Exportdaten ausschließen möchten. Bei **FALSE** werden Authentifizierungsschlüssel in die Exportausgabe eingeschlossen. Andernfalls werden Schlüssel als **NULL** exportiert.

Der folgende C#-Codeausschnitt zeigt, wie Sie einen Exportauftrag initiieren, der Geräteauthentifizierungsschlüssel in die exportierten Daten einbezieht und für den Abschluss eine Umfrage durchführt:

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

Der Auftrag speichert seine Ausgabe im angegebenen Blobcontainer als Blockblob mit dem Namen **devices.txt**. Die Ausgabedaten bestehen aus serialisierten JSON-Gerätedaten mit einem Gerät pro Zeile.

Das folgende Beispiel zeigt die Ausgabedaten:

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

Wenn Sie Zugriff auf diese Daten im Code benötigen, können Sie diese Daten mithilfe der **ExportImportDevice** -Klasse problemlos deserialisieren. Der folgende C#-Codeausschnitt zeigt, wie Geräte-Informationen gelesen werden, die zuvor in einen Blockblob exportiert wurden:

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [!NOTE]
> Sie können auch die **GetDevicesAsync**-Methode der **RegistryManager**-Klasse verwenden, um eine Liste mit Ihren Geräten abzurufen. Bei diesem Ansatz gilt jedoch eine feste Obergrenze von 1000 für die Anzahl der Geräteobjekte, die zurückgegeben werden. Die **GetDevicesAsync** -Methode sollte nur in Entwicklungsszenarien zum Unterstützen des Debuggens verwendet werden und wird für Produktionsworkloads nicht empfohlen.
> 
> 

## <a name="import-devices"></a>Importieren von Geräten
Die **ImportDevicesAsync**-Methode in der **RegistryManager**-Klasse ermöglicht Ihnen das Anwenden von Massenvorgängen (Import/Synchronisierung) auf eine IoT Hub-Identitätsregistrierung. Wie die **ExportDevicesAsync**-Methode auch, wird für die **ImportDevicesAsync**-Methode das **Job**-Framework verwendet.

Nutzen Sie die **ImportDevicesAsync**-Methode umsichtig, damit mit ihr nicht nur neue Geräte in der Identitätsregistrierung bereitgestellt, sondern auch vorhandene Geräte aktualisiert und gelöscht werden können.

> [!WARNING]
> Ein Importvorgang kann nicht rückgängig gemacht werden. Sichern Sie vor dem Verwenden der **ExportDevicesAsync**-Methode stets Ihre vorhandenen Daten in einem anderen Blobcontainer, bevor Sie Massenänderungen an Ihrer Identitätsregistrierung vornehmen.
> 
> 

Die **ImportDevicesAsync** -Methode erfordert zwei Parameter:

* Eine *Zeichenfolge*, die einen URI eines [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)-Blobcontainers als *Eingabe* in den Auftrag enthält. Dieser URI muss ein SAS-Token enthalten, das Lesezugriff auf den Container gewährt. Dieser Container muss ein Blob mit dem Namen **devices.txt** enthalten, das die serialisierten Gerätedaten enthält, die in Ihre Identitätsregistrierung importiert werden sollen. Die importierten Daten müssen Geräteinformationen im gleichen JSON-Format enthalten, das vom **ExportImportDevice**-Auftrag verwendet wird, wenn er das Blob **devices.txt** erstellt. Das SAS-Token muss diese Berechtigungen enthalten:
  
   ```
   SharedAccessBlobPermissions.Read
   ```
* Eine *Zeichenfolge*, die einen URI eines [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)-Blobcontainers als *Ausgabe* aus dem Auftrag enthält. Der Auftrag erstellt in diesem Container ein Blockblob zum Speichern von Fehlerinformationen aus dem abgeschlossenen **Importauftrag**. Das SAS-Token muss diese Berechtigungen enthalten:
  
   ```
   SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
   ```

> [!NOTE]
> Die beiden Parameter können auf den gleichen Blobcontainer verweisen. Die separaten Parameter ermöglichen einfach mehr Kontrolle über Ihre Daten, da der Ausgabecontainer zusätzliche Berechtigungen erfordert.
> 
> 

Der folgende C#-Codeausschnitt zeigt, wie ein Importauftrag ausgelöst wird:

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>Importverhalten
Sie können die **ImportDevicesAsync** -Methode zum Anwenden der folgenden Massenvorgänge auf Ihre Identitätsregistrierung verwenden:

* Massenregistrierung neuer Geräte
* Massenlöschung vorhandener Geräte
* Massenstatusänderungen (Aktivieren oder Deaktivieren von Geräten)
* Massenzuweisung neuer Geräteauthentifizierungsschlüssel
* Automatische Massenneugenerierung von Geräteauthentifizierungsschlüsseln

Sie können eine beliebige Kombination der obigen Vorgänge innerhalb eines einzelnen Aufrufs von **ImportDevicesAsync** ausführen. Sie können beispielsweise gleichzeitig neue Geräte registrieren oder vorhandene Geräte aktualisieren und löschen. Bei Verwendung zusammen mit der **ExportDevicesAsync** -Methode können Sie alle Ihre Geräte vollständig von einem IoT Hub zu einem anderen migrieren.

Verwenden Sie die optionale **importMode**-Eigenschaft in den Importserialisierungsdaten für jedes Gerät, um den Importprozess gerätebezogen zu steuern. Die **importMode** -Eigenschaft hat die folgenden Optionen:

| importMode | Beschreibung |
| --- | --- |
| **createOrUpdate** |Wenn ein Gerät mit der angegebenen **ID**nicht vorhanden ist, wird es neu registriert. <br/>Wenn das Gerät bereits vorhanden ist, werden vorhandene Informationen mit den bereitgestellten Eingabedaten ohne Berücksichtigung des **ETag** -Werts überschrieben. |
| **create** |Wenn ein Gerät mit der angegebenen **ID**nicht vorhanden ist, wird es neu registriert. <br/>Wenn das Gerät bereits vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. |
| **update** |Wenn ein Gerät mit der angegebenen **ID** bereits vorhanden ist, werden vorhandene Informationen durch die bereitgestellten Eingabedaten ohne Berücksichtigung des **ETag**-Werts überschrieben. <br/>Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. |
| **updateIfMatchETag** |Wenn ein Gerät mit der angegebenen **ID** bereits vorhanden ist, werden vorhandene Informationen durch die bereitgestellten Eingabedaten nur überschrieben, wenn es eine Übereinstimmung mit einem **ETag** gibt. <br/>Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. <br/>Wenn es keine Übereinstimmung mit einem **ETag** gibt, wird ein Fehler in die Protokolldatei geschrieben. |
| **createOrUpdateIfMatchETag** |Wenn ein Gerät mit der angegebenen **ID**nicht vorhanden ist, wird es neu registriert. <br/>Wenn das Gerät bereits vorhanden ist, werden vorhandene Informationen mit den bereitgestellten Eingabedaten nur überschrieben, wenn es eine Übereinstimmung mit einem **ETag** gibt. <br/>Wenn es keine Übereinstimmung mit einem **ETag** gibt, wird ein Fehler in die Protokolldatei geschrieben. |
| **delete** |Wenn ein Gerät mit der angegebenen **ID** bereits vorhanden ist, wird es ohne Berücksichtigung des **ETag**-Werts gelöscht. <br/>Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. |
| **deleteIfMatchETag** |Wenn ein Gerät mit der angegebenen **ID** bereits vorhanden ist, wird es nur gelöscht, wenn es eine Übereinstimmung mit einem **ETag** gibt. Wenn das Gerät nicht vorhanden ist, wird ein Fehler in die Protokolldatei geschrieben. <br/>Wenn es keine Übereinstimmung mit einem ETag gibt, wird ein Fehler in die Protokolldatei geschrieben. |

> [!NOTE]
> Wenn die Serialisierungsdaten nicht explizit ein **importMode**-Flag für ein Gerät definieren, wird standardmäßig **createOrUpdate** während des Importvorgangs gewählt.
> 
> 

## <a name="import-devices-example--bulk-device-provisioning"></a>Beispiel für das Importieren von Geräten – Massengerätebereitstellung
Im folgenden C#-Codebeispiel wird veranschaulicht, wie Sie mehrere Geräteidentitäten generieren, mit denen Folgendes durchgeführt wird:

* Einfügen von Authentifizierungsschlüsseln
* Schreiben von Geräteinformationen in ein Blockblob
* Importieren der Geräte in die Identitätsregistrierung

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>Beispiel für das Importieren von Geräten – Massenlöschung
Im folgenden Codebeispiel wird das Löschen der Geräte gezeigt, die Sie mithilfe des vorherigen Codebeispiels hinzugefügt haben:

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>Abrufen des SAS-URI des Containers
Das folgende Codebeispiel veranschaulicht das Erstellen eines [SAS-URI](../storage/storage-dotnet-shared-access-signature-part-2.md) mit Lese-, Schreib- und Löschberechtigungen für einen Blobcontainer:

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>Nächste Schritte
In diesem Artikel haben Sie gelernt, wie Massenvorgänge auf die Identitätsregistrierung in einem IoT Hub angewendet werden. Folgen Sie diesen Links, um mehr über das Verwalten von Azure IoT Hub zu erfahren:

* [IoT Hub-Metriken][lnk-metrics]
* [Vorgangsüberwachung][lnk-monitor]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit dem IoT Gateway SDK][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md

