---
title: Verbinden eines Raspberry Pi 3 mit Azure IoT Hub | Microsoft Docs
description: Erste Schritte mit dem Raspberry Pi 3, erstellen Sie Ihre Azure IoT Hub-Instanz, und verbinden Sie Ihren Pi mit Ihrer IoT Hub-Instanz.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: Azure IoT Hub, erste Schritte mit dem Internet der Dinge, IoT Toolkit
experimental: true
experiment_id: xshi-happypathemu-20161202
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: d04f9d219451d683b572df43f73944011d686797


---
# <a name="get-started-with-raspberry-pi-3-nodejs"></a>Erste Schritte mit Raspberry Pi 3 (Node.js)
> [!div class="op_single_selector"]
> * [Node.JS](iot-hub-raspberry-pi-kit-node-get-started.md)
> * [C](iot-hub-raspberry-pi-kit-c-get-started.md)

In diesem Tutorial erlernen Sie die Grundlagen der Verwendung von Raspberry Pi 3 und Raspbian. Anschließend erfahren Sie, wie Sie Ihre Geräte mithilfe von [Azure IoT Hub](iot-hub-what-is-iot-hub.md) nahtlos mit der Cloud verbinden. Beispiele für Windows 10 IoT Core finden Sie im [Windows Dev Center](http://www.windowsondevices.com/).

Sie haben noch kein Kit? Beginnen Sie [hier](https://azure.microsoft.com/develop/iot/starter-kits).

## <a name="lesson-1-configure-your-device"></a>Lektion 1: Konfigurieren Ihres Geräts
![Lektion 1: Komplettes Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

In dieser Lektion konfigurieren Sie den Raspberry Pi 3 mit einem Betriebssystem, richten Ihre Entwicklungsumgebung ein und stellen eine Anwendung auf dem Pi bereit.

### <a name="configure-your-device"></a>Konfigurieren Ihres Geräts
Konfigurieren Sie Raspberry Pi 3 für die erste Verwendung, und installieren Sie Raspbian. Raspbian ist ein kostenloses Betriebssystem, das für die Raspberry Pi-Hardware optimiert ist.

*Geschätzter Zeitaufwand: 30 Minuten*

Wechseln Sie zu [Configure your device](iot-hub-raspberry-pi-kit-node-lesson1-configure-your-device.md) (Konfigurieren Ihres Geräts).

### <a name="get-the-tools"></a>Tools herunterladen
Laden Sie Tools und Software zum Erstellen und Bereitstellen Ihrer ersten Anwendung für Raspberry Pi 3 herunter.

*Geschätzter Zeitaufwand: 20 Minuten*

Wechseln Sie zu [Get the tools](iot-hub-raspberry-pi-kit-node-lesson1-get-the-tools-win32.md) (Herunterladen der Tools).

### <a name="create-and-deploy-the-blink-application"></a>Erstellen und Bereitstellen der Blinkanwendung
Klonen Sie die Beispiel-Node.js-Blinkanwendung aus Github, und verwenden Sie Gulp zum Bereitstellen der Anwendung auf dem Raspberry Pi 3. Diese Beispielanwendung bewirkt, dass die mit dem Pi verbundene LED alle zwei Sekunden blinkt.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Create and deploy the blink application](iot-hub-raspberry-pi-kit-node-lesson1-deploy-blink-app.md) (Erstellen und Bereitstellen der Blinkanwendung).

## <a name="lesson-2-create-your-iot-hub"></a>Lektion 2: Erstellen Sie Ihren IoT Hub
![Lektion 2: Komplettes Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

In dieser Lektion erstellen Sie ein kostenloses Azure-Konto, stellen Ihren Azure IoT Hub bereit und erstellen Ihr erstes Gerät in IoT Hub.

Schließen Sie Lektion 1 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="get-the-azure-tools"></a>Azure Tools herunterladen
Installieren der Azure-Befehlszeilenschnittstelle (Azure-CLI).

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Get Azure tools](iot-hub-raspberry-pi-kit-node-lesson2-get-azure-tools-win32.md) (Herunterladen der Azure-Tools).

### <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Erstellen eines IoT-Hubs und Registrieren des Raspberry Pi 3
Erstellen Sie Ihre Ressourcengruppe, stellen Sie Ihren ersten Azure IoT Hub bereit, und fügen Sie mithilfe der Azure-CLI Ihr erstes Gerät dem IoT-Hub hinzu.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-node-lesson2-prepare-azure-iot-hub.md) (Erstellen eines IoT-Hubs und Registrieren des Raspberry Pi 3).

## <a name="lesson-3-send-device-to-cloud-messages"></a>Lektion 3: Senden von Gerät-an-Cloud-Nachrichten
![Lektion 3: Komplettes Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

In dieser Lektion senden Sie Nachrichten von Ihrem Pi an den IoT-Hub. Außerdem erstellen Sie eine Azure-Funktionen-App, die eingehende Nachrichten von Ihrem IoT-Hub übernimmt und sie in den Azure-Tabellenspeicher schreibt.

Schließen Sie Lektion 1 und Lektion 2 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos
Verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Create an Azure function app and Azure storage account](iot-hub-raspberry-pi-kit-node-lesson3-deploy-resource-manager-template.md) (Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos).

### <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten
Bereitstellen und Ausführen einer Beispielanwendung auf dem Raspberry Pi 3, die Nachrichten an den IoT-Hub sendet.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Run a sample application to send device-to-cloud messages](iot-hub-raspberry-pi-kit-node-lesson3-run-azure-blink.md) (Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten).

### <a name="read-messages-persisted-in-azure-storage"></a>Lesen von Nachrichten in Azure Storage
Sie überwachen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud), während diese in Azure Storage geschrieben werden.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Read messages persisted in Azure Storage](iot-hub-raspberry-pi-kit-node-lesson3-read-table-storage.md) (Lesen von Nachrichten in Azure Storage).

## <a name="lesson-4-send-cloud-to-device-messages"></a>Lektion 4: Senden von C2D-Nachrichten
![Lektion 4: Komplettes Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

Diese Lektion zeigt, wie Nachrichten von Azure IoT Hub an den Raspberry Pi 3 gesendet werden. Die Nachrichten steuern die LED (Ein/Aus), die mit dem Pi verbunden ist. Zur Umsetzung dieser Aufgabe wird eine Beispielanwendung vorbereitet.

Schließen Sie Lektion 1, Lektion 2 und Lektion 3 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Ausführen der Beispielanwendung, um C2D-Nachrichten zu empfangen
Die Beispielanwendung in Lektion 4 wird auf dem Pi ausgeführt und überwacht eingehende Nachrichten von Ihrem IoT-Hub. Ein neuer Gulp-Task sendet Nachrichten von Ihrem IoT-Hub an den Pi, damit die LED blinkt.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Run the sample application to receive cloud-to-device messages](iot-hub-raspberry-pi-kit-node-lesson4-send-cloud-to-device-messages.md) (Ausführen der Beispielanwendung, um C2D-Nachrichten zu empfangen).

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Optionaler Abschnitt: Ändern des Ein- und Aus-Verhaltens der LED
Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Optional section: Change the on and off behavior of the LED](iot-hub-raspberry-pi-kit-node-lesson4-change-led-behavior.md) (Optional: Ändern des LED-Verhaltens).

## <a name="troubleshooting"></a>Problembehandlung
Wenn während der Lektionen Probleme auftreten, können Sie im Artikel [Problembehandlung](iot-hub-raspberry-pi-kit-node-troubleshooting.md) nach Lösungen suchen.




<!--HONumber=Dec16_HO2-->


