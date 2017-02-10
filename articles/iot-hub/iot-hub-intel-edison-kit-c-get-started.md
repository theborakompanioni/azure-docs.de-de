---
title: Erste Schritte mit dem Azure IoT Intel Edison Starter Kit | Microsoft-Dokumentation
description: "Führen Sie erste Schritte mit dem Intel Edison aus, erstellen Sie Ihre Azure IoT Hub-Instanz, und verbinden Sie den Edison mit dieser IoT Hub-Instanz."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Intel Edison Entwicklung, Azure IoT Hub, erste Schritte mit dem Internet der Dinge, Internet der Dinge Tutorial, Adafruit Internet der Dinge, Intel Edison Arduino, erste Schritte mit Arduino
ms.assetid: 4885fa2c-c2ee-4253-b37f-ccd55f92b006
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/7/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: cb18052f74528e245264bb2f400b625fe550ed85
ms.openlocfilehash: 4c13a79d5701c91d845b5d141631801dec2a22d1


---
# <a name="get-started-with-intel-edison-c"></a>Erste Schritte mit Intel Edison (C)
> [!div class="op_single_selector"]
> * [Node.JS](iot-hub-intel-edison-kit-node-get-started.md)
> * [C](iot-hub-intel-edison-kit-c-get-started.md)

In diesem Tutorial machen Sie sich zuerst mit den Grundlagen der Verwendung des Intel Edison vertraut. Anschließend erfahren Sie, wie Sie Ihre Geräte mithilfe von [Azure IoT Hub](iot-hub-what-is-iot-hub.md) nahtlos mit der Cloud verbinden.

Sie haben noch kein Kit? Beginnen Sie [hier](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="lesson-1-configure-your-device"></a>Lektion 1: Konfigurieren Ihres Geräts
![Lektion 1: Komplettes Diagramm](media/iot-hub-intel-edison-lessons/e2e-lesson1.png)

In dieser Lektion konfigurieren Sie den Intel Edison mit einem Betriebssystem, richten die Entwicklungsumgebung ein und stellen eine Anwendung auf dem Edison bereit.

### <a name="configure-your-device"></a>Konfigurieren Ihres Geräts
Sie konfigurieren den Intel Edison für die erstmalige Verwendung, indem Sie die Platine einbauen, diese einschalten und das Konfigurationstool in Ihrem Desktopbetriebssystem mit dem Ziel installieren, die Firmware des Edison einzuspielen, das dazugehörige Kennwort festzulegen und sie mit dem WLAN zu verbinden.  

*Geschätzter Zeitaufwand: 30 Minuten*

Wechseln Sie zu [Konfigurieren Ihres Geräts][configure-your-device].

### <a name="get-the-tools"></a>Tools herunterladen
Laden Sie die Tools und Software zum Erstellen und Bereitstellen Ihrer ersten Anwendung für den Intel Edison herunter.

*Geschätzter Zeitaufwand: 20 Minuten*

Wechseln Sie zu [Abrufen der Tools][get-the-tools].

### <a name="create-and-deploy-the-blink-application"></a>Erstellen und Bereitstellen der Blinkanwendung
Klonen Sie die Blinkbeispielanwendung aus GitHub, und verwenden Sie Gulp zum Bereitstellen der Anwendung auf der Intel Edison-Platine. Diese Beispielanwendung bewirkt, dass die mit dem Pi verbundene LED alle zwei Sekunden blinkt.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Erstellen und Bereitstellen der Blinkanwendung][create-and-deploy-the-blink-application].

## <a name="lesson-2-create-your-iot-hub"></a>Lektion 2: Erstellen Sie Ihren IoT Hub
![Lektion 2: Komplettes Diagramm](media/iot-hub-intel-edison-lessons/e2e-lesson2.png)

In dieser Lektion erstellen Sie ein kostenloses Azure-Konto, stellen Ihre Azure IoT Hub-Instanz bereit und erstellen Ihr erstes Gerät im IoT Hub.

Schließen Sie Lektion 1 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="get-the-azure-tools"></a>Azure Tools herunterladen
Installieren der Azure-Befehlszeilenschnittstelle (Azure-CLI).

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Abrufen der Azure-Tools][get-azure-tools].

### <a name="create-your-iot-hub-and-register-intel-edison"></a>Erstellen der IoT Hub-Instanz und Registrieren von Intel Edison
Erstellen Sie Ihre Ressourcengruppe, stellen Sie Ihren ersten Azure IoT Hub bereit, und fügen Sie dem IoT Hub mithilfe der Azure-Befehlszeilenschnittstelle Ihr erstes Gerät hinzu.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Erstellen der IoT Hub-Instanz und Registrieren von Intel Edison](iot-hub-intel-edison-kit-c-lesson2-prepare-azure-iot-hub.md).

## <a name="lesson-3-send-device-to-cloud-messages"></a>Lektion 3: Senden von Gerät-an-Cloud-Nachrichten
![Lektion 3: Komplettes Diagramm](media/iot-hub-intel-edison-lessons/e2e-lesson3.png)

In dieser Lektion senden Sie Nachrichten von Ihrem Edison an den IoT Hub. Außerdem erstellen Sie eine Azure-Funktionen-App, die eingehende Nachrichten von Ihrem IoT-Hub übernimmt und sie in den Azure-Tabellenspeicher schreibt.

Schließen Sie die Lektionen 1 und 2 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos
Verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos][create-an-azure-function-app-and-azure-storage-account].

### <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten
Stellen Sie eine Beispielanwendung auf dem Intel Edison bereit, die Nachrichten an den IoT Hub sendet.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten][send-device-to-cloud-messages].

### <a name="read-messages-persisted-in-azure-storage"></a>Lesen von Nachrichten in Azure Storage
Sie überwachen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud), während diese in Azure Storage geschrieben werden.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Lesen von Nachrichten in Azure Storage][read-messages-persisted-in-azure-storage].

## <a name="lesson-4-send-cloud-to-device-messages"></a>Lektion 4: Senden von C2D-Nachrichten
![Lektion 4: Komplettes Diagramm](media/iot-hub-intel-edison-lessons/e2e-lesson4.png)

Diese Lektion zeigt, wie Nachrichten vom Azure IoT Hub an den Intel Edison gesendet werden. Die Nachrichten steuern die LED (Ein/Aus), die mit dem Edison verbunden ist. Zur Umsetzung dieser Aufgabe wird eine Beispielanwendung vorbereitet.

Schließen Sie die Lektionen 1, 2 und 3 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Ausführen der Beispielanwendung, um C2D-Nachrichten zu empfangen
Die Beispielanwendung in Lektion 4 wird auf dem Edison ausgeführt und überwacht eingehende Nachrichten von Ihrem IoT Hub. Ein neuer Gulp-Task sendet Nachrichten vom IoT Hub an den Edison, damit die LED blinkt.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Ausführen der Beispielanwendung, um C2D-Nachrichten zu empfangen][receive-cloud-to-device-messages].

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Optionaler Abschnitt: Ändern des Ein- und Aus-Verhaltens der LED
Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Optionaler Abschnitt: Ändern des Ein- und Aus-Verhaltens der LED][change-the-on-and-off-behavior-of-the-led].

## <a name="troubleshooting"></a>Problembehandlung
Falls Sie bei den Lektionen Probleme haben, helfen Ihnen die Lösungen im Artikel zur [Problembehandlung][troubleshooting] weiter.
<!-- Images and links -->

[configure-your-device]: iot-hub-intel-edison-kit-c-lesson1-configure-your-device.md
[get-the-tools]: iot-hub-intel-edison-kit-c-lesson1-get-the-tools-win32.md
[create-and-deploy-the-blink-application]: iot-hub-intel-edison-kit-c-lesson1-deploy-blink-app.md
[get-azure-tools]: iot-hub-intel-edison-kit-c-lesson2-get-azure-tools-win32.md
[create-an-azure-function-app-and-azure-storage-account]: iot-hub-intel-edison-kit-c-lesson3-deploy-resource-manager-template.md
[send-device-to-cloud-messages]: iot-hub-intel-edison-kit-c-lesson3-run-azure-blink.md
[read-messages-persisted-in-azure-storage]: iot-hub-intel-edison-kit-c-lesson3-read-table-storage.md
[receive-cloud-to-device-messages]:iot-hub-intel-edison-kit-c-lesson4-send-cloud-to-device-messages.md
[change-the-on-and-off-behavior-of-the-led]: iot-hub-intel-edison-kit-c-lesson4-change-led-behavior.md
[troubleshooting]: iot-hub-intel-edison-kit-c-troubleshooting.md


<!--HONumber=Dec16_HO2-->


