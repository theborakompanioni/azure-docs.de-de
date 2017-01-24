---
title: Azure IoT-Protokollgateway | Microsoft Docs
description: "Erfahren Sie, wie Sie mit einem Azure IoT-Protokollgateway die IoT Hub-Funktionen und die unterstützten Protokolle erweitern, damit Geräte Verbindungen mit Ihrem Hub mithilfe von Protokollen, die nicht nativ von IoT Hub unterstützt werden, herstellen können."
services: iot-hub
documentationcenter: 
author: kdotchkoff
manager: timlt
editor: 
ms.assetid: 555e59ae-3136-4533-8ba8-f3a3b6acf648
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2016
ms.author: kdotchko
translationtype: Human Translation
ms.sourcegitcommit: e223d0613cd48994315451da87e6b7066585bdb6
ms.openlocfilehash: 17250760526e72c05ee178e235a57082e2dfff94


---
# <a name="support-additional-protocols-for-iot-hub"></a>Unterstützen zusätzlicher Protokolle für IoT Hub
Azure IoT Hub bietet nativ Unterstützung für die Kommunikation über die Protokolle MQTT, AMQP und HTTP. In einigen Fällen können Geräte oder Bereichsgateways möglicherweise keines dieser Standardprotokolle verwenden und erfordern eine Protokollanpassung. In solchen Fällen können Sie ein benutzerdefiniertes Gateway verwenden. Ein benutzerdefiniertes Gateway kann die Protokollanpassung für IoT Hub-Endpunkte ermöglichen und für den IoT Hub-Datenverkehr als Brücke fungieren. Sie können das [Azure IoT-Protokollgateway](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md) als benutzerdefiniertes Gateways zum Ermöglichen der Protokollanpassung für IoT Hub verwenden.

## <a name="azure-iot-protocol-gateway"></a>Azure IoT-Protokollgateway
Das Azure IoT-Protokollgateway ist ein Framework zur Protokollanpassung für die hoch skalierbare, bidirektionale Gerätekommunikation mit IoT Hub. Das Protokollgateway ist eine Passthrough-Komponente, die Geräteverbindungen über ein bestimmtes Protokoll akzeptiert. Es fungiert als Brücke für den Datenverkehr zum IoT Hub über AMQP 1.0. Das Azure IoT-Protokollgateway steht als Open-Source-Softwareprojekt zur Verfügung und ermöglicht eine flexible Unterstützung für eine Vielzahl von Protokollen und Protokollversionen.

Sie können das Protokollgateway in Azure auf hochgradig skalierbare Weise mithilfe von Azure Cloud Services-Workerrollen bereitstellen. Darüber hinaus kann das Protokollgateway in lokalen Umgebungen wie z. B. Bereichsgateways bereitgestellt werden.

Das Azure IoT-Protokollgateway bietet einen MQTT-Protokolladapter, mit dem Sie bei Bedarf das Verhalten des Protokolls MQTT anpassen können. Da IoT Hub integrierte Unterstützung für das Protokoll MQTT 3.1.1 bietet, sollten Sie den MQTT IP-Adapter nur erwägen, wenn Protokollanpassungen erforderlich sind oder bestimmte Anforderungen für zusätzliche Funktionalität gelten.

Der MQTT-Adapter veranschaulicht außerdem das Programmiermodell zum Erstellen von Protokolladaptern für andere Protokolle. Darüber hinaus unterstützt das Programmiermodell des Azure IoT-Protokollgateways die Integration benutzerdefinierter Komponenten für spezielle Verarbeitungsaufgaben – zum Beispiel benutzerdefinierte Authentifizierung, Nachrichtentransformationen, Komprimierung/Dekomprimierung oder Verschlüsselung/Entschlüsselung des Datenverkehrs zwischen Geräten und IoT Hub.

Für mehr Flexibilität werden das Protokollgateway und die MQTT-Implementierung als Open-Source-Softwareprojekt bereitgestellt. Auf diese Weise können Sie die Implementierung nach Bedarf anpassen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über das Azure IoT-Protokollgateway sowie dessen Verwendung und Bereitstellung im Rahmen Ihrer IoT-Lösung finden Sie unter:

* [Repository für das Azure IoT-Protokollgateway auf GitHub](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md)
* [Entwicklungsleitfaden für das Azure IoT-Protokollgateway](https://github.com/Azure/azure-iot-protocol-gateway/blob/master/docs/DeveloperGuide.md)

Weitere Informationen zum Planen Ihrer IoT Hub-Bereitstellung finden Sie unter:

* [Vergleich mit Event Hubs][lnk-compare]
* [Skalierung, hohe Verfügbarkeit und Notfallwiederherstellung][lnk-scaling]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit dem IoT Gateway SDK][lnk-gateway]

[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md



<!--HONumber=Dec16_HO1-->


