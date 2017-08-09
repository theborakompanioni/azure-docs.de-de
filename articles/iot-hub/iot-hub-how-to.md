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
ms.date: 07/25/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 786121ae249d69376b4be4c74000868cbb208989
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---
# <a name="how-to-use-azure-iot-hub"></a>Vorgehensweise: Verwenden von Azure IoT Hub

Sie haben verschiedene Möglichkeiten, um Ihre Kenntnisse zum IoT Hub-Dienst zu vertiefen:

* Lesen Sie die konzeptionellen Artikel, in denen die Funktionen von IoT Hub im Detail beschrieben werden.
* Absolvieren Sie eines der Tutorials, in denen die verschiedenen Funktionen von IoT Hub erläutert werden.

## <a name="developer-guide"></a>Entwicklerhandbuch

Als Entwickler können Sie detaillierte konzeptionelle Anleitungen zu IoT Hub im [Entwicklerhandbuch][lnk-devguide] lesen. Dieses Handbuch umfasst Folgendes:

* Detaillierte Beschreibungen zu allen IoT Hub-Funktionen, um Ihnen Informationen zu ihrer Verwendung bereitzustellen
* Unterstützende Informationen zur Auswahl, wenn mehrere Optionen verfügbar sind.

## <a name="tutorials"></a>Lernprogramme

Wenn Sie lieber bestimmte IoT Hub-Features kennenlernen möchten, indem Sie praktische Übungen durcharbeiten, stehen Ihnen mehrere Tutorials zur Auswahl. Viele dieser Tutorials sind in verschiedenen Programmiersprachen verfügbar. Diese Tutorials enthalten:

- [Verarbeiten von IoT Hub-D2C-Nachrichten mit Routen (.NET)][lnk-routes-tutorial]. Dieses Tutorial zeigt Ihnen, wie Sie IoT Hub-Routingregeln auf einfache Weise und auf Konfigurationsbasis für das Senden von D2C-Nachrichten einsetzen.

- [Senden von C2D-Nachrichten mit IoT Hub (.NET)][lnk-c2d-tutorial]. In diesem Tutorial erfahren Sie, wie Sie C2D-Nachrichten über IoT Hub senden und C2D-Nachrichten auf einem Gerät empfangen.

- [Hochladen von Dateien von Geräten in die Cloud mit IoT Hub][lnk-upload-tutorial]. In diesem Tutorial wird gezeigt, wie Sie die Dateihochladefunktionen von IoT Hub verwenden.

- [Erste Schritte mit Gerätezwillingen][lnk-twin-tutorial]. Dieses Tutorial macht Sie mit Gerätezwillingen, gemeldeten Eigenschaften gewünschten Eigenschaften und Tags vertraut. Mit Gerätezwillingen synchronisieren Sie Werte mit Ihren Geräten.

- [Verwenden direkter Methoden][lnk-methods-tutorial]. In diesem Tutorial wird die Verwendung direkter Methoden beschrieben. Sie fügen einen Handler für eine direkte Methode dem simulierten Gerät hinzu und rufen die direkte Methode von IoT Hub aus auf.

- [Erste Schritte mit der Geräteverwaltung][lnk-dm-tutorial]. In diesem Tutorial wird gezeigt, wie Sie wichtige Geräteverwaltungsfeatures wie Gerätezwillinge und direkte Methoden verwenden. Diese Funktionen verwenden Sie, um das simulierte Gerät remote neu zu starten.

- [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten][lnk-properties-tutorial]. In diesem Tutorial erfahren Sie, wie Sie die gewünschten und gemeldeten Eigenschaften des Gerätezwillings verwenden, um Ihr Gerät remote zu konfigurieren.

- [Initiieren eines Gerätefirmwareupdates mithilfe der Geräteverwaltung (Node/Node)][lnk-jobs-tutorial]. In diesem Tutorial wird gezeigt, wie Sie wichtige Geräteverwaltungsfeatures wie Gerätezwillinge und direkte Methoden verwenden. Sie erfahren, wie Sie mithilfe dieser Funktionen die Firmware des Geräts remote aktualisieren.

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
