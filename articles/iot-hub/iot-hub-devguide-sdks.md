---
title: 'Entwicklungsleitfaden: Azure IoT SDKs | Microsoft-Dokumentation'
description: "Entwicklungsleitfaden für Azure IoT Hub: Informationen und Links zu verschiedenen Geräte- und Dienst-SDKs für Azure IoT."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: c18a1b16cb561edabd69f17ecebedf686732ac34
ms.openlocfilehash: 58dc546ee2db9a0925ecf0fc074795b531323e03


---
# <a name="azure-iot-sdks"></a>Azure IoT SDKs
## <a name="azure-iot-device-sdk"></a>Azure IoT-Geräte-SDK
Die Microsoft Azure IoT-Geräte-SDKs enthalten Code, der das Erstellen von Geräten und Anwendungen ermöglicht, die eine Verbindung mit Azure IoT Hub-Diensten herstellen und von ihnen verwaltet werden.

Die folgenden Azure IoT-Geräte-SDKs sind auf GitHub zum Download verfügbar:

* [Azure IoT-Geräte-SDK für C][lnk-c-device-sdk] wurde in ANSI C (C99) geschrieben und ist auf Portabilität und hohe Plattformkompatibilität ausgelegt.
* [Azure IoT-Geräte-SDK für .NET][lnk-dotnet-device-sdk]
* [Azure IoT-Geräte-SDK für Java][lnk-java-device-sdk]
* [Azure IoT-Geräte-SDK für Node.js][lnk-node-device-sdk]
* [Microsoft Azure IoT-Geräte-SDK für Python 2.7][lnk-python-device-sdk]

> [!NOTE]
> In den „Readme“-Dateien in den GitHub-Repositorys finden Sie Informationen zum Verwenden sprach- und plattformspezifischer Paket-Manager zum Installieren von Binärdateien und Abhängigkeiten auf Ihrem Entwicklungscomputer.
> 
> 

## <a name="os-platforms-and-hardware-compatibility"></a>Betriebssystemplattformen und Hardwarekompatibilität
Weitere Informationen zur Kompatibilität von SDKs mit bestimmten Hardwaregeräten finden Sie im [Katalog mit für Azure zertifizierten IoT-Geräten][lnk-certified].

## <a name="azure-iot-service-sdk"></a>Azure IoT-Geräte-SDK
Die Azure IoT-Dienst-SDKs enthalten Code, der das Erstellen von Anwendungen ermöglicht, die direkt mit IoT Hub interagieren, um die Geräte und die Sicherheit zu verwalten.

Die folgenden Azure IoT-Dienst-SDKs sind auf GitHub zum Download verfügbar:

* [Azure IoT-Dienst-SDK für .NET][lnk-dotnet-service-sdk]
* [Azure IoT-Dienst-SDK für Node.js][lnk-node-service-sdk]
* [Azure IoT-Dienst-SDK für Java][lnk-java-service-sdk]

> [!NOTE]
> In den „Readme“-Dateien in den GitHub-Repositorys finden Sie Informationen zum Verwenden sprach- und plattformspezifischer Paket-Manager zum Installieren von Binärdateien und Abhängigkeiten auf Ihrem Entwicklungscomputer.
> 
> 

## <a name="azure-iot-gateway-sdk"></a>Azure IoT Gateway SDK
Dieses Azure IoT Gateway SDK enthält die Infrastruktur und Module zum Erstellen von IoT-Gatewaylösungen. Sie können das SDK erweitern, um maßgeschneiderte Gateways für beliebige End-to-End-Szenarien zu erstellen.

Sie können das [Azure IoT Gateway SDK][lnk-gateway-sdk] von GitHub herunterladen.

## <a name="online-api-reference-documentation"></a>Online verfügbare API-Referenzdokumentation
Die folgende Liste enthält Links zur online verfügbaren API-Referenzdokumentation für Azure IoT-Gerätebibliotheken, -Dienstbibliotheken und -Gatewaybibliotheken:

* [Internet der Dinge (IoT, Internet of Things) .NET][lnk-dotnet-ref]
* [IoT Hub REST][lnk-rest-ref]
* [Azure IoT-Geräte-SDK für C][lnk-c-ref]
* [Azure IoT-Geräte-SDK für Java][lnk-java-ref]
* [Azure IoT-Dienst-SDK für Java][lnk-java-service-ref]
* [Azure IoT-Geräte-SDK für Node.js][lnk-node-ref]
* [Azure IoT-Dienst-SDK für Node.js][lnk-node-service-ref]
* [Azure IoT Gateway SDK][lnk-gateway-ref]

## <a name="next-steps"></a>Nächste Schritte
Weitere Referenzthemen in diesem IoT Hub-Entwicklungsleitfaden:

* [IoT Hub-Endpunkte][lnk-devguide-endpoints]
* [IoT Hub-Abfragesprache für Gerätezwillinge und Aufträge][lnk-devguide-query]
* [Kontingente und Drosselung][lnk-devguide-quotas]
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/c/readme.md
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/device/readme.md
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/readme.md
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/csharp/service/README.md
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/java/service/readme.md
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/device/readme.md
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/node/service/README.md
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdks/blob/master/python/device/readme.md
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/README.md

[lnk-dotnet-ref]: https://msdn.microsoft.com/library/mt488521.aspx
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: http://azure.github.io/azure-iot-sdks/java/device/api_reference/index.html
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-java-service-ref]: http://azure.github.io/azure-iot-sdks/java/service/api_reference/index.html
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md



<!--HONumber=Nov16_HO5-->


