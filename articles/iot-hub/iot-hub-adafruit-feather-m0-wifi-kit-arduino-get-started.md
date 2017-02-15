---
title: Erste Schritte mit Adafruit Feather M0 WiFi Azure IoT Starterkit | Microsoft-Dokumentation
description: Erste Schritte mit Adafruit Feather M0 WiFi, Erstellen der Azure IoT Hub-Instanz und Verbinden von Adafruit Feather M0 WiFi mit der IoT Hub-Instanz
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure IoT Hub, erste Schritte mit dem Internet der Dinge, Internet der Dinge Tutorial, Adafruit Internet der Dinge, erste Schritte mit Arduino
ms.assetid: 51befcdb-332b-416f-a6a1-8aabdb67f283
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 9e8084fe26229ef9ed1676c0a3c34e0ee7be88b1
ms.openlocfilehash: 8689ee35e880b1aa774b09bb194b43f280b2af79


---
# <a name="get-started-with-your-arduino-board-adafruit-feather-m0-wifi"></a>Erste Schritte mit dem Arduino-Board: Adafruit Feather M0 WiFi

In diesem Tutorial machen Sie sich zuerst mit den Grundlagen der Verwendung des Arduino-Boards vertraut. Anschließend erfahren Sie, wie Sie Ihre Geräte mithilfe von [Azure IoT Hub](iot-hub-what-is-iot-hub.md) nahtlos mit der Cloud verbinden.

## <a name="lesson-1-configure-your-device"></a>Lektion 1: Konfigurieren Ihres Geräts
![Lektion 1: Komplettes Diagramm][Lesson-1-end-to-end-diagram]

In dieser Lektion konfigurieren Sie das Arduino-Board mit einem Betriebssystem, richten die Entwicklungsumgebung ein und stellen eine Anwendung auf dem Arduino-Board bereit.

### <a name="configure-your-device"></a>Konfigurieren Ihres Geräts
Konfigurieren Sie das Arduino-Board zur erstmaligen Verwendung, indem Sie das Board einbauen und einschalten.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Konfigurieren Ihres Geräts][configure-your-device].

### <a name="get-the-tools"></a>Tools herunterladen
Laden Sie Tools und Software zum Erstellen und Bereitstellen Ihrer ersten Anwendung für das Arduino-Board herunter.

*Geschätzter Zeitaufwand: 20 Minuten*

Wechseln Sie zu [Tools herunterladen][get-the-tools].

### <a name="create-and-deploy-the-blink-application"></a>Erstellen und Bereitstellen der Blinkanwendung
Klonen Sie die Arduino-Blinkbeispielanwendung aus GitHub, und verwenden Sie Gulp zum Bereitstellen der Anwendung auf dem Arduino-Board. Bei dieser Beispielanwendung blinkt die integrierte GPIO #13-LED alle zwei Sekunden.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Erstellen und Bereitstellen der Blinkanwendung][create-and-deploy-the-blink-application].

## <a name="lesson-2-create-your-iot-hub"></a>Lektion 2: Erstellen Sie Ihren IoT Hub
![Lektion 2: Komplettes Diagramm][lesson-2-end-to-end-diagram]

In dieser Lektion erstellen Sie ein kostenloses Azure-Konto, stellen Ihre Azure IoT Hub-Instanz bereit und erstellen Ihr erstes Gerät im IoT Hub.

Schließen Sie Lektion 1 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="get-the-azure-tools"></a>Azure Tools herunterladen
Installieren der Azure-Befehlszeilenschnittstelle (Azure-CLI).

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Abrufen der Azure-Tools][get-azure-tools].

### <a name="create-your-iot-hub-and-register-your-arduino-board"></a>Erstellen der IoT Hub-Instanz und Registrieren des Arduino-Boards
Erstellen Sie Ihre Ressourcengruppe, stellen Sie Ihre erste Azure IoT Hub-Instanz bereit, und fügen Sie ihr mithilfe der Azure-Befehlszeilenschnittstelle Ihr erstes Gerät hinzu.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Erstellen der IoT Hub-Instanz und Registrieren des Arduino-Boards][create-your-iot-hub-and-register-your-arduino-board].

## <a name="lesson-3-send-device-to-cloud-messages"></a>Lektion 3: Senden von Gerät-an-Cloud-Nachrichten
![Lektion 3: Komplettes Diagramm][lesson-3-end-to-end-diagram]

In dieser Lektion senden Sie Nachrichten vom Arduino-Board an den IoT Hub. Außerdem erstellen Sie eine Azure-Funktionen-App, die eingehende Nachrichten von Ihrem IoT-Hub übernimmt und sie in den Azure-Tabellenspeicher schreibt.

Schließen Sie die Lektionen 1 und 2 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos
Verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos][create-an-azure-function-app-and-azure-storage-account].

### <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten
Bereitstellen und Ausführen einer Beispielanwendung auf dem Arduino-Board, das Nachrichten an den IoT-Hub sendet.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten][send-device-to-cloud-messages].

### <a name="read-messages-persisted-in-azure-storage"></a>Lesen von Nachrichten in Azure Storage
Sie überwachen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud), während diese in Azure Storage geschrieben werden.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Lesen von Nachrichten in Azure Storage][read-messages-persisted-in-azure-storage].

## <a name="lesson-4-send-cloud-to-device-messages"></a>Lektion 4: Senden von C2D-Nachrichten
![Lektion 4: Komplettes Diagramm][lesson-4-end-to-end-diagram]

Diese Lektion zeigt, wie Nachrichten von Azure IoT Hub an das Arduino-Board gesendet werden. Die Nachrichten steuern das Ein- und Aus-Verhalten der integrierten GPIO #13-LED. Zur Umsetzung dieser Aufgabe wird eine Beispielanwendung vorbereitet.

Schließen Sie die Lektionen 1, 2 und 3 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Ausführen der Beispielanwendung, um C2D-Nachrichten zu empfangen
Die Beispielanwendung in Lektion 4 wird auf dem Arduino-Board ausgeführt und überwacht eingehende Nachrichten von IoT Hub. Ein neuer Gulp-Task sendet Nachrichten von IoT Hub an das Arduino-Board, damit die LED blinkt.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Ausführen der Beispielanwendung, um C2D-Nachrichten zu empfangen][receive-cloud-to-device-messages].

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Optionaler Abschnitt: Ändern des Ein- und Aus-Verhaltens der LED
Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Optionaler Abschnitt: Ändern des Ein- und Aus-Verhaltens der LED][change-the-on-and-off-behavior-of-the-led].

## <a name="troubleshooting"></a>Problembehandlung
Falls Sie bei den Lektionen Probleme haben, helfen Ihnen die Lösungen im Artikel zur [Problembehandlung][troubleshooting] weiter.

<!-- Images and links -->

[Lesson-1-end-to-end-diagram]: media/iot-hub-adafruit-feather-m0-wifi-lessons/e2e-lesson1.png
[configure-your-device]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md
[create-and-deploy-the-blink-application]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-deploy-blink-app.md
[lesson-2-end-to-end-diagram]: media/iot-hub-adafruit-feather-m0-wifi-lessons/e2e-lesson2.png
[get-azure-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-get-azure-tools-win32.md
[create-your-iot-hub-and-register-your-arduino-board]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson2-prepare-azure-iot-hub.md
[lesson-3-end-to-end-diagram]: media/iot-hub-adafruit-feather-m0-wifi-lessons/e2e-lesson3.png
[create-an-azure-function-app-and-azure-storage-account]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-deploy-resource-manager-template.md
[send-device-to-cloud-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-run-azure-blink.md
[read-messages-persisted-in-azure-storage]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson3-read-table-storage.md
[lesson-4-end-to-end-diagram]: media/iot-hub-adafruit-feather-m0-wifi-lessons/e2e-lesson4.png
[receive-cloud-to-device-messages]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-send-cloud-to-device-messages.md
[change-the-on-and-off-behavior-of-the-led]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson4-change-led-behavior.md
[troubleshooting]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md


<!--HONumber=Dec16_HO2-->


