---
title: "Azure-Lösungen für das Internet der Dinge (IoT Suite) | Microsoft-Dokumentation"
description: "Eine Übersicht über IoT in Azure einschließlich einer beispielhaften Lösungsarchitektur und Informationen zu deren Beziehung zu Azure IoT Suite und den vorkonfigurierten Lösungen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 437d2655-896f-4a9e-a4a8-b864790d3ef8
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: 1c8703d64ff45e589a7ce93f1f2176681e1bf331
ms.contentlocale: de-de
ms.lasthandoff: 04/25/2017


---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="azure-iot-suite"></a>Azure IoT Suite
Microsoft Azure IoT Suite ist eine Lösung für Unternehmen und ermöglicht dank verschiedener erweiterbarer, vorkonfigurierter Lösungen einen schnellen Einstieg. Diese Lösungen werden für allgemeine IoT-Szenarien verwendet, z.B. [Remoteüberwachung][lnk-preconfigured-solutions], [vorbeugende Wartung][lnk-predictive-maintenance] und [verbundene Factory][lnk-connected-factory]. Diese Lösungen sind Implementierungen der in diesem Artikel beschriebenen IoT-Lösungsarchitektur.

Die vorkonfigurierten Lösungen sind vollständige, funktionsfähige End-to-End-Lösungen, die Folgendes umfassen:

- Simulierte Geräte, die Ihnen den Einstieg erleichtern
- Vorkonfigurierte Azure-Dienste, z.B. [Azure IoT Hub][Azure IoT Hub], [Azure Event Hubs][Azure Event Hubs], [Azure Stream Analytics][Azure Stream Analytics], [Azure Machine Learning][Azure Machine Learning] und [Azure Storage][Azure storage]
- Lösungsspezifische Verwaltungskonsolen

Die vorkonfigurierten Lösungen enthalten bewährten, einsatzbereiten Code, den Sie anpassen und erweitern können, um Ihre eigenen spezifischen IoT-Szenarios zu implementieren.

Unter Umständen ist auch der [Azure IoT Hub][Azure IoT Hub]-Dienst für Sie interessant, der für viele vorkonfigurierte Lösungen verwendet wird. [Azure IoT Hub][Azure IoT Hub] ermöglicht die sichere und zuverlässige bidirektionale Kommunikation zwischen Geräten und der Cloud, die in der vorkonfigurierten Lösungsarchitektur verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich die folgenden Ressourcen an, um mehr über IoT-Suite und die vorkonfigurierten Lösungen zu erfahren:

* [Was ist Azure IoT Suite?][lnk-whatissuite]
* [Was sind vorkonfigurierte Azure IoT Suite-Lösungen?][lnk-whatarepreconfigured]

[lnk-whatissuite]: iot-suite-overview.md
[lnk-whatarepreconfigured]: iot-suite-what-are-preconfigured-solutions.md

[lnk-preconfigured-solutions]: iot-suite-getstarted-preconfigured-solutions.md
[Azure IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[Azure Event Hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[Azure Machine Learning]: https://azure.microsoft.com/documentation/services/machine-learning/
[Azure storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-connected-factory]: iot-suite-connected-factory-overview.md
