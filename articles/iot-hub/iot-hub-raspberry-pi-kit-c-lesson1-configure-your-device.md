---
title: "Verbinden von Raspberry Pi (C) mit Azure IoT – Lektion 1: Konfigurieren des Geräts | Microsoft-Dokumentation"
description: "Konfigurieren Sie den Raspberry Pi 3 für die erstmalige Verwendung, und installieren Sie Raspbian, ein kostenloses Betriebssystem, das für die Raspberry Pi-Hardware optimiert ist."
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: "Raspbian installieren, Raspbian-Download, Installation von Raspbian, Raspbian-Setup, Raspberry Pi-Installation, Betriebssystem Raspberry Pi installieren, Raspberry Pi SD-Karteninstallation, Raspberry Pi verbinden, Verbindung für Raspberry Pi herstellen, Raspberry Pi-Verbindung"
ms.assetid: 8ee9b23c-93f7-43ff-8ea1-e7761eb87a6f
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/28/2016
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 64e69df256404e98f6175f77357500b562d74318
ms.openlocfilehash: 4fdd8de3a24bf521b8ea13416900e69d24e1b62d


---
# <a name="configure-your-device"></a>Konfigurieren Ihres Geräts
## <a name="what-you-will-do"></a>Aufgaben
Führen Sie die erste Konfiguration von Pi durch, und installieren Sie das Raspbian-Betriebssystem. Raspbian ist ein kostenloses Betriebssystem, das für die Raspberry Pi-Hardware optimiert ist. Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-raspberry-pi-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes
In diesem Artikel lernen Sie Folgendes:

* Installieren von Raspbian auf dem Raspberry Pi
* Herstellen der Stromversorgung für den Raspberry Pi mit einem USB-Kabel
* Herstellen der Netzwerkverbindung für den Raspberry Pi mit einem Ethernet-Kabel oder per WLAN
* Hinzufügen einer LED zur Steckplatine und Herstellen der Verbindung mit dem Raspberry Pi

## <a name="what-you-need"></a>Erforderliches Element
Für diesen Vorgang benötigen Sie die folgenden Teile aus Ihrem Raspberry Pi 3 Starter Kit:

* Raspberry Pi-3-Board
* 16-GB-microSD-Karte
* Netzteil (5 V 2 A) mit Micro-USB-Kabel (1,8 m)
* Steckplatine
* Anschlussdrähte
* 1 Widerstand 560 Ohm
* 1 diffuse 10-mm-LED
* Ethernet-Kabel

![Teile aus dem Starter Kit](media/iot-hub-raspberry-pi-lessons/lesson1/starter_kit.jpg)

Außerdem benötigen Sie:

* Netzwerk (Ethernet oder WLAN) für die Verbindung für den Raspberry Pi
* 1 USB-SD-Adapter oder miniSD-Karte, um das Betriebssystemimage auf die microSD-Karte zu kopieren
* Einen Computer (Windows, Mac oder Linux). Der Computer wird für die Installation von Raspbian auf der microSD-Karte verwendet.
* Eine Internetverbindung zum Herunterladen der notwendigen Tools und der Software

## <a name="install-raspbian-on-the-microsd-card"></a>Installieren von Raspbian auf der microSD-Karte
Bereiten Sie die microSD-Karte für die Installation des Raspbian-Image vor.

1. Laden Sie Raspbian herunter.
   1. [Laden](https://www.raspberrypi.org/downloads/raspbian/) Sie die ZIP-Datei für Raspbian Jessie mit Pixel herunter.
   2. Entpacken Sie das Raspbian-Image in einen Ordner auf Ihrem Computer.
2. Installieren Sie Raspbian auf der microSD-Karte.
   1. [Laden](https://www.etcher.io) und installieren Sie das Dienstprogramm Etcher SD Card Burner.
   2. Führen Sie Etcher aus, und wählen Sie das Raspbian-Image, das Sie in Schritt 1 entpackt haben.
   3. Wählen Sie das microSD-Kartenlaufwerk.
      Hinweis: Etcher hat unter Umständen bereits das richtige Laufwerk ausgewählt.
   4. Klicken Sie auf **Flash**, um Raspbian auf der microSD-Karte zu installieren.
   5. Entfernen Sie die microSD-Karte aus dem Computer, wenn die Installation abgeschlossen ist.
      Es ist sicher, die microSD-Karte direkt zu entfernen, da Etcher die microSD-Karte nach Abschluss des Vorgangs automatisch auswirft bzw. die Bereitstellung aufhebt.
   6. Führen Sie die microSD-Karte in Ihren Raspberry Pi ein.

![SD-Karte einlegen](media/iot-hub-raspberry-pi-lessons/lesson1/insert_sdcard.jpg)

## <a name="turn-on-pi"></a>Pi einschalten
Verbinden Sie den Raspberry Pi mit dem Micro-USB-Kabel mit der Stromversorgung.

![Verbindung mit der Stromversorgung](media/iot-hub-raspberry-pi-lessons/lesson1/micro_usb_power_on.jpg)

> [!NOTE]
> Es ist wichtig, die Stromversorgung im Kit zu verwenden, die mindestens 2 A liefert. So wird sichergestellt, dass der Raspberry genügend Strom erhält und richtig funktioniert.

## <a name="connect-raspberry-pi-3-to-the-network"></a>Verbinden des Raspberry Pi 3 mit dem Netzwerk
Sie können den Raspberry Pi mit einem Kabelnetzwerk oder mit einem Drahtlosnetzwerk verbinden. Stellen Sie sicher, dass der Raspberry Pi mit dem gleichen Netzwerk wie der Computer verbunden ist. Beispielsweise können Sie den Raspberry Pi mit dem gleichen Switch verbinden, mit dem der Computer verbunden ist.

### <a name="connect-to-a-wired-network"></a>Herstellen der Verbindung mit einem Kabelnetzwerk
Verwenden Sie ein Ethernet-Kabel, um den Raspberry Pi mit dem Kabelnetzwerk zu verbinden. Die zwei LEDs auf dem Raspberry Pi leuchten, wenn die Verbindung hergestellt ist.

![Herstellen einer Verbindung per Ethernet-Kabel](media/iot-hub-raspberry-pi-lessons/lesson1/connect_ethernet.jpg)

### <a name="connect-to-a-wireless-network"></a>Herstellen der Verbindung mit einem WLAN
Führen Sie die [Anweisungen](https://www.raspberrypi.org/learning/software-guide/wifi/) der Raspberry Pi Foundation aus, um den Pi mit einem Drahtlosnetzwerk (WLAN) zu verbinden. Bei dieser Vorgehensweise ist es erforderlich, dass Sie zuerst einen Monitor und eine Tastatur an den Raspberry Pi anschließen.

## <a name="connect-the-led-to-pi"></a>Verbinden der LED mit dem Raspberry Pi
Zum Ausführen dieser Aufgabe verwenden Sie die [Steckplatine](https://learn.sparkfun.com/tutorials/how-to-use-a-breadboard), die Anschlusskabel, die LED und den Widerstand. Verbinden Sie diese mit den GPIO-Ports ([General-Purpose Input/Output](https://www.raspberrypi.org/documentation/usage/gpio/)) des Raspberry Pi.

![Steckplatine, LED und Widerstand](media/iot-hub-raspberry-pi-lessons/lesson1/breadboard_led_resistor.jpg)

1. Verbinden Sie den kürzeren Anschluss der LED mit **GPIO GND (Pin 6)**.
2. Verbinden Sie den längeren Anschluss der LED mit einem Anschluss des Widerstands.
3. Verbinden Sie den anderen Anschluss des Widerstands mit **GPIO 4 (Pin 7)**.

Beachten Sie, dass die LED-Polarität wichtig ist. Diese Polaritätskonfiguration wird auch als aktiv LOW bezeichnet.

![Pinout](media/iot-hub-raspberry-pi-lessons/lesson1/pinout_breadboard.png)

Glückwunsch! Sie haben den Raspberry Pi erfolgreich konfiguriert.

## <a name="summary"></a>Zusammenfassung
In diesem Artikel haben Sie erfahren, wie Sie den Raspberry Pi konfigurieren, indem Sie Raspbian installieren, den Pi mit einem Netzwerk verbinden und eine LED mit dem Pi verbinden. Beachten Sie, dass die LED noch nicht leuchtet. Der nächste Schritt ist das Installieren der erforderlichen Tools und Software als Vorbereitung für eine Beispielanwendung, die auf dem Raspberry Pi ausgeführt wird.

![Hardware ist bereit](media/iot-hub-raspberry-pi-lessons/lesson1/hardware_ready.jpg)

## <a name="next-steps"></a>Nächste Schritte
[Abrufen der Tools](iot-hub-raspberry-pi-kit-c-lesson1-get-the-tools-win32.md)




<!--HONumber=Jan17_HO4-->


