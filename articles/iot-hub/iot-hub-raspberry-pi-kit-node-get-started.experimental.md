---
title: 'Raspberry Pi in der Cloud (Node.js): Verbinden von Raspberry Pi mit Azure IoT Hub | Microsoft-Dokumentation'
description: Verbinden Sie Raspberry Pi mit Azure IoT Hub, damit Raspberry Pi Daten an die Azure-Cloud sendet.
services: iot-hub
documentationcenter: 
author: shizn
manager: timtl
tags: 
keywords: Azure IoT Raspberry Pi, Raspberry Pi IoT Hub, Raspberry Pi sendet Daten an Cloud, Raspberry Pi in der Cloud
experiment_id: xshi-happypathemu-20161202
ms.assetid: b0e14bfa-8e64-440a-a6ec-e507ca0f76ba
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/14/2017
ms.author: xshi
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: 867914b78022e3ac288df079c973692c922158be
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="connect-raspberry-pi-to-azure-iot-hub-nodejs"></a>Verbinden von Raspberry Pi mit Azure IoT Hub (Node.js)

[!INCLUDE [iot-hub-get-started-device-selector](../../includes/iot-hub-get-started-device-selector.md)]

In diesem Tutorial erlernen Sie die Grundlagen der Verwendung von Raspberry Pi 3 und Raspbian. Anschließend erfahren Sie, wie Sie Ihre Geräte mithilfe von [Azure IoT Hub](iot-hub-what-is-iot-hub.md) nahtlos mit der Cloud verbinden. Beispiele für Windows 10 IoT Core finden Sie im [Windows Dev Center](http://www.windowsondevices.com/).

Sie haben noch kein Kit? Testen Sie den [Raspberry Pi 3-Emulator](https://blogs.msdn.microsoft.com/iliast/2016/11/10/how-to-emulate-raspberry-pi/). Oder erwerben Sie [hier](https://azure.microsoft.com/develop/iot/starter-kits) ein neues Kit.

## <a name="what-you-do"></a>Aufgaben

* Richten Sie Raspberry Pi ein.
* Erstellen Sie einen IoT Hub.
* Registrieren Sie ein Gerät für Pi in Ihrem IoT Hub.
* Führen Sie eine Beispielanwendung auf Pi aus, um Sensordaten an Ihren IoT Hub zu senden.

Verbinden Sie Raspberry Pi mit einem von Ihnen erstellten IoT Hub. Führen Sie anschließend eine Beispielanwendung auf Pi aus, um Temperatur- und Feuchtigkeitsdaten eines BME280-Sensors zu erfassen. Senden Sie abschließend die Sensordaten an Ihren IoT Hub.

## <a name="what-you-learn"></a>Lerninhalt

* Erstellen eines Azure IoT Hubs und Abrufen der Verbindungszeichenfolge für Ihr neues Gerät
* Herstellen der Verbindung von Pi mit einem BME280-Sensor
* Erfassen von Sensordaten durch Ausführen einer Beispielanwendung auf Pi
* Senden von Sensordaten an Ihren IoT Hub

## <a name="what-you-need"></a>Erforderliches Element

![Erforderliches Element](media/iot-hub-raspberry-pi-kit-node-get-started/0_starter_kit.jpg)

* Raspberry Pi 2- oder Raspberry Pi 3-Platine.
* Ein aktives Azure-Abonnement. Wenn Sie kein Azure-Konto besitzen, können Sie in nur wenigen Minuten ein [kostenloses Azure-Testkonto](https://azure.microsoft.com/free/) erstellen.
* Monitor, USB-Tastatur und Maus, die mit Pi verbunden werden.
* Mac oder PC, auf dem Windows oder Linux ausgeführt wird
* Internetverbindung.
* microSD-Karte mit mindestens 16 GB
* USB-SD-Adapter oder microSD-Karte, um das Betriebssystemimage auf die microSD-Karte zu kopieren
* Netzteil (5 V, 2 A) mit Micro-USB-Kabel (1,8 m)

Die folgenden Elemente sind optional:

* Ein zusammengebauter Adafruit BME280-Sensor für Temperatur, Luftdruck und Luftfeuchtigkeit
* Eine Steckplatine
* 6 F/M-Jumperdrähte
* 1 LED, diffus, 10 mm

  > [!NOTE] 
  Diese Elemente sind optional, da das Codebeispiel simulierte Sensordaten unterstützt.

[!INCLUDE [iot-hub-get-started-create-hub-and-device](../../includes/iot-hub-get-started-create-hub-and-device.md)]

## <a name="setup-raspberry-pi"></a>Einrichten von Raspberry Pi

### <a name="install-the-raspbian-operating-system-for-pi"></a>Installieren des Betriebssystems Raspbian für Pi

Bereiten Sie die microSD-Karte für die Installation des Raspbian-Image vor.

1. Laden Sie Raspbian herunter.
   1. [Laden Sie für Raspbian Jessie mit Pixel herunter](https://www.raspberrypi.org/downloads/raspbian/) (die ZIP-Datei).
   1. Entpacken Sie das Raspbian-Image in einen Ordner auf Ihrem Computer.
1. Installieren Sie Raspbian auf der microSD-Karte.
   1. [Laden Sie das SD-Kartenbrennprogramm Etcher herunter, und installieren Sie es](https://etcher.io/).
   1. Führen Sie Etcher aus, und wählen Sie das Raspbian-Image, das Sie in Schritt 1 entpackt haben.
   1. Wählen Sie das microSD-Kartenlaufwerk. Hinweis: Etcher hat unter Umständen bereits das richtige Laufwerk ausgewählt.
   1. Klicken Sie auf „Flash“, um Raspbian auf der microSD-Karte zu installieren.
   1. Entfernen Sie die microSD-Karte aus dem Computer, wenn die Installation abgeschlossen ist. Es ist sicher, die microSD-Karte direkt zu entfernen, da Etcher die microSD-Karte nach Abschluss des Vorgangs automatisch auswirft bzw. die Bereitstellung aufhebt.
   1. Legen Sie die microSD-Karte in den Raspberry Pi ein.

### <a name="enable-ssh-and-i2c"></a>Aktivieren von SSH und I2C

1. Verbinden Sie Pi mit dem Monitor, der Tastatur und Maus. Starten Sie Pi, und melden Sie sich bei Raspbian mit `pi` als Benutzername und `raspberry` als Kennwort an.
1. Klicken Sie auf das Raspberry-Symbol und dann auf **Preferences** > **Raspberry Pi Configuration**.

   ![Das Raspbian-Menü „Preferences“](media/iot-hub-raspberry-pi-kit-node-get-started/1_raspbian-preferences-menu.png)

1. Legen Sie auf der Registerkarte **Interfaces** die Einstellungen **I2C** und **SSH** auf **Enable** fest, und klicken Sie dann auf **OK**.

   ![Aktivieren von I2C und SSH auf Raspberry Pi](media/iot-hub-raspberry-pi-kit-node-get-started/2_enable-i2c-ssh-on-raspberry-pi.png)

> [!NOTE] 
Weitere Referenzdokumente zum Aktivieren von SSH und I2C finden Sie auf[raspberrypi.org](https://www.raspberrypi.org/documentation/remote-access/ssh/) und unter [RASPI-CONFIG](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-4-gpio-setup/configuring-i2c).

### <a name="connect-the-sensor-to-pi"></a>Verbinden des Sensors mit Pi

Verwenden Sie die Steckplatine und Jumperdrähte, um eine LED und einen BME280-Sensor wie folgt zu verbinden. Wenn Sie keinen Sensor haben, überspringen Sie diesen Abschnitt.

![Die Raspberry Pi- und Sensorverbindung](media/iot-hub-raspberry-pi-kit-node-get-started/3_raspberry-pi-sensor-connection.png)


Für Sensorstifte verwenden Sie die folgende Verkabelung:

| Start (Sensor und LED)     | Ende (Board)            | Kabelfarbe   |
| -----------------------  | ---------------------- | ------------: |
| VDD (Stift 5G)             | 3,3 V PWR (Stift 1)       | Weißes Kabel   |
| GND (Stift 7G)             | GND (Stift 6)            | Braunes Kabel   |
| SCK (Stift 8G)             | I2C1 SDA (Stift 3)       | Oranges Kabel  |
| SDI (Stift 10G)            | I2C1 SCL (Stift 5)       | Rotes Kabel     |
| LED VDD (Stift 18F)        | GPIO 24 (Stift 18)       | Weißes Kabel   |
| LED GND (Stift 17F)        | GND (Stift 20)           | Schwarzes Kabel   |

Klicken Sie hier, um die [Raspberry Pi 2 und 3-Stiftzuordnungen](https://developer.microsoft.com/windows/iot/docs/pinmappingsrpi) zur Referenz anzuzeigen.

Nachdem Sie den BME280 erfolgreich mit Ihrem Raspberry Pi verbunden haben, sollte das Gerät wie in der nachstehenden Abbildung aussehen.

![Verbindung zwischen Pi und BME280](media/iot-hub-raspberry-pi-kit-node-get-started/4_connected-pi.jpg)

### <a name="connect-pi-to-the-network"></a>Verbindung zwischen Pi und dem Netzwerk

Verbinden Sie den Raspberry Pi mit dem Micro-USB-Kabel mit der Stromversorgung. Verwenden Sie das Ethernet-Kabel zum Verbinden von Pi mit Ihrem verkabelten Netzwerk, oder befolgen Sie die [Anweisungen der Raspberry Pi Foundation](https://www.raspberrypi.org/learning/software-guide/wifi/), um Pi mit Ihrem WLAN zu verbinden. Notieren Sie sich die [IP-Adresse von Pi](https://learn.adafruit.com/adafruits-raspberry-pi-lesson-3-network-setup/finding-your-pis-ip-address), nachdem es eine Verbindung zum Netzwerk hergestellt hat.

![Mit verkabeltem Netzwerk verbunden](media/iot-hub-raspberry-pi-kit-node-get-started/5_power-on-pi.jpg)

> [!NOTE]
> Stellen Sie sicher, dass der Raspberry Pi mit dem gleichen Netzwerk wie der Computer verbunden ist. Wenn der Computer beispielsweise mit einem Drahtlosnetzwerk und Pi mit einem verkabelten Netzwerk verbunden ist, wird in der Ausgabe des devdisco-Befehls die IP-Adresse möglicherweise nicht angezeigt.

## <a name="run-a-sample-application-on-pi"></a>Ausführen einer Beispielanwendung auf Pi

### <a name="clone-sample-application-and-install-the-prerequisite-packages"></a>Klonen der Beispielanwendung und Installieren der Pakete mit den erforderlichen Komponenten

1. Verwenden Sie einen der folgenden SSH-Clients auf Ihrem Hostcomputer, um die Verbindung mit Ihrem Raspberry Pi herzustellen.
    - [PuTTY](http://www.putty.org/) für Windows. Sie benötigen die IP-Adresse von Pi, um eine SSH-Verbindung herzustellen.
    - Den integrierten SSH-Client unter Ubuntu oder macOS. Möglicherweise müssen Sie `ssh pi@<ip address of pi>` ausführen, um eine SSH-Verbindung mit Pi herzustellen.

   > [!NOTE] 
   Der Standardbenutzername ist `pi`, und das Kennwort ist `raspberry`.

1. Installieren von Node.js und NPM auf Pi
   
   Prüfen Sie zunächst Ihre Version von „node.js“ mit dem folgenden Befehl. 
   
   ```bash
   node -v
   ```

   Wenn die Version niedriger als 4.x ist, oder sich kein „node.js“ auf Ihrem Pi befindet, führen Sie den folgenden Befehl aus, um „node.js“ zu installieren oder zu aktualisieren.

   ```bash
   curl -sL http://deb.nodesource.com/setup_4.x | sudo -E bash
   sudo apt-get -y install nodejs
   ```

1. Klonen Sie die Beispielanwendung durch Ausführen des folgenden Befehls:

   ```bash
   git clone https://github.com/Azure-Samples/iot-hub-node-raspberrypi-client-app
   ```

1. Installieren Sie alle Pakete über den folgenden Befehl. Dazu gehören das Azure IoT-Geräte-SDK, die BME280-Sensorbibliothek und die Pi-Bibliothek zur Verkabelung.

   ```bash
   cd iot-hub-node-raspberrypi-client-app
   sudo npm install
   ```
   > [!NOTE] 
   Je nach Netzwerkverbindung kann das Abschließen dieses Installationsvorgangs mehrere Minuten dauern.

### <a name="configure-the-sample-application"></a>Konfigurieren der Beispielanwendung

1. Öffnen Sie die Konfigurationsdatei durch Ausführen der folgenden Befehle:

   ```bash
   nano config.json
   ```

   ![Konfigurationsdatei](media/iot-hub-raspberry-pi-kit-node-get-started/6_config-file.png)

   Es gibt zwei Argumente in dieser Datei, die Sie konfigurieren können. Das erste ist `interval`, welches das Zeitintervall zwischen zwei Nachrichten bestimmt, die an die Cloud gesendet werden. Das zweite heißt `simulatedData` und ist ein boolescher Wert, der bestimmt, ob simulierte Sensordaten verwendet werden sollen.

   Wenn Sie **keinen Sensor haben**, legen Sie den Wert von `simulatedData` auf `true` fest, damit die Beispielanwendung simulierte Sensordaten erstellt und nutzt.

1. Speichern und beenden Sie durch Drücken von Control+O > EINGABETASTE > Control+X.

### <a name="run-the-sample-application"></a>Ausführen der Beispielanwendung

1. Führen Sie die Beispielanwendung durch Aufrufen des folgenden Befehls aus:

   ```bash
   sudo node index.js '<your Azure IoT hub device connection string>'
   ```

   > [!NOTE] 
   Stellen Sie sicher, dass Sie die Verbindungszeichenfolge des Geräts kopieren und zwischen einfachen Anführungszeichen einfügen.


Die folgende Ausgabe sollte angezeigt werden, die die Sensordaten und Nachrichten zeigt, die an Ihren IoT Hub gesendet werden.

![Ausgabe: Von Raspberry Pi an Ihren IoT Hub gesendete Sensordaten](media/iot-hub-raspberry-pi-kit-node-get-started/8_run-output.png)

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine Beispielanwendung ausgeführt, die Sensordaten sammelt und an Ihren IoT Hub sendet.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
