---
title: "Azure IoT Hub: Erste Schritte beim Herstellen von Verbindungen zwischen IoT-Geräten und der Cloud | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihre IoT-Boards und Starter Kits mit Azure IoT Hub verbinden. Ihre Geräte können Telemetriedaten an IoT Hub senden, und IoT Hub kann Ihre Geräte überwachen und verwalten."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
keywords: Tutorial zu Azure IoT Hub
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 2e1387e6cf5fedb983063a7f72ffe74c73948534
ms.contentlocale: de-de
ms.lasthandoff: 08/01/2017

---
# <a name="azure-iot-hub-get-started-tutorials"></a>Tutorials für die ersten Schritte mit Azure IoT Hub

Mit Azure IoT Hub und den Azure IoT-Geräte-SDKs können Sie IoT-Lösungen (Internet of Things, Internet der Dinge) entwickeln:

* Azure IoT Hub ist ein vollständig verwalteter Dienst in der Cloud, der Ihre IoT-Geräte sicher verbindet, überwacht und verwaltet. Verwenden Sie die Azure IoT-Geräte-SDKs, um Ihre IoT-Geräte zu implementieren.
* Verwenden Sie ein IoT-Gateway in komplexeren IoT-Szenarien, z.B., wenn Sie Faktoren wie veraltete Geräte, Kosten für Bandbreite, Sicherheit, Datenschutzrichtlinien oder Edgedatenverarbeitung berücksichtigen müssen. Verwenden Sie in diesen Szenarien Azure IoT Edge zum Implementieren eines Gateways, das Geräte mit Ihrem IoT Hub verbindet.

## <a name="what-the-tutorials-cover"></a>Themen in den Tutorials

Diese Tutorials dienen als Einführung in Azure IoT Hub und die Geräte-SDKs. In den Tutorials werden allgemeine IoT-Szenarien behandelt, um die Fähigkeiten von IoT Hub zu veranschaulichen. Die Tutorials erläutern auch das Kombinieren von IoT Hub mit anderen Azure-Diensten und -Tools zum Erstellen leistungsfähiger IoT-Lösungen. In den Tutorials können Sie entweder simulierte oder echte IoT-Geräte verwenden. Darüber hinaus erfahren Sie, wie Sie ein Gateway nutzen können, um Geräten eine Verbindung mit Ihrem IoT Hub zu ermöglichen.

## <a name="set-up-your-device"></a>Einrichten des Geräts

Verbinden Sie ein IoT-Gerät oder ein Gateway mit Azure IoT Hub. Sie können für die ersten Schritte ein physisches oder ein simuliertes Gerät wählen:

| IoT-Gerät                       | Programmiersprache |
|----------------------------------|----------------------|
| Raspberry Pi                     | [Node.js][Pi_Nd], [C][Pi_C], [Python][Pi_Py]    |
| Intel Edison                     | [Node.js][Ed_Nd], [C][Ed_C]    |
| Adafruit Feather HUZZAH ESP8266  | [Arduino][Hu_Ard]              |
| Sparkfun ESP8266 Thing Dev       | [Arduino][Th_Ard]              |
| Adafruit Feather M0              | [Arduino][M0_Ard]              |
| Simuliertes Gerät auf PC           | [.NET][Sim_NET], [Java][Sim_Jav], [Node.js][Sim_Nd], [Python][Sim_Pyth] |
| Onlinegerätesimulator         | [Raspberry Pi (Node.js)][Ol_Sim] |

Darüber hinaus können Sie ein IoT Edge-Gateway nutzen, um Geräten eine Verbindung mit IoT Hub zu ermöglichen:

| Gatewaygerät               | Programmiersprache | Plattform         |
|------------------------------|----------------------|------------------|
| Intel NUC (Modell DE3815TYKE) | C                    | [Wind River Linux][NUC_Lnx] |
| Simuliertes Gateway            | C                    | [Linux][Sim_Lnx], [Windows][Sim_Win] |

[!INCLUDE [iot-hub-get-started-extended](../../includes/iot-hub-get-started-extended.md)]

[Pi_Nd]: iot-hub-raspberry-pi-kit-node-get-started.md
[Pi_C]: iot-hub-raspberry-pi-kit-c-get-started.md
[Pi_Py]: iot-hub-raspberry-pi-kit-python-get-started.md
[Ed_Nd]: iot-hub-intel-edison-kit-node-get-started.md
[Ed_C]: iot-hub-intel-edison-kit-c-get-started.md
[Hu_Ard]: iot-hub-arduino-huzzah-esp8266-get-started.md
[Th_Ard]: iot-hub-sparkfun-esp8266-thing-dev-get-started.md
[M0_Ard]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-get-started.md
[Sim_NET]: iot-hub-csharp-csharp-getstarted.md
[Sim_Jav]: iot-hub-java-java-getstarted.md
[Sim_Nd]: iot-hub-node-node-getstarted.md
[Sim_Pyth]: iot-hub-python-getstarted.md
[NUC_Lnx]: iot-hub-gateway-kit-c-lesson1-set-up-nuc.md
[Sim_Lnx]: iot-hub-linux-iot-edge-get-started.md
[Sim_Win]: iot-hub-windows-iot-edge-get-started.md
[Ol_Sim]: iot-hub-raspberry-pi-web-simulator-get-started.md

