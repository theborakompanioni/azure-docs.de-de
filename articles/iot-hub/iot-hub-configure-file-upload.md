---
title: Konfigurieren des Dateiuploads im Azure-Portal | Microsoft Docs
description: "Konfigurieren des IoT Hubs im Azure-Portal zum Aktivieren von Dateiuploads von verbundenen Geräten. Enthält Informationen zum Konfigurieren des Azure-Zielspeicherkontos."
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
ms.date: 01/05/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: a243e4f64b6cd0bf7b0776e938150a352d424ad1
ms.openlocfilehash: aadbee1388e2c2aafaf0a5c50d2af195284d2bec


---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Konfigurieren des Dateiuploads in IoT Hub im Azure-Portal
## <a name="file-upload"></a>Dateiupload
Damit Sie die [Dateiuploadfunktion in IoT Hub][lnk-upload] verwenden können, müssen Sie Ihrem Hub zunächst ein Azure Storage-Konto zuordnen. Wählen Sie die **Dateiupload**-Einstellungen zum Anzeigen einer Liste der Dateiuploadeigenschaften für den IoT Hub, der geändert wird.

![][13]

**Speichercontainer**: Verwenden Sie das Azure-Portal, um einen Blobcontainer in einem Azure Storage-Konto Ihres aktuellen Azure-Abonnements auszuwählen, der Ihrer IoT Hub-Instanz zugeordnet werden soll. Erstellen Sie bei Bedarf auf dem Blatt **Speicherkonten** ein Azure Storage-Konto und auf dem Blatt **Container** einen Blobcontainer. IoT Hub generiert automatisch SAS-URIs mit Schreibberechtigungen für diesen Blobcontainer, die Geräte beim Hochladen von Dateien verwenden können.

![][14]

**Benachrichtigungen für hochgeladene Dateien empfangen**: Aktivieren oder deaktivieren Sie Dateiuploadbenachrichtigungen über den Umschalter.

**SAS-TTL**: Diese Einstellung dient zum Festlegen der Gültigkeitsdauer der SAS-URIs, die von IoT Hub an das Gerät zurückgegeben werden. Standardmäßig auf 1 Stunde festgelegt, kann aber mithilfe des Schiebereglers in andere Werte geändert werden.

**Standard-TTL für Dateibenachrichtigungseinstellungen**: Die Gültigkeitsdauer einer Dateiuploadbenachrichtigung (Zeit bis zum Ablauf). Standardmäßig auf 1 Tag festgelegt, kann aber mithilfe des Schiebereglers in andere Werte geändert werden.

**Anzahl maximaler Zustellungen für Dateibenachrichtigungen**: Gibt an, wie oft IoT Hub versucht, eine Dateiuploadbenachrichtigung zu senden. Standardmäßig auf 10 festgelegt, kann aber mithilfe des Schiebereglers in andere Werte geändert werden.

![][15]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu den Dateiuploadfunktionen von IoT Hub finden Sie unter [Hochladen von Dateien von einem Gerät][lnk-upload] im IoT Hub-Entwicklerhandbuch.

Folgen Sie diesen Links, um mehr über das Verwalten von Azure IoT Hub zu erfahren:

* [Massenverwaltung von IoT-Geräten][lnk-bulk]
* [IoT Hub-Metriken][lnk-metrics]
* [Vorgangsüberwachung][lnk-monitor]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit dem IoT Gateway SDK][lnk-gateway]
* [Schützen Ihrer IoT-Lösung von Grund auf][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md



<!--HONumber=Dec16_HO1-->


