---
title: "Konfigurieren einer Verbindungszeichenfolge für Azure Storage | Microsoft Docs"
description: "Konfigurieren Sie eine Verbindungszeichenfolge für ein Azure-Speicherkonto. Eine Verbindungszeichenfolge enthält die erforderlichen Informationen zum Authentifizieren des Zugriffs zur Laufzeit auf ein Speicherkonto aus Ihrer Anwendung."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: ecb0acb5-90a9-4eb2-93e6-e9860eda5e53
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 1a3c754bf2f2b73d0bf72cbf48b906d8085eaef1
ms.openlocfilehash: f4410c10ce66d50b64307e364e64a3367b9397f9


---
# <a name="configure-azure-storage-connection-strings"></a>Konfigurieren von Azure Storage-Verbindungszeichenfolgen
## <a name="overview"></a>Übersicht
Eine Verbindungszeichenfolge enthält die erforderlichen Authentifizierungsinformationen zum Zugriff auf Daten in einem Azure-Speicherkonto aus Ihrer Anwendung zur Laufzeit. Sie können eine Verbindungszeichenfolge konfigurieren, um Folgendes zu tun:

* Verbinden mit dem Azure-Speicheremulator
* Zugreifen auf ein Speicherkonto in Azure
* Zugreifen auf angegebene Ressourcen in Azure über eine Shared Access Signature (SAS)

[!INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Speichern der Verbindungszeichenfolge
Die Anwendung muss zur Laufzeit auf die Verbindungszeichenfolge zugreifen, um bei Azure Storage eingehende Anforderungen zu authentifizieren. Es gibt mehrere Möglichkeiten, die Verbindungszeichenfolge zu speichern:

* Für eine Anwendung, die auf dem Desktop oder auf einem Gerät ausgeführt wird, können Sie die Verbindungszeichenfolge in der Datei **app.config** oder der Datei **web.config** speichern. Fügen Sie die Verbindungszeichenfolge dem Abschnitt **AppSettings** hinzu.
* Wenn Ihre Anwendung in einem Clouddienst in Azure ausgeführt wird, speichern Sie die Verbindungszeichenfolge zumeist im [Azure-Dienstkonfigurationsschema (CSCFG-Datei)](https://msdn.microsoft.com/library/ee758710.aspx). Fügen Sie die Verbindungszeichenfolge zum Abschnitt **ConfigurationSettings** der Dienstkonfigurationsdatei hinzu.
* Sie können die Verbindungszeichenfolge auch direkt in Ihrem Code verwenden. In den meisten Fällen wird jedoch empfohlen, dass Sie die Konfigurationszeichenfolge in einer Konfigurationsdatei speichern.

Das Speichern der Verbindungszeichenfolge in der Konfigurationsdatei erleichtert die Aktualisierung der Verbindungszeichenfolge, um zwischen dem Speicheremulator und einem Azure-Speicherkonto in der Cloud zu wechseln. Sie müssen die Verbindungszeichenfolge nur so bearbeiten, dass sie auf Ihre Zielumgebung verweist.

Über die [Microsoft Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) -Klasse können Sie zur Laufzeit auf die Verbindungszeichenfolge zugreifen, unabhängig davon, wo Ihre Anwendung ausgeführt wird.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Erstellen einer Verbindungszeichenfolge mit dem Speicheremulator
[!INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Unter [Einsatz des Azure-Speicheremulators für Entwicklung und Tests](storage-use-emulator.md) finden Sie weitere Informationen zum Speicheremulator.

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Erstellen einer Verbindungszeichenfolge für ein Azure-Speicherkonto
Um eine Verbindungszeichenfolge für Ihr Azure-Speicherkonto zu erstellen, verwenden Sie das nachstehende Format der Verbindungszeichenfolge. Geben Sie an, ob Sie über HTTP oder HTTPS (empfohlen) eine Verbindung mit dem Speicherkonto herstellen möchten, ersetzen Sie *myAccountName* durch den Namen Ihres Speicherkontos, und ersetzen Sie *myAccountKey* durch Ihren Kontozugriffsschlüssel:

`DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey`

Ihre Verbindungszeichenfolge sollte der folgenden Beispiel-Verbindungszeichenfolge ähneln:

`DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>`

> [!NOTE]
> Azure Storage unterstützt sowohl HTTP als auch HTTPS. Die Verwendung von HTTPS wird jedoch ausdrücklich empfohlen.
>
>

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Erstellen einer Verbindungszeichenfolge mit einer Shared Access Signature
[!INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Erstellen einer Verbindungszeichenfolge für einen bestimmten Speicherendpunkt
Sie können die Dienstendpunkte in Ihrer Verbindungszeichenfolge explizit angeben, anstatt die Standardendpunkte zu verwenden: Um eine Verbindungszeichenfolge zu erstellen, die einen expliziten Endpunkt festlegt, geben Sie für jeden Dienst den vollständigen Dienstendpunkt einschließlich der Protokollspezifikation (HTTPS (empfohlen) oder HTTP) im folgenden Format an:

```
DefaultEndpointsProtocol=[http|https];
BlobEndpoint=myBlobEndpoint;
QueueEndpoint=myQueueEndpoint;
TableEndpoint=myTableEndpoint;
FileEndpoint=myFileEndpoint;
AccountName=myAccountName;
AccountKey=myAccountKey
```

Bei einem möglichen Szenario für die Angabe eines expliziten Endpunkts haben Sie den Endpunkt Ihres Blobspeichers einer benutzerdefinierten Domäne zugeordnet. In diesem Fall können Sie Ihren benutzerdefinierten Endpunkt für Blob Storage in der Verbindungszeichenfolge angeben und optional die Standardendpunkte für den anderen Dienst angeben, wenn die Anwendung sie verwendet.

Es folgen Beispiele für gültige Verbindungszeichenfolgen, die einen expliziten Endpunkt für den Blobdienst angeben:

```
# Blob endpoint only
DefaultEndpointsProtocol=https;
BlobEndpoint=www.mydomain.com;
AccountName=storagesample;
AccountKey=account-key

# All service endpoints
DefaultEndpointsProtocol=https;
BlobEndpoint=www.mydomain.com;
FileEndpoint=myaccount.file.core.windows.net;
QueueEndpoint=myaccount.queue.core.windows.net;
TableEndpoint=myaccount;
AccountName=storagesample;
AccountKey=account-key
```

Der in der Verbindungszeichenfolge aufgeführte Endpunktwert wird verwendet, um die Anforderungs-URIs für den BLOB-Dienst zu erstellen. Ferner gibt er die Form aller URIs vor, die dem Code zurückgegeben werden.

Wenn Sie einen Endpunkt in der Verbindungszeichenfolge auslassen, können Sie nicht anhand dieser Verbindungszeichenfolge von Ihrem Code aus auf die Daten in diesem Dienst zugreifen.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Erstellen einer Verbindungszeichenfolge mit einem Endpunktsuffix
Zum Erstellen einer Verbindungszeichenfolge für den Speicherdienst in Regionen oder Instanzen mit anderen Endpunktsuffixen, z. B. für Azure China oder Azure Governance, verwenden Sie das folgende Format der Verbindungszeichenfolge. Geben Sie an, ob Sie über HTTP oder HTTPS eine Verbindung mit dem Speicherkonto herstellen möchten, ersetzen Sie *myAccountName* durch den Namen Ihres Speicherkontos, ersetzen Sie *myAccountKey* durch Ihren Kontozugriffsschlüssel, und ersetzen Sie *mySuffix* durch den URI-Suffix:

```
DefaultEndpointsProtocol=[http|https];
AccountName=myAccountName;
AccountKey=myAccountKey;
EndpointSuffix=mySuffix;
```

Ihre Verbindungszeichenfolge sollte beispielsweise der folgenden Verbindungszeichenfolge ähneln:

```
DefaultEndpointsProtocol=https;
AccountName=storagesample;
AccountKey=<account-key>;
EndpointSuffix=core.chinacloudapi.cn;
```

## <a name="parsing-a-connection-string"></a>Analysieren einer Verbindungszeichenfolge
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

## <a name="next-steps"></a>Nächste Schritte
* [Einsatz des Azure-Speicheremulators für Entwicklung und Tests](storage-use-emulator.md)
* [Windows Azure-Speicher-Explorer](storage-explorers.md)
* [Verwenden von Shared Access Signatures (SAS)](storage-dotnet-shared-access-signature-part-1.md)




<!--HONumber=Dec16_HO2-->


