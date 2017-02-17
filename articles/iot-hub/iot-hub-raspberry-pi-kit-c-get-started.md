---
title: "Verbinden von Raspberry Pi (C) mit Azure IoT – Erste Schritte | Microsoft-Dokumentation"
description: "Führen Sie erste Schritte mit Raspberry Pi 3 aus, erstellen Sie Ihre Azure IoT Hub-Instanz, und verbinden Sie Pi mit dieser IoT Hub-Instanz."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure IoT Hub, erste Schritte mit dem Internet der Dinge, IoT Toolkit
ms.assetid: 68c0e730-1dc8-4e26-ac6b-573b217b302d
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 58398a8963ca834ed04d50a4715b29cbd3200b53


---
# <a name="get-started-with-raspberry-pi-3-c"></a>Erste Schritte mit Raspberry Pi 3 (C)
> [!div class="op_single_selector"]
> * [Node.JS](iot-hub-raspberry-pi-kit-node-get-started.md)
> * [C](iot-hub-raspberry-pi-kit-c-get-started.md)

In diesem Tutorial erlernen Sie die Grundlagen der Verwendung von Raspberry Pi 3 und Raspbian. Anschließend erfahren Sie, wie Sie Ihre Geräte mithilfe von [Azure IoT Hub](iot-hub-what-is-iot-hub.md) nahtlos mit der Cloud verbinden. Beispiele für Windows 10 IoT Core finden Sie im [Windows Dev Center](http://www.windowsondevices.com/).

## <a name="lesson-1-configure-your-device"></a>Lektion 1: Konfigurieren Ihres Geräts
![Lektion 1: Komplettes Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson1.png)

In dieser Lektion konfigurieren Sie den Raspberry Pi 3 mit einem Betriebssystem, richten die Entwicklungsumgebung ein und stellen eine Anwendung auf dem Pi bereit.

### <a name="configure-your-device"></a>Konfigurieren Ihres Geräts
Konfigurieren Sie Raspberry Pi 3 für die erste Verwendung, und installieren Sie Raspbian. Raspbian ist ein kostenloses Betriebssystem, das für die Raspberry Pi-Hardware optimiert ist.

*Geschätzter Zeitaufwand: 30 Minuten*

Wechseln Sie zu [Configure your device](iot-hub-raspberry-pi-kit-c-lesson1-configure-your-device.md) (Konfigurieren Ihres Geräts).

### <a name="get-the-tools"></a>Tools herunterladen
Laden Sie Tools und Software zum Erstellen und Bereitstellen Ihrer ersten Anwendung für Raspberry Pi 3 herunter.

*Geschätzter Zeitaufwand: 20 Minuten*

Wechseln Sie zu [Get the tools](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md) (Herunterladen der Tools).

### <a name="create-and-deploy-the-blink-application"></a>Erstellen und Bereitstellen der Blinkanwendung
Klonen Sie die C-Beispielblinkanwendung aus GitHub, und stellen Sie sie mithilfe von Gulp auf dem Raspberry Pi 3 bereit. Diese Beispielanwendung bewirkt, dass die mit dem Pi verbundene LED alle zwei Sekunden blinkt.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Create and deploy the blink application](iot-hub-raspberry-pi-kit-c-lesson1-deploy-blink-app.md) (Erstellen und Bereitstellen der Blinkanwendung).

## <a name="lesson-2-create-your-iot-hub"></a>Lektion 2: Erstellen Sie Ihren IoT Hub
![Lektion 2: Komplettes Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson2.png)

In dieser Lektion erstellen Sie ein kostenloses Azure-Konto, stellen Ihre Azure IoT Hub-Instanz bereit und erstellen Ihr erstes Gerät im IoT Hub.

Schließen Sie Lektion 1 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="get-the-azure-tools"></a>Azure Tools herunterladen
Installieren der Azure-Befehlszeilenschnittstelle (Azure-CLI).

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Get Azure tools](iot-hub-raspberry-pi-kit-c-lesson2-get-azure-tools-win32.md) (Herunterladen der Azure-Tools).

### <a name="create-your-iot-hub-and-register-raspberry-pi-3"></a>Erstellen eines IoT-Hubs und Registrieren von Raspberry Pi 3
Erstellen Sie Ihre Ressourcengruppe, stellen Sie Ihre erste Azure IoT Hub-Instanz bereit, und fügen Sie ihr mithilfe der Azure-Befehlszeilenschnittstelle Ihr erstes Gerät hinzu.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Create your IoT hub and register Raspberry Pi 3](iot-hub-raspberry-pi-kit-c-lesson2-prepare-azure-iot-hub.md) (Erstellen eines IoT-Hubs und Registrieren des Raspberry Pi 3).

## <a name="lesson-3-send-device-to-cloud-messages"></a>Lektion 3: Senden von Gerät-an-Cloud-Nachrichten
![Lektion 3: Komplettes Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson3.png)

In dieser Lektion senden Sie Nachrichten von Ihrem Pi an den IoT-Hub. Außerdem erstellen Sie eine Azure-Funktionen-App, die eingehende Nachrichten von Ihrem IoT-Hub übernimmt und sie in den Azure-Tabellenspeicher schreibt.

Schließen Sie die Lektionen 1 und 2 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="create-an-azure-function-app-and-azure-storage-account"></a>Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos
Verwenden Sie eine Azure Resource Manager-Vorlage zum Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Erstellen einer Azure-Funktionen-App und eines Azure Storage-Kontos](iot-hub-raspberry-pi-kit-c-lesson3-deploy-resource-manager-template.md).

### <a name="run-a-sample-application-to-send-device-to-cloud-messages"></a>Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten
Bereitstellen und Ausführen einer Beispielanwendung auf dem Raspberry Pi 3, die Nachrichten an den IoT-Hub sendet.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Run a sample application to send device-to-cloud messages](iot-hub-raspberry-pi-kit-c-lesson3-run-azure-blink.md) (Ausführen einer Beispielanwendung zum Senden von D2C-Nachrichten).

### <a name="read-messages-persisted-in-azure-storage"></a>Lesen von Nachrichten in Azure Storage
Sie überwachen D2C-Nachrichten (Device-to-Cloud, Gerät-zu-Cloud), während diese in Azure Storage geschrieben werden.

*Geschätzter Zeitaufwand: 5 Minuten*

Wechseln Sie zu [Read messages persisted in Azure Storage](iot-hub-raspberry-pi-kit-c-lesson3-read-table-storage.md) (Lesen von Nachrichten in Azure Storage).

## <a name="lesson-4-send-cloud-to-device-messages"></a>Lektion 4: Senden von C2D-Nachrichten
![Lektion 4: Komplettes Diagramm](media/iot-hub-raspberry-pi-lessons/e2e-lesson4.png)

Diese Lektion zeigt, wie Nachrichten von Azure IoT Hub an den Raspberry Pi 3 gesendet werden. Die Nachrichten steuern die LED (Ein/Aus), die mit dem Pi verbunden ist. Zur Umsetzung dieser Aufgabe wird eine Beispielanwendung vorbereitet.

Schließen Sie die Lektionen 1, 2 und 3 ab, bevor Sie mit dieser Lektion beginnen.

### <a name="run-the-sample-application-to-receive-cloud-to-device-messages"></a>Ausführen der Beispielanwendung, um C2D-Nachrichten zu empfangen
Die Beispielanwendung in Lektion 4 wird auf dem Pi ausgeführt und überwacht eingehende Nachrichten von Ihrem IoT-Hub. Ein neuer Gulp-Task sendet Nachrichten von Ihrem IoT-Hub an den Pi, damit die LED blinkt.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Run the sample application to receive cloud-to-device messages](iot-hub-raspberry-pi-kit-c-lesson4-send-cloud-to-device-messages.md) (Ausführen der Beispielanwendung, um C2D-Nachrichten zu empfangen).

### <a name="optional-section-change-the-on-and-off-behavior-of-the-led"></a>Optionaler Abschnitt: Ändern des Ein- und Aus-Verhaltens der LED
Passen Sie die Nachrichten an, um das Ein- und Aus-Verhalten der LED zu ändern.

*Geschätzter Zeitaufwand: 10 Minuten*

Wechseln Sie zu [Optional section: Change the on and off behavior of the LED](iot-hub-raspberry-pi-kit-c-lesson4-change-led-behavior.md) (Optional: Ändern des LED-Verhaltens).

## <a name="troubleshooting"></a>Problembehandlung
Falls Sie bei den Lektionen Probleme haben, helfen Ihnen die Lösungen im Artikel zur [Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md) weiter.



<!--HONumber=Jan17_HO4-->


