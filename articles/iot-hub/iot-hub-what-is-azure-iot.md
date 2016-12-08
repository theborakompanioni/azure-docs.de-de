---
title: "Azure-Lösungen für das Internet der Dinge | Microsoft Docs"
description: "Eine Übersicht über IoT in Azure einschließlich einer beispielhaften Lösungsarchitektur und Informationen zur Beziehung zu Azure IoT Hub, Geräte-SDKs und vorkonfigurierten Lösungen."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: c18a1b16cb561edabd69f17ecebedf686732ac34
ms.openlocfilehash: 00b2e90901d763d218d1118f1d7ef2bf617a2d2f


---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a>Nächste Schritte
Azure IoT Hub ist ein Azure-Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Ihrem Anwendungs-Back-End und Millionen von Geräten ermöglicht. Dies ermöglicht dem Anwendungs-Back-End Folgendes:

* Empfangen umfangreicher Telemetriedaten von Ihren Geräten
* Weiterleiten von Gerätedaten an einen Datenstrom-Ereignisprozessor
* Empfangen von Dateiuploads von Geräten
* Senden von Cloud-zu-Gerät-Befehlen an bestimmte Geräte

Sie können IoT Hub zum Implementieren Ihrer eigenen Back-End-Lösung verwenden. Darüber hinaus umfasst IoT Hub eine Identitätsregistrierung, die zum Bereitstellen von Geräten, ihren sicherheitsbezogenen Anmeldeinformationen und ihren Rechten zum Herstellen einer Verbindung mit dem IoT-Hub dient. Weitere Informationen zu IoT Hub finden Sie unter [Was ist Azure IoT Hub?][lnk-iot-hub].

Informationen dazu, wie Azure IoT Hub die standardbasierte Geräteverwaltung ermöglicht, sodass Sie Ihre Geräte remote verwalten, konfigurieren und aktualisieren können, finden Sie unter [Übersicht über die Geräteverwaltung mit IoT Hub][lnk-device-management].

Mit den Azure IoT-Geräte SDKs können Sie Clientanwendungen auf einer Vielzahl von Gerätehardwareplattformen und Betriebssystemen implementieren. Die Geräte SDKs enthalten Bibliotheken, die das Senden von Telemetriedaten an einen IoT Hub und das Empfangen von C2D-Befehlen ermöglichen. Bei Verwendung der Geräte SDKs stehen für die Kommunikation mit IoT Hub mehrere Netzwerkprotokolle zur Auswahl. Erfahren Sie mehr in den [Informationen zu Geräte SDKs (in englischer Sprache)][lnk-device-sdks].

Hilfe zu den ersten Schritten beim Schreiben von Code und dem Ausführen von Beispielen finden Sie im Tutorial [Erste Schritte mit IoT Hub][lnk-getstarted].

Möglicherweise interessieren Sie sich auch für [Azure IoT Suite][lnk-iot-suite], eine Sammlung vorkonfigurierter Lösungen. IoT Suite ermöglicht den schnellen Einstieg und das Skalieren von IoT-Projekten und eignet sich damit für allgemeine IoT-Szenarien wie Remoteüberwachung, Asset Management und vorbeugende Wartung.

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks/blob/master/readme.md
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md



<!--HONumber=Nov16_HO5-->


