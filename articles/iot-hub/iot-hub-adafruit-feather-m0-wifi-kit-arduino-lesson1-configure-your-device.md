---
title: "Verbinden von Arduino (C) mit Azure IoT – Lektion 1: Konfigurieren des Geräts | Microsoft-Dokumentation"
description: Konfigurieren Sie Adafruit Feather M0 WiFi zur erstmaligen Verwendung.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Arduino-Setup, Arduino mit PC verbinden, Setup von Arduino, Arduino-Platine
ms.assetid: f5b334f0-a148-41aa-b374-ce7b9f5b305a
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/13/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 3789c07cd6142ba5b6a58a67813c0ead6b87bd76


---
# <a name="configure-your-device"></a>Konfigurieren Ihres Geräts
## <a name="what-you-will-do"></a>Aufgaben
Konfigurieren Sie Ihre Adafruit Feather M0 WiFi Arduino-Platine zur erstmaligen Verwendung, indem Sie die Platine einbauen und einschalten. Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-adafruit-feather-m0-wifi-kit-arduino-troubleshooting.md).

## <a name="what-you-need"></a>Erforderliches Element
Für diesen Vorgang benötigen Sie die folgenden Teile aus Ihrem Adafruit Feather M0 WiFi Starter Kit:

* Adafruit Feather M0 WiFi-Platine
* USB-Kabel (Micro B zu Typ A)

![Kit][kit]

Außerdem benötigen Sie:

* Einen Computer (Windows, Mac oder Linux).
* Eine Drahtlosverbindung für Ihre Arduino-Platine
* Eine Internetverbindung zum Herunterladen des Konfigurationstools

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:

* Wie Sie Ihre Arduino-Platine für die folgenden Lektionen einbauen und einschalten
* Wie Sie unter Ubuntu Berechtigungen für den seriellen Anschluss hinzufügen

## <a name="connect-your-arduino-board-to-your-computer"></a>Verbinden der Arduino-Platine mit dem Computer

1. Stecken Sie das Micro-USB-Kabel in den oberen Micro-USB-Anschluss ein.

   ![Oberer Micro-USB-Anschluss][top-micro-usb-port]

2. Stecken Sie das andere Ende des USB-Kabels in den Computer ein.

   ![USB-Anschluss des Computers][computer-usb]

## <a name="add-serial-port-permissions-on-ubuntu"></a>Hinzufügen von Berechtigungen für den seriellen Anschluss unter Ubuntu

Sie können diesen Abschnitt überspringen, wenn Sie Windows oder MacOS verwenden. Für Ubuntu müssen Sie die folgenden Schritte ausführen, um sicherzustellen, dass der normale Linux-Benutzer über die Berechtigungen für den USB-Anschluss Ihrer Arduino-Platine verfügt.

1. Führen Sie auf dem Terminal als normaler Benutzer den folgenden Befehl aus:

   ```bash
   ls -l /dev/ttyUSB*
   # Or
   ls -l /dev/ttyACM*
   ```

   Die Ausgabe sollte in etwa wie folgt aussehen:

   ```bash
   crw-rw---- 1 root uucp 188, 0 5 apr 23.01 ttyUSB0
   # Or
   crw-rw---- 1 root dialout 188, 0 5 apr 23.01 ttyACM0
   ```

   Es ist möglich, dass anstelle von „0“ eine andere Zahl angezeigt wird oder mehrere Einträge zurückgegeben werden. Im ersten Fall benötigen wir `uucp` und im zweiten Fall `dialout`, den Gruppenbesitzer der Datei.

2. Fügen Sie den Benutzer der Gruppe hinzu:

   ```bash
   sudo usermod -a -G group-name username
   ```

   `group-name` sind die im ersten Schritt zurückgegebenen Daten, und `username` ist Ihr Linux-Benutzername.

3. Sie müssen sich ab- und erneut anmelden, damit diese Änderung wirksam wird, und das Setup abschließen.

## <a name="summary"></a>Zusammenfassung
In diesem Artikel haben Sie erfahren, wie Sie Ihre Arduino-Platine konfigurieren. Der nächste Schritt ist das Installieren der erforderlichen Tools und Software als Vorbereitung für die Ausführung einer Beispielanwendung auf der Arduino-Platine.

![Hardware ist bereit][hardware-is-ready]

## <a name="next-steps"></a>Nächste Schritte
[Abrufen der Tools][get-the-tools]
<!-- Images and links -->

[kit]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/kit.png
[top-micro-usb-port]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/top_usbport.jpg
[computer-usb]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/computer_usb.jpg
[hardware-is-ready]: media/iot-hub-adafruit-feather-m0-wifi-lessons/lesson1/hardware_ready.jpg
[get-the-tools]: iot-hub-adafruit-feather-m0-wifi-kit-arduino-lesson1-get-the-tools-win32.md


<!--HONumber=Jan17_HO4-->


