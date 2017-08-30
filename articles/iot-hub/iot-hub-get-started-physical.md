---
title: "Erste Schritte beim Herstellen von Verbindungen zwischen physischen Geräten und Azure IoT Hub | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Verbindungen von physischen Geräten und Boards mit Azure IoT Hub herstellen. Ihre Geräte können Telemetriedaten an IoT Hub senden, und IoT Hub kann Ihre Geräte überwachen und verwalten."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: Tutorial zu Azure IoT Hub
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: f21984b4680eff78cd87b53561a604b49683705e
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="azure-iot-hub-get-started-with-physical-devices-tutorials"></a>Lernprogramme zu ersten Schritten für Azure IoT Hub mit physischen Geräten

Diese Tutorials dienen als Einführung in Azure IoT Hub und die Geräte-SDKs. In den Tutorials werden allgemeine IoT-Szenarien behandelt, um die Fähigkeiten von IoT Hub zu veranschaulichen. Die Tutorials erläutern auch das Kombinieren von IoT Hub mit anderen Azure-Diensten und -Tools zum Erstellen leistungsfähiger IoT-Lösungen. In den in der folgenden Tabelle aufgeführten Lernprogrammen wird veranschaulicht, wie physische IoT-Geräten erstellt werden.

| IoT-Gerät                       | Programmiersprache |
|---------------------------------|----------------------|
| Raspberry Pi                    | [Node.js][Pi_Nd], [C][Pi_C], [Python][Pi_Py]           |
| IoT DevKit                      | [Arduino in Visual Studio Code][DevKit]     |
| Intel Edison                    | [Node.js][Ed_Nd], [C][Ed_C]           |
| Adafruit Feather HUZZAH ESP8266 | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 Thing Dev      | [Arduino][Th_Ard]              |
| Adafruit Feather M0             | [Arduino][M0_Ard]              |

Darüber hinaus können Sie ein IoT Edge-Gateway nutzen, um Geräten eine Verbindung mit Ihrem IoT Hub zu ermöglichen.

| Gatewaygerät               | Programmiersprache | Plattform         |
|------------------------------|----------------------|------------------|
| Intel NUC (Modell DE3815TYKE) | C                    | [Wind River Linux][NUC_Lnx] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]


[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[DevKit]: iot-hub-arduino-iot-devkit-az3166-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md

