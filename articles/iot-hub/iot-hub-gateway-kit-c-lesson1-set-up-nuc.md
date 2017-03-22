---
title: "SensorTag-Gerät und Azure IoT Gateway – Lektion 1: Einrichten von NUC | Microsoft-Dokumentation"
description: Es wird beschrieben, wie Sie Intel NUC als IoT Gateway zwischen einem Sensor und Azure IoT Hub einrichten, um Sensorinformationen zu sammeln und an IoT Hub zu senden.
services: iot-hub
documentationcenter: 
author: shizn
manager: yjianfeng
tags: 
keywords: IoT Gateway, Intel NUC, NUC-Computer, DE3815TYKE
ms.assetid: 917090d6-35c2-495b-a620-ca6f9c02b317
ms.service: iot-hub
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/21/2017
ms.author: xshi
translationtype: Human Translation
ms.sourcegitcommit: 61e9a9fc7876094c04238c61cfc38efdd97b05f7
ms.openlocfilehash: 53e709c5134eec29d71be1d75353d606aa651273
ms.lasthandoff: 01/25/2017


---
# <a name="set-up-intel-nuc-as-an-iot-gateway"></a>Einrichten von Intel NUC als IoT Gateway

## <a name="what-you-will-do"></a>Aufgaben

- Einrichten von Intel NUC als IoT Gateway
- Installieren des Azure IoT Gateway SDK-Pakets auf einem Intel NUC
- Ausführen einer „hello_world“-Beispielanwendung auf einem Intel NUC, um die Gatewayfunktionalität zu überprüfen
Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="what-you-will-learn"></a>Sie lernen Folgendes

In dieser Lektion lernen Sie Folgendes:

- Herstellen der Verbindung mit einem Intel NUC mithilfe von Peripheriegeräten
- Installieren und Aktualisieren der erforderlichen Pakete auf dem Intel NUC mit dem Smart Package Manager
- Ausführen einer „hello_world“-Beispielanwendung, um die Gatewayfunktionalität zu überprüfen

## <a name="what-you-need"></a>Erforderliches Element

- Ein Intel NUC Kit DE3815TYKE mit der Intel IoT Gateway Software Suite (Wind River Linux *7.0.0.13) ist vorinstalliert.
- Ein Ethernet-Kabel
- Eine Tastatur
- Ein HDMI- oder VGA-Kabel
- Ein Monitor mit HDMI- oder VGA-Anschluss

![Gateway-Kit](media/iot-hub-gateway-kit-lessons/lesson1/kit.png)

## <a name="connect-intel-nuc-with-the-peripherals"></a>Verbinden des Intel NUC mit den Peripheriegeräten

Die folgende Abbildung ist ein Beispiel für einen Intel NUC, der mit verschiedenen Peripheriegeräten verbunden ist:

1. Verbindung mit einer Tastatur
2. Verbindung mit dem Monitor per VGA- oder HDMI-Kabel
3. Verbindung mit einem Kabelnetzwerk per Ethernet-Kabel
4. Verbindung mit der Stromversorgung per Netzkabel

![Verbindung des Intel NUC mit Peripheriegeräten](media/iot-hub-gateway-kit-lessons/lesson1/nuc.png)

## <a name="connect-to-the-intel-nuc-system-from-host-computer-via-secure-shell-ssh"></a>Herstellen der Verbindung mit dem Intel NUC-System vom Hostcomputer aus per Secure Shell (SSH)

Hierfür benötigen Sie eine Tastatur und einen Monitor, um die IP-Adresse Ihres NUC-Geräts abzurufen. Falls Ihnen die IP-Adresse bereits bekannt ist, können Sie mit Schritt 3 dieses Abschnitts fortfahren.

1. Schalten Sie den Intel NUC ein, indem Sie den Netzschalter betätigen, und melden Sie sich am System an.

   Standardbenutzername und -kennwort lauten jeweils `root`.

2. Rufen Sie die IP-Adresse des NUC ab, indem Sie den Befehl `ifconfig` ausführen. Dieser Schritt wird auf dem NUC-Gerät ausgeführt.

   Hier ist ein Beispiel für die Befehlsausgabe angegeben:

   ![Ausgabe von „ifconfig“ mit NUC-IP](media/iot-hub-gateway-kit-lessons/lesson1/ifconfig.png)

   In diesem Beispiel folgt auf `inet addr:` die IP-Adresse, die Sie beim Planen der Remoteverbindung von einem Hostcomputer zum Intel NUC benötigen.

3. Verwenden Sie einen der folgenden SSH-Clients von Ihrem Hostcomputer aus, um die Verbindung mit dem Intel NUC herzustellen.

   - [PuTTY](http://www.putty.org/) für Windows.
   - Der integrierte SSH-Client unter Ubuntu oder MacOS.

   Es ist effizienter und produktiver, Vorgänge für den Intel NUC auf einem Hostcomputer durchzuführen. Sie benötigen die IP-Adresse, den Benutzernamen und das Kennwort, um über den SSH-Client eine Verbindung mit dem NUC herzustellen. Hier ist ein Beispiel für die Verwendung des SSH-Clients unter MacOS.
   ![SSH-Client unter MacOS](media/iot-hub-gateway-kit-lessons/lesson1/ssh.png)

## <a name="install-the-azure-iot-gateway-sdk-package"></a>Installieren des Azure IoT Gateway SDK-Pakets

Das Azure IoT Gateway SDK-Paket enthält die vorkompilierten Binärdateien des SDK und die jeweiligen Abhängigkeiten. Diese Binärdateien sind das Azure IoT Gateway SDK, das Azure IoT SDK und die entsprechenden Tools. Das Paket enthält auch eine „hello_world“-Beispielanwendung, die zum Überprüfen der Gatewayfunktionalität verwendet wird. Das SDK ist der Kern des Gateways. Führen Sie diese Schritte aus, um das Paket zu installieren:

1. Fügen Sie das IoT-Cloudrepository hinzu, indem Sie die folgenden Befehle in einem Terminalfenster ausführen:

   ```bash
   rpm --import http://iotdk.intel.com/misc/iot_pub.key
   smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
   ```

   Mit dem Befehl `rpm` wird der rpm-Schlüssel importiert. Mit dem Befehl `smart channel` wird der rpm-Kanal dem Smart Package Manager hinzugefügt. Bevor Sie den Befehl `smart update` ausführen, wird eine Ausgabe wie unten angezeigt.

   ![Ausgabe der Kanalbefehle „rpm“ und „smart“](media/iot-hub-gateway-kit-lessons/lesson1/rpm_smart_channel.png)

   ```bash
   smart update
   ```

2. Installieren Sie das Paket, indem Sie den folgenden Befehl ausführen:

   ```bash
   smart install packagegroup-cloud-azure -y
   ```

   `packagegroup-cloud-azure` ist der Name des Pakets. Der Befehl `smart install` wird zum Installieren des Pakets verwendet.

   Nach dem Installieren des Pakets soll der Intel NUC als Gateway fungieren.

## <a name="run-the-azure-iot-gateway-sdk-helloworld-sample-application"></a>Ausführen der „hello_world“-Beispielanwendung des Azure IoT Gateway SDK

Navigieren Sie zu `azureiotgatewaysdk/samples`, und führen Sie die „hello_world“-Beispielanwendung aus. Bei dieser Beispielanwendung wird ein Gateway aus der Datei `hello_world.json` erstellt, und die grundlegenden Komponenten der Azure IoT Gateway SDK-Architektur werden verwendet, um alle fünf Sekunden eine „hello world“-Nachricht in einer Datei zu protokollieren.

Sie können die „hello_world“-Beispielanwendung mit dem folgenden Befehl ausführen:

```bash
cd /usr/share/azureiotgatewaysdk/samples/hello_world/
./hello_world hello_world.json
```

Die Beispielanwendung erzeugt die folgende Ausgabe, wenn die Gatewayfunktionalität richtig funktioniert:

![Anwendungsausgabe](media/iot-hub-gateway-kit-lessons/lesson1/hello_world.png)

Problemlösungen finden Sie auf der [Seite zur Problembehandlung](iot-hub-gateway-kit-c-troubleshooting.md).

## <a name="summary"></a>Zusammenfassung

Glückwunsch! Sie haben die Einrichtung des Intel NUC als Gateway abgeschlossen. Sie können jetzt mit der nächsten Lektion fortfahren, um den Hostcomputer einzurichten, einen Azure IoT Hub zu erstellen und das logische Azure IoT Hub-Gerät zu registrieren.

## <a name="next-steps"></a>Nächste Schritte
[Vorbereiten des Hostcomputers und von Azure IoT Hub](iot-hub-gateway-kit-c-lesson2-get-the-tools-win32.md)

