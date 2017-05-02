---
title: "Azure IoT Hub – Vorgehensweise | Microsoft-Dokumentation"
description: Wie verwende ich als Entwickler die verschiedenen IoT Hub-Features?
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 6fbdebd316cd00e7dd762487848e616fdd2317e8
ms.openlocfilehash: 358fd0888049f97e5fde6e2a6303ea6c4cf65da9
ms.lasthandoff: 03/01/2017


---
# <a name="how-to-use-azure-iot-hub"></a>Vorgehensweise: Verwenden von Azure IoT Hub

Sie haben verschiedene Optionen, um zu lernen, wie Sie für den IoT Hub-Dienst entwickeln. Sie können die Artikel zur Konzeption lesen, in denen die Features von IoT Hub im Detail beschrieben werden, oder einem der Tutorials folgen, die die verschiedenen Features von IoT Hub abdecken.

## <a name="the-developer-guide"></a>Das Entwicklerhandbuch

Als Entwickler können Sie detaillierte konzeptionelle Anleitungen zu IoT Hub im [Entwicklerhandbuch][lnk-devguide] lesen. Dieses Handbuch enthält ausführliche Beschreibungen aller IoT Hub-Features, mit denen Sie lernen, wie Sie sie verwenden können, und wie Sie sie auswählen, wenn mehrere Optionen verfügbar sind.


## <a name="the-tutorials"></a>Die Tutorials

Wenn Sie lieber bestimmte IoT Hub-Features kennenlernen möchten, indem Sie praktische Übungen durcharbeiten, stehen Ihnen mehrere Tutorials zur Auswahl. Viele dieser Tutorials sind in verschiedenen Programmiersprachen verfügbar. Diese Tutorials enthalten:

- [Verarbeiten von IoT Hub-D2C-Nachrichten mit Routen (.NET)][lnk-routes-tutorial]. Dieses Tutorial zeigt Ihnen, wie Sie IoT Hub-Routingregeln auf einfache Weise und auf Konfigurationsbasis für das Senden von D2C-Nachrichten einsetzen.

- [Senden von C2D-Nachrichten mit IoT Hub (.NET)][lnk-c2d-tutorial]. In diesem Tutorial erfahren Sie, wie Sie C2D-Nachrichten über IoT Hub senden und C2D-Nachrichten auf einem Gerät empfangen.

- [Hochladen von Dateien von Geräten in die Cloud mit IoT Hub][lnk-upload-tutorial]. In diesem Tutorial wird gezeigt, wie Sie die Dateihochladefunktionen von IoT Hub verwenden.

- [Erste Schritte mit Gerätezwillingen][lnk-twin-tutorial]. Dieses Tutorial macht Sie mit Gerätezwillingen, gemeldeten Eigenschaften gewünschten Eigenschaften und Tags vertraut. Mit Gerätezwillingen synchronisieren Sie Werte mit Ihren Geräten.

- [Verwenden direkter Methoden][lnk-methods-tutorial]. In diesem Tutorial wird die Verwendung direkter Methoden beschrieben. Sie fügen einen Handler für eine direkte Methode dem simulierten Gerät hinzu und rufen die direkte Methode von IoT Hub aus auf.

- [Erste Schritte mit der Geräteverwaltung][lnk-dm-tutorial]. In diesem Tutorial wird gezeigt, wie Sie wichtige Geräteverwaltungsfeatures wie z.B. Gerätezwillinge und direkte Methoden für den Remoteneustart Ihres simulierten Geräts verwenden.

- [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten][lnk-properties-tutorial]. In diesem Tutorial lernen Sie, wie Sie die gewünschten Eigenschaften des Gerätezwillings zusammen mit gemeldeten Eigenschaften verwenden, um Ihr Gerät remote zu konfigurieren.

- [Initiieren eines Gerätefirmwareupdates mithilfe der Geräteverwaltung (Node/Node)][lnk-jobs-tutorial]. In diesem Tutorial wird gezeigt, wie Sie wichtige Geräteverwaltungsfeatures wie z.B. Gerätezwillinge und direkte Methoden für das Remoteupdate der Firmware Ihres Geräts verwenden.

- [Planen und Übertragen von Aufträgen][lnk-schedule-tutorial]. Dieses Tutorial zeigt Ihnen die Verwendung von gewünschten Eigenschaften und direkten Methoden, um zu einem geplanten Zeitpunkt mit mehreren Geräte zu interagieren.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum IoT Hub-Dienst finden Sie im [Entwicklerhandbuch][lnk-devguide].

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-routes-tutorial]: ./iot-hub-csharp-csharp-process-d2c.md
[lnk-c2d-tutorial]: ./iot-hub-csharp-csharp-c2d.md
[lnk-upload-tutorial]: ./iot-hub-csharp-csharp-file-upload.md
[lnk-twin-tutorial]: ./iot-hub-node-node-twin-getstarted.md
[lnk-methods-tutorial]: ./iot-hub-node-node-direct-methods.md
[lnk-dm-tutorial]: ./iot-hub-node-node-device-management-get-started.md
[lnk-properties-tutorial]: ./iot-hub-node-node-twin-how-to-configure.md
[lnk-jobs-tutorial]: ./iot-hub-node-node-firmware-update.md
[lnk-schedule-tutorial]: ./iot-hub-node-node-schedule-jobs.md
