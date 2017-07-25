---
title: Verbinden eines Raspberry Pi mit Azure IoT Hub Suite | Microsoft-Dokumentation
description: "Tutorials, in denen Node.js oder C verwendet wird, um Ihnen zu vermitteln, wie das Microsoft Azure IoT Starter Kit für Raspberry Pi 3 und die IoT Suite-Remoteüberwachungslösung verwendet werden. Sie können ein Tutorial wählen, das entweder Telemetrie simuliert, echte Sensoren verwendet oder Remoteupdates der Firmware ermöglicht."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 7c4d5e161c9f7af33609be53e7b82f156bb0e33f
ms.openlocfilehash: 5efb78c2c514bd0b21be842ddee5465fe4e4dcf9
ms.contentlocale: de-de
ms.lasthandoff: 05/04/2017

---
# <a name="connect-your-microsoft-azure-iot-raspberry-pi-3-starter-kit-to-the-remote-monitoring-solution"></a>Verbinden Ihres Microsoft Azure IoT Raspberry Pi 3 Starter Kits mit der Remoteüberwachungslösung

In den Tutorials in diesem Abschnitt wird erklärt, wie Sie ein Raspberry Pi 3-Gerät mit der Remoteüberwachungslösung verbinden. Wählen Sie das Tutorial für Ihre bevorzugte Programmiersprache, und geben Sie an, ob Ihnen die Sensorhardware zur Verfügung steht, die mit Ihrem Raspberry Pi 3-Gerät verwendet werden soll.

## <a name="the-tutorials"></a>Die Tutorials

| Tutorial | Hinweise | Sprachen |
| -------- | ----- | --------- |
| Simulierte Telemetrie (einfach)| Simuliert Sensordaten. Verwendet ein eigenständiges Raspberry Pi. | [C][lnk-c-simulator], [Node.js][lnk-node-simulator] |
| Echter Sensor (mittel) | Verwendet Daten eines BME280-Sensors, der mit Ihrem Raspberry Pi verbunden ist. | [C][lnk-c-basic], [Node.js][lnk-node-basic] |
| Implementieren eines Firmwareupdates (fortgeschritten)| Verwendet Daten eines BME280-Sensors, der mit Ihrem Raspberry Pi verbunden ist. Ermöglicht Remoteupdates der Firmware auf Ihrem Raspberry Pi. | [C][lnk-c-advanced], [Node.js][lnk-node-advanced] |

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie das [Azure IoT Dev Center](https://azure.microsoft.com/develop/iot/), in dem Sie weitere Beispiele und Dokumentation zu Azure IoT finden.

[lnk-node-simulator]: iot-suite-raspberry-pi-kit-node-get-started-simulator.md
[lnk-node-basic]: iot-suite-raspberry-pi-kit-node-get-started-basic.md
[lnk-node-advanced]: iot-suite-raspberry-pi-kit-node-get-started-advanced.md
[lnk-c-simulator]: iot-suite-raspberry-pi-kit-c-get-started-simulator.md
[lnk-c-basic]: iot-suite-raspberry-pi-kit-c-get-started-basic.md
[lnk-c-advanced]: iot-suite-raspberry-pi-kit-c-get-started-advanced.md
