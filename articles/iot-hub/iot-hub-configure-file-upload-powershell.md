---
title: Konfigurieren des Hochladens von Dateien mit Azure PowerShell | Microsoft-Dokumentation
description: "Informationen zum Konfigurieren des IoT Hubs mit Azure PowerShell-Cmdlets zum Aktivieren des Hochladens von Dateien von verbundenen Geräten. Enthält Informationen zum Konfigurieren des Azure-Zielspeicherkontos."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: f9003c65d1818952c6a019f81080d595791f63bf
ms.openlocfilehash: 66bb54927f3137aa7bfeb467962655a51bf71b4b
ms.contentlocale: de-de
ms.lasthandoff: 08/09/2017

---
# <a name="configure-iot-hub-file-uploads-using-powershell"></a>Konfigurieren des Hochladens von Dateien in IoT Hub mit PowerShell

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

Damit Sie die [Funktion zum Hochladen von Dateien in IoT Hub][lnk-upload] verwenden können, müssen Sie Ihrem Hub zunächst ein Azure-Speicherkonto zuordnen. Sie können ein vorhandenes Speicherkonto auswählen oder ein neues erstellen.

Für dieses Tutorial benötigen Sie Folgendes:

* Ein aktives Azure-Konto. Wenn Sie kein Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.
* [Azure PowerShell-Cmdlets][lnk-powershell-install].
* Einen Azure IoT Hub. Wenn Sie keinen IoT Hub haben, können Sie mit dem Cmdlet [New-AzureRmIoTHub][lnk-powershell-iothub] einen erstellen. Sie können auch im Portal [einen IoT Hub erstellen][lnk-portal-hub].
* Ein Azure-Speicherkonto. Wenn Sie kein Azure-Speicherkonto haben, können Sie mithilfe der [PowerShell-Cmdlets für Azure Storage][lnk-powershell-storage] eines erstellen. Sie können auch im Portal [ein Speicherkonto erstellen][lnk-portal-storage].

## <a name="sign-in-and-set-your-azure-account"></a>Anmelden und Festlegen Ihres Azure-Kontos

Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.

1. Führen Sie an der PowerShell-Eingabeaufforderung das Cmdlet **Login-AzureRmAccount** aus:

    ```powershell
    Login-AzureRmAccount
    ```

1. Wenn Sie über mehrere Azure-Abonnements verfügen, erhalten Sie durch die Anmeldung bei Azure Zugriff auf alle Azure-Abonnements, die mit Ihren Anmeldeinformationen verknüpft sind. Führen Sie den folgenden Befehl aus, um eine Liste der Azure-Abonnements anzuzeigen, die Sie verwenden können:

    ```powershell
    Get-AzureRMSubscription
    ```

    Führen Sie den folgenden Befehl aus, um das Abonnement auszuwählen, das Sie zum Ausführen der Befehle zum Verwalten Ihres IoT Hubs verwenden möchten. Sie können entweder den Abonnementnamen oder die ID aus der Ausgabe des vorherigen Befehls verwenden:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

## <a name="retrieve-your-storage-account-details"></a>Abrufen der Details Ihres Speicherkontos

In den folgenden Schritten wird davon ausgegangen, dass Sie Ihr Speicherkonto mit dem **Ressourcen-Manager**-Bereitstellungsmodell und nicht mit dem **klassischen** Bereitstellungsmodell erstellt haben.

Sie benötigen die Verbindungszeichenfolge eines Azure-Speicherkontos, um das Hochladen von Dateien von Ihren Geräten zu konfigurieren. Das Speicherkonto muss sich in demselben Abonnement wie der IoT-Hub befinden. Sie benötigen auch den Name des Blobcontainers im Speicherkonto. Verwenden Sie den folgenden Befehl, um Ihre Speicherkontoschlüssel abzurufen:

```powershell
Get-AzureRmStorageAccountKey `
  -Name {your storage account name} `
  -ResourceGroupName {your storage account resource group}
```

Notieren Sie den Wert **key1** des Speicherkontoschlüssels. Sie benötigen ihn in den folgenden Schritten.

Sie können entweder einen vorhandenen Blobcontainer für das Hochladen von Dateien verwenden oder einen neuen erstellen:

* Um die vorhandenen Blobcontainer in Ihrem Speicherkonto aufzulisten, verwenden Sie die folgenden Befehle:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    Get-AzureStorageContainer -Context $ctx
    ```

* Um einen Blobcontainer in Ihrem Speicherkonto zu erstellen, geben Sie die folgenden Befehle an:

    ```powershell
    $ctx = New-AzureStorageContext `
        -StorageAccountName {your storage account name} `
        -StorageAccountKey {your storage account key}
    New-AzureStorageContainer `
        -Name {your new container name} `
        -Permission Off `
        -Context $ctx
    ```

## <a name="configure-your-iot-hub"></a>Konfigurieren Ihres IoT-Hubs

Sie können jetzt Ihren IoT Hub unter Verwendung der Details Ihres Speicherkontos so konfigurieren, dass die [Funktionalität für das Hochladen von Dateien][lnk-upload] aktiviert wird.

Die Konfiguration erfordert die folgenden Werte:

**Speichercontainer**: Ein Blobcontainer in einem Azure-Speicherkonto Ihres aktuellen Azure-Abonnements, der Ihrer IoT Hub-Instanz zugeordnet werden soll. Sie haben die erforderlichen Speicherkontoinformationen im vorherigen Abschnitt abgerufen. IoT Hub generiert automatisch SAS-URIs mit Schreibberechtigungen für diesen Blobcontainer, die Geräte beim Hochladen von Dateien verwenden können.

**Benachrichtigungen für hochgeladene Dateien empfangen**: Aktivieren oder deaktivieren Sie Benachrichtigungen zum Hochladen von Dateien.

**SAS-TTL**: Diese Einstellung dient zum Festlegen der Gültigkeitsdauer der SAS-URIs, die von IoT Hub an das Gerät zurückgegeben werden. Standardmäßig auf 1 Stunde festgelegt.

**Standard-TTL für Dateibenachrichtigungseinstellungen**: Die Gültigkeitsdauer einer Dateiuploadbenachrichtigung (Zeit bis zum Ablauf). Standardmäßig auf 1 Tag festgelegt.

**Anzahl maximaler Zustellungen für Dateibenachrichtigungen**: Gibt an, wie oft IoT Hub versucht, eine Dateiuploadbenachrichtigung zu senden. Standardmäßig auf 10 festgelegt.

Verwenden Sie das folgende PowerShell-Cmdlet zum Konfigurieren der Einstellungen für das Hochladen von Dateien für Ihren IoT Hub:

```powershell
Set-AzureRmIotHub `
    -ResourceGroupName "{your iot hub resource group}" `
    -Name "{your iot hub name}" `
    -FileUploadNotificationTtl "01:00:00" `
    -FileUploadSasUriTtl "01:00:00" `
    -EnableFileUploadNotifications $true `
    -FileUploadStorageConnectionString "DefaultEndpointsProtocol=https;AccountName={your storage account name};AccountKey={your storage account key};EndpointSuffix=core.windows.net" `
    -FileUploadContainerName "{your blob container name}" `
    -FileUploadNotificationMaxDeliveryCount 10
```

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Dateiuploadfunktionen von IoT Hub finden Sie unter [Hochladen von Dateien von einem Gerät][lnk-upload].

Folgen Sie diesen Links, um mehr über das Verwalten von Azure IoT Hub zu erfahren:

* [Massenverwaltung von IoT-Geräten][lnk-bulk]
* [IoT Hub-Metriken][lnk-metrics]
* [Vorgangsüberwachung][lnk-monitor]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit IoT Edge][lnk-iotedge]
* [Schützen Ihrer IoT-Lösung von Grund auf][lnk-securing]

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
[lnk-powershell-storage]: https://docs.microsoft.com/powershell/module/azurerm.storage/
[lnk-powershell-iothub]: https://docs.microsoft.com/powershell/module/azurerm.iothub/new-azurermiothub
[lnk-portal-hub]: iot-hub-create-through-portal.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal-storage]: ../storage/storage-create-storage-account.md
